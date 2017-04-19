# 第四章 C# 关键特征

在本章中，我描述了Web应用程序开发中使用的C#特征，这些特征尚未被广泛理解或经常引起混淆。 这不是关于C#的书，但是，我仅为每个特征提供一个简单的例子，以便您可以按照本书其余部分的示例，并在自己的项目中利用他们。 表4-1总结了本章的内容。

表4-1 本章汇总
|问题|解决方案|代码示例|
|----|-------|-------|
|避免访问空引用属性|使用空条件操作符|6-9|
|简化C# 属性|使用自动实现的属性|10-12|
|简化字符串构造|使用字符串插值|13|
|在单个步骤内创建对象并赋值|使用对象或集合初始化器|14-17|
|给类添加功能而无需修改类|使用扩展方法|18-25|
|单语句方法与简化委托|使用lambda表达式|26-33|
|使用隐含类型|使用var关键字|34|
|创建对象无需定义类型|使用匿名类型|35-36|
|简化异步方法的使用|使用async和await关键字|37-40|
|取得类或属性的名字无需定义静态字符串|使用nameof 表达式|41-42|

## 准备示例项目

在本章中，我使用ASP.NET Core Web应用程序（.NET Core）模板创建了一个名为LanguageFeatures的新Visual Studio项目。 不要选择“添加应用程序洞察到项目”选项，然后单击确定按钮，如图4-1所示。

![1选择项目类型](/imgs/fig.4-1.png)

图4-1 选择项目类型

当显示不同的ASP.NET项目配置时，我选择了空模板，如图4-2所示，然后单击确定按钮创建项目。

![选择初始项目内容](/imgs/fig.4-2.png)

图4-2 选择初始项目内容

## 启用ASP.NET Core MVC

空项目模板创建一个包含最小ASP.NET Core配置而没有任何MVC支持的项目。 这意味着Web应用程序模板添加的占位符内容不存在，但这也意味着需要一些额外的步骤来启用MVC，以便控件和视图等功能可以工作。 在本节中，我进行了修改，以在项目中添加启用MVC设置，但我不会详细介绍每个步骤目前的功能。 第一步是添加.NET MVC程序集，它在project.json文件的依赖项部分完成，如代码4-1所示。

Listing 4-1. 在文件project.json 中添加.NET MVC程序集
```cs
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
```
project.json文件的依赖关系部分列出了项目所需的程序集。 我添加了包含MVC类的Microsoft.AspNetCore.Mvc程序集。 请注意，在添加Microsoft.AspNetCore.Mvc程序集的上一行要添加行末尾的逗号。
JSON配置文件对正确的格式要求很严格的，如果忘记添加逗号，则会产生错误。

提示:使用每个程序集都要指定版本号。 您必须确保您指定的所有程序集版本都能在一起工作。 当您编辑project.json文件时，Visual Studio将提供可用的程序集版本列表，最简单的方法是确保您为Microsoft.AspNetCore.Mvc指定的版本与现有Visual Studio在创建项目时添加的依赖项部分的程序集的版本相同 。

下一步是告诉ASP.NET使用MVC，这是在Startup类中完成的，如代码4-2所示。

Listing 4-2. Startup.cs 启用ASP.NET MVC
```cs
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;

namespace LanguageFeatures {
public class Startup {
public void ConfigureServices(IServiceCollection services) {
services.AddMvc();
}
public void Configure(IApplicationBuilder app, IHostingEnvironment env,
ILoggerFactory loggerFactory) {
app.UseMvcWithDefaultRoute();
}
}
}
```

我将在第14章中解释如何配置ASP.NET Core MVC应用程序，但代码4-2中添加的两个语句提供了使用默认配置和约定的基本MVC设置。

## 创建 MVC 应用组件

现在MVC已经建立起来，我可以添加我将用来演示重要C#语言特征的MVC应用程序组件。

### 建立模型
我开始创建一个简单的模型类，以便我可以使用一些数据来处理。 我添加了一个名为Models的文件夹，并在其中创建了一个名为Product.cs的类文件，类定义如代码4-3所示。

Listing 4-3. Product.cs
```cs
namespace LanguageFeatures.Models {
public class Product {
public string Name { get; set; }
public decimal? Price { get; set; }
public static Product[] GetProducts() {
Product kayak = new Product {
Name = "Kayak", Price = 275M
};
Product lifejacket = new Product {
Name = "Lifejacket", Price = 48.95M
};
return new Product[] { kayak, lifejacket, null };
}
}
}
```
Products类定义名称和价格属性，并且有一个名为GetProducts的静态方法返回Products数组。 由GetProducts方法返回的数组中包含的元素之一设置为null。

### 建立控制器和视图

对于本章中的示例，我使用一个简单的控制器来演示不同的语言特性。 我创建了一个Controllers文件夹，并添加了一个名为HomeController.cs的类文件，其内容如代码4-4所示。 当使用默认的MVC配置时，MVC将发送HTTP请求给Home控制器。

Listing 4-4. HomeController.cs
```cs
using Microsoft.AspNetCore.Mvc;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
return View(new string[] { "C#", "Language", "Features" });
}
}
}
```

Index 方法告诉MVC渲染默认视图并传递一个字符串数组，转成发送给客户端的HTML。 要创建相应的视图，我添加了一个Views/Home文件夹（通过创建一个Views文件夹，然后在其中添加一个Home文件夹），并添加了一个名为Index.cshtml的视图文件，其内容如代码4-5所示。

Listing 4-5. Index.cshtml
```html
@model IEnumerable<string>
@{ Layout = null; }
<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width" />
<title>Language Features</title>
</head>
<body>
<ul>
@foreach (string s in Model) {
<li>@s</li>
}
</ul>
</body>
</html>
```

运行示例应用程序，您将看到如图4-3所示的输出。
![运行示例程序](/imgs/fig.4-3.png)

由于本章中所有示例的输出都是文本，我将显示浏览器显示的消息，如下所示：

-----------------------
C#
Language
Features
-----------------------

## 使null条件运算符

null条件运算符允许更优雅地检测空值。 在确定请求是否包含特定头或值或模型是否包含特定数据项时，要对MVC开发中的空值进行大量判断。 传统上，处理null需要进行明确的检查，当对象及其属性必须被检查时，这可能变得烦人。 null条件运算符使此过程更简单，更简洁，如代码4-6所示。

Listing 4-6. 检测空值
```cs 
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
List<string> results = new List<string>();
foreach (Product p in Product.GetProducts()) {
string name = p?.Name;
decimal? price = p?.Price;
results.Add(string.Format("Name: {0}, Price: {1}", name, price));
}
return View(results);
}
}
}
```
Product类定义的静态GetProducts方法返回我在控制器的Index操作方法中检查的对象数组，以获取名称和价格值的列表。 问题是数组中的对象和属性的值都可以为null，这意味着我不能在foreach循环中简单地引用p.Name或p.Price，这会引起NullReferenceException。 为了避免这种情况，我使用null条件运算符，像这样：


```cs
...
string name = p?.Name ;
decimal? price = p?.Price ;
...
```


空条件运算符是单个问号（？字符）。 如果p为空，则名称也将设置为null。 如果p不为空，那么name将被设置为Person.Name属性的值。 价格属性受同一测试。 请注意，使用null条件运算符时分配的变量必须能够被分配为null，这就是为什么价格变量被声明为可空的decimal（decimal？）。

## 链接空条件运算符

空条件运算符可以链接在一起以导航对象的层次结构，这是真正成为简化代码并允许安全导航的有效工具。 在代码4-7中，我已经将一个属性添加到产品类中，从而创建了一个更复杂的对象层次结构。

Listing 4-7. 增加Product中的属性
```cs
namespace LanguageFeatures.Models {
public class Product {
public string Name { get; set; }
public decimal? Price { get; set; }
public Product Related { get; set; }
public static Product[] GetProducts() {
Product kayak = new Product {
Name = "Kayak", Price = 275M
};
Product lifejacket = new Product {
Name = "Lifejacket", Price = 48.95M
};
kayak.Related = lifejacket;
return new Product[] { kayak, lifejacket, null };
}
}
}
```

每个Product对象都有一个可以引用另一个Product对象的“相关”属性。 在GetProducts方法中，我设置了代表皮划艇的Product对象的“相关”属性。 代码4-8显示了如何将null条件运算符链接在一起，以导航对象属性而不引起异常。

Listing 4-8. 检查嵌套的空值
```cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
List<string> results = new List<string>();
foreach (Product p in Product.GetProducts()) {
string name = p?.Name;
decimal? price = p?.Price;
string relatedName = p?.Related?.Name;
results.Add(string.Format("Name: {0}, Price: {1}, Related: {2}",
name, price, relatedName));
}
return View(results);
}
}
}
```

null条件运算符可以应用于属性链的每个部分，如下所示：

...
string relatedName = p?.Related? .Name;
...

结果是当p为空或p.Related为空时，relatedName变量为空。 否则，将为变量分配p.Related.Name属性的值。 如果运行该示例，您将在浏览器窗口中看到以下输出：
```cs
Name: Kayak, Price: 275, Related: Lifejacket
Name: Lifejacket, Price: 48.95, Related:
Name: , Price: , Related:
```
## 结合条件和合并运算符

将空条件运算符（单个问号）与空合并运算符（两个问号）组合以设置回退值以显示应用程序中使用的空值可能非常有用，如清单4-9所示。

Listing 4-9. 合并 Null运算符
```cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
List<string> results = new List<string>();
foreach (Product p in Product.GetProducts()) {
string name = p?.Name ?? "<No Name>";
decimal? price = p?.Price ?? 0;
string relatedName = p?.Related?.Name ?? "<None>";
results.Add(string.Format("Name: {0}, Price: {1}, Related: {2}",
name, price, relatedName));
}
return View(results);
}
}
}
```

空条件运算符确保在导航对象属性时不会得到NullReferenceException，并且空合并运算符确保在浏览器中显示的结果中不包括空值。 如果运行该示例，您将在浏览器窗口中看到以下结果：

```
Name: Kayak, Price: 275, Related: Lifejacket
Name: Lifejacket, Price: 48.95, Related: <None>
Name: <No Name> , Price: 0 , Related: <None>
```
## 使用自动实现的属性

C#支持自动实现的属性，我在上一节中定义Person类的属性时使用它们，如下所示：
```cs
namespace LanguageFeatures.Models {
public class Product {
public string Name { get; set; }
public decimal? Price { get; set; }
public Product Related { get; set; }
public static Product[] GetProducts() {
Product kayak = new Product {
Name = "Kayak", Price = 275M
};
Product lifejacket = new Product {
Name = "Lifejacket", Price = 48.95M
};
kayak.Related = lifejacket;
return new Product[] { kayak, lifejacket, null };
}
}
}
```

此功能允许我定义属性，而不必实现get和set主体。 使用自动实现的属性功能定义如下属性：

```cs
...
public string Name { get; set; }
...
is equivalent to the following code:
...
public string Name {
get { return name; }
set { name = value; }
}
...
```

这种类型的特征叫做语法糖，它能使C#更加愉快地工作 - 在这种情况下，它能消除最终被复制的冗余代码，而不会大大改变语言的行为方式。 术语"糖"可能看起来很贬义，但是使代码更容易编写和维护的任何增强功能都是有益的，特别是在大型复杂项目中。

## 使用自动实现的属性初始化器

自C＃3.0以来一直支持自动实现的属性。 最新版本的C＃支持自动实现的属性的初始化器，允许设置初始值，而不必使用构造函数，如代码清单4-10所示。

Listing 4-10. 使用自动实现的属性初始化器
```cs
namespace LanguageFeatures.Models {
public class Product {
public string Name { get; set; }
public string Category { get; set; } = "Watersports";
public decimal? Price { get; set; }
public Product Related { get; set; }
public static Product[] GetProducts() {
Product kayak = new Product {
    Name = "Kayak",
Category = "Water Craft",
Price = 275M
};
Product lifejacket = new Product {
Name = "Lifejacket", Price = 48.95M
};
kayak.Related = lifejacket;
return new Product[] { kayak, lifejacket, null };
}
}
}
```
将值分配给自动实现的属性不会阻止setter更改属性，并且只需对构造函数的简单类型的代码进行整理，以提供默认值。 在该示例中，初始化程序为“类别”属性分配了一个“Watersports”值。当我创建皮划艇对象时并需要指定一个值的水工艺时，我会做改变初始值。

## 建立只读的自动实现的属性

您可以通过使用初始化程序创建只读属性，属性中省略set关键字即可，如清单4-11所示。