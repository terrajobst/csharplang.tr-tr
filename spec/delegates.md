---
ms.openlocfilehash: d162d4b6a489032dcdfca9094a39d88fd03d4013
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704095"
---
# <a name="delegates"></a><span data-ttu-id="951a1-101">Temsilciler</span><span class="sxs-lookup"><span data-stu-id="951a1-101">Delegates</span></span>

<span data-ttu-id="951a1-102">Temsilciler C++, diğer dillerin (örneğin, Pascal ve modüla) işlev işaretçileriyle ilgilenmesini sağlayan senaryolara olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="951a1-102">Delegates enable scenarios that other languages—such as C++, Pascal, and Modula -- have addressed with function pointers.</span></span> <span data-ttu-id="951a1-103">Ancak C++ , işlev işaretçilerinden farklı olarak, temsilciler tam nesne yönelimlidir ve C++ üye işlevlerine yönelik işaretçilerin aksine, temsilciler hem bir nesne örneğini hem de bir yöntemi kapsüller.</span><span class="sxs-lookup"><span data-stu-id="951a1-103">Unlike C++ function pointers, however, delegates are fully object oriented, and unlike C++ pointers to member functions, delegates encapsulate both an object instance and a method.</span></span>

<span data-ttu-id="951a1-104">Temsilci bildirimi `System.Delegate` sınıfından türetilmiş bir sınıfı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="951a1-104">A delegate declaration defines a class that is derived from the class `System.Delegate`.</span></span> <span data-ttu-id="951a1-105">Bir temsilci örneği, her biri çağrılabilir bir varlık olarak adlandırılan bir veya daha fazla yöntemin listesi olan bir çağırma listesini kapsüller.</span><span class="sxs-lookup"><span data-stu-id="951a1-105">A delegate instance encapsulates an invocation list, which is a list of one or more methods, each of which is referred to as a callable entity.</span></span> <span data-ttu-id="951a1-106">Örnek yöntemleri için, çağrılabilir bir varlık bir örnekten ve bu örnekteki yöntemden oluşur.</span><span class="sxs-lookup"><span data-stu-id="951a1-106">For instance methods, a callable entity consists of an instance and a method on that instance.</span></span> <span data-ttu-id="951a1-107">Statik yöntemler için, çağrılabilir bir varlık yalnızca bir yöntemden oluşur.</span><span class="sxs-lookup"><span data-stu-id="951a1-107">For static methods, a callable entity consists of just a method.</span></span> <span data-ttu-id="951a1-108">Uygun bir bağımsız değişken kümesiyle bir temsilci örneği çağırmak, her bir temsilcinin çağrılabilir varlıkların belirtilen bağımsız değişken kümesiyle çağrılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="951a1-108">Invoking a delegate instance with an appropriate set of arguments causes each of the delegate's callable entities to be invoked with the given set of arguments.</span></span>

<span data-ttu-id="951a1-109">Bir temsilci örneğinin ilginç ve yararlı bir özelliği, kapsülleyen yöntemlerin sınıflarını bilmiyor veya ilgilenmez; Bu yöntemlerin tümünün, temsilcinin türüyle uyumlu olması ([temsilci bildirimleri](delegates.md#delegate-declarations)).</span><span class="sxs-lookup"><span data-stu-id="951a1-109">An interesting and useful property of a delegate instance is that it does not know or care about the classes of the methods it encapsulates; all that matters is that those methods be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with the delegate's type.</span></span> <span data-ttu-id="951a1-110">Bu, temsilcileri "anonim" çağrı için mükemmel bir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="951a1-110">This makes delegates perfectly suited for "anonymous" invocation.</span></span>

## <a name="delegate-declarations"></a><span data-ttu-id="951a1-111">Temsilci bildirimleri</span><span class="sxs-lookup"><span data-stu-id="951a1-111">Delegate declarations</span></span>

<span data-ttu-id="951a1-112">Bir *delegate_declaration* , yeni bir temsilci türü bildiren bir *type_declaration* ([tür bildirimleri](namespaces.md#type-declarations)).</span><span class="sxs-lookup"><span data-stu-id="951a1-112">A *delegate_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new delegate type.</span></span>

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

<span data-ttu-id="951a1-113">Aynı değiştiricinin bir temsilci bildiriminde birden çok kez görünmesi için derleme zamanı hatası vardır.</span><span class="sxs-lookup"><span data-stu-id="951a1-113">It is a compile-time error for the same modifier to appear multiple times in a delegate declaration.</span></span>

<span data-ttu-id="951a1-114">@No__t-0 değiştiricisine yalnızca başka bir tür içinde belirtilen Temsilcilerde izin verilir. Bu durumda, bu tür bir temsilcinin [Yeni değiştiricide](classes.md#the-new-modifier)açıklandığı gibi aynı ada sahip bir devralınmış üyeyi gizlediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="951a1-114">The `new` modifier is only permitted on delegates declared within another type, in which case it specifies that such a delegate hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="951a1-115">@No__t-0, `protected`, `internal` ve `private` değiştiricileri temsilci türünün erişilebilirliğini denetler.</span><span class="sxs-lookup"><span data-stu-id="951a1-115">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the delegate type.</span></span> <span data-ttu-id="951a1-116">Temsilci bildiriminin gerçekleştiği içeriğe bağlı olarak, bu değiştiricilerin bazılarına izin verilmeyebilir ([belirtilen erişilebilirlik](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="951a1-116">Depending on the context in which the delegate declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="951a1-117">Temsilcinin tür adı *tanımlayıcıdır*.</span><span class="sxs-lookup"><span data-stu-id="951a1-117">The delegate's type name is *identifier*.</span></span>

<span data-ttu-id="951a1-118">İsteğe bağlı *formal_parameter_list* , temsilcinin parametrelerini belirtir ve *Sbayrak* , temsilcinin dönüş türünü gösterir.</span><span class="sxs-lookup"><span data-stu-id="951a1-118">The optional *formal_parameter_list* specifies the parameters of the delegate, and *return_type* indicates the return type of the delegate.</span></span>

<span data-ttu-id="951a1-119">İsteğe bağlı *variant_type_parameter_list* ([değişken türü parametre listeleri](interfaces.md#variant-type-parameter-lists)), temsilcinin kendisi için tür parametrelerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="951a1-119">The optional *variant_type_parameter_list* ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)) specifies the type parameters to the delegate itself.</span></span>

<span data-ttu-id="951a1-120">Bir temsilci türünün dönüş türü `void` ya da çıkış açısından güvenli ([varyans güvenliği](interfaces.md#variance-safety)) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="951a1-120">The return type of a delegate type must be either `void`, or output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span>

<span data-ttu-id="951a1-121">Bir temsilci türünün tüm biçimsel parametre türleri giriş açısından güvenli olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="951a1-121">All the formal parameter types of a delegate type must be input-safe.</span></span> <span data-ttu-id="951a1-122">Ayrıca, `out` veya `ref` parametre türlerinin Ayrıca çıkış açısından güvenli olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="951a1-122">Additionally, any `out` or `ref` parameter types must also be output-safe.</span></span> <span data-ttu-id="951a1-123">Temel yürütme platformunun sınırlamasından dolayı, `out` parametrelerinin de giriş açısından güvenli olması gerektiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="951a1-123">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="951a1-124">İçindeki C# temsilci türleri, yapısal olarak eşdeğer değil, ad eşdeğerdedir.</span><span class="sxs-lookup"><span data-stu-id="951a1-124">Delegate types in C# are name equivalent, not structurally equivalent.</span></span> <span data-ttu-id="951a1-125">Özellikle, aynı parametre listelerine ve dönüş türüne sahip iki farklı temsilci türü farklı temsilci türleri olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="951a1-125">Specifically, two different delegate types that have the same parameter lists and return type are considered different delegate types.</span></span> <span data-ttu-id="951a1-126">Ancak, iki ayrı ancak yapısal olarak eşdeğer temsilci türünün örnekleri eşit ([temsilci eşitlik işleçleri](expressions.md#delegate-equality-operators)) olarak karşılaştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="951a1-126">However, instances of two distinct but structurally equivalent delegate types may compare as equal ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="951a1-127">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="951a1-127">For example:</span></span>

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

<span data-ttu-id="951a1-128">@No__t-0 ve `B.M1` yöntemleri aynı dönüş türü ve parametre listesine sahip oldukları için hem `D1` hem de `D2` temsilci türleriyle uyumludur; Bununla birlikte, bu temsilci türleri iki farklı türlerdir, bu nedenle bunlar birbirinin yerine kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="951a1-128">The methods `A.M1` and `B.M1` are compatible with both the delegate types `D1` and `D2` , since they have the same return type and parameter list; however, these delegate types are two different types, so they are not interchangeable.</span></span> <span data-ttu-id="951a1-129">@No__t-0, `B.M3` ve `B.M4` yöntemleri farklı dönüş türlerine veya parametre listelerine sahip olduklarından, `D1` ve `D2` temsilci türleriyle uyumsuzdur.</span><span class="sxs-lookup"><span data-stu-id="951a1-129">The methods `B.M2`, `B.M3`, and `B.M4` are incompatible with the delegate types `D1` and `D2`, since they have different return types or parameter lists.</span></span>

<span data-ttu-id="951a1-130">Diğer genel tür bildirimleri gibi, oluşturulmuş bir temsilci türü oluşturmak için tür bağımsız değişkenleri verilmelidir.</span><span class="sxs-lookup"><span data-stu-id="951a1-130">Like other generic type declarations, type arguments must be given to create a constructed delegate type.</span></span> <span data-ttu-id="951a1-131">Oluşturulmuş bir temsilci türünün parametre türleri ve dönüş türü, temsilci bildirimindeki her bir tür parametresi için yerine, oluşturulan temsilci türünün karşılık gelen tür bağımsız değişkeni ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="951a1-131">The parameter types and return type of a constructed delegate type are created by substituting, for each type parameter in the delegate declaration, the corresponding type argument of the constructed delegate type.</span></span> <span data-ttu-id="951a1-132">Elde edilen dönüş türü ve parametre türleri, oluşturulan bir temsilci türüyle hangi yöntemlerin uyumlu olduğunu belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="951a1-132">The resulting return type and parameter types are used in determining what methods are compatible with a constructed delegate type.</span></span> <span data-ttu-id="951a1-133">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="951a1-133">For example:</span></span>

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

<span data-ttu-id="951a1-134">@No__t-0 yöntemi, `Predicate<int>` temsilci türüyle uyumludur ve `X.G` yöntemi `Predicate<string>` temsilci türüyle uyumludur.</span><span class="sxs-lookup"><span data-stu-id="951a1-134">The method `X.F` is compatible with the delegate type `Predicate<int>` and the method `X.G` is compatible with the delegate type `Predicate<string>` .</span></span>

<span data-ttu-id="951a1-135">Bir temsilci türü bildirmenin tek yolu bir *delegate_declaration*aracılığıyla yapılır.</span><span class="sxs-lookup"><span data-stu-id="951a1-135">The only way to declare a delegate type is via a *delegate_declaration*.</span></span> <span data-ttu-id="951a1-136">Temsilci türü `System.Delegate` ' dan türetilen bir sınıf türüdür.</span><span class="sxs-lookup"><span data-stu-id="951a1-136">A delegate type is a class type that is derived from `System.Delegate`.</span></span> <span data-ttu-id="951a1-137">Temsilci türleri örtük olarak `sealed` olduğundan, temsilci türünden herhangi bir tür türetmeye izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="951a1-137">Delegate types are implicitly `sealed`, so it is not permissible to derive any type from a delegate type.</span></span> <span data-ttu-id="951a1-138">Ayrıca, `System.Delegate` ' dan temsilci olmayan bir sınıf türü türetmeye izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="951a1-138">It is also not permissible to derive a non-delegate class type from `System.Delegate`.</span></span> <span data-ttu-id="951a1-139">@No__t-0 ' ın kendisi bir temsilci türü değil; Bu, tüm temsilci türlerinin türetildiği bir sınıf türüdür.</span><span class="sxs-lookup"><span data-stu-id="951a1-139">Note that `System.Delegate` is not itself a delegate type; it is a class type from which all delegate types are derived.</span></span>

<span data-ttu-id="951a1-140">C#temsilci örneklemesi ve çağrısı için özel sözdizimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="951a1-140">C# provides special syntax for delegate instantiation and invocation.</span></span> <span data-ttu-id="951a1-141">Örnek oluşturma haricinde, bir sınıfa veya sınıf örneğine uygulanabilen her türlü işlem sırasıyla temsilci sınıfına veya örneğe de uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="951a1-141">Except for instantiation, any operation that can be applied to a class or class instance can also be applied to a delegate class or instance, respectively.</span></span> <span data-ttu-id="951a1-142">Özellikle, `System.Delegate` türünde üyelere olağan üye erişim sözdizimi aracılığıyla erişmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="951a1-142">In particular, it is possible to access members of the `System.Delegate` type via the usual member access syntax.</span></span>

<span data-ttu-id="951a1-143">Bir temsilci örneğiyle kapsüllenmiş yöntemlerin kümesine çağrı listesi denir.</span><span class="sxs-lookup"><span data-stu-id="951a1-143">The set of methods encapsulated by a delegate instance is called an invocation list.</span></span> <span data-ttu-id="951a1-144">Tek bir yöntemden bir temsilci örneği oluşturulduğunda ([temsilci uyumluluğu](delegates.md#delegate-compatibility)), bu yöntemi kapsüller ve çağırma listesi yalnızca bir giriş içerir.</span><span class="sxs-lookup"><span data-stu-id="951a1-144">When a delegate instance is created ([Delegate compatibility](delegates.md#delegate-compatibility)) from a single method, it encapsulates that method, and its invocation list contains only one entry.</span></span> <span data-ttu-id="951a1-145">Bununla birlikte, iki null olmayan temsilci örneği birleştirildiğinde, iki veya daha fazla giriş içeren yeni bir çağırma listesi oluşturmak için, bu, iki veya daha fazla girişi içeren yeni bir çağırma listesi oluşturmak üzere, çağrı listeleri birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="951a1-145">However, when two non-null delegate instances are combined, their invocation lists are concatenated -- in the order left operand then right operand -- to form a new invocation list, which contains two or more entries.</span></span>

<span data-ttu-id="951a1-146">Temsilciler `+` ([toplama işleci](expressions.md#addition-operator)) ve `+=` Işleçleri ([bileşik atama](expressions.md#compound-assignment)) kullanılarak birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="951a1-146">Delegates are combined using the binary `+` ([Addition operator](expressions.md#addition-operator)) and `+=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="951a1-147">Bir temsilci, `-` ([çıkarma işleci](expressions.md#subtraction-operator)) ve `-=` Işleçleri ([bileşik atama](expressions.md#compound-assignment)) kullanılarak bir temsilciler birleşiminden kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="951a1-147">A delegate can be removed from a combination of delegates, using the binary `-` ([Subtraction operator](expressions.md#subtraction-operator)) and `-=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="951a1-148">Temsilciler eşitlik için karşılaştırılabilir ([temsilci eşitlik işleçleri](expressions.md#delegate-equality-operators)).</span><span class="sxs-lookup"><span data-stu-id="951a1-148">Delegates can be compared for equality ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="951a1-149">Aşağıdaki örnekte, bir dizi temsilciyi ve bunlara karşılık gelen çağrı listelerini gösteren örnek gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="951a1-149">The following example shows the instantiation of a number of delegates, and their corresponding invocation lists:</span></span>

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

<span data-ttu-id="951a1-150">@No__t-0 ve `cd2` örneği oluşturulduğunda, her biri bir yöntemi kapsüller.</span><span class="sxs-lookup"><span data-stu-id="951a1-150">When `cd1` and `cd2` are instantiated, they each encapsulate one method.</span></span> <span data-ttu-id="951a1-151">@No__t-0 örneği oluşturulduğunda, iki yöntemin çağırma listesine sahiptir, bu sırayla `M1` ve `M2`.</span><span class="sxs-lookup"><span data-stu-id="951a1-151">When `cd3` is instantiated, it has an invocation list of two methods, `M1` and `M2`, in that order.</span></span> <span data-ttu-id="951a1-152">`cd4` ' ın çağrı listesi, bu sırayla `M1`, `M2` ve `M1` içerir.</span><span class="sxs-lookup"><span data-stu-id="951a1-152">`cd4`'s invocation list contains `M1`, `M2`, and `M1`, in that order.</span></span> <span data-ttu-id="951a1-153">Son olarak, `cd5` ' ın çağrı listesi bu sırayla `M1`, `M2`, `M1`, `M1` ve `M2` ' i içerir.</span><span class="sxs-lookup"><span data-stu-id="951a1-153">Finally, `cd5`'s invocation list contains `M1`, `M2`, `M1`, `M1`, and `M2`, in that order.</span></span> <span data-ttu-id="951a1-154">Temsilcileri birleştirme (ve kaldırmanın yanı sıra) hakkında daha fazla örnek için bkz. [temsilci çağrısı](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="951a1-154">For more examples of combining (as well as removing) delegates, see [Delegate invocation](delegates.md#delegate-invocation).</span></span>

## <a name="delegate-compatibility"></a><span data-ttu-id="951a1-155">Temsilci uyumluluğu</span><span class="sxs-lookup"><span data-stu-id="951a1-155">Delegate compatibility</span></span>

<span data-ttu-id="951a1-156">@No__t-0 bir yöntem veya temsilci, aşağıdakilerin tümü doğru ise, `D` bir temsilci türüyle ***uyumludur*** :</span><span class="sxs-lookup"><span data-stu-id="951a1-156">A method or delegate `M` is ***compatible*** with a delegate type `D` if all of the following are true:</span></span>

*  <span data-ttu-id="951a1-157">`D` ve `M` aynı sayıda parametreye sahiptir ve `D` ' deki her bir parametre `M` ' teki karşılık gelen parametreyle aynı `ref` veya `out` değiştiricilerine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="951a1-157">`D` and `M` have the same number of parameters, and each parameter in `D` has the same `ref` or `out` modifiers as the corresponding parameter in `M`.</span></span>
*  <span data-ttu-id="951a1-158">Her değer parametresi için (`ref` veya `out` değiştiricisi olmayan bir parametre), `D` ' teki parametre türünden bir kimlik dönüştürme ([kimlik dönüştürme](conversions.md#identity-conversion)) veya örtük başvuru dönüştürmesi ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions)) var `M` ' teki karşılık gelen parametre türü.</span><span class="sxs-lookup"><span data-stu-id="951a1-158">For each value parameter (a parameter with no `ref` or `out` modifier), an identity conversion ([Identity conversion](conversions.md#identity-conversion)) or implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the parameter type in `D` to the corresponding parameter type in `M`.</span></span>
*  <span data-ttu-id="951a1-159">Her `ref` veya `out` parametresi için, `D` ' deki parametre türü `M` ' teki parametre türü ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="951a1-159">For each `ref` or `out` parameter, the parameter type in `D` is the same as the parameter type in `M`.</span></span>
*  <span data-ttu-id="951a1-160">@No__t-0 dönüş türünden `D` dönüş türüne bir kimlik veya örtük başvuru dönüştürmesi var.</span><span class="sxs-lookup"><span data-stu-id="951a1-160">An identity or implicit reference conversion exists from the return type of `M` to the return type of `D`.</span></span>

## <a name="delegate-instantiation"></a><span data-ttu-id="951a1-161">Temsilci oluşturma</span><span class="sxs-lookup"><span data-stu-id="951a1-161">Delegate instantiation</span></span>

<span data-ttu-id="951a1-162">Temsilcinin bir örneği, bir *delegate_creation_expression* ([temsilci oluşturma ifadeleri](expressions.md#delegate-creation-expressions)) veya bir temsilci türüne dönüştürme tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="951a1-162">An instance of a delegate is created by a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) or a conversion to a delegate type.</span></span> <span data-ttu-id="951a1-163">Yeni oluşturulan temsilci örneği bundan sonra şu şekilde başvurur:</span><span class="sxs-lookup"><span data-stu-id="951a1-163">The newly created delegate instance then refers to either:</span></span>

*  <span data-ttu-id="951a1-164">*Delegate_creation_expression*içinde başvurulan statik yöntem veya</span><span class="sxs-lookup"><span data-stu-id="951a1-164">The static method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="951a1-165">Hedef nesne (`null` olamaz) ve *delegate_creation_expression*içinde başvurulan örnek yöntemi</span><span class="sxs-lookup"><span data-stu-id="951a1-165">The target object (which cannot be `null`) and instance method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="951a1-166">Başka bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="951a1-166">Another delegate.</span></span>

<span data-ttu-id="951a1-167">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="951a1-167">For example:</span></span>

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

<span data-ttu-id="951a1-168">Örneği oluşturulduktan sonra, temsilci örnekleri her zaman aynı hedef nesne ve yönteme başvurur.</span><span class="sxs-lookup"><span data-stu-id="951a1-168">Once instantiated, delegate instances always refer to the same target object and method.</span></span> <span data-ttu-id="951a1-169">İki temsilci birleştirildiğinde ya da biri diğerinden kaldırıldığında, yeni bir temsilcinin kendi çağrı listesi ile sonuçlandığını unutmayın; Birleşik veya kaldırılan temsilcilerin çağırma listeleri değişmeden kalır.</span><span class="sxs-lookup"><span data-stu-id="951a1-169">Remember, when two delegates are combined, or one is removed from another, a new delegate results with its own invocation list; the invocation lists of the delegates combined or removed remain unchanged.</span></span>

## <a name="delegate-invocation"></a><span data-ttu-id="951a1-170">Temsilci çağırma</span><span class="sxs-lookup"><span data-stu-id="951a1-170">Delegate invocation</span></span>

<span data-ttu-id="951a1-171">C#bir temsilciyi çağırmak için özel bir sözdizimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="951a1-171">C# provides special syntax for invoking a delegate.</span></span> <span data-ttu-id="951a1-172">Çağırma listesi bir giriş içeren null olmayan bir temsilci örneği çağrıldığında, aynı bağımsız değişkenlerle bir yöntemi çağırır ve başvurulan yöntemiyle aynı değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="951a1-172">When a non-null delegate instance whose invocation list contains one entry is invoked, it invokes the one method with the same arguments it was given, and returns the same value as the referred to method.</span></span> <span data-ttu-id="951a1-173">(Temsilci çağırma hakkında ayrıntılı bilgi için bkz. [temsilci etkinleştirmeleri](expressions.md#delegate-invocations) .) Bu tür bir temsilcinin çağrılması sırasında bir özel durum oluşursa ve bu özel durum çağrılan yöntem içinde yakalanmadığında, özel durum catch yan tümcesi için arama, bu yöntemin doğrudan adı Bu temsilcinin başvurduğu yöntem.</span><span class="sxs-lookup"><span data-stu-id="951a1-173">(See [Delegate invocations](expressions.md#delegate-invocations) for detailed information on delegate invocation.) If an exception occurs during the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, as if that method had directly called the method to which that delegate referred.</span></span>

<span data-ttu-id="951a1-174">Çağırma listesi birden çok giriş içeren bir temsilci örneğinin çağrılması, çağrı listesindeki yöntemlerin her birini zaman uyumlu olarak sırayla çağırarak ilerler.</span><span class="sxs-lookup"><span data-stu-id="951a1-174">Invocation of a delegate instance whose invocation list contains multiple entries proceeds by invoking each of the methods in the invocation list, synchronously, in order.</span></span> <span data-ttu-id="951a1-175">Çağrısı yapılan her yöntem, temsilci örneğine verilen olarak aynı bağımsız değişken kümesine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="951a1-175">Each method so called is passed the same set of arguments as was given to the delegate instance.</span></span> <span data-ttu-id="951a1-176">Bu tür bir temsilci çağrısı başvuru parametreleri ([başvuru parametreleri](classes.md#reference-parameters)) içeriyorsa, her yöntem çağrısı aynı değişkene bir başvuru ile gerçekleşir; çağırma listesindeki bir yönteme göre söz konusu değişkende yapılan değişiklikler, çağrı listesinin daha aşağı bir yöntemi olarak görünür olacaktır.</span><span class="sxs-lookup"><span data-stu-id="951a1-176">If such a delegate invocation includes reference parameters ([Reference parameters](classes.md#reference-parameters)), each method invocation will occur with a reference to the same variable; changes to that variable by one method in the invocation list will be visible to methods further down the invocation list.</span></span> <span data-ttu-id="951a1-177">Temsilci çağrısı çıkış parametreleri veya dönüş değeri içeriyorsa, son değeri listedeki son temsilcinin çağrısından gelir.</span><span class="sxs-lookup"><span data-stu-id="951a1-177">If the delegate invocation includes output parameters or a return value, their final value will come from the invocation of the last delegate in the list.</span></span>

<span data-ttu-id="951a1-178">Böyle bir temsilcinin çağrılması sırasında bir özel durum oluşursa ve bu özel durum çağrılan yöntem içinde yakalanmadığında, özel durum catch yan tümcesi için arama, temsilciyi çağıran yönteme ve diğer yöntemlere devam eder çağırma listesi çağrılmıyor.</span><span class="sxs-lookup"><span data-stu-id="951a1-178">If an exception occurs during processing of the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, and any methods further down the invocation list are not invoked.</span></span>

<span data-ttu-id="951a1-179">Değeri null olan bir temsilci örneği çağrılmaya çalışılıyor `System.NullReferenceException` türünde bir özel durum oluşur.</span><span class="sxs-lookup"><span data-stu-id="951a1-179">Attempting to invoke a delegate instance whose value is null results in an exception of type `System.NullReferenceException`.</span></span>

<span data-ttu-id="951a1-180">Aşağıdaki örnek, temsilcileri oluşturma, birleştirme, kaldırma ve çağırma işlemlerinin nasıl yapılacağını göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="951a1-180">The following example shows how to instantiate, combine, remove, and invoke delegates:</span></span>

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

<span data-ttu-id="951a1-181">@No__t-0 ifadesinde gösterildiği gibi, bir temsilci bir çağırma listesinde birden çok kez bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="951a1-181">As shown in the statement `cd3 += cd1;`, a delegate can be present in an invocation list multiple times.</span></span> <span data-ttu-id="951a1-182">Bu durumda, her oluşum için yalnızca bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="951a1-182">In this case, it is simply invoked once per occurrence.</span></span> <span data-ttu-id="951a1-183">Bunun gibi bir çağırma listesinde, bu temsilci kaldırıldığında, aslında kaldırılan çağrı listesindeki son oluşum, aslında kaldırılmış bir şeydir.</span><span class="sxs-lookup"><span data-stu-id="951a1-183">In an invocation list such as this, when that delegate is removed, the last occurrence in the invocation list is the one actually removed.</span></span>

<span data-ttu-id="951a1-184">Son deyimin yürütmeden hemen önce, `cd3 -= cd1;`, `cd3` temsilcisi boş bir çağırma listesine başvurur.</span><span class="sxs-lookup"><span data-stu-id="951a1-184">Immediately prior to the execution of the final statement, `cd3 -= cd1;`, the delegate `cd3` refers to an empty invocation list.</span></span> <span data-ttu-id="951a1-185">Bir temsilciyi boş bir listeden kaldırma girişimi (veya varolmayan bir temsilciyi boş olmayan bir listeden kaldırmak için) bir hata değildir.</span><span class="sxs-lookup"><span data-stu-id="951a1-185">Attempting to remove a delegate from an empty list (or to remove a non-existent delegate from a non-empty list) is not an error.</span></span>

<span data-ttu-id="951a1-186">Oluşturulan çıkış:</span><span class="sxs-lookup"><span data-stu-id="951a1-186">The output produced is:</span></span>

```console
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
