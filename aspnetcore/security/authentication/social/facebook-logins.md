---
title: "在 ASP.NET Core Facebook 外部登入安裝程式"
author: rick-anderson
description: "在 ASP.NET Core Facebook 外部登入安裝程式"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 8/1/2017
ms.topic: article
ms.assetid: 8c65179b-688c-4af1-8f5e-1862920cda95
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/facebook-logins
ms.openlocfilehash: da019ad3fd6cefa23b8331c98cc36e50ac9c1105
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="configuring-facebook-authentication"></a><span data-ttu-id="fde12-104">設定 Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="fde12-104">Configuring Facebook authentication</span></span>

<a name=security-authentication-facebook-logins></a>

<span data-ttu-id="fde12-105">由[Valeriy Novytskyy](https://github.com/01binary)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fde12-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fde12-106">本教學課程會示範如何讓使用者以使用範例 ASP.NET Core 2.0 專案上建立其 Facebook 帳戶登入[上一頁](index.md)。</span><span class="sxs-lookup"><span data-stu-id="fde12-106">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span> <span data-ttu-id="fde12-107">我們先建立 Facebook 應用程式識別碼依照[官方步驟](https://www.facebook.com/unsupportedbrowser)。</span><span class="sxs-lookup"><span data-stu-id="fde12-107">We start by creating a Facebook App ID by following the [official steps](https://www.facebook.com/unsupportedbrowser).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="fde12-108">在 Facebook 中建立應用程式</span><span class="sxs-lookup"><span data-stu-id="fde12-108">Create the app in Facebook</span></span>

*  <span data-ttu-id="fde12-109">瀏覽至[Facebook 開發人員](https://www.facebook.com/unsupportedbrowser)頁面上，並登入。</span><span class="sxs-lookup"><span data-stu-id="fde12-109">Navigate to the [Facebook for Developers](https://www.facebook.com/unsupportedbrowser) page and sign in.</span></span> <span data-ttu-id="fde12-110">如果您還沒有的 Facebook 帳戶，使用**註冊 Facebook**建立一個登入頁面上的連結。</span><span class="sxs-lookup"><span data-stu-id="fde12-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="fde12-111">點選**建立應用程式**按鈕右上角來建立新的應用程式識別碼。</span><span class="sxs-lookup"><span data-stu-id="fde12-111">Tap the **Create App** button in the upper right corner to create a new App ID.</span></span>

   ![在 Microsoft Edge 開啟 Facebook 開發人員入口網站](index/_static/FBMyApps.png)

* <span data-ttu-id="fde12-113">填寫表單，然後點選**建立應用程式識別碼** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="fde12-113">Fill out the form and tap the **Create App ID** button.</span></span>

   ![建立新的應用程式識別碼的表單](index/_static/FBNewAppId.png)

* <span data-ttu-id="fde12-115">當有**選取產品**提示字元中，按一下**設定**上**Facebook 登入**卡。</span><span class="sxs-lookup"><span data-stu-id="fde12-115">When presented with **Select a product** prompt, Click **Set Up** on the **Facebook Login** card.</span></span>

   ![產品安裝程式頁面](index/_static/FBProductSetup.png)

* <span data-ttu-id="fde12-117">**快速入門**精靈將會啟動具有**選擇平台**做為第一頁。</span><span class="sxs-lookup"><span data-stu-id="fde12-117">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="fde12-118">立即略過此精靈，即可**設定**左側功能表中的連結：</span><span class="sxs-lookup"><span data-stu-id="fde12-118">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

   ![略過快速入門](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="fde12-120">您會看到**用戶端的 OAuth 設定**頁面：</span><span class="sxs-lookup"><span data-stu-id="fde12-120">You are presented with the **Client OAuth Settings** page:</span></span>

![用戶端的 OAuth 設定頁面](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="fde12-122">輸入您的開發 URI 與*/signin-facebook*附加到**有效的 OAuth 重新導向 Uri**欄位 (例如： `https://localhost:44320/signin-facebook`)。</span><span class="sxs-lookup"><span data-stu-id="fde12-122">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="fde12-123">稍後在本教學課程中設定的 Facebook 驗證將會自動處理在要求*/signin-facebook*實作 OAuth 流程的路由。</span><span class="sxs-lookup"><span data-stu-id="fde12-123">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="fde12-124">按一下**儲存變更**。</span><span class="sxs-lookup"><span data-stu-id="fde12-124">Click **Save Changes**.</span></span>

* <span data-ttu-id="fde12-125">按一下**儀表板**位於左邊功能窗格中的連結。</span><span class="sxs-lookup"><span data-stu-id="fde12-125">Click the **Dashboard** link in the left navigation.</span></span> 

    <span data-ttu-id="fde12-126">這個頁面上，請記下您`App ID`和您`App Secret`。</span><span class="sxs-lookup"><span data-stu-id="fde12-126">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="fde12-127">您將在下一節加入到您的 ASP.NET Core 應用程式：</span><span class="sxs-lookup"><span data-stu-id="fde12-127">You will add both into your ASP.NET Core application in the next section:</span></span>

   ![Facebook 開發人員儀表板](index/_static/FBDashboard.png)

* <span data-ttu-id="fde12-129">部署站台時需要檢討**Facebook 登入**安裝頁面上，並註冊新的公用 URI。</span><span class="sxs-lookup"><span data-stu-id="fde12-129">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="fde12-130">儲存 Facebook 應用程式識別碼和應用程式密碼</span><span class="sxs-lookup"><span data-stu-id="fde12-130">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="fde12-131">連結機密設定，例如 Facebook`App ID`和`App Secret`您應用程式組態使用[密碼管理員](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="fde12-131">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="fde12-132">基於本教學課程的目的，名稱語彙基元`Authentication:Facebook:AppId`和`Authentication:Facebook:AppSecret`。</span><span class="sxs-lookup"><span data-stu-id="fde12-132">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

## <a name="configure-facebook-authentication"></a><span data-ttu-id="fde12-133">設定 Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="fde12-133">Configure Facebook Authentication</span></span>

<span data-ttu-id="fde12-134">在本教學課程中使用的專案範本，可確保[Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook)封裝是否已經安裝。</span><span class="sxs-lookup"><span data-stu-id="fde12-134">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package is already installed.</span></span>

* <span data-ttu-id="fde12-135">若要安裝此套件與 Visual Studio 2017，以滑鼠右鍵按一下專案，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="fde12-135">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="fde12-136">若要安裝的.NET Core CLI，請在您的專案目錄中執行下列：</span><span class="sxs-lookup"><span data-stu-id="fde12-136">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fde12-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fde12-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fde12-138">加入在 Facebook 服務`ConfigureServices`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="fde12-138">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

<span data-ttu-id="fde12-139">`AddAuthentication`方法只能呼叫一次時新增多個驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="fde12-139">The `AddAuthentication` method should only be called once when adding multiple authentication providers.</span></span> <span data-ttu-id="fde12-140">後續呼叫可能會覆寫任何先前設定的[AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions)屬性。</span><span class="sxs-lookup"><span data-stu-id="fde12-140">Subsequent calls to it have the potential of overriding any previously configured [AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) properties.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fde12-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fde12-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fde12-142">新增 Facebook 中的介軟體中`Configure`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="fde12-142">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="fde12-143">請參閱[FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) API 參考，如需有關支援 Facebook 驗證的組態選項。</span><span class="sxs-lookup"><span data-stu-id="fde12-143">See the [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="fde12-144">組態選項可用來：</span><span class="sxs-lookup"><span data-stu-id="fde12-144">Configuration options can be used to:</span></span>

* <span data-ttu-id="fde12-145">要求的使用者不同的資訊。</span><span class="sxs-lookup"><span data-stu-id="fde12-145">Request different information about the user.</span></span>
* <span data-ttu-id="fde12-146">加入自訂登入體驗的查詢字串引數。</span><span class="sxs-lookup"><span data-stu-id="fde12-146">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="fde12-147">使用 Facebook 登入</span><span class="sxs-lookup"><span data-stu-id="fde12-147">Sign in with Facebook</span></span>

<span data-ttu-id="fde12-148">執行您的應用程式，然後按一下**登入**。</span><span class="sxs-lookup"><span data-stu-id="fde12-148">Run your application and click **Log in**.</span></span> <span data-ttu-id="fde12-149">您會看到一個選項來使用 Facebook 登入。</span><span class="sxs-lookup"><span data-stu-id="fde12-149">You see an option to sign in with Facebook.</span></span>

![Web 應用程式： 未驗證的使用者](index/_static/DoneFacebook.png)

<span data-ttu-id="fde12-151">當您按一下  **Facebook**，將您重新導向 Facebook 進行驗證：</span><span class="sxs-lookup"><span data-stu-id="fde12-151">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Facebook 驗證頁面](index/_static/FBLogin.png)

<span data-ttu-id="fde12-153">Facebook 驗證會要求預設公用設定檔和電子郵件地址：</span><span class="sxs-lookup"><span data-stu-id="fde12-153">Facebook authentication requests public profile and email address by default:</span></span>

![Facebook 驗證頁面](index/_static/FBLoginDone.png)

<span data-ttu-id="fde12-155">一旦您輸入您的 Facebook 認證將您重新導向回您的網站，您可以設定您的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="fde12-155">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="fde12-156">您現在會記錄在使用您的 Facebook 認證：</span><span class="sxs-lookup"><span data-stu-id="fde12-156">You are now logged in using your Facebook credentials:</span></span>

![Web 應用程式： 已驗證的使用者](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="fde12-158">疑難排解</span><span class="sxs-lookup"><span data-stu-id="fde12-158">Troubleshooting</span></span>

* <span data-ttu-id="fde12-159">**ASP.NET Core 2.x 僅：**如果身分識別未設定藉由呼叫`services.AddIdentity`中`ConfigureServices`，嘗試驗證將會導致*ArgumentException： 必須提供 'SignInScheme' 選項*。</span><span class="sxs-lookup"><span data-stu-id="fde12-159">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="fde12-160">在本教學課程中使用的專案範本可確保，這已完成。</span><span class="sxs-lookup"><span data-stu-id="fde12-160">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="fde12-161">如果尚未套用初始移轉建立站台資料庫，您會收到*處理要求時，資料庫作業失敗*錯誤。</span><span class="sxs-lookup"><span data-stu-id="fde12-161">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="fde12-162">點選**套用移轉**來建立資料庫，並重新整理 以忽略錯誤繼續執行。</span><span class="sxs-lookup"><span data-stu-id="fde12-162">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fde12-163">後續步驟</span><span class="sxs-lookup"><span data-stu-id="fde12-163">Next steps</span></span>

* <span data-ttu-id="fde12-164">這篇文章會示範您可以向 Facebook。</span><span class="sxs-lookup"><span data-stu-id="fde12-164">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="fde12-165">您可以依照類似的方法，來向其他提供者列在[上一頁](index.md)。</span><span class="sxs-lookup"><span data-stu-id="fde12-165">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="fde12-166">一旦您將您的網站發行至 Azure web 應用程式時，您應該重設`AppSecret`Facebook 開發人員入口網站中。</span><span class="sxs-lookup"><span data-stu-id="fde12-166">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="fde12-167">設定`Authentication:Facebook:AppId`和`Authentication:Facebook:AppSecret`為在 Azure 入口網站應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="fde12-167">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="fde12-168">設定系統設定來讀取環境變數中的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="fde12-168">The configuration system is set up to read keys from environment variables.</span></span>