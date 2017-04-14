# 第二章 第一个MVC 应用程序

学习一个软件开发框架的最好方法是跳进他的内部并使用它。在本章，你将用ASP.NET Core MVC创建一个简单的数据登录应用。我将它一步一步地展示，以便你能看清楚怎样构建一个MVC 应用程序。为了让事情简单，我跳过了一些技术细节，但是不要担心，如果你是一个MVC的新手，你将会发现许多东西足够提起你的兴趣。因为我用的东西有些没做解释，所以我提供了一些参考以便你可以看到所有的细节的东西。

## 安装Visual Studio
要想根据本书实践的话，必须安装Visual studio 2015，它提供了你需要ASP.NET Core MVC开发的所有的东西。我用的是免费的Visual Studio 2015 Community 版，你可以在www.visualstudio.com这里下载它。安装的时候，一定要确保选中了Microsoft web Web Develooper Tools 选项。

提示：Visual Studio 仅支持Windows 平台，在其他平台上，你可以使用Visual Studio Code 来开发，但是它没有提供本书中示例程序所需要的全部工具，关于这些，请参见第13章。

如果你已经安装了Visual Studio，你必须确保更到了Visual Studio Update 3，因为它才能提供ASP.NET Core 开发的支持。如果是新安装的话，Update3 会自动安装上。如果你仅需要Update, 你可以去http://go.microsoft.com/fwlink/?linkId=691129 这里下载它。

然后，你必须下载并安装.NET Core, 在这里：https://go.microsoft.com/fwlink/?LinkId=817245
。即是是全新安装的Visual Studio,你也需要下载.NET Core。

最后一步，你必须下载安装git，在这里可以下载：https://git-scm.com/download。
Visual Studio 包含了他自己版本的git,但是不太好用，并且有时候会产生难以预料的东西。例如和其他工具如Bower一起使用的时候。我在第6章会讲述Bower。当你安装git的时候，要告诉安装器，把这个工具加入PATH环境变量中，以确保Visual Studio 能够找到新版本的git。

![将git加入path](/imgs/fig.2-1.png)

图2-1 将git加入path。

启动Visual Studio ，选择Tools-> Options ，导航到项目和解决方案 -> 展开Web Tools 项，如图2-2。去掉勾选$(VSINSTALLDIR)\Web\External\Git 以让Visual Studio自带的版本失效，但是要确保$(Path)项是有效的，以使用刚刚安装的版本。

![配置git](/imgs/fig.2-2.png)

图2-2 在Visual Studio里配置git。

关于ASP.NET Core MVC和Visual Studio 的未来
微软预估了创建ASP.NET Core 和 ASP.NET Core MVC 需要多长时间。原计划是在Visual Studio2015里一起发布的，但是在ASP.NET上延迟了。我写这本书的时候，微软已经开始了下一版本的Visual Studio的开发。这意味着在下一个版本Visual Studio发行的时候，支持开发ASP.NET Core MVC应用的工具会有所变化。如果工具稳定了，我会提供创建例子程序的更新说明，请关注Apress.com 中本书的网页。

## 建立一个新的ASP.NET Core MVC工程(Project)

我将在Visual Studio里建立一个新的ASP.NET Core MVC工程。在Visual Studio内， 从菜单中选择New -> Project 来打开新工程的对话框。如果在左边栏中导航到 Template->Visual C#->Web项，你将看到ASP.NET Web Application(.Net Core)工程模板，选中这个工程类型，如图2-3所示。

![应用工程模板](/imgs/fig.2-3.png)
图2-3 Visual Studio Core Web应用工程模板

提示: 选择项目模板时可能会有点困惑，因为它们几个很类似。这里解释一下：ASP.NET
Web Application (.NET Framework) 模板用于使用ASP.NET 和ASP.NET MVC框架来建立工程，它是ASP.NET Core的早期版本。另外两个模板都是用来建立ASP.NET Core 的应用，只是使用不同的运行时库。可以在.Net Framework 和.NET Core 之间选择。我将在第6章解释他们之间的不同。但是我再整本书中都使用.NET Core选项，因此他是你唯一的选择，以确保你运行例子应用程序得到正确的结果。

为新工程设置名字为PartyInvites 并且确保Add Application Insights to Project 选项不被选中。如图2-3。 点击OK 按钮继续，然后你会看到另一个对话框，如图2-4，他将让你设置项目的初始内容。

![选择初始项目配置](/imgs/fig.2-4.png)
图2-4 选择初始项目配置

这里有三个ASP.NET Core 模板选项，每个都使用不同的初始内容建立一个工程。 对本章来说，选择Web Application选项，这个选项可以使用预定义的内容建立一个MVC 应用来开始开发。

注意: 这是唯一的一个使用Web 应用工程模板的一章。我不喜欢预定义的项目模板，因为他们鼓励开发人员象一个黑盒子一样对待一些重要的特征，例如身份验证。在这本书中，我的目标是让你懂得和管理MVC 应用的每一方面的知识，因此本书中以后的章节中，我都使用空模版来开始一个工程。在本章，我想让你快速地入门，这个模板是很合适的。

点击 Change Authentication 按钮，选择上No Authentication 选项，如图2-5。这个工程不需要身份验证，但是我会在第28-30章阐述如何保证ASP.NET 应用程序的安全。

![选择身份验证设置](/imgs/fig.2-5.png)

图2-5 选择身份验证设置

点击OK 关闭 Change Authentication 对话框。 确保Host in the Cloud 没有被选中，然后单击OK ，建立PartyInvites 工程。 一旦Visual Studio 创建完工程，你将看到一些文件和文件夹在解决方案窗口中显示出来，如图2-6。这是使用Web应用程序模板创建的新MVC 项目的默认结构。你将很快就能明白这里的每一个文件和文件夹的作用。

![初始文件和文件夹](/imgs/fig.2-6.png)

图2-6 ASP.NET Core MVC 工程中初始的文件和文件夹结构

你可以从Debug 菜单中选择Start Debugging，(如果它提示你打开调试，点击OK 按钮就是)，你这样做了，Visual Studio 编译生成本应用程序，然后使用叫做IIS的应用程序服务器来运行它，然后打开Web浏览器请求应用的内容，你会看到下面结果，如图2-7。

![运行](/imgs/fig.2-7.png)

图2-7 运行例子工程

当Visual Studio 使用Web应用模板来创建一个项目时，它往里面加入了基础的代码和内容，当你运行本应用时，你也能看到这些内容。接下来，在本章我将会替换工程里的内容部分来创建简单的MVC应用程序。

当你做完了上面这些步骤，如果浏览器显示错误信息，请关闭浏览器窗口以停止调试。或者回到Visual Studio，在Debug菜单里选择Stop Debugging。
就像你刚才看到的那样，Visual Studio打开浏览器以显示工程。你可以在Web Browser菜单中，单击IIS Express的右侧箭头并从选项列表中选择浏览器的方式选择任何一个你已经安装的浏览器。如图2-8。

![选择浏览器](/imgs/fig.2-8.png)
图2-8 选择一个浏览器

从这里开始，我将在本书中使用Google Chrome 或Google Chrome Canary 来截屏。但是你可以使用任何现代浏览器来显示本书中的例子，包括Microsoft Edge 和最新版的Internet Explorer。

## 加入控制器(Controller)

在MVC 模式里，传入的请求被控制器(Controller)处理，在ASP.NET Core MVC,控制器只是一个类，同城继承自内建的MVC控制器基类Microsoft.AspNetCore.Mvc.Controller。
控制器中每一个公共方法都叫做动作(action)方法。意思是你可以通过一些URL从Web上英法一个动作。按照MVC的传统约定，控制器类一般都放入controllers文件夹内，这个文件夹是由Visual Studio 在创建工程的时候建立的。

提示：你不必非得遵循MVC的约定，但是我建议你这样做，因为它更合理一些。

Visual Studio 在项目里加入了默认的Controller 类，如果你在解决方案浏览器窗口扩展开Controllers 文件夹就会看到。默认的Controller叫做HomeController.cs。控制器类名字是以Controller结尾的，意思是当你看到HomeController.cs文件时，就知道他是一个叫做Home的控制器(Controller)，HomeController是MVC应用的默认控制器。单击HomeController.cs 文件你可以编辑它。你会看到List 2-1所示的代码。

List 2-1. HomeController的初始内容
```cs
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
```
用下面List 2-2的内容替换HomeController.cs文件中原来的代码。这里，我只保留了一个方法，更改了结果类型和他的实现，并移除了那些不需要的using语句。

Listing 2-2. 更改 HomeController.cs 
```cs
using Microsoft.AspNetCore.Mvc;
namespace PartyInvites.Controllers {
public class HomeController : Controller {
public string Index() {
return "Hello World";
}
}
}
```

这些更改没有动态效果，但是是一个很好的演示。我已经更改了Index方法让他返回“Hello World”。现在，点击Debug -> Start Debugging菜单再次运行项目。

提示:如果及在前面的一节一直保持应用程序在运行状态，你需要选择 Debugging -> Restart菜单。当然，如果你愿意，你就选择停止调试然后再开始调试。

浏览器将会发起一个HTTP请求到服务器。默认的MVC配置规定了这个请求将被Index方法（叫做Action方法或就叫做Action）处理，该方法的结果会发回之浏览器，如图2-9显示的那样。

![Action输出](/imgs/fig.2-9.png)

图2-9  Action 方法的输出

提示：请注意，图中Visual Studio已经将浏览器指向了端口57628。你看到你的浏览器上的URL端口号几乎肯定与图上不同。因为VS在创建工程的时候会随机分配端口。如果你看到Windows任务条上通知区域，你会发现一个IIS Express 的图标，它是一个简化的IIS 应用程序服务器的版本，随着VS 一起来的，用于在开发的时候交付ASP.NET 内容和提供服务。在第12章，我将会告诉你如何发布一个MVC工程到生产环境。

## 理解路由
除了Model,View和控制器，MVC 应用也会使用ASP.NET 路由系统，它合同决定URL如何映射到Controller和Action。路由是一个用来决定如和处理请求的规则。当VS创建MVC项目时，它帮你加了一些默认的路由。你可以请求下列URL，它们都会指向HomeController中的 Index action 。

* /
* /Home
* /Home/Index

因此，当一个浏览器请求http://yoursite/ 或者 http://yoursite/Home 它都会从HomeController 的Index方法返回结果。 你可以自己试一下在浏览器中改变URL，假设现在是http://localhost:57628,如果你在URL后面添加 /Home 或 /Home/Index 然后按回车键，你将会看见同样的Hello World 结果。

这是一个遵循ASP.NET Core MVC实现的约定的很好的例子，这个约定是：我将有一个叫做HomeController的控制器，它是MVC应用的入口点。 默认的VS 配置中，当它要创建一个新的MVC应用时，它会假定我们遵循这个约定。因为我遵循这个约定，我会自动的到前面列表的支持，如果我不遵循这个约定，我将必须修改配置以指向其他的Controller。 对于这个简单的例子来说，默认的配置是我所需要的。

## 渲染Web页面
前面例子里输出的不是HTML，他只是简单的”Hello World” 字符串。要想给浏览器产生HTML相应，我需要一个View，它将会告诉MVC如何为一个请求生成响应。
建立并渲染一个View
第一件事我需要做的是修改Index action方法，如程序2-3。我做的改动用加粗的字体显示了，也是使用了一个让例子变得很简单的约定。

Listing 2-3. 修改Action
```cs
using Microsoft.AspNetCore.Mvc;
namespace PartyInvites.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
return View("MyView");
}
}
}
```
当我从一个Action方法返回ViewResult 对象时，我指示MVC去渲染一个view。我通过调用View方法来创建一个ViewResult对象，调用时要制定View的名字MyView。如果你运行这个应用程序，你会发现MVC会试图寻找这个View，会出现如下图2-10所显示的错误信息。

![寻找视图](/imgs/fig.2-10.png)

图2-10 MVC试图寻找一个view

这个错误信息是非常有帮助的。 不仅解释了MVC找不到我在action方法里指定的View，也告诉了去哪找的。所有的View保存在Views文件夹，组织成子文件夹。例如，Home controller 里关联的View，会保存在叫做Views/Home的文件夹中。如果一个View没有固定的Controller,它将会保存在Views/shared文件夹。VS 会在Web 应用程序模板被使用的时候创建Home和Shared文件夹，并放入一些占位的View 以使工程能够启动。

要建立View，在解决方案浏览器窗口内，右键单击Views->Home，并在弹出菜单中选择Add -> New Item。VS 汇给你展示出来一些项模板，在左侧栏选择ASP.NET 类别然后在中间栏选择MVC View Page 项。如图2-11。

![建立View](/imgs/fig.2-11.png)

图2-11 建立View

提示: 在Views文件夹里你会看见一些文件，他们是VS 添加进来的用来提供就像图2-7显示的那样的初始内容。你可以忽略那些文件。

设置名字字段为MyView.cshtml 然后单击Add 按钮来建立View。VS将会建立Views/Home/MyView.cshtml文件然后打开它让你编辑。初始的时候View文件的内容只是一些注释和占位符，请用代码2-4替换他们。

提示:很容易在错误的文件夹中创建view文件，如果你没有最终在Views/Home里创建MyView.cshtml，将他们删除然后再创建就可以。

Listing 2-4. 替换 MyView.cshtml 文件内容
``` html
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
```
The new contents of the view file are mostly HTML. The exception is the part that looks like this:
```html
@{
Layout = null;
}
```

新的View文件中的内容大部分都是HTML,除了这一部分：
```html
@{
Layout = null;
}
```
这是一个被Razor View引擎解释的表达式，它会处理View的内容并生成发回给浏览器中的HTML。这是一个简单的Razor表达式，他会告诉Razor我选择不使用layout。Layout是一个HTML模板，我将在第五章讲解它。我将忽略Razor，过一会在回来。想要看效果，请点击Start Debugging 来运行应用程序。你将会看到图2-12中的结果。

![测试View](/imgs/fig.2-12.png)

图2-12 测试View

当我第一次编辑Index action 方法时，它返回一个字符串，也就是MVC会传递字符串值给浏览器，除此之外不会做别的。现在，Index方法返回一个ViewResult,MVC渲染一个视图（view）并返回它产生的HTML。我告诉了MVC该使用哪一个视图，所以它使用命名约定自动找到了它。这个约定就是视图有与Action相同的名字并且保存在以控制器命名的文件夹里。

除了字符串和ViewResult ,我还可以让Action方法返回其它类型。例如如果我返回一个RedirectResult,浏览器将重定向到另一个URL。如果我返回一个HttpUnauthorizedResult，I会强迫用户登录。这些对象被叫做Action result的集合控制。Action Result系统让你封装并重用通用的action返回。在17章，我会介绍更多的相关内容。

## 增加动态输出

整个web应用平台的关注点在于构建并显示动态输出内容。在MVC里，控制器负责构建一些数据并将其传给视图。视图负责渲染成HTML。
从控制器向视图传递数据的一种方式是使用ViewBag 对象，它是一个控制器基类的成员。ViewBag是一个动态对象，你可以给他赋值任意属性给视图来渲染用。代码2-5 演示了如何在HomeController里传递简单对象。
Listing 2-5. 设置视图数据
```cs
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
```

我向ViewBag.Greeting属性赋值，以给视图提供数据。Greeting属性在赋值之前是不存在的，这允许我以任意流畅的方式从控制器向视图传递数据而不必在赋值之前定义类。我在视图中引用了ViewBag.Greeting属性以获得他的值。如同代码2-6，这是修改后的MyView.cshtml。

Listing 2-6. 在视图里获取传递过来的值
```html
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
```
上面代码增加的部分是Razor表达式，它在MVC 使用视图生成相应的时候求值。当我在控制器内调用View方法的时候，MVC找到MyView.cshtml文件并请求Razor 视图引擎解析文件的内容。Razor 会查找象上面代码中的表达式。在本例中，处理表达式的意思是将ViewBag.Greeting属性插入到视图中。

Greeting这个属性名字没有什么特殊的东西，你可以使用任何其他的名字，并且一样好用。只要你在控制器中的名字与视图中的名字相同即可。你可以使用多个属性来传递多个数据。然后你运行一下看一下效果，如图2-13。

![动态响应](/imgs/fig.2-13.png)

图2-13 一个MVC的动态响应

## 创建一个简单的数据录入应用
在本章的剩下的部分，我将通过构建一个简单的数据录入应用来探索更多的基本MVC特征。这一节，我将会加快点速度。我的目标是用action来演示MVC，所以我将略过去一些讲解有些东西的内部原理。但是不要担心，我将会在以后的章节中讨论那些内容的。

#### 设置场景
想像一下，一个朋友决定了要举行一个新年晚会，他请我建立一个web 应用来跟踪它通过电子邀请函邀请的朋友。她需要以下四个关键功能：
####
* 一个关于这场晚会的信息的主页。
* 一个可以填写邀请函的窗体。
* 填写邀请函的时候需要验证，还要显示感谢页。
* 一个总结页面，用来显示谁将会参加这个晚会。

在接下来的段落里，我将在前面MVC工程的基础上逐渐增加内容，增加那些功能。第一步，我将马上就实现列表中的第一项，因为前面已经做了一些工作，只需要向现有的视图增加一些HTML来给出晚会的信息即可。代码2-7 显示了我向Views/Home/MyView.cshtml文件中增加的内容。

Listing 2-7. 显示晚会明细

```html
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
        <p>We're going to have an exciting party.<br />
            (To do: sell it better. Add pictures or something.)
        </p>
    </div>
    </body>
</html>
```

我继续。 如果你运行，你会看见晚会的信息，（额，还是一些占位符，但是你已经明白了吧）如图2-14.

![向视图里增加HTML](/imgs/fig.2-14.png)
图 2-14 向视图里增加HTML

## 增加一个数据模型
在MVC里，M 代表模型， 他是应用程序里最重要的部分。模型是真实世界对象的代表，处理，定义领域的规则。模型经常被称为是领域模型，包含C# 对象（领域对象）构成应用程序的世界和操纵他们的方法。视图和控制器会将领域用一致的方式暴露给客户端，并且，一个设计良好的MVC应用程序应该从一个设计良好的模型开始。然后控制器和视图才加入。

在PartyInvites工程里，我不需要复杂的模型，因为这是一个非常简单的应用，我只需要建立一个领域类，然后我将会调用GuestResponse.这个对象将负责保存，验证和确认一个邀请函。

MVC的约定一般把模型放在Models文件夹里，要建立这个文件夹，右击PartyInvites工程，从菜单中选择Add->New Folder ，然后设置名字为Models。

注意: 当程序运行时，你不能设置一个文件夹的名字。你可以从Debug菜单里选择Stop Debugging,右击你已经加入的NewFolder项，然后从弹出菜单中选择Rename，然后改成Models。

要建立一个类文件，右击Models文件夹，然后在弹出菜单里选择Add->Class。 设置新的类名字为GuestResponse.cs ，然后单击Add按钮，编辑新类的内容为代码2-8.

Listing 2-8 GuestResponse 领域类定义
```cs
namespace PartyInvites.Models {
    public class GuestResponse {
    public string Name { get; set; }
    public string Email { get; set; }
    public string Phone { get; set; }
    public bool? WillAttend { get; set; }
    }
}
```

提示: 你可能已经注意到了 WillAttend 属性是一个可空的bool型，意思是它可以是true,false,或null。我将解释她的基本原理在本章的"增加校验"节。

## 建立第二个Action和强类型的视图
我的应用程序的一个目标是包含一个邀请函窗体，也就是我将要定义一个行动（action）方法可以为它接收请求。一个单独的控制器类可以定义多个行动方法，默认约定是将相关的行动放到同一个控制器内。代码2-9展现了Home 控制器中新增加的行动方法。

```cs
Listing 2-9. Adding an Action Method in the HomeController.cs File
using System;
using Microsoft.AspNetCore.Mvc;
namespace PartyInvites.Controllers {
    public class HomeController : Controller {
        public ViewResult Index() {
            int hour = DateTime.Now.Hour;
            ViewBag.Greeting = hour < 12 ? "Good Morning" : "Good Afternoon";
                return View("MyView");
            }
            
            public ViewResult RsvpForm() {
                return View();
            }
    }
}
```
RsvpForm 行动方法调用View方法，不带参数，这将会告诉MVC取渲染连接于该行动方法的默认的视图，与行动方法的名字相同，在这里是RsvpForm.cshtml。
右击Views->Home 文件夹并在弹出菜单中选择Add->New Item。从ASP.NET 分类里选择MVC View Page模板，设置新的名字为RsvpForm.cshtml，并点击Add 按钮来创建文件。修改该文件的内容，让他变成代码 2-10那样。

Listing 2-10. 设置视图文件

```html
@model PartyInvites.Models.GuestResponse
@{
Layout = null;
}
<!DOCTYPE html>
<html>
    <head>
        <meta name="viewport" content="width=device-width" />
    <title>RsvpForm</title>
    </head>
    <body>
        <div>
            This is the RsvpForm.cshtml View
        </div>
    </body>
</html>
```
上面内容大部分为HTML，中间夹杂有@model Razor表达式，用来创建一个强类型的视图。强类型的视图用来渲染特定类型的模型，如果我指定一个类型（本例中，GuestResponse类），MVC可以创建一些有用的快捷方式并使它更容易。一会儿我将利用强类型的特征。
要测试新的行动方法和它的视图，启动应用程序，并使用浏览器浏览/Home/RsvpForm 。
MVC将使用命名约定来重定向请求到Home控制器中的RsvpForm行动方法。这个行动方法告诉MVC去渲染默认的视图，这里又使用了另一个命名规范，渲染RsvpForm.cshtml。图2-15展示了结果。

![渲染第二个视图](/imgs/fig.2-15.png)

图2-15 渲染第二个视图

## 连接行动方法
我想要从MyView视图内建立一个连接，以便我的客人能够看见RsvpForm视图而不必知道URL。如代码2-11。

Listing 2-11. 在MyView.cshtml里增加一个连接 
```html
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
<p>We're going to have an exciting party.<br />
(To do: sell it better. Add pictures or something.)
</p>
<a asp-action="RsvpForm">RSVP Now</a>
</div>
</body>
</html>
```
代码中增加的部分是一个具有asp-action属性的`<a>`标记。这是一个标记帮助器（tag helper)属性，他是一个给Razor的指令，在视图渲染的时候执行。这里的asp-action属性是用来给`<a>`标记增加一个href属性，包含指向一个行动方法的URL。我将会在第24,25和26章结束适合使用标记帮助器。但是这里是一个`<a>`标记的简单的标记帮助器，他告诉Razor 插入定义在与本视图相同的控制器中定义的一个行动方法的URL。程序运行的时候你会看到这个链接。如图2-16所示。

![在行动方法之间加链接](/imgs/fig.2-16.png)

图 2-16 在行动方法之间加链接

启动应用程序并将鼠标放在RSVP Now连接的上面，你会看到连接指向的是下面的URL(端口可能会有不同)：
http://localhost:57628/Home/RsvpForm

这里有一个重要的原则，即你应该使用MVC生成URL的功能，而不是在你的视图里硬编码。当标记帮助器给`<a`标记建立href属性时，他会检查当前应用程序的配置并计算出URL是什么样子。这允许改变应用程序配置来支持不同的URL格式而无需更新视图。我将会在第15章解释其原理。

## 建立窗体
现在我已经建立了强类型的视图，并且能够在Index视图中连接到它，我将在RsvpForm.cshtml文件里增加一些内容，并使他们变成一个HTML表单，用来编辑GuestResponse对象，如代码2-12。

Listing 2-12. 建立一个输入表单视图
```html
@model PartyInvites.Models.GuestResponse
@{
Layout = null;
}
<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width" />
<title>RsvpForm</title>
</head>
<body>
<form asp-action="RsvpForm" method="post">
<p>
<label asp-for="Name">Your name:</label>
<input asp-for="Name" />
</p>
<p>
<label asp-for="Email">Your email:</label>
<input asp-for="Email" />
</p>
<p>
<label asp-for="Phone">Your phone:</label>
<input asp-for="Phone" /></p>
<p>
<label>Will you attend?</label>
<select asp-for="WillAttend">
<option value="">Choose an option</option>
<option value="true">Yes, I'll be there</option>
<option value="false">No, I can't come</option>
</select>
</p>
<button type="submit">Submit RSVP</button>
</form>
</body>
</html>
```

我已经为GuestResponse模型类的每一个属性定义了一个标签和输入元素。每一个元素都使用asp-for属性连接一个模型的属性。这里的asp-for是另一个标签帮助器，他能配置将元素连接到模型对象。下面是一个例子，由标记帮助器生成，发给浏览器的HTML：

```html
<p>
<label for="Name">Your name:</label>
<input type="text" id="Name" name="Name" value="">
</p>
```
这里Label 上的asp-for 帮助器可以给for属性设置值。input 元素上的asp-for 可以设置元素的id 和name。这些都不是什么特殊的用途，但是你将会看到将元素连接到模型的属性会带来更多的好处。
马上你就会看到更多的asp-action属性应用到了form元素， 它使用了应用程序的URL路由配置来设置form的action属性为一个指向到一个特殊的行动方法的URL，如下面这样:

`<form method="post" action="/Home/RsvpForm">`
与我应用到元素的助手属性一样，这种方法的好处是更改应用程序使用的URL系统，标签助手生成的内容将反映自动变化。
运行该应用程序并点击RSVP Now 连接,可以看到窗体，如图2-17。


![在行动方法之间加链接](/imgs/fig.2-17.png)

图 2-17 在行动方法之间加链接

## 接收数据

我还没有告诉MVC当表单被发送到服务器时我想做什么。目前来看，点击Submit RSVP按钮只是清除您输入到表单中的值。那是因为表单将数据发回Home控制器的RsvpForm行动方法后只告诉MVC再次渲染这个视图，没做别的事情。 
要接收和处理提交的表单数据，我将使用核心控制器功能。我会加第二个RsvpForm动作方法来建立下面这些内容：
* 一个响应HTTP GET请求的方法：浏览器每次单击链接时，通常会引发一个GET请求。这个版本的行动方法将在用户第一次访问Home/RsvpForm时显示一个空白的表单。 
* 一个响应HTTP POST请求的方法: 默认的情况下，使用Html.BeginForm()的表单会被浏览器使用POST请求提交。这个版本的行动，将会负责接受提交的数据并决定下一步对这些数据作什么处理。
用不同的C#方法分别处理Get和POST请求会让你的代码看起来更简洁。因为两个方法应该行使不同的职责。两个方法会被同一个URL引发，但是MVC能够确根据GET请求或POST请求来调用适当的方法。代码2-13 可以看到HomeController类所做的更改。

Listing 2-13. 增加一个行动方法来支持POST请求
```cs
using System;
using Microsoft.AspNetCore.Mvc;
using PartyInvites.Models;
namespace PartyInvites.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
int hour = DateTime.Now.Hour;
ViewBag.Greeting = hour < 12 ? "Good Morning" : "Good Afternoon";
return View("MyView");
}
[HttpGet]
public ViewResult RsvpForm() {
return View();
}
[HttpPost]
public ViewResult RsvpForm(GuestResponse guestResponse) {
// TODO: store repsonse from guest
return View();
}
}
}
```
我已经给已经存在的RsvpForm行动方法加上了一个HttpGet的特性（attribute）,这回告诉MVC此方法仅能够被Get请求调用。然后我给RsvpForm方法增加了一个重载的版本，他可以接受GuestResponse 对象。 对这个方法我应用了HttpPost特性。它会告诉MVC 新的方法将处理POST请求。在后面的段落里我将会介绍这些代码是如何工作的。我也引入了一个叫做PartyInvites.Models的命名空间。加入它是为了我能够使用GuestResponse 模型类型。

## 使用模型绑定
第一个重载的RsvpForm行动方法渲染的视图同前面的一样--RsvpForm.cshtml--是用来生成图2-17那样的表单的。第二个重载的是个更有意思的事，但是考虑到响应于HTTP POST请求将调用行动方法，GuestResponse类型是一个C#类，两者是如何连接的？

答案是模型绑定，一个有用的MVC功能，其中输入数据被解析成HTTP请求中的键/值对，用于填充领域模型类型的属性。  模型绑定是一种功能强大且可自定义的功能，可以消除磨合而直接处理HTTP请求，并让您使用C#对象，而不是处理由浏览器发送的单个数据值。作为参数传递给Action方法的GuestResponse对象是自动填充表单字段中的数据的。 在第26章我将会介绍更多关于模型绑定的细节，包括如何定制。

应用程序中另一个目标是提供一个摘要页面，其中包含参加人的详细信息，这需要跟踪我收到的回复。 我将通过创建一个内存中的Collection对象来做到这一点。 这在实际应用中是没有什么用途的，因为应用程序停止或重新启动的话，数据将丢失，但这种方法允许我将重点放在MVC上并创建一个可以轻松地重置为初始状态的应用程序。

提示:第8章中,我演示了一个更实际的示例应用程序，将演示如何在MVC中永久地存储和访问数据。

我通过右键单击Models文件夹并从弹出窗口中选择Add->Class，将文件添加到项目中。 我将文件的名称设置为Repository.cs，并使用它来定义一个类，如清单2-14所示。

Listing 2-14. Repository.cs 文件的内容
```cs
using System.Collections.Generic;
namespace PartyInvites.Models {
 public static class Repository {
 private static List<GuestResponse> responses = new List<GuestResponse>();
 public static IEnumerable<GuestResponse> Responses {
 get {
 return responses;
 }
 }
 public static void AddResponse(GuestResponse response) {
 responses.Add(response);
 }
 }
}
```
Repository类及其成员是静态的，这将使我很容易从应用程序中的不同位置存储和检索数据。 MVC提供了一种更为复杂的方法来定义常见的功能，称为依赖注入，我在第18章中描述，但静态类是一个简单的应用程序入门的好方法。

## 保存响应

现在我有一个存储数据的地方，我可以更新接收HTTP POST请求的操作方法，如清单2-15所示。

Listing 2-15. 更新行动方法
```cs
using System;
using Microsoft.AspNetCore.Mvc;
using PartyInvites.Models;
namespace PartyInvites.Controllers {
 public class HomeController : Controller {
 public ViewResult Index() {
 int hour = DateTime.Now.Hour;
 ViewBag.Greeting = hour < 12 ? "Good Morning" : "Good Afternoon";
 return View("MyView");
 }
 [HttpGet]
 public ViewResult RsvpForm() {
 return View();
 }
 [HttpPost]
 public ViewResult RsvpForm(GuestResponse guestResponse) {
 Repository.AddResponse(guestResponse);
 return View("Thanks", guestResponse);
 }
 }
} 
```
处理请求中发送的表单数据的所有操作都是与传递给行动方法的GuestResponse对象一起使用 - 在这种情况下，将其作为参数传递给Repository.AddResponse方法，以便保存响应。

为什么模型绑定不像Web Forms?

在第一章中，我解释说传统ASP.NET Web窗体的一个缺点是它隐藏了开发人员的HTTP和HTML的细节。 您可能会想知道用于从代码2-15中的HTTP POST请求创建GuestResponse对象的MVC模型绑定是否会做同样的事情。
他没有这样做。模型绑定使我免除无聊且容易出错的任务，因为我们必须检查HTTP请求并提取我需要的所有数据值，但是（这是重要的部分），如果我想手动处理请求，那也可以，因为MVC可以方便地访问所有的请求数据。MVC没有对开发人员隐藏任何东西，但是有一些有用的功能可以使HTTP和HTML更简单。对于这些功能你可以用，也可以不用，随便。

 这似乎是一个微妙的区别，但是当您了解有关MVC的更多信息时，您将看到开发体验与传统Web窗体完全不同，并且您可以始终能够了解到应用程序收到的请求是如何处理的。

在RsvpForm操作方法中对View方法的调用告诉MVC渲染一个名为Thanks的视图，并将GuestResponse对象传递给视图。 要创建视图，请右键单击解决方案资源管理器中的“视图/主页”文件夹，然后从弹出菜单中选择“添加”->“新建项目”。 在ASP.NET类别中选择MVC视图页面模板，将名称设置为Thanks.cshtml，然后单击添加按钮。 Visual Studio将创建Views / Home / Thanks.cshtml文件并打开它进行编辑。 用代码2-16替换文件的内容。

Listing 2-16. Thanks.cshtml 文件的内容
```html
@model PartyInvites.Models.GuestResponse
@{
 Layout = null;
}
<!DOCTYPE html>
<html>
<head>
 <meta name="viewport" content="width=device-width" />
 <title>Thanks</title>
</head>
<body>
 <p>
 <h1>Thank you, @Model.Name!</h1>
 @if (Model.WillAttend == true) {
 @:It's great that you're coming. The drinks are already in the fridge!
 } else {
 @:Sorry to hear that you can't make it, but thanks for letting us know.
 }
 </p>
 <p>Click <a asp-action="ListResponses">here</a> to see who is coming.</p>
</body>
</html> 
```

Thanks.cshtml视图使用Razor来显示RsvpForm操作方法中传递给View方法的GuestResponse属性的值。 Razor中，@model表达式指定强制键入视图的领域模型类型。
  要访问域对象中的属性的值，我使用Model.PropertyName。 例如，要获取Name属性的值，我调用Model.Name。 如果不了解Razor的语法，不要担心，我在第5章更详细地解释它。
  现在我已经创建了Thanks视图，我已经有了一个基本的工作示例-使用MVC一个表单。
通过从Debug菜单中选择Start Debugging来启动Visual Studio中的应用程序，单击“RSVP Now”链接，在表单中添加一些数据，然后单击“Submit RSVP”按钮， 你会看到如图2-18所示的结果（尽管如果你的名字不是Joe）。

![感谢视图](/imgs/fig.2-18.png)

图2-18 感谢视图

## 显示响应

在Thanks.cshtml视图的结尾，我添加了一个元素来创建一个链接来显示参加派对的人员列表。 我使用asp-action标签帮助器属性来创建一个目标名为ListResponses的动作方法的URL，像这样：

...
`<p>Click <a asp-action="ListResponses">here</a> to see who is coming.</p>`
... 

如果您将鼠标悬停在浏览器显示的链接上，您将看到它的URL是/Home/ListResponses。 这与Home控制器中的任何操作方法不对应，如果单击链接，您将看到一个空页面。 打开浏览器的开发工具并查看服务器发送的响应会显示服务器发回404 - 未找到错误（Chrome有一点奇怪的是它不会向用户显示错误消息，但是 我将在第14章解释如何产生有意义的错误消息）。
  我将通过创建Home控制器中URL定位的操作方法来解决问题，如清单2-17所示。

Listing 2-17. 在控制器里增加一个行动方法
```cs
using System;
using Microsoft.AspNetCore.Mvc;
using PartyInvites.Models;
using System.Linq;
namespace PartyInvites.Controllers {
 public class HomeController : Controller {
 public ViewResult Index() {
 int hour = DateTime.Now.Hour;
 ViewBag.Greeting = hour < 12 ? "Good Morning" : "Good Afternoon";
 return View("MyView");
 } 
[HttpGet]
 public ViewResult RsvpForm() {
 return View();
 }
 [HttpPost]
 public ViewResult RsvpForm(GuestResponse guestResponse) {
 Repository.AddResponse(guestResponse);
 return View("Thanks", guestResponse);
 }
 public ViewResult ListResponses() {
 return View(Repository.Responses.Where(r => r.WillAttend == true));
 }
 }
} 
```

新的Action方法称为ListResponses，它使用Repository调用View方法。 响应属性作为参数。 这是向强类型视图提供数据的一种操作方法。
使用LINQ过滤GuestResponse对象的集合，以便得到正确响应。 ListResponses行动方法没有指定应该用于显示GuestResponse对象的集合的视图的名称，这意味着将使用默认的命名约定，MVC将在Views/Home和Views/Shared文件夹中查找名为ListResponses.cshtml的视图。要创建视图，请右键单击解决方案资源管理器中的“视图/主页”文件夹，然后从弹出菜单中选择“添加”->“新建项目”。 在ASP.NET类别中选择MVC视图页面模板，将名称设置为ListResponses.cshtml，然后单击添加按钮。 编辑新视图的内容如代码2-18。

Listing 2-18. 显示接收
```html
@model IEnumerable<PartyInvites.Models.GuestResponse>
@{
 Layout = null;
}
<!DOCTYPE html>
<html>
<head>
 <meta name="viewport" content="width=device-width" />
 <title>Responses</title>
</head>
<body>
 <h2>Here is the list of people attending the party</h2>
 <table>
 <thead>
 <tr>
 <th>Name</th>
 <th>Email</th>
 <th>Phone</th>
 </tr>
 </thead>
 <tbody>
 @foreach (PartyInvites.Models.GuestResponse r in Model) {
 <tr>
 <td>@r.Name</td>
 <td>@r.Email</td>
 <td>@r.Phone</td>
 </tr>
 }
 </tbody>
 </table>
</body>
</html> 
```
Razor视图文件具有cshtml文件扩展名，因为它们是C＃代码和HTML元素的组合。
您可以在清单2-18中看到这一点，其中我使用foreach循环来处理使用View方法将action方法传递给视图的每个GuestResponse对象。与正常的C＃foreach循环不同，Razor foreach循环的主体包含添加到将被发送回浏览器的响应中的HTML元素。在此视图中，每个GuestResponse对象都生成一个tr元素，其中包含用对象属性的值填充的td元素。
 要查看工作中的列表，请通过从开始菜单中选择启动调试来运行应用程序，提交一些表单数据，然后单击链接以查看响应列表。您将看到从应用程序启动后输入的数据摘要，如图2-19所示。该视图呈现数据的方式不太美观，但还可以了，本章稍后将介绍应用程序的样式。

![显示参加人员列表](/imgs/fig.2-19.png)

图2-18 显示参加人员列表

## 加入数据验证

我现在可以向我的应用程序添加数据验证。 没有验证，用户可以瞎输入数据，甚至提交一个空的表单。 在MVC应用程序中，通常将验证应用于领域模型，而不是在用户界面中。 您可以在一个位置定义验证，但是它将在使用模型类的应用程序中的任何位置生效。 MVC支持使用system.ComponentModel.DataAnnotations 命名空间中的属性定义的声明性验证规则，这意味着可以使用标准C#属性特征表示验证约束。 清单2-19显示了如何将这些属性应用于GuestResponse模型类。

Listing 2-19. 应用数据验证
```cs
using System.ComponentModel.DataAnnotations;
namespace PartyInvites.Models {
 public class GuestResponse {
 [Required(ErrorMessage = "Please enter your name")]
 public string Name { get; set; }
 [Required(ErrorMessage = "Please enter your email address")]
 [RegularExpression(".+\\@.+\\..+",
 ErrorMessage = "Please enter a valid email address")]
 public string Email { get; set; }
 [Required(ErrorMessage = "Please enter your phone number")]
 public string Phone { get; set; }
 [Required(ErrorMessage = "Please specify whether you'll attend")]
 public bool? WillAttend { get; set; }
 }
} 
```

MVC会自动检测属性，并在模型绑定过程中使用它们来验证数据。 我导入了包含验证属性的命名空间，所以我可以引用它们，而不需要限定他们的名字。

提示: 如前所述，我为WillAttend属性使用了可空的bool类型。 这样做可以让我应用必需的验证属性。 如果我使用了常规bool类型，我通过模型绑定收到的值可能只是真或假，我无法判断用户是否选择了一个值。 可空的bool有三个可能的值：true，false和null。 如果用户没有选择值，浏览器会发送一个空值，这会导致Required属性报告验证错误。 这是一个很好的例子，演示MVC如何优雅地将C#功能与HTML和HTTP混合在一起。

我使用Controller类中的ModelState.IsValid属性来检查是否存在验证问题。 清单2-20显示了如何在Home控制器类的POST对应的RsvpForm行动方法中完成此操作。

Listing 2-20. 校验表单中的错误
```cs
using System;
using Microsoft.AspNetCore.Mvc;
using PartyInvites.Models;
using System.Linq; 
namespace PartyInvites.Controllers 
{
 public class HomeController : Controller 
 {
 public ViewResult Index() {
 int hour = DateTime.Now.Hour;
 ViewBag.Greeting = hour < 12 ? "Good Morning" : "Good Afternoon";
 return View("MyView");
 }
 [HttpGet]
 public ViewResult RsvpForm() {
 return View();
 }
 [HttpPost]
 public ViewResult RsvpForm(GuestResponse guestResponse) {
 if (ModelState.IsValid) {
 Repository.AddResponse(guestResponse);
 return View("Thanks", guestResponse);
 } else {
 // there is a validation error
 return View();
 }
 }
 public ViewResult ListResponses() {
 return View(Repository.Responses.Where(r => r.WillAttend == true));
 }
 }
} 
```
Controller基类提供了一个名为ModelState的属性，它提供有关将HTTP请求数据转换为C#对象的信息。如果ModelState.IsValue属性返回true，那么我知道MVC已经能够满足通过GuestResponse类中的属性指定的验证约束。当这种情况发生时，我就像以前一样渲染了Thanks视图。
如果ModelState.IsValue属性返回false，那么我知道有验证错误。由ModelState属性返回的对象提供了错误的详细信息，但是我不需要进入该级别的详细信息，因为我可以使用一个有用的功能，自动请求用户解决任何通过调用View方法没有任何参数的问题。当MVC呈现视图时，Razor可以访问与请求相关联的任何验证错误的详细信息，标签助手可以访问详细信息以向用户显示验证错误。清单2-21显示了向RsvpForm视图添加验证标签助手属性。

Listing 2-21. 增加验证汇总
```html
@model PartyInvites.Models.GuestResponse
@{
 Layout = null;
} 

<!DOCTYPE html>
<html>
<head>
 <meta name="viewport" content="width=device-width" />
 <title>RsvpForm</title>
</head>
<body>
 <form asp-action="RsvpForm" method="post">
 <div asp-validation-summary="All"></div>
 <p>
 <label asp-for="Name">Your name:</label>
 <input asp-for="Name" />
 </p>
 <p>
 <label asp-for="Email">Your email:</label>
 <input asp-for="Email" />
 </p>
 <p>
 <label asp-for="Phone">Your phone:</label>
 <input asp-for="Phone" /></p>
 <p>
 <label>Will you attend?</label>
 <select asp-for="WillAttend">
 <option value="">Choose an option</option>
 <option value="true">Yes, I'll be there</option>
 <option value="false">No, I can't come</option>
 </select>
 </p>
 <button type="submit">Submit RSVP</button>
 </form>
</body>
</html> 
```
将asp-validation-summary属性应用于div元素，并在显示视图时显示验证错误列表。 asp-validation-summary属性的值是一个名为ValidationSummary的枚举的值，它指定了摘要将包含哪些类型的验证错误。 我指定了All，这对于大多数应用程序来说都很适用，我将在第27章描述其他值并解释它们中的工作原理。
要了解验证摘要的工作原理，请运行应用程序，填写“名称”字段，并提交表单而不输入任何其他数据。 您将看到验证错误的摘要，如图2-20所示。

![显示验证错误](/imgs/fig.2-20.png)

图2-20 显示验证错误

如果GuestResponse类的约束得没有得到验证，RsvpForm操作方法将不会呈现“感谢”视图。 请注意，当Razor使用验证摘要呈现视图时，它会保留并显示输入到“名称”字段中的数据。 这是模型绑定的另一个好处，它简化了处理表单数据的工作。

注意: 如果你使用过ASP.NET Web Forms，你会知道Web Forms会将值序列化为名为`__VIEWSTATE`的隐藏表单字段来保留服务器控件的状态。 MVC将模型绑定到Web窗体服务器控件与View State的概念无关。 MVC不会在您呈现的HTML页面中注入隐藏的`__VIEWSTATE`字段。 而是通过设置输入元素的值属性来包含数据。

## 高亮显示无效字段

将模型属性与元素相关联的标签助手属性具有模型绑定的便捷功能。 当模型类属性验证失败时，帮助器属性将生成稍微不同的HTML。 以下是当没有验证错误时的为“电话”字段生成的输入元素：
```html
<input type="text" data-val="true" data-val-required="Please enter your phone number" id="Phone" name="Phone" value=""> 
```
相对的，这是在用户在文本字段中不输入任何数据并提交表单产生的HTML元素，（这是一个验证错误，我将Required的验证属性应用到了GuestResponse类的Phone属性上）：

```html
<input type="text" class="input-validation-error" data-val="true"
 data-val-required="Please enter your phone number" id="Phone"
 name="Phone" value=""> 
 ```

 我已经高亮显示了二者的区别：asp-for标签帮助器属性将输入元素添加到一个名为input-validation-error的类中。 我创建一个包含此类的CSS样式的样式表，不同的HTML助手属性使用的其他样式表来实现这个效果。  MVC项目中的约定是将传递给客户端的静态内容放入wwwroot文件夹中，按内容类型进行组织，以便CSS样式表进入wwwroot/css文件夹，JavaScript文件进入wwwroot/js文件夹，依此类推。
  要创建样式表，请右键单击Visual Studio解决方案资源管理器中的wwwroot/css文件夹，选择添加->新项，导航到客户端部分，然后从模板列表中选择样式表，如图2-21。

![建立CSS样式表](/imgs/fig.2-21.png)

图2-21 建立CSS样式表

提示:当使用Web应用程序模板创建项目时，Visual Studio会在wwwroot / css文件夹中创建一个style.css文件。 你可以忽略这个文件，我在本章不使用它。

将文件的名称设置为styles.css，单击添加按钮创建样式表，然后编辑新文件，把他改成代码2-22所示的样式。

Listing 2-22. styles.css 文件的内容

```html
.field-validation-error {color: #f00;}
.field-validation-valid { display: none;}
.input-validation-error { border: 1px solid #f00; background-color: #fee; }
.validation-summary-errors { font-weight: bold; color: #f00;}
.validation-summary-valid { display: none;}
 To apply this stylesheet, I have added a link element to the head section of the RsvpForm view, as shown
in Listing 2-23 .
 Listing 2-23. Applying a Stylesheet in the RsvpForm.cshtml File
...
<head>
 <meta name="viewport" content="width=device-width" />
 <title>RsvpForm</title>
 <link rel="stylesheet" href="/css/styles.css" />
</head>
... 
```

link 元素使用href属性来指定样式表的位置。 请注意，URL中省略了wwwroot文件夹。 ASP.NET的默认配置包括支持静态内容，如图像，CSS样式表和JavaScript文件，并将请求自动映射到wwwroot文件夹。 我在第14章中描述了ASP.NET和MVC配置过程。

提示:有一个特殊的标签帮助器来处理样式表，如果你有很多文件要管理，这可以很有用。 详见第25章。

随着样式表的应用，当提交导致验证错误的数据时，会显示更明显的验证错误，如图2-22所示。

![自动高亮错误](/imgs/fig.2-22.png)

图2-22 自动高亮错误

## 设置样式

应用程序的所有功能目标都完成了，但应用程序的整体外观还需要调整一下。 当您使用Web应用程序模板创建项目时，如本章中的示例所示，Visual Studio安装了一些常见的客户端开发包。 虽然我不是使用模板的粉丝，但我喜欢Microsoft选择的客户端库。 其中一个被称为Bootstrap，它是一个很好的CSS框架，最初由Twitter开发，已经成为一个主要的开源项目，它已经成为Web应用程序开发的支柱。

注意:Bootstrap 3是我写的当前版本，但是第4版正在开发中。 Microsoft可能会选择在Visual Studio的更高版本中更新Web应用程序模板使用的Bootstrap版本，这可能会导致内容显示不同。 这对于本书中的其他章节来说不会是一个问题，因为我向您展示如何明确指定包版本，以便获得预期的结果。

## 设置Welcome视图的样式

wwwroot/lib/bootstrap文件夹的文件中定义了一些CSS选择器,基本的Bootstrap功能通过将类应用到与添加到CSS选择器相对应的元素来工作。 您可以从http://getbootstrap.com 获取Bootstrap定义的类的详细信息，代码2-24演示了如何将一些基本样式应用于MyView.cshtml视图。

Listing 2-24. 给MyView.cshtml添加Bootstrap
```html
@{
 Layout = null;
}
<!DOCTYPE html>
<html>
<head>
 <meta name="viewport" content="width=device-width" />
 <title>Index</title>
 <link rel="stylesheet" href="/lib/bootstrap/dist/css/bootstrap.css" />
</head>
<body>
 <div class="text-center">
 <h3>We're going to have an exciting party!</h3>
 <h4>And you are invited</h4>
 <a class="btn btn-primary" asp-action="RsvpForm">RSVP Now</a>
 </div>
</body>
</html> 
```

我添加了其中link元素，他的href属性是wwwroot/lib/bootstrap/dist/css/bootstrap.css。 第三方CSS和JavaScript包安装在wwwroot/lib文件夹中是一个常用约定，我将在第6章中描述用于管理这些包的工具。
  导入Bootstrap样式表后，我需要为我的元素设置样式。 这是一个简单的例子，所以我只需要使用少量的Bootstrap CSS类：text-center，btn和btn-primary。
  text-center class将元素及其子元素的内容置于中心位置。 btn类将按钮、input元素变得更漂亮，btn-primary指定我想要的按钮的颜色范围。 运行应用程序可以看到效果，如图2-23所示。

![设置视图样式](/imgs/fig.2-23.png)

图2-23 设置视图样式

很明显，我不是网页设计师。 事实上，作为一个孩子，我根本就没有任何才华，因此我被排除在艺术课之外。 这使得我有更多时间来上数学课，但意味着我的艺术能力并没有超过10岁的平均水平。 对于一个真正的项目，我会寻求一位专业人士来帮助设计和设计内容，但是在这个例子中，我一个人就这样做了，也就是应用Bootstrap可以像我一样有一些限制和一致性。

## 设置RsvpForm视图的样式

Bootstrap定义了可以用于样式表单的类。 我不会详细介绍他们，但是您可以看到我已经在清单2-25中应用了这些类。

Listing 2-25. 给RsvpForm.cshtml添加Bootstrap 
```html
@model PartyInvites.Models.GuestResponse
@{
Layout = null;
}
<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width" />
<title>RsvpForm</title>
<link rel="stylesheet" href="/css/styles.css" />
<link rel="stylesheet" href="/lib/bootstrap/dist/css/bootstrap.css" />
</head>
<body>
<div class="panel panel-success">
<div class="panel-heading text-center"><h4>RSVP</h4></div>
<div class="panel-body">
<form class="p-a-1" asp-action="RsvpForm" method="post">
<div asp-validation-summary="All"></div>
<div class="form-group">
<label asp-for="Name">Your name:</label>
<input class="form-control" asp-for="Name" />
</div>
<div class="form-group">
<label asp-for="Email">Your email:</label>
<input class="form-control" asp-for="Email" />
</div>
<div class="form-group">
<label asp-for="Phone">Your phone:</label>
<input class="form-control" asp-for="Phone" />
</div>
<div class="form-group">
<label>Will you attend?</label>
<select class="form-control" asp-for="WillAttend">
<option value="">Choose an option</option>
<option value="true">Yes, I'll be there</option>
<option value="false">No, I can't come</option>
 </select>
 </div>
 <div class="text-center">
 <button class="btn btn-primary" type="submit">
 Submit RSVP
 </button>
 </div>
 </form>
 </div>
 </div>
</body>
</html> 
```
此示例中的Bootstrap类创建一个标题，只给了布局的结构。 要设置样式，我使用了form-group类，用于给标签元素和相关的input或select元素设置样式。 您可以在图2-24中看到效果。

![设置RsvpForm视图的样式](/imgs/fig.2-24.png)

图2-24 设置RsvpForm视图的样式

## 设置Thanks视图的样式

下一个要设置样式的视图是Thanks.cshtml，您可以使用类似于我用于其他视图的CSS类来看清楚如何在代码2-26中完成此操作。 为了使应用程序更易于管理，尽可能避免重复代码和标记，这一个很好的原则。 MVC提供了几个功能来帮助减少重复，我将在后面的章节中介绍。 这些功能包括Razor布局（第5章），部分视图（第21章）和视图组件（第22章）。

Listing 2-26. 设置Thanks.cshtml样式
```html
@model PartyInvites.Models.GuestResponse
@{
 Layout = null;
}
<!DOCTYPE html>
<html>
<head>
 <meta name="viewport" content="width=device-width" />
 <title>Thanks</title>
 <link rel="stylesheet" href="/lib/bootstrap/dist/css/bootstrap.css" />
</head>
<body class="text-center">
 <p>
 <h1>Thank you, @Model.Name!</h1>
 @if (Model.WillAttend == true) {
 @:It's great that you're coming. The drinks are already in the fridge!
 } else {
 @:Sorry to hear that you can't make it, but thanks for letting us know.
 }
 </p>
 Click <a class="nav-link" asp-action="ListResponses">here</a>
 to see who is coming.
</body>
</html> 
```
图2-25 显示了样式的效果。

![Thanks视图的样式](/imgs/fig.2-25.png)

图2-25 Thanks视图的样式

## 设置列表视图的样式

最后的风格是ListResponses，它列出了与会者的列表。 对内容设置样式遵循与所有Bootstrap样式相同的基本方法，如代码2-27所示。

Listing 2-27. ListResponses.cshtml 加 Bootstrap
```html
@model IEnumerable<PartyInvites.Models.GuestResponse>
@{
 Layout = null;
}
<!DOCTYPE html>
<html>
<head>
 <meta name="viewport" content="width=device-width" />
 <link rel="stylesheet" href="/lib/bootstrap/dist/css/bootstrap.css" />
 <title>Responses</title>
</head>
<body>
 <div class="panel-body">
 <h2>Here is the list of people attending the party</h2>
 <table class="table table-sm table-striped table-bordered">
 <thead>
 <tr>
 <th>Name</th>
 <th>Email</th>
 <th>Phone</th>
 </tr>
 </thead>
 <tbody> 
 @foreach (PartyInvites.Models.GuestResponse r in Model) {
 <tr>
 <td>@r.Name</td>
 <td>@r.Email</td>
 <td>@r.Phone</td>
 </tr>
 }
 </tbody>
 </table>
 </div>
</body>
</html> 
 ```
图2-26显示了与会者的表格的呈现方式。 将这些样式添加到视图中完成了示例应用程序，该应用程序现在实现了所有的开发目标，并且外观也已经改的很好了。

![ListResponses视图的样式](/imgs/fig.2-26.png)

图2-26 ListResponses视图的样式

## 小结

在本章中，我创建了一个新的MVC项目，并使用它来构建一个简单的数据录入应用程序，让您首先了解ASP.NET Core MVC架构和方法。 我跳过了一些关键的东西（包括Razor语法，路由和测试），但我将在后面的章节深入介绍这些内容。 在下一章中，我将描述MVC设计模式，这是开发ASP.NET Core MVC应用程序的基础。
