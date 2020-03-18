---
ms.openlocfilehash: 922353d043653ddb651534a01f3fb98f85cd756e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484484"
---
# <a name="readonly-locals-and-parameters"></a><span data-ttu-id="299b7-101">salt okunur Yereller ve parametreler</span><span class="sxs-lookup"><span data-stu-id="299b7-101">readonly locals and parameters</span></span>

* <span data-ttu-id="299b7-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="299b7-102">[x] Proposed</span></span>
* <span data-ttu-id="299b7-103">[] Prototipi</span><span class="sxs-lookup"><span data-stu-id="299b7-103">[ ] Prototype</span></span>
* <span data-ttu-id="299b7-104">[] Uygulama</span><span class="sxs-lookup"><span data-stu-id="299b7-104">[ ] Implementation</span></span>
* <span data-ttu-id="299b7-105">[] Belirtimi</span><span class="sxs-lookup"><span data-stu-id="299b7-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="299b7-106">Özet</span><span class="sxs-lookup"><span data-stu-id="299b7-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="299b7-107">Bu Yereller ve parametrelerin yüzeysel bir şekilde bırakılmasını engellemek için Yereller ve parametrelerin ReadOnly olarak açıklanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="299b7-107">Allow locals and parameters to be annotated as readonly in order to prevent shallow mutation of those locals and parameters.</span></span>

## <a name="motivation"></a><span data-ttu-id="299b7-108">Amacı</span><span class="sxs-lookup"><span data-stu-id="299b7-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="299b7-109">Bugün, `readonly` anahtar sözcüğü alanlara uygulanabilir; Bu, bir alanın yalnızca oluşturma sırasında (statik bir alan olması durumunda statik oluşturma veya bir örnek alanı durumunda örnek oluşturma) yazılmasına olanak sağlayan, aksi durumda, yanlışlıkla değiştirilmemesi gereken durum üzerine yazarak, geliştiricilerin hata yaşamamasını sağlamaya yönelik etkiye sahiptir.</span><span class="sxs-lookup"><span data-stu-id="299b7-109">Today, the `readonly` keyword can be applied to fields; this has the effect of ensuring that a field can only be written to during construction (static construction in the case of a static field, or instance construction in the case of an instance field), which helps developers avoid mistakes by accidentally overwriting state which should not be modified.</span></span> <span data-ttu-id="299b7-110">Ancak alanlar, geliştiricilerin değer değiştirici olmamasını sağlamak istedikleri tek yer değildir.</span><span class="sxs-lookup"><span data-stu-id="299b7-110">But fields aren't the only places developers want to ensure that values aren't mutated.</span></span> <span data-ttu-id="299b7-111">Özellikle, geçici durumu depolamak için yerel bir değişken oluşturmak yaygındır ve yanlışlıkla bu geçici durumu güncellemek, özellikle de bu tür "Yereller" lambda içinde yakalandıklarında, bu tür yükseltilmemiş alanlarını `readonly`olarak İşaretlemede bugün bir yol yoktur.</span><span class="sxs-lookup"><span data-stu-id="299b7-111">In particular, it's common to create a local variable to store temporary state, and accidentally updating that temporary state can result in erroneous calculations and other such bugs, especially when such "locals" are captured in lambdas, at which point they are lifted to fields, but there's no way today to mark such lifted fields as `readonly`.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="299b7-112">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="299b7-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="299b7-113">Yalnızca bildirim sırasında ayarlandıklarından (örneğin, bir ' foreach ' döngüsünde yineleme değişkeni veya ' Using ' bloğunda kullanılan değişken), ancak şu anda bir geliştiricinin C# diğer yerelleri `readonly`olarak işaretleyebilme özelliği olan ' de, yerel öğeler `readonly` olarak Annotatable olacaktır.</span><span class="sxs-lookup"><span data-stu-id="299b7-113">Locals will be annotatable as `readonly` as well, with the compiler ensuring that they're only set at the time of declaration (certain locals in C# are already implicitly readonly, such as the iteration variable in a 'foreach' loop or the used variable in a 'using' block, but currently a developer has no ability to mark other locals as `readonly`).</span></span> <span data-ttu-id="299b7-114">Bu `readonly` Yereller bir başlatıcıya sahip olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="299b7-114">Such `readonly` locals must have an initializer:</span></span>

```csharp
readonly long maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

<span data-ttu-id="299b7-115">`readonly var`için de toplu olarak, var olan bağlamsal anahtar sözcük `let` kullanılabilir, örn.</span><span class="sxs-lookup"><span data-stu-id="299b7-115">And as shorthand for `readonly var`, the existing contextual keyword `let` may be used, e.g.</span></span>

```csharp
let maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

<span data-ttu-id="299b7-116">Başlatıcıya ait olabilecek özel kısıtlamalar yoktur ve şu anda Yereller için bir başlatıcı olarak geçerli olan herhangi bir şey olabilir. Örneğin,</span><span class="sxs-lookup"><span data-stu-id="299b7-116">There are no special constraints for what the initializer can be, and can be anything currently valid as an initializer for locals, e.g.</span></span>

```csharp
readonly T data = arg1 ?? arg2;
```

<span data-ttu-id="299b7-117">Yereller üzerindeki `readonly`, Lambdalar ve kapanışlarla çalışırken özellikle değerlidir.</span><span class="sxs-lookup"><span data-stu-id="299b7-117">`readonly` on locals is particularly valuable when working with lambdas and closures.</span></span> <span data-ttu-id="299b7-118">Anonim bir yöntem veya lambda, kapsayan kapsamdan yerel duruma eriştiğinde, bu durum derleyici tarafından bir "görüntü sınıfı" ile temsil edilen bir kapatılmak üzere yakalanır.</span><span class="sxs-lookup"><span data-stu-id="299b7-118">When an anonymous method or lambda accesses local state from the enclosing scope, that state is captured into a closure by the compiler, which is represented by a "display class."</span></span>  <span data-ttu-id="299b7-119">Yakalanan her bir "yerel" Bu sınıftaki bir alandır, ancak derleyici sizin adınıza bu alanı oluşturduğundan, lambda 'nin "yerel" e (aslında yerel değil, sonuçta elde edilen MSIL 'de değil, gerçekten bir yerel olmadığı için) yanlışlıkla yazılmasını engellemek için `readonly` olarak not ekleme şansınız yoktur.</span><span class="sxs-lookup"><span data-stu-id="299b7-119">Each "local" that's captured is a field in this class, yet because the compiler is generating this field on your behalf, you have no opportunity to annotate it as `readonly` in order to prevent the lambda from erroneously writing to the "local" (in quotes because it's really not a local, at least not in the resulting MSIL).</span></span> <span data-ttu-id="299b7-120">`readonly` Yereller sayesinde, derleyici, lambda 'nin yerel konuma yazmasını önleyebilir, bu da özellikle de hatalı bir yazma tehlikeli, ancak nadir ve çok fazla eşzamanlılık hatasına neden olabilecek çoklu iş parçacığı oluşturma senaryolarıyla ilgili senaryolarda değerlidir.</span><span class="sxs-lookup"><span data-stu-id="299b7-120">With `readonly` locals, the compiler can prevent the lambda from writing to local, which is particularly valuable in scenarios involving multithreading where an erroneous write could result in a dangerous but rare and hard-to-find concurrency bug.</span></span>

```csharp
readonly long index = ...;
Parallel.ForEach(data, item => {
    T element = item[index];
    index = 0; // Error: can't assign to readonly locals outside of declaration
});
```

<span data-ttu-id="299b7-121">Özel bir yerel form olarak parametreler de `readonly`olarak Annotatable olacaktır.</span><span class="sxs-lookup"><span data-stu-id="299b7-121">As a special form of local, parameters will also be annotatable as `readonly`.</span></span> <span data-ttu-id="299b7-122">Bu, yöntemin çağıranın parametreye geçebileceği herhangi bir etkiye sahip değildir (yalnızca bir `readonly` alanında depolanabilecek değerler için hiçbir kısıtlama olmadığı gibi), derleyici, koddan sonra kodun `readonly` bir parametreye yazmasını yasakladığından, yöntemin gövdesi parametreye yazılmak anlamına gelir, bu da yöntemin gövdesinin parametreye yazılması yasaktır.</span><span class="sxs-lookup"><span data-stu-id="299b7-122">This would have no effect on what the caller of the method is able to pass to the parameter (just as there's no constraint on what values may be stored into a `readonly` field), but as with any `readonly` local, the compiler would prohibit code from writing to the parameter after declaration, which means the body of the method is prohibited from writing to the parameter.</span></span>

```csharp
public void Update(readonly int index = 0) // Default values are ok though not required
{
    ...
    index = 0; // Error: can't assign to readonly parameters
    ...
}
```

<span data-ttu-id="299b7-123">`readonly` parametreler, bu yöntem için derleyici tarafından yayılan imza/meta verileri etkilemez ve derleyicinin metodun gövdesinin derlemesini nasıl işleyeceğini etkiler.</span><span class="sxs-lookup"><span data-stu-id="299b7-123">`readonly` parameters do not affect the signature/metadata emitted by the compiler for that method, and simply affect how the compiler handles the compilation of the method's body.</span></span> <span data-ttu-id="299b7-124">Bu nedenle, örneğin, bir taban sanal yönteminin bir `readonly` parametresi olabilir ve bu parametre bir geçersiz kılma içinde yazılabilir.</span><span class="sxs-lookup"><span data-stu-id="299b7-124">Thus, for example, a base virtual method could have a `readonly` parameter, and that parameter could be writable in an override.</span></span>

<span data-ttu-id="299b7-125">Alanlarda olduğu gibi, Yereller ve parametreler için `readonly` basit bir deyişle, depolama konumunu etkileyen, ancak nesne grafiğini etkilemeden geçişli bir değer kalmaz.</span><span class="sxs-lookup"><span data-stu-id="299b7-125">As with fields, `readonly` for locals and parameters is shallow, affecting the storage location but not transitively affecting the object graph.</span></span> <span data-ttu-id="299b7-126">Bununla birlikte, Ayrıca, bir `readonly` yerel/parametre yapısı üzerinde bir yöntemi çağırmak, aslında yapının bir kopyasını yapar ve `this`iç muatmasını önlemek için kopyada yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="299b7-126">However, also as with fields, calling a method on a `readonly` local/parameter struct will actually make a copy of the struct and call the method on the copy, in order to avoid internal mutation of `this`.</span></span>

<span data-ttu-id="299b7-127">`readonly` Yereller ve parametreler, `ref readonly` desteklenmediği sürece `ref` veya `out` bağımsız değişken olarak geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="299b7-127">`readonly` locals and parameters can't be passed as `ref` or `out` arguments, unless/until `ref readonly` is also supported.</span></span>

## <a name="alternatives"></a><span data-ttu-id="299b7-128">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="299b7-128">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="299b7-129">`val` `let`için alternatif bir toplu değer olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="299b7-129">`val` could be used as an alternative shorthand to `let`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="299b7-130">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="299b7-130">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="299b7-131">`readonly ref` / `ref readonly` / `readonly ref readonly`: Bu tekliften ayrı olarak `ref readonly` nasıl işleyeceğinizi gösteren sorudan ayrıldım.</span><span class="sxs-lookup"><span data-stu-id="299b7-131">`readonly ref` / `ref readonly` / `readonly ref readonly`: I've left the question of how to handle `ref readonly` as separate from this proposal.</span></span>
- <span data-ttu-id="299b7-132">Bu teklif, salt okunur yapılar/Sabit türler oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="299b7-132">This proposal does not tackle readonly structs / immutable types.</span></span> <span data-ttu-id="299b7-133">Bu, ayrı bir teklif için bırakılır.</span><span class="sxs-lookup"><span data-stu-id="299b7-133">That is left for a separate proposal.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="299b7-134">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="299b7-134">Design meetings</span></span>

- <span data-ttu-id="299b7-135">21 Ocak 2015 ' de kısaca ele alınmaktadır (<https://github.com/dotnet/roslyn/issues/98>)</span><span class="sxs-lookup"><span data-stu-id="299b7-135">Briefly discussed on Jan 21, 2015 (<https://github.com/dotnet/roslyn/issues/98>)</span></span>
