# <a name="unsafe-code"></a><span data-ttu-id="53d9f-101">Güvenli olmayan kod</span><span class="sxs-lookup"><span data-stu-id="53d9f-101">Unsafe code</span></span>

<span data-ttu-id="53d9f-102">C# dili, çekirdek önceki bölümde tanımlanan özellikle C ve C++ içindeki veri türü olarak işaretçiler, atlama farklıdır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-102">The core C# language, as defined in the preceding chapters, differs notably from C and C++ in its omission of pointers as a data type.</span></span> <span data-ttu-id="53d9f-103">Bunun yerine, C# başvurular ve atık toplayıcısı tarafından yönetilen nesneleri oluşturma yeteneği sağlar.</span><span class="sxs-lookup"><span data-stu-id="53d9f-103">Instead, C# provides references and the ability to create objects that are managed by a garbage collector.</span></span> <span data-ttu-id="53d9f-104">Diğer özelliklerle birlikte bu tasarım, C# C veya C++ daha çok daha güvenli dil sağlar.</span><span class="sxs-lookup"><span data-stu-id="53d9f-104">This design, coupled with other features, makes C# a much safer language than C or C++.</span></span> <span data-ttu-id="53d9f-105">Çekirdek C# dilinde, yalnızca başlatılmayan bir değişken, "Sallantıdaki" bir işaretçi veya dizi sınırlarının ötesine dizinler bir ifade olması mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-105">In the core C# language it is simply not possible to have an uninitialized variable, a "dangling" pointer, or an expression that indexes an array beyond its bounds.</span></span> <span data-ttu-id="53d9f-106">Tüm kategoriler hataların C düzenli olarak bu tehdit ve bu nedenle C++ programları ortadan kalkar.</span><span class="sxs-lookup"><span data-stu-id="53d9f-106">Whole categories of bugs that routinely plague C and C++ programs are thus eliminated.</span></span>

<span data-ttu-id="53d9f-107">Neredeyse her işaretçi türü yapısı C veya C++ içinde bir başvuru türü karşılığı C# ' de olsa da yine de, burada seçeneği işaretçi türlerine erişimi olur durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-107">While practically every pointer type construct in C or C++ has a reference type counterpart in C#, nonetheless, there are situations where access to pointer types becomes a necessity.</span></span> <span data-ttu-id="53d9f-108">Örneğin, temel işletim sistemi ile arabirim, bellekle eşlenen bir cihaza erişmek ve zaman açısından kritik bir algoritma uygulayan mümkün ve pratik işaretçileri erişimi olmadan olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-108">For example, interfacing with the underlying operating system, accessing a memory-mapped device, or implementing a time-critical algorithm may not be possible or practical without access to pointers.</span></span> <span data-ttu-id="53d9f-109">Bu ihtiyacı karşılamak için C# yazma olanağı sağlar ***güvenli olmayan kod***.</span><span class="sxs-lookup"><span data-stu-id="53d9f-109">To address this need, C# provides the ability to write ***unsafe code***.</span></span>

<span data-ttu-id="53d9f-110">Güvenli olmayan kod bildirmek ve işaretçileri ve değişkenleri adresini almak için tam sayı türleri arasında dönüştürmeler gerçekleştirmek için işaretçiler üzerinde çalışacağı vb. mümkündür.</span><span class="sxs-lookup"><span data-stu-id="53d9f-110">In unsafe code it is possible to declare and operate on pointers, to perform conversions between pointers and integral types, to take the address of variables, and so forth.</span></span> <span data-ttu-id="53d9f-111">Bir anlamda, güvenli olmayan kod yazma C kodu bir C# programı içinde yazma gibi olduğu kadar.</span><span class="sxs-lookup"><span data-stu-id="53d9f-111">In a sense, writing unsafe code is much like writing C code within a C# program.</span></span>

<span data-ttu-id="53d9f-112">Güvenli olmayan kod aslında bir "güvenli" hem geliştiriciler hem de kullanıcılar açısından özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-112">Unsafe code is in fact a "safe" feature from the perspective of both developers and users.</span></span> <span data-ttu-id="53d9f-113">Güvenli olmayan kod değiştiriciyle açıkça da işaretlenmelidir `unsafe`, böylece geliştiriciler büyük olasılıkla kullanamaz güvenli olmayan özellikler yanlışlıkla ve yürütme altyapısı, güvenli olmayan kod güvenilmeyen bir ortamda çalıştırılamayacağından emin olmak için çalışır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-113">Unsafe code must be clearly marked with the modifier `unsafe`, so developers can't possibly use unsafe features accidentally, and the execution engine works to ensure that unsafe code cannot be executed in an untrusted environment.</span></span>

## <a name="unsafe-contexts"></a><span data-ttu-id="53d9f-114">Güvenli olmayan bağlamları</span><span class="sxs-lookup"><span data-stu-id="53d9f-114">Unsafe contexts</span></span>

<span data-ttu-id="53d9f-115">C# güvenli olmayan özellikler yalnızca güvenli olmayan bağlamda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-115">The unsafe features of C# are available only in unsafe contexts.</span></span> <span data-ttu-id="53d9f-116">Güvenli olmayan bir bağlam dahil ederek sunulan bir `unsafe` değiştiricisi bildiriminde tür veya üye veya kullanan bir *unsafe_statement*:</span><span class="sxs-lookup"><span data-stu-id="53d9f-116">An unsafe context is introduced by including an `unsafe` modifier in the declaration of a type or member, or by employing an *unsafe_statement*:</span></span>

*  <span data-ttu-id="53d9f-117">Bir sınıf, yapı, arabirim veya temsilci bildirimi içerebilir bir `unsafe` değiştiricisi, güvenli olmayan bir bağlam çalışması (sınıf, yapı veya arabirim gövdesinin dahil) bu tür bildirimi tüm metinsel kapsamını değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-117">A declaration of a class, struct, interface, or delegate may include an `unsafe` modifier, in which case the entire textual extent of that type declaration (including the body of the class, struct, or interface) is considered an unsafe context.</span></span>
*  <span data-ttu-id="53d9f-118">Alan, yöntemi, özelliği, olay, dizin oluşturucu, işleci, örnek oluşturucusu, yok Edicisi veya statik Oluşturucu bildirimi içerebilir bir `unsafe` içinde bu üye bildirimi tüm metinsel kapsamını çalışması olarak kabul edilir güvenli olmayan bir değiştiricisi bağlamı.</span><span class="sxs-lookup"><span data-stu-id="53d9f-118">A declaration of a field, method, property, event, indexer, operator, instance constructor, destructor, or static constructor may include an `unsafe` modifier, in which case the entire textual extent of that member declaration is considered an unsafe context.</span></span>
*  <span data-ttu-id="53d9f-119">Bir *unsafe_statement* güvenli olmayan bir bağlam içinde kullanımını sağlayan bir *blok*.</span><span class="sxs-lookup"><span data-stu-id="53d9f-119">An *unsafe_statement* enables the use of an unsafe context within a *block*.</span></span> <span data-ttu-id="53d9f-120">Metinsel ilişkili tüm kapsamı *blok* güvenli olmayan bir bağlam kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-120">The entire textual extent of the associated *block* is considered an unsafe context.</span></span>

<span data-ttu-id="53d9f-121">İlişkili dilbilgisi üretim aşağıda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-121">The associated grammar productions are shown below.</span></span>

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

<span data-ttu-id="53d9f-122">Örnekte</span><span class="sxs-lookup"><span data-stu-id="53d9f-122">In the example</span></span>

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

<span data-ttu-id="53d9f-123">`unsafe` struct bildiriminde belirtilen değiştirici güvenli olmayan bir bağlam olmasını yapı bildirimi tüm metinsel kapsamını neden olur.</span><span class="sxs-lookup"><span data-stu-id="53d9f-123">the `unsafe` modifier specified in the struct declaration causes the entire textual extent of the struct declaration to become an unsafe context.</span></span> <span data-ttu-id="53d9f-124">Bu nedenle, bildirmek olası `Left` ve `Right` işaretçi türünde alanlar.</span><span class="sxs-lookup"><span data-stu-id="53d9f-124">Thus, it is possible to declare the `Left` and `Right` fields to be of a pointer type.</span></span> <span data-ttu-id="53d9f-125">Yukarıdaki örnekte ayrıca yazılabilir</span><span class="sxs-lookup"><span data-stu-id="53d9f-125">The example above could also be written</span></span>

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

<span data-ttu-id="53d9f-126">Burada, `unsafe` alanın bildiriminde değiştiriciler neden güvensiz bir bağlamı olarak kabul edilmesi bu bildirimleri.</span><span class="sxs-lookup"><span data-stu-id="53d9f-126">Here, the `unsafe` modifiers in the field declarations cause those declarations to be considered unsafe contexts.</span></span>

<span data-ttu-id="53d9f-127">Bu nedenle işaretçi türleri kullanımına izin veren bir güvenli olmayan bir bağlam oluşturma dışında `unsafe` değiştiricisi bir tür veya üye üzerinde hiçbir etkisi.</span><span class="sxs-lookup"><span data-stu-id="53d9f-127">Other than establishing an unsafe context, thus permitting the use of pointer types, the `unsafe` modifier has no effect on a type or a member.</span></span> <span data-ttu-id="53d9f-128">Örnekte</span><span class="sxs-lookup"><span data-stu-id="53d9f-128">In the example</span></span>

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

<span data-ttu-id="53d9f-129">`unsafe` değiştiricisi `F` yönteminde `A` metinsel kapsamını yalnızca neden `F` güvenli olmayan dil özelliklerinin kullanılabilecek güvenli olmayan bir bağlam olacak.</span><span class="sxs-lookup"><span data-stu-id="53d9f-129">the `unsafe` modifier on the `F` method in `A` simply causes the textual extent of `F` to become an unsafe context in which the unsafe features of the language can be used.</span></span> <span data-ttu-id="53d9f-130">Geçersiz kılmasını içinde `F` içinde `B`, yeniden belirtmek için gerek yoktur `unsafe` değiştiricisi--sürece, Elbette `F` yönteminde `B` kendisini güvensiz özellikleri erişmesi gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="53d9f-130">In the override of `F` in `B`, there is no need to re-specify the `unsafe` modifier -- unless, of course, the `F` method in `B` itself needs access to unsafe features.</span></span>

<span data-ttu-id="53d9f-131">Bir işaretçi türü yöntemin imzasını bir parçası olduğunda durumu biraz farklıdır</span><span class="sxs-lookup"><span data-stu-id="53d9f-131">The situation is slightly different when a pointer type is part of the method's signature</span></span>

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

<span data-ttu-id="53d9f-132">Burada, çünkü `F`'s imzası içerir bir işaretçi türü, onu yalnızca güvenli olmayan bir bağlamda yazılabilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-132">Here, because `F`'s signature includes a pointer type, it can only be written in an unsafe context.</span></span> <span data-ttu-id="53d9f-133">Ancak, güvenli olmayan bir bağlam içinde olduğu gibi ya da sınıfın tamamı güvenli değildir, yaparak tanıtılabilir `A`, ya da dahil olmak üzere bir `unsafe` değiştirici yöntem bildiriminde içinde olduğu gibi `B`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-133">However, the unsafe context can be introduced by either making the entire class unsafe, as is the case in `A`, or by including an `unsafe` modifier in the method declaration, as is the case in `B`.</span></span>

## <a name="pointer-types"></a><span data-ttu-id="53d9f-134">İşaretçi türleri</span><span class="sxs-lookup"><span data-stu-id="53d9f-134">Pointer types</span></span>

<span data-ttu-id="53d9f-135">Güvenli olmayan bir bağlamda bir *türü* ([türleri](types.md)) olabilir bir *pointer_type* yanı *value_type* veya *reference_type* .</span><span class="sxs-lookup"><span data-stu-id="53d9f-135">In an unsafe context, a *type* ([Types](types.md)) may be a *pointer_type* as well as a *value_type* or a *reference_type*.</span></span> <span data-ttu-id="53d9f-136">Ancak, bir *pointer_type* içinde de kullanılabilir bir `typeof` ifade ([anonim nesne oluşturma ifadeleri](expressions.md#anonymous-object-creation-expressions)) dışında güvenli olmayan bir bağlam nedenle kullanım güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-136">However, a *pointer_type* may also be used in a `typeof` expression ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) outside of an unsafe context as such usage is not unsafe.</span></span>

```antlr
type_unsafe
    : pointer_type
    ;
```

<span data-ttu-id="53d9f-137">A *pointer_type* olarak yazılmış bir *unmanaged_type* veya anahtar sözcüğü `void`ve ardından bir `*` belirteci:</span><span class="sxs-lookup"><span data-stu-id="53d9f-137">A *pointer_type* is written as an *unmanaged_type* or the keyword `void`, followed by a `*` token:</span></span>

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

<span data-ttu-id="53d9f-138">Önce belirtilen tür `*` bir işaretçi türü olarak adlandırılır ***başvurulan türü*** işaretçi türü.</span><span class="sxs-lookup"><span data-stu-id="53d9f-138">The type specified before the `*` in a pointer type is called the ***referent type*** of the pointer type.</span></span> <span data-ttu-id="53d9f-139">Bu değişkenin işaret ettiği işaretçi türünde bir değer türünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="53d9f-139">It represents the type of the variable to which a value of the pointer type points.</span></span>

<span data-ttu-id="53d9f-140">Başvuruları (başvuru türleri değerlerinin), farklı işaretçiler çöp toplayıcı tarafından izlenmez--çöp toplayıcı işaretçiler ve bunların işaret veri olanağıyla sahiptir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-140">Unlike references (values of reference types), pointers are not tracked by the garbage collector -- the garbage collector has no knowledge of pointers and the data to which they point.</span></span> <span data-ttu-id="53d9f-141">Bu nedenle bir işaretçi, başvuru noktası izin verilmiyor veya bir yapı için başvuruları içeren ve grup türü bir işaretçi olmalıdır bir *unmanaged_type*.</span><span class="sxs-lookup"><span data-stu-id="53d9f-141">For this reason a pointer is not permitted to point to a reference or to a struct that contains references, and the referent type of a pointer must be an *unmanaged_type*.</span></span>

<span data-ttu-id="53d9f-142">Bir *unmanaged_type* olmayan herhangi bir tür bir *reference_type* veya türü oluşturulur ve içermiyor *reference_type* veya oluşturulan tüm düzeylerindeki tür alanları iç içe geçirme.</span><span class="sxs-lookup"><span data-stu-id="53d9f-142">An *unmanaged_type* is any type that isn't a *reference_type* or constructed type, and doesn't contain *reference_type* or constructed type fields at any level of nesting.</span></span> <span data-ttu-id="53d9f-143">Diğer bir deyişle, bir *unmanaged_type* aşağıdakilerden biridir:</span><span class="sxs-lookup"><span data-stu-id="53d9f-143">In other words, an *unmanaged_type* is one of the following:</span></span>

*  <span data-ttu-id="53d9f-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, veya `bool`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, or `bool`.</span></span>
*  <span data-ttu-id="53d9f-145">Tüm *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="53d9f-145">Any *enum_type*.</span></span>
*  <span data-ttu-id="53d9f-146">Tüm *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="53d9f-146">Any *pointer_type*.</span></span>
*  <span data-ttu-id="53d9f-147">Tüm kullanıcı tanımlı *struct_type* oluşturulan bir tür değil ve alanlarını içerir *unmanaged_type*yalnızca s.</span><span class="sxs-lookup"><span data-stu-id="53d9f-147">Any user-defined *struct_type* that is not a constructed type and contains fields of *unmanaged_type*s only.</span></span>

<span data-ttu-id="53d9f-148">İşaretçiler ve başvurular karıştırmak için sezgisel referents başvuruları (nesneler) işaretçiler içeren izin verilir, ancak referents işaretçiler, başvurular izin verilmeyen kuralıdır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-148">The intuitive rule for mixing of pointers and references is that referents of references (objects) are permitted to contain pointers, but referents of pointers are not permitted to contain references.</span></span>

<span data-ttu-id="53d9f-149">İşaretçi türlerinin bazı örnekler, aşağıdaki tabloda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="53d9f-149">Some examples of pointer types are given in the table below:</span></span>

| <span data-ttu-id="53d9f-150">__Örnek__</span><span class="sxs-lookup"><span data-stu-id="53d9f-150">__Example__</span></span> | <span data-ttu-id="53d9f-151">__Açıklama__</span><span class="sxs-lookup"><span data-stu-id="53d9f-151">__Description__</span></span>                               |
|-------------|-----------------------------------------------|
| `byte*`     | <span data-ttu-id="53d9f-152">İşaretçi `byte`</span><span class="sxs-lookup"><span data-stu-id="53d9f-152">Pointer to `byte`</span></span>                             |
| `char*`     | <span data-ttu-id="53d9f-153">İşaretçi `char`</span><span class="sxs-lookup"><span data-stu-id="53d9f-153">Pointer to `char`</span></span>                             |
| `int**`     | <span data-ttu-id="53d9f-154">İşaretçi işaretçisi `int`</span><span class="sxs-lookup"><span data-stu-id="53d9f-154">Pointer to pointer to `int`</span></span>                   |
| `int*[]`    | <span data-ttu-id="53d9f-155">Tek boyutlu dizi işaretçileri `int`</span><span class="sxs-lookup"><span data-stu-id="53d9f-155">Single-dimensional array of pointers to `int`</span></span> |
| `void*`     | <span data-ttu-id="53d9f-156">Bilinmeyen tür işaretçisi</span><span class="sxs-lookup"><span data-stu-id="53d9f-156">Pointer to unknown type</span></span>                       |

<span data-ttu-id="53d9f-157">Belirli bir uygulama için tüm işaretçi türleri aynı büyüklük ve gösterimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-157">For a given implementation, all pointer types must have the same size and representation.</span></span>

<span data-ttu-id="53d9f-158">C ve C++, C# ' de aynı bildirimde birden çok işaretçi bildirildiğinde aksine `*` yalnızca, temel alınan türü ile birlikte, her bir işaretçi adı bir önek noktalama işaretçisi olarak yazılır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-158">Unlike C and C++, when multiple pointers are declared in the same declaration, in C# the `*` is written along with the underlying type only, not as a prefix punctuator on each pointer name.</span></span> <span data-ttu-id="53d9f-159">Örneğin</span><span class="sxs-lookup"><span data-stu-id="53d9f-159">For example</span></span>

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

<span data-ttu-id="53d9f-160">Türü olan bir işaretçi değeri `T*` türünde bir değişkenin adresini temsil eden `T`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-160">The value of a pointer having type `T*` represents the address of a variable of type `T`.</span></span> <span data-ttu-id="53d9f-161">İşaretçi yöneltme işleci `*` ([işaretçi yöneltmesi](unsafe-code.md#pointer-indirection)) Bu değişken erişmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-161">The pointer indirection operator `*` ([Pointer indirection](unsafe-code.md#pointer-indirection)) may be used to access this variable.</span></span> <span data-ttu-id="53d9f-162">Örneğin, bir değişken verilen `P` türü `int*`, ifade `*P` gösterir `int` yer alan adreste bulunan değişkeni `P`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-162">For example, given a variable `P` of type `int*`, the expression `*P` denotes the `int` variable found at the address contained in `P`.</span></span>

<span data-ttu-id="53d9f-163">Bir nesne başvurusu gibi bir işaretçi olabilir `null`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-163">Like an object reference, a pointer may be `null`.</span></span> <span data-ttu-id="53d9f-164">İçin yöneltme işlecini bir `null` işaretçi uygulama tanımlı davranış sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-164">Applying the indirection operator to a `null` pointer results in implementation-defined behavior.</span></span> <span data-ttu-id="53d9f-165">Bir işaretçi değeri `null` BITS tüm sıfır tarafından temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-165">A pointer with value `null` is represented by all-bits-zero.</span></span>

<span data-ttu-id="53d9f-166">`void*` Türünü temsil eden bir işaretçi türü bilinmiyor.</span><span class="sxs-lookup"><span data-stu-id="53d9f-166">The `void*` type represents a pointer to an unknown type.</span></span> <span data-ttu-id="53d9f-167">Grup türü bilinmediğinden, türünde bir işaretçi yöneltme işleci uygulanamaz `void*`, ya da herhangi bir aritmetik tür bir işaretçi üzerinde gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-167">Because the referent type is unknown, the indirection operator cannot be applied to a pointer of type `void*`, nor can any arithmetic be performed on such a pointer.</span></span> <span data-ttu-id="53d9f-168">Ancak, bir işaretçi türü `void*` herhangi bir işaretçi türü (ve tersi) dönüştürme olabilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-168">However, a pointer of type `void*` can be cast to any other pointer type (and vice versa).</span></span>

<span data-ttu-id="53d9f-169">İşaretçi türleri için ayrı bir kategori türleridir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-169">Pointer types are a separate category of types.</span></span> <span data-ttu-id="53d9f-170">İşaretçi türleri seçeneğinden devralmamanızı başvuru türleri ve değer türlerinin aksine, `object` ve işaretçi türleri arasında dönüştürme işlemi yok var ve `object`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-170">Unlike reference types and value types, pointer types do not inherit from `object` and no conversions exist between pointer types and `object`.</span></span> <span data-ttu-id="53d9f-171">Özellikle, kutulama ve kutudan çıkarma ([kutulama ve kutudan çıkarma](types.md#boxing-and-unboxing)) işaretçiler için desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="53d9f-171">In particular, boxing and unboxing ([Boxing and unboxing](types.md#boxing-and-unboxing)) are not supported for pointers.</span></span> <span data-ttu-id="53d9f-172">Ancak, farklı işaretçi türleri ve işaretçi türleri ve tamsayı türleri arasında dönüştürmeler izin verilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-172">However, conversions are permitted between different pointer types and between pointer types and the integral types.</span></span> <span data-ttu-id="53d9f-173">Bu açıklanan [işaretçi dönüşümleri](unsafe-code.md#pointer-conversions).</span><span class="sxs-lookup"><span data-stu-id="53d9f-173">This is described in [Pointer conversions](unsafe-code.md#pointer-conversions).</span></span>

<span data-ttu-id="53d9f-174">A *pointer_type* tür bağımsız değişkeni kullanılamaz ([oluşturulan türler](types.md#constructed-types)) ve anlam çıkarma ([anlam çıkarma](expressions.md#type-inference)) algılanan genel yöntem çağrıları başarısız bir tür bağımsız değişkeni, bir işaretçi türü.</span><span class="sxs-lookup"><span data-stu-id="53d9f-174">A *pointer_type* cannot be used as a type argument ([Constructed types](types.md#constructed-types)), and type inference ([Type inference](expressions.md#type-inference)) fails on generic method calls that would have inferred a type argument to be a pointer type.</span></span>

<span data-ttu-id="53d9f-175">A *pointer_type* geçici bir alan türü olarak kullanılabilir ([geçici alanları](classes.md#volatile-fields)).</span><span class="sxs-lookup"><span data-stu-id="53d9f-175">A *pointer_type* may be used as the type of a volatile field ([Volatile fields](classes.md#volatile-fields)).</span></span>

<span data-ttu-id="53d9f-176">İşaretçileri olarak geçirilebilir ancak `ref` veya `out` parametreleri, işaretçi yanı sıra beri tanımsız davranışa neden olabilir, böylece çağrılan yöntem döndürüldüğünde, artık var olmayan bir yerel değişken veya sabit hangi BT nesneye işaret edecek şekilde ayarlanması işaret etmek için kullanılan, artık düzeltildi.</span><span class="sxs-lookup"><span data-stu-id="53d9f-176">Although pointers can be passed as `ref` or `out` parameters, doing so can cause undefined behavior, since the pointer may well be set to point to a local variable which no longer exists when the called method returns, or the fixed object to which it used to point, is no longer fixed.</span></span> <span data-ttu-id="53d9f-177">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="53d9f-177">For example:</span></span>

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

<span data-ttu-id="53d9f-178">Bir yöntem bazı türünde bir değer döndürebilir ve bu tür bir işaretçi olabilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-178">A method can return a value of some type, and that type can be a pointer.</span></span> <span data-ttu-id="53d9f-179">Örneğin, bir işaretçi bitişik dizisi ile verildiğinde `int`s, bu sırasının öğe sayısı ve bazı diğer `int` değeri, aşağıdaki yöntemi bir eşleşme olursa o sırada değeri adresini döndürür; Aksi halde döndürür`null`:</span><span class="sxs-lookup"><span data-stu-id="53d9f-179">For example, when given a pointer to a contiguous sequence of `int`s, that sequence's element count, and some other `int` value, the following method returns the address of that value in that sequence, if a match occurs; otherwise it returns `null`:</span></span>

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

<span data-ttu-id="53d9f-180">Güvenli olmayan bir bağlamda işaretçiler üzerinde çalışan için çeşitli yapıları kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="53d9f-180">In an unsafe context, several constructs are available for operating on pointers:</span></span>

*  <span data-ttu-id="53d9f-181">`*` İşleç, işaretçi yöneltmesi gerçekleştirmek için kullanılabilir ([işaretçi yöneltmesi](unsafe-code.md#pointer-indirection)).</span><span class="sxs-lookup"><span data-stu-id="53d9f-181">The `*` operator may be used to perform pointer indirection ([Pointer indirection](unsafe-code.md#pointer-indirection)).</span></span>
*  <span data-ttu-id="53d9f-182">`->` İşleci bir işaretçiyi bir yapı üyesi erişmek için kullanılabilir ([işaretçi üye erişimi](unsafe-code.md#pointer-member-access)).</span><span class="sxs-lookup"><span data-stu-id="53d9f-182">The `->` operator may be used to access a member of a struct through a pointer ([Pointer member access](unsafe-code.md#pointer-member-access)).</span></span>
*  <span data-ttu-id="53d9f-183">`[]` İşleci, bir işaretçi dizini oluşturmak için kullanılabilir ([işaretçi öğe erişimi](unsafe-code.md#pointer-element-access)).</span><span class="sxs-lookup"><span data-stu-id="53d9f-183">The `[]` operator may be used to index a pointer ([Pointer element access](unsafe-code.md#pointer-element-access)).</span></span>
*  <span data-ttu-id="53d9f-184">`&` İşleci, bir değişkenin adresini almak için kullanılabilir ([address-of işlecini](unsafe-code.md#the-address-of-operator)).</span><span class="sxs-lookup"><span data-stu-id="53d9f-184">The `&` operator may be used to obtain the address of a variable ([The address-of operator](unsafe-code.md#the-address-of-operator)).</span></span>
*  <span data-ttu-id="53d9f-185">`++` Ve `--` işaretçileri artırma ve azaltma işleçleri kullanılabilir ([işaretçi artırma ve azaltma](unsafe-code.md#pointer-increment-and-decrement)).</span><span class="sxs-lookup"><span data-stu-id="53d9f-185">The `++` and `--` operators may be used to increment and decrement pointers ([Pointer increment and decrement](unsafe-code.md#pointer-increment-and-decrement)).</span></span>
*  <span data-ttu-id="53d9f-186">`+` Ve `-` işleçleri, işaretçi aritmetik işlemleri için kullanılabilir ([işaretçi aritmetiği](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="53d9f-186">The `+` and `-` operators may be used to perform pointer arithmetic ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span>
*  <span data-ttu-id="53d9f-187">`==`, `!=`, `<`, `>`, `<=`, Ve `=>` işaretçileri Karşılaştırma işleçleri kullanılabilir ([işaretçi karşılaştırması](unsafe-code.md#pointer-comparison)).</span><span class="sxs-lookup"><span data-stu-id="53d9f-187">The `==`, `!=`, `<`, `>`, `<=`, and `=>` operators may be used to compare pointers ([Pointer comparison](unsafe-code.md#pointer-comparison)).</span></span>
*  <span data-ttu-id="53d9f-188">`stackalloc` İşleci, çağrı yığınından bellek ayırmak için kullanılabilir ([sabit boyutlu arabellekler](unsafe-code.md#fixed-size-buffers)).</span><span class="sxs-lookup"><span data-stu-id="53d9f-188">The `stackalloc` operator may be used to allocate memory from the call stack ([Fixed size buffers](unsafe-code.md#fixed-size-buffers)).</span></span>
*  <span data-ttu-id="53d9f-189">`fixed` Deyimi adresini alınabilmesi için bir değişkeni geçici olarak gidermek için kullanılabilir ([fixed deyimi](unsafe-code.md#the-fixed-statement)).</span><span class="sxs-lookup"><span data-stu-id="53d9f-189">The `fixed` statement may be used to temporarily fix a variable so its address can be obtained ([The fixed statement](unsafe-code.md#the-fixed-statement)).</span></span>

## <a name="fixed-and-moveable-variables"></a><span data-ttu-id="53d9f-190">Sabit ve taşınabilir değişkenleri</span><span class="sxs-lookup"><span data-stu-id="53d9f-190">Fixed and moveable variables</span></span>

<span data-ttu-id="53d9f-191">Address-of işlecini ([address-of işlecini](unsafe-code.md#the-address-of-operator)) ve `fixed` deyimi ([fixed deyimi](unsafe-code.md#the-fixed-statement)) iki kategoride değişkenleri bölmek: ***değişkenlerisabit***ve ***taşınabilir değişkenleri***.</span><span class="sxs-lookup"><span data-stu-id="53d9f-191">The address-of operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) and the `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) divide variables into two categories: ***Fixed variables*** and ***moveable variables***.</span></span>

<span data-ttu-id="53d9f-192">Sabit değişkenler, atık toplayıcı bir işlem tarafından etkilenmez, depolama konumları olarak yer alır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-192">Fixed variables reside in storage locations that are unaffected by operation of the garbage collector.</span></span> <span data-ttu-id="53d9f-193">(Sabit değişkenler yerel değişkenler, değer parametreleri ve değişkenleri başvuruluyor işaretçiler tarafından oluşturulan örneklerindendir.) Öte yandan, yeniden konumlandırma veya çöp toplayıcı tarafından elden tabidir depolama konumları olarak taşınabilir değişkenleri bulunur.</span><span class="sxs-lookup"><span data-stu-id="53d9f-193">(Examples of fixed variables include local variables, value parameters, and variables created by dereferencing pointers.) On the other hand, moveable variables reside in storage locations that are subject to relocation or disposal by the garbage collector.</span></span> <span data-ttu-id="53d9f-194">(Taşınabilir değişkenleri örnekleri alanları nesneler ve diziler öğelerini içerir.)</span><span class="sxs-lookup"><span data-stu-id="53d9f-194">(Examples of moveable variables include fields in objects and elements of arrays.)</span></span>

<span data-ttu-id="53d9f-195">`&` İşleci ([address-of işlecini](unsafe-code.md#the-address-of-operator)) kısıtlama olmadan alınacağı sabit değişkenin adresini verir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-195">The `&` operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) permits the address of a fixed variable to be obtained without restrictions.</span></span> <span data-ttu-id="53d9f-196">Yeniden konumlandırma veya çöp toplayıcı tarafından elden tabi taşınabilir bir değişken olduğundan, ancak taşınabilir değişkenin adresini yalnızca kullanılarak elde edilebilir bir `fixed` deyimi ([fixed deyimi](unsafe-code.md#the-fixed-statement)) ve bu adresi yalnızca bu süre için geçerli kalır `fixed` deyimi.</span><span class="sxs-lookup"><span data-stu-id="53d9f-196">However, because a moveable variable is subject to relocation or disposal by the garbage collector, the address of a moveable variable can only be obtained using a `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)), and that address remains valid only for the duration of that `fixed` statement.</span></span>

<span data-ttu-id="53d9f-197">Kesin bağlamında, sabit bir değişken aşağıdakilerden birini verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="53d9f-197">In precise terms, a fixed variable is one of the following:</span></span>

*  <span data-ttu-id="53d9f-198">Bir değişken kaynaklanan bir *simple_name* ([basit adları](expressions.md#simple-names)) başvuran bir yerel değişken ya da bir değer parametresini sürece değişkeni anonim bir işlev tarafından yakalanır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-198">A variable resulting from a *simple_name* ([Simple names](expressions.md#simple-names)) that refers to a local variable or a value parameter, unless the variable is captured by an anonymous function.</span></span>
*  <span data-ttu-id="53d9f-199">Bir değişken kaynaklanan bir *member_access* ([üye erişimi](expressions.md#member-access)) biçiminde `V.I`burada `V` , sabit bir değişken bir *struct_type*.</span><span class="sxs-lookup"><span data-stu-id="53d9f-199">A variable resulting from a *member_access* ([Member access](expressions.md#member-access)) of the form `V.I`, where `V` is a fixed variable of a *struct_type*.</span></span>
*  <span data-ttu-id="53d9f-200">Bir değişken kaynaklanan bir *pointer_indirection_expression* ([işaretçi yöneltmesi](unsafe-code.md#pointer-indirection)) biçiminde `*P`, *pointer_member_access* ([İşaretçi üye erişimi](unsafe-code.md#pointer-member-access)) biçiminde `P->I`, veya bir *pointer_element_access* ([işaretçi öğe erişimi](unsafe-code.md#pointer-element-access)) biçiminde `P[E]`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-200">A variable resulting from a *pointer_indirection_expression* ([Pointer indirection](unsafe-code.md#pointer-indirection)) of the form `*P`, a *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) of the form `P->I`, or a *pointer_element_access* ([Pointer element access](unsafe-code.md#pointer-element-access)) of the form `P[E]`.</span></span>

<span data-ttu-id="53d9f-201">Diğer tüm değişkenleri taşınabilir değişkenleri olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-201">All other variables are classified as moveable variables.</span></span>

<span data-ttu-id="53d9f-202">Statik alan taşınabilir bir değişken olarak sınıflandırılır unutmayın.</span><span class="sxs-lookup"><span data-stu-id="53d9f-202">Note that a static field is classified as a moveable variable.</span></span> <span data-ttu-id="53d9f-203">Ayrıca bir `ref` veya `out` parametresi parametresi için belirtilen bağımsız değişkeni sabit bir değişken olsa bile taşınabilir bir değişken olarak sınıflandırılmış.</span><span class="sxs-lookup"><span data-stu-id="53d9f-203">Also note that a `ref` or `out` parameter is classified as a moveable variable, even if the argument given for the parameter is a fixed variable.</span></span> <span data-ttu-id="53d9f-204">Son olarak, bir işaretçinin tarafından üretilen bir değişken her zaman sabit bir değişken olarak sınıflandırıldığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="53d9f-204">Finally, note that a variable produced by dereferencing a pointer is always classified as a fixed variable.</span></span>

## <a name="pointer-conversions"></a><span data-ttu-id="53d9f-205">İşaretçi dönüşümleri</span><span class="sxs-lookup"><span data-stu-id="53d9f-205">Pointer conversions</span></span>

<span data-ttu-id="53d9f-206">Mevcut örtük dönüştürmeler kümesini güvenli olmayan bir bağlamda ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) aşağıdaki örtük işaretçi dönüşümleri kapsayacak şekilde genişletilir:</span><span class="sxs-lookup"><span data-stu-id="53d9f-206">In an unsafe context, the set of available implicit conversions ([Implicit conversions](conversions.md#implicit-conversions)) is extended to include the following implicit pointer conversions:</span></span>

*  <span data-ttu-id="53d9f-207">Herhangi *pointer_type* türüne `void*`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-207">From any *pointer_type* to the type `void*`.</span></span>
*  <span data-ttu-id="53d9f-208">Gelen `null` herhangi bir değişmez değer *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="53d9f-208">From the `null` literal to any *pointer_type*.</span></span>

<span data-ttu-id="53d9f-209">Ayrıca, güvenli olmayan bir bağlamda, kullanılabilir açık dönüştürmeler kümesini ([açık dönüştürmeler](conversions.md#explicit-conversions)) aşağıdaki açık işaretçi dönüşümleri kapsayacak şekilde genişletilir:</span><span class="sxs-lookup"><span data-stu-id="53d9f-209">Additionally, in an unsafe context, the set of available explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) is extended to include the following explicit pointer conversions:</span></span>

*  <span data-ttu-id="53d9f-210">Herhangi *pointer_type* herhangi diğer *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="53d9f-210">From any *pointer_type* to any other *pointer_type*.</span></span>
*  <span data-ttu-id="53d9f-211">Gelen `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, veya `ulong` herhangi *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="53d9f-211">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong` to any *pointer_type*.</span></span>
*  <span data-ttu-id="53d9f-212">Herhangi *pointer_type* için `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, veya `ulong`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-212">From any *pointer_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="53d9f-213">Son olarak, güvenli olmayan bir bağlamda, bir dizi standart örtük dönüştürmeler ([standart örtük dönüştürmeler](conversions.md#standard-implicit-conversions)) aşağıdaki işaretçi dönüştürme içerir:</span><span class="sxs-lookup"><span data-stu-id="53d9f-213">Finally, in an unsafe context, the set of standard implicit conversions ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) includes the following pointer conversion:</span></span>

*  <span data-ttu-id="53d9f-214">Herhangi *pointer_type* türüne `void*`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-214">From any *pointer_type* to the type `void*`.</span></span>

<span data-ttu-id="53d9f-215">İki işaretçi türleri arasında dönüştürmeler, gerçek bir işaretçi değeri hiçbir zaman değiştirin.</span><span class="sxs-lookup"><span data-stu-id="53d9f-215">Conversions between two pointer types never change the actual pointer value.</span></span> <span data-ttu-id="53d9f-216">Diğer bir deyişle, bir işaretçi türüne dönüştürmeye başka bir işaretçi tarafından verilen temel adresi üzerinde etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="53d9f-216">In other words, a conversion from one pointer type to another has no effect on the underlying address given by the pointer.</span></span>

<span data-ttu-id="53d9f-217">Elde edilen işaretçi işaret edilen türü için doğru bir şekilde hizalanmamış bir işaretçi türü diğerine dönüştürülür, sonuç referans edildi, davranış tanımlanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-217">When one pointer type is converted to another, if the resulting pointer is not correctly aligned for the pointed-to type, the behavior is undefined if the result is dereferenced.</span></span> <span data-ttu-id="53d9f-218">Genel olarak, "düzgün hizalanmış" kavramı geçişli: tür işaretçisi, `A` tür işaretçisi için düzgün hizalanmış `B`, sırasıyla bir işaretçi türüne doğru hizalanır `C`, ardından bir işaretçi türüne `A`bir işaretçi türüne düzgün hizalanmış `C`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-218">In general, the concept "correctly aligned" is transitive: if a pointer to type `A` is correctly aligned for a pointer to type `B`, which, in turn, is correctly aligned for a pointer to type `C`, then a pointer to type `A` is correctly aligned for a pointer to type `C`.</span></span>

<span data-ttu-id="53d9f-219">Bir türü sahip bir değişken, farklı türden bir işaretçi aracılığıyla erişilir aşağıdaki örneği inceleyin:</span><span class="sxs-lookup"><span data-stu-id="53d9f-219">Consider the following case in which a variable having one type is accessed via a pointer to a different type:</span></span>

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

<span data-ttu-id="53d9f-220">Ne zaman bir işaretçi türü bayt, bir işaretçi değişken sonuç noktalarını düşük adresli bayta dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="53d9f-220">When a pointer type is converted to a pointer to byte, the result points to the lowest addressed byte of the variable.</span></span> <span data-ttu-id="53d9f-221">Bu değişkenin kalan baytlar işaretçilere değişkeni en fazla sonuç art arda gelen artışlarla yield.</span><span class="sxs-lookup"><span data-stu-id="53d9f-221">Successive increments of the result, up to the size of the variable, yield pointers to the remaining bytes of that variable.</span></span> <span data-ttu-id="53d9f-222">Örneğin, aşağıdaki yöntemi her sekiz bayt bir çift olarak bir onaltılık değer görüntüler:</span><span class="sxs-lookup"><span data-stu-id="53d9f-222">For example, the following method displays each of the eight bytes in a double as a hexadecimal value:</span></span>

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

<span data-ttu-id="53d9f-223">Elbette, üretilen çıktı endianness üzerinde bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-223">Of course, the output produced depends on endianness.</span></span>

<span data-ttu-id="53d9f-224">Uygulama tanımlı işaretçiler ve tamsayılar arasındaki eşlemeleri.</span><span class="sxs-lookup"><span data-stu-id="53d9f-224">Mappings between pointers and integers are implementation-defined.</span></span> <span data-ttu-id="53d9f-225">Ancak, 32 üzerinde \* ve 64-bit CPU mimarileri doğrusal bir adres alanı ile ya da tam sayı türlerinden dönüştürmeler işaretçiler genelde tam olarak dönüştürülme gibi davranır `uint` veya `ulong` değerler, sırasıyla ya da bu tam sayı türlerinden.</span><span class="sxs-lookup"><span data-stu-id="53d9f-225">However, on 32\* and 64-bit CPU architectures with a linear address space, conversions of pointers to or from integral types typically behave exactly like conversions of `uint` or `ulong` values, respectively, to or from those integral types.</span></span>

### <a name="pointer-arrays"></a><span data-ttu-id="53d9f-226">İşaretçi dizileri</span><span class="sxs-lookup"><span data-stu-id="53d9f-226">Pointer arrays</span></span>

<span data-ttu-id="53d9f-227">Güvenli olmayan bir bağlamda işaretçiler dizileri oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-227">In an unsafe context, arrays of pointers can be constructed.</span></span> <span data-ttu-id="53d9f-228">Diğer bir dizi türü için geçerli dönüşümler yalnızca bazıları, işaretçi dizilerde izin verilir:</span><span class="sxs-lookup"><span data-stu-id="53d9f-228">Only some of the conversions that apply to other array types are allowed on pointer arrays:</span></span>

*  <span data-ttu-id="53d9f-229">Örtük bir başvuru dönüştürmesi ([örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions)) herhangi *array_type* için `System.Array` ve ayrıca bunu uyguladığı arayüzlerin işaretçi dizileri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-229">The implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) from any *array_type* to `System.Array` and the interfaces it implements also applies to pointer arrays.</span></span> <span data-ttu-id="53d9f-230">Ancak, tüm dizi öğeleri aracılığıyla erişme girişimi `System.Array` veya işaretçi türleri dönüştürülebilir değildir. Bunu uygulayan arabirimler çalışma zamanı bir özel duruma neden olur `object`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-230">However, any attempt to access the array elements through `System.Array` or the interfaces it implements will result in an exception at run-time, as pointer types are not convertible to `object`.</span></span>
*  <span data-ttu-id="53d9f-231">Örtük ve açık dönüştürmeler başvuru ([örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions), [açık başvuru dönüşümleri](conversions.md#explicit-reference-conversions)) bir tek boyutlu dizi türünden `S[]` için `System.Collections.Generic.IList<T>` ve Genel temel arabirimlerinden işaretçi türleri, tür bağımsız değişkeni olarak kullanılamaz ve işaretçi olmayan türlerin işaretçi türünden hiçbir dönüştürme hiçbir zaman işaretçi dizileri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-231">The implicit and explicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions), [Explicit reference conversions](conversions.md#explicit-reference-conversions)) from a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its generic base interfaces never apply to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>
*  <span data-ttu-id="53d9f-232">Açık bir başvuru dönüştürmesi ([açık başvuru dönüşümleri](conversions.md#explicit-reference-conversions)) öğesinden `System.Array` ve arabirimler için uygular *array_type* işaretçi dizileri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-232">The explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Array` and the interfaces it implements to any *array_type* applies to pointer arrays.</span></span>
*  <span data-ttu-id="53d9f-233">Açık bir başvuru Dönüşümleri ([açık başvuru dönüşümleri](conversions.md#explicit-reference-conversions)) öğesinden `System.Collections.Generic.IList<S>` ve bunun temel arabirimleri için bir tek boyutlu dizi türü `T[]` işaretçi türleri olamaz bu yana hiçbir zaman işaretçi dizileri için geçerlidir kullanılan farklı tür bağımsız değişkenleri ve işaretçi olmayan türlerin işaretçi türünden dönüştürme yok.</span><span class="sxs-lookup"><span data-stu-id="53d9f-233">The explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]` never applies to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>

<span data-ttu-id="53d9f-234">Bu kısıtlamalar için genişletme anlamına `foreach` diziler üzerinden deyimi açıklanan [foreach deyimi](statements.md#the-foreach-statement) işaretçi dizileri için uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="53d9f-234">These restrictions mean that the expansion for the `foreach` statement over arrays described in [The foreach statement](statements.md#the-foreach-statement) cannot be applied to pointer arrays.</span></span> <span data-ttu-id="53d9f-235">Bunun yerine, formun foreach deyimi</span><span class="sxs-lookup"><span data-stu-id="53d9f-235">Instead, a foreach statement of the form</span></span>

```csharp
foreach (V v in x) embedded_statement
```

<span data-ttu-id="53d9f-236">Burada türünü `x` biçiminde bir dizi türü `T[,,...,]`, `N` eksi 1 dimensions sayısı ve `T` veya `V` işaretçi türünde, iç içe geçmiş for döngüleri kullanarak aşağıdaki gibi genişletilir:</span><span class="sxs-lookup"><span data-stu-id="53d9f-236">where the type of `x` is an array type of the form `T[,,...,]`, `N` is the number of dimensions minus 1 and `T` or `V` is a pointer type, is expanded using nested for-loops as follows:</span></span>

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

<span data-ttu-id="53d9f-237">Değişkenleri `a`, `i0`, `i1`,..., `iN` görünür veya erişilebilir olmayan `x` veya *embedded_statement* veya programın başka bir kaynak kodu.</span><span class="sxs-lookup"><span data-stu-id="53d9f-237">The variables `a`, `i0`, `i1`, ..., `iN` are not visible to or accessible to `x` or the *embedded_statement* or any other source code of the program.</span></span> <span data-ttu-id="53d9f-238">Değişken `v` katıştırılmış deyiminde salt okunur.</span><span class="sxs-lookup"><span data-stu-id="53d9f-238">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="53d9f-239">Açık bir dönüştürme ise ([işaretçi dönüşümleri](unsafe-code.md#pointer-conversions)) öğesinden `T` (öğe türü) için `V`, bir hata oluşturulur ve başka bir adım alınır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-239">If there is not an explicit conversion ([Pointer conversions](unsafe-code.md#pointer-conversions)) from `T` (the element type) to `V`, an error is produced and no further steps are taken.</span></span> <span data-ttu-id="53d9f-240">Varsa `x` değerine sahip `null`, `System.NullReferenceException` çalışma zamanında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="53d9f-240">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

## <a name="pointers-in-expressions"></a><span data-ttu-id="53d9f-241">İşaretçi ifadeleri</span><span class="sxs-lookup"><span data-stu-id="53d9f-241">Pointers in expressions</span></span>

<span data-ttu-id="53d9f-242">Güvensiz bir bağlamı bir ifade bir işaretçi türünün bir sonucunun hızlanmasını sağlayabilir, ancak isteğe bağlı olarak güvensiz bir bağlamı dışında bir derleme zamanı hatası için bir işaretçi türü olarak ifade bulunmamaktadır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-242">In an unsafe context, an expression may yield a result of a pointer type, but outside an unsafe context it is a compile-time error for an expression to be of a pointer type.</span></span> <span data-ttu-id="53d9f-243">Varsa güvensiz bir bağlamı dışında kesin bağlamında, bir derleme zamanı hatası oluşur *simple_name* ([basit adları](expressions.md#simple-names)), *member_access* ([üye erişimi ](expressions.md#member-access)), *invocation_expression* ([çağrı ifadeleri](expressions.md#invocation-expressions)), veya *element_access* ([öğe erişimi](expressions.md#element-access)) bir işaretçi türü.</span><span class="sxs-lookup"><span data-stu-id="53d9f-243">In precise terms, outside an unsafe context a compile-time error occurs if any *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)), or *element_access* ([Element access](expressions.md#element-access)) is of a pointer type.</span></span>

<span data-ttu-id="53d9f-244">Güvenli olmayan bir bağlamda *primary_no_array_creation_expression* ([birincil ifadeler](expressions.md#primary-expressions)) ve *unary_expression* ([birliişleçler](expressions.md#unary-operators)) Üretim aşağıdaki ek yapıları izin ver:</span><span class="sxs-lookup"><span data-stu-id="53d9f-244">In an unsafe context, the *primary_no_array_creation_expression* ([Primary expressions](expressions.md#primary-expressions)) and *unary_expression* ([Unary operators](expressions.md#unary-operators)) productions permit the following additional constructs:</span></span>

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

<span data-ttu-id="53d9f-245">Bu yapılar, aşağıdaki bölümlerde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-245">These constructs are described in the following sections.</span></span> <span data-ttu-id="53d9f-246">Öncelik ve ilişkisellik güvenli olmayan işleçlerinin dilbilgisi tarafından kapsanan.</span><span class="sxs-lookup"><span data-stu-id="53d9f-246">The precedence and associativity of the unsafe operators is implied by the grammar.</span></span>

### <a name="pointer-indirection"></a><span data-ttu-id="53d9f-247">İşaretçi yöneltmesi</span><span class="sxs-lookup"><span data-stu-id="53d9f-247">Pointer indirection</span></span>

<span data-ttu-id="53d9f-248">A *pointer_indirection_expression* oluşur yıldız işareti (`*`) tarafından izlenen bir *unary_expression*.</span><span class="sxs-lookup"><span data-stu-id="53d9f-248">A *pointer_indirection_expression* consists of an asterisk (`*`) followed by a *unary_expression*.</span></span>

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

<span data-ttu-id="53d9f-249">Birli `*` işleci işaretçi yöneltmesi gösterir ve bir işaretçinin işaret ettiği değişken almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-249">The unary `*` operator denotes pointer indirection and is used to obtain the variable to which a pointer points.</span></span> <span data-ttu-id="53d9f-250">Değerlendirme sonucu `*P`burada `P` bir işaretçi türü ifade `T*`, türünde bir değişken `T`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-250">The result of evaluating `*P`, where `P` is an expression of a pointer type `T*`, is a variable of type `T`.</span></span> <span data-ttu-id="53d9f-251">Birli uygulamak için bir derleme zamanı hata `*` türündeki bir ifade işlecine `void*` veya işaretçi türünde olmayan bir ifade.</span><span class="sxs-lookup"><span data-stu-id="53d9f-251">It is a compile-time error to apply the unary `*` operator to an expression of type `void*` or to an expression that isn't of a pointer type.</span></span>

<span data-ttu-id="53d9f-252">Birli etkisi `*` işleci bir `null` işaretçisidir uygulama tanımlı.</span><span class="sxs-lookup"><span data-stu-id="53d9f-252">The effect of applying the unary `*` operator to a `null` pointer is implementation-defined.</span></span> <span data-ttu-id="53d9f-253">Özellikle, bu işlemi oluşturan bir garanti yoktur bir `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-253">In particular, there is no guarantee that this operation throws a `System.NullReferenceException`.</span></span>

<span data-ttu-id="53d9f-254">Geçersiz bir değer işaretçisi, birli davranışını atandıysa `*` işleci tanımsızdır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-254">If an invalid value has been assigned to the pointer, the behavior of the unary `*` operator is undefined.</span></span> <span data-ttu-id="53d9f-255">Birli tarafından bir işaretçiye başvuruluyor için geçersiz değerler arasında `*` işleci işaret türü için uygunsuz bir şekilde hizalanmış bir adresi olan (örnekte bakın [işaretçi dönüşümleri](unsafe-code.md#pointer-conversions)) ve sonra bir değişkenin adresi geçerlilik süresi.</span><span class="sxs-lookup"><span data-stu-id="53d9f-255">Among the invalid values for dereferencing a pointer by the unary `*` operator are an address inappropriately aligned for the type pointed to (see example in [Pointer conversions](unsafe-code.md#pointer-conversions)), and the address of a variable after the end of its lifetime.</span></span>

<span data-ttu-id="53d9f-256">Belirli atama onayına çözümleme amacıyla bir değişken üretilen biçiminde bir ifade değerlendirmesi tarafından `*P` başlangıçta atanan olarak kabul edilir ([değişkenleri başlangıçta atanan](variables.md#initially-assigned-variables)).</span><span class="sxs-lookup"><span data-stu-id="53d9f-256">For purposes of definite assignment analysis, a variable produced by evaluating an expression of the form `*P` is considered initially assigned ([Initially assigned variables](variables.md#initially-assigned-variables)).</span></span>

### <a name="pointer-member-access"></a><span data-ttu-id="53d9f-257">İşaretçi üye erişimi</span><span class="sxs-lookup"><span data-stu-id="53d9f-257">Pointer member access</span></span>

<span data-ttu-id="53d9f-258">A *pointer_member_access* oluşan bir *primary_expression*ve ardından bir "`->`" belirteci, izleyen bir *tanımlayıcı* ve isteğe bağlı bir *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="53d9f-258">A *pointer_member_access* consists of a *primary_expression*, followed by a "`->`" token, followed by an *identifier* and an optional *type_argument_list*.</span></span>

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

<span data-ttu-id="53d9f-259">Bir işaretçi üye erişimi formun içinde `P->I`, `P` bir işaretçi türü ifade dışında olmalıdır `void*`, ve `I` erişilebilir bir türün üyesi belirtmek gerekir `P` noktaları.</span><span class="sxs-lookup"><span data-stu-id="53d9f-259">In a pointer member access of the form `P->I`, `P` must be an expression of a pointer type other than `void*`, and `I` must denote an accessible member of the type to which `P` points.</span></span>

<span data-ttu-id="53d9f-260">Bir işaretçi üye erişimi formun `P->I` tam olarak değerlendirilir `(*P).I`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-260">A pointer member access of the form `P->I` is evaluated exactly as `(*P).I`.</span></span> <span data-ttu-id="53d9f-261">İşaretçi yöneltme işleci bir açıklaması (`*`), bkz: [işaretçi yöneltmesi](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="53d9f-261">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="53d9f-262">Üye erişimi işleci bir açıklaması (`.`), bkz: [üye erişimi](expressions.md#member-access).</span><span class="sxs-lookup"><span data-stu-id="53d9f-262">For a description of the member access operator (`.`), see [Member access](expressions.md#member-access).</span></span>

<span data-ttu-id="53d9f-263">Örnekte</span><span class="sxs-lookup"><span data-stu-id="53d9f-263">In the example</span></span>

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

<span data-ttu-id="53d9f-264">`->` işleci alanlarına erişmek ve işaretçi üzerinden bir yapının bir yöntem çağırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-264">the `->` operator is used to access fields and invoke a method of a struct through a pointer.</span></span> <span data-ttu-id="53d9f-265">Çünkü işlemi `P->I` tam olarak değerine eşdeğer olan `(*P).I`, `Main` yöntemi eşit derecede iyi yazılı:</span><span class="sxs-lookup"><span data-stu-id="53d9f-265">Because the operation `P->I` is precisely equivalent to `(*P).I`, the `Main` method could equally well have been written:</span></span>

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

### <a name="pointer-element-access"></a><span data-ttu-id="53d9f-266">İşaretçi öğe erişimi</span><span class="sxs-lookup"><span data-stu-id="53d9f-266">Pointer element access</span></span>

<span data-ttu-id="53d9f-267">A *pointer_element_access* oluşan bir *primary_no_array_creation_expression* içine bir ifadenin ardından "`[`"ve"`]`".</span><span class="sxs-lookup"><span data-stu-id="53d9f-267">A *pointer_element_access* consists of a *primary_no_array_creation_expression* followed by an expression enclosed in "`[`" and "`]`".</span></span>

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

<span data-ttu-id="53d9f-268">Bir işaretçi öğe erişimi formun içinde `P[E]`, `P` bir işaretçi türü ifade dışında olmalıdır `void*`, ve `E` örtük olarak dönüştürülebilir bir ifade olmalıdır `int`, `uint`, `long`, veya `ulong`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-268">In a pointer element access of the form `P[E]`, `P` must be an expression of a pointer type other than `void*`, and `E` must be an expression that can be implicitly converted to `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="53d9f-269">Bir işaretçi öğe erişimi formun `P[E]` tam olarak değerlendirilir `*(P + E)`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-269">A pointer element access of the form `P[E]` is evaluated exactly as `*(P + E)`.</span></span> <span data-ttu-id="53d9f-270">İşaretçi yöneltme işleci bir açıklaması (`*`), bkz: [işaretçi yöneltmesi](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="53d9f-270">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="53d9f-271">İşaretçi Toplama işleci bir açıklaması (`+`), bkz: [işaretçi aritmetiği](unsafe-code.md#pointer-arithmetic).</span><span class="sxs-lookup"><span data-stu-id="53d9f-271">For a description of the pointer addition operator (`+`), see [Pointer arithmetic](unsafe-code.md#pointer-arithmetic).</span></span>

<span data-ttu-id="53d9f-272">Örnekte</span><span class="sxs-lookup"><span data-stu-id="53d9f-272">In the example</span></span>

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

<span data-ttu-id="53d9f-273">bir işaretçi öğe erişimi karakter arabelleğine başlatmak için kullanılan bir `for` döngü.</span><span class="sxs-lookup"><span data-stu-id="53d9f-273">a pointer element access is used to initialize the character buffer in a `for` loop.</span></span> <span data-ttu-id="53d9f-274">Çünkü işlemi `P[E]` tam olarak değerine eşdeğer olan `*(P + E)`, örnek eşit derecede iyi yazılmış:</span><span class="sxs-lookup"><span data-stu-id="53d9f-274">Because the operation `P[E]` is precisely equivalent to `*(P + E)`, the example could equally well have been written:</span></span>

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

<span data-ttu-id="53d9f-275">İşaretçi öğesi erişim işleci işlemleri denetlemez hataları ve erişirken davranışı bir öğe işlemleri tanımsız olur.</span><span class="sxs-lookup"><span data-stu-id="53d9f-275">The pointer element access operator does not check for out-of-bounds errors and the behavior when accessing an out-of-bounds element is undefined.</span></span> <span data-ttu-id="53d9f-276">C ve C++ ile aynı olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-276">This is the same as C and C++.</span></span>

### <a name="the-address-of-operator"></a><span data-ttu-id="53d9f-277">Address-of işleci</span><span class="sxs-lookup"><span data-stu-id="53d9f-277">The address-of operator</span></span>

<span data-ttu-id="53d9f-278">Bir *addressof_expression* oluşur ve işareti (`&`) tarafından izlenen bir *unary_expression*.</span><span class="sxs-lookup"><span data-stu-id="53d9f-278">An *addressof_expression* consists of an ampersand (`&`) followed by a *unary_expression*.</span></span>

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

<span data-ttu-id="53d9f-279">Bir ifade verilen `E` türü olduğu `T` ve sabit bir değişken olarak sınıflandırıldığını ([sabit ve taşınabilir değişkenleri](unsafe-code.md#fixed-and-moveable-variables)), yapı `&E` tarafındanverilendeğişkeninadresihesaplar`E`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-279">Given an expression `E` which is of a type `T` and is classified as a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), the construct `&E` computes the address of the variable given by `E`.</span></span> <span data-ttu-id="53d9f-280">Sonuç türü olan `T*` ve bir değer olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-280">The type of the result is `T*` and is classified as a value.</span></span> <span data-ttu-id="53d9f-281">Bir derleme zamanı hatası oluşur `E` durumunda bir değişken olarak sınıflandırılır değil `E` salt okunur yerel bir değişken, Sınıflandırılmamış veya `E` taşınabilir bir değişkeni gösterir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-281">A compile-time error occurs if `E` is not classified as a variable, if `E` is classified as a read-only local variable, or if `E` denotes a moveable variable.</span></span> <span data-ttu-id="53d9f-282">Son durumda, bir fixed deyimi ([fixed deyimi](unsafe-code.md#the-fixed-statement)) geçici olarak "değişkenin adresini edinme önce düzeltmek için" kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-282">In the last case, a fixed statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) can be used to temporarily "fix" the variable before obtaining its address.</span></span> <span data-ttu-id="53d9f-283">Bölümünde belirtildiği [üye erişimi](expressions.md#member-access), bir örnek oluşturucusu veya bir yapı ya da tanımlayan sınıf için statik Oluşturucu dışında bir `readonly` alan, bu alan bir değer, bir değişken değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-283">As stated in [Member access](expressions.md#member-access), outside an instance constructor or static constructor for a struct or class that defines a `readonly` field, that field is considered a value, not a variable.</span></span> <span data-ttu-id="53d9f-284">Bu nedenle, adresi alınamaz.</span><span class="sxs-lookup"><span data-stu-id="53d9f-284">As such, its address cannot be taken.</span></span> <span data-ttu-id="53d9f-285">Benzer şekilde, bir sabit adresi alınamaz.</span><span class="sxs-lookup"><span data-stu-id="53d9f-285">Similarly, the address of a constant cannot be taken.</span></span>

<span data-ttu-id="53d9f-286">`&` İşleci kesinlikle atanmış, ancak aşağıdaki bağımsız değişkeni gerekli olmadığı bir `&` işlemi, değişkenin işlecin uygulandığı değerlendirilir işleminin gerçekleştiği yürütme yolu kesinlikle atanmış.</span><span class="sxs-lookup"><span data-stu-id="53d9f-286">The `&` operator does not require its argument to be definitely assigned, but following an `&` operation, the variable to which the operator is applied is considered definitely assigned in the execution path in which the operation occurs.</span></span> <span data-ttu-id="53d9f-287">Bu sorumluluk değişkeninin doğru başlatmanın emin olmak için programcının kararına aslında bu durumda gerçekleşmesi olur.</span><span class="sxs-lookup"><span data-stu-id="53d9f-287">It is the responsibility of the programmer to ensure that correct initialization of the variable actually does take place in this situation.</span></span>

<span data-ttu-id="53d9f-288">Örnekte</span><span class="sxs-lookup"><span data-stu-id="53d9f-288">In the example</span></span>

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

<span data-ttu-id="53d9f-289">`i` Aşağıdaki kesinlikle atanmış olarak kabul edilir `&i` başlatmak için kullanılan işlem `p`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-289">`i` is considered definitely assigned following the `&i` operation used to initialize `p`.</span></span> <span data-ttu-id="53d9f-290">Atamayı `*p` yürürlükte başlatır `i`, ancak eklenmesi bu başlatma, programcının sorumluluğundadır ve atama kaldırıldıysa herhangi bir derleme zamanı hata oluşacak.</span><span class="sxs-lookup"><span data-stu-id="53d9f-290">The assignment to `*p` in effect initializes `i`, but the inclusion of this initialization is the responsibility of the programmer, and no compile-time error would occur if the assignment was removed.</span></span>

<span data-ttu-id="53d9f-291">İçin belirli atama onayına kurallarına `&` işleci mevcut sağlayacak şekilde yerel değişkenlerin yedekli başlatma önlenebilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-291">The rules of definite assignment for the `&` operator exist such that redundant initialization of local variables can be avoided.</span></span> <span data-ttu-id="53d9f-292">Örneğin, çok sayıda dış API API tarafından doldurulan bir yapıya bir işaretçi alır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-292">For example, many external APIs take a pointer to a structure which is filled in by the API.</span></span> <span data-ttu-id="53d9f-293">Bu API'lere giden çağrıların yerel yapı değişkenin adresini pass ve kural yapı değişkenin yedekli başlatma gerekli olacaktır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-293">Calls to such APIs typically pass the address of a local struct variable, and without the rule, redundant initialization of the struct variable would be required.</span></span>

### <a name="pointer-increment-and-decrement"></a><span data-ttu-id="53d9f-294">İşaretçi artırma ve azaltma</span><span class="sxs-lookup"><span data-stu-id="53d9f-294">Pointer increment and decrement</span></span>

<span data-ttu-id="53d9f-295">Güvenli olmayan bir bağlamda `++` ve `--` işleçleri ([sonek arttırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators) ve [önek arttırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)) işaretçiye uygulanabilir değişkenleri tüm türlerin `void*`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-295">In an unsafe context, the `++` and `--` operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)) can be applied to pointer variables of all types except `void*`.</span></span> <span data-ttu-id="53d9f-296">Bu nedenle, her bir işaretçi türü için `T*`, aşağıdaki işleçleri örtük olarak tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="53d9f-296">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

<span data-ttu-id="53d9f-297">İşleçler olarak aynı sonuçlar `x + 1` ve `x - 1`sırasıyla ([işaretçi aritmetiği](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="53d9f-297">The operators produce the same results as `x + 1` and `x - 1`, respectively ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span> <span data-ttu-id="53d9f-298">Diğer bir deyişle, bir işaretçi değişkeninin türü için `T*`, `++` işleci ekler `sizeof(T)` değişkeninde bulunan adresine ve `--` işleci çıkarır `sizeof(T)` değişkeninde bulunan adresinden.</span><span class="sxs-lookup"><span data-stu-id="53d9f-298">In other words, for a pointer variable of type `T*`, the `++` operator adds `sizeof(T)` to the address contained in the variable, and the `--` operator subtracts `sizeof(T)` from the address contained in the variable.</span></span>

<span data-ttu-id="53d9f-299">Bir işaretçi artırma veya azaltma işlemi taşmaları işaretçi türünün etki alanını, sonucu uygulama tanımlanır, ancak hiçbir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="53d9f-299">If a pointer increment or decrement operation overflows the domain of the pointer type, the result is implementation-defined, but no exceptions are produced.</span></span>

### <a name="pointer-arithmetic"></a><span data-ttu-id="53d9f-300">İşaretçi aritmetiği</span><span class="sxs-lookup"><span data-stu-id="53d9f-300">Pointer arithmetic</span></span>

<span data-ttu-id="53d9f-301">Güvenli olmayan bir bağlamda `+` ve `-` işleçleri ([Toplama işleci](expressions.md#addition-operator) ve [çıkarma işleci](expressions.md#subtraction-operator)) tüm İşaretçi türlerinin değerlerini uygulanabilir `void*`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-301">In an unsafe context, the `+` and `-` operators ([Addition operator](expressions.md#addition-operator) and [Subtraction operator](expressions.md#subtraction-operator)) can be applied to values of all pointer types except `void*`.</span></span> <span data-ttu-id="53d9f-302">Bu nedenle, her bir işaretçi türü için `T*`, aşağıdaki işleçleri örtük olarak tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="53d9f-302">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

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

<span data-ttu-id="53d9f-303">Bir ifade verilen `P` bir işaretçi türünün `T*` ve ifade `N` türü `int`, `uint`, `long`, veya `ulong`, ifadeleri `P + N` ve `N + P` işlem İşaretçi türünün değerini `T*` sonuçlarının eklemesini `N * sizeof(T)` tarafından verilen adresine `P`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-303">Given an expression `P` of a pointer type `T*` and an expression `N` of type `int`, `uint`, `long`, or `ulong`, the expressions `P + N` and `N + P` compute the pointer value of type `T*` that results from adding `N * sizeof(T)` to the address given by `P`.</span></span> <span data-ttu-id="53d9f-304">Benzer şekilde, ifade `P - N` türü işaretçi değeri hesaplar `T*` sonuçlarının arasındaki çıkarma işleminin `N * sizeof(T)` tarafından verilen adresinden `P`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-304">Likewise, the expression `P - N` computes the pointer value of type `T*` that results from subtracting `N * sizeof(T)` from the address given by `P`.</span></span>

<span data-ttu-id="53d9f-305">Verilen iki deyim `P` ve `Q`, bir işaretçi türünün `T*`, ifade `P - Q` tarafından verilen adresleri arasındaki farkı hesaplar `P` ve `Q` ve ardından bu fark tarafından böler`sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-305">Given two expressions, `P` and `Q`, of a pointer type `T*`, the expression `P - Q` computes the difference between the addresses given by `P` and `Q` and then divides that difference by `sizeof(T)`.</span></span> <span data-ttu-id="53d9f-306">Her zaman bir sonuç türü olan `long`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-306">The type of the result is always `long`.</span></span> <span data-ttu-id="53d9f-307">Aslında, `P - Q` olarak hesaplanan `((long)(P) - (long)(Q)) / sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-307">In effect, `P - Q` is computed as `((long)(P) - (long)(Q)) / sizeof(T)`.</span></span>

<span data-ttu-id="53d9f-308">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="53d9f-308">For example:</span></span>

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

<span data-ttu-id="53d9f-309">Bu çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="53d9f-309">which produces the output:</span></span>

```
p - q = -14
q - p = 14
```

<span data-ttu-id="53d9f-310">Etki alanı işaretçi türü bir işaretçi aritmetik işlemi taşıyor, sonucu bir uygulama tarafından tanımlanan biçimde kesilmiş, ancak hiçbir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="53d9f-310">If a pointer arithmetic operation overflows the domain of the pointer type, the result is truncated in an implementation-defined fashion, but no exceptions are produced.</span></span>

### <a name="pointer-comparison"></a><span data-ttu-id="53d9f-311">İşaretçi karşılaştırması</span><span class="sxs-lookup"><span data-stu-id="53d9f-311">Pointer comparison</span></span>

<span data-ttu-id="53d9f-312">Güvenli olmayan bir bağlamda `==`, `!=`, `<`, `>`, `<=`, ve `=>` işleçleri ([ilişkisel ve tür testi işleçleri](expressions.md#relational-and-type-testing-operators)) tüm değerlere uygulanan İşaretçi türleri.</span><span class="sxs-lookup"><span data-stu-id="53d9f-312">In an unsafe context, the `==`, `!=`, `<`, `>`, `<=`, and `=>` operators ([Relational and type-testing operators](expressions.md#relational-and-type-testing-operators)) can be applied to values of all pointer types.</span></span> <span data-ttu-id="53d9f-313">İşaretçi Karşılaştırma işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="53d9f-313">The pointer comparison operators are:</span></span>

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

<span data-ttu-id="53d9f-314">Örtük bir dönüştürme için herhangi bir işaretçi türü bulunduğundan `void*` Bu işleçleri kullanarak herhangi bir işaretçi türü işlenen türü karşılaştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-314">Because an implicit conversion exists from any pointer type to the `void*` type, operands of any pointer type can be compared using these operators.</span></span> <span data-ttu-id="53d9f-315">Karşılaştırma işleçleri, işaretsiz tamsayılar değilmiş gibi iki işlenen tarafından belirtilen adresi karşılaştırın.</span><span class="sxs-lookup"><span data-stu-id="53d9f-315">The comparison operators compare the addresses given by the two operands as if they were unsigned integers.</span></span>

### <a name="the-sizeof-operator"></a><span data-ttu-id="53d9f-316">Sizeof işleci</span><span class="sxs-lookup"><span data-stu-id="53d9f-316">The sizeof operator</span></span>

<span data-ttu-id="53d9f-317">`sizeof` İşleci tarafından verilen bir türde bir değişken kapladığı bayt sayısını döndürür.</span><span class="sxs-lookup"><span data-stu-id="53d9f-317">The `sizeof` operator returns the number of bytes occupied by a variable of a given type.</span></span> <span data-ttu-id="53d9f-318">İşleneni olarak belirtilen tür `sizeof` olmalıdır bir *unmanaged_type* ([işaretçi türleri](unsafe-code.md#pointer-types)).</span><span class="sxs-lookup"><span data-stu-id="53d9f-318">The type specified as an operand to `sizeof` must be an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

<span data-ttu-id="53d9f-319">Sonucu `sizeof` işleci türünde bir değer, `int`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-319">The result of the `sizeof` operator is a value of type `int`.</span></span> <span data-ttu-id="53d9f-320">Belirli türlerini önceden tanımlanmış `sizeof` aşağıdaki tabloda gösterildiği gibi bir sabit değer işlecini verir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-320">For certain predefined types, the `sizeof` operator yields a constant value as shown in the table below.</span></span>


| <span data-ttu-id="53d9f-321">__İfade__</span><span class="sxs-lookup"><span data-stu-id="53d9f-321">__Expression__</span></span>   | <span data-ttu-id="53d9f-322">__Sonuç__</span><span class="sxs-lookup"><span data-stu-id="53d9f-322">__Result__</span></span> |
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

<span data-ttu-id="53d9f-323">Diğer tüm türleri için sonucunu `sizeof` işleci uygulama tarafından tanımlanır ve sabit bir değer sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-323">For all other types, the result of the `sizeof` operator is implementation-defined and is classified as a value, not a constant.</span></span>

<span data-ttu-id="53d9f-324">Üyeleri bir yapı içinde paketlenir sırasını belirtilmemiş.</span><span class="sxs-lookup"><span data-stu-id="53d9f-324">The order in which members are packed into a struct is unspecified.</span></span>

<span data-ttu-id="53d9f-325">Hizalama amacıyla olabilir adlandırılmamış bir yapı içinde bir yapının başında ve sonunda, struct doldurma.</span><span class="sxs-lookup"><span data-stu-id="53d9f-325">For alignment purposes, there may be unnamed padding at the beginning of a struct, within a struct, and at the end of the struct.</span></span> <span data-ttu-id="53d9f-326">Doldurma olarak kullanılan bit içeriğini belirsiz.</span><span class="sxs-lookup"><span data-stu-id="53d9f-326">The contents of the bits used as padding are indeterminate.</span></span>

<span data-ttu-id="53d9f-327">Yapı türüne sahip bir işleç uygulandığında, sonucu herhangi doldurma dahil olmak üzere, bu türde bir değişken bayt toplam sayısıdır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-327">When applied to an operand that has struct type, the result is the total number of bytes in a variable of that type, including any padding.</span></span>

## <a name="the-fixed-statement"></a><span data-ttu-id="53d9f-328">Fixed deyimi</span><span class="sxs-lookup"><span data-stu-id="53d9f-328">The fixed statement</span></span>

<span data-ttu-id="53d9f-329">Güvenli olmayan bir bağlamda *embedded_statement* ([deyimleri](statements.md)) üretim izin veren bir ek yapı `fixed` "taşınabilir bir değişken düzeltmek için" kullanılan ifadesi gibi Adres deyim süresi boyunca sabit kalır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-329">In an unsafe context, the *embedded_statement* ([Statements](statements.md)) production permits an additional construct, the `fixed` statement, which is used to "fix" a moveable variable such that its address remains constant for the duration of the statement.</span></span>

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

<span data-ttu-id="53d9f-330">Her *fixed_pointer_declarator* yerel bir değişken bildirir verilen *pointer_type* ve karşılık gelen tarafından hesaplanan adresiyle bu yerel değişken başlatır *fixed_ pointer_initializer*.</span><span class="sxs-lookup"><span data-stu-id="53d9f-330">Each *fixed_pointer_declarator* declares a local variable of the given *pointer_type* and initializes that local variable with the address computed by the corresponding *fixed_pointer_initializer*.</span></span> <span data-ttu-id="53d9f-331">Bildirilen yerel değişken bir `fixed` deyimi, tüm erişilebilir *fixed_pointer_initializer*sağındaki değişkenin bildirimi ve içinde gerçekleşen s *embedded_statement* , `fixed` deyimi.</span><span class="sxs-lookup"><span data-stu-id="53d9f-331">A local variable declared in a `fixed` statement is accessible in any *fixed_pointer_initializer*s occurring to the right of that variable's declaration, and in the *embedded_statement* of the `fixed` statement.</span></span> <span data-ttu-id="53d9f-332">Tarafından bildirilen yerel değişken bir `fixed` deyimi salt okunur kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-332">A local variable declared by a `fixed` statement is considered read-only.</span></span> <span data-ttu-id="53d9f-333">Gömülü deyim bu yerel değişken değiştirme girişiminde bulunursa, bir derleme zamanı hatası oluşur (atama aracılığıyla veya `++` ve `--` işleçleri) veya olarak başarılı bir `ref` veya `out` parametresi.</span><span class="sxs-lookup"><span data-stu-id="53d9f-333">A compile-time error occurs if the embedded statement attempts to modify this local variable (via assignment or the `++` and `--` operators) or pass it as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="53d9f-334">A *fixed_pointer_initializer* aşağıdakilerden biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="53d9f-334">A *fixed_pointer_initializer* can be one of the following:</span></span>

*  <span data-ttu-id="53d9f-335">Belirteç "`&`" arkasından bir *variable_reference* ([belirli atama onayına belirlemek için kesin kurallar](variables.md#precise-rules-for-determining-definite-assignment)) taşınabilir bir değişkene ([sabit ve taşınabilir değişkenleri](unsafe-code.md#fixed-and-moveable-variables)) yönetilmeyen bir türü `T`, türü sağlanan `T*` verilen işaretçi türüne açıkça dönüştürülemez `fixed` deyimi.</span><span class="sxs-lookup"><span data-stu-id="53d9f-335">The token "`&`" followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) to a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="53d9f-336">Bu durumda, belirtilen değişkenin adresi Başlatıcı hesaplar ve değişkeni sabit bir adreste süresi boyunca kalan garanti `fixed` deyimi.</span><span class="sxs-lookup"><span data-stu-id="53d9f-336">In this case, the initializer computes the address of the given variable, and the variable is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>
*  <span data-ttu-id="53d9f-337">Bir ifade bir *array_type* nespravovaným typem öğelerini `T`, türü sağlanan `T*` verilen işaretçi türüne açıkça dönüştürülemez `fixed` deyimi.</span><span class="sxs-lookup"><span data-stu-id="53d9f-337">An expression of an *array_type* with elements of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="53d9f-338">Bu durumda, dizideki ilk öğe adresini Başlatıcı hesaplar ve tüm dizi sabit bir adreste süresi boyunca kalan garanti `fixed` deyimi.</span><span class="sxs-lookup"><span data-stu-id="53d9f-338">In this case, the initializer computes the address of the first element in the array, and the entire array is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="53d9f-339">Dizi ifadesi null ise veya dizi hiç öğe varsa, başlatıcı adresi eşit sıfıra hesaplar.</span><span class="sxs-lookup"><span data-stu-id="53d9f-339">If the array expression is null or if the array has zero elements, the initializer computes an address equal to zero.</span></span>
*  <span data-ttu-id="53d9f-340">Türündeki bir ifade `string`, türü sağlanan `char*` verilen işaretçi türüne açıkça dönüştürülemez `fixed` deyimi.</span><span class="sxs-lookup"><span data-stu-id="53d9f-340">An expression of type `string`, provided the type `char*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="53d9f-341">Bu durumda, ilk karakterin dizede adresi Başlatıcı hesaplar ve tüm dize sabit bir adreste süresi boyunca kalan garanti `fixed` deyimi.</span><span class="sxs-lookup"><span data-stu-id="53d9f-341">In this case, the initializer computes the address of the first character in the string, and the entire string is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="53d9f-342">Davranışını `fixed` deyimi uygulama tanımlı dize ifadesi null ise.</span><span class="sxs-lookup"><span data-stu-id="53d9f-342">The behavior of the `fixed` statement is implementation-defined if the string expression is null.</span></span>
*  <span data-ttu-id="53d9f-343">A *simple_name* veya *member_access* sabit boyutlu arabellek üyenin türü verilen işaretçi türüne örtük olarak dönüştürülebilir sağlanan taşınabilir bir değişken bir sabit boyutlu arabellek üyesi başvuran içinde `fixed` deyimi.</span><span class="sxs-lookup"><span data-stu-id="53d9f-343">A *simple_name* or *member_access* that references a fixed size buffer member of a moveable variable, provided the type of the fixed size buffer member is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="53d9f-344">Bu durumda, başlatıcı bir sabit boyutlu arabellek ilk öğesinin işaretçisi hesaplar ([boyutlu arabellekler ifadelerde sabit](unsafe-code.md#fixed-size-buffers-in-expressions)), ve sabit boyutlu arabellek süresiboyuncasabitbiradrestekalmasınagaranti`fixed`deyimi.</span><span class="sxs-lookup"><span data-stu-id="53d9f-344">In this case, the initializer computes a pointer to the first element of the fixed size buffer ([Fixed size buffers in expressions](unsafe-code.md#fixed-size-buffers-in-expressions)), and the fixed size buffer is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>

<span data-ttu-id="53d9f-345">Tarafından hesaplanan her adresi için bir *fixed_pointer_initializer* `fixed` deyimi adres tarafından başvurulan değişkeni yeniden konumlandırma veya çöp toplayıcı tarafından elden çıkarma süresince tabi değildir olmasını sağlar `fixed` deyimi.</span><span class="sxs-lookup"><span data-stu-id="53d9f-345">For each address computed by a *fixed_pointer_initializer* the `fixed` statement ensures that the variable referenced by the address is not subject to relocation or disposal by the garbage collector for the duration of the `fixed` statement.</span></span> <span data-ttu-id="53d9f-346">Örneğin, adresi tarafından hesaplanan bir *fixed_pointer_initializer* bir nesnenin bir alan veya bir dizi örnek, bir öğe başvuru `fixed` deyimi içeren bir nesne örneği değil konumlandırıldı güvence altına alır veya deyim ömrü boyunca elden.</span><span class="sxs-lookup"><span data-stu-id="53d9f-346">For example, if the address computed by a *fixed_pointer_initializer* references a field of an object or an element of an array instance, the `fixed` statement guarantees that the containing object instance is not relocated or disposed of during the lifetime of the statement.</span></span>

<span data-ttu-id="53d9f-347">Bu işaretçi tarafından oluşturulan emin olmak için programcının sorumluluğundadır `fixed` deyimleri Bu deyimler yürütme hayatta değil.</span><span class="sxs-lookup"><span data-stu-id="53d9f-347">It is the programmer's responsibility to ensure that pointers created by `fixed` statements do not survive beyond execution of those statements.</span></span> <span data-ttu-id="53d9f-348">Örneğin, ne zaman işaretçiler tarafından oluşturulan `fixed` deyimleri, dış API'lerine geçirilir, bu API'leri bu işaretçiler bellek korunmasını sağlamak için programcının sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-348">For example, when pointers created by `fixed` statements are passed to external APIs, it is the programmer's responsibility to ensure that the APIs retain no memory of these pointers.</span></span>

<span data-ttu-id="53d9f-349">(Bunlar taşınamıyor çünkü) sabit nesneler yığın parçalanmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-349">Fixed objects may cause fragmentation of the heap (because they can't be moved).</span></span> <span data-ttu-id="53d9f-350">Bu nedenle, nesneler yalnızca gerçekten gerekli olduğunda düzeltilmelidir ve ardından yalnızca zaman mümkün olan en kısa miktarı için.</span><span class="sxs-lookup"><span data-stu-id="53d9f-350">For that reason, objects should be fixed only when absolutely necessary and then only for the shortest amount of time possible.</span></span>

<span data-ttu-id="53d9f-351">Örnek</span><span class="sxs-lookup"><span data-stu-id="53d9f-351">The example</span></span>

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

<span data-ttu-id="53d9f-352">bazı kullanımlarını gösterir `fixed` deyimi.</span><span class="sxs-lookup"><span data-stu-id="53d9f-352">demonstrates several uses of the `fixed` statement.</span></span> <span data-ttu-id="53d9f-353">İlk ifade düzeltir ve adresini statik alanı alır, ikinci ifade düzeltir ve örnek alan adresini alır ve üçüncü deyim düzeltir ve bir dizi öğesinin adresini alır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-353">The first statement fixes and obtains the address of a static field, the second statement fixes and obtains the address of an instance field, and the third statement fixes and obtains the address of an array element.</span></span> <span data-ttu-id="53d9f-354">Her durumda, normal kullanmak için bir hata bulunması gereken `&` değişkenleri tüm taşınabilir değişkenleri olarak sınıflandırılan beri işleci.</span><span class="sxs-lookup"><span data-stu-id="53d9f-354">In each case it would have been an error to use the regular `&` operator since the variables are all classified as moveable variables.</span></span>

<span data-ttu-id="53d9f-355">Dördüncü `fixed` deyimi yukarıdaki örnekte, üçüncü benzer bir sonuç üretir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-355">The fourth `fixed` statement in the example above produces a similar result to the third.</span></span>

<span data-ttu-id="53d9f-356">Bu örneği `fixed` deyimi kullandığı `string`:</span><span class="sxs-lookup"><span data-stu-id="53d9f-356">This example of the `fixed` statement uses `string`:</span></span>

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

<span data-ttu-id="53d9f-357">Güvenli olmayan bir bağlamda tek boyutlu diziler, dizi öğeleri ile dizininden başlayarak, artan dizin sırasıyla depolanır `0` ve dizin ile biten `Length - 1`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-357">In an unsafe context array elements of single-dimensional arrays are stored in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="53d9f-358">Çok boyutlu diziler, dizi öğeleri dizinleri en sağdaki boyutun ilk olarak, artan şekilde depolanan sonra sonraki boyut vb. sola kaldı.</span><span class="sxs-lookup"><span data-stu-id="53d9f-358">For multi-dimensional arrays, array elements are stored such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span> <span data-ttu-id="53d9f-359">İçinde bir `fixed` bir işaretçi ifadesi `p` array örneğine `a`, işaretçi değerleri arasında değişen `p` için `p + a.Length - 1` dizideki öğelerin adresleri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="53d9f-359">Within a `fixed` statement that obtains a pointer `p` to an array instance `a`, the pointer values ranging from `p` to `p + a.Length - 1` represent addresses of the elements in the array.</span></span> <span data-ttu-id="53d9f-360">Benzer şekilde, arasında değişen değişkenleri `p[0]` için `p[a.Length - 1]` gerçek dizi öğeleri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="53d9f-360">Likewise, the variables ranging from `p[0]` to `p[a.Length - 1]` represent the actual array elements.</span></span> <span data-ttu-id="53d9f-361">İşlevmiş gibi doğrusal diziler depolandığı yolu verildiğinde, biz herhangi bir boyut dizisi davranabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="53d9f-361">Given the way in which arrays are stored, we can treat an array of any dimension as though it were linear.</span></span>

<span data-ttu-id="53d9f-362">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="53d9f-362">For example:</span></span>

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

<span data-ttu-id="53d9f-363">Bu çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="53d9f-363">which produces the output:</span></span>

```
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

<span data-ttu-id="53d9f-364">Örnekte</span><span class="sxs-lookup"><span data-stu-id="53d9f-364">In the example</span></span>

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

<span data-ttu-id="53d9f-365">bir `fixed` deyimi adresini bir işaretçi alır bir yönteme geçirilmesi için bir dizi düzeltmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-365">a `fixed` statement is used to fix an array so its address can be passed to a method that takes a pointer.</span></span>

<span data-ttu-id="53d9f-366">Örnekte:</span><span class="sxs-lookup"><span data-stu-id="53d9f-366">In the example:</span></span>

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

<span data-ttu-id="53d9f-367">fixed deyimi, bir yapının bir sabit boyutlu arabellek adresini bir işaretçi olarak kullanılacak şekilde düzeltmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-367">a fixed statement is used to fix a fixed size buffer of a struct so its address can be used as a pointer.</span></span>

<span data-ttu-id="53d9f-368">A `char*` değer üretilmediğini bir dize örneği her zaman bir boş sonlandırılmış dizeye işaret düzelterek.</span><span class="sxs-lookup"><span data-stu-id="53d9f-368">A `char*` value produced by fixing a string instance always points to a null-terminated string.</span></span> <span data-ttu-id="53d9f-369">Bir işaretçi bir fixed deyimi içinde `p` dize örneğine `s`, işaretçi değerleri arasında değişen `p` için `p + s.Length - 1` adresleri karakter dizesi ve işaretçi değerini temsil eden `p + s.Length` her zaman null bir karaktere işaret eder (değer karakteriyle `'\0'`).</span><span class="sxs-lookup"><span data-stu-id="53d9f-369">Within a fixed statement that obtains a pointer `p` to a string instance `s`, the pointer values ranging from `p` to `p + s.Length - 1` represent addresses of the characters in the string, and the pointer value `p + s.Length` always points to a null character (the character with value `'\0'`).</span></span>

<span data-ttu-id="53d9f-370">Sabit işaretçileri aracılığıyla yönetilen türü değiştirme nesneler, sonuçlar tanımsız davranışa olabilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-370">Modifying objects of managed type through fixed pointers can results in undefined behavior.</span></span> <span data-ttu-id="53d9f-371">Dizeleri sabit olduğundan, örneğin, bu karakterlerin sabit bir dizeye bir işaretçi tarafından başvurulan değiştirilmedi emin olmak için programcının sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-371">For example, because strings are immutable, it is the programmer's responsibility to ensure that the characters referenced by a pointer to a fixed string are not modified.</span></span>

<span data-ttu-id="53d9f-372">Otomatik null sonlandırma dizeleri "C-style" dizeleri bekleyen dış API'lar çağırırken özellikle kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-372">The automatic null-termination of strings is particularly convenient when calling external APIs that expect "C-style" strings.</span></span> <span data-ttu-id="53d9f-373">Ancak, bir dize örneği null karakterleri içeren izni olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="53d9f-373">Note, however, that a string instance is permitted to contain null characters.</span></span> <span data-ttu-id="53d9f-374">Bu tür null karakterler varsa, dize bir null ile sonlandırılmış kabul edilir, kesilmiş görünür `char*`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-374">If such null characters are present, the string will appear truncated when treated as a null-terminated `char*`.</span></span>

## <a name="fixed-size-buffers"></a><span data-ttu-id="53d9f-375">Sabit boyutlu arabellekler</span><span class="sxs-lookup"><span data-stu-id="53d9f-375">Fixed size buffers</span></span>

<span data-ttu-id="53d9f-376">Sabit boyutlu arabellekler "C style" satır içi dizi yapılar üye olarak bildirmek için kullanılır ve yönetilmeyen API'ler ile arabirim için öncelikle yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-376">Fixed size buffers are used to declare "C style" in-line arrays as members of structs, and are primarily useful for interfacing with unmanaged APIs.</span></span>

### <a name="fixed-size-buffer-declarations"></a><span data-ttu-id="53d9f-377">Sabit boyutlu arabellek bildirimleri</span><span class="sxs-lookup"><span data-stu-id="53d9f-377">Fixed size buffer declarations</span></span>

<span data-ttu-id="53d9f-378">A ***sabit boyutlu arabellek*** depolama için belirli bir türde değişken bir sabit uzunluk arabelleği temsil eden bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-378">A ***fixed size buffer*** is a member that represents storage for a fixed length buffer of variables of a given type.</span></span> <span data-ttu-id="53d9f-379">Bir sabit boyutlu arabellek bildiriminde belirtilen öğe türünün bir veya daha fazla sabit boyutlu arabellekler tanıtır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-379">A fixed size buffer declaration introduces one or more fixed size buffers of a given element type.</span></span> <span data-ttu-id="53d9f-380">Sabit boyutlu arabellekler yalnızca yapı bildirimlerinde izin verilir ve yalnızca güvenli bağlamlarda oluşabilir ([güvensiz bir bağlamı](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="53d9f-380">Fixed size buffers are only permitted in struct declarations and can only occur in unsafe contexts ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

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

<span data-ttu-id="53d9f-381">Sabit boyutlu arabellek bildirimi öznitelikleri kümesi içerebilir ([öznitelikleri](attributes.md)), `new` değiştiricisi ([değiştiriciler](classes.md#modifiers)), geçerli bir bileşimin dört erişim değiştiricileri ([türü parametreleri ve kısıtlamalar](classes.md#type-parameters-and-constraints)) ve bir `unsafe` değiştirici ([güvensiz bir bağlamı](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="53d9f-381">A fixed size buffer declaration may include a set of attributes ([Attributes](attributes.md)), a `new` modifier ([Modifiers](classes.md#modifiers)), a valid combination of the four access modifiers ([Type parameters and constraints](classes.md#type-parameters-and-constraints)) and an `unsafe` modifier ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="53d9f-382">Öznitelikler ve değiştiriciler tüm sabit boyutlu arabellek bildirim tarafından bildirilen üyeler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-382">The attributes and modifiers apply to all of the members declared by the fixed size buffer declaration.</span></span> <span data-ttu-id="53d9f-383">Aynı değiştiricisi bir sabit boyutlu arabellek bildiriminde birden çok kez görünmesi için bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-383">It is an error for the same modifier to appear multiple times in a fixed size buffer declaration.</span></span>

<span data-ttu-id="53d9f-384">Sabit boyutlu arabellek bildirimi dahil etmek için izin verilmiyor `static` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="53d9f-384">A fixed size buffer declaration is not permitted to include the `static` modifier.</span></span>

<span data-ttu-id="53d9f-385">Arabellek öğe türü sabit boyutlu arabellek bildirimi arabellekleri öğe türü bildirim tarafından tanıtılan belirtir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-385">The buffer element type of a fixed size buffer declaration specifies the element type of the buffer(s) introduced by the declaration.</span></span> <span data-ttu-id="53d9f-386">Arabellek öğe türü, önceden tanımlanmış türlerden biri olmalıdır `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, veya `bool`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-386">The buffer element type must be one of the predefined types `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `bool`.</span></span>

<span data-ttu-id="53d9f-387">Arabellek öğe türü, her biri yeni bir üye tanıtır listesini sabit boyutlu arabellek Bildirimciler tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-387">The buffer element type is followed by a list of fixed size buffer declarators, each of which introduces a new member.</span></span> <span data-ttu-id="53d9f-388">İçine bir sabit ifade ardından üye adları bir tanımlayıcının bir sabit boyutlu arabellek bildirimci oluşur `[` ve `]` belirteçleri.</span><span class="sxs-lookup"><span data-stu-id="53d9f-388">A fixed size buffer declarator consists of an identifier that names the member, followed by a constant expression enclosed in `[` and `]` tokens.</span></span> <span data-ttu-id="53d9f-389">Sabit ifade, sabit boyutlu arabellek bildirimci tarafından sunulan üye içindeki öğelerin sayısını gösterir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-389">The constant expression denotes the number of elements in the member introduced by that fixed size buffer declarator.</span></span> <span data-ttu-id="53d9f-390">Sabit ifade türü türüne örtük olarak dönüştürülebilir olmalıdır `int`, ve sıfır olmayan pozitif bir tamsayı değeri olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-390">The type of the constant expression must be implicitly convertible to type `int`, and the value must be a non-zero positive integer.</span></span>

<span data-ttu-id="53d9f-391">Sabit boyutlu arabellek öğelerini sırayla bellekte düzenlendiği garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-391">The elements of a fixed size buffer are guaranteed to be laid out sequentially in memory.</span></span>

<span data-ttu-id="53d9f-392">Birden çok sabit boyutlu arabellekler bildiren bir sabit boyutlu arabellek bildirimi, aynı özniteliklerle ve öğe türleri tek bir sabit boyutlu arabellek bildirimin birden fazla bildirimi eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-392">A fixed size buffer declaration that declares multiple fixed size buffers is equivalent to multiple declarations of a single fixed size buffer declaration with the same attributes, and element types.</span></span> <span data-ttu-id="53d9f-393">Örneğin</span><span class="sxs-lookup"><span data-stu-id="53d9f-393">For example</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

<span data-ttu-id="53d9f-394">eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="53d9f-394">is equivalent to</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a><span data-ttu-id="53d9f-395">İfadelerde sabit boyutlu arabellekler</span><span class="sxs-lookup"><span data-stu-id="53d9f-395">Fixed size buffers in expressions</span></span>

<span data-ttu-id="53d9f-396">Üye araması ([işleçleri](expressions.md#operators)) sabit boyutlu arabellek üye tam olarak bir alanın üye araması gibi devam eder.</span><span class="sxs-lookup"><span data-stu-id="53d9f-396">Member lookup ([Operators](expressions.md#operators)) of a fixed size buffer member proceeds exactly like member lookup of a field.</span></span>

<span data-ttu-id="53d9f-397">Kullanarak bir ifade bir sabit boyutlu arabellek başvurulabilen bir *simple_name* ([anlam çıkarma](expressions.md#type-inference)) veya bir *member_access* ([derleme zamanı denetimi dinamik aşırı yükleme çözünürlüğü](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="53d9f-397">A fixed size buffer can be referenced in an expression using a *simple_name* ([Type inference](expressions.md#type-inference)) or a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="53d9f-398">Sabit boyutlu arabellek üyesi basit bir ad başvurulduğunda etkisi biçiminde bir üye erişimi aynıdır `this.I`burada `I` sabit boyutlu arabellek üyesidir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-398">When a fixed size buffer member is referenced as a simple name, the effect is the same as a member access of the form `this.I`, where `I` is the fixed size buffer member.</span></span>

<span data-ttu-id="53d9f-399">Üye erişimi formun içinde `E.I`, `E` bir yapı türü ve üye araması için `I` yapı türünün sabit boyutlu üyesi ardından belirler, `E.I` olduğu bir sınıflandırılmış gibi değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="53d9f-399">In a member access of the form `E.I`, if `E` is of a struct type and a member lookup of `I` in that struct type identifies a fixed size member, then `E.I` is evaluated an classified as follows:</span></span>

*  <span data-ttu-id="53d9f-400">İfade `E.I` oluşmaz güvenli olmayan bir bağlamda, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="53d9f-400">If the expression `E.I` does not occur in an unsafe context, a compile-time error occurs.</span></span>
*  <span data-ttu-id="53d9f-401">Varsa `E` bir derleme zamanı hatası oluşur, bir değer olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-401">If `E` is classified as a value, a compile-time error occurs.</span></span>
*  <span data-ttu-id="53d9f-402">Aksi takdirde `E` taşınabilir bir değişken ([sabit ve taşınabilir değişkenleri](unsafe-code.md#fixed-and-moveable-variables)) ve ifade `E.I` değil bir *fixed_pointer_initializer* ([düzeltildi deyimi](unsafe-code.md#the-fixed-statement)), bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="53d9f-402">Otherwise, if `E` is a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) and the expression `E.I` is not a *fixed_pointer_initializer* ([The fixed statement](unsafe-code.md#the-fixed-statement)), a compile-time error occurs.</span></span>
*  <span data-ttu-id="53d9f-403">Aksi takdirde, `E` sabit bir değişkene başvuruyor ve sonuç ifadesi bir sabit boyutlu arabellek üyenin ilk öğesinin işaretçisi `I` içinde `E`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-403">Otherwise, `E` references a fixed variable and the result of the expression is a pointer to the first element of the fixed size buffer member `I` in `E`.</span></span> <span data-ttu-id="53d9f-404">Sonuç türünde `S*`burada `S` öğe türü `I`ve bir değer olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-404">The result is of type `S*`, where `S` is the element type of `I`, and is classified as a value.</span></span>

<span data-ttu-id="53d9f-405">Sabit boyutlu arabellek sonraki öğeler ilk öğeye işaretçi işlemlerini kullanarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-405">The subsequent elements of the fixed size buffer can be accessed using pointer operations from the first element.</span></span> <span data-ttu-id="53d9f-406">Dizilere erişim, farklı bir sabit boyutlu arabellek öğelere erişim güvenli olmayan bir işlemdir ve kontrol aralığı değil.</span><span class="sxs-lookup"><span data-stu-id="53d9f-406">Unlike access to arrays, access to the elements of a fixed size buffer is an unsafe operation and is not range checked.</span></span>

<span data-ttu-id="53d9f-407">Aşağıdaki örnek, bildirir ve sabit boyutlu arabellek üyesi bir yapı kullanır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-407">The following example declares and uses a struct with a fixed size buffer member.</span></span>

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

### <a name="definite-assignment-checking"></a><span data-ttu-id="53d9f-408">Belirli atama onayına denetleniyor</span><span class="sxs-lookup"><span data-stu-id="53d9f-408">Definite assignment checking</span></span>

<span data-ttu-id="53d9f-409">Sabit boyutlu arabellekler olmayan belirli atama onayına denetimi tabi ([belirli atama onayına](variables.md#definite-assignment)), ve sabit boyutlu arabellek üyeleri, struct türüne değişkenlerinin denetimi özelliğine belirli atama amacıyla yoksayılır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-409">Fixed size buffers are not subject to definite assignment checking ([Definite assignment](variables.md#definite-assignment)), and fixed size buffer members are ignored for purposes of definite assignment checking of struct type variables.</span></span>

<span data-ttu-id="53d9f-410">En dıştaki içeren yapı değişkeni bir sabit boyutlu arabellek üyesinin, bir statik değişken, bir sınıf örneği ya da bir dizi öğesine bir örnek değişkeni sabit boyutlu arabellek öğeleri varsayılan değerlerine otomatik olarak başlatılır ([Varsayılan değerler](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="53d9f-410">When the outermost containing struct variable of a fixed size buffer member is a static variable, an instance variable of a class instance, or an array element, the elements of the fixed size buffer are automatically initialized to their default values ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="53d9f-411">Diğer durumlarda, ilk sabit boyutlu arabellek içeriği tanımsızdır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-411">In all other cases, the initial content of a fixed size buffer is undefined.</span></span>

## <a name="stack-allocation"></a><span data-ttu-id="53d9f-412">Yığın ayırma</span><span class="sxs-lookup"><span data-stu-id="53d9f-412">Stack allocation</span></span>

<span data-ttu-id="53d9f-413">Bir yerel değişken bildiriminde güvenli olmayan bir bağlamda ([yerel değişken bildirimlerini](statements.md#local-variable-declarations)) çağrı yığından belleği ayırır bir yığın ayırma Başlatıcı içerebilir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-413">In an unsafe context, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) may include a stack allocation initializer which allocates memory from the call stack.</span></span>

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="53d9f-414">*Unmanaged_type* yeni ayrılan bir konumda depolanan öğelerin türünü belirtir ve *ifade* bu öğe sayısını gösterir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-414">The *unmanaged_type* indicates the type of the items that will be stored in the newly allocated location, and the *expression* indicates the number of these items.</span></span> <span data-ttu-id="53d9f-415">Birlikte ele alındığında, bunlar için gerekli ayırma boyutu belirtin.</span><span class="sxs-lookup"><span data-stu-id="53d9f-415">Taken together, these specify the required allocation size.</span></span> <span data-ttu-id="53d9f-416">Yığın ayırma boyutu negatif olamaz, bu öğeleri olarak sayısını belirtmek için bir derleme zamanı hatası olduğu bir *constant_expression* negatif bir değer olarak değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="53d9f-416">Since the size of a stack allocation cannot be negative, it is a compile-time error to specify the number of items as a *constant_expression* that evaluates to a negative value.</span></span>

<span data-ttu-id="53d9f-417">Bir yığın ayırma Başlatıcı formun `stackalloc T[E]` gerektirir `T` nespravovaným typem olmasını ([işaretçi türleri](unsafe-code.md#pointer-types)) ve `E` türündeki bir ifade olmasını `int`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-417">A stack allocation initializer of the form `stackalloc T[E]` requires `T` to be an unmanaged type ([Pointer types](unsafe-code.md#pointer-types)) and `E` to be an expression of type `int`.</span></span> <span data-ttu-id="53d9f-418">Yapı ayırır `E * sizeof(T)` çağrısından bayt yığın ve türünde bir işaretçi döndürür `T*`, yeni ayrılan bloğu için.</span><span class="sxs-lookup"><span data-stu-id="53d9f-418">The construct allocates `E * sizeof(T)` bytes from the call stack and returns a pointer, of type `T*`, to the newly allocated block.</span></span> <span data-ttu-id="53d9f-419">Varsa `E` tanımsız bir davranıştır sonra negatif bir değer olan.</span><span class="sxs-lookup"><span data-stu-id="53d9f-419">If `E` is a negative value, then the behavior is undefined.</span></span> <span data-ttu-id="53d9f-420">Varsa `E` sıfırsa, ardından hiçbir ayırma yapılan ve döndürülen işaretçi uygulama tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-420">If `E` is zero, then no allocation is made, and the pointer returned is implementation-defined.</span></span> <span data-ttu-id="53d9f-421">Verilen boyuta bloğunu ayırmak yeterli bellek yoksa bir `System.StackOverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="53d9f-421">If there is not enough memory available to allocate a block of the given size, a `System.StackOverflowException` is thrown.</span></span>

<span data-ttu-id="53d9f-422">Yeni ayrılan bellek içeriğini tanımsızdır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-422">The content of the newly allocated memory is undefined.</span></span>

<span data-ttu-id="53d9f-423">Yığın ayırma başlatıcıları içinde verilmez `catch` veya `finally` blokları ([try deyimi](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="53d9f-423">Stack allocation initializers are not permitted in `catch` or `finally` blocks ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="53d9f-424">Açıkça kullanılarak ayrılmış belleği boşaltmak için bir yolu yoktur `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="53d9f-424">There is no way to explicitly free memory allocated using `stackalloc`.</span></span> <span data-ttu-id="53d9f-425">Bu işlev üyesi döndürdüğünde işlevi üyesi yürütülmesi sırasında oluşturulan tüm yığın tarafından ayrılan bellek blokları otomatik olarak atılır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-425">All stack allocated memory blocks created during the execution of a function member are automatically discarded when that function member returns.</span></span> <span data-ttu-id="53d9f-426">Bu karşılık gelir `alloca` işlev, genellikle C ve C++ uygulamalarında bulunan bir uzantı.</span><span class="sxs-lookup"><span data-stu-id="53d9f-426">This corresponds to the `alloca` function, an extension commonly found in C and C++ implementations.</span></span>

<span data-ttu-id="53d9f-427">Örnekte</span><span class="sxs-lookup"><span data-stu-id="53d9f-427">In the example</span></span>

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

<span data-ttu-id="53d9f-428">bir `stackalloc` Başlatıcı kullanılan `IntToString` yöntemi bir yığında 16 karakter arabelleği ayrılamadı.</span><span class="sxs-lookup"><span data-stu-id="53d9f-428">a `stackalloc` initializer is used in the `IntToString` method to allocate a buffer of 16 characters on the stack.</span></span> <span data-ttu-id="53d9f-429">Yöntem döndürüldüğünde arabellek otomatik olarak atılır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-429">The buffer is automatically discarded when the method returns.</span></span>

## <a name="dynamic-memory-allocation"></a><span data-ttu-id="53d9f-430">Dinamik bellek ayırma</span><span class="sxs-lookup"><span data-stu-id="53d9f-430">Dynamic memory allocation</span></span>

<span data-ttu-id="53d9f-431">Dışında `stackalloc` işleci, C# önceden tanımlanmış hiçbir yapıları olmayan atık toplanan bellek yönetmek için sağlar.</span><span class="sxs-lookup"><span data-stu-id="53d9f-431">Except for the `stackalloc` operator, C# provides no predefined constructs for managing non-garbage collected memory.</span></span> <span data-ttu-id="53d9f-432">Bu tür Hizmetleri genellikle sınıf kitaplıkları destekleyerek sağlanan veya doğrudan alttaki işletim sisteminden alınan.</span><span class="sxs-lookup"><span data-stu-id="53d9f-432">Such services are typically provided by supporting class libraries or imported directly from the underlying operating system.</span></span> <span data-ttu-id="53d9f-433">Örneğin, `Memory` sınıfı aşağıdaki nasıl temel işletim sistemi yığın işlevlerini C# ' den erişilebilen gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="53d9f-433">For example, the `Memory` class below illustrates how the heap functions of an underlying operating system might be accessed from C#:</span></span>

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

<span data-ttu-id="53d9f-434">Kullanan bir örnek `Memory` sınıfı aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="53d9f-434">An example that uses the `Memory` class is given below:</span></span>

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

<span data-ttu-id="53d9f-435">Örneğin 256 bayt ile bellek ayırır `Memory.Alloc` ve bellek bloğu 0 ile 255 artırma değerlerle başlatır.</span><span class="sxs-lookup"><span data-stu-id="53d9f-435">The example allocates 256 bytes of memory through `Memory.Alloc` and initializes the memory block with values increasing from 0 to 255.</span></span> <span data-ttu-id="53d9f-436">Ardından 256 öğe bayt dizisi ayırır ve kullandığı `Memory.Copy` bellek bloğu içeriğini bayt dizisine kopyalamak için.</span><span class="sxs-lookup"><span data-stu-id="53d9f-436">It then allocates a 256 element byte array and uses `Memory.Copy` to copy the contents of the memory block into the byte array.</span></span> <span data-ttu-id="53d9f-437">Son olarak, bellek bloğu kullanılarak serbest `Memory.Free` ve bayt dizisinin içeriğini konsola.</span><span class="sxs-lookup"><span data-stu-id="53d9f-437">Finally, the memory block is freed using `Memory.Free` and the contents of the byte array are output on the console.</span></span>
