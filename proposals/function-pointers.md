---
ms.openlocfilehash: 8bf3a18dc42e225e64bd3ccda2106aed89b421ed
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108395"
---
# <a name="function-pointers"></a><span data-ttu-id="0344a-101">İşlev İşaretçileri</span><span class="sxs-lookup"><span data-stu-id="0344a-101">Function Pointers</span></span>

## <a name="summary"></a><span data-ttu-id="0344a-102">Özet</span><span class="sxs-lookup"><span data-stu-id="0344a-102">Summary</span></span>

<span data-ttu-id="0344a-103">Bu teklif, şu anda etkin şekilde erişilemeyen Il işlem kodları 'ları, bugün içinde C# `ldftn` ve `calli`.</span><span class="sxs-lookup"><span data-stu-id="0344a-103">This proposal provides language constructs that expose IL opcodes that cannot currently be accessed efficiently, or at all, in C# today: `ldftn` and `calli`.</span></span> <span data-ttu-id="0344a-104">Bu Il Opcode 'ları yüksek performans kodunda önemli olabilir ve geliştiricilerin bunlara erişmek için etkili bir yol olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="0344a-104">These IL opcodes can be important in high performance code and developers need an efficient way to access them.</span></span>

## <a name="motivation"></a><span data-ttu-id="0344a-105">Amacı</span><span class="sxs-lookup"><span data-stu-id="0344a-105">Motivation</span></span>

<span data-ttu-id="0344a-106">Bu özelliğin ilgi çekici ve arka planı, aşağıdaki konuda açıklanmaktadır (özelliğin olası bir uygulamasıdır):</span><span class="sxs-lookup"><span data-stu-id="0344a-106">The motivations and background for this feature are described in the following issue (as is a potential implementation of the feature):</span></span>

https://github.com/dotnet/csharplang/issues/191

<span data-ttu-id="0344a-107">Bu, [derleyici iç](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md) bilgileri için alternatif bir tasarım teklifinde bulunur</span><span class="sxs-lookup"><span data-stu-id="0344a-107">This is an alternate design proposal to [compiler intrinsics](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)</span></span>

## <a name="detailed-design"></a><span data-ttu-id="0344a-108">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="0344a-108">Detailed Design</span></span>

### <a name="function-pointers"></a><span data-ttu-id="0344a-109">İşlev işaretçileri</span><span class="sxs-lookup"><span data-stu-id="0344a-109">Function pointers</span></span>

<span data-ttu-id="0344a-110">Dil, `delegate*` sözdizimini kullanarak işlev işaretçilerinin bildirimine izin verir.</span><span class="sxs-lookup"><span data-stu-id="0344a-110">The language will allow for the declaration of function pointers using the `delegate*` syntax.</span></span> <span data-ttu-id="0344a-111">Tam sözdizimi, sonraki bölümde ayrıntılı olarak açıklanmıştır, ancak `Func` ve `Action` tür bildirimleri tarafından kullanılan sözdizimine benzer şekilde tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="0344a-111">The full syntax is described in detail in the next section but it is meant to resemble the syntax used by `Func` and `Action` type declarations.</span></span>

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

<span data-ttu-id="0344a-112">Bu türler, ECMA-335 ' de özetlenen işlev işaretçisi türü kullanılarak temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="0344a-112">These types are represented using the function pointer type as outlined in ECMA-335.</span></span> <span data-ttu-id="0344a-113">Bu, bir `delegate*` çağrısı, bir `delegate` çağırmanın `Invoke` yönteminde `callvirt` kullanacağı `calli` kullanacağı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="0344a-113">This means invocation of a `delegate*` will use `calli` where invocation of a `delegate` will use `callvirt` on the `Invoke` method.</span></span>
<span data-ttu-id="0344a-114">Sözdizimi, her iki yapı için de özdeş.</span><span class="sxs-lookup"><span data-stu-id="0344a-114">Syntactically though invocation is identical for both constructs.</span></span>

<span data-ttu-id="0344a-115">Yöntem işaretçilerinin ECMA-335 tanımı, tür imzasının bir parçası olarak çağırma kuralını içerir (Bölüm 7,1).</span><span class="sxs-lookup"><span data-stu-id="0344a-115">The ECMA-335 definition of method pointers includes the calling convention as part of the type signature (section 7.1).</span></span>
<span data-ttu-id="0344a-116">Varsayılan çağırma kuralı `managed`olacaktır.</span><span class="sxs-lookup"><span data-stu-id="0344a-116">The default calling convention will be `managed`.</span></span> <span data-ttu-id="0344a-117">`delegate*` sözdiziminden sonra uygun değiştirici eklenerek alternatif formlar belirtilebilir: `managed`, `cdecl`, `stdcall`, `thiscall`veya `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="0344a-117">Alternate forms can be specified by adding the appropriate modifier after the `delegate*` syntax: `managed`, `cdecl`, `stdcall`, `thiscall`, or `unmanaged`.</span></span> <span data-ttu-id="0344a-118">Örnek:</span><span class="sxs-lookup"><span data-stu-id="0344a-118">Example:</span></span>

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

<span data-ttu-id="0344a-119">`delegate*` türler arasındaki dönüştürmeler, çağırma kuralı dahil olmak üzere imzalarına göre yapılır.</span><span class="sxs-lookup"><span data-stu-id="0344a-119">Conversions between `delegate*` types is done based on their signature including the calling convention.</span></span>

``` csharp
unsafe class Example {
    void Conversions() {
        delegate*<int, int, int> p1 = ...;
        delegate* managed<int, int, int> p2 = ...;
        delegate* cdecl<int, int, int> p3 = ...;

        p1 = p2; // okay p1 and p2 have compatible signatures
        Console.WriteLine(p2 == p1); // True
        p2 = p3; // error: calling conventions are incompatible
    }
}
```

<span data-ttu-id="0344a-120">`delegate*` türü, standart bir işaretçi türünün tüm özelliklerine ve kısıtlamalarına sahip olduğu anlamına gelen bir işaretçi türüdür:</span><span class="sxs-lookup"><span data-stu-id="0344a-120">A `delegate*` type is a pointer type which means it has all of the capabilities and restrictions of a standard pointer type:</span></span>

- <span data-ttu-id="0344a-121">Yalnızca bir `unsafe` bağlamında geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="0344a-121">Only valid in an `unsafe` context.</span></span>
- <span data-ttu-id="0344a-122">Bir `delegate*` parametresi veya dönüş türü içeren yöntemler yalnızca bir `unsafe` bağlamından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="0344a-122">Methods which contain a `delegate*` parameter or return type can only be called from an `unsafe` context.</span></span>
- <span data-ttu-id="0344a-123">`object`dönüştürülemiyor.</span><span class="sxs-lookup"><span data-stu-id="0344a-123">Cannot be converted to `object`.</span></span>
- <span data-ttu-id="0344a-124">Genel bağımsız değişken olarak kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="0344a-124">Cannot be used as a generic argument.</span></span>
- <span data-ttu-id="0344a-125">`delegate*` `void*`örtülü olarak dönüştürebilir.</span><span class="sxs-lookup"><span data-stu-id="0344a-125">Can implicitly convert `delegate*` to `void*`.</span></span>
- <span data-ttu-id="0344a-126">`void*`, açıkça `delegate*`olarak dönüştürebilir.</span><span class="sxs-lookup"><span data-stu-id="0344a-126">Can explicitly convert from `void*` to `delegate*`.</span></span>

<span data-ttu-id="0344a-127">Larından</span><span class="sxs-lookup"><span data-stu-id="0344a-127">Restrictions:</span></span>

- <span data-ttu-id="0344a-128">Özel öznitelikler bir `delegate*` veya öğelerinden hiçbirine uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="0344a-128">Custom attributes cannot be applied to a `delegate*` or any of its elements.</span></span>
- <span data-ttu-id="0344a-129">Bir `delegate*` parametresi `params` olarak işaretlenemez</span><span class="sxs-lookup"><span data-stu-id="0344a-129">A `delegate*` parameter cannot be marked as `params`</span></span>
- <span data-ttu-id="0344a-130">Bir `delegate*` türü normal bir işaretçi türünün tüm kısıtlamalarına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="0344a-130">A `delegate*` type has all of the restrictions of a normal pointer type.</span></span>

### <a name="function-pointer-syntax"></a><span data-ttu-id="0344a-131">İşlev işaretçisi sözdizimi</span><span class="sxs-lookup"><span data-stu-id="0344a-131">Function pointer syntax</span></span>

<span data-ttu-id="0344a-132">Tam işlev işaretçisi sözdizimi aşağıdaki dilbilgisinde temsil edilir:</span><span class="sxs-lookup"><span data-stu-id="0344a-132">The full function pointer syntax is represented by the following grammar:</span></span>

```antlr
pointer_type
    : ...
    | funcptr_type
    ;

funcptr_type
    : 'delegate' '*' calling_convention? '<' (funcptr_parameter_modifier? type ',')* funcptr_return_modifier? return_type '>'
    ;

calling_convention
    : 'cdecl'
    | 'managed'
    | 'stdcall'
    | 'thiscall'
    | 'unmanaged'
    ;

funcptr_parameter_modifier
    : 'ref'
    | 'out'
    | 'in'
    ;

funcptr_return_modifier
    : 'ref'
    | 'ref readonly'
    ;
```

<span data-ttu-id="0344a-133">`unmanaged` çağırma kuralı, geçerli platformda yerel kod için varsayılan çağırma kuralını temsil eder ve WinAPI olarak kodlanır.</span><span class="sxs-lookup"><span data-stu-id="0344a-133">The `unmanaged` calling convention represents the default calling convention for native code on the current platform, and is encoded as winapi.</span></span>
<span data-ttu-id="0344a-134">Tüm `calling_convention`tüm `delegate*`öncesinde bağlamsal anahtar kelimelerdir.</span><span class="sxs-lookup"><span data-stu-id="0344a-134">All `calling_convention`s are contextual keywords when preceded by a `delegate*`.</span></span>

``` csharp
delegate int Func1(string s);
delegate Func1 Func2(Func1 f);

// Function pointer equivalent without calling convention
delegate*<string, int>;
delegate*<delegate*<string, int>, delegate*<string, int>>;

// Function pointer equivalent with calling convention
delegate* managed<string, int>;
delegate*<delegate* managed<string, int>, delegate*<string, int>>;
```

### <a name="function-pointer-conversions"></a><span data-ttu-id="0344a-135">İşlev işaretçisi dönüştürmeleri</span><span class="sxs-lookup"><span data-stu-id="0344a-135">Function pointer conversions</span></span>

<span data-ttu-id="0344a-136">Güvenli olmayan bir bağlamda, kullanılabilir örtük dönüştürmeler (örtük dönüştürmeler) kümesi aşağıdaki örtük işaretçi dönüşümlerini içerecek şekilde genişletilir:</span><span class="sxs-lookup"><span data-stu-id="0344a-136">In an unsafe context, the set of available implicit conversions (Implicit conversions) is extended to include the following implicit pointer conversions:</span></span>
- [<span data-ttu-id="0344a-137">_Mevcut dönüşümler_</span><span class="sxs-lookup"><span data-stu-id="0344a-137">_Existing conversions_</span></span>](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- <span data-ttu-id="0344a-138">_Funcptr\_türü_ `F0` başka bir _funcptr\_türü_ `F1`, aşağıdakilerin tümü doğru olarak sağlandıysa:</span><span class="sxs-lookup"><span data-stu-id="0344a-138">From _funcptr\_type_ `F0` to another _funcptr\_type_ `F1`, provided all of the following are true:</span></span>
    - <span data-ttu-id="0344a-139">`F0` ve `F1` aynı sayıda parametreye sahiptir ve `F0` `D0n` her bir parametre `ref`karşılık gelen parametre `out`aynı `in`, `D1n` veya `F1`değiştiricilerine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="0344a-139">`F0` and `F1` have the same number of parameters, and each parameter `D0n` in `F0` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter `D1n` in `F1`.</span></span>
    - <span data-ttu-id="0344a-140">Her bir değer parametresi için (`ref`, `out`veya `in` değiştiricisi olmayan bir parametre), `F0` içindeki parametre türünden `F1`öğesine karşılık gelen parametre türüne bir kimlik dönüştürme, örtük başvuru dönüştürme veya örtük işaretçi dönüştürme bulunur.</span><span class="sxs-lookup"><span data-stu-id="0344a-140">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `F0` to the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="0344a-141">Her `ref`, `out`veya `in` parametresi için `F0` parametre türü, `F1`karşılık gelen parametre türü ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="0344a-141">For each `ref`, `out`, or `in` parameter, the parameter type in `F0` is the same as the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="0344a-142">Dönüş türü değer (Hayır `ref` veya `ref readonly`) ise, `F1` dönüş türünden `F0`dönüş türüne bir kimlik, örtük başvuru veya örtük işaretçi dönüştürme bulunur.</span><span class="sxs-lookup"><span data-stu-id="0344a-142">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F1` to the return type of `F0`.</span></span>
    - <span data-ttu-id="0344a-143">Dönüş türü başvuruya (`ref` veya `ref readonly`), dönüş türü ve `F1` `ref` değiştiricileri, `ref` dönüş türü ve `F0`değiştiricilerine benzer.</span><span class="sxs-lookup"><span data-stu-id="0344a-143">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F1` are the same as the return type and `ref` modifiers of `F0`.</span></span>
    - <span data-ttu-id="0344a-144">`F0` çağırma kuralı, `F1`çağırma kuralıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="0344a-144">The calling convention of `F0` is the same as the calling convention of `F1`.</span></span>

### <a name="allow-address-of-to-target-methods"></a><span data-ttu-id="0344a-145">Hedef yöntemlere yönelik adrese izin ver</span><span class="sxs-lookup"><span data-stu-id="0344a-145">Allow address-of to target methods</span></span>

<span data-ttu-id="0344a-146">Yöntem gruplarına artık bir adres ifadesi için bağımsız değişken olarak izin verilir.</span><span class="sxs-lookup"><span data-stu-id="0344a-146">Method groups will now be allowed as arguments to an address-of expression.</span></span> <span data-ttu-id="0344a-147">Böyle bir ifadenin türü, hedef yöntemin ve yönetilen çağırma kuralının denk imzasına sahip bir `delegate*` olacaktır:</span><span class="sxs-lookup"><span data-stu-id="0344a-147">The type of such an expression will be a `delegate*` which has the equivalent signature of the target method and a managed calling convention:</span></span>

``` csharp
unsafe class Util {
    public static void Log() { }

    void Use() {
        delegate*<void> ptr1 = &Util.Log;

        // Error: type "delegate*<void>" not compatible with "delegate*<int>";
        delegate*<int> ptr2 = &Util.Log;

        // Okay. Conversion to void* is always allowed.
        void* v = &Util.Log;
   }
}
```

<span data-ttu-id="0344a-148">Güvenli olmayan bir bağlamda, bir yöntem `M`, aşağıdakilerin tümü doğru ise bir işlev işaretçisi `F` türü ile uyumludur:</span><span class="sxs-lookup"><span data-stu-id="0344a-148">In an unsafe context, a method `M` is compatible with a function pointer type `F` if all of the following are true:</span></span>
- <span data-ttu-id="0344a-149">`M` ve `F` aynı sayıda parametreye sahiptir ve `D` her bir parametre `out`karşılık gelen parametreyle aynı `ref`, `in` veya `F`değiştiricilerine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="0344a-149">`M` and `F` have the same number of parameters, and each parameter in `D` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter in `F`.</span></span>
- <span data-ttu-id="0344a-150">Her bir değer parametresi için (`ref`, `out`veya `in` değiştiricisi olmayan bir parametre), `M` içindeki parametre türünden `F`öğesine karşılık gelen parametre türüne bir kimlik dönüştürme, örtük başvuru dönüştürme veya örtük işaretçi dönüştürme bulunur.</span><span class="sxs-lookup"><span data-stu-id="0344a-150">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `M` to the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="0344a-151">Her `ref`, `out`veya `in` parametresi için `M` parametre türü, `F`karşılık gelen parametre türü ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="0344a-151">For each `ref`, `out`, or `in` parameter, the parameter type in `M` is the same as the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="0344a-152">Dönüş türü değer (Hayır `ref` veya `ref readonly`) ise, `F` dönüş türünden `M`dönüş türüne bir kimlik, örtük başvuru veya örtük işaretçi dönüştürme bulunur.</span><span class="sxs-lookup"><span data-stu-id="0344a-152">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F` to the return type of `M`.</span></span>
- <span data-ttu-id="0344a-153">Dönüş türü başvuruya (`ref` veya `ref readonly`), dönüş türü ve `F` `ref` değiştiricileri, `ref` dönüş türü ve `M`değiştiricilerine benzer.</span><span class="sxs-lookup"><span data-stu-id="0344a-153">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F` are the same as the return type and `ref` modifiers of `M`.</span></span>
- <span data-ttu-id="0344a-154">`M` çağırma kuralı, `F`çağırma kuralıyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="0344a-154">The calling convention of `M` is the same as the calling convention of `F`.</span></span>
- <span data-ttu-id="0344a-155">`M` statik bir yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="0344a-155">`M` is a static method.</span></span>

<span data-ttu-id="0344a-156">Güvenli olmayan bir bağlamda, hedefi bir yöntem grubu olan bir adres ifadeden (örneğin, hedefi bir yöntem grubu) `E` uyumlu bir işlev işaretçisi türünde bir örtük dönüştürme bulunur `F`, `E`, aşağıdaki konuda açıklandığı gibi, parametre türleri ve `F`değiştiriciler kullanılarak oluşturulan bir bağımsız değişken listesine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="0344a-156">In an unsafe context, an implicit conversion exists from an address-of expression whose target is a method group `E` to a compatible function pointer type `F` if `E` contains at least one method that is applicable in its normal form to an argument list constructed by use of the parameter types and modifiers of `F`, as described in the following.</span></span>
- <span data-ttu-id="0344a-157">Aşağıdaki değişikliklerle form `E(A)` bir yöntem çağrısına karşılık gelen tek bir yöntem `M` seçilir:</span><span class="sxs-lookup"><span data-stu-id="0344a-157">A single method `M` is selected corresponding to a method invocation of the form `E(A)` with the following modifications:</span></span>
   - <span data-ttu-id="0344a-158">Bağımsız değişkenler listesi `A`, her biri bir değişken olarak sınıflandırılan ve karşılık gelen _biçimsel\_parametre\_listesinin_ türü ve değiştiricisi (`ref`, `out`veya `in`) olan ifadelerin bir listesidir.`D`</span><span class="sxs-lookup"><span data-stu-id="0344a-158">The arguments list `A` is a list of expressions, each classified as a variable and with the type and modifier (`ref`, `out`, or `in`) of the corresponding _formal\_parameter\_list_ of `D`.</span></span>
   - <span data-ttu-id="0344a-159">Aday Yöntemler, genişletilmiş biçiminde uygulananlar değil, yalnızca normal biçimlerinde geçerli olan yöntemlerdir.</span><span class="sxs-lookup"><span data-stu-id="0344a-159">The candidate methods are only those methods that are applicable in their normal form, not those applicable in their expanded form.</span></span>
   - <span data-ttu-id="0344a-160">Aday yöntemler yalnızca statik olan yöntemlerdir.</span><span class="sxs-lookup"><span data-stu-id="0344a-160">The candidate methods are only those methods that are static.</span></span>
- <span data-ttu-id="0344a-161">Yöntem etkinleştirmeleri algoritması bir hata üretirse, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="0344a-161">If the algorithm of Method invocations produces an error, then a compile-time error occurs.</span></span> <span data-ttu-id="0344a-162">Aksi takdirde, algoritma `F` aynı sayıda parametreye sahip `M` tek bir en iyi yöntem üretir ve dönüştürme var olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="0344a-162">Otherwise, the algorithm produces a single best method `M` having the same number of parameters as `F` and the conversion is considered to exist.</span></span>
- <span data-ttu-id="0344a-163">Seçilen yöntem `M`, işlev işaretçisi türü `F`uyumlu olmalıdır (yukarıda tanımlandığı şekilde).</span><span class="sxs-lookup"><span data-stu-id="0344a-163">The selected method `M` must be compatible (as defined above) with the function pointer type `F`.</span></span> <span data-ttu-id="0344a-164">Aksi takdirde, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="0344a-164">Otherwise, a compile-time error occurs.</span></span>
- <span data-ttu-id="0344a-165">Dönüştürmenin sonucu `F`türünde bir işlev işaretçisidir.</span><span class="sxs-lookup"><span data-stu-id="0344a-165">The result of the conversion is a function pointer of type `F`.</span></span>

<span data-ttu-id="0344a-166">`E`içinde yalnızca bir statik yöntem `M` `void*` için, hedefi bir yöntem `E` grubu olan bir adres ifadesinde örtük bir dönüştürme bulunur.</span><span class="sxs-lookup"><span data-stu-id="0344a-166">An implicit conversion exists from an address-of expression whose target is a method group `E` to `void*` if there is only one static method `M` in `E`.</span></span>
<span data-ttu-id="0344a-167">Tek bir statik yöntem varsa, `E` en iyi yöntem `M`' dir.</span><span class="sxs-lookup"><span data-stu-id="0344a-167">If there is one static method, then the single best method from `E` is `M`.</span></span>
<span data-ttu-id="0344a-168">Aksi takdirde, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="0344a-168">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="0344a-169">Bu, geliştiricilerin adres operatörü ile birlikte çalışmak için aşırı yükleme çözümleme kurallarına bağlı olabileceği anlamına gelir:</span><span class="sxs-lookup"><span data-stu-id="0344a-169">This means developers can depend on overload resolution rules to work in conjunction with the address-of operator:</span></span>

``` csharp
unsafe class Util {
    public static void Log() { }
    public static void Log(string p1) { }
    public static void Log(int i) { };

    void Use() {
        delegate*<void> a1 = &Log; // Log()
        delegate*<int, void> a2 = &Log; // Log(int i)

        // Error: ambiguous conversion from method group Log to "void*"
        void* v = &Log;
    }
```

<span data-ttu-id="0344a-170">Address-of işleci `ldftn` yönergesi kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="0344a-170">The address-of operator will be implemented using the `ldftn` instruction.</span></span>

<span data-ttu-id="0344a-171">Bu özelliğin kısıtlamaları:</span><span class="sxs-lookup"><span data-stu-id="0344a-171">Restrictions of this feature:</span></span>

- <span data-ttu-id="0344a-172">Yalnızca `static`olarak işaretlenen yöntemler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="0344a-172">Only applies to methods marked as `static`.</span></span>
- <span data-ttu-id="0344a-173">`static` olmayan yerel işlevler `&`kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="0344a-173">Non-`static` local functions cannot be used in `&`.</span></span> <span data-ttu-id="0344a-174">Bu yöntemlerin uygulama ayrıntıları, dil tarafından kasıtlı olarak belirtilmez.</span><span class="sxs-lookup"><span data-stu-id="0344a-174">The implementation details of these methods are deliberately not specified by the language.</span></span> <span data-ttu-id="0344a-175">Bu, statik ve örnek veya tam olarak hangi imzayı yayındıklarınızı içerir.</span><span class="sxs-lookup"><span data-stu-id="0344a-175">This includes whether they are static vs. instance or exactly what signature they are emitted with.</span></span>

### <a name="better-function-member"></a><span data-ttu-id="0344a-176">Daha iyi işlev üyesi</span><span class="sxs-lookup"><span data-stu-id="0344a-176">Better function member</span></span>

<span data-ttu-id="0344a-177">Daha iyi işlev üyesi belirtimi aşağıdaki satırı içerecek şekilde değiştirilecek:</span><span class="sxs-lookup"><span data-stu-id="0344a-177">The better function member specification will be changed to include the following line:</span></span>

> <span data-ttu-id="0344a-178">`delegate*`, `void*` daha belirgin</span><span class="sxs-lookup"><span data-stu-id="0344a-178">A `delegate*` is more specific than `void*`</span></span>

<span data-ttu-id="0344a-179">Diğer bir deyişle, `void*` ve bir `delegate*` aşırı yükleme yapılabilir ve yine de Address-of işleci kullanılır sensibly.</span><span class="sxs-lookup"><span data-stu-id="0344a-179">This means that it is possible to overload on `void*` and a `delegate*` and still sensibly use the address-of operator.</span></span>

## <a name="open-issues"></a><span data-ttu-id="0344a-180">Açık sorunlar</span><span class="sxs-lookup"><span data-stu-id="0344a-180">Open Issues</span></span>

### <a name="nativecallableattribute"></a><span data-ttu-id="0344a-181">NativeCallableAttribute</span><span class="sxs-lookup"><span data-stu-id="0344a-181">NativeCallableAttribute</span></span>

<span data-ttu-id="0344a-182">Bu, çağrılırken yerel olarak yönetilen bir prolog 'dan kaçınmak için CLR tarafından kullanılan bir özniteliktir.</span><span class="sxs-lookup"><span data-stu-id="0344a-182">This is an attribute used by the CLR to avoid the managed to native prologue when invoking.</span></span> <span data-ttu-id="0344a-183">Bu öznitelik tarafından işaretlenen Yöntemler yönetilmeyen olarak yalnızca yerel koddan çağrılabilir (metotlar çağrılamaz, bir temsilci oluşturun vs...).</span><span class="sxs-lookup"><span data-stu-id="0344a-183">Methods marked by this attribute are only callable from native code, not managed (can’t call methods, create a delegate, etc …).</span></span> <span data-ttu-id="0344a-184">Özniteliği mscorlib için özel değildir; çalışma zamanı, bu ada sahip herhangi bir özniteliği aynı semantiğe işleyecek.</span><span class="sxs-lookup"><span data-stu-id="0344a-184">The attribute is not special to mscorlib; the runtime will treat any attribute with this name with the same semantics.</span></span>

<span data-ttu-id="0344a-185">Bu, çalışma zamanının ve dilin birlikte çalışarak bunu tam olarak desteklemesi mümkündür.</span><span class="sxs-lookup"><span data-stu-id="0344a-185">It's possible for the runtime and language to work together to fully support this.</span></span> <span data-ttu-id="0344a-186">Dil, `static` üyelerini belirtilen çağırma kuralına sahip `delegate*` olarak `NativeCallable` özniteliğiyle kabul etmek için seçim olabilir.</span><span class="sxs-lookup"><span data-stu-id="0344a-186">The language could choose to treat address-of `static` members with a `NativeCallable` attribute as a `delegate*` with the specified calling convention.</span></span>

``` csharp
unsafe class NativeCallableExample {
    [NativeCallable(CallingConvention.CDecl)]
    static void CloseHandle(IntPtr p) => Marshal.FreeHGlobal(p);

    void Use() {
        delegate*<IntPtr, void> p1 = &CloseHandle; // Error: Invalid calling convention

        delegate* cdecl<IntPtr, void> p2 = &CloseHandle; // Okay
    }
}

```

<span data-ttu-id="0344a-187">Ayrıca, dilin de şunları yapmak isteyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="0344a-187">Additionally the language would likely also want to:</span></span>

- <span data-ttu-id="0344a-188">Bir hata olarak `NativeCallable` etiketli bir yönteme yapılan yönetilen çağrıları bayrakla işaretle.</span><span class="sxs-lookup"><span data-stu-id="0344a-188">Flag any managed calls to a method tagged with `NativeCallable` as an error.</span></span> <span data-ttu-id="0344a-189">Verilen işlev yönetilen koddan çağrılamaz derleyicinin geliştiricilerin bu tür bir çağrı yapmasını engellemesini önlemelidir.</span><span class="sxs-lookup"><span data-stu-id="0344a-189">Given the function can't be invoked from managed code the compiler should prevent developers from attempting such an invocation.</span></span>
- <span data-ttu-id="0344a-190">Yöntem `NativeCallable`ile etiketlenmişse metot grubu dönüştürmelerini `delegate` engelleyin.</span><span class="sxs-lookup"><span data-stu-id="0344a-190">Prevent method group conversions to `delegate` when the method is tagged with `NativeCallable`.</span></span>

<span data-ttu-id="0344a-191">Bu da `NativeCallable` desteklemek için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="0344a-191">This is not necessary to support `NativeCallable` though.</span></span> <span data-ttu-id="0344a-192">Derleyici, var olan söz dizimini kullanarak `NativeCallable` özniteliğini destekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="0344a-192">The compiler can support the `NativeCallable` attribute as is using the existing syntax.</span></span> <span data-ttu-id="0344a-193">Programın doğru `delegate*` imzasına atama yapmadan önce `void*` 'e dönüştürülmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="0344a-193">The program would simply need to cast to `void*` before casting to the correct `delegate*` signature.</span></span> <span data-ttu-id="0344a-194">Bu, günümüzde destek 'ten daha kötü olmayacak.</span><span class="sxs-lookup"><span data-stu-id="0344a-194">That would be no worse than the support today.</span></span>

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a><span data-ttu-id="0344a-195">Genişletilebilir yönetilmeyen çağırma kuralları kümesi</span><span class="sxs-lookup"><span data-stu-id="0344a-195">Extensible set of unmanaged calling conventions</span></span>

<span data-ttu-id="0344a-196">Geçerli ECMA-335 kodlamaları tarafından desteklenen yönetilmeyen çağırma kuralları kümesi güncel değildir.</span><span class="sxs-lookup"><span data-stu-id="0344a-196">The set of unmanaged calling conventions supported by the current ECMA-335 encodings is outdated.</span></span> <span data-ttu-id="0344a-197">Daha yönetilmeyen çağrı kuralları için destek ekleme isteklerini gördük, örneğin:</span><span class="sxs-lookup"><span data-stu-id="0344a-197">We have seen requests to add support for more unmanaged calling conventions, for example:</span></span>

- <span data-ttu-id="0344a-198">[vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120</span><span class="sxs-lookup"><span data-stu-id="0344a-198">[vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120</span></span>
- <span data-ttu-id="0344a-199">Açık bu https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750 ile StdCall</span><span class="sxs-lookup"><span data-stu-id="0344a-199">StdCall with explicit this https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750</span></span>

<span data-ttu-id="0344a-200">Bu özelliğin tasarımı, daha sonra gerektiğinde, yönetilmeyen çağırma kuralları kümesinin genişlemesiyle izin verilmelidir.</span><span class="sxs-lookup"><span data-stu-id="0344a-200">The design of this feature should allow extending the set of unmanaged calling conventions as needed in future.</span></span> <span data-ttu-id="0344a-201">Sorunlar, kodlama kurallarını kodlamak için sınırlı alan içerir (`IMAGE_CEE_CS_CALLCONV_MASK`16 değerden 12 ' de alınır) ve yeni bir çağırma kuralı eklemek için dokunulmaları gereken yerlerin sayısı bulunur.</span><span class="sxs-lookup"><span data-stu-id="0344a-201">The problems include limited space for encoding calling conventions (12 out of 16 values are taken in `IMAGE_CEE_CS_CALLCONV_MASK`) and number of places that need to be touched in order to add a new calling convention.</span></span> <span data-ttu-id="0344a-202">Olası bir çözüm, [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) enum kullanarak çağırma kuralını temsil eden yeni bir kodlama tanıtmaktadır.</span><span class="sxs-lookup"><span data-stu-id="0344a-202">A potential solution is to introduce a new encoding that represents the calling convention using [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) enum.</span></span>

<span data-ttu-id="0344a-203">Başvuru için, https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h LLVM tarafından desteklenen çağırma kurallarının listesini içerir.</span><span class="sxs-lookup"><span data-stu-id="0344a-203">For reference, https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h has the list of calling conventions supported by LLVM.</span></span> <span data-ttu-id="0344a-204">.NET, tüm bunları desteklemeye gerek duymadan, çağrı kuralları alanının çok daha zengin olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="0344a-204">While it is unlikely that .NET will ever need to support all of them, it demonstrates that the space of calling conventions is very rich.</span></span>

## <a name="considerations"></a><span data-ttu-id="0344a-205">Dikkat Edilecekler</span><span class="sxs-lookup"><span data-stu-id="0344a-205">Considerations</span></span>

### <a name="allow-instance-methods"></a><span data-ttu-id="0344a-206">Örnek yöntemlerine izin ver</span><span class="sxs-lookup"><span data-stu-id="0344a-206">Allow instance methods</span></span>

<span data-ttu-id="0344a-207">Teklif, `EXPLICITTHIS` CLı çağırma kuralı (kodda `instance` olarak C# adlandırılır) avantajlarından yararlanarak örnek yöntemlerini destekleyecek şekilde genişletilebilir.</span><span class="sxs-lookup"><span data-stu-id="0344a-207">The proposal could be extended to support instance methods by taking advantage of the `EXPLICITTHIS` CLI calling convention (named `instance` in C# code).</span></span> <span data-ttu-id="0344a-208">CLı işlev işaretçilerinden oluşan bu form `this` parametresini, işlev işaretçisi sözdiziminin açık bir ilk parametresi olarak koyar.</span><span class="sxs-lookup"><span data-stu-id="0344a-208">This form of CLI function pointers puts the `this` parameter as an explicit first parameter of the function pointer syntax.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

<span data-ttu-id="0344a-209">Bu ses, ancak teklife bazı karmaşıkma ekler.</span><span class="sxs-lookup"><span data-stu-id="0344a-209">This is sound but adds some complication to the proposal.</span></span> <span data-ttu-id="0344a-210">Özellikle, çağırma kuralına göre farklı olan işlev işaretçileri `instance` ve `managed` aynı C# imzaya sahip yönetilen yöntemleri çağırmak için her iki durumda da kullanılmasa bile uyumsuz olacak.</span><span class="sxs-lookup"><span data-stu-id="0344a-210">Particularly because function pointers which differed by the calling convention `instance` and `managed` would be incompatible even though both cases are used to invoke managed methods with the same C# signature.</span></span> <span data-ttu-id="0344a-211">Her durumda, bu durumun basit bir iş olması için değerli olduğu kabul edilir: `static` yerel bir işlev kullanın.</span><span class="sxs-lookup"><span data-stu-id="0344a-211">Also in every case considered where this would be valuable to have there was a simple work around: use a `static` local function.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a><span data-ttu-id="0344a-212">Bildirimde güvenli olmayan iste</span><span class="sxs-lookup"><span data-stu-id="0344a-212">Don't require unsafe at declaration</span></span>

<span data-ttu-id="0344a-213">`delegate*`her kullanımda `unsafe` gerektirmek yerine, yalnızca bir yöntem grubunun `delegate*`dönüştürüldüğü noktada gereklidir.</span><span class="sxs-lookup"><span data-stu-id="0344a-213">Instead of requiring `unsafe` at every use of a `delegate*`, only require it at the point where a method group is converted to a `delegate*`.</span></span> <span data-ttu-id="0344a-214">Bu, temel güvenlik sorunlarının oynatılmasına (değer etkin olduğu sürece kapsayan derlemenin kaldırılamayacağını bilmek) neden olur.</span><span class="sxs-lookup"><span data-stu-id="0344a-214">This is where the core safety issues come into play (knowing that the containing assembly cannot be unloaded while the value is alive).</span></span> <span data-ttu-id="0344a-215">Diğer konumlarda `unsafe` gerektirme, aşırı olarak görülebilir.</span><span class="sxs-lookup"><span data-stu-id="0344a-215">Requiring `unsafe` on the other locations can be seen as excessive.</span></span>

<span data-ttu-id="0344a-216">Tasarımın ilk amacı budur.</span><span class="sxs-lookup"><span data-stu-id="0344a-216">This is how the design was originally intended.</span></span> <span data-ttu-id="0344a-217">Ancak elde edilen dil kuralları, çok daha fazla.</span><span class="sxs-lookup"><span data-stu-id="0344a-217">But the resulting language rules felt very awkward.</span></span> <span data-ttu-id="0344a-218">Bu bir işaretçi değeri olduğu ve `unsafe` anahtar sözcüğü olmadan bile göz atma işlemini gizleyebilmek olanaksızdır.</span><span class="sxs-lookup"><span data-stu-id="0344a-218">It's impossible to hide the fact that this is a pointer value and it kept peeking through even without the `unsafe` keyword.</span></span> <span data-ttu-id="0344a-219">Örneğin `object` dönüştürmeye izin verilmiyorsa, bir `class`üyesi olamaz, vb... C# Tasarım, tüm işaretçinin kullanması için `unsafe` gerektirirken, bu tasarımın bu tasarımı takip eder.</span><span class="sxs-lookup"><span data-stu-id="0344a-219">For example the conversion to `object` can't be allowed, it can't be a member of a `class`, etc ... The C# design is to require `unsafe` for all pointer uses and hence this design follows that.</span></span>

<span data-ttu-id="0344a-220">Geliştiriciler, `delegate*` değerlerinin üzerine, bugün normal işaretçi türleri için yaptıkları gibi bir _güvenli_ sarmalayıcı sunma yeteneğine sahip olmaya devam edecektir.</span><span class="sxs-lookup"><span data-stu-id="0344a-220">Developers will still be capable of presenting a _safe_ wrapper on top of `delegate*` values the same way that they do for normal pointer types today.</span></span> <span data-ttu-id="0344a-221">Göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="0344a-221">Consider:</span></span>

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a><span data-ttu-id="0344a-222">Temsilcileri kullanma</span><span class="sxs-lookup"><span data-stu-id="0344a-222">Using delegates</span></span>

<span data-ttu-id="0344a-223">`delegate*`yeni bir sözdizimi öğesi kullanmak yerine, mevcut `delegate` türlerini, türü takip eden bir `*` ile kullanmanız yeterlidir:</span><span class="sxs-lookup"><span data-stu-id="0344a-223">Instead of using a new syntax element, `delegate*`, simply use existing `delegate` types with a `*` following the type:</span></span>

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

<span data-ttu-id="0344a-224">Çağırma kuralını işleme, bir `CallingConvention` değeri belirten bir özniteliğe sahip `delegate` türlerine açıklama ekleyerek yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="0344a-224">Handling calling convention can be done by annotating the `delegate` types with an attribute that specifies a `CallingConvention` value.</span></span> <span data-ttu-id="0344a-225">Bir özniteliğin olmaması yönetilen çağırma kuralını işaret eder.</span><span class="sxs-lookup"><span data-stu-id="0344a-225">The lack of an attribute would signify the managed calling convention.</span></span>

<span data-ttu-id="0344a-226">Il 'de bunu kodlamada sorun vardır.</span><span class="sxs-lookup"><span data-stu-id="0344a-226">Encoding this in IL is problematic.</span></span> <span data-ttu-id="0344a-227">Temel alınan değerin bir işaretçi olarak temsil edilebilmesi için yine de şunları yapmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="0344a-227">The underlying value needs to be represented as a pointer yet it also must:</span></span>

1. <span data-ttu-id="0344a-228">Farklı işlev işaretçisi türleri olan aşırı yüklemelerin izin vermek için benzersiz bir türü vardır.</span><span class="sxs-lookup"><span data-stu-id="0344a-228">Have a unique type to allow for overloads with different function pointer types.</span></span>
1. <span data-ttu-id="0344a-229">Derleme sınırları genelinde OHI amaçları için eşdeğer olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0344a-229">Be equivalent for OHI purposes across assembly boundaries.</span></span>

<span data-ttu-id="0344a-230">Son nokta özellikle sorunlu.</span><span class="sxs-lookup"><span data-stu-id="0344a-230">The last point is particularly problematic.</span></span> <span data-ttu-id="0344a-231">Bu, `Func<int>*` bir derlemede tanımlansa bile, `Func<int>*` kullanan her derlemenin meta verilerde eşdeğer bir tür kodlanması gerektiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="0344a-231">This mean that every assembly which uses `Func<int>*` must encode an equivalent type in metadata even though `Func<int>*` is defined in an assembly though don't control.</span></span>
<span data-ttu-id="0344a-232">Ayrıca, mscorlib olmayan bir derlemede `System.Func<T>` adı ile tanımlanan diğer herhangi bir tür, mscorlib içinde tanımlanan sürümden farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0344a-232">Additionally any other type which is defined with the name `System.Func<T>` in an assembly that is not mscorlib must be different than the version defined in mscorlib.</span></span>

<span data-ttu-id="0344a-233">Araştırılan bir seçenek, `mod_req(Func<int>) void*`gibi bir işaretçiyi yaymıştı.</span><span class="sxs-lookup"><span data-stu-id="0344a-233">One option that was explored was emitting such a pointer as `mod_req(Func<int>) void*`.</span></span> <span data-ttu-id="0344a-234">`mod_req` bir `TypeSpec` bağlanamaz ve bu nedenle genel örneklemeleri hedeflenemez.</span><span class="sxs-lookup"><span data-stu-id="0344a-234">This doesn't work though as a `mod_req` cannot bind to a `TypeSpec` and hence cannot target generic instantiations.</span></span>

### <a name="named-function-pointers"></a><span data-ttu-id="0344a-235">Adlandırılmış işlev işaretçileri</span><span class="sxs-lookup"><span data-stu-id="0344a-235">Named function pointers</span></span>

<span data-ttu-id="0344a-236">İşlev işaretçisi sözdizimi, özellikle de iç içe geçmiş işlev işaretçileri gibi karmaşık durumlarda, kısabera olabilir.</span><span class="sxs-lookup"><span data-stu-id="0344a-236">The function pointer syntax can be cumbersome, particularly in complex cases like nested function pointers.</span></span> <span data-ttu-id="0344a-237">Geliştiricilerin, `delegate`işlev işaretçilerine yönelik adlandırılmış bildirimler için izin veren her seferinde imzayı yazmak yerine, ile yapılır.</span><span class="sxs-lookup"><span data-stu-id="0344a-237">Rather than have developers type out the signature every time the language could allow for named declarations of function pointers as is done with `delegate`.</span></span>

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

<span data-ttu-id="0344a-238">Buradaki sorunun bir bölümü, temel alınan CLı ilkel adının adı yoktur, bu nedenle yalnızca bir C# hata oluşur ve etkinleştirilmesi için bir veri kümesi çalışması gerekir.</span><span class="sxs-lookup"><span data-stu-id="0344a-238">Part of the problem here is the underlying CLI primitive doesn't have names hence this would be purely a C# invention and require a bit of metadata work to enable.</span></span> <span data-ttu-id="0344a-239">Bu, iş açısından önemli olan, ancak önemli bir çalışmadır.</span><span class="sxs-lookup"><span data-stu-id="0344a-239">That is doable but is a significant about of work.</span></span> <span data-ttu-id="0344a-240">Temelde bu adlar C# için def tablosuna yalnızca bir yardımcı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="0344a-240">It essentially requires C# to have a companion to the type def table purely for these names.</span></span>

<span data-ttu-id="0344a-241">Ayrıca, adlandırılmış işlev işaretçileri için bağımsız değişkenler incelendikleri zaman, diğer birçok senaryoya eşit bir şekilde uygulanamazlar.</span><span class="sxs-lookup"><span data-stu-id="0344a-241">Also when the arguments for named function pointers were examined we found they could apply equally well to a number of other scenarios.</span></span> <span data-ttu-id="0344a-242">Örneğin, tüm durumlarda tam imzayı yazma ihtiyacını azaltmak için adlandırılmış tanımlama gruplarını bildirmek uygun olabilir.</span><span class="sxs-lookup"><span data-stu-id="0344a-242">For example it would be just as convenient to declare named tuples to reduce the need to type out the full signature in all cases.</span></span>

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

<span data-ttu-id="0344a-243">Tartışmadan sonra `delegate*` türlerin adlandırılmış bildirimine izin verme kararı verdik.</span><span class="sxs-lookup"><span data-stu-id="0344a-243">After discussion we decided to not allow named declaration of `delegate*` types.</span></span> <span data-ttu-id="0344a-244">Bu, müşteri kullanımı geri bildirimlerine göre bu için önemli bir gereksinim olduğunu fark etmemiz halinde işlev işaretçileri, tanımlama grupları, genel türler, vb. için uygun bir adlandırma çözümü araştıracağız. Bu, formda tam `typedef` desteği gibi diğer önerilere benzer olması olasıdır.</span><span class="sxs-lookup"><span data-stu-id="0344a-244">If we find there is significant need for this based on customer usage feedback then we will investigate a naming solution that works for function pointers, tuples, generics, etc ... This is likely to be similar in form to other suggestions like full `typedef` support in the language.</span></span>

## <a name="future-considerations"></a><span data-ttu-id="0344a-245">Gelecekteki konular</span><span class="sxs-lookup"><span data-stu-id="0344a-245">Future Considerations</span></span>

### <a name="static-local-functions"></a><span data-ttu-id="0344a-246">Statik yerel işlevler</span><span class="sxs-lookup"><span data-stu-id="0344a-246">static local functions</span></span>

<span data-ttu-id="0344a-247">Bu, yerel işlevlerde `static` değiştiriciye izin veren [öneriyi](https://github.com/dotnet/csharplang/issues/1565) ifade eder.</span><span class="sxs-lookup"><span data-stu-id="0344a-247">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/1565) to allow the `static` modifier on local functions.</span></span> <span data-ttu-id="0344a-248">Bu tür bir işlevin, `static` olarak ve kaynak kodunda tam imzayla belirtilen şekilde yayınlanacağı garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="0344a-248">Such a function would be guaranteed to be emitted as `static` and with the exact signature specified in source code.</span></span> <span data-ttu-id="0344a-249">Bu tür bir işlev, yerel işlevlerin bugün bulunduğu sorunlardan hiçbirini içermediğinden `&` için geçerli bir bağımsız değişken olmalıdır</span><span class="sxs-lookup"><span data-stu-id="0344a-249">Such a function should be a valid argument to `&` as it contains none of the problems local functions have today</span></span>

### <a name="static-delegates"></a><span data-ttu-id="0344a-250">statik temsilciler</span><span class="sxs-lookup"><span data-stu-id="0344a-250">static delegates</span></span>

<span data-ttu-id="0344a-251">Bu, yalnızca `static` üyelere başvurabilen `delegate` türlerinin bildirimine izin veren [öneriyi](https://github.com/dotnet/csharplang/issues/302) ifade eder.</span><span class="sxs-lookup"><span data-stu-id="0344a-251">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/302) to allow for the declaration of `delegate` types which can only refer to `static` members.</span></span> <span data-ttu-id="0344a-252">Bu tür `delegate` örneklerin avantajı, performans duyarlı senaryolarda ücretsiz ve daha iyi bir şekilde ayrılabilmektedir.</span><span class="sxs-lookup"><span data-stu-id="0344a-252">The advantage being that such `delegate` instances can be allocation free and better in performance sensitive scenarios.</span></span>

<span data-ttu-id="0344a-253">İşlev işaretçisi özelliği uygulanmışsa, `static delegate` teklif büyük olasılıkla kapatılmış olur. Bu özelliğin önerilen avantajı, ayırma ücretsiz doğası olur.</span><span class="sxs-lookup"><span data-stu-id="0344a-253">If the function pointer feature is implemented the `static delegate` proposal will likely be closed out. The proposed advantage of that feature is the allocation free nature.</span></span> <span data-ttu-id="0344a-254">Ancak son araştırmalar, derleme kaldırma işlemi nedeniyle elde edilebilecek mümkün olmayan bir şekilde bulundu.</span><span class="sxs-lookup"><span data-stu-id="0344a-254">However recent investigations have found that is not possible to achieve due to assembly unloading.</span></span> <span data-ttu-id="0344a-255">Derlemenin içinden bellekten kaldırılmasını önlemek için `static delegate` için başvurduğu yönteme güçlü bir tanıtıcı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="0344a-255">There must be a strong handle from the `static delegate` to the method it refers to in order to keep the assembly from being unloaded out from under it.</span></span>

<span data-ttu-id="0344a-256">Her `static delegate` örneğinin bakımını yapmak için, teklifin hedeflerine sayaç çalıştıran yeni bir tanıtıcı tahsis etmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="0344a-256">To maintain every `static delegate` instance would be required to allocate a new handle which runs counter to the goals of the proposal.</span></span> <span data-ttu-id="0344a-257">Ayırmanın çağrı-site başına tek bir ayırmaya salsatılabileceği, ancak bir bit karmaşıktı olduğu ve bu da ticareti kapatmasının gerektiği bazı tasarımlar vardı.</span><span class="sxs-lookup"><span data-stu-id="0344a-257">There were some designs where the allocation could be amortized to a single allocation per call-site but that was a bit complex and didn't seem worth the trade off.</span></span>

<span data-ttu-id="0344a-258">Diğer bir deyişle, geliştiriciler temelde aşağıdaki denge ile karar vermek zorunda değildir:</span><span class="sxs-lookup"><span data-stu-id="0344a-258">That means developers essentially have to decide between the following trade offs:</span></span>

1. <span data-ttu-id="0344a-259">Derlemeyi kaldırma aşamasında güvenlik: Bu ayırmaları gerektirir ve bu nedenle `delegate` zaten yeterli bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="0344a-259">Safety in the face of assembly unloading: this requires allocations and hence `delegate` is already a sufficient option.</span></span>
1. <span data-ttu-id="0344a-260">Derleme kaldırma aşamasında güvenlik yok: `delegate*`kullanın.</span><span class="sxs-lookup"><span data-stu-id="0344a-260">No safety in face of assembly unloading: use a `delegate*`.</span></span> <span data-ttu-id="0344a-261">Bu, kodun geri kalanında bir `unsafe` bağlamı dışında kullanılmasına izin vermek için bir `struct` kaydırılmış olabilir.</span><span class="sxs-lookup"><span data-stu-id="0344a-261">This can be wrapped in a `struct` to allow usage outside an `unsafe` context in the rest of the code.</span></span>
