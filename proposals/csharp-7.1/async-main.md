---
ms.openlocfilehash: 405153448d0e3685d6f22725e00d75d9250b3e20
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484799"
---
# <a name="async-main"></a><span data-ttu-id="d76e3-101">Zaman uyumsuz ana</span><span class="sxs-lookup"><span data-stu-id="d76e3-101">Async Main</span></span>

* <span data-ttu-id="d76e3-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="d76e3-102">[x] Proposed</span></span>
* <span data-ttu-id="d76e3-103">[] Prototipi</span><span class="sxs-lookup"><span data-stu-id="d76e3-103">[ ] Prototype</span></span>
* <span data-ttu-id="d76e3-104">[] Uygulama</span><span class="sxs-lookup"><span data-stu-id="d76e3-104">[ ] Implementation</span></span>
* <span data-ttu-id="d76e3-105">[] Belirtimi</span><span class="sxs-lookup"><span data-stu-id="d76e3-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="d76e3-106">Özet</span><span class="sxs-lookup"><span data-stu-id="d76e3-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="d76e3-107">Giriş noktasının `Task` / `Task<int>` döndürmesini ve `async`olarak işaretlenmesini sağlayarak uygulamanın Main/EntryPoint yönteminde kullanılmasına `await` izin verin.</span><span class="sxs-lookup"><span data-stu-id="d76e3-107">Allow `await` to be used in an application's Main / entrypoint method by allowing the entrypoint to return `Task` / `Task<int>` and be marked `async`.</span></span>

## <a name="motivation"></a><span data-ttu-id="d76e3-108">Amacı</span><span class="sxs-lookup"><span data-stu-id="d76e3-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="d76e3-109">Öğrenirken C#, konsol tabanlı yardımcı programlar yazarken ve ana bilgisayardan `async` yöntemleri çağırmak ve `await` için küçük test uygulamaları yazarken çok yaygın hale gelir.</span><span class="sxs-lookup"><span data-stu-id="d76e3-109">It is very common when learning C#, when writing console-based utilities, and when writing small test apps to want to call and `await` `async` methods from Main.</span></span>  <span data-ttu-id="d76e3-110">Bugün, bu tür `await`' ı ayrı bir zaman uyumsuz yöntemde yapılacağından, geliştiricilerin çalışmaya başlamak için aşağıdaki gibi demirbaş yazmasına neden olan bir karmaşıklık düzeyi ekleyeceğiz:</span><span class="sxs-lookup"><span data-stu-id="d76e3-110">Today we add a level of complexity here by forcing such `await`'ing to be done in a separate async method, which causes developers to need to write boilerplate like the following just to get started:</span></span>

```csharp
public static void Main()
{
    MainAsync().GetAwaiter().GetResult();
}

private static async Task MainAsync()
{
    ... // Main body here
}
```

<span data-ttu-id="d76e3-111">Bu demirbaş için gereksinimi kaldırabiliriz ve yalnızca ana kendisinden `await`s 'nin kullanılabileceği gibi `async` izin vererek çalışmaya başlamanıza daha kolay hale getirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d76e3-111">We can remove the need for this boilerplate and make it easier to get started simply by allowing Main itself to be `async` such that `await`s can be used in it.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="d76e3-112">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="d76e3-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="d76e3-113">Şu anda Şu imzalara izin veriliyor entryPoints:</span><span class="sxs-lookup"><span data-stu-id="d76e3-113">The following signatures are currently allowed entrypoints:</span></span>

```csharp
static void Main()
static void Main(string[])
static int Main()
static int Main(string[])
```

<span data-ttu-id="d76e3-114">İzin verilen entryPoints listesini şunları içerecek şekilde genişlettik:</span><span class="sxs-lookup"><span data-stu-id="d76e3-114">We extend the list of allowed entrypoints to include:</span></span>

```csharp
static Task Main()
static Task<int> Main()
static Task Main(string[])
static Task<int> Main(string[])
```

<span data-ttu-id="d76e3-115">Uyumluluk risklerinden kaçınmak için, bu yeni imzalar yalnızca önceki küme için aşırı yükleme yoksa geçerli giriş noktaları olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="d76e3-115">To avoid compatibility risks, these new signatures will only be considered as valid entrypoints if no overloads of the previous set are present.</span></span>
<span data-ttu-id="d76e3-116">Dil/derleyici, giriş noktasının `async`olarak işaretlenmesini gerektirmez, ancak kullanımları büyük çoğunluğunun bu şekilde işaretleneceğini bekleiyoruz.</span><span class="sxs-lookup"><span data-stu-id="d76e3-116">The language / compiler will not require that the entrypoint be marked as `async`, though we expect the vast majority of uses will be marked as such.</span></span>

<span data-ttu-id="d76e3-117">Bunlardan biri giriş noktası olarak tanımlandığında, derleyici bu kodlanmış yöntemlerden birini çağıran gerçek bir giriş noktası yöntemini sentezle birleştirmeyecektir:</span><span class="sxs-lookup"><span data-stu-id="d76e3-117">When one of these is identified as the entrypoint, the compiler will synthesize an actual entrypoint method that calls one of these coded methods:</span></span>
- <span data-ttu-id="d76e3-118">```static Task Main()```, derleyicinin ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();``` eşdeğerini yayacaktır.</span><span class="sxs-lookup"><span data-stu-id="d76e3-118">```static Task Main()``` will result in the compiler emitting the equivalent of ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="d76e3-119">```static Task Main(string[])```, derleyicinin ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();``` eşdeğerini yayacaktır.</span><span class="sxs-lookup"><span data-stu-id="d76e3-119">```static Task Main(string[])``` will result in the compiler emitting the equivalent of ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="d76e3-120">```static Task<int> Main()```, derleyicinin ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();``` eşdeğerini yayacaktır.</span><span class="sxs-lookup"><span data-stu-id="d76e3-120">```static Task<int> Main()``` will result in the compiler emitting the equivalent of ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="d76e3-121">```static Task<int> Main(string[])```, derleyicinin ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();``` eşdeğerini yayacaktır.</span><span class="sxs-lookup"><span data-stu-id="d76e3-121">```static Task<int> Main(string[])``` will result in the compiler emitting the equivalent of ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span></span>

<span data-ttu-id="d76e3-122">Örnek kullanım:</span><span class="sxs-lookup"><span data-stu-id="d76e3-122">Example usage:</span></span>

```csharp
using System;
using System.Net.Http;

class Test
{
    static async Task Main(string[] args) =>
        Console.WriteLine(await new HttpClient().GetStringAsync(args[0]));
}
```

## <a name="drawbacks"></a><span data-ttu-id="d76e3-123">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="d76e3-123">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="d76e3-124">Ana sakıncası, ek giriş noktası imzalarını desteklemeye yönelik ek karmaşıklıkdır.</span><span class="sxs-lookup"><span data-stu-id="d76e3-124">The main drawback is simply the additional complexity of supporting additional entrypoint signatures.</span></span>

## <a name="alternatives"></a><span data-ttu-id="d76e3-125">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="d76e3-125">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="d76e3-126">Dikkate alınan diğer çeşitler:</span><span class="sxs-lookup"><span data-stu-id="d76e3-126">Other variants considered:</span></span>

<span data-ttu-id="d76e3-127">`async void`izin veriliyor.</span><span class="sxs-lookup"><span data-stu-id="d76e3-127">Allowing `async void`.</span></span>  <span data-ttu-id="d76e3-128">Kodun doğrudan çağrılması için semantiği aynı tutmanız gerekir, bu, oluşturulan bir giriş noktasının çağrı yapmasını zorlaştırır (görev döndürülmez).</span><span class="sxs-lookup"><span data-stu-id="d76e3-128">We need to keep the semantics the same for code calling it directly, which would then make it difficult for a generated entrypoint to call it (no Task returned).</span></span>  <span data-ttu-id="d76e3-129">Bunun gibi iki farklı yöntem oluşturarak bunu çözebiliriz.</span><span class="sxs-lookup"><span data-stu-id="d76e3-129">We could solve this by generating two other methods, e.g.</span></span>

```csharp
public static async void Main()
{
   ... // await code
}
```

<span data-ttu-id="d76e3-130">geldiğinde</span><span class="sxs-lookup"><span data-stu-id="d76e3-130">becomes</span></span>

```csharp
public static async void Main() => await $MainTask();

private static void $EntrypointMain() => Main().GetAwaiter().GetResult();

private static async Task $MainTask()
{
    ... // await code
}
```

<span data-ttu-id="d76e3-131">`async void`teşvik kullanımı konusunda da sorunlar vardır.</span><span class="sxs-lookup"><span data-stu-id="d76e3-131">There are also concerns around encouraging usage of `async void`.</span></span>

<span data-ttu-id="d76e3-132">Ad olarak "Main" yerine "MainAsync" kullanılıyor.</span><span class="sxs-lookup"><span data-stu-id="d76e3-132">Using "MainAsync" instead of "Main" as the name.</span></span>  <span data-ttu-id="d76e3-133">Zaman uyumsuz sonek görev döndüren yöntemler için önerilse de öncelikle kitaplık işlevselliği hakkında, ana olmayan ve "Main" dışındaki ek giriş noktası adlarını destekleyen bu değer bu kadar önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="d76e3-133">While the async suffix is recommended for Task-returning methods, that's primarily about library functionality, which Main is not, and supporting additional entrypoint names beyond "Main" is not worth it.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="d76e3-134">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="d76e3-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="d76e3-135">yok</span><span class="sxs-lookup"><span data-stu-id="d76e3-135">n/a</span></span>

## <a name="design-meetings"></a><span data-ttu-id="d76e3-136">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="d76e3-136">Design meetings</span></span>

<span data-ttu-id="d76e3-137">yok</span><span class="sxs-lookup"><span data-stu-id="d76e3-137">n/a</span></span>
