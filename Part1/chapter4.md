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

## 创建只读的自动实现的属性

您可以通过使用初始化程序创建只读属性，属性中省略set关键字即可，如清单4-11所示。

Listing 4-11. 创建只读属性
```cs
namespace LanguageFeatures.Models {
public class Product {
public string Name { get; set; }
public string Category { get; set; } = "Watersports";
public decimal? Price { get; set; }
public Product Related { get; set; }
public bool InStock { get; } = true;
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

InStock属性初始化为true，不能更改; 但是，该值可以在类型的构造函数中分配，如清单4-12所示。

Listing 4-12. 给只读属性赋值
```cs
namespace LanguageFeatures.Models {
public class Product {
public Product(bool stock = true) {
InStock = stock;
}
public string Name { get; set; }
public string Category { get; set; } = "Watersports";
public decimal? Price { get; set; }
public Product Related { get; set; }
public bool InStock { get; }
public static Product[] GetProducts() {
Product kayak = new Product {
Name = "Kayak",
Category = "Water Craft",
Price = 275M
};
Product lifejacket = new Product(false) {
Name = "Lifejacket",
Price = 48.95M
};
kayak.Related = lifejacket;
return new Product[] { kayak, lifejacket, null };
}
}
}
```
构造函数允许将只读属性的值指定为参数，如果未提供值，则默认为true。 由构造函数设置后，属性值不能更改。

## 使用字符串插值

string.Format方法是用于编写包含数据值的字符串的传统C#工具。 下面是Home控制器中使用这种技术的一个例子：

...
results.Add( string.Format("Name: {0}, Price: {1}, Related: {2}",
name, price, relatedName) );
...

C#6.0添加了对不同方法的支持，称为字符串插值，无需确保字符串模板中的{0}引用与指定为参数的变量匹配。 相反，字符串插入直接使用变量名，如代码4-13所示。

Listing 4-13. 使用字符串插值
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
results.Add($"Name: {name}, Price: {price}, Related: {relatedName}");
}
return View(results);
}
}
}
```
内插字符串前缀为$字符，并包含空格，这些引用是{和}字符中包含的值的引用。 当字符串被求值时，这些位置用指定的变量或常量的当前值填充。
Visual Studio为创建内插字符串提供了IntelliSense支持，并提供了{字符键入时可用成员的列表; 这有助于最小化打字错误，结果是更容易理解的字符串格式。

提示:字符串插值支持string.Format方法中可用的所有格式。 格式指定作为占位符(hole)的一部分包括在内，因此$“Price：{price：C2}”将格式化Price的值作为具有两位十进制数的货币值。

## 使用对象和集合初始化器

当我在Product类的静态GetProducts方法中创建一个对象时，我使用一个对象初始化器，它允许我创建一个对象并同时指定它的属性值，如下所示：
Product kayak = new Product {
Name = "Kayak",
Category = "Water Craft",
Price = 275M
};
...

这是另一种使C#更容易使用的语法糖功能。 没有这个功能，我必须调用Product构造函数，然后使用新创建的对象来设置每个属性，如下所示：

...
Product kayak = new Product();
kayak.Name = "Kayak";
kayak.Category = "Water Craft";
kayak.Price = 275M;
...

与此相关的功能是集合初始化器，它允许创建在单一步骤中指定的集合及其内容。 没有初始化程序，例如，创建一个字符串数组需要单独指定数组和数组元素的大小，如清单4-14所示。

Listing 4-14. 初始化对象
```cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
string[] names = new string[3];
names[0] = "Bob";
names[1] = "Joe";
names[2] = "Alice";
return View("Index", names);
}
}
}

```

使用集合初始化程序允许将数组的内容指定为构造的一部分，这将为编译器提供数组的大小，如清单4-15所示。


Listing 4-15. 使用集合初始化器
```cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
return View("Index", new string[] { "Bob", "Joe", "Alice" });
}
}
}    
```

数组元素在{和}字符之间指定，这允许对集合进行更简洁的定义，并可以在方法调用中内联定义集合。 清单4-15中的代码与清单4-14中的代码具有相同的效果，如果运行示例应用程序，您将在浏览器窗口中看到以下输出：

```
Bob
Joe
Alice
```

## 使用索引初始化器

C# 6 使用了一种更加整洁的新的集合初始化方式来创建带有索引的集合（如字典)。 清单4-16显示了使用C# 5 方法初始化字典来定义一个集合的重写索引操作。

Listing 4-16. 初始化字典对象
```cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
Dictionary<string, Product> products = new Dictionary<string, Product> {
{ "Kayak", new Product { Name = "Kayak", Price = 275M } },
{ "Lifejacket", new Product{ Name = "Lifejacket", Price = 48.95M } }
};
return View("Index", products.Keys);
}
}
}
```

初始化这种类型的集合的语法对{和}字符有太多依赖，特别是当集合值正在使用对象初始化器创建时。 C#6编译器支持一种更自然的方法来初始化索引集合，这与收集初始化后检索或修改值的方式一致，如清单4-17所示。

Listing 4-17. 使用集合初始化器语法
```cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
Dictionary<string, Product> products = new Dictionary<string, Product> {
["Kayak"] = new Product { Name = "Kayak", Price = 275M },
["Lifejacket"] = new Product { Name = "Lifejacket", Price = 48.95M }
};
return View("Index", products.Keys);
}
}
}
```

效果是相同的 - 创建一个字典，其键是Kayak和Lifejacket，其值是Product对象，但是使用用于其他集合操作的索引符号创建元素。 如果您运行应用程序，您将在浏览器中看到以下结果：

Kayak
Lifejacket

## 使用扩展方法

扩展方法是将方法添加到您不拥有并且不能直接修改的类的方便方法。 清单4-18显示了ShoppingCart类的定义，我将它添加到名为ShoppingCart.cs文件的文件中的Models文件夹中，该文件表示Product对象的集合。

Listing 4-18. ShoppingCart.cs 的内容
```cs
using System.Collections.Generic;
namespace LanguageFeatures.Models {
public class ShoppingCart {
public IEnumerable<Product> Products { get; set; }
}
}
```

这是一个简单的类，用作Product对象的包装（我只需要一个这个例子的基本类）。 假设我需要能够确定ShoppingCart类中Product对象的总价值，但是我无法修改类本身，也许是因为它来自第三方，我没有源代码。 我可以使用扩展方法来添加我需要的功能。 清单4-19显示了我添加到MyExtensionMethods.cs文件中的Models文件夹的MyExtensionMethods类。

Listing 4-19. MyExtensionMethods.cs 文件内容
```cs
namespace LanguageFeatures.Models {
public static class MyExtensionMethods {
public static decimal TotalPrices(this ShoppingCart cartParam) {
decimal total = 0;
foreach (Product prod in cartParam.Products) {
total += prod?.Price ?? 0;
}
return total;
}
}
}
```

第一个参数前面的这个关键字标记TotalPrices作为扩展方法。 在这种情况下，第一个参数告诉.NET哪个类的扩展方法可以应用于ShoppingCart。 我可以通过使用cartParam参数来引用已经应用扩展方法的ShoppingCart实例。 我的方法枚举ShoppingCart中的Product，并返回Product.Price属性的总和。 清单4-20显示了如何在Home控制器的Action方法中应用扩展方法。

提示: 扩展方法不会让你破坏方法、字段和属性定义的访问规则。 您可以使用扩展方法扩展类的功能，但只能使用您可以访问的类成员。

Listing 4-20. 应用扩展方法
```cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
ShoppingCart cart
= new ShoppingCart { Products = Product.GetProducts() };
decimal cartTotal = cart.TotalPrices();
return View("Index", new string[] { $"Total: {cartTotal:C2}" });
}
}
}
```

关键的语句是:
...
decimal cartTotal = cart.TotalPrices();
...

我将ShoppingCart对象的TotalPrices方法称为是ShoppingCart类的一部分，尽管它是由不同类完全定义的扩展方法。 如果扩展类在当前类的范围内，则会发现它们是同一个命名空间的一部分，也可以是作为using语句主题的命名空间。 如果您运行应用程序，您将在浏览器窗口中看到以下输出：

Total: $323.95

## 在接口上应用扩展方法

我还可以创建适用于接口的扩展方法，这样可以在实现该接口的所有类上调用扩展方法。 清单4-21显示了更新以实现IEnumerable <Product>接口的ShoppingCart类。

Listing 4-21. 实现一个接口
```cs
using System.Collections;
using System.Collections.Generic;
namespace LanguageFeatures.Models {
public class ShoppingCart : IEnumerable<Product> {
public IEnumerable<Product> Products { get; set; }
public IEnumerator<Product> GetEnumerator() {
return Products.GetEnumerator();
}
IEnumerator IEnumerable.GetEnumerator() {
return GetEnumerator();
}
}
}
```

现在我可以更新扩展方法，以处理IEnumerable <Product>，如清单4-22所示。

Listing 4-22. 更新扩展方法
```cs
using System.Collections.Generic;
namespace LanguageFeatures.Models {
public static class MyExtensionMethods {
public static decimal TotalPrices(this IEnumerable<Product> products) {
decimal total = 0;
foreach (Product prod in products) {
total += prod?.Price ?? 0;
}
return total;
}
}
}
```
第一个参数类型已更改为IEnumerable<Product>，这意味着方法体中的foreach循环直接用于Product对象。 使用接口的更新意味着我可以计算任何IEnumerable <Product>枚举的Product对象的总值，其中包含ShoppingCart的实例，也可以计算Product对象的数组，如清单4-23所示。

Listing 4-23. 在数组上应用扩展方法
```cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
ShoppingCart cart
= new ShoppingCart { Products = Product.GetProducts() };
Product[] productArray = {
new Product {Name = "Kayak", Price = 275M},
new Product {Name = "Lifejacket", Price = 48.95M}
};
decimal cartTotal = cart.TotalPrices();
decimal arrayTotal = productArray.TotalPrices();
return View("Index", new string[] {
$"Cart Total: {cartTotal:C2}",
$"Array Total: {arrayTotal:C2}" });
}
}
}
```
如果您启动该项目，您将看到以下结果，这些结果表明我从扩展方法获得相同的结果，而不管Product对象如何收集：

Cart Total: $323.95
Array Total: $323.95

## 创建过滤扩展方法

关于扩展方法的最后一件事是，它们可以用来过滤对象的集合。在IEnumerable <T>上运行并且还返回IEnumerable <T>的扩展方法可以使用yield关键字将选择条件应用于源数据中的项，以产生一组过滤后的结果。 清单4-24演示了我已经添加到MyExtensionMethods类中的一种方法。

Listing 4-24. 增加过滤扩展方法
```cs
using System.Collections.Generic;
namespace LanguageFeatures.Models {
public static class MyExtensionMethods {
public static decimal TotalPrices(this IEnumerable<Product> products) {
decimal total = 0;

foreach (Product prod in products) {
total += prod?.Price ?? 0;
}
return total;
}
public static IEnumerable<Product> FilterByPrice(
this IEnumerable<Product> productEnum, decimal minimumPrice) {
foreach (Product prod in productEnum) {
if ((prod?.Price ?? 0) >= minimumPrice) {
yield return prod;
}
}
}
}
}
```

这种扩展方法称为FilterByPrice，它使用一个附加参数，允许我过滤产品，以便在结果中返回Price属性匹配或超过参数的Product对象。 清单4-25显示了正在使用的方法。

Listing 4-25. 使用过滤扩展方法
```cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
Product[] productArray = {
new Product {Name = "Kayak", Price = 275M},
new Product {Name = "Lifejacket", Price = 48.95M},
new Product {Name = "Soccer ball", Price = 19.50M},
new Product {Name = "Corner flag", Price = 34.95M}
};
decimal arrayTotal = productArray.FilterByPrice(20).TotalPrices();
return View("Index", new string[] { $"Array Total: {arrayTotal:C2}" });
}
}
}
```

当我在Product对象数组中调用FilterByPrice方法时，只有那些花费超过$ 20的那些方法被TotalPrices方法接收并用于计算总数。 如果您运行应用程序，您将在浏览器窗口中看到以下输出：

Total: $358.90

## 使用Lambda表达式

Lambda表达式是一个可以引起大量混乱的语言特征，特别是因为它们简化的功能也令人迷惑。 请参考我在上一节中定义的FilterByPrice扩展方法。 这个方法可以按价格过滤产品对象，这意味着如果我想按名称过滤，我必须创建另一个方法，如代码4-26所示。

Listing 4-26. 增加一个过滤方法
```cs
using System.Collections.Generic;
namespace LanguageFeatures.Models {
public static class MyExtensionMethods {
public static decimal TotalPrices(this IEnumerable<Product> products) {
decimal total = 0;
foreach (Product prod in products) {
total += prod?.Price ?? 0;
}
return total;
}
public static IEnumerable<Product> FilterByPrice(
this IEnumerable<Product> productEnum, decimal minimumPrice) {
foreach (Product prod in productEnum) {
if ((prod?.Price ?? 0) >= minimumPrice) {
yield return prod;
}
}
}
public static IEnumerable<Product> FilterByName(
this IEnumerable<Product> productEnum, char firstLetter) {
foreach (Product prod in productEnum) {
if (prod?.Name?[0] == firstLetter) {
yield return prod;
}
}
}
}
}
```
代码4-27显示了使用控制器中应用的两种过滤方法来创建两个不同的总计。

Listing 4-27. 使用两个方法
```cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
Product[] productArray = {
new Product {Name = "Kayak", Price = 275M},
new Product {Name = "Lifejacket", Price = 48.95M},
new Product {Name = "Soccer ball", Price = 19.50M},
new Product {Name = "Corner flag", Price = 34.95M}
};
decimal priceFilterTotal = productArray.FilterByPrice(20).TotalPrices();
decimal nameFilterTotal = productArray.FilterByName('S').TotalPrices();
return View("Index", new string[] {
$"Price Total: {priceFilterTotal:C2}",
$"Name Total: {nameFilterTotal:C2}" });
}
}
}
```

第一个过滤器选择价格大于$20的所有产品，第二个过滤名称以字母S开头的产品。 如果运行示例应用程序，您将在浏览器窗口中看到以下输出：

Price Total: $358.90
Name Total: $19.50

### 定义函数

我可以重复这个过程，并为每个属性和属性组合创建不同的过滤器方法。一个更优雅的方法是将处理枚举的代码与选择标准分开。 通过允许函数作为对象传递，C#很容易做到这一点。 清单4-28显示了一个单独的扩展方法，用于过滤Product对象的枚举，但是将结果中包含哪些数据由单独的函数来决定。

Listing 4-28. 创建一个通用的过滤方法
```cs
using System.Collections.Generic;
using System;
namespace LanguageFeatures.Models {
public static class MyExtensionMethods {

public static decimal TotalPrices(this IEnumerable<Product> products) {
decimal total = 0;
foreach (Product prod in products) {
total += prod?.Price ?? 0;
}
return total;
}
public static IEnumerable<Product> Filter(
this IEnumerable<Product> productEnum,
Func<Product, bool> selector) {
foreach (Product prod in productEnum) {
if (selector(prod)) {
yield return prod;
}
}
}
}
}    
```

Filter方法的第二个参数是接受Product对象并返回bool值的函数。 Filter方法调用每个Product对象的函数，如果该函数返回true，则将其包含在结果中。 要使用Filter方法，我可以指定一个方法或创建一个独立的函数，如清单4-29所示。

Listing 4-29. 使用过滤器函数
```cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
using System;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
bool FilterByPrice(Product p) {
return (p?.Price ?? 0) >= 20;
}
public ViewResult Index() {
Product[] productArray = {
new Product {Name = "Kayak", Price = 275M},
new Product {Name = "Lifejacket", Price = 48.95M},
new Product {Name = "Soccer ball", Price = 19.50M},
new Product {Name = "Corner flag", Price = 34.95M}
};

Func<Product, bool> nameFilter = delegate (Product prod) {
return prod?.Name?[0] == 'S';
};    
decimal priceFilterTotal = productArray
.Filter(FilterByPrice)
.TotalPrices();
decimal nameFilterTotal = productArray
.Filter(nameFilter)
.TotalPrices();
return View("Index", new string[] {
$"Price Total: {priceFilterTotal:C2}",
$"Name Total: {nameFilterTotal:C2}" });
}
}
}
```

这两种方法都不理想。 定义如FilterByPrice方法必须定义一个类。 创建Func <Product，bool>对象避免了这个问题，但是要使用难以阅读和难以维护的语法。 为解决这个问题，我们可以用lambda表达式以更优雅和表现力的方式定义函数，如清单4-30所示。

Listing 4-30. 使用Lambda表达式
```cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
using System;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
Product[] productArray = {
new Product {Name = "Kayak", Price = 275M},
new Product {Name = "Lifejacket", Price = 48.95M},
new Product {Name = "Soccer ball", Price = 19.50M},
new Product {Name = "Corner flag", Price = 34.95M}
};
decimal priceFilterTotal = productArray
.Filter(p => (p?.Price ?? 0) >= 20)
.TotalPrices();
decimal nameFilterTotal = productArray
.Filter(p => p?.Name?[0] == 'S')
.TotalPrices();
return View("Index", new string[] {
$"Price Total: {priceFilterTotal:C2}",
$"Name Total: {nameFilterTotal:C2}" });
}
}
}
```

上面的lambda表达式以粗体显示。这里使用了无类型参数，编译器将自动推断类型。 =>字符朗读为“转到”并将参数链接到lambda表达式的结果。 在我的例子中，一个名为p的Product参数转到bool结果，如果第一个表达式中Price属性等于或大于20，或者Name属性以第二个表达式中的S开头，则该参数将为true。 该代码的工作方式与单独的方法和函数委托相同，但更简洁，对大多数人来说更容易阅读。

我不需要在lambda表达式中表达我的委托的逻辑。 可以轻松地调用一个方法，像这样：

prod => EvaluateProduct（prod）

如果我需要一个具有多个参数的委托的lambda表达式，我必须用括号括起参数，如下所示：

（prod，count）=> prod.Price> 20 && count> 0

最后，如果我需要在lambda表达式中需要多个语句的逻辑，我可以通过使用大括号（{}）并完成一个返回语句，如下所示：

（prod，count）=> {
// ...多个代码语句...
return result;
}

您不需要非使用lambda表达式不可，但是它能以可读和清晰的方式让复杂函数更加整洁。 我非常喜欢他们，你会看到，在这本书中我用了很多。

### 使用Lambda 表达式方法和属性
C#6已经扩展了对lambda表达式的支持，以便它们可以用于实现方法和属性。 在MVC开发中，尤其是在编写控制器的时候，你会经常使用包含一个选择要显示的数据和要呈现的视图的单个语句的方法。 在清单4-31中，我已经重写了Index操作方法，使其遵循这个常见模式。

Listing 4-31. 创建通用的 Action 模式
```cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
using System;
using System.Linq;

namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
return View(Product.GetProducts().Select(p => p?.Name));
}
}
}
```
这个Action方法从静态Product.GetProducts方法获取Product对象的集合,并使用LINQ来投影Name属性的值，然后将其用作默认视图的视图模型。 如果运行应用程序，您将在浏览器窗口中看到以下输出：

Kayak
Lifejacket

浏览器窗口中也会有一个空的列表项，因为GetProducts方法在其结果中包含一个空引用，但对本章的这一部分无关紧要。当一个方法体由单个语句组成时，它可以 被重写为lambda表达式如代码4-32所示。

Listing 4-32. 使用Lambda表达式的Action 方法
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
using System;
using System.Linq;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public ViewResult Index() =>
View(Product.GetProducts().Select(p => p?.Name));
}
}

方法的Lambda表达式忽略return关键字，并使用=>（转到）将方法签名（包括其参数）与其实现相关联。 清单4-32所示的Index方法的工作方式与清单4-31所示的方法相同，但更简洁。也可以使用相同的基本方法来定义属性。 列表4-33显示了添加一个使用lambda表达式给Product类的属性。

Listing 4-33. Lambda 表达式属性
```cs
namespace LanguageFeatures.Models {
public class Product {
public Product(bool stock = true) {
InStock = stock;
}

public string Name { get; set; }
public string Category { get; set; } = "Watersports";
public decimal? Price { get; set; }
public Product Related { get; set; }
public bool InStock { get; }
public bool NameBeginsWithS => Name?[0] == 'S';
public static Product[] GetProducts() {
Product kayak = new Product {
Name = "Kayak",
Category = "Water Craft",
Price = 275M
};
Product lifejacket = new Product(false) {
Name = "Lifejacket",
Price = 48.95M
};
kayak.Related = lifejacket;
return new Product[] { kayak, lifejacket, null };
}
}
}

```

## 使用类型推断和匿名类型

C# var关键字允许您定义一个局部变量而不显式指定变量类型，如清单4-34所示。 这称为类型推断或隐式类型。

Listing 4-34. 使用类型推断
```cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
using System;
using System.Linq;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
var names = new [] { "Kayak", "Lifejacket", "Soccer ball" };
return View(names);
}
}
}
```
这不是名称变量没有类型; 相反，我要求编译器推断类型。 编译器检查数组声明，并确定它是一个字符串数组。 运行示例生成以下输出：

Kayak
Lifejacket
Soccer ball

### 使用匿名类型

通过组合对象初始化器和类型推断，我可以创建简单的视图模型对象，这些对象在控制器和视图之间传输数据非常有用，而不必定义类或结构体，如代码清单4-35所示。


Listing 4-35. 创建匿名类型
```cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
using System;
using System.Linq;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
var products = new [] {
new { Name = "Kayak", Price = 275M },
new { Name = "Lifejacket", Price = 48.95M },
new { Name = "Soccer ball", Price = 19.50M },
new { Name = "Corner flag", Price = 34.95M }
};
return View(products.Select(p => p.Name));
}
}
}
```
product数组中的每个对象都是一个匿名类型的对象。 这并不意味着它在JavaScript变量是动态的意义上是动态的。 这仅仅意味着类型定义将由编译器自动创建。 强类型仍然执行。 例如，您可以获取并设置在初始化程序中定义的属性。 如果运行该示例，您将在浏览器窗口中看到以下输出：

Kayak
Lifejacket
Soccer ball
Corner flag

C＃编译器根据初始化程序中参数的名称和类型生成类。具有相同属性名称和类型的两个匿名类型的对象将被分配给相同的自动生成的类。 这意味着产品数组中的所有对象将具有相同的类型，因为它们定义了相同的属性。

提示: 我必须使用var关键字来定义匿名类型对象的数组，因为在编译代码之前不会创建类型，所以我不知道要使用的类型的名称。 匿名类型对象数组中的元素必须都定义相同的属性; 否则，编译器无法知道数组是什么类型。

为了演示这一问题，我已经从清单4-36中的示例更改了输出，以便它显示了类型名称，而不是Name属性的值。


Listing 4-36. 显示匿名类型名

```cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
using System;
using System.Linq;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
var products = new [] {
new { Name = "Kayak", Price = 275M },
new { Name = "Lifejacket", Price = 48.95M },
new { Name = "Soccer ball", Price = 19.50M },
new { Name = "Corner flag", Price = 34.95M }
};
return View(products.Select(p => p.GetType().Name));
}
}
}
```

数组中的所有对象都已分配相同的类型，如果运行该示例，则可以看到该对象。 类型名称不是用户友好的，但不须直接使用，您看到的可能与以下输出中显示的不同：

<>f__AnonymousType0`2
<>f__AnonymousType0`2
<>f__AnonymousType0`2
<>f__AnonymousType0`2

## 使用异步方法

C#中最新增加一个语言特性是改进异步方法的处理方式。 异步方法在后台工作，并在完成后通知您，当执行后台工作时，允许您的代码照顾其他业务。 异步方法是从代码中消除瓶颈的重要工具，允许应用程序利用多个处理器和多处理器核心并行执行工作。在MVC中，可以使用异步方法来提高应用程序的整体性能，它允许服务器在调度和执行请求的方式上具有更大的灵活性。 两个C#关键字-async 和 await用于异步执行工作。
为了准备这个部分，我需要在示例项目中添加一个新的.NET程序集，以便我可以进行异步HTTP请求。 清单4-37显示了我对project.json文件的依赖关系部分的添加。

Listing 4-37. 添加依赖的程序集
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
"Microsoft.AspNetCore.Mvc": "1.0.0",
"System.Net.Http": "4.1.0"
},
```

当保存project.json文件时，Visual Studio将下载System.Net.Http程序集并将其添加到项目中。 我在第6章更详细地描述这个过程。


### 直接使用任务

C#和.NET对异步方法有很好的支持，但代码往往是冗长的，而不习惯于并行编程的开发人员常常因不寻常的语法陷入僵局。 例如，清单4-38显示了一个称为GetPageLength的异步方法，它在MyAsyncMethods类中定义，并添加到名为MyAsyncMethods.cs的类文件中的Models文件夹中。

Listing 4-38. MyAsyncMethods.cs
```cs
using System.Net.Http;
using System.Threading.Tasks;
namespace LanguageFeatures.Models {
public class MyAsyncMethods {
public static Task<long?> GetPageLength() {
HttpClient client = new HttpClient();
var httpTask = client.GetAsync("http://apress.com");
// we could do other things here while the HTTP request is performed
return httpTask.ContinueWith((Task<HttpResponseMessage> antecedent) => {
return antecedent.Result.Content.Headers.ContentLength;
});
}
}
}    
```

此方法使用System.Net.Http.HttpClient对象请求Apress主页的内容页面并返回其长度。 .NET表示将作为Task异步执行的工作。 基于后台工作产生的结果，任务对象是强类型的。 所以，当我调用HttpClient.GetAsync方法，我得到的是一个Task <HttpResponseMessage>。 这告诉我，请求将在后台执行，请求的结果将是一个HttpResponseMessage对象。

提示: 当我使用像background这样的词时跳过了很多细节，以便提出要点,这对于MVC的世界很重要。 对异步方法和并行编程的.NET支持一般是非常好的，如果要创建可以利用多核和多处理器硬件的真正高性能应用程序，我们鼓励您更多地了解它。 当我介绍不同的功能时，您将看到MVC如何在本书中创建异步Web应用程序。

令大多数程序员感到困惑的部分是延续(Continuation)，这指的是在后台任务完成时指定要发生什么。 在这个例子中，我使用了ContinueWith方法来处理从HttpClient获取的HttpResponseMessage对象。
GetAsync方法，我使用一个lambda表达式返回一个属性的值，该属性包含从Apress Web服务器获取的内容的长度。 这是Continuation代码：

```cs
return httpTask.ContinueWith((Task<HttpResponseMessage> antecedent) => {
return antecedent.Result.Content.Headers.ContentLength;
});
```

请注意，我使用return关键字两次。 这是导致混乱的部分。 第一次使用return关键字指定我正在返回一个Task <HttpResponseMessage> 对象，当任务完成时，它将返回ContentLength头的长度。 ContentLength标头返回一个long？result（一个可空的long值），这表示我的GetPageLength方法的结果是Task <long？>，像这样：
...
public static Task<long?> GetPageLength() {
...

如果看不明白，不要担心，别人也和你差不多。 正是因为这个原因 Microsoft将关键字添加到C#中以简化异步方法。

##使用async 和await 关键字

Microsoft向C#引入了两个关键字，它们专门用于简化使用异步方法（如HttpClient.GetAsync）。 关键字是async、await，您可以看到我如何使用它们来简化清单4-39中的示例方法。


Listing 4-39. 使用async 和 await 关键字
```cs
using System.Net.Http;
using System.Threading.Tasks;
namespace LanguageFeatures.Models {
public class MyAsyncMethods {
public async static Task<long?> GetPageLength() {
HttpClient client = new HttpClient();
var httpMessage = await client.GetAsync("http://apress.com");
return httpMessage.Content.Headers.ContentLength;
}
}
}
```
调用异步方法时，我使用了await关键字。这告诉C#编译器，我想等待GetAsync方法返回的任务的结果，然后以相同的方法继续执行其他语句。应用await关键字意味着我可以将GetAsync方法的结果视为一种常规方法，并将HttpResponseMessage对象分配给一个变量。而且，更好的是，我可以用正常的方式使用return关键字来生成其他方法的结果 - 在这里是ContentLength属性的值。这是很自然的表达方式，我不必担心ContinueWith方法和return关键字的多次使用。当您使用await关键字时，还必须将async关键字添加到方法声明中，如我在示例中所做的那样。方法结果类型不会改变 - 我的示例GetPageLength方法仍然返回一个Task <long？>。这是因为使用一些聪明的编译器技巧实现等待和异步，它们允许更自然的语法，但是使用它们时，并不会更改方法中的逻辑。调用GetPageLength方法的人还需要处理一个 Task <long？>结果是因为后台还有一个可以产生可空的long类型的任务，尽管这个程序员也可以选择使用await和async关键字。

这种模式贯穿到MVC控制器中，这使得编写异步动作方法变得很容易，如清单4-40所示。

Listing 4-40. 定义异步Action方法
```cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
using System;
using System.Linq;
using System.Threading.Tasks;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public async Task<ViewResult> Index() {
long? length = await MyAsyncMethods.GetPageLength();
return View(new string[] { $"Length: {length}" });
}
}
}
```

我将Index操作方法的结果更改为Task<ViewResult>，它告诉MVC，action方法将返回一个任务，该任务会在完成后产生一个ViewResult对象，提供应该呈现的视图的详细信息和它需要的数据。 我已经将async关键字添加到方法的定义中，这允许我在调用MyAsyncMethods.GetPathLength方法时使用await关键字。 MVC和.NET负责处理这些Continuation，这些是易于编写，易于阅读和易于维护的异步代码。 如果运行应用程序，您将看到类似于以下内容的输出：

Length: 62164

## 获取名字

Web应用程序开发中有许多任务需要引用参数，变量，方法或类的名称。 常见的示例包括在处理来自用户的输入时抛出异常或创建验证错误。 传统的方法是使用名称硬编码的字符串值，如清单4-41所示。

Listing 4-41. 硬编码名字
```cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
using System;
using System.Linq;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
var products = new [] {
new { Name = "Kayak", Price = 275M },
new { Name = "Lifejacket", Price = 48.95M },
new { Name = "Soccer ball", Price = 19.50M },
new { Name = "Corner flag", Price = 34.95M }
};
return View(products.Select(p => $"Name: {p.Name}, Price: {p.Price}"));
}
}
}
```
对LINQ Select方法的调用生成一个字符串序列，每个字符串都包含对Name和Price属性的硬编码引用。 运行应用程序在浏览器窗口中生成以下输出：
```
Name: Kayak, Price: 275
Name: Lifejacket, Price: 48.95
Name: Soccer ball, Price: 19.50
Name: Corner flag, Price: 34.95
```
这种方法的问题是它容易出错，因为名称有可能拼写错误或代码重构之后字符串中的名称未正确更新。 结果可能会导致误导，向用户显示的消息可能会有问题。 C# 6引入了表达式的名称，编译器负责生成一个名称字符串，如清单4-42所示。

Listing 4-42. 使用nameof 表达式

using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using LanguageFeatures.Models;
using System;
using System.Linq;
namespace LanguageFeatures.Controllers {
public class HomeController : Controller {
public ViewResult Index() {
var products = new [] {
new { Name = "Kayak", Price = 275M },
new { Name = "Lifejacket", Price = 48.95M },
new { Name = "Soccer ball", Price = 19.50M },
new { Name = "Corner flag", Price = 34.95M }
};
return View(products.Select(p =>
$"{nameof(p.Name)}: {p.Name}, {nameof(p.Price)}: {p.Price}"));
}
}
}

编译器处理诸如p.Name的引用，以便只有最后一部分包含在字符串中，产生与前面示例相同的输出。 Visual Studio为名称表达式提供IntelliSense支持，因此将提示您选择引用，并在重构代码时正确更新表达式。 由于编译器负责处理nameof，因此使用无效的引用将导致编译器错误，从而防止错误或过期的引用异常通知。

本章小结

本章概述了能让程序员提高效率的MVC关键的C#语言特性。 C#是一种灵活的语言，通常有不同的方法来解决一个问题，但这里所介绍的是您在Web应用程序开发过程中最常遇到的功能，并且在本书的整个示例中都可以看到。 在下一章中，我将介绍Razor视图引擎，并介绍如何在MVC Web应用程序中生成动态内容。

