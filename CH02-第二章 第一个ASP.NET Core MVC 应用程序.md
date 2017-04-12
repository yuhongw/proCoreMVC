# 第二章 第一个MVC 应用程序(待续)

学习一个软件开发框架的最好方法是跳进他的内部并使用它。在本章，你将用ASP.NET Core MVC创建一个简单的数据登录应用。我将它一步一步地展示，以便你能看清楚怎样构建一个MVC 应用程序。为了让事情简单，我跳过了一些技术细节，但是不要担心，如果你是一个MVC的新手，你将会发现许多东西足够提起你的兴趣。因为我用的东西有些没做解释，所以我提供了一些参考以便你可以看到所有的细节的东西。

## 安装Visual Studio
要想根据本书实践的话，必须安装Visual studio 2015，它提供了你需要ASP.NET Core MVC开发的所有的东西。我用的是免费的Visual Studio 2015 Community 版，你可以在www.visualstudio.com这里下载它。安装的时候，一定要确保选中了Microsoft web Web Develooper Tools 选项。

提示：Visual Studio 仅支持Windows 平台，在其他平台上，你可以使用Visual Studio Code 来开发，但是它没有提供本书中示例程序所需要的全部工具，关于这些，请参见第13章。

如果你已经安装了Visual Studio，你必须确保更到了Visual Studio Update 3，因为它才能提供ASP.NET Core 开发的支持。如果是新安装的话，Update3 会自动安装上。如果你仅需要Update, 你可以去http://go.microsoft.com/fwlink/?linkId=691129 这里下载它。

然后，你必须下载并安装.NET Core, 在这里：https://go.microsoft.com/fwlink/?LinkId=817245
。即是是全新安装的Visual Studio,你也需要下载.NET Core。

最后一步，你必须下载安装git，在这里可以下载：https://git-scm.com/download。
Visual Studio 包含了他自己版本的git,但是不太好用，并且有时候会产生难以预料的东西。例如和其他工具如Bower一起使用的时候。我在第6章会讲述Bower。当你安装git的时候，要告诉安装器，把这个工具加入PATH环境变量中，以确保Visual Studio 能够找到新版本的git。

图2-1 ，将git加入path。

启动Visual Studio ，选择Tools-> Options ，导航到项目和解决方案 -> 展开Web Tools 项，如图2-2。去掉勾选$(VSINSTALLDIR)\Web\External\Git 以让Visual Studio自带的版本失效，但是要确保$(Path)项是有效的，以使用刚刚安装的版本。

图2-2 在Visual Studio里配置git。





## 建立一个新的ASP.NET Core MVC工程(Project)

我将在Visual Studio里建立一个新的ASP.NET Core MVC工程。在Visual Studio内， 从菜单中选择New -> Project 来打开新工程的对话框。如果在左边栏中导航到 Template->Visual C#->Web项，你将看到ASP.NET Web Application(.Net Core)工程模板，选中这个工程类型，如图2-3所示。


图2-3 Visual Studio Core Web应用工程模板

提示: 选择项目模板时可能会有点困惑，因为它们几个很类似。这里解释一下：ASP.NET
Web Application (.NET Framework) 模板用于使用ASP.NET 和ASP.NET MVC框架来建立工程，它是ASP.NET Core的早期版本。另外两个模板都是用来建立ASP.NET Core 的应用，只是使用不同的运行时库。可以在.Net Framework 和.NET Core 之间选择。我将在第6章解释他们之间的不同。但是我再整本书中都使用.NET Core选项，因此他是你唯一的选择，以确保你运行例子应用程序得到正确的结果。

为新工程设置名字为PartyInvites 并且确保Add Application Insights to Project 选项不被选中。如图2-3。 点击OK 按钮继续，然后你会看到另一个对话框，如图2-4，他将让你设置项目的初始内容。

这里有三个ASP.NET Core 模板选项，每个都使用不同的初始内容建立一个工程。 对本章来说，选择Web Application选项，这个选项可以使用预定义的内容建立一个MVC 应用来开始开发。

注意: 这是唯一的一个使用Web 应用工程模板的一章。我不喜欢预定义的项目模板，因为他们鼓励开发人员象一个黑盒子一样对待一些重要的特征，例如身份验证。在这本书中，我的目标是让你懂得和管理MVC 应用的每一方面的知识，因此本书中以后的章节中，我都使用空模版来开始一个工程。在本章，我想让你快速地入门，这个模板是很合适的。

点击 Change Authentication 按钮，选择上No Authentication 选项，如图2-5。这个工程不需要身份验证，但是我会在第28-30章阐述如何保证ASP.NET 应用程序的安全。

图2-5 选择身份验证设置

点击OK 关闭 Change Authentication 对话框。 确保Host in the Cloud 没有被选中，然后单击OK ，建立PartyInvites 工程。 一旦Visual Studio 创建完工程，你将看到一些文件和文件夹在解决方案窗口中显示出来，如图2-6。这是使用Web应用程序模板创建的新MVC 项目的默认结构。你将很快就能明白这里的每一个文件和文件夹的作用。


图2-6 ASP.NET Core MVC 工程中初始的文件和文件夹结构

你可以从Debug 菜单中选择Start Debugging，(如果它提示你打开调试，点击OK 按钮就是)，你这样做了，Visual Studio 编译生成本应用程序，然后使用叫做IIS的应用程序服务器来运行它，然后打开Web浏览器请求应用的内容，你会看到下面结果，如图2-7。



图2-7 运行例子工程

当Visual Studio 使用Web应用模板来创建一个项目时，它往里面加入了基础的代码和内容，当你运行本应用时，你也能看到这些内容。接下来，在本章我将会替换工程里的内容部分来创建简单的MVC应用程序。

当你做完了上面这些步骤，如果浏览器显示错误信息，请关闭浏览器窗口以停止调试。或者回到Visual Studio，在Debug菜单里选择Stop Debugging。
就像你刚才看到的那样，Visual Studio打开浏览器以显示工程。你可以在Web Browser菜单中，单击IIS Express的右侧箭头并从选项列表中选择浏览器的方式选择任何一个你已经安装的浏览器。如图2-8。


图2-8 选择一个浏览器

从这里开始，我将在本书中使用Google Chrome 或Google Chrome Canary 来截屏。但是你可以使用任何现代浏览器来显示本书中的例子，包括Microsoft Edge 和最新版的Internet Explorer。

## 加入控制器(Controller)

在MVC 模式里，传入的请求被控制器(Controller)处理，在ASP.NET Core MVC,控制器只是一个类，同城继承自内建的MVC控制器基类Microsoft.AspNetCore.Mvc.Controller。
控制器中每一个公共方法都叫做动作(action)方法。意思是你可以通过一些URL从Web上英法一个动作。按照MVC的传统约定，控制器类一般都放入controllers文件夹内，这个文件夹是由Visual Studio 在创建工程的时候建立的。

提示：你不必非得遵循MVC的约定，但是我建议你这样做，因为它更合理一些。

Visual Studio 在项目里加入了默认的Controller 类，如果你在解决方案浏览器窗口扩展开Controllers 文件夹就会看到。默认的Controller叫做HomeController.cs。控制器类名字是以Controller结尾的，意思是当你看到HomeController.cs文件时，就知道他是一个叫做Home的控制器(Controller)，HomeController是MVC应用的默认控制器。单击HomeController.cs 文件你可以编辑它。你会看到List 2-1所示的代码。

List 2-1. HomeController的初始内容
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
namespace PartyInvites.Controllers {
public class HomeController : Controller {
public IActionResult Index() {
return View();
}
public IActionResult About() {
ViewData["Message"] = "Your application description page.";
return View();
}
public IActionResult Contact() {
ViewData["Message"] = "Your contact page.";
return View();
}
public IActionResult Error() {
return View();
}
}
}
用下面List 2-2的内容替换HomeController.cs文件中原来的代码。这里，我只保留了一个方法，更改了结果类型和他的实现，并移除了那些不需要的using语句。

Listing 2-2. 更改 HomeController.cs 
using Microsoft.AspNetCore.Mvc;
namespace PartyInvites.Controllers {
public class HomeController : Controller {
public string Index() {
return "Hello World";
}
}
}

这些更改没有动态效果，但是是一个很好的演示。我已经更改了Index方法让他返回“Hello World”。现在，点击Debug -> Start Debugging菜单再次运行项目。

提示:如果及在前面的一节一直保持应用程序在运行状态，你需要选择 Debugging -> Restart菜单。当然，如果你愿意，你就选择停止调试然后再开始调试。

浏览器将会发起一个HTTP请求到服务器。默认的MVC配置规定了这个请求将被Index方法（叫做Action方法或就叫做Action）处理，该方法的结果会发回之浏览器，如图2-9显示的那样。


图2-9  Action 方法的输出

提示：请注意，图中Visual Studio已经将浏览器指向了端口57628。你看到你的浏览器上的URL端口号几乎肯定与图上不同。因为VS在创建工程的时候会随机分配端口。如果你看到Windows任务条上通知区域，你会发现一个IIS Express 的图标，它是一个简化的IIS 应用程序服务器的版本，随着VS 一起来的，用于在开发的时候交付ASP.NET 内容和提供服务。在第12章，我将会告诉你如何发布一个MVC工程到生产环境。

## 理解路由
除了Model,View和控制器，MVC 应用也会使用ASP.NET 路由系统，它合同决定URL如何映射到Controller和Action。路由是一个用来决定如和处理请求的规则。当VS创建MVC项目时，它帮你加了一些默认的路由。你可以请求下列URL，它们都会指向HomeController中的 Index action 。
?/
?/Home
?/Home/Index

因此，当一个浏览器请求http://yoursite/ 或者 http://yoursite/Home 它都会从HomeController 的Index方法返回结果。 你可以自己试一下在浏览器中改变URL，假设现在是http://localhost:57628,如果你在URL后面添加 /Home 或 /Home/Index 然后按回车键，你将会看见同样的Hello World 结果。

这是一个遵循ASP.NET Core MVC实现的约定的很好的例子，这个约定是：我将有一个叫做HomeController的控制器，它是MVC应用的入口点。 默认的VS 配置中，当它要创建一个新的MVC应用时，它会假定我们遵循这个约定。因为我遵循这个约定，我会自动的到前面列表的支持，如果我不遵循这个约定，我将必须修改配置以指向其他的Controller。 对于这个简单的例子来说，默认的配置是我所需要的。

## 渲染Web页面
前面例子里输出的不是HTML，他只是简单的”Hello World” 字符串。要想给浏览器产生HTML相应，我需要一个View，它将会告诉MVC如何为一个请求生成响应。
建立并渲染一个View
第一件事我需要做的是修改Index action方法，如程序2-3。我做的改动用加粗的字体显示了，也是使用了一个让例子变得很简单的约定。

Listing 2-3. 修改Action
using Microsoft.AspNetCore.Mvc;
namespace PartyInvites.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
return View("MyView");
}
}
}
当我从一个Action方法返回ViewResult 对象时，我指示MVC去渲染一个view。我通过调用View方法来创建一个ViewResult对象，调用时要制定View的名字MyView。如果你运行这个应用程序，你会发现MVC会试图寻找这个View，会出现如下图2-10所显示的错误信息。



图2-10 MVC试图寻找一个view

这个错误信息是非常有帮助的。 不仅解释了MVC找不到我在action方法里指定的View，也告诉了去哪找的。所有的View保存在Views文件夹，组织成子文件夹。例如，Home controller 里关联的View，会保存在叫做Views/Home的文件夹中。如果一个View没有固定的Controller,它将会保存在Views/shared文件夹。VS 会在Web 应用程序模板被使用的时候创建Home和Shared文件夹，并放入一些占位的View 以使工程能够启动。

要建立View，在解决方案浏览器窗口内，右键单击Views->Home，并在弹出菜单中选择Add -> New Item。VS 汇给你展示出来一些项模板，在左侧栏选择ASP.NET 类别然后在中间栏选择MVC View Page 项。如图2-11。

图2-11 建立View

提示: 在Views文件夹里你会看见一些文件，他们是VS 添加进来的用来提供就像图2-7显示的那样的初始内容。你可以忽略那些文件。

设置名字字段为MyView.cshtml 然后单击Add 按钮来建立View。VS将会建立Views/Home/MyView.cshtml文件然后打开它让你编辑。初始的时候View文件的内容只是一些注释和占位符，请用代码2-4替换他们。

提示:很容易在错误的文件夹中创建view文件，如果你没有最终在Views/Home里创建MyView.cshtml，将他们删除然后再创建就可以。

Listing 2-4. 替换 MyView.cshtml 文件内容

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
<div>
Hello World (from the view)
</div>
</body>
</html>
The new contents of the view file are mostly HTML. The exception is the part that looks like this:
...
@{
Layout = null;
}
...

新的View文件中的内容大部分都是HTML,除了这一部分：
@{
Layout = null;
}
这是一个被Razor View引擎解释的表达式，它会处理View的内容并生成发回给浏览器中的HTML。这是一个简单的Razor表达式，他会告诉Razor我选择不使用layout。Layout是一个HTML模板，我将在第五章讲解它。我将忽略Razor，过一会在回来。想要看效果，请点击Start Debugging 来运行应用程序。你将会看到图2-12中的结果。

图2-12 测试View
当我第一次编辑Index action 方法时，它返回一个字符串，也就是MVC会传递字符串值给浏览器，除此之外不会做别的。现在，Index方法返回一个ViewResult,MVC渲染一个视图（view）并返回它产生的HTML。我告诉了MVC该使用哪一个视图，所以它使用命名约定自动找到了它。这个约定就是视图有与Action相同的名字并且保存在以控制器命名的文件夹里。

除了字符串和ViewResult ,我还可以让Action方法返回其它类型。例如如果我返回一个RedirectResult,浏览器将重定向到另一个URL。如果我返回一个HttpUnauthorizedResult，I会强迫用户登录。这些对象被叫做Action result的集合控制。Action Result系统让你封装并重用通用的action返回。在17章，我会介绍更多的相关内容。

## 增加动态输出

整个web应用平台的关注点在于构建并显示动态输出内容。在MVC里，控制器负责构建一些数据并将其传给视图。视图负责渲染成HTML。
从控制器向视图传递数据的一种方式是使用ViewBag 对象，它是一个控制器基类的成员。ViewBag是一个动态对象，你可以给他赋值任意属性给视图来渲染用。代码2-5 演示了如何在HomeController里传递简单对象。
Listing 2-5. 设置视图数据
using System;
using Microsoft.AspNetCore.Mvc;
namespace PartyInvites.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
int hour = DateTime.Now.Hour;
ViewBag.Greeting = hour < 12 ? "Good Morning" : "Good Afternoon";
return View("MyView");
}
}
}

我向ViewBag.Greeting属性赋值，以给视图提供数据。Greeting属性在赋值之前是不存在的，这允许我以任意流畅的方式从控制器向视图传递数据而不必在赋值之前定义类。我在视图中引用了ViewBag.Greeting属性以获得他的值。如同代码2-6，这是修改后的MyView.cshtml。

Listing 2-6. 在视图里获取传递过来的值
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
<div>
@ViewBag.Greeting World (from the view)
</div>
</body>
</html>

上面代码增加的部分是Razor表达式，它在MVC 使用视图生成相应的时候求值。当我在控制器内调用View方法的时候，MVC找到MyView.cshtml文件并请求Razor 视图引擎解析文件的内容。Razor 会查找象上面代码中的表达式。在本例中，处理表达式的意思是将ViewBag.Greeting属性插入到视图中。

Greeting这个属性名字没有什么特殊的东西，你可以使用任何其他的名字，并且一样好用。只要你在控制器中的名字与视图中的名字相同即可。你可以使用多个属性来传递多个数据。然后你运行一下看一下效果，如图2-13。

图2-13 一个MVC的动态响应

## 创建一个简单的数据录入