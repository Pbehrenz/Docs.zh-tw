---
title: "從 ASP.NET Web API 移轉"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4f0564b4-ed4e-4e1e-9755-c1144d21a0ef
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/webapi
ms.openlocfilehash: 2dd2d40aef3803ad2f75504920a1174fee5c2444
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-from-aspnet-web-api"></a><span data-ttu-id="0984f-103">從 ASP.NET Web API 移轉</span><span class="sxs-lookup"><span data-stu-id="0984f-103">Migrating from ASP.NET Web API</span></span>

<span data-ttu-id="0984f-104">由[Steve Smith](https://ardalis.com/)和[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="0984f-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="0984f-105">Web 應用程式開發介面為連線用戶端，包括瀏覽器和行動裝置的較大範圍的 HTTP 服務。</span><span class="sxs-lookup"><span data-stu-id="0984f-105">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="0984f-106">ASP.NET Core MVC 包括建置 Web 應用程式開發介面提供單一且一致的方式，建立 web 應用程式的支援。</span><span class="sxs-lookup"><span data-stu-id="0984f-106">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="0984f-107">在本文中，我們會示範從 ASP.NET Web API 的 Web API 實作移轉至 ASP.NET Core MVC 所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="0984f-107">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

[<span data-ttu-id="0984f-108">檢視或下載範例程式碼</span><span class="sxs-lookup"><span data-stu-id="0984f-108">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample)

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="0984f-109">檢閱 ASP.NET Web API 專案</span><span class="sxs-lookup"><span data-stu-id="0984f-109">Review ASP.NET Web API Project</span></span>

<span data-ttu-id="0984f-110">本文使用範例專案， *ProductsApp*建立發行項的[開始使用 ASP.NET Web API](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)做為起點。</span><span class="sxs-lookup"><span data-stu-id="0984f-110">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="0984f-111">在該專案中，簡單的 ASP.NET Web API 專案設定，如下所示。</span><span class="sxs-lookup"><span data-stu-id="0984f-111">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="0984f-112">在*Global.asax.cs*，進行呼叫以`WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="0984f-112">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

<span data-ttu-id="0984f-113">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="0984f-113">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]</span></span>

<span data-ttu-id="0984f-114">`WebApiConfig`定義於*App_Start*，且具有一個靜態`Register`方法：</span><span class="sxs-lookup"><span data-stu-id="0984f-114">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

<span data-ttu-id="0984f-115">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]</span><span class="sxs-lookup"><span data-stu-id="0984f-115">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]</span></span>


<span data-ttu-id="0984f-116">這個類別會設定[屬性路由](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)，但實際上不會在專案中使用。</span><span class="sxs-lookup"><span data-stu-id="0984f-116">This class configures [attribute routing](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="0984f-117">它也會設定 ASP.NET Web API 所使用的路由表。</span><span class="sxs-lookup"><span data-stu-id="0984f-117">It also configures the routing table which is used by ASP.NET Web API.</span></span> <span data-ttu-id="0984f-118">在此情況下，ASP.NET Web 應用程式開發介面會預期以符合格式的 Url */api/ {controller} / {id}*，與*{id}*為選擇性。</span><span class="sxs-lookup"><span data-stu-id="0984f-118">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="0984f-119">*ProductsApp*專案包含一個簡單的控制器，繼承自`ApiController`和公開兩個方法：</span><span class="sxs-lookup"><span data-stu-id="0984f-119">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

<span data-ttu-id="0984f-120">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]</span><span class="sxs-lookup"><span data-stu-id="0984f-120">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]</span></span>

<span data-ttu-id="0984f-121">最後，模型*產品*中，由*ProductsApp*，是簡單的類別：</span><span class="sxs-lookup"><span data-stu-id="0984f-121">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

<span data-ttu-id="0984f-122">[!code-csharp[Main](webapi/sample/ProductsApp/Models/Product.cs)]</span><span class="sxs-lookup"><span data-stu-id="0984f-122">[!code-csharp[Main](webapi/sample/ProductsApp/Models/Product.cs)]</span></span>

<span data-ttu-id="0984f-123">現在，我們有簡單的專案開始，我們可以示範如何將這個 Web API 專案移轉至 ASP.NET Core MVC。</span><span class="sxs-lookup"><span data-stu-id="0984f-123">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="0984f-124">建立目的專案</span><span class="sxs-lookup"><span data-stu-id="0984f-124">Create the Destination Project</span></span>

<span data-ttu-id="0984f-125">使用 Visual Studio，建立新的空白方案，並將它命名*WebAPIMigration*。</span><span class="sxs-lookup"><span data-stu-id="0984f-125">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="0984f-126">加入現有*ProductsApp*專案，然後將新的 ASP.NET Core Web 應用程式專案加入方案。</span><span class="sxs-lookup"><span data-stu-id="0984f-126">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="0984f-127">將新專案*ProductsCore*。</span><span class="sxs-lookup"><span data-stu-id="0984f-127">Name the new project *ProductsCore*.</span></span>

![網站範本來開啟 [新增專案] 對話方塊](webapi/_static/add-web-project.png)

<span data-ttu-id="0984f-129">接下來，選擇 Web API 專案範本。</span><span class="sxs-lookup"><span data-stu-id="0984f-129">Next, choose the Web API project template.</span></span> <span data-ttu-id="0984f-130">我們將會移轉*ProductsApp*內容到這個新的專案。</span><span class="sxs-lookup"><span data-stu-id="0984f-130">We will migrate the *ProductsApp* contents to this new project.</span></span>

![ASP.NET Core 範本清單中選取的 Web API 的專案範本的新 Web 應用程式對話方塊](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="0984f-132">刪除`Project_Readme.html`檔案從新的專案。</span><span class="sxs-lookup"><span data-stu-id="0984f-132">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="0984f-133">您的方案現在看起來應該像這樣：</span><span class="sxs-lookup"><span data-stu-id="0984f-133">Your solution should now look like this:</span></span>

![在 [方案總管] 中顯示檔案和資料夾的 ProductsApp 和 ProductsCore 專案中開啟應用程式方案](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="0984f-135">移轉設定</span><span class="sxs-lookup"><span data-stu-id="0984f-135">Migrate Configuration</span></span>

<span data-ttu-id="0984f-136">不會再使用 ASP.NET Core *Global.asax*， *web.config*，或*App_Start*資料夾。</span><span class="sxs-lookup"><span data-stu-id="0984f-136">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="0984f-137">相反地，完成所有啟動工作*Startup.cs*專案的根目錄中 (請參閱[應用程式啟動](../fundamentals/startup.md))。</span><span class="sxs-lookup"><span data-stu-id="0984f-137">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="0984f-138">在 ASP.NET Core MVC 中，屬性為基礎的路由現在會包含預設時`UseMvc()`稱為; 而且，這是建議的方法來設定 Web API 路由 （而且是 Web API 入門專案處理路由的方式）。</span><span class="sxs-lookup"><span data-stu-id="0984f-138">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

<span data-ttu-id="0984f-139">[!code-none[Main](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]</span><span class="sxs-lookup"><span data-stu-id="0984f-139">[!code-none[Main](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]</span></span>

<span data-ttu-id="0984f-140">假設您想要使用屬性路由從現在開始在專案中，不需要進行其他設定。</span><span class="sxs-lookup"><span data-stu-id="0984f-140">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="0984f-141">只需套用的屬性視您的控制器和動作，如同您在此範例`ValuesController`Web API 的入門專案中包含的類別：</span><span class="sxs-lookup"><span data-stu-id="0984f-141">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that is included in the Web API starter project:</span></span>

<span data-ttu-id="0984f-142">[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]</span><span class="sxs-lookup"><span data-stu-id="0984f-142">[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]</span></span>

<span data-ttu-id="0984f-143">請注意是否存在*[控制器]*第 8 行上。</span><span class="sxs-lookup"><span data-stu-id="0984f-143">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="0984f-144">屬性為基礎的路由現在支援特定語彙基元，例如*[控制器]*和*[動作]*。</span><span class="sxs-lookup"><span data-stu-id="0984f-144">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="0984f-145">這些語彙基元會取代在執行階段的控制器或動作，名稱分別屬性套用。</span><span class="sxs-lookup"><span data-stu-id="0984f-145">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="0984f-146">這是用來減少的識別字串，在專案中，並且確保路由將會保留其對應的控制器和動作與同步處理時自動重新命名重構功能，就會套用。</span><span class="sxs-lookup"><span data-stu-id="0984f-146">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="0984f-147">若要移轉的產品的 API 控制器，我們必須先將複製*ProductsController*到新的專案。</span><span class="sxs-lookup"><span data-stu-id="0984f-147">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="0984f-148">然後只需要包含在控制器上的路由屬性：</span><span class="sxs-lookup"><span data-stu-id="0984f-148">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="0984f-149">您也需要加入`[HttpGet]`屬性設定為兩種方法，因為它們都應該會透過 HTTP Get 呼叫。</span><span class="sxs-lookup"><span data-stu-id="0984f-149">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="0984f-150">針對屬性中包含的 「 識別碼 」 參數的預期`GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="0984f-150">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="0984f-151">此時，路由已正確設定。不過，我們無法尚未測試它。</span><span class="sxs-lookup"><span data-stu-id="0984f-151">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="0984f-152">必須先進行其他變更*ProductsController*會進行編譯。</span><span class="sxs-lookup"><span data-stu-id="0984f-152">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="0984f-153">移轉模型和控制站</span><span class="sxs-lookup"><span data-stu-id="0984f-153">Migrate Models and Controllers</span></span>

<span data-ttu-id="0984f-154">這個簡單的 Web API 專案的移轉程序的最後一個步驟是複製到控制器，他們使用的任何模型。</span><span class="sxs-lookup"><span data-stu-id="0984f-154">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="0984f-155">在此情況下，只需複製*Controllers/ProductsController.cs*從原始專案到新的專案。</span><span class="sxs-lookup"><span data-stu-id="0984f-155">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="0984f-156">然後，將整個模型資料夾從原始專案複製到新的專案。</span><span class="sxs-lookup"><span data-stu-id="0984f-156">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="0984f-157">調整以符合新的專案名稱的命名空間 (*ProductsCore*)。</span><span class="sxs-lookup"><span data-stu-id="0984f-157">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="0984f-158">此時，您可以建置應用程式，而您會發現多個編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="0984f-158">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="0984f-159">這些通常應該可分成下列類別：</span><span class="sxs-lookup"><span data-stu-id="0984f-159">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="0984f-160">*ApiController*不存在</span><span class="sxs-lookup"><span data-stu-id="0984f-160">*ApiController* does not exist</span></span>

* <span data-ttu-id="0984f-161">*System.Web.Http*命名空間不存在</span><span class="sxs-lookup"><span data-stu-id="0984f-161">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="0984f-162">*IHttpActionResult*不存在</span><span class="sxs-lookup"><span data-stu-id="0984f-162">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="0984f-163">幸運的是，這些是所有很容易修正：</span><span class="sxs-lookup"><span data-stu-id="0984f-163">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="0984f-164">變更*ApiController*至*控制器*(您可能需要加入*使用 Microsoft.AspNetCore.Mvc*)</span><span class="sxs-lookup"><span data-stu-id="0984f-164">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="0984f-165">刪除任何使用陳述式參考*System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="0984f-165">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="0984f-166">變更任何方法傳回*IHttpActionResult*傳回*IActionResult*</span><span class="sxs-lookup"><span data-stu-id="0984f-166">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="0984f-167">一旦這些變更都已和未使用 using 陳述式移除，移轉*ProductsController*類別看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="0984f-167">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

<span data-ttu-id="0984f-168">[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]</span><span class="sxs-lookup"><span data-stu-id="0984f-168">[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]</span></span>

<span data-ttu-id="0984f-169">您現在應該能夠執行移轉的專案，並瀏覽至*/api/產品*; 而且，您應該會看到 3 產品的完整清單。</span><span class="sxs-lookup"><span data-stu-id="0984f-169">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="0984f-170">瀏覽至*/api/products/1*和您應該會看到第一次的產品。</span><span class="sxs-lookup"><span data-stu-id="0984f-170">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="summary"></a><span data-ttu-id="0984f-171">總結</span><span class="sxs-lookup"><span data-stu-id="0984f-171">Summary</span></span>

<span data-ttu-id="0984f-172">將簡單的 ASP.NET Web API 專案移轉至 ASP.NET Core MVC 就相當簡單，這適用於 ASP.NET Core MVC 中 Web Api 的內建支援。</span><span class="sxs-lookup"><span data-stu-id="0984f-172">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="0984f-173">每個 ASP.NET Web API 專案必須移轉的主要部分是路由、 控制器、 和模型，以及更新至控制器和動作所使用的類型。</span><span class="sxs-lookup"><span data-stu-id="0984f-173">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>