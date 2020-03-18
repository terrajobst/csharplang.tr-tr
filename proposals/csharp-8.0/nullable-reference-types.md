---
ms.openlocfilehash: ecdad8c863d0695bc901e4d96d9ca3decbc248eb
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484617"
---
# <a name="nullable-reference-types-in-c"></a><span data-ttu-id="1330e-101">İçinde null yapılabilir başvuru türleriC#</span><span class="sxs-lookup"><span data-stu-id="1330e-101">Nullable reference types in C#</span></span> #

<span data-ttu-id="1330e-102">Bu özelliğin amacı:</span><span class="sxs-lookup"><span data-stu-id="1330e-102">The goal of this feature is to:</span></span>

* <span data-ttu-id="1330e-103">Geliştiricilerin bir değişkenin, parametrenin veya başvuru türü sonucunun null olması amaçlanıp sunmadığını ifade etmelerine izin verin.</span><span class="sxs-lookup"><span data-stu-id="1330e-103">Allow developers to express whether a variable, parameter or result of a reference type is intended to be null or not.</span></span>
* <span data-ttu-id="1330e-104">Bu amaca göre bu tür değişkenler, parametreler ve sonuçlar kullanılmazsa Uyarılar sağlayın.</span><span class="sxs-lookup"><span data-stu-id="1330e-104">Provide warnings when such variables, parameters and results are not used according to that intent.</span></span>

## <a name="expression-of-intent"></a><span data-ttu-id="1330e-105">Amaç ifadesi</span><span class="sxs-lookup"><span data-stu-id="1330e-105">Expression of intent</span></span>

<span data-ttu-id="1330e-106">Dil, değer türleri için `T?` sözdizimini zaten içeriyor.</span><span class="sxs-lookup"><span data-stu-id="1330e-106">The language already contains the `T?` syntax for value types.</span></span> <span data-ttu-id="1330e-107">Bu söz dizimini başvuru türlerine genişletmek basittir.</span><span class="sxs-lookup"><span data-stu-id="1330e-107">It is straightforward to extend this syntax to reference types.</span></span>

<span data-ttu-id="1330e-108">Benzersiz olmayan bir başvuru türü amacı `T` bunun null olmadığı varsayılır.</span><span class="sxs-lookup"><span data-stu-id="1330e-108">It is assumed that the intent of an unadorned reference type `T` is for it to be non-null.</span></span>

## <a name="checking-of-nullable-references"></a><span data-ttu-id="1330e-109">Null yapılabilir başvuruların denetlenmesi</span><span class="sxs-lookup"><span data-stu-id="1330e-109">Checking of nullable references</span></span>

<span data-ttu-id="1330e-110">Akış Analizi, null yapılabilir başvuru değişkenlerini izler.</span><span class="sxs-lookup"><span data-stu-id="1330e-110">A flow analysis tracks nullable reference variables.</span></span> <span data-ttu-id="1330e-111">Analiz, bu kişilerin null olmaması (örneğin, bir denetim veya atamadan sonra), bu değerin null olmayan bir başvuru olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="1330e-111">Where the analysis deems that they would not be null (e.g. after a check or an assignment), their value will be considered a non-null reference.</span></span>

<span data-ttu-id="1330e-112">Null yapılabilir bir başvuru ayrıca, sonek `x!` işleci ("dammıt" işleci) ile null olmayan olarak kabul edilebilir, çünkü akış analizi, geliştiricinin orada olduğunu belirten null olmayan bir durum oluşturamıyor.</span><span class="sxs-lookup"><span data-stu-id="1330e-112">A nullable reference can also explicitly be treated as non-null with the postfix `x!` operator (the "dammit" operator), for when flow analysis cannot establish a non-null situation that the developer knows is there.</span></span>

<span data-ttu-id="1330e-113">Aksi takdirde, bir null yapılabilir başvuruya başvuruluyorsa veya null olmayan bir türe dönüştürülürse bir uyarı verilir.</span><span class="sxs-lookup"><span data-stu-id="1330e-113">Otherwise, a warning is given if a nullable reference is dereferenced, or is converted to a non-null type.</span></span>

<span data-ttu-id="1330e-114">`S[]`, `T?[]` `S?[]` ve `T[]`arasında dönüştürme sırasında bir uyarı verilir.</span><span class="sxs-lookup"><span data-stu-id="1330e-114">A warning is given when converting from `S[]` to `T?[]` and from `S?[]` to `T[]`.</span></span>

<span data-ttu-id="1330e-115">Tür parametresi birlikte değişken (`out`) ve `C<S?>` dönüştürme sırasında tür parametresi değişken karşıtı (`C<T>`) olduğunda, `C<T?>` `C<S>` bir uyarı verilir.`in`</span><span class="sxs-lookup"><span data-stu-id="1330e-115">A warning is given when converting from `C<S>` to `C<T?>` except when the type parameter is covariant (`out`), and when converting from `C<S?>` to `C<T>` except when the type parameter is contravariant (`in`).</span></span>

<span data-ttu-id="1330e-116">Tür parametresinde null olmayan kısıtlamalar varsa `C<T?>` bir uyarı verilir.</span><span class="sxs-lookup"><span data-stu-id="1330e-116">A warning is given on `C<T?>` if the type parameter has non-null constraints.</span></span> 

## <a name="checking-of-non-null-references"></a><span data-ttu-id="1330e-117">Null olmayan başvuruların denetlenmesi</span><span class="sxs-lookup"><span data-stu-id="1330e-117">Checking of non-null references</span></span>

<span data-ttu-id="1330e-118">Null olmayan bir değişkene atandığında veya null olmayan bir parametre olarak geçirilmemişse bir uyarı verilir.</span><span class="sxs-lookup"><span data-stu-id="1330e-118">A warning is given if a null literal is assigned to a non-null variable or passed as a non-null parameter.</span></span>

<span data-ttu-id="1330e-119">Oluşturucu null olmayan başvuru alanlarını açıkça başlatmadıysanız bir uyarı da verilir.</span><span class="sxs-lookup"><span data-stu-id="1330e-119">A warning is also given if a constructor does not explicitly initialize non-null reference fields.</span></span>

<span data-ttu-id="1330e-120">Null olmayan başvuruların dizisinin tüm öğelerinin başlatıldığını yeterince izleyemedik.</span><span class="sxs-lookup"><span data-stu-id="1330e-120">We cannot adequately track that all elements of an array of non-null references are initialized.</span></span> <span data-ttu-id="1330e-121">Ancak, yeni oluşturulan bir dizinin bir öğesi, dizi okunmadan veya geçirilmeden önce öğesine atanmamışsa bir uyarı verebilir.</span><span class="sxs-lookup"><span data-stu-id="1330e-121">However, we could issue a warning if no element of a newly created array is assigned to before the array is read from or passed on.</span></span> <span data-ttu-id="1330e-122">Bu, çok gürültülü olmayan ortak durumu işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="1330e-122">That might handle the common case without being too noisy.</span></span>

<span data-ttu-id="1330e-123">`default(T)` bir uyarı mı üreteceğine karar vermemiz gerekir, yoksa yalnızca `T?`türünde olduğu kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="1330e-123">We need to decide whether `default(T)` generates a warning, or is simply treated as being of the type `T?`.</span></span>

## <a name="metadata-representation"></a><span data-ttu-id="1330e-124">Meta veri gösterimi</span><span class="sxs-lookup"><span data-stu-id="1330e-124">Metadata representation</span></span>

<span data-ttu-id="1330e-125">Null olabilme manları öznitelik olarak meta verilerde temsil edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="1330e-125">Nullability adornments should be represented in metadata as attributes.</span></span> <span data-ttu-id="1330e-126">Bu, alt düzey derleyicilerin bunları yoksayması anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="1330e-126">This means that downlevel compilers will ignore them.</span></span>

<span data-ttu-id="1330e-127">Yalnızca null yapılabilir ek açıklamaların dahil edilip edilmeyeceğini veya derlemede null olmayan "açık" olup olmadığını belirlemek istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="1330e-127">We need to decide if only nullable annotations are included, or there's also some indication of whether non-null was "on" in the assembly.</span></span>

## <a name="generics"></a><span data-ttu-id="1330e-128">Genel Türler</span><span class="sxs-lookup"><span data-stu-id="1330e-128">Generics</span></span>

<span data-ttu-id="1330e-129">Tür parametresinde `T` null yapılamayan kısıtlamalar varsa, kapsamı içinde null atanamaz olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="1330e-129">If a type parameter `T` has non-nullable constraints, it is treated as non-nullable within its scope.</span></span>

<span data-ttu-id="1330e-130">Bir tür parametresi kısıtlanmamışsa veya yalnızca null yapılabilir kısıtlamalara sahipse, bu durum biraz daha karmaşıktır: Bu, karşılık gelen tür bağımsız *değişkeninin Nullable veya null yapılamayan olabileceği anlamına* gelir.</span><span class="sxs-lookup"><span data-stu-id="1330e-130">If a type parameter is unconstrained or has only nullable constraints, the situation is a little more complex: this means that the corresponding type argument could be *either* nullable or non-nullable.</span></span> <span data-ttu-id="1330e-131">Bu durumda yapılacak güvenli şey, tür parametresini hem null yapılabilir hem *de* null değer atanabilir olarak değerlendirmek, her iki durumda da uyarı vermek olur.</span><span class="sxs-lookup"><span data-stu-id="1330e-131">The safe thing to do in that situation is to treat the type parameter as *both* nullable and non-nullable, giving warnings when either is violated.</span></span> 

<span data-ttu-id="1330e-132">Açık null yapılabilir başvuru kısıtlamalarına izin verilip verilmeyeceğini göz önünde bulundurmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="1330e-132">It is worth considering whether explicit nullable reference constraints should be allowed.</span></span> <span data-ttu-id="1330e-133">Ancak, null yapılabilir başvuru türlerinin belirli durumlarda (devralınan kısıtlamalar) *örtük olarak* kısıtlamalar olmasını önlamadığımızda unutmayın.</span><span class="sxs-lookup"><span data-stu-id="1330e-133">Note, however, that we cannot avoid having nullable reference types *implicitly* be constraints in certain cases (inherited constraints).</span></span>

<span data-ttu-id="1330e-134">`class` kısıtlaması null değil.</span><span class="sxs-lookup"><span data-stu-id="1330e-134">The `class` constraint is non-null.</span></span> <span data-ttu-id="1330e-135">`class?`, "Nullable başvuru türü" belirten geçerli bir null yapılabilir kısıtlama olup olmayacağını düşünebiliriz.</span><span class="sxs-lookup"><span data-stu-id="1330e-135">We can consider whether `class?` should be a valid nullable constraint denoting "nullable reference type".</span></span>

## <a name="type-inference"></a><span data-ttu-id="1330e-136">Tür çıkarımı</span><span class="sxs-lookup"><span data-stu-id="1330e-136">Type inference</span></span>

<span data-ttu-id="1330e-137">Tür çıkarımı içinde, katkıda bulunan bir tür null yapılabilir bir başvuru türü ise, sonuçta elde edilen tür null yapılabilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1330e-137">In type inference, if a contributing type is a nullable reference type, the resulting type should be nullable.</span></span> <span data-ttu-id="1330e-138">Diğer bir deyişle, nulllılık yayılır.</span><span class="sxs-lookup"><span data-stu-id="1330e-138">In other words, nullness is propagated.</span></span>

<span data-ttu-id="1330e-139">Bir katılım ifadesi olarak `null` sabit değerinin boş değer içerip içermediğini düşünmemiz gerekir.</span><span class="sxs-lookup"><span data-stu-id="1330e-139">We should consider whether the `null` literal as a participating expression should contribute nullness.</span></span> <span data-ttu-id="1330e-140">Bugün değil: bir hataya neden olan değer türleri için, ancak başvuru türleri için null değeri, düz türe başarıyla dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="1330e-140">It doesn't today: for value types it leads to an error, whereas for reference types the null successfully converts to the plain type.</span></span> 

```csharp
string? n = "world";
var x = b ? "Hello" : n; // string?
var y = b ? "Hello" : null; // string? or error
var z = b ? 7 : null; // Error today, could be int?
```

## <a name="breaking-changes"></a><span data-ttu-id="1330e-141">Yeni değişiklikler</span><span class="sxs-lookup"><span data-stu-id="1330e-141">Breaking changes</span></span>

<span data-ttu-id="1330e-142">Null olmayan uyarılar, mevcut kodda belirgin bir büyük değişiklik ve bir katılım mekanizması ile birlikte kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1330e-142">Non-null warnings are an obvious breaking change on existing code, and should be accompanied with an opt-in mechanism.</span></span>

<span data-ttu-id="1330e-143">Daha az açıktır, null yapılabilir türlerden uyarılar (yukarıda açıklandığı gibi), null değer alabilme 'in örtük olduğu belirli senaryolarda, var olan kodda önemli bir değişiklik olur:</span><span class="sxs-lookup"><span data-stu-id="1330e-143">Less obviously, warnings from nullable types (as described above) are a breaking change on existing code in certain scenarios where the nullability is implicit:</span></span>

* <span data-ttu-id="1330e-144">Kısıtlanmış olmayan tür parametreleri örtük olarak null yapılabilir olarak değerlendirilir, bu nedenle `object` veya erişim, örneğin `ToString`, uyarılar verecek.</span><span class="sxs-lookup"><span data-stu-id="1330e-144">Unconstrained type parameters will be treated as implicitly nullable, so assigning them to `object` or accessing e.g. `ToString` will yield warnings.</span></span>
* <span data-ttu-id="1330e-145">Tür çıkarımı `null` ifadelerden boş bir değer içeriyorsa, mevcut kod bazen null yapılamayan türler yerine null yapılabilir, bu da yeni uyarılara yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="1330e-145">if type inference infers nullness from `null` expressions, then existing code will sometimes yield nullable rather than non-nullable types, which can lead to new warnings.</span></span>

<span data-ttu-id="1330e-146">Bu nedenle null yapılabilir uyarıların da isteğe bağlı olması gerekir</span><span class="sxs-lookup"><span data-stu-id="1330e-146">So nullable warnings also need to be optional</span></span>

<span data-ttu-id="1330e-147">Son olarak, var olan bir API 'ye ek açıklamalar eklemek, kitaplığı yükselttiğinizde uyarıları kabul eden kullanıcılara bir son değişiklik olacaktır.</span><span class="sxs-lookup"><span data-stu-id="1330e-147">Finally, adding annotations to an existing API will be a breaking change to users who have opted in to warnings, when they upgrade the library.</span></span> <span data-ttu-id="1330e-148">Bu, çok, kabul etme veya kapatma imkanını de sağlar. "Hata düzeltmelerini istiyorum, ancak yeni ek açıklamalarıyla uğraşmak için hazırlanma"</span><span class="sxs-lookup"><span data-stu-id="1330e-148">This, too, merits the ability to opt in or out. "I want the bug fixes, but I am not ready to deal with their new annotations"</span></span>

<span data-ttu-id="1330e-149">Özet bölümünde şunları kabul etmeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="1330e-149">In summary, you need to be able to opt in/out of:</span></span>
* <span data-ttu-id="1330e-150">Null yapılabilir uyarılar</span><span class="sxs-lookup"><span data-stu-id="1330e-150">Nullable warnings</span></span>
* <span data-ttu-id="1330e-151">Null olmayan uyarılar</span><span class="sxs-lookup"><span data-stu-id="1330e-151">Non-null warnings</span></span>
* <span data-ttu-id="1330e-152">Diğer dosyalardaki ek açıklamaların uyarıları</span><span class="sxs-lookup"><span data-stu-id="1330e-152">Warnings from annotations in other files</span></span>

<span data-ttu-id="1330e-153">Kabul etme özelliğinin ayrıntı düzeyi, swaths of Code 'un pragmalar ile kabul eteceği ve önem düzeyleri Kullanıcı tarafından seçilebilir şekilde, çözümleyici benzeri bir model önerir.</span><span class="sxs-lookup"><span data-stu-id="1330e-153">The granularity of the opt-in suggests an analyzer-like model, where swaths of code can opt in and out with pragmas and severity levels can be chosen by the user.</span></span> <span data-ttu-id="1330e-154">Ayrıca, kitaplık başına seçenekler ("sonlanmaya hazır olana kadar JSON.NET ek açıklamalarını yoksayın"), kodda öznitelik olarak ifade edilebilir.</span><span class="sxs-lookup"><span data-stu-id="1330e-154">Additionally, per-library options ("ignore the annotations from JSON.NET until I'm ready to deal with the fall out") may be expressible in code as attributes.</span></span>

<span data-ttu-id="1330e-155">Katılım/geçiş deneyiminin tasarımı, bu özelliğin başarısı ve yararlılığı açısından önemlidir.</span><span class="sxs-lookup"><span data-stu-id="1330e-155">The design of the opt-in/transition experience is crucial to the success and usefulness of this feature.</span></span> <span data-ttu-id="1330e-156">Şunları yaptığınızdan emin olmak istiyoruz:</span><span class="sxs-lookup"><span data-stu-id="1330e-156">We need to make sure that:</span></span>

* <span data-ttu-id="1330e-157">Kullanıcılar, null olabilme denetlemesini</span><span class="sxs-lookup"><span data-stu-id="1330e-157">Users can adopt nullability checking gradually as they want to</span></span>
* <span data-ttu-id="1330e-158">Kitaplık yazarları, son müşterilerin korku olmadan null değer alabilirlik ek açıklamaları ekleyebilir</span><span class="sxs-lookup"><span data-stu-id="1330e-158">Library authors can add nullability annotations without fear of breaking customers</span></span>
* <span data-ttu-id="1330e-159">Bunlara rağmen, "yapılandırmanızın gecemi" hakkında bir fikir yoktur</span><span class="sxs-lookup"><span data-stu-id="1330e-159">Despite these, there is not a sense of "configuration nightmare"</span></span>

## <a name="tweaks"></a><span data-ttu-id="1330e-160">Atabileceği</span><span class="sxs-lookup"><span data-stu-id="1330e-160">Tweaks</span></span>

<span data-ttu-id="1330e-161">Yereller üzerinde `?` ek açıklamalarını kullanmayabilirsiniz, ancak bunlara atanan değerlere uygun olarak kullanılıp kullanılmayacağını gözlemliyoruz.</span><span class="sxs-lookup"><span data-stu-id="1330e-161">We could consider not using the `?` annotations on locals, but just observing whether they are used in accordance with what gets assigned to them.</span></span> <span data-ttu-id="1330e-162">Bunu tercih ediyorum; İnsanların amacını hızlı bir şekilde sunalım etmem gerektiğini düşündük.</span><span class="sxs-lookup"><span data-stu-id="1330e-162">I don't favor this; I think we should uniformly let people express their intent.</span></span>

<span data-ttu-id="1330e-163">Bir çalışma zamanı null denetimini otomatik olarak üreten parametreler üzerinde bir toplu `T! x` düşünebiliriz.</span><span class="sxs-lookup"><span data-stu-id="1330e-163">We could consider a shorthand `T! x` on parameters, that auto-generates a runtime null check.</span></span>

<span data-ttu-id="1330e-164">`FirstOrDefault` veya `TryGet`gibi genel türlerde bazı desenler, null olamayan tür bağımsız değişkenleriyle biraz tuhaf davranışa sahiptir, çünkü açıkça belirli durumlarda varsayılan değerler verir.</span><span class="sxs-lookup"><span data-stu-id="1330e-164">Certain patterns on generic types, such as `FirstOrDefault` or `TryGet`, have slightly weird behavior with non-nullable type arguments, because they explicitly yield default values in certain situations.</span></span> <span data-ttu-id="1330e-165">Tür sistemini bu daha iyi uyum sağlayacak şekilde kullanmayı deneyebiliriz.</span><span class="sxs-lookup"><span data-stu-id="1330e-165">We could try to nuance the type system to accommodate these better.</span></span> <span data-ttu-id="1330e-166">Örneğin, tür bağımsız değişkeni zaten null yapılabilir olmasına rağmen kısıtlanmış tür parametrelerine `?` izin vereceğiz.</span><span class="sxs-lookup"><span data-stu-id="1330e-166">For instance, we could allow `?` on unconstrained type parameters, even though the type argument could already be nullable.</span></span> <span data-ttu-id="1330e-167">Ona değdiğini biliyorum ve null yapılabilir *değer* türleriyle etkileşim ile ilgili olarak bir sorun doğurur.</span><span class="sxs-lookup"><span data-stu-id="1330e-167">I doubt that it is worth it, and it leads to weirdness related to interaction with nullable *value* types.</span></span> 

## <a name="nullable-value-types"></a><span data-ttu-id="1330e-168">Boş değer atanabilen değer türleri</span><span class="sxs-lookup"><span data-stu-id="1330e-168">Nullable value types</span></span>

<span data-ttu-id="1330e-169">Null yapılabilir değer türleri için yukarıdaki semantiklerinden bazılarını benimseme de düşüneceğiz.</span><span class="sxs-lookup"><span data-stu-id="1330e-169">We could consider adopting some of the above semantics for nullable value types as well.</span></span>

<span data-ttu-id="1330e-170">Yalnızca bir hata vermek yerine `(7, null)``int?` çıkarsanımız tür çıkarımı zaten belirtiliyor.</span><span class="sxs-lookup"><span data-stu-id="1330e-170">We already mentioned type inference, where we could infer `int?` from `(7, null)`, instead of just giving an error.</span></span>

<span data-ttu-id="1330e-171">Farklı bir fırsat, akış analizinin null yapılabilir değer türlerine uygulanmasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="1330e-171">Another opportunity is to apply the flow analysis to nullable value types.</span></span> <span data-ttu-id="1330e-172">Null olmayan kabul edildiğinde, gerçekte null yapılamayan tür olarak kullanılmasına izin veririz (örn. üye erişimi).</span><span class="sxs-lookup"><span data-stu-id="1330e-172">When they are deemed non-null, we could actually allow using as the non-nullable type in certain ways (e.g. member access).</span></span> <span data-ttu-id="1330e-173">Yalnızca null yapılabilen bir değer türü üzerinde *zaten* yapabileceğiniz şeylerin, geriye doğru uyumluluk nedenleriyle tercih edildiğini dikkatli etmemiz gerekir.</span><span class="sxs-lookup"><span data-stu-id="1330e-173">We just have to be careful that the things that you can *already* do on a nullable value type will be preferred, for back compat reasons.</span></span>
