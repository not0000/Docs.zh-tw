---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 教學課程： 開始使用 SignalR 2 和 MVC 5 |Microsoft 文件
author: pfletcher
description: 本教學課程會示範如何使用 ASP.NET SignalR 2 建立即時聊天的應用程式。 您將 MVC 5 應用程式中加入 SignalR 和建立交談檢視...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: ca0471114da7363c5df9d459308708e7ab4f8b84
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a>教學課程： 開始使用 SignalR 2 和 MVC 5
====================
由[Patrick Fletcher](https://github.com/pfletcher)， [Tim Teebken](https://github.com/timlt)

[下載完成的專案](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> 本教學課程會示範如何使用 ASP.NET SignalR 2 建立即時聊天的應用程式。 您將 MVC 5 應用程式中加入 SignalR 和建立交談檢視來傳送，並顯示訊息。 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - MVC 5
> - SignalR 第 2 版
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>使用 Visual Studio 2012 進行這個教學課程
> 
> 
> 透過本教學課程中使用 Visual Studio 2012，請執行下列作業：
> 
> - 更新您[封裝管理員](http://docs.nuget.org/docs/start-here/installing-nuget)的最新版本。
> - 安裝[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)。
> - 在 Web Platform Installer 中搜尋及安裝**ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012**。 這會安裝 Visual Studio 範本 SignalR 的類別，例如**中樞**。
> - 某些範本 (例如**OWIN 啟動類別**) 將無法使用; 這些項目，請改用類別檔案。
> 
> 
> ## <a name="tutorial-versions"></a>教學課程版本
> 
> 如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。
> 
> ## <a name="questions-and-comments"></a>問題和註解
> 
> 請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


## <a name="overview"></a>總覽

本教學課程為您介紹即時 web 應用程式開發 ASP.NET SignalR 2 和 ASP.NET MVC 5。 教學課程使用相同的交談應用程式程式碼做為[SignalR 快速入門教學課程](tutorial-getting-started-with-signalr.md)，但是會顯示如何將它加入至 MVC 5 應用程式。

本主題中，您將學習下列 SignalR 開發工作：

- 將 SignalR 程式庫加入至 MVC 5 應用程式。
- 建立中樞和 OWIN 啟動類別內容推播至用戶端。
- 使用 SignalR jQuery 程式庫，在網頁中的傳送訊息，並顯示從中樞的更新。

下列螢幕擷取畫面顯示瀏覽器中執行之已完成的交談應用程式。

![交談的執行個體](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

章節：

- [設定專案](#setup)
- [執行範例](#run)
- [檢查程式碼](#code)
- [後續步驟](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>設定專案

必要條件：

- Visual Studio 2013. 如果您沒有 Visual Studio，請參閱[ASP.NET 下載](https://www.asp.net/downloads)取得免費 Visual Studio 2013 Express 開發的工具。

本節說明如何建立 ASP.NET MVC 5 應用程式、 新增 SignalR 程式庫和建立交談應用程式。

1. 在 Visual Studio 中建立的 C# ASP.NET 應用程式目標為.NET Framework 4.5、 SignalRChat，將其命名並按一下 [確定]。

    ![建立 web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. 在`New ASP.NET Project` 對話方塊中，然後選取**MVC**，然後按一下**變更驗證**。

    ![建立 web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. 選取**非驗證**中**變更驗證**對話方塊，然後按一下**確定**。

    ![選取 沒有驗證](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > 如果您的應用程式中，選取不同的驗證提供者`Startup.cs`類別將為您建立; 您不需要建立您自己`Startup.cs`類別中的步驟 10 以下。
4. 按一下**確定**中**新增 ASP.NET 專案**對話方塊。
5. 開啟**工具 |程式庫套件管理員 |Package Manager Console** ，然後執行下列命令。 這個步驟可將一組指令碼檔案和啟用 SignalR 功能的組件參考加入至專案。

    `install-package Microsoft.AspNet.SignalR`
6. 在**方案總管 中**，展開 指令碼 資料夾。 請注意 SignalR 的指令碼程式庫已加入至專案。

    ![指令碼 資料夾](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. 在**方案總管 中**，以滑鼠右鍵按一下專案，選取**新增 |新的資料夾**，並加入新的資料夾，名為**集線器**。
8. 以滑鼠右鍵按一下**集線器**資料夾中，按一下 **新增 |新項目**，選取**Visual C# |Web |SignalR**節點**已安裝**窗格中，選取**SignalR 中樞類別 (v2)** 從中間窗格中，並建立名為新的中樞**ChatHub.cs**。 您將使用此類別將訊息傳送至所有用戶端的 SignalR 伺服器集線器。

    ![建立新的中樞](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. 中的程式碼取代**ChatHub**為下列程式碼的類別。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. 建立新的類別稱為 Startup.cs。 下列變更檔案的內容。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. 編輯`HomeController`類別中找到**Controllers/HomeController.cs**並將下列方法加入至類別。 這個方法會傳回**聊天**您將在稍後步驟中建立的檢視。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. 以滑鼠右鍵按一下**Views/Home**資料夾，然後選取**新增.|檢視**。
13. 在**加入檢視**對話方塊中，新的檢視**聊天**。

    ![新增檢視](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. 取代內容**Chat.cshtml**為下列程式碼。

    > [!IMPORTANT]
    > 當您將 SignalR 和其他指令碼程式庫加入您的 Visual Studio 專案時，封裝管理員可能會安裝的版本比本主題所顯示的版本還新的 SignalR 指令碼檔案。 請確定您的程式碼中的指令碼參考符合安裝在您的專案中的指令碼程式庫的版本。

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. **儲存所有**專案。

<a id="run"></a>

## <a name="run-the-sample"></a>執行範例

1. 按 F5 以偵錯模式執行專案。
2. 在瀏覽器位址列中，附加 **/首頁/聊天**專案的預設網頁的 url。 聊天室頁面載入瀏覽器執行個體，並提示輸入使用者名稱。

    ![輸入使用者名稱](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. 輸入使用者名稱。
4. 從瀏覽器位址列複製 URL，並使用它來開啟兩個的多個瀏覽器執行個體。 在每個瀏覽器執行個體中，輸入唯一的使用者名稱。
5. 每個瀏覽器執行個體中，加入註解，然後按一下**傳送**。 這些註解應該顯示在所有瀏覽器執行個體。

    > [!NOTE]
    > 這個簡單的聊天應用程式不會維持在伺服器上的討論內容。 集線器會廣播到所有目前使用者的註解。 稍後加入交談的使用者會看到訊息的時間加入它們加入。
6. 下列螢幕擷取畫面顯示瀏覽器中執行之交談應用程式。

    ![對談瀏覽器](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. 在**方案總管 中**，檢查**指令碼文件**節點執行的應用程式。 如果您使用 Internet Explorer 瀏覽器，此節點會顯示在偵錯模式。 沒有名為指令碼檔案**集線器**SignalR library 動態產生在執行階段。 這個檔案會管理 jQuery 指令碼和伺服器端程式碼之間的通訊。 如果您使用非 Internet Explorer 的瀏覽器，您也可以存取動態**集線器**瀏覽至它直接管理，例如 http://mywebsite/signalr/hubs 檔案。

<a id="code"></a>

## <a name="examine-the-code"></a>檢查程式碼

SignalR 交談應用程式將示範兩個基本的 SignalR 開發工作： 協調主要伺服器上的物件，以建立中樞和使用 SignalR jQuery 程式庫來傳送和接收訊息。

### <a name="signalr-hubs"></a>SignalR 中樞

下列程式碼範例**ChatHub**類別衍生自**Microsoft.AspNet.SignalR.Hub**類別。 衍生自**中樞**類別是有用的方式來建置 SignalR 應用程式。 您可以建立中樞類別上的公用方法，然後呼叫在網頁中的指令碼的方式來存取這些方法。

在對談程式碼中，用戶端呼叫**ChatHub.Send**傳送新訊息的方法。 中樞接著將訊息傳送至所有用戶端藉由呼叫**Clients.All.addNewMessageToPage**。

**傳送**方法將示範中樞的幾個概念：

- 中樞上宣告的公用方法，可讓用戶端呼叫。
- 使用**Microsoft.AspNet.SignalR.Hub.Clients**屬性來存取所有用戶端連線到此中樞。
- 在用戶端上呼叫的函式 (例如`addNewMessageToPage`函式) 來更新用戶端。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR 和 jQuery

**Chat.cshtml**檢視檔案中的程式碼範例示範如何使用 SignalR jQuery 程式庫與 SignalR 中樞通訊。 在程式碼中的重要工作建立的自動產生 proxy 集線器，宣告的函式推入內容給用戶端，可以呼叫的伺服器和啟動連線將訊息傳送至中樞的參考。

下列程式碼會宣告中樞 proxy 的參考。

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> 在 JavaScript 中在 camel 命名法的情況下會伺服器類別和其成員的參考。 程式碼範例會參考 C# **ChatHub**類別在 JavaScript 中為**chatHub**。 如果您想要參考`ChatHub`類別在 jQuery 與傳統 Pascal 大小寫在 C# 中一樣編輯 ChatHub.cs 類別檔案。 新增`using`陳述式來參考`Microsoft.AspNet.SignalR.Hubs`命名空間。 然後加入`HubName`屬性`ChatHub`類別，例如`[HubName("ChatHub")]`。 最後，更新您 jQuery 參考`ChatHub`類別。


下列程式碼會示範如何在指令碼中建立的回呼函式。 在伺服器上的中樞類別會呼叫此函式可將內容更新推送到每個用戶端。 選擇性呼叫`htmlEncode`函式便會顯示為 HTML 的方式之後，顯示在頁面中，以避免指令碼資料隱碼的方式編碼的訊息內容。

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

下列程式碼會示範如何開啟與中樞的連線。 此程式碼啟動連線，並接著將該函式來處理 click 事件上**傳送**聊天室網頁中的按鈕。

> [!NOTE]
> 這個方法可確保此事件處理常式執行之前，已建立連線。


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>後續步驟

您已學習 SignalR 是建置即時 web 應用程式的架構。 您也學到幾個 SignalR 開發工作： 如何將 ASP.NET 應用程式中的 SignalR、 如何建立中樞類別，以及如何傳送和接收來自中樞的訊息。

如需如何部署範例 SignalR 應用程式至 Azure 的逐步解說，請參閱[與 Azure App Service 中的 Web 應用程式的使用 SignalR](../deployment/using-signalr-with-azure-web-sites.md)。 如需如何部署 Visual Studio web 專案至 Windows Azure 網站的詳細資訊，請參閱[Azure App Service 中建立 ASP.NET web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)。

深入了解更多進階的 SignalR 發展概念，請造訪下列網站 SignalR 原始碼和資源：

- [SignalR 專案](http://signalr.net)
- [SignalR Github 和範例](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
