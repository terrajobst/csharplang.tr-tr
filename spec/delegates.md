---
ms.openlocfilehash: 08c14d9ef2afe30580f456995066c141653ede92
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488978"
---
# <a name="delegates"></a><span data-ttu-id="6bcaf-101">Temsilciler</span><span class="sxs-lookup"><span data-stu-id="6bcaf-101">Delegates</span></span>

<span data-ttu-id="6bcaf-102">Temsilciler, diğer diller senaryoları etkinleştirmek — sahip işlev işaretçilerine, C++, Pascal ve Modula--gibi giderdik.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-102">Delegates enable scenarios that other languages—such as C++, Pascal, and Modula -- have addressed with function pointers.</span></span> <span data-ttu-id="6bcaf-103">C++ işlev işaretçilerinden farklı ancak temsilcileri nesne yönelimli tam olarak, ve hem nesne örneği hem de bir yöntem üye işlevlerinin C++ işaretçileri, temsilciler kapsüller.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-103">Unlike C++ function pointers, however, delegates are fully object oriented, and unlike C++ pointers to member functions, delegates encapsulate both an object instance and a method.</span></span>

<span data-ttu-id="6bcaf-104">Temsilci bildirimi sınıfından türetilen bir sınıf tanımlar `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-104">A delegate declaration defines a class that is derived from the class `System.Delegate`.</span></span> <span data-ttu-id="6bcaf-105">Bir temsilci örneği her biri için çağrılabilen bir varlık olarak adlandırılır yöntemleri, bir veya daha fazla listesi olan çağrı listesine kapsüller.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-105">A delegate instance encapsulates an invocation list, which is a list of one or more methods, each of which is referred to as a callable entity.</span></span> <span data-ttu-id="6bcaf-106">Örneği için yöntemleri, örneği ve bir yöntem örneğine çağrılabilir varlık oluşur.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-106">For instance methods, a callable entity consists of an instance and a method on that instance.</span></span> <span data-ttu-id="6bcaf-107">Statik yöntemler için yalnızca bir yöntem çağrılabilir varlık oluşur.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-107">For static methods, a callable entity consists of just a method.</span></span> <span data-ttu-id="6bcaf-108">Uygun bir bağımsız değişkenler kümesiyle bir temsilci örneği çağırma her temsilcinin çağrılabilir varlıkların belirli bağımsız değişkenler kümesiyle çağrılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-108">Invoking a delegate instance with an appropriate set of arguments causes each of the delegate's callable entities to be invoked with the given set of arguments.</span></span>

<span data-ttu-id="6bcaf-109">Bir ilgi çekici ve kullanışlı bir temsilci örneği bildirin veya desteklemez, kapsülleyen yöntemleri sınıfları hakkında önemli olduğunu özelliğidir; önemli olan bu yöntemleri uyumlu ([temsilci bildirimi](delegates.md#delegate-declarations)) temsilcinin türüne sahip.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-109">An interesting and useful property of a delegate instance is that it does not know or care about the classes of the methods it encapsulates; all that matters is that those methods be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with the delegate's type.</span></span> <span data-ttu-id="6bcaf-110">Bu temsilciler mükemmel "anonim" çağırma için uygun hale getirir.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-110">This makes delegates perfectly suited for "anonymous" invocation.</span></span>

## <a name="delegate-declarations"></a><span data-ttu-id="6bcaf-111">Temsilci bildirimi</span><span class="sxs-lookup"><span data-stu-id="6bcaf-111">Delegate declarations</span></span>

<span data-ttu-id="6bcaf-112">A *delegate_declaration* olduğu bir *type_declaration* ([tür bildirimleri](namespaces.md#type-declarations)), yeni bir temsilci türü bildirir.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-112">A *delegate_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new delegate type.</span></span>

```antlr
delegate_declaration
    : attributes? delegate_modifier* 'delegate' return_type
      identifier variant_type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;

delegate_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | delegate_modifier_unsafe
    ;
```

<span data-ttu-id="6bcaf-113">Aynı değiştiricisi bir temsilci bildiriminde birden çok kez görünmesi için bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-113">It is a compile-time error for the same modifier to appear multiple times in a delegate declaration.</span></span>

<span data-ttu-id="6bcaf-114">`new` Değiştiricisi temsilciler üzerinde yalnızca izin başka bir türde bildirilen, bu durumda, bu tür bir temsilci belirtir devralınmış bir üyeyi aynı adla açıklandığı gizler [yeni değiştiricisini](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="6bcaf-114">The `new` modifier is only permitted on delegates declared within another type, in which case it specifies that such a delegate hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="6bcaf-115">`public`, `protected`, `internal`, Ve `private` değiştiriciler temsilci türünün erişilebilirlik denetimi.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-115">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the delegate type.</span></span> <span data-ttu-id="6bcaf-116">Temsilci bildirimi oluştuğu bağlama bağlı olarak bu değiştiriciler bazıları değil izin verilir ([erişilebilirlik bildirilen](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="6bcaf-116">Depending on the context in which the delegate declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="6bcaf-117">Temsilcinin türü adı *tanımlayıcı*.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-117">The delegate's type name is *identifier*.</span></span>

<span data-ttu-id="6bcaf-118">İsteğe bağlı *formal_parameter_list* temsilcinin parametre belirtir ve *Döndür_tür* temsilcinin dönüş türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-118">The optional *formal_parameter_list* specifies the parameters of the delegate, and *return_type* indicates the return type of the delegate.</span></span>

<span data-ttu-id="6bcaf-119">İsteğe bağlı *variant_type_parameter_list* ([değişken türü parametre listeleri](interfaces.md#variant-type-parameter-lists)) için temsilci tür parametreleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-119">The optional *variant_type_parameter_list* ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)) specifies the type parameters to the delegate itself.</span></span>

<span data-ttu-id="6bcaf-120">Dönüş türü bir temsilci türü olmalıdır `void`, veya çıkış için güvenli ([varyansı güvenliği](interfaces.md#variance-safety)).</span><span class="sxs-lookup"><span data-stu-id="6bcaf-120">The return type of a delegate type must be either `void`, or output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span>

<span data-ttu-id="6bcaf-121">Tüm biçimsel parametre türleri temsilci türü giriş uyumlu olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-121">All the formal parameter types of a delegate type must be input-safe.</span></span> <span data-ttu-id="6bcaf-122">Ayrıca, tüm `out` veya `ref` parametre türleri de çıkış uyumlu olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-122">Additionally, any `out` or `ref` parameter types must also be output-safe.</span></span> <span data-ttu-id="6bcaf-123">Not, hatta `out` giriş güvenli bir temel yürütme platform sınırlaması nedeniyle olmasını gerekli parametreler.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-123">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="6bcaf-124">C# ' temsilci türleri olan eşdeğer, yapısal olarak eşdeğer ad.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-124">Delegate types in C# are name equivalent, not structurally equivalent.</span></span> <span data-ttu-id="6bcaf-125">Özellikle, aynı parametreye sahip iki farklı temsilci türleri listeler ve dönüş türü farklı bir temsilci türleri olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-125">Specifically, two different delegate types that have the same parameter lists and return type are considered different delegate types.</span></span> <span data-ttu-id="6bcaf-126">Ancak, iki farklı ancak yapısal olarak eşdeğer temsilci türleri örneklerini eşit olarak karşılaştırılır ([temsilci eşitlik işleçleri](expressions.md#delegate-equality-operators)).</span><span class="sxs-lookup"><span data-stu-id="6bcaf-126">However, instances of two distinct but structurally equivalent delegate types may compare as equal ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="6bcaf-127">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6bcaf-127">For example:</span></span>

```csharp
delegate int D1(int i, double d);

class A
{
    public static int M1(int a, double b) {...}
}

class B
{
    delegate int D2(int c, double d);
    public static int M1(int f, double g) {...}
    public static void M2(int k, double l) {...}
    public static int M3(int g) {...}
    public static void M4(int g) {...}
}
```

<span data-ttu-id="6bcaf-128">Yöntemleri `A.M1` ve `B.M1 `her iki temsilci türleriyle uyumlu `D1` ve `D2` , sahip oldukları olduğundan aynı dönüş türü ve parametre listesi; ancak, bu temsilci türleri olmadıkları için iki farklı türleri olan değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-128">The methods `A.M1` and `B.M1 `are compatible with both the delegate types `D1` and `D2` , since they have the same return type and parameter list; however, these delegate types are two different types, so they are not interchangeable.</span></span> <span data-ttu-id="6bcaf-129">Yöntemleri `B.M2`, `B.M3`, ve `B.M4` temsilci türleriyle uyumsuz `D1` ve `D2`, parametre listeleri ya da farklı dönüş türlerine sahip olduğu.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-129">The methods `B.M2`, `B.M3`, and `B.M4` are incompatible with the delegate types `D1` and `D2`, since they have different return types or parameter lists.</span></span>

<span data-ttu-id="6bcaf-130">Diğer genel tür bildirimleri gibi bir oluşturulmuş bir temsilci türü oluşturmak için tür bağımsız değişkenleri verilmelidir.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-130">Like other generic type declarations, type arguments must be given to create a constructed delegate type.</span></span> <span data-ttu-id="6bcaf-131">Parametre türleri ve dönüş türü bir oluşturulmuş bir temsilci türü, her tür parametresi temsilci bildirimi için karşılık gelen tür bağımsız değişkeni oluşturulmuş bir temsilci türünün getirilmesiyle oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-131">The parameter types and return type of a constructed delegate type are created by substituting, for each type parameter in the delegate declaration, the corresponding type argument of the constructed delegate type.</span></span> <span data-ttu-id="6bcaf-132">Parametre türleri ve sonuçta elde edilen dönüş türünü belirlemede yöntemleri oluşturulmuş temsilci türüyle uyumlu kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-132">The resulting return type and parameter types are used in determining what methods are compatible with a constructed delegate type.</span></span> <span data-ttu-id="6bcaf-133">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6bcaf-133">For example:</span></span>

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

<span data-ttu-id="6bcaf-134">Yöntem `X.F` temsilci türüyle uyumlu `Predicate<int>` ve yöntem `X.G` temsilci türüyle uyumlu `Predicate<string>` .</span><span class="sxs-lookup"><span data-stu-id="6bcaf-134">The method `X.F` is compatible with the delegate type `Predicate<int>` and the method `X.G` is compatible with the delegate type `Predicate<string>` .</span></span>

<span data-ttu-id="6bcaf-135">Aracılığıyla bir temsilci türü bildirmek için tek yolu olan bir *delegate_declaration*.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-135">The only way to declare a delegate type is via a *delegate_declaration*.</span></span> <span data-ttu-id="6bcaf-136">Bir temsilci türü türetilen bir sınıf türüdür `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-136">A delegate type is a class type that is derived from `System.Delegate`.</span></span> <span data-ttu-id="6bcaf-137">Örtük olarak temsilci türleri olan `sealed`, herhangi bir tür bir temsilci türünden türetilmesi döndürülmesine izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-137">Delegate types are implicitly `sealed`, so it is not permissible to derive any type from a delegate type.</span></span> <span data-ttu-id="6bcaf-138">Ayrıca bir temsilci olmayan sınıf türünden türetmek için izin verilen değil `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-138">It is also not permissible to derive a non-delegate class type from `System.Delegate`.</span></span> <span data-ttu-id="6bcaf-139">Unutmayın `System.Delegate` kendisi bir temsilci türü; tüm temsilci türleri türetildiği bir sınıf türü bir değer.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-139">Note that `System.Delegate` is not itself a delegate type; it is a class type from which all delegate types are derived.</span></span>

<span data-ttu-id="6bcaf-140">C# özel sözdizimi için temsilci örnek oluşturma ve çağırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-140">C# provides special syntax for delegate instantiation and invocation.</span></span> <span data-ttu-id="6bcaf-141">Örnek oluşturma dışında bir sınıf veya sınıf örneği uygulanabilir herhangi bir işlem de bir temsilci sınıfı veya örneği, sırasıyla uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-141">Except for instantiation, any operation that can be applied to a class or class instance can also be applied to a delegate class or instance, respectively.</span></span> <span data-ttu-id="6bcaf-142">Özellikle, üyelere erişim mümkündür `System.Delegate` normal üye erişimi sözdizimini aracılığıyla türü.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-142">In particular, it is possible to access members of the `System.Delegate` type via the usual member access syntax.</span></span>

<span data-ttu-id="6bcaf-143">Bir temsilci örneği tarafından kapsüllenmiş yöntem kümesi çağrı listesine çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-143">The set of methods encapsulated by a delegate instance is called an invocation list.</span></span> <span data-ttu-id="6bcaf-144">Bir temsilci örneği oluşturulduğunda ([temsilci Uyumluluk](delegates.md#delegate-compatibility)) isteğe bağlı olarak tek bir yöntemi, bu yöntem kapsüller ve çağırma listesinde yalnızca bir giriş içeriyor.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-144">When a delegate instance is created ([Delegate compatibility](delegates.md#delegate-compatibility)) from a single method, it encapsulates that method, and its invocation list contains only one entry.</span></span> <span data-ttu-id="6bcaf-145">İki null olmayan temsilci örneklerini bir araya geldiğinde, ancak çağırma listelerini--iki veya daha fazla giriş içeren yeni bir çağırma listesi oluşturmak için işlenen sonra sağ işlenen--sol sırayla bitiştirilir.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-145">However, when two non-null delegate instances are combined, their invocation lists are concatenated -- in the order left operand then right operand -- to form a new invocation list, which contains two or more entries.</span></span>

<span data-ttu-id="6bcaf-146">Temsilciler, ikili kullanılarak birleştirilir `+` ([Toplama işleci](expressions.md#addition-operator)) ve `+=` işleçleri ([bileşik atama](expressions.md#compound-assignment)).</span><span class="sxs-lookup"><span data-stu-id="6bcaf-146">Delegates are combined using the binary `+` ([Addition operator](expressions.md#addition-operator)) and `+=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="6bcaf-147">Bir temsilci bir bileşimini kullanarak ikili temsilcilerin kaldırılabilir `-` ([çıkarma işleci](expressions.md#subtraction-operator)) ve `-=` işleçleri ([bileşik atama](expressions.md#compound-assignment)).</span><span class="sxs-lookup"><span data-stu-id="6bcaf-147">A delegate can be removed from a combination of delegates, using the binary `-` ([Subtraction operator](expressions.md#subtraction-operator)) and `-=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="6bcaf-148">Temsilciler, eşitlik için karşılaştırılabilir ([temsilci eşitlik işleçleri](expressions.md#delegate-equality-operators)).</span><span class="sxs-lookup"><span data-stu-id="6bcaf-148">Delegates can be compared for equality ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="6bcaf-149">Aşağıdaki örnek temsilcilerin sayısına örneğinin gösterir ve bunların karşılık gelen çağırma listeler:</span><span class="sxs-lookup"><span data-stu-id="6bcaf-149">The following example shows the instantiation of a number of delegates, and their corresponding invocation lists:</span></span>

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public static void M2(int i) {...}

}

class Test
{
    static void Main() {
        D cd1 = new D(C.M1);      // M1
        D cd2 = new D(C.M2);      // M2
        D cd3 = cd1 + cd2;        // M1 + M2
        D cd4 = cd3 + cd1;        // M1 + M2 + M1
        D cd5 = cd4 + cd3;        // M1 + M2 + M1 + M1 + M2
    }

}
```

<span data-ttu-id="6bcaf-150">Zaman `cd1` ve `cd2` olan örneği, her bir yöntem kapsüller.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-150">When `cd1` and `cd2` are instantiated, they each encapsulate one method.</span></span> <span data-ttu-id="6bcaf-151">Zaman `cd3` olan örneği, iki yönteme bir çağrı listesine sahiptir `M1` ve `M2`bu sırası.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-151">When `cd3` is instantiated, it has an invocation list of two methods, `M1` and `M2`, in that order.</span></span> <span data-ttu-id="6bcaf-152">`cd4`ın çağırma listesi içeren `M1`, `M2`, ve `M1`bu sırası.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-152">`cd4`'s invocation list contains `M1`, `M2`, and `M1`, in that order.</span></span> <span data-ttu-id="6bcaf-153">Son olarak, `cd5`'s çağırma listesi içeren `M1`, `M2`, `M1`, `M1`, ve `M2`bu sırası.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-153">Finally, `cd5`'s invocation list contains `M1`, `M2`, `M1`, `M1`, and `M2`, in that order.</span></span> <span data-ttu-id="6bcaf-154">Temsilciler (de olarak kaldırma) birleştirerek daha fazla örnek için bkz: [temsilci çağırma](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="6bcaf-154">For more examples of combining (as well as removing) delegates, see [Delegate invocation](delegates.md#delegate-invocation).</span></span>

## <a name="delegate-compatibility"></a><span data-ttu-id="6bcaf-155">Temsilci uyumluluğu</span><span class="sxs-lookup"><span data-stu-id="6bcaf-155">Delegate compatibility</span></span>

<span data-ttu-id="6bcaf-156">Bir yöntem veya temsilci `M` olduğu ***uyumlu*** bir temsilci türüyle `D` aşağıdakilerin tümü doğru olduğunda:</span><span class="sxs-lookup"><span data-stu-id="6bcaf-156">A method or delegate `M` is ***compatible*** with a delegate type `D` if all of the following are true:</span></span>

*  <span data-ttu-id="6bcaf-157">`D` ve `M` parametreleri ve her parametresinde aynı sayıda `D` aynı `ref` veya `out` karşılık gelen parametre olarak değiştiriciler `M`.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-157">`D` and `M` have the same number of parameters, and each parameter in `D` has the same `ref` or `out` modifiers as the corresponding parameter in `M`.</span></span>
*  <span data-ttu-id="6bcaf-158">Her değer parametresi için (parametre olmadan `ref` veya `out` değiştiricisi), bir kimlik dönüştürme ([kimlik dönüştürme](conversions.md#identity-conversion)) ya da örtük bir başvuru dönüştürmesi ([örtükbirbaşvurudönüşümleri](conversions.md#implicit-reference-conversions)) parametre türünden var. `D` karşılık gelen parametre türüne `M`.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-158">For each value parameter (a parameter with no `ref` or `out` modifier), an identity conversion ([Identity conversion](conversions.md#identity-conversion)) or implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the parameter type in `D` to the corresponding parameter type in `M`.</span></span>
*  <span data-ttu-id="6bcaf-159">Her `ref` veya `out` içinde tür parametresi, parametre `D` parametre türü aynı `M`.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-159">For each `ref` or `out` parameter, the parameter type in `D` is the same as the parameter type in `M`.</span></span>
*  <span data-ttu-id="6bcaf-160">Dönüş türünü bir kimlik veya örtük bir başvuru dönüştürmesi var `M` dönüş türüne `D`.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-160">An identity or implicit reference conversion exists from the return type of `M` to the return type of `D`.</span></span>

## <a name="delegate-instantiation"></a><span data-ttu-id="6bcaf-161">Temsilci örneği oluşturma</span><span class="sxs-lookup"><span data-stu-id="6bcaf-161">Delegate instantiation</span></span>

<span data-ttu-id="6bcaf-162">Bir temsilci örneği tarafından oluşturulan bir *delegate_creation_expression* ([temsilci oluşturma ifadeleri](expressions.md#delegate-creation-expressions)) veya bir temsilci türüne dönüştürme.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-162">An instance of a delegate is created by a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) or a conversion to a delegate type.</span></span> <span data-ttu-id="6bcaf-163">Yeni oluşturulan temsilci örneği sonra ya da ifade eder:</span><span class="sxs-lookup"><span data-stu-id="6bcaf-163">The newly created delegate instance then refers to either:</span></span>

*  <span data-ttu-id="6bcaf-164">Başvurulan statik yöntem *delegate_creation_expression*, veya</span><span class="sxs-lookup"><span data-stu-id="6bcaf-164">The static method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="6bcaf-165">Hedef nesne (olamaz `null`) ve örnek yöntemi içinde başvurulan *delegate_creation_expression*, veya</span><span class="sxs-lookup"><span data-stu-id="6bcaf-165">The target object (which cannot be `null`) and instance method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="6bcaf-166">Başka bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-166">Another delegate.</span></span>

<span data-ttu-id="6bcaf-167">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6bcaf-167">For example:</span></span>

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public void M2(int i) {...}
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);        // static method
        C t = new C();
        D cd2 = new D(t.M2);        // instance method
        D cd3 = new D(cd2);        // another delegate
    }
}
```

<span data-ttu-id="6bcaf-168">Oluşturulduktan sonra temsilci örneklerini her zaman aynı hedef nesne ve yöntem bakın.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-168">Once instantiated, delegate instances always refer to the same target object and method.</span></span> <span data-ttu-id="6bcaf-169">Unutmayın, ne zaman iki temsilci olduğunda birleştirilir veya bir başka bir yeni bir temsilci sonuçları kendi çağırma listesiyle kaldırılır; Birleştirilen ya da kaldırılmış temsilcileri çağırma listelerini değişmeden kalır.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-169">Remember, when two delegates are combined, or one is removed from another, a new delegate results with its own invocation list; the invocation lists of the delegates combined or removed remain unchanged.</span></span>

## <a name="delegate-invocation"></a><span data-ttu-id="6bcaf-170">Temsilci çağrısı</span><span class="sxs-lookup"><span data-stu-id="6bcaf-170">Delegate invocation</span></span>

<span data-ttu-id="6bcaf-171">C# bir temsilci çağırma için özel bir sözdizimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-171">C# provides special syntax for invoking a delegate.</span></span> <span data-ttu-id="6bcaf-172">Bir null olmayan temsilci örneği olan çağrı listesine bir giriş içerir çağrıldığında verildi ve başvurulan aynı değeri döndürür aynı bağımsız değişkenlerle bir yöntem çağırdığı için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-172">When a non-null delegate instance whose invocation list contains one entry is invoked, it invokes the one method with the same arguments it was given, and returns the same value as the referred to method.</span></span> <span data-ttu-id="6bcaf-173">(Bkz [temsilci çağrılarını](expressions.md#delegate-invocations) temsilci çağrısı hakkında ayrıntılı bilgi için.) Bu yöntem doğrudan adında gibi bir temsilcinin çağırma sırasında bir özel durum oluşur ve çağrıldı yöntemi içinde özel durum yakalandı, arama gibi özel durum yakalama yan tümcesi için temsilci çağıran yöntemi devam eder yöntemi, temsilci olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-173">(See [Delegate invocations](expressions.md#delegate-invocations) for detailed information on delegate invocation.) If an exception occurs during the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, as if that method had directly called the method to which that delegate referred.</span></span>

<span data-ttu-id="6bcaf-174">Çağrı, çağırma listesinde birden fazla varlık içeriyor. bir temsilci örneği, çağırma listesinde, yöntemlerin her biri zaman uyumlu olarak, sırasıyla çağırarak devam eder.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-174">Invocation of a delegate instance whose invocation list contains multiple entries proceeds by invoking each of the methods in the invocation list, synchronously, in order.</span></span> <span data-ttu-id="6bcaf-175">Yorumun her yöntem için temsilci örneği sağlanmadı olarak bağımsız değişkenler aynı dizi geçirilir.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-175">Each method so called is passed the same set of arguments as was given to the delegate instance.</span></span> <span data-ttu-id="6bcaf-176">Bu tür bir temsilci çağrısı başvuru parametreleri içeriyorsa ([başvuru parametreleri](classes.md#reference-parameters)), her yöntem çağırma aynı değişken başvuru ile meydana gelir; bu değişkeni bir yöntem çağırma listesinde tarafından yapılan olacaktır çağırma listeyi daha ayrıntılı yöntemler için görünür.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-176">If such a delegate invocation includes reference parameters ([Reference parameters](classes.md#reference-parameters)), each method invocation will occur with a reference to the same variable; changes to that variable by one method in the invocation list will be visible to methods further down the invocation list.</span></span> <span data-ttu-id="6bcaf-177">Temsilci çağrısı çıkış parametreleri veya dönüş değeri içeriyorsa, bunların son değerini listedeki son temsilcinin çağırma kaynağından gelir.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-177">If the delegate invocation includes output parameters or a return value, their final value will come from the invocation of the last delegate in the list.</span></span>

<span data-ttu-id="6bcaf-178">Bu tür bir temsilci çağrısı işlenirken bir özel durum oluşur ve çağrıldı yöntemi içinde özel durum yakalandı, temsilci çağrılan yöntemi bir özel durum yakalama yan tümcesi için arama işlemi devam eder ve herhangi bir yöntem daha da aşağı değil çağırma listesi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-178">If an exception occurs during processing of the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, and any methods further down the invocation list are not invoked.</span></span>

<span data-ttu-id="6bcaf-179">Türünde bir özel durum'içinde null sonuç değeri olan bir temsilci örneği çağırmaya çalışırken `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-179">Attempting to invoke a delegate instance whose value is null results in an exception of type `System.NullReferenceException`.</span></span>

<span data-ttu-id="6bcaf-180">Aşağıdaki örnek, örneği, birleştirmek, kaldırmak ve temsilci çağırma gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="6bcaf-180">The following example shows how to instantiate, combine, remove, and invoke delegates:</span></span>

```csharp
using System;

delegate void D(int x);

class C
{
    public static void M1(int i) {
        Console.WriteLine("C.M1: " + i);
    }

    public static void M2(int i) {
        Console.WriteLine("C.M2: " + i);
    }

    public void M3(int i) {
        Console.WriteLine("C.M3: " + i);
    }
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);
        cd1(-1);                // call M1

        D cd2 = new D(C.M2);
        cd2(-2);                // call M2

        D cd3 = cd1 + cd2;
        cd3(10);                // call M1 then M2

        cd3 += cd1;
        cd3(20);                // call M1, M2, then M1

        C c = new C();
        D cd4 = new D(c.M3);
        cd3 += cd4;
        cd3(30);                // call M1, M2, M1, then M3

        cd3 -= cd1;             // remove last M1
        cd3(40);                // call M1, M2, then M3

        cd3 -= cd4;
        cd3(50);                // call M1 then M2

        cd3 -= cd2;
        cd3(60);                // call M1

        cd3 -= cd2;             // impossible removal is benign
        cd3(60);                // call M1

        cd3 -= cd1;             // invocation list is empty so cd3 is null

        cd3(70);                // System.NullReferenceException thrown

        cd3 -= cd1;             // impossible removal is benign
    }
}
```

<span data-ttu-id="6bcaf-181">İfadede gösterildiği `cd3 += cd1;`, bir temsilci birden çok kez bir çağırma listesinde var olabilir.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-181">As shown in the statement `cd3 += cd1;`, a delegate can be present in an invocation list multiple times.</span></span> <span data-ttu-id="6bcaf-182">Bu durumda, yalnızca bir kez örneği çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-182">In this case, it is simply invoked once per occurrence.</span></span> <span data-ttu-id="6bcaf-183">Bu temsilci kaldırıldığında, bunun gibi bir çağırma listesinde çağırma listedeki son oluşum gerçekten kaldırıldı bir bileşendir.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-183">In an invocation list such as this, when that delegate is removed, the last occurrence in the invocation list is the one actually removed.</span></span>

<span data-ttu-id="6bcaf-184">Son deyiminin yürütülmeden hemen önce `cd3 -= cd1;`, temsilci `cd3` bir boş çağırma listesine başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-184">Immediately prior to the execution of the final statement, `cd3 -= cd1;`, the delegate `cd3` refers to an empty invocation list.</span></span> <span data-ttu-id="6bcaf-185">Bir temsilci boş listeden kaldırmak için (veya var olmayan bir temsilci boş listeden kaldırmak için) çalışırken bir hata değildir.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-185">Attempting to remove a delegate from an empty list (or to remove a non-existent delegate from a non-empty list) is not an error.</span></span>

<span data-ttu-id="6bcaf-186">Oluşturulan çıktı.</span><span class="sxs-lookup"><span data-stu-id="6bcaf-186">The output produced is:</span></span>

```
C.M1: -1
C.M2: -2
C.M1: 10
C.M2: 10
C.M1: 20
C.M2: 20
C.M1: 20
C.M1: 30
C.M2: 30
C.M1: 30
C.M3: 30
C.M1: 40
C.M2: 40
C.M3: 40
C.M1: 50
C.M2: 50
C.M1: 60
C.M1: 60
```
