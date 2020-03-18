---
ms.openlocfilehash: 91afbc3e3412049cd183c36c8035f1862c520413
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2019
ms.locfileid: "79485058"
---
# <a name="pattern-based-using-and-using-declarations"></a><span data-ttu-id="d1783-101">"model tabanlı kullanımı" ve "bildirimleri kullanma"</span><span class="sxs-lookup"><span data-stu-id="d1783-101">"pattern-based using" and "using declarations"</span></span>

## <a name="summary"></a><span data-ttu-id="d1783-102">Özet</span><span class="sxs-lookup"><span data-stu-id="d1783-102">Summary</span></span>

<span data-ttu-id="d1783-103">Dil, Kaynak yönetiminin daha basit olması için `using` bildirimi etrafında iki yeni özellik ekler: `using`, `IDisposable` ek olarak bir atılabilir deseninin tanınması ve dile bir `using` bildirimi eklemektir.</span><span class="sxs-lookup"><span data-stu-id="d1783-103">The language will add two new capabilities around the `using` statement in order to make resource management simpler: `using` should recognize a disposable pattern in addition to `IDisposable` and add a `using` declaration to the language.</span></span>

## <a name="motivation"></a><span data-ttu-id="d1783-104">Amacı</span><span class="sxs-lookup"><span data-stu-id="d1783-104">Motivation</span></span>

<span data-ttu-id="d1783-105">`using` ifade, bugün kaynak yönetimi için etkili bir araçtır, ancak tam olarak biraz sayıda sertifika gerektirir.</span><span class="sxs-lookup"><span data-stu-id="d1783-105">The `using` statement is an effective tool for resource management today but it requires quite a bit of ceremony.</span></span> <span data-ttu-id="d1783-106">Yönetilecek sayıda kaynağı olan Yöntemler, bir dizi `using` deyimleriyle sözdizimsel sürüklenmeden altına alabilir.</span><span class="sxs-lookup"><span data-stu-id="d1783-106">Methods that have a number of resources to manage can get syntactically bogged down with a series of `using` statements.</span></span> <span data-ttu-id="d1783-107">Bu sözdizimi yükü, çoğu kodlama stili yönergelerinin bu senaryo için küme ayraçları etrafında açık bir özel duruma sahip olması yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="d1783-107">This syntax burden is enough that most coding style guidelines explicitly have an exception around braces for this scenario.</span></span> 

<span data-ttu-id="d1783-108">`using` bildirimi buradaki pek çok sertifikayı ortadan kaldırır ve kaynak yönetim bloklarını içeren C# diğer dillere göre değer alır.</span><span class="sxs-lookup"><span data-stu-id="d1783-108">The `using` declaration removes much of the ceremony here and gets C# on par with other languages that include resource management blocks.</span></span> <span data-ttu-id="d1783-109">Ayrıca, model tabanlı `using`, geliştiricilerin buraya katılabileceğiniz tür kümesini genişletmesine imkan tanır.</span><span class="sxs-lookup"><span data-stu-id="d1783-109">Additionally the pattern-based `using` lets developers expand the set of types that can participate here.</span></span> <span data-ttu-id="d1783-110">Birçok durumda, yalnızca bir `using` bildiriminde bir değer kullanımına izin vermek için var olan sarmalayıcı türleri oluşturma ihtiyacını ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="d1783-110">In many cases removing the need to create wrapper types that only exist to allow for a values use in a `using` statement.</span></span> 

<span data-ttu-id="d1783-111">Bu özellikler, geliştiricilerin `using` uygulanabilecek senaryoları basitleştirmesini ve genişletmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="d1783-111">Together these features allow developers to simplify and expand the scenarios where `using` can be applied.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="d1783-112">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="d1783-112">Detailed Design</span></span> 

### <a name="using-declaration"></a><span data-ttu-id="d1783-113">using bildirimi</span><span class="sxs-lookup"><span data-stu-id="d1783-113">using declaration</span></span>

<span data-ttu-id="d1783-114">Dil, `using` yerel bir değişken bildirimine eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="d1783-114">The language will allow for `using` to be added to a local variable declaration.</span></span> <span data-ttu-id="d1783-115">Bu tür bir bildirim, aynı konumdaki `using` deyimindeki değişkeni bildirerek aynı etkiye sahip olacaktır.</span><span class="sxs-lookup"><span data-stu-id="d1783-115">Such a declaration will have the same effect as declaring the variable in a `using` statement at the same location.</span></span>

```csharp
if (...) 
{ 
   using FileStream f = new FileStream(@"C:\users\jaredpar\using.md");
   // statements
}

// Equivalent to 
if (...) 
{ 
   using (FileStream f = new FileStream(@"C:\users\jaredpar\using.md")) 
   {
    // statements
   }
}
```

<span data-ttu-id="d1783-116">Bir yerel `using` ömrü, bildirildiği kapsamın sonuna genişletilir.</span><span class="sxs-lookup"><span data-stu-id="d1783-116">The lifetime of a `using` local will extend to the end of the scope in which it is declared.</span></span> <span data-ttu-id="d1783-117">`using` Yereller, bildirildiği ters sırada atılecektir.</span><span class="sxs-lookup"><span data-stu-id="d1783-117">The `using` locals will then be disposed in the reverse order in which they are declared.</span></span> 

```csharp
{ 
    using var f1 = new FileStream("...");
    using var f2 = new FileStream("..."), f3 = new FileStream("...");
    ...
    // Dispose f3
    // Dispose f2 
    // Dispose f1
}
```

<span data-ttu-id="d1783-118">`goto`ya da başka bir denetim akışı yapısının `using` bildirimi çerçevesinde hiçbir kısıtlama yoktur.</span><span class="sxs-lookup"><span data-stu-id="d1783-118">There are no restrictions around `goto`, or any other control flow construct in the face of a `using` declaration.</span></span> <span data-ttu-id="d1783-119">Bunun yerine, kod yalnızca eşdeğer `using` ifadesiyle olduğu gibi davranır:</span><span class="sxs-lookup"><span data-stu-id="d1783-119">Instead the code acts just as it would for the equivalent `using` statement:</span></span>

```csharp
{
    using var f1 = new FileStream("...");
  target:
    using var f2 = new FileStream("...");
    if (someCondition) 
    {
        // Causes f2 to be disposed but has no effect on f1
        goto target;
    }
}
```

<span data-ttu-id="d1783-120">`using` yerel bildiriminde belirtilen yerel, örtük olarak salt okunurdur.</span><span class="sxs-lookup"><span data-stu-id="d1783-120">A local declared in a `using` local declaration will be implicitly read-only.</span></span> <span data-ttu-id="d1783-121">Bu, bir `using` bildiriminde belirtilen Yereller davranışıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="d1783-121">This matches the behavior of locals declared in a `using` statement.</span></span> 

<span data-ttu-id="d1783-122">`using` bildirimleri için dil dilbilgisi aşağıdaki gibi olacaktır:</span><span class="sxs-lookup"><span data-stu-id="d1783-122">The language grammar for `using` declarations will be the following:</span></span>

```antlr
local-using-declaration:
  using type using-declarators

using-declarators:
  using-declarator
  using-declarators , using-declarator
  
using-declarator:
  identifier = expression
```

<span data-ttu-id="d1783-123">`using` bildirimi etrafındaki kısıtlamalar:</span><span class="sxs-lookup"><span data-stu-id="d1783-123">Restrictions around `using` declaration:</span></span>

- <span data-ttu-id="d1783-124">Doğrudan bir `case` etiketinin içinde görünmeyebilir, bunun yerine `case` etiketi içindeki bir blok dahilinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d1783-124">May not appear directly inside a `case` label but instead must be within a block inside the `case` label.</span></span>
- <span data-ttu-id="d1783-125">`out` değişken bildiriminin parçası olarak görünmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="d1783-125">May not appear as part of an `out` variable declaration.</span></span> 
- <span data-ttu-id="d1783-126">Her bildirimci için bir başlatıcıya sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d1783-126">Must have an initializer for each declarator.</span></span>
- <span data-ttu-id="d1783-127">Yerel türün `IDisposable` için örtük olarak dönüştürülebilir olması veya `using` deseninin yerine getirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="d1783-127">The local type must be implicitly convertible to `IDisposable` or fulfill the `using` pattern.</span></span>

### <a name="pattern-based-using"></a><span data-ttu-id="d1783-128">model tabanlı using</span><span class="sxs-lookup"><span data-stu-id="d1783-128">pattern-based using</span></span>

<span data-ttu-id="d1783-129">Dil bir atılabilir deseninin kavramını ekler: Bu, erişilebilir bir `Dispose` örnek yöntemi olan bir türdür.</span><span class="sxs-lookup"><span data-stu-id="d1783-129">The language will add the notion of a disposable pattern: that is a type which has an accessible `Dispose` instance method.</span></span> <span data-ttu-id="d1783-130">Atılabilir düzenine uyan türler, `IDisposable`uygulanması gerekmeden `using` ifadeye veya bildirimine katılabilir.</span><span class="sxs-lookup"><span data-stu-id="d1783-130">Types which fit the disposable pattern can participate in a `using` statement or declaration without being required to implement `IDisposable`.</span></span> 

```csharp
class Resource
{ 
    public void Dispose() { ... }
}

using (var r = new Resource())
{
    // statements
}
```

<span data-ttu-id="d1783-131">Bu, geliştiricilerin birçok yeni senaryoda `using` yararlanmasını sağlar:</span><span class="sxs-lookup"><span data-stu-id="d1783-131">This will allow developers to leverage `using` in a number of new scenarios:</span></span>

- <span data-ttu-id="d1783-132">`ref struct`: Bu türler, arabirimler uygulayamaz ve bu nedenle `using` deyimlerine katılamaz.</span><span class="sxs-lookup"><span data-stu-id="d1783-132">`ref struct`: These types can't implement interfaces today and hence can't participate in `using` statements.</span></span>
- <span data-ttu-id="d1783-133">Uzantı yöntemleri, geliştiricilerin `using` deyimlerine katılmak için diğer derlemelerdeki türleri genişletmelerine izin verir.</span><span class="sxs-lookup"><span data-stu-id="d1783-133">Extension methods will allow developers to augment types in other assemblies to participate in `using` statements.</span></span>

<span data-ttu-id="d1783-134">Bir türün `IDisposable` örtük olarak dönüştürülebileceği ve ayrıca atılabilir düzenine uygun olduğu durumlarda, `IDisposable` tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="d1783-134">In the situation where a type can be implicitly converted to `IDisposable` and also fits the disposable pattern, then `IDisposable` will be preferred.</span></span> <span data-ttu-id="d1783-135">Bu, `foreach` karşı yaklaşımla (arabirim üzerinden tercih edilen model), geriye doğru uyumluluk için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="d1783-135">While this takes the opposite approach of `foreach` (pattern preferred over interface) it is necessary for backwards compatibility.</span></span>

<span data-ttu-id="d1783-136">Geleneksel bir `using` deyimden aynı kısıtlamalar burada da geçerlidir: `using` olarak belirtilen yerel değişkenler salt okunurdur, bir `null` değeri özel durumun oluşturulmasına neden olmaz, vb... Kod üretimi yalnızca, Dispose çağrılmadan önce `IDisposable` için bir dönüştürme olmayacak şekilde farklı olacaktır:</span><span class="sxs-lookup"><span data-stu-id="d1783-136">The same restrictions from a traditional `using` statement apply here as well: local variables declared in the `using` are read-only, a `null` value will not cause an exception to be thrown, etc ... The code generation will be different only in that there will not be a cast to `IDisposable` before calling Dispose:</span></span>

```csharp
{
      Resource r = new Resource();
      try {
            // statements
      }
      finally {
            if (resource != null) resource.Dispose();
      }
}
```

<span data-ttu-id="d1783-137">Atılabilir düzenine sığması için `Dispose` yöntemi erişilebilir, parametresiz ve `void` bir dönüş türüne sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d1783-137">In order to fit the disposable pattern the `Dispose` method must be accessible, parameterless and have a `void` return type.</span></span> <span data-ttu-id="d1783-138">Başka kısıtlama yoktur.</span><span class="sxs-lookup"><span data-stu-id="d1783-138">There are no other restrictions.</span></span> <span data-ttu-id="d1783-139">Bu açıkça, uzantı yöntemlerinin burada kullanılabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="d1783-139">This explicitly means that extension methods can be used here.</span></span>

## <a name="considerations"></a><span data-ttu-id="d1783-140">Dikkat edilmesi gerekenler</span><span class="sxs-lookup"><span data-stu-id="d1783-140">Considerations</span></span>

### <a name="case-labels-without-blocks"></a><span data-ttu-id="d1783-141">bloklar olmadan Case etiketleri</span><span class="sxs-lookup"><span data-stu-id="d1783-141">case labels without blocks</span></span>

<span data-ttu-id="d1783-142">`using declaration` gerçek yaşam süresi etrafında karmaşıklıklar nedeniyle doğrudan bir `case` etiketinin içinde geçersizdir.</span><span class="sxs-lookup"><span data-stu-id="d1783-142">A `using declaration` is illegal directly inside a `case` label due to complications around its actual lifetime.</span></span> <span data-ttu-id="d1783-143">Olası bir çözüm aynı yaşam süresine aynı konumda `out var`.</span><span class="sxs-lookup"><span data-stu-id="d1783-143">One potential solution is to simply give it the same lifetime as an `out var` in the same location.</span></span> <span data-ttu-id="d1783-144">Özellik uygulamasına yönelik ekstra karmaşıklık ve çözüm kolaylığı (yalnızca `case` etiketine bir blok eklemeniz), bu yolun yönlendirilmeyeceğini kabul etmedi.</span><span class="sxs-lookup"><span data-stu-id="d1783-144">It was deemed the extra complexity to the feature implementation and the ease of the work around (just add a block to the `case` label) didn't justify taking this route.</span></span>

## <a name="future-expansions"></a><span data-ttu-id="d1783-145">Geleceğe yönelik genişletmeleri</span><span class="sxs-lookup"><span data-stu-id="d1783-145">Future Expansions</span></span>

### <a name="fixed-locals"></a><span data-ttu-id="d1783-146">Sabit Yereller</span><span class="sxs-lookup"><span data-stu-id="d1783-146">fixed locals</span></span>

<span data-ttu-id="d1783-147">`fixed` deyiminde, `using` yerellerine sahip bir özelliği olan `using` deyimlerinin tüm özellikleri vardır.</span><span class="sxs-lookup"><span data-stu-id="d1783-147">A `fixed` statement has all of the properties of `using` statements that motivated the ability to have `using` locals.</span></span> <span data-ttu-id="d1783-148">Bu özelliği, Yereller de `fixed` genişletmek için dikkat edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="d1783-148">Consideration should be given to extending this feature to `fixed` locals as well.</span></span> <span data-ttu-id="d1783-149">Yaşam süresi ve sıralama kuralları `using` için eşit ölçüde iyi bir şekilde uygulanmalıdır ve burada `fixed`.</span><span class="sxs-lookup"><span data-stu-id="d1783-149">The lifetime and ordering rules should apply equally well for `using` and `fixed` here.</span></span>
