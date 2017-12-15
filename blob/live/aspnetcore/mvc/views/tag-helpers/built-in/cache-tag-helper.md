---
title: "快取中 ASP.NET Core MVC 標記協助程式"
author: pkellner
description: "示範如何使用快取標記協助程式"
keywords: "ASP.NET Core,標記協助程式"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a012
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 74080d089dc7a72da96f9f18d613cb313cd930db
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2017
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="1785e-104">快取中 ASP.NET Core MVC 標記協助程式</span><span class="sxs-lookup"><span data-stu-id="1785e-104">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="1785e-105">由 [Peter Kellner](http://peterkellner.net) 提供</span><span class="sxs-lookup"><span data-stu-id="1785e-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="1785e-106">快取標記協助程式提供了可大幅改善其內部的 ASP.NET Core 快取提供者的內容快取的 ASP.NET Core 應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="1785e-106">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="1785e-107">Razor 檢視引擎設定的預設`expires-after`為 20 分鐘。</span><span class="sxs-lookup"><span data-stu-id="1785e-107">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="1785e-108">下列的 Razor 標記會快取的日期/時間：</span><span class="sxs-lookup"><span data-stu-id="1785e-108">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="1785e-109">包含頁面的第一個要求`CacheTagHelper`會顯示目前的日期/時間。</span><span class="sxs-lookup"><span data-stu-id="1785e-109">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="1785e-110">其他要求會顯示快取的值，直到快取到期 （預設值 20 分鐘），或者由記憶體不足的壓力移出記憶體。</span><span class="sxs-lookup"><span data-stu-id="1785e-110">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="1785e-111">您可以設定快取期間具有下列屬性：</span><span class="sxs-lookup"><span data-stu-id="1785e-111">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="1785e-112">快取標記協助程式屬性</span><span class="sxs-lookup"><span data-stu-id="1785e-112">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="1785e-113">enabled</span><span class="sxs-lookup"><span data-stu-id="1785e-113">enabled</span></span>    


| <span data-ttu-id="1785e-114">屬性類型</span><span class="sxs-lookup"><span data-stu-id="1785e-114">Attribute Type</span></span>    | <span data-ttu-id="1785e-115">有效值</span><span class="sxs-lookup"><span data-stu-id="1785e-115">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="1785e-116">boolean</span><span class="sxs-lookup"><span data-stu-id="1785e-116">boolean</span></span>           | <span data-ttu-id="1785e-117">"true"（預設值）</span><span class="sxs-lookup"><span data-stu-id="1785e-117">"true" (default)</span></span>  |
|                   | <span data-ttu-id="1785e-118">"false"</span><span class="sxs-lookup"><span data-stu-id="1785e-118">"false"</span></span>   |


<span data-ttu-id="1785e-119">決定是否要快取內容快取標記協助程式 」 所括住。</span><span class="sxs-lookup"><span data-stu-id="1785e-119">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="1785e-120">預設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="1785e-120">The default is `true`.</span></span>  <span data-ttu-id="1785e-121">如果設定為`false`此快取標記協助程式不會影響快取上轉譯的輸出。</span><span class="sxs-lookup"><span data-stu-id="1785e-121">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="1785e-122">範例：</span><span class="sxs-lookup"><span data-stu-id="1785e-122">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="1785e-123">到期日</span><span class="sxs-lookup"><span data-stu-id="1785e-123">expires-on</span></span> 

| <span data-ttu-id="1785e-124">屬性類型</span><span class="sxs-lookup"><span data-stu-id="1785e-124">Attribute Type</span></span>    | <span data-ttu-id="1785e-125">範例值</span><span class="sxs-lookup"><span data-stu-id="1785e-125">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="1785e-126">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="1785e-126">DateTimeOffset</span></span>    | <span data-ttu-id="1785e-127">「@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="1785e-127">"@new DateTime(2025,1,29,17,02,0)"</span></span>    |


<span data-ttu-id="1785e-128">設定絕對到期日。</span><span class="sxs-lookup"><span data-stu-id="1785e-128">Sets an absolute expiration date.</span></span> <span data-ttu-id="1785e-129">下列範例將快取的快取標記協助程式內容，直到 2025 年 1 月 29，下午 5:02。</span><span class="sxs-lookup"><span data-stu-id="1785e-129">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="1785e-130">範例：</span><span class="sxs-lookup"><span data-stu-id="1785e-130">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="1785e-131">過期之後</span><span class="sxs-lookup"><span data-stu-id="1785e-131">expires-after</span></span>

| <span data-ttu-id="1785e-132">屬性類型</span><span class="sxs-lookup"><span data-stu-id="1785e-132">Attribute Type</span></span>    | <span data-ttu-id="1785e-133">範例值</span><span class="sxs-lookup"><span data-stu-id="1785e-133">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="1785e-134">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="1785e-134">TimeSpan</span></span>    | <span data-ttu-id="1785e-135">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="1785e-135">"@TimeSpan.FromSeconds(120)"</span></span>    |


<span data-ttu-id="1785e-136">從快取內容的第一個要求時間設定時間的長度。</span><span class="sxs-lookup"><span data-stu-id="1785e-136">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="1785e-137">範例：</span><span class="sxs-lookup"><span data-stu-id="1785e-137">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="1785e-138">滑動過期</span><span class="sxs-lookup"><span data-stu-id="1785e-138">expires-sliding</span></span>

| <span data-ttu-id="1785e-139">屬性類型</span><span class="sxs-lookup"><span data-stu-id="1785e-139">Attribute Type</span></span>    | <span data-ttu-id="1785e-140">範例值</span><span class="sxs-lookup"><span data-stu-id="1785e-140">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="1785e-141">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="1785e-141">TimeSpan</span></span>    | <span data-ttu-id="1785e-142">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="1785e-142">"@TimeSpan.FromSeconds(60)"</span></span>     |


<span data-ttu-id="1785e-143">設定應該收回快取項目，如果它尚未被存取的時間。</span><span class="sxs-lookup"><span data-stu-id="1785e-143">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="1785e-144">範例：</span><span class="sxs-lookup"><span data-stu-id="1785e-144">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="1785e-145">改變-依標頭</span><span class="sxs-lookup"><span data-stu-id="1785e-145">vary-by-header</span></span>

| <span data-ttu-id="1785e-146">屬性類型</span><span class="sxs-lookup"><span data-stu-id="1785e-146">Attribute Type</span></span>    | <span data-ttu-id="1785e-147">範例值</span><span class="sxs-lookup"><span data-stu-id="1785e-147">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="1785e-148">字串</span><span class="sxs-lookup"><span data-stu-id="1785e-148">String</span></span>            | <span data-ttu-id="1785e-149">「 使用者代理程式 」</span><span class="sxs-lookup"><span data-stu-id="1785e-149">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="1785e-150">「 使用者代理程式，內容編碼方式 」</span><span class="sxs-lookup"><span data-stu-id="1785e-150">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="1785e-151">接受單一標頭值或其變更時觸發快取重新整理的標頭值的逗號分隔清單。</span><span class="sxs-lookup"><span data-stu-id="1785e-151">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="1785e-152">下列範例會監視的標頭值`User-Agent`。</span><span class="sxs-lookup"><span data-stu-id="1785e-152">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="1785e-153">此範例會將快取內容的每個不同`User-Agent`呈現給 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="1785e-153">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="1785e-154">範例：</span><span class="sxs-lookup"><span data-stu-id="1785e-154">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="1785e-155">改變由-查詢</span><span class="sxs-lookup"><span data-stu-id="1785e-155">vary-by-query</span></span>

| <span data-ttu-id="1785e-156">屬性類型</span><span class="sxs-lookup"><span data-stu-id="1785e-156">Attribute Type</span></span>    | <span data-ttu-id="1785e-157">範例值</span><span class="sxs-lookup"><span data-stu-id="1785e-157">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="1785e-158">字串</span><span class="sxs-lookup"><span data-stu-id="1785e-158">String</span></span>            | <span data-ttu-id="1785e-159">「 讓 」</span><span class="sxs-lookup"><span data-stu-id="1785e-159">"Make"</span></span>                |
|                   | <span data-ttu-id="1785e-160">「 請模型 」</span><span class="sxs-lookup"><span data-stu-id="1785e-160">"Make,Model"</span></span> |

<span data-ttu-id="1785e-161">接受單一標頭值或以逗號分隔清單的標頭值變更時觸發快取重新整理的標頭值。</span><span class="sxs-lookup"><span data-stu-id="1785e-161">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="1785e-162">下列範例會查看值`Make`和`Model`。</span><span class="sxs-lookup"><span data-stu-id="1785e-162">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="1785e-163">範例：</span><span class="sxs-lookup"><span data-stu-id="1785e-163">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="1785e-164">改變由-路由</span><span class="sxs-lookup"><span data-stu-id="1785e-164">vary-by-route</span></span>

| <span data-ttu-id="1785e-165">屬性類型</span><span class="sxs-lookup"><span data-stu-id="1785e-165">Attribute Type</span></span>    | <span data-ttu-id="1785e-166">範例值</span><span class="sxs-lookup"><span data-stu-id="1785e-166">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="1785e-167">字串</span><span class="sxs-lookup"><span data-stu-id="1785e-167">String</span></span>            | <span data-ttu-id="1785e-168">「 讓 」</span><span class="sxs-lookup"><span data-stu-id="1785e-168">"Make"</span></span>                |
|                   | <span data-ttu-id="1785e-169">「 請模型 」</span><span class="sxs-lookup"><span data-stu-id="1785e-169">"Make,Model"</span></span> |

<span data-ttu-id="1785e-170">接受單一標頭值或路由資料的參數值變更時觸發快取重新整理的標頭值的逗號分隔清單。</span><span class="sxs-lookup"><span data-stu-id="1785e-170">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="1785e-171">範例：</span><span class="sxs-lookup"><span data-stu-id="1785e-171">Example:</span></span>

<span data-ttu-id="1785e-172">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="1785e-172">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
<span data-ttu-id="1785e-173">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1785e-173">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="1785e-174">改變-由-cookie</span><span class="sxs-lookup"><span data-stu-id="1785e-174">vary-by-cookie</span></span>

| <span data-ttu-id="1785e-175">屬性類型</span><span class="sxs-lookup"><span data-stu-id="1785e-175">Attribute Type</span></span>    | <span data-ttu-id="1785e-176">範例值</span><span class="sxs-lookup"><span data-stu-id="1785e-176">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="1785e-177">字串</span><span class="sxs-lookup"><span data-stu-id="1785e-177">String</span></span>            | <span data-ttu-id="1785e-178">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="1785e-178">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="1785e-179">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="1785e-179">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="1785e-180">接受單一標頭值或標頭值 (s) 變更時觸發快取重新整理的標頭值的逗號分隔清單。</span><span class="sxs-lookup"><span data-stu-id="1785e-180">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="1785e-181">下列範例會查看與 ASP.NET 識別相關聯的 cookie。</span><span class="sxs-lookup"><span data-stu-id="1785e-181">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="1785e-182">當使用者通過驗證設定要求 cookie 觸發快取重新整理。</span><span class="sxs-lookup"><span data-stu-id="1785e-182">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="1785e-183">範例：</span><span class="sxs-lookup"><span data-stu-id="1785e-183">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="1785e-184">改變由-使用者</span><span class="sxs-lookup"><span data-stu-id="1785e-184">vary-by-user</span></span>

| <span data-ttu-id="1785e-185">屬性類型</span><span class="sxs-lookup"><span data-stu-id="1785e-185">Attribute Type</span></span>    | <span data-ttu-id="1785e-186">範例值</span><span class="sxs-lookup"><span data-stu-id="1785e-186">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="1785e-187">Boolean</span><span class="sxs-lookup"><span data-stu-id="1785e-187">Boolean</span></span>             | <span data-ttu-id="1785e-188">"true"</span><span class="sxs-lookup"><span data-stu-id="1785e-188">"true"</span></span>                  |
|                     | <span data-ttu-id="1785e-189">"false"（預設值）</span><span class="sxs-lookup"><span data-stu-id="1785e-189">"false" (default)</span></span> |

<span data-ttu-id="1785e-190">指定已登入使用者 （或內容主體） 變更時，應該重設快取。</span><span class="sxs-lookup"><span data-stu-id="1785e-190">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="1785e-191">目前的使用者就是所謂的要求內容主體，而且可以藉由參考檢視 Razor 檢視`@User.Identity.Name`。</span><span class="sxs-lookup"><span data-stu-id="1785e-191">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="1785e-192">下列範例會查看目前的登入的使用者。</span><span class="sxs-lookup"><span data-stu-id="1785e-192">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="1785e-193">範例：</span><span class="sxs-lookup"><span data-stu-id="1785e-193">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="1785e-194">使用這個屬性會維護透過在登入和登出循環的快取中的內容。</span><span class="sxs-lookup"><span data-stu-id="1785e-194">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="1785e-195">當使用`vary-by-user="true"`，登入和登出的動作會導致無效的快取驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="1785e-195">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="1785e-196">快取無效，因為登入，產生新的唯一的 cookie 值。</span><span class="sxs-lookup"><span data-stu-id="1785e-196">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="1785e-197">快取保留匿名狀態時沒有任何 cookie，或已過期。</span><span class="sxs-lookup"><span data-stu-id="1785e-197">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="1785e-198">這表示如果沒有使用者登入，仍會維護快取。</span><span class="sxs-lookup"><span data-stu-id="1785e-198">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="1785e-199">不同的</span><span class="sxs-lookup"><span data-stu-id="1785e-199">vary-by</span></span>

| <span data-ttu-id="1785e-200">屬性類型</span><span class="sxs-lookup"><span data-stu-id="1785e-200">Attribute Type</span></span>    | <span data-ttu-id="1785e-201">範例值</span><span class="sxs-lookup"><span data-stu-id="1785e-201">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="1785e-202">字串</span><span class="sxs-lookup"><span data-stu-id="1785e-202">String</span></span>             | <span data-ttu-id="1785e-203">"@Model"</span><span class="sxs-lookup"><span data-stu-id="1785e-203">"@Model"</span></span>                 |


<span data-ttu-id="1785e-204">可讓您自訂的哪些資料取得快取。</span><span class="sxs-lookup"><span data-stu-id="1785e-204">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="1785e-205">更新屬性的字串值的變更，快取標記協助程式的內容所參考的物件時。</span><span class="sxs-lookup"><span data-stu-id="1785e-205">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="1785e-206">通常模型值的字串串連會指派給這個屬性。</span><span class="sxs-lookup"><span data-stu-id="1785e-206">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="1785e-207">實際上，這表示，串連的值的任何更新的快取失效。</span><span class="sxs-lookup"><span data-stu-id="1785e-207">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="1785e-208">下列範例假設控制器方法轉譯檢視加總兩個路由參數的整數值`myParam1`和`myParam2`，並傳回，當做單一模型屬性。</span><span class="sxs-lookup"><span data-stu-id="1785e-208">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="1785e-209">當這個總和變更時，快取標記協助程式的內容轉譯，而且一次快取。</span><span class="sxs-lookup"><span data-stu-id="1785e-209">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="1785e-210">範例：</span><span class="sxs-lookup"><span data-stu-id="1785e-210">Example:</span></span>

<span data-ttu-id="1785e-211">動作：</span><span class="sxs-lookup"><span data-stu-id="1785e-211">Action:</span></span>

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

<span data-ttu-id="1785e-212">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1785e-212">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="1785e-213">priority</span><span class="sxs-lookup"><span data-stu-id="1785e-213">priority</span></span>

| <span data-ttu-id="1785e-214">屬性類型</span><span class="sxs-lookup"><span data-stu-id="1785e-214">Attribute Type</span></span>    | <span data-ttu-id="1785e-215">範例值</span><span class="sxs-lookup"><span data-stu-id="1785e-215">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="1785e-216">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="1785e-216">CacheItemPriority</span></span>  | <span data-ttu-id="1785e-217">「 高 」</span><span class="sxs-lookup"><span data-stu-id="1785e-217">"High"</span></span>                   |
|                    | <span data-ttu-id="1785e-218">「 低 」</span><span class="sxs-lookup"><span data-stu-id="1785e-218">"Low"</span></span> |
|                    | <span data-ttu-id="1785e-219">「 NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="1785e-219">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="1785e-220">"Normal"</span><span class="sxs-lookup"><span data-stu-id="1785e-220">"Normal"</span></span> |

<span data-ttu-id="1785e-221">提供了內建的快取提供者快取收回的指引。</span><span class="sxs-lookup"><span data-stu-id="1785e-221">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="1785e-222">網頁伺服器將會收回`Low`時更新快取項目第一次記憶體不足壓力下。</span><span class="sxs-lookup"><span data-stu-id="1785e-222">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="1785e-223">範例：</span><span class="sxs-lookup"><span data-stu-id="1785e-223">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="1785e-224">`priority`屬性並不保證快取保留特定層級。</span><span class="sxs-lookup"><span data-stu-id="1785e-224">The `priority` attribute does not guarantee a specific level of cache retention.</span></span> <span data-ttu-id="1785e-225">`CacheItemPriority`是只是建議。</span><span class="sxs-lookup"><span data-stu-id="1785e-225">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="1785e-226">將此屬性設定為`NeverRemove`不保證一定會保留在快取。</span><span class="sxs-lookup"><span data-stu-id="1785e-226">Setting this attribute to `NeverRemove` does not guarantee that the cache will always be retained.</span></span> <span data-ttu-id="1785e-227">請參閱[其他資源](#additional-resources)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="1785e-227">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="1785e-228">快取標記協助程式是依賴[記憶體快取服務](xref:performance/caching/memory)。</span><span class="sxs-lookup"><span data-stu-id="1785e-228">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="1785e-229">如果尚未加入快取標記協助程式就會加入服務。</span><span class="sxs-lookup"><span data-stu-id="1785e-229">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1785e-230">其他資源</span><span class="sxs-lookup"><span data-stu-id="1785e-230">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>