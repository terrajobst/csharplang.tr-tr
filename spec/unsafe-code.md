---
ms.openlocfilehash: 4faef9a12bdff54fa59a55a0206fa72bda4ea585
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704057"
---
# <a name="unsafe-code"></a><span data-ttu-id="0ad4d-101">Güvenli olmayan kod</span><span class="sxs-lookup"><span data-stu-id="0ad4d-101">Unsafe code</span></span>

<span data-ttu-id="0ad4d-102">Önceki bölümlerde C# tanımlandığı gibi çekirdek dil, C 'den ve C++ veri türü olarak işaretçilerin atlanından farklıdır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-102">The core C# language, as defined in the preceding chapters, differs notably from C and C++ in its omission of pointers as a data type.</span></span> <span data-ttu-id="0ad4d-103">Bunun yerine C# , başvuruları ve çöp toplayıcı tarafından yönetilen nesneler oluşturma özelliğini sağlar.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-103">Instead, C# provides references and the ability to create objects that are managed by a garbage collector.</span></span> <span data-ttu-id="0ad4d-104">Diğer özelliklerle bağlanmış bu tasarım, C veya C# C++' den çok daha güvenli bir dil sağlar.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-104">This design, coupled with other features, makes C# a much safer language than C or C++.</span></span> <span data-ttu-id="0ad4d-105">Çekirdek C# dilde, başlatılmamış bir değişkene, "Dangling" işaretçisine veya bir diziyi, sınırlarının ötesinde dizinleyen bir ifadeye sahip olmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-105">In the core C# language it is simply not possible to have an uninitialized variable, a "dangling" pointer, or an expression that indexes an array beyond its bounds.</span></span> <span data-ttu-id="0ad4d-106">C ve programları düzenli olarak oluşturan hataların tüm kategorileri bundan C++ sonra ortadan kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-106">Whole categories of bugs that routinely plague C and C++ programs are thus eliminated.</span></span>

<span data-ttu-id="0ad4d-107">Neredeyse her işaretçi türü yapısını C 'de veya C++ içinde C#bir başvuru türüne sahip olsa da, işaretçi türlerine erişimin bir zorunludur hale geldiği durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-107">While practically every pointer type construct in C or C++ has a reference type counterpart in C#, nonetheless, there are situations where access to pointer types becomes a necessity.</span></span> <span data-ttu-id="0ad4d-108">Örneğin, temel alınan işletim sistemiyle arabirim oluşturma, bellek eşlemeli cihaza erişme veya zaman açısından kritik bir algoritma uygulama, işaretçilere erişim olmadan mümkün veya pratik olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-108">For example, interfacing with the underlying operating system, accessing a memory-mapped device, or implementing a time-critical algorithm may not be possible or practical without access to pointers.</span></span> <span data-ttu-id="0ad4d-109">Bu gereksinimi karşılamak için C# ***güvenli olmayan kod***yazma özelliği sağlar.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-109">To address this need, C# provides the ability to write ***unsafe code***.</span></span>

<span data-ttu-id="0ad4d-110">Güvenli olmayan kodda, işaretçiler ve integral türleri arasında dönüştürmeler gerçekleştirmek, değişkenlerin adresini almak için ve benzeri işlemleri yapmak üzere işaretçiler üzerinde bildirmek ve çalıştırmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-110">In unsafe code it is possible to declare and operate on pointers, to perform conversions between pointers and integral types, to take the address of variables, and so forth.</span></span> <span data-ttu-id="0ad4d-111">Bir anlamda, güvenli olmayan kod yazmak bir C# program içinde C kodu yazma gibidir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-111">In a sense, writing unsafe code is much like writing C code within a C# program.</span></span>

<span data-ttu-id="0ad4d-112">Güvenli olmayan kod, geliştiricilerin ve kullanıcıların perspektifinden "güvenli" bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-112">Unsafe code is in fact a "safe" feature from the perspective of both developers and users.</span></span> <span data-ttu-id="0ad4d-113">Güvenli olmayan kod `unsafe` değiştiricisiyle açıkça işaretlenmemelidir, bu nedenle geliştiriciler yanlışlıkla güvenli olmayan özellikleri kullanamaz ve yürütme altyapısı, güvenli olmayan kodun güvenilmeyen bir ortamda yürütülmemesini sağlamak için çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-113">Unsafe code must be clearly marked with the modifier `unsafe`, so developers can't possibly use unsafe features accidentally, and the execution engine works to ensure that unsafe code cannot be executed in an untrusted environment.</span></span>

## <a name="unsafe-contexts"></a><span data-ttu-id="0ad4d-114">Güvenli olmayan bağlamlar</span><span class="sxs-lookup"><span data-stu-id="0ad4d-114">Unsafe contexts</span></span>

<span data-ttu-id="0ad4d-115">Güvenli olmayan özellikleri C# yalnızca güvenli olmayan bağlamlarda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-115">The unsafe features of C# are available only in unsafe contexts.</span></span> <span data-ttu-id="0ad4d-116">Güvenli olmayan bir bağlam, bir tür veya üyenin bildiriminde `unsafe` değiştiricisi eklenerek veya bir *unsafe_statement*kullanarak ortaya çıkartılır:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-116">An unsafe context is introduced by including an `unsafe` modifier in the declaration of a type or member, or by employing an *unsafe_statement*:</span></span>

*  <span data-ttu-id="0ad4d-117">Bir sınıf, yapı, arabirim veya temsilcinin bir bildirimi `unsafe` değiştiricisi içerebilir; bu durumda, bu tür bildiriminin tüm metinsel uzantısı (sınıf, yapı veya arabirim gövdesi dahil) güvenli olmayan bir bağlam olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-117">A declaration of a class, struct, interface, or delegate may include an `unsafe` modifier, in which case the entire textual extent of that type declaration (including the body of the class, struct, or interface) is considered an unsafe context.</span></span>
*  <span data-ttu-id="0ad4d-118">Bir alan, yöntem, özellik, olay, Dizin Oluşturucu, işleç, örnek Oluşturucu, yıkıcı veya statik oluşturucunun bildirimi `unsafe` değiştiricisi içerebilir; bu durumda, bu üye bildiriminin tüm metin kapsamı güvenli olmayan bir bağlam olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-118">A declaration of a field, method, property, event, indexer, operator, instance constructor, destructor, or static constructor may include an `unsafe` modifier, in which case the entire textual extent of that member declaration is considered an unsafe context.</span></span>
*  <span data-ttu-id="0ad4d-119">Bir *unsafe_statement* , *blok*içinde güvenli olmayan bir bağlam kullanılmasına izin vermez.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-119">An *unsafe_statement* enables the use of an unsafe context within a *block*.</span></span> <span data-ttu-id="0ad4d-120">İlişkili *bloğun* tüm metinsel uzantısı güvenli olmayan bir bağlam olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-120">The entire textual extent of the associated *block* is considered an unsafe context.</span></span>

<span data-ttu-id="0ad4d-121">İlişkili dilbilgisi üretimleri aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-121">The associated grammar productions are shown below.</span></span>

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

<span data-ttu-id="0ad4d-122">Örnekte</span><span class="sxs-lookup"><span data-stu-id="0ad4d-122">In the example</span></span>

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

<span data-ttu-id="0ad4d-123">struct bildiriminde belirtilen `unsafe` değiştiricisi, struct bildiriminin tüm metinsel kapsamının güvenli olmayan bir bağlam haline gelmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-123">the `unsafe` modifier specified in the struct declaration causes the entire textual extent of the struct declaration to become an unsafe context.</span></span> <span data-ttu-id="0ad4d-124">Bu nedenle, `Left` ve `Right` alanlarını bir işaretçi türü olarak bildirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-124">Thus, it is possible to declare the `Left` and `Right` fields to be of a pointer type.</span></span> <span data-ttu-id="0ad4d-125">Yukarıdaki örnek de yazılabilir</span><span class="sxs-lookup"><span data-stu-id="0ad4d-125">The example above could also be written</span></span>

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

<span data-ttu-id="0ad4d-126">Burada, alan bildirimlerinde `unsafe` değiştiriciler, bu bildirimlerin güvenli olmayan bağlamlar olarak kabul edilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-126">Here, the `unsafe` modifiers in the field declarations cause those declarations to be considered unsafe contexts.</span></span>

<span data-ttu-id="0ad4d-127">Güvenli olmayan bir bağlam oluşturma dışında, işaretçi türlerinin kullanılmasına izin veren `unsafe` değiştiricisinin bir tür veya üye üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-127">Other than establishing an unsafe context, thus permitting the use of pointer types, the `unsafe` modifier has no effect on a type or a member.</span></span> <span data-ttu-id="0ad4d-128">Örnekte</span><span class="sxs-lookup"><span data-stu-id="0ad4d-128">In the example</span></span>

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

<span data-ttu-id="0ad4d-129">`A` ' deki `F` yönteminde `unsafe` değiştiricisi, `F` ' in metinsel kapsamının, dilin güvenli olmayan özelliklerinin kullanılabileceği güvenli olmayan bir bağlam haline gelmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-129">the `unsafe` modifier on the `F` method in `A` simply causes the textual extent of `F` to become an unsafe context in which the unsafe features of the language can be used.</span></span> <span data-ttu-id="0ad4d-130">@No__t-0 ' ı `B` ' de geçersiz kılmada, `unsafe` değiştiricisini yeniden belirtmeniz gerekmez--kuşkusuz, `B` ' teki `F` yönteminin güvenli olmayan özelliklere erişmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-130">In the override of `F` in `B`, there is no need to re-specify the `unsafe` modifier -- unless, of course, the `F` method in `B` itself needs access to unsafe features.</span></span>

<span data-ttu-id="0ad4d-131">İşaretçi türü metodun imzasının bir parçası olduğunda durum biraz farklıdır</span><span class="sxs-lookup"><span data-stu-id="0ad4d-131">The situation is slightly different when a pointer type is part of the method's signature</span></span>

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

<span data-ttu-id="0ad4d-132">Burada, `F` ' ın imzası bir işaretçi türü içerdiğinden, yalnızca güvenli olmayan bir bağlamda yazılabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-132">Here, because `F`'s signature includes a pointer type, it can only be written in an unsafe context.</span></span> <span data-ttu-id="0ad4d-133">Bununla birlikte, güvenli olmayan bağlam, `A` ' da olduğu gibi, ya da yöntem bildiriminde bir `unsafe` değiştiricisi ekleyerek veya `B` ' de olduğu gibi, tüm sınıfı güvenli hale getirerek eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-133">However, the unsafe context can be introduced by either making the entire class unsafe, as is the case in `A`, or by including an `unsafe` modifier in the method declaration, as is the case in `B`.</span></span>

## <a name="pointer-types"></a><span data-ttu-id="0ad4d-134">İşaretçi türleri</span><span class="sxs-lookup"><span data-stu-id="0ad4d-134">Pointer types</span></span>

<span data-ttu-id="0ad4d-135">Güvenli olmayan bir bağlamda, bir *tür* ([Types](types.md)) bir *pointer_type* ve *value_type* ya da *reference_type*olabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-135">In an unsafe context, a *type* ([Types](types.md)) may be a *pointer_type* as well as a *value_type* or a *reference_type*.</span></span> <span data-ttu-id="0ad4d-136">Ancak, bir *pointer_type* `typeof` Ifadesinde ([anonim nesne oluşturma ifadelerinde](expressions.md#anonymous-object-creation-expressions)), bu kullanım güvenli olmadığı için güvenli olmayan bir bağlam dışında da kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-136">However, a *pointer_type* may also be used in a `typeof` expression ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) outside of an unsafe context as such usage is not unsafe.</span></span>

```antlr
type_unsafe
    : pointer_type
    ;
```

<span data-ttu-id="0ad4d-137">Bir *pointer_type* , bir *unmanaged_type* ya da `void` anahtar sözcüğü ve ardından `*` belirteci ile yazılır:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-137">A *pointer_type* is written as an *unmanaged_type* or the keyword `void`, followed by a `*` token:</span></span>

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

<span data-ttu-id="0ad4d-138">İşaretçi türünde `*` ' dan önce belirtilen tür, işaretçi türünün ***başvurulan türü*** olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-138">The type specified before the `*` in a pointer type is called the ***referent type*** of the pointer type.</span></span> <span data-ttu-id="0ad4d-139">İşaretçi türündeki bir değerin işaret ettiği değişkenin türünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-139">It represents the type of the variable to which a value of the pointer type points.</span></span>

<span data-ttu-id="0ad4d-140">Başvuruların aksine (başvuru türlerinin değerleri), işaretçiler çöp toplayıcı tarafından izlenmez. çöp toplayıcı, hiçbir işaretçi bilgisine ve işaret ettikleri verilere sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-140">Unlike references (values of reference types), pointers are not tracked by the garbage collector -- the garbage collector has no knowledge of pointers and the data to which they point.</span></span> <span data-ttu-id="0ad4d-141">Bu nedenle, bir işaretçiye başvuruya veya başvuruları içeren bir yapıya işaret eden bir işaretçi için bir *unmanaged_type*olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-141">For this reason a pointer is not permitted to point to a reference or to a struct that contains references, and the referent type of a pointer must be an *unmanaged_type*.</span></span>

<span data-ttu-id="0ad4d-142">*Unmanaged_type* , *reference_type* veya oluşturulmuş bir tür olmayan herhangi bir türdür ve herhangi bir iç içe geçme düzeyinde *reference_type* veya oluşturulmuş tür alanları içermez.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-142">An *unmanaged_type* is any type that isn't a *reference_type* or constructed type, and doesn't contain *reference_type* or constructed type fields at any level of nesting.</span></span> <span data-ttu-id="0ad4d-143">Diğer bir deyişle, *unmanaged_type* aşağıdakilerden biridir:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-143">In other words, an *unmanaged_type* is one of the following:</span></span>

*  <span data-ttu-id="0ad4d-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, 0, 1 veya 2.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, or `bool`.</span></span>
*  <span data-ttu-id="0ad4d-145">Herhangi bir *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-145">Any *enum_type*.</span></span>
*  <span data-ttu-id="0ad4d-146">Herhangi bir *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-146">Any *pointer_type*.</span></span>
*  <span data-ttu-id="0ad4d-147">Oluşturulmuş bir tür olmayan ve yalnızca *unmanaged_type*s alanlarını içeren Kullanıcı tanımlı *struct_type* .</span><span class="sxs-lookup"><span data-stu-id="0ad4d-147">Any user-defined *struct_type* that is not a constructed type and contains fields of *unmanaged_type*s only.</span></span>

<span data-ttu-id="0ad4d-148">İşaretçilerin ve başvuruların karıştırılmasına yönelik sezgisel kural, başvuru (nesneler) başvurularının, işaretçiler içermesine izin vermelerdir, ancak işaretçilerin başvurularının başvuru içermesine izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-148">The intuitive rule for mixing of pointers and references is that referents of references (objects) are permitted to contain pointers, but referents of pointers are not permitted to contain references.</span></span>

<span data-ttu-id="0ad4d-149">Aşağıdaki tabloda işaretçi türlerine bazı örnekler verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-149">Some examples of pointer types are given in the table below:</span></span>

| <span data-ttu-id="0ad4d-150">__Örnek__</span><span class="sxs-lookup"><span data-stu-id="0ad4d-150">__Example__</span></span> | <span data-ttu-id="0ad4d-151">__Açıklama__</span><span class="sxs-lookup"><span data-stu-id="0ad4d-151">__Description__</span></span>                               |
|-------------|-----------------------------------------------|
| `byte*`     | <span data-ttu-id="0ad4d-152">@No__t işaretçisi-0</span><span class="sxs-lookup"><span data-stu-id="0ad4d-152">Pointer to `byte`</span></span>                             |
| `char*`     | <span data-ttu-id="0ad4d-153">@No__t işaretçisi-0</span><span class="sxs-lookup"><span data-stu-id="0ad4d-153">Pointer to `char`</span></span>                             |
| `int**`     | <span data-ttu-id="0ad4d-154">@No__t işaretçisinin işaretçisi-0</span><span class="sxs-lookup"><span data-stu-id="0ad4d-154">Pointer to pointer to `int`</span></span>                   |
| `int*[]`    | <span data-ttu-id="0ad4d-155">@No__t işaretçilerin tek boyutlu dizisi-0</span><span class="sxs-lookup"><span data-stu-id="0ad4d-155">Single-dimensional array of pointers to `int`</span></span> |
| `void*`     | <span data-ttu-id="0ad4d-156">Bilinmeyen tür işaretçisi</span><span class="sxs-lookup"><span data-stu-id="0ad4d-156">Pointer to unknown type</span></span>                       |

<span data-ttu-id="0ad4d-157">Belirli bir uygulama için, tüm işaretçi türleri aynı boyuta ve gösterimine sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-157">For a given implementation, all pointer types must have the same size and representation.</span></span>

<span data-ttu-id="0ad4d-158">C ve C++' nin aksine, aynı bildirimde birden çok işaretçi bildirildiğinde, C# `*`, her işaretçi adında ön ek noktalama işareti olarak değil, yalnızca temel alınan türle birlikte yazılır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-158">Unlike C and C++, when multiple pointers are declared in the same declaration, in C# the `*` is written along with the underlying type only, not as a prefix punctuator on each pointer name.</span></span> <span data-ttu-id="0ad4d-159">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-159">For example</span></span>

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

<span data-ttu-id="0ad4d-160">@No__t-0 türüne sahip bir işaretçinin değeri, `T` türünde bir değişkenin adresini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-160">The value of a pointer having type `T*` represents the address of a variable of type `T`.</span></span> <span data-ttu-id="0ad4d-161">Bu değişkene erişmek için, işaretçi yöneltme işleci `*` ([işaretçi yöneltme](unsafe-code.md#pointer-indirection)) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-161">The pointer indirection operator `*` ([Pointer indirection](unsafe-code.md#pointer-indirection)) may be used to access this variable.</span></span> <span data-ttu-id="0ad4d-162">Örneğin, `int*` türünde `P` değişkeni verildiğinde, `*P` ifadesi `P` ' te bulunan adreste bulunan `int` değişkenini gösterir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-162">For example, given a variable `P` of type `int*`, the expression `*P` denotes the `int` variable found at the address contained in `P`.</span></span>

<span data-ttu-id="0ad4d-163">Bir nesne başvurusu gibi bir işaretçi `null` olabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-163">Like an object reference, a pointer may be `null`.</span></span> <span data-ttu-id="0ad4d-164">@No__t-0 işaretçisine yöneltme işlecini uygulamak, uygulama tanımlı davranışa neden olur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-164">Applying the indirection operator to a `null` pointer results in implementation-defined behavior.</span></span> <span data-ttu-id="0ad4d-165">@No__t-0 değeri olan bir işaretçi, All-bit-Zero ile temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-165">A pointer with value `null` is represented by all-bits-zero.</span></span>

<span data-ttu-id="0ad4d-166">@No__t-0 türü, bilinmeyen bir türe yönelik bir işaretçi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-166">The `void*` type represents a pointer to an unknown type.</span></span> <span data-ttu-id="0ad4d-167">Başvurulan tür bilinmediği için, yöneltme işleci `void*` türünde bir işaretçiye uygulanamaz, ne de bu tür bir işaretçi üzerinde herhangi bir aritmetik işlem yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-167">Because the referent type is unknown, the indirection operator cannot be applied to a pointer of type `void*`, nor can any arithmetic be performed on such a pointer.</span></span> <span data-ttu-id="0ad4d-168">Ancak, `void*` türünde bir işaretçi diğer işaretçi türlerine (ve tam tersi) dönüşebilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-168">However, a pointer of type `void*` can be cast to any other pointer type (and vice versa).</span></span>

<span data-ttu-id="0ad4d-169">İşaretçi türleri ayrı bir tür kategorisidir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-169">Pointer types are a separate category of types.</span></span> <span data-ttu-id="0ad4d-170">Başvuru türleri ve değer türlerinin aksine, işaretçi türleri `object` ' dan aktarılmaz ve işaretçi türleri ve `object` arasında dönüştürme yok.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-170">Unlike reference types and value types, pointer types do not inherit from `object` and no conversions exist between pointer types and `object`.</span></span> <span data-ttu-id="0ad4d-171">Özellikle, kutulama ve kutudan çıkarma ([kutulama ve kutudan](types.md#boxing-and-unboxing)çıkarma) işaretçiler için desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-171">In particular, boxing and unboxing ([Boxing and unboxing](types.md#boxing-and-unboxing)) are not supported for pointers.</span></span> <span data-ttu-id="0ad4d-172">Ancak, farklı işaretçi türleri arasında ve işaretçi türleri ile integral türleri arasında dönüştürmeye izin verilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-172">However, conversions are permitted between different pointer types and between pointer types and the integral types.</span></span> <span data-ttu-id="0ad4d-173">Bu, [işaretçi dönüştürmelerinde](unsafe-code.md#pointer-conversions)açıklanır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-173">This is described in [Pointer conversions](unsafe-code.md#pointer-conversions).</span></span>

<span data-ttu-id="0ad4d-174">Bir *pointer_type* , tür bağımsız değişkeni olarak kullanılamaz ([oluşturulmuş türler](types.md#constructed-types)) ve tür çıkarımı ([tür çıkarımı](expressions.md#type-inference)), bir tür bağımsız değişkenini bir işaretçi türü olacak şekilde çıkartılan genel yöntem çağrılarında başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-174">A *pointer_type* cannot be used as a type argument ([Constructed types](types.md#constructed-types)), and type inference ([Type inference](expressions.md#type-inference)) fails on generic method calls that would have inferred a type argument to be a pointer type.</span></span>

<span data-ttu-id="0ad4d-175">Bir *pointer_type* , geçici bir alanın türü olarak kullanılabilir ([geçici alanlar](classes.md#volatile-fields)).</span><span class="sxs-lookup"><span data-stu-id="0ad4d-175">A *pointer_type* may be used as the type of a volatile field ([Volatile fields](classes.md#volatile-fields)).</span></span>

<span data-ttu-id="0ad4d-176">İşaretçiler `ref` veya `out` parametresi olarak geçirilebilse de, işaretçi, çağrılan yöntemin döndürdüğü zaman mevcut olmayan bir yerel değişkene işaret etmek üzere ayarlanmış olabileceği veya işaret etmek için kullandığı sabit nesne olduğu için tanımsız davranışa neden olabilir. , artık düzeltilmedi.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-176">Although pointers can be passed as `ref` or `out` parameters, doing so can cause undefined behavior, since the pointer may well be set to point to a local variable which no longer exists when the called method returns, or the fixed object to which it used to point, is no longer fixed.</span></span> <span data-ttu-id="0ad4d-177">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-177">For example:</span></span>

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

<span data-ttu-id="0ad4d-178">Bir yöntem bir tür değeri döndürebilir ve bu tür bir işaretçi olabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-178">A method can return a value of some type, and that type can be a pointer.</span></span> <span data-ttu-id="0ad4d-179">Örneğin, `int`s ardışık dizisine, bu dizinin öğe sayısına ve diğer `int` değerine bir işaretçi verildiğinde, bir eşleşme meydana gelirse, aşağıdaki yöntem o dizideki bu değerin adresini döndürür; Aksi takdirde, `null` döndürür:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-179">For example, when given a pointer to a contiguous sequence of `int`s, that sequence's element count, and some other `int` value, the following method returns the address of that value in that sequence, if a match occurs; otherwise it returns `null`:</span></span>

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

<span data-ttu-id="0ad4d-180">Güvenli olmayan bir bağlamda, işaretçilerde çalıştırmak için birkaç yapı mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-180">In an unsafe context, several constructs are available for operating on pointers:</span></span>

*  <span data-ttu-id="0ad4d-181">@No__t-0 işleci, işaretçi yöneltme ([işaretçi yöneltme](unsafe-code.md#pointer-indirection)) yapmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-181">The `*` operator may be used to perform pointer indirection ([Pointer indirection](unsafe-code.md#pointer-indirection)).</span></span>
*  <span data-ttu-id="0ad4d-182">@No__t-0 işleci, bir yapının üyesine işaretçi aracılığıyla erişmek için kullanılabilir ([işaretçi üyesi erişimi](unsafe-code.md#pointer-member-access)).</span><span class="sxs-lookup"><span data-stu-id="0ad4d-182">The `->` operator may be used to access a member of a struct through a pointer ([Pointer member access](unsafe-code.md#pointer-member-access)).</span></span>
*  <span data-ttu-id="0ad4d-183">@No__t-0 işleci bir işaretçiye dizin eklemek için kullanılabilir ([işaretçi öğesi erişimi](unsafe-code.md#pointer-element-access)).</span><span class="sxs-lookup"><span data-stu-id="0ad4d-183">The `[]` operator may be used to index a pointer ([Pointer element access](unsafe-code.md#pointer-element-access)).</span></span>
*  <span data-ttu-id="0ad4d-184">@No__t-0 işleci bir değişkenin adresini ([Adres operatörü](unsafe-code.md#the-address-of-operator)) almak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-184">The `&` operator may be used to obtain the address of a variable ([The address-of operator](unsafe-code.md#the-address-of-operator)).</span></span>
*  <span data-ttu-id="0ad4d-185">@No__t-0 ve `--` işleçleri, işaretçileri artırmak ve azaltmak için kullanılabilir ([işaretçi artışı ve azalması](unsafe-code.md#pointer-increment-and-decrement)).</span><span class="sxs-lookup"><span data-stu-id="0ad4d-185">The `++` and `--` operators may be used to increment and decrement pointers ([Pointer increment and decrement](unsafe-code.md#pointer-increment-and-decrement)).</span></span>
*  <span data-ttu-id="0ad4d-186">@No__t-0 ve `-` işleçleri, işaretçi aritmetiği ([işaretçi aritmetiği](unsafe-code.md#pointer-arithmetic)) gerçekleştirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-186">The `+` and `-` operators may be used to perform pointer arithmetic ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span>
*  <span data-ttu-id="0ad4d-187">@No__t-0, `!=`, `<`, `>`, `<=` ve `=>` işleçleri işaretçileri karşılaştırmak için kullanılabilir ([işaretçi karşılaştırması](unsafe-code.md#pointer-comparison)).</span><span class="sxs-lookup"><span data-stu-id="0ad4d-187">The `==`, `!=`, `<`, `>`, `<=`, and `=>` operators may be used to compare pointers ([Pointer comparison](unsafe-code.md#pointer-comparison)).</span></span>
*  <span data-ttu-id="0ad4d-188">@No__t-0 işleci, çağrı yığınından ([sabit boyutlu arabellekler](unsafe-code.md#fixed-size-buffers)) bellek ayırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-188">The `stackalloc` operator may be used to allocate memory from the call stack ([Fixed size buffers](unsafe-code.md#fixed-size-buffers)).</span></span>
*  <span data-ttu-id="0ad4d-189">@No__t-0 deyimleri, bir değişkeni geçici olarak çözmek için kullanılabilir ve bu nedenle adresi elde edilebilir ([fixed deyimidir](unsafe-code.md#the-fixed-statement)).</span><span class="sxs-lookup"><span data-stu-id="0ad4d-189">The `fixed` statement may be used to temporarily fix a variable so its address can be obtained ([The fixed statement](unsafe-code.md#the-fixed-statement)).</span></span>

## <a name="fixed-and-moveable-variables"></a><span data-ttu-id="0ad4d-190">Sabit ve taşınabilir değişkenler</span><span class="sxs-lookup"><span data-stu-id="0ad4d-190">Fixed and moveable variables</span></span>

<span data-ttu-id="0ad4d-191">Address-of işleci ([Address-of işleci](unsafe-code.md#the-address-of-operator)) ve `fixed` deyimin ([fixed deyimin](unsafe-code.md#the-fixed-statement)) değişkenleri iki kategoriye ayırır: ***Sabit değişkenler*** ve ***taşınamayacak değişkenler***.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-191">The address-of operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) and the `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) divide variables into two categories: ***Fixed variables*** and ***moveable variables***.</span></span>

<span data-ttu-id="0ad4d-192">Sabit değişkenler çöp toplayıcı 'nin işleminden etkilenmeyen depolama konumlarında bulunur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-192">Fixed variables reside in storage locations that are unaffected by operation of the garbage collector.</span></span> <span data-ttu-id="0ad4d-193">(Sabit değişkenlere örnek olarak, yerel değişkenler, değer parametreleri ve başvuru işaretçileri tarafından oluşturulan değişkenler verilebilir.) Diğer yandan, taşınabilir değişkenler çöp toplayıcısının yerini değiştirme veya aktiften çıkarma konusunda yer alan depolama konumlarında bulunur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-193">(Examples of fixed variables include local variables, value parameters, and variables created by dereferencing pointers.) On the other hand, moveable variables reside in storage locations that are subject to relocation or disposal by the garbage collector.</span></span> <span data-ttu-id="0ad4d-194">(Taşınamayacak değişkenlerin örnekleri, nesne ve dizi öğelerindeki alanları içerir.)</span><span class="sxs-lookup"><span data-stu-id="0ad4d-194">(Examples of moveable variables include fields in objects and elements of arrays.)</span></span>

<span data-ttu-id="0ad4d-195">@No__t-0 işleci ([Adres operatörü](unsafe-code.md#the-address-of-operator)), sabit bir değişkenin adresinin kısıtlama olmadan elde edilebilir olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-195">The `&` operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) permits the address of a fixed variable to be obtained without restrictions.</span></span> <span data-ttu-id="0ad4d-196">Ancak, taşınabilir bir değişken çöp toplayıcısının yeniden konumlandırılmasını veya aktiften çıkarılma tabi olduğundan, taşınabilir bir değişkenin adresi yalnızca `fixed` ([fixed](unsafe-code.md#the-fixed-statement)) ifadesiyle elde edilebilir ve bu adres yalnızca Bu `fixed` ifadesinin süresi.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-196">However, because a moveable variable is subject to relocation or disposal by the garbage collector, the address of a moveable variable can only be obtained using a `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)), and that address remains valid only for the duration of that `fixed` statement.</span></span>

<span data-ttu-id="0ad4d-197">Kesin koşullarda, sabit bir değişken aşağıdakilerden biridir:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-197">In precise terms, a fixed variable is one of the following:</span></span>

*  <span data-ttu-id="0ad4d-198">Değişken anonim bir işlev tarafından yakalanmadığı müddetçe, bir yerel değişkene veya bir değer parametresine başvuran bir *simple_name* ([basit adlar](expressions.md#simple-names)) sonucu elde edilen bir değişkendir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-198">A variable resulting from a *simple_name* ([Simple names](expressions.md#simple-names)) that refers to a local variable or a value parameter, unless the variable is captured by an anonymous function.</span></span>
*  <span data-ttu-id="0ad4d-199">@No__t-2 ' nin bir *member_access* ([üye erişimi](expressions.md#member-access)) sonucu olan bir değişken, `V` ' ün bir *struct_type*sabit değişkenidir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-199">A variable resulting from a *member_access* ([Member access](expressions.md#member-access)) of the form `V.I`, where `V` is a fixed variable of a *struct_type*.</span></span>
*  <span data-ttu-id="0ad4d-200">Bir değişken, formun @no__t -2, bir *pointer_member_access* ([işaretçi üyesi erişimi](unsafe-code.md#pointer-member-access)[) @no__t](unsafe-code.md#pointer-indirection)-5 veya bir *pointer_element_access* ( [İşaretçi öğesi erişimi](unsafe-code.md#pointer-element-access)) `P[E]` biçiminde.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-200">A variable resulting from a *pointer_indirection_expression* ([Pointer indirection](unsafe-code.md#pointer-indirection)) of the form `*P`, a *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) of the form `P->I`, or a *pointer_element_access* ([Pointer element access](unsafe-code.md#pointer-element-access)) of the form `P[E]`.</span></span>

<span data-ttu-id="0ad4d-201">Diğer tüm değişkenler taşınabilir değişkenler olarak sınıflandırılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-201">All other variables are classified as moveable variables.</span></span>

<span data-ttu-id="0ad4d-202">Statik bir alanın taşınamayacak değişken olarak sınıflandırıldığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-202">Note that a static field is classified as a moveable variable.</span></span> <span data-ttu-id="0ad4d-203">Ayrıca, parametre için verilen bağımsız değişken sabit bir değişken olsa bile `ref` veya `out` parametresinin taşınabilir bir değişken olarak sınıflandırıldığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-203">Also note that a `ref` or `out` parameter is classified as a moveable variable, even if the argument given for the parameter is a fixed variable.</span></span> <span data-ttu-id="0ad4d-204">Son olarak, bir işaretçinin başvurusunun kaldırılması tarafından oluşturulan bir değişkenin her zaman sabit bir değişken olarak sınıflandırıldığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-204">Finally, note that a variable produced by dereferencing a pointer is always classified as a fixed variable.</span></span>

## <a name="pointer-conversions"></a><span data-ttu-id="0ad4d-205">İşaretçi Dönüştürmeler</span><span class="sxs-lookup"><span data-stu-id="0ad4d-205">Pointer conversions</span></span>

<span data-ttu-id="0ad4d-206">Güvenli olmayan bir bağlamda, kullanılabilir örtük dönüştürmeler ([örtük dönüştürmeler](conversions.md#implicit-conversions)) kümesi aşağıdaki örtük işaretçi dönüşümlerini içerecek şekilde genişletilir:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-206">In an unsafe context, the set of available implicit conversions ([Implicit conversions](conversions.md#implicit-conversions)) is extended to include the following implicit pointer conversions:</span></span>

*  <span data-ttu-id="0ad4d-207">Herhangi bir *pointer_type* `void*` türüne.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-207">From any *pointer_type* to the type `void*`.</span></span>
*  <span data-ttu-id="0ad4d-208">@No__t-0 değişmez değerinden herhangi bir *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-208">From the `null` literal to any *pointer_type*.</span></span>

<span data-ttu-id="0ad4d-209">Ayrıca, güvenli olmayan bir bağlamda, kullanılabilir açık dönüştürmeler ([Açık dönüştürmeler](conversions.md#explicit-conversions)) kümesi aşağıdaki açık işaretçi dönüşümlerini içerecek şekilde genişletilir:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-209">Additionally, in an unsafe context, the set of available explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) is extended to include the following explicit pointer conversions:</span></span>

*  <span data-ttu-id="0ad4d-210">Herhangi bir *pointer_type* başka bir *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-210">From any *pointer_type* to any other *pointer_type*.</span></span>
*  <span data-ttu-id="0ad4d-211">@No__t-0, `byte`, `short`, `ushort`, `int`, `uint`, `long` veya `ulong` herhangi bir *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-211">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong` to any *pointer_type*.</span></span>
*  <span data-ttu-id="0ad4d-212">Herhangi bir *pointer_type* `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long` ya da `ulong` ' e kadar.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-212">From any *pointer_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="0ad4d-213">Son olarak, güvenli olmayan bir bağlamda, standart örtük dönüştürmeler kümesi ([Standart örtük dönüştürmeler](conversions.md#standard-implicit-conversions)) aşağıdaki işaretçi dönüşümünü içerir:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-213">Finally, in an unsafe context, the set of standard implicit conversions ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) includes the following pointer conversion:</span></span>

*  <span data-ttu-id="0ad4d-214">Herhangi bir *pointer_type* `void*` türüne.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-214">From any *pointer_type* to the type `void*`.</span></span>

<span data-ttu-id="0ad4d-215">İki işaretçi türü arasındaki dönüşümler hiçbir şekilde gerçek işaretçi değerini değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-215">Conversions between two pointer types never change the actual pointer value.</span></span> <span data-ttu-id="0ad4d-216">Diğer bir deyişle, bir işaretçi türünden diğerine dönüştürmenin, işaretçi tarafından verilen temel adres üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-216">In other words, a conversion from one pointer type to another has no effect on the underlying address given by the pointer.</span></span>

<span data-ttu-id="0ad4d-217">Bir işaretçi türü diğerine dönüştürüldüğünde, ortaya çıkan işaretçi, işaret türü için doğru hizalanmazsa, sonuç başvuru başvurusu varsa, bu davranış tanımsızdır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-217">When one pointer type is converted to another, if the resulting pointer is not correctly aligned for the pointed-to type, the behavior is undefined if the result is dereferenced.</span></span> <span data-ttu-id="0ad4d-218">Genel olarak, "doğru hizalı" kavram geçişlidir: `A` türüne yönelik bir işaretçi, `B` türü bir işaretçi için doğru hizalanmışsa, bu, sırasıyla `C` türünde bir işaretçi için doğru hizalanmışsa, bu durumda `A` türü bir işaretçi bir `C` türü işaretçi.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-218">In general, the concept "correctly aligned" is transitive: if a pointer to type `A` is correctly aligned for a pointer to type `B`, which, in turn, is correctly aligned for a pointer to type `C`, then a pointer to type `A` is correctly aligned for a pointer to type `C`.</span></span>

<span data-ttu-id="0ad4d-219">Farklı bir türe yönelik bir işaretçi aracılığıyla bir türe sahip bir değişkene erişildiği aşağıdaki durumu göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-219">Consider the following case in which a variable having one type is accessed via a pointer to a different type:</span></span>

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

<span data-ttu-id="0ad4d-220">Bir işaretçi türü bayta bir işaretçiye dönüştürüldüğünde, sonuç değişkenin en düşük adresli bayta işaret eder.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-220">When a pointer type is converted to a pointer to byte, the result points to the lowest addressed byte of the variable.</span></span> <span data-ttu-id="0ad4d-221">Sonucun ardışık artışlarına kadar, değişkenin boyutuna kadar, bu değişkenin kalan baytlarına kadar dönen işaretçiler.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-221">Successive increments of the result, up to the size of the variable, yield pointers to the remaining bytes of that variable.</span></span> <span data-ttu-id="0ad4d-222">Örneğin, aşağıdaki yöntem bir Double içindeki sekiz baytın her birini onaltılık bir değer olarak görüntüler:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-222">For example, the following method displays each of the eight bytes in a double as a hexadecimal value:</span></span>

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

<span data-ttu-id="0ad4d-223">Tabii ki, üretilen çıkış, bitime 'ye bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-223">Of course, the output produced depends on endianness.</span></span>

<span data-ttu-id="0ad4d-224">İşaretçiler ve tamsayılar arasındaki eşlemeler uygulama tanımlı.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-224">Mappings between pointers and integers are implementation-defined.</span></span> <span data-ttu-id="0ad4d-225">Bununla birlikte, doğrusal bir adres alanı ile 32 \* ve 64 bit CPU mimarilerinde, tam sayı türlerine veya bu tür bir değere yapılan işaretçilerin dönüştürmeleri genellikle `uint` veya `ulong` değerlerinin, sırasıyla, bu integral türlerine veya bu tür integral türlerine dönüştürülmesine benzer şekilde davranır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-225">However, on 32\* and 64-bit CPU architectures with a linear address space, conversions of pointers to or from integral types typically behave exactly like conversions of `uint` or `ulong` values, respectively, to or from those integral types.</span></span>

### <a name="pointer-arrays"></a><span data-ttu-id="0ad4d-226">İşaretçi dizileri</span><span class="sxs-lookup"><span data-stu-id="0ad4d-226">Pointer arrays</span></span>

<span data-ttu-id="0ad4d-227">Güvenli olmayan bir bağlamda, işaretçiler dizileri oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-227">In an unsafe context, arrays of pointers can be constructed.</span></span> <span data-ttu-id="0ad4d-228">İşaretçi dizileri üzerinde yalnızca diğer dizi türleri için uygulanan Dönüştürmelere izin verilir:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-228">Only some of the conversions that apply to other array types are allowed on pointer arrays:</span></span>

*  <span data-ttu-id="0ad4d-229">Herhangi bir *array_type* 'den `System.Array` ' ye örtük başvuru dönüştürmesi ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions)) ve onun uyguladığı arabirimler, işaretçi dizileri için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-229">The implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) from any *array_type* to `System.Array` and the interfaces it implements also applies to pointer arrays.</span></span> <span data-ttu-id="0ad4d-230">Ancak, dizi öğelerine `System.Array` veya uyguladığı arabirimler aracılığıyla erişmek için herhangi bir girişim, çalışma zamanında bir özel durum oluşmasına neden olur, çünkü işaretçi türleri `object` ' e dönüştürülemez.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-230">However, any attempt to access the array elements through `System.Array` or the interfaces it implements will result in an exception at run-time, as pointer types are not convertible to `object`.</span></span>
*  <span data-ttu-id="0ad4d-231">Tek boyutlu dizi türündeki örtük ve açık başvuru dönüştürmeleri ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions), [açık başvuru dönüştürmeleri](conversions.md#explicit-reference-conversions)) `S[]` ' ye `System.Collections.Generic.IList<T>` ve genel temel arabirimleri hiçbir şekilde işaretçi dizileri için uygulanmaz. işaretçi türleri tür bağımsız değişkenleri olarak kullanılakullanılamadığından ve işaretçi türlerinden işaretçi olmayan türlere dönüştürme yok.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-231">The implicit and explicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions), [Explicit reference conversions](conversions.md#explicit-reference-conversions)) from a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its generic base interfaces never apply to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>
*  <span data-ttu-id="0ad4d-232">@No__t-1 ' den açık başvuru dönüştürmesi ([açık başvuru dönüştürmeleri](conversions.md#explicit-reference-conversions)) ve herhangi bir *array_type* için uyguladığı arabirimler işaretçi dizileri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-232">The explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Array` and the interfaces it implements to any *array_type* applies to pointer arrays.</span></span>
*  <span data-ttu-id="0ad4d-233">@No__t-1 ' den ve temel arabirimlerinden `T[]` ' den tek boyutlu dizi türüne açık başvuru dönüştürmeleri ([açık başvuru dönüştürmeleri](conversions.md#explicit-reference-conversions)), işaretçi türleri tür bağımsız değişkenleri olarak kullanılmadığından ve bu öğe işaretçi türünden işaretçi olmayan türlere dönüştürme yok.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-233">The explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]` never applies to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>

<span data-ttu-id="0ad4d-234">Bu kısıtlamalar, [foreach ifadesinde](statements.md#the-foreach-statement) açıklanan diziler üzerinde `foreach` ifadesinin genişlemesinin işaretçi dizilerine uygulanamadığını ifade ediyor.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-234">These restrictions mean that the expansion for the `foreach` statement over arrays described in [The foreach statement](statements.md#the-foreach-statement) cannot be applied to pointer arrays.</span></span> <span data-ttu-id="0ad4d-235">Bunun yerine, formun foreach ifadesi</span><span class="sxs-lookup"><span data-stu-id="0ad4d-235">Instead, a foreach statement of the form</span></span>

```csharp
foreach (V v in x) embedded_statement
```

<span data-ttu-id="0ad4d-236">`x` türünün türü `T[,,...,]` biçiminde bir dizi türüdür, `N` boyut sayısı eksi 1 ve `T` ya da `V` bir işaretçi türü, iç içe for-döngüleri kullanılarak aşağıdaki şekilde genişletilir:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-236">where the type of `x` is an array type of the form `T[,,...,]`, `N` is the number of dimensions minus 1 and `T` or `V` is a pointer type, is expanded using nested for-loops as follows:</span></span>

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

<span data-ttu-id="0ad4d-237">@No__t-0, `i0`, `i1`,..., `iN`, `x` veya *embedded_statement* ya da programın herhangi bir kaynak kodu için görünür veya erişilebilir değildir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-237">The variables `a`, `i0`, `i1`, ..., `iN` are not visible to or accessible to `x` or the *embedded_statement* or any other source code of the program.</span></span> <span data-ttu-id="0ad4d-238">Değişken `v` , gömülü ifadede salt okunurdur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-238">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="0ad4d-239">@No__t-1 ' den (öğe türü) `V` ' ye açık bir dönüştürme ([işaretçi dönüşümleri](unsafe-code.md#pointer-conversions)) yoksa, bir hata oluşturulur ve başka bir adım alınmaz.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-239">If there is not an explicit conversion ([Pointer conversions](unsafe-code.md#pointer-conversions)) from `T` (the element type) to `V`, an error is produced and no further steps are taken.</span></span> <span data-ttu-id="0ad4d-240">`System.NullReferenceException` Değeri `x` varsa`null`, çalışma zamanında bir oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-240">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

## <a name="pointers-in-expressions"></a><span data-ttu-id="0ad4d-241">İfadelerdeki işaretçiler</span><span class="sxs-lookup"><span data-stu-id="0ad4d-241">Pointers in expressions</span></span>

<span data-ttu-id="0ad4d-242">Güvenli olmayan bir bağlamda, bir ifade bir işaretçi türü sonucunu verebilir, ancak güvenli olmayan bir bağlam dışında bir ifadenin işaretçi türü olması için derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-242">In an unsafe context, an expression may yield a result of a pointer type, but outside an unsafe context it is a compile-time error for an expression to be of a pointer type.</span></span> <span data-ttu-id="0ad4d-243">Kesin koşullarda, güvenli olmayan bir bağlam dışında, *simple_name* ([basit adlar](expressions.md#simple-names)), *member_access* ([üye erişimi](expressions.md#member-access)), *invocation_expression* ([çağırma ifadeleri](expressions.md#invocation-expressions)) veya  *element_access* ([öğe erişimi](expressions.md#element-access)) bir işaretçi türü.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-243">In precise terms, outside an unsafe context a compile-time error occurs if any *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)), or *element_access* ([Element access](expressions.md#element-access)) is of a pointer type.</span></span>

<span data-ttu-id="0ad4d-244">Güvenli olmayan bir bağlamda, *primary_no_array_creation_expression* ([birincil ifadeler](expressions.md#primary-expressions)) ve *unary_expression* ([Birli İşleçler](expressions.md#unary-operators)) üretimleri aşağıdaki ek yapılara izin verir:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-244">In an unsafe context, the *primary_no_array_creation_expression* ([Primary expressions](expressions.md#primary-expressions)) and *unary_expression* ([Unary operators](expressions.md#unary-operators)) productions permit the following additional constructs:</span></span>

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

<span data-ttu-id="0ad4d-245">Bu yapılar aşağıdaki bölümlerde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-245">These constructs are described in the following sections.</span></span> <span data-ttu-id="0ad4d-246">Güvenli olmayan işleçlerin önceliği ve ilişkilendirilebilirliği, dilbilgisi tarafından kapsanıyor.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-246">The precedence and associativity of the unsafe operators is implied by the grammar.</span></span>

### <a name="pointer-indirection"></a><span data-ttu-id="0ad4d-247">İşaretçi yöneltme</span><span class="sxs-lookup"><span data-stu-id="0ad4d-247">Pointer indirection</span></span>

<span data-ttu-id="0ad4d-248">Bir *pointer_indirection_expression* , bir yıldız işareti (`*`) ve arkasından bir *unary_expression*oluşur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-248">A *pointer_indirection_expression* consists of an asterisk (`*`) followed by a *unary_expression*.</span></span>

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

<span data-ttu-id="0ad4d-249">Birli `*` işleci işaretçi yöneltme 'yi gösterir ve bir işaretçinin işaret ettiği değişkeni elde etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-249">The unary `*` operator denotes pointer indirection and is used to obtain the variable to which a pointer points.</span></span> <span data-ttu-id="0ad4d-250">@No__t-0 ' ın değerlendirilmasının sonucu; burada `P`, `T*` işaretçisi türünün bir ifadesidir, `T` türünde bir değişkendir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-250">The result of evaluating `*P`, where `P` is an expression of a pointer type `T*`, is a variable of type `T`.</span></span> <span data-ttu-id="0ad4d-251">Birli `*` işlecini `void*` türünde bir ifadeye veya işaretçi türünde olmayan bir ifadeye uygulamak için derleme zamanı hatası.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-251">It is a compile-time error to apply the unary `*` operator to an expression of type `void*` or to an expression that isn't of a pointer type.</span></span>

<span data-ttu-id="0ad4d-252">Birli `*` işlecini `null` işaretçisine uygulamanın etkisi, uygulama tanımlı ' dır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-252">The effect of applying the unary `*` operator to a `null` pointer is implementation-defined.</span></span> <span data-ttu-id="0ad4d-253">Özellikle, bu işlemin bir @no__t (0) oluşturduğunda garanti yoktur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-253">In particular, there is no guarantee that this operation throws a `System.NullReferenceException`.</span></span>

<span data-ttu-id="0ad4d-254">İşaretçiye geçersiz bir değer atanmışsa, birli `*` işlecinin davranışı tanımsız olur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-254">If an invalid value has been assigned to the pointer, the behavior of the unary `*` operator is undefined.</span></span> <span data-ttu-id="0ad4d-255">Birli `*` işleci tarafından bir işaretçinin başvurusunun kaldırılması için geçersiz değerler arasında, işaret edilen tür için uygun şekilde hizalı bir adrestir (bkz. [işaretçi dönüştürmelerinde](unsafe-code.md#pointer-conversions)örnek) ve yaşam süresinin sonundan sonraki bir değişkenin adresi.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-255">Among the invalid values for dereferencing a pointer by the unary `*` operator are an address inappropriately aligned for the type pointed to (see example in [Pointer conversions](unsafe-code.md#pointer-conversions)), and the address of a variable after the end of its lifetime.</span></span>

<span data-ttu-id="0ad4d-256">Kesin atama analizinin amaçları doğrultusunda, `*P` biçiminde bir ifade hesaplanarak üretilen bir değişken başlangıçta atanan ([Başlangıçta atanan değişkenler](variables.md#initially-assigned-variables)) olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-256">For purposes of definite assignment analysis, a variable produced by evaluating an expression of the form `*P` is considered initially assigned ([Initially assigned variables](variables.md#initially-assigned-variables)).</span></span>

### <a name="pointer-member-access"></a><span data-ttu-id="0ad4d-257">İşaretçi üye erişimi</span><span class="sxs-lookup"><span data-stu-id="0ad4d-257">Pointer member access</span></span>

<span data-ttu-id="0ad4d-258">Bir *pointer_member_access* , sonrasında bir *primary_expression*ve ardından bir "`->`" belirteci ve ardından bir *tanımlayıcı* ve isteğe bağlı *type_argument_list*oluşur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-258">A *pointer_member_access* consists of a *primary_expression*, followed by a "`->`" token, followed by an *identifier* and an optional *type_argument_list*.</span></span>

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

<span data-ttu-id="0ad4d-259">@No__t-0 ' a bir işaretçi üye erişimi `P` ' in `void*` dışındaki bir işaretçi türünün bir ifadesi olması gerekir ve `I` ' ün, `P` noktaları olan erişilebilir bir üyeyi belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-259">In a pointer member access of the form `P->I`, `P` must be an expression of a pointer type other than `void*`, and `I` must denote an accessible member of the type to which `P` points.</span></span>

<span data-ttu-id="0ad4d-260">@No__t-0 biçiminde bir işaretçi üyesi erişimi, tam olarak `(*P).I` olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-260">A pointer member access of the form `P->I` is evaluated exactly as `(*P).I`.</span></span> <span data-ttu-id="0ad4d-261">İşaretçi yöneltme işlecinin (`*`) bir açıklaması için bkz. [işaretçi yöneltme](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="0ad4d-261">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="0ad4d-262">Üye erişim işlecinin (`.`) açıklaması için bkz. [üye erişimi](expressions.md#member-access).</span><span class="sxs-lookup"><span data-stu-id="0ad4d-262">For a description of the member access operator (`.`), see [Member access](expressions.md#member-access).</span></span>

<span data-ttu-id="0ad4d-263">Örnekte</span><span class="sxs-lookup"><span data-stu-id="0ad4d-263">In the example</span></span>

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

<span data-ttu-id="0ad4d-264">`->` işleci, alanlara erişmek ve bir yapı yöntemini işaretçi aracılığıyla çağırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-264">the `->` operator is used to access fields and invoke a method of a struct through a pointer.</span></span> <span data-ttu-id="0ad4d-265">@No__t-0 işlemi tam olarak `(*P).I` ' e eşit olduğundan, `Main` yöntemi eşit olarak de yazılabilir:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-265">Because the operation `P->I` is precisely equivalent to `(*P).I`, the `Main` method could equally well have been written:</span></span>

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

### <a name="pointer-element-access"></a><span data-ttu-id="0ad4d-266">İşaretçi öğesi erişimi</span><span class="sxs-lookup"><span data-stu-id="0ad4d-266">Pointer element access</span></span>

<span data-ttu-id="0ad4d-267">*Pointer_element_access* , bir *primary_no_array_creation_expression* ve ardından "`[`" ve "`]`" olarak gelen bir ifade içerir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-267">A *pointer_element_access* consists of a *primary_no_array_creation_expression* followed by an expression enclosed in "`[`" and "`]`".</span></span>

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

<span data-ttu-id="0ad4d-268">@No__t-0 ' a bir işaretçi öğesi erişiminin `P` `void*` dışındaki bir işaretçi türünün bir ifadesi olması ve `E` ' ün örtük olarak `int`, `uint`, `long` veya `ulong` ' ye dönüştürülebileceği bir ifade olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-268">In a pointer element access of the form `P[E]`, `P` must be an expression of a pointer type other than `void*`, and `E` must be an expression that can be implicitly converted to `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="0ad4d-269">@No__t-0 biçiminde bir işaretçi öğesi erişimi, tam olarak `*(P + E)` olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-269">A pointer element access of the form `P[E]` is evaluated exactly as `*(P + E)`.</span></span> <span data-ttu-id="0ad4d-270">İşaretçi yöneltme işlecinin (`*`) bir açıklaması için bkz. [işaretçi yöneltme](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="0ad4d-270">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="0ad4d-271">İşaretçi ek işlecinin (`+`) açıklaması için bkz. [işaretçi aritmetiği](unsafe-code.md#pointer-arithmetic).</span><span class="sxs-lookup"><span data-stu-id="0ad4d-271">For a description of the pointer addition operator (`+`), see [Pointer arithmetic](unsafe-code.md#pointer-arithmetic).</span></span>

<span data-ttu-id="0ad4d-272">Örnekte</span><span class="sxs-lookup"><span data-stu-id="0ad4d-272">In the example</span></span>

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

<span data-ttu-id="0ad4d-273">bir işaretçi öğesi erişimi, `for` döngüsünde karakter arabelleğini başlatmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-273">a pointer element access is used to initialize the character buffer in a `for` loop.</span></span> <span data-ttu-id="0ad4d-274">@No__t-0 işlemi tam olarak `*(P + E)` ' e eşit olduğundan, örnek de eşit olarak yazılabilir:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-274">Because the operation `P[E]` is precisely equivalent to `*(P + E)`, the example could equally well have been written:</span></span>

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

<span data-ttu-id="0ad4d-275">İşaretçi öğesi erişim işleci, sınırların dışı hataları denetlemez ve bir sınır dışı öğeye erişirken bu davranış tanımlı değildir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-275">The pointer element access operator does not check for out-of-bounds errors and the behavior when accessing an out-of-bounds element is undefined.</span></span> <span data-ttu-id="0ad4d-276">Bu, C ve ile C++aynıdır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-276">This is the same as C and C++.</span></span>

### <a name="the-address-of-operator"></a><span data-ttu-id="0ad4d-277">Address-of işleci</span><span class="sxs-lookup"><span data-stu-id="0ad4d-277">The address-of operator</span></span>

<span data-ttu-id="0ad4d-278">Bir *addressof_expression* , ve sonrasında bir *unary_expression*(`&`) oluşur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-278">An *addressof_expression* consists of an ampersand (`&`) followed by a *unary_expression*.</span></span>

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

<span data-ttu-id="0ad4d-279">@No__t-1 türünde olan ve sabit bir değişken ([sabit ve taşınabilir değişkenler](unsafe-code.md#fixed-and-moveable-variables)) olarak sınıflandırılan `E` ifadesi verildiğinde, `&E` yapısı `E` tarafından verilen değişkenin adresini hesaplar.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-279">Given an expression `E` which is of a type `T` and is classified as a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), the construct `&E` computes the address of the variable given by `E`.</span></span> <span data-ttu-id="0ad4d-280">Sonucun türü `T*` ' dır ve bir değer olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-280">The type of the result is `T*` and is classified as a value.</span></span> <span data-ttu-id="0ad4d-281">@No__t-0 bir değişken olarak sınıflandırılmayabilir, `E` bir salt okunurdur yerel değişken olarak sınıflandırıldığında veya `E` taşınabilir bir değişkeni alıyorsa, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-281">A compile-time error occurs if `E` is not classified as a variable, if `E` is classified as a read-only local variable, or if `E` denotes a moveable variable.</span></span> <span data-ttu-id="0ad4d-282">Son durumda, bir sabit ifade ([fixed deyimidir](unsafe-code.md#the-fixed-statement)), adresini almadan önce değişkeni geçici olarak "onarmak" için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-282">In the last case, a fixed statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) can be used to temporarily "fix" the variable before obtaining its address.</span></span> <span data-ttu-id="0ad4d-283">[Üye erişimde](expressions.md#member-access)belirtildiği gibi, bir örnek oluşturucu veya `readonly` alanını tanımlayan bir sınıf için statik oluşturucu dışında, bu alan değişken değil değer olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-283">As stated in [Member access](expressions.md#member-access), outside an instance constructor or static constructor for a struct or class that defines a `readonly` field, that field is considered a value, not a variable.</span></span> <span data-ttu-id="0ad4d-284">Bu nedenle, adresi alınamaz.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-284">As such, its address cannot be taken.</span></span> <span data-ttu-id="0ad4d-285">Benzer şekilde, bir sabit adresi alınamaz.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-285">Similarly, the address of a constant cannot be taken.</span></span>

<span data-ttu-id="0ad4d-286">@No__t-0 işleci, bağımsız değişkeninin kesinlikle atanmasını gerektirmez, ancak bir `&` işleminin ardından, işlecin uygulandığı değişken, işlemin gerçekleştiği yürütme yolunda kesin olarak atanır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-286">The `&` operator does not require its argument to be definitely assigned, but following an `&` operation, the variable to which the operator is applied is considered definitely assigned in the execution path in which the operation occurs.</span></span> <span data-ttu-id="0ad4d-287">Bu durumda, değişkenin doğru başlatılmasının gerçekten gerçekleşmekte olduğundan emin olmak için programcı sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-287">It is the responsibility of the programmer to ensure that correct initialization of the variable actually does take place in this situation.</span></span>

<span data-ttu-id="0ad4d-288">Örnekte</span><span class="sxs-lookup"><span data-stu-id="0ad4d-288">In the example</span></span>

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

<span data-ttu-id="0ad4d-289">`i`, `p` ' i başlatmak için kullanılan `&i` işleminin ardından kesinlikle atanmış olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-289">`i` is considered definitely assigned following the `&i` operation used to initialize `p`.</span></span> <span data-ttu-id="0ad4d-290">@No__t-0 ' a atama, `i` ' i başlatır, ancak bu başlatmanın eklenmesi programcının sorumluluğundadır ve atama kaldırılırsa hiçbir derleme zamanı hatası oluşmaz.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-290">The assignment to `*p` in effect initializes `i`, but the inclusion of this initialization is the responsibility of the programmer, and no compile-time error would occur if the assignment was removed.</span></span>

<span data-ttu-id="0ad4d-291">@No__t-0 işlecinin kesin atama kuralları, yerel değişkenlerin gereksiz şekilde başlatılmasına karşı kaçınılabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-291">The rules of definite assignment for the `&` operator exist such that redundant initialization of local variables can be avoided.</span></span> <span data-ttu-id="0ad4d-292">Örneğin, birçok harici API, API tarafından doldurulan bir yapıya bir işaretçi alır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-292">For example, many external APIs take a pointer to a structure which is filled in by the API.</span></span> <span data-ttu-id="0ad4d-293">Bu tür API çağrıları genellikle yerel bir yapı değişkeninin adresini geçer ve kural olmadan yapı değişkeninin gereksiz şekilde başlatılmasına gerek olur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-293">Calls to such APIs typically pass the address of a local struct variable, and without the rule, redundant initialization of the struct variable would be required.</span></span>

### <a name="pointer-increment-and-decrement"></a><span data-ttu-id="0ad4d-294">İşaretçi artışı ve azaltma</span><span class="sxs-lookup"><span data-stu-id="0ad4d-294">Pointer increment and decrement</span></span>

<span data-ttu-id="0ad4d-295">Güvenli olmayan bir bağlamda `++` ve `--` işleçleri ([sonek artırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators) ve [ön ek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)), `void*` hariç tüm türlerin işaretçi değişkenlerine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-295">In an unsafe context, the `++` and `--` operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)) can be applied to pointer variables of all types except `void*`.</span></span> <span data-ttu-id="0ad4d-296">Bu nedenle, her işaretçi türü için `T*`, aşağıdaki işleçler örtülü olarak tanımlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-296">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

<span data-ttu-id="0ad4d-297">İşleçler, sırasıyla `x + 1` ve `x - 1` ile aynı sonuçları üretir ([işaretçi aritmetiği](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="0ad4d-297">The operators produce the same results as `x + 1` and `x - 1`, respectively ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span> <span data-ttu-id="0ad4d-298">Diğer bir deyişle, `T*` türünde bir işaretçi değişkeni için `++` işleci, değişkende bulunan adrese `sizeof(T)` ekler ve `--` işleci, değişkende bulunan adresten `sizeof(T)` ' ü çıkartır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-298">In other words, for a pointer variable of type `T*`, the `++` operator adds `sizeof(T)` to the address contained in the variable, and the `--` operator subtracts `sizeof(T)` from the address contained in the variable.</span></span>

<span data-ttu-id="0ad4d-299">Bir işaretçi artıma veya azaltma işlemi işaretçi türünün etki alanını taşarsa, sonuç uygulama tanımlı olur, ancak hiçbir özel durum üretilmez.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-299">If a pointer increment or decrement operation overflows the domain of the pointer type, the result is implementation-defined, but no exceptions are produced.</span></span>

### <a name="pointer-arithmetic"></a><span data-ttu-id="0ad4d-300">İşaretçi aritmetiği</span><span class="sxs-lookup"><span data-stu-id="0ad4d-300">Pointer arithmetic</span></span>

<span data-ttu-id="0ad4d-301">Güvenli olmayan bir bağlamda `+` ve `-` işleçleri ([toplama işleci](expressions.md#addition-operator) ve [çıkarma işleci](expressions.md#subtraction-operator)), `void*` hariç tüm işaretçi türlerinin değerlerine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-301">In an unsafe context, the `+` and `-` operators ([Addition operator](expressions.md#addition-operator) and [Subtraction operator](expressions.md#subtraction-operator)) can be applied to values of all pointer types except `void*`.</span></span> <span data-ttu-id="0ad4d-302">Bu nedenle, her işaretçi türü için `T*`, aşağıdaki işleçler örtülü olarak tanımlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-302">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

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

<span data-ttu-id="0ad4d-303">@No__t-3, `uint`, `long` veya `ulong` türünde bir `T*` işaretçi türü `P` ' a bir ifade verildiğinde, `P + N` ve `N + P` @no__t `T*` türündeki işaretçi değeri @no__t 1 tarafından verilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-303">Given an expression `P` of a pointer type `T*` and an expression `N` of type `int`, `uint`, `long`, or `ulong`, the expressions `P + N` and `N + P` compute the pointer value of type `T*` that results from adding `N * sizeof(T)` to the address given by `P`.</span></span> <span data-ttu-id="0ad4d-304">Benzer şekilde, `P - N` ifadesi, `P` tarafından verilen adresten `N * sizeof(T)` ' nin çıkarılmasına neden olan `T*` türünde işaretçi değerini hesaplar.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-304">Likewise, the expression `P - N` computes the pointer value of type `T*` that results from subtracting `N * sizeof(T)` from the address given by `P`.</span></span>

<span data-ttu-id="0ad4d-305">@No__t-0 ve `Q` bir işaretçi türü `T*` olduğunda, `P - Q` ifadesi `P` ve `Q` tarafından verilen adresler arasındaki farkı hesaplar ve ardından bu farkı `sizeof(T)` ' ya böler.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-305">Given two expressions, `P` and `Q`, of a pointer type `T*`, the expression `P - Q` computes the difference between the addresses given by `P` and `Q` and then divides that difference by `sizeof(T)`.</span></span> <span data-ttu-id="0ad4d-306">Sonucun türü her zaman `long` ' dır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-306">The type of the result is always `long`.</span></span> <span data-ttu-id="0ad4d-307">Aslında `P - Q` `((long)(P) - (long)(Q)) / sizeof(T)` olarak hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-307">In effect, `P - Q` is computed as `((long)(P) - (long)(Q)) / sizeof(T)`.</span></span>

<span data-ttu-id="0ad4d-308">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-308">For example:</span></span>

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

<span data-ttu-id="0ad4d-309">çıktıyı üreten:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-309">which produces the output:</span></span>

```console
p - q = -14
q - p = 14
```

<span data-ttu-id="0ad4d-310">Bir işaretçi aritmetik işlemi işaretçi türünün etki alanını taşarsa, sonuç uygulama tanımlı bir biçimde kesilir, ancak hiçbir özel durum üretilmez.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-310">If a pointer arithmetic operation overflows the domain of the pointer type, the result is truncated in an implementation-defined fashion, but no exceptions are produced.</span></span>

### <a name="pointer-comparison"></a><span data-ttu-id="0ad4d-311">İşaretçi karşılaştırması</span><span class="sxs-lookup"><span data-stu-id="0ad4d-311">Pointer comparison</span></span>

<span data-ttu-id="0ad4d-312">Güvenli olmayan bir bağlamda `==`, `!=`, `<`, `>`, `<=` ve `=>` işleçleri ([ilişkisel ve tür-test işleçleri](expressions.md#relational-and-type-testing-operators)) tüm işaretçi türlerinin değerlerine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-312">In an unsafe context, the `==`, `!=`, `<`, `>`, `<=`, and `=>` operators ([Relational and type-testing operators](expressions.md#relational-and-type-testing-operators)) can be applied to values of all pointer types.</span></span> <span data-ttu-id="0ad4d-313">İşaretçi karşılaştırma işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-313">The pointer comparison operators are:</span></span>

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

<span data-ttu-id="0ad4d-314">Örtük bir dönüştürme herhangi bir işaretçi türünden `void*` türüne mevcut olduğundan, herhangi bir işaretçi türünün işlenenleri bu işleçler kullanılarak karşılaştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-314">Because an implicit conversion exists from any pointer type to the `void*` type, operands of any pointer type can be compared using these operators.</span></span> <span data-ttu-id="0ad4d-315">Karşılaştırma işleçleri, iki işlenen tarafından verilen adresleri işaretsiz tamsayılar gibi karşılaştırır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-315">The comparison operators compare the addresses given by the two operands as if they were unsigned integers.</span></span>

### <a name="the-sizeof-operator"></a><span data-ttu-id="0ad4d-316">Sizeof işleci</span><span class="sxs-lookup"><span data-stu-id="0ad4d-316">The sizeof operator</span></span>

<span data-ttu-id="0ad4d-317">@No__t-0 işleci, verilen türdeki bir değişken tarafından bulunan bayt sayısını döndürür.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-317">The `sizeof` operator returns the number of bytes occupied by a variable of a given type.</span></span> <span data-ttu-id="0ad4d-318">@No__t-0 ' a işlenen olarak belirtilen tür bir *unmanaged_type* ([işaretçi türleri](unsafe-code.md#pointer-types)) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-318">The type specified as an operand to `sizeof` must be an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

<span data-ttu-id="0ad4d-319">@No__t-0 işlecinin sonucu, `int` türünde bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-319">The result of the `sizeof` operator is a value of type `int`.</span></span> <span data-ttu-id="0ad4d-320">Önceden tanımlanmış bazı türler için `sizeof` işleci, aşağıdaki tabloda gösterildiği gibi sabit bir değer verir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-320">For certain predefined types, the `sizeof` operator yields a constant value as shown in the table below.</span></span>


| <span data-ttu-id="0ad4d-321">__İfadesini__</span><span class="sxs-lookup"><span data-stu-id="0ad4d-321">__Expression__</span></span>   | <span data-ttu-id="0ad4d-322">__Sonuç__</span><span class="sxs-lookup"><span data-stu-id="0ad4d-322">__Result__</span></span> |
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

<span data-ttu-id="0ad4d-323">Tüm diğer türler için, `sizeof` işlecinin sonucu uygulama tanımlı olur ve bir değer olarak sınıflandırıldı, sabit değildir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-323">For all other types, the result of the `sizeof` operator is implementation-defined and is classified as a value, not a constant.</span></span>

<span data-ttu-id="0ad4d-324">Üyelerin bir yapıya paketleneme sırası belirtilmemiş.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-324">The order in which members are packed into a struct is unspecified.</span></span>

<span data-ttu-id="0ad4d-325">Hizalama amacıyla, bir yapının başlangıcında, bir yapı içinde ve yapının sonunda adlandırılmamış doldurma olabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-325">For alignment purposes, there may be unnamed padding at the beginning of a struct, within a struct, and at the end of the struct.</span></span> <span data-ttu-id="0ad4d-326">Doldurma olarak kullanılan bitlerin içeriği belirsiz.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-326">The contents of the bits used as padding are indeterminate.</span></span>

<span data-ttu-id="0ad4d-327">Yapı türüne sahip bir işlenene uygulandığında, sonuç, herhangi bir doldurma dahil olmak üzere bu tür bir değişkende toplam bayt sayısıdır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-327">When applied to an operand that has struct type, the result is the total number of bytes in a variable of that type, including any padding.</span></span>

## <a name="the-fixed-statement"></a><span data-ttu-id="0ad4d-328">Fixed ekstresi</span><span class="sxs-lookup"><span data-stu-id="0ad4d-328">The fixed statement</span></span>

<span data-ttu-id="0ad4d-329">Güvenli olmayan bir bağlamda, *embedded_statement* ([deyimler](statements.md)) üretimi ek bir yapı, bir taşınabilir değişkeni "onarmak" için kullanılan `fixed` ifadesi sağlar .</span><span class="sxs-lookup"><span data-stu-id="0ad4d-329">In an unsafe context, the *embedded_statement* ([Statements](statements.md)) production permits an additional construct, the `fixed` statement, which is used to "fix" a moveable variable such that its address remains constant for the duration of the statement.</span></span>

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

<span data-ttu-id="0ad4d-330">Her *fixed_pointer_declarator* verilen *pointer_type* yerel bir değişkenini bildirir ve ilgili *fixed_pointer_initializer*tarafından hesaplanan adresle bu yerel değişkeni başlatır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-330">Each *fixed_pointer_declarator* declares a local variable of the given *pointer_type* and initializes that local variable with the address computed by the corresponding *fixed_pointer_initializer*.</span></span> <span data-ttu-id="0ad4d-331">@No__t-0 ifadesinde belirtilen yerel bir değişkene, bu değişkenin bildiriminin sağında gerçekleşen tüm *fixed_pointer_initializer*lar ve `fixed` ifadesinin *embedded_statement* erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-331">A local variable declared in a `fixed` statement is accessible in any *fixed_pointer_initializer*s occurring to the right of that variable's declaration, and in the *embedded_statement* of the `fixed` statement.</span></span> <span data-ttu-id="0ad4d-332">@No__t-0 ifadesiyle tanımlanmış bir yerel değişken salt okunurdur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-332">A local variable declared by a `fixed` statement is considered read-only.</span></span> <span data-ttu-id="0ad4d-333">Katıştırılmış deyimin bu yerel değişkeni (atama veya `++` ve `--` işleçleri) değiştirmeye çalışırsa bir derleme zamanı hatası oluşur veya onu `ref` veya `out` parametresi olarak geçirin.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-333">A compile-time error occurs if the embedded statement attempts to modify this local variable (via assignment or the `++` and `--` operators) or pass it as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="0ad4d-334">Bir *fixed_pointer_initializer* aşağıdakilerden biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-334">A *fixed_pointer_initializer* can be one of the following:</span></span>

*  <span data-ttu-id="0ad4d-335">"@No__t-0" belirtecini izleyen bir *variable_reference* ([kesin atamayı belirlemek için kesin kurallar](variables.md#precise-rules-for-determining-definite-assignment)), yönetilmeyen bir türdeki `T` ' e taşınabilir bir değişkene ([sabit ve taşınabilir değişkenler](unsafe-code.md#fixed-and-moveable-variables)), `T*` türü sağlanmış `fixed` ifadesinde verilen işaretçi türüne örtük olarak dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-335">The token "`&`" followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) to a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="0ad4d-336">Bu durumda, başlatıcı verilen değişkenin adresini hesaplar ve değişken `fixed` ifadesinin süresi boyunca sabit bir adreste kalacak şekilde garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-336">In this case, the initializer computes the address of the given variable, and the variable is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>
*  <span data-ttu-id="0ad4d-337">@No__t-2 türü `fixed` ifadesinde verilen işaretçi türüne örtülü olarak dönüştürülebilir `T` yönetilmeyen türdeki öğelerle bir *array_type* ifadesi.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-337">An expression of an *array_type* with elements of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="0ad4d-338">Bu durumda, başlatıcı dizideki ilk öğenin adresini hesaplar ve tüm diziyi `fixed` ifadesinin süresi boyunca sabit bir adreste kalacak şekilde garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-338">In this case, the initializer computes the address of the first element in the array, and the entire array is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="0ad4d-339">Dizi ifadesi null ise veya dizide sıfır öğe varsa, başlatıcı bir adresi sıfıra eşit olarak hesaplar.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-339">If the array expression is null or if the array has zero elements, the initializer computes an address equal to zero.</span></span>
*  <span data-ttu-id="0ad4d-340">@No__t-0 türünde bir ifade, `char*` türü belirtilen `fixed` ifadesinde verilen işaretçi türüne örtük olarak dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-340">An expression of type `string`, provided the type `char*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="0ad4d-341">Bu durumda, başlatıcı dizedeki ilk karakterin adresini hesaplar ve tüm dize, `fixed` ifadesinin süresi boyunca sabit bir adreste kalacak şekilde garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-341">In this case, the initializer computes the address of the first character in the string, and the entire string is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="0ad4d-342">@No__t-0 deyiminin davranışı, dize ifadesi null ise uygulama tanımlı olur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-342">The behavior of the `fixed` statement is implementation-defined if the string expression is null.</span></span>
*  <span data-ttu-id="0ad4d-343">Taşınabilir bir değişkenin sabit boyut arabelleği üyesine başvuran bir *simple_name* veya *member_access* , sabit boyutlu arabellek üyesinin türü, `fixed` ifadesinde verilen işaretçi türüne örtük olarak dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-343">A *simple_name* or *member_access* that references a fixed size buffer member of a moveable variable, provided the type of the fixed size buffer member is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="0ad4d-344">Bu durumda, başlatıcı sabit boyutlu arabelleğin ([Ifadelerde sabit boyutlu arabellekler](unsafe-code.md#fixed-size-buffers-in-expressions)) ilk öğesine bir işaretçi hesaplar ve sabit boyutlu arabelleğin `fixed` deyimi süresince sabit bir adreste kalması garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-344">In this case, the initializer computes a pointer to the first element of the fixed size buffer ([Fixed size buffers in expressions](unsafe-code.md#fixed-size-buffers-in-expressions)), and the fixed size buffer is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>

<span data-ttu-id="0ad4d-345">Bir *fixed_pointer_initializer* tarafından hesaplanan her adres için `fixed` ifadesinde, adres tarafından başvurulan değişkenin, `fixed` ifadesinin süresi boyunca çöp toplayıcı tarafından yeniden konumlandırma veya aktiften çıkarma konusu olmamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-345">For each address computed by a *fixed_pointer_initializer* the `fixed` statement ensures that the variable referenced by the address is not subject to relocation or disposal by the garbage collector for the duration of the `fixed` statement.</span></span> <span data-ttu-id="0ad4d-346">Örneğin, bir *fixed_pointer_initializer* tarafından hesaplanan adres bir nesnenin alanına veya bir dizi örneğinin öğesine başvuruyorsa, `fixed` ifade, kapsayan nesne örneğinin, deyimin ömrü.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-346">For example, if the address computed by a *fixed_pointer_initializer* references a field of an object or an element of an array instance, the `fixed` statement guarantees that the containing object instance is not relocated or disposed of during the lifetime of the statement.</span></span>

<span data-ttu-id="0ad4d-347">@No__t-0 deyimleri tarafından oluşturulan işaretçilerin Bu deyimlerin yürütülmesinin ötesinde devam etmez.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-347">It is the programmer's responsibility to ensure that pointers created by `fixed` statements do not survive beyond execution of those statements.</span></span> <span data-ttu-id="0ad4d-348">Örneğin, `fixed` deyimleri tarafından oluşturulan işaretçiler dış API 'lere geçirildiğinde, API 'Lerin bu işaretçilerin hiçbir bellek içermediğinden emin olmak programcı sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-348">For example, when pointers created by `fixed` statements are passed to external APIs, it is the programmer's responsibility to ensure that the APIs retain no memory of these pointers.</span></span>

<span data-ttu-id="0ad4d-349">Sabit nesneler yığının parçalanmasına neden olabilir (çünkü taşınamazlar).</span><span class="sxs-lookup"><span data-stu-id="0ad4d-349">Fixed objects may cause fragmentation of the heap (because they can't be moved).</span></span> <span data-ttu-id="0ad4d-350">Bu nedenle, nesneler yalnızca kesinlikle gerekli olduğunda ve yalnızca en kısa sürede mümkün olduğunda düzeltilmelidir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-350">For that reason, objects should be fixed only when absolutely necessary and then only for the shortest amount of time possible.</span></span>

<span data-ttu-id="0ad4d-351">Örnek</span><span class="sxs-lookup"><span data-stu-id="0ad4d-351">The example</span></span>

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

<span data-ttu-id="0ad4d-352">`fixed` ifadesinin çeşitli kullanımlarını gösterir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-352">demonstrates several uses of the `fixed` statement.</span></span> <span data-ttu-id="0ad4d-353">İlk ifade bir statik alanın adresini düzeltir ve edinir, ikinci ifade bir örnek alanının adresini düzeltir ve edinir ve üçüncü ifade, bir dizi öğesinin adresini düzeltir ve edinir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-353">The first statement fixes and obtains the address of a static field, the second statement fixes and obtains the address of an instance field, and the third statement fixes and obtains the address of an array element.</span></span> <span data-ttu-id="0ad4d-354">Her durumda, değişkenlerin tamamen taşınabilir değişkenler olarak sınıflandırıldığından, bu, normal `&` işlecini kullanmanın bir hatası oluyordu.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-354">In each case it would have been an error to use the regular `&` operator since the variables are all classified as moveable variables.</span></span>

<span data-ttu-id="0ad4d-355">Yukarıdaki örnekteki dördüncü `fixed` deyimleri, üçüncü için de benzer bir sonuç üretir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-355">The fourth `fixed` statement in the example above produces a similar result to the third.</span></span>

<span data-ttu-id="0ad4d-356">@No__t-0 ifadesinin bu örneği `string` kullanır:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-356">This example of the `fixed` statement uses `string`:</span></span>

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

<span data-ttu-id="0ad4d-357">Tek boyutlu dizilerin güvenli olmayan bir bağlam dizisi öğelerinde, `0` dizininden başlayıp Dizin `Length - 1` ile biten Dizin sırasında artan dizin sırasında depolanır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-357">In an unsafe context array elements of single-dimensional arrays are stored in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="0ad4d-358">Çok boyutlu diziler için, dizi öğeleri, en sağdaki boyutun dizinlerinin önce, ardından bir sonraki sol boyutun ve bu şekilde sol tarafında arttığı şekilde depolanır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-358">For multi-dimensional arrays, array elements are stored such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span> <span data-ttu-id="0ad4d-359">@No__t-2 ' den bir dizi örneğine `p` işaretçisi alan `fixed` ifadesinde, `p` ' ten `p + a.Length - 1` ' e kadar olan işaretçi değerleri dizideki öğelerin adreslerini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-359">Within a `fixed` statement that obtains a pointer `p` to an array instance `a`, the pointer values ranging from `p` to `p + a.Length - 1` represent addresses of the elements in the array.</span></span> <span data-ttu-id="0ad4d-360">Benzer şekilde, `p[0]` ile `p[a.Length - 1]` arasındaki değişkenler gerçek dizi öğelerini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-360">Likewise, the variables ranging from `p[0]` to `p[a.Length - 1]` represent the actual array elements.</span></span> <span data-ttu-id="0ad4d-361">Dizilerin nerede depolandığına göre, herhangi bir boyutun dizisini doğrusal hale gelse de işleyebiliriz.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-361">Given the way in which arrays are stored, we can treat an array of any dimension as though it were linear.</span></span>

<span data-ttu-id="0ad4d-362">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-362">For example:</span></span>

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

<span data-ttu-id="0ad4d-363">çıktıyı üreten:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-363">which produces the output:</span></span>

```console
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

<span data-ttu-id="0ad4d-364">Örnekte</span><span class="sxs-lookup"><span data-stu-id="0ad4d-364">In the example</span></span>

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

<span data-ttu-id="0ad4d-365">`fixed` bir ifade, bir diziyi çözmek için kullanılır, bu nedenle adresinin bir işaretçi alan bir yönteme geçirilmesi sağlanır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-365">a `fixed` statement is used to fix an array so its address can be passed to a method that takes a pointer.</span></span>

<span data-ttu-id="0ad4d-366">Örnekte:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-366">In the example:</span></span>

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

<span data-ttu-id="0ad4d-367">sabit bir ifade, bir yapının sabit boyutlu arabelleğini gidermek için kullanılır, bu nedenle adresi bir işaretçi olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-367">a fixed statement is used to fix a fixed size buffer of a struct so its address can be used as a pointer.</span></span>

<span data-ttu-id="0ad4d-368">Bir dize örneğini düzelterek oluşturulan @no__t 0 değeri her zaman null ile sonlandırılmış bir dizeye işaret eder.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-368">A `char*` value produced by fixing a string instance always points to a null-terminated string.</span></span> <span data-ttu-id="0ad4d-369">-1 @no__t bir dize örneğine `p` işaretçisi elde eden bir fixed ifadesinde, `p` ' den `p + s.Length - 1` ' e kadar olan işaretçi değerleri dizedeki karakterlerin adreslerini temsil eder ve `p + s.Length` işaretçi değeri her zaman bir null karakteri işaret eder ( `'\0'` değeri olan karakter.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-369">Within a fixed statement that obtains a pointer `p` to a string instance `s`, the pointer values ranging from `p` to `p + s.Length - 1` represent addresses of the characters in the string, and the pointer value `p + s.Length` always points to a null character (the character with value `'\0'`).</span></span>

<span data-ttu-id="0ad4d-370">Yönetilen türdeki nesneleri sabit işaretçiler aracılığıyla değiştirmek tanımsız davranışa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-370">Modifying objects of managed type through fixed pointers can results in undefined behavior.</span></span> <span data-ttu-id="0ad4d-371">Örneğin, dizeler sabittir olduğundan, sabit bir dizeye yönelik işaretçinin başvurduğu karakterlerin değiştirilmediğinden emin olmak programcı sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-371">For example, because strings are immutable, it is the programmer's responsibility to ensure that the characters referenced by a pointer to a fixed string are not modified.</span></span>

<span data-ttu-id="0ad4d-372">Dizelerin otomatik olarak boş sonlandırması, "C stili" dizelerini bekleyen dış API 'Leri çağırırken özellikle kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-372">The automatic null-termination of strings is particularly convenient when calling external APIs that expect "C-style" strings.</span></span> <span data-ttu-id="0ad4d-373">Ancak, bir dize örneğinin null karakter içermesine izin verildiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-373">Note, however, that a string instance is permitted to contain null characters.</span></span> <span data-ttu-id="0ad4d-374">Bu null karakterler varsa dize, null sonlandırılmış `char*` olarak kabul edildiğinde kesilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-374">If such null characters are present, the string will appear truncated when treated as a null-terminated `char*`.</span></span>

## <a name="fixed-size-buffers"></a><span data-ttu-id="0ad4d-375">Sabit Boyutlu Arabellekler</span><span class="sxs-lookup"><span data-stu-id="0ad4d-375">Fixed size buffers</span></span>

<span data-ttu-id="0ad4d-376">Sabit boyutlu arabellekler, "C stili" satır içi dizileri yapıların üyeleri olarak bildirmek için kullanılır ve öncelikle yönetilmeyen API 'lerle arabirim oluşturma için faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-376">Fixed size buffers are used to declare "C style" in-line arrays as members of structs, and are primarily useful for interfacing with unmanaged APIs.</span></span>

### <a name="fixed-size-buffer-declarations"></a><span data-ttu-id="0ad4d-377">Sabit boyutlu arabellek bildirimleri</span><span class="sxs-lookup"><span data-stu-id="0ad4d-377">Fixed size buffer declarations</span></span>

<span data-ttu-id="0ad4d-378">***Sabit boyutlu arabellek*** , belirli bir türdeki değişkenlerin sabit uzunluklu arabelleği için depolamayı temsil eden bir üyedir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-378">A ***fixed size buffer*** is a member that represents storage for a fixed length buffer of variables of a given type.</span></span> <span data-ttu-id="0ad4d-379">Sabit boyutlu bir arabellek bildirimi, belirli bir öğe türünün bir veya daha fazla sabit boyutlu arabelleğini tanıtır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-379">A fixed size buffer declaration introduces one or more fixed size buffers of a given element type.</span></span> <span data-ttu-id="0ad4d-380">Sabit boyutlu arabelleklere yalnızca struct bildirimlerinde izin verilir ve yalnızca güvenli olmayan bağlamlarda ([güvensiz bağlamlarda](unsafe-code.md#unsafe-contexts)) bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-380">Fixed size buffers are only permitted in struct declarations and can only occur in unsafe contexts ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

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

<span data-ttu-id="0ad4d-381">Sabit boyutlu bir arabellek bildiriminde bir dizi öznitelik ([öznitelik](attributes.md)), `new` değiştirici ([değiştiriciler](classes.md#modifiers)), dört erişim değiştiricisinin geçerli bir birleşimi ([tür parametreleri ve kısıtlamalar](classes.md#type-parameters-and-constraints)) ve bir `unsafe` değiştiricisi (güvenli olmayan bir şekilde) bulunabilir.[ bağlam](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="0ad4d-381">A fixed size buffer declaration may include a set of attributes ([Attributes](attributes.md)), a `new` modifier ([Modifiers](classes.md#modifiers)), a valid combination of the four access modifiers ([Type parameters and constraints](classes.md#type-parameters-and-constraints)) and an `unsafe` modifier ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="0ad4d-382">Öznitelikler ve değiştiriciler, sabit boyutlu arabellek bildirimi tarafından belirtilen tüm Üyeler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-382">The attributes and modifiers apply to all of the members declared by the fixed size buffer declaration.</span></span> <span data-ttu-id="0ad4d-383">Aynı değiştiricinin sabit boyutlu bir arabellek bildiriminde birden çok kez görünmesi hatadır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-383">It is an error for the same modifier to appear multiple times in a fixed size buffer declaration.</span></span>

<span data-ttu-id="0ad4d-384">Sabit boyutlu bir arabellek bildiriminin `static` değiştiricisini içerme izni yoktur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-384">A fixed size buffer declaration is not permitted to include the `static` modifier.</span></span>

<span data-ttu-id="0ad4d-385">Sabit boyutlu bir arabellek bildiriminin arabellek öğesi türü, bildirim tarafından tanıtılan arabelleğin öğe türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-385">The buffer element type of a fixed size buffer declaration specifies the element type of the buffer(s) introduced by the declaration.</span></span> <span data-ttu-id="0ad4d-386">Arabellek öğesi türü önceden tanımlanmış türlerden biri olmalıdır `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, 0 veya 1.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-386">The buffer element type must be one of the predefined types `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `bool`.</span></span>

<span data-ttu-id="0ad4d-387">Buffer öğe türü, her biri yeni bir üye tanıtan sabit boyutlu arabellek bildirimcilerinin bir listesi tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-387">The buffer element type is followed by a list of fixed size buffer declarators, each of which introduces a new member.</span></span> <span data-ttu-id="0ad4d-388">Sabit boyutlu bir arabellek bildirimci, üyeyi isimlediği bir tanımlayıcıdan, ardından `[` ve `]` belirteçlerine sahip sabit bir ifadeyle oluşur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-388">A fixed size buffer declarator consists of an identifier that names the member, followed by a constant expression enclosed in `[` and `]` tokens.</span></span> <span data-ttu-id="0ad4d-389">Sabit ifade, bu sabit boyutlu arabellek bildirimci tarafından tanıtılan üye içindeki öğe sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-389">The constant expression denotes the number of elements in the member introduced by that fixed size buffer declarator.</span></span> <span data-ttu-id="0ad4d-390">Sabit ifadenin türü `int` türüne örtük olarak dönüştürülebilir olmalıdır ve değer sıfır olmayan pozitif bir tamsayı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-390">The type of the constant expression must be implicitly convertible to type `int`, and the value must be a non-zero positive integer.</span></span>

<span data-ttu-id="0ad4d-391">Sabit boyutlu bir arabelleğin öğelerinin ardışık olarak bellekte düzenlenme garantisi vardır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-391">The elements of a fixed size buffer are guaranteed to be laid out sequentially in memory.</span></span>

<span data-ttu-id="0ad4d-392">Birden çok sabit boyut arabelleği bildiren sabit boyutlu bir arabellek bildirimi, aynı özniteliklere ve öğe türlerine sahip tek bir sabit boyutlu arabellek bildiriminin birden çok bildirimine eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-392">A fixed size buffer declaration that declares multiple fixed size buffers is equivalent to multiple declarations of a single fixed size buffer declaration with the same attributes, and element types.</span></span> <span data-ttu-id="0ad4d-393">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-393">For example</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

<span data-ttu-id="0ad4d-394">eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="0ad4d-394">is equivalent to</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a><span data-ttu-id="0ad4d-395">İfadelerde sabit boyutlu arabellekler</span><span class="sxs-lookup"><span data-stu-id="0ad4d-395">Fixed size buffers in expressions</span></span>

<span data-ttu-id="0ad4d-396">Sabit boyutlu bir arabellek üyesinin üye araması ([işleçler](expressions.md#operators)), bir alanın üye aramasına tam olarak devam eder.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-396">Member lookup ([Operators](expressions.md#operators)) of a fixed size buffer member proceeds exactly like member lookup of a field.</span></span>

<span data-ttu-id="0ad4d-397">Sabit boyutlu bir arabelleğe, bir *simple_name* ([tür çıkarımı](expressions.md#type-inference)) veya *member_access* ([dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) kullanılarak bir ifadede başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-397">A fixed size buffer can be referenced in an expression using a *simple_name* ([Type inference](expressions.md#type-inference)) or a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="0ad4d-398">Sabit boyutlu bir arabellek üyesine basit ad olarak başvuruluyorsa, efekt `this.I` biçiminde bir üye erişimiyle aynıdır; burada `I` sabit boyutlu arabellek üyesidir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-398">When a fixed size buffer member is referenced as a simple name, the effect is the same as a member access of the form `this.I`, where `I` is the fixed size buffer member.</span></span>

<span data-ttu-id="0ad4d-399">@No__t-0 biçiminde üye erişiminde, `E` bir yapı türüdür ve bu yapı türünde `I` ' nin üye araması sabit boyutlu bir üyeyi tanımlarsa, `E.I` ' ün sınıflandırılan aşağıdaki gibi değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-399">In a member access of the form `E.I`, if `E` is of a struct type and a member lookup of `I` in that struct type identifies a fixed size member, then `E.I` is evaluated an classified as follows:</span></span>

*  <span data-ttu-id="0ad4d-400">@No__t-0 ifadesi güvenli olmayan bir bağlamda gerçekleşmezse, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-400">If the expression `E.I` does not occur in an unsafe context, a compile-time error occurs.</span></span>
*  <span data-ttu-id="0ad4d-401">@No__t-0 değeri bir değer olarak sınıflandırıldığında, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-401">If `E` is classified as a value, a compile-time error occurs.</span></span>
*  <span data-ttu-id="0ad4d-402">Aksi takdirde, `E` taşınabilir bir değişkendir ([sabit ve taşınabilir değişkenler](unsafe-code.md#fixed-and-moveable-variables)) ve `E.I` ifadesi bir *fixed_pointer_initializer* ([fixed Deyimi](unsafe-code.md#the-fixed-statement)) değilse, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-402">Otherwise, if `E` is a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) and the expression `E.I` is not a *fixed_pointer_initializer* ([The fixed statement](unsafe-code.md#the-fixed-statement)), a compile-time error occurs.</span></span>
*  <span data-ttu-id="0ad4d-403">Aksi takdirde, `E` sabit bir değişkene başvurur ve ifadenin sonucu, `E` ' deki sabit boyutlu arabellek üyesinin `I` ' in ilk öğesine yönelik bir işaretçidir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-403">Otherwise, `E` references a fixed variable and the result of the expression is a pointer to the first element of the fixed size buffer member `I` in `E`.</span></span> <span data-ttu-id="0ad4d-404">Sonuç `S*` türündedir, burada `S` `I` ' nin öğe türüdür ve bir değer olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-404">The result is of type `S*`, where `S` is the element type of `I`, and is classified as a value.</span></span>

<span data-ttu-id="0ad4d-405">Sabit boyutlu arabelleğin sonraki öğelerine, ilk öğeden işaretçi işlemleri kullanılarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-405">The subsequent elements of the fixed size buffer can be accessed using pointer operations from the first element.</span></span> <span data-ttu-id="0ad4d-406">Dizilere erişimin aksine, sabit boyutlu bir arabelleğin öğelerine erişimi güvenli olmayan bir işlemdir ve Aralık işaretli değildir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-406">Unlike access to arrays, access to the elements of a fixed size buffer is an unsafe operation and is not range checked.</span></span>

<span data-ttu-id="0ad4d-407">Aşağıdaki örnek, sabit boyutlu bir arabellek üyesine sahip bir struct bildirir ve kullanır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-407">The following example declares and uses a struct with a fixed size buffer member.</span></span>

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

### <a name="definite-assignment-checking"></a><span data-ttu-id="0ad4d-408">Kesin atama denetimi</span><span class="sxs-lookup"><span data-stu-id="0ad4d-408">Definite assignment checking</span></span>

<span data-ttu-id="0ad4d-409">Sabit boyutlu arabellekler kesin atama denetimine ([kesin atama](variables.md#definite-assignment)) tabi değildir ve yapı türü değişkenlerinin kesin atama denetimi amacıyla sabit boyutlu arabellek üyeleri göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-409">Fixed size buffers are not subject to definite assignment checking ([Definite assignment](variables.md#definite-assignment)), and fixed size buffer members are ignored for purposes of definite assignment checking of struct type variables.</span></span>

<span data-ttu-id="0ad4d-410">Sabit boyutlu bir arabellek üyesinin struct değişkenini içeren en dıştaki, bir statik değişken, bir sınıf örneğinin örnek değişkeni veya bir dizi öğesi olduğunda, sabit boyutlu arabelleğin öğeleri otomatik olarak varsayılan değerlerine başlatılır ([varsayılan değerler](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="0ad4d-410">When the outermost containing struct variable of a fixed size buffer member is a static variable, an instance variable of a class instance, or an array element, the elements of the fixed size buffer are automatically initialized to their default values ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="0ad4d-411">Diğer tüm durumlarda, sabit boyutlu bir arabelleğin ilk içeriği tanımsızdır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-411">In all other cases, the initial content of a fixed size buffer is undefined.</span></span>

## <a name="stack-allocation"></a><span data-ttu-id="0ad4d-412">Yığın ayırma</span><span class="sxs-lookup"><span data-stu-id="0ad4d-412">Stack allocation</span></span>

<span data-ttu-id="0ad4d-413">Güvenli olmayan bir bağlamda, yerel değişken bildirimi ([yerel değişken bildirimleri](statements.md#local-variable-declarations)), çağrı yığınından bellek ayıran bir yığın ayırma başlatıcısı içerebilir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-413">In an unsafe context, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) may include a stack allocation initializer which allocates memory from the call stack.</span></span>

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="0ad4d-414">*Unmanaged_type* , yeni ayrılan konumda depolanacak öğelerin türünü gösterir ve *ifade* bu öğelerin sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-414">The *unmanaged_type* indicates the type of the items that will be stored in the newly allocated location, and the *expression* indicates the number of these items.</span></span> <span data-ttu-id="0ad4d-415">Birlikte getirildiğinde, gerekli ayırma boyutunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-415">Taken together, these specify the required allocation size.</span></span> <span data-ttu-id="0ad4d-416">Yığın ayırma boyutu negatif olamaz, bu, öğelerin sayısını negatif bir değer değerlendiren bir *constant_expression* olarak belirtmek için bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-416">Since the size of a stack allocation cannot be negative, it is a compile-time error to specify the number of items as a *constant_expression* that evaluates to a negative value.</span></span>

<span data-ttu-id="0ad4d-417">@No__t-0 formunun bir yığın ayırma başlatıcısı, yönetilmeyen bir tür ([işaretçi türleri](unsafe-code.md#pointer-types)) ve `E` ' ü `int` türünde bir ifade olacak şekilde @no__t gerektirir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-417">A stack allocation initializer of the form `stackalloc T[E]` requires `T` to be an unmanaged type ([Pointer types](unsafe-code.md#pointer-types)) and `E` to be an expression of type `int`.</span></span> <span data-ttu-id="0ad4d-418">Yapı, çağrı yığınından `E * sizeof(T)` bayt ayırır ve yeni ayrılan bloğa `T*` türünde bir işaretçi döndürür.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-418">The construct allocates `E * sizeof(T)` bytes from the call stack and returns a pointer, of type `T*`, to the newly allocated block.</span></span> <span data-ttu-id="0ad4d-419">@No__t-0 negatif bir değer ise, davranış tanımsızdır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-419">If `E` is a negative value, then the behavior is undefined.</span></span> <span data-ttu-id="0ad4d-420">@No__t-0 sıfırsa, hiçbir ayırma yapılmaz ve döndürülen işaretçi uygulama tanımlı olur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-420">If `E` is zero, then no allocation is made, and the pointer returned is implementation-defined.</span></span> <span data-ttu-id="0ad4d-421">Verilen boyutta bir blok ayırmak için yeterli kullanılabilir bellek yoksa, bir `System.StackOverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-421">If there is not enough memory available to allocate a block of the given size, a `System.StackOverflowException` is thrown.</span></span>

<span data-ttu-id="0ad4d-422">Yeni ayrılan belleğin içeriği tanımlı değil.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-422">The content of the newly allocated memory is undefined.</span></span>

<span data-ttu-id="0ad4d-423">@No__t-0 veya `finally` bloklarda ([TRY ifadesinde](statements.md#the-try-statement)) yığın ayırma başlatıcılarına izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-423">Stack allocation initializers are not permitted in `catch` or `finally` blocks ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="0ad4d-424">@No__t-0 kullanılarak ayrılan belleği açıkça serbest bırakma yöntemi yoktur.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-424">There is no way to explicitly free memory allocated using `stackalloc`.</span></span> <span data-ttu-id="0ad4d-425">İşlev üyesinin yürütülmesi sırasında oluşturulan tüm yığın ayrılmış bellek blokları, bu işlev üyesi döndürüldüğünde otomatik olarak atılır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-425">All stack allocated memory blocks created during the execution of a function member are automatically discarded when that function member returns.</span></span> <span data-ttu-id="0ad4d-426">Bu, C ve C++ uygulamalarında yaygın olarak bulunan bir uzantı olan `alloca` işlevine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-426">This corresponds to the `alloca` function, an extension commonly found in C and C++ implementations.</span></span>

<span data-ttu-id="0ad4d-427">Örnekte</span><span class="sxs-lookup"><span data-stu-id="0ad4d-427">In the example</span></span>

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

<span data-ttu-id="0ad4d-428">yığın üzerinde 16 karakterlik bir arabellek ayırmak için `IntToString` yönteminde `stackalloc` başlatıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-428">a `stackalloc` initializer is used in the `IntToString` method to allocate a buffer of 16 characters on the stack.</span></span> <span data-ttu-id="0ad4d-429">Yöntem döndürüldüğünde arabellek otomatik olarak atılır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-429">The buffer is automatically discarded when the method returns.</span></span>

## <a name="dynamic-memory-allocation"></a><span data-ttu-id="0ad4d-430">Dinamik bellek ayırma</span><span class="sxs-lookup"><span data-stu-id="0ad4d-430">Dynamic memory allocation</span></span>

<span data-ttu-id="0ad4d-431">@No__t-0 işleci dışında, C# atık olmayan toplanan belleği yönetmek için önceden tanımlanmış bir yapı sağlar.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-431">Except for the `stackalloc` operator, C# provides no predefined constructs for managing non-garbage collected memory.</span></span> <span data-ttu-id="0ad4d-432">Bu tür hizmetler genellikle sınıf kitaplıklarını destekleyerek veya doğrudan temel işletim sisteminden içeri aktarılarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-432">Such services are typically provided by supporting class libraries or imported directly from the underlying operating system.</span></span> <span data-ttu-id="0ad4d-433">Örneğin, aşağıdaki `Memory` sınıfı, temel alınan bir işletim sisteminin yığın işlevlerine nasıl erişilebileceğini göstermektedir C#:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-433">For example, the `Memory` class below illustrates how the heap functions of an underlying operating system might be accessed from C#:</span></span>

```csharp
using System;
using System.Runtime.InteropServices;

public unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    static int ph = GetProcessHeap();

    // Private instance constructor to prevent instantiation.
    private Memory() {}

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size) {
        void* result = HeapAlloc(ph, HEAP_ZERO_MEMORY, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count) {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd) {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd) {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block) {
        if (!HeapFree(ph, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size) {
        void* result = HeapReAlloc(ph, HEAP_ZERO_MEMORY, block, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block) {
        int result = HeapSize(ph, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    static extern int GetProcessHeap();

    [DllImport("kernel32")]
    static extern void* HeapAlloc(int hHeap, int flags, int size);

    [DllImport("kernel32")]
    static extern bool HeapFree(int hHeap, int flags, void* block);

    [DllImport("kernel32")]
    static extern void* HeapReAlloc(int hHeap, int flags, void* block, int size);

    [DllImport("kernel32")]
    static extern int HeapSize(int hHeap, int flags, void* block);
}
```

<span data-ttu-id="0ad4d-434">@No__t-0 sınıfını kullanan bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="0ad4d-434">An example that uses the `Memory` class is given below:</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            byte* buffer = (byte*)Memory.Alloc(256);
            try {
                for (int i = 0; i < 256; i++) buffer[i] = (byte)i;
                byte[] array = new byte[256];
                fixed (byte* p = array) Memory.Copy(buffer, p, 256); 
            }
            finally {
                Memory.Free(buffer);
            }
            for (int i = 0; i < 256; i++) Console.WriteLine(array[i]);
        }
    }
}
```

<span data-ttu-id="0ad4d-435">Örnek, `Memory.Alloc` üzerinden 256 baytlık belleği ayırır ve bellek bloğunu 0 ' dan 255 ' e kadar artan değerlerle başlatır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-435">The example allocates 256 bytes of memory through `Memory.Alloc` and initializes the memory block with values increasing from 0 to 255.</span></span> <span data-ttu-id="0ad4d-436">Daha sonra bir 256 öğe bayt dizisi ayırır ve bellek bloğunun içeriğini bayt dizisine kopyalamak için `Memory.Copy` kullanır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-436">It then allocates a 256 element byte array and uses `Memory.Copy` to copy the contents of the memory block into the byte array.</span></span> <span data-ttu-id="0ad4d-437">Son olarak, bellek bloğu `Memory.Free` kullanılarak serbest bırakılır ve byte dizisinin içeriği konsolun çıktılardır.</span><span class="sxs-lookup"><span data-stu-id="0ad4d-437">Finally, the memory block is freed using `Memory.Free` and the contents of the byte array are output on the console.</span></span>
