# 使用 Razor 

在ASP.NET Core MVC应用程序中，视图引擎用于生成发送给客户端的内容。 默认视图引擎称为Razor，它处理带注释的HTML文件，以便将动态内容插入到发送到浏览器的输出中。
在本章中，我将快速浏览Razor语法，以便您将来可以认识Razor表达式。 本章不提供详尽的Razor参考; 这更像是一个语法上的速成课。 在我继续阅读本书的同时，在其他MVC功能的内容里，我会深入探索Razor。 表5-1列出了对Razor的基本认识。

Table 5-1 将Razor基本认识

Q:它是什么？ 
A:Razor是负责将数据合并到HTML文档中的视图引擎。

Q:它有什么用？
A:动态生成内容的能力对于能够编写Web应用程序至关重要。 Razor提供了使用C#语句与ASP.NET Core MVC的其余部分一起使用的功能。怎么用Razor表达式添加到视图文件中的静态HTML。评估表达式以生成对客户端请求的响应。
Q:是否有任何陷阱或局限性？ 
A:Razor表达式可以包含几乎任何C#语句，并且很难确定逻辑是属于视图还是属于控制器，这可能会损害对MVC模式至关重要的问题的分离。

Q:有什么替代方案吗？
A:您可以编写自己的视图引擎，正如我在第21章中所解释的。有一些第三方视图引擎可用，但它们往往对于特殊的情况下是有用的，不能吸引长期的支持。

Q:MVC 5之后有变化吗？ 
A:Razor的工作原理与MVC 5的方式大致相同，但有一些有用的改进。视图导入文件用于指定在处理视图时将搜索类型的命名空间，并且还定义了我在第23章中描述的标签助手的位置。

表5-2汇总了本章内容。

|问题|解决方案|代码|
|----|-------|----|
|访问视图模型|使用@Model表达式定义模型类型，使用@model表达式访问模型对象。|6,15,18|
|不带命名空间的前缀使用类型名|建立一个视图导入文件|7-8|
|定义被多个视图使用的内容|使用layout|9-11|
|指定默认的layout|使用视图起始文件|12-14|
|不使用视图模型从控制器传递数据到视图|使用view bag|16-17|
|选择性地生成内容|使用Razor条件表达式|19.20|
|为一个数组或集合中的每一项生成内容|使用Razor foreach表达式|21-22|

## 准备示例项目
为了演示Razor的工作原理，我创建了一个使用Empty模板的名为Razor的ASP.NET Core Web应用程序（.NET Core）项目，就像上一章一样。 我通过编辑project.json文件的依赖关系部分添加了MVC程序集，如清单5-1所示。

Listing 5-1. project.json里添加MVC引用
```cs 
...
"dependencies": {
"Microsoft.NETCore.App": {
"version": "1.0.0",
"type": "platform"
},
"Microsoft.AspNetCore.Diagnostics": "1.0.0",
"Microsoft.AspNetCore.Server.IISIntegration": "1.0.0",
"Microsoft.AspNetCore.Server.Kestrel": "1.0.0",
"Microsoft.Extensions.Logging.Console": "1.0.0",
"Microsoft.AspNetCore.Mvc": "1.0.0"
},
...
```
将更改保存到project.json文件时，Visual Studio将在项目中添加Microsoft.AspNetCore.Mvc 程序集。 接下来，我启动了在Startup.cs文件中的默认配置的MVC，如清单5-2所示。
