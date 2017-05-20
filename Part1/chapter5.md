# 使用 Razor 

在ASP.NET Core MVC应用程序中，视图引擎用于生成发送给客户端的内容。 默认视图引擎称为Razor，它处理带注释的HTML文件，以便将动态内容插入到发送到浏览器的输出中。
在本章中，我将快速浏览Razor语法，以便您将来可以认识Razor表达式。 本章不提供详尽的Razor参考; 这更像是一个语法上的速成课。 在我继续阅读本书的同时，在其他MVC功能的内容里，我会深入探索Razor。 表5-1列出了对Razor的基本认识。

Table 5-1 Razor基本认识

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

Listing 5-2. 在Startup.cs 里打开MVC功能
```cs

using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;
namespace Razor 
{
    public class Startup 
    {
        public void ConfigureServices(IServiceCollection services) 
        {
            services.AddMvc();
        }

        public void Configure(IApplicationBuilder app, IHostingEnvironment env,ILoggerFactory loggerFactory) 
        {
            app.UseMvcWithDefaultRoute(); 
        }
    }
}
```

## 定义模型

接下来，创建了一个Models文件夹，并添加了一个名为Product.cs的类文件，用于定义如清单5-3所示的简单模型类。

Listing 5-3. Product.cs
```cs
namespace Razor.Models {
public class Product {
        public int ProductID { get; set; }
        public string Name { get; set; }
        public string Description { get; set; }
        public decimal Price { get; set; }
        public string Category { set; get; }
    }
}
```

## 建立控制器
在Startup.cs文件中设置的默认配置遵循默认情况下向控制器发送请求的MVC约定。 我创建了一个Controllers文件夹，并添加了一个名为HomeController.cs的类文件，我用于定义如清单5-4所示的简单控制器。

Listing 5-4. HomeController.cs
``` cs
using Microsoft.AspNetCore.Mvc;
using Razor.Models;
namespace Razor.Controllers {
    public class HomeController : Controller {
        public ViewResult Index() {
            Product myProduct = new Product {
                ProductID = 1,
                Name = "Kayak",
                Description = "A boat for one person",
                Category = "Watersports",
                Price = 275 M
            };
            return View(myProduct);
        }
    }
}
```

控制器定义了一个称为Index的Action方法，我创建并填充Product对象的属性。 将产品传递给View方法，以便在渲染视图时将其用作模型。 当调用View方法时，不指定视图文件的名称，因此将使用action方法的默认视图。

## 建立视图

要创建Index Action方法的默认视图，我创建了一个Views/Home文件夹，并添加了一个名为Index.cshtml的MVC视图页面文件，添加了清单5-5所示的内容。

Listing 5-5. Index.cshtml
``` html
@model Razor.Models.Product
@{
    Layout = null;
}
<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>Index</title>
</head>
<body>
        Content will go here
</body>
</html>
```

在下面的章节中，我将介绍一下Razor视图的不同部分，并展示一些您可以使用的不同内容。 在了解Razor时，记住存在用于向用户表达模型的一个或多个部分的视图，这意味着生成显示从一个或多个对象检索的数据的HTML。 如果你记得我总是试图建立一个可以发送给客户端的HTML页面，那么Razor所做的一切都开始有意义。 如果你运行
应用程序，您将看到如图5-1所示的简单输出。

图 5-1 运行实例应用程序

## 使用Model对象
我们先看看Index.cshtml视图的第一行：

@model Razor.Models.Product

Razor表达式以@字符开头。 在这种情况下，@model表达式将声明从action方法传递给视图的模型对象的类型。 这允许我通过@Model引用视图模型对象的方法，字段和属性，如清单5-6所示，它显示了Index视图的简单内容。

Listing 5-6. 在视图中引用Model对象属性
```html
@model Razor.Models.Product
@{
    Layout = null;
}
<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>Index</title>
</head>
<body>
    @Model.Name
</body>
</html>
```

请注意，我使用@ model（小写m）声明视图模型对象类型，并使用@Model（大写字母M）访问Name属性。 当您开始与Razor合作时，这很有点混乱，但你很快就会习惯它了。

如果你此时运行应用，你会看到如图5-2输出。

图5-2 在视图里读取属性值


使用@model表达式指定类型的视图称为强类型视图。 Visual Studio能够使用@model表达式在您键入@Model后跟一个句点时弹出成员名称的建议，如图5-3所示。


图5-3 Visual Studio 基于@Model表达式提供成员名字建议的提示。


Visual Studio对成员名称的建议有助于避免Razor视图中的错误。 您可以忽略这些建议，如果您愿意，Visual Studio将突出显示成员名称的问题，以便您进行更正，就像常规C#类文件一样。 您可以看到图5-4中的问题突出显示的示例，其中我试图引用@Model.NotARealProperty。 Visual Studio已经意识到，我在模型类型中指定的Product类没有这样的属性，并在编辑器中突出显示错误。

图5-4 Visual Studio报告@Model表达式的问题

## 使用视图导入(Imports)

当我在Index.cshtml文件的开头定义模型对象时，我必须包括包含模型类的命名空间，如下所示：

@model Razor.Models. Product

默认情况下，强类型Razor视图中引用的所有类型必须使用命名空间进行限定。 当模型对象的唯一类型引用是不重要的，但是当编写更复杂的Razor表达式（如本章稍后描述的）时，它可能会使读者更难阅读。
您可以通过将视图导入文件添加到项目来指定一组应该搜索类型的命名空间。 视图导入文件放在Views文件夹中，名为 ViewImports.cshtml。

注意: 名称以下划线（_）开头的“视图”文件夹中的文件不会返回给用户，这样可以使文件名区分要渲染的视图和支持它们的文件。 查看导入的文件和布局（我稍后描述）前缀为下划线。

要创建视图导入文件，请在解决方案资源管理器中右键单击Views文件夹，从弹出菜单中选择Add➤New Item，然后从ASP.NET类别中选择MVC View Imports Page模板，如图5-5所示。
Visual Studio将自动将文件的名称设置为ViewImports.cshtml，然后单击添加按钮将创建该文件。 添加如表5-7所示的表达式。

图5-5 建立视图导入文件

Listing 5-7 _ViewIimports.cshtml

@using Razor.Models

应该使用Razor视图中使用的类搜索的命名空间使用@using表达式，后跟命名空间来指定。 在清单5-7中，我添加了一个Razor.Models命名空间的条目，其中包含示例应用程序中的模型类。
现在，Razor.Models命名空间包含在视图导入文件中，我可以从Index.cshtml文件中删除命名空间，如清单5-8所示。

Listing 5-8. 引用没有命名空间的模型类
```html
@model Product
@{
    Layout = null;
}
<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>Index</title>
</head>
<body>
    @Model.Name
</body>
</html>
```
提示: 您还可以将@using表达式添加到单个视图文件中，这允许在单个视图中使用不带命名空间的类型。

##使用布局(Layouts)

Index.cshtml视图文件中还有一个重要的Razor表达式：
...
@ {
Layout = null;
}
...
这是一个Razor代码块的例子，它允许我在视图中包含C#语句。代码块用@{打开，并用}关闭，并且在渲染视图时对其包含的语句进行求值。
该代码块将Layout属性的值设置为null。 Razor视图在MVC应用程序中被编译成C#类，使用的基类定义了Layout属性。我在第21章会告诉你如何工作，但是将Layout属性设置为null的效果是告诉MVC该视图是自包含的
并将呈现客户端所需的所有内容。
独立的视图适用于简单的示例应用程序，但是真正的项目可以有数十个视图，一些视图将具有共享内容。在视图中复制共享内容变得很难管理，特别是当您需要进行更改时，必须跟踪所有需要更改的视图。
更好的方法是使用Razor布局，它是一个包含常见内容并可应用于一个或多个视图的模板。当您对布局进行更改时，更改将自动影响所有使用它的视图。

##建立布局

布局通常由多个控制器使用的视图共享，并存储在名为Views/Shared的文件夹中，该文件夹是Razor在尝试查找文件时查找的位置之一。 要创建布局，请创建Views/Shared文件夹，右键单击它，然后从弹出菜单中选择Add➤New Item。 从ASP.NET类别中选择MVC视图布局页面模板，并将文件名设置为BasicLayout.cshtml，如图5-6所示。 单击添加按钮创建文件。 （像查看导入文件一样，布局文件的名称以下划线开头。）
Visual Studio在创建文件时添加的BasicLayout.cshtml文件的初始内容如表5-9所示。


图5-6 创建一个布局

Listing 5-9. _BasicLayout.cshtml 初始内容
```html 
<!DOCTYPE html>
<html>
    <head>
        <meta name="viewport" content="width=device-width" />
        <title >@ViewBag.Title </title>
    </head>
    <body>
        <div>
            @RenderBody()
        </div>
    </body>
</html>
```
布局是一种专门的视图。 对@RenderBody方法的调用将由action方法指定的视图的内容插入到布局标记中。 布局中的另一个Razor表达式查找名为ViewBag.Title的属性，以设置标题元素的内容。 ViewBag是一个方便的功能，允许数据值在应用程序之间传递，在这种情况下，可以在视图和布局之间传递。 当我将布局应用于视图时，您将看到它如何工作。
布局中的HTML元素将应用于使用它的任何视图，提供定义常见内容的模板。 在清单5-10中，我已经为布局添加了一些简单的标记，使得它的模板效果显而易见。

Listing 5-10. _BasicLayout.cshtml 增加一些内容
``` html
<!DOCTYPE html>
<html>
    <head>
        <meta name="viewport" content="width=device-width" />
        <title>@ViewBag.Title</title>
        <style>
            #mainDiv {
            padding: 20px;
            border: solid medium black;
            font-size: 20 pt
            }
        </style>
    </head>
    <body>
        <h1>Product Information</h1>
        <div id="mainDiv">
            @RenderBody()
        </div>
    </body>
</html>
```

我添加了一个标题元素以及一些CSS来为包含@RenderBody表达式的div元素的内容设置样式，只是为了清楚布局中的内容以及视图中的内容。

## 应用布局

要将布局应用于视图，我需要设置Layout属性的值，并删除现在将由布局提供的HTML，例如html，head和body元素，如清单5-11所示。

Listing 5-11. 在Index.cshtml中应用布局
``` html 
@model Product
@{
Layout = "_BasicLayout";
ViewBag.Title = "Product Name";
}
Product Name: @Model.Name
```
Layout属性指定将用于视图的布局文件的名称，而不使用cshtml文件扩展名。 Razor将在/ Views / Home和Views / Shared文件夹中查找指定的布局文件。
我还在列表中设置了ViewBag.Title属性。 当渲染视图时，这将由布局使用来设置标题元素的内容。

视图的转变是戏剧性的，即使是这样一个简单的应用程序。 布局包含任何HTML响应所需的所有结构，这将使视图仅关注将数据呈现给用户的动态内容。 当MVC处理Index.cshtml文件时，它将应用布局来创建统一的HTML响应，如图5-7所示。

图5-7 视图应用布局的效果

## 使用View Start文件

我仍然需要再简单处理一下:必须在每个视图中指定我想要的布局文件。
因此，如果我需要重新命名布局文件，我将不得不查找引用它的每个视图，并进行更改，这将是一个容易出错的过程，并且与MVC开发的简单维护的理念相违背。
可以通过使用视图启动文件来解决这个问题。 当它呈现视图时，MVC会查找一个名为ViewStart.cshtml的文件。 该文件的内容将被视为包含在视图文件本身中，我可以使用此功能自动设置布局属性的值。
要创建视图启动文件，请右键单击Views文件夹，从弹出菜单中选择Add➤New Item，然后从ASP.NET类别中选择MVC View Start Page模板，如图5-8所示。

图5-8 创建一个视图启动文件

Visual Studio将自动将文件的名称设置为ViewStart.cshtml，然后单击添加按钮将创建具有清单5-12所示初始内容的文件。

Listing 5-12. _ViewStart.cshtml 文件的初始内容
@{
Layout = "_Layout";
}

要将我的布局应用于应用程序中的所有视图，我更改了分配给Layout属性的值，如清单5-13所示。

Listing 5-13. 在_ViewStart.cshtml应用默认视图
@{
Layout = "_BasicLayout";
}

由于视图起始文件包含Layout属性的值，因此我可以从Index.cshtml文件中删除相应的表达式，如清单5-14所示。

Listing 5-14. 更新Index.cshtml以使用视图启动文件
@model Product
@{
ViewBag.Title = "Product Name";
}
Product Name: @Model.Name


我不必指定我想使用视图启动文件。 MVC将定位文件并自动使用其内容。 视图文件中定义的值优先，这样可以轻松覆盖视图启动文件。
您还可以使用多个视图启动文件为应用程序的不同部分设置默认值。 Razor将查找正在处理的视图的最接近的视图启动文件，这意味着您可以通过将视图启动文件添加到Views/Home或Views/Shared文件夹来覆盖默认设置。

注意: 了解从视图文件中省略Layout属性并将其设置为null之间的区别很重要。 如果您的视图是自包含的，并且不想使用布局，则将Layout属性设置为null。 如果省略了Layout属性，那么MVC将假设您需要一个布局，并且应该使用它在视图起始文件中找到的值。



