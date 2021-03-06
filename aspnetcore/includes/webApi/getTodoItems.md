::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

上述程式碼中定義不含方法的 API 控制器類別。 在接下來的幾節中，我們將新增方法藉以實作 API。
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

上述程式碼中定義不含方法的 API 控制器類別。 在接下來的幾節中，我們將新增方法藉以實作 API。 類別將以 `[ApiController]` 屬性進行標註來啟用一些便捷的功能。 如需屬性所啟用功能的資訊，請參閱[以 ApiControllerAttribute 標註類別](xref:web-api/index#annotate-class-with-apicontrollerattribute)。
::: moniker-end

控制器的建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將資料庫內容 (`TodoContext`) 插入至控制器中。 控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。 建構函式會將項目新增至記憶體內部資料庫 (如果項目不存在的話)。

## <a name="get-to-do-items"></a>取得待辦事項

若要取得待辦事項，請在 `TodoController` 類別中新增下列方法：

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end

這些方法會實作兩個 GET 方法：

* `GET /api/todo`
* `GET /api/todo/{id}`

以下是 `GetAll` 方法的範例 HTTP 回應：

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

稍後在本教學課程中，我們將示範如何使用 [Postman](https://www.getpostman.com/) 或 [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) 來檢視 HTTP 回應。

### <a name="routing-and-url-paths"></a>傳送和 URL 路徑

`[HttpGet]` 屬性代表回應 HTTP GET 要求的方法。 每個方法的 URL 路徑的建構方式如下：

* 在控制器的 `Route` 屬性中採用範本字串：

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end

* 以控制器的名稱取代 `[controller]`，也就是將控制器類別名稱減去 "Controller" 字尾。 在此範例中，控制器類別名稱是 **Todo**Controller，而根名稱是 "todo"。 ASP.NET Core [路由](xref:mvc/controllers/routing)不區分大小寫。
* 如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("/products")]`)，請將其附加到路徑。 此範例不使用範本。 如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。

在下列 `GetById` 方法中，`"{id}"` 是待辦事項唯一識別碼的預留位置變數。 在叫用 `GetById` 時，它會將 URL 中的 `"{id}"` 值指派給方法的 `id` 參數。

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

`Name = "GetTodo"` 會建立具名路由。 具名路由：

* 讓應用程式以該路由名稱建立 HTTP 連結。
* 本教學課程稍後會說明。

### <a name="return-values"></a>傳回值

`GetAll` 方法會傳回 `TodoItem` 物件的集合。 MVC 會自動將物件序列化為 [JSON](https://www.json.org/)，並將 JSON 寫入至回應訊息的本文。 這個方法的回應碼為 200，假設沒有任何未處理的例外狀況。 未處理的例外狀況會轉譯成 5xx 錯誤。

::: moniker range="<= aspnetcore-2.0"
反之，`GetById` 方法會傳回更普通的 [IActionResult 類型](xref:web-api/action-return-types#iactionresult-type)，其代表各種不同的傳回型別。 `GetById` 有兩種不同的傳回型別：

* 如果沒有項目符合所要求的識別碼，方法會傳回 404 錯誤。 傳回 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 會傳回 HTTP 404 回應。
* 否則，方法會傳回 200 與 JSON 回應本文。 傳回 [OK](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) 會導致 HTTP 200 回應。
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
反之，`GetById` 方法會傳回 [ActionResult\<T> 類型](xref:web-api/action-return-types#actionresultt-type)，其代表各種不同的傳回型別。 `GetById` 有兩種不同的傳回型別：

* 如果沒有項目符合所要求的識別碼，方法會傳回 404 錯誤。 傳回 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 會傳回 HTTP 404 回應。
* 否則，方法會傳回 200 與 JSON 回應本文。 傳回 `item` 會導致 HTTP 200 回應。
::: moniker-end