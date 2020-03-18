---
ms.openlocfilehash: ac4c8760e3b6a0934f01ae634f666af60aa1c0fe
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484568"
---
# <a name="compiler-intrinsics"></a><span data-ttu-id="f6c58-101">Derleyici İç Bilgileri</span><span class="sxs-lookup"><span data-stu-id="f6c58-101">Compiler Intrinsics</span></span>

## <a name="summary"></a><span data-ttu-id="f6c58-102">Özet</span><span class="sxs-lookup"><span data-stu-id="f6c58-102">Summary</span></span>

<span data-ttu-id="f6c58-103">Bu teklif, şu anda etkin şekilde erişilemeyen düşük düzeydeki Il işlem kodları 'ları, `ldftn`, `ldvirtftn`, `ldtoken` ve `calli`sunan dil yapıları sağlar.</span><span class="sxs-lookup"><span data-stu-id="f6c58-103">This proposal provides language constructs that expose low level IL opcodes that cannot currently be accessed efficiently, or at all: `ldftn`, `ldvirtftn`, `ldtoken` and `calli`.</span></span> <span data-ttu-id="f6c58-104">Bu düşük düzey Opcode 'ları yüksek performans kodunda önemli olabilir ve geliştiricilerin bunlara erişmek için etkili bir yol olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="f6c58-104">These low level opcodes can be important in high performance code and developers need an efficient way to access them.</span></span>

## <a name="motivation"></a><span data-ttu-id="f6c58-105">Amacı</span><span class="sxs-lookup"><span data-stu-id="f6c58-105">Motivation</span></span>

<span data-ttu-id="f6c58-106">Bu özelliğin ilgi çekici ve arka planı, aşağıdaki konuda açıklanmaktadır (özelliğin olası bir uygulamasıdır):</span><span class="sxs-lookup"><span data-stu-id="f6c58-106">The motivations and background for this feature are described in the following issue (as is a potential implementation of the feature):</span></span> 

https://github.com/dotnet/csharplang/issues/191

<span data-ttu-id="f6c58-107">Bu alternatif tasarım teklifi, özgün teklifin prototip uygulamasını gözden geçirdikten sonra, önemli bir kod tabanı genelinde @msjabby.</span><span class="sxs-lookup"><span data-stu-id="f6c58-107">This alternate design proposal comes after reviewing a prototype implementation of the original proposal by @msjabby as well as the use throughout a significant code base.</span></span> <span data-ttu-id="f6c58-108">Bu tasarım @mjsabby, @tmat ve @jkotasönemli bir giriş ile gerçekleştirildi.</span><span class="sxs-lookup"><span data-stu-id="f6c58-108">This design was done with significant input from @mjsabby, @tmat and @jkotas.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="f6c58-109">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="f6c58-109">Detailed Design</span></span> 

### <a name="allow-address-of-to-target-methods"></a><span data-ttu-id="f6c58-110">Hedef yöntemlere adrese izin ver</span><span class="sxs-lookup"><span data-stu-id="f6c58-110">Allow address of to target methods</span></span>

<span data-ttu-id="f6c58-111">Yöntem gruplarına artık bir adres ifadesi için bağımsız değişken olarak izin verilir.</span><span class="sxs-lookup"><span data-stu-id="f6c58-111">Method groups will now be allowed as arguments to an address-of expression.</span></span> <span data-ttu-id="f6c58-112">Böyle bir ifadenin türü `void*`olacaktır.</span><span class="sxs-lookup"><span data-stu-id="f6c58-112">The type of such an expression will be `void*`.</span></span> 

``` csharp
class Util { 
    public static void Log() { } 
}

// ldftn Util.Log
void* ptr = &Util.Log; 
```

<span data-ttu-id="f6c58-113">Burada herhangi bir temsilci dönüştürme yok, yöntem grubundaki filtreleme üyeleri için tek mekanizma statik/örnek erişimine göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="f6c58-113">Given there is no delegate conversion here the only mechanism for filtering members in the method group is by static / instance access.</span></span> <span data-ttu-id="f6c58-114">Bu, üyeleri ayıramadığından, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="f6c58-114">If that cannot distinguish the members then a compile time error will occur.</span></span>

``` csharp
class Util { 
    public void Log() { } 
    public void Log(string p1) { } 
    public static void Log(int i) { };
}

unsafe {
    // Error: Method group Log has more than one applicable candidate.
    void* ptr1 = &Log; 

    // Okay: only one static member to consider here.
    void* ptr2 = &Util.Log;
}
```

<span data-ttu-id="f6c58-115">Bu bağlamdaki AddressOf ifadesi aşağıdaki şekilde uygulanacaktır:</span><span class="sxs-lookup"><span data-stu-id="f6c58-115">The addressof expression in this context will be implemented in the following manner:</span></span>

- <span data-ttu-id="f6c58-116">ldftn: yöntem sanal olmayan bir değer.</span><span class="sxs-lookup"><span data-stu-id="f6c58-116">ldftn: when the method is non-virtual.</span></span>
- <span data-ttu-id="f6c58-117">ldvirtftn: yöntem sanal olduğunda.</span><span class="sxs-lookup"><span data-stu-id="f6c58-117">ldvirtftn: when the method is virtual.</span></span>

<span data-ttu-id="f6c58-118">Bu özelliğin kısıtlamaları:</span><span class="sxs-lookup"><span data-stu-id="f6c58-118">Restrictions of this feature:</span></span>

- <span data-ttu-id="f6c58-119">Örnek yöntemleri yalnızca bir değer üzerinde çağırma ifadesi kullanılırken belirtilebilir</span><span class="sxs-lookup"><span data-stu-id="f6c58-119">Instance methods can only be specified when using an invocation expression on a value</span></span>
- <span data-ttu-id="f6c58-120">Yerel işlevler `&`kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="f6c58-120">Local functions cannot be used in `&`.</span></span> <span data-ttu-id="f6c58-121">Bu yöntemlerin uygulama ayrıntıları, dil tarafından kasıtlı olarak belirtilmez.</span><span class="sxs-lookup"><span data-stu-id="f6c58-121">The implementation details of these methods are deliberately not specified by the language.</span></span> <span data-ttu-id="f6c58-122">Bu, statik ve örnek veya tam olarak hangi imzayı yayındıklarınızı içerir.</span><span class="sxs-lookup"><span data-stu-id="f6c58-122">This includes whether they are static vs. instance or exactly what signature they are emitted with.</span></span>

### <a name="handleof"></a><span data-ttu-id="f6c58-123">handleof</span><span class="sxs-lookup"><span data-stu-id="f6c58-123">handleof</span></span>

<span data-ttu-id="f6c58-124">`handleof` bağlamsal anahtar sözcüğü `ldtoken` yönergesini kullanarak bir alanı, üyeyi veya türü eşdeğer `RuntimeHandle` türüne çevirir.</span><span class="sxs-lookup"><span data-stu-id="f6c58-124">The `handleof` contextual keyword will translate a field, member or type into their equivalent `RuntimeHandle` type using the `ldtoken` instruction.</span></span> <span data-ttu-id="f6c58-125">İfadenin tam türü, `handleof`ad türüne bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="f6c58-125">The exact type of the expression will depend on the kind of the name in `handleof`:</span></span>

- <span data-ttu-id="f6c58-126">Alan: `RuntimeFieldHandle`</span><span class="sxs-lookup"><span data-stu-id="f6c58-126">field: `RuntimeFieldHandle`</span></span>
- <span data-ttu-id="f6c58-127">Tür: `RuntimeTypeHandle`</span><span class="sxs-lookup"><span data-stu-id="f6c58-127">type: `RuntimeTypeHandle`</span></span>
- <span data-ttu-id="f6c58-128">Yöntem: `RuntimeMethodHandle`</span><span class="sxs-lookup"><span data-stu-id="f6c58-128">method: `RuntimeMethodHandle`</span></span>

<span data-ttu-id="f6c58-129">`handleof` bağımsız değişkenler `nameof`benzerdir.</span><span class="sxs-lookup"><span data-stu-id="f6c58-129">The arguments to `handleof` are identical to `nameof`.</span></span> <span data-ttu-id="f6c58-130">Bu, bir basit ad, tam ad, üye erişimi, belirtilen üye ile temel erişim veya belirtilen üyeyle bu erişim olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f6c58-130">It must be a simple name, qualified name, member access, base access with a specified member, or this access with a specified member.</span></span> <span data-ttu-id="f6c58-131">Bağımsız değişken ifadesi bir kod tanımını tanımlar, ancak hiçbir şekilde değerlendirilmez.</span><span class="sxs-lookup"><span data-stu-id="f6c58-131">The argument expression identifies a code definition, but it is never evaluated.</span></span>

<span data-ttu-id="f6c58-132">`handleof` ifadesi çalışma zamanında değerlendirilir ve `RuntimeHandle`dönüş türüne sahiptir.</span><span class="sxs-lookup"><span data-stu-id="f6c58-132">The `handleof` expression is evaluated at runtime and has a return type of `RuntimeHandle`.</span></span> <span data-ttu-id="f6c58-133">Bu, güvenli kod ve güvenli olmayan bir şekilde çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="f6c58-133">This can be executed in safe code as well as unsafe.</span></span> 

``` 
RuntimeHandle stringHandle = handleof(string);
```

<span data-ttu-id="f6c58-134">Bu özelliğin kısıtlamaları:</span><span class="sxs-lookup"><span data-stu-id="f6c58-134">Restrictions of this feature:</span></span>

- <span data-ttu-id="f6c58-135">`handleof` ifadesinde özellikler kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="f6c58-135">Properties cannot be used in a `handleof` expression.</span></span>
- <span data-ttu-id="f6c58-136">Kapsamda var olan bir `handleof` adı olduğunda `handleof` ifadesi kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="f6c58-136">The `handleof` expression cannot be used when there is an existing `handleof` name in scope.</span></span> <span data-ttu-id="f6c58-137">Örneğin bir tür, ad alanı, vb...</span><span class="sxs-lookup"><span data-stu-id="f6c58-137">For example a type, namespace, etc ...</span></span>

### <a name="calli"></a><span data-ttu-id="f6c58-138">Calli</span><span class="sxs-lookup"><span data-stu-id="f6c58-138">calli</span></span>

<span data-ttu-id="f6c58-139">Derleyici, `.calli` yönergesine verimli bir şekilde çeviren yeni bir `extern` işlevi türü için destek ekler.</span><span class="sxs-lookup"><span data-stu-id="f6c58-139">The compiler will add support for a new type of `extern` function that efficiently translates into a `.calli` instruction.</span></span> <span data-ttu-id="f6c58-140">Extern özniteliği aşağıdaki şeklin özniteliğiyle işaretlenir:</span><span class="sxs-lookup"><span data-stu-id="f6c58-140">The extern attribute will be marked with an attribute of the following shape:</span></span>

``` csharp
[AttributeUsage(AttributeTargets.Method)]
public sealed class CallIndirectAttribute : Attribute
{
    public CallingConvention CallingConvention { get; }
    public CallIndirectAttribute(CallingConvention callingConvention)
    {
        CallingConvention = callingConvention;
    }
}
```

<span data-ttu-id="f6c58-141">Bu, geliştiricilerin aşağıdaki biçimde Yöntemler tanımlamasına olanak tanır:</span><span class="sxs-lookup"><span data-stu-id="f6c58-141">This allows developers to define methods in the following form:</span></span>

``` csharp
[CallIndirect(CallingConvention.Cdecl)]
static extern int MapValue(string s, void *ptr);

unsafe {
    var i = MapValue("42", &int.Parse);
    Console.WriteLine(i);
}
```

<span data-ttu-id="f6c58-142">Yöntem üzerinde `CallIndirect` özniteliği uygulanan kısıtlamalar:</span><span class="sxs-lookup"><span data-stu-id="f6c58-142">Restrictions on the method which has the `CallIndirect` attribute applied:</span></span>

- <span data-ttu-id="f6c58-143">`DllImport` özniteliğine sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="f6c58-143">Cannot have a `DllImport` attribute.</span></span>
- <span data-ttu-id="f6c58-144">Genel olamaz.</span><span class="sxs-lookup"><span data-stu-id="f6c58-144">Cannot be generic.</span></span>

## <a name="open-issues"></a><span data-ttu-id="f6c58-145">Açık sorunlar</span><span class="sxs-lookup"><span data-stu-id="f6c58-145">Open Issues</span></span>

### <a name="callingconvention"></a><span data-ttu-id="f6c58-146">CallingConvention</span><span class="sxs-lookup"><span data-stu-id="f6c58-146">CallingConvention</span></span>

<span data-ttu-id="f6c58-147">Tasarlanan `CallIndirectAttribute`, yönetilen çağırma kuralları için bir girişi olmayan `CallingConvention` enum kullanır.</span><span class="sxs-lookup"><span data-stu-id="f6c58-147">The `CallIndirectAttribute` as designed uses the `CallingConvention` enum which lacks an entry for managed calling conventions.</span></span> <span data-ttu-id="f6c58-148">Numaralama, bu çağırma kuralını içerecek şekilde genişletilmelidir veya özniteliğin farklı bir yaklaşım yapması gerekir.</span><span class="sxs-lookup"><span data-stu-id="f6c58-148">The enum either needs to be extended to include this calling convention or the attribute needs to take a different approach.</span></span>

## <a name="considerations"></a><span data-ttu-id="f6c58-149">Dikkat edilmesi gerekenler</span><span class="sxs-lookup"><span data-stu-id="f6c58-149">Considerations</span></span>

### <a name="disambiguating-method-groups"></a><span data-ttu-id="f6c58-150">Yöntem gruplarını Kesinleştirme</span><span class="sxs-lookup"><span data-stu-id="f6c58-150">Disambiguating method groups</span></span>

<span data-ttu-id="f6c58-151">Bir adres ifadesine geçirilen yöntem gruplarının belirsizliğini daha kolay hale getirmek için özellikler etrafında bazı tartışmalar vardı.</span><span class="sxs-lookup"><span data-stu-id="f6c58-151">There was some discussion around features that would make it easier to disambiguate method groups passed to an address-of expression.</span></span> <span data-ttu-id="f6c58-152">Örneğin, sözdizimine imza öğeleri ekleme olasılığı vardır:</span><span class="sxs-lookup"><span data-stu-id="f6c58-152">For instance potentially adding signature elements to the syntax:</span></span>

``` csharp
class Util {
    public static void Log() { ... }
    public static void Log(string) { ... }
}

unsafe {
    // Error: ambiguous Log
    void *ptr1 = &Util.Log;

    // Use Util.Log();
    void *ptr2 = &Util.Log();
}
```

<span data-ttu-id="f6c58-153">Bu, etkileyici bir servis talebi yapıldığından veya basit bir sözdiziminin envisioned olmadığı için reddedildi.</span><span class="sxs-lookup"><span data-stu-id="f6c58-153">This was rejected because a compelling case could not be made nor could a simple syntax be envisioned here.</span></span> <span data-ttu-id="f6c58-154">Ayrıca, çok basit bir ileri doğru iş vardır: basit olan ve istenen işleve çağrı yapmak için kodu C# kullanan başka bir yöntem belirleyin.</span><span class="sxs-lookup"><span data-stu-id="f6c58-154">Also there is a fairly straight forward work around: simple define another method that is unambiguous and uses C# code to call into the desired function.</span></span> 

``` csharp
class Workaround {
    public static void LocalLog() => Util.Log();
}
unsafe { 
    void* ptr = &Workaround.LocalLog;
}
```

<span data-ttu-id="f6c58-155">Bu, `static` yerel işlevler dile girerken daha da kolay hale gelir.</span><span class="sxs-lookup"><span data-stu-id="f6c58-155">This becomes even simpler if `static` local functions enter the language.</span></span> <span data-ttu-id="f6c58-156">Sonra, geçici bir işlem, belirsiz bir işlem olan işlev için aynı işlevde tanımlanabilir:</span><span class="sxs-lookup"><span data-stu-id="f6c58-156">Then the work around could be defined in the same function that used the ambiguous address-of operation:</span></span>

``` csharp
unsafe { 
    static void LocalLog() => Util.Log();
    void* ptr = &Workaround.LocalLog;
}
```

### <a name="loadtypetokenint32"></a><span data-ttu-id="f6c58-157">LoadTypeTokenInt32</span><span class="sxs-lookup"><span data-stu-id="f6c58-157">LoadTypeTokenInt32</span></span>

<span data-ttu-id="f6c58-158">Meta veri belirteçlerinin derleme zamanında `int` değerler olarak yüklenmesine izin verilen orijinal teklif.</span><span class="sxs-lookup"><span data-stu-id="f6c58-158">The original proposal allowed for metadata tokens to be loaded as `int` values at compile time.</span></span> <span data-ttu-id="f6c58-159">Temelde, `handleof` aynı bağımsız değişkenlere sahip olan, ancak derleme zamanında `int` sabitine değerlendirilen `tokenof` vardır.</span><span class="sxs-lookup"><span data-stu-id="f6c58-159">Essentially have `tokenof` that has the same arguments as `handleof` but is evaluated at compile time to an `int` constant.</span></span> 

<span data-ttu-id="f6c58-160">Bu, Il yeniden yazarken önemli bir soruna neden olduğundan (.NET 'in birçok sürümü olduğu için) reddedildi.</span><span class="sxs-lookup"><span data-stu-id="f6c58-160">This was rejected as it causes significant problem for IL rewrites (of which .NET has many).</span></span> <span data-ttu-id="f6c58-161">Bu tür reyazarlar genellikle meta veri tablolarını bu değerleri geçersiz kılacak şekilde işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="f6c58-161">Such rewriters often manipulate the metadata tables in a way that could invalidate these values.</span></span> <span data-ttu-id="f6c58-162">Bu tür reyazanların basit `int` değerler olarak depolandıklarında bu değerleri güncelleştirmesi için makul bir yol yoktur.</span><span class="sxs-lookup"><span data-stu-id="f6c58-162">There is no reasonable way for such rewriters to update these values when they are stored as simple `int` values.</span></span>

<span data-ttu-id="f6c58-163">Meta veri girdileri için donuk bir tutamacı olmanın temel fikri, çalışma zamanı ekibi tarafından araştırmeye devam edecektir.</span><span class="sxs-lookup"><span data-stu-id="f6c58-163">The underlying idea of having an opaque handle for metadata entries will continue to be explored by the runtime team.</span></span> 

## <a name="future-considerations"></a><span data-ttu-id="f6c58-164">Gelecekteki konular</span><span class="sxs-lookup"><span data-stu-id="f6c58-164">Future Considerations</span></span>

### <a name="static-local-functions"></a><span data-ttu-id="f6c58-165">statik yerel işlevler</span><span class="sxs-lookup"><span data-stu-id="f6c58-165">static local functions</span></span>

<span data-ttu-id="f6c58-166">Bu, yerel işlevlerde `static` değiştiriciye izin veren [öneriyi](https://github.com/dotnet/csharplang/issues/1565) ifade eder.</span><span class="sxs-lookup"><span data-stu-id="f6c58-166">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/1565) to allow the `static` modifier on local functions.</span></span> <span data-ttu-id="f6c58-167">Bu tür bir işlevin, `static` olarak ve kaynak kodunda tam imzayla belirtilen şekilde yayınlanacağı garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="f6c58-167">Such a function would be guaranteed to be emitted as `static` and with the exact signature specified in source code.</span></span> <span data-ttu-id="f6c58-168">Bu tür bir işlev, yerel işlevlerin bugünkü sorunlarından hiçbirini içermediğinden `&` için geçerli bir bağımsız değişken olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f6c58-168">Such a function should be a valid argument to `&` as it contains none of the problems local functions have today.</span></span>

### <a name="nativecallableattribute"></a><span data-ttu-id="f6c58-169">NativeCallableAttribute</span><span class="sxs-lookup"><span data-stu-id="f6c58-169">NativeCallableAttribute</span></span>

<span data-ttu-id="f6c58-170">CLR 'nin, yönetilen yöntemlerin yerel koddan doğrudan çağrılabilir şekilde oluşturulmasına izin veren bir özelliği vardır.</span><span class="sxs-lookup"><span data-stu-id="f6c58-170">The CLR has a feature that allows for managed methods to be emitted in such a way that they are directly callable from native code.</span></span> <span data-ttu-id="f6c58-171">Bu, `NativeCallableAttribute` yöntemlere eklenerek yapılır.</span><span class="sxs-lookup"><span data-stu-id="f6c58-171">This is done by adding the `NativeCallableAttribute` to methods.</span></span> <span data-ttu-id="f6c58-172">Böyle bir yöntem yalnızca yerel koddan çağrılabilir ve bu nedenle yalnızca İmzada blittable türleri içermelidir.</span><span class="sxs-lookup"><span data-stu-id="f6c58-172">Such a method is only callable from native code and hence must contain only blittable types in the signature.</span></span> <span data-ttu-id="f6c58-173">Yönetilen koddan çağırma, bir çalışma zamanı hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="f6c58-173">Calling from managed code results in a runtime error.</span></span> 

<span data-ttu-id="f6c58-174">Bu özellik, izin verilen şekilde bu teklifle birlikte kalıp düzenine neden olur:</span><span class="sxs-lookup"><span data-stu-id="f6c58-174">This feature would pattern well with this proposal as it would allow:</span></span>

- <span data-ttu-id="f6c58-175">Yönetilen kodda tanımlanan bir işlevi, yönetilen veya yerel kodda ek yüke sahip olmayan bir işlev işaretçisi (adres aracılığıyla) olarak yerel koda geçirme.</span><span class="sxs-lookup"><span data-stu-id="f6c58-175">Passing a function defined in managed code to native code as a function pointer (via address-of) with no overhead in managed or native code.</span></span> 
- <span data-ttu-id="f6c58-176">Çalışma zamanı, Yönetilen koddaki bu gibi işlevler için site hatalarını, derleme zamanında çağrılmasını engellemek için kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="f6c58-176">Runtime can introduce use site errors for such functions in managed code to prevent them from being invoked at compile time.</span></span>




