---
layout: post
title: 调用 RequestRateAndReviewAppAsync 导致应用闪退：一个 try/catch 抓不住的崩溃
description: 当 C# 的 try/catch 完全失效时，如何排查和规避系统原生组件内部的 Access Violation
date: 2026-04-23
author: JP
image: /img/post-bg-coffee.jpeg
toc: true
tags:
- WPF
- .NET
- MSIX
- Windows Store API
- Crash Analysis
- Access Violation

---

> 记录一次 WPF 桌面应用调用 Windows Store 评分 API 导致进程直接终止的排查过程。崩溃发生在微软系统组件内部，`try/catch` 完全无效，最终通过 URI 协议规避。

## 1. 问题背景

我们的 WPF 桌面应用（.NET 8, MSIX 打包）中有一个评分弹窗功能：用户评分后，点击"去 Store 评价"按钮，调用 Windows 提供的 `StoreContext.RequestRateAndReviewAppAsync()` API 弹出原生 Store 评分对话框。

调用链：

```
ButtonClickHandler()
  → ShowStoreRatingDialogAsync(hwnd)
    → StoreContext.RequestRateAndReviewAppAsync()
```

**现象：** 在部分设备上，用户点击按钮后，应用直接闪退，没有任何异常弹窗，没有 crash dialog。

## 2. 现象与证据

### 2.1 应用日志

```log
[INF]  Review button clicked
[INF]  Review window hidden, starting Store rating dialog
[INF]  Store ShowStoreRatingDialogAsync start
[DBG] Attempting to show native Store rating dialog
        （之后再无日志，进程已终止）
```

注意：**没有任何 `catch` 块的日志被输出。** 这是第一个线索——异常绕过了 `try/catch`。

### 2.2 事件查看器

在 Windows 事件查看器中找到两条关联记录：

**事件 1026（.NET Runtime）：**

```
Application: MyApp.exe
CoreCLR Version: 8.0.2626.16921
.NET Version: 8.0.26
Description: The process was terminated due to an unhandled exception.
Exception Info: exception code c0000005, exception address 00007FFE5A253672
Stack:                              （空，无托管调用栈）
```

**事件 1000（Application Error）：**

```
出错应用程序名称:  MyApp.exe
出错模块名称:      Windows.ApplicationModel.Store.dll
版本:              10.0.26100.8246
异常代码:          0xc0000005 (ACCESS_VIOLATION)
错误偏移:          0x00000000000b3672
```

关键发现：

| 字段 | 第一次崩溃 | 第二次崩溃 |
|---|---|---|
| 出错模块 | `Windows.ApplicationModel.Store.dll` | `Windows.ApplicationModel.Store.dll` |
| 模块版本 | `10.0.26100.8246` | `10.0.26100.8246` |
| 异常代码 | `0xc0000005` | `0xc0000005` |
| 错误偏移 | `0x00000000000b3672` | `0x00000000000b3672` |
| Stack | 空 | 空 |

**偏移 `0xb3672` 完全一致**，说明是 `Store.dll` 内部**同一行 C++ 代码**每次都在同一位置崩溃，100% 稳定复现（在该设备上）。

## 3. 根因分析

### 3.1 崩溃发生在哪里

`RequestRateAndReviewAppAsync()` 的内部执行路径大致如下：

```
C# 业务代码
  → StoreContext.RequestRateAndReviewAppAsync()        [WinRT 投影]
    → Windows.ApplicationModel.Store.dll (C++ native)  [系统组件]
      → 与 Microsoft Store 服务进程通信
      → 获取 App 的 Store 注册信息
      → 创建并显示原生评分对话框
        → 💥 Access Violation @ offset 0xb3672
```

崩溃点在 `Store.dll` 的纯原生 C++ 代码中。两次崩溃偏移完全一致，Stack 为空，说明是 `Store.dll` 内部某个原生函数对空指针解引用，且没有做防御性检查。

### 3.2 根因结论

这是 `Windows.ApplicationModel.Store.dll` 版本 `10.0.26100.8246` 的 **bug**。业务代码不管怎么写，只要调到 `RequestRateAndReviewAppAsync()` 就会触发（在受影响的设备上）。

## 4. 为什么 try/catch 抓不住

这是这次排查中最有价值的技术点。代码明明写了完整的 `try/catch`：

```csharp
try
{
    StoreRateAndReviewResult result = await storeContext.RequestRateAndReviewAppAsync();
    // ...
}
catch (Exception ex)  // 看起来应该能兜底？
{
    logger.LogError(ex, "Exception while showing Store rating dialog");
}
```

但 `catch` 从未执行。原因在于异常的分层结构：

```
┌─────────────────────────────────────┐
│  C# try/catch                       │  ← 只能捕获 managed exceptions
├─────────────────────────────────────┤
│  .NET CLR 异常处理                  │  ← 部分 SEH 会被转换为 managed 异常
├─────────────────────────────────────┤
│  Windows SEH                        │  ← 0xc0000005 发生在这里
├─────────────────────────────────────┤
│  Windows.ApplicationModel.          │  ← Store.dll 原生 C++ 代码
│  Store.dll (native code)            │     对无效指针解引用
└─────────────────────────────────────┘
```

各层级的可捕获性：

| 异常层级 | 示例 | C# catch 能否捕获 |
|---|---|---|
| C# managed exception | `InvalidOperationException` | ✅ 能 |
| .NET 包装的 SEH | 托管代码中的 `NullReferenceException` | ✅ 能 |
| CLR 管理范围内的原生 AV | 托管代码触发的 `AccessViolationException` | ⚠️ .NET Framework 可通过 `[HandleProcessCorruptedStateExceptions]` 捕获；.NET Core/5+ **已移除该支持** |
| 纯 native DLL 中的 AV | `Store.dll` 内部的 `0xc0000005` | ❌ **不能** — CLR 直接 Fail Fast 终止进程 |

`Store.dll` 是纯原生 C++ 代码，Access Violation 发生时根本不经过 CLR 的异常处理管道。CLR 检测到进程状态已损坏（Corrupted State），判定无法安全继续运行，直接触发 **Fail Fast** 终止进程。这就是为什么事件查看器中 Stack 字段为空——崩溃时根本没有托管调用栈参与。

**简单说：这不是一个能被 catch "解决"的问题，唯一正确的做法是不让它发生。**

## 5. 为什么不是所有设备都崩溃

这个 bug 并非必现，在我们的测试中只在部分设备上触发。这是"环境触发型"问题的典型特征，常见差异因素包括：

| 差异维度 | 影响方式 |
|---|---|
| **Windows 累积补丁 / Store.dll 版本** | 同为 Win11，不同补丁级别的 Store.dll 内部代码路径可能不同 |
| **Microsoft Store 登录状态** | 是否登录微软账号会影响 Store 服务返回的数据结构 |
| **应用安装来源** | 商店安装 vs 旁加载 vs 企业分发，PFN/可评分状态不同 |
| **Store 缓存与服务状态** | 商店缓存损坏、服务不可用时，内部返回数据可能为空 |
| **企业策略 / 网络代理** | GPO 限制 Store 功能、VPN/代理影响 Store 服务通信 |
| **区域 / 语言配置** | 不同区域的 Store 行为和可用性不同 |

本质上，`Store.dll` 内部在某些环境条件下拿到了空/无效数据，但没有做 null check 就直接解引用，导致 AV。在数据恰好有效的设备上不会触发。

## 6. 修复方案

### 6.1 排除的方案

| 方案 | 为什么不行 |
|---|---|
| 加 `try/catch` | 原生 AV 无法被 C# catch 捕获 |
| 加 `[HandleProcessCorruptedStateExceptions]` | .NET Core/5+ 已移除该特性支持 |
| 调整 HWND / 窗口可见性 / 线程调度 | 实测无效，崩溃偏移完全一致 |
| 在单独 AppDomain 中调用 | .NET Core 不支持多 AppDomain |
| 在单独进程中调用 | 过于复杂，且有更简单的替代方案 |

### 6.2 最终方案：使用 Store URI 协议

不再调用 `RequestRateAndReviewAppAsync()`，改用 `ms-windows-store://review/` URI 协议：

```csharp
public async Task<bool> ShowStoreRatingDialogAsync()
{
    try
    {
        var pfn = Windows.ApplicationModel.Package.Current.Id.FamilyName;
        var uri = new Uri($"ms-windows-store://review/?PFN={pfn}");
        bool launched = await Windows.System.Launcher.LaunchUriAsync(uri);
        return launched;
    }
    catch (Exception ex)
    {
        logger.LogError(ex, "Exception while opening Store review page");
        return false;
    }
}
```

### 6.3 为什么这个方案安全

`Launcher.LaunchUriAsync` 是一个**进程间通信**操作：

```
你的进程                         Store 进程
  │                                │
  ├─ LaunchUriAsync ──────────────→│
  │   (只是发送一个 URI)            ├─ 打开评分页面
  │                                │   即使这里崩了
  │   你的进程完全不受影响 ✅       │   也是 Store 自己的事
```

对比之前的 `RequestRateAndReviewAppAsync`：

```
你的进程
  │
  ├─ RequestRateAndReviewAppAsync
  │     ├─ 加载 Store.dll 到你的进程内存空间
  │     ├─ 在你的进程内执行原生代码
  │     └─ 💥 AV → 你的进程终止
```

## 7. 经验总结

1. **不要假设 `try/catch` 能兜住一切。** 当你调用的是 WinRT/COM/原生组件时，底层 native 崩溃可以直接杀死进程。
2. **事件查看器是排查闪退的第一工具。** 当应用日志中看不到异常、`catch` 块没有执行，第一时间查事件查看器的 Application Error (1000) 和 .NET Runtime (1026) 事件。
3. **崩溃偏移一致 = 同一个 bug。** 如果多次崩溃的模块名、偏移完全一致，说明是确定性的代码缺陷，不是随机内存损坏。
4. **进程内调用 vs 进程间调用的风险差异巨大。** 能用 URI 协议/IPC 解决的，就不要把第三方/系统原生代码加载到自己进程里跑。
5. **非必现 ≠ 非确定性。** "只在部分设备出现"通常是环境条件差异，不是时序随机。找到差异维度就能解释。