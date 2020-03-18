---
ms.openlocfilehash: 39fb0aab5e8bb0d422f25fd2e92ab3d8256d3f59
ms.sourcegitcommit: b8f1103eb686c5d82e294837c9386d9b667da292
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2019
ms.locfileid: "79485114"
---
# <a name="readonly-references"></a><span data-ttu-id="ef06a-101">Salt okunur başvurular</span><span class="sxs-lookup"><span data-stu-id="ef06a-101">Readonly references</span></span>

* <span data-ttu-id="ef06a-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="ef06a-102">[x] Proposed</span></span>
* <span data-ttu-id="ef06a-103">[x] prototipi</span><span class="sxs-lookup"><span data-stu-id="ef06a-103">[x] Prototype</span></span>
* <span data-ttu-id="ef06a-104">[x] uygulama: başlatıldı</span><span class="sxs-lookup"><span data-stu-id="ef06a-104">[x] Implementation: Started</span></span>
* <span data-ttu-id="ef06a-105">[] Belirtimi: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="ef06a-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="ef06a-106">Özet</span><span class="sxs-lookup"><span data-stu-id="ef06a-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="ef06a-107">"Salt okunur başvurular" özelliği aslında değişkenleri başvuruya göre geçirme ve değişiklikler için verileri açığa çıkarmadan faydalanma verimliliğini kullanan bir özellik grubudur:</span><span class="sxs-lookup"><span data-stu-id="ef06a-107">The "readonly references" feature is actually a group of features that leverage the efficiency of passing variables by reference, but without exposing the data to modifications:</span></span>  
- <span data-ttu-id="ef06a-108">`in` parametreleri</span><span class="sxs-lookup"><span data-stu-id="ef06a-108">`in` parameters</span></span>
- <span data-ttu-id="ef06a-109">`ref readonly` döndürür</span><span class="sxs-lookup"><span data-stu-id="ef06a-109">`ref readonly` returns</span></span>
- <span data-ttu-id="ef06a-110">`readonly` yapılar</span><span class="sxs-lookup"><span data-stu-id="ef06a-110">`readonly` structs</span></span>
- <span data-ttu-id="ef06a-111">`ref`/`in` uzantısı yöntemleri</span><span class="sxs-lookup"><span data-stu-id="ef06a-111">`ref`/`in` extension methods</span></span>
- <span data-ttu-id="ef06a-112">`ref readonly` Yereller</span><span class="sxs-lookup"><span data-stu-id="ef06a-112">`ref readonly` locals</span></span>
- <span data-ttu-id="ef06a-113">Koşullu ifadeleri `ref`</span><span class="sxs-lookup"><span data-stu-id="ef06a-113">`ref` conditional expressions</span></span>

## <a name="passing-arguments-as-readonly-references"></a><span data-ttu-id="ef06a-114">Bağımsız değişkenleri ReadOnly başvuruları olarak geçirme.</span><span class="sxs-lookup"><span data-stu-id="ef06a-114">Passing arguments as readonly references.</span></span>

<span data-ttu-id="ef06a-115">Bu konu başlığı altında, çok sayıda ayrıntıya geçmeden salt okunur parametrelere özel bir durum olarak https://github.com/dotnet/roslyn/issues/115 dokunan mevcut bir teklif vardır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-115">There is an existing proposal that touches this topic https://github.com/dotnet/roslyn/issues/115 as a special case of readonly parameters without going into many details.</span></span>
<span data-ttu-id="ef06a-116">Burada yalnızca kendi fikrinin çok yeni olmayacağını bildirmek istiyorum.</span><span class="sxs-lookup"><span data-stu-id="ef06a-116">Here I just want to acknowledge that the idea by itself is not very new.</span></span>

### <a name="motivation"></a><span data-ttu-id="ef06a-117">Amacı</span><span class="sxs-lookup"><span data-stu-id="ef06a-117">Motivation</span></span>

<span data-ttu-id="ef06a-118">Bu özellikten C# önce, hiçbir değişiklik yapılmadan, salt okunur amaçlar için yapı değişkenlerini metot çağrılarına geçirmek zorunda bir ifade etmenin etkili bir yolu yoktu.</span><span class="sxs-lookup"><span data-stu-id="ef06a-118">Prior to this feature C# did not have an efficient way of expressing a desire to pass struct variables into method calls for readonly purposes with no intention of modifying.</span></span> <span data-ttu-id="ef06a-119">Normal değere sahip bağımsız değişken geçirme, gereksiz maliyetler ekleyen kopyalamayı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-119">Regular by-value argument passing implies copying, which adds unnecessary costs.</span></span>  <span data-ttu-id="ef06a-120">Bu, kullanıcıların-ref bağımsız değişkenini kullanmasını ve verilerin aranan tarafından atlanması gerektiğini belirtmek için açıklamaları/belgeleri geçen ve kullanıcılara yönlendiren bir belge kullanır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-120">That drives users to use by-ref argument passing and rely on comments/documentation to indicate that the data is not supposed to be mutated by the callee.</span></span> <span data-ttu-id="ef06a-121">Birçok nedenden dolayı iyi bir çözüm değildir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-121">It is not a good solution for many reasons.</span></span>  
<span data-ttu-id="ef06a-122">Örnek olarak, performans konuları nedeniyle, [XNA](https://msdn.microsoft.com/library/bb194944.aspx) gibi grafik kitaplıklarında çok sayıda vektör/matris matematik işleçleri başvuru işlenenleri olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-122">The examples are numerous - vector/matrix math operators in graphics libraries like [XNA](https://msdn.microsoft.com/library/bb194944.aspx) are known to have ref operands purely because of performance considerations.</span></span> <span data-ttu-id="ef06a-123">Roslyn derleyicisinde, ayırmaların önüne geçmek için yapılar kullanan ve sonra maliyetleri kopyalamayı önlemek için bunları başvuruya göre ileten kod vardır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-123">There is code in Roslyn compiler itself that uses structs to avoid allocations and then passes them by reference to avoid copying costs.</span></span>

### <a name="solution-in-parameters"></a><span data-ttu-id="ef06a-124">Çözüm (`in` Parameters)</span><span class="sxs-lookup"><span data-stu-id="ef06a-124">Solution (`in` parameters)</span></span>

<span data-ttu-id="ef06a-125">`out` parametrelerine benzer şekilde, `in` parametreler, çağrılan ek garantilere sahip yönetilen başvurular olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-125">Similarly to the `out` parameters, `in` parameters are passed as managed references with additional guarantees from the callee.</span></span>  
<span data-ttu-id="ef06a-126">Diğer herhangi bir kullanılmadan önce çağrılan tarafından atanması _gereken_ `out` parametrelerinden farklı olarak, `in` parametreleri, aranan tarafından atanamaz.</span><span class="sxs-lookup"><span data-stu-id="ef06a-126">Unlike `out` parameters which _must_ be assigned by the callee before any other use, `in` parameters cannot be assigned by the callee at all.</span></span>

<span data-ttu-id="ef06a-127">Sonuç olarak `in` parametreler, çağrılan dolaylı bağımsız değişken, çağıran tarafından barbeklere bağımsız değişkenler ortaya çıkarmadan geçen verimlilik için izin verir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-127">As a result `in` parameters allow for effectiveness of indirect argument passing without exposing arguments to mutations by the callee.</span></span>

### <a name="declaring-in-parameters"></a><span data-ttu-id="ef06a-128">`in` parametrelerini bildirme</span><span class="sxs-lookup"><span data-stu-id="ef06a-128">Declaring `in` parameters</span></span>

<span data-ttu-id="ef06a-129">`in` parametreler, `in` anahtar sözcüğü parametre imzasında bir değiştirici olarak kullanılarak bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-129">`in` parameters are declared by using `in` keyword as a modifier in the parameter signature.</span></span>

<span data-ttu-id="ef06a-130">Tüm amaçlar için `in` parametresi bir `readonly` değişken olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-130">For all purposes the `in` parameter is treated as a `readonly` variable.</span></span> <span data-ttu-id="ef06a-131">Yöntemi içinde `in` parametrelerinin kullanımı ile ilgili kısıtlamaların çoğu `readonly` alanlarla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-131">Most of the restrictions on the use of `in` parameters inside the method are the same as with `readonly` fields.</span></span>

> <span data-ttu-id="ef06a-132">Gerçekten bir `in` parametresi bir `readonly` alanı temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-132">Indeed an `in` parameter may represent a `readonly` field.</span></span> <span data-ttu-id="ef06a-133">Kısıtlamaların benzerliği bir rastlantı değildir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-133">Similarity of restrictions is not a coincidence.</span></span>

<span data-ttu-id="ef06a-134">Örneğin, bir yapı türüne sahip bir `in` parametresinin alanları, `readonly` değişkenleri olarak özyinelemeli olarak sınıflandırıldı.</span><span class="sxs-lookup"><span data-stu-id="ef06a-134">For example fields of an `in` parameter which has a struct type are all recursively classified as `readonly` variables .</span></span>

```csharp
static Vector3 Add (in Vector3 v1, in Vector3 v2)
{
    // not OK!!
    v1 = default(Vector3);

    // not OK!!
    v1.X = 0;

    // not OK!!
    foo(ref v1.X);

    // OK
    return new Vector3(v1.X + v2.X, v1.Y + v2.Y, v1.Z + v2.Z);
}
```

- <span data-ttu-id="ef06a-135">`in` parametrelere, normal ByVal parametrelerine izin verilen her yerde izin verilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-135">`in` parameters are allowed anywhere where ordinary byval parameters are allowed.</span></span> <span data-ttu-id="ef06a-136">Bu, Dizin oluşturucular, işleçler (dönüşümler dahil), temsilciler, Lambdalar, yerel işlevler içerir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-136">This includes indexers, operators (including conversions), delegates, lambdas, local functions.</span></span>

> ```csharp
>  (in int x) => x                                                     // lambda expression  
>  TValue this[in TKey index];                                         // indexer
>  public static Vector3 operator +(in Vector3 x, in Vector3 y) => ... // operator
>  ```

- <span data-ttu-id="ef06a-137">`in` `out` veya `out` birlikte birleştirmediğinden hiçbir şeyle birlikte kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="ef06a-137">`in` is not allowed in combination with `out` or with anything that `out` does not combine with.</span></span>

- <span data-ttu-id="ef06a-138">`in` farklılıkları /`out``ref`/aşırı yüklenmesine izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="ef06a-138">It is not permitted to overload on `ref`/`out`/`in` differences.</span></span>

- <span data-ttu-id="ef06a-139">Normal ByVal üzerinde aşırı yükleme ve `in` farklılıklarına izin verilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-139">It is permitted to overload on ordinary byval and `in` differences.</span></span>

- <span data-ttu-id="ef06a-140">OHI (aşırı yükleme, gizleme, uygulama) amacı için `in` `out` parametreye benzer şekilde davranır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-140">For the purpose of OHI (Overloading, Hiding, Implementing), `in` behaves similarly to an `out` parameter.</span></span>
<span data-ttu-id="ef06a-141">Aynı kuralların hepsi geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-141">All the same rules apply.</span></span>
<span data-ttu-id="ef06a-142">Örneğin, geçersiz kılma yöntemi, kimlik dönüştürülebilir bir türün `in` parametreleriyle `in` parametrelerle eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-142">For example the overriding method will have to match `in` parameters with `in` parameters of an identity-convertible type.</span></span>

- <span data-ttu-id="ef06a-143">Temsilci/Lambda/Yöntem grubu dönüştürmelerinde `in`, bir `out` parametresine benzer şekilde davranır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-143">For the purpose of delegate/lambda/method group conversions, `in` behaves similarly to an `out` parameter.</span></span>
<span data-ttu-id="ef06a-144">Lambdalar ve geçerli yöntem grubu dönüştürme adayları, bir kimlik dönüştürülebilir türün `in` parametreleriyle hedef temsilcinin `in` parametreleriyle eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-144">Lambdas and applicable method group conversion candidates will have to match `in` parameters of the target delegate with `in` parameters of an identity-convertible type.</span></span>

- <span data-ttu-id="ef06a-145">Genel varyans amacına yönelik `in` parametreler değişken değildir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-145">For the purpose of generic variance, `in` parameters are nonvariant.</span></span>

> <span data-ttu-id="ef06a-146">NOTE: başvuru veya temel türler içeren `in` parametrelerinde uyarı yok.</span><span class="sxs-lookup"><span data-stu-id="ef06a-146">NOTE: There are no warnings on `in` parameters that have reference or primitives types.</span></span>
<span data-ttu-id="ef06a-147">Genel olarak daha az olabilir, ancak bazı durumlarda, kullanıcıların temelleri `in`olarak geçirmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-147">It may be pointless in general, but in some cases user must/want to pass primitives as `in`.</span></span> <span data-ttu-id="ef06a-148">Örnekler-`T` `int`ya da `Volatile.Read(in int location)` gibi yöntemlere sahip olduğunda `Method(in T param)` gibi genel bir yöntemi geçersiz kılma.</span><span class="sxs-lookup"><span data-stu-id="ef06a-148">Examples - overriding a generic method like `Method(in T param)` when `T` was substituted to be `int`, or when having methods like `Volatile.Read(in int location)`</span></span>
>
> <span data-ttu-id="ef06a-149">`in` parametrelerinin verimsiz kullanımı konusunda uyaran bir çözümleyici Conceivable, ancak bu tür analizler için kurallar bir dil belirtiminin parçası olmak için çok belirsiz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-149">It is conceivable to have an analyzer that warns in cases of inefficient use of `in` parameters, but the rules for such analysis would be too fuzzy to be a part of a language specification.</span></span>

### <a name="use-of-in-at-call-sites-in-arguments"></a><span data-ttu-id="ef06a-150">Çağıran sitelerde `in` kullanımı.</span><span class="sxs-lookup"><span data-stu-id="ef06a-150">Use of `in` at call sites.</span></span> <span data-ttu-id="ef06a-151">(`in` bağımsız değişkenler)</span><span class="sxs-lookup"><span data-stu-id="ef06a-151">(`in` arguments)</span></span>

<span data-ttu-id="ef06a-152">Bağımsız değişkenleri `in` parametrelere geçirmek için iki yol vardır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-152">There are two ways to pass arguments to `in` parameters.</span></span>

#### <a name="in-arguments-can-match-in-parameters"></a><span data-ttu-id="ef06a-153">`in` bağımsız değişkenler `in` parametreleriyle eşleştirebilir:</span><span class="sxs-lookup"><span data-stu-id="ef06a-153">`in` arguments can match `in` parameters:</span></span>

<span data-ttu-id="ef06a-154">Çağrı sitesinde `in` değiştiricisi olan bir bağımsız değişken `in` parametreleriyle eşleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-154">An argument with an `in` modifier at the call site can match `in` parameters.</span></span>

```csharp
int x = 1;

void M1<T>(in T x)
{
  // . . .
}

var x = M1(in x);  // in argument to a method

class D
{
    public string this[in Guid index];
}

D dictionary = . . . ;
var y = dictionary[in Guid.Empty]; // in argument to an indexer
```

- <span data-ttu-id="ef06a-155">`in` bağımsız değişkeni _okunabilir_ bir lvalue (\*) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-155">`in` argument must be a _readable_ LValue(\*).</span></span>
<span data-ttu-id="ef06a-156">Örnek: `M1(in 42)` geçersiz</span><span class="sxs-lookup"><span data-stu-id="ef06a-156">Example: `M1(in 42)` is invalid</span></span>

> <span data-ttu-id="ef06a-157">(\*) [Lvalue/rvalue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) kavramı diller arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-157">(\*) The notion of [LValue/RValue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) vary between languages.</span></span>  
<span data-ttu-id="ef06a-158">Burada, LValue tarafından doğrudan başvurulabilen bir konumu temsil eden bir ifade geliyor.</span><span class="sxs-lookup"><span data-stu-id="ef06a-158">Here, by LValue I mean an expression that represent a location that can be referred to directly.</span></span>
<span data-ttu-id="ef06a-159">Ve RValue, kendi kendine kalıcı olmayan geçici bir sonuç veren bir ifade anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-159">And RValue means an expression that yields a temporary result which does not persist on its own.</span></span>  

- <span data-ttu-id="ef06a-160">Özellikle `readonly` alanları, `in` parametreleri veya diğer resmi `readonly` değişkenlerini `in` bağımsız değişken olarak geçirmek için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-160">In particular it is valid to pass `readonly` fields, `in` parameters or other formally `readonly` variables as `in` arguments.</span></span>
<span data-ttu-id="ef06a-161">Örnek: `dictionary[in Guid.Empty]` geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-161">Example: `dictionary[in Guid.Empty]` is legal.</span></span> <span data-ttu-id="ef06a-162">`Guid.Empty` statik salt okunur bir alandır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-162">`Guid.Empty` is a static readonly field.</span></span>

- <span data-ttu-id="ef06a-163">`in` bağımsız değişkeni parametre türüne _dönüştürülebilir_ olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-163">`in` argument must have type _identity-convertible_ to the type of the parameter.</span></span>
<span data-ttu-id="ef06a-164">Örnek: `M1<object>(in Guid.Empty)` geçersiz.</span><span class="sxs-lookup"><span data-stu-id="ef06a-164">Example: `M1<object>(in Guid.Empty)` is invalid.</span></span> <span data-ttu-id="ef06a-165">`Guid.Empty`, `object` için _kimlik dönüştürülebilir_ değil</span><span class="sxs-lookup"><span data-stu-id="ef06a-165">`Guid.Empty` is not _identity-convertible_ to `object`</span></span>

<span data-ttu-id="ef06a-166">Yukarıdaki kurallara ilişkin mosyon, bağımsız değişkenlerin bağımsız değişken değişkeninin _diğer_ adını garanti `in`.</span><span class="sxs-lookup"><span data-stu-id="ef06a-166">The motivation for the above rules is that `in` arguments guarantee _aliasing_ of the argument variable.</span></span> <span data-ttu-id="ef06a-167">Aranan her zaman bağımsız değişkenle temsil edilen konuma bir doğrudan başvuru alır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-167">The callee always receives a direct reference to the same location as represented by the argument.</span></span>

- <span data-ttu-id="ef06a-168">`in` bağımsız değişkenlerin yığın olarak, aynı çağrının işlenenleri olarak kullanılan `await` ifadeler nedeniyle yığın olarak bir arada olması gerektiğinde, bu davranış `out` ve `ref` bağımsız değişkenlerle aynı olur. değişken, zaman uyumlu olarak saydam olarak şeffaf bir şekilde bir hata bildirilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-168">in rare situations when `in` arguments must be stack-spilled due to `await` expressions used as operands of the same call, the behavior is the same as with `out` and `ref` arguments - if the variable cannot be spilled in referentially-transparent manner, an error is reported.</span></span>

<span data-ttu-id="ef06a-169">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="ef06a-169">Examples:</span></span>
1. <span data-ttu-id="ef06a-170">`M1(in staticField, await SomethingAsync())` geçerli.</span><span class="sxs-lookup"><span data-stu-id="ef06a-170">`M1(in staticField, await SomethingAsync())`  is valid.</span></span>
<span data-ttu-id="ef06a-171">`staticField`, observable yan etkileri olmadan birden çok kez erişilebilen statik bir alandır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-171">`staticField` is a static field which can be accessed more than once without observable side effects.</span></span> <span data-ttu-id="ef06a-172">Bu nedenle, yan etkileri ve diğer ad gereksinimlerinin sırası belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-172">Therefore both the order of side effects and aliasing requirements can be provided.</span></span>
2. <span data-ttu-id="ef06a-173">`M1(in RefReturningMethod(), await SomethingAsync())` bir hata oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="ef06a-173">`M1(in RefReturningMethod(), await SomethingAsync())`  will produce an error.</span></span>
<span data-ttu-id="ef06a-174">`RefReturningMethod()`, `ref` döndüren bir yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-174">`RefReturningMethod()` is a `ref` returning method.</span></span> <span data-ttu-id="ef06a-175">Bir yöntem çağrısında observable yan etkileri olabilir, bu nedenle `SomethingAsync()` işleneni önce değerlendirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-175">A method call may have observable side effects, therefore it must be evaluated before the `SomethingAsync()` operand.</span></span> <span data-ttu-id="ef06a-176">Ancak, çağrının sonucu, doğrudan başvuru gereksinimini olanaksız hale getirecek `await` askıya alma noktası genelinde korunmayan bir başvurudur.</span><span class="sxs-lookup"><span data-stu-id="ef06a-176">However the result of the invocation is a reference that cannot be preserved across the `await` suspension point which make the direct reference requirement impossible.</span></span>

> <span data-ttu-id="ef06a-177">Not: yığın atımı hatası, uygulamaya özgü sınırlamalar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-177">NOTE: the stack spilling errors are considered to be implementation-specific limitations.</span></span> <span data-ttu-id="ef06a-178">Bu nedenle, aşırı yükleme çözünürlüğü veya lambda çıkarımı üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="ef06a-178">Therefore they do not have effect on overload resolution or lambda inference.</span></span>

#### <a name="ordinary-byval-arguments-can-match-in-parameters"></a><span data-ttu-id="ef06a-179">Sıradan ByVal bağımsız değişkenleri `in` parametreleriyle eşleştirebilir:</span><span class="sxs-lookup"><span data-stu-id="ef06a-179">Ordinary byval arguments can match `in` parameters:</span></span>

<span data-ttu-id="ef06a-180">Değiştiriciler olmadan normal bağımsız değişkenler `in` parametreleriyle eşleşemez.</span><span class="sxs-lookup"><span data-stu-id="ef06a-180">Regular arguments without modifiers can match `in` parameters.</span></span> <span data-ttu-id="ef06a-181">Bu tür durumlarda bağımsız değişkenler, sıradan bir ByVal bağımsız değişkenleri ile aynı gevşek kısıtlamalara sahiptir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-181">In such case the arguments have the same relaxed constraints as an ordinary byval arguments would have.</span></span>

<span data-ttu-id="ef06a-182">Bu senaryo için mosyon, bağımsız değişkenler doğrudan başvuru-EX olarak geçirilmezse, API 'lerdeki `in` parametrelerinin Kullanıcı için nedeniyle ile sonuçlanmasına neden olabilir: değişmez değerler, hesaplanan veya `await`Ed sonuçlar veya daha belirli türlere sahip olan bağımsız değişkenler.</span><span class="sxs-lookup"><span data-stu-id="ef06a-182">The motivation for this scenario is that `in` parameters in APIs may result in inconveniences for the user when arguments cannot be passed as a direct reference - ex: literals, computed or `await`-ed results or arguments that happen to have more specific types.</span></span>  
<span data-ttu-id="ef06a-183">Tüm bu durumlarda, bağımsız değişken değerini uygun bir yerel türde depolamanın ve bu yerel değeri bir `in` bağımsız değişkeni olarak geçirerek çok basit bir çözüm vardır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-183">All these cases have a trivial solution of storing the argument value in a temporary local of appropriate type and passing that local as an `in` argument.</span></span>  
<span data-ttu-id="ef06a-184">Bu tür bir ortak kod derleyicisi gereksinimini azaltmak için, arama sitesinde `in` değiştiricisi yoksa, gerekirse aynı dönüştürmeyi gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-184">To reduce the need for such boilerplate code compiler can perform the same transformation, if needed, when `in` modifier is not present at the call site.</span></span>  

<span data-ttu-id="ef06a-185">Ayrıca, işleçler veya `in` genişletme yöntemlerinin çağrılması gibi bazı durumlarda `in` belirtmenin herhangi bir sözdizimi yoktur.</span><span class="sxs-lookup"><span data-stu-id="ef06a-185">In addition, in some cases, such as invocation of operators, or `in` extension methods, there is no syntactical way to specify `in` at all.</span></span> <span data-ttu-id="ef06a-186">Tek başına, `in` parametreleriyle eşleştiklerinde sıradan ByVal bağımsız değişkenlerinin davranışının belirtilmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-186">That alone requires specifying the behavior of ordinary byval arguments when they match `in` parameters.</span></span>

<span data-ttu-id="ef06a-187">Özellikle:</span><span class="sxs-lookup"><span data-stu-id="ef06a-187">In particular:</span></span>

- <span data-ttu-id="ef06a-188">RValues geçişi için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-188">it is valid to pass RValues.</span></span>
<span data-ttu-id="ef06a-189">Bu tür bir durumda geçici bir başvuru geçirilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-189">A reference to a temporary is passed in such case.</span></span>
<span data-ttu-id="ef06a-190">Örnek:</span><span class="sxs-lookup"><span data-stu-id="ef06a-190">Example:</span></span>
```csharp
Print("hello");      // not an error.

void Print<T>(in T x)
{
  //. . .
}
```

- <span data-ttu-id="ef06a-191">örtük Dönüştürmelere izin verilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-191">implicit conversions are allowed.</span></span>

> <span data-ttu-id="ef06a-192">Bu aslında RValue geçirme özel bir durumdur</span><span class="sxs-lookup"><span data-stu-id="ef06a-192">This is actually a special case of passing an RValue</span></span>  

<span data-ttu-id="ef06a-193">Bu tür bir durumda, geçici olarak bir dönüştürülmüş değer tutan bir başvuru geçirilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-193">A reference to a temporary holding converted value is passed in such case.</span></span>
<span data-ttu-id="ef06a-194">Örnek:</span><span class="sxs-lookup"><span data-stu-id="ef06a-194">Example:</span></span>
```csharp
Print<int>(Short.MaxValue)     // not an error.
```

- <span data-ttu-id="ef06a-195">`in` uzantısı yönteminin alıcısında (`ref` uzantısı yöntemlerinin aksine), RValues veya örtük _Bu bağımsız değişken dönüştürmelerine_ izin verilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-195">in a case of a receiver of an `in` extension method (as opposed to `ref` extension methods), RValues or implicit _this-argument-conversions_ are allowed.</span></span>
<span data-ttu-id="ef06a-196">Bu tür bir durumda, geçici olarak bir dönüştürülmüş değer tutan bir başvuru geçirilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-196">A reference to a temporary holding converted value is passed in such case.</span></span>
<span data-ttu-id="ef06a-197">Örnek:</span><span class="sxs-lookup"><span data-stu-id="ef06a-197">Example:</span></span>
```csharp
public static IEnumerable<T> Concat<T>(in this (IEnumerable<T>, IEnumerable<T>) arg)  => . . .;

("aa", "bb").Concat<char>()    // not an error.
```
<span data-ttu-id="ef06a-198">`ref`/`in` uzantı yöntemleri hakkında daha fazla bilgi bu belgede daha fazla sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-198">More information on `ref`/`in` extension methods is provided further in this document.</span></span>

- <span data-ttu-id="ef06a-199">bağımsız değişken, `await` işlenenleri nedeniyle, gerekirse "değere göre" taşarak taşma olabilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-199">argument spilling due to `await` operands could spill "by-value", if necessary.</span></span>
<span data-ttu-id="ef06a-200">Bir bağımsız değişkene doğrudan başvuru sağlamak `await` için bağımsız değişken olarak sağlanan senaryolarda, bağımsız değişkenin değerinin bir kopyası bunun yerine taşılır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-200">In scenarios where providing a direct reference to the argument is not possible due to intervening `await` a copy of the argument's value is spilled instead.</span></span>  
<span data-ttu-id="ef06a-201">Örnek:</span><span class="sxs-lookup"><span data-stu-id="ef06a-201">Example:</span></span>
```csharp
M1(RefReturningMethod(), await SomethingAsync())   // not an error.
```
<span data-ttu-id="ef06a-202">Yan etkili bir çağrının sonucu `await` askıya alma boyunca korunmayan bir başvuru olduğundan, gerçek değeri içeren geçici bir olay bunun yerine korunur (normal bir ByVal parametre durumunda olduğu gibi).</span><span class="sxs-lookup"><span data-stu-id="ef06a-202">Since the result of a side-effecting invocation is a reference that cannot be preserved across `await` suspension, a temporary containing the actual value will be preserved instead (as it would in an ordinary byval parameter case).</span></span>

#### <a name="omitted-optional-arguments"></a><span data-ttu-id="ef06a-203">Atlanan isteğe bağlı bağımsız değişkenler</span><span class="sxs-lookup"><span data-stu-id="ef06a-203">Omitted optional arguments</span></span>

<span data-ttu-id="ef06a-204">Bir `in` parametresi için varsayılan bir değer belirtmek için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-204">It is permitted for an `in` parameter to specify a default value.</span></span> <span data-ttu-id="ef06a-205">Bu, karşılık gelen bağımsız değişkeni isteğe bağlı hale getirir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-205">That makes the corresponding argument optional.</span></span>

<span data-ttu-id="ef06a-206">Çağrı sitesinde isteğe bağlı bağımsız değişkeni atlama, varsayılan değeri geçici olarak geçirme ile sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-206">Omitting optional argument at the call site results in passing the default value via a temporary.</span></span>

```csharp
Print("hello");      // not an error, same as
Print("hello", c: Color.Black);

void Print(string s, in Color c = Color.Black)
{
    // . . .
}
```

### <a name="aliasing-behavior-in-general"></a><span data-ttu-id="ef06a-207">Genel olarak diğer ad davranışı</span><span class="sxs-lookup"><span data-stu-id="ef06a-207">Aliasing behavior in general</span></span>

<span data-ttu-id="ef06a-208">`ref` ve `out` değişkenleri gibi `in` değişkenler, var olan konumların başvuruları/diğer adları.</span><span class="sxs-lookup"><span data-stu-id="ef06a-208">Just like `ref` and `out` variables, `in` variables are references/aliases to existing locations.</span></span>

<span data-ttu-id="ef06a-209">Çağrılan tarafından bunlara yazma izni verilmediğinden, bir `in` parametresi okumak diğer değerlendirmelere yan bir etkisi olarak farklı değerleri gözlemleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef06a-209">While callee is not allowed to write into them, reading an `in` parameter can observe different values as a side effect of other evaluations.</span></span>

<span data-ttu-id="ef06a-210">Örnek:</span><span class="sxs-lookup"><span data-stu-id="ef06a-210">Example:</span></span>

```C#
static Vector3 v = Vector3.UnitY;

static void Main()
{
    Test(v);
}

static void Test(in Vector3 v1)
{
    Debug.Assert(v1 == Vector3.UnitY);
    // changes v1 deterministically (no races required)
    ChangeV();
    Debug.Assert(v1 == Vector3.UnitX);
}

static void ChangeV()
{
    v = Vector3.UnitX;
}
```

### <a name="in-parameters-and-capturing-of-local-variables"></a><span data-ttu-id="ef06a-211">parametreleri `in` ve yerel değişkenlerin yakalanması.</span><span class="sxs-lookup"><span data-stu-id="ef06a-211">`in` parameters and capturing of local variables.</span></span>  
<span data-ttu-id="ef06a-212">Lambda/zaman uyumsuz yakalama `in` parametrelerinin amacı, `out` ve `ref` parametreleriyle aynı şekilde davranır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-212">For the purpose of lambda/async capturing `in` parameters behave the same as `out` and `ref` parameters.</span></span>

- <span data-ttu-id="ef06a-213">`in` parametreler bir kapanışda yakalanamaz</span><span class="sxs-lookup"><span data-stu-id="ef06a-213">`in` parameters cannot be captured in a closure</span></span>
- <span data-ttu-id="ef06a-214">Yineleyici metotlarda `in` parametrelere izin verilmiyor</span><span class="sxs-lookup"><span data-stu-id="ef06a-214">`in` parameters are not allowed in iterator methods</span></span>
- <span data-ttu-id="ef06a-215">zaman uyumsuz yöntemlerde `in` parametrelere izin verilmez</span><span class="sxs-lookup"><span data-stu-id="ef06a-215">`in` parameters are not allowed in async methods</span></span>

### <a name="temporary-variables"></a><span data-ttu-id="ef06a-216">Geçici değişkenler.</span><span class="sxs-lookup"><span data-stu-id="ef06a-216">Temporary variables.</span></span>  
<span data-ttu-id="ef06a-217">`in` parametresi geçirmenin bazı kullanımları, geçici bir yerel değişkenin dolaylı kullanımını gerektirebilir:</span><span class="sxs-lookup"><span data-stu-id="ef06a-217">Some uses of `in` parameter passing may require indirect use of a temporary local variable:</span></span>  
- <span data-ttu-id="ef06a-218">`in` bağımsız değişkenler her zaman, Call-site `in`kullandığında doğrudan diğer adlar olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-218">`in` arguments are always passed as direct aliases when call-site uses `in`.</span></span> <span data-ttu-id="ef06a-219">Geçici, böyle bir durumda hiçbir şekilde kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="ef06a-219">Temporary is never used in such case.</span></span>
- <span data-ttu-id="ef06a-220">çağrı sitesi `in`kullanmıyorsa `in` bağımsız değişkenlerin doğrudan takma adlar olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="ef06a-220">`in` arguments are not required to be direct aliases when call-site does not use `in`.</span></span> <span data-ttu-id="ef06a-221">Bağımsız değişken bir LValue olmadığında geçici bir şekilde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-221">When argument is not an LValue, a temporary may be used.</span></span>
- <span data-ttu-id="ef06a-222">`in` parametre varsayılan değere sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-222">`in` parameter may have default value.</span></span> <span data-ttu-id="ef06a-223">Çağrı sitesinde karşılık gelen bağımsız değişken atlandığında, varsayılan değer geçici olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-223">When corresponding argument is omitted at the call site, the default value are passed via a temporary.</span></span>
- <span data-ttu-id="ef06a-224">`in` bağımsız değişkenler, kimliği korumayan bulunanlar da dahil olmak üzere örtük Dönüştürmelere sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-224">`in` arguments may have implicit conversions, including those that do not preserve identity.</span></span> <span data-ttu-id="ef06a-225">Geçici olarak bu durumlarda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-225">A temporary is used in those cases.</span></span>
- <span data-ttu-id="ef06a-226">sıradan yapı çağrılarının alıcıları yazılabilir LValues (**mevcut durum!** ) olamaz.</span><span class="sxs-lookup"><span data-stu-id="ef06a-226">receivers of ordinary struct calls may not be writeable LValues (**existing case!**).</span></span> <span data-ttu-id="ef06a-227">Geçici olarak bu durumlarda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-227">A temporary is used in those cases.</span></span>

<span data-ttu-id="ef06a-228">Geçiciler bağımsız değişkeninin yaşam süresi, Call-site ' ın en yakın çevreleme kapsamıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-228">The life time of the argument temporaries matches the closest encompassing scope of the call-site.</span></span>

<span data-ttu-id="ef06a-229">Geçici değişkenlerin biçimsel yaşam süresi, başvuruya göre döndürülen değişkenlerin kaçış analizini içeren senaryolarda anlam açısından önemlidir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-229">The formal life time of temporary variables is semantically significant in scenarios involving escape analysis of variables returned by reference.</span></span>

### <a name="metadata-representation-of-in-parameters"></a><span data-ttu-id="ef06a-230">`in` parametrelerinin meta veri temsili.</span><span class="sxs-lookup"><span data-stu-id="ef06a-230">Metadata representation of `in` parameters.</span></span>
<span data-ttu-id="ef06a-231">`System.Runtime.CompilerServices.IsReadOnlyAttribute` bir ByRef parametresine uygulandığında, parametrenin bir `in` parametresi olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-231">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to a byref parameter, it means that the parameter is an `in` parameter.</span></span>

<span data-ttu-id="ef06a-232">Ayrıca, yöntem *soyut* veya *sanal*ise, bu parametre (ve yalnızca bu parametreler) imzasının `modreq[System.Runtime.InteropServices.InAttribute]`olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-232">In addition, if the method is *abstract* or *virtual*, then the signature of such parameters (and only such parameters) must have `modreq[System.Runtime.InteropServices.InAttribute]`.</span></span>

<span data-ttu-id="ef06a-233">**Mosyon**: bu, `in` parametrelerini geçersiz kılan/uygulayan bir yöntem durumunda olduğundan emin olmak için yapılır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-233">**Motivation**: this is done to ensure that in a case of method overriding/implementing the `in` parameters match.</span></span>

<span data-ttu-id="ef06a-234">Temsilcilerle `Invoke` yöntemler için de aynı gereksinimler geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-234">Same requirements apply to `Invoke` methods in delegates.</span></span>

<span data-ttu-id="ef06a-235">**Mosyon**: Bu, mevcut derleyicilerin temsilcileri oluştururken veya atarken `readonly` yoksaymasını sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-235">**Motivation**: this is to ensure that existing compilers cannot simply ignore `readonly` when creating or assigning delegates.</span></span>

## <a name="returning-by-readonly-reference"></a><span data-ttu-id="ef06a-236">ReadOnly başvurusuyla döndürülüyor.</span><span class="sxs-lookup"><span data-stu-id="ef06a-236">Returning by readonly reference.</span></span>

### <a name="motivation"></a><span data-ttu-id="ef06a-237">Amacı</span><span class="sxs-lookup"><span data-stu-id="ef06a-237">Motivation</span></span>
<span data-ttu-id="ef06a-238">Bu alt özellik için mosyon, `in` parametrelerinin nedenleri için kabaca simetrik olarak yapılır, ancak döndürülen tarafta, kopyalamayı önler.</span><span class="sxs-lookup"><span data-stu-id="ef06a-238">The motivation for this sub-feature is roughly symmetrical to the reasons for the `in` parameters - avoiding copying, but on the returning side.</span></span> <span data-ttu-id="ef06a-239">Bu özellikten önce, bir yöntem veya dizin oluşturucunun iki seçeneği vardır: 1) başvuruya göre geri dönün ve olası mutasyonların veya 2), kopyalama ile sonuçlanan değere göre döndürülür.</span><span class="sxs-lookup"><span data-stu-id="ef06a-239">Prior to this feature, a method or an indexer had two options: 1) return by reference and be exposed to possible mutations or 2) return by value which results in copying.</span></span>

### <a name="solution-ref-readonly-returns"></a><span data-ttu-id="ef06a-240">Çözüm (`ref readonly` döndürür)</span><span class="sxs-lookup"><span data-stu-id="ef06a-240">Solution (`ref readonly` returns)</span></span>  
<span data-ttu-id="ef06a-241">Özelliği, bir üyenin değişkenleri bir başvuruya göre geri almasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-241">The feature allows a member to return variables by reference without exposing them to mutations.</span></span>

### <a name="declaring-ref-readonly-returning-members"></a><span data-ttu-id="ef06a-242">`ref readonly` döndüren Üyeler bildirme</span><span class="sxs-lookup"><span data-stu-id="ef06a-242">Declaring `ref readonly` returning members</span></span>

<span data-ttu-id="ef06a-243">Dönüş imzasında değiştiriciler `ref readonly` birleşimi, üyenin salt okunur bir başvuru döndürdüğünü göstermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-243">A combination of modifiers `ref readonly` on the return signature is used to to indicate that the member returns a readonly reference.</span></span>

<span data-ttu-id="ef06a-244">Tüm amaçlar için bir `ref readonly` üye `readonly` alanlara ve `in` parametrelere benzer bir `readonly` değişken olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-244">For all purposes a `ref readonly` member is treated as a `readonly` variable - similar to `readonly` fields and `in` parameters.</span></span>

<span data-ttu-id="ef06a-245">Örneğin, yapı türüne sahip `ref readonly` üyesinin alanları, `readonly` değişkenleri olarak özyinelemeli olarak sınıflandırıldı.</span><span class="sxs-lookup"><span data-stu-id="ef06a-245">For example fields of `ref readonly` member which has a struct type are all recursively classified as `readonly` variables.</span></span> <span data-ttu-id="ef06a-246">-`ref` veya `out` bağımsız değişken olarak değil, bunları `in` bağımsız değişken olarak geçirmeye izin verilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-246">- It is permitted to pass them as `in` arguments, but not as `ref` or `out` arguments.</span></span>

```csharp
ref readonly Guid Method1()
{
}

Method2(in Method1()); // valid. Can pass as `in` argument.

Method3(ref Method1()); // not valid. Cannot pass as `ref` argument
```

- <span data-ttu-id="ef06a-247">`ref readonly` dönüşler aynı yerlerde izin verilir `ref` dönüşlerine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-247">`ref readonly` returns are allowed in the same places were `ref` returns are allowed.</span></span>
<span data-ttu-id="ef06a-248">Bu, Dizin oluşturucular, temsilciler, Lambdalar, yerel işlevler içerir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-248">This includes indexers, delegates, lambdas, local functions.</span></span>

- <span data-ttu-id="ef06a-249">`ref`/`ref readonly`/farklılığı aşırı yüklenmesine izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="ef06a-249">It is not permitted to overload on `ref`/`ref readonly` /  differences.</span></span>

- <span data-ttu-id="ef06a-250">Normal ByVal üzerinde aşırı yükleme ve dönüş farklılıkları `ref readonly` izin verilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-250">It is permitted to overload on ordinary byval and `ref readonly` return differences.</span></span>

- <span data-ttu-id="ef06a-251">OHI (aşırı yükleme, gizleme, uygulama) amacı için `ref readonly` benzer ancak `ref`farklıdır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-251">For the purpose of OHI (Overloading, Hiding, Implementing), `ref readonly` is similar but distinct from `ref`.</span></span>
<span data-ttu-id="ef06a-252">Örneğin, `ref readonly` geçersiz kılan bir yöntem, kendisinin `ref readonly` ve kimlik dönüştürülebilir türü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-252">For example the a method that overrides `ref readonly` one, must itself be `ref readonly` and have identity-convertible type.</span></span>

- <span data-ttu-id="ef06a-253">Temsilci/Lambda/Yöntem grubu dönüştürmelerinde, `ref readonly` benzer ancak `ref`farklıdır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-253">For the purpose of delegate/lambda/method group conversions, `ref readonly` is similar but distinct from `ref`.</span></span>
<span data-ttu-id="ef06a-254">Lambdalar ve geçerli yöntem grubu dönüştürme adayları, kimlik dönüştürülebilir olan türün `ref readonly` dönüşü ile hedef temsilcinin `ref readonly` geri dönmesi ile eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-254">Lambdas and applicable method group conversion candidates have to match `ref readonly` return of the target delegate with `ref readonly` return of the type that is identity-convertible.</span></span>

- <span data-ttu-id="ef06a-255">Genel varyans için `ref readonly` dönüşler, değişken olmayan bir değer değildir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-255">For the purpose of generic variance, `ref readonly` returns are nonvariant.</span></span>

> <span data-ttu-id="ef06a-256">NOTE: başvuru ya da ilkel türler içeren `ref readonly` dönüşde uyarı yok.</span><span class="sxs-lookup"><span data-stu-id="ef06a-256">NOTE: There are no warnings on `ref readonly` returns that have reference or primitives types.</span></span>
<span data-ttu-id="ef06a-257">Genel olarak daha az olabilir, ancak bazı durumlarda, kullanıcıların temelleri `in`olarak geçirmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-257">It may be pointless in general, but in some cases user must/want to pass primitives as `in`.</span></span> <span data-ttu-id="ef06a-258">Örnekler-`T` `int`yerine `ref readonly T Method()` gibi genel bir yöntemi geçersiz kılma.</span><span class="sxs-lookup"><span data-stu-id="ef06a-258">Examples - overriding a generic method like `ref readonly T Method()` when `T` was substituted to be `int`.</span></span>
>
><span data-ttu-id="ef06a-259">`ref readonly` dönüşün verimsiz kullanımı durumlarında uyaran bir çözümleyiciye sahip olmak, ancak bu tür analizler için kurallar bir dil belirtiminin parçası olmak için çok belirsiz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-259">It is conceivable to have an analyzer that warns in cases of inefficient use of `ref readonly` returns, but the rules for such analysis would be too fuzzy to be a part of a language specification.</span></span>

### <a name="returning-from-ref-readonly-members"></a><span data-ttu-id="ef06a-260">`ref readonly` üyelerinden döndürme</span><span class="sxs-lookup"><span data-stu-id="ef06a-260">Returning from `ref readonly` members</span></span>
<span data-ttu-id="ef06a-261">Yöntem gövdesinin içinde sözdizimi, normal ref döndürimiyle aynıdır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-261">Inside the method body the syntax is the same as with regular ref returns.</span></span> <span data-ttu-id="ef06a-262">`readonly`, kapsayan yöntemden çıkarsanacaktır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-262">The `readonly` will be inferred from the containing method.</span></span>

<span data-ttu-id="ef06a-263">Mosyon, `return ref readonly <expression>` gereksiz bir süredir ve yalnızca `readonly` bölümünde hatalara neden olacak uyuşmazlıkları sağlar.</span><span class="sxs-lookup"><span data-stu-id="ef06a-263">The motivation is that `return ref readonly <expression>` is unnecessary long and only allows for mismatches on the `readonly` part that would always result in errors.</span></span>
<span data-ttu-id="ef06a-264">Ancak `ref`, bir şeyin kesin diğer ad ile ve değere göre geçirildiği diğer senaryolarla tutarlılık için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-264">The `ref` is, however, required for consistency with other scenarios where something is passed via strict aliasing vs. by value.</span></span>

> <span data-ttu-id="ef06a-265">`in` parametrelerinin aksine, `ref readonly` hiçbir süre bir yerel kopya aracılığıyla döndürmez.</span><span class="sxs-lookup"><span data-stu-id="ef06a-265">Unlike the case with `in` parameters, `ref readonly` returns never return via a local copy.</span></span> <span data-ttu-id="ef06a-266">Kopyanın bu tür bir uygulama döndürüldüğünde hemen mevcut olmaya başlayacağından emin olmak, daha az ve tehlikeli olur.</span><span class="sxs-lookup"><span data-stu-id="ef06a-266">Considering that the copy would cease to exist immediately upon returning such practice would be pointless and dangerous.</span></span> <span data-ttu-id="ef06a-267">Bu nedenle `ref readonly` dönüşler her zaman doğrudan başvurulardır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-267">Therefore `ref readonly` returns are always direct references.</span></span>

<span data-ttu-id="ef06a-268">Örnek:</span><span class="sxs-lookup"><span data-stu-id="ef06a-268">Example:</span></span>

```csharp
struct ImmutableArray<T>
{
    private readonly T[] array;

    public ref readonly T ItemRef(int i)
    {
        // returning a readonly reference to an array element
        return ref this.array[i];
    }
}

```

- <span data-ttu-id="ef06a-269">`return ref` bağımsız değişkeni bir LValue olmalıdır (**varolan kural**)</span><span class="sxs-lookup"><span data-stu-id="ef06a-269">An argument of `return ref` must be an LValue (**existing rule**)</span></span>
- <span data-ttu-id="ef06a-270">`return ref` bağımsız değişkeninin "dönmesi için güvenli" olması gerekir (**var olan kural**)</span><span class="sxs-lookup"><span data-stu-id="ef06a-270">An argument of `return ref` must be "safe to return" (**existing rule**)</span></span>
- <span data-ttu-id="ef06a-271">Bir `ref readonly` üyesinde `return ref` bağımsız değişkeninin _yazılabilir olması_ gerekmez.</span><span class="sxs-lookup"><span data-stu-id="ef06a-271">In a `ref readonly` member an argument of `return ref` is _not required to be writeable_ .</span></span>
<span data-ttu-id="ef06a-272">Örneğin, bu tür bir üye, salt okunur bir alanı veya `in` parametrelerinden birini döndürür.</span><span class="sxs-lookup"><span data-stu-id="ef06a-272">For example such member can ref-return a readonly field or one of its `in` parameters.</span></span>

### <a name="safe-to-return-rules"></a><span data-ttu-id="ef06a-273">Kuralları döndürmek için güvenli.</span><span class="sxs-lookup"><span data-stu-id="ef06a-273">Safe to Return rules.</span></span>
<span data-ttu-id="ef06a-274">Başvuru kuralları için normal güvenli, salt okunur başvurulara de uygulanır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-274">Normal safe to return rules for references will apply to readonly references as well.</span></span>

<span data-ttu-id="ef06a-275">`ref readonly`, normal `ref` yerel/parametre/dönüşten elde edilebilir, ancak bunun etrafında değil.</span><span class="sxs-lookup"><span data-stu-id="ef06a-275">Note that a `ref readonly` can be obtained from a regular `ref` local/parameter/return, but not the other way around.</span></span> <span data-ttu-id="ef06a-276">Aksi takdirde `ref readonly` dönüşün güvenliği, normal `ref` dönüşler ile aynı şekilde algılanır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-276">Otherwise the safety of `ref readonly` returns is inferred the same way as for regular `ref` returns.</span></span>

<span data-ttu-id="ef06a-277">RValues 'un `in` parametresi olarak geçirilebileceğini ve `ref readonly` olarak döndürüldüğünden emin olmak için, bir daha fazla kural gerekir- **rvalues başvuruya göre güvenli-geri döndürülemez**.</span><span class="sxs-lookup"><span data-stu-id="ef06a-277">Considering that RValues can be passed as `in` parameter and returned as `ref readonly` we need one more rule - **RValues are not safe-to-return by reference**.</span></span>

> <span data-ttu-id="ef06a-278">Bir RValue, bir `in` parametreye bir kopya aracılığıyla geçirildiğinde ve sonra bir `ref readonly`biçiminde geri döndürüldüğünde durumu göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="ef06a-278">Consider the situation when an RValue is passed to an `in` parameter via a copy and then returned back in a form of a `ref readonly`.</span></span> <span data-ttu-id="ef06a-279">Çağıran bağlamında, bu tür çağrının sonucu, yerel verilere yönelik bir başvurudur ve bu nedenle bu, döndürülmek üzere güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-279">In the context of the caller the result of such invocation is a reference to local data and as such is unsafe to return.</span></span>
> <span data-ttu-id="ef06a-280">RValues değeri dönmek için güvenli olmadıktan sonra var olan kural `#6` bu durumu zaten işler.</span><span class="sxs-lookup"><span data-stu-id="ef06a-280">Once RValues are not safe to return, the existing rule `#6` already handles this case.</span></span>

<span data-ttu-id="ef06a-281">Örnek:</span><span class="sxs-lookup"><span data-stu-id="ef06a-281">Example:</span></span>
```csharp
ref readonly Vector3 Test1()
{
    // can pass an RValue as "in" (via a temp copy)
    // but the result is not safe to return
    // because the RValue argument was not safe to return by reference
    return ref Test2(default(Vector3));
}

ref readonly Vector3 Test2(in Vector3 r)
{
    // this is ok, r is returnable
    return ref r;
}
```

<span data-ttu-id="ef06a-282">`safe to return` kuralları güncelleştirildi:</span><span class="sxs-lookup"><span data-stu-id="ef06a-282">Updated `safe to return` rules:</span></span>

1.  <span data-ttu-id="ef06a-283">**yığındaki değişkenlere başvuruların dönmesi için güvenlidir**</span><span class="sxs-lookup"><span data-stu-id="ef06a-283">**refs to variables on the heap are safe to return**</span></span>
2.  <span data-ttu-id="ef06a-284">**ref/in parametrelerinin geri dönmesi**
`in` parametreleri doğal olarak yalnızca ReadOnly olarak döndürülebilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-284">**ref/in parameters are safe to return**
`in` parameters naturally can only be returned as readonly.</span></span>
3.  <span data-ttu-id="ef06a-285">**Out parametrelerinin dönmesi güvenlidir** (ancak zaten bugün olduğu gibi, kesinlikle atanması gerekir)</span><span class="sxs-lookup"><span data-stu-id="ef06a-285">**out parameters are safe to return** (but must be definitely assigned, as is already the case today)</span></span>
4.  <span data-ttu-id="ef06a-286">**alıcının dönmesi güvenli olduğu sürece örnek struct alanları geri dönmek için güvenlidir**</span><span class="sxs-lookup"><span data-stu-id="ef06a-286">**instance struct fields are safe to return as long as the receiver is safe to return**</span></span>
5.  <span data-ttu-id="ef06a-287">**' this ', yapı üyelerinden dönmek için güvenli değildir**</span><span class="sxs-lookup"><span data-stu-id="ef06a-287">**'this' is not safe to return from struct members**</span></span>
6.  <span data-ttu-id="ef06a-288">**başka bir yöntemden döndürülen ref, bu yönteme biçimsel parametreler olarak geçirilen tüm ReFS/ıse 'nin dönmesi güvenli olduğu durumlarda döndürülür.** alıcı, *alıcının bir struct, Class veya genel tür parametresi olarak yazılmış olmasına bakılmaksızın, bu, özellikle alıcının dönmesi güvenli hale getirdiğinden ilgisiz değildir.* 
</span><span class="sxs-lookup"><span data-stu-id="ef06a-288">**a ref, returned from another method is safe to return if all refs/outs passed to that method as formal parameters were safe to return.**
*Specifically it is irrelevant if receiver is safe to return, regardless whether receiver is a struct, class or typed as a generic type parameter.*</span></span>
7.  <span data-ttu-id="ef06a-289">**Rvalues, başvuruya göre dönmek için güvenli değildir.** 
*özel olarak RValues, parametrelere göre doğrudan geçiş için güvenlidir.*</span><span class="sxs-lookup"><span data-stu-id="ef06a-289">**RValues are not safe to return by reference.**
*Specifically RValues are safe to pass as in parameters.*</span></span>

> <span data-ttu-id="ef06a-290">NOTE: ref benzeri türler ve ref atamaları dahil edildiğinde yürütmeye gelen dönüşlerle ilgili ek kurallar vardır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-290">NOTE: There are additional rules regarding safety of returns that come into play when ref-like types and ref-reassignments are involved.</span></span>
> <span data-ttu-id="ef06a-291">Kurallar `ref` ve `ref readonly` üyelerine eşit olarak uygulanır ve bu nedenle burada bahsedilmez.</span><span class="sxs-lookup"><span data-stu-id="ef06a-291">The rules equally apply to `ref` and `ref readonly` members and therefore are not mentioned here.</span></span>

### <a name="aliasing-behavior"></a><span data-ttu-id="ef06a-292">Diğer ad davranışı.</span><span class="sxs-lookup"><span data-stu-id="ef06a-292">Aliasing behavior.</span></span>
<span data-ttu-id="ef06a-293">`ref readonly` Üyeler sıradan `ref` üyeleriyle aynı diğer ad davranışını sağlar (salt okunur olması dışında).</span><span class="sxs-lookup"><span data-stu-id="ef06a-293">`ref readonly` members provide the same aliasing behavior as ordinary `ref` members (except for being readonly).</span></span>
<span data-ttu-id="ef06a-294">Bu nedenle, Lambdalar, zaman uyumsuz, yineleyiciler, yığın sıçraıcı vb. için yakalama amacına yöneliktir. aynı kısıtlamalar geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-294">Therefore for the purpose of capturing in lambdas, async, iterators, stack spilling etc... the same restrictions apply.</span></span> <span data-ttu-id="ef06a-295">Yani.</span><span class="sxs-lookup"><span data-stu-id="ef06a-295">- I.E.</span></span> <span data-ttu-id="ef06a-296">gerçek başvuruların yakalanmasının nedeni ve üye değerlendirmesinin yan yana etkili olması nedeniyle bu senaryolara izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="ef06a-296">due to inability to capture the actual references and due to side-effecting nature of member evaluation such scenarios are disallowed.</span></span>

> <span data-ttu-id="ef06a-297">`ref readonly` return, normal bir yazılabilir başvuru olarak `this` alan bir düzenli yapı yöntemleri alıcısında olduğunda bir kopya oluşturmak için izin verilir ve gereklidir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-297">It is permitted and required to make a copy when `ref readonly` return is a receiver of regular struct methods, which take `this` as an ordinary writeable reference.</span></span> <span data-ttu-id="ef06a-298">Tarihsel olarak, bu tür bir çağırmaları ReadOnly değişkenine uygulandığı her durumda yerel bir kopya yapılır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-298">Historically in all cases where such invocations are applied to readonly variable a local copy is made.</span></span>

### <a name="metadata-representation"></a><span data-ttu-id="ef06a-299">Meta veri gösterimi.</span><span class="sxs-lookup"><span data-stu-id="ef06a-299">Metadata representation.</span></span>
<span data-ttu-id="ef06a-300">ByRef döndüren metodun dönüşe `System.Runtime.CompilerServices.IsReadOnlyAttribute` uygulandığında, yöntemin salt okunur bir başvuru döndürdüğü anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-300">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to the return of a byref returning method, it means that the method returns a readonly reference.</span></span>

<span data-ttu-id="ef06a-301">Ayrıca, bu yöntemlerin sonuç imzasında (ve yalnızca bu yöntemler) `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-301">In addition, the result signature of such methods (and only those methods) must have `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`.</span></span>

<span data-ttu-id="ef06a-302">**Mosyon**: Bu, mevcut derleyicilerin `ref readonly` dönüşlerle yöntemleri çağırırken `readonly` yoksaymasını sağlamaktır</span><span class="sxs-lookup"><span data-stu-id="ef06a-302">**Motivation**: this is to ensure that existing compilers cannot simply ignore `readonly` when invoking methods with `ref readonly` returns</span></span>

## <a name="readonly-structs"></a><span data-ttu-id="ef06a-303">ReadOnly yapılar</span><span class="sxs-lookup"><span data-stu-id="ef06a-303">Readonly structs</span></span>
<span data-ttu-id="ef06a-304">Kısa bir özellik içinde, `in` oluşturucular hariç, bir yapının tüm örnek üyelerinin `this` parametresini oluşturan bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-304">In short - a feature that makes `this` parameter of all instance members of a struct, except for constructors, an `in` parameter.</span></span>

### <a name="motivation"></a><span data-ttu-id="ef06a-305">Amacı</span><span class="sxs-lookup"><span data-stu-id="ef06a-305">Motivation</span></span>
<span data-ttu-id="ef06a-306">Derleyici, bir struct örneğindeki herhangi bir yöntem çağrısının örneği değiştireolabileceğini varsaymalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-306">Compiler must assume that any method call on a struct instance may modify the instance.</span></span> <span data-ttu-id="ef06a-307">Aslında yazılabilir bir başvuru yönteme `this` parametresi olarak geçirilir ve bu davranışı tamamen sağlar.</span><span class="sxs-lookup"><span data-stu-id="ef06a-307">Indeed a writeable reference is passed to the method as `this` parameter and fully enables this behavior.</span></span> <span data-ttu-id="ef06a-308">`readonly` değişkenlerde bu tür çağırmaları sağlamak için, çağırmaları geçici kopyalara uygulanır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-308">To allow such invocations on `readonly` variables, the invocations are applied to temp copies.</span></span> <span data-ttu-id="ef06a-309">Bu, sezgisel hale gelebilir ve bazen kişilerin performans nedenleriyle `readonly` iptal etmeye zorlar.</span><span class="sxs-lookup"><span data-stu-id="ef06a-309">That could be unintuitive and sometimes forces people to abandon `readonly` for performance reasons.</span></span>  
<span data-ttu-id="ef06a-310">Örnek: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/</span><span class="sxs-lookup"><span data-stu-id="ef06a-310">Example: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/</span></span>

<span data-ttu-id="ef06a-311">`in` parametreleri için destek eklendikten ve `ref readonly`, salt okunur değişkenler daha yaygın hale gelediğinden, savunmaya karşı kopyalama sorununu geri döndürmektedir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-311">After adding support for `in` parameters and `ref readonly` returns the problem of defensive copying will get worse since readonly variables will become more common.</span></span>

### <a name="solution"></a><span data-ttu-id="ef06a-312">Çözüm</span><span class="sxs-lookup"><span data-stu-id="ef06a-312">Solution</span></span>
<span data-ttu-id="ef06a-313">Yapı bildirimlerinde `readonly` değiştiriciye izin verin; bu, `this` oluşturucular hariç tüm struct örneği yöntemlerinde `in` parametre olarak değerlendirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-313">Allow `readonly` modifier on struct declarations which would result in `this` being treated as `in` parameter on all struct instance methods except for constructors.</span></span>

```csharp
static void Test(in Vector3 v1)
{
    // no need to make a copy of v1 since Vector3 is a readonly struct
    System.Console.WriteLine(v1.ToString());
}

readonly struct Vector3
{
    . . .

    public override string ToString()
    {
        // not OK!!  `this` is an `in` parameter
        foo(ref this.X);

        // OK
        return $"X: {X}, Y: {Y}, Z: {Z}";
    }
}
```

### <a name="restrictions-on-members-of-readonly-struct"></a><span data-ttu-id="ef06a-314">ReadOnly yapısının üyeleri hakkında kısıtlamalar</span><span class="sxs-lookup"><span data-stu-id="ef06a-314">Restrictions on members of readonly struct</span></span>
- <span data-ttu-id="ef06a-315">ReadOnly yapısının örnek alanları salt okunur olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-315">Instance fields of a readonly struct must be readonly.</span></span>  
<span data-ttu-id="ef06a-316">**Mosyon:** yalnızca harici olarak yazılabilir ancak üyelere eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-316">**Motivation:** can only be written to externally, but not through members.</span></span>
- <span data-ttu-id="ef06a-317">Salt okunur bir yapının örnek oto özellikleri salt al olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-317">Instance autoproperties of a readonly struct must be get-only.</span></span>  
<span data-ttu-id="ef06a-318">**Mosyon:** örnek alanlarında kısıtlamanın sonucu.</span><span class="sxs-lookup"><span data-stu-id="ef06a-318">**Motivation:** consequence of restriction on instance fields.</span></span>
- <span data-ttu-id="ef06a-319">ReadOnly struct alan benzeri olayları bildiremeyebilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-319">Readonly struct may not declare field-like events.</span></span>  
<span data-ttu-id="ef06a-320">**Mosyon:** örnek alanlarında kısıtlamanın sonucu.</span><span class="sxs-lookup"><span data-stu-id="ef06a-320">**Motivation:** consequence of restriction on instance fields.</span></span>

### <a name="metadata-representation"></a><span data-ttu-id="ef06a-321">Meta veri gösterimi.</span><span class="sxs-lookup"><span data-stu-id="ef06a-321">Metadata representation.</span></span>
<span data-ttu-id="ef06a-322">Değer türüne `System.Runtime.CompilerServices.IsReadOnlyAttribute` uygulandığında, türün bir `readonly struct`olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-322">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to a value type, it means that the type is a `readonly struct`.</span></span>

<span data-ttu-id="ef06a-323">Özellikle:</span><span class="sxs-lookup"><span data-stu-id="ef06a-323">In particular:</span></span>
-  <span data-ttu-id="ef06a-324">`IsReadOnlyAttribute` türünün kimliği önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-324">The identity of the `IsReadOnlyAttribute` type is unimportant.</span></span> <span data-ttu-id="ef06a-325">Aslında, gerekirse kapsayan derlemede derleyici tarafından gömülebilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-325">In fact it can be embedded by the compiler in the containing assembly if needed.</span></span>

## <a name="refin-extension-methods"></a><span data-ttu-id="ef06a-326">`ref`/`in` uzantısı yöntemleri</span><span class="sxs-lookup"><span data-stu-id="ef06a-326">`ref`/`in` extension methods</span></span>
<span data-ttu-id="ef06a-327">Aslında mevcut bir teklif (https://github.com/dotnet/roslyn/issues/165) ve karşılık gelen prototip PR (https://github.com/dotnet/roslyn/pull/15650)vardır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-327">There is actually an existing proposal (https://github.com/dotnet/roslyn/issues/165) and corresponding prototype PR (https://github.com/dotnet/roslyn/pull/15650).</span></span>
<span data-ttu-id="ef06a-328">Yalnızca bu fikrin tamamen yeni olduğunu bildirmek istiyorum.</span><span class="sxs-lookup"><span data-stu-id="ef06a-328">I just want to acknowledge that this idea is not entirely new.</span></span> <span data-ttu-id="ef06a-329">Bununla birlikte, bu tür yöntemler hakkında çok sayıda sorunu ortadan kaldırdığından ve RValue alıcılarından ne yapmanız gerektiğini `ref readonly`.</span><span class="sxs-lookup"><span data-stu-id="ef06a-329">It is, however, relevant here since `ref readonly` elegantly removes the most contentious issue about such methods - what to do with RValue receivers.</span></span>

<span data-ttu-id="ef06a-330">Genel fikir, tür bir yapı türü olarak bilindiğinde, uzantı yöntemlerinin `this` parametresini başvuruya göre geçirmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="ef06a-330">The general idea is allowing extension methods to take the `this` parameter by reference, as long as the type is known to be a struct type.</span></span>

```csharp
public static void Extension(ref this Guid self)
{
    // do something
}
```

<span data-ttu-id="ef06a-331">Bu tür uzantı yöntemlerini yazma nedenleri öncelikle şunlardır:</span><span class="sxs-lookup"><span data-stu-id="ef06a-331">The reasons for writing such extension methods are primarily:</span></span>  
1.  <span data-ttu-id="ef06a-332">Alıcı büyük bir yapı olduğunda kopyalamayı önleyin</span><span class="sxs-lookup"><span data-stu-id="ef06a-332">Avoid copying when receiver is a large struct</span></span>
2.  <span data-ttu-id="ef06a-333">Yapı birimlerinde uzantı yöntemlerinin değiştirilmesine izin ver</span><span class="sxs-lookup"><span data-stu-id="ef06a-333">Allow mutating extension methods on structs</span></span>

<span data-ttu-id="ef06a-334">Sınıflarda buna izin vermek istemediğimiz nedenler</span><span class="sxs-lookup"><span data-stu-id="ef06a-334">The reasons why we do not want to allow this on classes</span></span>  
1.  <span data-ttu-id="ef06a-335">Çok sınırlı bir amaç olabilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-335">It would be of very limited purpose.</span></span>
2.  <span data-ttu-id="ef06a-336">Yöntem çağrısının, çağrıdan sonra `null` hale gelmesi için`null` olmayan alıcıyı dönemeyeceği uzun bir bozar bozabilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-336">It would break long standing invariant that a method call cannot turn non-`null` receiver to become `null` after invocation.</span></span>
> <span data-ttu-id="ef06a-337">Aslında, `ref` veya `out`tarafından _açıkça_ atanmamışsa veya geçirilmemişse`null` olmayan bir değişken `null` olamaz.</span><span class="sxs-lookup"><span data-stu-id="ef06a-337">In fact, currently a non-`null` variable cannot become `null` unless _explicitly_ assigned or passed by `ref` or `out`.</span></span>
> <span data-ttu-id="ef06a-338">Okunabilirliği veya diğer "Bu tür", "Bu bir null" analizinden büyük ölçüde yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="ef06a-338">That greatly aids readability or other forms of "can this be a null here" analysis.</span></span>
3.  <span data-ttu-id="ef06a-339">Null koşullu erişimlerin "bir kez değerlendir" semantiğinin uzlanması zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-339">It would be hard to reconcile with "evaluate once" semantics of null-conditional accesses.</span></span>
<span data-ttu-id="ef06a-340">Örnek: `obj.stringField?.RefExtension(...)`-null denetimini anlamlı hale getirmek için bir `stringField` kopyasının yakalanması gerekir, ancak daha sonra RefExtension içindeki `this` atamaları alana geri yansıtılmaz.</span><span class="sxs-lookup"><span data-stu-id="ef06a-340">Example: `obj.stringField?.RefExtension(...)` - need to capture a copy of `stringField` to make the null check meaningful, but then assignments to `this` inside RefExtension would not be reflected back to the field.</span></span>

<span data-ttu-id="ef06a-341">Başvuruya göre ilk bağımsız değişkeni alan **yapılar** üzerinde uzantı yöntemleri bildirme yeteneği uzun süreli bir istek idi.</span><span class="sxs-lookup"><span data-stu-id="ef06a-341">An ability to declare extension methods on **structs** that take the first argument by reference was a long-standing request.</span></span> <span data-ttu-id="ef06a-342">Engellenmeden biri "alıcı LValue değilse ne olur?" idi.</span><span class="sxs-lookup"><span data-stu-id="ef06a-342">One of the blocking consideration was "what happens if receiver is not an LValue?".</span></span>

- <span data-ttu-id="ef06a-343">Herhangi bir uzantı yönteminin statik bir yöntem olarak da çağrılacağından, bazı durumlarda belirsizlik çözümlenmenin tek yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-343">There is a precedent that any extension method could also be called as a static method (sometimes it is the only way to resolve ambiguity).</span></span> <span data-ttu-id="ef06a-344">RValue alıcılarının izin verilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-344">It would dictate that RValue receivers should be disallowed.</span></span>
- <span data-ttu-id="ef06a-345">Öte yandan, yapı örneği yöntemleri dahil edildiğinde benzer durumlarda bir kopyaya çağrı yapma yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-345">On the other hand there is a practice of making invocation on a copy in similar situations when struct instance methods are involved.</span></span>

<span data-ttu-id="ef06a-346">"Örtük kopyalama" olmasının nedeni, yapı yöntemlerinin büyük çoğunluğunun, bunu belirtemediği sürece yapıyı gerçekten değiştirmeleridir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-346">The reason why the "implicit copying" exists is because the majority of struct methods do not actually modify the struct while not being able to indicate that.</span></span> <span data-ttu-id="ef06a-347">Bu nedenle en pratik çözüm, yalnızca bir kopyaya çağrı yapmak için, ancak bu uygulama, çok fazla performans ve hatalara neden olduğu bilinmektedir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-347">Therefore the most practical solution was to just make the invocation on a copy, but this practice is known for harming performance and causing bugs.</span></span>

<span data-ttu-id="ef06a-348">Artık `in` parametrelerinin kullanılabilirliğiyle, bir uzantının amacı işaret etmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="ef06a-348">Now, with availability of `in` parameters, it is possible for an extension to signal the intent.</span></span> <span data-ttu-id="ef06a-349">Bu nedenle, `in` uzantıları gerektiğinde örtük kopyalamaya izin verdiğinden `ref` uzantılarının yazılabilir alıcılarla çağrılması zorunlu kılarak Conundrum çözülebilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-349">Therefore the conundrum can be resolved by requiring `ref` extensions to be called with writeable receivers while `in` extensions permit implicit copying if necessary.</span></span>

```csharp
// this can be called on either RValue or an LValue
public static void Reader(in this Guid self)
{
    // do something nonmutating.
    WriteLine(self == default(Guid));
}

// this can be called only on an LValue
public static void Mutator(ref this Guid self)
{
    // can mutate self
    self = new Guid();
}
```

### <a name="in-extensions-and-generics"></a><span data-ttu-id="ef06a-350">`in` uzantıları ve genel türler.</span><span class="sxs-lookup"><span data-stu-id="ef06a-350">`in` extensions and generics.</span></span>
<span data-ttu-id="ef06a-351">`ref` uzantısı yöntemlerinin amacı, alıcıyı doğrudan veya üye değiştirici çağırarak çağırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-351">The purpose of `ref` extension methods is to mutate the receiver directly or by invoking mutating members.</span></span> <span data-ttu-id="ef06a-352">Bu nedenle, `T` bir struct olarak kısıtlandığı sürece `ref this T` uzantılarına izin verilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-352">Therefore `ref this T` extensions are allowed as long as `T` is constrained to be a struct.</span></span>

<span data-ttu-id="ef06a-353">Diğer taraftan `in` genişletme yöntemleri özellikle örtülü kopyalamayı azaltmak için vardır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-353">On the other hand `in` extension methods exist specifically to reduce implicit copying.</span></span> <span data-ttu-id="ef06a-354">Ancak, bir `in T` parametresinin tüm kullanımı bir arabirim üyesi aracılığıyla yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-354">However any use of an `in T` parameter will have to be done through an interface member.</span></span> <span data-ttu-id="ef06a-355">Tüm arabirim üyeleri değişikliğe karşı kabul edildiğinden, bu tür bir kullanım için bir kopya gerekir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-355">Since all interface members are considered mutating, any such use would require a copy.</span></span> <span data-ttu-id="ef06a-356">-Kopyalamayı azaltmak yerine, efekt tersi olur.</span><span class="sxs-lookup"><span data-stu-id="ef06a-356">- Instead of reducing copying, the effect would be the opposite.</span></span> <span data-ttu-id="ef06a-357">Bu nedenle, `T` bir genel tür parametresi kısıtlamalardan bağımsız olarak `in this T` izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="ef06a-357">Therefore `in this T` is not allowed when `T` is a generic type parameter regardless of constraints.</span></span>

### <a name="valid-kinds-of-extension-methods-recap"></a><span data-ttu-id="ef06a-358">Geçerli uzantı yöntemleri türleri (Recap):</span><span class="sxs-lookup"><span data-stu-id="ef06a-358">Valid kinds of extension methods (recap):</span></span>
<span data-ttu-id="ef06a-359">Bir genişletme yönteminde aşağıdaki `this` bildirimine yönelik olarak şu formlara izin verilir:</span><span class="sxs-lookup"><span data-stu-id="ef06a-359">The following forms of `this` declaration in an extension method are now allowed:</span></span>
1) <span data-ttu-id="ef06a-360">`this T arg`-normal ByVal uzantısı.</span><span class="sxs-lookup"><span data-stu-id="ef06a-360">`this T arg` - regular byval extension.</span></span> <span data-ttu-id="ef06a-361">(**mevcut durum**)</span><span class="sxs-lookup"><span data-stu-id="ef06a-361">(**existing case**)</span></span>
- <span data-ttu-id="ef06a-362">T, başvuru türleri veya tür parametreleri de dahil olmak üzere herhangi bir tür olabilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-362">T can be any type, including reference types or type parameters.</span></span>
<span data-ttu-id="ef06a-363">Örnek, çağrıdan sonra aynı değişken olacaktır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-363">Instance will be the same variable after the call.</span></span>
<span data-ttu-id="ef06a-364">_Bu bağımsız değişken dönüştürme_ türünün örtük dönüştürmelerine izin verir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-364">Allows implicit conversions of _this-argument-conversion_ kind.</span></span>
<span data-ttu-id="ef06a-365">, RValues üzerinde çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-365">Can be called on RValues.</span></span>

- <span data-ttu-id="ef06a-366">`in this T self` - `in` uzantısı.</span><span class="sxs-lookup"><span data-stu-id="ef06a-366">`in this T self` - `in` extension.</span></span>
<span data-ttu-id="ef06a-367">T gerçek bir yapı türü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-367">T must be an actual struct type.</span></span>
<span data-ttu-id="ef06a-368">Örnek, çağrıdan sonra aynı değişken olacaktır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-368">Instance will be the same variable after the call.</span></span>
<span data-ttu-id="ef06a-369">_Bu bağımsız değişken dönüştürme_ türünün örtük dönüştürmelerine izin verir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-369">Allows implicit conversions of _this-argument-conversion_ kind.</span></span>
<span data-ttu-id="ef06a-370">, RValues üzerinde çağrılabilir (gerekirse geçici bir durum üzerinde çağrılabilir).</span><span class="sxs-lookup"><span data-stu-id="ef06a-370">Can be called on RValues (may be invoked on a temp if needed).</span></span>

- <span data-ttu-id="ef06a-371">`ref this T self` - `ref` uzantısı.</span><span class="sxs-lookup"><span data-stu-id="ef06a-371">`ref this T self` - `ref` extension.</span></span>
<span data-ttu-id="ef06a-372">T bir struct türü ya da bir struct olarak kısıtlanmış genel tür parametresi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-372">T must be a struct type or a generic type parameter constrained to be a struct.</span></span>
<span data-ttu-id="ef06a-373">Örnek, çağırma tarafından yazılmış olabilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-373">Instance may be written to by the invocation.</span></span>
<span data-ttu-id="ef06a-374">Yalnızca kimlik dönüştürmelerine izin verir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-374">Allows only identity conversions.</span></span>
<span data-ttu-id="ef06a-375">Yazılabilir LValue üzerinde çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-375">Must be called on writeable LValue.</span></span> <span data-ttu-id="ef06a-376">(bir geçici aracılığıyla hiçbir şekilde çağırılmaz).</span><span class="sxs-lookup"><span data-stu-id="ef06a-376">(never invoked via a temp).</span></span>

## <a name="readonly-ref-locals"></a><span data-ttu-id="ef06a-377">Salt okunur başvuru yerelleri.</span><span class="sxs-lookup"><span data-stu-id="ef06a-377">Readonly ref locals.</span></span>

### <a name="motivation"></a><span data-ttu-id="ef06a-378">Amacı.</span><span class="sxs-lookup"><span data-stu-id="ef06a-378">Motivation.</span></span>
<span data-ttu-id="ef06a-379">`ref readonly` Üyeler tanıtıldıktan sonra, uygun tür yerel ile eşleştirilmeleri gereken kullanımı ortadan kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="ef06a-379">Once `ref readonly` members were introduced, it was clear from the use that they need to be paired with appropriate kind of local.</span></span> <span data-ttu-id="ef06a-380">Bir üyenin değerlendirilmesi yan etkileri oluşturabilir veya gözlemlenebilir, bu nedenle sonuç birden çok kez kullanılacaksa, depolanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-380">Evaluation of a member may produce or observe side effects, therefore if the result must be used more than once, it needs to be stored.</span></span> <span data-ttu-id="ef06a-381">Sıradan `ref` Yereller, `readonly` bir başvuruya atanmadığından burada yardım etmez.</span><span class="sxs-lookup"><span data-stu-id="ef06a-381">Ordinary `ref` locals do not help here since they cannot be assigned a `readonly` reference.</span></span>   

### <a name="solution"></a><span data-ttu-id="ef06a-382">Çözümden.</span><span class="sxs-lookup"><span data-stu-id="ef06a-382">Solution.</span></span>
<span data-ttu-id="ef06a-383">`ref readonly` yerelleri bildirmek için izin verin.</span><span class="sxs-lookup"><span data-stu-id="ef06a-383">Allow declaring `ref readonly` locals.</span></span> <span data-ttu-id="ef06a-384">Bu, yazılabilir olmayan `ref` yeni yereltür.</span><span class="sxs-lookup"><span data-stu-id="ef06a-384">This is a new kind of `ref` locals that is not writeable.</span></span> <span data-ttu-id="ef06a-385">Sonuç olarak `ref readonly` Yereller, yazma işlemleri için bu değişkenleri kullanıma açmadan ReadOnly değişkenlerine başvuruları kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-385">As a result `ref readonly` locals can accept references to readonly variables without exposing these variables to writes.</span></span>

### <a name="declaring-and-using-ref-readonly-locals"></a><span data-ttu-id="ef06a-386">`ref readonly` Yereller bildirme ve kullanma.</span><span class="sxs-lookup"><span data-stu-id="ef06a-386">Declaring and using `ref readonly` locals.</span></span>

<span data-ttu-id="ef06a-387">Bu tür Yereller söz dizimi, bildirim sitesinde (söz konusu sırada) `ref readonly` değiştiricileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-387">The syntax of such locals uses `ref readonly` modifiers at declaration site (in that specific order).</span></span> <span data-ttu-id="ef06a-388">Sıradan `ref` Yerellerden benzer şekilde, `ref readonly` Yereller, bildirimde, ref tarafından başlatılmış olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-388">Similarly to ordinary `ref` locals, `ref readonly` locals must be ref-initialized at declaration.</span></span> <span data-ttu-id="ef06a-389">Normal `ref` Yerellerden farklı olarak, `ref readonly` Yereller `in` parametreler, `readonly` alanları `ref readonly` Yöntemler gibi `readonly` LValues 'a başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-389">Unlike regular `ref` locals, `ref readonly` locals can refer to `readonly` LValues like `in` parameters, `readonly` fields, `ref readonly` methods.</span></span>

<span data-ttu-id="ef06a-390">Tüm amaçlar için bir `ref readonly` yerel bir `readonly` değişken olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-390">For all purposes a `ref readonly` local is treated as a `readonly` variable.</span></span> <span data-ttu-id="ef06a-391">Kullanım üzerindeki kısıtlamaların çoğu `readonly` alanlarla veya `in` parametreleriyle aynıdır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-391">Most of the restrictions on the use are the same as with `readonly` fields or `in` parameters.</span></span>

<span data-ttu-id="ef06a-392">Örneğin, bir yapı türüne sahip bir `in` parametresinin alanları, `readonly` değişkenleri olarak özyinelemeli olarak sınıflandırıldı.</span><span class="sxs-lookup"><span data-stu-id="ef06a-392">For example fields of an `in` parameter which has a struct type are all recursively classified as `readonly` variables .</span></span>   

```csharp
static readonly ref Vector3 M1() => . . .

static readonly ref Vector3 M1_Trace()
{
    // OK
    ref readonly var r1 = ref M1();

    // Not valid. Need an LValue
    ref readonly Vector3 r2 = ref default(Vector3);

    // Not valid. r1 is readonly.
    Mutate(ref r1);

    // OK.
    Print(in r1);

    // OK.
    return ref r1;
}
```

### <a name="restrictions-on-use-of-ref-readonly-locals"></a><span data-ttu-id="ef06a-393">`ref readonly` Yereller kullanımıyla ilgili kısıtlamalar</span><span class="sxs-lookup"><span data-stu-id="ef06a-393">Restrictions on use of `ref readonly` locals</span></span>
<span data-ttu-id="ef06a-394">`readonly` doğası haricinde, `ref readonly` Yereller sıradan `ref` Yereller gibi davranır ve tamamen aynı kısıtlamalara tabidir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-394">Except for their `readonly` nature, `ref readonly` locals behave like ordinary `ref` locals and are subject to exactly same restrictions.</span></span>  
<span data-ttu-id="ef06a-395">Örneğin, kapanışlarda yakalamaya yönelik kısıtlamalar, `async` yöntemleri veya `safe-to-return` analizini bildirmek `ref readonly` Yereller için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-395">For example restrictions related to capturing in closures, declaring in `async` methods or the `safe-to-return` analysis equally applies to `ref readonly` locals.</span></span>

## <a name="ternary-ref-expressions-aka-conditional-lvalues"></a><span data-ttu-id="ef06a-396">Üçlü `ref` ifadeleri.</span><span class="sxs-lookup"><span data-stu-id="ef06a-396">Ternary `ref` expressions.</span></span> <span data-ttu-id="ef06a-397">(diğer adıyla "şartlı LValues")</span><span class="sxs-lookup"><span data-stu-id="ef06a-397">(aka "Conditional LValues")</span></span>

### <a name="motivation"></a><span data-ttu-id="ef06a-398">Amacı</span><span class="sxs-lookup"><span data-stu-id="ef06a-398">Motivation</span></span>
<span data-ttu-id="ef06a-399">`ref` kullanımı ve `ref readonly` Yereller, bir koşula bağlı olarak bir veya başka bir hedef değişkeni ile bu tür yerelleri bir veya daha fazla yerelden başlatmaya gerek</span><span class="sxs-lookup"><span data-stu-id="ef06a-399">Use of `ref` and `ref readonly` locals exposed a need to ref-initialize such locals with one or another target variable based on a condition.</span></span>

<span data-ttu-id="ef06a-400">Tipik bir geçici çözüm, şunun gibi bir yöntemi tanıtmaktadır:</span><span class="sxs-lookup"><span data-stu-id="ef06a-400">A typical workaround is to introduce a method like:</span></span>

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

<span data-ttu-id="ef06a-401">_Tüm_ bağımsız değişkenlerin, sezgisel olmayan davranış ve hatalara yönelik olarak önde gelen çağrı sitesinde değerlendirilmesi gerektiğinden, `Choice` üçlü bir değiştirme değildir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-401">Note that `Choice` is not an exact replacement of a ternary since _all_ arguments must be evaluated at the call site, which was leading to unintuitive behavior and bugs.</span></span>

<span data-ttu-id="ef06a-402">Aşağıdakiler beklendiği gibi çalışmayacak:</span><span class="sxs-lookup"><span data-stu-id="ef06a-402">The following will not work as expected:</span></span>

```csharp
    // will crash with NRE because 'arr[0]' will be executed unconditionally
    ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

### <a name="solution"></a><span data-ttu-id="ef06a-403">Çözüm</span><span class="sxs-lookup"><span data-stu-id="ef06a-403">Solution</span></span>
<span data-ttu-id="ef06a-404">Bir koşula bağlı olarak LValue bağımsız değişkeninden birine başvuru olarak değerlendirilen, özel tür koşullu ifadeye izin verin.</span><span class="sxs-lookup"><span data-stu-id="ef06a-404">Allow special kind of conditional expression that evaluates to a reference to one of LValue argument based on a condition.</span></span>

### <a name="using-ref-ternary-expression"></a><span data-ttu-id="ef06a-405">`ref` Üçlü ifade kullanma.</span><span class="sxs-lookup"><span data-stu-id="ef06a-405">Using `ref` ternary expression.</span></span>

<span data-ttu-id="ef06a-406">Koşullu bir ifadenin `ref` Flavor için sözdizimi `<condition> ? ref <consequence> : ref <alternative>;`</span><span class="sxs-lookup"><span data-stu-id="ef06a-406">The syntax for the `ref` flavor of a conditional expression is `<condition> ? ref <consequence> : ref <alternative>;`</span></span>

<span data-ttu-id="ef06a-407">Yalnızca normal koşullu ifade gibi `<consequence>` veya `<alternative>`, Boolean koşul ifadesinin sonucuna bağlı olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-407">Just like with the ordinary conditional expression only `<consequence>` or `<alternative>` is evaluated depending on result of the boolean condition expression.</span></span>

<span data-ttu-id="ef06a-408">Sıradan koşullu ifadenin aksine, koşullu ifade `ref`:</span><span class="sxs-lookup"><span data-stu-id="ef06a-408">Unlike ordinary conditional expression, `ref` conditional expression:</span></span>
- <span data-ttu-id="ef06a-409">`<consequence>` ve `<alternative>` LValues gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-409">requires that `<consequence>` and `<alternative>` are LValues.</span></span>
- <span data-ttu-id="ef06a-410">`ref` koşullu ifadenin kendisi bir LValue ve</span><span class="sxs-lookup"><span data-stu-id="ef06a-410">`ref` conditional expression itself is an LValue and</span></span>
- <span data-ttu-id="ef06a-411">hem `<consequence>` hem de `<alternative>` yazılabilir LValues ise `ref` koşullu ifade yazılabilir</span><span class="sxs-lookup"><span data-stu-id="ef06a-411">`ref` conditional expression is writeable if both `<consequence>` and `<alternative>` are writeable LValues</span></span>

<span data-ttu-id="ef06a-412">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="ef06a-412">Examples:</span></span>  
<span data-ttu-id="ef06a-413">`ref` üçlü bir LValue ve başvuruya göre geçirilebilir/atanabilir/döndürülebilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-413">`ref` ternary is an LValue and as such it can be passed/assigned/returned by reference;</span></span>
```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="ef06a-414">LValue olarak da atanabilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-414">Being an LValue, it can also be assigned to.</span></span>
```csharp
     // assign to
     (arr != null ? ref arr[0]: ref otherArr[0]) = 1;

     // error. readOnlyField is readonly and thus conditional expression is readonly
     (arr != null ? ref arr[0]: ref obj.readOnlyField) = 1;
```

<span data-ttu-id="ef06a-415">, Bir yöntem çağrısının alıcısı olarak kullanılabilir ve gerekirse kopyalamayı atlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef06a-415">Can be used as a receiver of a method call and skip copying if necessary.</span></span>
```csharp
     // no copies
     (arr != null ? ref arr[0]: ref otherArr[0]).StructMethod();

     // invoked on a copy.
     // The receiver is `readonly` because readOnlyField is readonly.
     (arr != null ? ref arr[0]: ref obj.readOnlyField).StructMethod();

     // no copies. `ReadonlyStructMethod` is a method on a `readonly` struct
     // and can be invoked directly on a readonly receiver
     (arr != null ? ref arr[0]: ref obj.readOnlyField).ReadonlyStructMethod();
```

<span data-ttu-id="ef06a-416">`ref` Üçlü, normal (Ref değil) bağlamda de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-416">`ref` ternary can be used in a regular (not ref) context as well.</span></span>
```csharp
     // only an example
     // a regular ternary could work here just the same
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```

### <a name="drawbacks"></a><span data-ttu-id="ef06a-417">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="ef06a-417">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="ef06a-418">Başvurular ve salt okunur başvurular için gelişmiş desteğe karşı iki önemli bağımsız değişken görebiliyorum:</span><span class="sxs-lookup"><span data-stu-id="ef06a-418">I can see two major arguments against enhanced support for references and readonly references:</span></span>

1) <span data-ttu-id="ef06a-419">Burada çözülen sorunlar çok eski.</span><span class="sxs-lookup"><span data-stu-id="ef06a-419">The problems that are solved here are very old.</span></span> <span data-ttu-id="ef06a-420">Özellikle de, var olan koda yardımcı olmadığından, neden daha önce bu dosyaları şimdi çözmektedir?</span><span class="sxs-lookup"><span data-stu-id="ef06a-420">Why suddenly solve them now, especially since it would not help existing code?</span></span>

<span data-ttu-id="ef06a-421">Yeni etki alanlarında C# bulduğumuz ve .NET tarafından kullanıldıkları gibi bazı sorunlar daha belirgin hale gelir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-421">As we find C# and .Net used in new domains, some problems become more prominent.</span></span>  
<span data-ttu-id="ef06a-422">Hesaplama fazla kafaları hakkında ortalamaya göre daha kritik olan ortamlara örnek olarak, şunları Listelerim</span><span class="sxs-lookup"><span data-stu-id="ef06a-422">As examples of environments that are more critical than average about computation overheads, I can list</span></span>

* <span data-ttu-id="ef06a-423">hesaplamanın faturalandırılması ve yanıt verebildiği bulut/veri merkezi senaryoları rekabetçi bir avantajdır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-423">cloud/datacenter scenarios where computation is billed for and responsiveness is a competitive advantage.</span></span>
* <span data-ttu-id="ef06a-424">Gecikme sürelerinde geçici gerçek zamanlı gereksinimlere sahip Oyunlar/VR/AR</span><span class="sxs-lookup"><span data-stu-id="ef06a-424">Games/VR/AR with soft-realtime requirements on latencies</span></span>     

<span data-ttu-id="ef06a-425">Bu özellik, bazı yaygın senaryolarda daha fazla gözlerine izin verirken tür-güvenlik gibi mevcut güçlerin hiçbirini etkilemez.</span><span class="sxs-lookup"><span data-stu-id="ef06a-425">This feature does not sacrifice any of the existing strengths such as type-safety, while allowing to lower overheads in some common scenarios.</span></span>

2) <span data-ttu-id="ef06a-426">`readonly` sözleşmelerde, çağrılan kuralların kuralları tarafından oynamasını makul bir şekilde garanti edebilir misiniz?</span><span class="sxs-lookup"><span data-stu-id="ef06a-426">Can we reasonably guarantee that the callee will play by the rules when it opts into `readonly` contracts?</span></span>

<span data-ttu-id="ef06a-427">`out`kullanırken buna benzer bir güveniz var.</span><span class="sxs-lookup"><span data-stu-id="ef06a-427">We have similar trust when using `out`.</span></span> <span data-ttu-id="ef06a-428">Yanlış `out` uygulanması belirtilmeyen davranışa neden olabilir, ancak gerçekte nadiren meydana gelir.</span><span class="sxs-lookup"><span data-stu-id="ef06a-428">Incorrect implementation of `out` can cause unspecified behavior, but in reality it rarely happens.</span></span>  

<span data-ttu-id="ef06a-429">Resmi doğrulama kurallarının `ref readonly` tanıdık getirilmesi, güven sorununu daha da azaltır.</span><span class="sxs-lookup"><span data-stu-id="ef06a-429">Making the formal verification rules familiar with `ref readonly` would further mitigate the trust issue.</span></span>

### <a name="alternatives"></a><span data-ttu-id="ef06a-430">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="ef06a-430">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="ef06a-431">Asıl rekabet tasarımı gerçekten "hiçbir şey yapmaz".</span><span class="sxs-lookup"><span data-stu-id="ef06a-431">The main competing design is really "do nothing".</span></span>

### <a name="unresolved-questions"></a><span data-ttu-id="ef06a-432">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="ef06a-432">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

### <a name="design-meetings"></a><span data-ttu-id="ef06a-433">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="ef06a-433">Design meetings</span></span>

<span data-ttu-id="ef06a-434">https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md</span><span class="sxs-lookup"><span data-stu-id="ef06a-434">https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md</span></span>
