---
ms.openlocfilehash: 76065293f652979ab395e131d657e44899c5a65b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484554"
---
# <a name="simplified-null-argument-checking"></a><span data-ttu-id="f83df-101">Basitleştirilmiş null bağımsız değişken denetimi</span><span class="sxs-lookup"><span data-stu-id="f83df-101">Simplified Null Argument Checking</span></span>

## <a name="summary"></a><span data-ttu-id="f83df-102">Özet</span><span class="sxs-lookup"><span data-stu-id="f83df-102">Summary</span></span>
<span data-ttu-id="f83df-103">Bu teklif, yöntem bağımsız değişkenlerinin doğrulanması için basitleştirilmiş bir sözdizimi sağlar `null` ve `ArgumentNullException` uygun şekilde oluşturmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="f83df-103">This proposal provides a simplified syntax for validating method arguments are not `null` and throwing `ArgumentNullException` appropriately.</span></span>

## <a name="motivation"></a><span data-ttu-id="f83df-104">Amacı</span><span class="sxs-lookup"><span data-stu-id="f83df-104">Motivation</span></span>
<span data-ttu-id="f83df-105">Null yapılabilir başvuru türleri tasarlamada yapılan iş, `null` bağımsız değişken doğrulaması için gereken kodu incelemektir.</span><span class="sxs-lookup"><span data-stu-id="f83df-105">The work on designing nullable reference types has caused us to examine the code necessary for `null` argument validation.</span></span> <span data-ttu-id="f83df-106">Bu, NRT 'nin kod yürütme geliştiricilerinin etkilemediği, tamamen `null` Temizleme olan projelerde bile `if (arg is null) throw` Boiler levha kodu eklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="f83df-106">Given that NRT doesn't affect code execution developers still must add `if (arg is null) throw` boiler plate code even in projects which are fully `null` clean.</span></span> <span data-ttu-id="f83df-107">Bu, dilde `null` doğrulaması için en az bir sözdizimi keşfetmeye yönelik bir değer vermiştir.</span><span class="sxs-lookup"><span data-stu-id="f83df-107">This gave us the desire to explore a minimal syntax for argument `null` validation in the language.</span></span> 

<span data-ttu-id="f83df-108">Bu `null` parametre doğrulama sözdiziminin, NRT ile sıklıkla eşleştirilme beklenirken, teklif bundan tamamen bağımsızdır.</span><span class="sxs-lookup"><span data-stu-id="f83df-108">While this `null` parameter validation syntax is expected to pair frequently with NRT the proposal is fully independent of it.</span></span> <span data-ttu-id="f83df-109">Sözdizimi `#nullable` yönergelerinden bağımsız olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f83df-109">The syntax can be used independent of `#nullable` directives.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="f83df-110">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="f83df-110">Detailed Design</span></span> 

### <a name="null-validation-parameter-syntax"></a><span data-ttu-id="f83df-111">Null doğrulama parametresi sözdizimi</span><span class="sxs-lookup"><span data-stu-id="f83df-111">Null validation parameter syntax</span></span>
<span data-ttu-id="f83df-112">Vurma işleci `!`, bir parametre listesindeki bir parametre adından sonra konumlandırılabilir ve bu parametre için C# derleyicinin standart `null` denetim kodunu yaymasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="f83df-112">The bang operator, `!`, can be positioned after a parameter name in a parameter list and this will cause the C# compiler to emit standard `null` checking code for that parameter.</span></span> <span data-ttu-id="f83df-113">Bu, `null` doğrulama parametresi sözdizimi olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="f83df-113">This is referred to as `null` validation parameter syntax.</span></span> <span data-ttu-id="f83df-114">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f83df-114">For example:</span></span>

``` csharp
void M(string name!) {
    ...
}
```

<span data-ttu-id="f83df-115">Şu şekilde çevrilecek:</span><span class="sxs-lookup"><span data-stu-id="f83df-115">Will be translated into:</span></span>

``` csharp
void M(string name) {
    if (name is null) {
        throw new ArgumentNullException(nameof(name));
    }
    ...
}
```

<span data-ttu-id="f83df-116">Oluşturulan `null` denetimi, herhangi bir geliştirici koddan önce oluşur.</span><span class="sxs-lookup"><span data-stu-id="f83df-116">The generated `null` check will occur before any developer authored code in the method.</span></span> <span data-ttu-id="f83df-117">Birden çok parametre `!` işlecini içerdiğinde, parametreler bildirildiği sırada denetimler de gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="f83df-117">When multiple parameters contain the `!` operator then the checks will occur in the same order as the parameters are declared.</span></span>

``` csharp
void M(string p1, string p2) {
    if (p1 is null) {
        throw new ArgumentNullException(nameof(p1));
    }
    if (p2 is null) {
        throw new ArgumentNullException(nameof(p2));
    }
    ...
}
```

<span data-ttu-id="f83df-118">Denetim, `null`için özel olarak başvuru eşitliği olacak, `==` veya Kullanıcı tanımlı işleçleri çağırmaz.</span><span class="sxs-lookup"><span data-stu-id="f83df-118">The check will be specifically for reference equality to `null`, it does not invoke `==` or any user defined operators.</span></span> <span data-ttu-id="f83df-119">Bu ayrıca `!` işlecinin yalnızca, `null`karşı eşitlik için test edilebilir parametrelere eklenebileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="f83df-119">This also means the `!` operator can only be added to parameters whose type can be tested for equality against `null`.</span></span> <span data-ttu-id="f83df-120">Bu, türü değer türü olarak bilinen bir parametre üzerinde kullanılamayan anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="f83df-120">This means it can't be used on a parameter whose type is known to be a value type.</span></span>

``` csharp
// Error: Cannot use ! on parameters who types derive from System.ValueType
void G<T>(T arg!) where T : struct {

}
```

<span data-ttu-id="f83df-121">Bir Oluşturucu söz konusu olduğunda `null` doğrulama, kurucudaki diğer koddan önce oluşur.</span><span class="sxs-lookup"><span data-stu-id="f83df-121">In the case of a constructor the `null` validation will occur before any other code in the constructor.</span></span> <span data-ttu-id="f83df-122">Şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="f83df-122">That includes:</span></span> 

- <span data-ttu-id="f83df-123">`this` veya `base` ile diğer oluşturuculara zincir oluşturma</span><span class="sxs-lookup"><span data-stu-id="f83df-123">Chaining to other constructors with `this` or `base`</span></span> 
- <span data-ttu-id="f83df-124">Oluşturucuya örtük olarak oluşan alan başlatıcıları</span><span class="sxs-lookup"><span data-stu-id="f83df-124">Field initializers which implicitly occur in the constructor</span></span>

<span data-ttu-id="f83df-125">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f83df-125">For example:</span></span>

``` csharp
class C {
    string field = GetString();
    C(string name!): this(name) {
        ...
    }
}
```

<span data-ttu-id="f83df-126">, Kabaca şu şekilde çevrilecek:</span><span class="sxs-lookup"><span data-stu-id="f83df-126">Will be roughly translated into the following:</span></span>

``` csharp
class C {
    C(string name)
        if (name is null) {
            throw new ArgumentNullException(nameof(name));
        }
        field = GetString();
        :this(name);
        ...
}
```

<span data-ttu-id="f83df-127">Not: Bu yasal C# bir kod değildir, ancak yalnızca uygulamanın ne yaptığını bir yaklaşık olarak karşılarsınız.</span><span class="sxs-lookup"><span data-stu-id="f83df-127">Note: this is not legal C# code but instead just an approximation of what the implementation does.</span></span> 

<span data-ttu-id="f83df-128">`null` doğrulama parametresi sözdizimi, lambda parametresi listelerinde de geçerli olacaktır.</span><span class="sxs-lookup"><span data-stu-id="f83df-128">The `null` validation parameter syntax will also be valid on lambda parameter lists.</span></span> <span data-ttu-id="f83df-129">Bu, parleştirir olmayan tek parametre sözdiziminde bile geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="f83df-129">This is valid even in the single parameter syntax that lacks parens.</span></span>

``` csharp
void G() {
    // An identity lambda which throws on a null input
    Func<string, string> s = x! => x;
}
```

<span data-ttu-id="f83df-130">Sözdizimi Ayrıca Yineleyici yöntemlerine yönelik parametrelerde de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="f83df-130">The syntax is also valid on parameters to iterator methods.</span></span> <span data-ttu-id="f83df-131">Yineleyicinizdeki diğer koddan farklı olarak, yineleyici yöntemi çağrıldığında, temeldeki Numaralandırıcı celenmiş olmadığında değil, `null` doğrulaması oluşur.</span><span class="sxs-lookup"><span data-stu-id="f83df-131">Unlike other code in the iterator the `null` validation will occur when the iterator method is invoked, not when the underlying enumerator is walked.</span></span> <span data-ttu-id="f83df-132">Bu, geleneksel veya `async` yineleyiciler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="f83df-132">This is true for traditional or `async` iterators.</span></span>

``` csharp
class Iterators {
    IEnumerable<char> GetCharacters(string s!) {
        foreach (var c in s) {
            yield return c;
        }
    }

    void Use() {
        // The invocation of GetCharacters will throw
        IEnumerable<char> e = GetCharacters(null);
    }
}
```

<span data-ttu-id="f83df-133">`!` işleci yalnızca ilişkili yöntem gövdesine sahip olan parametre listeleri için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f83df-133">The `!` operator can only be used for parameter lists which have an associated method body.</span></span> <span data-ttu-id="f83df-134">Bu, bir `abstract` yöntemde, `interface`, `delegate` veya `partial` yöntem tanımında kullanılamayan anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="f83df-134">This means it cannot be used in an `abstract` method, `interface`, `delegate` or `partial` method definition.</span></span>

### <a name="extending-is-null"></a><span data-ttu-id="f83df-135">Genişletme null</span><span class="sxs-lookup"><span data-stu-id="f83df-135">Extending is null</span></span>
<span data-ttu-id="f83df-136">`is null` ifadesi geçerli olan türler, kısıtlanmış olmayan tür parametreleri içerecek şekilde genişletilir.</span><span class="sxs-lookup"><span data-stu-id="f83df-136">The types for which the expression `is null` is valid will be extended to include unconstrained type parameters.</span></span> <span data-ttu-id="f83df-137">Bu, bir `null` denetiminin geçerli olduğu tüm türlerde `null` denetleme amacını doldurmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="f83df-137">This will allow it to fill the intent of checking for `null` on all types which a `null` check is valid.</span></span> <span data-ttu-id="f83df-138">Özellikle, kesin olarak değer türleri olmak üzere bilinen türlerdir.</span><span class="sxs-lookup"><span data-stu-id="f83df-138">Specifically that is types which are not definitely known to be value types.</span></span> <span data-ttu-id="f83df-139">Örneğin, `struct` kısıtlı olan tür parametreleri bu söz dizimi ile kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="f83df-139">For example Type parameters which are constrained to `struct` cannot be used with this syntax.</span></span>

``` csharp
void NullCheck<T1, T2>(T1 p1, T2 p2) where T2 : struct {
    // Okay: T1 could be a class or struct here.
    if (p1 is null) {
        ...
    }

    // Error 
    if (p2 is null) { 
        ...
    }
}
```

<span data-ttu-id="f83df-140">Bir tür parametresindeki `is null` davranışı bugün `== null` aynı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="f83df-140">The behavior of `is null` on a type parameter will be the same as `== null` today.</span></span> <span data-ttu-id="f83df-141">Tür parametresinin değer türü olarak örneklendiği durumlarda, kod `false`olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="f83df-141">In the cases where the type parameter is instantiated as a value type the code will be evaluated as `false`.</span></span> <span data-ttu-id="f83df-142">Bir başvuru türü olduğu durumlarda kod, uygun bir `is null` denetimi olur.</span><span class="sxs-lookup"><span data-stu-id="f83df-142">For cases where it is a reference type the code will do a proper `is null` check.</span></span>

### <a name="intersection-with-nullable-reference-types"></a><span data-ttu-id="f83df-143">Null yapılabilir başvuru türleriyle kesişim</span><span class="sxs-lookup"><span data-stu-id="f83df-143">Intersection with Nullable Reference Types</span></span>
<span data-ttu-id="f83df-144">Bir `!` işlecinin adına uygulanmış olduğu herhangi bir parametre, null yapılabilir durum `null`olarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="f83df-144">Any parameter which has a `!` operator applied to it's name will start with the nullable state being not `null`.</span></span> <span data-ttu-id="f83df-145">Bu, parametrenin kendisinin türü potansiyel olarak `null`olsa da geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="f83df-145">This is true even if the type of the parameter itself is potentially `null`.</span></span> <span data-ttu-id="f83df-146">Bu, `string?`gibi açıkça null yapılabilir bir tür ile veya kısıtlanmış olmayan bir tür parametresiyle meydana gelebilir.</span><span class="sxs-lookup"><span data-stu-id="f83df-146">That can occur with an explicitly nullable type, such as say `string?`, or with an unconstrained type parameter.</span></span> 

<span data-ttu-id="f83df-147">Parametrelerde `!` söz dizimi parametre üzerinde açıkça null yapılabilir bir tür ile birleştirildiğinde, derleyici tarafından bir uyarı verilir:</span><span class="sxs-lookup"><span data-stu-id="f83df-147">When a `!` syntax on parameters is combined with an explicitly nullable type on the parameter then a warning will be issued by the compiler:</span></span>

``` csharp
void WarnCase<T>(
    string? name!, // Warning: combining explicit null checking with a nullable type
    T value1 // Okay
)
```

## <a name="open-issues"></a><span data-ttu-id="f83df-148">Açık sorunlar</span><span class="sxs-lookup"><span data-stu-id="f83df-148">Open Issues</span></span>
<span data-ttu-id="f83df-149">Yok</span><span class="sxs-lookup"><span data-stu-id="f83df-149">None</span></span>

## <a name="considerations"></a><span data-ttu-id="f83df-150">Dikkat edilmesi gerekenler</span><span class="sxs-lookup"><span data-stu-id="f83df-150">Considerations</span></span>

### <a name="constructors"></a><span data-ttu-id="f83df-151">Oluşturucular</span><span class="sxs-lookup"><span data-stu-id="f83df-151">Constructors</span></span>
<span data-ttu-id="f83df-152">Oluşturucular için kod üretimi küçük bir, ancak bir standart `null` doğrulamadan ve `null` doğrulama parametresi söz dizimine (`!`) geçiş yaparken bir observable ve davranış değişikliği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="f83df-152">The code generation for constructors means there is a small, but observable, behavior change when moving from standard `null` validation today and the `null` validation parameter syntax (`!`).</span></span> <span data-ttu-id="f83df-153">Standart doğrulama `null` denetimi hem alan başlatıcıları hem de `base` veya `this` çağrılarından sonra oluşur.</span><span class="sxs-lookup"><span data-stu-id="f83df-153">The `null` check in standard validation occurs after both field initializers and any `base` or `this` calls.</span></span> <span data-ttu-id="f83df-154">Bu, bir geliştiricinin `null` doğrulamasının %100 ' i yeni sözdizimine geçiremeyeceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="f83df-154">This means a developer can't necessarily migrate 100% of their `null` validation to the new syntax.</span></span> <span data-ttu-id="f83df-155">Oluşturucular en az bir inceleme gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f83df-155">Constructors at least require some inspection.</span></span>

<span data-ttu-id="f83df-156">Tartışmadan sonra, önemli benimseme sorunlarına neden olması çok düşüktür.</span><span class="sxs-lookup"><span data-stu-id="f83df-156">After discussion though it was decided that this is very unlikely to cause any significant adoption issues.</span></span> <span data-ttu-id="f83df-157">Kurucudaki herhangi bir mantığdan önce `null` denetiminin çalıştırılması daha mantıklı.</span><span class="sxs-lookup"><span data-stu-id="f83df-157">It's more logical that the `null` check run before any logic in the constructor does.</span></span> <span data-ttu-id="f83df-158">Önemli uyumluluk sorunları tespit edildiğinde tekrar tekrar bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="f83df-158">Can revisit if significant compat issues are discovered.</span></span>

### <a name="warning-when-mixing--and-"></a><span data-ttu-id="f83df-159">Karıştırdığınızda uyarı mı?</span><span class="sxs-lookup"><span data-stu-id="f83df-159">Warning when mixing ?</span></span> <span data-ttu-id="f83df-160">'!</span><span class="sxs-lookup"><span data-stu-id="f83df-160">and !</span></span>
<span data-ttu-id="f83df-161">`!` sözdizimi, açıkça null olabilen bir tür olarak yazılmış bir parametreye uygulandığında, uyarı verilip verilmeyeceğini belirten bir sorun olup olmadığını belirten uzun bir tartışma vardı.</span><span class="sxs-lookup"><span data-stu-id="f83df-161">There was a lengthy discussion on whether or not a warning should be issued when the `!` syntax is applied to a parameter which is explicitly typed to a nullable type.</span></span> <span data-ttu-id="f83df-162">Bu yüzey, geliştirici tarafından sensik olmayan bir bildirim gibi görünüyor, ancak tür hiyerarşisinin geliştiricilere bu gibi bir duruma zorlabildiği durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="f83df-162">On the surface it seems like a nonsensical declaration by the developer but there are cases where type hierarchies could force developers into such a situation.</span></span> 

<span data-ttu-id="f83df-163">Aşağıdaki sınıf hiyerarşisini bir dizi derleme genelinde göz önünde bulundurun (tümünün `null` denetlemesi etkinleştirilmiş olarak derlendiğini varsayarak):</span><span class="sxs-lookup"><span data-stu-id="f83df-163">Consider the following class hierarchy across a series of assemblies (assuming all are compiled with `null` checking enabled):</span></span>

``` csharp
// Assembly1
abstract class C1 {
    protected abstract void M(object o); 
}

// Assembly2
abstract class C2 : C1 {

}

// Assembly3
abstract class C3 : C2 { 
    protected override void M(object o!) {
        ...
    }
}
```

<span data-ttu-id="f83df-164">Burada `C3` yazarı `o`parametreye `null` doğrulaması eklemeye karar verdi.</span><span class="sxs-lookup"><span data-stu-id="f83df-164">Here the author of `C3` decided to add `null` validation to the parameter `o`.</span></span> <span data-ttu-id="f83df-165">Bu, özelliğin kullanım amacına göre tamamen bir satır içinde olur.</span><span class="sxs-lookup"><span data-stu-id="f83df-165">This is completely in line with how the feature is intended to be used.</span></span>

<span data-ttu-id="f83df-166">Şimdi, Assembly2 yazarı aşağıdaki geçersiz kılmayı eklemeye karar verdiğinde daha sonraki bir tarihte düşünün:</span><span class="sxs-lookup"><span data-stu-id="f83df-166">Now imagine at a later date the author of Assembly2 decides to add the following override:</span></span>

``` csharp
// Assembly2
abstract class C2 : C1 {
   protected override void M(object? o) { 
       ...
   }
}
```

<span data-ttu-id="f83df-167">Bu, sözleşmenin giriş konumlarına yönelik daha esnek hale getirilebileceği için, null yapılabilir başvuru türleri tarafından izin verilir.</span><span class="sxs-lookup"><span data-stu-id="f83df-167">This is allowed by nullable reference types as it's legal to make the contract more flexible for input positions.</span></span> <span data-ttu-id="f83df-168">Genel olarak NRT özelliği, parametre veya dönüş null değer alabilme üzerinde makul ortak/değişken varyans sağlar.</span><span class="sxs-lookup"><span data-stu-id="f83df-168">The NRT feature in general allows for reasonable co/contravariance on parameter / return nullability.</span></span> <span data-ttu-id="f83df-169">Ancak dil, özgün bildirime değil, en özel geçersiz kılma temelinde ortak/değişken varyans denetimi yapar.</span><span class="sxs-lookup"><span data-stu-id="f83df-169">However the language does the co/contravariance checking based on the most specific override, not the original declaration.</span></span> <span data-ttu-id="f83df-170">Bu, Assembly3 yazarı, eşleşmeyen `o` türü hakkında bir uyarı alacak ve bunu ortadan kaldırmak için imzayı aşağıdaki şekilde değiştirmesinin gerektiği anlamına gelir:</span><span class="sxs-lookup"><span data-stu-id="f83df-170">This means the author of Assembly3 will get a warning about the type of `o` not matching and will need to change the signature to the following to eliminate it:</span></span> 

``` csharp
// Assembly3
abstract class C3 : C2 { 
    protected override void M(object? o!) {
        ...
    }
}
```

<span data-ttu-id="f83df-171">Bu noktada, Assembly3 'in yazarı birkaç seçeneğe sahiptir:</span><span class="sxs-lookup"><span data-stu-id="f83df-171">At this point the author of Assembly3 has a few choices:</span></span>

- <span data-ttu-id="f83df-172">`object?` ve `object` uyumsuzluğu hakkında uyarı kabul edebilir/göstermez.</span><span class="sxs-lookup"><span data-stu-id="f83df-172">They can accept / suppress the warning about `object?` and `object` mismatch.</span></span>
- <span data-ttu-id="f83df-173">`object?` ve `!` uyumsuzluğu hakkında uyarı kabul edebilir/göstermez.</span><span class="sxs-lookup"><span data-stu-id="f83df-173">They can accept / suppress the warning about `object?` and `!` mismatch.</span></span>
- <span data-ttu-id="f83df-174">Yalnızca `null` doğrulama denetimini kaldırabilir (`!` silebilir ve açıkça denetim yapabilir)</span><span class="sxs-lookup"><span data-stu-id="f83df-174">They can just remove the `null` validation check (delete `!` and do explicit checking)</span></span>

<span data-ttu-id="f83df-175">Bu gerçek bir senaryodur ancak şimdilik, uyarı ile ileriye doğru ilerlemedir.</span><span class="sxs-lookup"><span data-stu-id="f83df-175">This is a real scenario but for now the idea is to move forward with the warning.</span></span> <span data-ttu-id="f83df-176">Uyarı, daha sık öngörüldüğünde, daha sonra kaldırabiliriz (tersi doğru değildir).</span><span class="sxs-lookup"><span data-stu-id="f83df-176">If it turns out the warning happens more frequently than we anticipate then we can remove it later (the reverse is not true).</span></span>

### <a name="implicit-property-setter-arguments"></a><span data-ttu-id="f83df-177">Örtük özellik ayarlayıcı bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="f83df-177">Implicit property setter arguments</span></span>
<span data-ttu-id="f83df-178">Bir parametrenin `value` bağımsız değişkeni örtük ve herhangi bir parametre listesinde görünmez.</span><span class="sxs-lookup"><span data-stu-id="f83df-178">The `value` argument of a parameter is implicit and does not appear in any parameter list.</span></span> <span data-ttu-id="f83df-179">Yani bu özelliğin hedefi olamaz.</span><span class="sxs-lookup"><span data-stu-id="f83df-179">That means it cannot be a target of this feature.</span></span> <span data-ttu-id="f83df-180">Özellik ayarlayıcı sözdizimi, `!` işlecinin uygulanmasına izin vermek için bir parametre listesi içerecek şekilde genişletilebilir.</span><span class="sxs-lookup"><span data-stu-id="f83df-180">The property setter syntax could be extended to include a parameter list to allow the `!` operator to be applied.</span></span> <span data-ttu-id="f83df-181">Ancak bu özellik, `null` doğrulamasını daha kolay hale getiren bu özelliğin fikrine karşı karşılaştırılır.</span><span class="sxs-lookup"><span data-stu-id="f83df-181">But that cuts against the idea of this feature making `null` validation simpler.</span></span> <span data-ttu-id="f83df-182">Bu nedenle örtük `value` bağımsız değişkeni bu özellikle çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="f83df-182">As such the implicit `value` argument just won't work with this feature.</span></span>

## <a name="future-considerations"></a><span data-ttu-id="f83df-183">Gelecekteki konular</span><span class="sxs-lookup"><span data-stu-id="f83df-183">Future Considerations</span></span>
<span data-ttu-id="f83df-184">Yok</span><span class="sxs-lookup"><span data-stu-id="f83df-184">None</span></span>
