# <a name="namespaces"></a><span data-ttu-id="de958-101">Ad Alanları</span><span class="sxs-lookup"><span data-stu-id="de958-101">Namespaces</span></span>

<span data-ttu-id="de958-102">Ad alanlarını kullanarak C# programları düzenlenir.</span><span class="sxs-lookup"><span data-stu-id="de958-102">C# programs are organized using namespaces.</span></span> <span data-ttu-id="de958-103">Ad alanları, hem bir program için bir "İç" Kuruluş sistemi olarak ve "harici" Kuruluş sistemi olarak kullanılır — diğer programlara sunulan program öğelerini sunmak için bir yol.</span><span class="sxs-lookup"><span data-stu-id="de958-103">Namespaces are used both as an "internal" organization system for a program, and as an "external" organization system—a way of presenting program elements that are exposed to other programs.</span></span>

<span data-ttu-id="de958-104">Using yönergeleri ([yönergeleri kullanarak](namespaces.md#using-directives)) ad alanları kullanımını kolaylaştırmak için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="de958-104">Using directives ([Using directives](namespaces.md#using-directives)) are provided to facilitate the use of namespaces.</span></span>

## <a name="compilation-units"></a><span data-ttu-id="de958-105">Derleme birimi</span><span class="sxs-lookup"><span data-stu-id="de958-105">Compilation units</span></span>

<span data-ttu-id="de958-106">A *compilation_unit* kaynak dosya genel yapısını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="de958-106">A *compilation_unit* defines the overall structure of a source file.</span></span> <span data-ttu-id="de958-107">Sıfır veya daha fazla derleme biriminde oluşur *using_directive*s ardından sıfır veya daha fazla *global_attributes* ardından sıfır veya daha fazla *namespace_member_declaration*s .</span><span class="sxs-lookup"><span data-stu-id="de958-107">A compilation unit consists of zero or more *using_directive*s followed by zero or more *global_attributes* followed by zero or more *namespace_member_declaration*s.</span></span>

```antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? namespace_member_declaration*
    ;
```

<span data-ttu-id="de958-108">Bir C# programı bir veya birden çok derleme biriminden oluşur, her bir ayrı bir kaynak dosyasında yer alan.</span><span class="sxs-lookup"><span data-stu-id="de958-108">A C# program consists of one or more compilation units, each contained in a separate source file.</span></span> <span data-ttu-id="de958-109">Bir C# programı derlendiğinde, tüm derleme birimlerinin birlikte işlenir.</span><span class="sxs-lookup"><span data-stu-id="de958-109">When a C# program is compiled, all of the compilation units are processed together.</span></span> <span data-ttu-id="de958-110">Bu nedenle, derleme biriminden birbirleri üzerinde büyük olasılıkla dönüşümlü bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="de958-110">Thus, compilation units can depend on each other, possibly in a circular fashion.</span></span>

<span data-ttu-id="de958-111">*Using_directive*derleme birimi etkiler, s *global_attributes* ve *namespace_member_declaration*s, bu derleme biriminin ancak sahip hiçbir etkisi diğer derleme birimi.</span><span class="sxs-lookup"><span data-stu-id="de958-111">The *using_directive*s of a compilation unit affect the *global_attributes* and *namespace_member_declaration*s of that compilation unit, but have no effect on other compilation units.</span></span>

<span data-ttu-id="de958-112">*Global_attributes* ([öznitelikleri](attributes.md)) derleme birimi hedef assembly ve module öznitelikleri belirtilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="de958-112">The *global_attributes* ([Attributes](attributes.md)) of a compilation unit permit the specification of attributes for the target assembly and module.</span></span> <span data-ttu-id="de958-113">Derlemeler ve modüller türleri için fiziksel bir kapsayıcı olarak davranır.</span><span class="sxs-lookup"><span data-stu-id="de958-113">Assemblies and modules act as physical containers for types.</span></span> <span data-ttu-id="de958-114">Bir derlemeyi birden fazla fiziksel olarak ayrı modüllerini içerebilir.</span><span class="sxs-lookup"><span data-stu-id="de958-114">An assembly may consist of several physically separate modules.</span></span>

<span data-ttu-id="de958-115">*Namespace_member_declaration*s her derleme biriminde bir programın, genel ad alanı adı verilen bir tek bildirim alanına üyeleri katkıda.</span><span class="sxs-lookup"><span data-stu-id="de958-115">The *namespace_member_declaration*s of each compilation unit of a program contribute members to a single declaration space called the global namespace.</span></span> <span data-ttu-id="de958-116">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="de958-116">For example:</span></span>

<span data-ttu-id="de958-117">Dosya `A.cs`:</span><span class="sxs-lookup"><span data-stu-id="de958-117">File `A.cs`:</span></span>
```csharp
class A {}
```

<span data-ttu-id="de958-118">Dosya `B.cs`:</span><span class="sxs-lookup"><span data-stu-id="de958-118">File `B.cs`:</span></span>
```csharp
class B {}
```

<span data-ttu-id="de958-119">Bu durumda tam olarak nitelenmiş ada sahip iki sınıfları bildirme iki derleme biriminden tek genel ad alanına katkıda `A` ve `B`.</span><span class="sxs-lookup"><span data-stu-id="de958-119">The two compilation units contribute to the single global namespace, in this case declaring two classes with the fully qualified names `A` and `B`.</span></span> <span data-ttu-id="de958-120">İki derleme biriminden aynı bildirim alanına katkıda bulunmak için her bir bildirimi aynı ada sahip bir üye içeriyorsa bir hata olacaktı.</span><span class="sxs-lookup"><span data-stu-id="de958-120">Because the two compilation units contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

## <a name="namespace-declarations"></a><span data-ttu-id="de958-121">Namespace bildirimi</span><span class="sxs-lookup"><span data-stu-id="de958-121">Namespace declarations</span></span>

<span data-ttu-id="de958-122">A *namespace_declaration* anahtar sözcüğünü oluşur `namespace`, ardından bir ad alanı adı ve gövde, isteğe bağlı olarak noktalı virgül ekleyin.</span><span class="sxs-lookup"><span data-stu-id="de958-122">A *namespace_declaration* consists of the keyword `namespace`, followed by a namespace name and body, optionally followed by a semicolon.</span></span>

```antlr
namespace_declaration
    : 'namespace' qualified_identifier namespace_body ';'?
    ;

qualified_identifier
    : identifier ('.' identifier)*
    ;

namespace_body
    : '{' extern_alias_directive* using_directive* namespace_member_declaration* '}'
    ;
```

<span data-ttu-id="de958-123">A *namespace_declaration* bir üst düzey bildiriminde olarak ortaya çıkabilecek bir *compilation_unit* veya bir üye bildirimi içindeki başka olarak *namespace_declaration*.</span><span class="sxs-lookup"><span data-stu-id="de958-123">A *namespace_declaration* may occur as a top-level declaration in a *compilation_unit* or as a member declaration within another *namespace_declaration*.</span></span> <span data-ttu-id="de958-124">Olduğunda bir *namespace_declaration* bir üst düzey bildiriminde olarak gerçekleşir bir *compilation_unit*, ad alanı genel ad alanının bir üyesi haline gelir.</span><span class="sxs-lookup"><span data-stu-id="de958-124">When a *namespace_declaration* occurs as a top-level declaration in a *compilation_unit*, the namespace becomes a member of the global namespace.</span></span> <span data-ttu-id="de958-125">Olduğunda bir *namespace_declaration* başka gerçekleşir *namespace_declaration*, iç ad alanı, dış ad alanının bir üyesi haline gelir.</span><span class="sxs-lookup"><span data-stu-id="de958-125">When a *namespace_declaration* occurs within another *namespace_declaration*, the inner namespace becomes a member of the outer namespace.</span></span> <span data-ttu-id="de958-126">Her iki durumda da, bir ad alanı adını içeren ad alanı içinde benzersiz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="de958-126">In either case, the name of a namespace must be unique within the containing namespace.</span></span>

<span data-ttu-id="de958-127">Ad alanları olan örtük olarak `public` ve hiçbir erişim değiştiricileri bir ad alanı bildirimi içeremez.</span><span class="sxs-lookup"><span data-stu-id="de958-127">Namespaces are implicitly `public` and the declaration of a namespace cannot include any access modifiers.</span></span>

<span data-ttu-id="de958-128">İçinde bir *namespace_body*, isteğe bağlı *using_directive*s içeri aktarma diğer ad alanları, türler ve üyeler, nitelikli adlar doğrudan yerine, başvurulabilmesi için adları.</span><span class="sxs-lookup"><span data-stu-id="de958-128">Within a *namespace_body*, the optional *using_directive*s import the names of other namespaces, types and members, allowing them to be referenced directly instead of through qualified names.</span></span> <span data-ttu-id="de958-129">İsteğe bağlı *namespace_member_declaration*s bildirim alanına ad alanının üyeleri katkıda bulunun.</span><span class="sxs-lookup"><span data-stu-id="de958-129">The optional *namespace_member_declaration*s contribute members to the declaration space of the namespace.</span></span> <span data-ttu-id="de958-130">Unutmayın tüm *using_directive*s, herhangi bir üye bildirimleri önce görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="de958-130">Note that all *using_directive*s must appear before any member declarations.</span></span>

<span data-ttu-id="de958-131">*Qualified_identifier* , bir *namespace_declaration* tek bir tanımlayıcı veya ayrılmış tanımlayıcılar bir dizi olabilir "`.`" belirteçleri.</span><span class="sxs-lookup"><span data-stu-id="de958-131">The *qualified_identifier* of a *namespace_declaration* may be a single identifier or a sequence of identifiers separated by "`.`" tokens.</span></span> <span data-ttu-id="de958-132">İkinci form, birden fazla ad alanı bildirimi sözcüksel olarak iç içe geçirme olmadan bir iç içe geçmiş ad alanı tanımlamak için bir program izin verir.</span><span class="sxs-lookup"><span data-stu-id="de958-132">The latter form permits a program to define a nested namespace without lexically nesting several namespace declarations.</span></span> <span data-ttu-id="de958-133">Örneğin,</span><span class="sxs-lookup"><span data-stu-id="de958-133">For example,</span></span>

```csharp
namespace N1.N2
{
    class A {}

    class B {}
}
```
<span data-ttu-id="de958-134">anlamsal olarak eşdeğer olan</span><span class="sxs-lookup"><span data-stu-id="de958-134">is semantically equivalent to</span></span>
```csharp
namespace N1
{
    namespace N2
    {
        class A {}

        class B {}
    }
}
```

<span data-ttu-id="de958-135">Açık uçlu ad alanlarını ve aynı tam ada sahip iki ad alanı bildirimi katkıda aynı bildirim alanına ([bildirimleri](basic-concepts.md#declarations)).</span><span class="sxs-lookup"><span data-stu-id="de958-135">Namespaces are open-ended, and two namespace declarations with the same fully qualified name contribute to the same declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="de958-136">Örnekte</span><span class="sxs-lookup"><span data-stu-id="de958-136">In the example</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N1.N2
{
    class B {}
}
```
<span data-ttu-id="de958-137">Bu durumda tam olarak nitelenmiş ada sahip iki sınıfları bildirme aynı bildirim alanına, yukarıdaki iki ad alanı bildirimi katkıda `N1.N2.A` ve `N1.N2.B`.</span><span class="sxs-lookup"><span data-stu-id="de958-137">the two namespace declarations above contribute to the same declaration space, in this case declaring two classes with the fully qualified names `N1.N2.A` and `N1.N2.B`.</span></span> <span data-ttu-id="de958-138">İki bildirimi aynı bildirim alanına katkıda bulunmak için bir hata her varsa aynı ada sahip bir üye bildirimi bulunan olacaktı.</span><span class="sxs-lookup"><span data-stu-id="de958-138">Because the two declarations contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

## <a name="extern-aliases"></a><span data-ttu-id="de958-139">Extern diğer adları</span><span class="sxs-lookup"><span data-stu-id="de958-139">Extern aliases</span></span>

<span data-ttu-id="de958-140">Bir *extern_alias_directive* bir ad alanı için bir diğer ad olarak hizmet veren bir tanımlayıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="de958-140">An *extern_alias_directive* introduces an identifier that serves as an alias for a namespace.</span></span> <span data-ttu-id="de958-141">Diğer adlı ad alanı belirtimi, programın kaynak koduna dış ve diğer adlı ad alanı iç içe geçmiş ad alanları için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="de958-141">The specification of the aliased namespace is external to the source code of the program and applies also to nested namespaces of the aliased namespace.</span></span>

```antlr
extern_alias_directive
    : 'extern' 'alias' identifier ';'
    ;
```

<span data-ttu-id="de958-142">Kapsamı bir *extern_alias_directive* üzerinden genişletir *using_directive*s, *global_attributes* ve *namespace_member_declaration*hemen içeren derleme birimi veya ad alanı gövdesinde, s.</span><span class="sxs-lookup"><span data-stu-id="de958-142">The scope of an *extern_alias_directive* extends over the *using_directive*s, *global_attributes* and *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span>

<span data-ttu-id="de958-143">İçeren bir derleme birimi veya ad alanı gövdesi içindeki bir *extern_alias_directive*, tarafından tanıtılan tanımlayıcısı *extern_alias_directive* diğer adlı ad alanı başvuru için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="de958-143">Within a compilation unit or namespace body that contains an *extern_alias_directive*, the identifier introduced by the *extern_alias_directive* can be used to reference the aliased namespace.</span></span> <span data-ttu-id="de958-144">Bir derleme zamanı hata için *tanımlayıcı* kelime olarak `global`.</span><span class="sxs-lookup"><span data-stu-id="de958-144">It is a compile-time error for the *identifier* to be the word `global`.</span></span>

<span data-ttu-id="de958-145">Bir *extern_alias_directive* bir derleme belirli birimi veya ad alanı gövdesi içindeki diğer kullanılabilir hale getirir, ancak tüm yeni üyeleri temel alınan bildirim alanına gereksinimdir.</span><span class="sxs-lookup"><span data-stu-id="de958-145">An *extern_alias_directive* makes an alias available within a particular compilation unit or namespace body, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="de958-146">Diğer bir deyişle, bir *extern_alias_directive* geçişli değildir ancak bunun yerine oluştuğu yalnızca derleme birimi veya ad alanı gövdesi etkiler.</span><span class="sxs-lookup"><span data-stu-id="de958-146">In other words, an *extern_alias_directive* is not transitive, but, rather, affects only the compilation unit or namespace body in which it occurs.</span></span>

<span data-ttu-id="de958-147">Aşağıdaki program bildirir ve iki extern diğer adları kullanır `X` ve `Y`, her biri ayrı ad alanı hiyerarşisinin kökü temsil:</span><span class="sxs-lookup"><span data-stu-id="de958-147">The following program declares and uses two extern aliases, `X` and `Y`, each of which represent the root of a distinct namespace hierarchy:</span></span>
```csharp
extern alias X;
extern alias Y;

class Test
{
    X::N.A a;
    X::N.B b1;
    Y::N.B b2;
    Y::N.C c;
}
```

<span data-ttu-id="de958-148">Program extern varlığını diğer adları bildirir `X` ve `Y`, ancak diğer adlar gerçek tanımlarını programa dış.</span><span class="sxs-lookup"><span data-stu-id="de958-148">The program declares the existence of the extern aliases `X` and `Y`, but the actual definitions of the aliases are external to the program.</span></span> <span data-ttu-id="de958-149">Aynı adlı `N.B` sınıfları artık başvurulabilir olarak `X.N.B` ve `Y.N.B`, veya ad alanı diğer ad niteleyicisi kullanarak `X::N.B` ve `Y::N.B`.</span><span class="sxs-lookup"><span data-stu-id="de958-149">The identically named `N.B` classes can now be referenced as `X.N.B` and `Y.N.B`, or, using the namespace alias qualifier, `X::N.B` and `Y::N.B`.</span></span> <span data-ttu-id="de958-150">Bir program dış tanım olarak sağlanan bir extern diğer adı bildiren bir hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="de958-150">An error occurs if a program declares an extern alias for which no external definition is provided.</span></span>

## <a name="using-directives"></a><span data-ttu-id="de958-151">using yönergeleri</span><span class="sxs-lookup"><span data-stu-id="de958-151">Using directives</span></span>

<span data-ttu-id="de958-152">***Using yönergeleri*** ad alanları ve diğer ad alanlarında tanımlanan türlerin kullanımı kolaylaştırmak.</span><span class="sxs-lookup"><span data-stu-id="de958-152">***Using directives*** facilitate the use of namespaces and types defined in other namespaces.</span></span> <span data-ttu-id="de958-153">Ad çözümleme işlemi yönergeleri etkisi kullanarak *namespace_or_type_name*s ([Namespace ve tür adları](basic-concepts.md#namespace-and-type-names)) ve *simple_name*s ([basit adları ](expressions.md#simple-names)), ancak bildirimleri yönergeleri kullanarak, farklı değil katkıda yeni üyeler temel alınan bildirim alanlarına derleme biriminden veya ad alanları içinde bunlar kullanılır.</span><span class="sxs-lookup"><span data-stu-id="de958-153">Using directives impact the name resolution process of *namespace_or_type_name*s ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) and *simple_name*s ([Simple names](expressions.md#simple-names)), but unlike declarations, using directives do not contribute new members to the underlying declaration spaces of the compilation units or namespaces within which they are used.</span></span>

```antlr
using_directive
    : using_alias_directive
    | using_namespace_directive
    | using_static_directive
    ;
```

<span data-ttu-id="de958-154">A *using_alias_directive* ([diğer yönergeleri kullanarak](namespaces.md#using-alias-directives)) ad alanı veya tür için bir diğer ad tanıtır.</span><span class="sxs-lookup"><span data-stu-id="de958-154">A *using_alias_directive* ([Using alias directives](namespaces.md#using-alias-directives)) introduces an alias for a namespace or type.</span></span>

<span data-ttu-id="de958-155">A *using_namespace_directive* ([ad alanı yönergeleri kullanarak](namespaces.md#using-namespace-directives)) bir ad alanı, tür üyeleri alır.</span><span class="sxs-lookup"><span data-stu-id="de958-155">A *using_namespace_directive* ([Using namespace directives](namespaces.md#using-namespace-directives)) imports the type members of a namespace.</span></span>

<span data-ttu-id="de958-156">A *using_static_directive* ([statik yönergeleri kullanarak](namespaces.md#using-static-directives)) bir türün statik üyeleri ve iç içe geçmiş türleri içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="de958-156">A *using_static_directive* ([Using static directives](namespaces.md#using-static-directives)) imports the nested types and static members of a type.</span></span>

<span data-ttu-id="de958-157">Kapsamı bir *using_directive* üzerinden genişletir *namespace_member_declaration*hemen içeren derleme birimi veya ad alanı gövdesinde, s.</span><span class="sxs-lookup"><span data-stu-id="de958-157">The scope of a *using_directive* extends over the *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="de958-158">Kapsamı bir *using_directive* özellikle, eş içermez *using_directive*s.</span><span class="sxs-lookup"><span data-stu-id="de958-158">The scope of a *using_directive* specifically does not include its peer *using_directive*s.</span></span> <span data-ttu-id="de958-159">Bu nedenle, eş *using_directive*s birbirine etkilemez ve bunlar yazılan sırası belirli değildir.</span><span class="sxs-lookup"><span data-stu-id="de958-159">Thus, peer *using_directive*s do not affect each other, and the order in which they are written is insignificant.</span></span>

### <a name="using-alias-directives"></a><span data-ttu-id="de958-160">Using diğer adı yönergeleri</span><span class="sxs-lookup"><span data-stu-id="de958-160">Using alias directives</span></span>

<span data-ttu-id="de958-161">A *using_alias_directive* için bir ad alanı veya tür kapsayan derleme birimi veya ad alanı gövdesi içinde bir diğer ad olarak hizmet veren bir tanımlayıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="de958-161">A *using_alias_directive* introduces an identifier that serves as an alias for a namespace or type within the immediately enclosing compilation unit or namespace body.</span></span>

```antlr
using_alias_directive
    : 'using' identifier '=' namespace_or_type_name ';'
    ;
```

<span data-ttu-id="de958-162">Üye bildirimleri gövdesindeki içeren bir derleme birimi veya ad alanı içinde bir *using_alias_directive*, tarafından tanıtılan tanımlayıcısı *using_alias_directive* kullanılabilir başvurmak için verilen ad alanı veya tür.</span><span class="sxs-lookup"><span data-stu-id="de958-162">Within member declarations in a compilation unit or namespace body that contains a *using_alias_directive*, the identifier introduced by the *using_alias_directive* can be used to reference the given namespace or type.</span></span> <span data-ttu-id="de958-163">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="de958-163">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;

    class B: A {}
}
```

<span data-ttu-id="de958-164">Yukarıdaki üye bildirimlerinde içinde `N3` ad alanı, `A` için bir diğer addır `N1.N2.A`ve bu nedenle sınıf `N3.B` sınıfından türetilen `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="de958-164">Above, within member declarations in the `N3` namespace, `A` is an alias for `N1.N2.A`, and thus class `N3.B` derives from class `N1.N2.A`.</span></span> <span data-ttu-id="de958-165">Bir diğer ad oluşturarak aynı etkiyi elde edilebilir `R` için `N1.N2` ve ardından başvuran `R.A`:</span><span class="sxs-lookup"><span data-stu-id="de958-165">The same effect can be obtained by creating an alias `R` for `N1.N2` and then referencing `R.A`:</span></span>
```csharp
namespace N3
{
    using R = N1.N2;

    class B: R.A {}
}
```

<span data-ttu-id="de958-166">*Tanımlayıcı* , bir *using_alias_directive* derleme biriminde veya hemen içeren ad alanı bildirimi alanı içinde benzersiz olmalıdır *using_alias_directive* .</span><span class="sxs-lookup"><span data-stu-id="de958-166">The *identifier* of a *using_alias_directive* must be unique within the declaration space of the compilation unit or namespace that immediately contains the *using_alias_directive*.</span></span> <span data-ttu-id="de958-167">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="de958-167">For example:</span></span>
```csharp
namespace N3
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;        // Error, A already exists
}
```

<span data-ttu-id="de958-168">Yukarıdaki `N3` zaten bir üye içeriyorsa `A`, bir derleme zamanı hatası için bu nedenle bir *using_alias_directive* tanımlayıcı kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="de958-168">Above, `N3` already contains a member `A`, so it is a compile-time error for a *using_alias_directive* to use that identifier.</span></span> <span data-ttu-id="de958-169">Benzer şekilde, iki veya daha fazlası için bir derleme zamanı hatası olduğu *using_alias_directive*aynı adlı diğer ad bildirmek için aynı derleme birimi veya ad alanı gövdesinde s.</span><span class="sxs-lookup"><span data-stu-id="de958-169">Likewise, it is a compile-time error for two or more *using_alias_directive*s in the same compilation unit or namespace body to declare aliases by the same name.</span></span>

<span data-ttu-id="de958-170">A *using_alias_directive* bir derleme belirli birimi veya ad alanı gövdesi içindeki diğer kullanılabilir hale getirir, ancak tüm yeni üyeleri temel alınan bildirim alanına gereksinimdir.</span><span class="sxs-lookup"><span data-stu-id="de958-170">A *using_alias_directive* makes an alias available within a particular compilation unit or namespace body, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="de958-171">Diğer bir deyişle, bir *using_alias_directive* geçişli değildir ancak bunun yerine oluştuğu yalnızca derleme birimi veya ad alanı gövdesi etkiler.</span><span class="sxs-lookup"><span data-stu-id="de958-171">In other words, a *using_alias_directive* is not transitive but rather affects only the compilation unit or namespace body in which it occurs.</span></span> <span data-ttu-id="de958-172">Örnekte</span><span class="sxs-lookup"><span data-stu-id="de958-172">In the example</span></span>
```csharp
namespace N3
{
    using R = N1.N2;
}

namespace N3
{
    class B: R.A {}            // Error, R unknown
}
```
<span data-ttu-id="de958-173">kapsamı *using_alias_directive* sunan `R` yalnızca üye bildirimleri, bu yer alan, ad alanı gövdesinde genişleyen şekilde `R` ikinci ad alanı bildiriminde bilinmiyor.</span><span class="sxs-lookup"><span data-stu-id="de958-173">the scope of the *using_alias_directive* that introduces `R` only extends to member declarations in the namespace body in which it is contained, so `R` is unknown in the second namespace declaration.</span></span> <span data-ttu-id="de958-174">Ancak, yerleştirme *using_alias_directive* içeren derlemede, her iki ad alanı bildirimi içinde kullanılabilir olana kadar diğer birim neden olur:</span><span class="sxs-lookup"><span data-stu-id="de958-174">However, placing the *using_alias_directive* in the containing compilation unit causes the alias to become available within both namespace declarations:</span></span>
```csharp
using R = N1.N2;

namespace N3
{
    class B: R.A {}
}

namespace N3
{
    class C: R.A {}
}
```

<span data-ttu-id="de958-175">Yalnızca normal üyeler gibi adlar kullanılmaya tarafından *using_alias_directive*s, iç içe geçmiş kapsamlar benzer ada üyeleri tarafından gizlidir.</span><span class="sxs-lookup"><span data-stu-id="de958-175">Just like regular members, names introduced by *using_alias_directive*s are hidden by similarly named members in nested scopes.</span></span> <span data-ttu-id="de958-176">Örnekte</span><span class="sxs-lookup"><span data-stu-id="de958-176">In the example</span></span>
```csharp
using R = N1.N2;

namespace N3
{
    class R {}

    class B: R.A {}        // Error, R has no member A
}
```
<span data-ttu-id="de958-177">başvuru `R.A` bildiriminde `B` çünkü bir derleme zamanı hatasına neden olur `R` başvurduğu `N3.R`değil `N1.N2`.</span><span class="sxs-lookup"><span data-stu-id="de958-177">the reference to `R.A` in the declaration of `B` causes a compile-time error because `R` refers to `N3.R`, not `N1.N2`.</span></span>

<span data-ttu-id="de958-178">Bir sırayı *using_alias_directive*s sahip hiçbir anlam ve çözünürlüğüne göre yazılmış *namespace_or_type_name* tarafından başvurulan bir *using_alias_directive*etkilenmez *using_alias_directive* kendisini veya diğer *using_directive*hemen içeren derleme birimi veya ad alanı gövdesinde s.</span><span class="sxs-lookup"><span data-stu-id="de958-178">The order in which *using_alias_directive*s are written has no significance, and resolution of the *namespace_or_type_name* referenced by a *using_alias_directive* is not affected by the *using_alias_directive* itself or by other *using_directive*s in the immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="de958-179">Diğer bir deyişle, *namespace_or_type_name* , bir *using_alias_directive* hemen içeren derleme birimi veya ad alanı gövdesi yok varmış gibi çözümlenir *using_directive*s.</span><span class="sxs-lookup"><span data-stu-id="de958-179">In other words, the *namespace_or_type_name* of a *using_alias_directive* is resolved as if the immediately containing compilation unit or namespace body had no *using_directive*s.</span></span> <span data-ttu-id="de958-180">A *using_alias_directive* ancak etkilenen *extern_alias_directive*hemen içeren derleme birimi veya ad alanı gövdesinde s.</span><span class="sxs-lookup"><span data-stu-id="de958-180">A *using_alias_directive* may however be affected by *extern_alias_directive*s in the immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="de958-181">Örnekte</span><span class="sxs-lookup"><span data-stu-id="de958-181">In the example</span></span>
```csharp
namespace N1.N2 {}

namespace N3
{
    extern alias E;

    using R1 = E.N;        // OK

    using R2 = N1;         // OK

    using R3 = N1.N2;      // OK

    using R4 = R2.N2;      // Error, R2 unknown
}
```
<span data-ttu-id="de958-182">Son *using_alias_directive* ilk tarafından etkilenmez, çünkü bir derleme zamanı hatası oluşur *using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="de958-182">the last *using_alias_directive* results in a compile-time error because it is not affected by the first *using_alias_directive*.</span></span> <span data-ttu-id="de958-183">İlk *using_alias_directive* extern diğer adı kapsamını beri hatayla sonuçlanmaz `E` içerir *using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="de958-183">The first *using_alias_directive* does not result in an error since the scope of the extern alias `E` includes the *using_alias_directive*.</span></span>

<span data-ttu-id="de958-184">A *using_alias_directive* herhangi bir ad alanı veya tür, bu ad alanı içinde iç içe geçmiş ve herhangi bir ad alanı veya içinde geçtiği ad alanı dahil türü için bir diğer ad oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="de958-184">A *using_alias_directive* can create an alias for any namespace or type, including the namespace within which it appears and any namespace or type nested within that namespace.</span></span>

<span data-ttu-id="de958-185">Ad alanı veya tür bir diğer ad erişme, bu ad alanı veya tür, bildirilen ad erişme tam olarak aynı sonucu verir.</span><span class="sxs-lookup"><span data-stu-id="de958-185">Accessing a namespace or type through an alias yields exactly the same result as accessing that namespace or type through its declared name.</span></span> <span data-ttu-id="de958-186">Örneğin,</span><span class="sxs-lookup"><span data-stu-id="de958-186">For example, given</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using R1 = N1;
    using R2 = N1.N2;

    class B
    {
        N1.N2.A a;            // refers to N1.N2.A
        R1.N2.A b;            // refers to N1.N2.A
        R2.A c;               // refers to N1.N2.A
    }
}
```
<span data-ttu-id="de958-187">adları `N1.N2.A`, `R1.N2.A`, ve `R2.A` olan eşdeğer ve tüm başvuru tam adı olan sınıfa `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="de958-187">the names `N1.N2.A`, `R1.N2.A`, and `R2.A` are equivalent and all refer to the class whose fully qualified name is `N1.N2.A`.</span></span>

<span data-ttu-id="de958-188">Diğer adların kullanımı kapalı bir oluşturulmuş tür adı, ancak tür bağımsız değişkenleri sağlamadan adı ilişkisiz genel tür bildirimi olamaz.</span><span class="sxs-lookup"><span data-stu-id="de958-188">Using aliases can name a closed constructed type, but cannot name an unbound generic type declaration without supplying type arguments.</span></span> <span data-ttu-id="de958-189">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="de958-189">For example:</span></span>
```csharp
namespace N1
{
    class A<T>
    {
        class B {}
    }
}

namespace N2
{
    using W = N1.A;          // Error, cannot name unbound generic type

    using X = N1.A.B;        // Error, cannot name unbound generic type

    using Y = N1.A<int>;     // Ok, can name closed constructed type

    using Z<T> = N1.A<T>;    // Error, using alias cannot have type parameters
}
```

### <a name="using-namespace-directives"></a><span data-ttu-id="de958-190">Ad alanı yönergeleri kullanma</span><span class="sxs-lookup"><span data-stu-id="de958-190">Using namespace directives</span></span>

<span data-ttu-id="de958-191">A *using_namespace_directive* niteleme olmadan kullanılmasını için her tür tanımlayıcısını etkinleştirme kapsayan derleme birimi veya ad alanı gövdesine, bir ad alanında bulunan türleri içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="de958-191">A *using_namespace_directive* imports the types contained in a namespace into the immediately enclosing compilation unit or namespace body, enabling the identifier of each type to be used without qualification.</span></span>

```antlr
using_namespace_directive
    : 'using' namespace_name ';'
    ;
```

<span data-ttu-id="de958-192">Üye bildirimleri gövdesindeki içeren bir derleme birimi veya ad alanı içinde bir *using_namespace_directive*, verilen ad alanında yer alan tür doğrudan başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="de958-192">Within member declarations in a compilation unit or namespace body that contains a *using_namespace_directive*, the types contained in the given namespace can be referenced directly.</span></span> <span data-ttu-id="de958-193">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="de958-193">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1.N2;

    class B: A {}
}
```

<span data-ttu-id="de958-194">Yukarıdaki üye bildirimlerinde içinde `N3` ad alanı, tür üyelerinin `N1.N2` doğrudan kullanılabilir ve bu nedenle sınıf `N3.B` sınıfından türetilen `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="de958-194">Above, within member declarations in the `N3` namespace, the type members of `N1.N2` are directly available, and thus class `N3.B` derives from class `N1.N2.A`.</span></span>

<span data-ttu-id="de958-195">A *using_namespace_directive* verilen ad alanında bulunan türleri içeri aktarır, ancak iç içe ad alanlarını özellikle içeri aktarmaz.</span><span class="sxs-lookup"><span data-stu-id="de958-195">A *using_namespace_directive* imports the types contained in the given namespace, but specifically does not import nested namespaces.</span></span> <span data-ttu-id="de958-196">Örnekte</span><span class="sxs-lookup"><span data-stu-id="de958-196">In the example</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1;

    class B: N2.A {}        // Error, N2 unknown
}
```
<span data-ttu-id="de958-197">*using_namespace_directive* bulunan türleri içeri aktarır `N1`, ancak ad iç içe geçmiş `N1`.</span><span class="sxs-lookup"><span data-stu-id="de958-197">the *using_namespace_directive* imports the types contained in `N1`, but not the namespaces nested in `N1`.</span></span> <span data-ttu-id="de958-198">Bu nedenle, başvuru `N2.A` bildiriminde `B` üye yok adlı bir derleme zamanı hatası oluşur `N2` kapsam içindedir.</span><span class="sxs-lookup"><span data-stu-id="de958-198">Thus, the reference to `N2.A` in the declaration of `B` results in a compile-time error because no members named `N2` are in scope.</span></span>

<span data-ttu-id="de958-199">Farklı bir *using_alias_directive*, *using_namespace_directive* , tanımlayıcıları zaten kapsayan derleme birimi veya ad alanı gövdesinde tanımlanan türleri alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="de958-199">Unlike a *using_alias_directive*, a *using_namespace_directive* may import types whose identifiers are already defined within the enclosing compilation unit or namespace body.</span></span> <span data-ttu-id="de958-200">Aslında, adları içeri aktarılan bir *using_namespace_directive* kapsayan derleme birimi veya ad alanı gövdesinde benzer ada üyeleri tarafından gizlenir.</span><span class="sxs-lookup"><span data-stu-id="de958-200">In effect, names imported by a *using_namespace_directive* are hidden by similarly named members in the enclosing compilation unit or namespace body.</span></span> <span data-ttu-id="de958-201">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="de958-201">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}

    class B {}
}

namespace N3
{
    using N1.N2;

    class A {}
}
```

<span data-ttu-id="de958-202">Burada, üye bildirimlerinde içinde `N3` ad alanı, `A` başvurduğu `N3.A` yerine `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="de958-202">Here, within member declarations in the `N3` namespace, `A` refers to `N3.A` rather than `N1.N2.A`.</span></span>

<span data-ttu-id="de958-203">Birden fazla ad alanı veya tür alınan tarafından ne zaman *using_namespace_directive*s veya *using_static_directive*s aynı derleme birimi veya ad alanı gövdesinde türleri için aynı adla başvurular içerir Bu ada bir *type_name* belirsiz olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="de958-203">When more than one namespace or type imported by *using_namespace_directive*s or *using_static_directive*s in the same compilation unit or namespace body contain types by the same name, references to that name as a *type_name* are considered ambiguous.</span></span> <span data-ttu-id="de958-204">Örnekte</span><span class="sxs-lookup"><span data-stu-id="de958-204">In the example</span></span>
```csharp
namespace N1
{
    class A {}
}

namespace N2
{
    class A {}
}

namespace N3
{
    using N1;

    using N2;

    class B: A {}                // Error, A is ambiguous
}
```
<span data-ttu-id="de958-205">her ikisi de `N1` ve `N2` bir üyeyi içeren `A`ve çünkü `N3` her ikisi de başvuru alır `A` içinde `N3` bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="de958-205">both `N1` and `N2` contain a member `A`, and because `N3` imports both, referencing `A` in `N3` is a compile-time error.</span></span> <span data-ttu-id="de958-206">Bu durumda, çakışma ya da Nitelik yapılan başvuruların üzerinden çözümlenebilir `A`, ya da Giriş bir *using_alias_directive* , belirli bir seçer `A`.</span><span class="sxs-lookup"><span data-stu-id="de958-206">In this situation, the conflict can be resolved either through qualification of references to `A`, or by introducing a *using_alias_directive* that picks a particular `A`.</span></span> <span data-ttu-id="de958-207">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="de958-207">For example:</span></span>
```csharp
namespace N3
{
    using N1;

    using N2;

    using A = N1.A;

    class B: A {}                // A means N1.A
}
```

<span data-ttu-id="de958-208">Ayrıca, ne zaman birden fazla ad alanı veya tür içe aktarılan tarafından *using_namespace_directive*s veya *using_static_directive*türleri veya üyeleri tarafından aynı derleme birimi veya ad alanı gövdesinde s içerir aynı ada başvuran o ada bir *simple_name* belirsiz olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="de958-208">Furthermore, when more than one namespace or type imported by *using_namespace_directive*s or *using_static_directive*s in the same compilation unit or namespace body contain types or members by the same name, references to that name as a *simple_name* are considered ambiguous.</span></span> <span data-ttu-id="de958-209">Örnekte</span><span class="sxs-lookup"><span data-stu-id="de958-209">In the example</span></span>
```csharp
namespace N1
{
    class A {}
}

class C
{
    public static int A;
}

namespace N2
{
    using N1;
    using static C;

    class B
    {
        void M() 
        { 
            A a = new A();   // Ok, A is unambiguous as a type-name
            A.Equals(2);     // Error, A is ambiguous as a simple-name
        }
    }
}
```
<span data-ttu-id="de958-210">`N1` bir tür üyesi içeren `A`, ve `C` içeren bir statik alan `A`ve çünkü `N2` her ikisi de başvuru alır `A` olarak bir *simple_name* belirsiz ve derleme zamanı bir hata oluştu.</span><span class="sxs-lookup"><span data-stu-id="de958-210">`N1` contains a type member `A`, and `C` contains a static field `A`, and because `N2` imports both, referencing `A` as a *simple_name* is ambiguous and a compile-time error.</span></span> 

<span data-ttu-id="de958-211">Gibi bir *using_alias_directive*, *using_namespace_directive* derleme biriminde veya ad alanı temel alınan bildirim alanına tüm yeni üyeleri katkıda değil, ancak bunun yerine yalnızca etkiler göründüğü derleme birimi veya ad alanı gövdesi.</span><span class="sxs-lookup"><span data-stu-id="de958-211">Like a *using_alias_directive*, a *using_namespace_directive* does not contribute any new members to the underlying declaration space of the compilation unit or namespace, but rather affects only the compilation unit or namespace body in which it appears.</span></span>

<span data-ttu-id="de958-212">*Namespace_name* tarafından başvurulan bir *using_namespace_directive* aynı şekilde çözümlenen *namespace_or_type_name* tarafından başvurulan bir  *using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="de958-212">The *namespace_name* referenced by a *using_namespace_directive* is resolved in the same way as the *namespace_or_type_name* referenced by a *using_alias_directive*.</span></span> <span data-ttu-id="de958-213">Bu nedenle, *using_namespace_directive*s aynı derleme birimi veya ad alanı gövdesinde birbirine etkilemez ve herhangi bir sırada yazılabilir.</span><span class="sxs-lookup"><span data-stu-id="de958-213">Thus, *using_namespace_directive*s in the same compilation unit or namespace body do not affect each other and can be written in any order.</span></span>

### <a name="using-static-directives"></a><span data-ttu-id="de958-214">Statik yönergeleri kullanma</span><span class="sxs-lookup"><span data-stu-id="de958-214">Using static directives</span></span>

<span data-ttu-id="de958-215">A *using_static_directive* her üyesi ve tür tanımlayıcısı etkinleştirme statik üyelerini doğrudan gövdesine kapsayan derleme birimi veya ad alanı, tür bildiriminde bulunan ve iç içe geçmiş türleri içeri aktarır niteleme olmadan kullanılır.</span><span class="sxs-lookup"><span data-stu-id="de958-215">A *using_static_directive* imports the nested types and static members contained directly in a type declaration into the immediately enclosing compilation unit or namespace body, enabling the identifier of each member and type to be used without qualification.</span></span>

```antlr
using_static_directive
    : 'using' 'static' type_name ';'
    ;
```

<span data-ttu-id="de958-216">Üye bildirimleri gövdesindeki içeren bir derleme birimi veya ad alanı içinde bir *using_static_directive*, erişilebilir iç içe türler ve doğrudan bildiriminde bulunan statik üyeler (hariç, genişletme yöntemleri) Belirtilen türü doğrudan başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="de958-216">Within member declarations in a compilation unit or namespace body that contains a *using_static_directive*, the accessible nested types and static members (except extension methods) contained directly in the declaration of the given type can be referenced directly.</span></span> <span data-ttu-id="de958-217">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="de958-217">For example:</span></span>

```csharp
namespace N1
{
    class A 
    {
        public class B{}
        public static B M(){ return new B(); }
    }
}

namespace N2
{
    using static N1.A;
    class C
    {
        void N() { B b = M(); }
    }
}
```

<span data-ttu-id="de958-218">Yukarıdaki üye bildirimlerinde içinde `N2` ad alanı, iç içe geçmiş türleri ve statik üyeler `N1.A` doğrudan kullanılabilir ve bu nedenle yöntemi `N` her ikisi de başvuru bulabildiği `B` ve `M` üyeleri`N1.A`.</span><span class="sxs-lookup"><span data-stu-id="de958-218">Above, within member declarations in the `N2` namespace, the static members and nested types of `N1.A` are directly available, and thus the method `N` is able to reference both the `B` and `M` members of `N1.A`.</span></span>

<span data-ttu-id="de958-219">A *using_static_directive* özellikle genişletme yöntemleri statik yöntemler olarak doğrudan içeri aktarmaz, ancak uzantı metodu çağırma için kullanılabilir hale getirir ([uzantısı yöntem çağrıları](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="de958-219">A *using_static_directive* specifically does not import extension methods directly as static methods, but makes them available for extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="de958-220">Örnekte</span><span class="sxs-lookup"><span data-stu-id="de958-220">In the example</span></span>

```csharp
namespace N1 
{
    static class A 
    {
        public static void M(this string s){}
    }
}

namespace N2
{
    using static N1.A;

    class B
    {
        void N() 
        {
            M("A");      // Error, M unknown
            "B".M();     // Ok, M known as extension method
            N1.A.M("C"); // Ok, fully qualified
        }
    }
}
```
<span data-ttu-id="de958-221">*using_static_directive* genişletme yöntemini alır `M` bulunan `N1.A`, ancak yalnızca bir genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="de958-221">the *using_static_directive* imports the extension method `M` contained in `N1.A`, but only as an extension method.</span></span> <span data-ttu-id="de958-222">Bu nedenle, ilk başvuru `M` gövdesinde `B.N` üye yok adlı bir derleme zamanı hatası oluşur `M` kapsam içindedir.</span><span class="sxs-lookup"><span data-stu-id="de958-222">Thus, the first reference to `M` in the body of `B.N` results in a compile-time error because no members named `M` are in scope.</span></span>

<span data-ttu-id="de958-223">A *using_static_directive* üyeleri ve türleri temel sınıflarda bildirilen üyeler ve doğrudan belirli türü bildirilmiş türleri yalnızca içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="de958-223">A *using_static_directive* only imports members and types declared directly in the given type, not members and types declared in base classes.</span></span>

<span data-ttu-id="de958-224">TODO: Örnek</span><span class="sxs-lookup"><span data-stu-id="de958-224">TODO: Example</span></span>

<span data-ttu-id="de958-225">Birden çok arasında belirsizlikleri *using_namespace_directives* ve *using_static_directives* ele alınmıştır [ad alanı yönergeleri kullanarak](namespaces.md#using-namespace-directives).</span><span class="sxs-lookup"><span data-stu-id="de958-225">Ambiguities between multiple *using_namespace_directives* and *using_static_directives* are discussed in [Using namespace directives](namespaces.md#using-namespace-directives).</span></span>

## <a name="namespace-members"></a><span data-ttu-id="de958-226">Namespace üyelerini</span><span class="sxs-lookup"><span data-stu-id="de958-226">Namespace members</span></span>

<span data-ttu-id="de958-227">A *namespace_member_declaration* ya da bir *namespace_declaration* ([Namespace bildirimi](namespaces.md#namespace-declarations)) veya bir *type_declaration* () [Tür bildirimleri](namespaces.md#type-declarations)).</span><span class="sxs-lookup"><span data-stu-id="de958-227">A *namespace_member_declaration* is either a *namespace_declaration* ([Namespace declarations](namespaces.md#namespace-declarations)) or a *type_declaration* ([Type declarations](namespaces.md#type-declarations)).</span></span>

```antlr
namespace_member_declaration
    : namespace_declaration
    | type_declaration
    ;
```

<span data-ttu-id="de958-228">Bir derleme biriminde veya ad alanı gövdesi içerebilir *namespace_member_declaration*s ve bu tür bildirimleri katkıda yeni üye temel alınan bildirim alanına içeren derleme birimi veya ad alanı gövdesi.</span><span class="sxs-lookup"><span data-stu-id="de958-228">A compilation unit or a namespace body can contain *namespace_member_declaration*s, and such declarations contribute new members to the underlying declaration space of the containing compilation unit or namespace body.</span></span>

## <a name="type-declarations"></a><span data-ttu-id="de958-229">Tür bildirimleri</span><span class="sxs-lookup"><span data-stu-id="de958-229">Type declarations</span></span>

<span data-ttu-id="de958-230">A *type_declaration* olduğu bir *class_declaration* ([sınıf bildirimleri](classes.md#class-declarations)), *struct_declaration* ([yapısı bildirimleri](structs.md#struct-declarations)), bir *interface_declaration* ([arabirim bildirimleri](interfaces.md#interface-declarations)), bir *enum_declaration* ([sabit listesi bildirimleri](enums.md#enum-declarations)), veya bir *delegate_declaration* ([temsilci bildirimi](delegates.md#delegate-declarations)).</span><span class="sxs-lookup"><span data-stu-id="de958-230">A *type_declaration* is a *class_declaration* ([Class declarations](classes.md#class-declarations)), a *struct_declaration* ([Struct declarations](structs.md#struct-declarations)), an *interface_declaration* ([Interface declarations](interfaces.md#interface-declarations)), an *enum_declaration* ([Enum declarations](enums.md#enum-declarations)), or a *delegate_declaration* ([Delegate declarations](delegates.md#delegate-declarations)).</span></span>

```antlr
type_declaration
    : class_declaration
    | struct_declaration
    | interface_declaration
    | enum_declaration
    | delegate_declaration
    ;
```

<span data-ttu-id="de958-231">A *type_declaration* derleme biriminde bir üst düzey bildiriminde veya olarak bir üye bildirimi bir ad alanı, sınıf veya yapı içinde ortaya çıkabilir.</span><span class="sxs-lookup"><span data-stu-id="de958-231">A *type_declaration* can occur as a top-level declaration in a compilation unit or as a member declaration within a namespace, class, or struct.</span></span>

<span data-ttu-id="de958-232">Bir tür için bir tür bildirimi olduğunda `T` gerçekleşir bir derleme biriminde bir üst düzey bildiriminde olarak yeni bildirilen türü tam adı olduğunu yalnızca `T`.</span><span class="sxs-lookup"><span data-stu-id="de958-232">When a type declaration for a type `T` occurs as a top-level declaration in a compilation unit, the fully qualified name of the newly declared type is simply `T`.</span></span> <span data-ttu-id="de958-233">Bir tür için bir tür bildirimi olduğunda `T` sınıf veya yapının yeni bildirilen türü tam adı ad alanı içinde gerçekleşir `N.T`burada `N` içeren ad alanı, sınıf veya yapı tam adıdır.</span><span class="sxs-lookup"><span data-stu-id="de958-233">When a type declaration for a type `T` occurs within a namespace, class, or struct, the fully qualified name of the newly declared type is `N.T`, where `N` is the fully qualified name of the containing namespace, class, or struct.</span></span>

<span data-ttu-id="de958-234">Bir tür bir sınıf içinde bildirilen veya yapı, iç içe geçmiş bir tür çağrılır ([türlerin](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="de958-234">A type declared within a class or struct is called a nested type ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="de958-235">İzin verilen erişim değiştiricileri ve varsayılan erişim türü bildirimi için bildirimi yer alan bağlam üzerinde bağlıdır ([erişilebilirlik bildirilen](basic-concepts.md#declared-accessibility)):</span><span class="sxs-lookup"><span data-stu-id="de958-235">The permitted access modifiers and the default access for a type declaration depend on the context in which the declaration takes place ([Declared accessibility](basic-concepts.md#declared-accessibility)):</span></span>

*  <span data-ttu-id="de958-236">Türleri derleme biriminden veya ad alanı bildirimi olabilir `public` veya `internal` erişim.</span><span class="sxs-lookup"><span data-stu-id="de958-236">Types declared in compilation units or namespaces can have `public` or `internal` access.</span></span> <span data-ttu-id="de958-237">Varsayılan değer `internal` erişim.</span><span class="sxs-lookup"><span data-stu-id="de958-237">The default is `internal` access.</span></span>
*  <span data-ttu-id="de958-238">Sınıflarda bildirilen türleri olabilir `public`, `protected internal`, `protected`, `internal`, veya `private` erişim.</span><span class="sxs-lookup"><span data-stu-id="de958-238">Types declared in classes can have `public`, `protected internal`, `protected`, `internal`, or `private` access.</span></span> <span data-ttu-id="de958-239">Varsayılan değer `private` erişim.</span><span class="sxs-lookup"><span data-stu-id="de958-239">The default is `private` access.</span></span>
*  <span data-ttu-id="de958-240">Yapılar için bildirilen türleri olabilir `public`, `internal`, veya `private` erişim.</span><span class="sxs-lookup"><span data-stu-id="de958-240">Types declared in structs can have `public`, `internal`, or `private` access.</span></span> <span data-ttu-id="de958-241">Varsayılan değer `private` erişim.</span><span class="sxs-lookup"><span data-stu-id="de958-241">The default is `private` access.</span></span>

## <a name="namespace-alias-qualifiers"></a><span data-ttu-id="de958-242">Namespace diğer adı niteleyicileri</span><span class="sxs-lookup"><span data-stu-id="de958-242">Namespace alias qualifiers</span></span>

<span data-ttu-id="de958-243">***Ad alanı diğer ad niteleyicisi*** `::` tür adı aramaları yeni türler ve üyeler sunulmasıyla tarafından etkilenmez garanti mümkün kılar.</span><span class="sxs-lookup"><span data-stu-id="de958-243">The ***namespace alias qualifier*** `::` makes it possible to guarantee that type name lookups are unaffected by the introduction of new types and members.</span></span> <span data-ttu-id="de958-244">Ad alanı diğer ad niteleyicisi her zaman sol ve sağ tanımlayıcı olarak başvurulan iki tanımlayıcı arasında görünür.</span><span class="sxs-lookup"><span data-stu-id="de958-244">The namespace alias qualifier always appears between two identifiers referred to as the left-hand and right-hand identifiers.</span></span> <span data-ttu-id="de958-245">Normal aksine `.` niteleyicisi, sol taraftaki tanımlayıcısını `::` niteleyicisi Aranan up yalnızca extern veya diğer ad kullanımı.</span><span class="sxs-lookup"><span data-stu-id="de958-245">Unlike the regular `.` qualifier, the left-hand identifier of the `::` qualifier is looked up only as an extern or using alias.</span></span>

<span data-ttu-id="de958-246">A *qualified_alias_member* şu şekilde tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="de958-246">A *qualified_alias_member* is defined as follows:</span></span>

```antlr
qualified_alias_member
    : identifier '::' identifier type_argument_list?
    ;
```

<span data-ttu-id="de958-247">A *qualified_alias_member* olarak kullanılabilir bir *namespace_or_type_name* ([Namespace ve tür adları](basic-concepts.md#namespace-and-type-names)) veya sol işlenen olarak bir *member_access* ([Üye erişimi](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="de958-247">A *qualified_alias_member* can be used as a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) or as the left operand in a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="de958-248">A *qualified_alias_member* iki farklı vardır:</span><span class="sxs-lookup"><span data-stu-id="de958-248">A *qualified_alias_member* has one of two forms:</span></span>

*  <span data-ttu-id="de958-249">`N::I<A1, ..., Ak>`, burada `N` ve `I` tanımlayıcılarını temsil eder ve `<A1, ..., Ak>` türü bağımsız değişken listesi.</span><span class="sxs-lookup"><span data-stu-id="de958-249">`N::I<A1, ..., Ak>`, where `N` and `I` represent identifiers, and `<A1, ..., Ak>` is a type argument list.</span></span> <span data-ttu-id="de958-250">(`K` her zaman en az bir olur.)</span><span class="sxs-lookup"><span data-stu-id="de958-250">(`K` is always at least one.)</span></span>
*  <span data-ttu-id="de958-251">`N::I`, burada `N` ve `I` tanımlayıcılarını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="de958-251">`N::I`, where `N` and `I` represent identifiers.</span></span> <span data-ttu-id="de958-252">(Bu durumda, `K` sıfır olarak kabul edilir.)</span><span class="sxs-lookup"><span data-stu-id="de958-252">(In this case, `K` is considered to be zero.)</span></span>

<span data-ttu-id="de958-253">Bu gösterim anlamını kullanılarak bir *qualified_alias_member* şu şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="de958-253">Using this notation, the meaning of a *qualified_alias_member* is determined as follows:</span></span>

*  <span data-ttu-id="de958-254">Varsa `N` tanımlayıcı `global`, sonra da için genel ad aranır `I`:</span><span class="sxs-lookup"><span data-stu-id="de958-254">If `N` is the identifier `global`, then the global namespace is searched for `I`:</span></span>
   * <span data-ttu-id="de958-255">Genel ad alanı adındaki bir ad içeriyorsa `I` ve `K` sıfırsa, ardından *qualified_alias_member* bu isim uzayına başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="de958-255">If the global namespace contains a namespace named `I` and `K` is zero, then the *qualified_alias_member* refers to that namespace.</span></span>
   * <span data-ttu-id="de958-256">Aksi durumda, genel olmayan bir tür adı genel ad içeriyorsa `I` ve `K` sıfırsa, ardından *qualified_alias_member* bu türe başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="de958-256">Otherwise, if the global namespace contains a non-generic type named `I` and `K` is zero, then the *qualified_alias_member* refers to that type.</span></span>
   * <span data-ttu-id="de958-257">Aksi takdirde genel ad adlı bir tür içeriyorsa `I` olan `K`  tür parametrelerindeki, ardından *qualified_alias_member* belirli tür bağımsız değişkenleri ile oluşturulan türü ifade eder.</span><span class="sxs-lookup"><span data-stu-id="de958-257">Otherwise, if the global namespace contains a type named `I` that has `K` type parameters, then the *qualified_alias_member* refers to that type constructed with the given type arguments.</span></span>
   * <span data-ttu-id="de958-258">Aksi takdirde, *qualified_alias_member* olup tanımsız ve bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="de958-258">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>

*  <span data-ttu-id="de958-259">Aksi takdirde, ad alanı bildirimi ile başlayan ([Namespace bildirimi](namespaces.md#namespace-declarations)) hemen içeren *qualified_alias_member* (varsa), her kapsayan ad alanı bildirimi ile devam ediliyor (varsa) ve içeren derleme birimi ile biten *qualified_alias_member*, aşağıdaki adımları bir varlığı bulunana kadar değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="de958-259">Otherwise, starting with the namespace declaration ([Namespace declarations](namespaces.md#namespace-declarations)) immediately containing the *qualified_alias_member* (if any), continuing with each enclosing namespace declaration (if any), and ending with the compilation unit containing the *qualified_alias_member*, the following steps are evaluated until an entity is located:</span></span>

   * <span data-ttu-id="de958-260">Ad alanı bildirimi veya derleme birimi içeriyorsa, bir *using_alias_directive* , ilişkilendirir `N` bir türle sonra *qualified_alias_member* tanımlı değildir ve derleme zamanı hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="de958-260">If the namespace declaration or compilation unit contains a *using_alias_directive* that associates `N` with a type, then the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>
   * <span data-ttu-id="de958-261">Aksi takdirde ad alanı bildirimi veya derleme biriminin içeriyorsa bir *extern_alias_directive* veya *using_alias_directive* , ilişkilendirir `N` bir ad alanı, ardından:</span><span class="sxs-lookup"><span data-stu-id="de958-261">Otherwise, if the namespace declaration or compilation unit contains an *extern_alias_directive* or *using_alias_directive* that associates `N` with a namespace, then:</span></span>
     * <span data-ttu-id="de958-262">Ad alanı ile ilişkili değilse `N` adlı bir ad alanı içerir `I` ve `K` sıfırsa, ardından *qualified_alias_member* bu isim uzayına başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="de958-262">If the namespace associated with `N` contains a namespace named `I` and `K` is zero, then the *qualified_alias_member* refers to that namespace.</span></span>
     * <span data-ttu-id="de958-263">Aksi takdirde, ad alanı ile ilişkili `N` adlı bir genel olmayan tür içeren `I` ve `K` sıfırsa, ardından *qualified_alias_member* bu türe başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="de958-263">Otherwise, if the namespace associated with `N` contains a non-generic type named `I` and `K` is zero, then the *qualified_alias_member* refers to that type.</span></span>
     * <span data-ttu-id="de958-264">Aksi takdirde, ad alanı ile ilişkili `N` adlı bir tür içeren `I` olan `K`  tür parametrelerindeki, ardından *qualified_alias_member* türü ile oluşturulan ifade eder Belirtilen tür bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="de958-264">Otherwise, if the namespace associated with `N` contains a type named `I` that has `K` type parameters, then the *qualified_alias_member* refers to that type constructed with the given type arguments.</span></span>
     * <span data-ttu-id="de958-265">Aksi takdirde, *qualified_alias_member* olup tanımsız ve bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="de958-265">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="de958-266">Aksi takdirde, *qualified_alias_member* olup tanımsız ve bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="de958-266">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>

<span data-ttu-id="de958-267">Ad alanı diğer ad niteleyicisi bir türe başvuran bir diğer ad ile kullanarak bir derleme zamanı hatası neden olacağını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="de958-267">Note that using the namespace alias qualifier with an alias that references a type causes a compile-time error.</span></span> <span data-ttu-id="de958-268">Ayrıca tanımlayıcı unutmayın `N` olduğu `global`, olsa bile bir diğer ad ilişkilendirme kullanarak genel ad alanında arama gerçekleştirilir `global` türü veya ad alanı.</span><span class="sxs-lookup"><span data-stu-id="de958-268">Also note that if the identifier `N` is `global`, then lookup is performed in the global namespace, even if there is a using alias associating `global` with a type or namespace.</span></span>

### <a name="uniqueness-of-aliases"></a><span data-ttu-id="de958-269">Diğer ad benzersizliğini</span><span class="sxs-lookup"><span data-stu-id="de958-269">Uniqueness of aliases</span></span>

<span data-ttu-id="de958-270">Extern diğer adlar için ayrı bildirim alanı her derleme birimi ve ad alanı gövdesi olan ve diğer adların kullanımı.</span><span class="sxs-lookup"><span data-stu-id="de958-270">Each compilation unit and namespace body has a separate declaration space for extern aliases and using aliases.</span></span> <span data-ttu-id="de958-271">Bu nedenle, bir extern diğer adı veya diğer ad kullanımı adını extern diğer adlar kümesi içinde benzersiz olmalıdır ve hemen içeren derleme birimi veya ad alanı gövdesinde bildirilen diğer adların kullanımı bir diğer ad bir tür veya ad alanı olduğu sürece aynı ada sahip izin verilen ediyorum Yalnızca kullanılan t `::` niteleyicisi.</span><span class="sxs-lookup"><span data-stu-id="de958-271">Thus, while the name of an extern alias or using alias must be unique within the set of extern aliases and using aliases declared in the immediately containing compilation unit or namespace body, an alias is permitted to have the same name as a type or namespace as long as it is used only with the `::` qualifier.</span></span>

<span data-ttu-id="de958-272">Örnekte</span><span class="sxs-lookup"><span data-stu-id="de958-272">In the example</span></span>
```csharp
namespace N
{
    public class A {}

    public class B {}
}

namespace N
{
    using A = System.IO;

    class X
    {
        A.Stream s1;            // Error, A is ambiguous

        A::Stream s2;           // Ok
    }
}
```
<span data-ttu-id="de958-273">adı `A` iki olası anlama sahip ikinci bir ad alanı gövdesinde olduğundan her iki sınıf `A` ve kullanarak diğer ad `A` kapsamındaki.</span><span class="sxs-lookup"><span data-stu-id="de958-273">the name `A` has two possible meanings in the second namespace body because both the class `A` and the using alias `A` are in scope.</span></span> <span data-ttu-id="de958-274">Bu nedenle, kullanım `A` tam adda `A.Stream` belirsiz ve bir derleme zamanı hatası oluşmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="de958-274">For this reason, use of `A` in the qualified name `A.Stream` is ambiguous and causes a compile-time error to occur.</span></span> <span data-ttu-id="de958-275">Ancak, kullanım `A` ile `::` niteleyicisi olmayan bir hata nedeniyle `A` yalnızca bir ad alanı diğer adı aranır.</span><span class="sxs-lookup"><span data-stu-id="de958-275">However, use of `A` with the `::` qualifier is not an error because `A` is looked up only as a namespace alias.</span></span>
