---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: 建立自訂的路由條件約束 (VB) |Microsoft Docs
author: StephenWalther
description: Stephen Walther 會示範如何建立自訂路由條件約束。 我們會實作簡單的自訂條件約束可以防止路由比對 w...
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 72389d11467cbf7baea4cc9452266edb8ab81125
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840464"
---
<a name="creating-a-custom-route-constraint-vb"></a>建立自訂的路由條件約束 (VB)
====================
藉由[Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther 會示範如何建立自訂路由條件約束。 我們會實作簡單的自訂條件約束，可防止瀏覽器要求從遠端電腦時要比對路由。


本教學課程的目的在於示範如何建立自訂路由條件約束。 自訂路由條件約束可讓您防止某些自訂的條件會比對，除非符合路由。

在本教學課程中，我們會建立 Localhost 的路由條件約束。 Localhost 路由條件約束只會比對從本機電腦所提出的要求。 從遠端在網際網路上的要求不符。

您可以實作 IRouteConstraint 介面，以實作自訂路由條件約束。 這是非常簡單的介面描述單一方法：

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

方法會傳回布林值。 如果傳回 False 時，含有條件約束相關聯的路由不符合瀏覽器要求。

列表 1 中包含的 Localhost 條件約束。

**列表 1-LocalhostConstraint.vb**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

列表 1 中的條件約束會利用 HttpRequest 類別所公開的 IsLocal 屬性。 當要求的 IP 位址是任一 127.0.0.1 或要求的 IP 是伺服器的 IP 位址相同，則這個屬性會傳回 true。

您使用自訂的條件約束內 Global.asax 檔案中定義的路由。 列表 2 中的 Global.asax 檔會使用 Localhost 條件約束，防止任何人要求系統管理員 頁面，除非它們從本機伺服器提出要求。 例如，從遠端伺服器時，將會失敗 /Admin/DeleteAll 的要求。

**列表 2-Global.asax**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

管理路由的定義所用的 Localhost 條件約束。 此路由不會比對遠端瀏覽器要求。 不過，要注意的是，其他 Global.asax 中所定義的路由可能會符合相同的要求。 請務必了解條件約束可防止特定路由相符的要求，而且並非所有的路由定義於 Global.asax 檔案。

請注意，預設路由具有已標記為註解列表 2 中的 Global.asax 檔中。 如果您包含預設路由時，預設路由會比對系統管理員控制站的要求。 在此情況下，遠端使用者可能仍然叫用動作的系統管理控制站即使其要求不符合管理路由。

> [!div class="step-by-step"]
> [上一步](creating-a-route-constraint-vb.md)
