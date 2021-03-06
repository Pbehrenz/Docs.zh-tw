---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 第 2 部分： 資料存取層 |Microsoft Docs
author: JoeStagner
description: 本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 2 部分涵蓋新增資料存取層。
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: c1d61681952cbf1f0587f0e42da21f8436d79ebb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812028"
---
<a name="part-2-data-access-layer"></a>第 2 部分： 資料存取層
====================
藉由[Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks 示範建立功能強大、 可擴充的應用程式，適用於.NET 平台是如何富含簡單。 它會展示如何在 ASP.NET 4 中使用最棒的新功能，建置線上商店，包括購物、 簽出，以及系統管理。
> 
> 本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 2 部分涵蓋新增資料存取層。


## <a id="_Toc260221668"></a>  新增資料存取層

我們的電子商務應用程式將取決於兩個資料庫。

客戶資訊中，我們將使用標準 ASP.NET 成員資格資料庫。 我們的購物車 」 和 「 產品類別目錄中，我們將實作的 SQL Express 資料庫執行，如下所示。

![](tailspin-spyworks-part-2/_static/image1.jpg)

在應用程式的應用程式中建立資料庫 (Commerce.mdf)\_我們可以繼續建立我們使用.NET Entity Framework 的資料存取層的 [Data] 資料夾。

我們將建立名為 「 資料\_存取 」 和它們在該資料夾上按一下滑鼠右鍵，然後選取 新增新的項目 」。

在 「 已安裝的範本 」 項目，然後選取 「 ADO.NET 實體資料模型 」 輸入 EDM\_Commerce.edmx 做為名稱，然後按一下 [新增] 按鈕。

![](tailspin-spyworks-part-2/_static/image2.jpg)

選擇 [從資料庫產生]。

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

儲存並建置。

現在，我們已經準備好加入我們的第一項功能 – 產品類別目錄 功能表。

> [!div class="step-by-step"]
> [上一頁](tailspin-spyworks-part-1.md)
> [下一頁](tailspin-spyworks-part-3.md)
