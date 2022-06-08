---
layout: post
title: 如何在 Prism 8 WPF 应用程序中注册日志服务
subtitle:   
date:   2021-10-27
author: JP
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
- WPF
- Prism
- Serilog
---

日志记录是几乎每个应用程序的重要组成部分。但是，在为 WPF 编写 Prism 应用程序时，很难找到有关如何使用 IoC 容器准确注册日志记录的文档。在这篇文章中，我们将向 DryIoC 或 Unity 添加日志记录（使用 Serilog 和 Microsoft Logging Abstractions）。这种方法适用于 Prism 7 以上。


### 必要的 Nuget 包

您首先必须将以下 NuGet 包添加到您的启动项目（包含 App.xaml 和 App.xaml 的包.cs）：

- [Microsoft.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/)
- [Microsoft.Extensions.DependencyInjection](https://www.nuget.org/packages/Microsoft.Extensions.DependencyInjection)   您需要它来使用 IServiceCollection，它有助于注册日志。

您还需要为您选择的日志框架包，我们将在这个例子中使用 Serilog，所以包括
- [Serilog Extensions Logging](https://www.nuget.org/packages/Serilog.Extensions.Logging/)

但是，您可以轻松地将其替换为 NLog 或任何其他日志记录框架。

并且为了将 IServiceCollection 添加到您的 IOC 容器中，您需要

- [DryIoc.Microsoft.DependencyInjection](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection) 为DryIoc 或者
- [Unity.Microsoft.DependencyInjection](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection) 为Unity


### 向容器注册日志

要将日志记录框架注册到 IoC 容器，请将以下覆盖添加到 App.xaml.cs 并从之前添加的 nuget 包中导入所有相关使用：

#### DryIoC

```c#
protected override IContainerExtension CreateContainerExtension()
{
    var serviceCollection = new ServiceCollection();
    serviceCollection.AddLogging(loggingBuilder =>
        loggingBuilder.AddSerilog(dispose: true));

    return new DryIocContainerExtension(new Container(CreateContainerRules())
        .WithDependencyInjectionAdapter(serviceCollection));
}
```

在这里，我们首先创建 ServiceCollection，通过调用... AddLogging() 可以非常轻松地添加日志记录！如果您更喜欢 Nlog 或 Log4net 或设置最小日志记录级别等，这也是添加另一个日志记录框架的地方。

然后我们必须确保在 ServiceCollection 注册的服务也在应用程序其余部分使用的 DryIoC Container 中注册。为此，我们使用了 DryIoc.Microsoft.DependencyInjection 中的 WithDependencyInjectionAdapter 扩展方法。

#### Unity

```c#
protected override IContainerExtension CreateContainerExtension()
{
    var serviceCollection = new ServiceCollection();
    serviceCollection.AddLogging(loggingBuilder =>
        loggingBuilder.AddSerilog(dispose: true));

    var container = new UnityContainer();
    container.BuildServiceProvider(serviceCollection);

    return new UnityContainerExtension(container);
}
```

代码与 DryIoC 情况几乎相同，但 Unity.Microsoft.DependencyInjection 的工作方式略有不同，因此您必须调用 BuildServiceProvider。

### 配置日志

在创建 Shell 之前配置记录器，例如将以下代码添加到 CreateShell：

```c#
protected override Window CreateShell()
{
    Log.Logger = new LoggerConfiguration()
        .Enrich.FromLogContext()
        .WriteTo.Debug()
        .CreateLogger();

    return Container.Resolve<MainWindow>();
}
```

这只是一个基本配置，更多信息可以在[项目页面](https://serilog.net/)上找到。为了能够使用 Debug 作为接收器，您需要 [Serilog.Sinks.Debug](https://www.nuget.org/packages/Serilog.Sinks.Debug/)。

### 使用日志

在您的服务中，您现在可以注入通用 ILogger，例如

```c#
public class MyService : IMyService
{
    public MyService(ILogger<MyService> logger)
    {
        logger.LogInformation("Hello World from your logger!");
    }
}
```


翻译👉 [Logging-In-Prism](https://www.andicode.com/prism/wpf/logging/2021/05/21/Logging-In-Prism.html)
