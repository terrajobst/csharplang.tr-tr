---
ms.openlocfilehash: dbea611280a644adc25247b9887986e129c59b68
ms.sourcegitcommit: a5e393b018b04dfa55aae0000290ca087b508495
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2019
ms.locfileid: "72310363"
---
# <a name="unsafe-code"></a><span data-ttu-id="378d4-101">Güvenli olmayan kod</span><span class="sxs-lookup"><span data-stu-id="378d4-101">Unsafe code</span></span>

<span data-ttu-id="378d4-102">Önceki bölümlerde C# tanımlandığı gibi çekirdek dil, C 'den ve C++ veri türü olarak işaretçilerin atlanından farklıdır.</span><span class="sxs-lookup"><span data-stu-id="378d4-102">The core C# language, as defined in the preceding chapters, differs notably from C and C++ in its omission of pointers as a data type.</span></span> <span data-ttu-id="378d4-103">Bunun yerine C# , başvuruları ve çöp toplayıcı tarafından yönetilen nesneler oluşturma özelliğini sağlar.</span><span class="sxs-lookup"><span data-stu-id="378d4-103">Instead, C# provides references and the ability to create objects that are managed by a garbage collector.</span></span> <span data-ttu-id="378d4-104">Diğer özelliklerle bağlanmış bu tasarım, C veya C# C++' den çok daha güvenli bir dil sağlar.</span><span class="sxs-lookup"><span data-stu-id="378d4-104">This design, coupled with other features, makes C# a much safer language than C or C++.</span></span> <span data-ttu-id="378d4-105">Çekirdek C# dilde, başlatılmamış bir değişkene, "Dangling" işaretçisine veya bir diziyi, sınırlarının ötesinde dizinleyen bir ifadeye sahip olmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="378d4-105">In the core C# language it is simply not possible to have an uninitialized variable, a "dangling" pointer, or an expression that indexes an array beyond its bounds.</span></span> <span data-ttu-id="378d4-106">C ve programları düzenli olarak oluşturan hataların tüm kategorileri bundan C++ sonra ortadan kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="378d4-106">Whole categories of bugs that routinely plague C and C++ programs are thus eliminated.</span></span>

<span data-ttu-id="378d4-107">Neredeyse her işaretçi türü yapısını C 'de veya C++ içinde C#bir başvuru türüne sahip olsa da, işaretçi türlerine erişimin bir zorunludur hale geldiği durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="378d4-107">While practically every pointer type construct in C or C++ has a reference type counterpart in C#, nonetheless, there are situations where access to pointer types becomes a necessity.</span></span> <span data-ttu-id="378d4-108">Örneğin, temel alınan işletim sistemiyle arabirim oluşturma, bellek eşlemeli cihaza erişme veya zaman açısından kritik bir algoritma uygulama, işaretçilere erişim olmadan mümkün veya pratik olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-108">For example, interfacing with the underlying operating system, accessing a memory-mapped device, or implementing a time-critical algorithm may not be possible or practical without access to pointers.</span></span> <span data-ttu-id="378d4-109">Bu gereksinimi karşılamak için C# ***güvenli olmayan kod***yazma özelliği sağlar.</span><span class="sxs-lookup"><span data-stu-id="378d4-109">To address this need, C# provides the ability to write ***unsafe code***.</span></span>

<span data-ttu-id="378d4-110">Güvenli olmayan kodda, işaretçiler ve integral türleri arasında dönüştürmeler gerçekleştirmek, değişkenlerin adresini almak için ve benzeri işlemleri yapmak üzere işaretçiler üzerinde bildirmek ve çalıştırmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="378d4-110">In unsafe code it is possible to declare and operate on pointers, to perform conversions between pointers and integral types, to take the address of variables, and so forth.</span></span> <span data-ttu-id="378d4-111">Bir anlamda, güvenli olmayan kod yazmak bir C# program içinde C kodu yazma gibidir.</span><span class="sxs-lookup"><span data-stu-id="378d4-111">In a sense, writing unsafe code is much like writing C code within a C# program.</span></span>

<span data-ttu-id="378d4-112">Güvenli olmayan kod, geliştiricilerin ve kullanıcıların perspektifinden "güvenli" bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="378d4-112">Unsafe code is in fact a "safe" feature from the perspective of both developers and users.</span></span> <span data-ttu-id="378d4-113">Güvenli olmayan kod değiştirici `unsafe`açıkça işaretlenmelidir, bu nedenle geliştiriciler yanlışlıkla güvenli olmayan özellikler kullanamaz ve yürütme altyapısı, güvenli olmayan kodun güvenilmeyen bir ortamda yürütülmemesini sağlamak için çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="378d4-113">Unsafe code must be clearly marked with the modifier `unsafe`, so developers can't possibly use unsafe features accidentally, and the execution engine works to ensure that unsafe code cannot be executed in an untrusted environment.</span></span>

## <a name="unsafe-contexts"></a><span data-ttu-id="378d4-114">Güvenli olmayan bağlamlar</span><span class="sxs-lookup"><span data-stu-id="378d4-114">Unsafe contexts</span></span>

<span data-ttu-id="378d4-115">Güvenli olmayan özellikleri C# yalnızca güvenli olmayan bağlamlarda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-115">The unsafe features of C# are available only in unsafe contexts.</span></span> <span data-ttu-id="378d4-116">Güvenli olmayan bir bağlam, bir tür veya üyenin bildiriminde `unsafe` değiştiricisi eklenerek veya bir *unsafe_statement*kullanarak ortaya çıkartılır:</span><span class="sxs-lookup"><span data-stu-id="378d4-116">An unsafe context is introduced by including an `unsafe` modifier in the declaration of a type or member, or by employing an *unsafe_statement*:</span></span>

*  <span data-ttu-id="378d4-117">Bir sınıf, yapı, arabirim veya temsilcinin bir bildirimi `unsafe` değiştirici içerebilir ve bu durumda, bu tür bildiriminin tüm metinsel uzantısı (sınıf, yapı veya arabirim gövdesi dahil) güvenli olmayan bir bağlam olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-117">A declaration of a class, struct, interface, or delegate may include an `unsafe` modifier, in which case the entire textual extent of that type declaration (including the body of the class, struct, or interface) is considered an unsafe context.</span></span>
*  <span data-ttu-id="378d4-118">Bir alan, yöntem, özellik, olay, Dizin Oluşturucu, işleç, örnek Oluşturucu, yıkıcı veya statik oluşturucunun bir bildirimi `unsafe` değiştirici içerebilir, bu durumda bu üye bildiriminin tüm metin kapsamı güvenli olmayan bir bağlam olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-118">A declaration of a field, method, property, event, indexer, operator, instance constructor, destructor, or static constructor may include an `unsafe` modifier, in which case the entire textual extent of that member declaration is considered an unsafe context.</span></span>
*  <span data-ttu-id="378d4-119">*Unsafe_statement* bir *blok*içinde güvenli olmayan bir bağlam kullanımını mümkün bir şekilde sunar.</span><span class="sxs-lookup"><span data-stu-id="378d4-119">An *unsafe_statement* enables the use of an unsafe context within a *block*.</span></span> <span data-ttu-id="378d4-120">İlişkili *bloğun* tüm metinsel uzantısı güvenli olmayan bir bağlam olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-120">The entire textual extent of the associated *block* is considered an unsafe context.</span></span>

<span data-ttu-id="378d4-121">İlişkili dilbilgisi üretimleri aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="378d4-121">The associated grammar productions are shown below.</span></span>

```antlr
class_modifier_unsafe
    : 'unsafe'
    ;

struct_modifier_unsafe
    : 'unsafe'
    ;

interface_modifier_unsafe
    : 'unsafe'
    ;

delegate_modifier_unsafe
    : 'unsafe'
    ;

field_modifier_unsafe
    : 'unsafe'
    ;

method_modifier_unsafe
    : 'unsafe'
    ;

property_modifier_unsafe
    : 'unsafe'
    ;

event_modifier_unsafe
    : 'unsafe'
    ;

indexer_modifier_unsafe
    : 'unsafe'
    ;

operator_modifier_unsafe
    : 'unsafe'
    ;

constructor_modifier_unsafe
    : 'unsafe'
    ;

destructor_declaration_unsafe
    : attributes? 'extern'? 'unsafe'? '~' identifier '(' ')' destructor_body
    | attributes? 'unsafe'? 'extern'? '~' identifier '(' ')' destructor_body
    ;

static_constructor_modifiers_unsafe
    : 'extern'? 'unsafe'? 'static'
    | 'unsafe'? 'extern'? 'static'
    | 'extern'? 'static' 'unsafe'?
    | 'unsafe'? 'static' 'extern'?
    | 'static' 'extern'? 'unsafe'?
    | 'static' 'unsafe'? 'extern'?
    ;

embedded_statement_unsafe
    : unsafe_statement
    | fixed_statement
    ;

unsafe_statement
    : 'unsafe' block
    ;
```

<span data-ttu-id="378d4-122">örnekte</span><span class="sxs-lookup"><span data-stu-id="378d4-122">In the example</span></span>

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

<span data-ttu-id="378d4-123">struct bildiriminde belirtilen `unsafe` değiştirici, struct bildiriminin tüm metinsel kapsamının güvenli olmayan bir bağlam haline gelmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="378d4-123">the `unsafe` modifier specified in the struct declaration causes the entire textual extent of the struct declaration to become an unsafe context.</span></span> <span data-ttu-id="378d4-124">Bu nedenle, `Left` ve `Right` alanlarını bir işaretçi türü olarak bildirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="378d4-124">Thus, it is possible to declare the `Left` and `Right` fields to be of a pointer type.</span></span> <span data-ttu-id="378d4-125">Yukarıdaki örnek de yazılabilir</span><span class="sxs-lookup"><span data-stu-id="378d4-125">The example above could also be written</span></span>

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

<span data-ttu-id="378d4-126">Burada, alan bildirimlerinde `unsafe` değiştiriciler, bu bildirimlerin güvenli olmayan bağlamlar olarak kabul edilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="378d4-126">Here, the `unsafe` modifiers in the field declarations cause those declarations to be considered unsafe contexts.</span></span>

<span data-ttu-id="378d4-127">Güvenli olmayan bir bağlam oluşturma dışında, işaretçi türlerinin kullanılmasına izin veren `unsafe` değiştiricinin bir tür veya üye üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="378d4-127">Other than establishing an unsafe context, thus permitting the use of pointer types, the `unsafe` modifier has no effect on a type or a member.</span></span> <span data-ttu-id="378d4-128">örnekte</span><span class="sxs-lookup"><span data-stu-id="378d4-128">In the example</span></span>

```csharp
public class A
{
    public unsafe virtual void F() {
        char* p;
        ...
    }
}

public class B: A
{
    public override void F() {
        base.F();
        ...
    }
}
```

<span data-ttu-id="378d4-129">`A` `F` yönteminde `unsafe` değiştirici, `F` 'ın metin kapsamının, dilin güvenli olmayan özelliklerinin kullanılabileceği güvenli olmayan bir bağlam haline gelmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="378d4-129">the `unsafe` modifier on the `F` method in `A` simply causes the textual extent of `F` to become an unsafe context in which the unsafe features of the language can be used.</span></span> <span data-ttu-id="378d4-130">`B``F` geçersiz kılmada, `unsafe` değiştiricinin yeniden belirtilmesi gerekmez--kuşkusuz, `B` içindeki `F` yönteminin güvenli olmayan özelliklere erişmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="378d4-130">In the override of `F` in `B`, there is no need to re-specify the `unsafe` modifier -- unless, of course, the `F` method in `B` itself needs access to unsafe features.</span></span>

<span data-ttu-id="378d4-131">İşaretçi türü metodun imzasının bir parçası olduğunda durum biraz farklıdır</span><span class="sxs-lookup"><span data-stu-id="378d4-131">The situation is slightly different when a pointer type is part of the method's signature</span></span>

```csharp
public unsafe class A
{
    public virtual void F(char* p) {...}
}

public class B: A
{
    public unsafe override void F(char* p) {...}
}
```

<span data-ttu-id="378d4-132">Burada `F`imzası bir işaretçi türü içerdiğinden, yalnızca güvenli olmayan bir bağlamda yazılabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-132">Here, because `F`'s signature includes a pointer type, it can only be written in an unsafe context.</span></span> <span data-ttu-id="378d4-133">Bununla birlikte, güvenli olmayan bağlam, `A`olduğu gibi, tüm sınıfı güvenli hale getirerek veya yöntem bildiriminde bir `unsafe` değiştiricisi ekleyerek (`B`durumda olduğu gibi) gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-133">However, the unsafe context can be introduced by either making the entire class unsafe, as is the case in `A`, or by including an `unsafe` modifier in the method declaration, as is the case in `B`.</span></span>

## <a name="pointer-types"></a><span data-ttu-id="378d4-134">İşaretçi türleri</span><span class="sxs-lookup"><span data-stu-id="378d4-134">Pointer types</span></span>

<span data-ttu-id="378d4-135">Güvenli olmayan bir bağlamda, bir *tür* ([türler](types.md)) *pointer_type* bir *value_type* ya da *reference_type*olabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-135">In an unsafe context, a *type* ([Types](types.md)) may be a *pointer_type* as well as a *value_type* or a *reference_type*.</span></span> <span data-ttu-id="378d4-136">Ancak bir *pointer_type* , bu kullanım güvenli olmadığı için güvenli olmayan bir bağlam dışında bir `typeof` ifadesinde de kullanılabilir ([anonim nesne oluşturma ifadelerinde](expressions.md#anonymous-object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="378d4-136">However, a *pointer_type* may also be used in a `typeof` expression ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) outside of an unsafe context as such usage is not unsafe.</span></span>

```antlr
type_unsafe
    : pointer_type
    ;
```

<span data-ttu-id="378d4-137">Bir *pointer_type* *unmanaged_type* veya anahtar sözcüğü `void`, ardından bir `*` belirteci tarafından yazılır:</span><span class="sxs-lookup"><span data-stu-id="378d4-137">A *pointer_type* is written as an *unmanaged_type* or the keyword `void`, followed by a `*` token:</span></span>

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

<span data-ttu-id="378d4-138">Bir işaretçi türündeki `*` önce belirtilen tür, işaretçi türünün ***başvurulan türü*** olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="378d4-138">The type specified before the `*` in a pointer type is called the ***referent type*** of the pointer type.</span></span> <span data-ttu-id="378d4-139">İşaretçi türündeki bir değerin işaret ettiği değişkenin türünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="378d4-139">It represents the type of the variable to which a value of the pointer type points.</span></span>

<span data-ttu-id="378d4-140">Başvuruların aksine (başvuru türlerinin değerleri), işaretçiler çöp toplayıcı tarafından izlenmez. çöp toplayıcı, hiçbir işaretçi bilgisine ve işaret ettikleri verilere sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="378d4-140">Unlike references (values of reference types), pointers are not tracked by the garbage collector -- the garbage collector has no knowledge of pointers and the data to which they point.</span></span> <span data-ttu-id="378d4-141">Bu nedenle, bir işaretçiye başvuruya veya başvuruları içeren bir yapıya işaret eden bir işaretçi için bir *unmanaged_type*olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="378d4-141">For this reason a pointer is not permitted to point to a reference or to a struct that contains references, and the referent type of a pointer must be an *unmanaged_type*.</span></span>

<span data-ttu-id="378d4-142">*Unmanaged_type* , *reference_type* veya oluşturulmuş tür olmayan ve herhangi bir iç içe geçme düzeyinde *reference_type* veya oluşturulmuş tür alanları içermeyen bir türdür.</span><span class="sxs-lookup"><span data-stu-id="378d4-142">An *unmanaged_type* is any type that isn't a *reference_type* or constructed type, and doesn't contain *reference_type* or constructed type fields at any level of nesting.</span></span> <span data-ttu-id="378d4-143">Diğer bir deyişle, *unmanaged_type* aşağıdakilerden biridir:</span><span class="sxs-lookup"><span data-stu-id="378d4-143">In other words, an *unmanaged_type* is one of the following:</span></span>

*  <span data-ttu-id="378d4-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`veya `bool`.</span><span class="sxs-lookup"><span data-stu-id="378d4-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, or `bool`.</span></span>
*  <span data-ttu-id="378d4-145">Herhangi bir *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="378d4-145">Any *enum_type*.</span></span>
*  <span data-ttu-id="378d4-146">Herhangi bir *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="378d4-146">Any *pointer_type*.</span></span>
*  <span data-ttu-id="378d4-147">Oluşturulmuş bir tür olmayan ve yalnızca *unmanaged_type*s alanlarını içeren kullanıcı tanımlı *struct_type* .</span><span class="sxs-lookup"><span data-stu-id="378d4-147">Any user-defined *struct_type* that is not a constructed type and contains fields of *unmanaged_type*s only.</span></span>

<span data-ttu-id="378d4-148">İşaretçilerin ve başvuruların karıştırılmasına yönelik sezgisel kural, başvuru (nesneler) başvurularının, işaretçiler içermesine izin vermelerdir, ancak işaretçilerin başvurularının başvuru içermesine izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="378d4-148">The intuitive rule for mixing of pointers and references is that referents of references (objects) are permitted to contain pointers, but referents of pointers are not permitted to contain references.</span></span>

<span data-ttu-id="378d4-149">Aşağıdaki tabloda işaretçi türlerine bazı örnekler verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="378d4-149">Some examples of pointer types are given in the table below:</span></span>

| <span data-ttu-id="378d4-150">__Örnek__</span><span class="sxs-lookup"><span data-stu-id="378d4-150">__Example__</span></span> | <span data-ttu-id="378d4-151">__Açıklama__</span><span class="sxs-lookup"><span data-stu-id="378d4-151">__Description__</span></span>                               |
|-------------|-----------------------------------------------|
| `byte*`     | <span data-ttu-id="378d4-152">`byte` işaretçisi</span><span class="sxs-lookup"><span data-stu-id="378d4-152">Pointer to `byte`</span></span>                             |
| `char*`     | <span data-ttu-id="378d4-153">`char` işaretçisi</span><span class="sxs-lookup"><span data-stu-id="378d4-153">Pointer to `char`</span></span>                             |
| `int**`     | <span data-ttu-id="378d4-154">`int` işaretçisine işaretçi</span><span class="sxs-lookup"><span data-stu-id="378d4-154">Pointer to pointer to `int`</span></span>                   |
| `int*[]`    | <span data-ttu-id="378d4-155">`int` işaretçilerin tek boyutlu dizisi</span><span class="sxs-lookup"><span data-stu-id="378d4-155">Single-dimensional array of pointers to `int`</span></span> |
| `void*`     | <span data-ttu-id="378d4-156">Bilinmeyen tür işaretçisi</span><span class="sxs-lookup"><span data-stu-id="378d4-156">Pointer to unknown type</span></span>                       |

<span data-ttu-id="378d4-157">Belirli bir uygulama için, tüm işaretçi türleri aynı boyuta ve gösterimine sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="378d4-157">For a given implementation, all pointer types must have the same size and representation.</span></span>

<span data-ttu-id="378d4-158">C ve C++' nin aksine, aynı bildirimde C# birden çok işaretçi bildirildiğinde, `*` her işaretçi adında ön ek noktalama işareti olarak değil, yalnızca temel alınan türle birlikte yazılır.</span><span class="sxs-lookup"><span data-stu-id="378d4-158">Unlike C and C++, when multiple pointers are declared in the same declaration, in C# the `*` is written along with the underlying type only, not as a prefix punctuator on each pointer name.</span></span> <span data-ttu-id="378d4-159">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="378d4-159">For example</span></span>

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

<span data-ttu-id="378d4-160">Türü `T*` olan bir işaretçinin değeri, `T`türünde bir değişkenin adresini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="378d4-160">The value of a pointer having type `T*` represents the address of a variable of type `T`.</span></span> <span data-ttu-id="378d4-161">İşaretçi yöneltme işleci `*` ([işaretçi yöneltme](unsafe-code.md#pointer-indirection)) bu değişkene erişmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-161">The pointer indirection operator `*` ([Pointer indirection](unsafe-code.md#pointer-indirection)) may be used to access this variable.</span></span> <span data-ttu-id="378d4-162">Örneğin, `int*`türünde bir değişken `P` verildiğinde ifade `*P`, `P`bulunan adreste bulunan `int` değişkenini gösterir.</span><span class="sxs-lookup"><span data-stu-id="378d4-162">For example, given a variable `P` of type `int*`, the expression `*P` denotes the `int` variable found at the address contained in `P`.</span></span>

<span data-ttu-id="378d4-163">Bir nesne başvurusu gibi bir işaretçi de `null`olabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-163">Like an object reference, a pointer may be `null`.</span></span> <span data-ttu-id="378d4-164">`null` işaretçisine yöneltme işlecini uygulamak, uygulama tanımlı davranışa neden olur.</span><span class="sxs-lookup"><span data-stu-id="378d4-164">Applying the indirection operator to a `null` pointer results in implementation-defined behavior.</span></span> <span data-ttu-id="378d4-165">Değer `null` bir işaretçi, All-bit-sıfır ile temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-165">A pointer with value `null` is represented by all-bits-zero.</span></span>

<span data-ttu-id="378d4-166">`void*` türü, bilinmeyen bir türe yönelik bir işaretçi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="378d4-166">The `void*` type represents a pointer to an unknown type.</span></span> <span data-ttu-id="378d4-167">Ayrılan tür bilinmediği için, yöneltme işleci `void*`türünde bir işaretçiye uygulanamaz, ne de bu tür bir işaretçi üzerinde herhangi bir aritmetik işlem yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-167">Because the referent type is unknown, the indirection operator cannot be applied to a pointer of type `void*`, nor can any arithmetic be performed on such a pointer.</span></span> <span data-ttu-id="378d4-168">Ancak, `void*` bir işaretçi diğer işaretçi türlerine (ve tam tersi) dönüşebilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-168">However, a pointer of type `void*` can be cast to any other pointer type (and vice versa).</span></span>

<span data-ttu-id="378d4-169">İşaretçi türleri ayrı bir tür kategorisidir.</span><span class="sxs-lookup"><span data-stu-id="378d4-169">Pointer types are a separate category of types.</span></span> <span data-ttu-id="378d4-170">Başvuru türleri ve değer türlerinin aksine, işaretçi türleri `object` 'ten aktarılmaz ve işaretçi türleri ve `object`arasında dönüştürme yok.</span><span class="sxs-lookup"><span data-stu-id="378d4-170">Unlike reference types and value types, pointer types do not inherit from `object` and no conversions exist between pointer types and `object`.</span></span> <span data-ttu-id="378d4-171">Özellikle, kutulama ve kutudan çıkarma ([kutulama ve kutudan](types.md#boxing-and-unboxing)çıkarma) işaretçiler için desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="378d4-171">In particular, boxing and unboxing ([Boxing and unboxing](types.md#boxing-and-unboxing)) are not supported for pointers.</span></span> <span data-ttu-id="378d4-172">Ancak, farklı işaretçi türleri arasında ve işaretçi türleri ile integral türleri arasında dönüştürmeye izin verilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-172">However, conversions are permitted between different pointer types and between pointer types and the integral types.</span></span> <span data-ttu-id="378d4-173">Bu, [işaretçi dönüştürmelerinde](unsafe-code.md#pointer-conversions)açıklanır.</span><span class="sxs-lookup"><span data-stu-id="378d4-173">This is described in [Pointer conversions](unsafe-code.md#pointer-conversions).</span></span>

<span data-ttu-id="378d4-174">*Pointer_type* , tür bağımsız değişkeni olarak kullanılamaz ([oluşturulmuş türler](types.md#constructed-types)) ve tür çıkarımı ([tür çıkarımı](expressions.md#type-inference)), bir tür bağımsız değişkenini bir işaretçi türü olacak şekilde çıkartılan genel metot çağrılarında başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="378d4-174">A *pointer_type* cannot be used as a type argument ([Constructed types](types.md#constructed-types)), and type inference ([Type inference](expressions.md#type-inference)) fails on generic method calls that would have inferred a type argument to be a pointer type.</span></span>

<span data-ttu-id="378d4-175">Bir *pointer_type* geçici bir alanın türü olarak kullanılabilir ([geçici alanlar](classes.md#volatile-fields)).</span><span class="sxs-lookup"><span data-stu-id="378d4-175">A *pointer_type* may be used as the type of a volatile field ([Volatile fields](classes.md#volatile-fields)).</span></span>

<span data-ttu-id="378d4-176">İşaretçiler `ref` veya `out` parametresi olarak geçirilebilse de, işaretçi, çağrılan yöntem döndürüldüğünde veya işaret etmek için kullanılan sabit nesne artık sabitlenmemiş bir yerel değişkene işaret etmek üzere ayarlanabildiğinden, bu durum tanımsız davranışa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-176">Although pointers can be passed as `ref` or `out` parameters, doing so can cause undefined behavior, since the pointer may well be set to point to a local variable which no longer exists when the called method returns, or the fixed object to which it used to point, is no longer fixed.</span></span> <span data-ttu-id="378d4-177">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="378d4-177">For example:</span></span>

```csharp
using System;

class Test
{
    static int value = 20;

    unsafe static void F(out int* pi1, ref int* pi2) {
        int i = 10;
        pi1 = &i;

        fixed (int* pj = &value) {
            // ...
            pi2 = pj;
        }
    }

    static void Main() {
        int i = 10;
        unsafe {
            int* px1;
            int* px2 = &i;

            F(out px1, ref px2);

            Console.WriteLine("*px1 = {0}, *px2 = {1}",
                *px1, *px2);    // undefined behavior
        }
    }
}
```

<span data-ttu-id="378d4-178">Bir yöntem bir tür değeri döndürebilir ve bu tür bir işaretçi olabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-178">A method can return a value of some type, and that type can be a pointer.</span></span> <span data-ttu-id="378d4-179">Örneğin, `int`s bitişik dizisine işaretçi verildiğinde, bu dizinin öğe sayısı ve başka bir `int` değeri olduğunda, bir eşleşme meydana gelirse, aşağıdaki yöntem o dizideki bu değerin adresini döndürür; Aksi takdirde `null`döndürür:</span><span class="sxs-lookup"><span data-stu-id="378d4-179">For example, when given a pointer to a contiguous sequence of `int`s, that sequence's element count, and some other `int` value, the following method returns the address of that value in that sequence, if a match occurs; otherwise it returns `null`:</span></span>

```csharp
unsafe static int* Find(int* pi, int size, int value) {
    for (int i = 0; i < size; ++i) {
        if (*pi == value) 
            return pi;
        ++pi;
    }
    return null;
}
```

<span data-ttu-id="378d4-180">Güvenli olmayan bir bağlamda, işaretçilerde çalıştırmak için birkaç yapı mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="378d4-180">In an unsafe context, several constructs are available for operating on pointers:</span></span>

*  <span data-ttu-id="378d4-181">`*` işleci, işaretçi yöneltme ([işaretçi yöneltme](unsafe-code.md#pointer-indirection)) yapmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-181">The `*` operator may be used to perform pointer indirection ([Pointer indirection](unsafe-code.md#pointer-indirection)).</span></span>
*  <span data-ttu-id="378d4-182">`->` işleci, bir yapının üyesine işaretçi aracılığıyla erişmek için kullanılabilir ([işaretçi üyesi erişimi](unsafe-code.md#pointer-member-access)).</span><span class="sxs-lookup"><span data-stu-id="378d4-182">The `->` operator may be used to access a member of a struct through a pointer ([Pointer member access](unsafe-code.md#pointer-member-access)).</span></span>
*  <span data-ttu-id="378d4-183">`[]` işleci bir işaretçiye dizin eklemek için kullanılabilir ([işaretçi öğesi erişimi](unsafe-code.md#pointer-element-access)).</span><span class="sxs-lookup"><span data-stu-id="378d4-183">The `[]` operator may be used to index a pointer ([Pointer element access](unsafe-code.md#pointer-element-access)).</span></span>
*  <span data-ttu-id="378d4-184">`&` işleci, bir değişkenin adresini ([Adres operatörü](unsafe-code.md#the-address-of-operator)) almak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-184">The `&` operator may be used to obtain the address of a variable ([The address-of operator](unsafe-code.md#the-address-of-operator)).</span></span>
*  <span data-ttu-id="378d4-185">`++` ve `--` işleçleri, işaretçileri artırmak ve azaltmak için kullanılabilir ([işaretçi artışı ve azalması](unsafe-code.md#pointer-increment-and-decrement)).</span><span class="sxs-lookup"><span data-stu-id="378d4-185">The `++` and `--` operators may be used to increment and decrement pointers ([Pointer increment and decrement](unsafe-code.md#pointer-increment-and-decrement)).</span></span>
*  <span data-ttu-id="378d4-186">`+` ve `-` işleçleri, işaretçi aritmetiği ([işaretçi aritmetiği](unsafe-code.md#pointer-arithmetic)) gerçekleştirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-186">The `+` and `-` operators may be used to perform pointer arithmetic ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span>
*  <span data-ttu-id="378d4-187">`==`, `!=`, `<`, `>`, `<=`ve `=>` işleçleri, işaretçileri ([işaretçi karşılaştırması](unsafe-code.md#pointer-comparison)) karşılaştırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-187">The `==`, `!=`, `<`, `>`, `<=`, and `=>` operators may be used to compare pointers ([Pointer comparison](unsafe-code.md#pointer-comparison)).</span></span>
*  <span data-ttu-id="378d4-188">`stackalloc` işleci, çağrı yığınından ([sabit boyutlu arabellekler](unsafe-code.md#fixed-size-buffers)) bellek ayırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-188">The `stackalloc` operator may be used to allocate memory from the call stack ([Fixed size buffers](unsafe-code.md#fixed-size-buffers)).</span></span>
*  <span data-ttu-id="378d4-189">`fixed` deyimleri, bir değişkeni geçici olarak çözmek için kullanılabilir ve bu nedenle adresini elde edebilir ([fixed deyimidir](unsafe-code.md#the-fixed-statement)).</span><span class="sxs-lookup"><span data-stu-id="378d4-189">The `fixed` statement may be used to temporarily fix a variable so its address can be obtained ([The fixed statement](unsafe-code.md#the-fixed-statement)).</span></span>

## <a name="fixed-and-moveable-variables"></a><span data-ttu-id="378d4-190">Sabit ve taşınabilir değişkenler</span><span class="sxs-lookup"><span data-stu-id="378d4-190">Fixed and moveable variables</span></span>

<span data-ttu-id="378d4-191">Address-of işleci ([Address-of işleci](unsafe-code.md#the-address-of-operator)) ve `fixed` deyimin ([fixed deyimin](unsafe-code.md#the-fixed-statement)) değişkenleri Iki kategoriye böler: ***sabit değişkenler*** ve ***Taşınabilir değişkenler***.</span><span class="sxs-lookup"><span data-stu-id="378d4-191">The address-of operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) and the `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) divide variables into two categories: ***Fixed variables*** and ***moveable variables***.</span></span>

<span data-ttu-id="378d4-192">Sabit değişkenler çöp toplayıcı 'nin işleminden etkilenmeyen depolama konumlarında bulunur.</span><span class="sxs-lookup"><span data-stu-id="378d4-192">Fixed variables reside in storage locations that are unaffected by operation of the garbage collector.</span></span> <span data-ttu-id="378d4-193">(Sabit değişkenlere örnek olarak, yerel değişkenler, değer parametreleri ve başvuru işaretçileri tarafından oluşturulan değişkenler verilebilir.) Diğer yandan, taşınabilir değişkenler çöp toplayıcısının yerini değiştirme veya aktiften çıkarma konusunda yer alan depolama konumlarında bulunur.</span><span class="sxs-lookup"><span data-stu-id="378d4-193">(Examples of fixed variables include local variables, value parameters, and variables created by dereferencing pointers.) On the other hand, moveable variables reside in storage locations that are subject to relocation or disposal by the garbage collector.</span></span> <span data-ttu-id="378d4-194">(Taşınamayacak değişkenlerin örnekleri, nesne ve dizi öğelerindeki alanları içerir.)</span><span class="sxs-lookup"><span data-stu-id="378d4-194">(Examples of moveable variables include fields in objects and elements of arrays.)</span></span>

<span data-ttu-id="378d4-195">`&` işleci ([Adres operatörü](unsafe-code.md#the-address-of-operator)), sabit bir değişkenin adresinin kısıtlama olmadan elde edilebilir olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="378d4-195">The `&` operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) permits the address of a fixed variable to be obtained without restrictions.</span></span> <span data-ttu-id="378d4-196">Ancak, taşınabilir bir değişken çöp toplayıcısının yeniden konumlandırılmasını veya aktiften çıkarılma tabi olduğundan, taşınabilir bir değişkenin adresi yalnızca bir `fixed` ekstresi ([fixed ifadesiyle](unsafe-code.md#the-fixed-statement)) kullanılarak elde edilebilir ve bu adres yalnızca o `fixed` deyimin süresi boyunca geçerli kalır.</span><span class="sxs-lookup"><span data-stu-id="378d4-196">However, because a moveable variable is subject to relocation or disposal by the garbage collector, the address of a moveable variable can only be obtained using a `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)), and that address remains valid only for the duration of that `fixed` statement.</span></span>

<span data-ttu-id="378d4-197">Kesin koşullarda, sabit bir değişken aşağıdakilerden biridir:</span><span class="sxs-lookup"><span data-stu-id="378d4-197">In precise terms, a fixed variable is one of the following:</span></span>

*  <span data-ttu-id="378d4-198">Değişken anonim bir işlev tarafından yakalanmadığı takdirde, bir yerel değişkene veya bir değer parametresine başvuran *simple_name* ([basit adlar](expressions.md#simple-names)) sonucu.</span><span class="sxs-lookup"><span data-stu-id="378d4-198">A variable resulting from a *simple_name* ([Simple names](expressions.md#simple-names)) that refers to a local variable or a value parameter, unless the variable is captured by an anonymous function.</span></span>
*  <span data-ttu-id="378d4-199">`V` sabit *struct_type*bir değişken olduğu, form `V.I`*member_access* ([üye erişimi](expressions.md#member-access)) sonucu olan bir değişken.</span><span class="sxs-lookup"><span data-stu-id="378d4-199">A variable resulting from a *member_access* ([Member access](expressions.md#member-access)) of the form `V.I`, where `V` is a fixed variable of a *struct_type*.</span></span>
*  <span data-ttu-id="378d4-200">Form `*P`*pointer_indirection_expression* ([işaretçi yöneltme](unsafe-code.md#pointer-indirection)), form `P->I`*pointer_member_access* (işaretçi[üyesi erişimi](unsafe-code.md#pointer-member-access)) *veya pointer_element_access form `P[E]`(* [işaretçi öğesi erişimi](unsafe-code.md#pointer-element-access)) sonucu.</span><span class="sxs-lookup"><span data-stu-id="378d4-200">A variable resulting from a *pointer_indirection_expression* ([Pointer indirection](unsafe-code.md#pointer-indirection)) of the form `*P`, a *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) of the form `P->I`, or a *pointer_element_access* ([Pointer element access](unsafe-code.md#pointer-element-access)) of the form `P[E]`.</span></span>

<span data-ttu-id="378d4-201">Diğer tüm değişkenler taşınabilir değişkenler olarak sınıflandırılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="378d4-201">All other variables are classified as moveable variables.</span></span>

<span data-ttu-id="378d4-202">Statik bir alanın taşınamayacak değişken olarak sınıflandırıldığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="378d4-202">Note that a static field is classified as a moveable variable.</span></span> <span data-ttu-id="378d4-203">Ayrıca, parametre için verilen bağımsız değişken sabit bir değişken olsa da, bir `ref` veya `out` parametresinin taşınabilir bir değişken olarak sınıflandırıldığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="378d4-203">Also note that a `ref` or `out` parameter is classified as a moveable variable, even if the argument given for the parameter is a fixed variable.</span></span> <span data-ttu-id="378d4-204">Son olarak, bir işaretçinin başvurusunun kaldırılması tarafından oluşturulan bir değişkenin her zaman sabit bir değişken olarak sınıflandırıldığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="378d4-204">Finally, note that a variable produced by dereferencing a pointer is always classified as a fixed variable.</span></span>

## <a name="pointer-conversions"></a><span data-ttu-id="378d4-205">İşaretçi Dönüştürmeler</span><span class="sxs-lookup"><span data-stu-id="378d4-205">Pointer conversions</span></span>

<span data-ttu-id="378d4-206">Güvenli olmayan bir bağlamda, kullanılabilir örtük dönüştürmeler ([örtük dönüştürmeler](conversions.md#implicit-conversions)) kümesi aşağıdaki örtük işaretçi dönüşümlerini içerecek şekilde genişletilir:</span><span class="sxs-lookup"><span data-stu-id="378d4-206">In an unsafe context, the set of available implicit conversions ([Implicit conversions](conversions.md#implicit-conversions)) is extended to include the following implicit pointer conversions:</span></span>

*  <span data-ttu-id="378d4-207">*Pointer_type* `void*`türüne yazın.</span><span class="sxs-lookup"><span data-stu-id="378d4-207">From any *pointer_type* to the type `void*`.</span></span>
*  <span data-ttu-id="378d4-208">`null` değişmez değerinden *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="378d4-208">From the `null` literal to any *pointer_type*.</span></span>

<span data-ttu-id="378d4-209">Ayrıca, güvenli olmayan bir bağlamda, kullanılabilir açık dönüştürmeler ([Açık dönüştürmeler](conversions.md#explicit-conversions)) kümesi aşağıdaki açık işaretçi dönüşümlerini içerecek şekilde genişletilir:</span><span class="sxs-lookup"><span data-stu-id="378d4-209">Additionally, in an unsafe context, the set of available explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) is extended to include the following explicit pointer conversions:</span></span>

*  <span data-ttu-id="378d4-210">Herhangi bir *pointer_type* başka bir *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="378d4-210">From any *pointer_type* to any other *pointer_type*.</span></span>
*  <span data-ttu-id="378d4-211">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`veya `ulong` herhangi bir *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="378d4-211">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong` to any *pointer_type*.</span></span>
*  <span data-ttu-id="378d4-212">Herhangi bir *pointer_type* `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`veya `ulong`.</span><span class="sxs-lookup"><span data-stu-id="378d4-212">From any *pointer_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="378d4-213">Son olarak, güvenli olmayan bir bağlamda, standart örtük dönüştürmeler kümesi ([Standart örtük dönüştürmeler](conversions.md#standard-implicit-conversions)) aşağıdaki işaretçi dönüşümünü içerir:</span><span class="sxs-lookup"><span data-stu-id="378d4-213">Finally, in an unsafe context, the set of standard implicit conversions ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) includes the following pointer conversion:</span></span>

*  <span data-ttu-id="378d4-214">*Pointer_type* `void*`türüne yazın.</span><span class="sxs-lookup"><span data-stu-id="378d4-214">From any *pointer_type* to the type `void*`.</span></span>

<span data-ttu-id="378d4-215">İki işaretçi türü arasındaki dönüşümler hiçbir şekilde gerçek işaretçi değerini değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="378d4-215">Conversions between two pointer types never change the actual pointer value.</span></span> <span data-ttu-id="378d4-216">Diğer bir deyişle, bir işaretçi türünden diğerine dönüştürmenin, işaretçi tarafından verilen temel adres üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="378d4-216">In other words, a conversion from one pointer type to another has no effect on the underlying address given by the pointer.</span></span>

<span data-ttu-id="378d4-217">Bir işaretçi türü diğerine dönüştürüldüğünde, ortaya çıkan işaretçi, işaret türü için doğru hizalanmazsa, sonuç başvuru başvurusu varsa, bu davranış tanımsızdır.</span><span class="sxs-lookup"><span data-stu-id="378d4-217">When one pointer type is converted to another, if the resulting pointer is not correctly aligned for the pointed-to type, the behavior is undefined if the result is dereferenced.</span></span> <span data-ttu-id="378d4-218">Genel olarak, "doğru hizalı" kavram geçişlidir: tür `A` bir işaretçi, `B`türüne yönelik bir işaretçi için doğru hizalanmışsa, bu, sırasıyla `C`türü bir işaretçi için doğru şekilde hizalanmışsa, `A` türüne yönelik bir işaretçi, `C`türünde bir işaretçi için doğru şekilde hizalanmıştır.</span><span class="sxs-lookup"><span data-stu-id="378d4-218">In general, the concept "correctly aligned" is transitive: if a pointer to type `A` is correctly aligned for a pointer to type `B`, which, in turn, is correctly aligned for a pointer to type `C`, then a pointer to type `A` is correctly aligned for a pointer to type `C`.</span></span>

<span data-ttu-id="378d4-219">Farklı bir türe yönelik bir işaretçi aracılığıyla bir türe sahip bir değişkene erişildiği aşağıdaki durumu göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="378d4-219">Consider the following case in which a variable having one type is accessed via a pointer to a different type:</span></span>

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

<span data-ttu-id="378d4-220">Bir işaretçi türü bayta bir işaretçiye dönüştürüldüğünde, sonuç değişkenin en düşük adresli bayta işaret eder.</span><span class="sxs-lookup"><span data-stu-id="378d4-220">When a pointer type is converted to a pointer to byte, the result points to the lowest addressed byte of the variable.</span></span> <span data-ttu-id="378d4-221">Sonucun ardışık artışlarına kadar, değişkenin boyutuna kadar, bu değişkenin kalan baytlarına kadar dönen işaretçiler.</span><span class="sxs-lookup"><span data-stu-id="378d4-221">Successive increments of the result, up to the size of the variable, yield pointers to the remaining bytes of that variable.</span></span> <span data-ttu-id="378d4-222">Örneğin, aşağıdaki yöntem bir Double içindeki sekiz baytın her birini onaltılık bir değer olarak görüntüler:</span><span class="sxs-lookup"><span data-stu-id="378d4-222">For example, the following method displays each of the eight bytes in a double as a hexadecimal value:</span></span>

```csharp
using System;

class Test
{
    unsafe static void Main() {
      double d = 123.456e23;
        unsafe {
           byte* pb = (byte*)&d;
            for (int i = 0; i < sizeof(double); ++i)
               Console.Write("{0:X2} ", *pb++);
            Console.WriteLine();
        }
    }
}
```

<span data-ttu-id="378d4-223">Tabii ki, üretilen çıkış, bitime 'ye bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="378d4-223">Of course, the output produced depends on endianness.</span></span>

<span data-ttu-id="378d4-224">İşaretçiler ve tamsayılar arasındaki eşlemeler uygulama tanımlı.</span><span class="sxs-lookup"><span data-stu-id="378d4-224">Mappings between pointers and integers are implementation-defined.</span></span> <span data-ttu-id="378d4-225">Bununla birlikte, doğrusal bir adres alanı ile 32 \* ve 64 bit CPU mimarilerinde, tam sayı türlerine veya bu tür bir değere yapılan işaretçilerin dönüştürmeleri genellikle `uint` ya da `ulong` değerlerinin, sırasıyla, bu integral türlerine veya bu tür dönüştürmeleri gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="378d4-225">However, on 32\* and 64-bit CPU architectures with a linear address space, conversions of pointers to or from integral types typically behave exactly like conversions of `uint` or `ulong` values, respectively, to or from those integral types.</span></span>

### <a name="pointer-arrays"></a><span data-ttu-id="378d4-226">İşaretçi dizileri</span><span class="sxs-lookup"><span data-stu-id="378d4-226">Pointer arrays</span></span>

<span data-ttu-id="378d4-227">Güvenli olmayan bir bağlamda, işaretçiler dizileri oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-227">In an unsafe context, arrays of pointers can be constructed.</span></span> <span data-ttu-id="378d4-228">İşaretçi dizileri üzerinde yalnızca diğer dizi türleri için uygulanan Dönüştürmelere izin verilir:</span><span class="sxs-lookup"><span data-stu-id="378d4-228">Only some of the conversions that apply to other array types are allowed on pointer arrays:</span></span>

*  <span data-ttu-id="378d4-229">Herhangi bir *array_type* `System.Array` ve onun uyguladığı arayüzlerden örtük başvuru dönüştürmesi ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions)), işaretçi dizileri için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="378d4-229">The implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) from any *array_type* to `System.Array` and the interfaces it implements also applies to pointer arrays.</span></span> <span data-ttu-id="378d4-230">Ancak, dizi öğelerine `System.Array` veya uyguladığı arabirimler aracılığıyla erişmek için herhangi bir girişim, çalışma zamanında bir özel durum oluşmasına neden olur, çünkü işaretçi türleri `object`dönüştürülebilir değildir.</span><span class="sxs-lookup"><span data-stu-id="378d4-230">However, any attempt to access the array elements through `System.Array` or the interfaces it implements will result in an exception at run-time, as pointer types are not convertible to `object`.</span></span>
*  <span data-ttu-id="378d4-231">Tek boyutlu dizi türünden `System.Collections.Generic.IList<T>` ve genel temel arabirimlerine `S[]` örtük ve açık başvuru dönüştürmeleri ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions), [açık başvuru dönüştürmeleri](conversions.md#explicit-reference-conversions)) hiçbir şekilde hiçbir şekilde uygulanmaz çünkü işaretçi türleri tür bağımsız değişkeni olarak kullanılamaz ve işaretçi türlerinden işaretçi olmayan türlere dönüştürme yok.</span><span class="sxs-lookup"><span data-stu-id="378d4-231">The implicit and explicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions), [Explicit reference conversions](conversions.md#explicit-reference-conversions)) from a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its generic base interfaces never apply to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>
*  <span data-ttu-id="378d4-232">`System.Array` ve *array_type* uyguladığı arabirimlerin açık başvuru dönüştürmesi ([açık başvuru dönüştürmeleri](conversions.md#explicit-reference-conversions)), işaretçi dizileri için geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="378d4-232">The explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Array` and the interfaces it implements to any *array_type* applies to pointer arrays.</span></span>
*  <span data-ttu-id="378d4-233">`System.Collections.Generic.IList<S>` ve temel arabirimlerinden bir tek boyutlu dizi türüne `T[]` açık başvuru dönüştürmeleri ([açık başvuru dönüştürmeleri](conversions.md#explicit-reference-conversions)), işaretçi türleri tür bağımsız değişkenleri olarak kullanılamadığından ve işaretçi türlerinden işaretçi olmayan türlere dönüştürme olmadığı için hiçbir şekilde işaretçi dizileri için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="378d4-233">The explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]` never applies to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>

<span data-ttu-id="378d4-234">Bu kısıtlamalar, [foreach ifadesinde](statements.md#the-foreach-statement) açıklanan diziler üzerinde `foreach` bildirimine yönelik genişletmenin işaretçi dizilerine uygulanamadığını ifade ediyor.</span><span class="sxs-lookup"><span data-stu-id="378d4-234">These restrictions mean that the expansion for the `foreach` statement over arrays described in [The foreach statement](statements.md#the-foreach-statement) cannot be applied to pointer arrays.</span></span> <span data-ttu-id="378d4-235">Bunun yerine, formun foreach ifadesi</span><span class="sxs-lookup"><span data-stu-id="378d4-235">Instead, a foreach statement of the form</span></span>

```csharp
foreach (V v in x) embedded_statement
```

<span data-ttu-id="378d4-236">`x` türü, form `T[,,...,]`bir dizi türü olduğunda, `N` boyut sayısı eksi 1 ve `T` veya `V` bir işaretçi türü, iç içe for-döngüleri kullanılarak aşağıdaki gibi genişletilir:</span><span class="sxs-lookup"><span data-stu-id="378d4-236">where the type of `x` is an array type of the form `T[,,...,]`, `N` is the number of dimensions minus 1 and `T` or `V` is a pointer type, is expanded using nested for-loops as follows:</span></span>

```csharp
{
    T[,,...,] a = x;
    for (int i0 = a.GetLowerBound(0); i0 <= a.GetUpperBound(0); i0++)
    for (int i1 = a.GetLowerBound(1); i1 <= a.GetUpperBound(1); i1++)
    ...
    for (int iN = a.GetLowerBound(N); iN <= a.GetUpperBound(N); iN++) {
        V v = (V)a.GetValue(i0,i1,...,iN);
        embedded_statement
    }
}
```

<span data-ttu-id="378d4-237">`a`, `i0`, `i1`,..., `iN` değişkenler, `x` veya *embedded_statement* ya da programın herhangi bir kaynak kodu için görünür veya erişilebilir değildir.</span><span class="sxs-lookup"><span data-stu-id="378d4-237">The variables `a`, `i0`, `i1`, ..., `iN` are not visible to or accessible to `x` or the *embedded_statement* or any other source code of the program.</span></span> <span data-ttu-id="378d4-238">`v` değişkeni, gömülü ifadede salt okunurdur.</span><span class="sxs-lookup"><span data-stu-id="378d4-238">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="378d4-239">`T` (öğe türü) öğesinden `V`bir açık dönüştürme ([işaretçi dönüşümleri](unsafe-code.md#pointer-conversions)) yoksa, bir hata oluşturulur ve başka bir adım alınmaz.</span><span class="sxs-lookup"><span data-stu-id="378d4-239">If there is not an explicit conversion ([Pointer conversions](unsafe-code.md#pointer-conversions)) from `T` (the element type) to `V`, an error is produced and no further steps are taken.</span></span> <span data-ttu-id="378d4-240">`x` `null`değeri varsa, çalışma zamanında bir `System.NullReferenceException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="378d4-240">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

## <a name="pointers-in-expressions"></a><span data-ttu-id="378d4-241">İfadelerdeki işaretçiler</span><span class="sxs-lookup"><span data-stu-id="378d4-241">Pointers in expressions</span></span>

<span data-ttu-id="378d4-242">Güvenli olmayan bir bağlamda, bir ifade bir işaretçi türü sonucunu verebilir, ancak güvenli olmayan bir bağlam dışında bir ifadenin işaretçi türü olması için derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="378d4-242">In an unsafe context, an expression may yield a result of a pointer type, but outside an unsafe context it is a compile-time error for an expression to be of a pointer type.</span></span> <span data-ttu-id="378d4-243">Kesin koşullarda, güvenli olmayan bir bağlam dışında, bir *simple_name* ([basit adlar](expressions.md#simple-names)), *member_access* ([üye erişimi](expressions.md#member-access)), *invocation_expression* ([çağırma ifadeleri](expressions.md#invocation-expressions)) veya *element_access* ([öğe erişimi](expressions.md#element-access)) bir işaretçi türü ise, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="378d4-243">In precise terms, outside an unsafe context a compile-time error occurs if any *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)), or *element_access* ([Element access](expressions.md#element-access)) is of a pointer type.</span></span>

<span data-ttu-id="378d4-244">Güvenli olmayan bir bağlamda *primary_no_array_creation_expression* ([birincil ifadeler](expressions.md#primary-expressions)) ve *unary_expression* ([Birli İşleçler](expressions.md#unary-operators)) üretimleri aşağıdaki ek yapılara izin verir:</span><span class="sxs-lookup"><span data-stu-id="378d4-244">In an unsafe context, the *primary_no_array_creation_expression* ([Primary expressions](expressions.md#primary-expressions)) and *unary_expression* ([Unary operators](expressions.md#unary-operators)) productions permit the following additional constructs:</span></span>

```antlr
primary_no_array_creation_expression_unsafe
    : pointer_member_access
    | pointer_element_access
    | sizeof_expression
    ;

unary_expression_unsafe
    : pointer_indirection_expression
    | addressof_expression
    ;
```

<span data-ttu-id="378d4-245">Bu yapılar aşağıdaki bölümlerde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="378d4-245">These constructs are described in the following sections.</span></span> <span data-ttu-id="378d4-246">Güvenli olmayan işleçlerin önceliği ve ilişkilendirilebilirliği, dilbilgisi tarafından kapsanıyor.</span><span class="sxs-lookup"><span data-stu-id="378d4-246">The precedence and associativity of the unsafe operators is implied by the grammar.</span></span>

### <a name="pointer-indirection"></a><span data-ttu-id="378d4-247">İşaretçi yöneltme</span><span class="sxs-lookup"><span data-stu-id="378d4-247">Pointer indirection</span></span>

<span data-ttu-id="378d4-248">*Pointer_indirection_expression* , bir yıldız işareti (`*`) ve ardından bir *unary_expression*oluşur.</span><span class="sxs-lookup"><span data-stu-id="378d4-248">A *pointer_indirection_expression* consists of an asterisk (`*`) followed by a *unary_expression*.</span></span>

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

<span data-ttu-id="378d4-249">Birli `*` işleci işaretçi yöneltme 'yi gösterir ve bir işaretçinin işaret ettiği değişkeni elde etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="378d4-249">The unary `*` operator denotes pointer indirection and is used to obtain the variable to which a pointer points.</span></span> <span data-ttu-id="378d4-250">`*P`değerlendirilme sonucu, `P` işaretçi türü `T*`bir ifadesiyse, `T`türünde bir değişkendir.</span><span class="sxs-lookup"><span data-stu-id="378d4-250">The result of evaluating `*P`, where `P` is an expression of a pointer type `T*`, is a variable of type `T`.</span></span> <span data-ttu-id="378d4-251">Birli `*` işlecini `void*` türünde bir ifadeye veya işaretçi türünde olmayan bir ifadeye uygulamak için derleme zamanı hatası.</span><span class="sxs-lookup"><span data-stu-id="378d4-251">It is a compile-time error to apply the unary `*` operator to an expression of type `void*` or to an expression that isn't of a pointer type.</span></span>

<span data-ttu-id="378d4-252">Birli `*` işlecini bir `null` işaretçisine uygulamanın etkisi, uygulama tanımlı ' dır.</span><span class="sxs-lookup"><span data-stu-id="378d4-252">The effect of applying the unary `*` operator to a `null` pointer is implementation-defined.</span></span> <span data-ttu-id="378d4-253">Özellikle, bu işlemin bir `System.NullReferenceException`oluşturduğunda garanti yoktur.</span><span class="sxs-lookup"><span data-stu-id="378d4-253">In particular, there is no guarantee that this operation throws a `System.NullReferenceException`.</span></span>

<span data-ttu-id="378d4-254">İşaretçiye geçersiz bir değer atanmışsa, birli `*` işlecinin davranışı tanımsızdır.</span><span class="sxs-lookup"><span data-stu-id="378d4-254">If an invalid value has been assigned to the pointer, the behavior of the unary `*` operator is undefined.</span></span> <span data-ttu-id="378d4-255">Birli `*` işleci tarafından bir işaretçinin başvurusunun kaldırılması için geçersiz değerler arasında, işaret edilen tür için uygun şekilde hizalı bir adrestir (bkz. [işaretçi dönüştürmelerinde](unsafe-code.md#pointer-conversions)örnek) ve yaşam süresinin sonundan sonraki bir değişkenin adresi.</span><span class="sxs-lookup"><span data-stu-id="378d4-255">Among the invalid values for dereferencing a pointer by the unary `*` operator are an address inappropriately aligned for the type pointed to (see example in [Pointer conversions](unsafe-code.md#pointer-conversions)), and the address of a variable after the end of its lifetime.</span></span>

<span data-ttu-id="378d4-256">Kesin atama analizinin amaçları doğrultusunda, form `*P` bir ifade hesaplanarak üretilen bir değişken başlangıçta atanan ([Başlangıçta atanan değişkenler](variables.md#initially-assigned-variables)) olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-256">For purposes of definite assignment analysis, a variable produced by evaluating an expression of the form `*P` is considered initially assigned ([Initially assigned variables](variables.md#initially-assigned-variables)).</span></span>

### <a name="pointer-member-access"></a><span data-ttu-id="378d4-257">İşaretçi üye erişimi</span><span class="sxs-lookup"><span data-stu-id="378d4-257">Pointer member access</span></span>

<span data-ttu-id="378d4-258">Bir *pointer_member_access* *primary_expression*ve ardından bir "`->`" belirteci ve ardından bir *tanımlayıcı* ve isteğe bağlı bir *type_argument_list*oluşur.</span><span class="sxs-lookup"><span data-stu-id="378d4-258">A *pointer_member_access* consists of a *primary_expression*, followed by a "`->`" token, followed by an *identifier* and an optional *type_argument_list*.</span></span>

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

<span data-ttu-id="378d4-259">Bir işaretçi üye `P->I`, `void*`dışında bir işaretçi türünün bir ifadesi olmalıdır ve `I`, `P` işaret eden `P` bir erişilebilir üyeyi belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="378d4-259">In a pointer member access of the form `P->I`, `P` must be an expression of a pointer type other than `void*`, and `I` must denote an accessible member of the type to which `P` points.</span></span>

<span data-ttu-id="378d4-260">`P->I` form işaretçisi üye erişimi, tam olarak `(*P).I`olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-260">A pointer member access of the form `P->I` is evaluated exactly as `(*P).I`.</span></span> <span data-ttu-id="378d4-261">İşaretçi yöneltme işlecinin (`*`) bir açıklaması için bkz. [işaretçi yöneltme](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="378d4-261">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="378d4-262">Üye erişim işlecinin (`.`) bir açıklaması için bkz. [üye erişimi](expressions.md#member-access).</span><span class="sxs-lookup"><span data-stu-id="378d4-262">For a description of the member access operator (`.`), see [Member access](expressions.md#member-access).</span></span>

<span data-ttu-id="378d4-263">örnekte</span><span class="sxs-lookup"><span data-stu-id="378d4-263">In the example</span></span>

```csharp
using System;

struct Point
{
    public int x;
    public int y;

    public override string ToString() {
        return "(" + x + "," + y + ")";
    }
}

class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            p->x = 10;
            p->y = 20;
            Console.WriteLine(p->ToString());
        }
    }
}
```

<span data-ttu-id="378d4-264">`->` işleci, alanlara erişmek ve bir yapı yöntemini işaretçi aracılığıyla çağırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="378d4-264">the `->` operator is used to access fields and invoke a method of a struct through a pointer.</span></span> <span data-ttu-id="378d4-265">İşlem `P->I` tam olarak `(*P).I`eşdeğer olduğundan, `Main` yöntemi eşit olarak da yazılabilir:</span><span class="sxs-lookup"><span data-stu-id="378d4-265">Because the operation `P->I` is precisely equivalent to `(*P).I`, the `Main` method could equally well have been written:</span></span>

```csharp
class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            (*p).x = 10;
            (*p).y = 20;
            Console.WriteLine((*p).ToString());
        }
    }
}
```

### <a name="pointer-element-access"></a><span data-ttu-id="378d4-266">İşaretçi öğesi erişimi</span><span class="sxs-lookup"><span data-stu-id="378d4-266">Pointer element access</span></span>

<span data-ttu-id="378d4-267">Bir *pointer_element_access* , ardından "`[`" ve "`]`" içinde olan bir ifade tarafından izlenen bir *primary_no_array_creation_expression* oluşur.</span><span class="sxs-lookup"><span data-stu-id="378d4-267">A *pointer_element_access* consists of a *primary_no_array_creation_expression* followed by an expression enclosed in "`[`" and "`]`".</span></span>

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

<span data-ttu-id="378d4-268">`P[E]`bir işaretçi öğesinde, `P` `void*`dışında bir işaretçi türünün bir ifadesi olmalıdır ve `E` örtük olarak `int`, `uint`, `long`veya `ulong`olarak dönüştürülebileceği bir ifade olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="378d4-268">In a pointer element access of the form `P[E]`, `P` must be an expression of a pointer type other than `void*`, and `E` must be an expression that can be implicitly converted to `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="378d4-269">`P[E]` form işaretçisi öğesi erişimi, tam olarak `*(P + E)`olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-269">A pointer element access of the form `P[E]` is evaluated exactly as `*(P + E)`.</span></span> <span data-ttu-id="378d4-270">İşaretçi yöneltme işlecinin (`*`) bir açıklaması için bkz. [işaretçi yöneltme](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="378d4-270">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="378d4-271">İşaretçi ek işlecinin (`+`) bir açıklaması için bkz. [işaretçi aritmetiği](unsafe-code.md#pointer-arithmetic).</span><span class="sxs-lookup"><span data-stu-id="378d4-271">For a description of the pointer addition operator (`+`), see [Pointer arithmetic](unsafe-code.md#pointer-arithmetic).</span></span>

<span data-ttu-id="378d4-272">örnekte</span><span class="sxs-lookup"><span data-stu-id="378d4-272">In the example</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) p[i] = (char)i;
        }
    }
}
```

<span data-ttu-id="378d4-273">bir işaretçi öğesi erişimi, bir `for` döngüsünde karakter arabelleğini başlatmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="378d4-273">a pointer element access is used to initialize the character buffer in a `for` loop.</span></span> <span data-ttu-id="378d4-274">İşlem `P[E]` tam olarak `*(P + E)`eşdeğer olduğundan, örnek de eşit olarak yazılabilir:</span><span class="sxs-lookup"><span data-stu-id="378d4-274">Because the operation `P[E]` is precisely equivalent to `*(P + E)`, the example could equally well have been written:</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) *(p + i) = (char)i;
        }
    }
}
```

<span data-ttu-id="378d4-275">İşaretçi öğesi erişim işleci, sınırların dışı hataları denetlemez ve bir sınır dışı öğeye erişirken bu davranış tanımlı değildir.</span><span class="sxs-lookup"><span data-stu-id="378d4-275">The pointer element access operator does not check for out-of-bounds errors and the behavior when accessing an out-of-bounds element is undefined.</span></span> <span data-ttu-id="378d4-276">Bu, C ve ile C++aynıdır.</span><span class="sxs-lookup"><span data-stu-id="378d4-276">This is the same as C and C++.</span></span>

### <a name="the-address-of-operator"></a><span data-ttu-id="378d4-277">Address-of işleci</span><span class="sxs-lookup"><span data-stu-id="378d4-277">The address-of operator</span></span>

<span data-ttu-id="378d4-278">*Addressof_expression* , ve sonrasında *unary_expression*gelen bir ve işareti (`&`) oluşur.</span><span class="sxs-lookup"><span data-stu-id="378d4-278">An *addressof_expression* consists of an ampersand (`&`) followed by a *unary_expression*.</span></span>

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

<span data-ttu-id="378d4-279">Bir tür `T` olan ve sabit bir değişken ([sabit ve taşınabilir değişkenler](unsafe-code.md#fixed-and-moveable-variables)) olarak sınıflandırılan bir ifade `E` verildiğinde, yapı `&E` `E`tarafından verilen değişkenin adresini hesaplar.</span><span class="sxs-lookup"><span data-stu-id="378d4-279">Given an expression `E` which is of a type `T` and is classified as a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), the construct `&E` computes the address of the variable given by `E`.</span></span> <span data-ttu-id="378d4-280">Sonucun türü `T*` ve bir değer olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="378d4-280">The type of the result is `T*` and is classified as a value.</span></span> <span data-ttu-id="378d4-281">`E` bir değişken olarak sınıflandırılmadığından, `E` salt okunurdur bir yerel değişken olarak sınıflandırıldığında veya taşınabilir bir değişken `E` içeriyorsa derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="378d4-281">A compile-time error occurs if `E` is not classified as a variable, if `E` is classified as a read-only local variable, or if `E` denotes a moveable variable.</span></span> <span data-ttu-id="378d4-282">Son durumda, bir sabit ifade ([fixed deyimidir](unsafe-code.md#the-fixed-statement)), adresini almadan önce değişkeni geçici olarak "onarmak" için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-282">In the last case, a fixed statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) can be used to temporarily "fix" the variable before obtaining its address.</span></span> <span data-ttu-id="378d4-283">[Üye erişimde](expressions.md#member-access)belirtildiği gibi, bir yapı veya bir `readonly` alanı tanımlayan sınıf için bir örnek Oluşturucu veya statik oluşturucu dışında, bu alan değişken değil değer olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-283">As stated in [Member access](expressions.md#member-access), outside an instance constructor or static constructor for a struct or class that defines a `readonly` field, that field is considered a value, not a variable.</span></span> <span data-ttu-id="378d4-284">Bu nedenle, adresi alınamaz.</span><span class="sxs-lookup"><span data-stu-id="378d4-284">As such, its address cannot be taken.</span></span> <span data-ttu-id="378d4-285">Benzer şekilde, bir sabit adresi alınamaz.</span><span class="sxs-lookup"><span data-stu-id="378d4-285">Similarly, the address of a constant cannot be taken.</span></span>

<span data-ttu-id="378d4-286">`&` işleci, bağımsız değişkeninin kesinlikle atanmasını gerektirmez, ancak bir `&` işlemi sonrasında, işlecin uygulandığı değişken, işlemin gerçekleştiği yürütme yolunda kesin olarak atanır.</span><span class="sxs-lookup"><span data-stu-id="378d4-286">The `&` operator does not require its argument to be definitely assigned, but following an `&` operation, the variable to which the operator is applied is considered definitely assigned in the execution path in which the operation occurs.</span></span> <span data-ttu-id="378d4-287">Bu durumda, değişkenin doğru başlatılmasının gerçekten gerçekleşmekte olduğundan emin olmak için programcı sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="378d4-287">It is the responsibility of the programmer to ensure that correct initialization of the variable actually does take place in this situation.</span></span>

<span data-ttu-id="378d4-288">örnekte</span><span class="sxs-lookup"><span data-stu-id="378d4-288">In the example</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int i;
        unsafe {
            int* p = &i;
            *p = 123;
        }
        Console.WriteLine(i);
    }
}
```

<span data-ttu-id="378d4-289">`i`, `p`başlatmak için kullanılan `&i` işleminin ardından kesinlikle atanmış olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-289">`i` is considered definitely assigned following the `&i` operation used to initialize `p`.</span></span> <span data-ttu-id="378d4-290">Etkin `*p` atama `i`başlatır, ancak bu başlatmanın eklenmesi programcının sorumluluğundadır ve atama kaldırılırsa hiçbir derleme zamanı hatası oluşmaz.</span><span class="sxs-lookup"><span data-stu-id="378d4-290">The assignment to `*p` in effect initializes `i`, but the inclusion of this initialization is the responsibility of the programmer, and no compile-time error would occur if the assignment was removed.</span></span>

<span data-ttu-id="378d4-291">`&` işleci için kesin atamanın kuralları, yerel değişkenlerin gereksiz şekilde başlatılmasına karşı kaçınılabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-291">The rules of definite assignment for the `&` operator exist such that redundant initialization of local variables can be avoided.</span></span> <span data-ttu-id="378d4-292">Örneğin, birçok harici API, API tarafından doldurulan bir yapıya bir işaretçi alır.</span><span class="sxs-lookup"><span data-stu-id="378d4-292">For example, many external APIs take a pointer to a structure which is filled in by the API.</span></span> <span data-ttu-id="378d4-293">Bu tür API çağrıları genellikle yerel bir yapı değişkeninin adresini geçer ve kural olmadan yapı değişkeninin gereksiz şekilde başlatılmasına gerek olur.</span><span class="sxs-lookup"><span data-stu-id="378d4-293">Calls to such APIs typically pass the address of a local struct variable, and without the rule, redundant initialization of the struct variable would be required.</span></span>

### <a name="pointer-increment-and-decrement"></a><span data-ttu-id="378d4-294">İşaretçi artışı ve azaltma</span><span class="sxs-lookup"><span data-stu-id="378d4-294">Pointer increment and decrement</span></span>

<span data-ttu-id="378d4-295">Güvenli olmayan bir bağlamda, `++` ve `--` işleçleri ([sonek artırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators) ve [ön ek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)), `void*`dışındaki tüm türlerin işaretçi değişkenlerine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-295">In an unsafe context, the `++` and `--` operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)) can be applied to pointer variables of all types except `void*`.</span></span> <span data-ttu-id="378d4-296">Bu nedenle, her işaretçi türü için `T*`, aşağıdaki işleçler örtülü olarak tanımlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="378d4-296">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

<span data-ttu-id="378d4-297">İşleçler, sırasıyla `x + 1` ve `x - 1`ile aynı sonuçları üretir ([işaretçi aritmetiği](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="378d4-297">The operators produce the same results as `x + 1` and `x - 1`, respectively ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span> <span data-ttu-id="378d4-298">Diğer bir deyişle, `T*`türünde bir işaretçi değişkeni için `++` işleci, değişkende bulunan adrese `sizeof(T)` ekler ve `--` işleci, değişkende bulunan adresten `sizeof(T)` çıkartır.</span><span class="sxs-lookup"><span data-stu-id="378d4-298">In other words, for a pointer variable of type `T*`, the `++` operator adds `sizeof(T)` to the address contained in the variable, and the `--` operator subtracts `sizeof(T)` from the address contained in the variable.</span></span>

<span data-ttu-id="378d4-299">Bir işaretçi artıma veya azaltma işlemi işaretçi türünün etki alanını taşarsa, sonuç uygulama tanımlı olur, ancak hiçbir özel durum üretilmez.</span><span class="sxs-lookup"><span data-stu-id="378d4-299">If a pointer increment or decrement operation overflows the domain of the pointer type, the result is implementation-defined, but no exceptions are produced.</span></span>

### <a name="pointer-arithmetic"></a><span data-ttu-id="378d4-300">İşaretçi aritmetiği</span><span class="sxs-lookup"><span data-stu-id="378d4-300">Pointer arithmetic</span></span>

<span data-ttu-id="378d4-301">Güvenli olmayan bir bağlamda `+` ve `-` işleçleri ([toplama işleci](expressions.md#addition-operator) ve [çıkarma işleci](expressions.md#subtraction-operator)), `void*`dışındaki tüm işaretçi türlerinin değerlerine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-301">In an unsafe context, the `+` and `-` operators ([Addition operator](expressions.md#addition-operator) and [Subtraction operator](expressions.md#subtraction-operator)) can be applied to values of all pointer types except `void*`.</span></span> <span data-ttu-id="378d4-302">Bu nedenle, her işaretçi türü için `T*`, aşağıdaki işleçler örtülü olarak tanımlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="378d4-302">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator +(T* x, int y);
T* operator +(T* x, uint y);
T* operator +(T* x, long y);
T* operator +(T* x, ulong y);

T* operator +(int x, T* y);
T* operator +(uint x, T* y);
T* operator +(long x, T* y);
T* operator +(ulong x, T* y);

T* operator -(T* x, int y);
T* operator -(T* x, uint y);
T* operator -(T* x, long y);
T* operator -(T* x, ulong y);

long operator -(T* x, T* y);
```

<span data-ttu-id="378d4-303">Bir işaretçi türü `P` `T*` ve `int`, `uint`, `long`veya `ulong`türünde bir ifade `N`, ifadeler `P + N` ve `N + P` `T*` tarafından verilen adrese eklenmesinin sonucu olan `N * sizeof(T)` türündeki işaretçi değerini hesaplamak `P`.</span><span class="sxs-lookup"><span data-stu-id="378d4-303">Given an expression `P` of a pointer type `T*` and an expression `N` of type `int`, `uint`, `long`, or `ulong`, the expressions `P + N` and `N + P` compute the pointer value of type `T*` that results from adding `N * sizeof(T)` to the address given by `P`.</span></span> <span data-ttu-id="378d4-304">Benzer şekilde, `P - N` ifade, `P`tarafından verilen adresten `N * sizeof(T)` çıkarılmasına neden olan `T*` türündeki işaretçi değerini hesaplar.</span><span class="sxs-lookup"><span data-stu-id="378d4-304">Likewise, the expression `P - N` computes the pointer value of type `T*` that results from subtracting `N * sizeof(T)` from the address given by `P`.</span></span>

<span data-ttu-id="378d4-305">`P` ve `Q`bir işaretçi türü `T*`olan iki ifade verildiğinde, ifade `P - Q` `P` ve `Q` tarafından verilen adresler arasındaki farkı hesaplar ve ardından bu farkı `sizeof(T)`böler.</span><span class="sxs-lookup"><span data-stu-id="378d4-305">Given two expressions, `P` and `Q`, of a pointer type `T*`, the expression `P - Q` computes the difference between the addresses given by `P` and `Q` and then divides that difference by `sizeof(T)`.</span></span> <span data-ttu-id="378d4-306">Sonucun türü her zaman `long`.</span><span class="sxs-lookup"><span data-stu-id="378d4-306">The type of the result is always `long`.</span></span> <span data-ttu-id="378d4-307">Aslında `P - Q` `((long)(P) - (long)(Q)) / sizeof(T)`olarak hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="378d4-307">In effect, `P - Q` is computed as `((long)(P) - (long)(Q)) / sizeof(T)`.</span></span>

<span data-ttu-id="378d4-308">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="378d4-308">For example:</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        unsafe {
            int* values = stackalloc int[20];
            int* p = &values[1];
            int* q = &values[15];
            Console.WriteLine("p - q = {0}", p - q);
            Console.WriteLine("q - p = {0}", q - p);
        }
    }
}
```

<span data-ttu-id="378d4-309">çıktıyı üreten:</span><span class="sxs-lookup"><span data-stu-id="378d4-309">which produces the output:</span></span>

```console
p - q = -14
q - p = 14
```

<span data-ttu-id="378d4-310">Bir işaretçi aritmetik işlemi işaretçi türünün etki alanını taşarsa, sonuç uygulama tanımlı bir biçimde kesilir, ancak hiçbir özel durum üretilmez.</span><span class="sxs-lookup"><span data-stu-id="378d4-310">If a pointer arithmetic operation overflows the domain of the pointer type, the result is truncated in an implementation-defined fashion, but no exceptions are produced.</span></span>

### <a name="pointer-comparison"></a><span data-ttu-id="378d4-311">İşaretçi karşılaştırması</span><span class="sxs-lookup"><span data-stu-id="378d4-311">Pointer comparison</span></span>

<span data-ttu-id="378d4-312">Güvenli olmayan bir bağlamda `==`, `!=`, `<`, `>`, `<=`ve `=>` işleçleri ([ilişkisel ve tür-test işleçleri](expressions.md#relational-and-type-testing-operators)) tüm işaretçi türlerinin değerlerine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-312">In an unsafe context, the `==`, `!=`, `<`, `>`, `<=`, and `=>` operators ([Relational and type-testing operators](expressions.md#relational-and-type-testing-operators)) can be applied to values of all pointer types.</span></span> <span data-ttu-id="378d4-313">İşaretçi karşılaştırma işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="378d4-313">The pointer comparison operators are:</span></span>

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

<span data-ttu-id="378d4-314">Örtük bir dönüştürme herhangi bir işaretçi türünden `void*` türüne sahip olduğundan, herhangi bir işaretçi türünün işlenenleri bu işleçler kullanılarak karşılaştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-314">Because an implicit conversion exists from any pointer type to the `void*` type, operands of any pointer type can be compared using these operators.</span></span> <span data-ttu-id="378d4-315">Karşılaştırma işleçleri, iki işlenen tarafından verilen adresleri işaretsiz tamsayılar gibi karşılaştırır.</span><span class="sxs-lookup"><span data-stu-id="378d4-315">The comparison operators compare the addresses given by the two operands as if they were unsigned integers.</span></span>

### <a name="the-sizeof-operator"></a><span data-ttu-id="378d4-316">Sizeof işleci</span><span class="sxs-lookup"><span data-stu-id="378d4-316">The sizeof operator</span></span>

<span data-ttu-id="378d4-317">`sizeof` işleci, verilen türdeki bir değişken tarafından bulunan bayt sayısını döndürür.</span><span class="sxs-lookup"><span data-stu-id="378d4-317">The `sizeof` operator returns the number of bytes occupied by a variable of a given type.</span></span> <span data-ttu-id="378d4-318">`sizeof` bir işlenen olarak belirtilen tür bir *unmanaged_type* ([işaretçi türü](unsafe-code.md#pointer-types)) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="378d4-318">The type specified as an operand to `sizeof` must be an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

<span data-ttu-id="378d4-319">`sizeof` işlecinin sonucu `int`türünde bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="378d4-319">The result of the `sizeof` operator is a value of type `int`.</span></span> <span data-ttu-id="378d4-320">Önceden tanımlanmış bazı türler için `sizeof` işleci, aşağıdaki tabloda gösterildiği gibi sabit bir değer verir.</span><span class="sxs-lookup"><span data-stu-id="378d4-320">For certain predefined types, the `sizeof` operator yields a constant value as shown in the table below.</span></span>


| <span data-ttu-id="378d4-321">__İfadesini__</span><span class="sxs-lookup"><span data-stu-id="378d4-321">__Expression__</span></span>   | <span data-ttu-id="378d4-322">__Sonuç__</span><span class="sxs-lookup"><span data-stu-id="378d4-322">__Result__</span></span> |
|------------------|------------|
| `sizeof(sbyte)`  | `1`        |
| `sizeof(byte)`   | `1`        |
| `sizeof(short)`  | `2`        |
| `sizeof(ushort)` | `2`        |
| `sizeof(int)`    | `4`        |
| `sizeof(uint)`   | `4`        |
| `sizeof(long)`   | `8`        |
| `sizeof(ulong)`  | `8`        |
| `sizeof(char)`   | `2`        |
| `sizeof(float)`  | `4`        |
| `sizeof(double)` | `8`        |
| `sizeof(bool)`   | `1`        |

<span data-ttu-id="378d4-323">Tüm diğer türler için `sizeof` işlecinin sonucu uygulama tarafından tanımlanır ve bir değer olarak sınıflandırıldı, sabit değildir.</span><span class="sxs-lookup"><span data-stu-id="378d4-323">For all other types, the result of the `sizeof` operator is implementation-defined and is classified as a value, not a constant.</span></span>

<span data-ttu-id="378d4-324">Üyelerin bir yapıya paketleneme sırası belirtilmemiş.</span><span class="sxs-lookup"><span data-stu-id="378d4-324">The order in which members are packed into a struct is unspecified.</span></span>

<span data-ttu-id="378d4-325">Hizalama amacıyla, bir yapının başlangıcında, bir yapı içinde ve yapının sonunda adlandırılmamış doldurma olabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-325">For alignment purposes, there may be unnamed padding at the beginning of a struct, within a struct, and at the end of the struct.</span></span> <span data-ttu-id="378d4-326">Doldurma olarak kullanılan bitlerin içeriği belirsiz.</span><span class="sxs-lookup"><span data-stu-id="378d4-326">The contents of the bits used as padding are indeterminate.</span></span>

<span data-ttu-id="378d4-327">Yapı türüne sahip bir işlenene uygulandığında, sonuç, herhangi bir doldurma dahil olmak üzere bu tür bir değişkende toplam bayt sayısıdır.</span><span class="sxs-lookup"><span data-stu-id="378d4-327">When applied to an operand that has struct type, the result is the total number of bytes in a variable of that type, including any padding.</span></span>

## <a name="the-fixed-statement"></a><span data-ttu-id="378d4-328">Fixed ekstresi</span><span class="sxs-lookup"><span data-stu-id="378d4-328">The fixed statement</span></span>

<span data-ttu-id="378d4-329">Güvenli olmayan bir bağlamda, *embedded_statement* ([deyimler](statements.md)) üretimi ek bir yapı, bir taşınabilir değişkenini "onarmak" için kullanılan `fixed` bildirimine izin verir. Bu, örneğin, adresi deyimin süresi boyunca sabit kalır.</span><span class="sxs-lookup"><span data-stu-id="378d4-329">In an unsafe context, the *embedded_statement* ([Statements](statements.md)) production permits an additional construct, the `fixed` statement, which is used to "fix" a moveable variable such that its address remains constant for the duration of the statement.</span></span>

```antlr
fixed_statement
    : 'fixed' '(' pointer_type fixed_pointer_declarators ')' embedded_statement
    ;

fixed_pointer_declarators
    : fixed_pointer_declarator (','  fixed_pointer_declarator)*
    ;

fixed_pointer_declarator
    : identifier '=' fixed_pointer_initializer
    ;

fixed_pointer_initializer
    : '&' variable_reference
    | expression
    ;
```

<span data-ttu-id="378d4-330">Her *fixed_pointer_declarator* , verilen *pointer_type* yerel bir değişkenini bildirir ve bu yerel değişkeni karşılık gelen *fixed_pointer_initializer*tarafından hesaplanan adresle başlatır.</span><span class="sxs-lookup"><span data-stu-id="378d4-330">Each *fixed_pointer_declarator* declares a local variable of the given *pointer_type* and initializes that local variable with the address computed by the corresponding *fixed_pointer_initializer*.</span></span> <span data-ttu-id="378d4-331">`fixed` bildiriminde belirtilen bir yerel değişkene, bu değişkenin bildiriminin sağında oluşan tüm *fixed_pointer_initializer*ve `fixed` ifadesinin *embedded_statement* erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-331">A local variable declared in a `fixed` statement is accessible in any *fixed_pointer_initializer*s occurring to the right of that variable's declaration, and in the *embedded_statement* of the `fixed` statement.</span></span> <span data-ttu-id="378d4-332">`fixed` ifadesiyle belirtilen yerel değişken salt okunurdur.</span><span class="sxs-lookup"><span data-stu-id="378d4-332">A local variable declared by a `fixed` statement is considered read-only.</span></span> <span data-ttu-id="378d4-333">Katıştırılmış deyimin bu yerel değişkeni (atama veya `++` ile `--` işleçleri) değiştirmeye veya `ref` ya da `out` parametresi olarak geçirmeye çalışırsa bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="378d4-333">A compile-time error occurs if the embedded statement attempts to modify this local variable (via assignment or the `++` and `--` operators) or pass it as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="378d4-334">*Fixed_pointer_initializer* aşağıdakilerden biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="378d4-334">A *fixed_pointer_initializer* can be one of the following:</span></span>

*  <span data-ttu-id="378d4-335">"`&`" belirteci, yönetilmeyen bir tür `T`taşınabilir bir değişken ([sabit ve taşınabilir değişkenler](unsafe-code.md#fixed-and-moveable-variables)) için bir *variable_reference* ([kesin bir atamayı belirlemek için kesin kurallar](variables.md#precise-rules-for-determining-definite-assignment)) ile birlikte, türü `T*` örtük olarak `fixed` ifadesinde verilen işaretçi türüne dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-335">The token "`&`" followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) to a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="378d4-336">Bu durumda, başlatıcı verilen değişkenin adresini hesaplar ve değişken `fixed` deyimin süresi boyunca sabit bir adreste kalacak şekilde garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-336">In this case, the initializer computes the address of the given variable, and the variable is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>
*  <span data-ttu-id="378d4-337">Yönetilmeyen tür `T`öğeleriyle *array_type* bir ifadesi, `T*` türü `fixed` deyiminde verilen işaretçi türüne örtük olarak dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-337">An expression of an *array_type* with elements of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="378d4-338">Bu durumda, başlatıcı dizideki ilk öğenin adresini hesaplar ve `fixed` deyimin süresi boyunca sabit bir adreste kaldığı tüm diziyi garanti eder.</span><span class="sxs-lookup"><span data-stu-id="378d4-338">In this case, the initializer computes the address of the first element in the array, and the entire array is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="378d4-339">Dizi ifadesi null ise veya dizide sıfır öğe varsa, başlatıcı bir adresi sıfıra eşit olarak hesaplar.</span><span class="sxs-lookup"><span data-stu-id="378d4-339">If the array expression is null or if the array has zero elements, the initializer computes an address equal to zero.</span></span>
*  <span data-ttu-id="378d4-340">`string`türünde bir ifade, `char*` türü `fixed` deyiminde verilen işaretçi türüne örtük olarak dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-340">An expression of type `string`, provided the type `char*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="378d4-341">Bu durumda, başlatıcı dizedeki ilk karakterin adresini hesaplar ve tüm dize, `fixed` deyimin süresi boyunca sabit bir adreste kalacak şekilde garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-341">In this case, the initializer computes the address of the first character in the string, and the entire string is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="378d4-342">`fixed` deyiminin davranışı, dize ifadesi null ise uygulama tanımlı ' dır.</span><span class="sxs-lookup"><span data-stu-id="378d4-342">The behavior of the `fixed` statement is implementation-defined if the string expression is null.</span></span>
*  <span data-ttu-id="378d4-343">Taşınabilir bir değişkenin sabit boyut arabelleği üyesine başvuran bir *simple_name* veya *member_access* , sabit boyutlu arabellek üyesinin türü, `fixed` bildiriminde verilen işaretçi türüne örtük olarak dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-343">A *simple_name* or *member_access* that references a fixed size buffer member of a moveable variable, provided the type of the fixed size buffer member is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="378d4-344">Bu durumda, başlatıcı sabit boyutlu arabelleğin ([Ifadelerde sabit boyutlu arabellekler](unsafe-code.md#fixed-size-buffers-in-expressions)) ilk öğesine bir işaretçi hesaplar ve sabit boyutlu arabelleğin `fixed` deyimi süresince sabit bir adreste kalması garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-344">In this case, the initializer computes a pointer to the first element of the fixed size buffer ([Fixed size buffers in expressions](unsafe-code.md#fixed-size-buffers-in-expressions)), and the fixed size buffer is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>

<span data-ttu-id="378d4-345">*Fixed_pointer_initializer* tarafından hesaplanan her adres için `fixed` ifade, adres tarafından başvurulan değişkenin, `fixed` deyimin süresi boyunca çöp toplayıcı tarafından yeniden konumlandırma veya elden çıkarılma tabi olmamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="378d4-345">For each address computed by a *fixed_pointer_initializer* the `fixed` statement ensures that the variable referenced by the address is not subject to relocation or disposal by the garbage collector for the duration of the `fixed` statement.</span></span> <span data-ttu-id="378d4-346">Örneğin, bir *fixed_pointer_initializer* tarafından hesaplanan adres bir nesnenin alanına veya bir dizi örneğinin öğesine başvuruyorsa, `fixed` ifade, kapsayan nesne örneğinin deyimin ömrü boyunca yeniden konumlandırılıp çıkarılmadığından emin olur.</span><span class="sxs-lookup"><span data-stu-id="378d4-346">For example, if the address computed by a *fixed_pointer_initializer* references a field of an object or an element of an array instance, the `fixed` statement guarantees that the containing object instance is not relocated or disposed of during the lifetime of the statement.</span></span>

<span data-ttu-id="378d4-347">`fixed` deyimleri tarafından oluşturulan işaretçilerin Bu deyimlerin yürütülmesinden fazla devam etmez.</span><span class="sxs-lookup"><span data-stu-id="378d4-347">It is the programmer's responsibility to ensure that pointers created by `fixed` statements do not survive beyond execution of those statements.</span></span> <span data-ttu-id="378d4-348">Örneğin, `fixed` deyimleri tarafından oluşturulan işaretçiler dış API 'lere geçirildiğinde, API 'Lerin bu işaretçilerin herhangi bir bellek içermediğinden emin olmak programcı sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="378d4-348">For example, when pointers created by `fixed` statements are passed to external APIs, it is the programmer's responsibility to ensure that the APIs retain no memory of these pointers.</span></span>

<span data-ttu-id="378d4-349">Sabit nesneler yığının parçalanmasına neden olabilir (çünkü taşınamazlar).</span><span class="sxs-lookup"><span data-stu-id="378d4-349">Fixed objects may cause fragmentation of the heap (because they can't be moved).</span></span> <span data-ttu-id="378d4-350">Bu nedenle, nesneler yalnızca kesinlikle gerekli olduğunda ve yalnızca en kısa sürede mümkün olduğunda düzeltilmelidir.</span><span class="sxs-lookup"><span data-stu-id="378d4-350">For that reason, objects should be fixed only when absolutely necessary and then only for the shortest amount of time possible.</span></span>

<span data-ttu-id="378d4-351">Örnek</span><span class="sxs-lookup"><span data-stu-id="378d4-351">The example</span></span>

```csharp
class Test
{
    static int x;
    int y;

    unsafe static void F(int* p) {
        *p = 1;
    }

    static void Main() {
        Test t = new Test();
        int[] a = new int[10];
        unsafe {
            fixed (int* p = &x) F(p);
            fixed (int* p = &t.y) F(p);
            fixed (int* p = &a[0]) F(p);
            fixed (int* p = a) F(p);
        }
    }
}
```

<span data-ttu-id="378d4-352">`fixed` deyimin çeşitli kullanımlarını gösterir.</span><span class="sxs-lookup"><span data-stu-id="378d4-352">demonstrates several uses of the `fixed` statement.</span></span> <span data-ttu-id="378d4-353">İlk ifade bir statik alanın adresini düzeltir ve edinir, ikinci ifade bir örnek alanının adresini düzeltir ve edinir ve üçüncü ifade, bir dizi öğesinin adresini düzeltir ve edinir.</span><span class="sxs-lookup"><span data-stu-id="378d4-353">The first statement fixes and obtains the address of a static field, the second statement fixes and obtains the address of an instance field, and the third statement fixes and obtains the address of an array element.</span></span> <span data-ttu-id="378d4-354">Her durumda, değişkenlerin tamamen taşınabilir değişkenler olarak sınıflandırıldığından, bu, normal `&` işlecini kullanmanın bir hatası oluyordu.</span><span class="sxs-lookup"><span data-stu-id="378d4-354">In each case it would have been an error to use the regular `&` operator since the variables are all classified as moveable variables.</span></span>

<span data-ttu-id="378d4-355">Yukarıdaki örnekteki dördüncü `fixed` deyimleri, üçüncü için de benzer bir sonuç üretir.</span><span class="sxs-lookup"><span data-stu-id="378d4-355">The fourth `fixed` statement in the example above produces a similar result to the third.</span></span>

<span data-ttu-id="378d4-356">`fixed` deyimin bu örneği `string`kullanır:</span><span class="sxs-lookup"><span data-stu-id="378d4-356">This example of the `fixed` statement uses `string`:</span></span>

```csharp
class Test
{
    static string name = "xx";

    unsafe static void F(char* p) {
        for (int i = 0; p[i] != '\0'; ++i)
            Console.WriteLine(p[i]);
    }

    static void Main() {
        unsafe {
            fixed (char* p = name) F(p);
            fixed (char* p = "xx") F(p);
        }
    }
}
```

<span data-ttu-id="378d4-357">Tek boyutlu dizilerin güvenli olmayan bir bağlam dizisi öğelerinde, Dizin `0` başlayıp Dizin `Length - 1`sona ermek üzere dizin sırasında artan dizin sırasında depolanır.</span><span class="sxs-lookup"><span data-stu-id="378d4-357">In an unsafe context array elements of single-dimensional arrays are stored in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="378d4-358">Çok boyutlu diziler için, dizi öğeleri, en sağdaki boyutun dizinlerinin önce, ardından bir sonraki sol boyutun ve bu şekilde sol tarafında arttığı şekilde depolanır.</span><span class="sxs-lookup"><span data-stu-id="378d4-358">For multi-dimensional arrays, array elements are stored such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span> <span data-ttu-id="378d4-359">Dizi örneği `a``p` bir işaretçi alan `fixed` bir bildiriminde, `p` ile `p + a.Length - 1` arasındaki işaretçi değerleri dizideki öğelerin adreslerini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="378d4-359">Within a `fixed` statement that obtains a pointer `p` to an array instance `a`, the pointer values ranging from `p` to `p + a.Length - 1` represent addresses of the elements in the array.</span></span> <span data-ttu-id="378d4-360">Benzer şekilde, `p[0]` ile `p[a.Length - 1]` arasındaki değişkenler gerçek dizi öğelerini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="378d4-360">Likewise, the variables ranging from `p[0]` to `p[a.Length - 1]` represent the actual array elements.</span></span> <span data-ttu-id="378d4-361">Dizilerin nerede depolandığına göre, herhangi bir boyutun dizisini doğrusal hale gelse de işleyebiliriz.</span><span class="sxs-lookup"><span data-stu-id="378d4-361">Given the way in which arrays are stored, we can treat an array of any dimension as though it were linear.</span></span>

<span data-ttu-id="378d4-362">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="378d4-362">For example:</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int[,,] a = new int[2,3,4];
        unsafe {
            fixed (int* p = a) {
                for (int i = 0; i < a.Length; ++i)    // treat as linear
                    p[i] = i;
            }
        }

        for (int i = 0; i < 2; ++i)
            for (int j = 0; j < 3; ++j) {
                for (int k = 0; k < 4; ++k)
                    Console.Write("[{0},{1},{2}] = {3,2} ", i, j, k, a[i,j,k]);
                Console.WriteLine();
            }
    }
}
```

<span data-ttu-id="378d4-363">çıktıyı üreten:</span><span class="sxs-lookup"><span data-stu-id="378d4-363">which produces the output:</span></span>

```console
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

<span data-ttu-id="378d4-364">örnekte</span><span class="sxs-lookup"><span data-stu-id="378d4-364">In the example</span></span>

```csharp
class Test
{
    unsafe static void Fill(int* p, int count, int value) {
        for (; count != 0; count--) *p++ = value;
    }

    static void Main() {
        int[] a = new int[100];
        unsafe {
            fixed (int* p = a) Fill(p, 100, -1);
        }
    }
}
```

<span data-ttu-id="378d4-365">bir `fixed` ifade, bir diziyi çözmek için kullanılır, bu nedenle adresinin bir işaretçi alan bir yönteme geçirilmesi sağlanır.</span><span class="sxs-lookup"><span data-stu-id="378d4-365">a `fixed` statement is used to fix an array so its address can be passed to a method that takes a pointer.</span></span>

<span data-ttu-id="378d4-366">Örnekte:</span><span class="sxs-lookup"><span data-stu-id="378d4-366">In the example:</span></span>

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    Font f;

    unsafe static void Main()
    {
        Test test = new Test();
        test.f.size = 10;
        fixed (char* p = test.f.name) {
            PutString("Times New Roman", p, 32);
        }
    }
}
```

<span data-ttu-id="378d4-367">sabit bir ifade, bir yapının sabit boyutlu arabelleğini gidermek için kullanılır, bu nedenle adresi bir işaretçi olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-367">a fixed statement is used to fix a fixed size buffer of a struct so its address can be used as a pointer.</span></span>

<span data-ttu-id="378d4-368">Bir dize örneğini düzelterek üretilen `char*` değeri her zaman null ile sonlandırılmış bir dizeye işaret eder.</span><span class="sxs-lookup"><span data-stu-id="378d4-368">A `char*` value produced by fixing a string instance always points to a null-terminated string.</span></span> <span data-ttu-id="378d4-369">Bir `s`dize örneğine `p` işaretçi alan bir fixed ifadesinde, `p + s.Length - 1` `p` arasındaki işaretçi değerleri dizedeki karakterlerin adreslerini temsil eder ve işaretçi değeri `p + s.Length` her zaman bir null karakteri gösterir (değeri `'\0'`olan karakter).</span><span class="sxs-lookup"><span data-stu-id="378d4-369">Within a fixed statement that obtains a pointer `p` to a string instance `s`, the pointer values ranging from `p` to `p + s.Length - 1` represent addresses of the characters in the string, and the pointer value `p + s.Length` always points to a null character (the character with value `'\0'`).</span></span>

<span data-ttu-id="378d4-370">Yönetilen türdeki nesneleri sabit işaretçiler aracılığıyla değiştirmek tanımsız davranışa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-370">Modifying objects of managed type through fixed pointers can results in undefined behavior.</span></span> <span data-ttu-id="378d4-371">Örneğin, dizeler sabittir olduğundan, sabit bir dizeye yönelik işaretçinin başvurduğu karakterlerin değiştirilmediğinden emin olmak programcı sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="378d4-371">For example, because strings are immutable, it is the programmer's responsibility to ensure that the characters referenced by a pointer to a fixed string are not modified.</span></span>

<span data-ttu-id="378d4-372">Dizelerin otomatik olarak boş sonlandırması, "C stili" dizelerini bekleyen dış API 'Leri çağırırken özellikle kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="378d4-372">The automatic null-termination of strings is particularly convenient when calling external APIs that expect "C-style" strings.</span></span> <span data-ttu-id="378d4-373">Ancak, bir dize örneğinin null karakter içermesine izin verildiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="378d4-373">Note, however, that a string instance is permitted to contain null characters.</span></span> <span data-ttu-id="378d4-374">Bu null karakterler varsa, null sonlandırılmış `char*`olarak kabul edildiğinde dize kesilebilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-374">If such null characters are present, the string will appear truncated when treated as a null-terminated `char*`.</span></span>

## <a name="fixed-size-buffers"></a><span data-ttu-id="378d4-375">Sabit Boyutlu Arabellekler</span><span class="sxs-lookup"><span data-stu-id="378d4-375">Fixed size buffers</span></span>

<span data-ttu-id="378d4-376">Sabit boyutlu arabellekler, "C stili" satır içi dizileri yapıların üyeleri olarak bildirmek için kullanılır ve öncelikle yönetilmeyen API 'lerle arabirim oluşturma için faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="378d4-376">Fixed size buffers are used to declare "C style" in-line arrays as members of structs, and are primarily useful for interfacing with unmanaged APIs.</span></span>

### <a name="fixed-size-buffer-declarations"></a><span data-ttu-id="378d4-377">Sabit boyutlu arabellek bildirimleri</span><span class="sxs-lookup"><span data-stu-id="378d4-377">Fixed size buffer declarations</span></span>

<span data-ttu-id="378d4-378">***Sabit boyutlu arabellek*** , belirli bir türdeki değişkenlerin sabit uzunluklu arabelleği için depolamayı temsil eden bir üyedir.</span><span class="sxs-lookup"><span data-stu-id="378d4-378">A ***fixed size buffer*** is a member that represents storage for a fixed length buffer of variables of a given type.</span></span> <span data-ttu-id="378d4-379">Sabit boyutlu bir arabellek bildirimi, belirli bir öğe türünün bir veya daha fazla sabit boyutlu arabelleğini tanıtır.</span><span class="sxs-lookup"><span data-stu-id="378d4-379">A fixed size buffer declaration introduces one or more fixed size buffers of a given element type.</span></span> <span data-ttu-id="378d4-380">Sabit boyutlu arabelleklere yalnızca struct bildirimlerinde izin verilir ve yalnızca güvenli olmayan bağlamlarda ([güvensiz bağlamlarda](unsafe-code.md#unsafe-contexts)) bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-380">Fixed size buffers are only permitted in struct declarations and can only occur in unsafe contexts ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

```antlr
struct_member_declaration_unsafe
    : fixed_size_buffer_declaration
    ;

fixed_size_buffer_declaration
    : attributes? fixed_size_buffer_modifier* 'fixed' buffer_element_type fixed_size_buffer_declarator+ ';'
    ;

fixed_size_buffer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'unsafe'
    ;

buffer_element_type
    : type
    ;

fixed_size_buffer_declarator
    : identifier '[' constant_expression ']'
    ;
```

<span data-ttu-id="378d4-381">Sabit boyutlu bir arabellek bildirimi, bir dizi öznitelik ([öznitelik](attributes.md)), `new` değiştirici ([değiştiriciler](classes.md#modifiers)), dört erişim değiştiricisinin geçerli bir birleşimini ([tür parametreleri ve kısıtlamaları](classes.md#type-parameters-and-constraints)) ve bir `unsafe` değiştirici ([güvenli olmayan bağlamlar](unsafe-code.md#unsafe-contexts)) içerebilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-381">A fixed size buffer declaration may include a set of attributes ([Attributes](attributes.md)), a `new` modifier ([Modifiers](classes.md#modifiers)), a valid combination of the four access modifiers ([Type parameters and constraints](classes.md#type-parameters-and-constraints)) and an `unsafe` modifier ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="378d4-382">Öznitelikler ve değiştiriciler, sabit boyutlu arabellek bildirimi tarafından belirtilen tüm Üyeler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="378d4-382">The attributes and modifiers apply to all of the members declared by the fixed size buffer declaration.</span></span> <span data-ttu-id="378d4-383">Aynı değiştiricinin sabit boyutlu bir arabellek bildiriminde birden çok kez görünmesi hatadır.</span><span class="sxs-lookup"><span data-stu-id="378d4-383">It is an error for the same modifier to appear multiple times in a fixed size buffer declaration.</span></span>

<span data-ttu-id="378d4-384">Sabit boyutlu bir arabellek bildiriminin `static` değiştiricisini içerme izni yoktur.</span><span class="sxs-lookup"><span data-stu-id="378d4-384">A fixed size buffer declaration is not permitted to include the `static` modifier.</span></span>

<span data-ttu-id="378d4-385">Sabit boyutlu bir arabellek bildiriminin arabellek öğesi türü, bildirim tarafından tanıtılan arabelleğin öğe türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="378d4-385">The buffer element type of a fixed size buffer declaration specifies the element type of the buffer(s) introduced by the declaration.</span></span> <span data-ttu-id="378d4-386">Buffer öğe türü önceden tanımlanmış türlerden biri olmalıdır `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`veya `bool`.</span><span class="sxs-lookup"><span data-stu-id="378d4-386">The buffer element type must be one of the predefined types `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `bool`.</span></span>

<span data-ttu-id="378d4-387">Buffer öğe türü, her biri yeni bir üye tanıtan sabit boyutlu arabellek bildirimcilerinin bir listesi tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="378d4-387">The buffer element type is followed by a list of fixed size buffer declarators, each of which introduces a new member.</span></span> <span data-ttu-id="378d4-388">Sabit boyutlu bir arabellek bildirimci, üyeyi belirten bir tanımlayıcıdan, ardından `[` ve `]` belirteçleriyle bir sabit ifadeyle oluşur.</span><span class="sxs-lookup"><span data-stu-id="378d4-388">A fixed size buffer declarator consists of an identifier that names the member, followed by a constant expression enclosed in `[` and `]` tokens.</span></span> <span data-ttu-id="378d4-389">Sabit ifade, bu sabit boyutlu arabellek bildirimci tarafından tanıtılan üye içindeki öğe sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="378d4-389">The constant expression denotes the number of elements in the member introduced by that fixed size buffer declarator.</span></span> <span data-ttu-id="378d4-390">Sabit ifadenin türü `int`türüne örtük olarak dönüştürülebilir olmalıdır ve değer sıfır olmayan pozitif bir tamsayı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="378d4-390">The type of the constant expression must be implicitly convertible to type `int`, and the value must be a non-zero positive integer.</span></span>

<span data-ttu-id="378d4-391">Sabit boyutlu bir arabelleğin öğelerinin ardışık olarak bellekte düzenlenme garantisi vardır.</span><span class="sxs-lookup"><span data-stu-id="378d4-391">The elements of a fixed size buffer are guaranteed to be laid out sequentially in memory.</span></span>

<span data-ttu-id="378d4-392">Birden çok sabit boyut arabelleği bildiren sabit boyutlu bir arabellek bildirimi, aynı özniteliklere ve öğe türlerine sahip tek bir sabit boyutlu arabellek bildiriminin birden çok bildirimine eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="378d4-392">A fixed size buffer declaration that declares multiple fixed size buffers is equivalent to multiple declarations of a single fixed size buffer declaration with the same attributes, and element types.</span></span> <span data-ttu-id="378d4-393">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="378d4-393">For example</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

<span data-ttu-id="378d4-394">eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="378d4-394">is equivalent to</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a><span data-ttu-id="378d4-395">İfadelerde sabit boyutlu arabellekler</span><span class="sxs-lookup"><span data-stu-id="378d4-395">Fixed size buffers in expressions</span></span>

<span data-ttu-id="378d4-396">Sabit boyutlu bir arabellek üyesinin üye araması ([işleçler](expressions.md#operators)), bir alanın üye aramasına tam olarak devam eder.</span><span class="sxs-lookup"><span data-stu-id="378d4-396">Member lookup ([Operators](expressions.md#operators)) of a fixed size buffer member proceeds exactly like member lookup of a field.</span></span>

<span data-ttu-id="378d4-397">Sabit boyutlu arabelleğe bir *simple_name* ([tür çıkarımı](expressions.md#type-inference)) veya bir *member_access* ([dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) kullanılarak bir ifadede başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-397">A fixed size buffer can be referenced in an expression using a *simple_name* ([Type inference](expressions.md#type-inference)) or a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="378d4-398">Sabit boyutlu bir arabellek üyesine basit ad olarak başvuruluyorsa, efekt `this.I`form üye erişimi ile aynıdır; burada `I` sabit boyut arabelleği üyesidir.</span><span class="sxs-lookup"><span data-stu-id="378d4-398">When a fixed size buffer member is referenced as a simple name, the effect is the same as a member access of the form `this.I`, where `I` is the fixed size buffer member.</span></span>

<span data-ttu-id="378d4-399">Formun üye erişiminin `E.I`, `E` bir yapı türüdür ve bu yapı türünde `I` üye araması sabit boyutlu bir üyeyi tanımlarsa, `E.I` sınıflandırılan aşağıdaki gibi değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="378d4-399">In a member access of the form `E.I`, if `E` is of a struct type and a member lookup of `I` in that struct type identifies a fixed size member, then `E.I` is evaluated an classified as follows:</span></span>

*  <span data-ttu-id="378d4-400">`E.I` ifadesi güvenli olmayan bir bağlamda gerçekleşmezse, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="378d4-400">If the expression `E.I` does not occur in an unsafe context, a compile-time error occurs.</span></span>
*  <span data-ttu-id="378d4-401">`E` bir değer olarak sınıflandırıldığında, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="378d4-401">If `E` is classified as a value, a compile-time error occurs.</span></span>
*  <span data-ttu-id="378d4-402">Aksi takdirde, `E` taşınabilir bir değişkendir ([sabit ve taşınabilir değişkenler](unsafe-code.md#fixed-and-moveable-variables)) ve `E.I` ifadesi bir *fixed_pointer_initializer* ([fixed Deyimi](unsafe-code.md#the-fixed-statement)) değilse, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="378d4-402">Otherwise, if `E` is a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) and the expression `E.I` is not a *fixed_pointer_initializer* ([The fixed statement](unsafe-code.md#the-fixed-statement)), a compile-time error occurs.</span></span>
*  <span data-ttu-id="378d4-403">Aksi takdirde, `E` sabit bir değişkene başvuruda bulunur ve ifadenin sonucu, `E`içindeki sabit boyutlu arabellek üyesi `I` ilk öğesine yönelik bir işaretçidir.</span><span class="sxs-lookup"><span data-stu-id="378d4-403">Otherwise, `E` references a fixed variable and the result of the expression is a pointer to the first element of the fixed size buffer member `I` in `E`.</span></span> <span data-ttu-id="378d4-404">`S*`, `S` `I`öğe türü olduğu ve bir değer olarak sınıflandırılan türüdür.</span><span class="sxs-lookup"><span data-stu-id="378d4-404">The result is of type `S*`, where `S` is the element type of `I`, and is classified as a value.</span></span>

<span data-ttu-id="378d4-405">Sabit boyutlu arabelleğin sonraki öğelerine, ilk öğeden işaretçi işlemleri kullanılarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-405">The subsequent elements of the fixed size buffer can be accessed using pointer operations from the first element.</span></span> <span data-ttu-id="378d4-406">Dizilere erişimin aksine, sabit boyutlu bir arabelleğin öğelerine erişimi güvenli olmayan bir işlemdir ve Aralık işaretli değildir.</span><span class="sxs-lookup"><span data-stu-id="378d4-406">Unlike access to arrays, access to the elements of a fixed size buffer is an unsafe operation and is not range checked.</span></span>

<span data-ttu-id="378d4-407">Aşağıdaki örnek, sabit boyutlu bir arabellek üyesine sahip bir struct bildirir ve kullanır.</span><span class="sxs-lookup"><span data-stu-id="378d4-407">The following example declares and uses a struct with a fixed size buffer member.</span></span>

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    unsafe static void Main()
    {
        Font f;
        f.size = 10;
        PutString("Times New Roman", f.name, 32);
    }
}
```

### <a name="definite-assignment-checking"></a><span data-ttu-id="378d4-408">Kesin atama denetimi</span><span class="sxs-lookup"><span data-stu-id="378d4-408">Definite assignment checking</span></span>

<span data-ttu-id="378d4-409">Sabit boyutlu arabellekler kesin atama denetimine ([kesin atama](variables.md#definite-assignment)) tabi değildir ve yapı türü değişkenlerinin kesin atama denetimi amacıyla sabit boyutlu arabellek üyeleri göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-409">Fixed size buffers are not subject to definite assignment checking ([Definite assignment](variables.md#definite-assignment)), and fixed size buffer members are ignored for purposes of definite assignment checking of struct type variables.</span></span>

<span data-ttu-id="378d4-410">Sabit boyutlu bir arabellek üyesinin yapı değişkenini içeren en dıştaki, bir statik değişken, bir sınıf örneğinin örnek değişkeni veya bir dizi öğesi olduğunda, sabit boyutlu arabelleğin öğeleri otomatik olarak varsayılan değerlerine ([varsayılan değerler](variables.md#default-values)) başlatılır.</span><span class="sxs-lookup"><span data-stu-id="378d4-410">When the outermost containing struct variable of a fixed size buffer member is a static variable, an instance variable of a class instance, or an array element, the elements of the fixed size buffer are automatically initialized to their default values ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="378d4-411">Diğer tüm durumlarda, sabit boyutlu bir arabelleğin ilk içeriği tanımsızdır.</span><span class="sxs-lookup"><span data-stu-id="378d4-411">In all other cases, the initial content of a fixed size buffer is undefined.</span></span>

## <a name="stack-allocation"></a><span data-ttu-id="378d4-412">Yığın ayırma</span><span class="sxs-lookup"><span data-stu-id="378d4-412">Stack allocation</span></span>

<span data-ttu-id="378d4-413">Güvenli olmayan bir bağlamda, yerel değişken bildirimi ([yerel değişken bildirimleri](statements.md#local-variable-declarations)), çağrı yığınından bellek ayıran bir yığın ayırma başlatıcısı içerebilir.</span><span class="sxs-lookup"><span data-stu-id="378d4-413">In an unsafe context, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) may include a stack allocation initializer which allocates memory from the call stack.</span></span>

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="378d4-414">*Unmanaged_type* , yeni ayrılan konumda depolanacak öğelerin türünü gösterir ve *ifade* bu öğelerin sayısını gösterir.</span><span class="sxs-lookup"><span data-stu-id="378d4-414">The *unmanaged_type* indicates the type of the items that will be stored in the newly allocated location, and the *expression* indicates the number of these items.</span></span> <span data-ttu-id="378d4-415">Birlikte getirildiğinde, gerekli ayırma boyutunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="378d4-415">Taken together, these specify the required allocation size.</span></span> <span data-ttu-id="378d4-416">Yığın ayırma boyutu negatif olamaz, öğelerin sayısını negatif bir değer değerlendiren bir *constant_expression* olarak belirtmek için derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="378d4-416">Since the size of a stack allocation cannot be negative, it is a compile-time error to specify the number of items as a *constant_expression* that evaluates to a negative value.</span></span>

<span data-ttu-id="378d4-417">Form `stackalloc T[E]` bir yığın ayırma başlatıcısı, yönetilmeyen bir tür ([işaretçi türleri](unsafe-code.md#pointer-types)) ve `E` `int`türünde bir ifade olacak şekilde `T` gerektirir.</span><span class="sxs-lookup"><span data-stu-id="378d4-417">A stack allocation initializer of the form `stackalloc T[E]` requires `T` to be an unmanaged type ([Pointer types](unsafe-code.md#pointer-types)) and `E` to be an expression of type `int`.</span></span> <span data-ttu-id="378d4-418">Yapı, çağrı yığınından `E * sizeof(T)` bayt ayırır ve yeni ayrılan bloğa `T*`türünde bir işaretçi döndürür.</span><span class="sxs-lookup"><span data-stu-id="378d4-418">The construct allocates `E * sizeof(T)` bytes from the call stack and returns a pointer, of type `T*`, to the newly allocated block.</span></span> <span data-ttu-id="378d4-419">`E` negatif bir değer ise, davranış tanımsızdır.</span><span class="sxs-lookup"><span data-stu-id="378d4-419">If `E` is a negative value, then the behavior is undefined.</span></span> <span data-ttu-id="378d4-420">`E` sıfırsa, hiçbir ayırma yapılmaz ve döndürülen işaretçi uygulama tanımlı olur.</span><span class="sxs-lookup"><span data-stu-id="378d4-420">If `E` is zero, then no allocation is made, and the pointer returned is implementation-defined.</span></span> <span data-ttu-id="378d4-421">Verilen boyutta bir blok ayırmak için yeterli kullanılabilir bellek yoksa bir `System.StackOverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="378d4-421">If there is not enough memory available to allocate a block of the given size, a `System.StackOverflowException` is thrown.</span></span>

<span data-ttu-id="378d4-422">Yeni ayrılan belleğin içeriği tanımlı değil.</span><span class="sxs-lookup"><span data-stu-id="378d4-422">The content of the newly allocated memory is undefined.</span></span>

<span data-ttu-id="378d4-423">`catch` veya `finally` blokları 'nda ([TRY ifadesinde](statements.md#the-try-statement)) yığın ayırma başlatıcılarına izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="378d4-423">Stack allocation initializers are not permitted in `catch` or `finally` blocks ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="378d4-424">`stackalloc`kullanılarak ayrılan belleği açık bir şekilde serbest bırakma yöntemi yoktur.</span><span class="sxs-lookup"><span data-stu-id="378d4-424">There is no way to explicitly free memory allocated using `stackalloc`.</span></span> <span data-ttu-id="378d4-425">İşlev üyesinin yürütülmesi sırasında oluşturulan tüm yığın ayrılmış bellek blokları, bu işlev üyesi döndürüldüğünde otomatik olarak atılır.</span><span class="sxs-lookup"><span data-stu-id="378d4-425">All stack allocated memory blocks created during the execution of a function member are automatically discarded when that function member returns.</span></span> <span data-ttu-id="378d4-426">Bu, C ve C++ uygulamalarında yaygın olarak bulunan `alloca` işlevine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="378d4-426">This corresponds to the `alloca` function, an extension commonly found in C and C++ implementations.</span></span>

<span data-ttu-id="378d4-427">örnekte</span><span class="sxs-lookup"><span data-stu-id="378d4-427">In the example</span></span>

```csharp
using System;

class Test
{
    static string IntToString(int value) {
        int n = value >= 0? value: -value;
        unsafe {
            char* buffer = stackalloc char[16];
            char* p = buffer + 16;
            do {
                *--p = (char)(n % 10 + '0');
                n /= 10;
            } while (n != 0);
            if (value < 0) *--p = '-';
            return new string(p, 0, (int)(buffer + 16 - p));
        }
    }

    static void Main() {
        Console.WriteLine(IntToString(12345));
        Console.WriteLine(IntToString(-999));
    }
}
```

<span data-ttu-id="378d4-428">yığın üzerinde 16 karakterlik bir arabellek ayırmak için `IntToString` yönteminde `stackalloc` başlatıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="378d4-428">a `stackalloc` initializer is used in the `IntToString` method to allocate a buffer of 16 characters on the stack.</span></span> <span data-ttu-id="378d4-429">Yöntem döndürüldüğünde arabellek otomatik olarak atılır.</span><span class="sxs-lookup"><span data-stu-id="378d4-429">The buffer is automatically discarded when the method returns.</span></span>

## <a name="dynamic-memory-allocation"></a><span data-ttu-id="378d4-430">Dinamik bellek ayırma</span><span class="sxs-lookup"><span data-stu-id="378d4-430">Dynamic memory allocation</span></span>

<span data-ttu-id="378d4-431">`stackalloc` işleci dışında, C# atık olmayan toplanan belleği yönetmek için önceden tanımlanmış bir yapı sağlar.</span><span class="sxs-lookup"><span data-stu-id="378d4-431">Except for the `stackalloc` operator, C# provides no predefined constructs for managing non-garbage collected memory.</span></span> <span data-ttu-id="378d4-432">Bu tür hizmetler genellikle sınıf kitaplıklarını destekleyerek veya doğrudan temel işletim sisteminden içeri aktarılarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="378d4-432">Such services are typically provided by supporting class libraries or imported directly from the underlying operating system.</span></span> <span data-ttu-id="378d4-433">Örneğin, aşağıdaki `Memory` sınıfı, temel alınan bir işletim sisteminin yığın işlevlerine nasıl erişilebileceğini göstermektedir C#:</span><span class="sxs-lookup"><span data-stu-id="378d4-433">For example, the `Memory` class below illustrates how the heap functions of an underlying operating system might be accessed from C#:</span></span>

```csharp
using System;
using System.Runtime.InteropServices;

public static unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    private static readonly IntPtr s_heap = GetProcessHeap();

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size)
    {
        void* result = HeapAlloc(s_heap, HEAP_ZERO_MEMORY, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count)
    {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd)
        {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd)
        {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block)
    {
        if (!HeapFree(s_heap, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size)
    {
        void* result = HeapReAlloc(s_heap, HEAP_ZERO_MEMORY, block, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block)
    {
        int result = (int)HeapSize(s_heap, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    private const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    private static extern IntPtr GetProcessHeap();

    [DllImport("kernel32")]
    private static extern void* HeapAlloc(IntPtr hHeap, int flags, UIntPtr size);

    [DllImport("kernel32")]
    private static extern bool HeapFree(IntPtr hHeap, int flags, void* block);

    [DllImport("kernel32")]
    private static extern void* HeapReAlloc(IntPtr hHeap, int flags, void* block, UIntPtr size);

    [DllImport("kernel32")]
    private static extern UIntPtr HeapSize(IntPtr hHeap, int flags, void* block);
}
```

<span data-ttu-id="378d4-434">`Memory` sınıfını kullanan bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="378d4-434">An example that uses the `Memory` class is given below:</span></span>

```csharp
class Test
{
    static unsafe void Main()
    {
        byte* buffer = null;
        try
        {
            const int Size = 256;
            buffer = (byte*)Memory.Alloc(Size);
            for (int i = 0; i < Size; i++) buffer[i] = (byte)i;
            byte[] array = new byte[Size];
            fixed (byte* p = array) Memory.Copy(buffer, p, Size);
            for (int i = 0; i < Size; i++) Console.WriteLine(array[i]);
        }
        finally
        {
            if (buffer != null) Memory.Free(buffer);
        }
    }
}
```

<span data-ttu-id="378d4-435">Örnek, `Memory.Alloc` aracılığıyla 256 baytlık belleği ayırır ve bellek bloğunu 0 ' dan 255 ' e kadar artan değerlerle başlatır.</span><span class="sxs-lookup"><span data-stu-id="378d4-435">The example allocates 256 bytes of memory through `Memory.Alloc` and initializes the memory block with values increasing from 0 to 255.</span></span> <span data-ttu-id="378d4-436">Daha sonra bir 256 öğe bayt dizisi ayırır ve bellek bloğunun içeriğini bayt dizisine kopyalamak için `Memory.Copy` kullanır.</span><span class="sxs-lookup"><span data-stu-id="378d4-436">It then allocates a 256 element byte array and uses `Memory.Copy` to copy the contents of the memory block into the byte array.</span></span> <span data-ttu-id="378d4-437">Son olarak, bellek bloğu `Memory.Free` kullanılarak serbest bırakılır ve byte dizisinin içeriği konsolun çıktılardır.</span><span class="sxs-lookup"><span data-stu-id="378d4-437">Finally, the memory block is freed using `Memory.Free` and the contents of the byte array are output on the console.</span></span>
