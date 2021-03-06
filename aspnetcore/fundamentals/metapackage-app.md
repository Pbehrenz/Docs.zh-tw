---
title: ASP.NET Core 2.1 和更新版本的 Microsoft.AspNetCore.App 中繼套件
author: Rick-Anderson
description: Microsoft.AspNetCore.App 中繼套件包含所有支援的 ASP.NET Core 和 Entity Framework Core 套件。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage-app
ms.openlocfilehash: 4840d0a7536b1e9d8da835690b285ac2074967f5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277468"
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a>ASP.NET Core 2.1 的 Microsoft.AspNetCore.App 中繼套件

這項功能需要以 .NET Core 2.1 和更新版本為目標的 ASP.NET Core 2.1 和更新版本。

ASP.NET Core 的 [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [中繼套件](/dotnet/core/packages#metapackages)：

* 不包含協力廠商相依性，[Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/)、[Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/) 和 [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/) 除外。 這些協力廠商相依性是確保主要架構功能運作的必要項。
* 包含所有由 ASP.NET Core 小組支援的套件，除了含有協力廠商相依性的套件以外 (非先前所述)。
* 包含所有由 Entity Framework Core 小組支援的套件，除了含有協力廠商相依性的套件以外 (非先前所述)。

`Microsoft.AspNetCore.App` 套件包含 ASP.NET Core 2.1 和更新版本以及 Entity Framework Core 2.1 和更新版本的所有功能。 以 ASP.NET Core 2.1 和更新版本為目標的預設專案範本會使用此套件。 建議以 ASP.NET Core 2.1 和更新版本以及 Entity Framework Core 2.1 和更新版本為目標的應用程式使用 `Microsoft.AspNetCore.App` 套件。

`Microsoft.AspNetCore.App` 中繼套件的版本號碼代表 ASP.NET Core 版本和 Entity Framework Core 版本。

使用 `Microsoft.AspNetCore.App` 中繼套件提供版本限制來保護您的應用程式：

* 如果內含套件具有 `Microsoft.AspNetCore.App` 中套件的可轉移 (非直接) 相依性，而且這些版本號碼不同，則 NuGet 會產生錯誤。
* 其他新增至您應用程式的套件無法變更 `Microsoft.AspNetCore.App` 中包含的套件版本。
* 版本一致性有助於確保可靠的體驗。 `Microsoft.AspNetCore.App` 的設計目的是為了防止相關位元之未經測試的版本組合在同一個應用程式中搭配使用。

使用 `Microsoft.AspNetCore.App` 中繼套件的應用程式會自動利用 ASP.NET Core 共用架構。 當您使用 `Microsoft.AspNetCore.App` 中繼套件時，**不會**使用應用程式部署所參考之 ASP.NET Core NuGet 套件的任何資產 &mdash; ASP.NET Core 共用架構包含這些資產。 共用架構中的資產會先行編譯，以改善應用程式啟動時間。 如需詳細資訊，請參閱 [.NET Core 發佈封裝](/dotnet/core/build/distribution-packaging)中的「共用架構」。

下列 *.csproj* 專案檔參考 ASP.NET Core 的 `Microsoft.AspNetCore.App` 中繼套件：

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>

```

上述標記代表一般 ASP.NET Core 2.1 和更新版本的範本。 它不會指定 `Microsoft.AspNetCore.App` 套件參考的版本號碼。 未指定版本時，SDK 會指定[隱含](https://github.com/dotnet/core/blob/master/release-notes/1.0/sdk/1.0-rc3-implicit-package-refs.md)版本，也就是 `Microsoft.NET.Sdk.Web`。 建議依賴 SDK 指定的隱含版本，而不要明確設定套件參考的版本號碼。 您可以在 [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/aspnet/Docs/issues/6430) (Microsoft.AspNetCore.App 隱含版本討論區) 留下 GitHub 意見。

可攜式應用程式的隱含版本會設定為 `major.minor.0`。 共用架構向前復原機制會在已安裝共用架構中的最新相容版本上執行應用程式。 為了保證開發、測試和生產均使用相同版本，請務必在所有環境中安裝相同版本的共用架構。 針對獨立應用程式，隱含版本號碼會設定為已安裝 SDK 隨附之共用架構的 `major.minor.patch`。

指定 `Microsoft.AspNetCore.App` 參考的版本號碼**不**保證會選擇該版本的共用架構。 例如，假設指定版本 "2.1.1"，但安裝 "2.1.3"。 在此情況下，應用程式會使用 "2.1.3"。 您可以停用向前復原 (修補及/或次要)，但不建議這樣做。 如需 dotnet 主機向前復原及如何設定其行為的詳細資訊，請參閱 [dotnet 主機向前復原](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md)。

`Microsoft.AspNetCore.App` [中繼套件](/dotnet/core/packages#metapackages)不是從 NuGet 更新的傳統套件。 類似於 `Microsoft.NETCore.App`，`Microsoft.AspNetCore.App` 代表共用執行階段，其具有在 NuGet 外部處理的特殊版本控制語意。 如需詳細資訊，請參閱[套件、中繼套件和架構](/dotnet/core/packages)。

`<Project Sdk` 必須設定為 `Microsoft.NET.Sdk.Web`，才能使用隱含版本 `Microsoft.AspNetCore.App`。  使用 `<Project Sdk="Microsoft.NET.Sdk">` 時，會產生下列警告：

*警告 NU1604: 專案相依性 Microsoft.AspNetCore.App 未包含內含的下限。請在相依性版本包含下限，以確保還原結果一致。*

*警告 NU1602: [專案名稱] 未針對相依性 Microsoft.AspNetCore.App 提供內含的下限。已解決近似最符合項目 Microsoft.AspNetCore.App 2.1.0。*

如果您的應用程式先前使用 `Microsoft.AspNetCore.All`，請參閱[從 Microsoft.AspNetCore.All 移轉至 Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate)。
