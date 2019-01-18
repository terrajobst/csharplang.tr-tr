---
ms.openlocfilehash: 0a09585f4f885647230354c66a2449abb7ef1f44
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229962"
---
# <a name="interfaces"></a><span data-ttu-id="794ed-101">Arabirimler</span><span class="sxs-lookup"><span data-stu-id="794ed-101">Interfaces</span></span>

<span data-ttu-id="794ed-102">Bir arabirim bir sözleşmeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="794ed-102">An interface defines a contract.</span></span> <span data-ttu-id="794ed-103">Bir sınıf ya da bir arabirimi uygulayan yapı bu sözleşmeye uymalıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-103">A class or struct that implements an interface must adhere to its contract.</span></span> <span data-ttu-id="794ed-104">Birden fazla temel Ara birimden arabirim devralabilir ve bir sınıf veya yapı birden fazla arabirim uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="794ed-104">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="794ed-105">Arabirim yöntemleri, özellikleri, olayları ve dizin oluşturucular içerebilir.</span><span class="sxs-lookup"><span data-stu-id="794ed-105">Interfaces can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="794ed-106">Arabirim, kendisini tanımlayan üyeleri için uygulamaları sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="794ed-106">The interface itself does not provide implementations for the members that it defines.</span></span> <span data-ttu-id="794ed-107">Arabirimi yalnızca sınıf ya da arabirimi uygulayan yapının tarafından sağlanması gereken üyeleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="794ed-107">The interface merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

## <a name="interface-declarations"></a><span data-ttu-id="794ed-108">Arabirim bildirimi</span><span class="sxs-lookup"><span data-stu-id="794ed-108">Interface declarations</span></span>

<span data-ttu-id="794ed-109">Bir *interface_declaration* olduğu bir *type_declaration* ([tür bildirimleri](namespaces.md#type-declarations)), yeni bir arabirim türü bildirir.</span><span class="sxs-lookup"><span data-stu-id="794ed-109">An *interface_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new interface type.</span></span>

```antlr
interface_declaration
    : attributes? interface_modifier* 'partial'? 'interface'
      identifier variant_type_parameter_list? interface_base?
      type_parameter_constraints_clause* interface_body ';'?
    ;
```

<span data-ttu-id="794ed-110">Bir *interface_declaration* isteğe bağlı bir kümesinden oluşur *öznitelikleri* ([öznitelikleri](attributes.md)) ve ardından isteğe bağlı bir dizi *interface_modifier*s ([arabirim değiştiriciler](interfaces.md#interface-modifiers)) ve ardından isteğe bağlı `partial` değiştiricisi anahtar sözcüğü ve ardından, `interface` ve *tanımlayıcı* arabirimi adları İsteğe bağlı izlenen *variant_type_parameter_list* belirtimi ([değişken türü parametre listeleri](interfaces.md#variant-type-parameter-lists)) ve ardından isteğe bağlı *interface_base* belirtimi ([temel arabirimleri](interfaces.md#base-interfaces)) ve ardından isteğe bağlı *type_parameter_constraints_clause*s belirtimi ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) , ardından bir *interface_body* ([arabirim gövdesi](interfaces.md#interface-body)), noktalı virgül tarafından izlenen isteğe bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="794ed-110">An *interface_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *interface_modifier*s ([Interface modifiers](interfaces.md#interface-modifiers)), followed by an optional `partial` modifier, followed by the keyword `interface` and an *identifier* that names the interface, followed by an optional *variant_type_parameter_list* specification ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)), followed by an optional *interface_base* specification ([Base interfaces](interfaces.md#base-interfaces)), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by an *interface_body* ([Interface body](interfaces.md#interface-body)), optionally followed by a semicolon.</span></span>

### <a name="interface-modifiers"></a><span data-ttu-id="794ed-111">Değiştiriciler arabirimi</span><span class="sxs-lookup"><span data-stu-id="794ed-111">Interface modifiers</span></span>

<span data-ttu-id="794ed-112">Bir *interface_declaration* isteğe bağlı olarak bir dizi arabirimi değiştiriciler içerebilir:</span><span class="sxs-lookup"><span data-stu-id="794ed-112">An *interface_declaration* may optionally include a sequence of interface modifiers:</span></span>

```antlr
interface_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | interface_modifier_unsafe
    ;
```

<span data-ttu-id="794ed-113">Aynı değiştiricisi arabirim bildirimiyle birden çok kez görünmesi için bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-113">It is a compile-time error for the same modifier to appear multiple times in an interface declaration.</span></span>

<span data-ttu-id="794ed-114">`new` Değiştiricisi bir sınıf içinde tanımlanmış arabirimlere yalnızca verilir.</span><span class="sxs-lookup"><span data-stu-id="794ed-114">The `new` modifier is only permitted on interfaces defined within a class.</span></span> <span data-ttu-id="794ed-115">Bölümünde anlatıldığı gibi arabirimi aynı ada göre devralınmış bir üyeyi gizler belirtir [yeni değiştiricisini](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="794ed-115">It specifies that the interface hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="794ed-116">`public`, `protected`, `internal`, Ve `private` değiştiriciler arabirimi erişilebilirliğini denetim.</span><span class="sxs-lookup"><span data-stu-id="794ed-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the interface.</span></span> <span data-ttu-id="794ed-117">Arabirim bildirimi oluştuğu bağlama bağlı olarak bu değiştiriciler yalnızca bazıları izin verilir ([erişilebilirlik bildirilen](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="794ed-117">Depending on the context in which the interface declaration occurs, only some of these modifiers may be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="794ed-118">Partial değiştiricisi</span><span class="sxs-lookup"><span data-stu-id="794ed-118">Partial modifier</span></span>

<span data-ttu-id="794ed-119">`partial` Değiştiricisi gösteren bu *interface_declaration* kısmi türü bildirimi.</span><span class="sxs-lookup"><span data-stu-id="794ed-119">The `partial` modifier indicates that this *interface_declaration* is a partial type declaration.</span></span> <span data-ttu-id="794ed-120">Kapsayan ad alanı veya tür bildirimi içinde aynı ada sahip birden fazla kısmi bir arabirim bildirimi birleştirmek için forma bir arabirim bildirimi, aşağıdaki kurallar belirtilen [kısmi türlerinden](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="794ed-120">Multiple partial interface declarations with the same name within an enclosing namespace or type declaration combine to form one interface declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="variant-type-parameter-lists"></a><span data-ttu-id="794ed-121">Değişken türü parametre listeleri</span><span class="sxs-lookup"><span data-stu-id="794ed-121">Variant type parameter lists</span></span>

<span data-ttu-id="794ed-122">Değişken türü parametre listeleri yalnızca arabirim ve temsilci türlerinin üzerinde oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="794ed-122">Variant type parameter lists can only occur on interface and delegate types.</span></span> <span data-ttu-id="794ed-123">Sıradan fark *type_parameter_list*s'dir isteğe bağlı *variance_annotation* üzerinde her tür parametresi.</span><span class="sxs-lookup"><span data-stu-id="794ed-123">The difference from ordinary *type_parameter_list*s is the optional *variance_annotation* on each type parameter.</span></span>

```antlr
variant_type_parameter_list
    : '<' variant_type_parameters '>'
    ;

variant_type_parameters
    : attributes? variance_annotation? type_parameter
    | variant_type_parameters ',' attributes? variance_annotation? type_parameter
    ;

variance_annotation
    : 'in'
    | 'out'
    ;
```

<span data-ttu-id="794ed-124">Varyans ek açıklama olup olmadığını `out`, tür parametresi olarak kabul edilir ***birlikte değişken***.</span><span class="sxs-lookup"><span data-stu-id="794ed-124">If the variance annotation is `out`, the type parameter is said to be ***covariant***.</span></span> <span data-ttu-id="794ed-125">Varyans ek açıklama olup olmadığını `in`, tür parametresi olarak kabul edilir ***değişken karşıtı***.</span><span class="sxs-lookup"><span data-stu-id="794ed-125">If the variance annotation is `in`, the type parameter is said to be ***contravariant***.</span></span> <span data-ttu-id="794ed-126">Varyans ek açıklaması yoksa, tür parametresi olarak kabul edilir ***sabit***.</span><span class="sxs-lookup"><span data-stu-id="794ed-126">If there is no variance annotation, the type parameter is said to be ***invariant***.</span></span>

<span data-ttu-id="794ed-127">Örnekte</span><span class="sxs-lookup"><span data-stu-id="794ed-127">In the example</span></span>
```csharp
interface C<out X, in Y, Z> 
{
  X M(Y y);
  Z P { get; set; }
}
```
<span data-ttu-id="794ed-128">`X` birlikte değişen olduğundan `Y` değişken karşıtı olduğu ve `Z` değişmezdir.</span><span class="sxs-lookup"><span data-stu-id="794ed-128">`X` is covariant, `Y` is contravariant and `Z` is invariant.</span></span>

#### <a name="variance-safety"></a><span data-ttu-id="794ed-129">Varyans güvenliği</span><span class="sxs-lookup"><span data-stu-id="794ed-129">Variance safety</span></span>

<span data-ttu-id="794ed-130">Bir tür değişken açıklamalarını türü parametre listesinde oluşumunu yerleri burada türleri içinde türü bildirimi oluşabilir kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="794ed-130">The occurrence of variance annotations in the type parameter list of a type restricts the places where types can occur within the type declaration.</span></span>

<span data-ttu-id="794ed-131">Bir tür `T` olduğu ***güvenli olmayan çıkış*** tutuyorsa aşağıdakilerden biri:</span><span class="sxs-lookup"><span data-stu-id="794ed-131">A type `T` is ***output-unsafe*** if one of the following holds:</span></span>

*  <span data-ttu-id="794ed-132">`T` değişken karşıtı parametre türüdür</span><span class="sxs-lookup"><span data-stu-id="794ed-132">`T` is a contravariant type parameter</span></span>
*  <span data-ttu-id="794ed-133">`T` Çıkış güvenli olmayan öğe türüne sahip bir dizi türü</span><span class="sxs-lookup"><span data-stu-id="794ed-133">`T` is an array type with an output-unsafe element type</span></span>
*  <span data-ttu-id="794ed-134">`T` bir arabirim veya temsilci türü `S<A1,...,Ak>` genel türünden oluşturulduğu `S<X1,...,Xk>` için en az bir where `Ai` aşağıdakilerden birini içerir:</span><span class="sxs-lookup"><span data-stu-id="794ed-134">`T` is an interface or delegate type `S<A1,...,Ak>` constructed from a generic type `S<X1,...,Xk>` where for at least one `Ai` one of the following holds:</span></span>
   * <span data-ttu-id="794ed-135">`Xi` birlikte değişken veya sabit ve `Ai` çıkış güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="794ed-135">`Xi` is covariant or invariant and `Ai` is output-unsafe.</span></span>
   * <span data-ttu-id="794ed-136">`Xi` değişken karşıtı veya sabit ve `Ai` giriş güvenlidir.</span><span class="sxs-lookup"><span data-stu-id="794ed-136">`Xi` is contravariant or invariant and `Ai` is input-safe.</span></span>
   
<span data-ttu-id="794ed-137">Bir tür `T` olduğu ***güvenli olmayan giriş*** tutuyorsa aşağıdakilerden biri:</span><span class="sxs-lookup"><span data-stu-id="794ed-137">A type `T` is ***input-unsafe*** if one of the following holds:</span></span>

*  <span data-ttu-id="794ed-138">`T` birlikte değişen türde parametresi</span><span class="sxs-lookup"><span data-stu-id="794ed-138">`T` is a covariant type parameter</span></span>
*  <span data-ttu-id="794ed-139">`T` Giriş güvenli olmayan öğe türüne sahip bir dizi türü</span><span class="sxs-lookup"><span data-stu-id="794ed-139">`T` is an array type with an input-unsafe element type</span></span>
*  <span data-ttu-id="794ed-140">`T` bir arabirim veya temsilci türü `S<A1,...,Ak>` genel türünden oluşturulduğu `S<X1,...,Xk>` için en az bir where `Ai` aşağıdakilerden birini içerir:</span><span class="sxs-lookup"><span data-stu-id="794ed-140">`T` is an interface or delegate type `S<A1,...,Ak>` constructed from a generic type `S<X1,...,Xk>` where for at least one `Ai` one of the following holds:</span></span>
   * <span data-ttu-id="794ed-141">`Xi` birlikte değişken veya sabit ve `Ai` giriş güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="794ed-141">`Xi` is covariant or invariant and `Ai` is input-unsafe.</span></span>
   * <span data-ttu-id="794ed-142">`Xi` değişken karşıtı veya sabit ve `Ai` çıkış güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="794ed-142">`Xi` is contravariant or invariant and `Ai` is output-unsafe.</span></span>

<span data-ttu-id="794ed-143">Sezgisel, çıkış güvenli olmayan bir tür bir çıkış konumunda yasaktır ve giriş güvenli olmayan bir tür Giriş bir konumda kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="794ed-143">Intuitively, an output-unsafe type is prohibited in an output position, and an input-unsafe type is prohibited in an input position.</span></span>

<span data-ttu-id="794ed-144">Bir tür ***çıkış güvenli*** çıkış güvenli, değilse ve ***giriş güvenli*** giriş güvenli değilse.</span><span class="sxs-lookup"><span data-stu-id="794ed-144">A type is ***output-safe*** if it is not output-unsafe, and ***input-safe*** if it is not input-unsafe.</span></span>

#### <a name="variance-conversion"></a><span data-ttu-id="794ed-145">Varyans dönüştürme</span><span class="sxs-lookup"><span data-stu-id="794ed-145">Variance conversion</span></span>

<span data-ttu-id="794ed-146">(Ancak yine de tür bakımından güvenli) daha esnek yapılan dönüştürmeler için arabirim ve temsilci türlerinin değişken açıklamalarını amacı sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="794ed-146">The purpose of variance annotations is to provide for more lenient (but still type safe) conversions to interface and delegate types.</span></span> <span data-ttu-id="794ed-147">Bunun için örtük tanımlarını bitiş ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) ve açık dönüştürmeler ([açık dönüştürmeler](conversions.md#explicit-conversions)) olun şu şekilde tanımlanır varyansı-convertibility kavramı kullanın:</span><span class="sxs-lookup"><span data-stu-id="794ed-147">To this end the definitions of implicit ([Implicit conversions](conversions.md#implicit-conversions)) and explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) make use of the notion of variance-convertibility, which is defined as follows:</span></span>

<span data-ttu-id="794ed-148">Bir tür `T<A1,...,An>` varyansı dönüştürülebilir olduğundan türüne `T<B1,...,Bn>` varsa `T` arabirim ya da bir temsilci türü değişken türünde parametreleri bildirilmiş `T<X1,...,Xn>`ve her değişken türünde bir parametre için `Xi` aşağıdakilerden birini Tutar:</span><span class="sxs-lookup"><span data-stu-id="794ed-148">A type `T<A1,...,An>` is variance-convertible to a type `T<B1,...,Bn>` if `T` is either an interface or a delegate type declared with the variant type parameters `T<X1,...,Xn>`, and for each variant type parameter `Xi` one of the following holds:</span></span>

*  <span data-ttu-id="794ed-149">`Xi` birlikte değişkendir ve gelen örtük bir başvuru veya kimlik dönüştürme var `Ai` için `Bi`</span><span class="sxs-lookup"><span data-stu-id="794ed-149">`Xi` is covariant and an implicit reference or identity conversion exists from `Ai` to `Bi`</span></span>
*  <span data-ttu-id="794ed-150">`Xi` değişken karşıtı ve örtük bir başvuru veya kimlik dönüştürme var gelen `Bi` için `Ai`</span><span class="sxs-lookup"><span data-stu-id="794ed-150">`Xi` is contravariant and an implicit reference or identity conversion exists from `Bi` to `Ai`</span></span>
*  <span data-ttu-id="794ed-151">`Xi` değişmez değer ve bir kimlik dönüştürme var gelen `Ai` için `Bi`</span><span class="sxs-lookup"><span data-stu-id="794ed-151">`Xi` is invariant and an identity conversion exists from `Ai` to `Bi`</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="794ed-152">Temel arabirimleri</span><span class="sxs-lookup"><span data-stu-id="794ed-152">Base interfaces</span></span>

<span data-ttu-id="794ed-153">Bir arabirim olarak adlandırılan sıfır veya daha fazla arabirim türünden devralınabilir ***açık temel arabirimleri*** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="794ed-153">An interface can inherit from zero or more interface types, which are called the ***explicit base interfaces*** of the interface.</span></span> <span data-ttu-id="794ed-154">Bir arabirim bir veya daha fazla temel arabirimde açık olduğunda, ardından o arabirimin bildiriminde arabirim tanımlayıcısı bir iki nokta üst üste ve virgül gelir ayrılmış temel arabirim türleri listesi.</span><span class="sxs-lookup"><span data-stu-id="794ed-154">When an interface has one or more explicit base interfaces, then in the declaration of that interface, the interface identifier is followed by a colon and a comma separated list of base interface types.</span></span>

```antlr
interface_base
    : ':' interface_type_list
    ;
```

<span data-ttu-id="794ed-155">Genel tür bildiriminde taban açık arabirim bildirimi alma ve değiştirme, her biri için oluşturulmuş bir oluşturulmuş arabirim türü için açık temel arabirimleri *type_parameter* , temel arabirim bildirimi, karşılık gelen *type_argument* oluşturulan türü.</span><span class="sxs-lookup"><span data-stu-id="794ed-155">For a constructed interface type, the explicit base interfaces are formed by taking the explicit base interface declarations on the generic type declaration, and substituting, for each *type_parameter* in the base interface declaration, the corresponding *type_argument* of the constructed type.</span></span>

<span data-ttu-id="794ed-156">Bir arabirimin açık temel arabirimleri en az arabirimi olarak olarak erişilebilir olmalıdır ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="794ed-156">The explicit base interfaces of an interface must be at least as accessible as the interface itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span> <span data-ttu-id="794ed-157">Örneğin, belirtmek için bir derleme zamanı hatası olduğu bir `private` veya `internal` arabiriminde *interface_base* , bir `public` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="794ed-157">For example, it is a compile-time error to specify a `private` or `internal` interface in the *interface_base* of a `public` interface.</span></span>

<span data-ttu-id="794ed-158">Doğrudan veya dolaylı olarak kendisinden devralmak bir arabirim için bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-158">It is a compile-time error for an interface to directly or indirectly inherit from itself.</span></span>

<span data-ttu-id="794ed-159">***Temel arabirimleri*** açık temel arabirimleri ve temel arabirimlerini bir arabirimi olan.</span><span class="sxs-lookup"><span data-stu-id="794ed-159">The ***base interfaces*** of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="794ed-160">Diğer bir deyişle, taban arabirimlerin açık temel arabirimleri, açık temel arabirimleri ve benzeri tam geçişli kapatılmasını kümesidir.</span><span class="sxs-lookup"><span data-stu-id="794ed-160">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span> <span data-ttu-id="794ed-161">Bir arabirim temel arabirimlerinden tüm üyelerini devralır.</span><span class="sxs-lookup"><span data-stu-id="794ed-161">An interface inherits all members of its base interfaces.</span></span> <span data-ttu-id="794ed-162">Örnekte</span><span class="sxs-lookup"><span data-stu-id="794ed-162">In the example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
<span data-ttu-id="794ed-163">temel arabirimleri `IComboBox` olan `IControl`, `ITextBox`, ve `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="794ed-163">the base interfaces of `IComboBox` are `IControl`, `ITextBox`, and `IListBox`.</span></span>

<span data-ttu-id="794ed-164">Diğer bir deyişle, `IComboBox` yukarıdaki arabirim üyeleri devralan `SetText` ve `SetItems` yanı `Paint`.</span><span class="sxs-lookup"><span data-stu-id="794ed-164">In other words, the `IComboBox` interface above inherits members `SetText` and `SetItems` as well as `Paint`.</span></span>

<span data-ttu-id="794ed-165">Bir arabirimin temel her arabirim çıkış güvenli olmalıdır ([varyansı güvenliği](interfaces.md#variance-safety)).</span><span class="sxs-lookup"><span data-stu-id="794ed-165">Every base interface of an interface must be output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span> <span data-ttu-id="794ed-166">Bir sınıf veya yapı ayrıca açık bir arabirimi uygulayan tüm arabiriminin temel arabirim uygular.</span><span class="sxs-lookup"><span data-stu-id="794ed-166">A class or struct that implements an interface also implicitly implements all of the interface's base interfaces.</span></span>

### <a name="interface-body"></a><span data-ttu-id="794ed-167">Gövde arabirimi</span><span class="sxs-lookup"><span data-stu-id="794ed-167">Interface body</span></span>

<span data-ttu-id="794ed-168">*İnterface_body* arabirimin üyelerini bir arabirimi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="794ed-168">The *interface_body* of an interface defines the members of the interface.</span></span>

```antlr
interface_body
    : '{' interface_member_declaration* '}'
    ;
```

## <a name="interface-members"></a><span data-ttu-id="794ed-169">Arabirim üyeleri</span><span class="sxs-lookup"><span data-stu-id="794ed-169">Interface members</span></span>

<span data-ttu-id="794ed-170">Bir arabirimin üyelerini temel Ara birimden devralınan üyeleri ve üye arabirim tarafından bildirilen.</span><span class="sxs-lookup"><span data-stu-id="794ed-170">The members of an interface are the members inherited from the base interfaces and the members declared by the interface itself.</span></span>

```antlr
interface_member_declaration
    : interface_method_declaration
    | interface_property_declaration
    | interface_event_declaration
    | interface_indexer_declaration
    ;
```

<span data-ttu-id="794ed-171">Bir arabirim bildirimi, sıfır veya daha fazla üye bildirebilir.</span><span class="sxs-lookup"><span data-stu-id="794ed-171">An interface declaration may declare zero or more members.</span></span> <span data-ttu-id="794ed-172">Yöntemleri, özellikleri, olayları ve dizin oluşturucular bir arabirim üyesi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-172">The members of an interface must be methods, properties, events, or indexers.</span></span> <span data-ttu-id="794ed-173">Bir arabirim, sabitleri, alanları, işleçleri, örnek Oluşturucular, Yıkıcılar veya türleri içeremez veya bir arabirim herhangi bir türde statik üyeler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="794ed-173">An interface cannot contain constants, fields, operators, instance constructors, destructors, or types, nor can an interface contain static members of any kind.</span></span>

<span data-ttu-id="794ed-174">Tüm arabirim üyeleri, örtük olarak genel erişime sahiptir.</span><span class="sxs-lookup"><span data-stu-id="794ed-174">All interface members implicitly have public access.</span></span> <span data-ttu-id="794ed-175">Arabirim üye bildirimleri tüm değiştiricilere dahil etmek için bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-175">It is a compile-time error for interface member declarations to include any modifiers.</span></span> <span data-ttu-id="794ed-176">Özellikle, arabirimler üye değiştiricilerini bildirilemez `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, veya `static`.</span><span class="sxs-lookup"><span data-stu-id="794ed-176">In particular, interfaces members cannot be declared with the modifiers `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, or `static`.</span></span>

<span data-ttu-id="794ed-177">Örnek</span><span class="sxs-lookup"><span data-stu-id="794ed-177">The example</span></span>
```csharp
public delegate void StringListEvent(IStringList sender);

public interface IStringList
{
    void Add(string s);
    int Count { get; }
    event StringListEvent Changed;
    string this[int index] { get; set; }
}
```
<span data-ttu-id="794ed-178">Her bir üye olası türleri içeren bir arabirim bildirir: Bir yöntem, özellik, bir olay ve dizin oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="794ed-178">declares an interface that contains one each of the possible kinds of members: A method, a property, an event, and an indexer.</span></span>

<span data-ttu-id="794ed-179">Bir *interface_declaration* yeni bir bildirim alanı oluşturur ([bildirimleri](basic-concepts.md#declarations)) ve *interface_member_declaration*hemen tarafındanbulunans*interface_declaration* bu bildirim alanına yeni üye tanıtır.</span><span class="sxs-lookup"><span data-stu-id="794ed-179">An *interface_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *interface_member_declaration*s immediately contained by the *interface_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="794ed-180">Aşağıdaki kurallar geçerli *interface_member_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="794ed-180">The following rules apply to *interface_member_declaration*s:</span></span>

*  <span data-ttu-id="794ed-181">Bir yöntemin adını, tüm özellikleri ve olayları aynı arabirimde bildirilen adlarından farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-181">The name of a method must differ from the names of all properties and events declared in the same interface.</span></span> <span data-ttu-id="794ed-182">Ayrıca, imza ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)), bir yöntem imzaları aynı arabirimde bildirilen diğer tüm yöntemlerden farklı gerekir ve aynı arabirimde bildirilen iki yöntem imzaları sahip olmayabilir, sadece onun tarafından farklı `ref` ve `out`.</span><span class="sxs-lookup"><span data-stu-id="794ed-182">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same interface, and two methods declared in the same interface may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="794ed-183">Bir özellik veya olay adı, aynı arabirimde bildirilen tüm diğer üyelerinin adları farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-183">The name of a property or event must differ from the names of all other members declared in the same interface.</span></span>
*  <span data-ttu-id="794ed-184">Bir dizin oluşturucu imzasının aynı arabirimde bildirilen tüm diğer dizin imzalarını farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-184">The signature of an indexer must differ from the signatures of all other indexers declared in the same interface.</span></span>

<span data-ttu-id="794ed-185">Devralınan üyeleri bir arabirimin özellikle değil arabirim bildirimi alanının parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-185">The inherited members of an interface are specifically not part of the declaration space of the interface.</span></span> <span data-ttu-id="794ed-186">Bu nedenle, bir arabirim, aynı ad veya imzaya sahip bir üyesi devralınmış bir üyeyi olarak bildirin izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="794ed-186">Thus, an interface is allowed to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="794ed-187">Bu durumda türetilmiş bir arabirim üyesi temel arabirim bir üyeyi gizlemek için kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="794ed-187">When this occurs, the derived interface member is said to hide the base interface member.</span></span> <span data-ttu-id="794ed-188">Devralınmış bir üyeyi gizlemek hata olarak kabul edilmez, ancak derleyici bir uyarı neden olmaz.</span><span class="sxs-lookup"><span data-stu-id="794ed-188">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="794ed-189">Bu uyarının gösterilmemesi için türetilmiş bir arabirim üyesi bildirimi içermelidir bir `new` türetilen üye temel üyeyi Gizle amaçlandığını belirtmek için değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="794ed-189">To suppress the warning, the declaration of the derived interface member must include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="794ed-190">Bu konunun ele alındığı daha ayrıntılı olarak [devralma yoluyla gizleme](basic-concepts.md#hiding-through-inheritance).</span><span class="sxs-lookup"><span data-stu-id="794ed-190">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="794ed-191">Varsa bir `new` değiştiricisi devralınan bir üyeyi gizlemek olmayan bir bildiriminde dahil, bir uyarı belirlememişse bu bağlamda verilir.</span><span class="sxs-lookup"><span data-stu-id="794ed-191">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning is issued to that effect.</span></span> <span data-ttu-id="794ed-192">Kaldırarak bu uyarı bastırılır `new` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="794ed-192">This warning is suppressed by removing the `new` modifier.</span></span>

<span data-ttu-id="794ed-193">Unutmayın sınıfı üyeleri `object` kesinlikle anlamda, herhangi bir arabirim üyesi değilseniz, ([arabirim üyeleri](interfaces.md#interface-members)).</span><span class="sxs-lookup"><span data-stu-id="794ed-193">Note that the members in class `object` are not, strictly speaking, members of any interface ([Interface members](interfaces.md#interface-members)).</span></span> <span data-ttu-id="794ed-194">Ancak, sınıf üyeleri `object` herhangi bir arabirim türüne içinde'üye araması aracılığıyla kullanılabilir ([üye araması](expressions.md#member-lookup)).</span><span class="sxs-lookup"><span data-stu-id="794ed-194">However, the members in class `object` are available via member lookup in any interface type ([Member lookup](expressions.md#member-lookup)).</span></span>

### <a name="interface-methods"></a><span data-ttu-id="794ed-195">Arabirim yöntemleri</span><span class="sxs-lookup"><span data-stu-id="794ed-195">Interface methods</span></span>

<span data-ttu-id="794ed-196">Arabirim yöntemleri kullanarak bildirilir *interface_method_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="794ed-196">Interface methods are declared using *interface_method_declaration*s:</span></span>

```antlr
interface_method_declaration
    : attributes? 'new'? return_type identifier type_parameter_list
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;
```

<span data-ttu-id="794ed-197">*Öznitelikleri*, *Döndür_tür*, *tanımlayıcı*, ve *formal_parameter_list* bir arabirim yöntemi bildirimi aynı sahip bir yöntem bildiriminde bir sınıf olarak anlamına gelir ([yöntemleri](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="794ed-197">The *attributes*, *return_type*, *identifier*, and *formal_parameter_list* of an interface method declaration have the same meaning as those of a method declaration in a class ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="794ed-198">Bir yöntem gövdesi belirtmek için bir arabirim yöntemi bildiriminde izin verilmez ve bildirimi bu nedenle her zaman noktalı virgülle biter.</span><span class="sxs-lookup"><span data-stu-id="794ed-198">An interface method declaration is not permitted to specify a method body, and the declaration therefore always ends with a semicolon.</span></span>

<span data-ttu-id="794ed-199">Her biçimsel parametre türü bir arabirim yönteminin giriş güvenli olmalıdır ([varyansı güvenliği](interfaces.md#variance-safety)), ve dönüş türü olmalıdır `void` veya çıkış için güvenli.</span><span class="sxs-lookup"><span data-stu-id="794ed-199">Each formal parameter type of an interface method must be input-safe ([Variance safety](interfaces.md#variance-safety)), and the return type must be either `void` or output-safe.</span></span> <span data-ttu-id="794ed-200">Ayrıca, her sınıf türü kısıtlaması, arabirim tür kısıtlaması ve yöntemin herhangi bir tür parametresi tür parametresi kısıtlaması giriş uyumlu olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="794ed-200">Furthermore, each class type constraint, interface type constraint and type parameter constraint on any type parameter of the method must be input-safe.</span></span>

<span data-ttu-id="794ed-201">Bu kurallar herhangi bir değişken sağlamak veya değişken karşıtı kullanım arabirimin tür kullanımı uyumlu kalır.</span><span class="sxs-lookup"><span data-stu-id="794ed-201">These rules ensure that any covariant or contravariant usage of the interface remains type-safe.</span></span> <span data-ttu-id="794ed-202">Örneğin,</span><span class="sxs-lookup"><span data-stu-id="794ed-202">For example,</span></span>
```csharp
interface I<out T> { void M<U>() where U : T; }
```
<span data-ttu-id="794ed-203">geçersiz olduğundan kullanımını `T` üzerinde tür parametresi kısıtlaması olarak `U` giriş açısından güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="794ed-203">is illegal because the usage of `T` as a type parameter constraint on `U` is not input-safe.</span></span>

<span data-ttu-id="794ed-204">Bu kısıtlama yerinde değil olan aşağıdaki şekilde tür güvenliği ihlal etmek mümkün olacaktır:</span><span class="sxs-lookup"><span data-stu-id="794ed-204">Were this restriction not in place it would be possible to violate type safety in the following manner:</span></span>
```csharp
class B {}
class D : B{}
class E : B {}
class C : I<D> { public void M<U>() {...} }
...
I<B> b = new C();
b.M<E>();
```
<span data-ttu-id="794ed-205">Bu, aslında bir çağrı `C.M<E>`.</span><span class="sxs-lookup"><span data-stu-id="794ed-205">This is actually a call to `C.M<E>`.</span></span> <span data-ttu-id="794ed-206">Çağrı, gerektirir. ancak bu `E` öğesinden türetilen `D`, tür güvenliği ihlal burada.</span><span class="sxs-lookup"><span data-stu-id="794ed-206">But that call requires that `E` derive from `D`, so type safety would be violated here.</span></span>

### <a name="interface-properties"></a><span data-ttu-id="794ed-207">Arabirim özellikleri</span><span class="sxs-lookup"><span data-stu-id="794ed-207">Interface properties</span></span>

<span data-ttu-id="794ed-208">Arabirim özellikleri kullanarak bildirilir *interface_property_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="794ed-208">Interface properties are declared using *interface_property_declaration*s:</span></span>

```antlr
interface_property_declaration
    : attributes? 'new'? type identifier '{' interface_accessors '}'
    ;

interface_accessors
    : attributes? 'get' ';'
    | attributes? 'set' ';'
    | attributes? 'get' ';' attributes? 'set' ';'
    | attributes? 'set' ';' attributes? 'get' ';'
    ;
```

<span data-ttu-id="794ed-209">*Öznitelikleri*, *türü*, ve *tanımlayıcı* bir arabirimi özelliği bildirimi, bir sınıfta bir özellik bildirimi içeriğiyle aynı anlama sahip ([ Özellikleri](classes.md#properties)).</span><span class="sxs-lookup"><span data-stu-id="794ed-209">The *attributes*, *type*, and *identifier* of an interface property declaration have the same meaning as those of a property declaration in a class ([Properties](classes.md#properties)).</span></span>

<span data-ttu-id="794ed-210">Bir sınıf özelliği bildirimindeki erişimcisi için bir arabirim özelliği bildiriminde erişicilerini karşılık gelir ([erişimcileri](classes.md#accessors)) erişimcisinin gövdesi her zaman noktalı olmalı, dışında.</span><span class="sxs-lookup"><span data-stu-id="794ed-210">The accessors of an interface property declaration correspond to the accessors of a class property declaration ([Accessors](classes.md#accessors)), except that the accessor body must always be a semicolon.</span></span> <span data-ttu-id="794ed-211">Bu nedenle, erişimciler yalnızca salt okunur, salt okunur veya salt yazılır özelliği olup olmadığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="794ed-211">Thus, the accessors simply indicate whether the property is read-write, read-only, or write-only.</span></span>

<span data-ttu-id="794ed-212">Arabirim özelliği bir alma erişimcisi varsa çıktı-güvenli türünde olmalıdır ve giriş ayarlama erişimcisine ise güvenli olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-212">The type of an interface property must be output-safe if there is a get accessor, and must be input-safe if there is a set accessor.</span></span>

### <a name="interface-events"></a><span data-ttu-id="794ed-213">Arabirim olayları</span><span class="sxs-lookup"><span data-stu-id="794ed-213">Interface events</span></span>

<span data-ttu-id="794ed-214">Arabirim olaylarını kullanarak bildirilir *interface_event_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="794ed-214">Interface events are declared using *interface_event_declaration*s:</span></span>

```antlr
interface_event_declaration
    : attributes? 'new'? 'event' type identifier ';'
    ;
```

<span data-ttu-id="794ed-215">*Öznitelikleri*, *türü*, ve *tanımlayıcı* bir arabirimi olay bildirimi, bir sınıf içinde olay bildiriminde sahip aynı anlamı sahip ([olayları ](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="794ed-215">The *attributes*, *type*, and *identifier* of an interface event declaration have the same meaning as those of an event declaration in a class ([Events](classes.md#events)).</span></span>

<span data-ttu-id="794ed-216">Bir arabirim olayı türü giriş uyumlu olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="794ed-216">The type of an interface event must be input-safe.</span></span>

### <a name="interface-indexers"></a><span data-ttu-id="794ed-217">Arabirim dizin oluşturucuları</span><span class="sxs-lookup"><span data-stu-id="794ed-217">Interface indexers</span></span>

<span data-ttu-id="794ed-218">Arabirim dizin oluşturucuları kullanarak bildirilir *interface_indexer_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="794ed-218">Interface indexers are declared using *interface_indexer_declaration*s:</span></span>

```antlr
interface_indexer_declaration
    : attributes? 'new'? type 'this' '[' formal_parameter_list ']' '{' interface_accessors '}'
    ;
```

<span data-ttu-id="794ed-219">*Öznitelikleri*, *türü*, ve *formal_parameter_list* bir arabirim dizin oluşturucu bildirimi bir dizin oluşturucu bildirimi sahip aynı anlamı bir sınıfta (sahip.[ Dizin oluşturucular](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="794ed-219">The *attributes*, *type*, and *formal_parameter_list* of an interface indexer declaration have the same meaning as those of an indexer declaration in a class ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="794ed-220">Bir arabirim dizin oluşturucu bildirimi erişicilerini erişicilerini geçersiz bir sınıf dizin oluşturucu bildirimi için karşılık gelen ([dizin oluşturucular](classes.md#indexers)) dışında erişimcisinin gövdesi her zaman noktalı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-220">The accessors of an interface indexer declaration correspond to the accessors of a class indexer declaration ([Indexers](classes.md#indexers)), except that the accessor body must always be a semicolon.</span></span> <span data-ttu-id="794ed-221">Bu nedenle, erişimciler yalnızca dizin oluşturucunun salt okunur, salt okunur veya sadece yazılabilir mi olduğunu belirtin.</span><span class="sxs-lookup"><span data-stu-id="794ed-221">Thus, the accessors simply indicate whether the indexer is read-write, read-only, or write-only.</span></span>

<span data-ttu-id="794ed-222">Tüm biçimsel parametre türleri arabirimi dizin oluşturucu giriş uyumlu olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="794ed-222">All the formal parameter types of an interface indexer must be input-safe .</span></span> <span data-ttu-id="794ed-223">Ayrıca, tüm `out` veya `ref` biçimsel parametre türleri de çıkış uyumlu olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-223">In addition, any `out` or `ref` formal parameter types must also be output-safe.</span></span> <span data-ttu-id="794ed-224">Not, hatta `out` giriş güvenli bir temel yürütme platform sınırlaması nedeniyle olmasını gerekli parametreler.</span><span class="sxs-lookup"><span data-stu-id="794ed-224">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="794ed-225">Arabirim dizin oluşturucu çıkış uyumlu bir alma erişimcisi varsa türünde olmalıdır ve giriş ayarlama erişimcisine ise güvenli olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-225">The type of an interface indexer must be output-safe if there is a get accessor, and must be input-safe if there is a set accessor.</span></span>

### <a name="interface-member-access"></a><span data-ttu-id="794ed-226">Arabirim üye erişimi</span><span class="sxs-lookup"><span data-stu-id="794ed-226">Interface member access</span></span>

<span data-ttu-id="794ed-227">Arabirim üyeleri, üye erişimi erişilir ([üye erişimi](expressions.md#member-access)) ve dizinleyici erişimi ([dizinleyici erişimi](expressions.md#indexer-access)) ifadeleri biçiminde `I.M` ve `I[A]`burada `I` bir arabirim türü `M` yöntemi, özelliği veya arabirim türü, olay ve `A` bir dizin oluşturucu bağımsız değişken listesi.</span><span class="sxs-lookup"><span data-stu-id="794ed-227">Interface members are accessed through member access ([Member access](expressions.md#member-access)) and indexer access ([Indexer access](expressions.md#indexer-access)) expressions of the form `I.M` and `I[A]`, where `I` is an interface type, `M` is a method, property, or event of that interface type, and `A` is an indexer argument list.</span></span>

<span data-ttu-id="794ed-228">Kesinlikle olan arabirimler için tek devralma (devralma zincirini her arabirim, tam olarak sıfır veya bir doğrudan temel arabirimi vardır), üye araması etkilerini ([üye araması](expressions.md#member-lookup)), yöntem çağırma ([ Yöntem çağrıları](expressions.md#method-invocations)) ve dizin oluşturucu erişim ([dizinleyici erişimi](expressions.md#indexer-access)) kuralları, tam olarak aynı sınıflar ve yapılar için: Daha fazla üyeleri Gizle daha az türetilmiş üyeleri aynı ad veya imzaya sahip türetilmiş.</span><span class="sxs-lookup"><span data-stu-id="794ed-228">For interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effects of the member lookup ([Member lookup](expressions.md#member-lookup)), method invocation ([Method invocations](expressions.md#method-invocations)), and indexer access ([Indexer access](expressions.md#indexer-access)) rules are exactly the same as for classes and structs: More derived members hide less derived members with the same name or signature.</span></span> <span data-ttu-id="794ed-229">Ancak, birden çok devralma arabirimleri için belirsizlikleri iki oluşabilir veya daha ilgisiz temel arabirimleri aynı ad veya imzaya üyeleri tanımlanmış olarak bildirin.</span><span class="sxs-lookup"><span data-stu-id="794ed-229">However, for multiple-inheritance interfaces, ambiguities can occur when two or more unrelated base interfaces declare members with the same name or signature.</span></span> <span data-ttu-id="794ed-230">Bu bölüm, bu tür durumlarda çeşitli örneklerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="794ed-230">This section shows several examples of such situations.</span></span> <span data-ttu-id="794ed-231">Tüm durumlarda açık yayınları belirsizlikleri gidermek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="794ed-231">In all cases, explicit casts can be used to resolve the ambiguities.</span></span>

<span data-ttu-id="794ed-232">Örnekte</span><span class="sxs-lookup"><span data-stu-id="794ed-232">In the example</span></span>
```csharp
interface IList
{
    int Count { get; set; }
}

interface ICounter
{
    void Count(int i);
}

interface IListCounter: IList, ICounter {}

class C
{
    void Test(IListCounter x) {
        x.Count(1);                  // Error
        x.Count = 1;                 // Error
        ((IList)x).Count = 1;        // Ok, invokes IList.Count.set
        ((ICounter)x).Count(1);      // Ok, invokes ICounter.Count
    }
}
```
<span data-ttu-id="794ed-233">ilk iki deyim olduğundan derleme zamanı hatalarına neden üye araması ([üye araması](expressions.md#member-lookup)), `Count` içinde `IListCounter` belirsiz.</span><span class="sxs-lookup"><span data-stu-id="794ed-233">the first two statements cause compile-time errors because the member lookup ([Member lookup](expressions.md#member-lookup)) of `Count` in `IListCounter` is ambiguous.</span></span> <span data-ttu-id="794ed-234">Örnekte gösterildiği gibi belirsizliği atama tarafından çözümlenen `x` uygun temel arabirim türü.</span><span class="sxs-lookup"><span data-stu-id="794ed-234">As illustrated by the example, the ambiguity is resolved by casting `x` to the appropriate base interface type.</span></span> <span data-ttu-id="794ed-235">Tür atamaları, hiçbir çalışma zamanı maliyetlerini sahip; bunlar yalnızca derleme zamanında daha az türetilmiş bir tür örneği görüntüleme oluşur.</span><span class="sxs-lookup"><span data-stu-id="794ed-235">Such casts have no run-time costs—they merely consist of viewing the instance as a less derived type at compile-time.</span></span>

<span data-ttu-id="794ed-236">Örnekte</span><span class="sxs-lookup"><span data-stu-id="794ed-236">In the example</span></span>
```csharp
interface IInteger
{
    void Add(int i);
}

interface IDouble
{
    void Add(double d);
}

interface INumber: IInteger, IDouble {}

class C
{
    void Test(INumber n) {
        n.Add(1);                // Invokes IInteger.Add
        n.Add(1.0);              // Only IDouble.Add is applicable
        ((IInteger)n).Add(1);    // Only IInteger.Add is a candidate
        ((IDouble)n).Add(1);     // Only IDouble.Add is a candidate
    }
}
```
<span data-ttu-id="794ed-237">Çağırma `n.Add(1)` seçer `IInteger.Add` aşırı yükleme çözünürlüğü kuralları uygulayarak [aşırı yükleme çözünürlüğü](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="794ed-237">the invocation `n.Add(1)` selects `IInteger.Add` by applying the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="794ed-238">Benzer şekilde çağırma `n.Add(1.0)` seçer `IDouble.Add`.</span><span class="sxs-lookup"><span data-stu-id="794ed-238">Similarly the invocation `n.Add(1.0)` selects `IDouble.Add`.</span></span> <span data-ttu-id="794ed-239">Açık yayınları eklenen olduğunda yalnızca bir aday yöntemi ve böylece belirsizlik olmaz.</span><span class="sxs-lookup"><span data-stu-id="794ed-239">When explicit casts are inserted, there is only one candidate method, and thus no ambiguity.</span></span>

<span data-ttu-id="794ed-240">Örnekte</span><span class="sxs-lookup"><span data-stu-id="794ed-240">In the example</span></span>
```csharp
interface IBase
{
    void F(int i);
}

interface ILeft: IBase
{
    new void F(int i);
}

interface IRight: IBase
{
    void G();
}

interface IDerived: ILeft, IRight {}

class A
{
    void Test(IDerived d) {
        d.F(1);                 // Invokes ILeft.F
        ((IBase)d).F(1);        // Invokes IBase.F
        ((ILeft)d).F(1);        // Invokes ILeft.F
        ((IRight)d).F(1);       // Invokes IBase.F
    }
}
```
<span data-ttu-id="794ed-241">`IBase.F` üyesi tarafından gizlenir `ILeft.F` üyesi.</span><span class="sxs-lookup"><span data-stu-id="794ed-241">the `IBase.F` member is hidden by the `ILeft.F` member.</span></span> <span data-ttu-id="794ed-242">Çağırma `d.F(1)` böylece seçer `ILeft.F`rağmen `IBase.F` aracılığıyla müşteri adayları erişim yolda gizlenmemiş görünmesine `IRight`.</span><span class="sxs-lookup"><span data-stu-id="794ed-242">The invocation `d.F(1)` thus selects `ILeft.F`, even though `IBase.F` appears to not be hidden in the access path that leads through `IRight`.</span></span>

<span data-ttu-id="794ed-243">Birden çok devralma arabirimlerde gizleme için sezgisel Kuralın yalnızca bu. Üye herhangi bir erişim yolunda gizli ise, tüm erişim yollarında gizlenir.</span><span class="sxs-lookup"><span data-stu-id="794ed-243">The intuitive rule for hiding in multiple-inheritance interfaces is simply this: If a member is hidden in any access path, it is hidden in all access paths.</span></span> <span data-ttu-id="794ed-244">Çünkü erişim yolundan `IDerived` için `ILeft` için `IBase` gizler `IBase.F`, üye erişimi yolundan gizlidir ayrıca `IDerived` için `IRight` için `IBase`.</span><span class="sxs-lookup"><span data-stu-id="794ed-244">Because the access path from `IDerived` to `ILeft` to `IBase` hides `IBase.F`, the member is also hidden in the access path from `IDerived` to `IRight` to `IBase`.</span></span>

## <a name="fully-qualified-interface-member-names"></a><span data-ttu-id="794ed-245">Tam bir arabirim üye adları</span><span class="sxs-lookup"><span data-stu-id="794ed-245">Fully qualified interface member names</span></span>

<span data-ttu-id="794ed-246">Bir arabirim üyesi, bazen olarak adlandırılır, ***tam nitelikli ad***.</span><span class="sxs-lookup"><span data-stu-id="794ed-246">An interface member is sometimes referred to by its ***fully qualified name***.</span></span> <span data-ttu-id="794ed-247">Arabirim üyesini tam adı, üyesi bildirilen, ardından bir nokta, üye adından önce gelen arabirimin adını oluşur.</span><span class="sxs-lookup"><span data-stu-id="794ed-247">The fully qualified name of an interface member consists of the name of the interface in which the member is declared, followed by a dot, followed by the name of the member.</span></span> <span data-ttu-id="794ed-248">Tam bir üyenin adını üye bildirildiği arabirimde başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="794ed-248">The fully qualified name of a member references the interface in which the member is declared.</span></span> <span data-ttu-id="794ed-249">Örneğin, bildirimleri verildiğinde</span><span class="sxs-lookup"><span data-stu-id="794ed-249">For example, given the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}
```
<span data-ttu-id="794ed-250">tam olarak nitelenmiş adını `Paint` olduğu `IControl.Paint` ve tam olarak nitelenmiş adını `SetText` olduğu `ITextBox.SetText`.</span><span class="sxs-lookup"><span data-stu-id="794ed-250">the fully qualified name of `Paint` is `IControl.Paint` and the fully qualified name of `SetText` is `ITextBox.SetText`.</span></span>

<span data-ttu-id="794ed-251">Yukarıdaki örnekte, başvurmak mümkün değildir `Paint` olarak `ITextBox.Paint`.</span><span class="sxs-lookup"><span data-stu-id="794ed-251">In the example above, it is not possible to refer to `Paint` as `ITextBox.Paint`.</span></span>

<span data-ttu-id="794ed-252">Bir arabirim bir ad alanının parçası olduğunda, bir arabirim üyesi tam adı ad alanı adı içerir.</span><span class="sxs-lookup"><span data-stu-id="794ed-252">When an interface is part of a namespace, the fully qualified name of an interface member includes the namespace name.</span></span> <span data-ttu-id="794ed-253">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="794ed-253">For example</span></span>
```csharp
namespace System
{
    public interface ICloneable
    {
        object Clone();
    }
}
```

<span data-ttu-id="794ed-254">Burada, tam olarak nitelenmiş adını `Clone` yöntemi `System.ICloneable.Clone`.</span><span class="sxs-lookup"><span data-stu-id="794ed-254">Here, the fully qualified name of the `Clone` method is `System.ICloneable.Clone`.</span></span>

## <a name="interface-implementations"></a><span data-ttu-id="794ed-255">Arabirim uygulamaları</span><span class="sxs-lookup"><span data-stu-id="794ed-255">Interface implementations</span></span>

<span data-ttu-id="794ed-256">Arabirimler, sınıflar ve yapılar tarafından uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="794ed-256">Interfaces may be implemented by classes and structs.</span></span> <span data-ttu-id="794ed-257">Bir sınıf veya yapı doğrudan bir arabirim uyguladığını belirtmek için arabirim tanımlayıcısı, sınıfın veya yapının temel sınıf listesinde dahildir.</span><span class="sxs-lookup"><span data-stu-id="794ed-257">To indicate that a class or struct directly implements an interface, the interface identifier is included in the base class list of the class or struct.</span></span> <span data-ttu-id="794ed-258">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="794ed-258">For example:</span></span>
```csharp
interface ICloneable
{
    object Clone();
}

interface IComparable
{
    int CompareTo(object other);
}

class ListEntry: ICloneable, IComparable
{
    public object Clone() {...}
    public int CompareTo(object other) {...}
}
```

<span data-ttu-id="794ed-259">Bir sınıf ya da doğrudan bir arabirimi doğrudan uygulayan yapı bu arabirimin temel arabirimleri tüm örtük olarak uygular.</span><span class="sxs-lookup"><span data-stu-id="794ed-259">A class or struct that directly implements an interface also directly implements all of the interface's base interfaces implicitly.</span></span> <span data-ttu-id="794ed-260">Sınıfın veya yapının tüm temel arabirimleri taban sınıfı listesinde açıkça listelenmiyor olsa bile bu geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="794ed-260">This is true even if the class or struct doesn't explicitly list all base interfaces in the base class list.</span></span> <span data-ttu-id="794ed-261">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="794ed-261">For example:</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    public void Paint() {...}
    public void SetText(string text) {...}
}
```

<span data-ttu-id="794ed-262">Burada, sınıf `TextBox` hem `IControl` ve `ITextBox`.</span><span class="sxs-lookup"><span data-stu-id="794ed-262">Here, class `TextBox` implements both `IControl` and `ITextBox`.</span></span>

<span data-ttu-id="794ed-263">Bir sınıf, `C` doğrudan uygulayan bir arabirim, C'den türetilmiş tüm sınıfları ayrıca arayüzü dolaylı olarak gerçekleştir.</span><span class="sxs-lookup"><span data-stu-id="794ed-263">When a class `C` directly implements an interface, all classes derived from C also implement the interface implicitly.</span></span> <span data-ttu-id="794ed-264">Bir sınıf bildiriminde belirtilenden temel arabirimleri oluşturulmuş arabirim türlerinde olabilir ([oluşturulan türler](types.md#constructed-types)).</span><span class="sxs-lookup"><span data-stu-id="794ed-264">The base interfaces specified in a class declaration can be constructed interface types ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="794ed-265">Kapsam türü parametreler içerebilir ancak bir taban arabirimi kendi kendine, bir tür parametresi olamaz.</span><span class="sxs-lookup"><span data-stu-id="794ed-265">A base interface cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span> <span data-ttu-id="794ed-266">Aşağıdaki kod nasıl bir sınıf uygulamak ve oluşturulan türler genişletmek gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="794ed-266">The following code illustrates how a class can implement and extend constructed types:</span></span>
```csharp
class C<U,V> {}

interface I1<V> {}

class D: C<string,int>, I1<string> {}

class E<T>: C<int,T>, I1<T> {}
```

<span data-ttu-id="794ed-267">Genel sınıf bildiriminin temel arabirimleri açıklanan benzersizlik kuralı karşılamalıdır [uygulanan arabirimleri benzersizliğini](interfaces.md#uniqueness-of-implemented-interfaces).</span><span class="sxs-lookup"><span data-stu-id="794ed-267">The base interfaces of a generic class declaration must satisfy the uniqueness rule described in [Uniqueness of implemented interfaces](interfaces.md#uniqueness-of-implemented-interfaces).</span></span>

### <a name="explicit-interface-member-implementations"></a><span data-ttu-id="794ed-268">Açık arabirim üyesi uygulamaları</span><span class="sxs-lookup"><span data-stu-id="794ed-268">Explicit interface member implementations</span></span>

<span data-ttu-id="794ed-269">Arabirimleri uygulama amacıyla, bir sınıf veya yapı bildirebilir ***açık arabirim üyesi uygulamalarını***.</span><span class="sxs-lookup"><span data-stu-id="794ed-269">For purposes of implementing interfaces, a class or struct may declare ***explicit interface member implementations***.</span></span> <span data-ttu-id="794ed-270">Açık arabirim üyesi tam arabirim üye adına başvuran bir yöntem, özellik, olay veya dizin oluşturucu bildirimi uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-270">An explicit interface member implementation is a method, property, event, or indexer declaration that references a fully qualified interface member name.</span></span> <span data-ttu-id="794ed-271">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="794ed-271">For example</span></span>
```csharp
interface IList<T>
{
    T[] GetElements();
}

interface IDictionary<K,V>
{
    V this[K key];
    void Add(K key, V value);
}

class List<T>: IList<T>, IDictionary<int,T>
{
    T[] IList<T>.GetElements() {...}
    T IDictionary<int,T>.this[int index] {...}
    void IDictionary<int,T>.Add(int index, T value) {...}
}
```

<span data-ttu-id="794ed-272">Burada `IDictionary<int,T>.this` ve `IDictionary<int,T>.Add` açık arabirim üyesi uygulamalarıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-272">Here `IDictionary<int,T>.this` and `IDictionary<int,T>.Add` are explicit interface member implementations.</span></span>

<span data-ttu-id="794ed-273">Bazı durumlarda, bir arabirim üyesinin adı çalışması arabirim üyesi, uygulanabilir açık arabirim üyesi uygulaması kullanarak uygulama sınıfı için uygun olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="794ed-273">In some cases, the name of an interface member may not be appropriate for the implementing class, in which case the interface member may be implemented using explicit interface member implementation.</span></span> <span data-ttu-id="794ed-274">Örneğin, bir dosya özeti uygulayan bir sınıf uygulamak büyük olasılıkla bir `Close` dosya kaynağı serbest etkisi ve uygulama üye işlevi `Dispose` yöntemi `IDisposable` arabirimi açık arabirim kullanma üye uygulaması:</span><span class="sxs-lookup"><span data-stu-id="794ed-274">A class implementing a file abstraction, for example, would likely implement a `Close` member function that has the effect of releasing the file resource, and implement the `Dispose` method of the `IDisposable` interface using explicit interface member implementation:</span></span>
```csharp
interface IDisposable
{
    void Dispose();
}

class MyFile: IDisposable
{
    void IDisposable.Dispose() {
        Close();
    }

    public void Close() {
        // Do what's necessary to close the file
        System.GC.SuppressFinalize(this);
    }
}
```

<span data-ttu-id="794ed-275">Tam adı bir yöntem çağrısı, özellik erişimi veya dizin oluşturucu erişim aracılığıyla üye açık arabirim uygulaması erişmek mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="794ed-275">It is not possible to access an explicit interface member implementation through its fully qualified name in a method invocation, property access, or indexer access.</span></span> <span data-ttu-id="794ed-276">Açık arabirim üyesi uygulaması yalnızca bir arabirim örneği erişilebilir ve bu durumda yalnızca üye adıyla başvurulur.</span><span class="sxs-lookup"><span data-stu-id="794ed-276">An explicit interface member implementation can only be accessed through an interface instance, and is in that case referenced simply by its member name.</span></span>

<span data-ttu-id="794ed-277">Erişim değiştiricileri içerecek şekilde açık arabirim üyesi uygulaması için bir derleme zamanı hata ve değiştiriciler dahil etmek için bir derleme zamanı hata `abstract`, `virtual`, `override`, veya `static`.</span><span class="sxs-lookup"><span data-stu-id="794ed-277">It is a compile-time error for an explicit interface member implementation to include access modifiers, and it is a compile-time error to include the modifiers `abstract`, `virtual`, `override`, or `static`.</span></span>

<span data-ttu-id="794ed-278">Açık arabirim üyesi uygulamaları, diğer üyelerinden farklı erişilebilirlik özelliklerine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="794ed-278">Explicit interface member implementations have different accessibility characteristics than other members.</span></span> <span data-ttu-id="794ed-279">Açık arabirim üyesi uygulamalarını tam adında bir yöntem çağrısı veya özellik erişimi üzerinden erişilebilir olduğundan, hiçbir zaman özel bir anlamda değildirler.</span><span class="sxs-lookup"><span data-stu-id="794ed-279">Because explicit interface member implementations are never accessible through their fully qualified name in a method invocation or a property access, they are in a sense private.</span></span> <span data-ttu-id="794ed-280">Ancak, bir arabirim örneği erişilebilir olduğundan, ayrıca genel anlamda uygulanır.</span><span class="sxs-lookup"><span data-stu-id="794ed-280">However, since they can be accessed through an interface instance, they are in a sense also public.</span></span>

<span data-ttu-id="794ed-281">Açık arabirim üyesi uygulamalarını iki birincil amaca hizmet eder:</span><span class="sxs-lookup"><span data-stu-id="794ed-281">Explicit interface member implementations serve two primary purposes:</span></span>

*  <span data-ttu-id="794ed-282">Açık arabirim üyesi uygulamalarını sınıfın veya yapının örneği erişilebilir olmadığından, bir sınıfın veya yapının ortak arabirimden hariç tutulacak arabirim uygulamalarına izin verin.</span><span class="sxs-lookup"><span data-stu-id="794ed-282">Because explicit interface member implementations are not accessible through class or struct instances, they allow interface implementations to be excluded from the public interface of a class or struct.</span></span> <span data-ttu-id="794ed-283">Bu özellikle yararlı bir sınıf veya yapı, sınıfın veya yapının bir tüketici hiçbir ilgi iç bir arabirim uygular.</span><span class="sxs-lookup"><span data-stu-id="794ed-283">This is particularly useful when a class or struct implements an internal interface that is of no interest to a consumer of that class or struct.</span></span>
*  <span data-ttu-id="794ed-284">Açık arabirim üyesi uygulamalarını arabirim üyelerinin aynı imzayla Kesinleştirme izin verir.</span><span class="sxs-lookup"><span data-stu-id="794ed-284">Explicit interface member implementations allow disambiguation of interface members with the same signature.</span></span> <span data-ttu-id="794ed-285">Açık arabirim üyesi uygulamalarını bu arabirim üyeleri aynı imzaya sahip ve, olarak, herhangi bir uygulama için bir sınıf veya yapı için imkansız olan dönüş türü farklı uygulamalarını sağlamak için bir sınıf veya yapı için mümkün olacaktır Tümünü arabirim üyelerinin ve aynı imzaya ancak farklı dönüş türlerine sahip.</span><span class="sxs-lookup"><span data-stu-id="794ed-285">Without explicit interface member implementations it would be impossible for a class or struct to have different implementations of interface members with the same signature and return type, as would it be impossible for a class or struct to have any implementation at all of interface members with the same signature but with different return types.</span></span>

<span data-ttu-id="794ed-286">Geçerli olması açık arabirim üyesi için bir uygulama, sınıf veya yapı, tam adı, türü ve parametre türleri tam olarak açık arabirim üyesi eşleşen bir üyeyi içeren kendi taban sınıfı listesinde bir arabirim adı olmalıdır uygulama.</span><span class="sxs-lookup"><span data-stu-id="794ed-286">For an explicit interface member implementation to be valid, the class or struct must name an interface in its base class list that contains a member whose fully qualified name, type, and parameter types exactly match those of the explicit interface member implementation.</span></span> <span data-ttu-id="794ed-287">Bu nedenle, aşağıdaki sınıfı</span><span class="sxs-lookup"><span data-stu-id="794ed-287">Thus, in the following class</span></span>
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
    int IComparable.CompareTo(object other) {...}    // invalid
}
```
<span data-ttu-id="794ed-288">bildirimi `IComparable.CompareTo` sonuçları bir derleme zamanı hatası nedeniyle `IComparable` temel sınıf listesinde listelenmeyen `Shape` ve bir taban arabirimi değil `ICloneable`.</span><span class="sxs-lookup"><span data-stu-id="794ed-288">the declaration of `IComparable.CompareTo` results in a compile-time error because `IComparable` is not listed in the base class list of `Shape` and is not a base interface of `ICloneable`.</span></span> <span data-ttu-id="794ed-289">Benzer şekilde, bildirimler</span><span class="sxs-lookup"><span data-stu-id="794ed-289">Likewise, in the declarations</span></span>
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
}

class Ellipse: Shape
{
    object ICloneable.Clone() {...}    // invalid
}
```
<span data-ttu-id="794ed-290">bildirimi `ICloneable.Clone` içinde `Ellipse` sonuçları bir derleme zamanı hatası nedeniyle `ICloneable` açıkça temel sınıf listesinde listelenmeyen `Ellipse`.</span><span class="sxs-lookup"><span data-stu-id="794ed-290">the declaration of `ICloneable.Clone` in `Ellipse` results in a compile-time error because `ICloneable` is not explicitly listed in the base class list of `Ellipse`.</span></span>

<span data-ttu-id="794ed-291">Arabirim üyesini tam adı, üyenin bildirilen arabirimi başvurmalıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-291">The fully qualified name of an interface member must reference the interface in which the member was declared.</span></span> <span data-ttu-id="794ed-292">Bu nedenle, bildirimler</span><span class="sxs-lookup"><span data-stu-id="794ed-292">Thus, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
}
```
<span data-ttu-id="794ed-293">Açık arabirim üye uygulamasıdır `Paint` olarak yazılmalıdır `IControl.Paint`.</span><span class="sxs-lookup"><span data-stu-id="794ed-293">the explicit interface member implementation of `Paint` must be written as `IControl.Paint`.</span></span>

### <a name="uniqueness-of-implemented-interfaces"></a><span data-ttu-id="794ed-294">Uygulanan arabirimlerin benzersizlik</span><span class="sxs-lookup"><span data-stu-id="794ed-294">Uniqueness of implemented interfaces</span></span>

<span data-ttu-id="794ed-295">Bir genel tür bildirimi tarafından uygulanan arabirimler olası tüm oluşturulan türler için benzersiz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-295">The interfaces implemented by a generic type declaration must remain unique for all possible constructed types.</span></span> <span data-ttu-id="794ed-296">Bu kural oluşturulan belirli türler için çağrılacak doğru yöntem belirlemek mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="794ed-296">Without this rule, it would be impossible to determine the correct method to call for certain constructed types.</span></span> <span data-ttu-id="794ed-297">Örneğin, bir genel sınıf bildirimi izin verilen gibi yazılacak varsayalım:</span><span class="sxs-lookup"><span data-stu-id="794ed-297">For example, suppose a generic class declaration were permitted to be written as follows:</span></span>
```csharp
interface I<T>
{
    void F();
}

class X<U,V>: I<U>, I<V>                    // Error: I<U> and I<V> conflict
{
    void I<U>.F() {...}
    void I<V>.F() {...}
}
```

<span data-ttu-id="794ed-298">Bu izin olan, aşağıdaki örnekte yürütmek için hangi kodun belirlemek mümkün olacaktır:</span><span class="sxs-lookup"><span data-stu-id="794ed-298">Were this permitted, it would be impossible to determine which code to execute in the following case:</span></span>
```csharp
I<int> x = new X<int,int>();
x.F();
```

<span data-ttu-id="794ed-299">Bir genel tür bildirimi arabirimi listesi geçerli olup olmadığını belirlemek için aşağıdaki adımları gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="794ed-299">To determine if the interface list of a generic type declaration is valid, the following steps are performed:</span></span>

*  <span data-ttu-id="794ed-300">İzin `L` doğrudan bir genel sınıf, yapı veya arabirim bildiriminde belirtilen arabirimlerin listesi `C`.</span><span class="sxs-lookup"><span data-stu-id="794ed-300">Let `L` be the list of interfaces directly specified in a generic class, struct, or interface declaration `C`.</span></span>
*  <span data-ttu-id="794ed-301">Ekleme `L` herhangi temel arabirimleri içinde arabirimlerin `L`.</span><span class="sxs-lookup"><span data-stu-id="794ed-301">Add to `L` any base interfaces of the interfaces already in `L`.</span></span>
*  <span data-ttu-id="794ed-302">Tüm yinelenenleri kaldırma `L`.</span><span class="sxs-lookup"><span data-stu-id="794ed-302">Remove any duplicates from `L`.</span></span>
*  <span data-ttu-id="794ed-303">Türü oluşturulan tüm olası oluşturulmuş, `C` olduğu tür bağımsız değişkenleri ile başvurulduğunda sonra `L`, iki arabirimi neden `L` aynı olmaları için bildirimi ardından `C` geçersiz.</span><span class="sxs-lookup"><span data-stu-id="794ed-303">If any possible constructed type created from `C` would, after type arguments are substituted into `L`, cause two interfaces in `L` to be identical, then the declaration of `C` is invalid.</span></span> <span data-ttu-id="794ed-304">Kısıtlama bildirimleri olası tüm oluşturulan türler belirlerken dikkate alınmaz.</span><span class="sxs-lookup"><span data-stu-id="794ed-304">Constraint declarations are not considered when determining all possible constructed types.</span></span>

<span data-ttu-id="794ed-305">Sınıf bildiriminde `X` arabirimi listesinin üst `L` oluşan `I<U>` ve `I<V>`.</span><span class="sxs-lookup"><span data-stu-id="794ed-305">In the class declaration `X` above, the interface list `L` consists of `I<U>` and `I<V>`.</span></span> <span data-ttu-id="794ed-306">Herhangi bir türü ile oluşturulmuş bildirimi geçersiz `U` ve `V` aynı türde olan aynı olması bu iki arabirim neden.</span><span class="sxs-lookup"><span data-stu-id="794ed-306">The declaration is invalid because any constructed type with `U` and `V` being the same type would cause these two interfaces to be identical types.</span></span>

<span data-ttu-id="794ed-307">Birleştirmek için farklı bir devralma düzeylerinde belirtilen arabirimleri mümkündür:</span><span class="sxs-lookup"><span data-stu-id="794ed-307">It is possible for interfaces specified at different inheritance levels to unify:</span></span>
```csharp
interface I<T>
{
    void F();
}

class Base<U>: I<U>
{
    void I<U>.F() {...}
}

class Derived<U,V>: Base<U>, I<V>    // Ok
{
    void I<V>.F() {...}
}
```

<span data-ttu-id="794ed-308">Bu kod olsa bile geçerlidir `Derived<U,V>` hem `I<U>` ve `I<V>`.</span><span class="sxs-lookup"><span data-stu-id="794ed-308">This code is valid even though `Derived<U,V>` implements both `I<U>` and `I<V>`.</span></span> <span data-ttu-id="794ed-309">Kodu</span><span class="sxs-lookup"><span data-stu-id="794ed-309">The code</span></span>
```csharp
I<int> x = new Derived<int,int>();
x.F();
```
<span data-ttu-id="794ed-310">yöntemi çağırır `Derived`, bu yana `Derived<int,int>` etkili bir şekilde yeniden uygular `I<int>` ([arabirimini yeniden uygulama](interfaces.md#interface-re-implementation)).</span><span class="sxs-lookup"><span data-stu-id="794ed-310">invokes the method in `Derived`, since `Derived<int,int>` effectively re-implements `I<int>` ([Interface re-implementation](interfaces.md#interface-re-implementation)).</span></span>

### <a name="implementation-of-generic-methods"></a><span data-ttu-id="794ed-311">Genel yöntemler uygulaması</span><span class="sxs-lookup"><span data-stu-id="794ed-311">Implementation of generic methods</span></span>

<span data-ttu-id="794ed-312">Ne zaman bir genel yöntemini örtük olarak uygulayan bir arabirim yöntemi her yöntem türü parametresi (parametreleri uygun tür bağımsız değişkenleri ile değiştirilir herhangi bir arabirim türüne sonra), burada iki bildirimlerinde eşdeğer olmalıdır için verilen kısıtlamaları yöntemi Tür parametreleri, soldan sağa sıralı konumları tarafından tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="794ed-312">When a generic method implicitly implements an interface method, the constraints given for each method type parameter must be equivalent in both declarations (after any interface type parameters are replaced with the appropriate type arguments), where method type parameters are identified by ordinal positions, left to right.</span></span>

<span data-ttu-id="794ed-313">Ancak, genel bir yöntem bir arabirim yöntemini açıkça kullanan kullandığınızda, hiçbir kısıtlama uygulama yöntemi izin verilir.</span><span class="sxs-lookup"><span data-stu-id="794ed-313">When a generic method explicitly implements an interface method, however, no constraints are allowed on the implementing method.</span></span> <span data-ttu-id="794ed-314">Bunun yerine, kısıtlamaları arabirimi yöntemden devralınır</span><span class="sxs-lookup"><span data-stu-id="794ed-314">Instead, the constraints are inherited from the interface method</span></span>

```csharp
interface I<A,B,C>
{
    void F<T>(T t) where T: A;
    void G<T>(T t) where T: B;
    void H<T>(T t) where T: C;
}

class C: I<object,C,string>
{
    public void F<T>(T t) {...}                    // Ok
    public void G<T>(T t) where T: C {...}         // Ok
    public void H<T>(T t) where T: string {...}    // Error
}
```

<span data-ttu-id="794ed-315">Yöntem `C.F<T>` örtük olarak uygulayan `I<object,C,string>.F<T>`.</span><span class="sxs-lookup"><span data-stu-id="794ed-315">The method `C.F<T>` implicitly implements `I<object,C,string>.F<T>`.</span></span> <span data-ttu-id="794ed-316">Bu durumda, `C.F<T>` gerekli (izin veya değil) kısıtlama belirtmek için `T:object` beri `object` tüm tür parametrelerindeki örtük bir sınırlamadır.</span><span class="sxs-lookup"><span data-stu-id="794ed-316">In this case, `C.F<T>` is not required (nor permitted) to specify the constraint `T:object` since `object` is an implicit constraint on all type parameters.</span></span> <span data-ttu-id="794ed-317">Yöntem `C.G<T>` örtük olarak uygulayan `I<object,C,string>.G<T>` arabirimi tür parametreleri karşılık gelen tür bağımsız değişkenleri ile değiştirildikten sonra sınırlamalar arabirim içindeki alanlarla eşleşmesi için.</span><span class="sxs-lookup"><span data-stu-id="794ed-317">The method `C.G<T>` implicitly implements `I<object,C,string>.G<T>` because the constraints match those in the interface, after the interface type parameters are replaced with the corresponding type arguments.</span></span> <span data-ttu-id="794ed-318">Yöntem kısıtlaması `C.H<T>` hata türleri korumalı olduğundan (`string` bu durumda) kısıtlama olarak kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="794ed-318">The constraint for method `C.H<T>` is an error because sealed types (`string` in this case) cannot be used as constraints.</span></span> <span data-ttu-id="794ed-319">Örtük arabirimi yöntem uygulamaları kısıtlamaları eşleştirilmesi için bu yana kısıtlaması atlama hata da olacaktır.</span><span class="sxs-lookup"><span data-stu-id="794ed-319">Omitting the constraint would also be an error since constraints of implicit interface method implementations are required to match.</span></span> <span data-ttu-id="794ed-320">Bu nedenle, örtük olarak uygulamak mümkün değildir `I<object,C,string>.H<T>`.</span><span class="sxs-lookup"><span data-stu-id="794ed-320">Thus, it is impossible to implicitly implement `I<object,C,string>.H<T>`.</span></span> <span data-ttu-id="794ed-321">Bu arabirim yöntemi yalnızca açık arabirim üyesi uygulaması kullanılarak uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="794ed-321">This interface method can only be implemented using an explicit interface member implementation:</span></span>
```csharp
class C: I<object,C,string>
{
    ...

    public void H<U>(U u) where U: class {...}

    void I<object,C,string>.H<T>(T t) {
        string s = t;    // Ok
        H<T>(t);
    }
}
```

<span data-ttu-id="794ed-322">Bu örnekte, açık arabirim üyesi uygulaması kesin olarak daha zayıf kısıtlamalarına sahip ortak bir yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="794ed-322">In this example, the explicit interface member implementation invokes a public method having strictly weaker constraints.</span></span> <span data-ttu-id="794ed-323">Unutmayın atamadan `t` için `s` itibaren geçerlidir `T` kısıtlaması, devralınan `T:string`rağmen bu kısıtlama, kaynak kodunda ifade edilemez.</span><span class="sxs-lookup"><span data-stu-id="794ed-323">Note that the assignment from `t` to `s` is valid since `T` inherits a constraint of `T:string`, even though this constraint is not expressible in source code.</span></span>

### <a name="interface-mapping"></a><span data-ttu-id="794ed-324">Arabirim eşleme</span><span class="sxs-lookup"><span data-stu-id="794ed-324">Interface mapping</span></span>

<span data-ttu-id="794ed-325">Bir sınıfın veya yapının sınıfın veya yapının temel sınıf listesinde listelenen arabirimler tüm üyelerinin uygulamalarını sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="794ed-325">A class or struct must provide implementations of all members of the interfaces that are listed in the base class list of the class or struct.</span></span> <span data-ttu-id="794ed-326">Uygulayan bir sınıf veya yapı içinde arabirim üyelerinin uygulamalarını bulma işlemi olarak bilinir ***arabirim eşleme***.</span><span class="sxs-lookup"><span data-stu-id="794ed-326">The process of locating implementations of interface members in an implementing class or struct is known as ***interface mapping***.</span></span>

<span data-ttu-id="794ed-327">Bir sınıf veya yapı için arabirim eşlemesi `C` her üye temel sınıf listesinde belirtilen her arabirim için bir uygulama bulur `C`.</span><span class="sxs-lookup"><span data-stu-id="794ed-327">Interface mapping for a class or struct `C` locates an implementation for each member of each interface specified in the base class list of `C`.</span></span> <span data-ttu-id="794ed-328">Belirli bir arabirim üye uygulamasıdır `I.M`burada `I` , arabirim üyesi `M` bildirilen, her bir sınıf veya yapı incelenerek belirlenir `S`ile başlayan `C` ve Her art arda gelen taban sınıfı için yinelenen `C`kadar bir eşleşme bulunur:</span><span class="sxs-lookup"><span data-stu-id="794ed-328">The implementation of a particular interface member `I.M`, where `I` is the interface in which the member `M` is declared, is determined by examining each class or struct `S`, starting with `C` and repeating for each successive base class of `C`, until a match is located:</span></span>

*  <span data-ttu-id="794ed-329">Varsa `S` eşleşen açık arabirim üyesi uygulaması bildirimi içeren `I` ve `M`, sonra da bu üye uygulamasıdır `I.M`.</span><span class="sxs-lookup"><span data-stu-id="794ed-329">If `S` contains a declaration of an explicit interface member implementation that matches `I` and `M`, then this member is the implementation of `I.M`.</span></span>
*  <span data-ttu-id="794ed-330">Aksi takdirde `S` eşleşen bir ortak statik olmayan üye bildirimi içeren `M`, sonra da bu üye uygulamasıdır `I.M`.</span><span class="sxs-lookup"><span data-stu-id="794ed-330">Otherwise, if `S` contains a declaration of a non-static public member that matches `M`, then this member is the implementation of `I.M`.</span></span> <span data-ttu-id="794ed-331">En fazla bir üye eşleşme unspecified ise hangi üye uygulamasıdır `I.M`.</span><span class="sxs-lookup"><span data-stu-id="794ed-331">If more than one member matches, it is unspecified which member is the implementation of `I.M`.</span></span> <span data-ttu-id="794ed-332">Bu durum yalnızca oluşabilir `S` bir oluşturulmuş tür burada genel türde bildirilen iki üyesi farklı imzalara sahip olduğunu, ancak bunların imzalarını tür bağımsız değişkenleri aynı olun.</span><span class="sxs-lookup"><span data-stu-id="794ed-332">This situation can only occur if `S` is a constructed type where the two members as declared in the generic type have different signatures, but the type arguments make their signatures identical.</span></span>

<span data-ttu-id="794ed-333">Tüm arabirimleri temel sınıf listesinde belirtilen tüm üyeleri için uygulamaları bulunamazsa, bir derleme zamanı hatası oluşur `C`.</span><span class="sxs-lookup"><span data-stu-id="794ed-333">A compile-time error occurs if implementations cannot be located for all members of all interfaces specified in the base class list of `C`.</span></span> <span data-ttu-id="794ed-334">Bir arabirimin üyelerini temel Ara birimden devralınır bu üyeler unutmayın.</span><span class="sxs-lookup"><span data-stu-id="794ed-334">Note that the members of an interface include those members that are inherited from base interfaces.</span></span>

<span data-ttu-id="794ed-335">Arabirim eşleme, bir sınıf üyesinin amacıyla `A` arabirim üyesiyle eşleşiyor `B` olduğunda:</span><span class="sxs-lookup"><span data-stu-id="794ed-335">For purposes of interface mapping, a class member `A` matches an interface member `B` when:</span></span>

*  <span data-ttu-id="794ed-336">`A` ve `B` yöntemleri ve adını, türünü ve biçimsel parametre listeleri `A` ve `B` aynıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-336">`A` and `B` are methods, and the name, type, and formal parameter lists of `A` and `B` are identical.</span></span>
*  <span data-ttu-id="794ed-337">`A` ve `B` özellikleri ad ve tür `A` ve `B` aynıdır ve `A` aynı erişimci olarak sahip `B` (`A` açık bir arabirim değil ek erişimcisi varsa izin verilir üye uygulaması).</span><span class="sxs-lookup"><span data-stu-id="794ed-337">`A` and `B` are properties, the name and type of `A` and `B` are identical, and `A` has the same accessors as `B` (`A` is permitted to have additional accessors if it is not an explicit interface member implementation).</span></span>
*  <span data-ttu-id="794ed-338">`A` ve `B` olaylar, ad ve tür `A` ve `B` aynıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-338">`A` and `B` are events, and the name and type of `A` and `B` are identical.</span></span>
*  <span data-ttu-id="794ed-339">`A` ve `B` dizin oluşturucular türünü ve biçimsel parametre listesi `A` ve `B` aynıdır ve `A` aynı erişimci olarak sahip `B` (`A` değilse ek erişimcilerine sahip. izin verilen bir Açık arabirim üyesi uygulaması).</span><span class="sxs-lookup"><span data-stu-id="794ed-339">`A` and `B` are indexers, the type and formal parameter lists of `A` and `B` are identical, and `A` has the same accessors as `B` (`A` is permitted to have additional accessors if it is not an explicit interface member implementation).</span></span>

<span data-ttu-id="794ed-340">Arabirimi eşleştirme algoritmasını önemli etkileri verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="794ed-340">Notable implications of the interface mapping algorithm are:</span></span>

*  <span data-ttu-id="794ed-341">Açık arabirim üyesi uygulamalarını diğer üyeleri aynı sınıfın veya yapının bir arabirim üyesi uygulayan sınıf veya yapı üyesi belirlerken önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="794ed-341">Explicit interface member implementations take precedence over other members in the same class or struct when determining the class or struct member that implements an interface member.</span></span>
*  <span data-ttu-id="794ed-342">Genel olmayan ne statik üyeleri arabirimi eşlemede katılın.</span><span class="sxs-lookup"><span data-stu-id="794ed-342">Neither non-public nor static members participate in interface mapping.</span></span>

<span data-ttu-id="794ed-343">Örnekte</span><span class="sxs-lookup"><span data-stu-id="794ed-343">In the example</span></span>
```csharp
interface ICloneable
{
    object Clone();
}

class C: ICloneable
{
    object ICloneable.Clone() {...}
    public object Clone() {...}
}
```
<span data-ttu-id="794ed-344">`ICloneable.Clone` üyesi `C` uygulaması haline gelir `Clone` içinde `ICloneable` çünkü açık arabirim üyesi uygulamalarını diğer üyelere göre önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="794ed-344">the `ICloneable.Clone` member of `C` becomes the implementation of `Clone` in `ICloneable` because explicit interface member implementations take precedence over other members.</span></span>

<span data-ttu-id="794ed-345">Bir sınıf veya yapı iki uyguluyorsa veya daha fazla arabirim bir üyeyi aynı adı, türü ve parametre türleri ile içeren, her biri tek bir sınıf veya yapı üyesi üzerine bu arabirim üyeleri eşlemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="794ed-345">If a class or struct implements two or more interfaces containing a member with the same name, type, and parameter types, it is possible to map each of those interface members onto a single class or struct member.</span></span> <span data-ttu-id="794ed-346">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="794ed-346">For example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface IForm
{
    void Paint();
}

class Page: IControl, IForm
{
    public void Paint() {...}
}
```

<span data-ttu-id="794ed-347">Burada, `Paint` hem de yöntemleri `IControl` ve `IForm` üzerine eşlenen `Paint` yönteminde `Page`.</span><span class="sxs-lookup"><span data-stu-id="794ed-347">Here, the `Paint` methods of both `IControl` and `IForm` are mapped onto the `Paint` method in `Page`.</span></span> <span data-ttu-id="794ed-348">Elbette da iki yöntem için ayrı açık arabirim üyesi uygulamalarını olması mümkündür.</span><span class="sxs-lookup"><span data-stu-id="794ed-348">It is of course also possible to have separate explicit interface member implementations for the two methods.</span></span>

<span data-ttu-id="794ed-349">Gizli üyeleri içeren bir arabirim uygularsa, bir sınıfın veya yapının bazı üyeleri mutlaka açık arabirim üyesi uygulamaları uygulanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-349">If a class or struct implements an interface that contains hidden members, then some members must necessarily be implemented through explicit interface member implementations.</span></span> <span data-ttu-id="794ed-350">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="794ed-350">For example</span></span>
```csharp
interface IBase
{
    int P { get; }
}

interface IDerived: IBase
{
    new int P();
}
```

<span data-ttu-id="794ed-351">Bu arabirim uygulaması en az bir açık arabirim üyesi uygulaması gerekir ve aşağıdaki biçimlerden birini alır</span><span class="sxs-lookup"><span data-stu-id="794ed-351">An implementation of this interface would require at least one explicit interface member implementation, and would take one of the following forms</span></span>
```csharp
class C: IDerived
{
    int IBase.P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    public int P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    int IBase.P { get {...} }
    public int P() {...}
}
```

<span data-ttu-id="794ed-352">Aynı temel arabirimi sahip birden çok arabirim arabirimini uygulayan bir sınıf, temel arabirim yalnızca bir adet olabilir.</span><span class="sxs-lookup"><span data-stu-id="794ed-352">When a class implements multiple interfaces that have the same base interface, there can be only one implementation of the base interface.</span></span> <span data-ttu-id="794ed-353">Örnekte</span><span class="sxs-lookup"><span data-stu-id="794ed-353">In the example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

class ComboBox: IControl, ITextBox, IListBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
    void IListBox.SetItems(string[] items) {...}
}
```
<span data-ttu-id="794ed-354">için farklı uygulamalara sahip mümkün değildir `IControl` temel sınıf listesinde adlı `IControl` tarafından devralınan `ITextBox`ve `IControl` tarafından devralınan `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="794ed-354">it is not possible to have separate implementations for the `IControl` named in the base class list, the `IControl` inherited by `ITextBox`, and the `IControl` inherited by `IListBox`.</span></span> <span data-ttu-id="794ed-355">Aslında bu arabirimler için ayrı kimlik kavramı yoktur.</span><span class="sxs-lookup"><span data-stu-id="794ed-355">Indeed, there is no notion of a separate identity for these interfaces.</span></span> <span data-ttu-id="794ed-356">Bunun yerine, uygulamalarına `ITextBox` ve `IListBox` aynı uygulamasını paylaşmak `IControl`, ve `ComboBox` üç arabirimleri de uygulamak için basitçe kabul `IControl`, `ITextBox`, ve `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="794ed-356">Rather, the implementations of `ITextBox` and `IListBox` share the same implementation of `IControl`, and `ComboBox` is simply considered to implement three interfaces, `IControl`, `ITextBox`, and `IListBox`.</span></span>

<span data-ttu-id="794ed-357">Bir temel sınıf üyelerinin arabirimi eşlemede katılın.</span><span class="sxs-lookup"><span data-stu-id="794ed-357">The members of a base class participate in interface mapping.</span></span> <span data-ttu-id="794ed-358">Örnekte</span><span class="sxs-lookup"><span data-stu-id="794ed-358">In the example</span></span>
```csharp
interface Interface1
{
    void F();
}

class Class1
{
    public void F() {}
    public void G() {}
}

class Class2: Class1, Interface1
{
    new public void G() {}
}
```
<span data-ttu-id="794ed-359">yöntem `F` içinde `Class1` kullanılır `Class2`'s uygulaması `Interface1`.</span><span class="sxs-lookup"><span data-stu-id="794ed-359">the method `F` in `Class1` is used in `Class2`'s implementation of `Interface1`.</span></span>

### <a name="interface-implementation-inheritance"></a><span data-ttu-id="794ed-360">Arabirimi uygulama devralma</span><span class="sxs-lookup"><span data-stu-id="794ed-360">Interface implementation inheritance</span></span>

<span data-ttu-id="794ed-361">Bir sınıf kendi temel sınıflar tarafından sağlanan tüm arabirim uygulamalarına devralır.</span><span class="sxs-lookup"><span data-stu-id="794ed-361">A class inherits all interface implementations provided by its base classes.</span></span>

<span data-ttu-id="794ed-362">Olmadan açıkça ***yeniden uygulama*** arabirim, türetilmiş bir sınıf kendi temel sınıftan devraldığı arabirim eşlemeleri herhangi bir şekilde değiştirilemiyor.</span><span class="sxs-lookup"><span data-stu-id="794ed-362">Without explicitly ***re-implementing*** an interface, a derived class cannot in any way alter the interface mappings it inherits from its base classes.</span></span> <span data-ttu-id="794ed-363">Örneğin, bildirimler</span><span class="sxs-lookup"><span data-stu-id="794ed-363">For example, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public void Paint() {...}
}

class TextBox: Control
{
    new public void Paint() {...}
}
```
<span data-ttu-id="794ed-364">`Paint` yöntemi `TextBox` gizler `Paint` yöntemi `Control`, ancak eşleme değiştirmez `Control.Paint` üzerine `IControl.Paint`ve çağrılar `Paint` sınıfı aracılığıyla örnekleri ve arabirimi örnekleri olur. Aşağıdaki etkileri</span><span class="sxs-lookup"><span data-stu-id="794ed-364">the `Paint` method in `TextBox` hides the `Paint` method in `Control`, but it does not alter the mapping of `Control.Paint` onto `IControl.Paint`, and calls to `Paint` through class instances and interface instances will have the following effects</span></span>
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes Control.Paint();
```

<span data-ttu-id="794ed-365">Bir arabirim yöntemi sanal bir yöntem bir sınıftaki eşlendiğinde ancak arabirim uygulamasını sanal yöntemi geçersiz kılın ve türetilen sınıflar için mümkündür.</span><span class="sxs-lookup"><span data-stu-id="794ed-365">However, when an interface method is mapped onto a virtual method in a class, it is possible for derived classes to override the virtual method and alter the implementation of the interface.</span></span> <span data-ttu-id="794ed-366">Örneğin, yukarıda bildirimi yeniden yazma</span><span class="sxs-lookup"><span data-stu-id="794ed-366">For example, rewriting the declarations above to</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public virtual void Paint() {...}
}

class TextBox: Control
{
    public override void Paint() {...}
}
```
<span data-ttu-id="794ed-367">Aşağıdaki etkileri artık gözlemlenen</span><span class="sxs-lookup"><span data-stu-id="794ed-367">the following effects will now be observed</span></span>
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes TextBox.Paint();
```

<span data-ttu-id="794ed-368">Açık arabirim üyesi uygulaması geçersiz kılmak mümkün değildir, bu yana açık arabirim üyesi uygulamalarını sanal olarak bildirilemez.</span><span class="sxs-lookup"><span data-stu-id="794ed-368">Since explicit interface member implementations cannot be declared virtual, it is not possible to override an explicit interface member implementation.</span></span> <span data-ttu-id="794ed-369">Ancak başka bir yöntem çağırmak açık arabirim üyesi uygulaması için mükemmel bir şekilde geçerli olduğundan ve başka bir yöntem izin vermek için sanal bildirilen, türetilmiş sınıfları geçersiz kılmak için.</span><span class="sxs-lookup"><span data-stu-id="794ed-369">However, it is perfectly valid for an explicit interface member implementation to call another method, and that other method can be declared virtual to allow derived classes to override it.</span></span> <span data-ttu-id="794ed-370">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="794ed-370">For example</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() { PaintControl(); }
    protected virtual void PaintControl() {...}
}

class TextBox: Control
{
    protected override void PaintControl() {...}
}
```

<span data-ttu-id="794ed-371">Burada, türetilmiş sınıflar `Control` yürütmesinin specialize `IControl.Paint` kılarak `PaintControl` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="794ed-371">Here, classes derived from `Control` can specialize the implementation of `IControl.Paint` by overriding the `PaintControl` method.</span></span>

### <a name="interface-re-implementation"></a><span data-ttu-id="794ed-372">Arabiriminin yeniden uygulanmasına</span><span class="sxs-lookup"><span data-stu-id="794ed-372">Interface re-implementation</span></span>

<span data-ttu-id="794ed-373">Bir arabirim uygulaması devralınan bir sınıf için izin verilen ***yeniden uygulama*** temel sınıf listesinde ekleyerek arabirime.</span><span class="sxs-lookup"><span data-stu-id="794ed-373">A class that inherits an interface implementation is permitted to ***re-implement*** the interface by including it in the base class list.</span></span>

<span data-ttu-id="794ed-374">Bir arabirimin bir yeniden uygulanmasına tam olarak aynı arabirimi eşleme kurallarını bir arabirim uygulaması ilk olarak izler.</span><span class="sxs-lookup"><span data-stu-id="794ed-374">A re-implementation of an interface follows exactly the same interface mapping rules as an initial implementation of an interface.</span></span> <span data-ttu-id="794ed-375">Bu nedenle, devralınan arabirimi eşleme, vermemektedir arabirimi eşlemeye arabiriminin yeniden uygulanmasına için oluşturulan hiçbir etkisi olmaz.</span><span class="sxs-lookup"><span data-stu-id="794ed-375">Thus, the inherited interface mapping has no effect whatsoever on the interface mapping established for the re-implementation of the interface.</span></span> <span data-ttu-id="794ed-376">Örneğin, bildirimler</span><span class="sxs-lookup"><span data-stu-id="794ed-376">For example, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() {...}
}

class MyControl: Control, IControl
{
    public void Paint() {}
}
```
<span data-ttu-id="794ed-377">Olgu, `Control` eşler `IControl.Paint` üzerine `Control.IControl.Paint` yeniden uygulamasında etkilemez `MyControl`, hangi haritalar `IControl.Paint` üzerine `MyControl.Paint`.</span><span class="sxs-lookup"><span data-stu-id="794ed-377">the fact that `Control` maps `IControl.Paint` onto `Control.IControl.Paint` doesn't affect the re-implementation in `MyControl`, which maps `IControl.Paint` onto `MyControl.Paint`.</span></span>

<span data-ttu-id="794ed-378">Genel üye bildirimleri ve devralınan açık arabirim üyesi bildirimi yeniden uygulanan arabirimleri için arabirim eşleme işlemi katılmak devralındı.</span><span class="sxs-lookup"><span data-stu-id="794ed-378">Inherited public member declarations and inherited explicit interface member declarations participate in the interface mapping process for re-implemented interfaces.</span></span> <span data-ttu-id="794ed-379">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="794ed-379">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
    void H();
    void I();
}

class Base: IMethods
{
    void IMethods.F() {}
    void IMethods.G() {}
    public void H() {}
    public void I() {}
}

class Derived: Base, IMethods
{
    public void F() {}
    void IMethods.H() {}
}
```

<span data-ttu-id="794ed-380">Burada, uygulanması `IMethods` içinde `Derived` eşleyen arabirim yöntemleri üzerine `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, ve `Base.I`.</span><span class="sxs-lookup"><span data-stu-id="794ed-380">Here, the implementation of `IMethods` in `Derived` maps the interface methods onto `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, and `Base.I`.</span></span>

<span data-ttu-id="794ed-381">Arabirimini uygulayan bir sınıf, arayüz, örtük olarak bu arabirimin temel arabirimleri de uygular.</span><span class="sxs-lookup"><span data-stu-id="794ed-381">When a class implements an interface, it implicitly also implements all of that interface's base interfaces.</span></span> <span data-ttu-id="794ed-382">Benzer şekilde, bir yeniden bir arabirimin de örtülü olarak tüm arabiriminin temel arabirimleri bir yeniden uygulanmasına uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="794ed-382">Likewise, a re-implementation of an interface is also implicitly a re-implementation of all of the interface's base interfaces.</span></span> <span data-ttu-id="794ed-383">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="794ed-383">For example</span></span>
```csharp
interface IBase
{
    void F();
}

interface IDerived: IBase
{
    void G();
}

class C: IDerived
{
    void IBase.F() {...}
    void IDerived.G() {...}
}

class D: C, IDerived
{
    public void F() {...}
    public void G() {...}
}
```

<span data-ttu-id="794ed-384">Burada, yeniden uygulanması `IDerived` de yeniden uygular `IBase`eşleme `IBase.F` üzerine `D.F`.</span><span class="sxs-lookup"><span data-stu-id="794ed-384">Here, the re-implementation of `IDerived` also re-implements `IBase`, mapping `IBase.F` onto `D.F`.</span></span>

### <a name="abstract-classes-and-interfaces"></a><span data-ttu-id="794ed-385">Soyut sınıflar ve arabirimler</span><span class="sxs-lookup"><span data-stu-id="794ed-385">Abstract classes and interfaces</span></span>

<span data-ttu-id="794ed-386">Soyut olmayan sınıfı gibi tüm üyeleri sınıfın temel sınıf listesinde listelenen arabirimler uygulamaları soyut bir sınıf sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="794ed-386">Like a non-abstract class, an abstract class must provide implementations of all members of the interfaces that are listed in the base class list of the class.</span></span> <span data-ttu-id="794ed-387">Ancak, soyut bir sınıf, arabirim yöntemleri soyut yöntemler üzerine eşlemek için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="794ed-387">However, an abstract class is permitted to map interface methods onto abstract methods.</span></span> <span data-ttu-id="794ed-388">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="794ed-388">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    public abstract void F();
    public abstract void G();
}
```

<span data-ttu-id="794ed-389">Burada, uygulanması `IMethods` eşler `F` ve `G` soyut yöntemler hangi kılınmalıdır öğesinden türetilen soyut olmayan sınıflarda `C`.</span><span class="sxs-lookup"><span data-stu-id="794ed-389">Here, the implementation of `IMethods` maps `F` and `G` onto abstract methods, which must be overridden in non-abstract classes that derive from `C`.</span></span>

<span data-ttu-id="794ed-390">Açık arabirim üyesi uygulamalarını soyut olamaz, ancak açık arabirim üyesi uygulamalarını Elbette soyut yöntemlerini çağırmak için izin verilen unutmayın.</span><span class="sxs-lookup"><span data-stu-id="794ed-390">Note that explicit interface member implementations cannot be abstract, but explicit interface member implementations are of course permitted to call abstract methods.</span></span> <span data-ttu-id="794ed-391">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="794ed-391">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    void IMethods.F() { FF(); }
    void IMethods.G() { GG(); }
    protected abstract void FF();
    protected abstract void GG();
}
```

<span data-ttu-id="794ed-392">Burada, türetilen soyut olmayan sınıflar `C` geçersiz kılmak için gerekli olacak `FF` ve `GG`, bu nedenle gerçek uygulamasını sağlayan `IMethods`.</span><span class="sxs-lookup"><span data-stu-id="794ed-392">Here, non-abstract classes that derive from `C` would be required to override `FF` and `GG`, thus providing the actual implementation of `IMethods`.</span></span>
