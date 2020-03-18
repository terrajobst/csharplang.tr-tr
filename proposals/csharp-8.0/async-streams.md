---
ms.openlocfilehash: 9ed1aa75d581cecbf754a84d1f523c6334b8c0ac
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79485261"
---
# <a name="async-streams"></a><span data-ttu-id="5b2ab-101">Zaman uyumsuz akışlar</span><span class="sxs-lookup"><span data-stu-id="5b2ab-101">Async Streams</span></span>

* <span data-ttu-id="5b2ab-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="5b2ab-102">[x] Proposed</span></span>
* <span data-ttu-id="5b2ab-103">[x] prototipi</span><span class="sxs-lookup"><span data-stu-id="5b2ab-103">[x] Prototype</span></span>
* <span data-ttu-id="5b2ab-104">[] Uygulama</span><span class="sxs-lookup"><span data-stu-id="5b2ab-104">[ ] Implementation</span></span>
* <span data-ttu-id="5b2ab-105">[] Belirtimi</span><span class="sxs-lookup"><span data-stu-id="5b2ab-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="5b2ab-106">Özet</span><span class="sxs-lookup"><span data-stu-id="5b2ab-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="5b2ab-107">C#Yineleyici yöntemlerine ve zaman uyumsuz yöntemlere yönelik desteğe sahiptir, ancak hem Yineleyici hem de async yöntemi olan bir yöntem için destek vermez.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-107">C# has support for iterator methods and async methods, but no support for a method that is both an iterator and an async method.</span></span>  <span data-ttu-id="5b2ab-108">`await`, yeni bir `IEnumerable<T>` `IEnumerator<T>`tüketilebilir bir `IAsyncEnumerable<T>` veya `await foreach`yerine bir `IAsyncEnumerable<T>` veya `IAsyncEnumerator<T>` döndüren yeni bir `async` Yineleyici biçiminde kullanılmasına izin vererek bunu düzeltmemiz gerekir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-108">We should rectify this by allowing for `await` to be used in a new form of `async` iterator, one that returns an `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` rather than an `IEnumerable<T>` or `IEnumerator<T>`, with `IAsyncEnumerable<T>` consumable in a new `await foreach`.</span></span>  <span data-ttu-id="5b2ab-109">Zaman uyumsuz temizlemeyi etkinleştirmek için bir `IAsyncDisposable` arabirimi de kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-109">An `IAsyncDisposable` interface is also used to enable asynchronous cleanup.</span></span>

## <a name="related-discussion"></a><span data-ttu-id="5b2ab-110">İlgili tartışma</span><span class="sxs-lookup"><span data-stu-id="5b2ab-110">Related discussion</span></span>
- https://github.com/dotnet/roslyn/issues/261
- https://github.com/dotnet/roslyn/issues/114

## <a name="detailed-design"></a><span data-ttu-id="5b2ab-111">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="5b2ab-111">Detailed design</span></span>
[design]: #detailed-design

## <a name="interfaces"></a><span data-ttu-id="5b2ab-112">Arabirimler</span><span class="sxs-lookup"><span data-stu-id="5b2ab-112">Interfaces</span></span>

### <a name="iasyncdisposable"></a><span data-ttu-id="5b2ab-113">IAsyncDisposable</span><span class="sxs-lookup"><span data-stu-id="5b2ab-113">IAsyncDisposable</span></span>

<span data-ttu-id="5b2ab-114">`IAsyncDisposable` çok fazla tartışma vardır (örn. https://github.com/dotnet/roslyn/issues/114) ve bunun iyi bir fikir olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-114">There has been much discussion of `IAsyncDisposable` (e.g. https://github.com/dotnet/roslyn/issues/114) and whether it's a good idea.</span></span>  <span data-ttu-id="5b2ab-115">Ancak, zaman uyumsuz yineleyiciler desteğini eklemek için gerekli bir kavramdır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-115">However, it's a required concept to add in support of async iterators.</span></span>  <span data-ttu-id="5b2ab-116">`finally` blokları `await`s içerdiğinden ve `finally` bloklarının yineleyiciler elden çıkarılma kapsamında çalıştırılması gerektiğinden, zaman uyumsuz olarak elden çıkarılmız gerekir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-116">Since `finally` blocks may contain `await`s, and since `finally` blocks need to be run as part of disposing of iterators, we need async disposal.</span></span>  <span data-ttu-id="5b2ab-117">Ayrıca, kaynakların temizlenmesi her zaman herhangi bir süre alabilir (örneğin, temizleme gerektiren), kaydını kaldırmak geri çağırmaları ve kayıt silme işleminin ne zaman tamamlandığını bilmeniz için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-117">It's also just generally useful any time cleaning up of resources might take any period of time, e.g. closing files (requiring flushes), deregistering callbacks and providing a way to know when deregistration has completed, etc.</span></span>

<span data-ttu-id="5b2ab-118">Çekirdek .NET kitaplıklarına aşağıdaki arabirim eklenmiştir (örn. System. Private. CoreLib/System. Runtime):</span><span class="sxs-lookup"><span data-stu-id="5b2ab-118">The following interface is added to the core .NET libraries (e.g. System.Private.CoreLib / System.Runtime):</span></span>
```csharp
namespace System
{
    public interface IAsyncDisposable
    {
        ValueTask DisposeAsync();
    }
}
```
<span data-ttu-id="5b2ab-119">`Dispose`olduğu gibi, `DisposeAsync` birden çok kez çağırma kabul edilebilir ve ilki, zaman uyumlu olarak tamamlanan başarılı bir görev döndüren (`DisposeAsync` iş parçacığı açısından güvenli olmaması, ancak eşzamanlı çağrıyı desteklememesinin gerekli olması gerekir).</span><span class="sxs-lookup"><span data-stu-id="5b2ab-119">As with `Dispose`, invoking `DisposeAsync` multiple times is acceptable, and subsequent invocations after the first should be treated as nops, returning a synchronously completed successful task (`DisposeAsync` need not be thread-safe, though, and need not support concurrent invocation).</span></span>  <span data-ttu-id="5b2ab-120">Ayrıca, türler hem `IDisposable` hem de `IAsyncDisposable`uygulayabilir ve bu durumda, `Dispose` çağırma ve `DisposeAsync` sonra da yalnızca ilki anlamlı olmalıdır ve sonraki etkinleştirmeleri bir NOP olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-120">Further, types may implement both `IDisposable` and `IAsyncDisposable`, and if they do, it's similarly acceptable to invoke `Dispose` and then `DisposeAsync` or vice versa, but only the first should be meaningful and subsequent invocations of either should be a nop.</span></span>  <span data-ttu-id="5b2ab-121">Bu nedenle, bir tür her ikisini de uygulıyorsa, tüketicilerin bir kez çağırmaları ve bağlamı temel alan daha ilgili yöntemi, zaman uyumlu bağlamlarda `Dispose` ve zaman uyumsuz olanlarda `DisposeAsync`.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-121">As such, if a type does implement both, consumers are encouraged to call once and only once the more relevant method based on the context, `Dispose` in synchronous contexts and `DisposeAsync` in asynchronous ones.</span></span>

<span data-ttu-id="5b2ab-122">(`IAsyncDisposable` `using` ile ayrı bir tartışmaya nasıl etkileşime giyorum.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-122">(I'm leaving discussion of how `IAsyncDisposable` interacts with `using` to a separate discussion.</span></span>  <span data-ttu-id="5b2ab-123">Bu teklifin ilerleyen kısımlarında `foreach` ile nasıl etkileşime girdiğinin kapsamı.)</span><span class="sxs-lookup"><span data-stu-id="5b2ab-123">And coverage of how it interacts with `foreach` is handled later in this proposal.)</span></span>

<span data-ttu-id="5b2ab-124">Dikkate alınan alternatifler:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-124">Alternatives considered:</span></span>
- <span data-ttu-id="5b2ab-125">_`CancellationToken`kabul`DisposeAsync`_ : teorik olarak, zaman uyumsuz bir şekilde iptal edilebilir, elden çıkarma Temizleme, işlemleri kapatma, ücretsiz kaynakları, vb., genellikle iptal edilmesi gereken bir şey değildir. iptal edilen işler için Temizleme hala önemlidir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-125">_`DisposeAsync` accepting a `CancellationToken`_: while in theory it makes sense that anything async can be canceled, disposal is about cleanup, closing things out, free'ing resources, etc., which is generally not something that should be canceled; cleanup is still important for work that's canceled.</span></span>  <span data-ttu-id="5b2ab-126">Fiili çalışmanın iptal edilmesine neden olan `CancellationToken`, genellikle, işin iptal edilmesi `DisposeAsync` bir NOP olmasına neden olacağından, `DisposeAsync` bir `DisposeAsync`geçirilen belirteç olur.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-126">The same `CancellationToken` that caused the actual work to be canceled would typically be the same token passed to `DisposeAsync`, making `DisposeAsync` worthless because cancellation of the work would cause `DisposeAsync` to be a nop.</span></span>  <span data-ttu-id="5b2ab-127">Birisi, elden çıkarılmayı beklerken engellenmemek isterse, sonuçta elde edilen `ValueTask`beklemeyi veya yalnızca belirli bir süre boyunca beklemeyi önleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-127">If someone wants to avoid being blocked waiting for disposal, they can avoid waiting on the resulting `ValueTask`, or wait on it only for some period of time.</span></span>
- <span data-ttu-id="5b2ab-128">_`DisposeAsync` `Task`döndürme_ : artık genel olmayan bir `ValueTask` var ve bir `IValueTaskSource`oluşturulabilir. bundan sonra `ValueTask` döndürme, var olan bir nesnenin `DisposeAsync` son zaman uyumsuz tamamlamayı temsil eden Promise olarak yeniden kullanılmasına izin veriyor ve `DisposeAsync`zaman uyumsuz olarak tamamlandığına `Task` bir ayırmayı kaydetmektir.`DisposeAsync`</span><span class="sxs-lookup"><span data-stu-id="5b2ab-128">_`DisposeAsync` returning a `Task`_: Now that a non-generic `ValueTask` exists and can be constructed from an `IValueTaskSource`, returning `ValueTask` from `DisposeAsync` allows an existing object to be reused as the promise representing the eventual async completion of `DisposeAsync`, saving a `Task` allocation in the case where `DisposeAsync` completes asynchronously.</span></span>
- <span data-ttu-id="5b2ab-129">_`DisposeAsync` `bool continueOnCapturedContext` (`ConfigureAwait`) Ile yapılandırma_: Bu tür bir kavramın `using`, `foreach`ve bunu kullanan diğer dil yapılarına, bir arabirim perspektifinden, hiçbir `await`değil ve yapılandırma için hiçbir şey yapma gibi sorunlar olabilir... `ValueTask` tüketicileri, ancak istedikleri gibi kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-129">_Configuring `DisposeAsync` with a `bool continueOnCapturedContext` (`ConfigureAwait`)_: While there may be issues related to how such a concept is exposed to `using`, `foreach`, and other language constructs that consume this, from an interface perspective it's not actually doing any `await`'ing and there's nothing to configure... consumers of the `ValueTask` can consume it however they wish.</span></span>
- <span data-ttu-id="5b2ab-130">_`IAsyncDisposable` devralan `IDisposable`_ : yalnızca bir veya diğeri kullanılması gerektiğinden, her ikisini de uygulamak için türleri zorlamak mantıklı değildir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-130">_`IAsyncDisposable` inheriting `IDisposable`_:  Since only one or the other should be used, it doesn't make sense to force types to implement both.</span></span>
- <span data-ttu-id="5b2ab-131">_`IAsyncDisposable`yerine`IDisposableAsync`_ : "zaman uyumsuz bir şey" olan ve işlemler "tamamlandı" olarak, bu nedenle türlerin ön ek olarak "Async" olması ve yöntemlerin bir sonek olarak "zaman uyumsuz" olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-131">_`IDisposableAsync` instead of `IAsyncDisposable`_: We've been following the naming that things/types are an "async something" whereas operations are "done async", so types have "Async" as a prefix and methods have "Async" as a suffix.</span></span>

### <a name="iasyncenumerable--iasyncenumerator"></a><span data-ttu-id="5b2ab-132">Iasyncenumerable/ıasyncenumerator</span><span class="sxs-lookup"><span data-stu-id="5b2ab-132">IAsyncEnumerable / IAsyncEnumerator</span></span>

<span data-ttu-id="5b2ab-133">Çekirdek .NET kitaplıklarına iki arabirim eklenir:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-133">Two interfaces are added to the core .NET libraries:</span></span>

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken cancellationToken = default);
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> MoveNextAsync();
        T Current { get; }
    }
}
```

<span data-ttu-id="5b2ab-134">Tipik tüketim (ek dil özellikleri olmadan) şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-134">Typical consumption (without additional language features) would look like:</span></span>

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
        Use(enumerator.Current);
    }
}
finally { await enumerator.DisposeAsync(); }
```

<span data-ttu-id="5b2ab-135">Değerlendirilen atılan seçenekler:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-135">Discarded options considered:</span></span>
- <span data-ttu-id="5b2ab-136">_`Task<bool> MoveNextAsync(); T current { get; }`_ : `Task<bool>` kullanılması, zaman uyumlu, başarılı `MoveNextAsync` çağrılarını temsil etmek için önbelleğe alınmış bir görev nesnesinin kullanımını destekleyecektir, ancak zaman uyumsuz tamamlama için bir ayırma gerekli olmaya devam eder.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-136">_`Task<bool> MoveNextAsync(); T current { get; }`_: Using `Task<bool>` would support using a cached task object to represent synchronous, successful `MoveNextAsync` calls, but an allocation would still be required for asynchronous completion.</span></span>  <span data-ttu-id="5b2ab-137">`ValueTask<bool>`döndüleyerek, Numaralandırıcı nesnesinin kendisi için `IValueTaskSource<bool>` uygulaması ve `MoveNextAsync`döndürülen `ValueTask<bool>` için yedekleme olarak kullanılması ve ayrıca önemli ölçüde azaltılan büyük kafa için izin verir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-137">By returning `ValueTask<bool>`, we enable the enumerator object to itself implement `IValueTaskSource<bool>` and be used as the backing for the `ValueTask<bool>` returned from `MoveNextAsync`, which in turn allows for significantly reduced overheads.</span></span>
- <span data-ttu-id="5b2ab-138">_`ValueTask<(bool, T)> MoveNextAsync();`_ : yalnızca tüketilmek zordur, ancak `T` artık değişkenle birlikte olmaması anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-138">_`ValueTask<(bool, T)> MoveNextAsync();`_: It's not only harder to consume, but it means that `T` can no longer be covariant.</span></span>
- <span data-ttu-id="5b2ab-139">_`ValueTask<T?> TryMoveNextAsync();`_ : covaryant değil.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-139">_`ValueTask<T?> TryMoveNextAsync();`_: Not covariant.</span></span>
- <span data-ttu-id="5b2ab-140">_`Task<T?> TryMoveNextAsync();`_ : birlikte değişken değil, her çağrıda ayırmalar, vb.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-140">_`Task<T?> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="5b2ab-141">_`ITask<T?> TryMoveNextAsync();`_ : birlikte değişken değil, her çağrıda ayırmalar, vb.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-141">_`ITask<T?> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="5b2ab-142">_`ITask<(bool,T)> TryMoveNextAsync();`_ : birlikte değişken değil, her çağrıda ayırmalar, vb.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-142">_`ITask<(bool,T)> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="5b2ab-143">_`Task<bool> TryMoveNextAsync(out T result);`_ : `out` sonucunun, işlem zaman uyumlu olarak döndürüldüğünde, daha sonra görevi zaman uyumsuz olarak tamamladığı zaman, sonucu iletmenin bir yolu olmadığı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-143">_`Task<bool> TryMoveNextAsync(out T result);`_: The `out` result would need to be set when the operation returns synchronously, not when it asynchronously completes the task potentially sometime long in the future, at which point there'd be no way to communicate the result.</span></span>
- <span data-ttu-id="5b2ab-144">_`IAsyncEnumerator<T>` `IAsyncDisposable`uygulamayız_ : bunları ayırmanızı seçebiliriz.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-144">_`IAsyncEnumerator<T>` not implementing `IAsyncDisposable`_: We could choose to separate these.</span></span>  <span data-ttu-id="5b2ab-145">Ancak, daha sonra, kodun bir Numaralandırıcı sağlamaması olasılığını karşılayamaz ve bu sayede, model tabanlı yardımcılar yazmayı zorlaştırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-145">However, doing so complicates certain other areas of the proposal, as code must then be able to deal with the possibility that an enumerator doesn't provide disposal, which makes it difficult to write pattern-based helpers.</span></span>  <span data-ttu-id="5b2ab-146">Ayrıca, numaralandırıcıların elden çıkarılma ihtiyacı olan (ör. bir finally bloğuna sahip tüm C# zaman uyumsuz Yineleyici, bir ağ bağlantısından verileri numaralandırma, vb.) ortak olacaktır ve bunlardan biri yoksa, yöntemi yalnızca en az ek yük `public ValueTask DisposeAsync() => default(ValueTask);` olarak uygulamak kolaydır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-146">Further, it will be common for enumerators to have a need for disposal (e.g. any C# async iterator that has a finally block, most things enumerating data from a network connection, etc.), and if one doesn't, it is simple to implement the method purely as `public ValueTask DisposeAsync() => default(ValueTask);` with minimal additional overhead.</span></span>
- <span data-ttu-id="5b2ab-147">_ `IAsyncEnumerator<T> GetAsyncEnumerator()`: iptal belirteci parametresi yok.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-147">_ `IAsyncEnumerator<T> GetAsyncEnumerator()`: No cancellation token parameter.</span></span>

#### <a name="viable-alternative"></a><span data-ttu-id="5b2ab-148">Uygulanabilir alternatif:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-148">Viable alternative:</span></span>

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator();
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> WaitForNextAsync();
        T TryGetNext(out bool success);
    }
}
```

<span data-ttu-id="5b2ab-149">`TryGetNext`, zaman uyumlu olarak kullanılabilir oldukları sürece tek bir arabirim çağrısıyla öğeleri kullanmak için bir iç döngüde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-149">`TryGetNext` is used in an inner loop to consume items with a single interface call as long as they're available synchronously.</span></span>  <span data-ttu-id="5b2ab-150">Bir sonraki öğe zaman uyumlu olarak alınamadığından, false döndürür ve false döndürdüğünde bir çağıran, bir sonraki öğenin kullanılabilir olmasını beklemek ya da hiçbir zaman başka bir öğe olmadığını anlamak için `WaitForNextAsync` çağırmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-150">When the next item can't be retrieved synchronously, it returns false, and any time it returns false, a caller must subsequently invoke `WaitForNextAsync` to either wait for the next item to be available or to determine that there will never be another item.</span></span> <span data-ttu-id="5b2ab-151">Tipik tüketim (ek dil özellikleri olmadan) şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-151">Typical consumption (without additional language features) would look like:</span></span>

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.WaitForNextAsync())
    {
        while (true)
        {
            int item = enumerator.TryGetNext(out bool success);
            if (!success) break;
            Use(item);
        }
    }
}
finally { await enumerator.DisposeAsync(); }
```

<span data-ttu-id="5b2ab-152">Bunun avantajı iki kat, bir küçük ve bir büyük:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-152">The advantage of this is two-fold, one minor and one major:</span></span>
- <span data-ttu-id="5b2ab-153">_İkincil: bir Numaralandırıcının birden çok tüketicileri desteklemesini sağlar_.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-153">_Minor: Allows for an enumerator to support multiple consumers_.</span></span> <span data-ttu-id="5b2ab-154">Bir Numaralandırıcının birden çok eşzamanlı tüketicilerini desteklemesi için değerli senaryolar olabilir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-154">There may be scenarios where it's valuable for an enumerator to support multiple concurrent consumers.</span></span>  <span data-ttu-id="5b2ab-155">`MoveNextAsync` ve `Current` ayrı olduğunda bu, bir uygulamanın kullanım atomik hale getirme yapabilmeleri için elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-155">That can't be achieved when `MoveNextAsync` and `Current` are separate such that an implementation can't make their usage atomic.</span></span>  <span data-ttu-id="5b2ab-156">Buna karşılık bu yaklaşım, numaralandırıcı İleri doğru göndermeyi ve sonraki öğeyi almayı destekleyen `TryGetNext` tek bir yöntem sağlar. bu nedenle, Numaralandırıcı istenirse, Numaralandırıcının kararlılık 'i sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-156">In contrast, this approach provides a single method `TryGetNext` that supports pushing the enumerator forward and getting the next item, so the enumerator can enable atomicity if desired.</span></span>  <span data-ttu-id="5b2ab-157">Ancak, bu tür senaryolar, her tüketiciye paylaşılan bir numaralandırıcıdan kendi Numaralandırıcı vererek da etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-157">However, it's likely that such scenarios could also be enabled by giving each consumer its own enumerator from a shared enumerable.</span></span>  <span data-ttu-id="5b2ab-158">Ayrıca, her Numaralandırıcı için çok sayıda büyük kafa gerektirmeyen büyük/küçük harfe basit olmayan büyük kafa ekler ve bu da bir arabirimin tüketicisinin genellikle bu herhangi bir şekilde dayanmadığı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-158">Further, we don't want to enforce that every enumerator support concurrent usage, as that would add non-trivial overheads to the majority case that doesn't require it, which means a consumer of the interface generally couldn't rely on this any way.</span></span>
- <span data-ttu-id="5b2ab-159">_Birincil: performans_.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-159">_Major: Performance_.</span></span> <span data-ttu-id="5b2ab-160">`MoveNextAsync`/`Current` yaklaşımı, işlem başına iki arabirim çağrısı gerektirir, ancak `WaitForNextAsync`/`TryGetNext` için en iyi durum, her bir işlem için yalnızca bir arabirim çağrısına sahip olduğumuz, `TryGetNext`ile sıkı bir iç döngüyü etkinleştirerek, en çok yineleme zaman uyumlu olarak tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-160">The `MoveNextAsync`/`Current` approach requires two interface calls per operation, whereas the best case for `WaitForNextAsync`/`TryGetNext` is that most iterations complete synchronously, enabling a tight inner loop with `TryGetNext`, such that we only have one interface call per operation.</span></span>  <span data-ttu-id="5b2ab-161">Bu durum, arabirimin, hesaplamayı ayırt ettiği durumlarda ölçülebilir bir etkiye sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-161">This can have a measurable impact in situations where the interface calls dominate the computation.</span></span>

<span data-ttu-id="5b2ab-162">Bununla birlikte, bu, el ile kullanılırken büyük ölçüde artan karmaşıklık ve bunları kullanırken hatalara yönelik daha fazla şans dahil olmak üzere, önemsiz olmayan küçük yanlar vardır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-162">However, there are non-trivial downsides, including significantly increased complexity when consuming these manually, and an increased chance of introducing bugs when using them.</span></span>  <span data-ttu-id="5b2ab-163">Ayrıca, performans avantajları mikro kıyaslamalar halinde gösterilirken, gerçek kullanımın büyük çoğunluğunda ne kadar etkili olduğunu düşünmedik.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-163">And while the performance benefits show up in microbenchmarks, we don't believe they'll be impactful in the vast majority of real usage.</span></span>  <span data-ttu-id="5b2ab-164">Bunlar ise, hafif bir şekilde ikinci bir arabirim kümesi tanıtıyoruz.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-164">If it turns out they are, we can introduce a second set of interfaces in a light-up fashion.</span></span>

<span data-ttu-id="5b2ab-165">Değerlendirilen atılan seçenekler:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-165">Discarded options considered:</span></span>
- <span data-ttu-id="5b2ab-166">`ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: `out` parametreler birlikte değişken olamaz.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-166">`ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: `out` parameters can't be covariant.</span></span>  <span data-ttu-id="5b2ab-167">Burada, bu durum büyük olasılıkla başvuru türü sonuçları için bir çalışma zamanı yazma engeli olan küçük bir etkisi (genel olarak TRY düzeniyle ilgili bir sorun) de vardır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-167">There's also a small impact here (an issue with the try pattern in general) that this likely incurs a runtime write barrier for reference type results.</span></span>

#### <a name="cancellation"></a><span data-ttu-id="5b2ab-168">İptal</span><span class="sxs-lookup"><span data-stu-id="5b2ab-168">Cancellation</span></span>

<span data-ttu-id="5b2ab-169">İptali desteklemeye yönelik birkaç olası yaklaşım vardır:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-169">There are several possible approaches to supporting cancellation:</span></span>
1. <span data-ttu-id="5b2ab-170">`IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` iptal-belirsiz: `CancellationToken` hiçbir yerde görünmez.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-170">`IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` are cancellation-agnostic: `CancellationToken` doesn't appear anywhere.</span></span>  <span data-ttu-id="5b2ab-171">İptal işlemi, `CancellationToken` uygun şekilde sayılabilen ve/veya numaralandırıcıların mantıklı bir şekilde (örneğin, bir yineleyici çağrılırken), `CancellationToken` Yineleyici yöntemine bir bağımsız değişken olarak geçirerek ve yineleyicinin gövdesinde, diğer herhangi bir parametre ile yapıldığı gibi mantıksal olarak bir şekilde inceleyerek elde edilir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-171">Cancellation is achieved by logically baking the `CancellationToken` into the enumerable and/or enumerator in whatever manner is appropriate, e.g. when calling an iterator, passing the `CancellationToken` as an argument to the iterator method and using it in the body of the iterator, as is done with any other parameter.</span></span>
2. <span data-ttu-id="5b2ab-172">`IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: bir `CancellationToken` `GetAsyncEnumerator`ve sonraki `MoveNextAsync` işlemleri buna göre daha da uygun hale gelir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-172">`IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: You pass a `CancellationToken` to `GetAsyncEnumerator`, and subsequent `MoveNextAsync` operations respect it however it can.</span></span>
3. <span data-ttu-id="5b2ab-173">`IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: `CancellationToken` her bir `MoveNextAsync` çağrısına geçiş yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-173">`IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: You pass a `CancellationToken` to each individual `MoveNextAsync` call.</span></span>
4. <span data-ttu-id="5b2ab-174">1 & & 2: `CancellationToken`her ikisi de numaralandırılabilir/numaralandırıcıuygulamanıza katıştırdığınızda ve `CancellationToken`s 'yi `GetAsyncEnumerator`olarak geçitirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-174">1 && 2: You both embed `CancellationToken`s into your enumerable/enumerator and pass `CancellationToken`s into `GetAsyncEnumerator`.</span></span>
5. <span data-ttu-id="5b2ab-175">1 & & 3: `CancellationToken`her ikisi de numaralandırılabilir/Numaralandırıcıya katıştırmanız ve `CancellationToken`s 'yi `MoveNextAsync`'e iletmeniz.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-175">1 && 3: You both embed `CancellationToken`s into your enumerable/enumerator and pass `CancellationToken`s into `MoveNextAsync`.</span></span>

<span data-ttu-id="5b2ab-176">Tamamen teorik olarak bir perspektiften (5) en sağlam bir `CancellationToken` kabul `MoveNextAsync`, bu, iptal edilme ve (b) `CancellationToken`, yalnızca Yineleyicilerde bağımsız değişken olarak geçirilebilecek, rastgele türlere gömülü olan diğer herhangi bir tür olur.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-176">From a purely theoretical perspective, (5) is the most robust, in that (a) `MoveNextAsync` accepting a `CancellationToken` enables the most fine-grained control over what's canceled, and (b) `CancellationToken` is just any other type that can passed as an argument into iterators, embedded in arbitrary types, etc.</span></span>

<span data-ttu-id="5b2ab-177">Ancak, bu yaklaşımda birden fazla sorun vardır:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-177">However, there are multiple problems with that approach:</span></span>
- <span data-ttu-id="5b2ab-178">`GetAsyncEnumerator` geçirilen `CancellationToken`, yineleyici gövdesinde nasıl yapılır?</span><span class="sxs-lookup"><span data-stu-id="5b2ab-178">How does a `CancellationToken` passed to `GetAsyncEnumerator` make it into the body of the iterator?</span></span>  <span data-ttu-id="5b2ab-179">`GetEnumerator`geçirilen `CancellationToken` erişim sağlamak için, nokta olarak kullanabileceğiniz yeni bir `iterator` anahtar sözcüğü kullanıma sunarız. ancak, çok fazla sayıda ek makine olan b) çok birinci sınıf vatandaşlık ve c), %99 servis talebinin bir yineleyici çağıran ve üzerinde `GetAsyncEnumerator` çağıran aynı kod gibi görünse de, bu durumda `CancellationToken` bir bağımsız değişken olarak yalnızca bir bağımsız değişken olarak geçirebileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-179">We could expose a new `iterator` keyword that you could dot off of to get access to the `CancellationToken` passed to `GetEnumerator`, but a) that's a lot of additional machinery, b) we're making it a very first-class citizen, and c) the 99% case would seem to be the same code both calling an iterator and calling `GetAsyncEnumerator` on it, in which case it can just pass the `CancellationToken` as an argument into the method.</span></span>
- <span data-ttu-id="5b2ab-180">`MoveNextAsync` geçirilen bir `CancellationToken` yöntemin gövdesine nasıl alınır?</span><span class="sxs-lookup"><span data-stu-id="5b2ab-180">How does a `CancellationToken` passed to `MoveNextAsync` get into the body of the method?</span></span>  <span data-ttu-id="5b2ab-181">Bu, `iterator` yerel bir nesne tarafından sunulmamış gibi daha da kötüleşirse, bu değerin değeri await genelinde değişebilir, bu da belirteçle kaydedilen tüm kodların, bir süre sonra yeniden kaydolmadan önce kayıt yapması gerekir; Ayrıca, bir yineleyici içinde derleyici tarafından uygulanıp uygulanmadığı veya bir geliştirici tarafından el ile yapılan her `MoveNextAsync` çağrısında kayıt ve kayıt silme yapmak çok pahalı bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-181">This is even worse, as if it's exposed off of an `iterator` local object, its value could change across awaits, which means any code that registered with the token would need to unregister from it prior to awaits and then re-register after; it's also potentially quite expensive to need to do such registering and unregistering in every `MoveNextAsync` call, regardless of whether implemented by the compiler in an iterator or by a developer manually.</span></span>
- <span data-ttu-id="5b2ab-182">Geliştirici `foreach` döngüsünü nasıl iptal eder?</span><span class="sxs-lookup"><span data-stu-id="5b2ab-182">How does a developer cancel a `foreach` loop?</span></span>  <span data-ttu-id="5b2ab-183">Bu işlem, numaralandırılabilir/Numaralandırıcı için bir `CancellationToken` verip, sonra da bir) Numaralandırıcılar üzerinde `foreach`' ı desteklememiz gerekir, bu, bunları ilk sınıf vatandaşları haline getiriyor ve artık numaralandırıcıların (örn. LINQ yöntemleri) veya b 'nin bulunduğu bir ekosistem hakkında düşünmeye başlamanız gerekir. `CancellationToken`, sunulan belirteci depolayan `IAsyncEnumerable<T>` `WithCancellation` uzantı yöntemine sahip olan ve daha sonra döndürülen yapı üzerindeki `GetAsyncEnumerator` olduğunda sarmalanmış numaralandırıcılara iletmeleri gerekir `GetAsyncEnumerator` çağrılır (Bu belirteç yok sayılıyor).</span><span class="sxs-lookup"><span data-stu-id="5b2ab-183">If it's done by giving a `CancellationToken` to an enumerable/enumerator, then either a) we need to support `foreach`'ing over enumerators, which raises them to being first-class citizens, and now you need to start thinking about an ecosystem built up around enumerators (e.g. LINQ methods) or b) we need to embed the `CancellationToken` in the enumerable anyway by having some `WithCancellation` extension method off of `IAsyncEnumerable<T>` that would store the provided token and then pass it into  the wrapped enumerable's `GetAsyncEnumerator` when the `GetAsyncEnumerator` on the returned struct is invoked (ignoring that token).</span></span>  <span data-ttu-id="5b2ab-184">Ya da yalnızca foreach gövdesinde bulunan `CancellationToken` kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-184">Or, you can just use the `CancellationToken` you have in the body of the foreach.</span></span>
- <span data-ttu-id="5b2ab-185">Sorgu anlama destekleniyorsa, `GetEnumerator` veya `MoveNextAsync` `CancellationToken` sağlanan her bir yan tümcesine nasıl geçirilecek?</span><span class="sxs-lookup"><span data-stu-id="5b2ab-185">If/when query comprehensions are supported, how would the `CancellationToken` supplied to `GetEnumerator` or `MoveNextAsync` be passed into each clause?</span></span>  <span data-ttu-id="5b2ab-186">En kolay yol, yan tümce yalnızca bunu yakalamak için, `GetAsyncEnumerator`/`MoveNextAsync` yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-186">The easiest way would simply be for the clause to capture it, at which point whatever token is passed to `GetAsyncEnumerator`/`MoveNextAsync` is ignored.</span></span>

<span data-ttu-id="5b2ab-187">Bu belgenin önceki bir sürümü önerilir (1), ancak karşılaştık (4).</span><span class="sxs-lookup"><span data-stu-id="5b2ab-187">An earlier version of this document recommended (1), but we since switched to (4).</span></span>

<span data-ttu-id="5b2ab-188">(1) ile ilgili iki ana sorun:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-188">The two main problems with (1):</span></span>
- <span data-ttu-id="5b2ab-189">üreticileri of cancellenebilir numaralar, bazı ortak uygulama ve derleyicinin yalnızca zaman uyumsuz-yineleyiciler desteğini `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` bir yöntem uygulamak için kullanabilirler.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-189">producers of cancellable enumerables have to implement some boilerplate, and can only leverage the compiler's support for async-iterators to implement a `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` method.</span></span>
- <span data-ttu-id="5b2ab-190">birçok üreticileri, bunun yerine zaman uyumsuz olan imzaya bir `CancellationToken` parametresi eklemeyi tercih edilebilir, bu da tüketicilerin bir `IAsyncEnumerable` türü verildiğinde istedikleri iptal belirtecini geçmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-190">it is likely that many producers would be tempted to just add a `CancellationToken` parameter to their async-enumerable signature instead, which will prevent consumers from passing the cancellation token they want when they are given an `IAsyncEnumerable` type.</span></span>

<span data-ttu-id="5b2ab-191">İki ana tüketim senaryosu vardır:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-191">There are two main consumption scenarios:</span></span>
1. <span data-ttu-id="5b2ab-192">tüketicinin zaman uyumsuz-yineleyici yöntemini çağırdığı `await foreach (var i in GetData(token)) ...`,</span><span class="sxs-lookup"><span data-stu-id="5b2ab-192">`await foreach (var i in GetData(token)) ...` where the consumer calls the async-iterator method,</span></span>
2. <span data-ttu-id="5b2ab-193">tüketicinin verilen bir `IAsyncEnumerable` örneğiyle ilgilenen `await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...`.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-193">`await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...` where the consumer deals with a given `IAsyncEnumerable` instance.</span></span>

<span data-ttu-id="5b2ab-194">Her iki senaryoyu da hem üreticileri hem de Async-Streams kullanıcıları için uygun bir şekilde desteklemeye yönelik makul bir uzlaşma olduğunu fark ettik. Bu, zaman uyumsuz-yineleyici yönteminde özel olarak açıklamalı bir parametre kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-194">We find that a reasonable compromise to support both scenarios in a way that is convenient for both producers and consumers of async-streams is to use a specially annotated parameter in the async-iterator method.</span></span> <span data-ttu-id="5b2ab-195">`[EnumeratorCancellation]` özniteliği bu amaçla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-195">The `[EnumeratorCancellation]` attribute is used for this purpose.</span></span> <span data-ttu-id="5b2ab-196">Bu özniteliği bir parametreye yerleştirmek derleyiciye bir belirteç `GetAsyncEnumerator` yöntemine geçirildiğinde, parametre için başlangıçta geçirilen değer yerine bu belirtecin kullanılması gerektiğini bildirir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-196">Placing this attribute on a parameter tells the compiler that if a token is passed to the `GetAsyncEnumerator` method, that token should be used instead of the value originally passed for the parameter.</span></span>

<span data-ttu-id="5b2ab-197">`IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-197">Consider `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`.</span></span> <span data-ttu-id="5b2ab-198">Bu yöntemin uygulayıcısı Yöntem gövdesinde parametresini yalnızca kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-198">The implementer of this method can simply use the parameter in the method body.</span></span> <span data-ttu-id="5b2ab-199">Tüketici, yukarıdaki tüketim desenlerini kullanabilir:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-199">The consumer can use either consumption patterns above:</span></span>
1. <span data-ttu-id="5b2ab-200">`GetData(token)`kullanırsanız, belirteç zaman uyumsuz-sıralanabilir olarak kaydedilir ve yineleme içinde kullanılacaktır,</span><span class="sxs-lookup"><span data-stu-id="5b2ab-200">if you use `GetData(token)`, then the token is saved into the async-enumerable and will be used in iteration,</span></span>
2. <span data-ttu-id="5b2ab-201">`givenIAsyncEnumerable.WithCancellation(token)`kullanırsanız, `GetAsyncEnumerator` öğesine geçirilen belirteç, zaman uyumsuz-numaralandırılabilir ' a kaydedilen belirtecin yerini alır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-201">if you use `givenIAsyncEnumerable.WithCancellation(token)`, then the token passed to `GetAsyncEnumerator` will supersede any token saved in the async-enumerable.</span></span>

## <a name="foreach"></a><span data-ttu-id="5b2ab-202">foreach</span><span class="sxs-lookup"><span data-stu-id="5b2ab-202">foreach</span></span>

<span data-ttu-id="5b2ab-203">`foreach`, mevcut `IEnumerable<T>`desteğinin yanı sıra `IAsyncEnumerable<T>` desteklemek için de genişletilebilir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-203">`foreach` will be augmented to support `IAsyncEnumerable<T>` in addition to its existing support for `IEnumerable<T>`.</span></span>  <span data-ttu-id="5b2ab-204">Ayrıca, ilgili Üyeler genel kullanıma sunulduğunu bir düzen olarak `IAsyncEnumerable<T>` eşdeğerini destekleyecektir, ancak ayırmayı önleyememesi gereken yapı tabanlı uzantıları etkinleştirmek ve `MoveNextAsync` ve `DisposeAsync`dönüş türü olarak alternatif awaitables kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-204">And it will support the equivalent of `IAsyncEnumerable<T>` as a pattern if the relevant members are exposed publicly, falling back to using the interface directly if not, in order to enable struct-based extensions that avoid allocating as well as using alternative awaitables as the return type of `MoveNextAsync` and `DisposeAsync`.</span></span>

### <a name="syntax"></a><span data-ttu-id="5b2ab-205">Sözdizimi</span><span class="sxs-lookup"><span data-stu-id="5b2ab-205">Syntax</span></span>

<span data-ttu-id="5b2ab-206">Sözdizimini kullanarak:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-206">Using the syntax:</span></span>

```csharp
foreach (var i in enumerable)
```

<span data-ttu-id="5b2ab-207">C#, zaman uyumsuz numaralar için ilgili API 'Leri kullanıma sunsa bile, zaman uyumlu olmayan bir şekilde işlemeye `enumerable` devam eder (model ortaya koyar veya arabirimini uygulayarak), yalnızca zaman uyumlu API 'Leri göz önünde bulunduracaktır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-207">C# will continue to treat `enumerable` as a synchronous enumerable, such that even if it exposes the relevant APIs for async enumerables (exposing the pattern or implementing the interface), it will only consider the synchronous APIs.</span></span>

<span data-ttu-id="5b2ab-208">`foreach` yerine yalnızca zaman uyumsuz API 'Leri göz önünde bulundurmanız için, `await` aşağıdaki gibi eklenir:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-208">To force `foreach` to instead only consider the asynchronous APIs, `await` is inserted as follows:</span></span>

```csharp
await foreach (var i in enumerable)
```

<span data-ttu-id="5b2ab-209">Async veya Sync API 'Lerini kullanmayı destekleyecek bir sözdizimi sağlanmaz; geliştirici, kullanılan sözdizimine göre seçim yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-209">No syntax would be provided that would support using either the async or the sync APIs; the developer must choose based on the syntax used.</span></span>

<span data-ttu-id="5b2ab-210">Değerlendirilen atılan seçenekler:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-210">Discarded options considered:</span></span>
- <span data-ttu-id="5b2ab-211">_`foreach (var i in await enumerable)`_ : Bu, zaten geçerli bir söz dizimi ve anlamının değiştirilmesi bir son değişiklik olacaktır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-211">_`foreach (var i in await enumerable)`_: This is already valid syntax, and changing its meaning would be a breaking change.</span></span>  <span data-ttu-id="5b2ab-212">Bu, `enumerable``await`, zaman uyumlu bir şekilde tekrardan geri dönüp bundan sonra zaman uyumlu şekilde yineleyebileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-212">This means to `await` the `enumerable`, get back something synchronously iterable from it, and then synchronously iterate through that.</span></span>
- <span data-ttu-id="5b2ab-213">_`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)`, `foreach (await var i in enumerable)`_ : Bu tümü bir sonraki öğeyi bekletireceğiz, ancak foreach ' te yer alan başka bir bekler vardır, özellikle de numaralandırılabilir bir `IAsyncDisposable`ise, zaman uyumsuz elden çıkarma elden çıkarıl`await`acaktır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-213">_`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)`, `foreach (await var i in enumerable)`_: These all suggest that we're awaiting the next item, but there are other awaits involved in foreach, in particular if the enumerable is an `IAsyncDisposable`, we will be `await`'ing its async disposal.</span></span>  <span data-ttu-id="5b2ab-214">Bu await, her bir öğe için değil, Foreach 'in kapsamı olarak, `await` anahtar sözcüğünün `foreach` düzeyinde olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-214">That await is as the scope of the foreach rather than for each individual element, and thus the `await` keyword deserves to be at the `foreach` level.</span></span>  <span data-ttu-id="5b2ab-215">Ayrıca, `foreach` ile ilişkili olması, farklı bir terime sahip `foreach` (örneğin, "await foreach") anlatmak için bir yol sunar.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-215">Further, having it associated with the `foreach` gives us a way to describe the `foreach` with a different term, e.g. a "await foreach".</span></span>  <span data-ttu-id="5b2ab-216">Ancak daha da önemlisi, `using` söz dizimi ile aynı anda `foreach` söz konusu sözdizimini göz önünde bulundururken bir değer vardır ve `using (await ...)` zaten geçerli olan sözdizimidir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-216">But more importantly, there's value in considering `foreach` syntax at the same time as `using` syntax, so that they remain consistent with each other, and `using (await ...)` is already valid syntax.</span></span>
- _`foreach await (var i in enumerable)`_

<span data-ttu-id="5b2ab-217">Yine de göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-217">Still to consider:</span></span>
- <span data-ttu-id="5b2ab-218">Bugün `foreach`, bir Numaralandırıcı boyunca yineleme desteklemez.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-218">`foreach` today does not support iterating through an enumerator.</span></span>  <span data-ttu-id="5b2ab-219">`IAsyncEnumerator<T>`s 'nin etrafından daha yaygın olacağını umduk ve bu nedenle hem `IAsyncEnumerable<T>` hem de `IAsyncEnumerator<T>`ile `await foreach` desteklemek de mümkündür.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-219">We expect it will be more common to have `IAsyncEnumerator<T>`s handed around, and thus it's tempting to support `await foreach` with both `IAsyncEnumerable<T>` and `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="5b2ab-220">Ancak bu tür destek eklendikten sonra, `IAsyncEnumerator<T>` birinci sınıf bir vatandaşlık olup olmadığı ve numaralar 'e ek olarak Numaralandırıcılar üzerinde çalışan kombinatör 'nin aşırı yüklemelerinin olması gerekip gerekmediğini ortaya koyuyor musunuz?</span><span class="sxs-lookup"><span data-stu-id="5b2ab-220">But once we add such support, it introduces the question of whether `IAsyncEnumerator<T>` is a first-class citizen, and whether we need to have overloads of combinators that operate on enumerators in addition to enumerables?</span></span>    <span data-ttu-id="5b2ab-221">Yöntemleri numaralar yerine Numaralandırıcılar döndürecek şekilde teşvik etmek istiyor musunuz?</span><span class="sxs-lookup"><span data-stu-id="5b2ab-221">Do we want to encourage methods to return enumerators rather than enumerables?</span></span> <span data-ttu-id="5b2ab-222">Bunu tartışmak için devam etmemiz gerekir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-222">We should continue to discuss this.</span></span>  <span data-ttu-id="5b2ab-223">Bunu desteklemek istemediğimiz için karar verdiğimiz bir genişletme yöntemi eklemek `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` bir Numaralandırıcının hala `foreach`gibi olmasına imkan tanıdık.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-223">If we decide we don't want to support it, we might want to introduce an extension method `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` that would allow an enumerator to still be `foreach`'d.</span></span>  <span data-ttu-id="5b2ab-224">Bunu desteklemek istediğimiz karar verdiğimiz için, `await foreach` numaralandırıcı üzerinde `DisposeAsync` çağırmaktan sorumlu olup olmadığına ve yanıtın büyük olasılıkla "Hayır, elden çıkarılma sırasında denetim, `GetEnumerator`olarak adlandırılıyordu."</span><span class="sxs-lookup"><span data-stu-id="5b2ab-224">If we decide we do want to support it, we'll need to also decide on whether the `await foreach` would be responsible for calling `DisposeAsync` on the enumerator, and the answer is likely "no, control over disposal should be handled by whoever called `GetEnumerator`."</span></span>

### <a name="pattern-based-compilation"></a><span data-ttu-id="5b2ab-225">Model tabanlı derleme</span><span class="sxs-lookup"><span data-stu-id="5b2ab-225">Pattern-based Compilation</span></span>

<span data-ttu-id="5b2ab-226">Derleyici, varsa model tabanlı API 'lere bağlanır, bu da arabirimi kullanarak bunları tercih eder (model, örnek yöntemleriyle veya uzantı yöntemleriyle karşılanacaktır).</span><span class="sxs-lookup"><span data-stu-id="5b2ab-226">The compiler will bind to the pattern-based APIs if they exist, preferring those over using the interface (the pattern may be satisfied with instance methods or extension methods).</span></span>  <span data-ttu-id="5b2ab-227">Düzenin gereksinimleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-227">The requirements for the pattern are:</span></span>
- <span data-ttu-id="5b2ab-228">Numaralandırılabilir değer, bağımsız değişken olmadan çağrılabilecek ve ilgili düzene uyan bir Numaralandırıcı döndüren bir `GetAsyncEnumerator` yöntemi kullanıma sunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-228">The enumerable must expose a `GetAsyncEnumerator` method that may be called with no arguments and that returns an enumerator that meets the relevant pattern.</span></span>
- <span data-ttu-id="5b2ab-229">Numaralandırıcı, bağımsız değişken olmadan çağrılabilecek bir `MoveNextAsync` yöntemi kullanıma sunmalı ve `await`gelmiş olabilecek ve `GetResult()` bir `bool`döndüren bir değer döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-229">The enumerator must expose a `MoveNextAsync` method that may be called with no arguments and that returns something which may be `await`ed and whose `GetResult()` returns a `bool`.</span></span>
- <span data-ttu-id="5b2ab-230">Numaralandırıcı Ayrıca alıcı, numaralandırılan veri türünü temsil eden bir `T` döndüren `Current` özelliğini kullanıma sunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-230">The enumerator must also expose `Current` property whose getter returns a `T` representing the kind of data being enumerated.</span></span>
- <span data-ttu-id="5b2ab-231">Numaralandırıcı isteğe bağlı olarak, bağımsız değişken olmadan çağrılabilecek bir `DisposeAsync` yöntemi sunabilir ve `await`ve `GetResult()` `void`döndüren bir şey döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-231">The enumerator may optionally expose a `DisposeAsync` method that may be invoked with no arguments and that returns something that can be `await`ed and whose `GetResult()` returns `void`.</span></span>

<span data-ttu-id="5b2ab-232">Bu kod:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-232">This code:</span></span>

```csharp
var enumerable = ...;
await foreach (T item in enumerable)
{
   ...
}
```

<span data-ttu-id="5b2ab-233">, öğesinin eşdeğerine çevrilir:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-233">is translated to the equivalent of:</span></span>

```csharp
var enumerable = ...;
var enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
       T item = enumerator.Current;
       ...
    }
}
finally
{
    await enumerator.DisposeAsync(); // omitted, along with the try/finally, if the enumerator doesn't expose DisposeAsync
}
```

<span data-ttu-id="5b2ab-234">Yinelenebilir tür doğru düzende kullanıma sunulmazsa, arabirimler kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-234">If the iterated type doesn't expose the right pattern, the interfaces will be used.</span></span>

### <a name="configureawait"></a><span data-ttu-id="5b2ab-235">ConfigureAwait</span><span class="sxs-lookup"><span data-stu-id="5b2ab-235">ConfigureAwait</span></span>

<span data-ttu-id="5b2ab-236">Bu kalıp tabanlı derleme, bir `ConfigureAwait` genişletme yöntemi aracılığıyla tüm await üzerinde `ConfigureAwait` kullanılmasına izin verir:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-236">This pattern-based compilation will allow `ConfigureAwait` to be used on all of the awaits, via a `ConfigureAwait` extension method:</span></span>

```csharp
await foreach (T item in enumerable.ConfigureAwait(false))
{
   ...
}
```

<span data-ttu-id="5b2ab-237">Bu, büyük olasılıkla System. Threading. Tasks. Extensions. dll ' de .NET 'e ekleyebileceğiniz türlere göre yapılır:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-237">This will be based on types we'll add to .NET as well, likely to System.Threading.Tasks.Extensions.dll:</span></span>

```csharp
// Approximate implementation, omitting arg validation and the like
namespace System.Threading.Tasks
{
    public static class AsyncEnumerableExtensions
    {
        public static ConfiguredAsyncEnumerable<T> ConfigureAwait<T>(this IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext) =>
            new ConfiguredAsyncEnumerable<T>(enumerable, continueOnCapturedContext);

        public struct ConfiguredAsyncEnumerable<T>
        {
            private readonly IAsyncEnumerable<T> _enumerable;
            private readonly bool _continueOnCapturedContext;

            internal ConfiguredAsyncEnumerable(IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext)
            {
                _enumerable = enumerable;
                _continueOnCapturedContext = continueOnCapturedContext;
            }

            public ConfiguredAsyncEnumerator<T> GetAsyncEnumerator() =>
                new ConfiguredAsyncEnumerator<T>(_enumerable.GetAsyncEnumerator(), _continueOnCapturedContext);

            public struct Enumerator
            {
                private readonly IAsyncEnumerator<T> _enumerator;
                private readonly bool _continueOnCapturedContext;

                internal Enumerator(IAsyncEnumerator<T> enumerator, bool continueOnCapturedContext)
                {
                    _enumerator = enumerator;
                    _continueOnCapturedContext = continueOnCapturedContext;
                }

                public ConfiguredValueTaskAwaitable<bool> MoveNextAsync() =>
                    _enumerator.MoveNextAsync().ConfigureAwait(_continueOnCapturedContext);

                public T Current => _enumerator.Current;

                public ConfiguredValueTaskAwaitable DisposeAsync() =>
                    _enumerator.DisposeAsync().ConfigureAwait(_continueOnCapturedContext);
            }
        }
    }
}
```

<span data-ttu-id="5b2ab-238">Bu yaklaşımın, `ConfigureAwait` model tabanlı numaralar birlikte kullanılmasını etkinleştiremediğini unutmayın, ancak bu durumda `ConfigureAwait` yalnızca `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` bir uzantı olarak gösterildiğine ve rastgele bir zaman uyumlu şeylere uygulanamadığından emin olun. yalnızca görevlere uygulandığında anlamlı olduğundan (görevin devamlılık desteği ' nde uygulanan bir davranışı denetler) ve bu nedenle, awasever 'in görev olmadığı bir model kullanırken anlamlı değildir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-238">Note that this approach will not enable `ConfigureAwait` to be used with pattern-based enumerables, but then again it's already the case that the `ConfigureAwait` is only exposed as an extension on `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` and can't be applied to arbitrary awaitable things, as it only makes sense when applied to Tasks (it controls a behavior implemented in Task's continuation support), and thus doesn't make sense when using a pattern where the awaitable things may not be tasks.</span></span>  <span data-ttu-id="5b2ab-239">Daha fazla işlem döndüren herkes, bu Gelişmiş senaryolarda kendi özel davranışlarını sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-239">Anyone returning awaitable things can provide their own custom behavior in such advanced scenarios.</span></span>

<span data-ttu-id="5b2ab-240">(Kapsam veya derleme düzeyi `ConfigureAwait` çözümünü desteklemeye yönelik bir şekilde karşılaştığımız için bu gerekli değildir.)</span><span class="sxs-lookup"><span data-stu-id="5b2ab-240">(If we can come up with some way to support a scope- or assembly-level `ConfigureAwait` solution, then this won't be necessary.)</span></span>

## <a name="async-iterators"></a><span data-ttu-id="5b2ab-241">Zaman uyumsuz yineleyiciler</span><span class="sxs-lookup"><span data-stu-id="5b2ab-241">Async Iterators</span></span>

<span data-ttu-id="5b2ab-242">Dil/derleyici, `IAsyncEnumerable<T>`s ve `IAsyncEnumerator<T>`s oluşturmayı ve bunlara ek olarak desteklemeyi destekleyecektir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-242">The language / compiler will support producing `IAsyncEnumerable<T>`s and `IAsyncEnumerator<T>`s in addition to consuming them.</span></span> <span data-ttu-id="5b2ab-243">Günümüzde dil, şunun gibi bir yineleyici yazmayı destekler:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-243">Today the language supports writing an iterator like:</span></span>

```csharp
static IEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            Thread.Sleep(1000);
            yield return i;
        }
    }
    finally
    {
        Thread.Sleep(200);
        Console.WriteLine("finally");
    }
}
```

<span data-ttu-id="5b2ab-244">Ancak `await` bu yineleyicilerin gövdesinde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-244">but `await` can't be used in the body of these iterators.</span></span>  <span data-ttu-id="5b2ab-245">Bu desteği ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-245">We will add that support.</span></span>

### <a name="syntax"></a><span data-ttu-id="5b2ab-246">Sözdizimi</span><span class="sxs-lookup"><span data-stu-id="5b2ab-246">Syntax</span></span>

<span data-ttu-id="5b2ab-247">Yineleyiciler için mevcut dil desteği, bir `yield`s içerip içermediğini temel alarak yöntemin yineleyicisini belirtir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-247">The existing language support for iterators infers the iterator nature of the method based on whether it contains any `yield`s.</span></span>  <span data-ttu-id="5b2ab-248">Aynı zaman uyumsuz yineleyiciler için de doğru olacaktır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-248">The same will be true for async iterators.</span></span>  <span data-ttu-id="5b2ab-249">Bu zaman uyumsuz yineleyiciler, imzaya `async` eklenerek zaman uyumlu yineleyiciler tarafından parçalanacaktır ve aynı zamanda `IAsyncEnumerable<T>` ya da dönüş türü olarak `IAsyncEnumerator<T>` olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-249">Such async iterators will be demarcated and differentiated from synchronous iterators via adding `async` to the signature, and must then also have either `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` as its return type.</span></span>  <span data-ttu-id="5b2ab-250">Örneğin, yukarıdaki örnek aşağıdaki şekilde zaman uyumsuz bir yineleyici olarak yazılabilir:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-250">For example, the above example could be written as an async iterator as follows:</span></span>

```csharp
static async IAsyncEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            await Task.Delay(1000);
            yield return i;
        }
    }
    finally
    {
        await Task.Delay(200);
        Console.WriteLine("finally");
    }
}
```

<span data-ttu-id="5b2ab-251">Dikkate alınan alternatifler:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-251">Alternatives considered:</span></span>
- <span data-ttu-id="5b2ab-252">_İmzada `async` kullanmamakta_: `async` kullanmak, bu bağlamda `await` geçerli olup olmadığını anlamak için kullandığı için teknik olarak derleyici tarafından gerektirilir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-252">_Not using `async` in the signature_: Using `async` is likely technically required by the compiler, as it uses it to determine whether `await` is valid in that context.</span></span>  <span data-ttu-id="5b2ab-253">Ancak gerekli olmasa bile, `await` yalnızca `async`olarak işaretlenen yöntemlerde kullanılabileceğini ve tutarlılığın tutulması önemli bir durum olduğunu belirledik.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-253">But even if it's not required, we've established that `await` may only be used in methods marked as `async`, and it seems important to keep the consistency.</span></span>
- <span data-ttu-id="5b2ab-254">_`IAsyncEnumerable<T>`için özel oluşturucular etkinleştiriliyor_ : Bu, geleceğe bakabiliriz ancak makineler karmaşıktır ve zaman uyumlu ortaklarınıza yönelik olarak desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-254">_Enabling custom builders for `IAsyncEnumerable<T>`_:  That's something we could look at for the future, but the machinery is complicated and we don't support that for the synchronous counterparts.</span></span>
- <span data-ttu-id="5b2ab-255">_İmzada bir `iterator` anahtar sözcüğüne sahip olma_: zaman uyumsuz yineleyiciler İmzada `async iterator` kullanır ve `yield` yalnızca `iterator`dahil `async` metotlarda kullanılabilir. `iterator`, zaman uyumlu yineleyiciler üzerinde isteğe bağlı olarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-255">_Having an `iterator` keyword in the signature_: Async iterators would use `async iterator` in the signature, and `yield` could only be used in `async` methods that included `iterator`; `iterator` would then be made optional on synchronous iterators.</span></span>  <span data-ttu-id="5b2ab-256">Bakış açısına bağlı olarak, `yield` izin verilmediği ve yöntemin, kodun `yield` kullanıp kullanmadığına bağlı olarak derleyici üretimi yerine `IAsyncEnumerable<T>` türündeki örnekleri döndürmesinin yanı sıra, yöntemin imzasına göre çok açık hale getirme avantajına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-256">Depending on your perspective, this has the benefit of making it very clear by the signature of the method whether `yield` is allowed and whether the method is actually meant to return instances of type `IAsyncEnumerable<T>` rather than the compiler manufacturing one based on whether the code uses `yield` or not.</span></span>  <span data-ttu-id="5b2ab-257">Ancak zaman uyumlu yineleyiciler farklıdır, bu, bir tane gerektirmez ve bu yapılamaz.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-257">But it is different from synchronous iterators, which don't and can't be made to require one.</span></span>  <span data-ttu-id="5b2ab-258">Ayrıca, bazı geliştiriciler ek söz dizimini beğenmez.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-258">Plus some developers don't like the extra syntax.</span></span>  <span data-ttu-id="5b2ab-259">Bunu sıfırdan tasarladığımızda, bu gerekli olacaktır, ancak bu noktada, zaman uyumsuz yineleyiciler eşitleme yineleyiciler için kapalı tutulması çok daha fazla değer vardır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-259">If we were designing it from scratch, we'd probably make this required, but at this point there's much more value in keeping async iterators close to sync iterators.</span></span>

## <a name="linq"></a><span data-ttu-id="5b2ab-260">LINQ</span><span class="sxs-lookup"><span data-stu-id="5b2ab-260">LINQ</span></span>

<span data-ttu-id="5b2ab-261">`System.Linq.Enumerable` sınıfında, hepsi `IEnumerable<T>`açısından tüm çalışan, bu yöntemlerin on ~ 200 aşırı yüklemesi vardır. Bu kabul `IEnumerable<T>`, bazıları `IEnumerable<T>`ve her ikisi de birçok do.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-261">There are over ~200 overloads of methods on the `System.Linq.Enumerable` class, all of which work in terms of `IEnumerable<T>`; some of these accept `IEnumerable<T>`, some of them produce `IEnumerable<T>`, and many do both.</span></span>  <span data-ttu-id="5b2ab-262">`IAsyncEnumerable<T>` için LINQ desteğinin eklenmesi, başka bir ~ 200 için bu aşırı yüklemelerin tümünü çoğaltarak sıraya alabilir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-262">Adding LINQ support for `IAsyncEnumerable<T>` would likely entail duplicating all of these overloads for it, for another ~200.</span></span>  <span data-ttu-id="5b2ab-263">`IAsyncEnumerator<T>`, zaman uyumsuz dünyada `IEnumerator<T>`, zaman uyumlu dünyada bulunan tek başına bir varlık olarak daha yaygın olduğundan, `IAsyncEnumerator<T>`ile çalışan başka bir ~ 200 aşırı yüklemesi de gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-263">And since `IAsyncEnumerator<T>` is likely to be more common as a standalone entity in the asynchronous world than `IEnumerator<T>` is in the synchronous world, we could potentially need another ~200 overloads that work with `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="5b2ab-264">Ayrıca, çok sayıda aşırı yükleme, koşullar (örneğin, `Func<T, bool>`alan `Where`) ile ilgilidir ve hem zaman uyumlu hem de zaman uyumsuz koşullara (örn. `Func<T, bool>`ek olarak `Func<T, ValueTask<bool>>`) dayalı `IAsyncEnumerable<T>`tabanlı aşırı yüklemeler olması istenebilir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-264">Plus, a large number of the overloads deal with predicates (e.g. `Where` that takes a `Func<T, bool>`), and it may be desirable to have `IAsyncEnumerable<T>`-based overloads that deal with both synchronous and asynchronous predicates (e.g. `Func<T, ValueTask<bool>>` in addition to `Func<T, bool>`).</span></span>  <span data-ttu-id="5b2ab-265">Bu, şu an ~ 400 yeni aşırı yükleme için geçerli olmasa da, kabaca bir hesaplama, bu da bir ~ 200 aşırı yüklemesi, yani toplam ~ 600 yeni yöntem için geçerli olacaktır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-265">While this isn't applicable to all of the now ~400 new overloads, a rough calculation is that it'd be applicable to half, which means another ~200 overloads, for a total of ~600 new methods.</span></span>

<span data-ttu-id="5b2ab-266">Bu, kademelendirme sayıda API 'dir ve etkileşimli uzantılar (x) gibi uzantı kitaplıkları kabul edildiğinde daha da mümkün olur.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-266">That is a staggering number of APIs, with the potential for even more when extension libraries like Interactive Extensions (Ix) are considered.</span></span>  <span data-ttu-id="5b2ab-267">Ancak x, bunlardan birçoğu için zaten bir uygulama içeriyor ve bu çalışmayı yinelemek için harika bir neden görünmüyor; Bunun yerine, topluluğun x 'i iyileştirmesine yardımcı olur ve geliştiriciler `IAsyncEnumerable<T>`LINQ kullanmak istediğinizde BT için önerilir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-267">But Ix already has an implementation of many of these, and there doesn't seem to be a great reason to duplicate that work; we should instead help the community improve Ix and recommend it for when developers want to use LINQ with `IAsyncEnumerable<T>`.</span></span>

<span data-ttu-id="5b2ab-268">Sorgu anlama söz dizimi sorunu da vardır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-268">There is also the issue of query comprehension syntax.</span></span>  <span data-ttu-id="5b2ab-269">Sorgu anlama 'nın model tabanlı doğası, bazı işleçlerle "yalnızca çalışma" yapmasına izin verir, örneğin, x aşağıdaki yöntemleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-269">The pattern-based nature of query comprehensions would allow them to "just work" with some operators, e.g. if Ix provides the following methods:</span></span>

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> func);
public static IAsyncEnumerable<T> Where(this IAsyncEnumerable<T> source, Func<T, bool> func);
```

<span data-ttu-id="5b2ab-270">Bu C# kod, "yalnızca çalışır" olacaktır:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-270">then this C# code will "just work":</span></span>

```csharp
IAsyncEnumerable<int> enumerable = ...;
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select item * 2;
```

<span data-ttu-id="5b2ab-271">Ancak, yan tümcelerinde `await` kullanımını destekleyen sorgu kavrama sözdizimi yoktur, yani x eklenirse, örneğin:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-271">However, there is no query comprehension syntax that supports using `await` in the clauses, so if Ix added, for example:</span></span>

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, ValueTask<TResult>> func);
```

<span data-ttu-id="5b2ab-272">Bu durumda "yalnızca çalışma" olur:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-272">then this would "just work":</span></span>

```csharp
IAsyncEnumerable<string> result = from url in urls
                                  where item % 2 == 0
                                  select SomeAsyncMethod(item);

async ValueTask<int> SomeAsyncMethod(int item)
{
    await Task.Yield();
    return item * 2;
}
```

<span data-ttu-id="5b2ab-273">Ancak, `select` yan tümcesinde `await` satır içi olarak yazmak için bir yol yoktur.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-273">but there'd be no way to write it with the `await` inline in the `select` clause.</span></span>  <span data-ttu-id="5b2ab-274">Ayrı bir çaba olarak, dile `async { ... }` ifadeleri ekleme hakkında görünebiliriz. bu noktada, sorgu anlama 'da kullanılmasına izin verdiğimiz ve yukarıdaki şöyle yazılabilir:</span><span class="sxs-lookup"><span data-stu-id="5b2ab-274">As a separate effort, we could look into adding `async { ... }` expressions to the language, at which point we could allow them to be used in query comprehensions and the above could instead be written as:</span></span>

```csharp
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select async
                               {
                                   await Task.Yield();
                                   return item * 2;
                               };
```

<span data-ttu-id="5b2ab-275">ya da `async from`destekleme gibi `await` doğrudan ifadelerde kullanılmasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-275">or to enabling `await` to be used directly in expressions, such as by supporting `async from`.</span></span>  <span data-ttu-id="5b2ab-276">Bununla birlikte, burada bir tasarımın bir yolu veya diğerini, bu özellik kümesinin geri kalanını etkiler ve bu, özellikle de bir çok yüksek değerli şey olmak üzere hemen yatırım yapacak bir şey değildir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-276">However, it's unlikely a design here would impact the rest of the feature set one way or the other, and this isn't a particularly high-value thing to invest in right now, so the proposal is to do nothing additional here right now.</span></span>

## <a name="integration-with-other-asynchronous-frameworks"></a><span data-ttu-id="5b2ab-277">Diğer zaman uyumsuz çerçevelerle tümleştirme</span><span class="sxs-lookup"><span data-stu-id="5b2ab-277">Integration with other asynchronous frameworks</span></span>

<span data-ttu-id="5b2ab-278">`IObservable<T>` ve diğer zaman uyumsuz çerçeveler (örneğin, reaktif akışlar) ile tümleştirme, dil düzeyi yerine kitaplık düzeyinde yapılır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-278">Integration with `IObservable<T>` and other asynchronous frameworks (e.g. reactive streams) would be done at the library level rather than at the language level.</span></span>  <span data-ttu-id="5b2ab-279">Örneğin, bir `IAsyncEnumerator<T>` tüm veriler `IObserver<T>` bir `await foreach`' de Numaralandırıcının üzerinde yer alan ve verileri gözlemci 'e `OnNext`bir `AsObservable<T>` bir genişletme yöntemi olabilerek yayımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-279">For example, all of the data from an `IAsyncEnumerator<T>` can be published to an `IObserver<T>` simply by `await foreach`'ing over the enumerator and `OnNext`'ing the data to the observer, so an `AsObservable<T>` extension method is possible.</span></span>  <span data-ttu-id="5b2ab-280">Bir `await foreach` `IObservable<T>` kullanmak için verilerin arabelleğe alınması gerekir (önceki öğe işlenmeye devam ederken başka bir öğenin itilmesi durumunda), ancak bir `IObservable<T>` `IAsyncEnumerator<T>`ile çekmesini sağlamak için bu tür bir çekme bağdaştırıcısı kolayca uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-280">Consuming an `IObservable<T>` in a `await foreach` requires buffering the data (in case another item is pushed while the previous item is still being processing), but such a push-pull adapter can easily be implemented to enable an `IObservable<T>` to be pulled from with an `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="5b2ab-281">Benzerlerini.  RX/x, bu tür uygulamaların prototiplerini zaten sağlıyor ve https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels gibi kitaplıklar çeşitli türlerde arabelleğe alma veri yapıları sunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-281">Etc.  Rx/Ix already provide prototypes of such implementations, and libraries like https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels provide various kinds of buffering data structures.</span></span>  <span data-ttu-id="5b2ab-282">Bu aşamada dilin dahil olmaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="5b2ab-282">The language need not be involved at this stage.</span></span>
