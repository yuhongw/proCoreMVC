#准备示例项目
在本章中，我使用空模板创建了一个名为 ConfigurationApps 的新项目。 在配置应用程序之前，我将做好一些基础的准备工作。

我将在本章中使用 Bootstrap 来设计 HTML，所以我使用 Bower，创建了 bower.json 文件作为项目文件模板，添加了如清单14-1中所示的软件包。

*清单 14-1. 在 bower.json 文件中添加 bootstrap*
```
{
"name": "asp.net",
"private": true,
"dependencies": {
"bootstrap": "3.3.6"
    }
}
```
接下来，我创建了 Controllers 文件夹，并添加了一个名为 HomeController.cs 的类文件，就如同我在清单14-2中定义一样。

*清单 14-2. Controllers 文件夹中 HomeController.cs 文件详情*
```
using System.Collections.Generic;
using Microsoft.AspNetCore.Mvc;

namespace ConfiguringApps.Controllers {

public class HomeController : Controller {
    public ViewResult Index()
        => View(new Dictionary<string, string> {
            ["Message"] = "This is the Index action"
        });
    }
}
```
我创建了 Views/Home 文件夹，并添加了一个名为 Index.cshtml 的试图文件，如清单14-3所示。

*清单 14-3.  Views/Home 文件夹中 Index.cshtml 文件详情*
```
@model Dictionary<string, string>
@{ Layout = null; }
<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <link asp-href-include="lib/bootstrap/dist/css/*.min.css" rel="stylesheet" />
    <title>Result</title>
</head>
<body class="panel-body">
    <table class="table table-condensed table-bordered table-striped">
    @foreach (var kvp in Model) {
        <tr><th>@kvp.Key</th><td>@kvp.Value</td></tr>
        }
    </table>
</body>
</html>
```
视图中的链接元素依赖于内置的能够匹配 Bootstrap CSS 的文件标签助手。要启用内置的标签助手，在 Views 文件夹中使用 MVC 视图导入页面模板创建 _ViewImports.cshtml 文件，并添加如清单14-4中的表达式。

*清单 14-4.  Views 文件夹中 _ViewImports.cshtml File 文件详情*
```
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
```

---

■ **注意** 此时应用程序不会构建或运行，因为控制器和视图依赖于项目中不存在的功能。我在下面的部分中介绍这一点，并介绍如何配置 ASP.NET Core MVC 应用程序。

---
