---
layout: post
title: Service Locator 是什么？
subtitle: 理解 Service Locator 模式及其在现代 .NET 中被视为反模式的原因
date: 2025-09-07
author: JP
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
- Service Locator
- IoC
- Dependency Injection
- Design Pattern

---

## 1. Service Locator 是什么？举个例子

### 概念类比

你可以把 Service Locator 想象成一个"大型工具箱"或者"万能电话簿"——当你需要一个服务时，不是通过参数传进来，而是去全局的"工具箱"里找。

### 简单代码示例（.NET 版）

```csharp
// 定义服务接口
public interface IMessageSender {
    void Send(string message);
}

// 实现类
public class EmailSender : IMessageSender {
    public void Send(string message) {
        Console.WriteLine($"Sending email: {message}");
    }
}

// Service Locator
public static class ServiceLocator {
    private static readonly Dictionary<Type, object> _services = new();

    public static void Register<T>(T service) {
        _services[typeof(T)] = service;
    }

    public static T Get<T>() {
        return (T)_services[typeof(T)];
    }
}

// 使用示例
public class OrderService {
    public void ProcessOrder(string order) {
        var sender = ServiceLocator.Get<IMessageSender>(); // 从全局取依赖
        sender.Send($"Order processed: {order}");
    }
}

// 注册和调用
ServiceLocator.Register<IMessageSender>(new EmailSender());
var orderService = new OrderService();
orderService.ProcessOrder("Order #123");
```

### 真实框架中的应用示例

- **Unity Container（早期用法）**：在 .NET Framework 时代，有些项目会直接通过 `UnityContainer.Resolve<T>()` 取依赖，这就是 Service Locator 模式。

- **ASP.NET MVC（早期 Global.asax 注入）**：很多老项目会在 Global.asax 里注册依赖，然后在控制器或服务里全局取。

## 2. 为什么在现代 .NET 项目中被认为是反模式？

### 现代 .NET 项目

指基于 .NET Core（2016）及之后的版本、.NET 5/6/7/8 等的新项目。

这些版本自带了轻量、标准化的 IoC 容器（`IServiceCollection` + `IServiceProvider`），提倡依赖注入（DI），而不是全局定位依赖。

框架天然支持构造函数注入，不需要手动调用 Service Locator。

### 反模式（Anti-pattern）

在软件设计里，反模式指的是一种"看似可行、但长期会带来问题"的设计方法。

**Service Locator 的问题：**

1. **隐藏依赖**：依赖关系不在构造函数中显式声明，阅读类代码时很难发现它需要哪些服务。

2. **难测试**：无法直接在构造函数传入 Mock 对象，需要修改全局注册才能测试。

3. **可维护性差**：全局状态（Static Dictionary）容易引发线程安全问题、生命周期管理混乱。

4. **违反单一职责原则**：类不仅要执行自己的业务逻辑，还要负责查找依赖。

## 3. 对比依赖注入（DI）与 Service Locator

| 特性 | Service Locator | 依赖注入（DI） |
|------|-----------------|----------------|
| 依赖声明方式 | 隐式，从全局取 | 显式，在构造函数参数声明 |
| 测试友好性 | 差，必须改全局配置 | 好，可直接传入 Mock |
| 生命周期管理 | 容易混乱，需要手动管理 | 容器自动管理 |
| 可维护性 | 依赖隐藏，读代码难发现 | 依赖透明，阅读代码即知需要哪些服务 |
| 现代 .NET 推荐度 | 不推荐（反模式） | 强烈推荐 |

### 代码对比示例

**Service Locator 方式：**

```csharp
public class OrderService {
    public void ProcessOrder(string order) {
        // 依赖是隐藏的，看不出需要什么服务
        var sender = ServiceLocator.Get<IMessageSender>();
        var logger = ServiceLocator.Get<ILogger>();
        // ... 使用服务
    }
}
```

**依赖注入方式：**

```csharp
public class OrderService {
    private readonly IMessageSender _sender;
    private readonly ILogger _logger;
    
    // 依赖明确声明，一目了然
    public OrderService(IMessageSender sender, ILogger logger) {
        _sender = sender;
        _logger = logger;
    }
    
    public void ProcessOrder(string order) {
        // 直接使用注入的依赖
        _sender.Send($"Order processed: {order}");
        _logger.Log("Order processing completed");
    }
}
```

## 4. 现代 .NET 中的依赖注入实践

在现代 .NET 项目中，推荐使用内置的依赖注入容器：

```csharp
// Program.cs 中注册服务
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddScoped<IMessageSender, EmailSender>();
builder.Services.AddScoped<ILogger, ConsoleLogger>();
builder.Services.AddScoped<OrderService>();

var app = builder.Build();

// 控制器中使用依赖注入
[ApiController]
public class OrderController : ControllerBase {
    private readonly OrderService _orderService;
    
    public OrderController(OrderService orderService) {
        _orderService = orderService; // 自动注入
    }
    
    [HttpPost]
    public IActionResult CreateOrder([FromBody] string order) {
        _orderService.ProcessOrder(order);
        return Ok();
    }
}
```

## 5. 什么时候 Service Locator 仍然有用？

虽然 Service Locator 在现代项目中不推荐作为主要的依赖解决方案，但在某些特殊场景下仍有其用途：

### 遗留系统迁移

```csharp
// 在迁移老项目时，可以作为过渡方案
public class LegacyService {
    public void DoWork() {
        // 逐步将 Service Locator 替换为构造函数注入
        var dependency = ServiceLocator.Get<IDependency>();
        // ...
    }
}
```

### 静态工厂类

```csharp
// 某些工厂模式场景
public static class ReportFactory {
    public static IReport CreateReport(ReportType type) {
        var formatter = ServiceLocator.Get<IFormatter>();
        return type switch {
            ReportType.Pdf => new PdfReport(formatter),
            ReportType.Excel => new ExcelReport(formatter),
            _ => throw new ArgumentException("Unsupported report type")
        };
    }
}
```

## 6. 总结

**Service Locator 的好处**是用起来方便，像一个万能工具箱，哪里需要就去拿。但是它的**问题是依赖关系是隐藏的**，你光看类的构造函数，根本不知道它还去全局拿了什么东西。这会让测试、维护都很麻烦。

在早期的 .NET Framework 项目里，这种方式还挺常见，但在**现代 .NET 项目**（也就是 .NET Core 及之后版本）里，这个模式几乎不再推荐，取而代之的是**依赖注入**，因为它能让依赖透明、可测试、可维护。

### 关键要点

1. **Service Locator** = 全局工具箱，需要时主动去拿
2. **依赖注入** = 构造函数明确声明需要什么，容器自动提供
3. **现代 .NET** 强烈推荐使用依赖注入而非 Service Locator
4. **可测试性**和**可维护性**是选择设计模式的重要考量因素
5. 在**遗留系统迁移**和**特殊场景**下，Service Locator 仍可作为过渡方案

### 最佳实践建议

- 新项目：直接使用依赖注入，避免 Service Locator
- 老项目：逐步重构，将 Service Locator 替换为构造函数注入
- 框架选择：优先使用 .NET 内置的 `IServiceCollection`
- 测试编写：确保依赖可以轻松 Mock，提高测试覆盖率
