---
ms.openlocfilehash: 6088a5cd41926d828013f1b8e5736fd2b7939e44
ms.sourcegitcommit: da452002c3f472165a0e1fa7759f494cc703ae31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "79485044"
---
# <a name="compile-time-enforcement-of-safety-for-ref-like-types"></a><span data-ttu-id="079f1-101">Ref benzeri türler için zaman güvenlik zorlaması derleme</span><span class="sxs-lookup"><span data-stu-id="079f1-101">Compile time enforcement of safety for ref-like types</span></span>

## <a name="introduction"></a><span data-ttu-id="079f1-102">Giriş</span><span class="sxs-lookup"><span data-stu-id="079f1-102">Introduction</span></span>

<span data-ttu-id="079f1-103">`Span<T>` ve `ReadOnlySpan<T>` gibi türlerle ilgilenirken ek güvenlik kurallarının ana nedeni, bu tür türlerin yürütme yığınına göre sınırlandırılanmasıdır.</span><span class="sxs-lookup"><span data-stu-id="079f1-103">The main reason for the additional safety rules when dealing with types like `Span<T>` and `ReadOnlySpan<T>` is that such types must be confined to the execution stack.</span></span>
 
<span data-ttu-id="079f1-104">`Span<T>` ve benzer türlerin yalnızca yığın türünde olması gereken iki neden vardır.</span><span class="sxs-lookup"><span data-stu-id="079f1-104">There are two reasons why `Span<T>` and similar types must be a stack-only types.</span></span>

1. <span data-ttu-id="079f1-105">`Span<T>` anlam ve Aralık `(ref T data, int length)`içeren bir struct ' tır.</span><span class="sxs-lookup"><span data-stu-id="079f1-105">`Span<T>` is semantically a struct containing a reference and a range - `(ref T data, int length)`.</span></span> <span data-ttu-id="079f1-106">Gerçek uygulamadan bağımsız olarak, bu tür yapıya yazma atomik olmaz.</span><span class="sxs-lookup"><span data-stu-id="079f1-106">Regardless of actual implementation, writes to such struct would not be atomic.</span></span> <span data-ttu-id="079f1-107">Bu tür yapının eşzamanlı "tearing", `data`eşleşmeyen `length` olasılığına neden olur. bu durum, sonuçta "güvenli" kodda GC yığın bozulması oluşmasına neden olabilecek, Aralık dışı erişimleri ve tür güvenlik ihlallerine yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="079f1-107">Concurrent "tearing" of such struct would lead to the possibility of `length` not matching the `data`, causing out-of-range accesses and type-safety violations, which ultimately could result in GC heap corruption in seemingly "safe" code.</span></span>
2. <span data-ttu-id="079f1-108">`Span<T>` bazı uygulamaları, alanlarından birinde yönetilen bir işaretçi içerir.</span><span class="sxs-lookup"><span data-stu-id="079f1-108">Some implementations of `Span<T>` literally contain a managed pointer in one of its fields.</span></span> <span data-ttu-id="079f1-109">Yığın nesnelerinin alanları ve GC yığınında yönetilen bir işaretçi koymak için yöneten kod, genellikle JıT zamanında çöktüğü için yönetilen işaretçiler desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="079f1-109">Managed pointers are not supported as fields of heap objects and code that manages to put a managed pointer on the GC heap typically crashes at JIT time.</span></span>

<span data-ttu-id="079f1-110">`Span<T>` örnekleri yalnızca yürütme yığınında var olarak kısıtlanacaksa yukarıdaki tüm sorunlar alleviated olur.</span><span class="sxs-lookup"><span data-stu-id="079f1-110">All the above problems would be alleviated if instances of `Span<T>` are constrained to exist only on the execution stack.</span></span> 

<span data-ttu-id="079f1-111">Birleşim nedeniyle ek bir sorun ortaya çıkar.</span><span class="sxs-lookup"><span data-stu-id="079f1-111">An additional problem arises due to composition.</span></span> <span data-ttu-id="079f1-112">`Span<T>` ve `ReadOnlySpan<T>` örnekleri katıştırabilecek daha karmaşık veri türleri oluşturmak genellikle istenebilir.</span><span class="sxs-lookup"><span data-stu-id="079f1-112">It would be generally desirable to build more complex data types that would embed `Span<T>` and `ReadOnlySpan<T>` instances.</span></span> <span data-ttu-id="079f1-113">Bu tür bileşik türlerin yapılar olması ve `Span<T>`tüm tehlikeleri ve gereksinimlerinin paylaşılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="079f1-113">Such composite types would have to be structs and would share all the hazards and requirements of `Span<T>`.</span></span> <span data-ttu-id="079f1-114">Sonuç olarak, burada açıklanan güvenlik kuralları **_ref benzeri türlerin_** tamamına uygun şekilde görüntülenmelidir.</span><span class="sxs-lookup"><span data-stu-id="079f1-114">As a result the safety rules described here should be viewed as applicable to the whole range of **_ref-like types_**.</span></span>

<span data-ttu-id="079f1-115">[Taslak dil belirtimi](#draft-language-specification) , ref benzeri bir türün değerlerinin yalnızca yığında yer aldığından emin olmak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="079f1-115">The [draft language specification](#draft-language-specification) is intended to ensure that values of a ref-like type occurs only on the stack.</span></span>

## <a name="generalized-ref-like-types-in-source-code"></a><span data-ttu-id="079f1-116">Kaynak kodunda Genelleştirilmiş `ref-like` türleri</span><span class="sxs-lookup"><span data-stu-id="079f1-116">Generalized `ref-like` types in source code</span></span>

<span data-ttu-id="079f1-117">`ref-like` yapılar, `ref` değiştirici kullanılarak kaynak kodda açıkça işaretlenir:</span><span class="sxs-lookup"><span data-stu-id="079f1-117">`ref-like` structs are explicitly marked in the source code using `ref` modifier:</span></span>

```csharp
ref struct TwoSpans<T>
{
    // can have ref-like instance fields
    public Span<T> first;
    public Span<T> second;
} 

// error: arrays of ref-like types are not allowed. 
TwoSpans<T>[] arr = null;

```

<span data-ttu-id="079f1-118">Bir yapının ref gibi belirlenmesi, yapının ref benzeri örnek alanlarına sahip olmasına olanak sağlar ve aynı zamanda yapı benzeri türlerin tüm gereksinimlerini yapısına uygulanabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="079f1-118">Designating a struct as ref-like will allow the struct to have ref-like instance fields and will also make all the requirements of ref-like types applicable to the struct.</span></span> 

## <a name="metadata-representation-or-ref-like-structs"></a><span data-ttu-id="079f1-119">Meta veri temsili veya ref benzeri yapılar</span><span class="sxs-lookup"><span data-stu-id="079f1-119">Metadata representation or ref-like structs</span></span>

<span data-ttu-id="079f1-120">Ref benzeri yapılar, **System. Runtime. CompilerServices. IsRefLikeAttribute** özniteliğiyle işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="079f1-120">Ref-like structs will be marked with **System.Runtime.CompilerServices.IsRefLikeAttribute** attribute.</span></span>

<span data-ttu-id="079f1-121">Öznitelik, `mscorlib`gibi ortak temel kitaplıklara eklenecektir.</span><span class="sxs-lookup"><span data-stu-id="079f1-121">The attribute will be added to common base libraries such as `mscorlib`.</span></span> <span data-ttu-id="079f1-122">Özniteliği kullanılabilir değilse, derleyici `IsReadOnlyAttribute`gibi diğer yerleşik isteğe bağlı özniteliklere benzer bir dahili bir olay oluşturur.</span><span class="sxs-lookup"><span data-stu-id="079f1-122">In a case if the attribute is not available, compiler will generate an internal one similarly to other embedded-on-demand attributes such as `IsReadOnlyAttribute`.</span></span>

<span data-ttu-id="079f1-123">Güvenlik kurallarına alışkın olmayan derleyicilerde ref benzeri yapıların kullanılmasını engellemek için ek bir ölçü alınır (Bu özelliğin uygulandığı bir derleyiciler içerir C# ).</span><span class="sxs-lookup"><span data-stu-id="079f1-123">An additional measure will be taken to prevent the use of ref-like structs in compilers not familiar with the safety rules (this includes C# compilers prior to the one in which this feature is implemented).</span></span> 

<span data-ttu-id="079f1-124">Eski derleyicilerde hizmet vermeden çalışan başka iyi alternatifler olmadan, bilinen bir dizeye sahip bir `Obsolete` özniteliği, tüm ref benzeri yapılara eklenecektir.</span><span class="sxs-lookup"><span data-stu-id="079f1-124">Having no other good alternatives that work in old compilers without servicing, an `Obsolete` attribute with a known string will be added to all ref-like structs.</span></span> <span data-ttu-id="079f1-125">Ref benzeri türlerin nasıl kullanılacağını bilen derleyiciler, bu `Obsolete`belirli biçimini yoksayar.</span><span class="sxs-lookup"><span data-stu-id="079f1-125">Compilers that know how to use ref-like types will ignore this particular form of `Obsolete`.</span></span>

<span data-ttu-id="079f1-126">Tipik meta veri gösterimi:</span><span class="sxs-lookup"><span data-stu-id="079f1-126">A typical metadata representation:</span></span>

```csharp
    [IsRefLike]
    [Obsolete("Types with embedded references are not supported in this version of your compiler.")]
    public struct TwoSpans<T>
    {
       // . . . .
    }
```

<span data-ttu-id="079f1-127">Not: eski derleyicilerde ref benzeri türlerin herhangi bir kullanımı %100 hatası verdiğinde, bunu yapmak için hedef değildir.</span><span class="sxs-lookup"><span data-stu-id="079f1-127">NOTE: it is not the goal to make it so that any use of ref-like types on old compilers fails 100%.</span></span> <span data-ttu-id="079f1-128">Bu, elde etmek zordur ve kesinlikle gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="079f1-128">That is hard to achieve and is not strictly necessary.</span></span> <span data-ttu-id="079f1-129">Örneğin, dinamik kod kullanarak `Obsolete`, örneğin, yansıma aracılığıyla bir ref benzeri tür dizisi oluşturmak için her zaman bir yol olabilir.</span><span class="sxs-lookup"><span data-stu-id="079f1-129">For example there would always be a way to get around the `Obsolete` using dynamic code or, for example, creating an array of ref-like types through reflection.</span></span>

<span data-ttu-id="079f1-130">Özellikle, Kullanıcı gerçekten bir `Obsolete` veya `Deprecated` özniteliğini ref benzeri bir türe koymak istiyorsa, `Obsolete` özniteliği birden çok kez uygulanamadığından, önceden tanımlanmış olanı yaymayan bir seçim olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="079f1-130">In particular, if user wants to actually put an `Obsolete` or `Deprecated` attribute on a ref-like type, we will have no choice other than not emitting the predefined one since `Obsolete` attribute cannot be applied more than once..</span></span>  

## <a name="examples"></a><span data-ttu-id="079f1-131">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="079f1-131">Examples:</span></span>

```csharp
SpanLikeType M1(ref SpanLikeType x, Span<byte> y)
{
    // this is all valid, unconcerned with stack-referring stuff
    var local = new SpanLikeType(y);
    x = local;
    return x;
}

void Test1(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    // this is allowed
    stackReferring2 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    stackReferring2 = M1(ref param1, stackReferring1);

    // this is NOT allowed
    param1 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    param2 = stackReferring1.Slice(10);

    // this is allowed
    param1 = new SpanLikeType(param2);

    // this is allowed
    stackReferring2 = param1;
}

ref SpanLikeType M2(ref SpanLikeType x)
{
    return ref x;
}

ref SpanLikeType Test2(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    ref var stackReferring3 = M2(ref stackReferring2);

    // this is allowed
    stackReferring3 = M1(ref stackReferring2, stackReferring1);

    // this is allowed
    M2(ref stackReferring3) = stackReferring2;

    // this is NOT allowed
    M1(ref param1) = stackReferring2;

    // this is NOT allowed
    param1 = stackReferring3;

    // this is NOT allowed
    return ref stackReferring3;

    // this is allowed
    return ref param1;
}

```

----------------

## <a name="draft-language-specification"></a><span data-ttu-id="079f1-132">Taslak dil belirtimi</span><span class="sxs-lookup"><span data-stu-id="079f1-132">Draft language specification</span></span>

<span data-ttu-id="079f1-133">Aşağıda, bu türlerin değerlerinin yalnızca yığında gerçekleşmesini sağlamak için, ref benzeri türler (`ref struct`s) için bir dizi güvenlik kuralı tanımlanıyoruz.</span><span class="sxs-lookup"><span data-stu-id="079f1-133">Below we describe a set of safety rules for ref-like types (`ref struct`s) to ensure that values of these types occur only on the stack.</span></span> <span data-ttu-id="079f1-134">Yereller başvuruya göre geçirilmezse, farklı, daha basit bir güvenlik kuralları kümesi mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="079f1-134">A different, simpler set of safety rules would be possible if locals cannot be passed by reference.</span></span> <span data-ttu-id="079f1-135">Bu belirtim Ayrıca, ref yerellerine yönelik güvenli yeniden atamaya de izin verir.</span><span class="sxs-lookup"><span data-stu-id="079f1-135">This specification would also permit the safe reassignment of ref locals.</span></span>

### <a name="overview"></a><span data-ttu-id="079f1-136">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="079f1-136">Overview</span></span>

<span data-ttu-id="079f1-137">Derleme zamanında, ifadenin "güvenli-kaçış" öğesine geçmesine izin verilen kapsam kavramıyla birlikte her ifadeyle ilişkilendiyoruz.</span><span class="sxs-lookup"><span data-stu-id="079f1-137">We associate with each expression at compile-time the concept of what scope that expression is permitted to escape to, "safe-to-escape".</span></span> <span data-ttu-id="079f1-138">Benzer şekilde, her lvalue için, başvuruya yönelik bir başvurunun "ref-Safe-Escape" öğesine geçmesine izin verilen bir kavram vardır.</span><span class="sxs-lookup"><span data-stu-id="079f1-138">Similarly, for each lvalue we maintain a concept of what scope a reference to it is permitted to escape to, "ref-safe-to-escape".</span></span> <span data-ttu-id="079f1-139">Verili bir lvalue ifadesi için bu farklı olabilir.</span><span class="sxs-lookup"><span data-stu-id="079f1-139">For a given lvalue expression, these may be different.</span></span>

<span data-ttu-id="079f1-140">Bunlar REF Yereller özelliğinin "güvenli hale getirme" ile benzerdir, ancak daha ayrıntılı bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="079f1-140">These are analogous to the "safe to return" of the ref locals feature, but it is more fine-grained.</span></span> <span data-ttu-id="079f1-141">Bir ifadenin "iadeyle güvenli" ifadesi, kapsayan metodun bir bütün olarak ne olduğunu (veya değil) bir bütün olarak bir bütün olarak, hangi kapsamdaki kapsama sahip olabileceğini (Bu kapsam ötesinde değil) kaydeder.</span><span class="sxs-lookup"><span data-stu-id="079f1-141">Where the "safe-to-return" of an expression records only whether (or not) it may escape the enclosing method as a whole, the safe-to-escape records which scope it may escape to (which scope it may not escape beyond).</span></span> <span data-ttu-id="079f1-142">Temel güvenlik mekanizması aşağıdaki gibi zorlanır.</span><span class="sxs-lookup"><span data-stu-id="079f1-142">The basic safety mechanism is enforced as follows.</span></span> <span data-ttu-id="079f1-143">Bir ifadeden atama, güvenli-çıkış kapsamı S1 ile bir (lvalue) Expression E2 ile, güvenli-kaçış kapsamı S2 ile bir (lvalue) ifadesinde bir atama verildiğinde, S2 S1 'ten daha geniş bir kapsamsa, bu bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="079f1-143">Given an assignment from an expression E1 with a safe-to-escape scope S1, to an (lvalue) expression E2 with safe-to-escape scope S2, it is an error if S2 is a wider scope than S1.</span></span> <span data-ttu-id="079f1-144">Oluşturma işlemi sırasında, S1 ve S2 iki kapsam bir iç içe ilişkide olduğundan, yasal bir ifade her zaman ifadeyi kapsayan bazı kapsamlardan her zaman güvenli bir şekilde döndürülür.</span><span class="sxs-lookup"><span data-stu-id="079f1-144">By construction, the two scopes S1 and S2 are in a nesting relationship, because a legal expression is always safe-to-return from some scope enclosing the expression.</span></span>

<span data-ttu-id="079f1-145">Analiz amacı için, çözümlemenin amacına yönelik olarak yalnızca iki kapsamı ve yöntemin en üst düzey kapsamını desteklemek için gereken süre.</span><span class="sxs-lookup"><span data-stu-id="079f1-145">For the time being it is sufficient, for the purpose of the analysis, to support just two scopes - external to the method, and top-level scope of the method.</span></span> <span data-ttu-id="079f1-146">Bunun nedeni, iç kapsamlarla başvuru benzeri değerler oluşturulamadığından ve ref yerelleri yeniden atamayı desteklemez.</span><span class="sxs-lookup"><span data-stu-id="079f1-146">That is because ref-like values with inner scopes cannot be created and ref locals do not support re-assignment.</span></span> <span data-ttu-id="079f1-147">Ancak kurallar, ikiden fazla kapsam düzeyini destekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="079f1-147">The rules, however, can support more than two scope levels.</span></span>

<span data-ttu-id="079f1-148">Bir ifadenin *güvenli dönüş* durumunu ve ifadelerin yasallığını yöneten kuralları izlemek için kesin kurallar.</span><span class="sxs-lookup"><span data-stu-id="079f1-148">The precise rules for computing the *safe-to-return* status of an expression, and the rules governing the legality of expressions, follow.</span></span>

### <a name="ref-safe-to-escape"></a><span data-ttu-id="079f1-149">ref-güvenli-çıkış</span><span class="sxs-lookup"><span data-stu-id="079f1-149">ref-safe-to-escape</span></span>

<span data-ttu-id="079f1-150">*Ref-Safe-kaçış* , lvalue ile kaçış için bir başvuru için güvenli olduğu bir lvalue ifadesi kapsayan bir kapsamdır.</span><span class="sxs-lookup"><span data-stu-id="079f1-150">The *ref-safe-to-escape* is a scope, enclosing an lvalue expression, to which it is safe for a ref to the lvalue to escape to.</span></span> <span data-ttu-id="079f1-151">Bu kapsam tüm yöntem ise, lvalue için bir başvurunun, yönteminden *geri dönmesi için güvenli* olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="079f1-151">If that scope is the entire method, we say that a ref to the lvalue is *safe to return* from the method.</span></span>

### <a name="safe-to-escape"></a><span data-ttu-id="079f1-152">güvenle kaçış</span><span class="sxs-lookup"><span data-stu-id="079f1-152">safe-to-escape</span></span>

<span data-ttu-id="079f1-153">*Güvenli-kaçış* , bir ifadeyi kapsayan ve değerin kaçış değeri için güvenli olduğu bir kapsamdır.</span><span class="sxs-lookup"><span data-stu-id="079f1-153">The *safe-to-escape* is a scope, enclosing an expression, to which it is safe for the value to escape to.</span></span> <span data-ttu-id="079f1-154">Bu kapsam tüm yöntem ise, bir değerin yönteminden *döndürülmesi için güvenli* olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="079f1-154">If that scope is the entire method, we say that a the value is *safe to return* from the method.</span></span>

<span data-ttu-id="079f1-155">Türü `ref struct` olmayan bir ifade, kapsayan metodun bütünü ile *güvenli bir şekilde* döndürülür.</span><span class="sxs-lookup"><span data-stu-id="079f1-155">An expression whose type is not a `ref struct` type is *safe-to-return* from the entire enclosing method.</span></span> <span data-ttu-id="079f1-156">Aksi takdirde aşağıdaki kurallara değineceğiz.</span><span class="sxs-lookup"><span data-stu-id="079f1-156">Otherwise we refer to the rules below.</span></span>

#### <a name="parameters"></a><span data-ttu-id="079f1-157">Parametreler</span><span class="sxs-lookup"><span data-stu-id="079f1-157">Parameters</span></span>

<span data-ttu-id="079f1-158">Biçimsel bir parametre atayarak bir lvalue, *ref-Safe-kaçış* (başvuruya göre) ile aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="079f1-158">An lvalue designating a formal parameter is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="079f1-159">Parametre bir `ref`, `out`veya `in` parametre ise, tüm metodun (ör. bir `return ref` ifadesiyle) *ref ile güvenli çıkış* olur; güvenmiyorsanız</span><span class="sxs-lookup"><span data-stu-id="079f1-159">If the parameter is a `ref`, `out`, or `in` parameter, it is *ref-safe-to-escape* from the entire method (e.g. by a `return ref` statement); otherwise</span></span>
- <span data-ttu-id="079f1-160">Parametresi bir yapı türünün `this` parametresi ise, yöntemin en üst düzey kapsamına *ref-to-kaçış* olur (ancak yöntemin tamamen değil); [Örnek](#struct-this-escape)</span><span class="sxs-lookup"><span data-stu-id="079f1-160">If the parameter is the `this` parameter of a struct type, it is *ref-safe-to-escape* to the top-level scope of the method (but not from the entire method itself); [Sample](#struct-this-escape)</span></span>
- <span data-ttu-id="079f1-161">Aksi takdirde parametre bir değer parametresidir ve yöntemin en üst düzey kapsamına *ref-Safe-kaçış* (yöntemin kendisinden değil).</span><span class="sxs-lookup"><span data-stu-id="079f1-161">Otherwise the parameter is a value parameter, and it is *ref-safe-to-escape* to the top-level scope of the method (but not from the method itself).</span></span>

<span data-ttu-id="079f1-162">Bir rvalue parametresinin kullanımını tanımlayan bir ifade, tüm metodun (örn. bir `return` deyimine göre) *güvenli kaçış* (değere göre) ' dır.</span><span class="sxs-lookup"><span data-stu-id="079f1-162">An expression that is an rvalue designating the use of a formal parameter is *safe-to-escape* (by value) from the entire method (e.g. by a `return` statement).</span></span> <span data-ttu-id="079f1-163">Bu `this` parametresi için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="079f1-163">This applies to the `this` parameter as well.</span></span>

#### <a name="locals"></a><span data-ttu-id="079f1-164">Yerel öğeler</span><span class="sxs-lookup"><span data-stu-id="079f1-164">Locals</span></span>

<span data-ttu-id="079f1-165">Yerel değişken atama bir lvalue, *ref-Safe-kaçış* (başvuruya göre) ile aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="079f1-165">An lvalue designating a local variable is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="079f1-166">Değişken bir `ref` değişkense, *ref-Safe-kaçış* , başlatma ifadesinin *ref-Safe-kaçış* değerinden alınır; güvenmiyorsanız</span><span class="sxs-lookup"><span data-stu-id="079f1-166">If the variable is a `ref` variable, then its *ref-safe-to-escape* is taken from the *ref-safe-to-escape* of its initializing expression; otherwise</span></span>
- <span data-ttu-id="079f1-167">Değişken *ref-Safe-Escape* ' in bildirildiği kapsamdır.</span><span class="sxs-lookup"><span data-stu-id="079f1-167">The variable is *ref-safe-to-escape* the scope in which it was declared.</span></span>

<span data-ttu-id="079f1-168">Bir yerel değişken kullanımını belirleme rvalue olan bir ifade, *güvenli çıkış* (değere göre) ile aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="079f1-168">An expression that is an rvalue designating the use of a local variable is *safe-to-escape* (by value) as follows:</span></span>
- <span data-ttu-id="079f1-169">Ancak yukarıdaki genel kural, türü `ref struct` olmayan yerel bir tür, kapsayan metodun tamamından daha *güvenli bir şekilde* döndürülür.</span><span class="sxs-lookup"><span data-stu-id="079f1-169">But the general rule above, a local whose type is not a `ref struct` type is *safe-to-return* from the entire enclosing method.</span></span>
- <span data-ttu-id="079f1-170">Değişken bir `foreach` döngüsünün yineleme değişkensiyse, değişkenin *güvenli-çıkış* kapsamı, `foreach` döngüsünün ifadesinin *güvenli kaçış* ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="079f1-170">If the variable is an iteration variable of a `foreach` loop, then the variable's *safe-to-escape* scope is the same as the *safe-to-escape* of the `foreach` loop's expression.</span></span>
- <span data-ttu-id="079f1-171">`ref struct` türü yerel ve bildirim noktasında başlatılmamış, kapsayan metodun tamamından daha *güvenli bir şekilde* döndürülür.</span><span class="sxs-lookup"><span data-stu-id="079f1-171">A local of `ref struct` type and uninitialized at the point of declaration is *safe-to-return* from the entire enclosing method.</span></span>
- <span data-ttu-id="079f1-172">Aksi takdirde, değişkenin türü bir `ref struct` türüdür ve değişkenin bildirimi bir başlatıcı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="079f1-172">Otherwise the variable's type is a `ref struct` type, and the variable's declaration requires an initializer.</span></span> <span data-ttu-id="079f1-173">Değişkenin güvenli- *Çıkış* kapsamı, başlatıcısının *güvenli kaçış* ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="079f1-173">The variable's *safe-to-escape* scope is the same as the *safe-to-escape* of its initializer.</span></span>

#### <a name="field-reference"></a><span data-ttu-id="079f1-174">Alan başvurusu</span><span class="sxs-lookup"><span data-stu-id="079f1-174">Field reference</span></span>

<span data-ttu-id="079f1-175">Bir lvalue, `e.F`bir alana başvuru atayarak aşağıdaki gibi *ref-Safe-kaçış* (başvuruya göre) olur:</span><span class="sxs-lookup"><span data-stu-id="079f1-175">An lvalue designating a reference to a field, `e.F`, is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="079f1-176">`e` bir başvuru türü ise, tüm yöntemin *ref ile güvenli çıkış* olur; güvenmiyorsanız</span><span class="sxs-lookup"><span data-stu-id="079f1-176">If `e` is of a reference type, it is *ref-safe-to-escape* from the entire method; otherwise</span></span>
- <span data-ttu-id="079f1-177">`e` bir değer türünde ise, *ref-Safe-kaçış* , `e`*ref-Safe* 'ten kaçış üzerinden alınır.</span><span class="sxs-lookup"><span data-stu-id="079f1-177">If `e` is of a value type, its *ref-safe-to-escape* is taken from the *ref-safe-to-escape* of `e`.</span></span>

<span data-ttu-id="079f1-178">`e.F`bir alana başvuru atayarak bir rvalue, `e`*güvenli kaçış* ile aynı olan *güvenli-çıkış* kapsamına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="079f1-178">An rvalue designating a reference to a field, `e.F`, has a *safe-to-escape* scope that is the same as the *safe-to-escape* of `e`.</span></span>

#### <a name="operators-including-"></a><span data-ttu-id="079f1-179">`?:` dahil işleçler</span><span class="sxs-lookup"><span data-stu-id="079f1-179">Operators including `?:`</span></span>

<span data-ttu-id="079f1-180">Kullanıcı tanımlı bir işlecin uygulaması, yöntem çağırma işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="079f1-180">The application of a user-defined operator is treated as a method invocation.</span></span>

<span data-ttu-id="079f1-181">`e1 + e2` veya `c ? e1 : e2`gibi bir rvalue veren bir operatör için, sonucun *güvenli-kaçış* , işlecin işlenenlerinin *güvenli-kaçış* arasındaki en dar kapsamdır.</span><span class="sxs-lookup"><span data-stu-id="079f1-181">For an operator that yields an rvalue, such as `e1 + e2` or `c ? e1 : e2`, the *safe-to-escape* of the result is the narrowest scope among the *safe-to-escape* of the operands of the operator.</span></span>  <span data-ttu-id="079f1-182">Sonuç olarak, `+e`gibi bir rvalue döndüren birli operatör için, *sonucun güvenli-* çıkış işlemi, işlenenin *güvenle kaçış* olur.</span><span class="sxs-lookup"><span data-stu-id="079f1-182">As a consequence, for a unary operator that yields an rvalue, such as `+e`, the *safe-to-escape* of the result is the *safe-to-escape* of the operand.</span></span>

<span data-ttu-id="079f1-183">`c ? ref e1 : ref e2` gibi bir lvalue veren bir operatör için</span><span class="sxs-lookup"><span data-stu-id="079f1-183">For an operator that yields an lvalue, such as `c ? ref e1 : ref e2`</span></span>
- <span data-ttu-id="079f1-184">sonucun *ref-Safe-kaçış* , işlecin işlenenlerinin *ref-Safe-kaçış* arasındaki en dar kapsamdır.</span><span class="sxs-lookup"><span data-stu-id="079f1-184">the *ref-safe-to-escape* of the result is the narrowest scope among the *ref-safe-to-escape* of the operands of the operator.</span></span>
- <span data-ttu-id="079f1-185">işlenenlerin *güvenli-çıkış* değeri kabul etmelidir ve sonuçta ortaya çıkan lvalue 'nin *güvenle kaçış* durumunda olur.</span><span class="sxs-lookup"><span data-stu-id="079f1-185">the *safe-to-escape* of the operands must agree, and that is the *safe-to-escape* of the resulting lvalue.</span></span>

#### <a name="method-invocation"></a><span data-ttu-id="079f1-186">Yöntem çağırma</span><span class="sxs-lookup"><span data-stu-id="079f1-186">Method invocation</span></span>

<span data-ttu-id="079f1-187">Ref döndüren yöntem çağırma `e1.M(e2, ...)` sonucu olan bir lvalue, aşağıdaki kapsamların en küçüğü olan *ref-Safe-Escape* ' dir:</span><span class="sxs-lookup"><span data-stu-id="079f1-187">An lvalue resulting from a ref-returning method invocation `e1.M(e2, ...)` is *ref-safe-to-escape* the smallest of the following scopes:</span></span>
- <span data-ttu-id="079f1-188">Kapsayan metodun tamamı</span><span class="sxs-lookup"><span data-stu-id="079f1-188">The entire enclosing method</span></span>
- <span data-ttu-id="079f1-189">Tüm `ref` ve `out` bağımsız değişken ifadelerinin *ref-Safe-kaçış* (alıcı hariç)</span><span class="sxs-lookup"><span data-stu-id="079f1-189">the *ref-safe-to-escape* of all `ref` and `out` argument expressions (excluding the receiver)</span></span>
- <span data-ttu-id="079f1-190">Yöntemin her `in` parametresi için, lvalue olan karşılık gelen bir ifade varsa, *ref-Safe-kaçış*, aksi halde en yakın kapsayan kapsam</span><span class="sxs-lookup"><span data-stu-id="079f1-190">For each `in` parameter of the method, if there is a corresponding expression that is an lvalue, its *ref-safe-to-escape*, otherwise the nearest enclosing scope</span></span>
- <span data-ttu-id="079f1-191">Tüm bağımsız değişken ifadelerinin (alıcı dahil) *güvenli-çıkış*</span><span class="sxs-lookup"><span data-stu-id="079f1-191">the *safe-to-escape* of all argument expressions (including the receiver)</span></span>

> <span data-ttu-id="079f1-192">Note: son madde işareti, gibi bir kodu işlemek için gereklidir</span><span class="sxs-lookup"><span data-stu-id="079f1-192">Note: the last bullet is necessary to handle code such as</span></span>
> ```csharp
> var sp = new Span(...)
> return ref sp[0];
> ```
> <span data-ttu-id="079f1-193">or</span><span class="sxs-lookup"><span data-stu-id="079f1-193">or</span></span>
> ```csharp
> return ref M(sp, 0);
> ```

<span data-ttu-id="079f1-194">Yöntem çağırma `e1.M(e2, ...)` sonucu olan bir rvalue, aşağıdaki kapsamların en küçüğü ile *güvenli çıkış* olur:</span><span class="sxs-lookup"><span data-stu-id="079f1-194">An rvalue resulting from a method invocation `e1.M(e2, ...)` is *safe-to-escape* from the smallest of the following scopes:</span></span>
- <span data-ttu-id="079f1-195">Kapsayan metodun tamamı</span><span class="sxs-lookup"><span data-stu-id="079f1-195">The entire enclosing method</span></span>
- <span data-ttu-id="079f1-196">Tüm bağımsız değişken ifadelerinin (alıcı dahil) *güvenli-çıkış*</span><span class="sxs-lookup"><span data-stu-id="079f1-196">the *safe-to-escape* of all argument expressions (including the receiver)</span></span>

#### <a name="an-rvalue"></a><span data-ttu-id="079f1-197">Bir rvalue</span><span class="sxs-lookup"><span data-stu-id="079f1-197">An Rvalue</span></span>
<span data-ttu-id="079f1-198">Rvalue, en yakın kapsayan kapsamdan *Çıkış açısından güvenlidir* .</span><span class="sxs-lookup"><span data-stu-id="079f1-198">An rvalue is *ref-safe-to-escape* from the nearest enclosing scope.</span></span> <span data-ttu-id="079f1-199">Bu, örneğin, `d` `dynamic`türünde olduğu `M(ref d.Length)` gibi bir çağırmada meydana gelir.</span><span class="sxs-lookup"><span data-stu-id="079f1-199">This occurs for example in an invocation such as `M(ref d.Length)` where `d` is of type `dynamic`.</span></span> <span data-ttu-id="079f1-200">Ayrıca, `in` parametrelere karşılık gelen bağımsız değişkenlerin işlenmesiyle (ve belki de bu şekilde) tutarlıdır. \*</span><span class="sxs-lookup"><span data-stu-id="079f1-200">It is also consistent with (and perhaps subsumes) our handling of arguments corresponding to `in` parameters.\*</span></span>

#### <a name="property-invocations"></a><span data-ttu-id="079f1-201">Özellik etkinleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="079f1-201">Property invocations</span></span>

<span data-ttu-id="079f1-202">Bir özellik çağrısı (`get` ya da `set`) Yukarıdaki kurallar tarafından temel alınan metodun Yöntem çağrısı olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="079f1-202">A property invocation (either `get` or `set`) it treated as a method invocation of the underlying method by the above rules.</span></span>

#### `stackalloc`

<span data-ttu-id="079f1-203">Bir stackalloc ifadesi, metodun en üst düzey kapsamına *güvenli-kaçış* olan bir rvalue 'dir (ancak yöntemin tamamen değil).</span><span class="sxs-lookup"><span data-stu-id="079f1-203">A stackalloc expression is an rvalue that is *safe-to-escape* to the top-level scope of the method (but not from the entire method itself).</span></span>

#### <a name="constructor-invocations"></a><span data-ttu-id="079f1-204">Oluşturucu etkinleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="079f1-204">Constructor invocations</span></span>

<span data-ttu-id="079f1-205">Bir oluşturucuyu çağıran `new` ifadesi, oluşturulmakta olan türü döndürmek için kabul edilen bir yöntem çağrısı ile aynı kurallara uyar.</span><span class="sxs-lookup"><span data-stu-id="079f1-205">A `new` expression that invokes a constructor obeys the same rules as a method invocation that is considered to return the type being constructed.</span></span>

<span data-ttu-id="079f1-206">Buna ek olarak, *güvenli-kaçış* , nesne Başlatıcı ifadelerinin tüm bağımsız değişkenlerin/işlenenlerinin, yani Başlatıcı varsa özyinelemeli olarak *güvenli-kaçış* özelliğinden daha büyük değildir.</span><span class="sxs-lookup"><span data-stu-id="079f1-206">In addition *safe-to-escape* is no wider than the smallest of the *safe-to-escape* of all arguments/operands of the object initializer expressions, recursively, if initializer is present.</span></span> 

#### <a name="span-constructor"></a><span data-ttu-id="079f1-207">Span Oluşturucusu</span><span class="sxs-lookup"><span data-stu-id="079f1-207">Span constructor</span></span>
<span data-ttu-id="079f1-208">Dil, aşağıdaki biçimdeki bir oluşturucuya sahip değil `Span<T>` bağımlıdır:</span><span class="sxs-lookup"><span data-stu-id="079f1-208">The language relies on `Span<T>` not having a constructor of the following form:</span></span>

```csharp
void Example(ref int x)
{
    // Create a span of length one
    var span = new Span<int>(ref x); 
}
```

<span data-ttu-id="079f1-209">Böyle bir Oluşturucu, bir `ref` alanından ayırt edilemeyen alan olarak kullanılan `Span<T>` yapar.</span><span class="sxs-lookup"><span data-stu-id="079f1-209">Such a constructor makes `Span<T>` which are used as fields indistinguishable from a `ref` field.</span></span> <span data-ttu-id="079f1-210">Bu belgede açıklanan güvenlik kuralları, veya .NET sürümünde C#geçerli bir yapı olmadığı `ref` alanlara bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="079f1-210">The safety rules described in this document depend on `ref` fields not being a valid construct in C#, or .NET.</span></span>

#### <a name="default-expressions"></a><span data-ttu-id="079f1-211">`default` ifadeleri</span><span class="sxs-lookup"><span data-stu-id="079f1-211">`default` expressions</span></span>

<span data-ttu-id="079f1-212">Bir `default` ifadesi, kapsayan metodun tamamından *güvenle kaçış* olur.</span><span class="sxs-lookup"><span data-stu-id="079f1-212">A `default` expression is *safe-to-escape* from the entire enclosing method.</span></span>

## <a name="language-constraints"></a><span data-ttu-id="079f1-213">Dil kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="079f1-213">Language Constraints</span></span>

<span data-ttu-id="079f1-214">`ref` yerel değişken olmamasını ve `ref struct` türde değişken olmamasını sağlamak istiyoruz, yığın belleğine veya artık etkin olmayan değişkenlere başvuruda bulunur.</span><span class="sxs-lookup"><span data-stu-id="079f1-214">We wish to ensure that no `ref` local variable, and no variable of `ref struct` type, refers to stack memory or variables that are no longer alive.</span></span> <span data-ttu-id="079f1-215">Bu nedenle, aşağıdaki dil kısıtlamalarına sahip olduğumuz:</span><span class="sxs-lookup"><span data-stu-id="079f1-215">We therefore have the following language constraints:</span></span>

- <span data-ttu-id="079f1-216">Bir başvuru parametresi veya bir başvuru yerel veya bir parametre ya da bir `ref struct` türü yerel bir lambda veya yerel işleve yükseltilmemiş olamaz.</span><span class="sxs-lookup"><span data-stu-id="079f1-216">Neither a ref parameter, nor a ref local, nor a parameter or local of a `ref struct` type can be lifted into a lambda or local function.</span></span>

- <span data-ttu-id="079f1-217">Hiçbir başvuru parametresi ne de `ref struct` bir parametre bir yineleyici yöntemi veya bir `async` metodu üzerinde bir bağımsız değişken olabilir.</span><span class="sxs-lookup"><span data-stu-id="079f1-217">Neither a ref parameter nor a parameter of a `ref struct` type may be an argument on an iterator method or an `async` method.</span></span>

- <span data-ttu-id="079f1-218">Bir ref yerel veya `ref struct` türü yerel bir `yield return` deyimi veya `await` ifadesi noktasında kapsam içinde olabilir.</span><span class="sxs-lookup"><span data-stu-id="079f1-218">Neither a ref local, nor a local of a `ref struct` type may be in scope at the point of a `yield return` statement or an `await` expression.</span></span>

- <span data-ttu-id="079f1-219">`ref struct` türü, tür bağımsız değişkeni olarak veya demet türünde bir öğe türü olarak kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="079f1-219">A `ref struct` type may not be used as a type argument, or as an element type in a tuple type.</span></span>

- <span data-ttu-id="079f1-220">Bir `ref struct` türü, başka bir `ref struct`örnek alanının belirtilen türü olabileceğinden, bir alanın bildirilmeyen türü olamaz.</span><span class="sxs-lookup"><span data-stu-id="079f1-220">A `ref struct` type may not be the declared type of a field, except that it may be the declared type of an instance field of another `ref struct`.</span></span>

- <span data-ttu-id="079f1-221">`ref struct` türü bir dizinin öğe türü olamaz.</span><span class="sxs-lookup"><span data-stu-id="079f1-221">A `ref struct` type may not be the element type of an array.</span></span>

- <span data-ttu-id="079f1-222">`ref struct` bir türün değeri kutulanmamış olabilir:</span><span class="sxs-lookup"><span data-stu-id="079f1-222">A value of a `ref struct` type may not be boxed:</span></span>
  - <span data-ttu-id="079f1-223">`ref struct` türünden `object` veya `System.ValueType`türüne dönüştürme yok.</span><span class="sxs-lookup"><span data-stu-id="079f1-223">There is no conversion from a `ref struct` type to the type `object` or the type `System.ValueType`.</span></span>
  - <span data-ttu-id="079f1-224">Bir `ref struct` türü, herhangi bir arabirim uygulamak için bildirilemez</span><span class="sxs-lookup"><span data-stu-id="079f1-224">A `ref struct` type may not be declared to implement any interface</span></span>
  - <span data-ttu-id="079f1-225">`object` veya `System.ValueType` içinde bildirilmemiş ancak `ref struct` türünde geçersiz kılınamayan bir örnek yöntemi bu `ref struct` türünde bir alıcısıyla çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="079f1-225">No instance method declared in `object` or in `System.ValueType` but not overridden in a `ref struct` type may be called with a receiver of that `ref struct` type.</span></span>
  - <span data-ttu-id="079f1-226">Bir `ref struct` türünün örnek yöntemi, bir temsilci türüne Yöntem dönüştürmeye göre yakalanamaz.</span><span class="sxs-lookup"><span data-stu-id="079f1-226">No instance method of a `ref struct` type may be captured by method conversion to a delegate type.</span></span>

- <span data-ttu-id="079f1-227">Ref yeniden atama `ref e1 = ref e2`için, *ref-safe* `e2`, `e1`*ref ile güvenli çıkış* olarak en az geniş bir kapsam olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="079f1-227">For a ref reassignment `ref e1 = ref e2`, the *ref-safe-to-escape* of `e2` must be at least as wide a scope as the *ref-safe-to-escape* of `e1`.</span></span>

- <span data-ttu-id="079f1-228">Ref Return ifadesinde `return ref e1`için *ref-Safe-kaçış* `e1` tüm metodun *ref-Safe-kaçış* olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="079f1-228">For a ref return statement `return ref e1`, the *ref-safe-to-escape* of `e1` must be *ref-safe-to-escape* from the entire method.</span></span> <span data-ttu-id="079f1-229">(TODO: `e1`, yöntemin tamamında *güvenli çıkış* olması veya gereksiz bir kural olması gerekir mi?)</span><span class="sxs-lookup"><span data-stu-id="079f1-229">(TODO: Do we also need a rule that `e1` must be *safe-to-escape* from the entire method, or is that redundant?)</span></span>

- <span data-ttu-id="079f1-230">Bir return ifadesinin `return e1`için, `e1` *güvenli kaçış* , tüm metodun *güvenle kaçış* olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="079f1-230">For a return statement `return e1`, the *safe-to-escape* of `e1` must be *safe-to-escape* from the entire method.</span></span>

- <span data-ttu-id="079f1-231">Bir atama `e1 = e2`için, `e1` türü bir `ref struct` türü ise, `e2` *güvenli* *çıkış olarak* en az geniş bir kapsam olan `e1`.</span><span class="sxs-lookup"><span data-stu-id="079f1-231">For an assignment `e1 = e2`, if the type of `e1` is a `ref struct` type, then the *safe-to-escape* of `e2` must be at least as wide a scope as the *safe-to-escape* of `e1`.</span></span>

- <span data-ttu-id="079f1-232">Bir `ref struct` türünün (alıcı dahil) bir `ref` veya `out` bağımsız değişkeni varsa, *güvenle kaçış* E1 ile, bir bağımsız değişken (alıcı dahil) varsa, daha dar bir *güvenli-kaçış* yöntemi olabilir.</span><span class="sxs-lookup"><span data-stu-id="079f1-232">For a method invocation if there is a `ref` or `out` argument of a `ref struct` type (including the receiver), with *safe-to-escape* E1, then no argument (including the receiver) may have a narrower *safe-to-escape* than E1.</span></span> [<span data-ttu-id="079f1-233">Örnek</span><span class="sxs-lookup"><span data-stu-id="079f1-233">Sample</span></span>](#method-arguments-must-match)

- <span data-ttu-id="079f1-234">Yerel bir işlev veya anonim işlev, kapsayan bir kapsamda belirtilen `ref struct` türünün yerel veya parametresine başvuramaz.</span><span class="sxs-lookup"><span data-stu-id="079f1-234">A local function or anonymous function may not refer to a local or parameter of `ref struct` type declared in an enclosing scope.</span></span>

> <span data-ttu-id="079f1-235">***Açık sorun:*** Örneğin, bir await ifadesinde bir `ref struct` türünün yığın değerini taşımamıza gerek olduğunda hata üretmemize olanak tanıyan bir kural olması gerekir, örneğin, koddaki</span><span class="sxs-lookup"><span data-stu-id="079f1-235">***Open Issue:*** We need some rule that permits us to produce an error when needing to spill a stack value of a `ref struct` type at an await expression, for example in the code</span></span>
> ```csharp
> Foo(new Span<int>(...), await e2);
> ```

## <a name="explanations"></a><span data-ttu-id="079f1-236">Açýkla</span><span class="sxs-lookup"><span data-stu-id="079f1-236">Explanations</span></span>
<span data-ttu-id="079f1-237">Bu açıklamalar ve örnekler, üzerinde kaç tane güvenlik kuralı olduğunu açıklamaya yardımcı olur</span><span class="sxs-lookup"><span data-stu-id="079f1-237">These explanations and samples help explain why many of the safety rules above exist</span></span>

### <a name="method-arguments-must-match"></a><span data-ttu-id="079f1-238">Yöntem bağımsız değişkenlerinin eşleşmesi gerekir</span><span class="sxs-lookup"><span data-stu-id="079f1-238">Method Arguments Must Match</span></span>
<span data-ttu-id="079f1-239">`out`, alıcı dahil bir `ref struct` `ref` parametresinin bulunduğu bir yöntemi çağırırken, tüm `ref struct` aynı yaşam süresine sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="079f1-239">When invoking a method where there is an `out`, `ref` parameter that is a `ref struct` including the receiver then all of the `ref struct` need to have the same lifetime.</span></span> <span data-ttu-id="079f1-240">Bu gereklidir çünkü C# , yöntemin imzasında ve çağrı sitesindeki değerlerin yaşam süresi içinde bulunan bilgilere göre yaşam süresi güvenlerinin her türlü kararlarını vermelidir.</span><span class="sxs-lookup"><span data-stu-id="079f1-240">This is necessary because C# must make all of it's decisions around lifetime safety based on the information available in the signature of the method and the lifetime of the values at the call site.</span></span> 

<span data-ttu-id="079f1-241">`ref struct` `ref` parametreler varsa, bunların içerikleri etrafında takas olabilecek olasılık vardır.</span><span class="sxs-lookup"><span data-stu-id="079f1-241">When there are `ref` parameters that are `ref struct` then there is the possiblity they could swap around their contents.</span></span> <span data-ttu-id="079f1-242">Bu nedenle, çağrı sitesinde bu **olası** tüm nesnelerin tümünün uyumlu olduğundan emin olunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="079f1-242">Hence at the call site we must ensure all of these **potential** swaps are compatible.</span></span> <span data-ttu-id="079f1-243">Dil bu şekilde zorlanmadıysa, aşağıdaki gibi hatalı kod sağlar.</span><span class="sxs-lookup"><span data-stu-id="079f1-243">If the language didn't enforce that then it will allow for bad code like the following.</span></span>

```csharp
void M1(ref Span<int> s1)
{
    Span<int> s2 = stackalloc int[1];
    Swap(ref s1, ref s2);
}

void Swap(ref Span<int> x, ref int Span<int> y)
{
    // This will effectively assign the stackalloc to the s1 parameter and allow it
    // to escape to the caller of M1
    ref x = ref y; 
}
```

<span data-ttu-id="079f1-244">İçeriğin hiçbiri ref ile güvenli kaçış olmadığından, alıcı üzerindeki kısıtlama gereklidir, ancak belirtilen değerleri depolayabilirler.</span><span class="sxs-lookup"><span data-stu-id="079f1-244">The restriction on the receiver is necessary because while none of its contents are ref-safe-to-escape it can store the provided values.</span></span> <span data-ttu-id="079f1-245">Bu, eşleşmeyen yaşam süreleri ile aşağıdaki şekilde bir tür güvenliği deliği oluşturabileceğiniz anlamına gelir:</span><span class="sxs-lookup"><span data-stu-id="079f1-245">This means with mismatched lifetimes you could create a type safety hole in the following way:</span></span>

```csharp
ref struct S
{
    public Span<int> Span;

    public void Set(Span<int> span)
    {
        Span = span;
    }
}

void Broken(ref S s)
{
    Span<int> span = stackalloc int[1];

    // The result of a stackalloc is now stored in s.Span and escaped to the caller
    // of Broken
    s.Set(span); 
}
```

### <a name="struct-this-escape"></a><span data-ttu-id="079f1-246">Bu kaçış yapısı</span><span class="sxs-lookup"><span data-stu-id="079f1-246">Struct This Escape</span></span>
<span data-ttu-id="079f1-247">Bir örnek üyesinde `this` değeri, üyeye bir parametre olarak modellenir.</span><span class="sxs-lookup"><span data-stu-id="079f1-247">When it comes to span safety rules the `this` value in an instance member is modeled as a parameter to the member.</span></span> <span data-ttu-id="079f1-248">Artık `struct` için `this` türü aslında yalnızca bir `class` `ref S` (örneğin, bir `S` olarak adlandırılan Üyeler için) `class / struct`.</span><span class="sxs-lookup"><span data-stu-id="079f1-248">Now for a `struct` the type of `this` is actually `ref S` where in a `class` it's simply `S` (for members of a `class / struct` named S).</span></span> 

<span data-ttu-id="079f1-249">Henüz `this` diğer `ref` parametrelerinden farklı kaçış kurallarına sahip.</span><span class="sxs-lookup"><span data-stu-id="079f1-249">Yet `this` has different escaping rules than other `ref` parameters.</span></span> <span data-ttu-id="079f1-250">Özel olarak, başka parametreler olduğu sürece ref ile güvenli kaçış değildir:</span><span class="sxs-lookup"><span data-stu-id="079f1-250">Specifically it is not ref-safe-to-escape while other parameters are:</span></span>

```csharp
ref struct S
{ 
    int Field;

    // Illegal because this isn't safe to escape as ref
    ref int Get() => ref Field;

    // Legal
    ref int GetParam(ref int p) => ref p;
}
```

<span data-ttu-id="079f1-251">Bu kısıtlamanın nedeni aslında `struct` üye çağırma ile biraz daha fazla yapılır.</span><span class="sxs-lookup"><span data-stu-id="079f1-251">The reason for this restriction actually has little to do with `struct` member invocation.</span></span> <span data-ttu-id="079f1-252">Alıcının rvalue olduğu `struct` üyelerdeki üye çağrısına göre çalışması gereken bazı kurallar vardır.</span><span class="sxs-lookup"><span data-stu-id="079f1-252">There are some rules that need to be worked out with respect to member invocation on `struct` members where the receiver is an rvalue.</span></span> <span data-ttu-id="079f1-253">Ancak bu çok ulaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="079f1-253">But that is very approachable.</span></span> 

<span data-ttu-id="079f1-254">Bu kısıtlamanın nedeni, arabirim çağırma ile ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="079f1-254">The reason for this restriction is actually about interface invocation.</span></span> <span data-ttu-id="079f1-255">Özellikle, aşağıdaki örneğin derlenmeyeceği veya derlenmemelidir.</span><span class="sxs-lookup"><span data-stu-id="079f1-255">Specifically it comes down to whether or not the following sample should or should not compile;</span></span>

```csharp
interface I1
{
    ref int Get();
}

ref int Use<T>(T p)
    where T : I1
{
    return ref p.Get();
}
```

<span data-ttu-id="079f1-256">`T` `struct`olarak örneklendiği durumu göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="079f1-256">Consider the case where `T` is instantiated as a `struct`.</span></span> <span data-ttu-id="079f1-257">`this` parametresi ref-Safe-kaçış ise, `p.Get` dönüşü yığına işaret verebilir (özellikle de, Örneklenmiş türdeki `T`).</span><span class="sxs-lookup"><span data-stu-id="079f1-257">If the `this` parameter is ref-safe-to-escape then the return of `p.Get` could point to the stack (specifically it could be a field inside of the instantiated type of `T`).</span></span> <span data-ttu-id="079f1-258">Bu, dilin bir yığın konumuna `ref` döndürülürken bu örneğin derlemeye izin vermediği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="079f1-258">That means the language could not allow this sample to compile as it could be returning a `ref` to a stack location.</span></span> <span data-ttu-id="079f1-259">Diğer taraftan, `this` ref-Safe-kaçış değilse `p.Get` yığına başvuramaz ve bu nedenle döndürmek güvenli hale gelir.</span><span class="sxs-lookup"><span data-stu-id="079f1-259">On the other hand if `this` is not ref-safe-to-escape then `p.Get` cannot refer to the stack and hence it's safe to return.</span></span> 

<span data-ttu-id="079f1-260">Bu, bir `struct` `this` escapability 'nin arabirimler hakkında olmasının nedendir.</span><span class="sxs-lookup"><span data-stu-id="079f1-260">This is why the escapability of `this` in a `struct` is really all about interfaces.</span></span> <span data-ttu-id="079f1-261">Bu, kesinlikle çalışmaya yapılabilir ancak bir ticareti vardır.</span><span class="sxs-lookup"><span data-stu-id="079f1-261">It can absolutely be made to work but it has a trade off.</span></span> <span data-ttu-id="079f1-262">Tasarım sonunda, arabirimleri daha esnek hale getirmek için bir süre aşağı doğru bir şekilde geldi.</span><span class="sxs-lookup"><span data-stu-id="079f1-262">The design eventually came down in favor of making interfaces more flexible.</span></span> 

<span data-ttu-id="079f1-263">Bunu gelecekte rahat hale getirmemizi mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="079f1-263">There is potential for us to relax this in the future though.</span></span> 

## <a name="future-considerations"></a><span data-ttu-id="079f1-264">Gelecekteki konular</span><span class="sxs-lookup"><span data-stu-id="079f1-264">Future Considerations</span></span>

### <a name="length-one-spant-over-ref-values"></a><span data-ttu-id="079f1-265">Ref değerleri üzerinde\<T > bir span uzunluğu</span><span class="sxs-lookup"><span data-stu-id="079f1-265">Length one Span\<T> over ref values</span></span>
<span data-ttu-id="079f1-266">Artık geçerli olmasa da, bir değer üzerinde bir `Span<T>` örneği oluşturmak yararlı olabilir:</span><span class="sxs-lookup"><span data-stu-id="079f1-266">Though not legal today there are cases where creating a length one `Span<T>` instance over a value would be beneficial:</span></span>

```csharp
void RefExample()
{
    int x = ...;

    // Today creating a length one Span<int> requires a stackalloc and a new 
    // local
    Span<int> span1 = stackalloc [] { x };
    Use(span1);
    x = span1[0]; 

    // Simpler to just allow length one span
    var span2 = new Span<int>(ref x);
    Use(span2);
}
```

<span data-ttu-id="079f1-267">Bu özellik, daha fazla uzunluğu `Span<T>` örneklerine izin vermekle, [sabit boyutlu arabelleklere](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md) yönelik kısıtlamaları kaldırırsam daha ilgi çekici bir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="079f1-267">This feature gets more compelling if we lift the restrictions on [fixed sized buffers](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md) as it would allow for `Span<T>` instances of even greater length.</span></span> 

<span data-ttu-id="079f1-268">Bu yol için bir sorun oluşursa, dil bu `Span<T>` tür örneklerin yalnızca bir yandan aşağı doğru olduğundan emin olarak buna uyum sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="079f1-268">If there is ever a need to go down this path then the language could accommodate this by ensuring such `Span<T>` instances were downward facing only.</span></span> <span data-ttu-id="079f1-269">Bu, yalnızca oluşturuldukları kapsama *güvenli bir şekilde kaçış* amaçlıdır.</span><span class="sxs-lookup"><span data-stu-id="079f1-269">That is they were only ever *safe-to-escape* to the scope in which they were created.</span></span> <span data-ttu-id="079f1-270">Bu, dilin bir `ref` değerini bir `ref struct``ref struct` dönüşü veya alanı aracılığıyla kaçışmak zorunda olmamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="079f1-270">This ensure the language never had to consider a `ref` value escaping a method via a `ref struct` return or field of `ref struct`.</span></span> <span data-ttu-id="079f1-271">Bunun yanı sıra, bu tür oluşturucuların bu şekilde `ref` bir parametre yakaladığı şekilde tanınması için de daha fazla değişiklik yapılmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="079f1-271">This would likely also require further changes to recognize such constructors as capturing a `ref` parameter in this way though.</span></span>
