---
ms.openlocfilehash: e0def754174ab8646f9b849abe86d2c375c958b6
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703978"
---
# <a name="classes"></a><span data-ttu-id="4c8df-101">Sınıflar</span><span class="sxs-lookup"><span data-stu-id="4c8df-101">Classes</span></span>

<span data-ttu-id="4c8df-102">Sınıf veri üyeleri (sabitler ve alanlar), işlev üyeleri (Yöntemler, özellikler, olaylar, Dizin oluşturucular, işleçler, örnek oluşturucular, Yıkıcılar ve statik oluşturucular) ve iç içe türler içerebilen bir veri yapısıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-102">A class is a data structure that may contain data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="4c8df-103">Sınıf türleri, türetilmiş bir sınıfın bir temel sınıfı genişletebileceği ve özelleştirileceği bir mekanizma olan devralmayı destekler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-103">Class types support inheritance, a mechanism whereby a derived class can extend and specialize a base class.</span></span>

## <a name="class-declarations"></a><span data-ttu-id="4c8df-104">Sınıf bildirimleri</span><span class="sxs-lookup"><span data-stu-id="4c8df-104">Class declarations</span></span>

<span data-ttu-id="4c8df-105">*Class_declaration* , yeni bir sınıf bildiren bir *type_declaration* ([tür bildirimleri](namespaces.md#type-declarations)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-105">A *class_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new class.</span></span>

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

<span data-ttu-id="4c8df-106">*Class_declaration* , isteğe bağlı bir *öznitelik* kümesi ([öznitelikler](attributes.md)) ve ardından isteğe bağlı bir *class_modifier*s kümesi ([sınıf değiştiricileri](classes.md#class-modifiers)) ve ardından isteğe bağlı bir `partial` `class` değiştiricisi ve sonra isteğe bağlı bir type_parameter_list belirtimi ( [tür parametreleri](classes.md#type-parameters)) ve ardından isteğe *bağlı bir* *class_base* belirtimi ([sınıf taban belirtimi](classes.md#class-base-specification)) ve ardından isteğe bağlı bir *type_parameter_constraints_clause*s kümesi ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) tarafından, ardından bir *class_body* ([sınıf gövdesi](classes.md#class-body)) ve daha sonra noktalı virgül gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-106">A *class_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *class_modifier*s ([Class modifiers](classes.md#class-modifiers)), followed by an optional `partial` modifier, followed by the keyword `class` and an *identifier* that names the class, followed by an optional *type_parameter_list* ([Type parameters](classes.md#type-parameters)), followed by an optional *class_base* specification ([Class base specification](classes.md#class-base-specification)) , followed by an optional set of *type_parameter_constraints_clause*s ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *class_body* ([Class body](classes.md#class-body)), optionally followed by a semicolon.</span></span>

<span data-ttu-id="4c8df-107">Bir sınıf bildirimi, ayrıca bir *type_parameter_list*sağlamadıkça *type_parameter_constraints_clause*s 'yi sağlayaamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-107">A class declaration cannot supply *type_parameter_constraints_clause*s unless it also supplies a *type_parameter_list*.</span></span>

<span data-ttu-id="4c8df-108">Bir *type_parameter_list* sağlayan bir sınıf bildirimi, ***Genel sınıf bildirimidir***.</span><span class="sxs-lookup"><span data-stu-id="4c8df-108">A class declaration that supplies a *type_parameter_list* is a ***generic class declaration***.</span></span> <span data-ttu-id="4c8df-109">Ayrıca, bir genel sınıf bildiriminde iç içe yerleştirilmiş olan herhangi bir sınıf ya da genel bir yapı bildirimi, oluşturulmuş bir tür oluşturmak için, kapsayan türün tür parametreleri sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-109">Additionally, any class nested inside a generic class declaration or a generic struct declaration is itself a generic class declaration, since type parameters for the containing type must be supplied to create a constructed type.</span></span>

### <a name="class-modifiers"></a><span data-ttu-id="4c8df-110">Sınıf değiştiricileri</span><span class="sxs-lookup"><span data-stu-id="4c8df-110">Class modifiers</span></span>

<span data-ttu-id="4c8df-111">*Class_declaration* , isteğe bağlı olarak bir sınıf değiştiricileri dizisi içerebilir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-111">A *class_declaration* may optionally include a sequence of class modifiers:</span></span>

```antlr
class_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'abstract'
    | 'sealed'
    | 'static'
    | class_modifier_unsafe
    ;
```

<span data-ttu-id="4c8df-112">Aynı değiştiricinin bir sınıf bildiriminde birden çok kez görünmesi için derleme zamanı hatası vardır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-112">It is a compile-time error for the same modifier to appear multiple times in a class declaration.</span></span>

<span data-ttu-id="4c8df-113">İç içe sınıflarda `new` değiştiriciye izin verilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-113">The `new` modifier is permitted on nested classes.</span></span> <span data-ttu-id="4c8df-114">Bu, sınıfın, [Yeni değiştirici](classes.md#the-new-modifier)bölümünde açıklandığı gibi, devralınan bir üyeyi aynı adla gizlediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-114">It specifies that the class hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span> <span data-ttu-id="4c8df-115">`new` değiştiricinin iç içe geçmiş sınıf bildirimi olmayan bir sınıf bildiriminde görünmesi için derleme zamanı hatası.</span><span class="sxs-lookup"><span data-stu-id="4c8df-115">It is a compile-time error for the `new` modifier to appear on a class declaration that is not a nested class declaration.</span></span>

<span data-ttu-id="4c8df-116">`public`, `protected`, `internal`ve `private` değiştiricileri, sınıfının erişilebilirliğini denetler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the class.</span></span> <span data-ttu-id="4c8df-117">Sınıf bildiriminin gerçekleştiği içeriğe bağlı olarak, bu değiştiricilerin bazılarına izin verilmeyebilir ([belirtilen erişilebilirlik](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-117">Depending on the context in which the class declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="4c8df-118">`abstract`, `sealed` ve `static` değiştiricileri aşağıdaki bölümlerde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-118">The `abstract`, `sealed` and `static` modifiers are discussed in the following sections.</span></span>

#### <a name="abstract-classes"></a><span data-ttu-id="4c8df-119">Soyut sınıflar</span><span class="sxs-lookup"><span data-stu-id="4c8df-119">Abstract classes</span></span>

<span data-ttu-id="4c8df-120">`abstract` değiştiricisi, bir sınıfın tamamlanmamış olduğunu ve yalnızca temel sınıf olarak kullanılması amaçlanan olduğunu göstermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-120">The `abstract` modifier is used to indicate that a class is incomplete and that it is intended to be used only as a base class.</span></span> <span data-ttu-id="4c8df-121">Soyut bir sınıf, soyut olmayan bir sınıftan aşağıdaki yollarla farklılık gösterir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-121">An abstract class differs from a non-abstract class in the following ways:</span></span>

*  <span data-ttu-id="4c8df-122">Soyut bir sınıf doğrudan başlatılamaz ve bir derleme zamanı hatası, bir soyut sınıf üzerinde `new` işlecini kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-122">An abstract class cannot be instantiated directly, and it is a compile-time error to use the `new` operator on an abstract class.</span></span> <span data-ttu-id="4c8df-123">Derleme zamanı türleri soyut olan değişkenler ve değerler olması mümkün olsa da, bu tür değişkenler ve değerler `null` veya soyut türlerden türetilmiş soyut olmayan sınıfların örneklerine başvurular içermelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-123">While it is possible to have variables and values whose compile-time types are abstract, such variables and values will necessarily either be `null` or contain references to instances of non-abstract classes derived from the abstract types.</span></span>
*  <span data-ttu-id="4c8df-124">Soyut bir sınıfa, soyut Üyeler içermesi için izin verilir (ancak gerekli değildir).</span><span class="sxs-lookup"><span data-stu-id="4c8df-124">An abstract class is permitted (but not required) to contain abstract members.</span></span>
*  <span data-ttu-id="4c8df-125">Soyut bir sınıf korumalı olamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-125">An abstract class cannot be sealed.</span></span>

<span data-ttu-id="4c8df-126">Soyut olmayan bir sınıf, soyut bir sınıftan türetilmişse, soyut olmayan sınıfın devralınan tüm soyut üyelerin gerçek uygulamalarını içermesi gerekir, böylece bu soyut üyeleri geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-126">When a non-abstract class is derived from an abstract class, the non-abstract class must include actual implementations of all inherited abstract members, thereby overriding those abstract members.</span></span> <span data-ttu-id="4c8df-127">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-127">In the example</span></span>
```csharp
abstract class A
{
    public abstract void F();
}

abstract class B: A
{
    public void G() {}
}

class C: B
{
    public override void F() {
        // actual implementation of F
    }
}
```
<span data-ttu-id="4c8df-128">Soyut sınıf `A` `F`soyut bir yöntem sunar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-128">the abstract class `A` introduces an abstract method `F`.</span></span> <span data-ttu-id="4c8df-129">Sınıf `B` ek bir yöntem `G`tanıtır, ancak `F`uygulanmasını sağlamadıklarından `B` de soyut olarak bildirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-129">Class `B` introduces an additional method `G`, but since it doesn't provide an implementation of `F`, `B` must also be declared abstract.</span></span> <span data-ttu-id="4c8df-130">Sınıf `C` geçersiz kılar `F` ve gerçek bir uygulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-130">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="4c8df-131">`C`içinde soyut üye olmadığından, `C` izin verilir (ancak zorunlu değildir).</span><span class="sxs-lookup"><span data-stu-id="4c8df-131">Since there are no abstract members in `C`, `C` is permitted (but not required) to be non-abstract.</span></span>

#### <a name="sealed-classes"></a><span data-ttu-id="4c8df-132">Mühürlü sınıflar</span><span class="sxs-lookup"><span data-stu-id="4c8df-132">Sealed classes</span></span>

<span data-ttu-id="4c8df-133">`sealed` değiştiricisi bir sınıftan türetmeyi engellemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-133">The `sealed` modifier is used to prevent derivation from a class.</span></span> <span data-ttu-id="4c8df-134">Bir Sealed sınıfı, başka bir sınıfın temel sınıfı olarak belirtilmişse, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-134">A compile-time error occurs if a sealed class is specified as the base class of another class.</span></span>

<span data-ttu-id="4c8df-135">Sealed bir sınıf de soyut bir sınıf olamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-135">A sealed class cannot also be an abstract class.</span></span>

<span data-ttu-id="4c8df-136">`sealed` değiştiricisi öncelikle istenmeden Türetmenin önlenmesi için kullanılır, ancak belirli çalışma zamanı iyileştirmelerini de sağlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-136">The `sealed` modifier is primarily used to prevent unintended derivation, but it also enables certain run-time optimizations.</span></span> <span data-ttu-id="4c8df-137">Özellikle, korumalı bir sınıfın hiç türetilmiş sınıfa sahip olmadığı bilindiğinden, korumalı sınıf örneklerine sanal işlev üye çağırmaları sanal olmayan çağırmaları dönüştürmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-137">In particular, because a sealed class is known to never have any derived classes, it is possible to transform virtual function member invocations on sealed class instances into non-virtual invocations.</span></span>

#### <a name="static-classes"></a><span data-ttu-id="4c8df-138">Statik sınıflar</span><span class="sxs-lookup"><span data-stu-id="4c8df-138">Static classes</span></span>

<span data-ttu-id="4c8df-139">`static` değiştiricisi, bir ***statik sınıf***olarak bildirildiği sınıfı işaretlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-139">The `static` modifier is used to mark the class being declared as a ***static class***.</span></span> <span data-ttu-id="4c8df-140">Statik bir sınıf örneği oluşturulamıyor, tür olarak kullanılamaz ve yalnızca statik üyeleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-140">A static class cannot be instantiated, cannot be used as a type and can contain only static members.</span></span> <span data-ttu-id="4c8df-141">Yalnızca bir statik sınıf uzantı yöntemlerinin bildirimlerini içerebilir ([Uzantı yöntemleri](classes.md#extension-methods)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-141">Only a static class can contain declarations of extension methods ([Extension methods](classes.md#extension-methods)).</span></span>

<span data-ttu-id="4c8df-142">Statik sınıf bildirimi aşağıdaki kısıtlamalara tabidir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-142">A static class declaration is subject to the following restrictions:</span></span>

*  <span data-ttu-id="4c8df-143">Statik bir sınıf, `sealed` veya `abstract` değiştiricisi içeremez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-143">A static class may not include a `sealed` or `abstract` modifier.</span></span> <span data-ttu-id="4c8df-144">Ancak, statik bir sınıf tarafından örneklenemez veya türetilmediği için, hem korumalı hem de soyut gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-144">Note, however, that since a static class cannot be instantiated or derived from, it behaves as if it was both sealed and abstract.</span></span>
*  <span data-ttu-id="4c8df-145">Statik bir sınıf *class_base* belirtimi ([sınıf taban belirtimi](classes.md#class-base-specification)) içeremez ve açıkça bir temel sınıf veya uygulanan arabirimlerin listesini belirtemez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-145">A static class may not include a *class_base* specification ([Class base specification](classes.md#class-base-specification)) and cannot explicitly specify a base class or a list of implemented interfaces.</span></span> <span data-ttu-id="4c8df-146">Statik bir sınıf örtülü olarak `object`türünden devralır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-146">A static class implicitly inherits from type `object`.</span></span>
*  <span data-ttu-id="4c8df-147">Statik bir sınıf yalnızca statik Üyeler ([statik ve örnek üyeleri](classes.md#static-and-instance-members)) içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-147">A static class can only contain static members ([Static and instance members](classes.md#static-and-instance-members)).</span></span> <span data-ttu-id="4c8df-148">Sabitler ve iç içe geçmiş türlerin statik üye olarak sınıflandırıldığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4c8df-148">Note that constants and nested types are classified as static members.</span></span>
*  <span data-ttu-id="4c8df-149">Statik bir sınıfta `protected` veya `protected internal` belirtilen erişilebilirliği olan Üyeler olamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-149">A static class cannot have members with `protected` or `protected internal` declared accessibility.</span></span>

<span data-ttu-id="4c8df-150">Bu kısıtlamaların herhangi birini ihlal etmek için derleme zamanı hatası vardır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-150">It is a compile-time error to violate any of these restrictions.</span></span>

<span data-ttu-id="4c8df-151">Statik bir sınıfın örnek oluşturucuları yok.</span><span class="sxs-lookup"><span data-stu-id="4c8df-151">A static class has no instance constructors.</span></span> <span data-ttu-id="4c8df-152">Statik bir sınıfta örnek Oluşturucu bildirmek mümkün değildir ve statik bir sınıf için varsayılan örnek Oluşturucu ([Varsayılan oluşturucular](classes.md#default-constructors)) sağlanmaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-152">It is not possible to declare an instance constructor in a static class, and no default instance constructor ([Default constructors](classes.md#default-constructors)) is provided for a static class.</span></span>

<span data-ttu-id="4c8df-153">Statik bir sınıfın üyeleri otomatik olarak statik değildir ve üye bildirimlerinin açıkça bir `static` değiştiricisi içermesi gerekir (sabitler ve iç içe türler hariç).</span><span class="sxs-lookup"><span data-stu-id="4c8df-153">The members of a static class are not automatically static, and the member declarations must explicitly include a `static` modifier (except for constants and nested types).</span></span> <span data-ttu-id="4c8df-154">Bir sınıf statik bir dış sınıf içinde iç içe olduğunda, açıkça bir `static` değiştiricisi içermiyorsa, iç içe yerleştirilmiş sınıf statik bir sınıf değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-154">When a class is nested within a static outer class, the nested class is not a static class unless it explicitly includes a `static` modifier.</span></span>

<span data-ttu-id="4c8df-155">__Statik Sınıf türlerine başvurma__</span><span class="sxs-lookup"><span data-stu-id="4c8df-155">__Referencing static class types__</span></span>

<span data-ttu-id="4c8df-156">Bir *namespace_or_type_name* ([ad alanı ve tür adları](basic-concepts.md#namespace-and-type-names)), şu durumlarda bir statik sınıfa başvurmak için izin verilir</span><span class="sxs-lookup"><span data-stu-id="4c8df-156">A *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="4c8df-157">*Namespace_or_type_name* , `T.I`*namespace_or_type_name* `T` veya</span><span class="sxs-lookup"><span data-stu-id="4c8df-157">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="4c8df-158">*Namespace_or_type_name* , `typeof(T)`formun *typeof_expression* ([bağımsız değişken listeleri](expressions.md#argument-lists)1) `T`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-158">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

<span data-ttu-id="4c8df-159">Bir *primary_expression* ([işlev üyeleri](expressions.md#function-members)), şu durumlarda bir statik sınıfa başvurmak için izin verilir</span><span class="sxs-lookup"><span data-stu-id="4c8df-159">A *primary_expression* ([Function members](expressions.md#function-members)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="4c8df-160">*Primary_expression* , `E.I`formun *member_access* ([dinamik aşırı yükleme çözümlemesi için derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) `E`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-160">The *primary_expression* is the `E` in a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) of the form `E.I`.</span></span>

<span data-ttu-id="4c8df-161">Diğer bir bağlamda, statik bir sınıfa başvurmak için derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-161">In any other context it is a compile-time error to reference a static class.</span></span> <span data-ttu-id="4c8df-162">Örneğin, bir statik sınıfın temel sınıf olarak kullanılabilmesi, bir üyenin bir bileşen türü ([Iç içe türler](classes.md#nested-types)), bir genel tür bağımsız değişkeni veya tür parametresi kısıtlaması olması hatadır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-162">For example, it is an error for a static class to be used as a base class, a constituent type ([Nested types](classes.md#nested-types)) of a member, a generic type argument, or a type parameter constraint.</span></span> <span data-ttu-id="4c8df-163">Benzer şekilde, bir statik sınıf dizi türünde, işaretçi türünde, `new` ifadesinde, atama ifadesinde, `is` ifadesinde, bir `as` ifadesinde, `sizeof` ifadesinde veya bir varsayılan değer ifadesinde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-163">Likewise, a static class cannot be used in an array type, a pointer type, a `new` expression, a cast expression, an `is` expression, an `as` expression, a `sizeof` expression, or a default value expression.</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="4c8df-164">Kısmi değiştirici</span><span class="sxs-lookup"><span data-stu-id="4c8df-164">Partial modifier</span></span>

<span data-ttu-id="4c8df-165">`partial` değiştirici, bu *class_declaration* kısmi bir tür bildirimi olduğunu göstermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-165">The `partial` modifier is used to indicate that this *class_declaration* is a partial type declaration.</span></span> <span data-ttu-id="4c8df-166">Bir kapsayan ad alanı veya tür bildiriminde aynı ada sahip birden çok kısmi tür bildirimi, [kısmi türlerde](classes.md#partial-types)belirtilen kurallara göre tek bir tür bildirimi oluşturmak için birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-166">Multiple partial type declarations with the same name within an enclosing namespace or type declaration combine to form one type declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

<span data-ttu-id="4c8df-167">Program metninin ayrı kesimleri üzerinde dağıtılan bir sınıf bildiriminin olması, bu parçaların farklı bağlamlarda üretilmesi veya saklanması durumunda yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-167">Having the declaration of a class distributed over separate segments of program text can be useful if these segments are produced or maintained in different contexts.</span></span> <span data-ttu-id="4c8df-168">Örneğin, bir sınıf bildiriminin bir kısmı makine tarafından oluşturulmuş olabilir, diğeri el ile yazılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-168">For instance, one part of a class declaration may be machine generated, whereas the other is manually authored.</span></span> <span data-ttu-id="4c8df-169">İki ' un metinsel ayrımı, diğer bir güncelleştirme ile çakışma arasından güncelleştirme yapılmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="4c8df-169">Textual separation of the two prevents updates by one from conflicting with updates by the other.</span></span>

### <a name="type-parameters"></a><span data-ttu-id="4c8df-170">Tür parametreleri</span><span class="sxs-lookup"><span data-stu-id="4c8df-170">Type parameters</span></span>

<span data-ttu-id="4c8df-171">Tür parametresi, oluşturulmuş bir tür oluşturmak için sağlanan bir tür bağımsız değişkeni için yer tutucuyu belirten basit bir tanıtıcıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-171">A type parameter is a simple identifier that denotes a placeholder for a type argument supplied to create a constructed type.</span></span> <span data-ttu-id="4c8df-172">Tür parametresi, daha sonra sağlanacak bir tür için resmi yer tutucudur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-172">A type parameter is a formal placeholder for a type that will be supplied later.</span></span> <span data-ttu-id="4c8df-173">Buna karşılık, bir tür bağımsız değişkeni ([tür bağımsız değişkenleri](types.md#type-arguments)) oluşturulmuş bir tür oluşturulduğunda tür parametresi için değiştirilen gerçek türdür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-173">By contrast, a type argument ([Type arguments](types.md#type-arguments)) is the actual type that is substituted for the type parameter when a constructed type is created.</span></span>

```antlr
type_parameter_list
    : '<' type_parameters '>'
    ;

type_parameters
    : attributes? type_parameter
    | type_parameters ',' attributes? type_parameter
    ;

type_parameter
    : identifier
    ;
```

<span data-ttu-id="4c8df-174">Bir sınıf bildirimindeki her tür parametresi, bu sınıfın bildirim alanında ([bildirimlerinde](basic-concepts.md#declarations)) bir ad tanımlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-174">Each type parameter in a class declaration defines a name in the declaration space ([Declarations](basic-concepts.md#declarations)) of that class.</span></span> <span data-ttu-id="4c8df-175">Bu nedenle, başka bir tür parametresiyle aynı ada veya bu sınıfta belirtilen bir üyeye sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-175">Thus, it cannot have the same name as another type parameter or a member declared in that class.</span></span> <span data-ttu-id="4c8df-176">Tür parametresi, türün kendisiyle aynı ada sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-176">A type parameter cannot have the same name as the type itself.</span></span>

### <a name="class-base-specification"></a><span data-ttu-id="4c8df-177">Sınıf temel belirtimi</span><span class="sxs-lookup"><span data-stu-id="4c8df-177">Class base specification</span></span>

<span data-ttu-id="4c8df-178">Sınıf bildirimi, sınıfının doğrudan temel sınıfını ve doğrudan sınıf tarafından uygulanan arabirimleri ([arabirimler](interfaces.md)) tanımlayan bir *class_base* belirtimi içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-178">A class declaration may include a *class_base* specification, which defines the direct base class of the class and the interfaces ([Interfaces](interfaces.md)) directly implemented by the class.</span></span>

```antlr
class_base
    : ':' class_type
    | ':' interface_type_list
    | ':' class_type ',' interface_type_list
    ;

interface_type_list
    : interface_type (',' interface_type)*
    ;
```

<span data-ttu-id="4c8df-179">Sınıf bildiriminde belirtilen temel sınıf, oluşturulmuş bir sınıf türü ([oluşturulmuş türler](types.md#constructed-types)) olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-179">The base class specified in a class declaration can be a constructed class type ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="4c8df-180">Bir temel sınıf kendi üzerinde bir tür parametresi olamaz, ancak kapsamdaki tür parametreleri de içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-180">A base class cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span>

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a><span data-ttu-id="4c8df-181">Temel sınıflar</span><span class="sxs-lookup"><span data-stu-id="4c8df-181">Base classes</span></span>

<span data-ttu-id="4c8df-182">*Class_base*bir *class_type* dahil edildiğinde, bildirildiği sınıfın doğrudan temel sınıfını belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-182">When a *class_type* is included in the *class_base*, it specifies the direct base class of the class being declared.</span></span> <span data-ttu-id="4c8df-183">Bir sınıf bildiriminde *class_base*yoksa veya *class_base* yalnızca arabirim türlerini listelemeli, doğrudan taban sınıfın `object`olduğu varsayılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-183">If a class declaration has no *class_base*, or if the *class_base* lists only interface types, the direct base class is assumed to be `object`.</span></span> <span data-ttu-id="4c8df-184">Bir sınıf, [Devralma](classes.md#inheritance)bölümünde açıklandığı gibi doğrudan temel sınıfından üyeleri devralır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-184">A class inherits members from its direct base class, as described in [Inheritance](classes.md#inheritance).</span></span>

<span data-ttu-id="4c8df-185">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-185">In the example</span></span>
```csharp
class A {}

class B: A {}
```
<span data-ttu-id="4c8df-186">sınıf `A`, `B`doğrudan temel sınıfı olarak kabul edilir ve `B` `A`türettiği söylenir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-186">class `A` is said to be the direct base class of `B`, and `B` is said to be derived from `A`.</span></span> <span data-ttu-id="4c8df-187">`A` açıkça doğrudan bir temel sınıf belirtmediğinden, doğrudan temel sınıfı örtülü olarak `object`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-187">Since `A` does not explicitly specify a direct base class, its direct base class is implicitly `object`.</span></span>

<span data-ttu-id="4c8df-188">Oluşturulmuş bir sınıf türü için, genel sınıf bildiriminde bir temel sınıf belirtilmişse, oluşturulan türün temel sınıfı, temel sınıf bildirimindeki her bir *type_parameter* , oluşturulan türün karşılık gelen *type_argument* yerine koyarak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-188">For a constructed class type, if a base class is specified in the generic class declaration, the base class of the constructed type is obtained by substituting, for each *type_parameter* in the base class declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="4c8df-189">Genel sınıf bildirimleri verildiğinde</span><span class="sxs-lookup"><span data-stu-id="4c8df-189">Given the generic class declarations</span></span>
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
<span data-ttu-id="4c8df-190">oluşturulan tür `G<int>` temel sınıfı `B<string,int[]>`olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-190">the base class of the constructed type `G<int>` would be `B<string,int[]>`.</span></span>

<span data-ttu-id="4c8df-191">Bir sınıf türünün doğrudan temel sınıfı en az sınıf türünün kendisi ([erişilebilirlik etki alanları](basic-concepts.md#accessibility-domains)) olarak erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-191">The direct base class of a class type must be at least as accessible as the class type itself ([Accessibility domains](basic-concepts.md#accessibility-domains)).</span></span> <span data-ttu-id="4c8df-192">Örneğin, bir `private` veya `internal` sınıfından türetmek `public` sınıfın derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-192">For example, it is a compile-time error for a `public` class to derive from a `private` or `internal` class.</span></span>

<span data-ttu-id="4c8df-193">Bir sınıf türünün doğrudan temel sınıfı şu türlerden biri olmalıdır: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`veya `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-193">The direct base class of a class type must not be any of the following types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="4c8df-194">Ayrıca, genel bir sınıf bildirimi `System.Attribute` doğrudan veya dolaylı temel sınıf olarak kullanamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-194">Furthermore, a generic class declaration cannot use `System.Attribute` as a direct or indirect base class.</span></span>

<span data-ttu-id="4c8df-195">Bir sınıf `B``A` doğrudan temel sınıf belirtiminin anlamı belirlenirken, `B` doğrudan taban sınıfının geçici olarak `object`olduğu varsayılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-195">While determining the meaning of the direct base class specification `A` of a class `B`, the direct base class of `B` is temporarily assumed to be `object`.</span></span> <span data-ttu-id="4c8df-196">Intuicanlı bu, bir temel sınıf belirtiminin anlamını özyinelemeli olarak kendi kendine bağımlı olmamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-196">Intuitively this ensures that the meaning of a base class specification cannot recursively depend on itself.</span></span> <span data-ttu-id="4c8df-197">Örnek:</span><span class="sxs-lookup"><span data-stu-id="4c8df-197">The example:</span></span>
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
<span data-ttu-id="4c8df-198">temel sınıf belirtiminde bu yana hata durumunda `A<C.B>` doğrudan taban `C` sınıfının `object`olduğu kabul edilir ve bu nedenle ( [ad alanı ve tür adlarının](basic-concepts.md#namespace-and-type-names)kuralları tarafından) `C` üye `B`kabul edilmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-198">is in error since in the base class specification `A<C.B>` the direct base class of `C` is considered to be `object`, and hence (by the rules of [Namespace and type names](basic-concepts.md#namespace-and-type-names))  `C` is not considered to have a member `B`.</span></span>

<span data-ttu-id="4c8df-199">Bir sınıf türünün temel sınıfları doğrudan taban sınıfıdır ve temel sınıflarıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-199">The base classes of a class type are the direct base class and its base classes.</span></span> <span data-ttu-id="4c8df-200">Diğer bir deyişle, temel sınıfların kümesi doğrudan temel sınıf ilişkisinin geçişli kapanışı olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-200">In other words, the set of base classes is the transitive closure of the direct base class relationship.</span></span> <span data-ttu-id="4c8df-201">Yukarıdaki örneğe başvurarak, `B` temel sınıfları `A` ve `object`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-201">Referring to the example above, the base classes of `B` are `A` and `object`.</span></span> <span data-ttu-id="4c8df-202">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-202">In the example</span></span>
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
<span data-ttu-id="4c8df-203">`D<int>` temel sınıfları `C<int[]>`, `B<IComparable<int[]>>`, `A`ve `object`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-203">the base classes of `D<int>` are `C<int[]>`, `B<IComparable<int[]>>`, `A`, and `object`.</span></span>

<span data-ttu-id="4c8df-204">Sınıf `object`hariç olmak üzere her sınıf türünün tam olarak bir doğrudan temel sınıfı vardır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-204">Except for class `object`, every class type has exactly one direct base class.</span></span> <span data-ttu-id="4c8df-205">`object` sınıfının doğrudan temel sınıfı yoktur ve diğer tüm sınıfların en son temel sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-205">The `object` class has no direct base class and is the ultimate base class of all other classes.</span></span>

<span data-ttu-id="4c8df-206">Bir sınıf `B` bir sınıf `A`türediği zaman, `A` `B`bağımlı olması için derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-206">When a class `B` derives from a class `A`, it is a compile-time error for `A` to depend on `B`.</span></span> <span data-ttu-id="4c8df-207">Bir sınıf doğrudan temel sınıfına (varsa) ***bağlıdır*** ve doğrudan iç içe geçmiş (varsa) sınıfa ***bağlıdır*** .</span><span class="sxs-lookup"><span data-stu-id="4c8df-207">A class ***directly depends on*** its direct base class (if any) and ***directly depends on*** the class within which it is immediately nested (if any).</span></span> <span data-ttu-id="4c8df-208">Bu tanım verildiğinde, bir sınıfın bağımlı olduğu tüm sınıf kümesi, ***doğrudan ilişkiye bağlı olarak değişir*** .</span><span class="sxs-lookup"><span data-stu-id="4c8df-208">Given this definition, the complete set of classes upon which a class depends is the reflexive and transitive closure of the ***directly depends on*** relationship.</span></span>

<span data-ttu-id="4c8df-209">Örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-209">The example</span></span>
```csharp
class A: A {}
```
<span data-ttu-id="4c8df-210">sınıf kendi kendine bağımlı olduğundan hatalı.</span><span class="sxs-lookup"><span data-stu-id="4c8df-210">is erroneous because the class depends on itself.</span></span> <span data-ttu-id="4c8df-211">Benzer şekilde, örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-211">Likewise, the example</span></span>
```csharp
class A: B {}
class B: C {}
class C: A {}
```
<span data-ttu-id="4c8df-212">sınıfların kendilerine döngüsel olarak bağımlı olması nedeniyle hata oluştu.</span><span class="sxs-lookup"><span data-stu-id="4c8df-212">is in error because the classes circularly depend on themselves.</span></span> <span data-ttu-id="4c8df-213">Son olarak, örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-213">Finally, the example</span></span>
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
<span data-ttu-id="4c8df-214">`A`, bir derleme zamanı hatasına neden olur çünkü bu, `A`bağımlı olan `B` (hemen kapsayan sınıfa) bağlı olan `B.C` (doğrudan temel sınıfına) bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-214">results in a compile-time error because `A` depends on `B.C` (its direct base class), which depends on `B` (its immediately enclosing class), which circularly depends on `A`.</span></span>

<span data-ttu-id="4c8df-215">Bir sınıfın içinde iç içe yerleştirilmiş sınıflara bağlı olmadığına unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4c8df-215">Note that a class does not depend on the classes that are nested within it.</span></span> <span data-ttu-id="4c8df-216">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-216">In the example</span></span>
```csharp
class A
{
    class B: A {}
}
```
<span data-ttu-id="4c8df-217">`B`, `A` bağımlıdır (`A` hem doğrudan temel sınıfı hem de onun hemen kapsayan sınıfı olduğundan), ancak `B` bir temel sınıf veya kapsayan bir sınıf olmadığından `A` bağımlı değildir.`B``A`</span><span class="sxs-lookup"><span data-stu-id="4c8df-217">`B` depends on `A` (because `A` is both its direct base class and its immediately enclosing class), but `A` does not depend on `B` (since `B` is neither a base class nor an enclosing class of `A`).</span></span> <span data-ttu-id="4c8df-218">Bu nedenle, örnek geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-218">Thus, the example is valid.</span></span>

<span data-ttu-id="4c8df-219">`sealed` sınıfından türetmek mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-219">It is not possible to derive from a `sealed` class.</span></span> <span data-ttu-id="4c8df-220">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-220">In the example</span></span>
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
<span data-ttu-id="4c8df-221">sınıf `B`, `A``sealed` sınıfından türetmeye çalıştığı için hatalı.</span><span class="sxs-lookup"><span data-stu-id="4c8df-221">class `B` is in error because it attempts to derive from the `sealed` class `A`.</span></span>

#### <a name="interface-implementations"></a><span data-ttu-id="4c8df-222">Arabirim Uygulamaları</span><span class="sxs-lookup"><span data-stu-id="4c8df-222">Interface implementations</span></span>

<span data-ttu-id="4c8df-223">Bir *class_base* belirtimi, arabirim türlerinin bir listesini içerebilir ve bu durumda sınıf verilen arabirim türlerini doğrudan uygulayacak şekilde söylenir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-223">A *class_base* specification may include a list of interface types, in which case the class is said to directly implement the given interface types.</span></span> <span data-ttu-id="4c8df-224">Arabirim Uygulamaları, [arabirim uygulamalarında](interfaces.md#interface-implementations)daha ayrıntılı bir şekilde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-224">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="type-parameter-constraints"></a><span data-ttu-id="4c8df-225">Tür parametresi kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="4c8df-225">Type parameter constraints</span></span>

<span data-ttu-id="4c8df-226">Genel tür ve yöntem bildirimleri, isteğe bağlı olarak *type_parameter_constraints_clause*s ekleyerek tür parametresi kısıtlamalarını belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-226">Generic type and method declarations can optionally specify type parameter constraints by including *type_parameter_constraints_clause*s.</span></span>

```antlr
type_parameter_constraints_clause
    : 'where' type_parameter ':' type_parameter_constraints
    ;

type_parameter_constraints
    : primary_constraint
    | secondary_constraints
    | constructor_constraint
    | primary_constraint ',' secondary_constraints
    | primary_constraint ',' constructor_constraint
    | secondary_constraints ',' constructor_constraint
    | primary_constraint ',' secondary_constraints ',' constructor_constraint
    ;

primary_constraint
    : class_type
    | 'class'
    | 'struct'
    ;

secondary_constraints
    : interface_type
    | type_parameter
    | secondary_constraints ',' interface_type
    | secondary_constraints ',' type_parameter
    ;

constructor_constraint
    : 'new' '(' ')'
    ;
```

<span data-ttu-id="4c8df-227">Her *type_parameter_constraints_clause* , belirteç `where`ve ardından bir tür parametresinin adından sonra iki nokta üst üste ve bu tür parametresi için kısıtlamalar listesinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-227">Each *type_parameter_constraints_clause* consists of the token `where`, followed by the name of a type parameter, followed by a colon and the list of constraints for that type parameter.</span></span> <span data-ttu-id="4c8df-228">Her tür parametresi için en fazla bir `where` yan tümcesi olabilir ve `where` yan tümceleri herhangi bir sırada listelenebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-228">There can be at most one `where` clause for each type parameter, and the `where` clauses can be listed in any order.</span></span> <span data-ttu-id="4c8df-229">Bir özellik erişimcisindeki `get` ve `set` belirteçleri gibi `where` belirteci bir anahtar sözcük değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-229">Like the `get` and `set` tokens in a property accessor, the `where` token is not a keyword.</span></span>

<span data-ttu-id="4c8df-230">Bir `where` yan tümcesinde verilen kısıtlamaların listesi, şu bileşenlerden herhangi birini içerebilir: tek bir birincil kısıtlama, bir veya daha fazla ikincil kısıtlama ve Oluşturucu kısıtlaması, `new()`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-230">The list of constraints given in a `where` clause can include any of the following components, in this order: a single primary constraint, one or more secondary constraints, and the constructor constraint, `new()`.</span></span>

<span data-ttu-id="4c8df-231">Birincil kısıtlama bir sınıf türü veya ***başvuru türü kısıtlaması*** `class` veya ***değer türü kısıtlaması*** `struct`olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-231">A primary constraint can be a class type or the ***reference type constraint*** `class` or the ***value type constraint*** `struct`.</span></span> <span data-ttu-id="4c8df-232">İkincil kısıtlama bir *type_parameter* veya *interface_type*olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-232">A secondary constraint can be a *type_parameter* or *interface_type*.</span></span>

<span data-ttu-id="4c8df-233">Başvuru türü kısıtlaması, tür parametresi için kullanılan bir tür bağımsız değişkeninin bir başvuru türü olması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-233">The reference type constraint specifies that a type argument used for the type parameter must be a reference type.</span></span> <span data-ttu-id="4c8df-234">Tüm sınıf türleri, arabirim türleri, temsilci türleri, dizi türleri ve başvuru türü olarak bilinen tür parametreleri (aşağıda tanımlandığı gibi), bu kısıtlamayı karşılar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-234">All class types, interface types, delegate types, array types, and type parameters known to be a reference type (as defined below) satisfy this constraint.</span></span>

<span data-ttu-id="4c8df-235">Değer türü kısıtlaması, tür parametresi için kullanılan bir tür bağımsız değişkeninin null yapılamayan bir değer türü olması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-235">The value type constraint specifies that a type argument used for the type parameter must be a non-nullable value type.</span></span> <span data-ttu-id="4c8df-236">Bu kısıtlamayı karşılayan, null olamayan tüm yapı türleri, sabit listesi türleri ve değer türü kısıtlamasına sahip tür parametreleri.</span><span class="sxs-lookup"><span data-stu-id="4c8df-236">All non-nullable struct types, enum types, and type parameters having the value type constraint satisfy this constraint.</span></span> <span data-ttu-id="4c8df-237">Değer türü olarak sınıflandırıldığından, null yapılabilir bir tür ([null yapılabilir türler](types.md#nullable-types)) değer türü kısıtlamasını karşılamadığına unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4c8df-237">Note that although classified as a value type, a nullable type ([Nullable types](types.md#nullable-types)) does not satisfy the value type constraint.</span></span> <span data-ttu-id="4c8df-238">Değer türü kısıtlamasına sahip bir tür parametresi de *constructor_constraint*sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-238">A type parameter having the value type constraint cannot also have the *constructor_constraint*.</span></span>

<span data-ttu-id="4c8df-239">İşaretçi türlerinin hiçbir şekilde tür bağımsız değişkeni olmasına izin verilmez ve başvuru türü veya değer türü kısıtlamalarını karşılamak için düşünülmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-239">Pointer types are never allowed to be type arguments and are not considered to satisfy either the reference type or value type constraints.</span></span>

<span data-ttu-id="4c8df-240">Bir kısıtlama bir sınıf türü, bir arabirim türü veya bir tür parametresi ise, bu tür parametresi için kullanılan her tür bağımsız değişkenin desteklemesi gereken en az "temel türü" değeri belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-240">If a constraint is a class type, an interface type, or a type parameter, that type specifies a minimal "base type" that every type argument used for that type parameter must support.</span></span> <span data-ttu-id="4c8df-241">Oluşturulmuş bir tür veya genel yöntem kullanıldığında, tür bağımsız değişkeni derleme zamanında tür parametresindeki kısıtlamalara karşı denetlenir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-241">Whenever a constructed type or generic method is used, the type argument is checked against the constraints on the type parameter at compile-time.</span></span> <span data-ttu-id="4c8df-242">Sağlanan tür bağımsız değişkeni, [kısıtlamaları](types.md#satisfying-constraints)karşıladığı koşullarda açıklanan koşullara uymalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-242">The type argument supplied must satisfy the conditions described in [Satisfying constraints](types.md#satisfying-constraints).</span></span>

<span data-ttu-id="4c8df-243">*Class_type* kısıtlamasının aşağıdaki kuralları karşılaması gerekir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-243">A *class_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="4c8df-244">Tür bir sınıf türü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-244">The type must be a class type.</span></span>
*  <span data-ttu-id="4c8df-245">Tür `sealed`olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-245">The type must not be `sealed`.</span></span>
*  <span data-ttu-id="4c8df-246">Tür şu türlerden biri olmalıdır: `System.Array`, `System.Delegate`, `System.Enum`veya `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-246">The type must not be one of the following types: `System.Array`, `System.Delegate`, `System.Enum`, or `System.ValueType`.</span></span>
*  <span data-ttu-id="4c8df-247">Tür `object`olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-247">The type must not be `object`.</span></span> <span data-ttu-id="4c8df-248">Tüm türler `object`türetildiğinden, bu tür bir kısıtlama izin verildiğinde hiçbir etkiye sahip olmaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-248">Because all types derive from `object`, such a constraint would have no effect if it were permitted.</span></span>
*  <span data-ttu-id="4c8df-249">Verilen tür parametresi için en fazla bir kısıtlama bir sınıf türü olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-249">At most one constraint for a given type parameter can be a class type.</span></span>

<span data-ttu-id="4c8df-250">*İnterface_type* kısıtlaması olarak belirtilen bir türün aşağıdaki kuralları karşılaması gerekir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-250">A type specified as an *interface_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="4c8df-251">Tür bir arabirim türü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-251">The type must be an interface type.</span></span>
*  <span data-ttu-id="4c8df-252">Bir tür, belirli bir `where` yan tümcesinde birden çok kez belirtilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-252">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="4c8df-253">Her iki durumda da kısıtlama, oluşturulmuş bir türün parçası olarak ilişkili tür veya yöntem bildiriminin herhangi bir tür parametresini içerebilir ve bu tür, bildirildiği türü içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-253">In either case, the constraint can involve any of the type parameters of the associated type or method declaration as part of a constructed type, and can involve the type being declared.</span></span>

<span data-ttu-id="4c8df-254">Tür parametresi kısıtlaması olarak belirtilen herhangi bir sınıf veya arabirim türünün, bildirildiği genel tür veya yöntem olarak en az erişilebilir ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-254">Any class or interface type specified as a type parameter constraint must be at least as accessible ([Accessibility constraints](basic-concepts.md#accessibility-constraints)) as the generic type or method being declared.</span></span>

<span data-ttu-id="4c8df-255">*Type_parameter* kısıtlaması olarak belirtilen bir türün aşağıdaki kuralları karşılaması gerekir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-255">A type specified as a *type_parameter* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="4c8df-256">Tür bir tür parametresi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-256">The type must be a type parameter.</span></span>
*  <span data-ttu-id="4c8df-257">Bir tür, belirli bir `where` yan tümcesinde birden çok kez belirtilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-257">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="4c8df-258">Ayrıca, bağımlılık grafiğinde tür parametrelerinin bağımlılık grafiğinde bir döngü olmaması gerekir; burada bağımlılık, tarafından tanımlanan geçişli bir ilişki olur:</span><span class="sxs-lookup"><span data-stu-id="4c8df-258">In addition there must be no cycles in the dependency graph of type parameters, where dependency is a transitive relation defined by:</span></span>

*  <span data-ttu-id="4c8df-259">Tür parametresi `T` tür parametresi için bir kısıtlama olarak kullanılırsa `S` `S` `T`***bağlıdır*** .</span><span class="sxs-lookup"><span data-stu-id="4c8df-259">If a type parameter `T` is used as a constraint for type parameter `S` then `S` ***depends on*** `T`.</span></span>
*  <span data-ttu-id="4c8df-260">Bir tür parametresi `S` bir tür parametresine bağlıdır `T` ve `T` bir tür parametresine bağlıdır `U` `S` ***bağlıdır*** .`U`</span><span class="sxs-lookup"><span data-stu-id="4c8df-260">If a type parameter `S` depends on a type parameter `T` and `T` depends on a type parameter `U` then `S` ***depends on*** `U`.</span></span>

<span data-ttu-id="4c8df-261">Bu ilişki verildiğinde, bir tür parametresinin kendisine (doğrudan veya dolaylı olarak) bağımlı olması için derleme zamanı hatası vardır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-261">Given this relation, it is a compile-time error for a type parameter to depend on itself (directly or indirectly).</span></span>

<span data-ttu-id="4c8df-262">Herhangi bir kısıtlama bağımlı tür parametreleri arasında tutarlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-262">Any constraints must be consistent among dependent type parameters.</span></span> <span data-ttu-id="4c8df-263">Tür parametresi `S` `T` tür parametresine bağımlıysa:</span><span class="sxs-lookup"><span data-stu-id="4c8df-263">If type parameter `S` depends on type parameter `T` then:</span></span>

*  <span data-ttu-id="4c8df-264">`T` değer türü kısıtlamasına sahip olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-264">`T` must not have the value type constraint.</span></span> <span data-ttu-id="4c8df-265">Aksi takdirde, `T`, `T`ile aynı tür olmaya zorlanacak ve iki tür parametresi gereksinimini ortadan kaldıran `S`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-265">Otherwise, `T` is effectively sealed so `S` would be forced to be the same type as `T`, eliminating the need for two type parameters.</span></span>
*  <span data-ttu-id="4c8df-266">`S` değer türü kısıtlaması varsa `T` *class_type* kısıtlamasına sahip olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-266">If `S` has the value type constraint then `T` must not have a *class_type* constraint.</span></span>
*  <span data-ttu-id="4c8df-267">`S` bir *class_type* kısıtlaması `A` ve `T` *class_type kısıtlaması varsa `B` bir kimlik* dönüştürmesi ya da `A` 'den `B` örtük bir başvuru dönüştürmesi olması gerekir.`B``A`</span><span class="sxs-lookup"><span data-stu-id="4c8df-267">If `S` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>
*  <span data-ttu-id="4c8df-268">`S` ayrıca tür parametresine `U` ve `U` bir *class_type* kısıtlaması `A` ve `T` class_type kısıtlaması varsa `B` `A` bir *`B` kısıtlamasına ya* da `B` 'den `A`bir örtük başvuru dönüştürmesi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-268">If `S` also depends on type parameter `U` and `U` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>

<span data-ttu-id="4c8df-269">`S` değer türü kısıtlamasına sahip olması ve `T` başvuru türü kısıtlamasına sahip olması için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-269">It is valid for `S` to have the value type constraint and `T` to have the reference type constraint.</span></span> <span data-ttu-id="4c8df-270">Bu, `T` `System.Object`, `System.ValueType`, `System.Enum`ve herhangi bir arabirim türüne göre sınırlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-270">Effectively this limits `T` to the types `System.Object`, `System.ValueType`, `System.Enum`, and any interface type.</span></span>

<span data-ttu-id="4c8df-271">Bir tür parametresi için `where` yan tümcesi bir Oluşturucu kısıtlaması içeriyorsa (Bu biçimde `new()`), türü örnekleri oluşturmak için `new` işlecini kullanmak mümkündür ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-271">If the `where` clause for a type parameter includes a constructor constraint (which has the form `new()`), it is possible to use the `new` operator to create instances of the type ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span> <span data-ttu-id="4c8df-272">Oluşturucu kısıtlaması olan bir tür parametresi için kullanılan herhangi bir tür bağımsız değişkeni Ortak parametresiz bir oluşturucuya sahip olmalıdır (Bu Oluşturucu herhangi bir değer türü için örtülü olarak bulunur) veya değer türü kısıtlaması veya Oluşturucu kısıtlamasına sahip bir tür parametresi olmalıdır (Ayrıntılar için bkz. [tür parametresi kısıtlamaları](classes.md#type-parameter-constraints) ).</span><span class="sxs-lookup"><span data-stu-id="4c8df-272">Any type argument used for a type parameter with a constructor constraint must have a public parameterless constructor (this constructor implicitly exists for any value type) or be a type parameter having the value type constraint or constructor constraint (see [Type parameter constraints](classes.md#type-parameter-constraints) for details).</span></span>

<span data-ttu-id="4c8df-273">Kısıtlamaların örnekleri aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-273">The following are examples of constraints:</span></span>
```csharp
interface IPrintable
{
    void Print();
}

interface IComparable<T>
{
    int CompareTo(T value);
}

interface IKeyProvider<T>
{
    T GetKey();
}

class Printer<T> where T: IPrintable {...}

class SortedList<T> where T: IComparable<T> {...}

class Dictionary<K,V>
    where K: IComparable<K>
    where V: IPrintable, IKeyProvider<K>, new()
{
    ...
}
```

<span data-ttu-id="4c8df-274">Tür parametrelerinin bağımlılık grafiğinde bir döngüye neden olduğu için aşağıdaki örnek hata durumunda:</span><span class="sxs-lookup"><span data-stu-id="4c8df-274">The following example is in error because it causes a circularity in the dependency graph of the type parameters:</span></span>
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

<span data-ttu-id="4c8df-275">Aşağıdaki örneklerde, ek geçersiz durumlar gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-275">The following examples illustrate additional invalid situations:</span></span>
```csharp
class Sealed<S,T>
    where S: T
    where T: struct        // Error, T is sealed
{
    ...
}

class A {...}

class B {...}

class Incompat<S,T>
    where S: A, T
    where T: B                // Error, incompatible class-type constraints
{
    ...
}

class StructWithClass<S,T,U>
    where S: struct, T
    where T: U
    where U: A                // Error, A incompatible with struct
{
    ...
}
```

<span data-ttu-id="4c8df-276">`T` bir tür parametresinin ***etkin temel sınıfı*** aşağıdaki gibi tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-276">The ***effective base class*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="4c8df-277">`T` birincil kısıtlamalara veya tür parametresi kısıtlamalarına sahip değilse, etkin taban sınıfı `object`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-277">If `T` has no primary constraints or type parameter constraints, its effective base class is `object`.</span></span>
*  <span data-ttu-id="4c8df-278">`T` değer türü kısıtlaması varsa, etkin taban sınıfı `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-278">If `T` has the value type constraint, its effective base class is `System.ValueType`.</span></span>
*  <span data-ttu-id="4c8df-279">`T` bir *class_type* kısıtlaması `C` ancak *type_parameter* kısıtlaması yoksa, etkin taban sınıfı `C`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-279">If `T` has a *class_type* constraint `C` but no *type_parameter* constraints, its effective base class is `C`.</span></span>
*  <span data-ttu-id="4c8df-280">`T` *class_type* kısıtlaması yoksa, ancak bir veya daha fazla *type_parameter* kısıtlaması varsa, kendi etkin taban sınıfı, *type_parameter* kısıtlamalarının etkin temel sınıfları kümesinde en çok kullanılan türdür ([yükseltilmemiş dönüştürme işleçleri](conversions.md#lifted-conversion-operators)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-280">If `T` has no *class_type* constraint but has one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set of effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="4c8df-281">Tutarlılık kuralları, bu tür bir en kapsamlı türün mevcut olmasını güvence altına alıyor.</span><span class="sxs-lookup"><span data-stu-id="4c8df-281">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="4c8df-282">`T` hem *class_type* kısıtlaması hem de bir veya daha fazla *type_parameter* kısıtlaması varsa, etkin taban sınıfı, `T` *class_type* kısıtlamasından ve *type_parameter* kısıtlamalarının etkin taban sınıflarına sahip olan kümesinde en kapsamlı tür ([yükseltilmemiş dönüştürme işleçleri](conversions.md#lifted-conversion-operators)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-282">If `T` has both a *class_type* constraint and one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set consisting of the *class_type* constraint of `T` and the effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="4c8df-283">Tutarlılık kuralları, bu tür bir en kapsamlı türün mevcut olmasını güvence altına alıyor.</span><span class="sxs-lookup"><span data-stu-id="4c8df-283">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="4c8df-284">`T` başvuru türü kısıtlamasına sahipse ancak *class_type* kısıtlaması yoksa, etkin taban sınıfı `object`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-284">If `T` has the reference type constraint but no *class_type* constraints, its effective base class is `object`.</span></span>

<span data-ttu-id="4c8df-285">Bu kuralların amacına yönelik olarak, T, bir *value_type*olan bir kısıtlama `V`, bunun yerine *class_type*olan en belirgin `V` taban türünü kullanın.</span><span class="sxs-lookup"><span data-stu-id="4c8df-285">For the purpose of these rules, if T has a constraint `V` that is a *value_type*, use instead the most specific base type of `V` that is a *class_type*.</span></span> <span data-ttu-id="4c8df-286">Bu, açıkça verilen kısıtlamada asla gerçekleşmeyebilir, ancak genel bir yöntemin kısıtlamaları geçersiz kılan bir yöntem bildirimi veya bir arabirim yönteminin açık bir uygulamasıyla dolaylı olarak devralındığında gerçekleşebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-286">This can never happen in an explicitly given constraint, but may occur when the constraints of a generic method are implicitly inherited by an overriding method declaration or an explicit implementation of an interface method.</span></span>

<span data-ttu-id="4c8df-287">Bu kurallar, etkin temel sınıfın her zaman bir *class_type*olduğundan emin olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-287">These rules ensure that the effective base class is always a *class_type*.</span></span>

<span data-ttu-id="4c8df-288">Bir tür parametresinin ***etkin arabirim kümesi*** `T` aşağıdaki gibi tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-288">The ***effective interface set*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="4c8df-289">`T` *secondary_constraints*yoksa, etkin arabirimi kümesi boş olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-289">If `T` has no *secondary_constraints*, its effective interface set is empty.</span></span>
*  <span data-ttu-id="4c8df-290">`T` *interface_type* kısıtlamaları varsa ancak *type_parameter* kısıtlama yoksa, etkin arabirimi kümesi *interface_type* kısıtlamaları kümesidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-290">If `T` has *interface_type* constraints but no *type_parameter* constraints, its effective interface set is its set of *interface_type* constraints.</span></span>
*  <span data-ttu-id="4c8df-291">`T` *interface_type* kısıtlaması yoksa ancak *type_parameter* kısıtlamalarına sahipse, etkin arabirim kümesi *type_parameter* kısıtlamalarının etkin arabirim kümelerinin birleşimidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-291">If `T` has no *interface_type* constraints but has *type_parameter* constraints, its effective interface set is the union of the effective interface sets of its *type_parameter* constraints.</span></span>
*  <span data-ttu-id="4c8df-292">`T` hem *interface_type* kısıtlamalarına hem de *type_parameter* kısıtlamalarına sahipse, etkin arabirim kümesi *interface_type* kısıtlamaları kümesinin ve *type_parameter* kısıtlamalarının etkin arabirim kümelerinin birleşimidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-292">If `T` has both *interface_type* constraints and *type_parameter* constraints, its effective interface set is the union of its set of *interface_type* constraints and the effective interface sets of its *type_parameter* constraints.</span></span>

<span data-ttu-id="4c8df-293">Bir tür parametresinin başvuru türü kısıtlaması varsa veya etkin taban sınıfı `object` veya `System.ValueType`değilse, ***başvuru türü olarak bilinir*** .</span><span class="sxs-lookup"><span data-stu-id="4c8df-293">A type parameter is ***known to be a reference type*** if it has the reference type constraint or its effective base class is not `object` or `System.ValueType`.</span></span>

<span data-ttu-id="4c8df-294">Kısıtlanmış tür parametre türünün değerleri, kısıtlamalar tarafından kapsanan örnek üyelerine erişmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-294">Values of a constrained type parameter type can be used to access the instance members implied by the constraints.</span></span> <span data-ttu-id="4c8df-295">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-295">In the example</span></span>
```csharp
interface IPrintable
{
    void Print();
}

class Printer<T> where T: IPrintable
{
    void PrintOne(T x) {
        x.Print();
    }
}
```
<span data-ttu-id="4c8df-296">`T` her zaman `IPrintable`uygulamak üzere kısıtlandığından, `IPrintable` yöntemleri doğrudan `x` üzerinde çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-296">the methods of `IPrintable` can be invoked directly on `x` because `T` is constrained to always implement `IPrintable`.</span></span>

### <a name="class-body"></a><span data-ttu-id="4c8df-297">Sınıf gövdesi</span><span class="sxs-lookup"><span data-stu-id="4c8df-297">Class body</span></span>

<span data-ttu-id="4c8df-298">Bir sınıfın *class_body* , bu sınıfın üyelerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-298">The *class_body* of a class defines the members of that class.</span></span>

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a><span data-ttu-id="4c8df-299">Kısmi türler</span><span class="sxs-lookup"><span data-stu-id="4c8df-299">Partial types</span></span>

<span data-ttu-id="4c8df-300">Bir tür bildirimi, birden çok ***Kısmi tür***bildirimine bölünebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-300">A type declaration can be split across multiple ***partial type declarations***.</span></span> <span data-ttu-id="4c8df-301">Tür bildirimi, bu bölümdeki kurallara göre, programın derleme zamanı ve çalışma zamanı işlemenin geri kalanı sırasında tek bir bildirim olarak değerlendirilmesinin ardından parçalarından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-301">The type declaration is constructed from its parts by following the rules in this section, whereupon it is treated as a single declaration during the remainder of the compile-time and run-time processing of the program.</span></span>

<span data-ttu-id="4c8df-302">Bir *class_declaration*, *struct_declaration* veya *interface_declaration* , bir `partial` değiştiricisi içeriyorsa kısmi tür bildirimini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="4c8df-302">A *class_declaration*, *struct_declaration* or *interface_declaration* represents a partial type declaration if it includes a `partial` modifier.</span></span> <span data-ttu-id="4c8df-303">`partial` bir anahtar sözcük değildir ve yalnızca bir değiştirici olarak, bir tür bildiriminde, `struct` veya `interface` ya da bir yöntem bildiriminde `void` bir şekilde `class`görünür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-303">`partial` is not a keyword, and only acts as a modifier if it appears immediately before one of the keywords `class`, `struct` or `interface` in a type declaration, or before the type `void` in a method declaration.</span></span> <span data-ttu-id="4c8df-304">Diğer bağlamlarda, normal tanımlayıcı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-304">In other contexts it can be used as a normal identifier.</span></span>

<span data-ttu-id="4c8df-305">Kısmi tür bildiriminin her bir bölümü `partial` değiştirici içermelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-305">Each part of a partial type declaration must include a `partial` modifier.</span></span> <span data-ttu-id="4c8df-306">Aynı ada sahip olmalıdır ve diğer bölümlerle aynı ad alanında veya tür bildiriminde bildirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-306">It must have the same name  and be declared in the same namespace or type declaration as the other parts.</span></span> <span data-ttu-id="4c8df-307">`partial` değiştiricisi, tür bildiriminin ek bölümlerinin başka bir yerde mevcut olabileceğini gösterir, ancak bu tür ek parçaların varlığı bir gereksinim değildir; `partial` değiştiricisini içermesi için tek bir bildirime sahip bir tür için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-307">The `partial` modifier indicates that additional parts of the type declaration may exist elsewhere, but the existence of such additional parts is not a requirement; it is valid for a type with a single declaration to include the `partial` modifier.</span></span>

<span data-ttu-id="4c8df-308">Kısmi bir türün tüm bölümlerinin, parçaların derleme zamanında tek bir tür bildirimine birleştirilebilmesi için birlikte derlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-308">All parts of a partial type must be compiled together such that the parts can be merged at compile-time into a single type declaration.</span></span> <span data-ttu-id="4c8df-309">Kısmi türler özellikle derlenmiş türlerin genişletilmesini izin vermez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-309">Partial types specifically do not allow already compiled types to be extended.</span></span>

<span data-ttu-id="4c8df-310">İç içe türler `partial` değiştiricisi kullanılarak birden çok bölümde bildirilemez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-310">Nested types may be declared in multiple parts by using the `partial` modifier.</span></span> <span data-ttu-id="4c8df-311">Genellikle, kapsayan tür `partial` kullanılarak ve iç içe türün her bölümü, kapsayan türün farklı bir bölümünde tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-311">Typically, the containing type is declared using `partial` as well, and each part of the nested type is declared in a different part of the containing type.</span></span>

<span data-ttu-id="4c8df-312">Temsilci veya Enum bildirimlerinde `partial` değiştiriciye izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-312">The `partial` modifier is not permitted on delegate or enum declarations.</span></span>

### <a name="attributes"></a><span data-ttu-id="4c8df-313">Öznitelikler</span><span class="sxs-lookup"><span data-stu-id="4c8df-313">Attributes</span></span>

<span data-ttu-id="4c8df-314">Kısmi bir türün öznitelikleri belirtilmemiş bir sıra, parçaların her birinin öznitelikleri birleştirilerek belirlenir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-314">The attributes of a partial type are determined by combining, in an unspecified order, the attributes of each of the parts.</span></span> <span data-ttu-id="4c8df-315">Bir öznitelik birden çok parçaya yerleştirilmişse, öznitelik türü üzerinde birden çok kez belirtmeye eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-315">If an attribute is placed on multiple parts, it is equivalent to specifying the attribute multiple times on the type.</span></span> <span data-ttu-id="4c8df-316">Örneğin, iki bölüm:</span><span class="sxs-lookup"><span data-stu-id="4c8df-316">For example, the two parts:</span></span>

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
<span data-ttu-id="4c8df-317">, şöyle bir bildirime eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-317">are equivalent to a declaration such as:</span></span>
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

<span data-ttu-id="4c8df-318">Tür parametrelerinin öznitelikleri benzer bir biçimde birleştirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-318">Attributes on type parameters combine in a similar fashion.</span></span>

### <a name="modifiers"></a><span data-ttu-id="4c8df-319">Değiştiriciler</span><span class="sxs-lookup"><span data-stu-id="4c8df-319">Modifiers</span></span>

<span data-ttu-id="4c8df-320">Kısmi bir tür bildirimi bir erişilebilirlik belirtimi (`public`, `protected`, `internal`ve `private` değiştiricileri) içerdiğinde, bir erişilebilirlik belirtimi içeren diğer tüm bölümleri kabul etmelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-320">When a partial type declaration includes an accessibility specification (the `public`, `protected`, `internal`, and `private` modifiers) it must agree with all other parts that include an accessibility specification.</span></span> <span data-ttu-id="4c8df-321">Kısmi bir türün hiçbir bölümü bir erişilebilirlik belirtimi içeriyorsa, türe uygun varsayılan erişilebilirlik (belirtilen[Erişilebilirlik](basic-concepts.md#declared-accessibility)) verilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-321">If no part of a partial type includes an accessibility specification, the type is given the appropriate default accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="4c8df-322">İç içe türün bir veya daha fazla kısmi bildirimi `new` değiştirici içeriyorsa, iç içe geçmiş türü devralınmış bir üyeyi ([Devralma yoluyla gizleme](basic-concepts.md#hiding-through-inheritance)) gizliyorsanız hiçbir uyarı bildirilmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-322">If one or more partial declarations of a nested type include a `new` modifier, no warning is reported if the nested type hides an inherited member ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

<span data-ttu-id="4c8df-323">Bir sınıfın bir veya daha fazla kısmi bildirimi `abstract` değiştirici içeriyorsa, sınıf soyut ([soyut sınıflar](classes.md#abstract-classes)) olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-323">If one or more partial declarations of a class include an `abstract` modifier, the class is considered abstract ([Abstract classes](classes.md#abstract-classes)).</span></span> <span data-ttu-id="4c8df-324">Aksi takdirde, sınıfı Özet olmayan olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-324">Otherwise, the class is considered non-abstract.</span></span>

<span data-ttu-id="4c8df-325">Bir sınıfın bir veya daha fazla kısmi bildirimi `sealed` değiştirici içeriyorsa, sınıf sealed ([Sealed sınıflar](classes.md#sealed-classes)) olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-325">If one or more partial declarations of a class include a `sealed` modifier, the class is considered sealed ([Sealed classes](classes.md#sealed-classes)).</span></span> <span data-ttu-id="4c8df-326">Aksi takdirde, sınıfı korumasız kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-326">Otherwise, the class is considered unsealed.</span></span>

<span data-ttu-id="4c8df-327">Bir sınıfın hem abstract hem de Sealed olamayacağını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4c8df-327">Note that a class cannot be both abstract and sealed.</span></span>

<span data-ttu-id="4c8df-328">`unsafe` değiştiricisi kısmi bir tür bildiriminde kullanıldığında, yalnızca bu belirli parça güvenli olmayan bir bağlam ([güvenli olmayan bağlamlar](unsafe-code.md#unsafe-contexts)) olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-328">When the `unsafe` modifier is used on a partial type declaration, only that particular part is considered an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

### <a name="type-parameters-and-constraints"></a><span data-ttu-id="4c8df-329">Tür parametreleri ve kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="4c8df-329">Type parameters and constraints</span></span>

<span data-ttu-id="4c8df-330">Genel bir tür birden çok bölümde bildirilirse, her parçanın tür parametrelerini durumu olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-330">If a generic type is declared in multiple parts, each part must state the type parameters.</span></span> <span data-ttu-id="4c8df-331">Her parçanın aynı sayıda tür parametreleri ve her tür parametresi için aynı adı sırasıyla aynı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-331">Each part must have the same number of type parameters, and the same name for each type parameter, in order.</span></span>

<span data-ttu-id="4c8df-332">Kısmi genel tür bildiriminde kısıtlamalar (`where` yan tümceleri) varsa, kısıtlamalar, kısıtlamaları içeren diğer tüm bölümleri kabul etmelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-332">When a partial generic type declaration includes constraints (`where` clauses), the constraints must agree with all other parts that include constraints.</span></span> <span data-ttu-id="4c8df-333">Özellikle, kısıtlamaları içeren her parçanın aynı tür parametreleri kümesi için kısıtlamaları olmalıdır ve her tür parametresi için, birincil, ikincil ve Oluşturucu kısıtlamalarının kümelerinin eşdeğeri olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-333">Specifically, each part that includes constraints must have constraints for the same set of type parameters, and for each type parameter the sets of primary, secondary, and constructor constraints must be equivalent.</span></span> <span data-ttu-id="4c8df-334">İki kısıtlama kümesi aynı üyeleri içeriyorsa eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-334">Two sets of constraints are equivalent if they contain the same members.</span></span> <span data-ttu-id="4c8df-335">Kısmi bir genel türün hiçbir bölümü tür parametresi kısıtlamalarını belirtiyorsa tür parametreleri Kısıtlanmamış olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-335">If no part of a partial generic type specifies type parameter constraints, the type parameters are considered unconstrained.</span></span>

<span data-ttu-id="4c8df-336">Örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-336">The example</span></span>
```csharp
partial class Dictionary<K,V>
    where K: IComparable<K>
    where V: IKeyProvider<K>, IPersistable
{
    ...
}

partial class Dictionary<K,V>
    where V: IPersistable, IKeyProvider<K>
    where K: IComparable<K>
{
    ...
}

partial class Dictionary<K,V>
{
    ...
}
```
<span data-ttu-id="4c8df-337">, kısıtlamaları (ilk iki) içeren parçalar, sırasıyla aynı tür parametreleri kümesi için aynı birincil, ikincil ve Oluşturucu kısıtlamaları kümesini etkin bir şekilde belirttiğinden doğrudur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-337">is correct because those parts that include constraints (the first two) effectively specify the same set of primary, secondary, and constructor constraints for the same set of type parameters, respectively.</span></span>

### <a name="base-class"></a><span data-ttu-id="4c8df-338">Temel sınıf</span><span class="sxs-lookup"><span data-stu-id="4c8df-338">Base class</span></span>

<span data-ttu-id="4c8df-339">Kısmi bir sınıf bildirimi bir temel sınıf belirtimi içerdiğinde, temel sınıf belirtimi içeren diğer tüm bölümleri kabul etmelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-339">When a partial class declaration includes a base class specification it must agree with all other parts that include a base class specification.</span></span> <span data-ttu-id="4c8df-340">Kısmi bir sınıfın hiçbir bölümü bir temel sınıf belirtimi içeriyorsa, taban sınıf `System.Object` olur ([temel sınıflar](classes.md#base-classes)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-340">If no part of a partial class includes a base class specification, the base class becomes `System.Object` ([Base classes](classes.md#base-classes)).</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="4c8df-341">Temel arabirimler</span><span class="sxs-lookup"><span data-stu-id="4c8df-341">Base interfaces</span></span>

<span data-ttu-id="4c8df-342">Birden çok bölümde belirtilen bir türün temel arabirimler kümesi, her bölümde belirtilen temel arabirimlerin birleşimidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-342">The set of base interfaces for a type declared in multiple parts is the union of the base interfaces specified on each part.</span></span> <span data-ttu-id="4c8df-343">Belirli bir temel arabirim her bölümde yalnızca bir kez adlandırılabilir, ancak birden fazla parçanın aynı temel arabirimleri adlandırmasına izin verilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-343">A particular base interface may only be named once on each part, but it is permitted for multiple parts to name the same base interface(s).</span></span> <span data-ttu-id="4c8df-344">Yalnızca belirli bir temel arabirimin üyelerinin bir uygulanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-344">There must only be one implementation of the members of any given base interface.</span></span>

<span data-ttu-id="4c8df-345">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-345">In the example</span></span>
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
<span data-ttu-id="4c8df-346">sınıf `C` için temel arabirimler kümesi `IA`, `IB`ve `IC`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-346">the set of base interfaces for class `C` is `IA`, `IB`, and `IC`.</span></span>

<span data-ttu-id="4c8df-347">Genellikle, her parça bu bölümde belirtilen arabirim (ler) in bir uygulamasını sağlar; Ancak, bu bir gereksinim değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-347">Typically, each part provides an implementation of the interface(s) declared on that part; however, this is not a requirement.</span></span> <span data-ttu-id="4c8df-348">Bir bölüm, farklı bir bölümde belirtilen bir arabirim için uygulama sağlayabilir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-348">A part may provide the implementation for an interface declared on a different part:</span></span>
```csharp
partial class X
{
    int IComparable.CompareTo(object o) {...}
}

partial class X: IComparable
{
    ...
}
```

### <a name="members"></a><span data-ttu-id="4c8df-349">Üyeler</span><span class="sxs-lookup"><span data-stu-id="4c8df-349">Members</span></span>

<span data-ttu-id="4c8df-350">Kısmi yöntemlerin ([kısmi Yöntemler](classes.md#partial-methods)) dışında, birden çok bölümde belirtilen bir türün üyeleri kümesi, her bölümde belirtilen üye kümesinin birleşimidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-350">With the exception of partial methods ([Partial methods](classes.md#partial-methods)), the set of members of a type declared in multiple parts is simply the union of the set of members declared in each part.</span></span> <span data-ttu-id="4c8df-351">Tür bildiriminin tüm bölümlerinin gövdeleri aynı bildirim alanını ([Bildirimler](basic-concepts.md#declarations)) ve her üyenin kapsamını ([kapsamlar](basic-concepts.md#scopes)) paylaşır ve tüm parçaların gövdeleriyle birlikte genişletilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-351">The bodies of all parts of the type declaration share the same declaration space ([Declarations](basic-concepts.md#declarations)), and the scope of each member ([Scopes](basic-concepts.md#scopes)) extends to the bodies of all the parts.</span></span> <span data-ttu-id="4c8df-352">Herhangi bir üyenin erişilebilirlik etki alanı her zaman kapsayan türün tüm parçalarını içerir; tek bir bölümde bildirildiği `private` üyeye, başka bir bölümden serbestçe erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-352">The accessibility domain of any member always includes all the parts of the enclosing type; a `private` member declared in one part is freely accessible from another part.</span></span> <span data-ttu-id="4c8df-353">Üyenin `partial` değiştiriciyle bir tür olmadığı durumlar dışında, türün birden fazla bölümünde aynı üyeyi bildirmek için derleme zamanı hatası vardır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-353">It is a compile-time error to declare the same member in more than one part of the type, unless that member is a type with the `partial` modifier.</span></span>

```csharp
partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int y;
    }
}

partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int z;
    }
}
```

<span data-ttu-id="4c8df-354">Bir tür içindeki üyelerin sıralaması kod için C# nadiren önemlidir, ancak diğer diller ve ortamlarla arabirim oluştururken önemli olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-354">The ordering of members within a type is rarely significant to C# code, but may be significant when interfacing with other languages and environments.</span></span> <span data-ttu-id="4c8df-355">Bu durumlarda, birden çok bölümde belirtilen bir tür içindeki üyelerin sıralaması tanımsızdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-355">In these cases, the ordering of members within a type declared in multiple parts is undefined.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="4c8df-356">Kısmi Yöntemler</span><span class="sxs-lookup"><span data-stu-id="4c8df-356">Partial methods</span></span>

<span data-ttu-id="4c8df-357">Kısmi Yöntemler, bir tür bildiriminin bir bölümünde tanımlanabilir ve başka bir şekilde uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-357">Partial methods can be defined in one part of a type declaration and implemented in another.</span></span> <span data-ttu-id="4c8df-358">Uygulama isteğe bağlıdır; kısmi yöntemi herhangi bir bölüm uygularsa, kısmi yöntem bildirimi ve buna yapılan tüm çağrılar, parçaların birleşiminden kaynaklanan tür bildiriminden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-358">The implementation is optional; if no part implements the partial method, the partial method declaration and all calls to it are removed from the type declaration resulting from the combination of the parts.</span></span>

<span data-ttu-id="4c8df-359">Kısmi Yöntemler erişim değiştiricileri tanımlanamaz, ancak örtülü olarak `private`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-359">Partial methods cannot define access modifiers, but are implicitly `private`.</span></span> <span data-ttu-id="4c8df-360">Dönüş türü `void`olmalıdır ve parametreleri `out` değiştiriciye sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-360">Their return type must be `void`, and their parameters cannot have the `out` modifier.</span></span> <span data-ttu-id="4c8df-361">Tanımlayıcı `partial`, bir yöntem bildiriminde özel bir anahtar sözcük olarak tanınır ve yalnızca `void` türünden önce görünür. Aksi takdirde, normal tanımlayıcı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-361">The identifier `partial` is recognized as a special keyword in a method declaration only if it appears right before the `void` type; otherwise it can be used as a normal identifier.</span></span> <span data-ttu-id="4c8df-362">Kısmi bir yöntem, arabirim yöntemlerini açıkça uygulayamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-362">A partial method cannot explicitly implement interface methods.</span></span>

<span data-ttu-id="4c8df-363">İki tür kısmi yöntem bildirimi vardır: Yöntem bildiriminin gövdesi noktalı virgül ise, bildirim bir ***tanımlayıcı kısmi yöntem bildirimi***olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-363">There are two kinds of partial method declarations: If the body of the method declaration is a semicolon, the declaration is said to be a ***defining partial method declaration***.</span></span> <span data-ttu-id="4c8df-364">Gövde bir *blok*olarak verilirse, bildirim bir ***Uygulama kısmi yöntem bildirimi***olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-364">If the body is given as a *block*, the declaration is said to be an ***implementing partial method declaration***.</span></span> <span data-ttu-id="4c8df-365">Bir tür bildiriminin bölümlerinde, belirli bir imzayla yalnızca bir adet kısmi yöntem bildirimi tanımlama olabilir ve belirli bir imzaya sahip kısmi yöntem bildirimi yalnızca bir uygulama olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-365">Across the parts of a type declaration there can be only one defining partial method declaration with a given signature, and there can be only one implementing partial method declaration with a given signature.</span></span> <span data-ttu-id="4c8df-366">Bir uygulama kısmi yöntem bildirimi verilirse, karşılık gelen bir tanımlayıcı kısmi yöntem bildirimi mevcut olmalıdır ve bildirimlerin aşağıdaki gibi eşleşmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-366">If an implementing partial method declaration is given, a corresponding defining partial method declaration must exist, and the declarations must match as specified in the following:</span></span>

* <span data-ttu-id="4c8df-367">Bildirimlerin aynı değiştiricilere sahip olması gerekir (aynı sırada olması olmasa da), yöntem adı, tür parametrelerinin sayısı ve parametre sayısı.</span><span class="sxs-lookup"><span data-stu-id="4c8df-367">The declarations must have the same modifiers (although not necessarily in the same order), method name, number of type parameters and number of parameters.</span></span>
* <span data-ttu-id="4c8df-368">Bildirimlerdeki karşılık gelen parametreler aynı değiştiricilere sahip olmalıdır (aynı sırada olması olmasa da) ve aynı türlerdir (tür parametre adlarında mod farklılıkları).</span><span class="sxs-lookup"><span data-stu-id="4c8df-368">Corresponding parameters in the declarations must have the same modifiers (although not necessarily in the same order) and the same types (modulo differences in type parameter names).</span></span>
* <span data-ttu-id="4c8df-369">Bildirimlerdeki karşılık gelen tür parametreleri aynı kısıtlamalara (tür parametresi adlarında mod farklılıkları) sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-369">Corresponding type parameters in the declarations must have the same constraints (modulo differences in type parameter names).</span></span>

<span data-ttu-id="4c8df-370">Uygulama kısmi yöntem bildirimi, karşılık gelen tanımlayan kısmi Yöntem bildirimiyle aynı bölümde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-370">An implementing partial method declaration can appear in the same part as the corresponding defining partial method declaration.</span></span>

<span data-ttu-id="4c8df-371">Yalnızca tanımlama bir kısmi yöntem aşırı yükleme çözümüne katılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-371">Only a defining partial method participates in overload resolution.</span></span> <span data-ttu-id="4c8df-372">Bu nedenle, bir uygulama bildiriminin verilip verilmediğini belirtir, çağırma ifadeleri kısmi yöntemin çağırmaları için çözüm alabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-372">Thus, whether or not an implementing declaration is given, invocation expressions may resolve to invocations of the partial method.</span></span> <span data-ttu-id="4c8df-373">Kısmi bir yöntem her zaman `void`döndürdüğünden, bu tür çağırma ifadeleri her zaman ifade deyimleri olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-373">Because a partial method always returns `void`, such invocation expressions will always be expression statements.</span></span> <span data-ttu-id="4c8df-374">Ayrıca, kısmi bir yöntem örtük olarak `private`, bu tür deyimler her zaman, kısmi yöntemin bildirildiği tür bildiriminin parçalarından biri içinde olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-374">Furthermore, because a partial method is implicitly `private`, such statements will always occur within one of the parts of the type declaration within which the partial method is declared.</span></span>

<span data-ttu-id="4c8df-375">Kısmi bir tür bildiriminin hiçbir bölümü verili bir kısmi Yöntem için uygulama bildirimi içermiyorsa, çağıran herhangi bir ifade deyimi yalnızca Birleşik tür bildiriminden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-375">If no part of a partial type declaration contains an implementing declaration for a given partial method, any expression statement invoking it is simply removed from the combined type declaration.</span></span> <span data-ttu-id="4c8df-376">Bu nedenle, bileşen ifadeleri dahil olmak üzere çağırma ifadesinin çalışma zamanında hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-376">Thus the invocation expression, including any constituent expressions, has no effect at run-time.</span></span> <span data-ttu-id="4c8df-377">Kısmi yöntemin kendisi de kaldırılır ve birleştirilmiş tür bildiriminin üyesi olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-377">The partial method itself is also removed and will not be a member of the combined type declaration.</span></span>

<span data-ttu-id="4c8df-378">Belirli bir kısmi Yöntem için uygulama bildirimi varsa, kısmi yöntemlerin etkinleştirmeleri korunur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-378">If an implementing declaration exist for a given partial method, the invocations of the partial methods are retained.</span></span> <span data-ttu-id="4c8df-379">Kısmi Yöntem, aşağıdaki durumlar hariç, uygulama kısmi Yöntem bildirimine benzer bir yöntem bildirimine göre artış sağlar:</span><span class="sxs-lookup"><span data-stu-id="4c8df-379">The partial method gives rise to a method declaration similar to the implementing partial method declaration except for the following:</span></span>

* <span data-ttu-id="4c8df-380">`partial` değiştiricisi dahil değildir</span><span class="sxs-lookup"><span data-stu-id="4c8df-380">The `partial` modifier is not included</span></span>
* <span data-ttu-id="4c8df-381">Elde edilen yöntem bildiriminde bulunan öznitelikler, tanımlama ve Uygulama kısmi Yöntem bildiriminin belirtilmemiş sırada tanımlanmasının Birleşik öznitelikleridir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-381">The attributes in the resulting method declaration are the combined attributes of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="4c8df-382">Yinelemeler kaldırılmaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-382">Duplicates are not removed.</span></span>
* <span data-ttu-id="4c8df-383">Sonuç yöntemi bildiriminin parametreleri üzerindeki öznitelikleri, tanımlanmasının karşılık gelen parametrelerinin Birleşik öznitelikleridir ve kısmi Yöntem bildiriminin uygulanması belirtilmemiş sırayla yapılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-383">The attributes on the parameters of the resulting method declaration are the combined attributes of the corresponding parameters of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="4c8df-384">Yinelemeler kaldırılmaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-384">Duplicates are not removed.</span></span>

<span data-ttu-id="4c8df-385">Bir tanımlama bildirimi ve kısmi Yöntem için bir uygulama bildirimi verilmezse, aşağıdaki kısıtlamalar uygulanır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-385">If a defining declaration but not an implementing declaration is given for a partial method M, the following restrictions apply:</span></span>

* <span data-ttu-id="4c8df-386">Yönteme bir temsilci oluşturmak için derleme zamanı hatası ([temsilci oluşturma ifadeleri](expressions.md#delegate-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-386">It is a compile-time error to create a delegate to method ([Delegate creation expressions](expressions.md#delegate-creation-expressions)).</span></span>
* <span data-ttu-id="4c8df-387">Bir ifade ağacı türüne dönüştürülen anonim bir işlev içinde `M` başvurmak için derleme zamanı hatası ([anonim işlev dönüştürmelerinden ifade ağacı türlerine yönelik değerlendirme](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-387">It is a compile-time error to refer to `M` inside an anonymous function that is converted to an expression tree type ([Evaluation of anonymous function conversions to expression tree types](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span></span>
* <span data-ttu-id="4c8df-388">`M` çağrısının bir parçası olarak gerçekleşen ifadeler, derleme zamanı hatalarına neden olabilecek kesin atama durumunu ([kesin atama](variables.md#definite-assignment)) etkilemez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-388">Expressions occurring as part of an invocation of `M` do not affect the definite assignment state ([Definite assignment](variables.md#definite-assignment)), which can potentially lead to compile-time errors.</span></span>
* <span data-ttu-id="4c8df-389">`M` bir uygulama için giriş noktası olamaz ([uygulama başlatma](basic-concepts.md#application-startup)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-389">`M` cannot be the entry point for an application ([Application Startup](basic-concepts.md#application-startup)).</span></span>

<span data-ttu-id="4c8df-390">Kısmi Yöntemler, bir tür bildiriminin bir bölümünün bir araç tarafından oluşturulan başka bir parçanın davranışını özelleştirmesine izin vermek için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-390">Partial methods are useful for allowing one part of a type declaration to customize the behavior of another part, e.g., one that is generated by a tool.</span></span> <span data-ttu-id="4c8df-391">Aşağıdaki kısmi sınıf bildirimini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="4c8df-391">Consider the following partial class declaration:</span></span>
```csharp
partial class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    partial void OnNameChanging(string newName);

    partial void OnNameChanged();
}
```

<span data-ttu-id="4c8df-392">Bu sınıf başka herhangi bir bölüm olmadan derlenirse, tanımlayıcı kısmi yöntem bildirimleri ve bunların etkinleştirmeleri kaldırılır ve sonuçta elde edilen birleştirilmiş sınıf bildirimi aşağıdaki gibi olacaktır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-392">If this class is compiled without any other parts, the defining partial method declarations and their invocations will be removed, and the resulting combined class declaration will be equivalent to the following:</span></span>
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set { name = value; }
    }
}
```

<span data-ttu-id="4c8df-393">Ancak kısmi yöntemlerin uygulama bildirimlerini sağlayan başka bir bölüme verildiğini varsayalım:</span><span class="sxs-lookup"><span data-stu-id="4c8df-393">Assume that another part is given, however, which provides implementing declarations of the partial methods:</span></span>
```csharp
partial class Customer
{
    partial void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    partial void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

<span data-ttu-id="4c8df-394">Sonuç olarak elde edilen birleştirilmiş sınıf bildirimi, aşağıdaki gibi olacaktır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-394">Then the resulting combined class declaration will be equivalent to the following:</span></span>
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

### <a name="name-binding"></a><span data-ttu-id="4c8df-395">Ad bağlama</span><span class="sxs-lookup"><span data-stu-id="4c8df-395">Name binding</span></span>

<span data-ttu-id="4c8df-396">Genişletilebilir bir türün her bölümü aynı ad alanı içinde bildirilmelidir, ancak parçalar genellikle farklı ad alanı bildirimleri içinde yazılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-396">Although each part of an extensible type must be declared within the same namespace, the parts are typically written within different namespace declarations.</span></span> <span data-ttu-id="4c8df-397">Bu nedenle, her bölüm için farklı `using` yönergeleri ([yönergeler kullanılarak](namespaces.md#using-directives)) bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-397">Thus, different `using` directives ([Using directives](namespaces.md#using-directives)) may be present for each part.</span></span> <span data-ttu-id="4c8df-398">Tek bir bölüm içindeki basit adları ([tür çıkarımı](expressions.md#type-inference)) yorumlarken, yalnızca o parçayı kapsayan ad alanı bildiriminin `using` yönergeleri göz önünde bulundurululur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-398">When interpreting simple names ([Type inference](expressions.md#type-inference)) within one part, only the `using` directives of the namespace declaration(s) enclosing that part are considered.</span></span> <span data-ttu-id="4c8df-399">Bu, farklı parçalardan farklı anlamlara sahip olan aynı tanımlayıcıya neden olabilir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-399">This may result in the same identifier having different meanings in different parts:</span></span>
```csharp
namespace N
{
    using List = System.Collections.ArrayList;

    partial class A
    {
        List x;                // x has type System.Collections.ArrayList
    }
}

namespace N
{
    using List = Widgets.LinkedList;

    partial class A
    {
        List y;                // y has type Widgets.LinkedList
    }
}
```

## <a name="class-members"></a><span data-ttu-id="4c8df-400">Sınıf üyeleri</span><span class="sxs-lookup"><span data-stu-id="4c8df-400">Class members</span></span>

<span data-ttu-id="4c8df-401">Bir sınıfın üyeleri, *class_member_declaration*s tarafından tanıtılan üyelerden ve doğrudan temel sınıftan devralınan üyelere oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-401">The members of a class consist of the members introduced by its *class_member_declaration*s and the members inherited from the direct base class.</span></span>

```antlr
class_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | destructor_declaration
    | static_constructor_declaration
    | type_declaration
    ;
```

<span data-ttu-id="4c8df-402">Bir sınıf türünün üyeleri aşağıdaki kategorilere ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-402">The members of a class type are divided into the following categories:</span></span>

*  <span data-ttu-id="4c8df-403">Sınıfla ilişkili sabit değerleri temsil eden sabitler ([sabitler](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-403">Constants, which represent constant values associated with the class ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="4c8df-404">Sınıfının değişkenleri olan alanlar ([alanlar](classes.md#fields)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-404">Fields, which are the variables of the class ([Fields](classes.md#fields)).</span></span>
*  <span data-ttu-id="4c8df-405">Sınıfı tarafından gerçekleştirilebilecek hesaplamalar ve eylemleri uygulayan Yöntemler ([Yöntemler](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-405">Methods, which implement the computations and actions that can be performed by the class ([Methods](classes.md#methods)).</span></span>
*  <span data-ttu-id="4c8df-406">Adlandırılmış özellikleri ve bu özellikleri okuma ve yazma ile ilişkili eylemleri tanımlayan Özellikler ([Özellikler](classes.md#properties)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-406">Properties, which define named characteristics and the actions associated with reading and writing those characteristics ([Properties](classes.md#properties)).</span></span>
*  <span data-ttu-id="4c8df-407">Sınıfı tarafından oluşturulabilecek bildirimleri tanımlayan Olaylar ([Olaylar](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-407">Events, which define notifications that can be generated by the class ([Events](classes.md#events)).</span></span>
*  <span data-ttu-id="4c8df-408">Dizin oluşturucular, sınıf örneklerinin (sözdizimsel) diziler ([Dizin oluşturucular](classes.md#indexers)) ile aynı şekilde dizine eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-408">Indexers, which permit instances of the class to be indexed in the same way (syntactically) as arrays ([Indexers](classes.md#indexers)).</span></span>
*  <span data-ttu-id="4c8df-409">İşleç, sınıfının örneklerine uygulanabilen ifade işleçlerini tanımlar ([işleçler](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-409">Operators, which define the expression operators that can be applied to instances of the class ([Operators](classes.md#operators)).</span></span>
*  <span data-ttu-id="4c8df-410">Sınıf örneklerinin başlatılması için gereken eylemleri uygulayan örnek oluşturucular ([örnek oluşturucular](classes.md#instance-constructors))</span><span class="sxs-lookup"><span data-stu-id="4c8df-410">Instance constructors, which implement the actions required to initialize instances of the class ([Instance constructors](classes.md#instance-constructors))</span></span>
*  <span data-ttu-id="4c8df-411">Sınıf örneklerinin kalıcı olarak atılmadan önce gerçekleştirilecek eylemleri uygulayan yok ediciler ([Yıkıcılar](classes.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-411">Destructors, which implement the actions to be performed before instances of the class are permanently discarded ([Destructors](classes.md#destructors)).</span></span>
*  <span data-ttu-id="4c8df-412">Sınıfının kendisini ([statik oluşturucular](classes.md#static-constructors)) başlatmak için gereken eylemleri uygulayan statik oluşturucular.</span><span class="sxs-lookup"><span data-stu-id="4c8df-412">Static constructors, which implement the actions required to initialize the class itself ([Static constructors](classes.md#static-constructors)).</span></span>
*  <span data-ttu-id="4c8df-413">, Sınıfının yerel olan türlerini temsil eden türler ([Iç içe türler](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-413">Types, which represent the types that are local to the class ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="4c8df-414">Yürütülebilir kod içerebilen Üyeler topluca sınıf türünün *işlev üyeleri* olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-414">Members that can contain executable code are collectively known as the *function members* of the class type.</span></span> <span data-ttu-id="4c8df-415">Bir sınıf türünün işlev üyeleri, bu sınıf türünün Yöntemler, özellikler, olaylar, Dizin oluşturucular, işleçler, örnek oluşturucular, Yıkıcılar ve statik oluşturuculardır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-415">The function members of a class type are the methods, properties, events, indexers, operators, instance constructors,  destructors, and static constructors of that class type.</span></span>

<span data-ttu-id="4c8df-416">*Class_declaration* yeni bir bildirim alanı ([Bildirimler](basic-concepts.md#declarations)) oluşturur ve hemen *class_declaration* tarafından bulunan *class_member_declaration*, bu bildirim alanına yeni üyeler getirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-416">A *class_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *class_member_declaration*s immediately contained by the *class_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="4c8df-417">*Class_member_declaration*s için aşağıdaki kurallar geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-417">The following rules apply to *class_member_declaration*s:</span></span>

*  <span data-ttu-id="4c8df-418">Örnek oluşturucular, Yıkıcılar ve statik oluşturucular, hemen kapsayan sınıf ile aynı ada sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-418">Instance constructors, destructors and static constructors must have the same name as the immediately enclosing class.</span></span> <span data-ttu-id="4c8df-419">Tüm diğer üyelerin, hemen kapsayan sınıfın adından farklı adlara sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-419">All other members must have names that differ from the name of the immediately enclosing class.</span></span>
*  <span data-ttu-id="4c8df-420">Bir sabit, alan, özellik, olay veya tür adı aynı sınıfta belirtilen diğer tüm üyelerin adlarından farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-420">The name of a constant, field, property, event, or type must differ from the names of all other members declared in the same class.</span></span>
*  <span data-ttu-id="4c8df-421">Bir yöntemin adı aynı sınıfta belirtilen diğer tüm yöntemlerin adlarından farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-421">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="4c8df-422">Buna ek olarak, bir yöntemin imzası ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)) aynı sınıfta belirtilen diğer tüm yöntemlerin imzalarından farklı olmalıdır ve aynı sınıfta belirtilen iki yöntem yalnızca `ref` ve `out`farklı imzalara sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-422">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="4c8df-423">Örnek oluşturucunun imzası, aynı sınıfta belirtilen diğer tüm örnek oluşturucuların imzalarından farklı olmalıdır ve aynı sınıfta belirtilen iki oluşturucunun yalnızca `ref` ve `out`farklı imzaları bulunmayabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-423">The signature of an instance constructor must differ from the signatures of all other instance constructors declared in the same class, and two constructors declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="4c8df-424">Bir dizin oluşturucunun imzası aynı sınıfta belirtilen diğer tüm dizin oluşturucularının imzalarından farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-424">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>
*  <span data-ttu-id="4c8df-425">Bir işlecin imzası aynı sınıfta belirtilen diğer tüm işleçlerin imzalarından farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-425">The signature of an operator must differ from the signatures of all other operators declared in the same class.</span></span>

<span data-ttu-id="4c8df-426">Bir sınıf türünün devralınan üyeleri ([Devralma](classes.md#inheritance)) bir sınıfın bildirim alanının parçası değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-426">The inherited members of a class type ([Inheritance](classes.md#inheritance)) are not part of the declaration space of a class.</span></span> <span data-ttu-id="4c8df-427">Bu nedenle, türetilmiş bir sınıfın devralınan üye ile aynı ad veya imzaya sahip bir üyeyi bildirmesine izin verilir (yani, devralınan üyeyi etkiler).</span><span class="sxs-lookup"><span data-stu-id="4c8df-427">Thus, a derived class is allowed to declare a member with the same name or signature as an inherited member (which in effect hides the inherited member).</span></span>

### <a name="the-instance-type"></a><span data-ttu-id="4c8df-428">Örnek türü</span><span class="sxs-lookup"><span data-stu-id="4c8df-428">The instance type</span></span>

<span data-ttu-id="4c8df-429">Her sınıf bildiriminde ilişkili bir bağlı tür ([bağlı ve ilişkisiz türler](types.md#bound-and-unbound-types)), ***örnek türü***vardır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-429">Each class declaration has an associated bound type ([Bound and unbound types](types.md#bound-and-unbound-types)), the ***instance type***.</span></span> <span data-ttu-id="4c8df-430">Genel sınıf bildiriminde, örnek türü, her bir sağlanan tür bağımsız değişkeni karşılık gelen tür parametresi olan tür bildiriminden oluşturulmuş bir tür ([oluşturulmuş türler](types.md#constructed-types)) oluşturularak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-430">For a generic class declaration, the instance type is formed by creating a constructed type ([Constructed types](types.md#constructed-types)) from the type declaration, with each of the supplied type arguments being the corresponding type parameter.</span></span> <span data-ttu-id="4c8df-431">Örnek türü tür parametrelerini kullandığından, yalnızca tür parametrelerinin kapsamda olduğu durumlarda kullanılabilir; diğer bir deyişle, sınıf bildiriminin içinde.</span><span class="sxs-lookup"><span data-stu-id="4c8df-431">Since the instance type uses the type parameters, it can only be used where the type parameters are in scope; that is, inside the class declaration.</span></span> <span data-ttu-id="4c8df-432">Örnek türü, sınıf bildiriminin içinde yazılmış kod için `this` türüdür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-432">The instance type is the type of `this` for code written inside the class declaration.</span></span> <span data-ttu-id="4c8df-433">Genel olmayan sınıflar için örnek türü yalnızca belirtilen sınıftır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-433">For non-generic classes, the instance type is simply the declared class.</span></span> <span data-ttu-id="4c8df-434">Aşağıda, örnek türleriyle birlikte çeşitli sınıf bildirimleri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-434">The following shows several class declarations along with their instance types:</span></span> 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a><span data-ttu-id="4c8df-435">Oluşturulmuş türlerin üyeleri</span><span class="sxs-lookup"><span data-stu-id="4c8df-435">Members of constructed types</span></span>

<span data-ttu-id="4c8df-436">Oluşturulmuş bir türün devralınmamış üyeleri, üye bildirimindeki her bir *type_parameter* , oluşturulan türün karşılık gelen *type_argument* yerine koyarak alınır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-436">The non-inherited members of a constructed type are obtained by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="4c8df-437">Değiştirme işlemi, tür bildirimlerinin anlam anlamını temel alır ve yalnızca metinsel değiştirme değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-437">The substitution process is based on the semantic meaning of type declarations, and is not simply textual substitution.</span></span>

<span data-ttu-id="4c8df-438">Örneğin, genel sınıf bildirimi verildiğinde</span><span class="sxs-lookup"><span data-stu-id="4c8df-438">For example, given the generic class declaration</span></span>
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
<span data-ttu-id="4c8df-439">oluşturulan tür `Gen<int[],IComparable<string>>` aşağıdaki üyelere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-439">the constructed type `Gen<int[],IComparable<string>>` has the following members:</span></span>
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

<span data-ttu-id="4c8df-440">Genel sınıf bildirimindeki üye `a` türü, "iki boyutlu `T`dizisi `Gen`, bu nedenle yukarıdaki oluşturulan türdeki üye `a` türü" tek boyutlu bir dizi `int`"veya `int[,][]`olan iki boyutlu dizi dizisi olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-440">The type of the member `a` in the generic class declaration `Gen` is "two-dimensional array of `T`", so the type of the member `a` in the constructed type above is "two-dimensional array of one-dimensional array of `int`", or `int[,][]`.</span></span>

<span data-ttu-id="4c8df-441">Örnek işlevi üyeleri içinde `this` türü, kapsayan bildirimin örnek türüdür ([örnek türü](classes.md#the-instance-type)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-441">Within instance function members, the type of `this` is the instance type ([The instance type](classes.md#the-instance-type)) of the containing declaration.</span></span>

<span data-ttu-id="4c8df-442">Bir genel sınıfın tüm üyeleri herhangi bir kapsayan sınıftan tür parametrelerini doğrudan veya oluşturulmuş bir türün bir parçası olarak kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-442">All members of a generic class can use type parameters from any enclosing class, either directly or as part of a constructed type.</span></span> <span data-ttu-id="4c8df-443">Çalışma zamanında belirli bir kapalı oluşturulmuş tür ([açık ve kapalı türler](types.md#open-and-closed-types)) kullanıldığında, bir tür parametresinin her kullanımı, oluşturulan türe sağlanan gerçek tür bağımsız değişkeniyle değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-443">When a particular closed constructed type ([Open and closed types](types.md#open-and-closed-types)) is used at run-time, each use of a type parameter is replaced with the actual type argument supplied to the constructed type.</span></span> <span data-ttu-id="4c8df-444">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4c8df-444">For example:</span></span>
```csharp
class C<V>
{
    public V f1;
    public C<V> f2 = null;

    public C(V x) {
        this.f1 = x;
        this.f2 = this;
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>(1);
        Console.WriteLine(x1.f1);        // Prints 1

        C<double> x2 = new C<double>(3.1415);
        Console.WriteLine(x2.f1);        // Prints 3.1415
    }
}
```

### <a name="inheritance"></a><span data-ttu-id="4c8df-445">Devralma</span><span class="sxs-lookup"><span data-stu-id="4c8df-445">Inheritance</span></span>

<span data-ttu-id="4c8df-446">Bir sınıf, kendi doğrudan temel sınıf türünün üyelerini ***devralır*** .</span><span class="sxs-lookup"><span data-stu-id="4c8df-446">A class ***inherits*** the members of its direct base class type.</span></span> <span data-ttu-id="4c8df-447">Devralma, bir sınıfın, temel sınıfın örnek oluşturucular, Yıkıcılar ve statik oluşturucular dışında dolaylı olarak kendi temel sınıf türünün tüm üyelerini içerdiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-447">Inheritance means that a class implicitly contains all members of its direct base class type, except for the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="4c8df-448">Devralmanın bazı önemli yönleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-448">Some important aspects of inheritance are:</span></span>

*  <span data-ttu-id="4c8df-449">Devralma geçişlidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-449">Inheritance is transitive.</span></span> <span data-ttu-id="4c8df-450">`C` `B`türediyse ve `B` `A`türetildiyse, `C` `B` belirtilen üyelerin yanı sıra `A`olarak belirtilen üyeleri devralır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-450">If `C` is derived from `B`, and `B` is derived from `A`, then `C` inherits the members declared in `B` as well as the members declared in `A`.</span></span>
*  <span data-ttu-id="4c8df-451">Türetilmiş bir sınıf doğrudan temel sınıfını genişletir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-451">A derived class extends its direct base class.</span></span> <span data-ttu-id="4c8df-452">Türetilmiş bir sınıf, devralananlara yeni üyeler ekleyebilir, ancak devralınmış bir üyenin tanımını kaldıramaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-452">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span>
*  <span data-ttu-id="4c8df-453">Örnek oluşturucular, Yıkıcılar ve statik oluşturucular devralınmaz, ancak diğer tüm Üyeler, belirtilen erişilebilirliği ([üye erişimi](basic-concepts.md#member-access)) ne olursa olsun.</span><span class="sxs-lookup"><span data-stu-id="4c8df-453">Instance constructors, destructors, and static constructors are not inherited, but all other members are, regardless of their declared accessibility ([Member access](basic-concepts.md#member-access)).</span></span> <span data-ttu-id="4c8df-454">Ancak, kendilerine ait olan erişilebilirliğe bağlı olarak, devralınan Üyeler türetilmiş bir sınıfta erişilebilir olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-454">However, depending on their declared accessibility, inherited members might not be accessible in a derived class.</span></span>
*  <span data-ttu-id="4c8df-455">Türetilmiş bir sınıf, aynı ad veya imzaya sahip yeni üyeler bildirerek devralınan üyeleri ([Devralma yoluyla gizleme](basic-concepts.md#hiding-through-inheritance)) de ***gizleyebilir*** .</span><span class="sxs-lookup"><span data-stu-id="4c8df-455">A derived class can ***hide*** ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)) inherited members by declaring new members with the same name or signature.</span></span> <span data-ttu-id="4c8df-456">Ancak devralınan bir üyeyi gizlemenin o üyeyi kaldırmadığını unutmayın; yalnızca bu üyeyi doğrudan türetilmiş sınıf üzerinden erişilmez hale getirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-456">Note however that hiding an inherited member does not remove that member—it merely makes that member inaccessible directly through the derived class.</span></span>
*  <span data-ttu-id="4c8df-457">Bir sınıf örneği, sınıfta ve temel sınıflarında belirtilen tüm örnek alanlarının bir kümesini içerir ve bir türetilmiş sınıf türünden herhangi bir temel sınıf türünden herhangi birine bir örtük dönüştürme ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions)) bulunur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-457">An instance of a class contains a set of all instance fields declared in the class and its base classes, and an implicit conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from a derived class type to any of its base class types.</span></span> <span data-ttu-id="4c8df-458">Bu nedenle, bir türetilmiş sınıfın örneğine bir başvuru, temel sınıflarının herhangi birinin bir örneğine başvuru olarak kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-458">Thus, a reference to an instance of some derived class can be treated as a reference to an instance of any of its base classes.</span></span>
*  <span data-ttu-id="4c8df-459">Bir sınıf sanal yöntemleri, özellikleri ve Dizin oluşturucuyu bildirebilir ve türetilmiş sınıflar bu işlev üyelerinin uygulamasını geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-459">A class can declare virtual methods, properties, and indexers, and derived classes can override the implementation of these function members.</span></span> <span data-ttu-id="4c8df-460">Bu, sınıfların bir işlev üyesi çağrısı tarafından gerçekleştirilen eylemlerde çok biçimli davranışlar sergilemesine olanak sağlar. Bu işlev üyesinin çağrıldığı örnek çalışma zamanı türüne bağlı olarak değişir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-460">This enables classes to exhibit polymorphic behavior wherein the actions performed by a function member invocation varies depending on the run-time type of the instance through which that function member is invoked.</span></span>

<span data-ttu-id="4c8df-461">Oluşturulmuş bir sınıf türünün devralınan üyesi, *class_base* belirtiminde karşılık gelen tür parametrelerinin her bir oluşumu için oluşturulan türün tür bağımsız değişkenlerini değiştirerek bulunan en hızlı temel sınıf türünün ([temel sınıflar](classes.md#base-classes)) üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-461">The inherited member of a constructed class type are the members of the immediate base class type ([Base classes](classes.md#base-classes)), which is found by substituting the type arguments of the constructed type for each occurrence of the corresponding type parameters in the *class_base* specification.</span></span> <span data-ttu-id="4c8df-462">Bu Üyeler sırasıyla, üye bildirimindeki her bir *type_parameter* için, *class_base* belirtiminin karşılık gelen *type_argument* yerine koyarak dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-462">These members, in turn, are transformed by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the *class_base* specification.</span></span>

```csharp
class B<U>
{
    public U F(long index) {...}
}

class D<T>: B<T[]>
{
    public T G(string s) {...}
}
```

<span data-ttu-id="4c8df-463">Yukarıdaki örnekte, oluşturulan tür `D<int>`, tür parametresi `int` tür bağımsız değişkeni yerine `public int G(string s)` devralınmamış bir üyeye sahiptir `T`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-463">In the above example, the constructed type `D<int>` has a non-inherited member `public int G(string s)` obtained by substituting the type argument `int` for the type parameter `T`.</span></span> <span data-ttu-id="4c8df-464">`D<int>` Ayrıca, sınıf bildirimi `B`devralınmış bir üyeye sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-464">`D<int>` also has an inherited member from the class declaration `B`.</span></span> <span data-ttu-id="4c8df-465">Bu devralınan üye, öncelikle temel sınıf belirtimi `B<T[]>``T` için `int` yerine `D<int>` temel sınıf türü `B<int[]>` belirlenerek belirlenir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-465">This inherited member is determined by first determining the base class type `B<int[]>` of `D<int>` by substituting `int` for `T` in the base class specification `B<T[]>`.</span></span> <span data-ttu-id="4c8df-466">`B`için bir tür bağımsız değişkeni olarak, `int[]`, devralınan üye `public int[] F(long index)`oluşturan `public U F(long index)``U` için değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-466">Then, as a type argument to `B`, `int[]` is substituted for `U` in `public U F(long index)`, yielding the inherited member `public int[] F(long index)`.</span></span>

### <a name="the-new-modifier"></a><span data-ttu-id="4c8df-467">Yeni değiştirici</span><span class="sxs-lookup"><span data-stu-id="4c8df-467">The new modifier</span></span>

<span data-ttu-id="4c8df-468">Bir *class_member_declaration* devralınan üyeyle aynı ada veya imzaya sahip bir üyeyi bildirmesine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-468">A *class_member_declaration* is permitted to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="4c8df-469">Bu gerçekleştiğinde, türetilmiş sınıf üyesi temel sınıf üyesini ***gizleyecek*** şekilde söylenir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-469">When this occurs, the derived class member is said to ***hide*** the base class member.</span></span> <span data-ttu-id="4c8df-470">Devralınan bir üyenin gizlenmesi bir hata sayılmaz, ancak derleyicinin bir uyarı vermesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-470">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="4c8df-471">Uyarıyı bastırmak için, türetilen sınıf üyesinin bildirimi, türetilen üyenin temel üyeyi gizleyecek şekilde olduğunu göstermek için `new` değiştiricisini içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-471">To suppress the warning, the declaration of the derived class member can include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="4c8df-472">Bu konu [Devralma yoluyla gizlenerek](basic-concepts.md#hiding-through-inheritance)daha ayrıntılı bir şekilde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-472">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="4c8df-473">Bir `new` değiştiricisi devralınan bir üyeyi gizlemez bir bildirime dahil edilir, bu etkiye bir uyarı verilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-473">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning to that effect is issued.</span></span> <span data-ttu-id="4c8df-474">Bu uyarı `new` değiştiricisi kaldırılarak bastırılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-474">This warning is suppressed by removing the `new` modifier.</span></span>

### <a name="access-modifiers"></a><span data-ttu-id="4c8df-475">Erişim değiştiricileri</span><span class="sxs-lookup"><span data-stu-id="4c8df-475">Access modifiers</span></span>

<span data-ttu-id="4c8df-476">*Class_member_declaration* , beş olası tanımlanmış erişilebilirliği ([belirtilen erişilebilirlik](basic-concepts.md#declared-accessibility)) herhangi birine sahip olabilir: `public`, `protected internal`, `protected`, `internal`veya `private`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-476">A *class_member_declaration* can have any one of the five possible kinds of declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`, or `private`.</span></span> <span data-ttu-id="4c8df-477">`protected internal` birleşimi haricinde, birden fazla erişim değiştiricisi belirtmek için derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-477">Except for the `protected internal` combination, it is a compile-time error to specify more than one access modifier.</span></span> <span data-ttu-id="4c8df-478">Bir *class_member_declaration* erişim değiştiricileri içermiyorsa, `private` varsayılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-478">When a *class_member_declaration* does not include any access modifiers, `private` is assumed.</span></span>

### <a name="constituent-types"></a><span data-ttu-id="4c8df-479">Anayent türleri</span><span class="sxs-lookup"><span data-stu-id="4c8df-479">Constituent types</span></span>

<span data-ttu-id="4c8df-480">Bir üyenin bildiriminde kullanılan türler, o üyenin bileşen türleri olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-480">Types that are used in the declaration of a member are called the constituent types of that member.</span></span> <span data-ttu-id="4c8df-481">Olası yapısal türler, bir sabit, alan, özellik, olay veya dizin oluşturucunun türü, bir yöntemin veya işlecin dönüş türü ve bir yöntem, Dizin Oluşturucu, işleç veya örnek oluşturucusunun parametre türleri olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-481">Possible constituent types are the type of a constant, field, property, event, or indexer, the return type of a method or operator, and the parameter types of a method, indexer, operator, or instance constructor.</span></span> <span data-ttu-id="4c8df-482">Üyenin bileşen türleri en az o üyenin kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-482">The constituent types of a member must be at least as accessible as that member itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

### <a name="static-and-instance-members"></a><span data-ttu-id="4c8df-483">Statik ve örnek üyeleri</span><span class="sxs-lookup"><span data-stu-id="4c8df-483">Static and instance members</span></span>

<span data-ttu-id="4c8df-484">Bir sınıfın üyeleri ***statik Üyeler*** veya ***örnek üyeleridir***.</span><span class="sxs-lookup"><span data-stu-id="4c8df-484">Members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="4c8df-485">Genel olarak, statik üyeleri nesnelere ait olan sınıf türlerine ve örnek üyelerine (sınıf türü örnekleri) ait olacak şekilde düşünmek yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-485">Generally speaking, it is useful to think of static members as belonging to class types and instance members as belonging to objects (instances of class types).</span></span>

<span data-ttu-id="4c8df-486">Bir alan, yöntem, özellik, olay, işleç veya Oluşturucu bildirimi `static` değiştiricisi içerdiğinde, statik bir üye bildirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-486">When a field, method, property, event, operator, or constructor declaration includes a `static` modifier, it declares a static member.</span></span> <span data-ttu-id="4c8df-487">Ayrıca, bir sabit veya tür bildirimi dolaylı olarak statik bir üye bildirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-487">In addition, a constant or type declaration implicitly declares a static member.</span></span> <span data-ttu-id="4c8df-488">Statik Üyeler aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-488">Static members have the following characteristics:</span></span>

*  <span data-ttu-id="4c8df-489">Statik bir üyeye `M`, form `E.M`*member_access* ([üye erişimi](expressions.md#member-access)) içinde başvuruluyorsa `E`, `M`içeren bir tür belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-489">When a static member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote a type containing `M`.</span></span> <span data-ttu-id="4c8df-490">Bir örneği göstermek için `E` derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-490">It is a compile-time error for `E` to denote an instance.</span></span>
*  <span data-ttu-id="4c8df-491">Statik bir alan, belirli bir kapalı sınıf türünün tüm örnekleri tarafından paylaşılacak tam olarak bir depolama konumunu tanımlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-491">A static field identifies exactly one storage location to be shared by all instances of a given closed class type.</span></span> <span data-ttu-id="4c8df-492">Belirli bir kapalı sınıf türünün kaç örneğinin oluşturulduğuna bakılmaksızın, bir statik alanın yalnızca bir kopyası vardır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-492">No matter how many instances of a given closed class type are created, there is only ever one copy of a static field.</span></span>
*  <span data-ttu-id="4c8df-493">Statik işlev üyesi (yöntem, özellik, olay, işleç veya Oluşturucu) belirli bir örnek üzerinde çalışmaz ve bu tür bir işlev üyesinde `this` başvuruda bulunmak için derleme zamanı hatası vardır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-493">A static function member (method, property, event, operator, or constructor) does not operate on a specific instance, and it is a compile-time error to refer to `this` in such a function member.</span></span>

<span data-ttu-id="4c8df-494">Bir alan, yöntem, özellik, olay, Dizin Oluşturucu, Oluşturucu veya yıkıcı bildirimi `static` değiştiricisi içermiyorsa, bir örnek üyesi bildirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-494">When a field, method, property, event, indexer, constructor, or destructor declaration does not include a `static` modifier, it declares an instance member.</span></span> <span data-ttu-id="4c8df-495">(Bir örnek üyesi bazen statik olmayan bir üye olarak adlandırılır.) Örnek üyeleri aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-495">(An instance member is sometimes called a non-static member.) Instance members have the following characteristics:</span></span>

*  <span data-ttu-id="4c8df-496">Bir örnek üyesine `M`, form `E.M`*member_access* ([üye erişimi](expressions.md#member-access)) içinde başvuruluyorsa `E`, `M`içeren bir türün örneğini belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-496">When an instance member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote an instance of a type containing `M`.</span></span> <span data-ttu-id="4c8df-497">`E` bir türü belirtmek için bağlama zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-497">It is a binding-time error for `E` to denote a type.</span></span>
*  <span data-ttu-id="4c8df-498">Bir sınıfın her örneği, sınıfının tüm örnek alanlarını ayrı bir kümesini içerir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-498">Every instance of a class contains a separate set of all instance fields of the class.</span></span>
*  <span data-ttu-id="4c8df-499">Örnek işlev üyesi (yöntem, özellik, Dizin Oluşturucu, örnek Oluşturucu veya yıkıcı), sınıfın belirli bir örneği üzerinde çalışır ve bu örneğe `this` ([Bu erişim](expressions.md#this-access)) olarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-499">An instance function member (method, property, indexer, instance constructor, or destructor) operates on a given instance of the class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="4c8df-500">Aşağıdaki örnekte statik ve örnek üyelerine erişim kuralları gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-500">The following example illustrates the rules for accessing static and instance members:</span></span>
```csharp
class Test
{
    int x;
    static int y;

    void F() {
        x = 1;            // Ok, same as this.x = 1
        y = 1;            // Ok, same as Test.y = 1
    }

    static void G() {
        x = 1;            // Error, cannot access this.x
        y = 1;            // Ok, same as Test.y = 1
    }

    static void Main() {
        Test t = new Test();
        t.x = 1;          // Ok
        t.y = 1;          // Error, cannot access static member through instance
        Test.x = 1;       // Error, cannot access instance member through type
        Test.y = 1;       // Ok
    }
}
```

<span data-ttu-id="4c8df-501">`F` yöntemi bir örnek işlev üyesinde, hem örnek üyelerine hem de statik üyelere erişmek için bir *simple_name* ([basit adlar](expressions.md#simple-names)) kullanılabileceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-501">The `F` method shows that in an instance function member, a *simple_name* ([Simple names](expressions.md#simple-names)) can be used to access both instance members and static members.</span></span> <span data-ttu-id="4c8df-502">`G` yöntemi, bir statik işlev üyesinde, bir *simple_name*bir örnek üyesine erişmek için derleme zamanı hatası olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-502">The `G` method shows that in a static function member, it is a compile-time error to access an instance member through a *simple_name*.</span></span> <span data-ttu-id="4c8df-503">`Main` yöntemi, bir *member_access* ([üye erişimi](expressions.md#member-access)), örnek üyelerine örnekler aracılığıyla erişilmesi ve statik üyelere türler aracılığıyla erişilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-503">The `Main` method shows that in a *member_access* ([Member access](expressions.md#member-access)), instance members must be accessed through instances, and static members must be accessed through types.</span></span>

### <a name="nested-types"></a><span data-ttu-id="4c8df-504">İç içe türler</span><span class="sxs-lookup"><span data-stu-id="4c8df-504">Nested types</span></span>

<span data-ttu-id="4c8df-505">Bir sınıf veya yapı bildiriminde belirtilen türe ***iç içe geçmiş tür***denir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-505">A type declared within a class or struct declaration is called a ***nested type***.</span></span> <span data-ttu-id="4c8df-506">Bir derleme birimi veya ad alanı içinde belirtilen bir tür, ***iç içe olmayan bir tür***olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-506">A type that is declared within a compilation unit or namespace is called a ***non-nested type***.</span></span>

<span data-ttu-id="4c8df-507">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-507">In the example</span></span>
```csharp
using System;

class A
{
    class B
    {
        static void F() {
            Console.WriteLine("A.B.F");
        }
    }
}
```
<span data-ttu-id="4c8df-508">sınıf `B`, sınıf `A`içinde bildirildiği ve sınıf `A`, bir derleme birimi içinde bildirildiği için iç içe olmayan bir tür olduğundan iç içe bir tür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-508">class `B` is a nested type because it is declared within class `A`, and class `A` is a non-nested type because it is declared within a compilation unit.</span></span>

#### <a name="fully-qualified-name"></a><span data-ttu-id="4c8df-509">Tam ad</span><span class="sxs-lookup"><span data-stu-id="4c8df-509">Fully qualified name</span></span>

<span data-ttu-id="4c8df-510">İç içe bir tür için tam nitelikli ad ([tam adlar](basic-concepts.md#fully-qualified-names)), `S` tür `N` bildirildiği türün tam adı `S.N`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-510">The fully qualified name ([Fully qualified names](basic-concepts.md#fully-qualified-names)) for a nested type is `S.N` where `S` is the fully qualified name of the type in which type `N` is declared.</span></span>

#### <a name="declared-accessibility"></a><span data-ttu-id="4c8df-511">Tanımlanan erişilebilirlik</span><span class="sxs-lookup"><span data-stu-id="4c8df-511">Declared accessibility</span></span>

<span data-ttu-id="4c8df-512">İç içe olmayan türlerde `public` veya `internal` tarafından tanımlanan erişilebilirlik bulunabilir ve varsayılan olarak `internal` olarak tanımlanmış erişilebilirliği vardır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-512">Non-nested types can have `public` or `internal` declared accessibility and have `internal` declared accessibility by default.</span></span> <span data-ttu-id="4c8df-513">İç içe türler, kapsayan türün bir sınıf veya yapı olmasına bağlı olarak, bu tanımlanmış erişilebilirlik ve bir veya daha fazla tanımlanmış erişilebilirlik biçimi içerebilir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-513">Nested types can have these forms of declared accessibility too, plus one or more additional forms of declared accessibility, depending on whether the containing type is a class or struct:</span></span>

*  <span data-ttu-id="4c8df-514">Bir sınıfta bildirildiği iç içe yerleştirilmiş bir tür, belirtilen beş erişilebilirliği (`public`, `protected internal`, `protected`, `internal`veya `private`) ve diğer sınıf üyeleri gibi, varsayılan olarak belirtilen erişilebilirliği `private` olarak ifade edebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-514">A nested type that is declared in a class can have any of five forms of declared accessibility (`public`, `protected internal`, `protected`, `internal`, or `private`) and, like other class members, defaults to `private` declared accessibility.</span></span>
*  <span data-ttu-id="4c8df-515">Bir yapıda tanımlanmış iç içe yerleştirilmiş bir tür, üç farklı yapı erişilebilirliği (`public`, `internal`veya `private`) ve diğer yapı üyeleri gibi, varsayılan olarak tanımlanmış erişilebilirliği `private`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-515">A nested type that is declared in a struct can have any of three forms of declared accessibility (`public`, `internal`, or `private`) and, like other struct members, defaults to `private` declared accessibility.</span></span>

<span data-ttu-id="4c8df-516">Örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-516">The example</span></span>
```csharp
public class List
{
    // Private data structure
    private class Node
    { 
        public object Data;
        public Node Next;

        public Node(object data, Node next) {
            this.Data = data;
            this.Next = next;
        }
    }

    private Node first = null;
    private Node last = null;

    // Public interface
    public void AddToFront(object o) {...}
    public void AddToBack(object o) {...}
    public object RemoveFromFront() {...}
    public object RemoveFromBack() {...}
    public int Count { get {...} }
}
```
<span data-ttu-id="4c8df-517">özel bir iç içe sınıf `Node`bildirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-517">declares a private nested class `Node`.</span></span>

#### <a name="hiding"></a><span data-ttu-id="4c8df-518">Larını</span><span class="sxs-lookup"><span data-stu-id="4c8df-518">Hiding</span></span>

<span data-ttu-id="4c8df-519">İç içe yerleştirilmiş bir tür, temel üyeyi gizleyebilir ([ad gizleyerek](basic-concepts.md#name-hiding)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-519">A nested type may hide ([Name hiding](basic-concepts.md#name-hiding)) a base member.</span></span> <span data-ttu-id="4c8df-520">`new` değiştiricisine, iç içe geçmiş tür bildirimlerinde izin verilir, böylece gizleme açıkça ifade edilebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-520">The `new` modifier is permitted on nested type declarations so that hiding can be expressed explicitly.</span></span> <span data-ttu-id="4c8df-521">Örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-521">The example</span></span>
```csharp
using System;

class Base
{
    public static void M() {
        Console.WriteLine("Base.M");
    }
}

class Derived: Base 
{
    new public class M 
    {
        public static void F() {
            Console.WriteLine("Derived.M.F");
        }
    }
}

class Test 
{
    static void Main() {
        Derived.M.F();
    }
}
```
<span data-ttu-id="4c8df-522">`Base`tanımlanan `M` metodunu gizleyen iç içe bir sınıf `M` gösterir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-522">shows a nested class `M` that hides the method `M` defined in `Base`.</span></span>

#### <a name="this-access"></a><span data-ttu-id="4c8df-523">Bu erişim</span><span class="sxs-lookup"><span data-stu-id="4c8df-523">this access</span></span>

<span data-ttu-id="4c8df-524">İç içe bir tür ve kapsayan türü *This_Access* ([Bu erişim](expressions.md#this-access)) ile ilgili özel bir ilişkiye sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-524">A nested type and its containing type do not have a special relationship with regard to *this_access* ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="4c8df-525">Özellikle, iç içe bir tür içindeki `this`, kapsayan türün örnek üyelerine başvurmak için kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-525">Specifically, `this` within a nested type cannot be used to refer to instance members of the containing type.</span></span> <span data-ttu-id="4c8df-526">İç içe bir türün, kapsayan türün örnek üyelerine erişmesi gereken durumlarda, kapsayan türün örneği için `this` iç içe geçmiş türü için bir Oluşturucu bağımsız değişkeni olarak sağlanarak erişim sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-526">In cases where a nested type needs access to the instance members of its containing type, access can be provided by providing the `this` for the instance of the containing type as a constructor argument for the nested type.</span></span> <span data-ttu-id="4c8df-527">Aşağıdaki örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-527">The following example</span></span>
```csharp
using System;

class C
{
    int i = 123;

    public void F() {
        Nested n = new Nested(this);
        n.G();
    }

    public class Nested
    {
        C this_c;

        public Nested(C c) {
            this_c = c;
        }

        public void G() {
            Console.WriteLine(this_c.i);
        }
    }
}

class Test
{
    static void Main() {
        C c = new C();
        c.F();
    }
}
```
<span data-ttu-id="4c8df-528">Bu tekniği gösterir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-528">shows this technique.</span></span> <span data-ttu-id="4c8df-529">Bir `C` örneği `Nested` örneğini oluşturur ve `C`örnek üyelerine sonraki erişimi sağlamak için kendi `this` `Nested`oluşturucusuna geçirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-529">An instance of `C` creates an instance of `Nested` and passes its own `this` to `Nested`'s constructor in order to provide subsequent access to `C`'s instance members.</span></span>

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a><span data-ttu-id="4c8df-530">Kapsayan türdeki özel ve korunan üyelere erişim</span><span class="sxs-lookup"><span data-stu-id="4c8df-530">Access to private and protected members of the containing type</span></span>

<span data-ttu-id="4c8df-531">İç içe bir tür, `private` sahip olan ve `protected` belirtilen erişilebilirliği içeren kapsayan türdeki Üyeler de dahil olmak üzere, kapsayan tür tarafından erişilebilen tüm üyelere erişebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-531">A nested type has access to all of the members that are accessible to its containing type, including members of the containing type that have `private` and `protected` declared accessibility.</span></span> <span data-ttu-id="4c8df-532">Örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-532">The example</span></span>
```csharp
using System;

class C 
{
    private static void F() {
        Console.WriteLine("C.F");
    }

    public class Nested 
    {
        public static void G() {
            F();
        }
    }
}

class Test 
{
    static void Main() {
        C.Nested.G();
    }
}
```
<span data-ttu-id="4c8df-533">iç içe bir sınıf `Nested`içeren bir sınıf `C` gösterir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-533">shows a class `C` that contains a nested class `Nested`.</span></span> <span data-ttu-id="4c8df-534">`Nested`içinde yöntemi `G`, `C`tanımlanmış statik yöntemi `F` çağırır ve `F` özel olarak tanımlanmış erişilebilirliği vardır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-534">Within `Nested`, the method `G` calls the static method `F` defined in `C`, and `F` has private declared accessibility.</span></span>

<span data-ttu-id="4c8df-535">İç içe yerleştirilmiş bir tür, kapsayan türünün temel türünde tanımlanan korumalı üyelere de erişebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-535">A nested type also may access protected members defined in a base type of its containing type.</span></span> <span data-ttu-id="4c8df-536">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-536">In the example</span></span>
```csharp
using System;

class Base 
{
    protected void F() {
        Console.WriteLine("Base.F");
    }
}

class Derived: Base 
{
    public class Nested 
    {
        public void G() {
            Derived d = new Derived();
            d.F();        // ok
        }
    }
}

class Test 
{
    static void Main() {
        Derived.Nested n = new Derived.Nested();
        n.G();
    }
}
```
<span data-ttu-id="4c8df-537">iç içe yerleştirilmiş sınıf `Derived.Nested`, `Derived`temel sınıfında tanımlanan `F` korunan yönteme erişir, `Base`bir `Derived`örneği aracılığıyla çağırarak.</span><span class="sxs-lookup"><span data-stu-id="4c8df-537">the nested class `Derived.Nested` accesses the protected method `F` defined in `Derived`'s base class, `Base`, by calling through an instance of `Derived`.</span></span>

#### <a name="nested-types-in-generic-classes"></a><span data-ttu-id="4c8df-538">Genel sınıflarda iç içe türler</span><span class="sxs-lookup"><span data-stu-id="4c8df-538">Nested types in generic classes</span></span>

<span data-ttu-id="4c8df-539">Genel sınıf bildirimi, iç içe geçmiş tür bildirimleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-539">A generic class declaration can contain nested type declarations.</span></span> <span data-ttu-id="4c8df-540">Kapsayan sınıfın tür parametreleri iç içe türler içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-540">The type parameters of the enclosing class can be used within the nested types.</span></span> <span data-ttu-id="4c8df-541">İç içe geçmiş tür bildirimi, yalnızca iç içe geçmiş tür için uygulanan ek tür parametreleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-541">A nested type declaration can contain additional type parameters that apply only to the nested type.</span></span>

<span data-ttu-id="4c8df-542">Genel sınıf bildiriminde yer alan her tür bildirimi örtük olarak genel bir tür bildirimidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-542">Every type declaration contained within a generic class declaration is implicitly a generic type declaration.</span></span> <span data-ttu-id="4c8df-543">Genel bir tür içinde iç içe geçmiş bir türe başvuru yazarken, türü bağımsız değişkenleri dahil olmak üzere bulunan oluşturulmuş tür, adlandırılmış olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-543">When writing a reference to a type nested within a generic type, the containing constructed type, including its type arguments, must be named.</span></span> <span data-ttu-id="4c8df-544">Ancak, dış sınıfın içinden iç içe geçmiş tür, nitelendirme olmadan kullanılabilir; Dış sınıfın örnek türü, iç içe geçmiş türü oluşturulurken örtük olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-544">However, from within the outer class, the nested type can be used without qualification; the instance type of the outer class can be implicitly used when constructing the nested type.</span></span> <span data-ttu-id="4c8df-545">Aşağıdaki örnek, `Inner`oluşturulan oluşturulmuş bir türe başvurmak için üç farklı doğru yolu göstermektedir; İlk ikisi eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-545">The following example shows three different correct ways to refer to a constructed type created from `Inner`; the first two are equivalent:</span></span>
```csharp
class Outer<T>
{
    class Inner<U>
    {
        public static void F(T t, U u) {...}
    }

    static void F(T t) {
        Outer<T>.Inner<string>.F(t, "abc");      // These two statements have
        Inner<string>.F(t, "abc");               // the same effect

        Outer<int>.Inner<string>.F(3, "abc");    // This type is different

        Outer.Inner<string>.F(t, "abc");         // Error, Outer needs type arg
    }
}
```

<span data-ttu-id="4c8df-546">Hatalı programlama stili olsa da, iç içe bir türdeki tür parametresi, dış türde belirtilen bir üyeyi veya tür parametresini gizleyebilir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-546">Although it is bad programming style, a type parameter in a nested type can hide a member or type parameter declared in the outer type:</span></span>
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a><span data-ttu-id="4c8df-547">Ayrılmış üye adları</span><span class="sxs-lookup"><span data-stu-id="4c8df-547">Reserved member names</span></span>

<span data-ttu-id="4c8df-548">Temel C# çalışma zamanı uygulamasını kolaylaştırmak için, bir özellik, olay veya Dizin Oluşturucu olan her kaynak üye bildirimi için, uygulama, üye bildirimi türüne, adına ve türüne göre iki yöntem imzasını ayırmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-548">To facilitate the underlying C# run-time implementation, for each source member declaration that is a property, event, or indexer, the implementation must reserve two method signatures based on the kind of the member declaration, its name, and its type.</span></span> <span data-ttu-id="4c8df-549">Bu, temeldeki çalışma zamanı uygulamasının bu ayırmaları kullanmasa bile, imzası bu ayrılmış imzalardan biriyle eşleşen bir üyeyi bildirmek için bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-549">It is a compile-time error for a program to declare a member whose signature matches one of these reserved signatures, even if the underlying run-time implementation does not make use of these reservations.</span></span>

<span data-ttu-id="4c8df-550">Ayrılmış adlar bildirimleri sunmaz, bu nedenle üye aramasına katılmaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-550">The reserved names do not introduce declarations, thus they do not participate in member lookup.</span></span> <span data-ttu-id="4c8df-551">Ancak, bir bildirimin ilişkili ayrılmış yöntem imzaları devralmaya ([Devralma](classes.md#inheritance)) katılır ve `new` değiştiricisi ([Yeni değiştirici](classes.md#the-new-modifier)) ile gizlenebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-551">However, a declaration's associated reserved method signatures do participate in inheritance ([Inheritance](classes.md#inheritance)), and can be hidden with the `new` modifier ([The new modifier](classes.md#the-new-modifier)).</span></span>

<span data-ttu-id="4c8df-552">Bu adların ayırması üç amaca hizmet eder:</span><span class="sxs-lookup"><span data-stu-id="4c8df-552">The reservation of these names serves three purposes:</span></span>

*  <span data-ttu-id="4c8df-553">Temel uygulamanın C# dil özelliğine Get veya set erişimi için bir yöntem adı olarak sıradan bir tanımlayıcı kullanmasına izin vermek için.</span><span class="sxs-lookup"><span data-stu-id="4c8df-553">To allow the underlying implementation to use an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="4c8df-554">Diğer dillerin, C# dil özelliğine Get veya set erişimi için bir yöntem adı olarak sıradan bir tanımlayıcı kullanarak birlikte çalışabilme izni vermek için.</span><span class="sxs-lookup"><span data-stu-id="4c8df-554">To allow other languages to interoperate using an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="4c8df-555">Tek bir uygun derleyici tarafından kabul edilen kaynağın, ayrılmış üye adlarının özelliklerinin tüm C# uygulamalarda tutarlı olduğundan emin olmak için.</span><span class="sxs-lookup"><span data-stu-id="4c8df-555">To help ensure that the source accepted by one conforming compiler is accepted by another, by making the specifics of reserved member names consistent across all C# implementations.</span></span>

<span data-ttu-id="4c8df-556">Yıkıcı ([Yıkıcılar](classes.md#destructors)) bildirimi de bir imzanın ayrılmasına neden olur (yok[ediciler için üye adları ayrılmıştır](classes.md#member-names-reserved-for-destructors)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-556">The declaration of a destructor ([Destructors](classes.md#destructors)) also causes a signature to be reserved ([Member names reserved for destructors](classes.md#member-names-reserved-for-destructors)).</span></span>

#### <a name="member-names-reserved-for-properties"></a><span data-ttu-id="4c8df-557">Özellikler için ayrılan üye adları</span><span class="sxs-lookup"><span data-stu-id="4c8df-557">Member names reserved for properties</span></span>

<span data-ttu-id="4c8df-558">`T`türündeki bir özellik `P` ([Özellikler](classes.md#properties)) için aşağıdaki imzalar ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-558">For a property `P` ([Properties](classes.md#properties)) of type `T`, the following signatures are reserved:</span></span>

```csharp
T get_P();
void set_P(T value);
```

<span data-ttu-id="4c8df-559">Özellik salt okunurdur veya salt yazılır olsa bile her iki imza de ayrılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-559">Both signatures are reserved, even if the property is read-only or write-only.</span></span>

<span data-ttu-id="4c8df-560">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-560">In the example</span></span>
```csharp
using System;

class A
{
    public int P {
        get { return 123; }
    }
}

class B: A
{
    new public int get_P() {
        return 456;
    }

    new public void set_P(int value) {
    }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        Console.WriteLine(a.P);
        Console.WriteLine(b.P);
        Console.WriteLine(b.get_P());
    }
}
```
<span data-ttu-id="4c8df-561">bir sınıf `A` salt okunurdur bir özellik `P`tanımlar, böylece `get_P` ve `set_P` yöntemlerine yönelik imzaları ayırırsınız.</span><span class="sxs-lookup"><span data-stu-id="4c8df-561">a class `A` defines a read-only property `P`, thus reserving signatures for `get_P` and `set_P` methods.</span></span> <span data-ttu-id="4c8df-562">Bir sınıf `B` `A` türetilir ve bu ayrılmış imzaların her ikisini birden gizler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-562">A class `B` derives from `A` and hides both of these reserved signatures.</span></span> <span data-ttu-id="4c8df-563">Örnek, çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-563">The example produces the output:</span></span>
```console
123
123
456
```

#### <a name="member-names-reserved-for-events"></a><span data-ttu-id="4c8df-564">Olaylar için ayrılan üye adları</span><span class="sxs-lookup"><span data-stu-id="4c8df-564">Member names reserved for events</span></span>

<span data-ttu-id="4c8df-565">`T`temsilci türünün olay `E` ([olayları](classes.md#events)) için aşağıdaki imzalar ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-565">For an event `E` ([Events](classes.md#events)) of delegate type `T`, the following signatures are reserved:</span></span>
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a><span data-ttu-id="4c8df-566">Dizin oluşturucular için ayrılan üye adları</span><span class="sxs-lookup"><span data-stu-id="4c8df-566">Member names reserved for indexers</span></span>

<span data-ttu-id="4c8df-567">Parametre listesi `L``T` türündeki bir Dizin Oluşturucu ([Dizin oluşturucular](classes.md#indexers)) için aşağıdaki imzalar ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-567">For an indexer ([Indexers](classes.md#indexers)) of type `T` with parameter-list `L`, the following signatures are reserved:</span></span>
```csharp
T get_Item(L);
void set_Item(L, T value);
```

<span data-ttu-id="4c8df-568">Dizin Oluşturucu salt okunurdur veya salt yazılır olsa bile her iki imza da ayrılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-568">Both signatures are reserved, even if the indexer is read-only or write-only.</span></span>

<span data-ttu-id="4c8df-569">Ayrıca `Item` üye adının ayrılmış olması.</span><span class="sxs-lookup"><span data-stu-id="4c8df-569">Furthermore the member name `Item` is reserved.</span></span>

#### <a name="member-names-reserved-for-destructors"></a><span data-ttu-id="4c8df-570">Yok ediciler için ayrılan üye adları</span><span class="sxs-lookup"><span data-stu-id="4c8df-570">Member names reserved for destructors</span></span>

<span data-ttu-id="4c8df-571">Yıkıcı ([Yıkıcılar](classes.md#destructors)) içeren bir sınıf için aşağıdaki imza ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-571">For a class containing a destructor ([Destructors](classes.md#destructors)), the following signature is reserved:</span></span>
```csharp
void Finalize();
```

## <a name="constants"></a><span data-ttu-id="4c8df-572">Sabitler</span><span class="sxs-lookup"><span data-stu-id="4c8df-572">Constants</span></span>

<span data-ttu-id="4c8df-573">***Sabit*** değeri, derleme zamanında hesaplanılabilen bir değer olan sabit bir değeri temsil eden bir sınıf üyesidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-573">A ***constant*** is a class member that represents a constant value: a value that can be computed at compile-time.</span></span> <span data-ttu-id="4c8df-574">*Constant_declaration* , belirli bir türün bir veya daha fazla sabitlerini tanıtır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-574">A *constant_declaration* introduces one or more constants of a given type.</span></span>

```antlr
constant_declaration
    : attributes? constant_modifier* 'const' type constant_declarators ';'
    ;

constant_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

<span data-ttu-id="4c8df-575">*Constant_declaration* bir *öznitelikler* kümesi ([öznitelikler](attributes.md)), bir `new` değiştiricisi ([Yeni değiştirici](classes.md#the-new-modifier)) ve dört erişim değiştiricisinin geçerli bir birleşimini ([erişim değiştiricileri](classes.md#access-modifiers)) içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-575">A *constant_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span> <span data-ttu-id="4c8df-576">Öznitelikler ve değiştiriciler *constant_declaration*tarafından belirtilen tüm üyelere uygulanır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-576">The attributes and modifiers apply to all of the members declared by the *constant_declaration*.</span></span> <span data-ttu-id="4c8df-577">Sabitler statik üye olarak kabul edilse de, *constant_declaration* gerektirmez ve `static` değiştiriciye izin vermez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-577">Even though constants are considered static members, a *constant_declaration* neither requires nor allows a `static` modifier.</span></span> <span data-ttu-id="4c8df-578">Aynı değiştiricinin Sabit bildiriminde birden çok kez görünmesi hatadır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-578">It is an error for the same modifier to appear multiple times in a constant declaration.</span></span>

<span data-ttu-id="4c8df-579">Constant_declaration *türü* , bildirim tarafından tanıtılan üyelerin türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-579">The *type* of a *constant_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="4c8df-580">Türün ardından her biri yeni bir üye tanıtan *constant_declarator*s listesi bulunur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-580">The type is followed by a list of *constant_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="4c8df-581">*Constant_declarator* , üyeyi belirten bir *tanımlayıcıdan* , ardından bir "`=`" belirteci ve sonra üyenin değerini veren bir *constant_expression* ([sabit ifadeler](expressions.md#constant-expressions)) oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-581">A *constant_declarator* consists of an *identifier* that names the member, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the member.</span></span>

<span data-ttu-id="4c8df-582">Sabit bildiriminde belirtilen *tür* `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, *enum_type*veya *reference_type*olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-582">The *type* specified in a constant declaration must be `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, an *enum_type*, or a *reference_type*.</span></span> <span data-ttu-id="4c8df-583">Her *constant_expression* , hedef türün bir değerini veya örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) tarafından hedef türe dönüştürülebilen bir türü vermelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-583">Each *constant_expression* must yield a value of the target type or of a type that can be converted to the target type by an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>

<span data-ttu-id="4c8df-584">Bir sabit *türünün* en az sabitin ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-584">The *type* of a constant must be at least as accessible as the constant itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="4c8df-585">Bir sabit değeri *simple_name* ([basit adlar](expressions.md#simple-names)) veya bir *member_access* ([üye erişimi](expressions.md#member-access)) kullanılarak bir ifadede elde edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-585">The value of a constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="4c8df-586">Bir sabit, bir *constant_expression*katılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-586">A constant can itself participate in a *constant_expression*.</span></span> <span data-ttu-id="4c8df-587">Bu nedenle, bir sabit, *constant_expression*gerektiren herhangi bir yapı içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-587">Thus, a constant may be used in any construct that requires a *constant_expression*.</span></span> <span data-ttu-id="4c8df-588">Bu tür yapılara örnek olarak `case` Etiketler, `goto case` deyimler, `enum` üye bildirimleri, öznitelikler ve diğer sabit bildirimler dahildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-588">Examples of such constructs include `case` labels, `goto case` statements, `enum` member declarations, attributes, and other constant declarations.</span></span>

<span data-ttu-id="4c8df-589">[Sabit ifadelerde](expressions.md#constant-expressions)açıklandığı gibi, *constant_expression* derleme zamanında tam olarak değerlendirilebilen bir ifadedir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-589">As described in [Constant expressions](expressions.md#constant-expressions), a *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span> <span data-ttu-id="4c8df-590">`string` dışında bir *reference_type* null olmayan bir değer oluşturmanın tek yolu `new` işlecini uygulamak ve `new` işlecine bir *constant_expression*izin verilmesinden bu *yana reference_type dışındaki `string` s sabitleri*için tek olası değer `null`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-590">Since the only way to create a non-null value of a *reference_type* other than `string` is to apply the `new` operator, and since the `new` operator is not permitted in a *constant_expression*, the only possible value for constants of *reference_type*s other than `string` is `null`.</span></span>

<span data-ttu-id="4c8df-591">Sabit bir değer için sembolik bir ad istendiğinde, ancak bu değerin türü sabit bildirimde izin verilmediğinde veya değer derleme zamanında bir *constant_expression*tarafından hesaplanmadığında, bunun yerine bir `readonly` alanı ([salt okunur alan](classes.md#readonly-fields)) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-591">When a symbolic name for a constant value is desired, but when the type of that value is not permitted in a constant declaration, or when the value cannot be computed at compile-time by a *constant_expression*, a `readonly` field ([Readonly fields](classes.md#readonly-fields)) may be used instead.</span></span>

<span data-ttu-id="4c8df-592">Birden çok sabiti bildiren bir sabit bildirim, aynı özniteliklere, Değiştiricilere ve türe sahip tek sabitlerin birden çok bildirimi ile eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-592">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="4c8df-593">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4c8df-593">For example</span></span>
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
<span data-ttu-id="4c8df-594">eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="4c8df-594">is equivalent to</span></span>
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

<span data-ttu-id="4c8df-595">Sabitler dairesel bir şekilde olmadığı sürece sabitlerin aynı program içindeki diğer sabitlere bağlı olmasına izin verilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-595">Constants are permitted to depend on other constants within the same program as long as the dependencies are not of a circular nature.</span></span> <span data-ttu-id="4c8df-596">Derleyici, sabit bildirimleri uygun sırada değerlendirmek için otomatik olarak düzenler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-596">The compiler automatically arranges to evaluate the constant declarations in the appropriate order.</span></span> <span data-ttu-id="4c8df-597">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-597">In the example</span></span>
```csharp
class A
{
    public const int X = B.Z + 1;
    public const int Y = 10;
}

class B
{
    public const int Z = A.Y + 1;
}
```
<span data-ttu-id="4c8df-598">derleyici önce `A.Y`değerlendirir, sonra `B.Z`değerlendirir ve son olarak `A.X`değerlendirir `10`, `11`ve `12`değerlerini üretir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-598">the compiler first evaluates `A.Y`, then evaluates `B.Z`, and finally evaluates `A.X`, producing the values `10`, `11`, and `12`.</span></span> <span data-ttu-id="4c8df-599">Sabit bildirimler diğer programlardaki sabitlere bağlı olabilir, ancak bu tür bağımlılıklar yalnızca tek bir yönde mümkündür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-599">Constant declarations may depend on constants from other programs, but such dependencies are only possible in one direction.</span></span> <span data-ttu-id="4c8df-600">Yukarıdaki örneğe başvurma, `A` ve `B` ayrı programlarda bildirilirse `A.X` `B.Z`bağımlı olması mümkündür ancak `B.Z` aynı anda `A.Y`bağımlı olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-600">Referring to the example above, if `A` and `B` were declared in separate programs, it would be possible for `A.X` to depend on `B.Z`, but `B.Z` could then not simultaneously depend on `A.Y`.</span></span>

## <a name="fields"></a><span data-ttu-id="4c8df-601">Alanlar</span><span class="sxs-lookup"><span data-stu-id="4c8df-601">Fields</span></span>

<span data-ttu-id="4c8df-602">***Alan*** , bir nesne veya sınıfla ilişkili bir değişkeni temsil eden bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-602">A ***field*** is a member that represents a variable associated with an object or class.</span></span> <span data-ttu-id="4c8df-603">*Field_declaration* , belirli bir türün bir veya daha fazla alanını tanıtır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-603">A *field_declaration* introduces one or more fields of a given type.</span></span>

```antlr
field_declaration
    : attributes? field_modifier* type variable_declarators ';'
    ;

field_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'readonly'
    | 'volatile'
    | field_modifier_unsafe
    ;

variable_declarators
    : variable_declarator (',' variable_declarator)*
    ;

variable_declarator
    : identifier ('=' variable_initializer)?
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

<span data-ttu-id="4c8df-604">Bir *field_declaration* bir *öznitelikler* kümesi ([öznitelikler](attributes.md)), bir `new` değiştirici ([Yeni değiştirici](classes.md#the-new-modifier)), dört erişim değiştiricisinin geçerli bir birleşimi ([erişim değiştiricileri](classes.md#access-modifiers)) ve bir `static` değiştiricisi ([statik ve örnek alanları](classes.md#static-and-instance-fields)) içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-604">A *field_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and a `static` modifier ([Static and instance fields](classes.md#static-and-instance-fields)).</span></span> <span data-ttu-id="4c8df-605">Ayrıca, bir *field_declaration* `readonly` değiştirici ([salt okunur alanlar](classes.md#readonly-fields)) veya `volatile` değiştirici ([geçici alanlar](classes.md#volatile-fields)) içerebilir, ancak ikisini birden içeremez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-605">In addition, a *field_declaration* may include a `readonly` modifier ([Readonly fields](classes.md#readonly-fields)) or a `volatile` modifier ([Volatile fields](classes.md#volatile-fields)) but not both.</span></span> <span data-ttu-id="4c8df-606">Öznitelikler ve değiştiriciler *field_declaration*tarafından belirtilen tüm üyelere uygulanır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-606">The attributes and modifiers apply to all of the members declared by the *field_declaration*.</span></span> <span data-ttu-id="4c8df-607">Aynı değiştiricinin bir alan bildiriminde birden çok kez görünmesi hatadır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-607">It is an error for the same modifier to appear multiple times in a field declaration.</span></span>

<span data-ttu-id="4c8df-608">Field_declaration *türü* , bildirim tarafından tanıtılan üyelerin türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-608">The *type* of a *field_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="4c8df-609">Türün ardından her biri yeni bir üye tanıtan *variable_declarator*s listesi bulunur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-609">The type is followed by a list of *variable_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="4c8df-610">*Variable_declarator* , bu üyeyi ve isteğe bağlı olarak bir "`=`" belirteci ve bu üyenin ilk değerini veren bir *variable_initializer* ([değişken başlatıcıları](classes.md#variable-initializers)) içeren bir *tanımlayıcıdan* oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-610">A *variable_declarator* consists of an *identifier* that names that member, optionally followed by an "`=`" token and a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)) that gives the initial value of that member.</span></span>

<span data-ttu-id="4c8df-611">Alanın *türü* en az alanın kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-611">The *type* of a field must be at least as accessible as the field itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="4c8df-612">Bir alanın değeri, *simple_name* ([basit adlar](expressions.md#simple-names)) veya bir *member_access* ([üye erişimi](expressions.md#member-access)) kullanılarak bir ifadede elde edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-612">The value of a field is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="4c8df-613">Salt okunur olmayan bir alanın değeri, *atama* ([atama işleçleri](expressions.md#assignment-operators)) kullanılarak değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-613">The value of a non-readonly field is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="4c8df-614">Salt okunur olmayan bir alanın değeri, sonek artırma ve azaltma işleçleri ([Sonek artışı ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators)) ve önek artırma ve azaltma Işleçleri ([önek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)) kullanılarak elde edilebilir ve değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-614">The value of a non-readonly field can be both obtained and modified using postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)) and prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="4c8df-615">Birden çok alanı bildiren bir alan bildirimi, aynı özniteliklere, Değiştiricilere ve türe sahip tek alanlara ait birden çok bildirime eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-615">A field declaration that declares multiple fields is equivalent to multiple declarations of single fields with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="4c8df-616">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4c8df-616">For example</span></span>
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
<span data-ttu-id="4c8df-617">eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="4c8df-617">is equivalent to</span></span>
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a><span data-ttu-id="4c8df-618">Statik ve örnek alanları</span><span class="sxs-lookup"><span data-stu-id="4c8df-618">Static and instance fields</span></span>

<span data-ttu-id="4c8df-619">Bir alan bildirimi `static` değiştirici içerdiğinde, bildirim tarafından tanıtılan alanlar ***statik alanlardır***.</span><span class="sxs-lookup"><span data-stu-id="4c8df-619">When a field declaration includes a `static` modifier, the fields introduced by the declaration are ***static fields***.</span></span> <span data-ttu-id="4c8df-620">`static` değiştirici yoksa, bildirim tarafından tanıtılan alanlar ***örnek alanlardır***.</span><span class="sxs-lookup"><span data-stu-id="4c8df-620">When no `static` modifier is present, the fields introduced by the declaration are ***instance fields***.</span></span> <span data-ttu-id="4c8df-621">Statik alanlar ve örnek alanları, C#tarafından desteklenen çeşitli değişken ([değişken](variables.md)) çeşitleridir ve bunlar sırasıyla ***statik değişkenler*** ve ***örnek değişkenler***olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-621">Static fields and instance fields are two of the several kinds of variables ([Variables](variables.md)) supported by C#, and at times they are referred to as ***static variables*** and ***instance variables***, respectively.</span></span>

<span data-ttu-id="4c8df-622">Statik alan, belirli bir örneğin bir parçası değildir; Bunun yerine, kapalı bir türün ([açık ve kapalı türler](types.md#open-and-closed-types)) tüm örnekleri arasında paylaşılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-622">A static field is not part of a specific instance; instead, it is shared amongst all instances of a closed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="4c8df-623">Kapalı bir sınıf türünün kaç örneğinin oluşturulduğuna bakılmaksızın, ilişkili uygulama etki alanı için yalnızca bir statik alan kopyası vardır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-623">No matter how many instances of a closed class type are created, there is only ever one copy of a static field for the associated application domain.</span></span>

<span data-ttu-id="4c8df-624">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4c8df-624">For example:</span></span>
```csharp
class C<V>
{
    static int count = 0;

    public C() {
        count++;
    }

    public static int Count {
        get { return count; }
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<double> x2 = new C<double>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<int> x3 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 2
    }
}
```

<span data-ttu-id="4c8df-625">Örnek alanı bir örneğe aittir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-625">An instance field belongs to an instance.</span></span> <span data-ttu-id="4c8df-626">Özellikle, bir sınıfın her örneği, bu sınıfın tüm örnek alanlarının ayrı bir kümesini içerir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-626">Specifically, every instance of a class contains a separate set of all the instance fields of that class.</span></span>

<span data-ttu-id="4c8df-627">Bir alana bir *member_access* ([üye erişimi](expressions.md#member-access)) `E.M`, `M` statik bir alan ise, `E` `M`içeren bir türü belirtmelidir ve `M` bir örnek alanı ise, E içeren bir türün örneğini belirtmelidir.`M`</span><span class="sxs-lookup"><span data-stu-id="4c8df-627">When a field is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static field, `E` must denote a type containing `M`, and if `M` is an instance field, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="4c8df-628">Statik ve örnek üyeleri arasındaki farklılıklar, [statik ve örnek üyelerinde](classes.md#static-and-instance-members)daha ayrıntılı bir şekilde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-628">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="readonly-fields"></a><span data-ttu-id="4c8df-629">ReadOnly alanları</span><span class="sxs-lookup"><span data-stu-id="4c8df-629">Readonly fields</span></span>

<span data-ttu-id="4c8df-630">Bir *field_declaration* `readonly` değiştiricisi içerdiğinde, bildirim tarafından tanıtılan alanlar ***salt okunur alanlardır***.</span><span class="sxs-lookup"><span data-stu-id="4c8df-630">When a *field_declaration* includes a `readonly` modifier, the fields introduced by the declaration are ***readonly fields***.</span></span> <span data-ttu-id="4c8df-631">Salt okunur alanlara doğrudan atamalar yalnızca bu bildirimin bir parçası veya aynı sınıftaki bir örnek Oluşturucu ya da statik oluşturucu içinde olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-631">Direct assignments to readonly fields can only occur as part of that declaration or in an instance constructor or static constructor in the same class.</span></span> <span data-ttu-id="4c8df-632">(Salt okunur bir alan, bu bağlamlarda birden çok kez atanabilir.) Özellikle, bir `readonly` alana doğrudan atamalara yalnızca aşağıdaki bağlamlarda izin verilir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-632">(A readonly field can be assigned to multiple times in these contexts.) Specifically, direct assignments to a `readonly` field are permitted only in the following contexts:</span></span>

*  <span data-ttu-id="4c8df-633">Alanı tanıtan *variable_declarator* (bildirime bir *variable_initializer* ekleyerek).</span><span class="sxs-lookup"><span data-stu-id="4c8df-633">In the *variable_declarator* that introduces the field (by including a *variable_initializer* in the declaration).</span></span>
*  <span data-ttu-id="4c8df-634">Bir örnek alanı için, alan bildirimini içeren sınıfın örnek oluşturucularında; statik bir alan için, alan bildirimini içeren sınıfın statik oluşturucusunda.</span><span class="sxs-lookup"><span data-stu-id="4c8df-634">For an instance field, in the instance constructors of the class that contains the field declaration; for a static field, in the static constructor of the class that contains the field declaration.</span></span> <span data-ttu-id="4c8df-635">Bunlar ayrıca, `readonly` alanı `out` veya `ref` parametresi olarak geçirmek için geçerli olduğu tek bağlamlardır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-635">These are also the only contexts in which it is valid to pass a `readonly` field as an `out` or `ref` parameter.</span></span>

<span data-ttu-id="4c8df-636">Bir `readonly` alana atamaya veya bir `out` ya da başka bir bağlamda `ref` parametre olarak geçirmeye çalışılması derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-636">Attempting to assign to a `readonly` field or pass it as an `out` or `ref` parameter in any other context is a compile-time error.</span></span>

#### <a name="using-static-readonly-fields-for-constants"></a><span data-ttu-id="4c8df-637">Sabitler için statik salt okunur alanlar kullanma</span><span class="sxs-lookup"><span data-stu-id="4c8df-637">Using static readonly fields for constants</span></span>

<span data-ttu-id="4c8df-638">Bir `static readonly` alanı, sabit değer için bir sembolik ad istendiği zaman, ancak değer türüne `const` bildiriminde izin verilmediğinde veya değer derleme zamanında hesaplanmadığında faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-638">A `static readonly` field is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a `const` declaration, or when the value cannot be computed at compile-time.</span></span> <span data-ttu-id="4c8df-639">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-639">In the example</span></span>
```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);

    private byte red, green, blue;

    public Color(byte r, byte g, byte b) {
        red = r;
        green = g;
        blue = b;
    }
}
```
<span data-ttu-id="4c8df-640">`Black`, `White`, `Red`, `Green`ve `Blue` üyeleri, değerlerinin derleme zamanında hesaplanamadığı için `const` üye olarak bildirilemez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-640">the `Black`, `White`, `Red`, `Green`, and `Blue` members cannot be declared as `const` members because their values cannot be computed at compile-time.</span></span> <span data-ttu-id="4c8df-641">Ancak, bunun yerine `static readonly` bildirimi aynı etkiye sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-641">However, declaring them `static readonly` instead has much the same effect.</span></span>

#### <a name="versioning-of-constants-and-static-readonly-fields"></a><span data-ttu-id="4c8df-642">Sabitler ve statik salt okunur alanların sürümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="4c8df-642">Versioning of constants and static readonly fields</span></span>

<span data-ttu-id="4c8df-643">Sabitler ve salt okunur alanlarında farklı ikili sürüm oluşturma semantiği vardır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-643">Constants and readonly fields have different binary versioning semantics.</span></span> <span data-ttu-id="4c8df-644">Bir ifade bir sabit değere başvurduğunda, sabit değer derleme zamanında elde edilir, ancak bir ifade salt okunur bir alana başvurduğunda alanın değeri çalışma zamanına kadar elde edilmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-644">When an expression references a constant, the value of the constant is obtained at compile-time, but when an expression references a readonly field, the value of the field is not obtained until run-time.</span></span> <span data-ttu-id="4c8df-645">İki ayrı programda oluşan bir uygulamayı düşünün:</span><span class="sxs-lookup"><span data-stu-id="4c8df-645">Consider an application that consists of two separate programs:</span></span>
```csharp
using System;

namespace Program1
{
    public class Utils
    {
        public static readonly int X = 1;
    }
}

namespace Program2
{
    class Test
    {
        static void Main() {
            Console.WriteLine(Program1.Utils.X);
        }
    }
}
```

<span data-ttu-id="4c8df-646">`Program1` ve `Program2` ad alanları ayrı olarak derlenen iki programı gösterir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-646">The `Program1` and `Program2` namespaces denote two programs that are compiled separately.</span></span> <span data-ttu-id="4c8df-647">`Program1.Utils.X` statik bir salt okunur alan olarak bildirildiği için, `Console.WriteLine` deyimin çıktısı derleme zamanında bilinmez, ancak bunun yerine çalışma zamanında elde edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-647">Because `Program1.Utils.X` is declared as a static readonly field, the value output by the `Console.WriteLine` statement is not known at compile-time, but rather is obtained at run-time.</span></span> <span data-ttu-id="4c8df-648">Bu nedenle, `X` değeri değiştirilirse ve `Program1` yeniden derlense, `Program2` yeniden derlenmese bile `Console.WriteLine` deyimin yeni değeri çıkışı olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-648">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` isn't recompiled.</span></span> <span data-ttu-id="4c8df-649">Ancak, `X` bir sabit değer vardı, `X` değeri `Program2` derlenerek elde edilir ve `Program2` yeniden derlenene kadar `Program1` değişikliklerinden etkilenmeden kalır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-649">However, had `X` been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would remain unaffected by changes in `Program1` until `Program2` is recompiled.</span></span>

### <a name="volatile-fields"></a><span data-ttu-id="4c8df-650">Geçici alanlar</span><span class="sxs-lookup"><span data-stu-id="4c8df-650">Volatile fields</span></span>

<span data-ttu-id="4c8df-651">Bir *field_declaration* `volatile` değiştiricisi içerdiğinde, bu bildirim tarafından tanıtılan alanlar ***geçici alanlardır***.</span><span class="sxs-lookup"><span data-stu-id="4c8df-651">When a *field_declaration* includes a `volatile` modifier, the fields introduced by that declaration are ***volatile fields***.</span></span>

<span data-ttu-id="4c8df-652">Geçici olmayan alanlar için, yönergeleri yeniden sıralamak için en iyi duruma getirme teknikleri, *lock_statement* ([kilit beyanı](statements.md#the-lock-statement)) tarafından sağlanmış gibi eşitleme olmadan alanlara erişen çok iş parçacıklı programlarda beklenmedik ve öngörülemeyen sonuçlara yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-652">For non-volatile fields, optimization techniques that reorder instructions can lead to unexpected and unpredictable results in multi-threaded programs that access fields without synchronization such as that provided by the *lock_statement* ([The lock statement](statements.md#the-lock-statement)).</span></span> <span data-ttu-id="4c8df-653">Bu iyileştirmeler derleyici tarafından, çalışma zamanı sistemine veya donanımla gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-653">These optimizations can be performed by the compiler, by the run-time system, or by hardware.</span></span> <span data-ttu-id="4c8df-654">Geçici alanlar için, bu tür yeniden sıralama iyileştirmeleri kısıtlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-654">For volatile fields, such reordering optimizations are restricted:</span></span>

*  <span data-ttu-id="4c8df-655">Geçici bir alan okuma, ***geçici okuma***olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-655">A read of a volatile field is called a ***volatile read***.</span></span> <span data-ttu-id="4c8df-656">Geçici okuma "alma semantiğini" içerir; diğer bir deyişle, yönerge dizisinde bundan sonra meydana gelen tüm bellek başvurularından önce gerçekleşmesi garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-656">A volatile read has "acquire semantics"; that is, it is guaranteed to occur prior to any references to memory that occur after it in the instruction sequence.</span></span>
*  <span data-ttu-id="4c8df-657">Geçici bir alan yazma, ***geçici yazma***olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-657">A write of a volatile field is called a ***volatile write***.</span></span> <span data-ttu-id="4c8df-658">Geçici yazma "yayın semantiğini" içerir; diğer bir deyişle, yönerge dizisindeki yazma yönergesinden önce herhangi bir bellek başvurularından sonra gerçekleşmesi garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-658">A volatile write has "release semantics"; that is, it is guaranteed to happen after any memory references prior to the write instruction in the instruction sequence.</span></span>

<span data-ttu-id="4c8df-659">Bu kısıtlamalar, tüm iş parçacıklarının gerçekleştirilen diğer bir iş parçacığı tarafından gerçekleştirilen geçici yazmaları gözlemleyecek şekilde tüm iş parçacıkları tarafından gerçekleştirildiğinden emin olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-659">These restrictions ensure that all threads will observe volatile writes performed by any other thread in the order in which they were performed.</span></span> <span data-ttu-id="4c8df-660">Tüm yürütme iş parçacıklarında görüldüğü şekilde, geçici yazma işlemlerinin tek toplam sıralamasını sağlamak için uygun bir uygulama gerekmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-660">A conforming implementation is not required to provide a single total ordering of volatile writes as seen from all threads of execution.</span></span> <span data-ttu-id="4c8df-661">Geçici bir alanın türü aşağıdakilerden biri olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-661">The type of a volatile field must be one of the following:</span></span>

*  <span data-ttu-id="4c8df-662">Bir *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="4c8df-662">A *reference_type*.</span></span>
*  <span data-ttu-id="4c8df-663">`byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`veya `System.UIntPtr`türü.</span><span class="sxs-lookup"><span data-stu-id="4c8df-663">The type `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, or `System.UIntPtr`.</span></span>
*  <span data-ttu-id="4c8df-664">`byte`, `sbyte`, `short`, `ushort`, `int`veya `uint`enum temel türüne sahip bir *enum_type* .</span><span class="sxs-lookup"><span data-stu-id="4c8df-664">An *enum_type* having an enum base type of `byte`, `sbyte`, `short`, `ushort`, `int`, or `uint`.</span></span>

<span data-ttu-id="4c8df-665">Örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-665">The example</span></span>
```csharp
using System;
using System.Threading;

class Test
{
    public static int result;   
    public static volatile bool finished;

    static void Thread2() {
        result = 143;    
        finished = true; 
    }

    static void Main() {
        finished = false;

        // Run Thread2() in a new thread
        new Thread(new ThreadStart(Thread2)).Start();

        // Wait for Thread2 to signal that it has a result by setting
        // finished to true.
        for (;;) {
            if (finished) {
                Console.WriteLine("result = {0}", result);
                return;
            }
        }
    }
}
```
<span data-ttu-id="4c8df-666">çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-666">produces the output:</span></span>
```console
result = 143
```

<span data-ttu-id="4c8df-667">Bu örnekte Yöntem `Main` yöntemi `Thread2`çalıştıran yeni bir iş parçacığı başlatır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-667">In this example, the method `Main` starts a new thread that runs the method `Thread2`.</span></span> <span data-ttu-id="4c8df-668">Bu yöntem, bir değeri `result`adlı geçici olmayan bir alana depolar, sonra `true` geçici alan `finished`depolar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-668">This method stores a value into a non-volatile field called `result`, then stores `true` in the volatile field `finished`.</span></span> <span data-ttu-id="4c8df-669">Ana iş parçacığı, alan `finished` `true`olarak ayarlanacağını bekler, sonra alanı `result`okur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-669">The main thread waits for the field `finished` to be set to `true`, then reads the field `result`.</span></span> <span data-ttu-id="4c8df-670">`finished` `volatile`olarak bildirildiği için, ana iş parçacığının `result`alanından `143` değeri okuması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-670">Since `finished` has been declared `volatile`, the main thread must read the value `143` from the field `result`.</span></span> <span data-ttu-id="4c8df-671">`finished` alan `volatile`bildirilmemiş ise, deponun `finished`depolamadan sonra ana iş parçacığında görünür hale `result` ve bu nedenle ana iş parçacığının alan `0` değer `result`okuması için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-671">If the field `finished` had not been declared `volatile`, then it would be permissible for the store to `result` to be visible to the main thread after the store to `finished`, and hence for the main thread to read the value `0` from the field `result`.</span></span> <span data-ttu-id="4c8df-672">`volatile` alanı olarak `finished` bildirme, bu tür tutarsızlığın yapılmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="4c8df-672">Declaring `finished` as a `volatile` field prevents any such inconsistency.</span></span>

### <a name="field-initialization"></a><span data-ttu-id="4c8df-673">Alan başlatma</span><span class="sxs-lookup"><span data-stu-id="4c8df-673">Field initialization</span></span>

<span data-ttu-id="4c8df-674">Bir alanın başlangıçtaki değeri, bir statik alan veya örnek alanı olsun, alanın türünün varsayılan değeridir ([varsayılan değerlerdir](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-674">The initial value of a field, whether it be a static field or an instance field, is the default value ([Default values](variables.md#default-values)) of the field's type.</span></span> <span data-ttu-id="4c8df-675">Bu varsayılan başlatma gerçekleştirilmeden önce bir alanın değerini gözlemlemek mümkün değildir ve bir alan bu nedenle hiçbir şekilde "başlatılmamış" olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-675">It is not possible to observe the value of a field before this default initialization has occurred, and a field is thus never "uninitialized".</span></span> <span data-ttu-id="4c8df-676">Örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-676">The example</span></span>
```csharp
using System;

class Test
{
    static bool b;
    int i;

    static void Main() {
        Test t = new Test();
        Console.WriteLine("b = {0}, i = {1}", b, t.i);
    }
}
```
<span data-ttu-id="4c8df-677">çıktıyı üretir</span><span class="sxs-lookup"><span data-stu-id="4c8df-677">produces the output</span></span>
```console
b = False, i = 0
```
<span data-ttu-id="4c8df-678">`b` ve `i` her ikisi de otomatik olarak varsayılan değerlere başlatılmış olduğundan.</span><span class="sxs-lookup"><span data-stu-id="4c8df-678">because `b` and `i` are both automatically initialized to default values.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="4c8df-679">Değişken başlatıcıları</span><span class="sxs-lookup"><span data-stu-id="4c8df-679">Variable initializers</span></span>

<span data-ttu-id="4c8df-680">Alan bildirimlerinde *variable_initializer*s bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-680">Field declarations may include *variable_initializer*s.</span></span> <span data-ttu-id="4c8df-681">Statik alanlar için, değişken başlatıcıları sınıf başlatma sırasında yürütülen atama ifadelerine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-681">For static fields, variable initializers correspond to assignment statements that are executed during class initialization.</span></span> <span data-ttu-id="4c8df-682">Örnek alanları için, değişken başlatıcıları, sınıfının bir örneği oluşturulduğunda yürütülen atama ifadelerine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-682">For instance fields, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span>

<span data-ttu-id="4c8df-683">Örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-683">The example</span></span>
```csharp
using System;

class Test
{
    static double x = Math.Sqrt(2.0);
    int i = 100;
    string s = "Hello";

    static void Main() {
        Test a = new Test();
        Console.WriteLine("x = {0}, i = {1}, s = {2}", x, a.i, a.s);
    }
}
```
<span data-ttu-id="4c8df-684">çıktıyı üretir</span><span class="sxs-lookup"><span data-stu-id="4c8df-684">produces the output</span></span>
```console
x = 1.4142135623731, i = 100, s = Hello
```
<span data-ttu-id="4c8df-685">`x` bir atama, statik alan başlatıcılarının yürütülmesi ve atamaları `i` ve `s` örnek alanı başlatıcılarının yürütülmesi durumunda meydana gelirse oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-685">because an assignment to `x` occurs when static field initializers execute and assignments to `i` and `s` occur when the instance field initializers execute.</span></span>

<span data-ttu-id="4c8df-686">[Alan başlatma](classes.md#field-initialization) bölümünde açıklanan varsayılan değer başlatma, değişken başlatıcıları olan alanlar da dahil olmak üzere tüm alanlar için gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-686">The default value initialization described in [Field initialization](classes.md#field-initialization) occurs for all fields, including fields that have variable initializers.</span></span> <span data-ttu-id="4c8df-687">Bu nedenle, bir sınıf başlatıldığında, bu sınıftaki tüm statik alanlar ilk olarak varsayılan değerlerine başlatılır ve statik alan başlatıcıları, metinsel sırada yürütülür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-687">Thus, when a class is initialized, all static fields in that class are first initialized to their default values, and then the static field initializers are executed in textual order.</span></span> <span data-ttu-id="4c8df-688">Benzer şekilde, bir sınıf örneği oluşturulduğunda, söz konusu örnekteki tüm örnek alanları ilk olarak varsayılan değerlerine başlatılır ve sonra örnek alanı başlatıcıları, metinsel sırada yürütülür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-688">Likewise, when an instance of a class is created, all instance fields in that instance are first initialized to their default values, and then the instance field initializers are executed in textual order.</span></span>

<span data-ttu-id="4c8df-689">Değişken başlatıcıların varsayılan değer durumunda gözlenecek statik alanlar mümkündür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-689">It is possible for static fields with variable initializers to be observed in their default value state.</span></span> <span data-ttu-id="4c8df-690">Ancak, bu stil önemli bir şekilde önerilmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-690">However, this is strongly discouraged as a matter of style.</span></span> <span data-ttu-id="4c8df-691">Örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-691">The example</span></span>
```csharp
using System;

class Test
{
    static int a = b + 1;
    static int b = a + 1;

    static void Main() {
        Console.WriteLine("a = {0}, b = {1}", a, b);
    }
}
```
<span data-ttu-id="4c8df-692">Bu davranışı sergiler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-692">exhibits this behavior.</span></span> <span data-ttu-id="4c8df-693">A ve b 'nin dairesel tanımlarına rağmen program geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-693">Despite the circular definitions of a and b, the program is valid.</span></span> <span data-ttu-id="4c8df-694">Çıkışa neden olur</span><span class="sxs-lookup"><span data-stu-id="4c8df-694">It results in the output</span></span>
```console
a = 1, b = 2
```
<span data-ttu-id="4c8df-695">statik alanlar `a` ve `b`, başlatıcıları yürütülmeden önce `0` (`int`için varsayılan değer) olarak başlatıldığından.</span><span class="sxs-lookup"><span data-stu-id="4c8df-695">because the static fields `a` and `b` are initialized to `0` (the default value for `int`) before their initializers are executed.</span></span> <span data-ttu-id="4c8df-696">`a` başlatıcısı çalıştığında, `b` değeri sıfırdır ve bu nedenle `a` `1`olarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-696">When the initializer for `a` runs, the value of `b` is zero, and so `a` is initialized to `1`.</span></span> <span data-ttu-id="4c8df-697">`b` başlatıcısı çalıştığında, `a` değeri zaten `1`ve `b` `2`olarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-697">When the initializer for `b` runs, the value of `a` is already `1`, and so `b` is initialized to `2`.</span></span>

#### <a name="static-field-initialization"></a><span data-ttu-id="4c8df-698">Statik alan başlatma</span><span class="sxs-lookup"><span data-stu-id="4c8df-698">Static field initialization</span></span>

<span data-ttu-id="4c8df-699">Bir sınıfın statik alan değişkeni başlatıcıları, sınıf bildiriminde göründükleri metin sırasına göre yürütülen atama dizisine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-699">The static field variable initializers of a class correspond to a sequence of assignments that are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="4c8df-700">Sınıfında statik bir Oluşturucu ([statik oluşturucular](classes.md#static-constructors)) varsa, statik alan başlatıcılarının yürütülmesi bu statik oluşturucunun yürütülmesi için hemen önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-700">If a static constructor ([Static constructors](classes.md#static-constructors)) exists in the class, execution of the static field initializers occurs immediately prior to executing that static constructor.</span></span> <span data-ttu-id="4c8df-701">Aksi takdirde, statik alan başlatıcıları, bu sınıfın statik alanı ilk kullanılmadan önce uygulamayla ilgili bir süre içinde yürütülür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-701">Otherwise, the static field initializers are executed at an implementation-dependent time prior to the first use of a static field of that class.</span></span> <span data-ttu-id="4c8df-702">Örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-702">The example</span></span>
```csharp
using System;

class Test 
{ 
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    public static int X = Test.F("Init A");
}

class B
{
    public static int Y = Test.F("Init B");
}
```
<span data-ttu-id="4c8df-703">çıktıyı üretebilir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-703">might produce either the output:</span></span>
```console
Init A
Init B
1 1
```
<span data-ttu-id="4c8df-704">veya çıkış:</span><span class="sxs-lookup"><span data-stu-id="4c8df-704">or the output:</span></span>
```console
Init B
Init A
1 1
```
<span data-ttu-id="4c8df-705">`X`başlatıcısının ve `Y`başlatıcısının yürütülmesi her iki sırada da gerçekleşebildiğinden; Bunlar yalnızca bu alanlara başvurularından önce gerçekleşirler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-705">because the execution of `X`'s initializer and `Y`'s initializer could occur in either order; they are only constrained to occur before the references to those fields.</span></span> <span data-ttu-id="4c8df-706">Ancak, örnekte:</span><span class="sxs-lookup"><span data-stu-id="4c8df-706">However, in the example:</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    static A() {}

    public static int X = Test.F("Init A");
}

class B
{
    static B() {}

    public static int Y = Test.F("Init B");
}
```
<span data-ttu-id="4c8df-707">çıkışın olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-707">the output must be:</span></span>
```console
Init B
Init A
1 1
```
<span data-ttu-id="4c8df-708">statik oluşturucuların çalıştırıldığı zaman ( [statik oluşturucularda](classes.md#static-constructors)tanımlandığı gibi) için kurallar, `B`statik oluşturucusunun (ve bu nedenle `B`statik alan başlatıcılarının) `A`statik Oluşturucusu ve alan başlatıcılarından önce çalıştırılması gerektiğini sağlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-708">because the rules for when static constructors execute (as defined in [Static constructors](classes.md#static-constructors)) provide that `B`'s static constructor (and hence `B`'s static field initializers) must run before `A`'s static constructor and field initializers.</span></span>

#### <a name="instance-field-initialization"></a><span data-ttu-id="4c8df-709">Örnek alanı başlatma</span><span class="sxs-lookup"><span data-stu-id="4c8df-709">Instance field initialization</span></span>

<span data-ttu-id="4c8df-710">Bir sınıfın örnek alanı değişken başlatıcıları, bu sınıfın örnek oluşturucularından ([Oluşturucu başlatıcıların](classes.md#constructor-initializers)) herhangi birine girişte hemen yürütülen atama dizisine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-710">The instance field variable initializers of a class correspond to a sequence of assignments that are executed immediately upon entry to any one of the instance constructors ([Constructor initializers](classes.md#constructor-initializers)) of that class.</span></span> <span data-ttu-id="4c8df-711">Değişken başlatıcıları, sınıf bildiriminde göründükleri metin sırasına göre yürütülür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-711">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="4c8df-712">Sınıf örneği oluşturma ve başlatma işlemi, [örnek oluşturucularda](classes.md#instance-constructors)daha ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-712">The class instance creation and initialization process is described further in [Instance constructors](classes.md#instance-constructors).</span></span>

<span data-ttu-id="4c8df-713">Örnek alanı için değişken Başlatıcısı oluşturulan örneğe başvuramaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-713">A variable initializer for an instance field cannot reference the instance being created.</span></span> <span data-ttu-id="4c8df-714">Bu nedenle, bir değişken başlatıcısındaki `this` başvurmak için derleme zamanı hatası, bir değişken başlatıcısı için bir *simple_name*aracılığıyla bir örnek üyesine başvuruda bulunmak üzere bir derleme zamanı hatası olduğundan.</span><span class="sxs-lookup"><span data-stu-id="4c8df-714">Thus, it is a compile-time error to reference `this` in a variable initializer, as it is a compile-time error for a variable initializer to reference any instance member through a *simple_name*.</span></span> <span data-ttu-id="4c8df-715">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-715">In the example</span></span>
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
<span data-ttu-id="4c8df-716">`y` için değişken başlatıcısı, oluşturulmakta olan Örneğin bir üyesine başvurduğundan derleme zamanı hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-716">the variable initializer for `y` results in a compile-time error because it references a member of the instance being created.</span></span>

## <a name="methods"></a><span data-ttu-id="4c8df-717">Yöntemler</span><span class="sxs-lookup"><span data-stu-id="4c8df-717">Methods</span></span>

<span data-ttu-id="4c8df-718">Bir ***Yöntem*** , bir nesne veya sınıf tarafından gerçekleştirilebilecek bir hesaplama veya eylem uygulayan bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-718">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="4c8df-719">Yöntemler *method_declaration*s kullanılarak bildirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-719">Methods are declared using *method_declaration*s:</span></span>

```antlr
method_declaration
    : method_header method_body
    ;

method_header
    : attributes? method_modifier* 'partial'? return_type member_name type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause*
    ;

method_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | 'async'
    | method_modifier_unsafe
    ;

return_type
    : type
    | 'void'
    ;

member_name
    : identifier
    | interface_type '.' identifier
    ;

method_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

<span data-ttu-id="4c8df-720">Bir *method_declaration* bir *öznitelikler* kümesi ([öznitelikler](attributes.md)) ve dört erişim değiştiricisinin ([erişim değiştiricileri](classes.md#access-modifiers)) geçerli bir birleşimini, `new` ([Yeni değiştirici](classes.md#the-new-modifier)), `static` ([statik ve örnek yöntemleri](classes.md#static-and-instance-methods)), `virtual` ([sanal yöntemler](classes.md#virtual-methods)), `override` ([geçersiz kılma yöntemleri](classes.md#override-methods)), `sealed` ([korumalı](classes.md#sealed-methods)Yöntemler), `abstract` ([soyut yöntemler](classes.md#abstract-methods)) ve `extern` ([dış Yöntemler](classes.md#external-methods)) değiştiricilerini içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-720">A *method_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="4c8df-721">Aşağıdakilerin tümü doğru ise bir bildirimin geçerli bir değiştiriciler birleşimi vardır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-721">A declaration has a valid combination of modifiers if all of the following are true:</span></span>

*  <span data-ttu-id="4c8df-722">Bildirim, geçerli bir erişim değiştiricileri ([erişim değiştiricileri](classes.md#access-modifiers)) birleşimini içerir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-722">The declaration includes a valid combination of access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span>
*  <span data-ttu-id="4c8df-723">Bildirim, aynı değiştiriciyi birden çok kez içermez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-723">The declaration does not include the same modifier multiple times.</span></span>
*  <span data-ttu-id="4c8df-724">Bildirim şu değiştiricilerin en çok birini içerir: `static`, `virtual`ve `override`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-724">The declaration includes at most one of the following modifiers: `static`, `virtual`, and `override`.</span></span>
*  <span data-ttu-id="4c8df-725">Bildirim şu değiştiricilerin en çok birini içerir: `new` ve `override`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-725">The declaration includes at most one of the following modifiers: `new` and `override`.</span></span>
*  <span data-ttu-id="4c8df-726">Bildirim `abstract` değiştiricisini içeriyorsa, bildirim şu değiştiricilerin hiçbirini içermez: `static`, `virtual`, `sealed` veya `extern`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-726">If the declaration includes the `abstract` modifier, then the declaration does not include any of the following modifiers: `static`, `virtual`, `sealed` or `extern`.</span></span>
*  <span data-ttu-id="4c8df-727">Bildirim `private` değiştiricisini içeriyorsa, bildirim şu değiştiricilerin hiçbirini içermez: `virtual`, `override`veya `abstract`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-727">If the declaration includes the `private` modifier, then the declaration does not include any of the following modifiers: `virtual`, `override`, or `abstract`.</span></span>
*  <span data-ttu-id="4c8df-728">Bildirim `sealed` değiştiricisini içeriyorsa, bildirim `override` değiştiricisini de içerir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-728">If the declaration includes the `sealed` modifier, then the declaration also includes the `override` modifier.</span></span>
*  <span data-ttu-id="4c8df-729">Bildirim `partial` değiştiricisini içeriyorsa, şu değiştiricilerin hiçbirini içermez: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`veya `extern`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-729">If the declaration includes the `partial` modifier, then it does not include any of the following modifiers: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`, or `extern`.</span></span>

<span data-ttu-id="4c8df-730">`async` değiştiricisine sahip bir yöntem zaman uyumsuz bir işlevdir ve [zaman uyumsuz işlevlerde](classes.md#async-functions)açıklanan kuralları izler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-730">A method that has the `async` modifier is an async function and follows the rules described in [Async functions](classes.md#async-functions).</span></span>

<span data-ttu-id="4c8df-731">Bir yöntem bildiriminin *return_type* , yöntemi tarafından hesaplanan ve döndürülen değerin türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-731">The *return_type* of a method declaration specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="4c8df-732">Yöntemin bir değer döndürmezse *return_type* `void`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-732">The *return_type* is `void` if the method does not return a value.</span></span> <span data-ttu-id="4c8df-733">Bildirim `partial` değiştiricisini içeriyorsa, dönüş türü `void`olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-733">If the declaration includes the `partial` modifier, then the return type must be `void`.</span></span>

<span data-ttu-id="4c8df-734">*MEMBER_NAME* , yöntemin adını belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-734">The *member_name* specifies the name of the method.</span></span> <span data-ttu-id="4c8df-735">Yöntem açık arabirim üyesi uygulaması ([Açık arabirim üyesi uygulamalar](interfaces.md#explicit-interface-member-implementations)) değilse, *MEMBER_NAME* yalnızca bir *tanıtıcıdır*.</span><span class="sxs-lookup"><span data-stu-id="4c8df-735">Unless the method is an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="4c8df-736">Açık arabirim üyesi uygulama için *MEMBER_NAME* , bir *interface_type* ve ardından bir "`.`" ve bir *tanımlayıcı*oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-736">For an explicit interface member implementation, the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="4c8df-737">İsteğe bağlı *type_parameter_list* , yöntemin tür parametrelerini belirtir ([tür parametreleri](classes.md#type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-737">The optional *type_parameter_list* specifies the type parameters of the method ([Type parameters](classes.md#type-parameters)).</span></span> <span data-ttu-id="4c8df-738">Bir *type_parameter_list* belirtilirse, yöntemi ***genel bir yöntemdir***.</span><span class="sxs-lookup"><span data-stu-id="4c8df-738">If a *type_parameter_list* is specified the method is a ***generic method***.</span></span> <span data-ttu-id="4c8df-739">Metodun bir `extern` değiştiricisi varsa, bir *type_parameter_list* belirtilemez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-739">If the method has an `extern` modifier, a *type_parameter_list* cannot be specified.</span></span>

<span data-ttu-id="4c8df-740">İsteğe bağlı *formal_parameter_list* yönteminin parametrelerini belirtir ([Yöntem parametreleri](classes.md#method-parameters)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-740">The optional *formal_parameter_list* specifies the parameters of the method ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="4c8df-741">İsteğe bağlı *type_parameter_constraints_clause*, bağımsız tür parametrelerinde ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) kısıtlamaları belirtir ve yalnızca bir *type_parameter_list* sağlandıysa ve yöntemin bir `override` değiştiricisi yoksa belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-741">The optional *type_parameter_constraints_clause*s specify constraints on individual type parameters ([Type parameter constraints](classes.md#type-parameter-constraints)) and may only be specified if a *type_parameter_list* is also supplied, and the method does not have an `override` modifier.</span></span>

<span data-ttu-id="4c8df-742">*Return_type* ve bir yöntemin *formal_parameter_list* başvuruda bulunulan türlerin her biri, metodun kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak en az erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-742">The *return_type* and each of the types referenced in the *formal_parameter_list* of a method must be at least as accessible as the method itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="4c8df-743">*Method_body* noktalı virgül, ***deyim gövdesi*** veya ***ifade gövdesi***.</span><span class="sxs-lookup"><span data-stu-id="4c8df-743">The *method_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="4c8df-744">Deyim gövdesi, yöntemi çağrıldığında yürütülecek deyimleri belirten bir *bloğundan*oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-744">A statement body consists of a *block*, which specifies the statements to execute when the method is invoked.</span></span> <span data-ttu-id="4c8df-745">Bir ifade gövdesi, bir *ifadenin* ve noktalı virgülden oluşan `=>` oluşur ve yöntem çağrıldığında gerçekleştirilecek tek bir ifadeyi gösterir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-745">An expression body consists of `=>` followed by an *expression* and a semicolon, and denotes a single expression to perform when the method is invoked.</span></span> 

<span data-ttu-id="4c8df-746">`abstract` ve `extern` yöntemleri için *method_body* yalnızca noktalı virgülden oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-746">For `abstract` and `extern` methods, the *method_body* consists simply of a semicolon.</span></span> <span data-ttu-id="4c8df-747">`partial` yöntemler için *method_body* noktalı virgül, blok gövdesinden veya bir ifade gövdesinden oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-747">For `partial` methods the *method_body* may consist of either a semicolon, a block body or an expression body.</span></span> <span data-ttu-id="4c8df-748">Diğer tüm yöntemler için *method_body* bir blok gövdesi ya da bir ifade gövdesidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-748">For all other methods, the *method_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="4c8df-749">*Method_body* noktalı virgül içeriyorsa, bildirim `async` değiştiricisini içermeyebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-749">If the *method_body* consists of a semicolon, then the declaration may not include the `async` modifier.</span></span>

<span data-ttu-id="4c8df-750">Ad, tür parametre listesi ve bir yöntemin biçimsel parametre listesi metodun imzasını ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)) tanımlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-750">The name, the type parameter list and the formal parameter list of a method define the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the method.</span></span> <span data-ttu-id="4c8df-751">Özellikle, bir yöntemin imzası adından, tür parametrelerinin sayısından ve biçimsel parametrelerinin sayısı, değiştiricilerinden ve türlerinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-751">Specifically, the signature of a method consists of its name, the number of type parameters and the number, modifiers, and types of its formal parameters.</span></span> <span data-ttu-id="4c8df-752">Bu amaçlar için, bir biçimsel parametre türünde oluşan yöntemin tür parametresi, kendi adı tarafından değil, yönteminin tür bağımsız değişkeni listesindeki sıra konumuna göre tanımlanır. Dönüş türü yöntemin imzasının bir parçası değildir, ya da tür parametrelerinin ya da biçimsel parametrelerin adları değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-752">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.The return type is not part of a method's signature, nor are the names of the type parameters or the formal parameters.</span></span>

<span data-ttu-id="4c8df-753">Bir yöntemin adı aynı sınıfta belirtilen diğer tüm yöntemlerin adlarından farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-753">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="4c8df-754">Ayrıca, bir yöntemin imzası aynı sınıfta belirtilen diğer tüm yöntemlerin imzalarından farklı olmalıdır ve aynı sınıfta belirtilen iki yöntem yalnızca `ref` ve `out`farklı imzalara sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-754">In addition, the signature of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>

<span data-ttu-id="4c8df-755">Yöntemin *type_parameter*s *method_declaration*tamamında kapsam içinde yer alabilir ve bu kapsam genelinde *return_type*, *method_body*ve *type_parameter_constraints_clause*, ancak *özniteliklerde*olmayan türleri oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-755">The method's *type_parameter*s are in scope throughout the *method_declaration*, and can be used to form types throughout that scope in *return_type*, *method_body*, and *type_parameter_constraints_clause*s but not in *attributes*.</span></span>

<span data-ttu-id="4c8df-756">Tüm biçimsel parametrelerin ve tür parametrelerinin adları farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-756">All formal parameters and type parameters must have different names.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="4c8df-757">Yöntem parametreleri</span><span class="sxs-lookup"><span data-stu-id="4c8df-757">Method parameters</span></span>

<span data-ttu-id="4c8df-758">Bir yöntemin parametreleri, varsa, yöntemin *formal_parameter_list*tarafından tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-758">The parameters of a method, if any, are declared by the method's *formal_parameter_list*.</span></span>

```antlr
formal_parameter_list
    : fixed_parameters
    | fixed_parameters ',' parameter_array
    | parameter_array
    ;

fixed_parameters
    : fixed_parameter (',' fixed_parameter)*
    ;

fixed_parameter
    : attributes? parameter_modifier? type identifier default_argument?
    ;

default_argument
    : '=' expression
    ;

parameter_modifier
    : 'ref'
    | 'out'
    | 'this'
    ;

parameter_array
    : attributes? 'params' array_type identifier
    ;
```

<span data-ttu-id="4c8df-759">Biçimsel parametre listesi bir veya daha fazla virgülle ayrılmış parametrelerden oluşur ve bunlardan yalnızca sonuncusu *parameter_array*olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-759">The formal parameter list consists of one or more comma-separated parameters of which only the last may be a *parameter_array*.</span></span>

<span data-ttu-id="4c8df-760">*Fixed_parameter* , isteğe bağlı bir *öznitelik* kümesi ([öznitelikler](attributes.md)), isteğe bağlı `ref`, `out` veya `this` değiştiricisi, bir *tür*, *tanımlayıcı* ve isteğe bağlı bir *default_argument*oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-760">A *fixed_parameter* consists of an optional set of *attributes* ([Attributes](attributes.md)), an optional `ref`, `out` or `this` modifier, a *type*, an *identifier* and an optional *default_argument*.</span></span> <span data-ttu-id="4c8df-761">Her *fixed_parameter* , verilen türdeki bir parametreyi verilen ada bildirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-761">Each *fixed_parameter* declares a parameter of the given type with the given name.</span></span> <span data-ttu-id="4c8df-762">`this` değiştiricisi yöntemi bir genişletme yöntemi olarak belirler ve yalnızca bir statik metodun ilk parametresinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-762">The `this` modifier designates the method as an extension method and is only allowed on the first parameter of a static method.</span></span> <span data-ttu-id="4c8df-763">Uzantı yöntemleri [uzantı yöntemlerinde](classes.md#extension-methods)daha ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-763">Extension methods are further described in [Extension methods](classes.md#extension-methods).</span></span>

<span data-ttu-id="4c8df-764">*Default_argument* bir *fixed_parameter* , ***isteğe bağlı bir parametre***olarak bilinir, ancak *default_argument* olmayan bir *fixed_parameter* ***gerekli bir parametredir***.</span><span class="sxs-lookup"><span data-stu-id="4c8df-764">A *fixed_parameter* with a *default_argument* is known as an ***optional parameter***, whereas a *fixed_parameter* without a *default_argument* is a ***required parameter***.</span></span> <span data-ttu-id="4c8df-765">Gerekli bir parametre, *formal_parameter_list*bir isteğe bağlı parametreden sonra görünmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-765">A required parameter may not appear after an optional parameter in a *formal_parameter_list*.</span></span>

<span data-ttu-id="4c8df-766">`ref` veya `out` parametresinin *default_argument*olamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-766">A `ref` or `out` parameter cannot have a *default_argument*.</span></span> <span data-ttu-id="4c8df-767">Bir *default_argument* *ifade* aşağıdakilerden biri olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-767">The *expression* in a *default_argument* must be one of the following:</span></span>

*  <span data-ttu-id="4c8df-768">bir *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="4c8df-768">a *constant_expression*</span></span>
*  <span data-ttu-id="4c8df-769">formun bir ifadesi `new S()` `S` bir değer türüdür</span><span class="sxs-lookup"><span data-stu-id="4c8df-769">an expression of the form `new S()` where `S` is a value type</span></span>
*  <span data-ttu-id="4c8df-770">formun bir ifadesi `default(S)` `S` bir değer türüdür</span><span class="sxs-lookup"><span data-stu-id="4c8df-770">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="4c8df-771">*İfade* , bir kimlikle örtük olarak dönüştürülebilir veya parametrenin türüne null olabilen dönüştürmeye sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-771">The *expression* must be implicitly convertible by an identity or nullable conversion to the type of the parameter.</span></span>

<span data-ttu-id="4c8df-772">Bir uygulama kısmi yöntem bildiriminde ([kısmi Yöntemler](classes.md#partial-methods)) isteğe bağlı parametreler oluşursa, açık bir arabirim üye uygulaması ([Açık arabirim üyesi uygulamalar](interfaces.md#explicit-interface-member-implementations)) veya tek parametreli Dizin Oluşturucu bildiriminde ([Dizin oluşturucular](classes.md#indexers)), bu Üyeler asla bağımsız değişkenlerin atlanmasına izin vermek için hiçbir şekilde çağrılabileceğinden, derleyici bir uyarı vermelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-772">If optional parameters occur in an implementing partial method declaration ([Partial methods](classes.md#partial-methods)) , an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)) or in a single-parameter indexer declaration ([Indexers](classes.md#indexers)) the compiler should give a warning, since these members can never be invoked in a way that permits arguments to be omitted.</span></span>

<span data-ttu-id="4c8df-773">*Parameter_array* , isteğe bağlı bir *öznitelikler* kümesinden ([öznitelikler](attributes.md)), bir `params` değiştiricisinden, bir *array_type*ve bir *tanımlayıcıdan*oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-773">A *parameter_array* consists of an optional set of *attributes* ([Attributes](attributes.md)), a `params` modifier, an *array_type*, and an *identifier*.</span></span> <span data-ttu-id="4c8df-774">Bir parametre dizisi verilen bir ada sahip belirtilen dizi türünün tek bir parametresini bildirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-774">A parameter array declares a single parameter of the given array type with the given name.</span></span> <span data-ttu-id="4c8df-775">Bir parametre dizisinin *array_type* , tek boyutlu bir dizi türü ([dizi türleri](arrays.md#array-types)) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-775">The *array_type* of a parameter array must be a single-dimensional array type ([Array types](arrays.md#array-types)).</span></span> <span data-ttu-id="4c8df-776">Bir yöntem çağrısında, bir parametre dizisi verilen dizi türünün tek bir bağımsız değişkeninin belirtilmesine izin verir veya dizi öğe türünde sıfır veya daha fazla bağımsız değişken belirtilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-776">In a method invocation, a parameter array permits either a single argument of the given array type to be specified, or it permits zero or more arguments of the array element type to be specified.</span></span> <span data-ttu-id="4c8df-777">Parametre dizileri, [parametre dizileri](classes.md#parameter-arrays)içinde daha ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-777">Parameter arrays are described further in [Parameter arrays](classes.md#parameter-arrays).</span></span>

<span data-ttu-id="4c8df-778">Bir *parameter_array* isteğe bağlı bir parametreden sonra gerçekleşebilir, ancak varsayılan bir değere sahip olamaz. bunun yerine *parameter_array* bağımsız değişkenlerin atlanmasından sonra boş bir dizi oluşturulmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-778">A *parameter_array* may occur after an optional parameter, but cannot have a default value -- the omission of arguments for a *parameter_array* would instead result in the creation of an empty array.</span></span>

<span data-ttu-id="4c8df-779">Aşağıdaki örnek farklı parametre türlerini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-779">The following example illustrates different kinds of parameters:</span></span>
```csharp
public void M(
    ref int      i,
    decimal      d,
    bool         b = false,
    bool?        n = false,
    string       s = "Hello",
    object       o = null,
    T            t = default(T),
    params int[] a
) { }
```

<span data-ttu-id="4c8df-780">`M`için *formal_parameter_list* , `i` gerekli bir başvuru parametresidir, `d` gerekli bir değer parametresi, `b`, `s`, `o` ve `t` ise isteğe bağlı değer parametreleridir ve `a` bir parametre dizisidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-780">In the *formal_parameter_list* for `M`, `i` is a required ref parameter, `d` is a required value parameter, `b`, `s`, `o` and `t` are optional value parameters and `a` is a parameter array.</span></span>

<span data-ttu-id="4c8df-781">Yöntem bildirimi parametreler, tür parametreleri ve yerel değişkenler için ayrı bir bildirim alanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-781">A method declaration creates a separate declaration space for parameters, type parameters and local variables.</span></span> <span data-ttu-id="4c8df-782">Adlar, bu bildirim alanına tür parametresi listesi ve yöntemin biçimsel parametre listesi ve yöntemin *bloğundaki* yerel değişken bildirimleri tarafından tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-782">Names are introduced into this declaration space by the type parameter list and the formal parameter list of the method and by local variable declarations in the *block* of the method.</span></span> <span data-ttu-id="4c8df-783">Bir yöntem bildirim alanının iki üyesinin aynı ada sahip olması için bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-783">It is an error for two members of a method declaration space to have the same name.</span></span> <span data-ttu-id="4c8df-784">Aynı ada sahip öğeleri içermesi için bir iç içe geçmiş bildirim alanının yöntem bildirim alanı ve yerel değişken bildirim alanı için bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-784">It is an error for the method declaration space and the local variable declaration space of a nested declaration space to contain elements with the same name.</span></span>

<span data-ttu-id="4c8df-785">Yöntem çağırma ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)), bu çağrıya özgü bir kopya oluşturur; Bu, biçimsel parametreler ve yöntemin yerel değişkenleri ve çağrının bağımsız değişken listesi, yeni oluşturulan biçimsel parametrelere değerler veya değişken başvuruları atar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-785">A method invocation ([Method invocations](expressions.md#method-invocations)) creates a copy, specific to that invocation, of the formal parameters and local variables of the method, and the argument list of the invocation assigns values or variable references to the newly created formal parameters.</span></span> <span data-ttu-id="4c8df-786">Bir yöntem *bloğunun* içinde, biçimsel parametrelere *simple_name* ifadelerinde tanımlayıcıları ([basit adlar](expressions.md#simple-names)) tarafından başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-786">Within the *block* of a method, formal parameters can be referenced by their identifiers in *simple_name* expressions ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="4c8df-787">Dört tür biçimsel parametre vardır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-787">There are four kinds of formal parameters:</span></span>

*  <span data-ttu-id="4c8df-788">Herhangi bir değiştirici olmadan bildirildiği değer parametreleri.</span><span class="sxs-lookup"><span data-stu-id="4c8df-788">Value parameters, which are declared without any modifiers.</span></span>
*  <span data-ttu-id="4c8df-789">`ref` değiştiricisiyle belirtilen başvuru parametreleri.</span><span class="sxs-lookup"><span data-stu-id="4c8df-789">Reference parameters, which are declared with the `ref` modifier.</span></span>
*  <span data-ttu-id="4c8df-790">`out` değiştiricisiyle belirtilen çıkış parametreleri.</span><span class="sxs-lookup"><span data-stu-id="4c8df-790">Output parameters, which are declared with the `out` modifier.</span></span>
*  <span data-ttu-id="4c8df-791">`params` değiştiricisiyle belirtilen parametre dizileri.</span><span class="sxs-lookup"><span data-stu-id="4c8df-791">Parameter arrays, which are declared with the `params` modifier.</span></span>

<span data-ttu-id="4c8df-792">[İmzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)' de açıklandığı gibi, `ref` ve `out` değiştiricileri yöntemin imzasının bir parçasıdır, ancak `params` değiştiricisi değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-792">As described in [Signatures and overloading](basic-concepts.md#signatures-and-overloading), the `ref` and `out` modifiers are part of a method's signature, but the `params` modifier is not.</span></span>

#### <a name="value-parameters"></a><span data-ttu-id="4c8df-793">Değer parametreleri</span><span class="sxs-lookup"><span data-stu-id="4c8df-793">Value parameters</span></span>

<span data-ttu-id="4c8df-794">Değiştirici olmadan belirtilen bir parametre bir değer parametresidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-794">A parameter declared with no modifiers is a value parameter.</span></span> <span data-ttu-id="4c8df-795">Değer parametresi, yöntem çağrısında sağlanan karşılık gelen bağımsız değişkenden ilk değerini alan yerel bir değişkene karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-795">A value parameter corresponds to a local variable that gets its initial value from the corresponding argument supplied in the method invocation.</span></span>

<span data-ttu-id="4c8df-796">Bir biçimsel parametre bir değer parametresi olduğunda, bir yöntem çağrısında karşılık gelen bağımsız değişken, biçimsel parametre türüne örtük olarak dönüştürülebilir ([örtük dönüştürmeler](conversions.md#implicit-conversions)) bir ifade olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-796">When a formal parameter is a value parameter, the corresponding argument in a method invocation must be an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the formal parameter type.</span></span>

<span data-ttu-id="4c8df-797">Bir yöntem bir değer parametresine yeni değerler atamaya izin verilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-797">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="4c8df-798">Bu atamalar yalnızca değer parametresi tarafından temsil edilen yerel depolama konumunu etkiler — Yöntem çağrısında verilen gerçek bağımsız değişkeni üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-798">Such assignments only affect the local storage location represented by the value parameter—they have no effect on the actual argument given in the method invocation.</span></span>

#### <a name="reference-parameters"></a><span data-ttu-id="4c8df-799">Başvuru parametreleri</span><span class="sxs-lookup"><span data-stu-id="4c8df-799">Reference parameters</span></span>

<span data-ttu-id="4c8df-800">`ref` değiştiricisi ile belirtilen bir parametre bir başvuru parametresidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-800">A parameter declared with a `ref` modifier is a reference parameter.</span></span> <span data-ttu-id="4c8df-801">Değer parametresinden farklı olarak, başvuru parametresi yeni bir depolama konumu oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-801">Unlike a value parameter, a reference parameter does not create a new storage location.</span></span> <span data-ttu-id="4c8df-802">Bunun yerine, başvuru parametresi, yöntem çağrısında bağımsız değişken olarak verilen değişkenle aynı depolama konumunu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="4c8df-802">Instead, a reference parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="4c8df-803">Bir biçimsel parametre bir başvuru parametresi olduğunda, bir yöntem çağrısında karşılık gelen bağımsız değişken, biçimsel parametre ile aynı türdeki bir *variable_reference* ([kesin atamayı belirlemek için kesin kurallar](variables.md#precise-rules-for-determining-definite-assignment)) içermelidir `ref`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-803">When a formal parameter is a reference parameter, the corresponding argument in a method invocation must consist of the keyword `ref` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="4c8df-804">Bir değişken başvuru parametresi olarak geçirilebilmesi için kesinlikle atanmalı.</span><span class="sxs-lookup"><span data-stu-id="4c8df-804">A variable must be definitely assigned before it can be passed as a reference parameter.</span></span>

<span data-ttu-id="4c8df-805">Bir yöntem içinde, başvuru parametresi her zaman kesinlikle atanmış olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-805">Within a method, a reference parameter is always considered definitely assigned.</span></span>

<span data-ttu-id="4c8df-806">Yineleyici ([yineleyiciler](classes.md#iterators)) olarak belirtilen bir yöntemin başvuru parametreleri olamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-806">A method declared as an iterator ([Iterators](classes.md#iterators)) cannot have reference parameters.</span></span>

<span data-ttu-id="4c8df-807">Örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-807">The example</span></span>
```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("i = {0}, j = {1}", i, j);
    }
}
```
<span data-ttu-id="4c8df-808">çıktıyı üretir</span><span class="sxs-lookup"><span data-stu-id="4c8df-808">produces the output</span></span>
```console
i = 2, j = 1
```

<span data-ttu-id="4c8df-809">`Main``Swap` çağrılması için `x` `i` temsil eder ve `y` `j`temsil eder.</span><span class="sxs-lookup"><span data-stu-id="4c8df-809">For the invocation of `Swap` in `Main`, `x` represents `i` and `y` represents `j`.</span></span> <span data-ttu-id="4c8df-810">Bu nedenle, çağrının `i` ve `j`değerlerini değiştirme etkisi vardır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-810">Thus, the invocation has the effect of swapping the values of `i` and `j`.</span></span>

<span data-ttu-id="4c8df-811">Başvuru parametreleri alan bir yöntemde, birden fazla ad aynı depolama konumunu temsil etmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-811">In a method that takes reference parameters it is possible for multiple names to represent the same storage location.</span></span> <span data-ttu-id="4c8df-812">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-812">In the example</span></span>
```csharp
class A
{
    string s;

    void F(ref string a, ref string b) {
        s = "One";
        a = "Two";
        b = "Three";
    }

    void G() {
        F(ref s, ref s);
    }
}
```
<span data-ttu-id="4c8df-813">`G` `F` çağrısı, `a` ve `b`için `s` başvurusunu geçirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-813">the invocation of `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="4c8df-814">Bu nedenle, bu çağrı için `s`, `a`ve `b` adları aynı depolama konumuna başvurur ve bu üç atama, örnek alanını `s`değiştirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-814">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance field `s`.</span></span>

#### <a name="output-parameters"></a><span data-ttu-id="4c8df-815">Çıktı parametreleri</span><span class="sxs-lookup"><span data-stu-id="4c8df-815">Output parameters</span></span>

<span data-ttu-id="4c8df-816">`out` değiştiricisi ile belirtilen bir parametre bir çıkış parametresidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-816">A parameter declared with an `out` modifier is an output parameter.</span></span> <span data-ttu-id="4c8df-817">Bir başvuru parametresine benzer şekilde, çıkış parametresi yeni bir depolama konumu oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-817">Similar to a reference parameter, an output parameter does not create a new storage location.</span></span> <span data-ttu-id="4c8df-818">Bunun yerine, bir çıktı parametresi, yöntem çağrısında bağımsız değişken olarak verilen değişkenle aynı depolama konumunu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="4c8df-818">Instead, an output parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="4c8df-819">Bir biçimsel parametre bir çıkış parametresi olduğunda, bir yöntem çağrısında karşılık gelen bağımsız değişken, biçimsel parametre ile aynı türdeki bir *variable_reference* ([kesin atamayı belirlemek için kesin kurallar](variables.md#precise-rules-for-determining-definite-assignment)) içermelidir `out`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-819">When a formal parameter is an output parameter, the corresponding argument in a method invocation must consist of the keyword `out` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="4c8df-820">Bir değişken, çıkış parametresi olarak geçirilebilmesi için kesinlikle atanmamalıdır, ancak bir değişkenin çıkış parametresi olarak geçirildiği bir çağrıdan sonra, değişken kesinlikle atanmış olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-820">A variable need not be definitely assigned before it can be passed as an output parameter, but following an invocation where a variable was passed as an output parameter, the variable is considered definitely assigned.</span></span>

<span data-ttu-id="4c8df-821">Bir yöntem içinde, yerel bir değişken gibi, bir çıkış parametresi başlangıçta atanmamış olarak kabul edilir ve değeri kullanılmadan önce kesinlikle atanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-821">Within a method, just like a local variable, an output parameter is initially considered unassigned and must be definitely assigned before its value is used.</span></span>

<span data-ttu-id="4c8df-822">Metodun her çıkış parametresi, yöntemin döndürüldüğünden önce kesinlikle atanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-822">Every output parameter of a method must be definitely assigned before the method returns.</span></span>

<span data-ttu-id="4c8df-823">Kısmi Yöntem ([kısmi Yöntemler](classes.md#partial-methods)) veya yineleyici ([yineleyiciler](classes.md#iterators)) olarak belirtilen bir yöntem çıkış parametrelerine sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-823">A method declared as a partial method ([Partial methods](classes.md#partial-methods)) or an iterator ([Iterators](classes.md#iterators)) cannot have output parameters.</span></span>

<span data-ttu-id="4c8df-824">Çıkış parametreleri genellikle birden çok dönüş değeri üreten yöntemlerde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-824">Output parameters are typically used in methods that produce multiple return values.</span></span> <span data-ttu-id="4c8df-825">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4c8df-825">For example:</span></span>
```csharp
using System;

class Test
{
    static void SplitPath(string path, out string dir, out string name) {
        int i = path.Length;
        while (i > 0) {
            char ch = path[i - 1];
            if (ch == '\\' || ch == '/' || ch == ':') break;
            i--;
        }
        dir = path.Substring(0, i);
        name = path.Substring(i);
    }

    static void Main() {
        string dir, name;
        SplitPath("c:\\Windows\\System\\hello.txt", out dir, out name);
        Console.WriteLine(dir);
        Console.WriteLine(name);
    }
}
```

<span data-ttu-id="4c8df-826">Örnek, çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-826">The example produces the output:</span></span>
```console
c:\Windows\System\
hello.txt
```

<span data-ttu-id="4c8df-827">`dir` ve `name` değişkenlerinin `SplitPath`geçirilmeden önce atanmadığını ve çağrının ardından kesinlikle atanmış olarak kabul edileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4c8df-827">Note that the `dir` and `name` variables can be unassigned before they are passed to `SplitPath`, and that they are considered definitely assigned following the call.</span></span>

#### <a name="parameter-arrays"></a><span data-ttu-id="4c8df-828">Parametre dizileri</span><span class="sxs-lookup"><span data-stu-id="4c8df-828">Parameter arrays</span></span>

<span data-ttu-id="4c8df-829">`params` değiştiricisi ile belirtilen bir parametre bir parametre dizisidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-829">A parameter declared with a `params` modifier is a parameter array.</span></span> <span data-ttu-id="4c8df-830">Bir biçimsel parametre listesi bir parametre dizisi içeriyorsa, listedeki son parametre olmalıdır ve tek boyutlu dizi türünde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-830">If a formal parameter list includes a parameter array, it must be the last parameter in the list and it must be of a single-dimensional array type.</span></span> <span data-ttu-id="4c8df-831">Örneğin, `string[]` ve `string[][]` türleri bir parametre dizisinin türü olarak kullanılabilir, ancak tür `string[,]` olamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-831">For example, the types `string[]` and `string[][]` can be used as the type of a parameter array, but the type `string[,]` can not.</span></span> <span data-ttu-id="4c8df-832">`params` değiştiricinin değiştiriciler `ref` ve `out`birleştirmek mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-832">It is not possible to combine the `params` modifier with the modifiers `ref` and `out`.</span></span>

<span data-ttu-id="4c8df-833">Bir parametre dizisi bağımsız değişkenlerin bir yöntem çağrısında iki yöntemden biriyle belirtilmesine izin verir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-833">A parameter array permits arguments to be specified in one of two ways in a method invocation:</span></span>

*  <span data-ttu-id="4c8df-834">Bir parametre dizisi için verilen bağımsız değişken, parametre dizisi türüne örtük olarak dönüştürülebilir ([örtük dönüştürmeler](conversions.md#implicit-conversions)) tek bir ifade olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-834">The argument given for a parameter array can be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="4c8df-835">Bu durumda, parametre dizisi tam olarak bir değer parametresi gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-835">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="4c8df-836">Alternatif olarak, çağırma parametre dizisi için sıfır veya daha fazla bağımsız değişken belirtebilir, burada her bağımsız değişken parametre dizisinin öğe türüne örtük olarak dönüştürülebilir ([örtük dönüştürmeler](conversions.md#implicit-conversions)) bir ifadedir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-836">Alternatively, the invocation can specify zero or more arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="4c8df-837">Bu durumda, çağırma parametre dizisi türünde bağımsız değişken sayısına karşılık gelen bir bir örnek oluşturur, dizi örneğinin öğelerini verilen bağımsız değişken değerleriyle başlatır ve gerçek olarak yeni oluşturulan dizi örneğini kullanır değişkendir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-837">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="4c8df-838">Bir çağırmada değişken sayıda bağımsız değişkene izin verilmesi dışında, bir parametre dizisi tam olarak aynı türdeki bir değer parametresine ([değer parametreleri](classes.md#value-parameters)) eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-838">Except for allowing a variable number of arguments in an invocation, a parameter array is precisely equivalent to a value parameter ([Value parameters](classes.md#value-parameters)) of the same type.</span></span>

<span data-ttu-id="4c8df-839">Örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-839">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params int[] args) {
        Console.Write("Array contains {0} elements:", args.Length);
        foreach (int i in args) 
            Console.Write(" {0}", i);
        Console.WriteLine();
    }

    static void Main() {
        int[] arr = {1, 2, 3};
        F(arr);
        F(10, 20, 30, 40);
        F();
    }
}
```
<span data-ttu-id="4c8df-840">çıktıyı üretir</span><span class="sxs-lookup"><span data-stu-id="4c8df-840">produces the output</span></span>
```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="4c8df-841">İlk `F` çağrısı, dizi `a` bir değer parametresi olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-841">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="4c8df-842">`F` ikinci çağırma, otomatik olarak verilen öğe değerleriyle dört öğeli bir `int[]` oluşturur ve bu dizi örneğini bir değer parametresi olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-842">The second invocation of `F` automatically creates a four-element `int[]` with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="4c8df-843">Benzer şekilde, üçüncü `F` çağrısı sıfır öğeli bir `int[]` oluşturur ve bu örneği bir değer parametresi olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-843">Likewise, the third invocation of `F` creates a zero-element `int[]` and passes that instance as a value parameter.</span></span> <span data-ttu-id="4c8df-844">İkinci ve üçüncü etkinleştirmeleri yazma ile tam olarak eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-844">The second and third invocations are precisely equivalent to writing:</span></span>
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

<span data-ttu-id="4c8df-845">Aşırı yükleme çözümlemesi gerçekleştirirken, parametre dizisi olan bir yöntem normal biçiminde veya genişletilmiş biçiminde ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-845">When performing overload resolution, a method with a parameter array may be applicable either in its normal form or in its expanded form ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="4c8df-846">Bir yöntemin genişletilmiş biçimi yalnızca, metodun normal formu geçerli değilse ve genişletilmiş formla aynı imzaya sahip geçerli bir yöntem aynı türde zaten bildirilmemiş ise kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-846">The expanded form of a method is available only if the normal form of the method is not applicable and only if an applicable method with the same signature as the expanded form is not already declared in the same type.</span></span>

<span data-ttu-id="4c8df-847">Örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-847">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params object[] a) {
        Console.WriteLine("F(object[])");
    }

    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object a0, object a1) {
        Console.WriteLine("F(object,object)");
    }

    static void Main() {
        F();
        F(1);
        F(1, 2);
        F(1, 2, 3);
        F(1, 2, 3, 4);
    }
}
```
<span data-ttu-id="4c8df-848">çıktıyı üretir</span><span class="sxs-lookup"><span data-stu-id="4c8df-848">produces the output</span></span>
```console
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

<span data-ttu-id="4c8df-849">Örnekte, bir parametre dizisi ile yönteminin mümkün olan genişletilmiş biçimlerinden ikisi, normal yöntemler olarak sınıfında zaten yer alır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-849">In the example, two of the possible expanded forms of the method with a parameter array are already included in the class as regular methods.</span></span> <span data-ttu-id="4c8df-850">Bu genişletilmiş formlar bu nedenle aşırı yükleme çözümlemesi gerçekleştirilirken değerlendirilmez ve ilk ve üçüncü yöntem etkinleştirmeleri bu nedenle normal yöntemleri seçer.</span><span class="sxs-lookup"><span data-stu-id="4c8df-850">These expanded forms are therefore not considered when performing overload resolution, and the first and third method invocations thus select the regular methods.</span></span> <span data-ttu-id="4c8df-851">Bir sınıf bir parametre dizisi olan bir yöntem bildiriyorsa, genişletilmiş formlardan bazılarını düzenli yöntemler olarak da içermesi yaygın olmayan bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-851">When a class declares a method with a parameter array, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="4c8df-852">Bunu yaparak, parametre dizisi olan bir yöntemin genişletilmiş formu çağrıldığında oluşan bir dizi örneğinin ayrılmasını önlemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-852">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a parameter array is invoked.</span></span>

<span data-ttu-id="4c8df-853">Bir parametre dizisinin türü `object[]`olduğunda, yöntemin normal biçimi ve tek bir `object` parametresi için expsona formu arasında olası bir belirsizlik oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-853">When the type of a parameter array is `object[]`, a potential ambiguity arises between the normal form of the method and the expended form for a single `object` parameter.</span></span> <span data-ttu-id="4c8df-854">Belirsizliğin nedeni, `object[]` `object`türüne örtülü olarak dönüştürülebilir bir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-854">The reason for the ambiguity is that an `object[]` is itself implicitly convertible to type `object`.</span></span> <span data-ttu-id="4c8df-855">Bununla birlikte, belirsizlik bir sorun değildir, ancak gerekirse bir atama eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-855">The ambiguity presents no problem, however, since it can be resolved by inserting a cast if needed.</span></span>

<span data-ttu-id="4c8df-856">Örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-856">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params object[] args) {
        foreach (object o in args) {
            Console.Write(o.GetType().FullName);
            Console.Write(" ");
        }
        Console.WriteLine();
    }

    static void Main() {
        object[] a = {1, "Hello", 123.456};
        object o = a;
        F(a);
        F((object)a);
        F(o);
        F((object[])o);
    }
}
```
<span data-ttu-id="4c8df-857">çıktıyı üretir</span><span class="sxs-lookup"><span data-stu-id="4c8df-857">produces the output</span></span>
```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="4c8df-858">`F`ilk ve son çağırma sürümünde, bağımsız değişken türünden parametre türüne örtük bir dönüştürme mevcut olduğu için `F` normal biçimi geçerlidir (her ikisi de `object[]`türündedir).</span><span class="sxs-lookup"><span data-stu-id="4c8df-858">In the first and last invocations of `F`, the normal form of `F` is applicable because an implicit conversion exists from the argument type to the parameter type (both are of type `object[]`).</span></span> <span data-ttu-id="4c8df-859">Bu nedenle, aşırı yükleme çözümlemesi normal `F`ve bağımsız değişken normal bir değer parametresi olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-859">Thus, overload resolution selects the normal form of `F`, and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="4c8df-860">İkinci ve üçüncü etkinleştirmeleri içinde, bağımsız değişken türünden parametre türüne örtük dönüştürme olmadığından `F` normal biçimi Uygulanamaz (tür `object` örtük olarak `object[]`türüne dönüştürülemez).</span><span class="sxs-lookup"><span data-stu-id="4c8df-860">In the second and third invocations, the normal form of `F` is not applicable because no implicit conversion exists from the argument type to the parameter type (type `object` cannot be implicitly converted to type `object[]`).</span></span> <span data-ttu-id="4c8df-861">Ancak, `F` genişletilmiş biçimi uygulanabilir, bu nedenle aşırı yükleme çözümlemesi tarafından seçilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-861">However, the expanded form of `F` is applicable, so it is selected by overload resolution.</span></span> <span data-ttu-id="4c8df-862">Sonuç olarak, tek öğeli bir `object[]`, çağrı tarafından oluşturulur ve dizideki tek öğe, verilen bağımsız değişken değeri (kendisi bir `object[]`başvurusu olan) ile başlatılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-862">As a result, a one-element `object[]` is created by the invocation, and the single element of the array is initialized with the given argument value (which itself is a reference to an `object[]`).</span></span>

### <a name="static-and-instance-methods"></a><span data-ttu-id="4c8df-863">Statik ve örnek yöntemleri</span><span class="sxs-lookup"><span data-stu-id="4c8df-863">Static and instance methods</span></span>

<span data-ttu-id="4c8df-864">Bir yöntem bildirimi `static` değiştirici içerdiğinde, bu yöntem statik bir yöntem olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-864">When a method declaration includes a `static` modifier, that method is said to be a static method.</span></span> <span data-ttu-id="4c8df-865">`static` değiştirici yoksa, yöntem bir örnek yöntemi olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-865">When no `static` modifier is present, the method is said to be an instance method.</span></span>

<span data-ttu-id="4c8df-866">Statik bir yöntem belirli bir örnek üzerinde çalışmaz ve statik bir yöntemde `this` başvurmak için derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-866">A static method does not operate on a specific instance, and it is a compile-time error to refer to `this` in a static method.</span></span>

<span data-ttu-id="4c8df-867">Bir örnek yöntemi bir sınıfın belirli bir örneği üzerinde çalışır ve bu örneğe `this` ([Bu erişim](expressions.md#this-access)) olarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-867">An instance method operates on a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="4c8df-868">Bir yönteme bir *member_access* ([üye erişimi](expressions.md#member-access)) `E.M`, `M` statik bir yöntem ise, `E` `M`içeren bir türü belirtmelidir ve `M` bir örnek yöntemi ise, `E` içeren bir türün örneğini belirtmelidir.`M`</span><span class="sxs-lookup"><span data-stu-id="4c8df-868">When a method is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static method, `E` must denote a type containing `M`, and if `M` is an instance method, `E` must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="4c8df-869">Statik ve örnek üyeleri arasındaki farklılıklar, [statik ve örnek üyelerinde](classes.md#static-and-instance-members)daha ayrıntılı bir şekilde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-869">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-methods"></a><span data-ttu-id="4c8df-870">Sanal yöntemler</span><span class="sxs-lookup"><span data-stu-id="4c8df-870">Virtual methods</span></span>

<span data-ttu-id="4c8df-871">Bir örnek yöntemi bildirimi `virtual` değiştiricisi içerdiğinde, bu yöntem sanal bir yöntem olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-871">When an instance method declaration includes a `virtual` modifier, that method is said to be a virtual method.</span></span> <span data-ttu-id="4c8df-872">`virtual` değiştirici yoksa, yöntemi sanal olmayan bir yöntem olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-872">When no `virtual` modifier is present, the method is said to be a non-virtual method.</span></span>

<span data-ttu-id="4c8df-873">Sanal olmayan bir yöntemin uygulanması değişmez: uygulamanın bildirildiği sınıfın bir örneğinde veya türetilmiş bir sınıfın örneği üzerinde çağrılmasından bağımsız olarak, uygulama aynı olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-873">The implementation of a non-virtual method is invariant: The implementation is the same whether the method is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="4c8df-874">Buna karşılık, sanal bir yöntemin uygulanması türetilmiş sınıflar tarafından değiştirilmiş olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-874">In contrast, the implementation of a virtual method can be superseded by derived classes.</span></span> <span data-ttu-id="4c8df-875">Devralınan bir sanal yöntemin uygulanmasını yerine geçen işlem, bu yöntemi ***geçersiz kılma*** ([geçersiz kılma yöntemleri](classes.md#override-methods)) olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-875">The process of superseding the implementation of an inherited virtual method is known as ***overriding*** that method ([Override methods](classes.md#override-methods)).</span></span>

<span data-ttu-id="4c8df-876">Bir sanal yöntem çağrısında, çağrının gerçekleştiği örneğin ***çalışma zamanı türü*** , çağrılacak gerçek Yöntem uygulamasını belirler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-876">In a virtual method invocation, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="4c8df-877">Sanal olmayan bir yöntem çağrısında, örneğin ***derleme zamanı türü*** belirleme faktörü olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-877">In a non-virtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span> <span data-ttu-id="4c8df-878">Kesin koşullarda `N` adlı bir yöntem, derleme zamanı türü `C` ve bir çalışma zamanı türü `R` olan bir örnek `A` bağımsız değişken listesi ile çağrıldığında (`R` `C` veya `C`sınıfından türetilen bir sınıf), çağrı aşağıdaki şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-878">In precise terms, when a method named `N` is invoked with an argument list `A` on an instance with a compile-time type `C` and a run-time type `R` (where `R` is either `C` or a class derived from `C`), the invocation is processed as follows:</span></span>

*  <span data-ttu-id="4c8df-879">İlk olarak, `C`, `N`ve `A`için aşırı yükleme çözümlemesi uygulanır ve `C`tarafından tanımlanan Yöntemler kümesinden `M` belirli bir yöntemi seçebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-879">First, overload resolution is applied to `C`, `N`, and `A`, to select a specific method `M` from the set of methods declared in and inherited by `C`.</span></span> <span data-ttu-id="4c8df-880">Bu, [Yöntem etkinleştirmeleri](expressions.md#method-invocations)bölümünde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-880">This is described in [Method invocations](expressions.md#method-invocations).</span></span>
*  <span data-ttu-id="4c8df-881">`M` sanal olmayan bir yöntem ise `M` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-881">Then, if `M` is a non-virtual method, `M` is invoked.</span></span>
*  <span data-ttu-id="4c8df-882">Aksi takdirde, `M` sanal bir yöntemdir ve `R` göre `M` en çok türetilen uygulama çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-882">Otherwise, `M` is a virtual method, and the most derived implementation of `M` with respect to `R` is invoked.</span></span>

<span data-ttu-id="4c8df-883">Bir sınıf tarafından tanımlanan veya devralan her sanal yöntem için, yönteminin bu sınıfa göre ***en çok türetilmiş bir uygulamasını*** vardır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-883">For every virtual method declared in or inherited by a class, there exists a ***most derived implementation*** of the method with respect to that class.</span></span> <span data-ttu-id="4c8df-884">Bir sanal yöntemin en fazla türetilmiş uygulamasının bir sınıfa göre `M` `R` aşağıdaki gibi belirlenir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-884">The most derived implementation of a virtual method `M` with respect to a class `R` is determined as follows:</span></span>

*  <span data-ttu-id="4c8df-885">`R`, `M`tanıtma `virtual` bildirimini içeriyorsa, bu `M`en çok türetilen uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-885">If `R` contains the introducing `virtual` declaration of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="4c8df-886">Aksi takdirde, `R` bir `M``override` içeriyorsa, bu `M`en çok türetilen uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-886">Otherwise, if `R` contains an `override` of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="4c8df-887">Aksi takdirde, `R` göre `M` en çok türetilen uygulama, `R`doğrudan taban sınıfına göre `M` en çok türetilen uygulamayla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-887">Otherwise, the most derived implementation of `M` with respect to `R` is the same as the most derived implementation of `M` with respect to the direct base class of `R`.</span></span>

<span data-ttu-id="4c8df-888">Aşağıdaki örnek, sanal ve sanal olmayan Yöntemler arasındaki farkları göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-888">The following example illustrates the differences between virtual and non-virtual methods:</span></span>
```csharp
using System;

class A
{
    public void F() { Console.WriteLine("A.F"); }

    public virtual void G() { Console.WriteLine("A.G"); }
}

class B: A
{
    new public void F() { Console.WriteLine("B.F"); }

    public override void G() { Console.WriteLine("B.G"); }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        a.F();
        b.F();
        a.G();
        b.G();
    }
}
```

<span data-ttu-id="4c8df-889">Örnekte, `A` sanal olmayan bir yöntem `F` ve sanal bir yöntem `G`tanıtır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-889">In the example, `A` introduces a non-virtual method `F` and a virtual method `G`.</span></span> <span data-ttu-id="4c8df-890">Sınıfı `B`, yeni bir sanal olmayan yöntem `F`tanıtır, bu nedenle devralınan `F`gizler ve ayrıca devralınan yöntem `G`geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-890">The class `B` introduces a new non-virtual method `F`, thus hiding the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="4c8df-891">Örnek, çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-891">The example produces the output:</span></span>
```console
A.F
B.F
B.G
B.G
```

<span data-ttu-id="4c8df-892">Deyimin `A.G`değil `B.G``a.G()` çağırdığına dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="4c8df-892">Notice that the statement `a.G()` invokes `B.G`, not `A.G`.</span></span> <span data-ttu-id="4c8df-893">Bunun nedeni, örneğin çalışma zamanı türünün (yani `B`), örneğin derleme zamanı türü (`A`) değil, çağrılacak gerçek Yöntem uygulamasını belirler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-893">This is because the run-time type of the instance (which is `B`), not the compile-time type of the instance (which is `A`), determines the actual method implementation to invoke.</span></span>

<span data-ttu-id="4c8df-894">Yöntemlerin devralınan yöntemleri gizleyebildiğinden, bir sınıfın aynı imzaya sahip çeşitli sanal yöntemler içermesi mümkündür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-894">Because methods are allowed to hide inherited methods, it is possible for a class to contain several virtual methods with the same signature.</span></span> <span data-ttu-id="4c8df-895">Bu, bir belirsizlik sorunu sunmaz, ancak en çok türetilen Yöntem gizlenir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-895">This does not present an ambiguity problem, since all but the most derived method are hidden.</span></span> <span data-ttu-id="4c8df-896">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-896">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() { Console.WriteLine("A.F"); }
}

class B: A
{
    public override void F() { Console.WriteLine("B.F"); }
}

class C: B
{
    new public virtual void F() { Console.WriteLine("C.F"); }
}

class D: C
{
    public override void F() { Console.WriteLine("D.F"); }
}

class Test
{
    static void Main() {
        D d = new D();
        A a = d;
        B b = d;
        C c = d;
        a.F();
        b.F();
        c.F();
        d.F();
    }
}
```
<span data-ttu-id="4c8df-897">`C` ve `D` sınıfları aynı imzaya sahip iki sanal yöntem içerir: `A` ve `C`tarafından tanıtılan bir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-897">the `C` and `D` classes contain two virtual methods with the same signature: The one introduced by `A` and the one introduced by `C`.</span></span> <span data-ttu-id="4c8df-898">`C` tarafından tanıtılan Yöntem, `A`devralınmış yöntemi gizler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-898">The method introduced by `C` hides the method inherited from `A`.</span></span> <span data-ttu-id="4c8df-899">Bu nedenle `D` geçersiz kılma bildirimi, `C`tarafından tanıtılan yöntemi geçersiz kılar ve `D` `A`tarafından tanıtılan yöntemi geçersiz kılmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-899">Thus, the override declaration in `D` overrides the method introduced by `C`, and it is not possible for `D` to override the method introduced by `A`.</span></span> <span data-ttu-id="4c8df-900">Örnek, çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-900">The example produces the output:</span></span>
```console
B.F
B.F
D.F
D.F
```

<span data-ttu-id="4c8df-901">Bir `D` örneğine, yöntemin gizlenmediği daha az türetilmiş bir tür aracılığıyla erişerek gizli sanal yöntemi çağırmak mümkün olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4c8df-901">Note that it is possible to invoke the hidden virtual method by accessing an instance of `D` through a less derived type in which the method is not hidden.</span></span>

### <a name="override-methods"></a><span data-ttu-id="4c8df-902">Geçersiz kılma yöntemleri</span><span class="sxs-lookup"><span data-stu-id="4c8df-902">Override methods</span></span>

<span data-ttu-id="4c8df-903">Bir örnek yöntemi bildirimi `override` değiştiricisi içerdiğinde, yöntemi bir ***geçersiz kılma yöntemi***olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-903">When an instance method declaration includes an `override` modifier, the method is said to be an ***override method***.</span></span> <span data-ttu-id="4c8df-904">Bir geçersiz kılma yöntemi, aynı imzaya sahip devralınmış bir sanal yöntemi geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-904">An override method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="4c8df-905">Sanal bir yöntem bildiriminde yeni bir yöntem tanıtıldığı halde, bir geçersiz kılma yöntemi bildirimi, bu yöntemin yeni bir uygulamasını sağlayarak, var olan bir devralınmış sanal yöntemi uzmanlık eder.</span><span class="sxs-lookup"><span data-stu-id="4c8df-905">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="4c8df-906">Bir `override` bildirimi tarafından geçersiz kılınan yöntem, ***geçersiz kılınan temel yöntem***olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-906">The method overridden by an `override` declaration is known as the ***overridden base method***.</span></span> <span data-ttu-id="4c8df-907">Bir sınıf `C`bildirildiği `M` geçersiz kılma yöntemi için, geçersiz kılınan taban yöntemi, `C` doğrudan temel sınıf türünden başlayıp, her bir ardışık doğrudan temel sınıf türüne devam eden her bir temel sınıf türü `C`inceleyerek belirlenir. Bu, belirli bir temel sınıf türüne göre aynı imzaya sahip olan en az bir erişilebilir Yöntem bulunduğundan, tür bağımsız değişkenlerinin yerine `M` aynı imzaya sahip olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-907">For an override method `M` declared in a class `C`, the overridden base method is determined by examining each base class type of `C`, starting with the direct base class type of `C` and continuing with each successive direct base class type, until in a given base class type at least one accessible method is located which has the same signature as `M` after substitution of type arguments.</span></span> <span data-ttu-id="4c8df-908">Geçersiz kılınan temel yöntemi bulma amaçları doğrultusunda, bir yöntem, `protected`, `protected internal`ise veya `internal` ve aynı programda `C`olarak bildirilirse `public`erişilebilir olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-908">For the purposes of locating the overridden base method, a method is considered accessible if it is `public`, if it is `protected`, if it is `protected internal`, or if it is `internal` and declared in the same program as `C`.</span></span>

<span data-ttu-id="4c8df-909">Bir geçersiz kılma bildirimi için aşağıdakilerin tümü doğru değilse bir derleme zamanı hatası oluşur:</span><span class="sxs-lookup"><span data-stu-id="4c8df-909">A compile-time error occurs unless all of the following are true for an override declaration:</span></span>

*  <span data-ttu-id="4c8df-910">Geçersiz kılınan bir temel yöntem yukarıda açıklandığı gibi bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-910">An overridden base method can be located as described above.</span></span>
*  <span data-ttu-id="4c8df-911">Yalnızca bir tür geçersiz kılınan temel yöntem vardır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-911">There is exactly one such overridden base method.</span></span> <span data-ttu-id="4c8df-912">Bu kısıtlama yalnızca temel sınıf türü, tür bağımsız değişkenlerinin yerine geçen iki yöntemin imzasını yaptığı oluşturulmuş bir tür ise etkindir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-912">This restriction has effect only if the base class type is a constructed type where the substitution of type arguments makes the signature of two methods the same.</span></span>
*  <span data-ttu-id="4c8df-913">Geçersiz kılınan taban yöntemi bir sanal, Özet veya geçersiz kılma yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-913">The overridden base method is a virtual, abstract, or override method.</span></span> <span data-ttu-id="4c8df-914">Diğer bir deyişle, geçersiz kılınan taban yöntemi statik veya sanal olmayan olamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-914">In other words, the overridden base method cannot be static or non-virtual.</span></span>
*  <span data-ttu-id="4c8df-915">Geçersiz kılınan taban yöntemi Sealed bir yöntem değil.</span><span class="sxs-lookup"><span data-stu-id="4c8df-915">The overridden base method is not a sealed method.</span></span>
*  <span data-ttu-id="4c8df-916">Override yöntemi ve geçersiz kılınan taban yöntemi aynı dönüş türüne sahip.</span><span class="sxs-lookup"><span data-stu-id="4c8df-916">The override method and the overridden base method have the same return type.</span></span>
*  <span data-ttu-id="4c8df-917">Geçersiz kılma bildirimi ve geçersiz kılınan taban yöntemi, aynı tanımlanmış erişilebilirliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-917">The override declaration and the overridden base method have the same declared accessibility.</span></span> <span data-ttu-id="4c8df-918">Diğer bir deyişle, geçersiz kılma bildirimi sanal yöntemin erişilebilirliğini değiştiremezler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-918">In other words, an override declaration cannot change the accessibility of the virtual method.</span></span> <span data-ttu-id="4c8df-919">Ancak, geçersiz kılınan temel yöntem, iç, geçersiz kılma yöntemini içeren derlemeden farklı bir derlemede bildirilirse, geçersiz kılma yönteminin tanımlanmış erişilebilirlik korunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-919">However, if the overridden base method is protected internal and it is declared in a different assembly than the assembly containing the override method then the override method's declared accessibility must be protected.</span></span>
*  <span data-ttu-id="4c8df-920">Geçersiz kılma bildirimi tür-parametresi-kısıtlamalar-tümceleri belirtmiyor.</span><span class="sxs-lookup"><span data-stu-id="4c8df-920">The override declaration does not specify type-parameter-constraints-clauses.</span></span> <span data-ttu-id="4c8df-921">Bunun yerine, kısıtlamalar geçersiz kılınan temel yöntemden devralınır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-921">Instead the constraints are inherited from the overridden base method.</span></span> <span data-ttu-id="4c8df-922">Geçersiz kılınan yöntemde tür parametreleri olan kısıtlamaların devralınan kısıtlamadaki tür bağımsız değişkenleriyle değiştirilmesini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4c8df-922">Note that constraints that are type parameters in the overridden method may be replaced by type arguments in the inherited constraint.</span></span> <span data-ttu-id="4c8df-923">Bu, değer türleri veya korumalı türler gibi açıkça belirtildiğinde yasal olmayan kısıtlamalara yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-923">This can lead to constraints that are not legal when explicitly specified, such as value types or sealed types.</span></span>

<span data-ttu-id="4c8df-924">Aşağıdaki örnek, geçersiz kılma kurallarının genel sınıflar için nasıl çalıştığını göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-924">The following example demonstrates how the overriding rules work for generic classes:</span></span>
```csharp
abstract class C<T>
{
    public virtual T F() {...}
    public virtual C<T> G() {...}
    public virtual void H(C<T> x) {...}
}

class D: C<string>
{
    public override string F() {...}            // Ok
    public override C<string> G() {...}         // Ok
    public override void H(C<T> x) {...}        // Error, should be C<string>
}

class E<T,U>: C<U>
{
    public override U F() {...}                 // Ok
    public override C<U> G() {...}              // Ok
    public override void H(C<T> x) {...}        // Error, should be C<U>
}
```

<span data-ttu-id="4c8df-925">Geçersiz kılma bildirimi, geçersiz kılınan temel yönteme *base_access* ([taban erişimi](expressions.md#base-access)) ile erişebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-925">An override declaration can access the overridden base method using a *base_access* ([Base access](expressions.md#base-access)).</span></span> <span data-ttu-id="4c8df-926">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-926">In the example</span></span>
```csharp
class A
{
    int x;

    public virtual void PrintFields() {
        Console.WriteLine("x = {0}", x);
    }
}

class B: A
{
    int y;

    public override void PrintFields() {
        base.PrintFields();
        Console.WriteLine("y = {0}", y);
    }
}
```
<span data-ttu-id="4c8df-927">`B` `base.PrintFields()` çağrısı, `A`içinde belirtilen `PrintFields` yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-927">the `base.PrintFields()` invocation in `B` invokes the `PrintFields` method declared in `A`.</span></span> <span data-ttu-id="4c8df-928">*Base_access* sanal çağırma mekanizmasını devre dışı bırakır ve temel yöntemi sanal olmayan bir yöntem olarak değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-928">A *base_access* disables the virtual invocation mechanism and simply treats the base method as a non-virtual method.</span></span> <span data-ttu-id="4c8df-929">`B` çağrısı vardı `((A)this).PrintFields()`, `A`sanal olduğundan ve `PrintFields` çalışma zamanı türü `((A)this)` olduğundan, `B`içinde bildirildiği `PrintFields` yöntemini yinelemeli olarak çağırır.`B`</span><span class="sxs-lookup"><span data-stu-id="4c8df-929">Had the invocation in `B` been written `((A)this).PrintFields()`, it would recursively invoke the `PrintFields` method declared in `B`, not the one declared in `A`, since `PrintFields` is virtual and the run-time type of `((A)this)` is `B`.</span></span>

<span data-ttu-id="4c8df-930">Yalnızca bir `override` değiştiricisi ekleyerek bir yöntem başka bir yöntemi geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-930">Only by including an `override` modifier can a method override another method.</span></span> <span data-ttu-id="4c8df-931">Diğer tüm durumlarda, devralınan bir yöntemle aynı imzaya sahip bir yöntem devralınan yöntemi gizler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-931">In all other cases, a method with the same signature as an inherited method simply hides the inherited method.</span></span> <span data-ttu-id="4c8df-932">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-932">In the example</span></span>
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    public virtual void F() {}        // Warning, hiding inherited F()
}
```
<span data-ttu-id="4c8df-933">`B` `F` yöntemi bir `override` değiştiricisi içermez ve bu nedenle `A``F` yöntemini geçersiz kılmaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-933">the `F` method in `B` does not include an `override` modifier and therefore does not override the `F` method in `A`.</span></span> <span data-ttu-id="4c8df-934">Bunun yerine, `B` `F` yöntemi `A`yöntemi gizler ve bildirim bir `new` değiştiricisi içermediğinden bir uyarı bildirilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-934">Rather, the `F` method in `B` hides the method in `A`, and a warning is reported because the declaration does not include a `new` modifier.</span></span>

<span data-ttu-id="4c8df-935">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-935">In the example</span></span>
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    new private void F() {}        // Hides A.F within body of B
}

class C: B
{
    public override void F() {}    // Ok, overrides A.F
}
```
<span data-ttu-id="4c8df-936">`B` `F` yöntemi `A`devralınan sanal `F` yöntemini gizler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-936">the `F` method in `B` hides the virtual `F` method inherited from `A`.</span></span> <span data-ttu-id="4c8df-937">`B` içindeki yeni `F` özel erişime sahip olduğundan, kapsamı yalnızca `B` sınıf gövdesini içerir ve `C`olarak genişlemez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-937">Since the new `F` in `B` has private access, its scope only includes the class body of `B` and does not extend to `C`.</span></span> <span data-ttu-id="4c8df-938">Bu nedenle, `C` `F` bildiriminin `A`devralınmış `F` geçersiz kılmasına izin verilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-938">Therefore, the declaration of `F` in `C` is permitted to override the `F` inherited from `A`.</span></span>

### <a name="sealed-methods"></a><span data-ttu-id="4c8df-939">Sealed yöntemleri</span><span class="sxs-lookup"><span data-stu-id="4c8df-939">Sealed methods</span></span>

<span data-ttu-id="4c8df-940">Bir örnek yöntemi bildirimi `sealed` değiştiricisi içerdiğinde, bu yöntem ***Sealed bir yöntem***olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-940">When an instance method declaration includes a `sealed` modifier, that method is said to be a ***sealed method***.</span></span> <span data-ttu-id="4c8df-941">Bir örnek yöntemi bildirimi `sealed` değiştiricisini içeriyorsa, `override` değiştiricisini de içermelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-941">If an instance method declaration includes the  `sealed` modifier, it must also include the `override` modifier.</span></span> <span data-ttu-id="4c8df-942">`sealed` değiştiricisinin kullanımı, türetilmiş bir sınıfın yöntemi daha fazla geçersiz kılmasını önler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-942">Use of the `sealed` modifier prevents a derived class from further overriding the method.</span></span>

<span data-ttu-id="4c8df-943">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-943">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }

    public virtual void G() {
        Console.WriteLine("A.G");
    }
}

class B: A
{
    sealed override public void F() {
        Console.WriteLine("B.F");
    } 

    override public void G() {
        Console.WriteLine("B.G");
    } 
}

class C: B
{
    override public void G() {
        Console.WriteLine("C.G");
    } 
}
```
<span data-ttu-id="4c8df-944">`B` sınıfı iki geçersiz kılma yöntemi sağlar: `sealed` değiştiricisi ve olmayan bir `G` yöntemi olan bir `F` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4c8df-944">the class `B` provides two override methods: an `F` method that has the `sealed` modifier and a `G` method that does not.</span></span> <span data-ttu-id="4c8df-945">`B`Sealed `modifier` kullanımı, `C` `F`üzerine geçersiz kılmayı önler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-945">`B`'s use of the sealed `modifier` prevents `C` from further overriding `F`.</span></span>

### <a name="abstract-methods"></a><span data-ttu-id="4c8df-946">Soyut yöntemler</span><span class="sxs-lookup"><span data-stu-id="4c8df-946">Abstract methods</span></span>

<span data-ttu-id="4c8df-947">Bir örnek yöntemi bildirimi `abstract` değiştiricisi içerdiğinde, bu yöntem ***soyut bir yöntem***olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-947">When an instance method declaration includes an `abstract` modifier, that method is said to be an ***abstract method***.</span></span> <span data-ttu-id="4c8df-948">Soyut bir yöntem örtülü olarak bir sanal yöntem olsa da, `virtual`değiştiriciye sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-948">Although an abstract method is implicitly also a virtual method, it cannot have the modifier `virtual`.</span></span>

<span data-ttu-id="4c8df-949">Soyut yöntem bildirimi yeni bir sanal yöntem tanıtır, ancak bu yöntemin bir uygulamasını sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-949">An abstract method declaration introduces a new virtual method but does not provide an implementation of that method.</span></span> <span data-ttu-id="4c8df-950">Bunun yerine, soyut olmayan türetilmiş sınıfların bu yöntemi geçersiz kılarak kendi uygulamasını sağlaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-950">Instead, non-abstract derived classes are required to provide their own implementation by overriding that method.</span></span> <span data-ttu-id="4c8df-951">Soyut bir yöntem gerçek uygulama sunmadığından, soyut bir yöntemin *method_body* noktalı virgülden oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-951">Because an abstract method provides no actual implementation, the *method_body* of an abstract method simply consists of a semicolon.</span></span>

<span data-ttu-id="4c8df-952">Soyut yöntem bildirimlerine yalnızca soyut sınıflarda izin verilir ([soyut sınıflar](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-952">Abstract method declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="4c8df-953">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-953">In the example</span></span>
```csharp
public abstract class Shape
{
    public abstract void Paint(Graphics g, Rectangle r);
}

public class Ellipse: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawEllipse(r);
    }
}

public class Box: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawRect(r);
    }
}
```
<span data-ttu-id="4c8df-954">`Shape` sınıfı, kendisini boyayacak bir geometrik şekil nesnesinin soyut kavramını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-954">the `Shape` class defines the abstract notion of a geometrical shape object that can paint itself.</span></span> <span data-ttu-id="4c8df-955">Anlamlı bir varsayılan uygulama olmadığından `Paint` yöntemi soyuttur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-955">The `Paint` method is abstract because there is no meaningful default implementation.</span></span> <span data-ttu-id="4c8df-956">`Ellipse` ve `Box` sınıfları somut `Shape` uygulamalarıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-956">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="4c8df-957">Bu sınıflar soyut olmadığından, `Paint` yönteminin geçersiz kılınması ve gerçek bir uygulama sağlaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-957">Because these classes are non-abstract, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="4c8df-958">Bir soyut metoda başvurmak için bir *base_access* ([taban erişimi](expressions.md#base-access)) için derleme zamanı hatası.</span><span class="sxs-lookup"><span data-stu-id="4c8df-958">It is a compile-time error for a *base_access* ([Base access](expressions.md#base-access)) to reference an abstract method.</span></span> <span data-ttu-id="4c8df-959">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-959">In the example</span></span>
```csharp
abstract class A
{
    public abstract void F();
}

class B: A
{
    public override void F() {
        base.F();                        // Error, base.F is abstract
    }
}
```
<span data-ttu-id="4c8df-960">bir soyut metoda başvurduğundan `base.F()` çağrısı için derleme zamanı hatası bildirilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-960">a compile-time error is reported for the `base.F()` invocation because it references an abstract method.</span></span>

<span data-ttu-id="4c8df-961">Bir soyut Yöntem bildiriminin sanal bir yöntemi geçersiz kılmasına izin verilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-961">An abstract method declaration is permitted to override a virtual method.</span></span> <span data-ttu-id="4c8df-962">Bu, bir soyut sınıfın türetilmiş sınıflarda yöntemin yeniden uygulanmasını zormasına olanak tanır ve yöntemin orijinal uygulamasını kullanılamaz hale getirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-962">This allows an abstract class to force re-implementation of the method in derived classes, and makes the original implementation of the method unavailable.</span></span> <span data-ttu-id="4c8df-963">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-963">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }
}

abstract class B: A
{
    public abstract override void F();
}

class C: B
{
    public override void F() {
        Console.WriteLine("C.F");
    }
}
```
<span data-ttu-id="4c8df-964">sınıf `A` bir sanal yöntem bildirir, sınıf `B` bu yöntemi soyut bir yöntemle geçersiz kılar ve sınıf `C` kendi uygulamasını sağlamak için soyut yöntemi geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-964">class `A` declares a virtual method, class `B` overrides this method with an abstract method, and class `C` overrides the abstract method to provide its own implementation.</span></span>

### <a name="external-methods"></a><span data-ttu-id="4c8df-965">Dış Yöntemler</span><span class="sxs-lookup"><span data-stu-id="4c8df-965">External methods</span></span>

<span data-ttu-id="4c8df-966">Bir yöntem bildirimi `extern` değiştiricisi içerdiğinde, bu yöntem bir ***dış yöntem***olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-966">When a method declaration includes an `extern` modifier, that method is said to be an ***external method***.</span></span> <span data-ttu-id="4c8df-967">Dış Yöntemler, genellikle dışında C#bir dil kullanılarak dışarıdan uygulanır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-967">External methods are implemented externally, typically using a language other than C#.</span></span> <span data-ttu-id="4c8df-968">Bir dış yöntem bildirimi gerçek uygulama sunmadığından, bir dış yöntemin *method_body* noktalı virgülden oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-968">Because an external method declaration provides no actual implementation, the *method_body* of an external method simply consists of a semicolon.</span></span> <span data-ttu-id="4c8df-969">Dış yöntem genel olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-969">An external method may not be generic.</span></span>

<span data-ttu-id="4c8df-970">`extern` değiştirici, genellikle bir `DllImport` özniteliğiyle birlikte kullanılır ([com ve Win32 bileşenleriyle birlikte çalışabilirlik](attributes.md#interoperation-with-com-and-win32-components)) ve dış yöntemlerin dll 'ler tarafından uygulanmasına izin verir (dinamik bağlantı kitaplıkları).</span><span class="sxs-lookup"><span data-stu-id="4c8df-970">The `extern` modifier is typically used in conjunction with a `DllImport` attribute ([Interoperation with COM and Win32 components](attributes.md#interoperation-with-com-and-win32-components)), allowing external methods to be implemented by DLLs (Dynamic Link Libraries).</span></span> <span data-ttu-id="4c8df-971">Yürütme ortamı, dış yöntemlerin uygulamalarının sağlanabildiği diğer mekanizmaları destekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-971">The execution environment may support other mechanisms whereby implementations of external methods can be provided.</span></span>

<span data-ttu-id="4c8df-972">Bir dış yöntem `DllImport` özniteliği içerdiğinde, yöntem bildirimi de bir `static` değiştiricisi içermelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-972">When an external method includes a `DllImport` attribute, the method declaration must also include a `static` modifier.</span></span> <span data-ttu-id="4c8df-973">Bu örnek `extern` değiştiricisi ve `DllImport` özniteliği kullanımını gösterir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-973">This example demonstrates the use of the `extern` modifier and the `DllImport` attribute:</span></span>
```csharp
using System.Text;
using System.Security.Permissions;
using System.Runtime.InteropServices;

class Path
{
    [DllImport("kernel32", SetLastError=true)]
    static extern bool CreateDirectory(string name, SecurityAttribute sa);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool RemoveDirectory(string name);

    [DllImport("kernel32", SetLastError=true)]
    static extern int GetCurrentDirectory(int bufSize, StringBuilder buf);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool SetCurrentDirectory(string name);
}
```

### <a name="partial-methods-recap"></a><span data-ttu-id="4c8df-974">Kısmi Yöntemler (Recap)</span><span class="sxs-lookup"><span data-stu-id="4c8df-974">Partial methods (recap)</span></span>

<span data-ttu-id="4c8df-975">Bir yöntem bildirimi `partial` değiştirici içerdiğinde, bu yöntem ***kısmi bir yöntem***olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-975">When a method declaration includes a `partial` modifier, that method is said to be a ***partial method***.</span></span> <span data-ttu-id="4c8df-976">Kısmi yöntemler yalnızca kısmi türlerin ([kısmi türlerin](classes.md#partial-types)) üyeleri olarak bildirilebilecek ve bir dizi kısıtlamayla tabidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-976">Partial methods can only be declared as members of partial types ([Partial types](classes.md#partial-types)), and are subject to a number of restrictions.</span></span> <span data-ttu-id="4c8df-977">Kısmi Yöntemler, [kısmi yöntemlerde](classes.md#partial-methods)daha ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-977">Partial methods are further described in [Partial methods](classes.md#partial-methods).</span></span>

### <a name="extension-methods"></a><span data-ttu-id="4c8df-978">Uzantı yöntemleri</span><span class="sxs-lookup"><span data-stu-id="4c8df-978">Extension methods</span></span>

<span data-ttu-id="4c8df-979">Bir yöntemin ilk parametresi `this` değiştiricisini içerdiğinde, bu yöntem bir ***genişletme yöntemi***olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-979">When the first parameter of a method includes the `this` modifier, that method is said to be an ***extension method***.</span></span> <span data-ttu-id="4c8df-980">Uzantı yöntemleri yalnızca genel olmayan, iç içe olmayan statik sınıflarda bildirilemez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-980">Extension methods can only be declared in non-generic, non-nested static classes.</span></span> <span data-ttu-id="4c8df-981">Bir genişletme yönteminin ilk parametresi, `this`dışında bir değiştirici içeremez ve parametre türü bir işaretçi türü olamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-981">The first parameter of an extension method can have no modifiers other than `this`, and the parameter type cannot be a pointer type.</span></span>

<span data-ttu-id="4c8df-982">Aşağıda iki uzantı yöntemi bildiren bir statik sınıfa bir örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-982">The following is an example of a static class that declares two extension methods:</span></span>
```csharp
public static class Extensions
{
    public static int ToInt32(this string s) {
        return Int32.Parse(s);
    }

    public static T[] Slice<T>(this T[] source, int index, int count) {
        if (index < 0 || count < 0 || source.Length - index < count)
            throw new ArgumentException();
        T[] result = new T[count];
        Array.Copy(source, index, result, 0, count);
        return result;
    }
}
```

<span data-ttu-id="4c8df-983">Uzantı yöntemi, normal bir statik yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-983">An extension method is a regular static method.</span></span> <span data-ttu-id="4c8df-984">Ayrıca, kapsayan statik sınıfının kapsam içinde olduğu yerlerde, ilk bağımsız değişken olarak alıcı ifadesi kullanılarak örnek yöntemi çağırma sözdizimi ([genişletme yöntemi etkinleştirmeleri](expressions.md#extension-method-invocations)) kullanılarak bir genişletme yöntemi çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-984">In addition, where its enclosing static class is in scope, an extension method can be invoked using instance method invocation syntax ([Extension method invocations](expressions.md#extension-method-invocations)), using the receiver expression as the first argument.</span></span>

<span data-ttu-id="4c8df-985">Aşağıdaki program, yukarıda belirtilen uzantı yöntemlerini kullanır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-985">The following program uses the extension methods declared above:</span></span>
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in strings.Slice(1, 2)) {
            Console.WriteLine(s.ToInt32());
        }
    }
}
```

<span data-ttu-id="4c8df-986">`Slice` yöntemi `string[]`kullanılabilir ve `ToInt32` yöntemi uzantı yöntemleri olarak bildirildiği için `string`üzerinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-986">The `Slice` method is available on the `string[]`, and the `ToInt32` method is available on `string`, because they have been declared as extension methods.</span></span> <span data-ttu-id="4c8df-987">Programın anlamı, sıradan statik yöntem çağrıları kullanılarak aşağıdakiler ile aynıdır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-987">The meaning of the program is the same as the following, using ordinary static method calls:</span></span>
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in Extensions.Slice(strings, 1, 2)) {
            Console.WriteLine(Extensions.ToInt32(s));
        }
    }
}
```

### <a name="method-body"></a><span data-ttu-id="4c8df-988">Yöntem gövdesi</span><span class="sxs-lookup"><span data-stu-id="4c8df-988">Method body</span></span>

<span data-ttu-id="4c8df-989">Yöntem bildiriminin *method_body* bir blok gövdesinden, bir ifade gövdesinden veya noktalı virgülden oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-989">The *method_body* of a method declaration consists of either a block body, an expression body or a semicolon.</span></span>

<span data-ttu-id="4c8df-990">Bir yöntemin ***sonuç türü*** , dönüş türü `void`ise veya yöntem zaman uyumsuz ise ve dönüş türü `System.Threading.Tasks.Task`ise `void`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-990">The ***result type*** of a method is `void` if the return type is `void`, or if the method is async and the return type is `System.Threading.Tasks.Task`.</span></span> <span data-ttu-id="4c8df-991">Aksi takdirde, zaman uyumsuz bir yöntemin sonuç türü dönüş türüdür ve dönüş türü `System.Threading.Tasks.Task<T>` zaman uyumsuz bir metodun sonuç türü `T`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-991">Otherwise, the result type of a non-async method is its return type, and the result type of an async method with return type `System.Threading.Tasks.Task<T>` is `T`.</span></span>

<span data-ttu-id="4c8df-992">Bir yöntemde `void` sonuç türü ve bir blok gövdesi olduğunda, bloktaki `return` deyimlerinin ([return deyimi](statements.md#the-return-statement)) bir ifade belirtmelerine izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-992">When a method has a `void` result type and a block body, `return` statements ([The return statement](statements.md#the-return-statement)) in the block are not permitted to specify an expression.</span></span> <span data-ttu-id="4c8df-993">Void yönteminin bloğunun yürütülmesi normal şekilde tamamlanırsa (diğer bir deyişle, bu yöntem, Yöntem gövdesinin sonundaki şekilde akar), bu yöntem yalnızca geçerli çağıranına döner.</span><span class="sxs-lookup"><span data-stu-id="4c8df-993">If execution of the block of a void method completes normally (that is, control flows off the end of the method body), that method simply returns to its current caller.</span></span>
    
<span data-ttu-id="4c8df-994">Bir yöntemde `void` sonucu ve bir ifade gövdesi olduğunda, `E` ifadesi bir *statement_expression*olmalıdır ve gövde `{ E; }`bir blok gövdesiyle tam olarak eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-994">When a method has a `void` result and an expression body, the expression `E` must be a *statement_expression*, and the body is exactly equivalent to a block body of the form `{ E; }`.</span></span>
    
<span data-ttu-id="4c8df-995">Bir yöntemde void olmayan bir sonuç türü ve bir blok gövdesi olduğunda, bloktaki her `return` deyimi, sonuç türüne örtük olarak dönüştürülebilir bir ifade belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-995">When a method has a non-void result type and a block body, each `return` statement in the block must specify an expression that is implicitly convertible to the result type.</span></span> <span data-ttu-id="4c8df-996">Değer döndüren metodun blok gövdesinin uç noktasına ulaşılamıyor olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-996">The endpoint of a block body of a value-returning method must not be reachable.</span></span> <span data-ttu-id="4c8df-997">Diğer bir deyişle, blok gövdesi olan bir değer döndüren yöntemde, denetimin Yöntem gövdesinin sonunu akışa girmesine izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-997">In other words, in a value-returning method with a block body, control is not permitted to flow off the end of the method body.</span></span>
    
<span data-ttu-id="4c8df-998">Bir yöntemde void olmayan bir sonuç türü ve bir ifade gövdesi olduğunda, ifadenin sonuç türüne örtülü olarak dönüştürülebilir olması gerekir ve gövde `{ return E; }`bir blok gövdesiyle tam olarak eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-998">When a method has a non-void result type and an expression body, the expression must be implicitly convertible to the result type, and the body is exactly equivalent to a block body of the form `{ return E; }`.</span></span>
    
<span data-ttu-id="4c8df-999">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-999">In the example</span></span>
```csharp
class A
{
    public int F() {}            // Error, return value required

    public int G() {
        return 1;
    }

    public int H(bool b) {
        if (b) {
            return 1;
        }
        else {
            return 0;
        }
    }

    public int I(bool b) => b ? 1 : 0;
}
```
<span data-ttu-id="4c8df-1000">değer döndüren `F` yöntemi derleme zamanı hatası ile sonuçlanır çünkü denetim Yöntem gövdesinin sonuna akabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1000">the value-returning `F` method results in a compile-time error because control can flow off the end of the method body.</span></span> <span data-ttu-id="4c8df-1001">`G` ve `H` yöntemleri doğru olduğundan, tüm olası yürütme yolları bir dönüş değeri belirten dönüş ifadesinde sona erdir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1001">The `G` and `H` methods are correct because all possible execution paths end in a return statement that specifies a return value.</span></span> <span data-ttu-id="4c8df-1002">`I` yöntemi doğrudur, çünkü gövdesi yalnızca tek bir dönüş ifadesiyle bir ifade bloğuna denk gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1002">The `I` method is correct, because its body is equivalent to a statement block with just a single return statement in it.</span></span>

### <a name="method-overloading"></a><span data-ttu-id="4c8df-1003">Yöntem aşırı yüklemesi</span><span class="sxs-lookup"><span data-stu-id="4c8df-1003">Method overloading</span></span>

<span data-ttu-id="4c8df-1004">Yöntem aşırı yükleme çözümleme kuralları [tür çıkarımı](expressions.md#type-inference)bölümünde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1004">The method overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="properties"></a><span data-ttu-id="4c8df-1005">Özellikler</span><span class="sxs-lookup"><span data-stu-id="4c8df-1005">Properties</span></span>

<span data-ttu-id="4c8df-1006">***Özelliği*** , bir nesnenin veya sınıfın özelliklerine erişim sağlayan bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1006">A ***property*** is a member that provides access to a characteristic of an object or a class.</span></span> <span data-ttu-id="4c8df-1007">Özellik örnekleri, bir dizenin uzunluğunu, bir yazı tipinin boyutunu, bir pencerenin başlığını, bir müşterinin adını vb. içerir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1007">Examples of properties include the length of a string, the size of a font, the caption of a window, the name of a customer, and so on.</span></span> <span data-ttu-id="4c8df-1008">Özellikler, alanlar için doğal bir uzantıdır. her ikisi de ilişkili türleri olan üyeler adlandırılmış ve alanlara ve özelliklere erişim için sözdizimi aynıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1008">Properties are a natural extension of fields—both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="4c8df-1009">Ancak, alanların aksine, Özellikler depolama konumlarını göstermiyor.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1009">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="4c8df-1010">Bunun yerine, özellikler, değerleri okunmak veya yazıldığında yürütülecek deyimleri belirten ***erişimcileri*** vardır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1010">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span> <span data-ttu-id="4c8df-1011">Bu sayede, eylemleri bir nesne özniteliklerinin okuma ve yazma ile ilişkilendirmek için bir mekanizma sağlar; Ayrıca, bu tür özniteliklere de hesaplanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1011">Properties thus provide a mechanism for associating actions with the reading and writing of an object's attributes; furthermore, they permit such attributes to be computed.</span></span>

<span data-ttu-id="4c8df-1012">Özellikler *property_declaration*s kullanılarak bildirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1012">Properties are declared using *property_declaration*s:</span></span>

```antlr
property_declaration
    : attributes? property_modifier* type member_name property_body
    ;

property_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | property_modifier_unsafe
    ;

property_body
    : '{' accessor_declarations '}' property_initializer?
    | '=>' expression ';'
    ;

property_initializer
    : '=' variable_initializer ';'
    ;
```

<span data-ttu-id="4c8df-1013">Bir *property_declaration* bir *öznitelikler* kümesi ([öznitelikler](attributes.md)) ve dört erişim değiştiricisinin ([erişim değiştiricileri](classes.md#access-modifiers)) geçerli bir birleşimini, `new` ([Yeni değiştirici](classes.md#the-new-modifier)), `static` ([statik ve örnek yöntemleri](classes.md#static-and-instance-methods)), `virtual` ([sanal yöntemler](classes.md#virtual-methods)), `override` ([geçersiz kılma yöntemleri](classes.md#override-methods)), `sealed` ([korumalı](classes.md#sealed-methods)Yöntemler), `abstract` ([soyut yöntemler](classes.md#abstract-methods)) ve `extern` ([dış Yöntemler](classes.md#external-methods)) değiştiricilerini içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1013">A *property_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="4c8df-1014">Özellik bildirimleri, geçerli değiştiriciler birleşimleriyle ilgili olarak yöntem bildirimleri ([Yöntemler](classes.md#methods)) ile aynı kurallara tabidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1014">Property declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="4c8df-1015">Bir özellik bildiriminin *türü* , bildirim tarafından tanıtılan özelliğin türünü belirtir ve *MEMBER_NAME* özelliğin adını belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1015">The *type* of a property declaration specifies the type of the property introduced by the declaration, and the *member_name* specifies the name of the property.</span></span> <span data-ttu-id="4c8df-1016">Özellik açık bir arabirim üyesi uygulama değilse, *MEMBER_NAME* yalnızca bir *tanıtıcıdır*.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1016">Unless the property is an explicit interface member implementation, the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="4c8df-1017">Açık arabirim üye uygulaması ([Açık arabirim üye uygulamaları](interfaces.md#explicit-interface-member-implementations)) için *member_name* , bir *interface_type* ve ardından bir "`.`" ve bir *tanımlayıcı*oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1017">For an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="4c8df-1018">Özelliğin *türü* en az özelliğin kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1018">The *type* of a property must be at least as accessible as the property itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="4c8df-1019">Bir *property_body* ***erişimci gövdesinden*** ya da bir ***ifade gövdesinden***oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1019">A *property_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="4c8df-1020">Bir erişimci gövdesinde, "`{`" ve "`}`" belirteçlerinin içine alınması gereken *accessor_declarations*, özelliğin erişimcileri ([erişimcileri](classes.md#accessors)) ' sini bildirin.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1020">In an accessor body,  *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="4c8df-1021">Erişimciler, özelliği okuma ve yazma ile ilişkili yürütülebilir deyimleri belirler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1021">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="4c8df-1022">`=>` içeren bir ifade gövdesi ve ardından bir *ifade* `E` ve noktalı virgül, `{ get { return E; } }`deyim gövdesi ile tam olarak eşdeğerdir ve bu nedenle yalnızca alıcı sonucunun tek bir ifade tarafından verildiği yerde yalnızca alıcı özelliklerini belirtmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1022">An expression body consisting of `=>` followed by an *expression* `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only properties where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="4c8df-1023">Bir *property_initializer* yalnızca otomatik olarak uygulanan bir Özellik ([otomatik olarak uygulanan özellikler](classes.md#automatically-implemented-properties)) için verilebilir ve bu tür özelliklerin temel alan, *ifade*tarafından verilen değerle başlatılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1023">A *property_initializer* may only be given for an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)), and causes the initialization of the underlying field of such properties with the value given by the *expression*.</span></span>

<span data-ttu-id="4c8df-1024">Bir özelliğe erişim sözdizimi, bir alanla ilgili olarak aynı olsa da, bir özellik değişken olarak sınıflandırılmıyor.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1024">Even though the syntax for accessing a property is the same as that for a field, a property is not classified as a variable.</span></span> <span data-ttu-id="4c8df-1025">Bu nedenle, bir özelliği `ref` veya `out` bağımsız değişken olarak geçirmek mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1025">Thus, it is not possible to pass a property as a `ref` or `out` argument.</span></span>

<span data-ttu-id="4c8df-1026">Bir özellik bildirimi `extern` değiştirici içerdiğinde, özelliği bir ***dış Özellik***olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1026">When a property declaration includes an `extern` modifier, the property is said to be an ***external property***.</span></span> <span data-ttu-id="4c8df-1027">Dış özellik bildirimi gerçek uygulama sağladığından, *accessor_declarations* her biri noktalı virgülle oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1027">Because an external property declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

### <a name="static-and-instance-properties"></a><span data-ttu-id="4c8df-1028">Statik ve örnek özellikleri</span><span class="sxs-lookup"><span data-stu-id="4c8df-1028">Static and instance properties</span></span>

<span data-ttu-id="4c8df-1029">Bir özellik bildirimi `static` değiştirici içerdiğinde, özelliği ***statik bir özellik***olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1029">When a property declaration includes a `static` modifier, the property is said to be a ***static property***.</span></span> <span data-ttu-id="4c8df-1030">`static` değiştirici yoksa, özelliği bir ***örnek özelliği***olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1030">When no `static` modifier is present, the property is said to be an ***instance property***.</span></span>

<span data-ttu-id="4c8df-1031">Statik bir özellik belirli bir örnekle ilişkili değildir ve statik bir özelliğin erişimcilerine `this` başvurmak için bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1031">A static property is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static property.</span></span>

<span data-ttu-id="4c8df-1032">Örnek özelliği bir sınıfın belirli bir örneğiyle ilişkilendirilir ve bu örneğe bu özelliğin erişimcilerinde `this` ([Bu erişim](expressions.md#this-access)) olarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1032">An instance property is associated with a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that property.</span></span>

<span data-ttu-id="4c8df-1033">Bir özelliğe bir *member_access* ([üye erişimi](expressions.md#member-access)) `E.M`, `M` statik bir özellik ise, `E` `M`içeren bir türü belirtmelidir ve `M` bir örnek özellik ise, E içeren bir türün örneğini belirtmelidir.`M`</span><span class="sxs-lookup"><span data-stu-id="4c8df-1033">When a property is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static property, `E` must denote a type containing `M`, and if `M` is an instance property, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="4c8df-1034">Statik ve örnek üyeleri arasındaki farklılıklar, [statik ve örnek üyelerinde](classes.md#static-and-instance-members)daha ayrıntılı bir şekilde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1034">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="accessors"></a><span data-ttu-id="4c8df-1035">C</span><span class="sxs-lookup"><span data-stu-id="4c8df-1035">Accessors</span></span>

<span data-ttu-id="4c8df-1036">Bir özelliğin *accessor_declarations* , bu özelliği okuma ve yazma ile ilişkili çalıştırılabilir deyimleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1036">The *accessor_declarations* of a property specify the executable statements associated with reading and writing that property.</span></span>

```antlr
accessor_declarations
    : get_accessor_declaration set_accessor_declaration?
    | set_accessor_declaration get_accessor_declaration?
    ;

get_accessor_declaration
    : attributes? accessor_modifier? 'get' accessor_body
    ;

set_accessor_declaration
    : attributes? accessor_modifier? 'set' accessor_body
    ;

accessor_modifier
    : 'protected'
    | 'internal'
    | 'private'
    | 'protected' 'internal'
    | 'internal' 'protected'
    ;

accessor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="4c8df-1037">Erişimci bildirimleri *get_accessor_declaration*, *set_accessor_declaration*veya her ikisinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1037">The accessor declarations consist of a *get_accessor_declaration*, a *set_accessor_declaration*, or both.</span></span> <span data-ttu-id="4c8df-1038">Her erişimci bildirimi `get` belirteç `set` ve ardından isteğe bağlı *accessor_modifier* ve bir *accessor_body*oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1038">Each accessor declaration consists of the token `get` or `set` followed by an optional *accessor_modifier* and an *accessor_body*.</span></span>

<span data-ttu-id="4c8df-1039">*Accessor_modifier*s kullanımı aşağıdaki kısıtlamalara tabidir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1039">The use of *accessor_modifier*s is governed by the following restrictions:</span></span>

*  <span data-ttu-id="4c8df-1040">Bir *accessor_modifier* , bir arabirimde veya açık arabirim üyesi uygulamasında kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1040">An *accessor_modifier* may not be used in an interface or in an explicit interface member implementation.</span></span>
*  <span data-ttu-id="4c8df-1041">`override` değiştiriciye sahip olmayan bir özellik veya Dizin Oluşturucu için *accessor_modifier* , yalnızca özellik veya dizin oluşturucunun hem `get` hem de `set` erişimcisine sahip olması ve bu erişimcilerle yalnızca biri için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1041">For a property or indexer that has no `override` modifier, an *accessor_modifier* is permitted only if the property or indexer has both a `get` and `set` accessor, and then is permitted only on one of those accessors.</span></span>
*  <span data-ttu-id="4c8df-1042">`override` değiştiricisi içeren bir özellik veya Dizin Oluşturucu için, erişimci geçersiz kılınmakta olan erişimcinin *accessor_modifier*eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1042">For a property or indexer that includes an `override` modifier, an accessor must match the *accessor_modifier*, if any, of the accessor being overridden.</span></span>
*  <span data-ttu-id="4c8df-1043">*Accessor_modifier* , özelliğin veya dizin oluşturucunun kendisi tarafından belirtilen erişilebilirliğine göre kesinlikle daha kısıtlayıcı bir erişilebilirlik bildirmelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1043">The *accessor_modifier* must declare an accessibility that is strictly more restrictive than the declared accessibility of the property or indexer itself.</span></span> <span data-ttu-id="4c8df-1044">Kesin olması için:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1044">To be precise:</span></span>
   * <span data-ttu-id="4c8df-1045">Özelliğin veya dizin oluşturucunun `public`tanımlanmış bir erişilebilirliği varsa, *accessor_modifier* `protected internal`, `internal`, `protected`veya `private`olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1045">If the property or indexer has a declared accessibility of `public`, the *accessor_modifier* may be either `protected internal`, `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="4c8df-1046">Özelliğin veya dizin oluşturucunun `protected internal`tanımlanmış bir erişilebilirliği varsa, *accessor_modifier* `internal`, `protected`veya `private`olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1046">If the property or indexer has a declared accessibility of `protected internal`, the *accessor_modifier* may be either `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="4c8df-1047">Özelliğin veya dizin oluşturucunun `internal` veya `protected`tanımlanmış bir erişilebilirliği varsa, *accessor_modifier* `private`olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1047">If the property or indexer has a declared accessibility of `internal` or `protected`, the *accessor_modifier* must be `private`.</span></span>
   * <span data-ttu-id="4c8df-1048">Özelliğin veya dizin oluşturucunun `private`tanımlanmış bir erişilebilirliği varsa *accessor_modifier* kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1048">If the property or indexer has a declared accessibility of `private`, no *accessor_modifier* may be used.</span></span>

<span data-ttu-id="4c8df-1049">`abstract` ve `extern` özellikleri için, belirtilen her erişimci için *accessor_body* yalnızca noktalı virgül olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1049">For `abstract` and `extern` properties, the *accessor_body* for each accessor specified is simply a semicolon.</span></span> <span data-ttu-id="4c8df-1050">Soyut olmayan, extern olmayan bir özelliğin her bir *accessor_body* noktalı virgül olması olabilir, bu durumda ***otomatik olarak uygulanan bir özelliktir*** ([Otomatik uygulanan özellikler](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-1050">A non-abstract, non-extern property may have each *accessor_body* be a semicolon, in which case it is an ***automatically implemented property*** ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="4c8df-1051">Otomatik olarak uygulanan özelliğin en az bir get erişimcisi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1051">An automatically implemented property must have at least a get accessor.</span></span> <span data-ttu-id="4c8df-1052">Diğer soyut olmayan, extern olmayan özelliğin erişimcileri için *accessor_body* , karşılık gelen erişimci çağrıldığında yürütülecek deyimleri belirten bir *bloğudur* .</span><span class="sxs-lookup"><span data-stu-id="4c8df-1052">For the accessors of any other non-abstract, non-extern property, the *accessor_body* is a *block* which specifies the statements to be executed when the corresponding accessor is invoked.</span></span>

<span data-ttu-id="4c8df-1053">`get` erişimcisi, özellik türünün dönüş değeri olan parametresiz bir yönteme karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1053">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="4c8df-1054">Bir atamanın hedefi haricinde, bir ifadede bir özelliğe başvurulduğunda, özelliğin `get` erişimcisi özelliğin değerini hesaplamak için çağrılır ([Ifadelerin değerleri](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-1054">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property ([Values of expressions](expressions.md#values-of-expressions)).</span></span> <span data-ttu-id="4c8df-1055">`get` erişimcisinin gövdesi, [Yöntem gövdesinde](classes.md#method-body)açıklanan değer döndüren yöntemlere yönelik kurallara uymalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1055">The body of a `get` accessor must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="4c8df-1056">Özellikle, bir `get` erişimcisinin gövdesindeki tüm `return` deyimleri, özellik türüne örtük olarak dönüştürülebilir bir ifade belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1056">In particular, all `return` statements in the body of a `get` accessor must specify an expression that is implicitly convertible to the property type.</span></span> <span data-ttu-id="4c8df-1057">Ayrıca, bir `get` erişimcisinin uç noktasına ulaşılamamalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1057">Furthermore, the endpoint of a `get` accessor must not be reachable.</span></span>

<span data-ttu-id="4c8df-1058">`set` erişimcisi, özellik türünün tek değerli parametresine sahip bir yönteme ve `void` dönüş türüne karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1058">A `set` accessor corresponds to a method with a single value parameter of the property type and a `void` return type.</span></span> <span data-ttu-id="4c8df-1059">`set` erişimcisinin örtük parametresi her zaman `value`olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1059">The implicit parameter of a `set` accessor is always named `value`.</span></span> <span data-ttu-id="4c8df-1060">Bir atamanın hedefi olarak bir özelliğe başvurulduğunda ([atama işleçleri](expressions.md#assignment-operators)), ya da `++` ya da `--` Işleneni ([sonek artırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators), [ön ek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)) olarak, `set` erişimci bir bağımsız değişkenle çağrılır (değeri atamanın sağ tarafındaki veya `++` veya `--` işlecinin Işleneni olan) yeni değeri ([basit atama](expressions.md#simple-assignment)) sağlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1060">When a property is referenced as the target of an assignment ([Assignment operators](expressions.md#assignment-operators)), or as the operand of `++` or `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), the `set` accessor is invoked with an argument (whose value is that of the right-hand side of the assignment or the operand of the `++` or `--` operator) that provides the new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="4c8df-1061">`set` erişimcisinin gövdesi, [Yöntem gövdesinde](classes.md#method-body)açıklanan `void` yöntemleri için kurallara uymalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1061">The body of a `set` accessor must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="4c8df-1062">Özellikle, `set` erişimci gövdesindeki `return` deyimlerinin bir ifade belirtmelerine izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1062">In particular, `return` statements in the `set` accessor body are not permitted to specify an expression.</span></span> <span data-ttu-id="4c8df-1063">`set` erişimcisinde örtük olarak `value`adlı bir parametre bulunduğundan, bu ada sahip olması için bir `set` erişimcisindeki bir yerel değişken veya sabit bildirimin derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1063">Since a `set` accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declaration in a `set` accessor to have that name.</span></span>

<span data-ttu-id="4c8df-1064">`get` ve `set` erişimcilerinin varlığına veya yokluğuna göre, bir özellik şu şekilde sınıflandırılır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1064">Based on the presence or absence of the `get` and `set` accessors, a property is classified as follows:</span></span>

*  <span data-ttu-id="4c8df-1065">Hem bir `get` erişimcisi hem de bir `set` erişimcisi içeren bir özellik, ***okuma-yazma*** özelliği olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1065">A property that includes both a `get` accessor and a `set` accessor is said to be a ***read-write*** property.</span></span>
*  <span data-ttu-id="4c8df-1066">Yalnızca bir `get` erişimcisi olan bir özellik ***salt okunurdur*** özelliği olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1066">A property that has only a `get` accessor is said to be a ***read-only*** property.</span></span> <span data-ttu-id="4c8df-1067">Bir salt okuma özelliğinin bir atamanın hedefi olması için derleme zamanı hatası.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1067">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>
*  <span data-ttu-id="4c8df-1068">Yalnızca bir `set` erişimcisi olan bir özellik ***salt yazılır*** bir özellik olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1068">A property that has only a `set` accessor is said to be a ***write-only*** property.</span></span> <span data-ttu-id="4c8df-1069">Atama hedefi dışında, bir ifadede salt yazılır bir özelliğe başvurmak için derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1069">Except as the target of an assignment, it is a compile-time error to reference a write-only property in an expression.</span></span>

<span data-ttu-id="4c8df-1070">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-1070">In the example</span></span>
```csharp
public class Button: Control
{
    private string caption;

    public string Caption {
        get {
            return caption;
        }
        set {
            if (caption != value) {
                caption = value;
                Repaint();
            }
        }
    }

    public override void Paint(Graphics g, Rectangle r) {
        // Painting code goes here
    }
}
```
<span data-ttu-id="4c8df-1071">`Button` denetimi bir ortak `Caption` özelliği bildirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1071">the `Button` control declares a public `Caption` property.</span></span> <span data-ttu-id="4c8df-1072">`Caption` özelliğinin `get` erişimcisi, özel `caption` alanında depolanan dizeyi döndürür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1072">The `get` accessor of the `Caption` property returns the string stored in the private `caption` field.</span></span> <span data-ttu-id="4c8df-1073">`set` erişimci, yeni değerin geçerli değerden farklı olup olmadığını denetler ve bu durumda yeni değeri depolar ve denetimi yeniden boyar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1073">The `set` accessor checks if the new value is different from the current value, and if so, it stores the new value and repaints the control.</span></span> <span data-ttu-id="4c8df-1074">Özellikler genellikle yukarıda gösterilen kalıbı izler: `get` erişimci bir özel alanda depolanan bir değeri döndürür ve `set` erişimci bu özel alanı değiştirir ve ardından nesnenin durumunu tamamen güncelleştirmek için gereken ek eylemleri gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1074">Properties often follow the pattern shown above: The `get` accessor simply returns a value stored in a private field, and the `set` accessor modifies that private field and then performs any additional actions required to fully update the state of the object.</span></span>

<span data-ttu-id="4c8df-1075">Yukarıdaki `Button` sınıfı verildiğinde, `Caption` özelliğinin kullanım örneği aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1075">Given the `Button` class above, the following is an example of use of the `Caption` property:</span></span>
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

<span data-ttu-id="4c8df-1076">Burada, `set` erişimcisi özelliğe bir değer atanarak çağrılır ve `get` erişimci bir ifadede özelliğe başvuruda bulunarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1076">Here, the `set` accessor is invoked by assigning a value to the property, and the `get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="4c8df-1077">Bir özelliğin `get` ve `set` erişimcileri farklı Üyeler değildir ve bir özelliğin erişimcilerinin ayrı ayrı bildirilmesini mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1077">The `get` and `set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="4c8df-1078">Bu nedenle, bir okuma-yazma özelliğinin iki erişimcinin farklı erişilebilirliği olması mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1078">As such, it is not possible for the two accessors of a read-write property to have different accessibility.</span></span> <span data-ttu-id="4c8df-1079">Örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-1079">The example</span></span>
```csharp
class A
{
    private string name;

    public string Name {                // Error, duplicate member name
        get { return name; }
    }

    public string Name {                // Error, duplicate member name
        set { name = value; }
    }
}
```
<span data-ttu-id="4c8df-1080">tek bir okuma-yazma özelliği bildirmiyor.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1080">does not declare a single read-write property.</span></span> <span data-ttu-id="4c8df-1081">Bunun yerine, aynı ada sahip iki özellik bildirir, bir salt okunurdur ve bir salt yazılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1081">Rather, it declares two properties with the same name, one read-only and one write-only.</span></span> <span data-ttu-id="4c8df-1082">Aynı sınıfta belirtilen iki üye aynı ada sahip olmadığından, örnek bir derleme zamanı hatası oluşmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1082">Since two members declared in the same class cannot have the same name, the example causes a compile-time error to occur.</span></span>

<span data-ttu-id="4c8df-1083">Türetilmiş bir sınıf devralınan bir özellik ile aynı ada sahip bir özelliği bildiriyorsa, türetilen Özellik devralınan özelliği hem okuma hem de yazma açısından gizler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1083">When a derived class declares a property by the same name as an inherited property, the derived property hides the inherited property with respect to both reading and writing.</span></span> <span data-ttu-id="4c8df-1084">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-1084">In the example</span></span>
```csharp
class A
{
    public int P {
        set {...}
    }
}

class B: A
{
    new public int P {
        get {...}
    }
}
```
<span data-ttu-id="4c8df-1085">`B` `P` özelliği, hem okuma hem de yazma açısından `A` `P` özelliğini gizler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1085">the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing.</span></span> <span data-ttu-id="4c8df-1086">Bu nedenle, deyimlerde</span><span class="sxs-lookup"><span data-stu-id="4c8df-1086">Thus, in the statements</span></span>
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
<span data-ttu-id="4c8df-1087">`b.P` atama, `B` ' deki `P` salt yazılır `P` `A`özelliğini gizleyeceği için derleme zamanı hatası raporlanmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1087">the assignment to `b.P` causes a compile-time error to be reported, since the read-only `P` property in `B` hides the write-only `P` property in `A`.</span></span> <span data-ttu-id="4c8df-1088">Ancak, bu tür bir dönüştürmenin gizli `P` özelliğine erişmek için kullanılabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1088">Note, however, that a cast can be used to access the hidden `P` property.</span></span>

<span data-ttu-id="4c8df-1089">Ortak alanlardan farklı olarak, özellikler bir nesnenin iç durumu ile ortak arabirimi arasında bir ayrım sağlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1089">Unlike public fields, properties provide a separation between an object's internal state and its public interface.</span></span> <span data-ttu-id="4c8df-1090">Örneği göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1090">Consider the example:</span></span>
```csharp
class Label
{
    private int x, y;
    private string caption;

    public Label(int x, int y, string caption) {
        this.x = x;
        this.y = y;
        this.caption = caption;
    }

    public int X {
        get { return x; }
    }

    public int Y {
        get { return y; }
    }

    public Point Location {
        get { return new Point(x, y); }
    }

    public string Caption {
        get { return caption; }
    }
}
```

<span data-ttu-id="4c8df-1091">Burada `Label` sınıfı, konumunu depolamak için `x` ve `y`iki `int` alanı kullanır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1091">Here, the `Label` class uses two `int` fields, `x` and `y`, to store its location.</span></span> <span data-ttu-id="4c8df-1092">Konum, hem `X` hem de `Y` özellik olarak ve `Point`türünde `Location` özelliği olarak kullanıma sunulur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1092">The location is publicly exposed both as an `X` and a `Y` property and as a `Location` property of type `Point`.</span></span> <span data-ttu-id="4c8df-1093">`Label`gelecek bir sürümünde, bu, konumun dahili olarak `Point` olarak depolanması daha uygun hale gelirse, değişikliğin sınıfın genel arabirimini etkilemeden bu değişiklik yapılabilir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1093">If, in a future version of `Label`, it becomes more convenient to store the location as a `Point` internally, the change can be made without affecting the public interface of the class:</span></span>
```csharp
class Label
{
    private Point location;
    private string caption;

    public Label(int x, int y, string caption) {
        this.location = new Point(x, y);
        this.caption = caption;
    }

    public int X {
        get { return location.x; }
    }

    public int Y {
        get { return location.y; }
    }

    public Point Location {
        get { return location; }
    }

    public string Caption {
        get { return caption; }
    }
}
```

<span data-ttu-id="4c8df-1094">`x` ve `y` `public readonly` alan vardı, bu tür bir değişikliği `Label` sınıfında yapmak imkansız olurdu.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1094">Had `x` and `y` instead been `public readonly` fields, it would have been impossible to make such a change to the `Label` class.</span></span>

<span data-ttu-id="4c8df-1095">Durumu özellikler aracılığıyla göstermek, alanları doğrudan açığa çıkarmadan daha az verimlidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1095">Exposing state through properties is not necessarily any less efficient than exposing fields directly.</span></span> <span data-ttu-id="4c8df-1096">Özellikle, bir özellik sanal olmayan ve yalnızca küçük miktarda kod içerdiğinde, yürütme ortamı erişimcileri çağrılarını erişimcilerinin gerçek koduyla değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1096">In particular, when a property is non-virtual and contains only a small amount of code, the execution environment may replace calls to accessors with the actual code of the accessors.</span></span> <span data-ttu-id="4c8df-1097">Bu işlem, ***satır içi***olarak bilinir ve özellik erişimini alan erişimi olarak verimli hale getirir, ancak özelliklerin daha fazla esnekliğini korur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1097">This process is known as ***inlining***, and it makes property access as efficient as field access, yet preserves the increased flexibility of properties.</span></span>

<span data-ttu-id="4c8df-1098">Bir `get` erişimcisinin çağrılması bir alanın değerini okumanın kavramsal olarak eşdeğeri olduğundan, `get` erişimcilerinin observable yan etkileri olması için hatalı programlama stili olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1098">Since invoking a `get` accessor is conceptually equivalent to reading the value of a field, it is considered bad programming style for `get` accessors to have observable side-effects.</span></span> <span data-ttu-id="4c8df-1099">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-1099">In the example</span></span>
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
<span data-ttu-id="4c8df-1100">`Next` özelliğinin değeri, özelliğin daha önce erişilme sayısına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1100">the value of the `Next` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="4c8df-1101">Bu nedenle, özelliğe erişmek bir observable yan etkisi oluşturur ve özellik bunun yerine bir yöntem olarak uygulanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1101">Thus, accessing the property produces an observable side-effect, and the property should be implemented as a method instead.</span></span>

<span data-ttu-id="4c8df-1102">`get` erişimcileri için "yan etkileri yok" kuralı `get` erişimcilerin yalnızca alanlarda depolanan değerleri döndürmek için her zaman yazılması anlamına gelmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1102">The "no side-effects" convention for `get` accessors doesn't mean that `get` accessors should always be written to simply return values stored in fields.</span></span> <span data-ttu-id="4c8df-1103">Aslında `get` erişimcileri genellikle birden çok alana erişerek veya yöntemleri çağırarak bir özelliğin değerini hesaplar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1103">Indeed, `get` accessors often compute the value of a property by accessing multiple fields or invoking methods.</span></span> <span data-ttu-id="4c8df-1104">Ancak düzgün şekilde tasarlanan bir `get` erişimcisi nesnenin durumunda observable değişikliklerine neden olan hiçbir eylem gerçekleştirmiyor.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1104">However, a properly designed `get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="4c8df-1105">Özellikler, ilk kez başvuruluncaya kadar bir kaynağın başlatılmasını geciktirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1105">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="4c8df-1106">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1106">For example:</span></span>
```csharp
using System.IO;

public class Console
{
    private static TextReader reader;
    private static TextWriter writer;
    private static TextWriter error;

    public static TextReader In {
        get {
            if (reader == null) {
                reader = new StreamReader(Console.OpenStandardInput());
            }
            return reader;
        }
    }

    public static TextWriter Out {
        get {
            if (writer == null) {
                writer = new StreamWriter(Console.OpenStandardOutput());
            }
            return writer;
        }
    }

    public static TextWriter Error {
        get {
            if (error == null) {
                error = new StreamWriter(Console.OpenStandardError());
            }
            return error;
        }
    }
}
```

<span data-ttu-id="4c8df-1107">`Console` sınıfı, sırasıyla standart giriş, çıkış ve hata cihazlarını temsil eden üç özellik, `In`, `Out`ve `Error`içerir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1107">The `Console` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="4c8df-1108">Bu üyelerin özellikler olarak kullanıma sunulmasıyla `Console` sınıfı, gerçekten kullanılana kadar başlatma durumlarını erteleyebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1108">By exposing these members as properties, the `Console` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="4c8df-1109">Örneğin, ilk olarak `Out` özelliğine başvurulduğunda</span><span class="sxs-lookup"><span data-stu-id="4c8df-1109">For example, upon first referencing the `Out` property, as in</span></span>
```csharp
Console.Out.WriteLine("hello, world");
```
<span data-ttu-id="4c8df-1110">çıkış cihazının temel alınan `TextWriter` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1110">the underlying `TextWriter` for the output device is created.</span></span> <span data-ttu-id="4c8df-1111">Ancak uygulama `In` ve `Error` özelliklerine başvuru yapmaz, bu durumda bu cihazlar için hiçbir nesne oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1111">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="4c8df-1112">Otomatik uygulanan özellikler</span><span class="sxs-lookup"><span data-stu-id="4c8df-1112">Automatically implemented properties</span></span>

<span data-ttu-id="4c8df-1113">Otomatik olarak uygulanan bir Özellik (ya da Short için ***Otomatik Özellik*** ), salt noktalı erişimci gövdeleriyle soyut olmayan extern olmayan bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1113">An automatically implemented property (or ***auto-property*** for short), is a non-abstract non-extern property with semicolon-only accessor bodies.</span></span> <span data-ttu-id="4c8df-1114">Otomatik Özellikler bir get erişimcisine sahip olmalı ve isteğe bağlı olarak bir set erişimcisine sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1114">Auto-properties must have a get accessor and can optionally have a set accessor.</span></span>

<span data-ttu-id="4c8df-1115">Bir özellik otomatik olarak uygulanan bir özellik olarak belirtildiğinde, özelliği için otomatik olarak bir yedekleme alanı kullanılabilir ve erişimciler, bu yedekleme alanına okuma ve yazma için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1115">When a property is specified as an automatically implemented property, a hidden backing field is automatically available for the property, and the accessors are implemented to read from and write to that backing field.</span></span> <span data-ttu-id="4c8df-1116">Auto özelliğinin ayarlanmış bir erişimcisi yoksa, yedekleme alanı `readonly` ([salt okunur alanlar](classes.md#readonly-fields)) olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1116">If the auto-property has no set accessor, the backing field is considered `readonly` ([Readonly fields](classes.md#readonly-fields)).</span></span> <span data-ttu-id="4c8df-1117">Yalnızca bir `readonly` alanı gibi, kapsayan sınıfın oluşturucusunun gövdesinde yalnızca bir alıcı otomatik özelliği de atanabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1117">Just like a `readonly` field, a getter-only auto-property can also be assigned to in the body of a constructor of the enclosing class.</span></span> <span data-ttu-id="4c8df-1118">Böyle bir atama, doğrudan özelliğinin salt okunur yedekleme alanına atar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1118">Such an assignment assigns directly to the readonly backing field of the property.</span></span>

<span data-ttu-id="4c8df-1119">Otomatik Özellik isteğe bağlı olarak, doğrudan bir *variable_initializer* ([değişken başlatıcıları](classes.md#variable-initializers)) olarak yedekleme alanına uygulanan bir *property_initializer*olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1119">An auto-property may optionally have a *property_initializer*, which is applied directly to the backing field as a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)).</span></span>

<span data-ttu-id="4c8df-1120">Aşağıdaki örnek:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1120">The following example:</span></span>
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
<span data-ttu-id="4c8df-1121">aşağıdaki bildirime eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1121">is equivalent to the following declaration:</span></span>
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

<span data-ttu-id="4c8df-1122">Aşağıdaki örnek:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1122">The following example:</span></span>
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
<span data-ttu-id="4c8df-1123">aşağıdaki bildirime eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1123">is equivalent to the following declaration:</span></span>
```csharp
public class ReadOnlyPoint
{
    private readonly int __x;
    private readonly int __y;
    public int X { get { return __x; } }
    public int Y { get { return __y; } }
    public ReadOnlyPoint(int x, int y) { __x = x; __y = y; }
}
```

<span data-ttu-id="4c8df-1124">ReadOnly alanının atamalarının, Oluşturucu içinde gerçekleştikleri için geçerli olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1124">Notice that the assignments to the readonly field are legal, because they occur within the constructor.</span></span>


### <a name="accessibility"></a><span data-ttu-id="4c8df-1125">Erişilebilirlik</span><span class="sxs-lookup"><span data-stu-id="4c8df-1125">Accessibility</span></span>

<span data-ttu-id="4c8df-1126">Bir erişimcinin *accessor_modifier*varsa, erişimcinin erişilebilirlik etki alanı ([erişilebilirlik etki alanları](basic-concepts.md#accessibility-domains)) *accessor_modifier*belirtilen erişilebilirliği kullanılarak belirlenir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1126">If an accessor has an *accessor_modifier*, the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the accessor is determined using the declared accessibility of the *accessor_modifier*.</span></span> <span data-ttu-id="4c8df-1127">Bir erişimcinin *accessor_modifier*yoksa, erişimcinin erişilebilirlik etki alanı, özelliğin veya dizin oluşturucunun tanımlanmış erişilebilirliğine göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1127">If an accessor does not have an *accessor_modifier*, the accessibility domain of the accessor is determined from the declared accessibility of the property or indexer.</span></span>

<span data-ttu-id="4c8df-1128">*Accessor_modifier* varlığı, üye aramasını ([işleçler](expressions.md#operators)) veya aşırı yükleme çözümünü ([aşırı yükleme çözümlemesi](expressions.md#overload-resolution)) hiçbir şekilde etkilemez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1128">The presence of an *accessor_modifier* never affects member lookup ([Operators](expressions.md#operators)) or overload resolution ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="4c8df-1129">Özellik veya dizin oluşturucudaki değiştiriciler her zaman erişim bağlamından bağımsız olarak hangi özelliğin veya dizin oluşturucunun bağlı olduğunu belirlenir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1129">The modifiers on the property or indexer always determine which property or indexer is bound to, regardless of the context of the access.</span></span>

<span data-ttu-id="4c8df-1130">Belirli bir özellik veya Dizin Oluşturucu seçildikten sonra, bu kullanımın geçerli olup olmadığını anlamak için ilgili erişimcilerinin erişilebilirlik etki alanları kullanılır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1130">Once a particular property or indexer has been selected, the accessibility domains of the specific accessors involved are used to determine if that usage is valid:</span></span>

*  <span data-ttu-id="4c8df-1131">Kullanım bir değer ([Ifadelerin değerleri](expressions.md#values-of-expressions)) ise, `get` erişimcisi bulunmalı ve erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1131">If the usage is as a value ([Values of expressions](expressions.md#values-of-expressions)), the `get` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="4c8df-1132">Kullanım basit bir atamanın hedefi ([basit atama](expressions.md#simple-assignment)) ise, `set` erişimcisi bulunmalı ve erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1132">If the usage is as the target of a simple assignment ([Simple assignment](expressions.md#simple-assignment)), the `set` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="4c8df-1133">Kullanım, bileşik atamanın hedefi ([bileşik atama](expressions.md#compound-assignment)) veya `++` ya da `--` işleçlerinin hedefi olarak ([işlev üyeleri](expressions.md#function-members).9, [çağırma ifadeleri](expressions.md#invocation-expressions)), hem `get` erişimcileri hem de `set` erişimcisi bulunmalı ve erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1133">If the usage is as the target of compound assignment ([Compound assignment](expressions.md#compound-assignment)), or as the target of the `++` or `--` operators ([Function members](expressions.md#function-members).9, [Invocation expressions](expressions.md#invocation-expressions)), both the `get` accessors and the `set` accessor must exist and be accessible.</span></span>

<span data-ttu-id="4c8df-1134">Aşağıdaki örnekte, özellik `A.Text`, yalnızca `set` erişimcisinin çağrıldığı bağlamlarda bile, özellik `B.Text`tarafından gizlenir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1134">In the following example, the property `A.Text` is hidden by the property `B.Text`, even in contexts where only the `set` accessor is called.</span></span> <span data-ttu-id="4c8df-1135">Buna karşılık, `B.Count` özelliği sınıf `M`erişemez, bu nedenle `A.Count` erişilebilir özellik kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1135">In contrast, the property `B.Count` is not accessible to class `M`, so the accessible property `A.Count` is used instead.</span></span>

```csharp
class A
{
    public string Text {
        get { return "hello"; }
        set { }
    }

    public int Count {
        get { return 5; }
        set { }
    }
}

class B: A
{
    private string text = "goodbye"; 
    private int count = 0;

    new public string Text {
        get { return text; }
        protected set { text = value; }
    }

    new protected int Count { 
        get { return count; }
        set { count = value; }
    }
}

class M
{
    static void Main() {
        B b = new B();
        b.Count = 12;             // Calls A.Count set accessor
        int i = b.Count;          // Calls A.Count get accessor
        b.Text = "howdy";         // Error, B.Text set accessor not accessible
        string s = b.Text;        // Calls B.Text get accessor
    }
}
```

<span data-ttu-id="4c8df-1136">Arabirim uygulamak için kullanılan bir erişimcinin *accessor_modifier*olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1136">An accessor that is used to implement an interface may not have an *accessor_modifier*.</span></span> <span data-ttu-id="4c8df-1137">Bir arabirim uygulamak için yalnızca bir erişimci kullanılırsa, diğer erişimci bir *accessor_modifier*ile bildirilemeyebilir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1137">If only one accessor is used to implement an interface, the other accessor may be declared with an *accessor_modifier*:</span></span>
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public string Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a><span data-ttu-id="4c8df-1138">Sanal, Sealed, override ve abstract Özellik erişimcileri</span><span class="sxs-lookup"><span data-stu-id="4c8df-1138">Virtual, sealed, override, and abstract property accessors</span></span>

<span data-ttu-id="4c8df-1139">`virtual` özelliği bildirimi, özelliğin erişimcilerinin sanal olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1139">A `virtual` property declaration specifies that the accessors of the property are virtual.</span></span> <span data-ttu-id="4c8df-1140">`virtual` değiştiricisi her iki bir okuma-yazma özelliği erişimcisi için geçerlidir; okuma-yazma özelliğinin yalnızca bir erişimcisinin sanal olması mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1140">The `virtual` modifier applies to both accessors of a read-write property—it is not possible for only one accessor of a read-write property to be virtual.</span></span>

<span data-ttu-id="4c8df-1141">`abstract` özelliği bildirimi, özelliğin erişimcilerinin sanal olduğunu, ancak erişimcilerinin gerçek bir uygulamasını sağlamamayı belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1141">An `abstract` property declaration specifies that the accessors of the property are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="4c8df-1142">Bunun yerine, soyut olmayan türetilmiş sınıfların, özelliği geçersiz kılarak erişimcileri için kendi uygulamasını sağlaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1142">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the property.</span></span> <span data-ttu-id="4c8df-1143">Soyut Özellik bildirimine yönelik bir erişimci gerçek uygulama sunmadığından, *accessor_body* yalnızca noktalı virgülden oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1143">Because an accessor for an abstract property declaration provides no actual implementation, its *accessor_body* simply consists of a semicolon.</span></span>

<span data-ttu-id="4c8df-1144">Hem `abstract` hem de `override` değiştiricilerini içeren bir özellik bildirimi, özelliğin soyut olduğunu ve bir temel özelliği geçersiz kıldığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1144">A property declaration that includes both the `abstract` and `override` modifiers specifies that the property is abstract and overrides a base property.</span></span> <span data-ttu-id="4c8df-1145">Böyle bir özelliğin erişimcileri de soyuttur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1145">The accessors of such a property are also abstract.</span></span>

<span data-ttu-id="4c8df-1146">Soyut Özellik bildirimlerine yalnızca soyut sınıflarda izin verilir ([soyut sınıflar](classes.md#abstract-classes)). Devralınan bir sanal özelliğin erişimcileri, bir `override` yönergesini belirten bir özellik bildirimi eklenerek, türetilmiş bir sınıfta geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1146">Abstract property declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).The accessors of an inherited virtual property can be overridden in a derived class by including a property declaration that specifies an `override` directive.</span></span> <span data-ttu-id="4c8df-1147">Bu, ***geçersiz kılma özelliği bildirimi***olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1147">This is known as an ***overriding property declaration***.</span></span> <span data-ttu-id="4c8df-1148">Geçersiz kılan özellik bildirimi yeni bir özellik bildirmiyor.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1148">An overriding property declaration does not declare a new property.</span></span> <span data-ttu-id="4c8df-1149">Bunun yerine, var olan bir sanal özelliğin erişimcilerinin uygulamalarını özelleştirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1149">Instead, it simply specializes the implementations of the accessors of an existing virtual property.</span></span>

<span data-ttu-id="4c8df-1150">Geçersiz kılma özelliği bildirimi devralınan özellik olarak aynı erişilebilirlik değiştiricilerini, türü ve adı belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1150">An overriding property declaration must specify the exact same accessibility modifiers, type, and name as the inherited property.</span></span> <span data-ttu-id="4c8df-1151">Devralınan özelliğin yalnızca tek bir erişimcisi varsa (yani devralınan özellik salt okunurdur veya salt yazılır ise), geçersiz kılma özelliği yalnızca o erişimciyi içermelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1151">If the inherited property has only a single accessor (i.e., if the inherited property is read-only or write-only), the overriding property must include only that accessor.</span></span> <span data-ttu-id="4c8df-1152">Devralınan özellik her iki erişimcileri de içeriyorsa (yani, devralınan Özellik okuma-yazma ise), geçersiz kılma özelliği tek bir erişimci veya her iki erişimci içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1152">If the inherited property includes both accessors (i.e., if the inherited property is read-write), the overriding property can include either a single accessor or both accessors.</span></span>

<span data-ttu-id="4c8df-1153">Geçersiz kılan bir özellik bildirimi `sealed` değiştiriciyi içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1153">An overriding property declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="4c8df-1154">Bu değiştiricinin kullanılması, türetilmiş bir sınıfın özelliği daha fazla geçersiz kılmasını önler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1154">Use of this modifier prevents a derived class from further overriding the property.</span></span> <span data-ttu-id="4c8df-1155">Sealed özelliğinin erişimcileri de korumalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1155">The accessors of a sealed property are also sealed.</span></span>

<span data-ttu-id="4c8df-1156">Bildirim ve çağırma sözdiziminde farklar haricinde, sanal, korumalı, geçersiz kılma ve soyut erişimciler, sanal, mühürlenmiş, geçersiz kılma ve soyut yöntemler gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1156">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="4c8df-1157">Özellikle, [sanal yöntemlerde](classes.md#virtual-methods)açıklanan kurallar, [geçersiz kılma yöntemleri](classes.md#override-methods), [korumalı Yöntemler](classes.md#sealed-methods)ve [soyut yöntemler](classes.md#abstract-methods) , erişimciler karşılık gelen bir formun yöntemlerimiz gibi geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1157">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form:</span></span>

*  <span data-ttu-id="4c8df-1158">`get` erişimcisi, özellik türünün dönüş değeri ve kapsayan özelliği ile aynı değiştiriciler içeren parametresiz bir yönteme karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1158">A `get` accessor corresponds to a parameterless method with a return value of the property type and the same modifiers as the containing property.</span></span>
*  <span data-ttu-id="4c8df-1159">`set` erişimcisi, özellik türünün tek değerli parametresine, `void` dönüş türüne ve kapsayan özelliği ile aynı değiştiricilere karşılık gelen bir yönteme karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1159">A `set` accessor corresponds to a method with a single value parameter of the property type, a `void` return type, and the same modifiers as the containing property.</span></span>

<span data-ttu-id="4c8df-1160">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-1160">In the example</span></span>
```csharp
abstract class A
{
    int y;

    public virtual int X {
        get { return 0; }
    }

    public virtual int Y {
        get { return y; }
        set { y = value; }
    }

    public abstract int Z { get; set; }
}
```
<span data-ttu-id="4c8df-1161">`X` sanal bir salt okunurdur, `Y` sanal bir okuma-yazma özelliğidir ve `Z` soyut bir okuma-yazma özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1161">`X` is a virtual read-only property, `Y` is a virtual read-write property, and `Z` is an abstract read-write property.</span></span> <span data-ttu-id="4c8df-1162">`Z` soyut olduğundan, kapsayan sınıf `A` de soyut olarak bildirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1162">Because `Z` is abstract, the containing class `A` must also be declared abstract.</span></span>

<span data-ttu-id="4c8df-1163">`A` türetilen bir sınıf aşağıda gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1163">A class that derives from `A` is show below:</span></span>
```csharp
class B: A
{
    int z;

    public override int X {
        get { return base.X + 1; }
    }

    public override int Y {
        set { base.Y = value < 0? 0: value; }
    }

    public override int Z {
        get { return z; }
        set { z = value; }
    }
}
```

<span data-ttu-id="4c8df-1164">Burada, `X`, `Y`ve `Z` bildirimleri Özellik bildirimlerini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1164">Here, the declarations of `X`, `Y`, and `Z` are overriding property declarations.</span></span> <span data-ttu-id="4c8df-1165">Her özellik bildirimi, karşılık gelen devralınmış özelliğin erişilebilirlik değiştiricilerini, türünü ve adını tam olarak eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1165">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="4c8df-1166">`X` `get` erişimcisi ve `Y` `set` erişimcisi, devralınan erişimcilere erişmek için `base` anahtar sözcüğünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1166">The `get` accessor of `X` and the `set` accessor of `Y` use the `base` keyword to access the inherited accessors.</span></span> <span data-ttu-id="4c8df-1167">`Z` bildirimi her iki soyut erişimciyi geçersiz kılar. bu nedenle, `B`içinde bekleyen soyut işlev üyeleri yoktur ve `B` soyut olmayan bir sınıf olarak izin verilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1167">The declaration of `Z` overrides both abstract accessors—thus, there are no outstanding abstract function members in `B`, and `B` is permitted to be a non-abstract class.</span></span>

<span data-ttu-id="4c8df-1168">Bir özellik `override`olarak bildirildiğinde, geçersiz kılınan erişimcilere geçersiz kılma kodu tarafından erişilebilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1168">When a property is declared as an `override`, any overridden accessors must be accessible to the overriding code.</span></span> <span data-ttu-id="4c8df-1169">Ayrıca, hem özelliğin hem de dizin oluşturucunun kendisi ve erişimcilerinin tanımlanmış erişilebilirliği, geçersiz kılınan üye ve erişimcilerinin ile eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1169">In addition, the declared accessibility of both the property or indexer itself, and of the accessors, must match that of the overridden member and accessors.</span></span> <span data-ttu-id="4c8df-1170">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1170">For example:</span></span>
```csharp
public class B
{
    public virtual int P {
        protected set {...}
        get {...}
    }
}

public class D: B
{
    public override int P {
        protected set {...}            // Must specify protected here
        get {...}                      // Must not have a modifier here
    }
}
```

## <a name="events"></a><span data-ttu-id="4c8df-1171">Olaylar</span><span class="sxs-lookup"><span data-stu-id="4c8df-1171">Events</span></span>

<span data-ttu-id="4c8df-1172">Bir ***olay*** , bir nesnenin veya sınıfın bildirimleri sağlamasını sağlayan bir üyedir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1172">An ***event*** is a member that enables an object or class to provide notifications.</span></span> <span data-ttu-id="4c8df-1173">İstemciler ***olay işleyicileri***sağlayarak olaylar için yürütülebilir kod ekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1173">Clients can attach executable code for events by supplying ***event handlers***.</span></span>

<span data-ttu-id="4c8df-1174">Olaylar *event_declaration*s kullanılarak bildirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1174">Events are declared using *event_declaration*s:</span></span>

```antlr
event_declaration
    : attributes? event_modifier* 'event' type variable_declarators ';'
    | attributes? event_modifier* 'event' type member_name '{' event_accessor_declarations '}'
    ;

event_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | event_modifier_unsafe
    ;

event_accessor_declarations
    : add_accessor_declaration remove_accessor_declaration
    | remove_accessor_declaration add_accessor_declaration
    ;

add_accessor_declaration
    : attributes? 'add' block
    ;

remove_accessor_declaration
    : attributes? 'remove' block
    ;
```

<span data-ttu-id="4c8df-1175">Bir *event_declaration* bir *öznitelikler* kümesi ([öznitelikler](attributes.md)) ve dört erişim değiştiricisinin ([erişim değiştiricileri](classes.md#access-modifiers)) geçerli bir birleşimini, `new` ([Yeni değiştirici](classes.md#the-new-modifier)), `static` ([statik ve örnek yöntemleri](classes.md#static-and-instance-methods)), `virtual` ([sanal yöntemler](classes.md#virtual-methods)), `override` ([geçersiz kılma yöntemleri](classes.md#override-methods)), `sealed` ([korumalı](classes.md#sealed-methods)Yöntemler), `abstract` ([soyut yöntemler](classes.md#abstract-methods)) ve `extern` ([dış Yöntemler](classes.md#external-methods)) değiştiricilerini içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1175">An *event_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="4c8df-1176">Olay bildirimleri, geçerli değiştiriciler birleşimleriyle ilgili olarak yöntem bildirimleri ([yöntemleriyle](classes.md#methods)) ile aynı kurallara tabidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1176">Event declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="4c8df-1177">Olay bildiriminin *türü* bir *delegate_type* ([başvuru türleri](types.md#reference-types)) olmalıdır ve bu *delegate_type* en azından olayın kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1177">The *type* of an event declaration must be a *delegate_type* ([Reference types](types.md#reference-types)), and that *delegate_type* must be at least as accessible as the event itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="4c8df-1178">Bir olay bildirimi *event_accessor_declarations*içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1178">An event declaration may include *event_accessor_declarations*.</span></span> <span data-ttu-id="4c8df-1179">Ancak, dış olmayan, soyut olmayan olaylar için derleyici onları otomatik olarak ([alan benzeri olaylar](classes.md#field-like-events)) sağlar; extern olaylar için, erişimciler dışarıdan sağlanır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1179">However, if it does not, for non-extern, non-abstract events, the compiler supplies them automatically ([Field-like events](classes.md#field-like-events)); for extern events, the accessors are provided externally.</span></span>

<span data-ttu-id="4c8df-1180">*Event_accessor_declarations* atan bir olay bildirimi, bir veya daha fazla olayı tanımlar — her biri *variable_declarator*s.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1180">An event declaration that omits *event_accessor_declarations* defines one or more events—one for each of the *variable_declarator*s.</span></span> <span data-ttu-id="4c8df-1181">Öznitelikler ve değiştiriciler, bu tür bir *event_declaration*tarafından belirtilen tüm üyelere uygulanır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1181">The attributes and modifiers apply to all of the members declared by such an *event_declaration*.</span></span>

<span data-ttu-id="4c8df-1182">*Event_declaration* , hem `abstract` değiştiricisini hem de küme ayracı ile ayrılmış *event_accessor_declarations*içermesi için derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1182">It is a compile-time error for an *event_declaration* to include both the `abstract` modifier and brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="4c8df-1183">Bir olay bildirimi `extern` değiştirici içerdiğinde, olay bir ***dış olay***olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1183">When an event declaration includes an `extern` modifier, the event is said to be an ***external event***.</span></span> <span data-ttu-id="4c8df-1184">Dış bir olay bildirimi gerçek uygulama sağladığından, `extern` değiştiricisini ve *event_accessor_declarations*içermesi bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1184">Because an external event declaration provides no actual implementation, it is an error for it to include both the `extern` modifier and *event_accessor_declarations*.</span></span>

<span data-ttu-id="4c8df-1185">Bir `abstract` veya `external` *değiştiricvariable_initializer*isine sahip bir olay bildirimi *variable_declarator* için derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1185">It is a compile-time error for a *variable_declarator* of an event declaration with an `abstract` or `external` modifier to include a *variable_initializer*.</span></span>

<span data-ttu-id="4c8df-1186">Bir olay, `+=` ve `-=` işleçlerinin ([olay atama](expressions.md#event-assignment)) sol tarafı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1186">An event can be used as the left-hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="4c8df-1187">Bu işleçler sırasıyla olay işleyicilerini bir olaydan kaldırmak ya da kaldırmak için kullanılır ve olayın erişim değiştiricileri, bu tür işlemlere izin verilen bağlamlara eklenir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1187">These operators are used, respectively, to attach event handlers to or to remove event handlers from an event, and the access modifiers of the event control the contexts in which such operations are permitted.</span></span>

<span data-ttu-id="4c8df-1188">`+=` ve `-=`, olayı bildiren türden bir olayda izin verilen tek işlemlerdir, dış kod bir olay için işleyiciler ekleyebilir ve kaldırabilir, ancak başka hiçbir şekilde olay işleyicilerinin temel alınan listesini alabilir veya değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1188">Since `+=` and `-=` are the only operations that are permitted on an event outside the type that declares the event, external code can add and remove handlers for an event, but cannot in any other way obtain or modify the underlying list of event handlers.</span></span>

<span data-ttu-id="4c8df-1189">`x += y` veya `x -= y`form işleminde, `x` bir olaysa ve başvuru, `x`bildirimini içeren türün dışında gerçekleşirken, işlemin sonucu tür `void` olur (`x`türüne sahip olarak, atamadan sonra `x` değeri ile).</span><span class="sxs-lookup"><span data-stu-id="4c8df-1189">In an operation of the form `x += y` or `x -= y`, when `x` is an event and the reference takes place outside the type that contains the declaration of `x`, the result of the operation has type `void` (as opposed to having the type of `x`, with the value of `x` after the assignment).</span></span> <span data-ttu-id="4c8df-1190">Bu kural, dış kodun bir olayın temel temsilcisini dolaylı olarak incelemeden yasaklar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1190">This rule prohibits external code from indirectly examining the underlying delegate of an event.</span></span>

<span data-ttu-id="4c8df-1191">Aşağıdaki örnek, `Button` sınıfının örneklerine nasıl eklenen olay işleyicilerinin olduğunu gösterir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1191">The following example shows how event handlers are attached to instances of the `Button` class:</span></span>
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;
}

public class LoginDialog: Form
{
    Button OkButton;
    Button CancelButton;

    public LoginDialog() {
        OkButton = new Button(...);
        OkButton.Click += new EventHandler(OkButtonClick);
        CancelButton = new Button(...);
        CancelButton.Click += new EventHandler(CancelButtonClick);
    }

    void OkButtonClick(object sender, EventArgs e) {
        // Handle OkButton.Click event
    }

    void CancelButtonClick(object sender, EventArgs e) {
        // Handle CancelButton.Click event
    }
}
```

<span data-ttu-id="4c8df-1192">Burada `LoginDialog` örneği Oluşturucusu iki `Button` örneği oluşturur ve olay işleyicilerini `Click` olaylarına ekler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1192">Here, the `LoginDialog` instance constructor creates two `Button` instances and attaches event handlers to the `Click` events.</span></span>

### <a name="field-like-events"></a><span data-ttu-id="4c8df-1193">Alan benzeri olaylar</span><span class="sxs-lookup"><span data-stu-id="4c8df-1193">Field-like events</span></span>

<span data-ttu-id="4c8df-1194">Bir olayın bildirimini içeren sınıfın veya yapının program metni içinde, bazı olaylar alan gibi kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1194">Within the program text of the class or struct that contains the declaration of an event, certain events can be used like fields.</span></span> <span data-ttu-id="4c8df-1195">Bu şekilde kullanılmak üzere, bir olay `abstract` veya `extern`olmamalı ve açıkça *event_accessor_declarations*içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1195">To be used in this way, an event must not be `abstract` or `extern`, and must not explicitly include *event_accessor_declarations*.</span></span> <span data-ttu-id="4c8df-1196">Bu tür bir olay, bir alana izin veren herhangi bir bağlamda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1196">Such an event can be used in any context that permits a field.</span></span> <span data-ttu-id="4c8df-1197">Alan, olaya eklenmiş olan olay işleyicileri listesine başvuran bir temsilci ([Temsilciler](delegates.md)) içerir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1197">The field contains a delegate ([Delegates](delegates.md)) which refers to the list of event handlers that have been added to the event.</span></span> <span data-ttu-id="4c8df-1198">Hiçbir olay işleyicisi eklenmemişse, alan `null`içerir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1198">If no event handlers have been added, the field contains `null`.</span></span>

<span data-ttu-id="4c8df-1199">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-1199">In the example</span></span>
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;

    protected void OnClick(EventArgs e) {
        if (Click != null) Click(this, e);
    }

    public void Reset() {
        Click = null;
    }
}
```
<span data-ttu-id="4c8df-1200">`Click`, `Button` sınıfında bir alan olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1200">`Click` is used as a field within the `Button` class.</span></span> <span data-ttu-id="4c8df-1201">Örneğin gösterdiği gibi, alan, temsilci çağırma ifadelerinde incelenebilir, değiştirilebilir ve kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1201">As the example demonstrates, the field can be examined, modified, and used in delegate invocation expressions.</span></span> <span data-ttu-id="4c8df-1202">`Button` sınıfındaki `OnClick` yöntemi `Click` olayını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1202">The `OnClick` method in the `Button` class "raises" the `Click` event.</span></span> <span data-ttu-id="4c8df-1203">Bir olayı oluşturma kavramı, olayın gösterdiği temsilciyi çağırmak için tam olarak eşdeğerdir. bu nedenle, olayları yükseltmek için özel dil yapıları yoktur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1203">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span> <span data-ttu-id="4c8df-1204">Temsilci çağrısının önünde, temsilcinin null olmamasını sağlayan bir denetim olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1204">Note that the delegate invocation is preceded by a check that ensures the delegate is non-null.</span></span>

<span data-ttu-id="4c8df-1205">`Button` sınıfının bildirimi dışında, `Click` üyesi yalnızca `+=` ve `-=` işleçlerinin sol tarafında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1205">Outside the declaration of the `Button` class, the `Click` member can only be used on the left-hand side of the `+=` and `-=` operators, as in</span></span>
```csharp
b.Click += new EventHandler(...);
```
<span data-ttu-id="4c8df-1206">bir temsilciyi `Click` olayının çağırma listesine ekler ve</span><span class="sxs-lookup"><span data-stu-id="4c8df-1206">which appends a delegate to the invocation list of the `Click` event, and</span></span>
```csharp
b.Click -= new EventHandler(...);
```
<span data-ttu-id="4c8df-1207">`Click` olayının çağırma listesinden bir temsilciyi kaldıran.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1207">which removes a delegate from the invocation list of the `Click` event.</span></span>

<span data-ttu-id="4c8df-1208">Alan benzeri bir olay derlenirken, derleyici temsilciyi tutmak için otomatik olarak depolama oluşturur ve temsilci alanına olay işleyicileri ekleyen veya çıkarmayan olay için erişimciler oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1208">When compiling a field-like event, the compiler automatically creates storage to hold the delegate, and creates accessors for the event that add or remove event handlers to the delegate field.</span></span> <span data-ttu-id="4c8df-1209">Ekleme ve kaldırma işlemleri iş parçacığı açısından güvenlidir ve bir örnek olayı için kapsayan nesne üzerinde kilit ([kilit deyimi](statements.md#the-lock-statement)) veya statik bir olay için tür nesnesi ([anonim nesne oluşturma ifadeleri](expressions.md#anonymous-object-creation-expressions)) tutulurken (ancak için gerekli değildir) yapılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1209">The addition and removal operations are thread safe, and may (but are not required to) be done while holding the lock ([The lock statement](statements.md#the-lock-statement)) on the containing object for an instance event, or the type object ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) for a static event.</span></span>

<span data-ttu-id="4c8df-1210">Bu nedenle, formun bir örnek olay bildirimi:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1210">Thus, an instance event declaration of the form:</span></span>
```csharp
class X
{
    public event D Ev;
}
```
<span data-ttu-id="4c8df-1211">Şuna eşdeğer bir değere derlenecek:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1211">will be compiled to something equivalent to:</span></span>
```csharp
class X
{
    private D __Ev;  // field to hold the delegate

    public event D Ev {
        add {
            /* add the delegate in a thread safe way */
        }

        remove {
            /* remove the delegate in a thread safe way */
        }
    }
}
```
<span data-ttu-id="4c8df-1212">`X`sınıfı içinde, `+=` ve `-=` işleçlerinin sol tarafındaki `Ev` başvuruları ekleme ve kaldırma erişimcilerinin çağrılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1212">Within the class `X`, references to `Ev` on the left-hand side of the `+=` and `-=` operators cause the add and remove accessors to be invoked.</span></span> <span data-ttu-id="4c8df-1213">`Ev` yapılan diğer tüm başvurular, bunun yerine `__Ev` gizli alana başvuracak şekilde derlenir ([üye erişimi](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-1213">All other references to `Ev` are compiled to reference the hidden field `__Ev` instead ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="4c8df-1214">"`__Ev`" adı rastgele; gizli alan herhangi bir ada veya hiç ada sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1214">The name "`__Ev`" is arbitrary; the hidden field could have any name or no name at all.</span></span>

### <a name="event-accessors"></a><span data-ttu-id="4c8df-1215">Olay erişimcileri</span><span class="sxs-lookup"><span data-stu-id="4c8df-1215">Event accessors</span></span>

<span data-ttu-id="4c8df-1216">Olay bildirimleri, yukarıdaki `Button` örnekte olduğu gibi genellikle *event_accessor_declarations*atlayın.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1216">Event declarations typically omit *event_accessor_declarations*, as in the `Button` example above.</span></span> <span data-ttu-id="4c8df-1217">Bunun için bir durum, her olay için bir alanın depolama maliyetinin kabul edilebilir olması durumunda oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1217">One situation for doing so involves the case in which the storage cost of one field per event is not acceptable.</span></span> <span data-ttu-id="4c8df-1218">Böyle durumlarda, bir sınıf *event_accessor_declarations* içerebilir ve olay işleyicilerinin listesini depolamak için özel bir mekanizma kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1218">In such cases, a class can include *event_accessor_declarations* and use a private mechanism for storing the list of event handlers.</span></span>

<span data-ttu-id="4c8df-1219">Bir olayın *event_accessor_declarations* , olay işleyicilerini ekleme ve kaldırma ile ilişkili yürütülebilir deyimleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1219">The *event_accessor_declarations* of an event specify the executable statements associated with adding and removing event handlers.</span></span>

<span data-ttu-id="4c8df-1220">Erişimci bildirimleri bir *add_accessor_declaration* ve bir *remove_accessor_declaration*oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1220">The accessor declarations consist of an *add_accessor_declaration* and a *remove_accessor_declaration*.</span></span> <span data-ttu-id="4c8df-1221">Her erişimci bildirimi, belirteç `add` veya `remove` sonrasında bir *blok*tarafından oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1221">Each accessor declaration consists of the token `add` or `remove` followed by a *block*.</span></span> <span data-ttu-id="4c8df-1222">Bir *add_accessor_declaration* ilişkili *blok* , bir olay işleyicisi eklendiğinde yürütülecek deyimleri belirtir ve bir *remove_accessor_declaration* ilişkili *blok* , bir olay işleyicisi kaldırıldığında yürütülecek deyimleri belirler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1222">The *block* associated with an *add_accessor_declaration* specifies the statements to execute when an event handler is added, and the *block* associated with a *remove_accessor_declaration* specifies the statements to execute when an event handler is removed.</span></span>

<span data-ttu-id="4c8df-1223">Her *add_accessor_declaration* ve *remove_accessor_declaration* , olay türünün tek değerli parametresine ve `void` dönüş türüne sahip bir yönteme karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1223">Each *add_accessor_declaration* and *remove_accessor_declaration* corresponds to a method with a single value parameter of the event type and a `void` return type.</span></span> <span data-ttu-id="4c8df-1224">Bir olay erişimcisinin örtük parametresi `value`olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1224">The implicit parameter of an event accessor is named `value`.</span></span> <span data-ttu-id="4c8df-1225">Olay atamasında bir olay kullanıldığında, uygun olay erişimcisi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1225">When an event is used in an event assignment, the appropriate event accessor is used.</span></span> <span data-ttu-id="4c8df-1226">Özellikle, atama işleci `+=`, Add erişimcisi kullanılır ve atama işleci `-=`, kaldırma erişimcisi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1226">Specifically, if the assignment operator is `+=` then the add accessor is used, and if the assignment operator is `-=` then the remove accessor is used.</span></span> <span data-ttu-id="4c8df-1227">Her iki durumda da, atama işlecinin sağ işleneni olay erişimcisinin bağımsız değişkeni olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1227">In either case, the right-hand operand of the assignment operator is used as the argument to the event accessor.</span></span> <span data-ttu-id="4c8df-1228">Bir *add_accessor_declaration* veya *Remove_accessor_declaration* bloğunun, [Yöntem gövdesinde](classes.md#method-body)açıklanan `void` yöntemlerine uygun kurallara uyması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1228">The block of an *add_accessor_declaration* or a *remove_accessor_declaration* must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="4c8df-1229">Özellikle, bu tür bir bloktaki `return` deyimlerinin bir ifade belirtmelerine izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1229">In particular, `return` statements in such a block are not permitted to specify an expression.</span></span>

<span data-ttu-id="4c8df-1230">Bir olay erişimcisinin örtük olarak `value`adlı bir parametresi olduğundan, bu ada sahip olması için bir yerel değişken veya bir olay erişimcisinde belirtilen sabit için derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1230">Since an event accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declared in an event accessor to have that name.</span></span>

<span data-ttu-id="4c8df-1231">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-1231">In the example</span></span>
```csharp
class Control: Component
{
    // Unique keys for events
    static readonly object mouseDownEventKey = new object();
    static readonly object mouseUpEventKey = new object();

    // Return event handler associated with key
    protected Delegate GetEventHandler(object key) {...}

    // Add event handler associated with key
    protected void AddEventHandler(object key, Delegate handler) {...}

    // Remove event handler associated with key
    protected void RemoveEventHandler(object key, Delegate handler) {...}

    // MouseDown event
    public event MouseEventHandler MouseDown {
        add { AddEventHandler(mouseDownEventKey, value); }
        remove { RemoveEventHandler(mouseDownEventKey, value); }
    }

    // MouseUp event
    public event MouseEventHandler MouseUp {
        add { AddEventHandler(mouseUpEventKey, value); }
        remove { RemoveEventHandler(mouseUpEventKey, value); }
    }

    // Invoke the MouseUp event
    protected void OnMouseUp(MouseEventArgs args) {
        MouseEventHandler handler; 
        handler = (MouseEventHandler)GetEventHandler(mouseUpEventKey);
        if (handler != null)
            handler(this, args);
    }
}
```
<span data-ttu-id="4c8df-1232">`Control` sınıfı, olaylar için bir iç depolama mekanizması uygular.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1232">the `Control` class implements an internal storage mechanism for events.</span></span> <span data-ttu-id="4c8df-1233">`AddEventHandler` yöntemi bir temsilci değerini bir anahtarla ilişkilendirir, `GetEventHandler` yöntemi şu anda bir anahtarla ilişkili olan temsilciyi döndürür ve `RemoveEventHandler` yöntemi belirtilen olay için bir temsilciyi olay işleyicisi olarak kaldırır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1233">The `AddEventHandler` method associates a delegate value with a key, the `GetEventHandler` method returns the delegate currently associated with a key, and the `RemoveEventHandler` method removes a delegate as an event handler for the specified event.</span></span> <span data-ttu-id="4c8df-1234">Temel alınan depolama mekanizması, bir `null` temsilci değerini bir anahtarla ilişkilendirme maliyeti olmaması ve bu nedenle işlenmemiş olayların depolama alanını tüketmesi gibi tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1234">Presumably, the underlying storage mechanism is designed such that there is no cost for associating a `null` delegate value with a key, and thus unhandled events consume no storage.</span></span>

### <a name="static-and-instance-events"></a><span data-ttu-id="4c8df-1235">Statik ve örnek olayları</span><span class="sxs-lookup"><span data-stu-id="4c8df-1235">Static and instance events</span></span>

<span data-ttu-id="4c8df-1236">Bir olay bildirimi `static` değiştirici içerdiğinde, olay ***statik bir olay***olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1236">When an event declaration includes a `static` modifier, the event is said to be a ***static event***.</span></span> <span data-ttu-id="4c8df-1237">`static` değiştirici yoksa, olay bir ***örnek olay***olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1237">When no `static` modifier is present, the event is said to be an ***instance event***.</span></span>

<span data-ttu-id="4c8df-1238">Statik bir olay, belirli bir örnekle ilişkili değildir ve statik olay erişimcilerinde `this` başvurmak için derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1238">A static event is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static event.</span></span>

<span data-ttu-id="4c8df-1239">Örnek olay, bir sınıfın belirli bir örneğiyle ilişkilendirilir ve bu örneğe bu olayın erişimcilerinde `this` ([Bu erişim](expressions.md#this-access)) olarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1239">An instance event is associated with a given instance of a class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that event.</span></span>

<span data-ttu-id="4c8df-1240">Bir olaya bir *member_access* ([üye erişimi](expressions.md#member-access)) `E.M`, `M` statik bir olaydır, `E` `M`içeren bir türü belirtmelidir ve `M` bir örnek olayıdır, E 'nin `M`içeren bir türün örneğini belirtmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1240">When an event is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static event, `E` must denote a type containing `M`, and if `M` is an instance event, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="4c8df-1241">Statik ve örnek üyeleri arasındaki farklılıklar, [statik ve örnek üyelerinde](classes.md#static-and-instance-members)daha ayrıntılı bir şekilde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1241">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a><span data-ttu-id="4c8df-1242">Sanal, korumalı, geçersiz kılma ve soyut olay erişimcileri</span><span class="sxs-lookup"><span data-stu-id="4c8df-1242">Virtual, sealed, override, and abstract event accessors</span></span>

<span data-ttu-id="4c8df-1243">`virtual` bir olay bildirimi, bu olayın erişimcilerinin sanal olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1243">A `virtual` event declaration specifies that the accessors of that event are virtual.</span></span> <span data-ttu-id="4c8df-1244">`virtual` değiştiricisi, her iki bir olayın erişimcisi için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1244">The `virtual` modifier applies to both accessors of an event.</span></span>

<span data-ttu-id="4c8df-1245">`abstract` bir olay bildirimi, olayın erişimcilerinin sanal olduğunu, ancak erişimcilerinin gerçek bir uygulamasını sağlamamayı belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1245">An `abstract` event declaration specifies that the accessors of the event are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="4c8df-1246">Bunun yerine, Özet olmayan türetilmiş sınıfların, olayı geçersiz kılarak erişimcileri için kendi uygulamasını sağlaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1246">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the event.</span></span> <span data-ttu-id="4c8df-1247">Soyut bir olay bildirimi gerçek uygulama sağlamadığı için, küme ayracı ile ayrılmış *event_accessor_declarations*sağlayamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1247">Because an abstract event declaration provides no actual implementation, it cannot provide brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="4c8df-1248">Hem `abstract` hem de `override` değiştiricilerini içeren bir olay bildirimi, olayın soyut olduğunu ve bir temel olayı geçersiz kıldığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1248">An event declaration that includes both the `abstract` and `override` modifiers specifies that the event is abstract and overrides a base event.</span></span> <span data-ttu-id="4c8df-1249">Böyle bir olayın erişimcileri de soyuttur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1249">The accessors of such an event are also abstract.</span></span>

<span data-ttu-id="4c8df-1250">Soyut olay bildirimlerine yalnızca soyut sınıflarda izin verilir ([soyut sınıflar](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-1250">Abstract event declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="4c8df-1251">Devralınan bir sanal olayın erişimcileri, bir `override` değiştiricisini belirten bir olay bildirimi eklenerek, türetilmiş bir sınıfta geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1251">The accessors of an inherited virtual event can be overridden in a derived class by including an event declaration that specifies an `override` modifier.</span></span> <span data-ttu-id="4c8df-1252">Bu, ***geçersiz kılan bir olay bildirimi***olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1252">This is known as an ***overriding event declaration***.</span></span> <span data-ttu-id="4c8df-1253">Geçersiz kılan bir olay bildirimi yeni bir olay bildirmiyor.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1253">An overriding event declaration does not declare a new event.</span></span> <span data-ttu-id="4c8df-1254">Bunun yerine, var olan bir sanal olay erişimcilerinin uygulamalarını özelleştirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1254">Instead, it simply specializes the implementations of the accessors of an existing virtual event.</span></span>

<span data-ttu-id="4c8df-1255">Geçersiz kılan bir olay bildirimi, geçersiz kılınan olayla aynı erişilebilirlik değiştiricilerini, türü ve adı belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1255">An overriding event declaration must specify the exact same accessibility modifiers, type, and name as the overridden event.</span></span>

<span data-ttu-id="4c8df-1256">Geçersiz kılan bir olay bildirimi `sealed` değiştiriciyi içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1256">An overriding event declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="4c8df-1257">Bu değiştiricinin kullanılması, türetilmiş bir sınıfın olayı daha fazla geçersiz kılmasını önler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1257">Use of this modifier prevents a derived class from further overriding the event.</span></span> <span data-ttu-id="4c8df-1258">Korumalı bir olayın erişimcileri de korumalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1258">The accessors of a sealed event are also sealed.</span></span>

<span data-ttu-id="4c8df-1259">Geçersiz kılan bir olay bildiriminin `new` değiştiricisini içermesi için derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1259">It is a compile-time error for an overriding event declaration to include a `new` modifier.</span></span>

<span data-ttu-id="4c8df-1260">Bildirim ve çağırma sözdiziminde farklar haricinde, sanal, korumalı, geçersiz kılma ve soyut erişimciler, sanal, mühürlenmiş, geçersiz kılma ve soyut yöntemler gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1260">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="4c8df-1261">Özellikle, [sanal yöntemlerde](classes.md#virtual-methods)açıklanan kurallar, [geçersiz kılma yöntemleri](classes.md#override-methods), [korumalı Yöntemler](classes.md#sealed-methods)ve [soyut yöntemler](classes.md#abstract-methods) , erişimciler karşılık gelen bir formun yöntemlerimiz gibi geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1261">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form.</span></span> <span data-ttu-id="4c8df-1262">Her erişimci, olay türünün tek değerli parametresine, `void` dönüş türüne ve kapsayan olayla aynı değiştiricilere karşılık gelen bir yönteme karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1262">Each accessor corresponds to a method with a single value parameter of the event type, a `void` return type, and the same modifiers as the containing event.</span></span>

## <a name="indexers"></a><span data-ttu-id="4c8df-1263">Dizin Oluşturucular</span><span class="sxs-lookup"><span data-stu-id="4c8df-1263">Indexers</span></span>

<span data-ttu-id="4c8df-1264">***Dizin Oluşturucu*** , bir nesnenin bir dizi ile aynı şekilde dizinlenmesini sağlayan bir üyedir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1264">An ***indexer*** is a member that enables an object to be indexed in the same way as an array.</span></span> <span data-ttu-id="4c8df-1265">Dizin oluşturucular *indexer_declaration*s kullanılarak bildirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1265">Indexers are declared using *indexer_declaration*s:</span></span>

```antlr
indexer_declaration
    : attributes? indexer_modifier* indexer_declarator indexer_body
    ;

indexer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | indexer_modifier_unsafe
    ;

indexer_declarator
    : type 'this' '[' formal_parameter_list ']'
    | type interface_type '.' 'this' '[' formal_parameter_list ']'
    ;

indexer_body
    : '{' accessor_declarations '}' 
    | '=>' expression ';'
    ;
```

<span data-ttu-id="4c8df-1266">Bir *indexer_declaration* , bir dizi *öznitelik* ([öznitelik](attributes.md)) ve dört erişim değiştiricisinin ([erişim değiştiricileri](classes.md#access-modifiers)), `new` ([Yeni değiştirici](classes.md#the-new-modifier)), `virtual` ([sanal yöntemler](classes.md#virtual-methods)), `override` ([geçersiz kılma yöntemleri](classes.md#override-methods)), `sealed` ([korumalı Yöntemler](classes.md#sealed-methods)), `abstract` ([soyut](classes.md#abstract-methods)Yöntemler) ve `extern` ([dış Yöntemler](classes.md#external-methods)) değiştiricilerinden geçerli bir birleşimini içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1266">An *indexer_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="4c8df-1267">Dizin Oluşturucu bildirimleri, bir özel durum, Dizin Oluşturucu bildiriminde statik değiştiriciye izin verilmemesine neden olacak şekilde, geçerli değiştiriciler birleşimleriyle ilgili olarak yöntem bildirimleri ([yöntemleri](classes.md#methods)) ile aynı kurallara tabidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1267">Indexer declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers, with the one exception being that the static modifier is not permitted on an indexer declaration.</span></span>

<span data-ttu-id="4c8df-1268">`virtual`, `override`ve `abstract` değiştiriciler tek bir durum dışında birbirini dışlıyor.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1268">The modifiers `virtual`, `override`, and `abstract` are mutually exclusive except in one case.</span></span> <span data-ttu-id="4c8df-1269">`abstract` ve `override` değiştiricileri birlikte kullanılabilir, böylece bir soyut dizin oluşturucunun sanal bir dizin oluşturucuyu geçersiz kılabilmesini sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1269">The `abstract` and `override` modifiers may be used together so that an abstract indexer can override a virtual one.</span></span>

<span data-ttu-id="4c8df-1270">Bir Dizin Oluşturucu bildiriminin *türü* , bildirim tarafından tanıtılan dizin oluşturucunun öğe türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1270">The *type* of an indexer declaration specifies the element type of the indexer introduced by the declaration.</span></span> <span data-ttu-id="4c8df-1271">Dizin Oluşturucu açık arabirim üyesi uygulama değilse, *türün* sonuna anahtar sözcüğü `this`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1271">Unless the indexer is an explicit interface member implementation, the *type* is followed by the keyword `this`.</span></span> <span data-ttu-id="4c8df-1272">Açık arabirim üyesi uygulama için, *türün* arkasından bir *interface_type*, "`.`" ve anahtar sözcüğü `this`gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1272">For an explicit interface member implementation, the *type* is followed by an *interface_type*, a "`.`", and the keyword `this`.</span></span> <span data-ttu-id="4c8df-1273">Diğer üyelerin aksine, Dizin oluşturucular Kullanıcı tanımlı adlara sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1273">Unlike other members, indexers do not have user-defined names.</span></span>

<span data-ttu-id="4c8df-1274">*Formal_parameter_list* , dizin oluşturucunun parametrelerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1274">The *formal_parameter_list* specifies the parameters of the indexer.</span></span> <span data-ttu-id="4c8df-1275">Bir dizin oluşturucunun biçimsel parametre listesi[bir yönteme karşılık](classes.md#method-parameters)gelir, ancak en az bir parametre belirtilmesi gerekir ve `ref` ve `out` parametre değiştiricilerine izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1275">The formal parameter list of an indexer corresponds to that of a method ([Method parameters](classes.md#method-parameters)), except that at least one parameter must be specified, and that the `ref` and `out` parameter modifiers are not permitted.</span></span>

<span data-ttu-id="4c8df-1276">Bir dizin oluşturucunun *türü* ve *formal_parameter_list* başvurulan her bir tür, en azından dizin oluşturucunun kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1276">The *type* of an indexer and each of the types referenced in the *formal_parameter_list* must be at least as accessible as the indexer itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="4c8df-1277">Bir *indexer_body* ***erişimci gövdesinden*** ya da bir ***ifade gövdesinden***oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1277">An *indexer_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="4c8df-1278">Bir erişimci gövdesinde, "`{`" ve "`}`" belirteçlerinin içine alınması gereken *accessor_declarations*, özelliğin erişimcileri ([erişimcileri](classes.md#accessors)) ' sini bildirin.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1278">In an accessor body, *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="4c8df-1279">Erişimciler, özelliği okuma ve yazma ile ilişkili yürütülebilir deyimleri belirler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1279">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="4c8df-1280">"`=>`" işaretinden sonra bir ifade `E` ve noktalı `{ get { return E; } }`virgülden sonra, yalnızca alıcı sonucunun tek bir ifade tarafından verildiği alıcı dizin oluşturucularının belirtilmesi için kullanılan bir ifade gövdesi.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1280">An expression body consisting of "`=>`" followed by an expression `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only indexers where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="4c8df-1281">Bir Dizin Oluşturucu öğesine erişmek için sözdizimi, bir dizi öğesiyle aynı olsa da, bir Dizin Oluşturucu öğesi değişken olarak sınıflandırılmaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1281">Even though the syntax for accessing an indexer element is the same as that for an array element, an indexer element is not classified as a variable.</span></span> <span data-ttu-id="4c8df-1282">Bu nedenle, bir Dizin Oluşturucu öğesini bir `ref` veya `out` bağımsız değişken olarak geçirmek mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1282">Thus, it is not possible to pass an indexer element as a `ref` or `out` argument.</span></span>

<span data-ttu-id="4c8df-1283">Bir dizin oluşturucunun biçimsel parametre listesi, dizin oluşturucunun imzasını ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)) tanımlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1283">The formal parameter list of an indexer defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the indexer.</span></span> <span data-ttu-id="4c8df-1284">Özellikle, bir dizin oluşturucunun imzası, resmi parametrelerinin sayısından ve türlerinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1284">Specifically, the signature of an indexer consists of the number and types of its formal parameters.</span></span> <span data-ttu-id="4c8df-1285">Öğe türü ve biçimsel parametrelerinin adları, dizin oluşturucunun imzasının bir parçası değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1285">The element type and names of the formal parameters are not part of an indexer's signature.</span></span>

<span data-ttu-id="4c8df-1286">Bir dizin oluşturucunun imzası aynı sınıfta belirtilen diğer tüm dizin oluşturucularının imzalarından farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1286">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>

<span data-ttu-id="4c8df-1287">Dizin oluşturucular ve Özellikler kavram bakımından çok benzerdir, ancak aşağıdaki yollarla farklılık gösterir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1287">Indexers and properties are very similar in concept, but differ in the following ways:</span></span>

*  <span data-ttu-id="4c8df-1288">Bir özellik adıyla tanımlanır, ancak bir Dizin Oluşturucu imza tarafından tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1288">A property is identified by its name, whereas an indexer is identified by its signature.</span></span>
*  <span data-ttu-id="4c8df-1289">Bir özelliğe *simple_name* ([basit adlar](expressions.md#simple-names)) veya bir *member_access* ([üye erişimi](expressions.md#member-access)) üzerinden erişilir, ancak bir Dizin Oluşturucu öğesine bir *element_access* ([Dizin Oluşturucu erişimi](expressions.md#indexer-access)) üzerinden erişilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1289">A property is accessed through a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)), whereas an indexer element is accessed through an *element_access* ([Indexer access](expressions.md#indexer-access)).</span></span>
*  <span data-ttu-id="4c8df-1290">Bir özellik `static` üyesi olabilir, ancak bir Dizin Oluşturucu her zaman bir örnek üyesidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1290">A property can be a `static` member, whereas an indexer is always an instance member.</span></span>
*  <span data-ttu-id="4c8df-1291">Bir özelliğin `get` erişimcisi parametresi olmayan bir yönteme karşılık gelir, ancak bir dizin oluşturucunun `get` erişimcisi, Dizin oluşturucudaki aynı biçimsel parametre listesine sahip bir yönteme karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1291">A `get` accessor of a property corresponds to a method with no parameters, whereas a `get` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer.</span></span>
*  <span data-ttu-id="4c8df-1292">Bir özelliğin `set` erişimcisi, `value`adlı tek parametreli bir yönteme karşılık gelir, ancak bir dizin oluşturucunun `set` erişimcisi, Dizin oluşturucudaki aynı biçimsel parametre listesine sahip bir yönteme karşılık gelir ve `value`adlı ek bir parametredir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1292">A `set` accessor of a property corresponds to a method with a single parameter named `value`, whereas a `set` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer, plus an additional parameter named `value`.</span></span>
*  <span data-ttu-id="4c8df-1293">Bir Dizin Oluşturucu erişimcisinin bir Dizin Oluşturucu parametresiyle aynı ada sahip yerel bir değişken bildirmesi için derleme zamanı hatası.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1293">It is a compile-time error for an indexer accessor to declare a local variable with the same name as an indexer parameter.</span></span>
*  <span data-ttu-id="4c8df-1294">Geçersiz kılan özellik bildiriminde, devralınan özelliğe `base.P`sözdizimi kullanılarak erişilir; burada `P` özellik adıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1294">In an overriding property declaration, the inherited property is accessed using the syntax `base.P`, where `P` is the property name.</span></span> <span data-ttu-id="4c8df-1295">Geçersiz kılan bir Dizin Oluşturucu bildiriminde, devralınan dizin oluşturucuya `base[E]`sözdizimi kullanılarak erişilir; burada `E` virgülle ayrılmış ifadelerin bir listesidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1295">In an overriding indexer declaration, the inherited indexer is accessed using the syntax `base[E]`, where `E` is a comma separated list of expressions.</span></span>
*  <span data-ttu-id="4c8df-1296">"Otomatik olarak uygulanan Dizin Oluşturucu" kavramı yoktur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1296">There is no concept of an "automatically implemented indexer".</span></span> <span data-ttu-id="4c8df-1297">Noktalı olmayan, dış olmayan ve olmayan bir dizinleyiciye sahip bir hata.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1297">It is an error to have a non-abstract, non-external indexer with semicolon accessors.</span></span>

<span data-ttu-id="4c8df-1298">Bu farklılıklardan farklı olarak, [erişimciler](classes.md#accessors) ve [Otomatik uygulanan özellikleri](classes.md#automatically-implemented-properties) tanımlanan tüm kurallar, Dizin Oluşturucu erişimcileri ve özellik erişimcileri için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1298">Aside from these differences, all rules defined in [Accessors](classes.md#accessors) and [Automatically implemented properties](classes.md#automatically-implemented-properties) apply to indexer accessors as well as to property accessors.</span></span>

<span data-ttu-id="4c8df-1299">Bir Dizin Oluşturucu bildirimi `extern` değiştirici içerdiğinde, Dizin Oluşturucu bir ***dış Dizin Oluşturucu***olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1299">When an indexer declaration includes an `extern` modifier, the indexer is said to be an ***external indexer***.</span></span> <span data-ttu-id="4c8df-1300">Bir dış Dizin Oluşturucu bildirimi gerçek uygulama sağladığından, *accessor_declarations* her biri noktalı virgülle oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1300">Because an external indexer declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

<span data-ttu-id="4c8df-1301">Aşağıdaki örnek, bit dizisindeki tek tek bitlerin erişimine yönelik bir Dizin Oluşturucu uygulayan `BitArray` sınıfını bildirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1301">The example below declares a `BitArray` class that implements an indexer for accessing the individual bits in the bit array.</span></span>
```csharp
using System;

class BitArray
{
    int[] bits;
    int length;

    public BitArray(int length) {
        if (length < 0) throw new ArgumentException();
        bits = new int[((length - 1) >> 5) + 1];
        this.length = length;
    }

    public int Length {
        get { return length; }
    }

    public bool this[int index] {
        get {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            return (bits[index >> 5] & 1 << index) != 0;
        }
        set {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            if (value) {
                bits[index >> 5] |= 1 << index;
            }
            else {
                bits[index >> 5] &= ~(1 << index);
            }
        }
    }
}
```

<span data-ttu-id="4c8df-1302">`BitArray` sınıfının bir örneği, karşılık gelen bir `bool[]` kıyasla önemli ölçüde daha az bellek tüketir (ilk değeri ikinci bir bayt yerine yalnızca bir bit kapladığından), ancak aynı işlemlere `bool[]`olarak izin verir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1302">An instance of the `BitArray` class consumes substantially less memory than a corresponding `bool[]` (since each value of the former occupies only one bit instead of the latter's one byte), but it permits the same operations as a `bool[]`.</span></span>

<span data-ttu-id="4c8df-1303">Aşağıdaki `CountPrimes` sınıfı, 1 ila verilen bir maksimum değer arasındaki primin sayısını hesaplamak için bir `BitArray` ve klasik "sıya" algoritmasını kullanır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1303">The following `CountPrimes` class uses a `BitArray` and the classical "sieve" algorithm to compute the number of primes between 1 and a given maximum:</span></span>
```csharp
class CountPrimes
{
    static int Count(int max) {
        BitArray flags = new BitArray(max + 1);
        int count = 1;
        for (int i = 2; i <= max; i++) {
            if (!flags[i]) {
                for (int j = i * 2; j <= max; j += i) flags[j] = true;
                count++;
            }
        }
        return count;
    }

    static void Main(string[] args) {
        int max = int.Parse(args[0]);
        int count = Count(max);
        Console.WriteLine("Found {0} primes between 1 and {1}", count, max);
    }
}
```

<span data-ttu-id="4c8df-1304">`BitArray` öğelerine erişim sözdiziminin tam olarak bir `bool[]`ile aynı olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1304">Note that the syntax for accessing elements of the `BitArray` is precisely the same as for a `bool[]`.</span></span>

<span data-ttu-id="4c8df-1305">Aşağıdaki örnek, iki parametreli bir dizin oluşturucuya sahip bir 26 \* 10 Grid sınıfı gösterir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1305">The following example shows a 26 \* 10 grid class that has an indexer with two parameters.</span></span> <span data-ttu-id="4c8df-1306">İlk parametrenin A-Z aralığında bir büyük veya küçük harf olması gerekir ve ikincisinin 0-9 aralığında bir tamsayı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1306">The first parameter is required to be an upper- or lowercase letter in the range A-Z, and the second is required to be an integer in the range 0-9.</span></span>

```csharp
using System;

class Grid
{
    const int NumRows = 26;
    const int NumCols = 10;

    int[,] cells = new int[NumRows, NumCols];

    public int this[char c, int col] {
        get {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            return cells[c - 'A', col];
        }

        set {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            cells[c - 'A', col] = value;
        }
    }
}
```

### <a name="indexer-overloading"></a><span data-ttu-id="4c8df-1307">Dizin Oluşturucu aşırı yüklemesi</span><span class="sxs-lookup"><span data-stu-id="4c8df-1307">Indexer overloading</span></span>

<span data-ttu-id="4c8df-1308">Dizin Oluşturucu aşırı yükleme çözümleme kuralları [tür çıkarımı](expressions.md#type-inference)' nda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1308">The indexer overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="operators"></a><span data-ttu-id="4c8df-1309">İşleçler</span><span class="sxs-lookup"><span data-stu-id="4c8df-1309">Operators</span></span>

<span data-ttu-id="4c8df-1310">***İşleci*** , sınıfının örneklerine uygulanabilen bir ifade işlecinin anlamını tanımlayan bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1310">An ***operator*** is a member that defines the meaning of an expression operator that can be applied to instances of the class.</span></span> <span data-ttu-id="4c8df-1311">İşleçler *operator_declaration*s kullanılarak bildirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1311">Operators are declared using *operator_declaration*s:</span></span>

```antlr
operator_declaration
    : attributes? operator_modifier+ operator_declarator operator_body
    ;

operator_modifier
    : 'public'
    | 'static'
    | 'extern'
    | operator_modifier_unsafe
    ;

operator_declarator
    : unary_operator_declarator
    | binary_operator_declarator
    | conversion_operator_declarator
    ;

unary_operator_declarator
    : type 'operator' overloadable_unary_operator '(' type identifier ')'
    ;

overloadable_unary_operator
    : '+' | '-' | '!' | '~' | '++' | '--' | 'true' | 'false'
    ;

binary_operator_declarator
    : type 'operator' overloadable_binary_operator '(' type identifier ',' type identifier ')'
    ;

overloadable_binary_operator
    : '+'   | '-'   | '*'   | '/'   | '%'   | '&'   | '|'   | '^'   | '<<'
    | right_shift | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
    ;

conversion_operator_declarator
    : 'implicit' 'operator' type '(' type identifier ')'
    | 'explicit' 'operator' type '(' type identifier ')'
    ;

operator_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

<span data-ttu-id="4c8df-1312">Fazla yüklenebilir operatörlerin üç kategorisi vardır: Birli İşleçler ([Birli İşleçler](classes.md#unary-operators)), ikili Işleçler ([ikili işleçler](classes.md#binary-operators)) ve dönüştürme işleçleri ([dönüştürme işleçleri](classes.md#conversion-operators)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-1312">There are three categories of overloadable operators: Unary operators ([Unary operators](classes.md#unary-operators)), binary operators ([Binary operators](classes.md#binary-operators)), and conversion operators ([Conversion operators](classes.md#conversion-operators)).</span></span>

<span data-ttu-id="4c8df-1313">*Operator_body* noktalı virgül, ***deyim gövdesi*** veya ***ifade gövdesi***.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1313">The *operator_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="4c8df-1314">Deyim gövdesi, işleç çağrıldığında yürütülecek deyimleri belirten bir *bloğundan*oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1314">A statement body consists of a *block*, which specifies the statements to execute when the operator is invoked.</span></span> <span data-ttu-id="4c8df-1315">*Blok* , [Yöntem gövdesinde](classes.md#method-body)açıklanan değer döndüren yöntemlere yönelik kurallara uymalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1315">The *block* must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="4c8df-1316">Bir ifade gövdesi, bir ifadenin ve noktalı virgülden oluşan `=>` oluşur ve işleç çağrıldığında gerçekleştirilecek tek bir ifadeyi gösterir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1316">An expression body consists of `=>` followed by an expression and a semicolon, and denotes a single expression to perform when the operator is invoked.</span></span>

<span data-ttu-id="4c8df-1317">`extern` işleçleri için, *operator_body* yalnızca noktalı virgülden oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1317">For `extern` operators, the *operator_body* consists simply of a semicolon.</span></span> <span data-ttu-id="4c8df-1318">Diğer tüm işleçler için *operator_body* bir blok gövdesi ya da bir ifade gövdesidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1318">For all other operators, the *operator_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="4c8df-1319">Tüm işleç bildirimleri için aşağıdaki kurallar geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1319">The following rules apply to all operator declarations:</span></span>

*  <span data-ttu-id="4c8df-1320">İşleç bildirimi hem `public` hem de `static` değiştiricisini içermelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1320">An operator declaration must include both a `public` and a `static` modifier.</span></span>
*  <span data-ttu-id="4c8df-1321">Bir işlecin parametre (ler) i değer parametreleri olmalıdır ([değer parametreleri](variables.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-1321">The parameter(s) of an operator must be value parameters ([Value parameters](variables.md#value-parameters)).</span></span> <span data-ttu-id="4c8df-1322">`ref` veya `out` parametrelerini belirtmek için bir operatör bildirimine yönelik derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1322">It is a compile-time error for an operator declaration to specify `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="4c8df-1323">Bir işlecin imzası ([Birli İşleçler](classes.md#unary-operators), [ikili işleçler](classes.md#binary-operators), [dönüştürme işleçleri](classes.md#conversion-operators)) aynı sınıfta belirtilen diğer tüm işleçlerin imzalarından farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1323">The signature of an operator ([Unary operators](classes.md#unary-operators), [Binary operators](classes.md#binary-operators), [Conversion operators](classes.md#conversion-operators)) must differ from the signatures of all other operators declared in the same class.</span></span>
*  <span data-ttu-id="4c8df-1324">Bir işleç bildiriminde başvurulan tüm türlerin en az işlecin kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1324">All types referenced in an operator declaration must be at least as accessible as the operator itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>
*  <span data-ttu-id="4c8df-1325">Aynı değiştiricinin bir operatör bildiriminde birden çok kez görünmesi bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1325">It is an error for the same modifier to appear multiple times in an operator declaration.</span></span>

<span data-ttu-id="4c8df-1326">Her operatör kategorisi, aşağıdaki bölümlerde açıklandığı gibi ek kısıtlamalar uygular.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1326">Each operator category imposes additional restrictions, as described in the following sections.</span></span>

<span data-ttu-id="4c8df-1327">Diğer Üyeler gibi, bir temel sınıfta belirtilen operatörler türetilmiş sınıflar tarafından devralınır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1327">Like other members, operators declared in a base class are inherited by derived classes.</span></span> <span data-ttu-id="4c8df-1328">İşleç bildirimleri, işlecin, işlecin imzasına katılması için bildirildiği sınıf veya yapının her zaman gerektirdiğinden, bir temel sınıfta belirtilen bir işleci gizlemek için türetilmiş sınıfta belirtilen bir operatör mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1328">Because operator declarations always require the class or struct in which the operator is declared to participate in the signature of the operator, it is not possible for an operator declared in a derived class to hide an operator declared in a base class.</span></span> <span data-ttu-id="4c8df-1329">Bu nedenle, `new` değiştiricisi hiçbir şekilde gerektirmez ve bu nedenle bir işleç bildiriminde hiçbir şekilde izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1329">Thus, the `new` modifier is never required, and therefore never permitted, in an operator declaration.</span></span>

<span data-ttu-id="4c8df-1330">Birli ve ikili işleçlerle ilgili ek bilgiler, [işleçlerinde](expressions.md#operators)bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1330">Additional information on unary and binary operators can be found in [Operators](expressions.md#operators).</span></span>

<span data-ttu-id="4c8df-1331">Dönüştürme işleçleri hakkında ek bilgi, [Kullanıcı tanımlı dönüştürmelerde](conversions.md#user-defined-conversions)bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1331">Additional information on conversion operators can be found in [User-defined conversions](conversions.md#user-defined-conversions).</span></span>

### <a name="unary-operators"></a><span data-ttu-id="4c8df-1332">Birli İşleçler</span><span class="sxs-lookup"><span data-stu-id="4c8df-1332">Unary operators</span></span>

<span data-ttu-id="4c8df-1333">Aşağıdaki kurallar, `T` birli operatör bildirimleri için geçerlidir; burada, işleç bildirimini içeren sınıfın veya yapının örnek türünü belirtir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1333">The following rules apply to unary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="4c8df-1334">Birli `+`, `-`, `!`veya `~` işleci, `T` veya `T?` türünde tek bir parametre almalıdır ve herhangi bir tür döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1334">A unary `+`, `-`, `!`, or `~` operator must take a single parameter of type `T` or `T?` and can return any type.</span></span>
*  <span data-ttu-id="4c8df-1335">Birli `++` veya `--` işleci, `T` veya `T?` türünde tek bir parametre almalıdır ve aynı türde veya ondan türetilmiş bir tür döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1335">A unary `++` or `--` operator must take a single parameter of type `T` or `T?` and must return that same type or a type derived from it.</span></span>
*  <span data-ttu-id="4c8df-1336">Birli `true` veya `false` işleci, `T` veya `T?` türünde tek bir parametre almalıdır ve tür `bool`döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1336">A unary `true` or `false` operator must take a single parameter of type `T` or `T?` and must return type `bool`.</span></span>

<span data-ttu-id="4c8df-1337">Birli işlecin imzası, işleç belirtecinden (`+`, `-`, `!`, `~`, `++`, `--`, `true`veya `false`) ve tek biçimsel parametrenin türünden oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1337">The signature of a unary operator consists of the operator token (`+`, `-`, `!`, `~`, `++`, `--`, `true`, or `false`) and the type of the single formal parameter.</span></span> <span data-ttu-id="4c8df-1338">Dönüş türü birli işlecin imzasının bir parçası değildir, ya da biçimsel parametrenin adıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1338">The return type is not part of a unary operator's signature, nor is the name of the formal parameter.</span></span>

<span data-ttu-id="4c8df-1339">`true` ve `false` Birli İşleçler çift yönlü bildirim gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1339">The `true` and `false` unary operators require pair-wise declaration.</span></span> <span data-ttu-id="4c8df-1340">Bir sınıf bu işleçlerden birini diğeri de bildirmeksizin bildirirse bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1340">A compile-time error occurs if a class declares one of these operators without also declaring the other.</span></span> <span data-ttu-id="4c8df-1341">`true` ve `false` işleçleri, [Kullanıcı tanımlı Koşullu mantıksal işleçler](expressions.md#user-defined-conditional-logical-operators) ve [Boole ifadelerinde](expressions.md#boolean-expressions)daha ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1341">The `true` and `false` operators are described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators) and [Boolean expressions](expressions.md#boolean-expressions).</span></span>

<span data-ttu-id="4c8df-1342">Aşağıdaki örnek, bir tamsayı vektör sınıfı için bir uygulama ve sonraki `operator ++` kullanımını gösterir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1342">The following example shows an implementation and subsequent usage of `operator ++` for an integer vector class:</span></span>
```csharp
public class IntVector
{
    public IntVector(int length) {...}

    public int Length {...}                 // read-only property

    public int this[int index] {...}        // read-write indexer

    public static IntVector operator ++(IntVector iv) {
        IntVector temp = new IntVector(iv.Length);
        for (int i = 0; i < iv.Length; i++)
            temp[i] = iv[i] + 1;
        return temp;
    }
}

class Test
{
    static void Main() {
        IntVector iv1 = new IntVector(4);    // vector of 4 x 0
        IntVector iv2;

        iv2 = iv1++;    // iv2 contains 4 x 0, iv1 contains 4 x 1
        iv2 = ++iv1;    // iv2 contains 4 x 2, iv1 contains 4 x 2
    }
}
```

<span data-ttu-id="4c8df-1343">İşleç yönteminin, sonek artırma ve azaltma işleçleri ([sonek](expressions.md#postfix-increment-and-decrement-operators)artışı ve azaltma işleçleri) ve ön ek artırma ve azaltma Işleçleri ([önek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)) gibi, işlenene 1 eklenerek üretilen değeri nasıl döndürdüğünü aklınızda.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1343">Note how the operator method returns the value produced by adding 1 to the operand, just like the  postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)), and the prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="4c8df-1344">Uygulamasının aksine C++, bu yöntemin işlenenin değerini doğrudan değiştirmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1344">Unlike in C++, this method need not modify the value of its operand directly.</span></span> <span data-ttu-id="4c8df-1345">Aslında, işlenen değerini değiştirmek, sonek artışı işlecinin standart semantiğini ihlal ediyor.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1345">In fact, modifying the operand value would violate the standard semantics of the postfix increment operator.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="4c8df-1346">İkili işleçler</span><span class="sxs-lookup"><span data-stu-id="4c8df-1346">Binary operators</span></span>

<span data-ttu-id="4c8df-1347">Aşağıdaki kurallar, `T` işleç bildirimini içeren sınıfın veya yapının örnek türünü belirten ikili işleç bildirimleri için geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1347">The following rules apply to binary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="4c8df-1348">İkili vardiya olmayan bir operatör iki parametre almalıdır, en az biri tür `T` veya `T?`olmalıdır ve herhangi bir tür döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1348">A binary non-shift operator must take two parameters, at least one of which must have type `T` or `T?`, and can return any type.</span></span>
*  <span data-ttu-id="4c8df-1349">Bir ikili `<<` veya `>>` işleci iki parametre almalıdır. Bu, ilki tür `T` ya da `T?` ve ikincisinin türü `int` veya `int?`olmalıdır ve herhangi bir tür döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1349">A binary `<<` or `>>` operator must take two parameters, the first of which must have type `T` or `T?` and the second of which must have type `int` or `int?`, and can return any type.</span></span>

<span data-ttu-id="4c8df-1350">Bir ikili işlecinin imzası, işleç belirtecinden (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`veya `<=`) ve iki biçimsel parametrenin türlerinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1350">The signature of a binary operator consists of the operator token (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, or `<=`) and the types of the two formal parameters.</span></span> <span data-ttu-id="4c8df-1351">Dönüş türü ve biçimsel parametrelerin adları bir ikili işlecin imzasının parçası değil.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1351">The return type and the names of the formal parameters are not part of a binary operator's signature.</span></span>

<span data-ttu-id="4c8df-1352">Belirli ikili işleçler çift yönlü bildirim gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1352">Certain binary operators require pair-wise declaration.</span></span> <span data-ttu-id="4c8df-1353">Bir çiftin her iki işlecinin her bildirimi için, çiftin diğer işlecinin eşleşen bir bildirimi olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1353">For every declaration of either operator of a pair, there must be a matching declaration of the other operator of the pair.</span></span> <span data-ttu-id="4c8df-1354">İki işleç bildirimi aynı dönüş türüne ve her parametre için aynı türe sahip olduklarında eşleşir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1354">Two operator declarations match when they have the same return type and the same type for each parameter.</span></span> <span data-ttu-id="4c8df-1355">Aşağıdaki işleçler çift yönlü bildirim gerektirir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1355">The following operators require pair-wise declaration:</span></span>

*  <span data-ttu-id="4c8df-1356">`operator ==` ve `operator !=`</span><span class="sxs-lookup"><span data-stu-id="4c8df-1356">`operator ==` and `operator !=`</span></span>
*  <span data-ttu-id="4c8df-1357">`operator >` ve `operator <`</span><span class="sxs-lookup"><span data-stu-id="4c8df-1357">`operator >` and `operator <`</span></span>
*  <span data-ttu-id="4c8df-1358">`operator >=` ve `operator <=`</span><span class="sxs-lookup"><span data-stu-id="4c8df-1358">`operator >=` and `operator <=`</span></span>

### <a name="conversion-operators"></a><span data-ttu-id="4c8df-1359">Dönüştürme işleçleri</span><span class="sxs-lookup"><span data-stu-id="4c8df-1359">Conversion operators</span></span>

<span data-ttu-id="4c8df-1360">Bir dönüştürme işleci bildirimi, önceden tanımlanmış örtük ve açık dönüştürmeleri genişleten ***Kullanıcı tanımlı bir dönüştürme*** ([Kullanıcı tanımlı dönüştürmeler](conversions.md#user-defined-conversions)) sunar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1360">A conversion operator declaration introduces a ***user-defined conversion*** ([User-defined conversions](conversions.md#user-defined-conversions)) which augments the pre-defined implicit and explicit conversions.</span></span>

<span data-ttu-id="4c8df-1361">`implicit` anahtar sözcüğünü içeren bir dönüştürme işleci bildirimi, Kullanıcı tanımlı bir örtük dönüştürmeyi tanıtır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1361">A conversion operator declaration that includes the `implicit` keyword introduces a user-defined implicit conversion.</span></span> <span data-ttu-id="4c8df-1362">Örtük dönüştürmeler işlev üyesi etkinleştirmeleri, atama ifadeleri ve atamalar dahil çeşitli durumlarda gerçekleşebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1362">Implicit conversions can occur in a variety of situations, including function member invocations, cast expressions, and assignments.</span></span> <span data-ttu-id="4c8df-1363">Bu, [örtük dönüştürmelerde](conversions.md#implicit-conversions)daha ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1363">This is described further in [Implicit conversions](conversions.md#implicit-conversions).</span></span>

<span data-ttu-id="4c8df-1364">`explicit` anahtar sözcüğünü içeren bir dönüştürme işleci bildirimi, Kullanıcı tanımlı bir açık dönüştürme sağlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1364">A conversion operator declaration that includes the `explicit` keyword introduces a user-defined explicit conversion.</span></span> <span data-ttu-id="4c8df-1365">Dönüştürme ifadelerinde açık dönüştürmeler gerçekleşebilir ve [Açık dönüştürmelerde](conversions.md#explicit-conversions)daha ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1365">Explicit conversions can occur in cast expressions, and are described further in [Explicit conversions](conversions.md#explicit-conversions).</span></span>

<span data-ttu-id="4c8df-1366">Bir dönüştürme işleci, dönüştürme işlecinin parametre türü ile belirtilen bir kaynak türünden, dönüştürme işlecinin dönüş türü tarafından belirtilen bir hedef türüne dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1366">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span>

<span data-ttu-id="4c8df-1367">Belirli bir kaynak türü `S` ve hedef türü `T`, `S` veya `T` null yapılabilir türlerse, `S0` ve `T0` temel türlerine başvurmasına izin verir, aksi takdirde `S0` ve `T0` sırasıyla `S` ve `T` eşittir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1367">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="4c8df-1368">Bir sınıf veya yapının bir kaynak türü `S` bir hedef türüne dönüştürme bildirmesine izin verilir `T` yalnızca aşağıdakilerin tümü doğru ise geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1368">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="4c8df-1369">`S0` ve `T0` farklı türlerdir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1369">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="4c8df-1370">`S0` ya da `T0`, işleç bildiriminin gerçekleştiği sınıf veya yapı türüdür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1370">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="4c8df-1371">Ne `S0` ne de `T0` bir *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1371">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="4c8df-1372">Kullanıcı tanımlı dönüştürmeler hariç olmak üzere, `S` `T` veya `T` ' den `S`' e kadar bir dönüştürme yok.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1372">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="4c8df-1373">Bu kuralların amaçları doğrultusunda, `S` veya `T` ilişkili herhangi bir tür parametresi, diğer türlerle devralma ilişkisi olmayan benzersiz türler olarak değerlendirilir ve bu tür parametrelerinin herhangi bir kısıtlaması yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1373">For the purposes of these rules, any type parameters associated with `S` or `T` are considered to be unique types that have no inheritance relationship with other types, and any constraints on those type parameters are ignored.</span></span>

<span data-ttu-id="4c8df-1374">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-1374">In the example</span></span>
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
<span data-ttu-id="4c8df-1375">sırasıyla .3, `T` ve `int` ve `string` gibi [Dizin oluşturucular](classes.md#indexers)için hiçbir ilişki olmadan benzersiz türler olduğu için ilk iki operatör bildirimi izin verilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1375">the first two operator declarations are permitted because, for the purposes of [Indexers](classes.md#indexers).3, `T` and `int` and `string` respectively are considered unique types with no relationship.</span></span> <span data-ttu-id="4c8df-1376">Ancak, üçüncü işleç bir hatadır çünkü `C<T>` `D<T>`taban sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1376">However, the third operator is an error because `C<T>` is the base class of `D<T>`.</span></span>

<span data-ttu-id="4c8df-1377">İkinci kuraldan, bir dönüştürme işlecinin, işlecin bildirildiği sınıf veya yapı türünden ya da türüne dönüştürülmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1377">From the second rule it follows that a conversion operator must convert either to or from the class or struct type in which the operator is declared.</span></span> <span data-ttu-id="4c8df-1378">Örneğin, bir sınıf veya yapı türü `C`, `C` 'den `int` ve `int` arasında bir dönüştürme tanımlamak, ancak `C`'den `int` 'ye değil.`bool`</span><span class="sxs-lookup"><span data-stu-id="4c8df-1378">For example, it is possible for a class or struct type `C` to define a conversion from `C` to `int` and from `int` to `C`, but not from `int` to `bool`.</span></span>

<span data-ttu-id="4c8df-1379">Önceden tanımlanmış bir dönüştürmeyi doğrudan yeniden tanımlamak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1379">It is not possible to directly redefine a pre-defined conversion.</span></span> <span data-ttu-id="4c8df-1380">Bu nedenle, örtük ve açık dönüştürmeler `object` ve diğer tüm türler arasında zaten mevcut olduğundan, dönüştürme işleçleri `object` veya ' ya dönüştürme yapılamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1380">Thus, conversion operators are not allowed to convert from or to `object` because implicit and explicit conversions already exist between `object` and all other types.</span></span> <span data-ttu-id="4c8df-1381">Benzer şekilde, bir dönüştürme daha sonra zaten mevcut olduğundan, bir dönüştürmenin kaynağı veya hedef türleri diğerinin temel türü olamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1381">Likewise, neither the source nor the target types of a conversion can be a base type of the other, since a conversion would then already exist.</span></span>

<span data-ttu-id="4c8df-1382">Ancak, belirli tür bağımsız değişkenleri için önceden tanımlanmış dönüştürmeler olarak zaten var olan dönüştürmeleri belirtmek üzere genel türlerde işleçler bildirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1382">However, it is possible to declare operators on generic types that, for particular type arguments, specify conversions that already exist as pre-defined conversions.</span></span> <span data-ttu-id="4c8df-1383">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-1383">In the example</span></span>
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
<span data-ttu-id="4c8df-1384">tür `object` `T`için bir tür bağımsız değişkeni olarak belirtildiğinde, İkinci işleç zaten var olan bir dönüştürme (örtük olarak) ve bu nedenle bir açık, her türden `object`) bir dönüşüm olduğunu bildirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1384">when type `object` is specified as a type argument for `T`, the second operator declares a conversion that already exists (an implicit, and therefore also an explicit, conversion exists from any type to type `object`).</span></span>

<span data-ttu-id="4c8df-1385">İki tür arasında önceden tanımlanmış bir dönüştürmenin mevcut olduğu durumlarda, bu türler arasındaki Kullanıcı tanımlı dönüştürmeler yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1385">In cases where a pre-defined conversion exists between two types, any user-defined conversions between those types are ignored.</span></span> <span data-ttu-id="4c8df-1386">Engelle</span><span class="sxs-lookup"><span data-stu-id="4c8df-1386">Specifically:</span></span>

*  <span data-ttu-id="4c8df-1387">Tür `S` `T`tür ' den önceden tanımlanmış örtük dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) varsa, `T` `S` olan tüm Kullanıcı tanımlı dönüştürmeler (örtük veya açık) yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1387">If a pre-defined implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from type `S` to type `T`, all user-defined conversions (implicit or explicit) from `S` to `T` are ignored.</span></span>
*  <span data-ttu-id="4c8df-1388">Tür `S` `T`türü ' nden önceden tanımlanmış bir açık dönüştürme ([Açık dönüştürmeler](conversions.md#explicit-conversions)) varsa, `T` `S` olan Kullanıcı tanımlı açık dönüştürmeler yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1388">If a pre-defined explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) exists from type `S` to type `T`, any user-defined explicit conversions from `S` to `T` are ignored.</span></span> <span data-ttu-id="4c8df-1389">Buna</span><span class="sxs-lookup"><span data-stu-id="4c8df-1389">Furthermore:</span></span>

<span data-ttu-id="4c8df-1390">`T` bir arabirim türü ise, Kullanıcı tanımlı örtük dönüştürmeler `S` `T` yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1390">If `T` is an interface type, user-defined implicit conversions from `S` to `T` are ignored.</span></span>

<span data-ttu-id="4c8df-1391">Aksi takdirde, `S` ile `T` arasında Kullanıcı tanımlı örtük dönüştürmeler yine de göz önünde bulundurulacaktır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1391">Otherwise, user-defined implicit conversions from `S` to `T` are still considered.</span></span>

<span data-ttu-id="4c8df-1392">Tüm türler için `object`, yukarıdaki `Convertible<T>` türü tarafından belirtilen işleçler önceden tanımlanmış dönüşümlerle çakışmaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1392">For all types but `object`, the operators declared by the `Convertible<T>` type above do not conflict with pre-defined conversions.</span></span> <span data-ttu-id="4c8df-1393">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1393">For example:</span></span>
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

<span data-ttu-id="4c8df-1394">Ancak, tür `object`için önceden tanımlı dönüştürmeler, her durumda Kullanıcı tanımlı dönüştürmeleri gizler, ancak bir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1394">However, for type `object`, pre-defined conversions hide the user-defined conversions in all cases but one:</span></span>

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

<span data-ttu-id="4c8df-1395">Kullanıcı tanımlı dönüştürmelerde veya *interface_type*s 'ye dönüştürme yapılamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1395">User-defined conversions are not allowed to convert from or to *interface_type*s.</span></span> <span data-ttu-id="4c8df-1396">Özellikle, bu kısıtlama, bir *interface_type*dönüştürülürken Kullanıcı tanımlı dönüştürmeler olmamasını ve bir *interface_type* dönüştürmenin yalnızca, belirtilen *interface_type*gerçekten uyguladığı durumlarda başarılı olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1396">In particular, this restriction ensures that no user-defined transformations occur when converting to an *interface_type*, and that a conversion to an *interface_type* succeeds only if the object being converted actually implements the specified *interface_type*.</span></span>

<span data-ttu-id="4c8df-1397">Bir dönüştürme işlecinin imzası, kaynak türü ve hedef türünden oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1397">The signature of a conversion operator consists of the source type and the target type.</span></span> <span data-ttu-id="4c8df-1398">(Bunun, dönüş türünün imzaya katıldığı tek üye formu olduğunu unutmayın.) Bir dönüştürme işlecinin `implicit` veya `explicit` sınıflandırması, işlecin imzasının bir parçası değil.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1398">(Note that this is the only form of member for which the return type participates in the signature.) The `implicit` or `explicit` classification of a conversion operator is not part of the operator's signature.</span></span> <span data-ttu-id="4c8df-1399">Bu nedenle, bir sınıf veya yapı aynı kaynak ve hedef türlerine sahip bir `implicit` ve `explicit` dönüştürme işleci bildiremez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1399">Thus, a class or struct cannot declare both an `implicit` and an `explicit` conversion operator with the same source and target types.</span></span>

<span data-ttu-id="4c8df-1400">Genel olarak, Kullanıcı tanımlı örtük dönüştürmeler hiçbir şekilde özel durum oluşturmaz ve hiçbir bilgi kaybedilmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1400">In general, user-defined implicit conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="4c8df-1401">Kullanıcı tanımlı bir dönüştürme özel durumlara (örneğin, kaynak bağımsız değişkeni aralık dışında) veya bilgi kaybına (yüksek sıralı bitleri atma gibi) izin verebileceğinden, bu dönüştürme açık bir dönüştürme olarak tanımlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1401">If a user-defined conversion can give rise to exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as an explicit conversion.</span></span>

<span data-ttu-id="4c8df-1402">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-1402">In the example</span></span>
```csharp
using System;

public struct Digit
{
    byte value;

    public Digit(byte value) {
        if (value < 0 || value > 9) throw new ArgumentException();
        this.value = value;
    }

    public static implicit operator byte(Digit d) {
        return d.value;
    }

    public static explicit operator Digit(byte b) {
        return new Digit(b);
    }
}
```
<span data-ttu-id="4c8df-1403">`Digit` dönüştürme işlemi hiçbir `byte` şekilde özel durum oluşturmaz veya bilgi kaybettiğinden, ancak `Digit` yalnızca bir `byte`olası değerlerinin bir alt kümesini temsil ettiğinden, `byte` 'den `Digit` 'ye dönüştürme açıktır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1403">the conversion from `Digit` to `byte` is implicit because it never throws exceptions or loses information, but the conversion from `byte` to `Digit` is explicit since `Digit` can only represent a subset of the possible values of a `byte`.</span></span>

## <a name="instance-constructors"></a><span data-ttu-id="4c8df-1404">Örnek oluşturucular</span><span class="sxs-lookup"><span data-stu-id="4c8df-1404">Instance constructors</span></span>

<span data-ttu-id="4c8df-1405">***Örnek Oluşturucu*** , bir sınıfın örneğini başlatmak için gereken eylemleri uygulayan bir üyedir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1405">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="4c8df-1406">Örnek oluşturucular *constructor_declaration*s kullanılarak bildirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1406">Instance constructors are declared using *constructor_declaration*s:</span></span>

```antlr
constructor_declaration
    : attributes? constructor_modifier* constructor_declarator constructor_body
    ;

constructor_modifier
    : 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'extern'
    | constructor_modifier_unsafe
    ;

constructor_declarator
    : identifier '(' formal_parameter_list? ')' constructor_initializer?
    ;

constructor_initializer
    : ':' 'base' '(' argument_list? ')'
    | ':' 'this' '(' argument_list? ')'
    ;

constructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="4c8df-1407">*Constructor_declaration* bir dizi *öznitelik* ([öznitelik](attributes.md)), dört erişim değiştiricisinin geçerli bir birleşimi ([erişim değiştiricileri](classes.md#access-modifiers)) ve bir `extern` ([dış Yöntemler](classes.md#external-methods)) değiştiricisi içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1407">A *constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and an `extern` ([External methods](classes.md#external-methods)) modifier.</span></span> <span data-ttu-id="4c8df-1408">Oluşturucu bildiriminin aynı değiştiriciyi birden çok kez içerme izni yoktur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1408">A constructor declaration is not permitted to include the same modifier multiple times.</span></span>

<span data-ttu-id="4c8df-1409">Bir *constructor_declarator* *tanımlayıcısı* , örnek oluşturucusunun bildirildiği sınıfa ad vermelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1409">The *identifier* of a *constructor_declarator* must name the class in which the instance constructor is declared.</span></span> <span data-ttu-id="4c8df-1410">Başka bir ad belirtilmişse, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1410">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="4c8df-1411">Örnek oluşturucusunun isteğe bağlı *formal_parameter_list* , yöntemin *formal_parameter_list* aynı kurallara tabidir ([Yöntemler](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-1411">The optional *formal_parameter_list* of an instance constructor is subject to the same rules as the *formal_parameter_list* of a method ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="4c8df-1412">Biçimsel parametre listesi bir örnek oluşturucusunun imzasını ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)) tanımlar ve aşırı yükleme çözümünün ([tür çıkarımı](expressions.md#type-inference)) bir çağrıdaki belirli bir örnek oluşturucusunu seçtiği işlemi yönetir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1412">The formal parameter list defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of an instance constructor and governs the process whereby overload resolution ([Type inference](expressions.md#type-inference)) selects a particular instance constructor in an invocation.</span></span>

<span data-ttu-id="4c8df-1413">Bir örnek oluşturucusunun *formal_parameter_list* başvuruda bulunulan türlerin her biri, oluşturucunun kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak en az erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1413">Each of the types referenced in the *formal_parameter_list* of an instance constructor must be at least as accessible as the constructor itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="4c8df-1414">İsteğe bağlı *constructor_initializer* , bu örnek oluşturucusunun *constructor_body* verilen deyimleri yürütmeden önce çağrılacak başka bir örnek Oluşturucu belirtir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1414">The optional *constructor_initializer* specifies another instance constructor to invoke before executing the statements given in the *constructor_body* of this instance constructor.</span></span> <span data-ttu-id="4c8df-1415">Bu, [Oluşturucu başlatıcılarda](classes.md#constructor-initializers)daha ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1415">This is described further in [Constructor initializers](classes.md#constructor-initializers).</span></span>

<span data-ttu-id="4c8df-1416">Bir Oluşturucu bildirimi `extern` değiştirici içerdiğinde, Oluşturucu bir ***dış Oluşturucu***olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1416">When a constructor declaration includes an `extern` modifier, the constructor is said to be an ***external constructor***.</span></span> <span data-ttu-id="4c8df-1417">Bir dış Oluşturucu bildirimi gerçek uygulama sağladığından *constructor_body* noktalı virgülden oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1417">Because an external constructor declaration provides no actual implementation, its *constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="4c8df-1418">Diğer tüm oluşturucular için *constructor_body* , sınıfın yeni bir örneğini başlatmak için deyimleri belirten bir *bloğundan* oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1418">For all other constructors, the *constructor_body* consists of a *block* which specifies the statements to initialize a new instance of the class.</span></span> <span data-ttu-id="4c8df-1419">Bu, tam olarak bir `void` dönüş türüne sahip bir örnek yönteminin *bloğuna* karşılık gelir ([Yöntem gövdesi](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-1419">This corresponds exactly to the *block* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="4c8df-1420">Örnek oluşturucular devralınmaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1420">Instance constructors are not inherited.</span></span> <span data-ttu-id="4c8df-1421">Bu nedenle, bir sınıf sınıf içinde gerçekten bildirilenler dışında bir örnek Oluşturucu içermez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1421">Thus, a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="4c8df-1422">Bir sınıf örnek Oluşturucu bildirimleri içermiyorsa, varsayılan bir örnek Oluşturucu otomatik olarak sağlanır ([Varsayılan oluşturucular](classes.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-1422">If a class contains no instance constructor declarations, a default instance constructor is automatically provided ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="4c8df-1423">Örnek oluşturucular *object_creation_expression*s ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)) ve *constructor_initializer*s aracılığıyla çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1423">Instance constructors are invoked by *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)) and through *constructor_initializer*s.</span></span>

### <a name="constructor-initializers"></a><span data-ttu-id="4c8df-1424">Oluşturucu başlatıcıları</span><span class="sxs-lookup"><span data-stu-id="4c8df-1424">Constructor initializers</span></span>

<span data-ttu-id="4c8df-1425">Tüm örnek oluşturucular (`object`sınıfları hariç) örtük olarak, başka bir örnek oluşturucusunun bir *constructor_body*hemen öncesine çağrılmasını içerir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1425">All instance constructors (except those for class `object`) implicitly include an invocation of another instance constructor immediately before the *constructor_body*.</span></span> <span data-ttu-id="4c8df-1426">Örtük olarak çağrılacak Oluşturucu *constructor_initializer*belirlenir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1426">The constructor to implicitly invoke is determined by the *constructor_initializer*:</span></span>

*  <span data-ttu-id="4c8df-1427">`base(argument_list)` veya `base()` formunun bir örnek Oluşturucu başlatıcısı, doğrudan taban sınıftan çağrılabilir bir örnek oluşturucusunun oluşmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1427">An instance constructor initializer of the form `base(argument_list)` or `base()` causes an instance constructor from the direct base class to be invoked.</span></span> <span data-ttu-id="4c8df-1428">Bu Oluşturucu, varsa *argument_list* ve [aşırı yükleme](expressions.md#overload-resolution)çözümlemesi çözüm kuralları kullanılarak seçilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1428">That constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="4c8df-1429">Aday örnek oluşturucular kümesi, doğrudan Taban sınıfında yer alan tüm erişilebilir örnek oluşturuculardan veya doğrudan temel sınıfta hiçbir örnek Oluşturucu bildirilmemiş ise varsayılan oluşturucuda ([Varsayılan oluşturucular](classes.md#default-constructors)) oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1429">The set of candidate instance constructors consists of all accessible instance constructors contained in the direct base class, or the default constructor ([Default constructors](classes.md#default-constructors)), if no instance constructors are declared in the direct base class.</span></span> <span data-ttu-id="4c8df-1430">Bu küme boşsa veya tek bir en iyi örnek Oluşturucu tanımlanamıyorsa, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1430">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span>
*  <span data-ttu-id="4c8df-1431">`this(argument-list)` veya `this()` formunun bir örnek Oluşturucu başlatıcısı, sınıfın kendisinden bir örnek oluşturucusunun çağrılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1431">An instance constructor initializer of the form `this(argument-list)` or `this()` causes an instance constructor from the class itself to be invoked.</span></span> <span data-ttu-id="4c8df-1432">Oluşturucu, varsa *argument_list* ve [aşırı yükleme](expressions.md#overload-resolution)çözümlemesi çözüm kuralları kullanılarak seçilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1432">The constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="4c8df-1433">Aday örnek oluşturucular kümesi, sınıfta belirtilen tüm erişilebilir örnek oluşturuculardan oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1433">The set of candidate instance constructors consists of all accessible instance constructors declared in the class itself.</span></span> <span data-ttu-id="4c8df-1434">Bu küme boşsa veya tek bir en iyi örnek Oluşturucu tanımlanamıyorsa, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1434">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span> <span data-ttu-id="4c8df-1435">Bir örnek Oluşturucu bildirimi, oluşturucuyu çağıran bir Oluşturucu başlatıcısı içeriyorsa, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1435">If an instance constructor declaration includes a constructor initializer that invokes the constructor itself, a compile-time error occurs.</span></span>

<span data-ttu-id="4c8df-1436">Bir örnek oluşturucusunun Oluşturucu başlatıcısı yoksa, `base()` form Oluşturucu başlatıcısı örtülü olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1436">If an instance constructor has no constructor initializer, a constructor initializer of the form `base()` is implicitly provided.</span></span> <span data-ttu-id="4c8df-1437">Bu nedenle, formun örnek Oluşturucu bildirimi</span><span class="sxs-lookup"><span data-stu-id="4c8df-1437">Thus, an instance constructor declaration of the form</span></span>
```csharp
C(...) {...}
```
<span data-ttu-id="4c8df-1438">tam olarak eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="4c8df-1438">is exactly equivalent to</span></span>
```csharp
C(...): base() {...}
```

<span data-ttu-id="4c8df-1439">Örnek Oluşturucu bildiriminin *formal_parameter_list* tarafından verilen parametrelerin kapsamı, bu bildirimin Oluşturucu başlatıcısını içerir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1439">The scope of the parameters given by the *formal_parameter_list* of an instance constructor declaration includes the constructor initializer of that declaration.</span></span> <span data-ttu-id="4c8df-1440">Bu nedenle, bir Oluşturucu başlatıcısının, oluşturucunun parametrelerine erişmesine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1440">Thus, a constructor initializer is permitted to access the parameters of the constructor.</span></span> <span data-ttu-id="4c8df-1441">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1441">For example:</span></span>
```csharp
class A
{
    public A(int x, int y) {}
}

class B: A
{
    public B(int x, int y): base(x + y, x - y) {}
}
```

<span data-ttu-id="4c8df-1442">Örnek Oluşturucu Başlatıcısı oluşturulan örneğe erişemez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1442">An instance constructor initializer cannot access the instance being created.</span></span> <span data-ttu-id="4c8df-1443">Bu nedenle, Oluşturucu başlatıcısının bir bağımsız değişken ifadesinde `this` başvurmak için derleme zamanı hatası, bir bağımsız değişken ifadesi için bir *simple_name*aracılığıyla herhangi bir örnek üyesine başvuruda bulunmak üzere bir derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1443">Therefore it is a compile-time error to reference `this` in an argument expression of the constructor initializer, as is it a compile-time error for an argument expression to reference any instance member through a *simple_name*.</span></span>

### <a name="instance-variable-initializers"></a><span data-ttu-id="4c8df-1444">Örnek değişken başlatıcıları</span><span class="sxs-lookup"><span data-stu-id="4c8df-1444">Instance variable initializers</span></span>

<span data-ttu-id="4c8df-1445">Bir örnek oluşturucusunun Oluşturucu başlatıcısı olmadığında veya `base(...)`form Oluşturucu başlatıcısı varsa, bu Oluşturucu kendi sınıfında belirtilen örnek alanlarının *variable_initializer*s tarafından belirtilen başlatmaları dolaylı olarak gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1445">When an instance constructor has no constructor initializer, or it has a constructor initializer of the form `base(...)`, that constructor implicitly performs the initializations specified by the *variable_initializer*s of the instance fields declared in its class.</span></span> <span data-ttu-id="4c8df-1446">Bu, oluşturucuya girişte ve doğrudan temel sınıf oluşturucusunun örtük çağrılmasıyla önce yürütülen bir atama dizisine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1446">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor and before the implicit invocation of the direct base class constructor.</span></span> <span data-ttu-id="4c8df-1447">Değişken başlatıcıları, sınıf bildiriminde göründükleri metin sırasına göre yürütülür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1447">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span>

### <a name="constructor-execution"></a><span data-ttu-id="4c8df-1448">Oluşturucu yürütme</span><span class="sxs-lookup"><span data-stu-id="4c8df-1448">Constructor execution</span></span>

<span data-ttu-id="4c8df-1449">Değişken başlatıcıları atama ifadelerine dönüştürülür ve bu atama deyimleri, temel sınıf örneği oluşturucusunun çağrısından önce yürütülür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1449">Variable initializers are transformed into assignment statements, and these assignment statements are executed before the invocation of the base class instance constructor.</span></span> <span data-ttu-id="4c8df-1450">Bu sıralama, bu örneğe erişimi olan herhangi bir deyim yürütülmeden önce tüm örnek alanlarının değişken başlatıcılara göre başlatılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1450">This ordering ensures that all instance fields are initialized by their variable initializers before any statements that have access to that instance are executed.</span></span>

<span data-ttu-id="4c8df-1451">Örnek verildiğinde</span><span class="sxs-lookup"><span data-stu-id="4c8df-1451">Given the example</span></span>
```csharp
using System;

class A
{
    public A() {
        PrintFields();
    }

    public virtual void PrintFields() {}
}

class B: A
{
    int x = 1;
    int y;

    public B() {
        y = -1;
    }

    public override void PrintFields() {
        Console.WriteLine("x = {0}, y = {1}", x, y);
    }
}
```
<span data-ttu-id="4c8df-1452">bir `B`örneğini oluşturmak için `new B()` kullanıldığında aşağıdaki çıktı oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1452">when `new B()` is used to create an instance of `B`, the following output is produced:</span></span>
```console
x = 1, y = 0
```

<span data-ttu-id="4c8df-1453">`x` değeri, temel sınıf örneği Oluşturucusu çağrılmadan önce değişken başlatıcısı yürütüldüğü için 1 ' dir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1453">The value of `x` is 1 because the variable initializer is executed before the base class instance constructor is invoked.</span></span> <span data-ttu-id="4c8df-1454">Ancak, `y` değeri 0 ' dır (`int`varsayılan değerdir) çünkü `y` atama, taban sınıf oluşturucusu dönene kadar yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1454">However, the value of `y` is 0 (the default value of an `int`) because the assignment to `y` is not executed until after the base class constructor returns.</span></span>

<span data-ttu-id="4c8df-1455">Örnek değişken başlatıcıları ve Oluşturucu başlatıcıları, *constructor_body*önce otomatik olarak eklenen deyimler olarak düşünmek yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1455">It is useful to think of instance variable initializers and constructor initializers as statements that are automatically inserted before the *constructor_body*.</span></span> <span data-ttu-id="4c8df-1456">Örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-1456">The example</span></span>
```csharp
using System;
using System.Collections;

class A
{
    int x = 1, y = -1, count;

    public A() {
        count = 0;
    }

    public A(int n) {
        count = n;
    }
}

class B: A
{
    double sqrt2 = Math.Sqrt(2.0);
    ArrayList items = new ArrayList(100);
    int max;

    public B(): this(100) {
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        max = n;
    }
}
```
<span data-ttu-id="4c8df-1457">çeşitli değişken başlatıcıları içerir; Ayrıca, her iki formun de Oluşturucu başlatıcıları (`base` ve `this`) içerir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1457">contains several variable initializers; it also contains constructor initializers of both forms (`base` and `this`).</span></span> <span data-ttu-id="4c8df-1458">Örnek, aşağıda gösterilen koda karşılık gelir; burada her yorum otomatik olarak yerleştirilen bir ifadeyi gösterir (otomatik olarak yerleştirilen Oluşturucu etkinleştirmeleri için kullanılan söz dizimi geçerli değildir, ancak yalnızca mekanizmayı göstermeye çalışır).</span><span class="sxs-lookup"><span data-stu-id="4c8df-1458">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement (the syntax used for the automatically inserted constructor invocations isn't valid, but merely serves to illustrate the mechanism).</span></span>

```csharp
using System.Collections;

class A
{
    int x, y, count;

    public A() {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = 0;
    }

    public A(int n) {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = n;
    }
}

class B: A
{
    double sqrt2;
    ArrayList items;
    int max;

    public B(): this(100) {
        B(100);                      // Invoke B(int) constructor
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        sqrt2 = Math.Sqrt(2.0);      // Variable initializer
        items = new ArrayList(100);  // Variable initializer
        A(n - 1);                    // Invoke A(int) constructor
        max = n;
    }
}
```

### <a name="default-constructors"></a><span data-ttu-id="4c8df-1459">Varsayılan oluşturucular</span><span class="sxs-lookup"><span data-stu-id="4c8df-1459">Default constructors</span></span>

<span data-ttu-id="4c8df-1460">Bir sınıf örnek Oluşturucu bildirimleri içermiyorsa, varsayılan bir örnek Oluşturucu otomatik olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1460">If a class contains no instance constructor declarations, a default instance constructor is automatically provided.</span></span> <span data-ttu-id="4c8df-1461">Bu varsayılan Oluşturucu yalnızca doğrudan temel sınıfın parametresiz oluşturucusunu çağırır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1461">That default constructor simply invokes the parameterless constructor of the direct base class.</span></span> <span data-ttu-id="4c8df-1462">Sınıf soyut ise, varsayılan Oluşturucu için belirtilen erişilebilirlik korunur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1462">If the class is abstract then the declared accessibility for the default constructor is protected.</span></span> <span data-ttu-id="4c8df-1463">Aksi takdirde, varsayılan Oluşturucu için belirtilen erişilebilirlik geneldir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1463">Otherwise, the declared accessibility for the default constructor is public.</span></span> <span data-ttu-id="4c8df-1464">Bu nedenle, varsayılan Oluşturucu her zaman formundadır</span><span class="sxs-lookup"><span data-stu-id="4c8df-1464">Thus, the default constructor is always of the form</span></span>

```csharp
protected C(): base() {}
```
<span data-ttu-id="4c8df-1465">veya</span><span class="sxs-lookup"><span data-stu-id="4c8df-1465">or</span></span>
```csharp
public C(): base() {}
```
<span data-ttu-id="4c8df-1466">Burada `C`, sınıfın adıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1466">where `C` is the name of the class.</span></span> <span data-ttu-id="4c8df-1467">Aşırı yükleme çözümlemesi temel sınıf Oluşturucu başlatıcısı için benzersiz bir en iyi aday tespit leyemiyorsa, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1467">If overload resolution is unable to determine a unique best candidate for the base class constructor initializer then a compile-time error occurs.</span></span>

<span data-ttu-id="4c8df-1468">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-1468">In the example</span></span>
```csharp
class Message
{
    object sender;
    string text;
}
```
<span data-ttu-id="4c8df-1469">sınıf örnek Oluşturucu bildirimleri içerdiğinden, varsayılan bir Oluşturucu sağlanır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1469">a default constructor is provided because the class contains no instance constructor declarations.</span></span> <span data-ttu-id="4c8df-1470">Bu nedenle, örnek tam olarak şu şekilde eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="4c8df-1470">Thus, the example is precisely equivalent to</span></span>
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a><span data-ttu-id="4c8df-1471">Özel oluşturucular</span><span class="sxs-lookup"><span data-stu-id="4c8df-1471">Private constructors</span></span>

<span data-ttu-id="4c8df-1472">Bir sınıf `T` yalnızca özel örnek oluşturucular bildirdiğinde, `T` program metni dışındaki sınıfların `T` türetmesini veya doğrudan `T`örneklerini oluşturmasını mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1472">When a class `T` declares only private instance constructors, it is not possible for classes outside the program text of `T` to derive from `T` or to directly create instances of `T`.</span></span> <span data-ttu-id="4c8df-1473">Bu nedenle, bir sınıf yalnızca statik üyeler içeriyorsa ve örneği oluşturulması amaçlanmazsa, boş bir özel örnek Oluşturucu eklemek, örnek oluşturmayı engeller.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1473">Thus, if a class contains only static members and isn't intended to be instantiated, adding an empty private instance constructor will prevent instantiation.</span></span> <span data-ttu-id="4c8df-1474">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1474">For example:</span></span>
```csharp
public class Trig
{
    private Trig() {}        // Prevent instantiation

    public const double PI = 3.14159265358979323846;

    public static double Sin(double x) {...}
    public static double Cos(double x) {...}
    public static double Tan(double x) {...}
}
```

<span data-ttu-id="4c8df-1475">`Trig` sınıfı ilgili yöntemleri ve sabitleri gruplandırır, ancak örneği oluşturulacak şekilde tasarlanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1475">The `Trig` class groups related methods and constants, but is not intended to be instantiated.</span></span> <span data-ttu-id="4c8df-1476">Bu nedenle, tek bir boş özel örnek Oluşturucu bildirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1476">Therefore it declares a single empty private instance constructor.</span></span> <span data-ttu-id="4c8df-1477">Varsayılan oluşturucunun otomatik olarak oluşturulmasını engellemek için en az bir örnek Oluşturucu bildirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1477">At least one instance constructor must be declared to suppress the automatic generation of a default constructor.</span></span>

### <a name="optional-instance-constructor-parameters"></a><span data-ttu-id="4c8df-1478">İsteğe bağlı örnek Oluşturucu parametreleri</span><span class="sxs-lookup"><span data-stu-id="4c8df-1478">Optional instance constructor parameters</span></span>

<span data-ttu-id="4c8df-1479">Oluşturucu başlatıcısının `this(...)` formu, isteğe bağlı örnek oluşturucu parametrelerini uygulamak için yaygın olarak aşırı yükleme ile birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1479">The `this(...)` form of constructor initializer is commonly used in conjunction with overloading to implement optional instance constructor parameters.</span></span> <span data-ttu-id="4c8df-1480">örnekte</span><span class="sxs-lookup"><span data-stu-id="4c8df-1480">In the example</span></span>
```csharp
class Text
{
    public Text(): this(0, 0, null) {}

    public Text(int x, int y): this(x, y, null) {}

    public Text(int x, int y, string s) {
        // Actual constructor implementation
    }
}
```
<span data-ttu-id="4c8df-1481">ilk iki örnek Oluşturucu yalnızca eksik bağımsız değişkenler için varsayılan değerleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1481">the first two instance constructors merely provide the default values for the missing arguments.</span></span> <span data-ttu-id="4c8df-1482">Her ikisi de, yeni örneği başlatma işini gerçekten yapan üçüncü örnek oluşturucuyu çağırmak için bir `this(...)` Oluşturucu başlatıcısı kullanın.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1482">Both use a `this(...)` constructor initializer to invoke the third instance constructor, which actually does the work of initializing the new instance.</span></span> <span data-ttu-id="4c8df-1483">Bu, isteğe bağlı Oluşturucu parametrelerinin etkidir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1483">The effect is that of optional constructor parameters:</span></span>
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a><span data-ttu-id="4c8df-1484">Statik oluşturucular</span><span class="sxs-lookup"><span data-stu-id="4c8df-1484">Static constructors</span></span>

<span data-ttu-id="4c8df-1485">***Statik Oluşturucu*** , kapalı bir sınıf türünü başlatmak için gereken eylemleri uygulayan bir üyedir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1485">A ***static constructor*** is a member that implements the actions required to initialize a closed class type.</span></span> <span data-ttu-id="4c8df-1486">Statik oluşturucular *static_constructor_declaration*s kullanılarak bildirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1486">Static constructors are declared using *static_constructor_declaration*s:</span></span>

```antlr
static_constructor_declaration
    : attributes? static_constructor_modifiers identifier '(' ')' static_constructor_body
    ;

static_constructor_modifiers
    : 'extern'? 'static'
    | 'static' 'extern'?
    | static_constructor_modifiers_unsafe
    ;

static_constructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="4c8df-1487">Bir *static_constructor_declaration* bir *öznitelikler* kümesi ([öznitelikler](attributes.md)) ve bir `extern` değiştirici ([dış Yöntemler](classes.md#external-methods)) içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1487">A *static_constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and an `extern` modifier ([External methods](classes.md#external-methods)).</span></span>

<span data-ttu-id="4c8df-1488">Bir *static_constructor_declaration* *tanımlayıcısı* , statik oluşturucunun bildirildiği sınıfı içermelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1488">The *identifier* of a *static_constructor_declaration* must name the class in which the static constructor is declared.</span></span> <span data-ttu-id="4c8df-1489">Başka bir ad belirtilmişse, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1489">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="4c8df-1490">Statik Oluşturucu bildirimi `extern` değiştirici içerdiğinde, statik oluşturucu bir ***dış statik Oluşturucu***olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1490">When a static constructor declaration includes an `extern` modifier, the static constructor is said to be an ***external static constructor***.</span></span> <span data-ttu-id="4c8df-1491">Dış bir statik Oluşturucu bildirimi gerçek uygulama sağladığından *static_constructor_body* noktalı virgülden oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1491">Because an external static constructor declaration provides no actual implementation, its *static_constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="4c8df-1492">Diğer tüm statik Oluşturucu bildirimleri için *static_constructor_body* , sınıfı başlatmak için yürütülecek deyimleri belirten bir *bloğundan* oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1492">For all other static constructor declarations, the *static_constructor_body* consists of a *block* which specifies the statements to execute in order to initialize the class.</span></span> <span data-ttu-id="4c8df-1493">Bu, tam olarak bir `void` dönüş türü ([Yöntem gövdesi](classes.md#method-body)) ile bir statik metodun *method_body* karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1493">This corresponds exactly to the *method_body* of a static method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="4c8df-1494">Statik oluşturucular devralınmaz ve doğrudan çağrılamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1494">Static constructors are not inherited, and cannot be called directly.</span></span>

<span data-ttu-id="4c8df-1495">Kapalı bir sınıf türü için statik oluşturucu, belirli bir uygulama etki alanında en çok bir kez yürütülür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1495">The static constructor for a closed class type executes at most once in a given application domain.</span></span> <span data-ttu-id="4c8df-1496">Bir statik oluşturucunun yürütülmesi, bir uygulama etki alanı içinde gerçekleşecek aşağıdaki olayların ilki tarafından tetiklenir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1496">The execution of a static constructor is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="4c8df-1497">Sınıf türünün bir örneği oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1497">An instance of the class type is created.</span></span>
*  <span data-ttu-id="4c8df-1498">Sınıf türünün statik üyelerinden herhangi birine başvurulur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1498">Any of the static members of the class type are referenced.</span></span>

<span data-ttu-id="4c8df-1499">Bir sınıf yürütmenin başladığı `Main` yöntemini ([uygulama başlatma](basic-concepts.md#application-startup)) içeriyorsa, bu sınıfın statik oluşturucusu, `Main` yöntemi çağrılmadan önce yürütülür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1499">If a class contains the `Main` method ([Application Startup](basic-concepts.md#application-startup)) in which execution begins, the static constructor for that class executes before the `Main` method is called.</span></span>

<span data-ttu-id="4c8df-1500">Yeni bir kapalı sınıf türünü başlatmak için, önce bu kapalı tür için yeni bir statik alanlar kümesi ([statik ve örnek alanları](classes.md#static-and-instance-fields)) oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1500">To initialize a new closed class type, first a new set of static fields ([Static and instance fields](classes.md#static-and-instance-fields)) for that particular closed type is created.</span></span> <span data-ttu-id="4c8df-1501">Statik alanların her biri varsayılan değerine ([varsayılan değerler](variables.md#default-values)) başlatılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1501">Each of the static fields is initialized to its default value ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="4c8df-1502">Daha sonra, statik alan başlatıcıları ([statik alan başlatma](classes.md#static-field-initialization)) Bu statik alanlar için yürütülür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1502">Next, the static field initializers ([Static field initialization](classes.md#static-field-initialization)) are executed for those static fields.</span></span> <span data-ttu-id="4c8df-1503">Son olarak, statik Oluşturucu yürütülür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1503">Finally, the static constructor is executed.</span></span>

<span data-ttu-id="4c8df-1504">Örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-1504">The example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        A.F();
        B.F();
    }
}

class A
{
    static A() {
        Console.WriteLine("Init A");
    }
    public static void F() {
        Console.WriteLine("A.F");
    }
}

class B
{
    static B() {
        Console.WriteLine("Init B");
    }
    public static void F() {
        Console.WriteLine("B.F");
    }
}
```
<span data-ttu-id="4c8df-1505">Çıktının üretilmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1505">must produce the output:</span></span>
```console
Init A
A.F
Init B
B.F
```
<span data-ttu-id="4c8df-1506">`A`statik oluşturucusunun yürütülmesi `A.F`çağrısı tarafından tetiklendiğinden ve `B`statik oluşturucusunun yürütülmesi `B.F`çağrısıyla tetikleniyor.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1506">because the execution of `A`'s static constructor is triggered by the call to `A.F`, and the execution of `B`'s static constructor is triggered by the call to `B.F`.</span></span>

<span data-ttu-id="4c8df-1507">Değişken başlatıcıların varsayılan değer durumunda gözlenecek statik alanlara izin veren dairesel bağımlılıklar oluşturmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1507">It is possible to construct circular dependencies that allow static fields with variable initializers to be observed in their default value state.</span></span>

<span data-ttu-id="4c8df-1508">Örnek</span><span class="sxs-lookup"><span data-stu-id="4c8df-1508">The example</span></span>
```csharp
using System;

class A
{
    public static int X;

    static A() {
        X = B.Y + 1;
    }
}

class B
{
    public static int Y = A.X + 1;

    static B() {}

    static void Main() {
        Console.WriteLine("X = {0}, Y = {1}", A.X, B.Y);
    }
}
```
<span data-ttu-id="4c8df-1509">çıktıyı üretir</span><span class="sxs-lookup"><span data-stu-id="4c8df-1509">produces the output</span></span>
```console
X = 1, Y = 2
```

<span data-ttu-id="4c8df-1510">`Main` yöntemini yürütmek için, sistem ilk olarak, sınıf `B`statik Oluşturucusu öncesinde, `B.Y`için başlatıcıyı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1510">To execute the `Main` method, the system first runs the initializer for `B.Y`, prior to class `B`'s static constructor.</span></span> <span data-ttu-id="4c8df-1511">`Y`başlatıcısı, `A.X` değerine başvurulduğundan `A`statik oluşturucusunun çalışmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1511">`Y`'s initializer causes `A`'s static constructor to be run because the value of `A.X` is referenced.</span></span> <span data-ttu-id="4c8df-1512"> `A` statik Oluşturucusu `X`değerini hesaplamak için devam eder ve bunun yapılması, sıfır olan `Y`varsayılan değerini getirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1512">The static constructor of `A` in turn proceeds to compute the value of `X`, and in doing so fetches the default value of `Y`, which is zero.</span></span> <span data-ttu-id="4c8df-1513">`A.X`, bu nedenle 1 olarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1513">`A.X` is thus initialized to 1.</span></span> <span data-ttu-id="4c8df-1514">`A`statik alan başlatıcıları ve statik oluşturucuyu çalıştırma işlemi daha sonra, `Y`başlangıç değerinin hesaplamasına dönerek, sonucu 2 olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1514">The process of running `A`'s static field initializers and static constructor then completes, returning to the calculation of the initial value of `Y`, the result of which becomes 2.</span></span>

<span data-ttu-id="4c8df-1515">Statik Oluşturucu her bir kapalı oluşturulmuş sınıf türü için tam olarak bir kez yürütüldüğü için, tür parametresinde, kısıtlamalar aracılığıyla ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) denetlenemeyen çalışma zamanı denetimlerini zorlamak için kullanışlı bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1515">Because the static constructor is executed exactly once for each closed constructed class type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="4c8df-1516">Örneğin, aşağıdaki tür, tür bağımsız değişkeninin bir sabit listesi olmasını zorlamak için bir statik Oluşturucu kullanır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1516">For example, the following type uses a static constructor to enforce that the type argument is an enum:</span></span>
```csharp
class Gen<T> where T: struct
{
    static Gen() {
        if (!typeof(T).IsEnum) {
            throw new ArgumentException("T must be an enum");
        }
    }
}
```

## <a name="destructors"></a><span data-ttu-id="4c8df-1517">Yıkıcılar</span><span class="sxs-lookup"><span data-stu-id="4c8df-1517">Destructors</span></span>

<span data-ttu-id="4c8df-1518">***Yıkıcı*** , bir sınıf örneğinin yapısını kaldırma için gereken eylemleri uygulayan bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1518">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="4c8df-1519">Yıkıcı bir *destructor_declaration*kullanılarak bildirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1519">A destructor is declared using a *destructor_declaration*:</span></span>

```antlr
destructor_declaration
    : attributes? 'extern'? '~' identifier '(' ')' destructor_body
    | destructor_declaration_unsafe
    ;

destructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="4c8df-1520">Bir *destructor_declaration* bir *öznitelikler* kümesi içerebilir ([öznitelikler](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="4c8df-1520">A *destructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="4c8df-1521">Bir *destructor_declaration* *tanımlayıcısı* , yok edicinin bildirildiği sınıfı isimlendiremelidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1521">The *identifier* of a *destructor_declaration* must name the class in which the destructor is declared.</span></span> <span data-ttu-id="4c8df-1522">Başka bir ad belirtilmişse, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1522">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="4c8df-1523">Yıkıcı bildirimi `extern` değiştirici içerdiğinde, yok edicinin ***dış yıkıcı***olduğu söylenir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1523">When a destructor declaration includes an `extern` modifier, the destructor is said to be an ***external destructor***.</span></span> <span data-ttu-id="4c8df-1524">Bir dış yıkıcı bildirimi gerçek uygulama sunmadığından, *destructor_body* noktalı virgülden oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1524">Because an external destructor declaration provides no actual implementation, its *destructor_body* consists of a semicolon.</span></span> <span data-ttu-id="4c8df-1525">Diğer tüm yok ediciler için *destructor_body* , sınıfın bir örneğinin çıkarılması için yürütülecek deyimleri belirten bir *bloğundan* oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1525">For all other destructors, the *destructor_body* consists of a *block* which specifies the statements to execute in order to destruct an instance of the class.</span></span> <span data-ttu-id="4c8df-1526">Bir *destructor_body* , bir örnek yönteminin `void` dönüş türüne ([Yöntem gövdesi](classes.md#method-body)) sahip *method_body* tam olarak karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1526">A *destructor_body* corresponds exactly to the *method_body* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="4c8df-1527">Yok ediciler devralınmaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1527">Destructors are not inherited.</span></span> <span data-ttu-id="4c8df-1528">Bu nedenle, bir sınıfta bu sınıfta bildirilebilecek olandan başka yok edicisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1528">Thus, a class has no destructors other than the one which may be declared in that class.</span></span>

<span data-ttu-id="4c8df-1529">Yok edicinin parametresi olmaması gerektiğinden aşırı yüklenemez, bu nedenle bir sınıf, en fazla bir yıkıcıya sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1529">Since a destructor is required to have no parameters, it cannot be overloaded, so a class can have, at most, one destructor.</span></span>

<span data-ttu-id="4c8df-1530">Yok ediciler otomatik olarak çağrılır ve açıkça çağrılamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1530">Destructors are invoked automatically, and cannot be invoked explicitly.</span></span> <span data-ttu-id="4c8df-1531">Bir örnek, hiçbir kodun bu örneği kullanabilmesi için artık mümkün olmadığında yok etme için uygun hale gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1531">An instance becomes eligible for destruction when it is no longer possible for any code to use that instance.</span></span> <span data-ttu-id="4c8df-1532">Örnek için yıkıcının yürütülmesi, örnek yok etme için uygun hale geldikten sonra herhangi bir zamanda gerçekleşebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1532">Execution of the destructor for the instance may occur at any time after the instance becomes eligible for destruction.</span></span> <span data-ttu-id="4c8df-1533">Bir örnek çıkarlandığınızda, bu örneğin devralma zincirindeki yok ediciler, en çok türetilen ve en az türetilmiş olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1533">When an instance is destructed, the destructors in that instance's inheritance chain are called, in order, from most derived to least derived.</span></span> <span data-ttu-id="4c8df-1534">Herhangi bir iş parçacığında yıkıcı çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1534">A destructor may be executed on any thread.</span></span> <span data-ttu-id="4c8df-1535">Yok edicinin ne zaman ve nasıl yürütüleceğini belirleyen kurallara ilişkin daha fazla tartışma için bkz. [Otomatik bellek yönetimi](basic-concepts.md#automatic-memory-management).</span><span class="sxs-lookup"><span data-stu-id="4c8df-1535">For further discussion of the rules that govern when and how a destructor is executed, see [Automatic memory management](basic-concepts.md#automatic-memory-management).</span></span>

<span data-ttu-id="4c8df-1536">Örneğin çıktısı</span><span class="sxs-lookup"><span data-stu-id="4c8df-1536">The output of the example</span></span>
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("A's destructor");
    }
}

class B: A
{
    ~B() {
        Console.WriteLine("B's destructor");
    }
}

class Test
{
   static void Main() {
        B b = new B();
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
   }
}
```
<span data-ttu-id="4c8df-1537">is</span><span class="sxs-lookup"><span data-stu-id="4c8df-1537">is</span></span>
```
B's destructor
A's destructor
```
<span data-ttu-id="4c8df-1538">devralma zincirindeki yok ediciler sırasıyla, en çok türetilen ve en az türeten olarak çağrıldıklarından.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1538">since destructors in an inheritance chain are called in order, from most derived to least derived.</span></span>

<span data-ttu-id="4c8df-1539">Yok ediciler, `System.Object``Finalize` sanal metot geçersiz kılınarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1539">Destructors are implemented by overriding the virtual method `Finalize` on `System.Object`.</span></span> <span data-ttu-id="4c8df-1540">C#programların bu yöntemi geçersiz kılmasına veya doğrudan çağırmasına (ya da geçersiz kılınmasına) izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1540">C# programs are not permitted to override this method or call it (or overrides of it) directly.</span></span> <span data-ttu-id="4c8df-1541">Örneğin, program</span><span class="sxs-lookup"><span data-stu-id="4c8df-1541">For instance, the program</span></span>
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
<span data-ttu-id="4c8df-1542">iki hata içerir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1542">contains two errors.</span></span>

<span data-ttu-id="4c8df-1543">Derleyici, bu yöntem ve geçersiz kılmalar gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1543">The compiler behaves as if this method, and overrides of it, do not exist at all.</span></span> <span data-ttu-id="4c8df-1544">Bu nedenle, bu program:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1544">Thus, this program:</span></span>
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
<span data-ttu-id="4c8df-1545">geçerlidir ve gösterilen yöntem `System.Object``Finalize` metodunu gizler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1545">is valid, and the method shown hides `System.Object`'s `Finalize` method.</span></span>

<span data-ttu-id="4c8df-1546">Yıkıcıdan bir özel durum oluştuğunda davranış tartışması için bkz. [özel durumların nasıl işlendiği](exceptions.md#how-exceptions-are-handled).</span><span class="sxs-lookup"><span data-stu-id="4c8df-1546">For a discussion of the behavior when an exception is thrown from a destructor, see [How exceptions are handled](exceptions.md#how-exceptions-are-handled).</span></span>

## <a name="iterators"></a><span data-ttu-id="4c8df-1547">Yineleyiciler</span><span class="sxs-lookup"><span data-stu-id="4c8df-1547">Iterators</span></span>

<span data-ttu-id="4c8df-1548">Yineleyici bloğu ([bloklar](statements.md#blocks)) kullanılarak uygulanan bir işlev üyesine ([işlev üyeleri](expressions.md#function-members)) ***Yineleyici***denir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1548">A function member ([Function members](expressions.md#function-members)) implemented using an iterator block ([Blocks](statements.md#blocks)) is called an ***iterator***.</span></span>

<span data-ttu-id="4c8df-1549">Bir yineleyici bloğu, karşılık gelen işlev üyesinin dönüş türü Numaralandırıcı arabirimlerinden ([Numaralandırıcı arabirimleri](classes.md#enumerator-interfaces)) biri veya sıralanabilir arabirimlerin ([sıralanabilir arabirimler](classes.md#enumerable-interfaces)) biri olduğu sürece bir işlev üyesinin gövdesi olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1549">An iterator block may be used as the body of a function member as long as the return type of the corresponding function member is one of the enumerator interfaces ([Enumerator interfaces](classes.md#enumerator-interfaces)) or one of the enumerable interfaces ([Enumerable interfaces](classes.md#enumerable-interfaces)).</span></span> <span data-ttu-id="4c8df-1550">*Method_body*, *operator_body* veya *accessor_body*olarak gerçekleşebilir, ancak olaylar, örnek oluşturucular, statik oluşturucular ve Yıkıcılar yineleyiciler olarak uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1550">It can occur as a *method_body*, *operator_body* or *accessor_body*, whereas events, instance constructors, static constructors and destructors cannot be implemented as iterators.</span></span>

<span data-ttu-id="4c8df-1551">Bir işlev üyesi Yineleyici bloğu kullanılarak uygulandığında, işlev üyesinin biçimsel parametre listesi için, herhangi bir `ref` veya `out` parametresi belirtmek üzere derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1551">When a function member is implemented using an iterator block, it is a compile-time error for the formal parameter list of the function member to specify any `ref` or `out` parameters.</span></span>

### <a name="enumerator-interfaces"></a><span data-ttu-id="4c8df-1552">Numaralandırıcı arabirimleri</span><span class="sxs-lookup"><span data-stu-id="4c8df-1552">Enumerator interfaces</span></span>

<span data-ttu-id="4c8df-1553">***Numaralandırıcı arabirimleri*** genel olmayan arabirim `System.Collections.IEnumerator` ve genel arabirim `System.Collections.Generic.IEnumerator<T>`tüm örneklemelerdir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1553">The ***enumerator interfaces*** are the non-generic interface `System.Collections.IEnumerator` and all instantiations of the generic interface `System.Collections.Generic.IEnumerator<T>`.</span></span> <span data-ttu-id="4c8df-1554">Breçekimi 'nin sake 'ı için bu bölümde sırasıyla `IEnumerator` ve `IEnumerator<T>`olarak başvurulur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1554">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerator` and `IEnumerator<T>`, respectively.</span></span>

### <a name="enumerable-interfaces"></a><span data-ttu-id="4c8df-1555">Sıralanabilir arabirimler</span><span class="sxs-lookup"><span data-stu-id="4c8df-1555">Enumerable interfaces</span></span>

<span data-ttu-id="4c8df-1556">***Sıralanabilir arabirimler*** genel olmayan arabirim `System.Collections.IEnumerable` ve genel arabirim `System.Collections.Generic.IEnumerable<T>`tüm örneklemelerdir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1556">The ***enumerable interfaces*** are the non-generic interface `System.Collections.IEnumerable` and all instantiations of the generic interface `System.Collections.Generic.IEnumerable<T>`.</span></span> <span data-ttu-id="4c8df-1557">Breçekimi 'nin sake 'ı için bu bölümde sırasıyla `IEnumerable` ve `IEnumerable<T>`olarak başvurulur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1557">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerable` and `IEnumerable<T>`, respectively.</span></span>

### <a name="yield-type"></a><span data-ttu-id="4c8df-1558">Yield türü</span><span class="sxs-lookup"><span data-stu-id="4c8df-1558">Yield type</span></span>

<span data-ttu-id="4c8df-1559">Bir yineleyici, hepsi aynı türden bir değer dizisi üretir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1559">An iterator produces a sequence of values, all of the same type.</span></span> <span data-ttu-id="4c8df-1560">Bu tür, yineleyicinin ***yield türü*** olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1560">This type is called the ***yield type*** of the iterator.</span></span>

*  <span data-ttu-id="4c8df-1561">`IEnumerator` veya `IEnumerable` döndüren bir yineleyicinin yield türü `object`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1561">The yield type of an iterator that returns `IEnumerator` or `IEnumerable` is `object`.</span></span>
*  <span data-ttu-id="4c8df-1562">`IEnumerator<T>` veya `IEnumerable<T>` döndüren bir yineleyicinin yield türü `T`.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1562">The yield type of an iterator that returns `IEnumerator<T>` or `IEnumerable<T>` is `T`.</span></span>

### <a name="enumerator-objects"></a><span data-ttu-id="4c8df-1563">Numaralandırıcı nesneleri</span><span class="sxs-lookup"><span data-stu-id="4c8df-1563">Enumerator objects</span></span>

<span data-ttu-id="4c8df-1564">Bir Numaralandırıcı arabirim türü döndüren bir işlev üyesi Yineleyici bloğu kullanılarak uygulandığında, işlev üyesini çağırmak kodu Yineleyici bloğunda hemen yürütmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1564">When a function member returning an enumerator interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="4c8df-1565">Bunun yerine, bir ***Numaralandırıcı nesnesi*** oluşturulup döndürülür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1565">Instead, an ***enumerator object*** is created and returned.</span></span> <span data-ttu-id="4c8df-1566">Bu nesne Yineleyici bloğunda belirtilen kodu kapsüller ve Numaralandırıcı nesnesinin `MoveNext` yöntemi çağrıldığında Yineleyici bloğunda kodun yürütülmesi oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1566">This object encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="4c8df-1567">Bir Numaralandırıcı nesnesi aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1567">An enumerator object has the following characteristics:</span></span>

*  <span data-ttu-id="4c8df-1568">`IEnumerator` ve `IEnumerator<T>`uygular; burada `T`, yineleyicinin yield türüdür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1568">It implements `IEnumerator` and `IEnumerator<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="4c8df-1569">`System.IDisposable`uygular.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1569">It implements `System.IDisposable`.</span></span>
*  <span data-ttu-id="4c8df-1570">Bağımsız değişken değerlerinin bir kopyasıyla başlatılır (varsa) ve örnek değeri işlev üyesine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1570">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>
*  <span data-ttu-id="4c8df-1571">Dört olası durum, daha ***önce***, ***çalışıyor***, ***askıya alındı***ve ***sonra***ilk olarak ***önceki*** durumda olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1571">It has four potential states, ***before***, ***running***, ***suspended***, and ***after***, and is initially in the ***before*** state.</span></span>

<span data-ttu-id="4c8df-1572">Numaralandırıcı nesnesi genellikle Yineleyici bloğundaki kodu kapsülleyen ve Numaralandırıcı arabirimlerini uygulayan derleyici tarafından oluşturulan Numaralandırıcı sınıfının bir örneğidir, ancak diğer uygulama yöntemleri mümkündür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1572">An enumerator object is typically an instance of a compiler-generated enumerator class that encapsulates the code in the iterator block and implements the enumerator interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="4c8df-1573">Bir Numaralandırıcı sınıfı derleyici tarafından oluşturulduysa, bu sınıf doğrudan veya dolaylı olarak işlev üyesini içeren sınıfta iç içe gelir, özel erişilebilirliği olur ve derleyici kullanımı ([tanımlayıcılar](lexical-structure.md#identifiers)) için ayrılmış bir ada sahip olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1573">If an enumerator class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="4c8df-1574">Bir Numaralandırıcı nesnesi, yukarıda belirtilenden daha fazla arabirim uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1574">An enumerator object may implement more interfaces than those specified above.</span></span>

<span data-ttu-id="4c8df-1575">Aşağıdaki bölümlerde, bir Numaralandırıcı nesnesi tarafından sunulan `IEnumerable` ve `IEnumerable<T>` arabirimi uygulamalarının `MoveNext`, `Current`ve `Dispose` üyelerinin tam davranışı açıklanır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1575">The following sections describe the exact behavior of the `MoveNext`, `Current`, and `Dispose` members of the `IEnumerable` and `IEnumerable<T>` interface implementations provided by an enumerator object.</span></span>

<span data-ttu-id="4c8df-1576">Numaralandırıcı nesnelerinin `IEnumerator.Reset` yöntemini desteklemediğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1576">Note that enumerator objects do not support the `IEnumerator.Reset` method.</span></span> <span data-ttu-id="4c8df-1577">Bu yöntemi çağırmak bir `System.NotSupportedException` oluşturulmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1577">Invoking this method causes a `System.NotSupportedException` to be thrown.</span></span>

#### <a name="the-movenext-method"></a><span data-ttu-id="4c8df-1578">MoveNext yöntemi</span><span class="sxs-lookup"><span data-stu-id="4c8df-1578">The MoveNext method</span></span>

<span data-ttu-id="4c8df-1579">Bir Numaralandırıcı nesnesinin `MoveNext` yöntemi bir yineleyici bloğunun kodunu kapsüller.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1579">The `MoveNext` method of an enumerator object encapsulates the code of an iterator block.</span></span> <span data-ttu-id="4c8df-1580">`MoveNext` yöntemini çağırmak Yineleyici bloğunda kodu yürütür ve Numaralandırıcı nesnesinin `Current` özelliğini uygun şekilde ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1580">Invoking the `MoveNext` method executes code in the iterator block and sets the `Current` property of the enumerator object as appropriate.</span></span> <span data-ttu-id="4c8df-1581">`MoveNext` tarafından gerçekleştirilen kesin eylem, `MoveNext` çağrıldığında Numaralandırıcı nesnesinin durumuna bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1581">The precise action performed by `MoveNext` depends on the state of the enumerator object when `MoveNext` is invoked:</span></span>

*  <span data-ttu-id="4c8df-1582">Numaralandırıcı nesnesinin durumu daha ***önce***ise `MoveNext`çağrılıyor:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1582">If the state of the enumerator object is ***before***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="4c8df-1583">Durumu ***çalışıyor***olarak değiştirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1583">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="4c8df-1584">Yineleyici bloğunun parametrelerini (`this`dahil), Numaralandırıcı nesnesi başlatıldığında kaydedilen bağımsız değişken değerlerine ve örnek değerine başlatır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1584">Initializes the parameters (including `this`) of the iterator block to the argument values and instance value saved when the enumerator object was initialized.</span></span>
   * <span data-ttu-id="4c8df-1585">, Yürütme kesintiye gelinceye kadar Yineleyici bloğunu baştan yürütür (aşağıda açıklandığı gibi).</span><span class="sxs-lookup"><span data-stu-id="4c8df-1585">Executes the iterator block from the beginning until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="4c8df-1586">Numaralandırıcı nesnesinin durumu ***çalışıyorsa***, `MoveNext` çağırma sonucu belirtilmemiş olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1586">If the state of the enumerator object is ***running***, the result of invoking `MoveNext` is unspecified.</span></span>
*  <span data-ttu-id="4c8df-1587">Numaralandırıcı nesnesinin durumu ***askıya***alınırsa, `MoveNext`çağrılıyor:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1587">If the state of the enumerator object is ***suspended***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="4c8df-1588">Durumu ***çalışıyor***olarak değiştirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1588">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="4c8df-1589">Tüm yerel değişkenlerin ve parametrelerin (Bu dahil) değerlerini, yineleyici bloğunun yürütülmesi son askıya alındığında kaydedilen değerlere geri yükler.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1589">Restores the values of all local variables and parameters (including this) to the values saved when execution of the iterator block was last suspended.</span></span> <span data-ttu-id="4c8df-1590">Bu değişkenlerin başvurduğu nesnelerin içeriklerinin, önceki MoveNext çağrısından bu yana değişmiş olabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1590">Note that the contents of any objects referenced by these variables may have changed since the previous call to MoveNext.</span></span>
   * <span data-ttu-id="4c8df-1591">Yürütmenin askıya alınmasına neden olan `yield return` deyimin hemen sonrasında Yineleyici bloğunun yürütülmesini sürdürür ve yürütme kesintiye gelinceye kadar devam eder (aşağıda açıklandığı gibi).</span><span class="sxs-lookup"><span data-stu-id="4c8df-1591">Resumes execution of the iterator block immediately following the `yield return` statement that caused the suspension of execution and continues until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="4c8df-1592">Numaralandırıcı nesnesinin durumu ***After***ise, çağırma `MoveNext` `false`döndürür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1592">If the state of the enumerator object is ***after***, invoking `MoveNext` returns `false`.</span></span>


<span data-ttu-id="4c8df-1593">`MoveNext` Yineleyici bloğunu yürüttüğünde, yürütme dört şekilde kesilebilir: bir `yield return` bildirimiyle, bir `yield break` ifadesiyle, yineleyici bloğunun sonuyla karşılaşarak ve bir özel durum oluşan ve yineleyici bloğunun dışına yayıldığında.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1593">When `MoveNext` executes the iterator block, execution can be interrupted in four ways: By a `yield return` statement, by a `yield break` statement, by encountering the end of the iterator block, and by an exception being thrown and propagated out of the iterator block.</span></span>

*  <span data-ttu-id="4c8df-1594">Bir `yield return` ifadesiyle karşılaşıldığında ([yield ekstresi](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="4c8df-1594">When a `yield return` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="4c8df-1595">Deyimde verilen ifade değerlendirilir, örtülü olarak yield türüne dönüştürülür ve Numaralandırıcı nesnesinin `Current` özelliğine atanır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1595">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
   * <span data-ttu-id="4c8df-1596">Yineleyici gövdesinin yürütülmesi askıya alındı.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1596">Execution of the iterator body is suspended.</span></span> <span data-ttu-id="4c8df-1597">Tüm yerel değişkenlerin ve parametrelerin (`this`dahil) değerleri, bu `yield return` ifadesinin konumu olduğu gibi kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1597">The values of all local variables and parameters (including `this`) are saved, as is the location of this `yield return` statement.</span></span> <span data-ttu-id="4c8df-1598">`yield return` deyimin bir veya daha fazla `try` bloğu içindeyse, ilişkili `finally` blokları şu an yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1598">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
   * <span data-ttu-id="4c8df-1599">Numaralandırıcı nesnesinin durumu ***askıya alındı***olarak değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1599">The state of the enumerator object is changed to ***suspended***.</span></span>
   * <span data-ttu-id="4c8df-1600">`MoveNext` yöntemi, yinelemesinin bir sonraki değere başarıyla ilerlemiş olduğunu belirten, çağırana `true` döndürür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1600">The `MoveNext` method returns `true` to its caller, indicating that the iteration successfully advanced to the next value.</span></span>
*  <span data-ttu-id="4c8df-1601">Bir `yield break` ifadesiyle karşılaşıldığında ([yield ekstresi](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="4c8df-1601">When a `yield break` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="4c8df-1602">`yield break` deyimin bir veya daha fazla `try` bloğu içindeyse, ilişkili `finally` blokları yürütülür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1602">If the `yield break` statement is within one or more `try` blocks, the associated `finally` blocks are executed.</span></span>
   * <span data-ttu-id="4c8df-1603">Numaralandırıcı nesnesinin durumu, daha ***sonra***olarak değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1603">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="4c8df-1604">`MoveNext` yöntemi, yinelemesinin tamamlandığını belirten, çağıranına `false` döndürür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1604">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="4c8df-1605">Yineleyici gövdesinin sonuna rastlandı:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1605">When the end of the iterator body is encountered:</span></span>
   * <span data-ttu-id="4c8df-1606">Numaralandırıcı nesnesinin durumu, daha ***sonra***olarak değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1606">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="4c8df-1607">`MoveNext` yöntemi, yinelemesinin tamamlandığını belirten, çağıranına `false` döndürür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1607">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="4c8df-1608">Bir özel durum oluştuğunda ve yineleyici bloğundan yayıldığında:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1608">When an exception is thrown and propagated out of the iterator block:</span></span>
   * <span data-ttu-id="4c8df-1609">Yineleyici gövdesinde uygun `finally` blokları özel durum yayma tarafından yürütülür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1609">Appropriate `finally` blocks in the iterator body will have been executed by the exception propagation.</span></span>
   * <span data-ttu-id="4c8df-1610">Numaralandırıcı nesnesinin durumu, daha ***sonra***olarak değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1610">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="4c8df-1611">Özel durum yayma `MoveNext` yöntemi çağıranına devam eder.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1611">The exception propagation continues to the caller of the `MoveNext` method.</span></span>

#### <a name="the-current-property"></a><span data-ttu-id="4c8df-1612">Geçerli özellik</span><span class="sxs-lookup"><span data-stu-id="4c8df-1612">The Current property</span></span>

<span data-ttu-id="4c8df-1613">Bir Numaralandırıcı nesnenin `Current` özelliği, yineleyici bloğundaki `yield return` deyimlerden etkilenir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1613">An enumerator object's `Current` property is affected by `yield return` statements in the iterator block.</span></span>

<span data-ttu-id="4c8df-1614">Bir Numaralandırıcı nesnesi ***askıya alınma*** durumundaysa, `Current` değeri önceki `MoveNext`çağrısı tarafından ayarlanan değerdir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1614">When an enumerator object is in the ***suspended*** state, the value of `Current` is the value set by the previous call to `MoveNext`.</span></span> <span data-ttu-id="4c8df-1615">Bir Numaralandırıcı nesnesi ***önceki***, ***çalışan***veya ***sonrasında*** olduğunda, `Current` erişimi sonucu belirtilmemiş olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1615">When an enumerator object is in the ***before***, ***running***, or ***after*** states, the result of accessing `Current` is unspecified.</span></span>

<span data-ttu-id="4c8df-1616">`object`dışında bir yield türü olan bir yineleyici için, Numaralandırıcı nesnenin `IEnumerable` uygulamasıyla `Current` erişmenin sonucu, Numaralandırıcı nesnenin `IEnumerator<T>` uygulama aracılığıyla `Current` erişme ve sonucu `object`olarak atama anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1616">For an iterator with a yield type other than `object`, the result of accessing `Current` through the enumerator object's `IEnumerable` implementation corresponds to accessing `Current` through the enumerator object's `IEnumerator<T>` implementation and casting the result to `object`.</span></span>

#### <a name="the-dispose-method"></a><span data-ttu-id="4c8df-1617">Dispose yöntemi</span><span class="sxs-lookup"><span data-stu-id="4c8df-1617">The Dispose method</span></span>

<span data-ttu-id="4c8df-1618">`Dispose` yöntemi, Numaralandırıcı nesnesini ***After*** durumuna getirerek yinelemeyi temizlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1618">The `Dispose` method is used to clean up the iteration by bringing the enumerator object to the ***after*** state.</span></span>

*  <span data-ttu-id="4c8df-1619">Numaralandırıcı nesnesinin durumu daha ***önce***ise, çağırma `Dispose` durumu ***sonra***olarak değiştirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1619">If the state of the enumerator object is ***before***, invoking `Dispose` changes the state to ***after***.</span></span>
*  <span data-ttu-id="4c8df-1620">Numaralandırıcı nesnesinin durumu ***çalışıyorsa***, `Dispose` çağırma sonucu belirtilmemiş olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1620">If the state of the enumerator object is ***running***, the result of invoking `Dispose` is unspecified.</span></span>
*  <span data-ttu-id="4c8df-1621">Numaralandırıcı nesnesinin durumu ***askıya***alınırsa, `Dispose`çağrılıyor:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1621">If the state of the enumerator object is ***suspended***, invoking `Dispose`:</span></span>
   * <span data-ttu-id="4c8df-1622">Durumu ***çalışıyor***olarak değiştirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1622">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="4c8df-1623">Son çalıştırılan `yield return` ifadesiyle `yield break` bir ifade olduğu sürece herhangi bir finally bloğunu yürütür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1623">Executes any finally blocks as if the last executed `yield return` statement were a `yield break` statement.</span></span> <span data-ttu-id="4c8df-1624">Bu durum, yineleyici gövdesinden oluşan bir özel durumun oluşturulmasına ve yayılmasına neden olursa, Numaralandırıcı nesnesinin durumu ***After*** olarak ayarlanır ve özel durum `Dispose` yönteminin çağıranına yayılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1624">If this causes an exception to be thrown and propagated out of the iterator body, the state of the enumerator object is set to ***after*** and the exception is propagated to the caller of the `Dispose` method.</span></span>
   * <span data-ttu-id="4c8df-1625">Durumu ***sonraki***olarak değiştirir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1625">Changes the state to ***after***.</span></span>
*  <span data-ttu-id="4c8df-1626">Numaralandırıcı nesnesinin durumu ***After***ise, çağırma `Dispose`, hiçbir etkisi olmaz.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1626">If the state of the enumerator object is ***after***, invoking `Dispose` has no affect.</span></span>

### <a name="enumerable-objects"></a><span data-ttu-id="4c8df-1627">Sıralanabilir nesneler</span><span class="sxs-lookup"><span data-stu-id="4c8df-1627">Enumerable objects</span></span>

<span data-ttu-id="4c8df-1628">Sıralanabilir bir arabirim türü döndüren bir işlev üyesi Yineleyici bloğu kullanılarak uygulandığında, işlev üyesini çağırmak kodu Yineleyici bloğunda hemen yürütmez.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1628">When a function member returning an enumerable interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="4c8df-1629">Bunun yerine, ***sıralanabilir bir nesne*** oluşturulur ve döndürülür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1629">Instead, an ***enumerable object*** is created and returned.</span></span> <span data-ttu-id="4c8df-1630">Sıralanabilir nesnenin `GetEnumerator` yöntemi, yineleyici bloğunda belirtilen kodu kapsülleyen bir Numaralandırıcı nesnesi döndürür ve Numaralandırıcı nesnesinin `MoveNext` yöntemi çağrıldığında Yineleyici bloğunda kodun yürütülmesi oluşur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1630">The enumerable object's `GetEnumerator` method returns an enumerator object that encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="4c8df-1631">Sıralanabilir bir nesne aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1631">An enumerable object has the following characteristics:</span></span>

*  <span data-ttu-id="4c8df-1632">`IEnumerable` ve `IEnumerable<T>`uygular; burada `T`, yineleyicinin yield türüdür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1632">It implements `IEnumerable` and `IEnumerable<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="4c8df-1633">Bağımsız değişken değerlerinin bir kopyasıyla başlatılır (varsa) ve örnek değeri işlev üyesine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1633">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>

<span data-ttu-id="4c8df-1634">Sıralanabilir bir nesne genellikle Yineleyici bloğunda kodu kapsülleyen ve numaralandırılabilir arabirimleri uygulayan, derleyici tarafından oluşturulan bir sıralanabilir sınıfın örneğidir, ancak diğer uygulama yöntemleri mümkündür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1634">An enumerable object is typically an instance of a compiler-generated enumerable class that encapsulates the code in the iterator block and implements the enumerable interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="4c8df-1635">Bir sıralanabilir sınıf derleyici tarafından oluşturulduysa, bu sınıf doğrudan veya dolaylı olarak işlev üyesini içeren sınıfta iç içe gelir, özel erişilebilirliği olur ve derleyici kullanımı ([tanımlayıcılar](lexical-structure.md#identifiers)) için ayrılmış bir ada sahip olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1635">If an enumerable class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="4c8df-1636">Sıralanabilir bir nesne, yukarıda belirtilenden daha fazla arabirim uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1636">An enumerable object may implement more interfaces than those specified above.</span></span> <span data-ttu-id="4c8df-1637">Özellikle de, sıralanabilir bir nesne `IEnumerator` ve `IEnumerator<T>`uygulayabilir, bu da hem numaralandırılabilir hem de numaralandırıcı olarak işlev görmesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1637">In particular, an enumerable object may also implement `IEnumerator` and `IEnumerator<T>`, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="4c8df-1638">Bu tür bir uygulamada, bir sıralanabilir nesnenin ilk kez `GetEnumerator` yöntemi çağrıldığında, sıralanabilir nesnenin kendisi döndürülür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1638">In that type of implementation, the first time an enumerable object's `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="4c8df-1639">Sıralanabilir nesnenin `GetEnumerator`sonraki etkinleştirmeleri, varsa, sıralanabilir nesnenin bir kopyasını döndürür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1639">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="4c8df-1640">Bu nedenle, döndürülen her Numaralandırıcı kendi durumuna sahiptir ve bir Numaralandırıcı içindeki değişiklikler başka bir Numaralandırıcı olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1640">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span>

#### <a name="the-getenumerator-method"></a><span data-ttu-id="4c8df-1641">GetEnumerator yöntemi</span><span class="sxs-lookup"><span data-stu-id="4c8df-1641">The GetEnumerator method</span></span>

<span data-ttu-id="4c8df-1642">Sıralanabilir bir nesne, `IEnumerable` ve `IEnumerable<T>` arabirimlerinin `GetEnumerator` yöntemlerinin bir uygulamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1642">An enumerable object provides an implementation of the `GetEnumerator` methods of the `IEnumerable` and `IEnumerable<T>` interfaces.</span></span> <span data-ttu-id="4c8df-1643">İki `GetEnumerator` yöntemi, kullanılabilir bir Numaralandırıcı nesnesi elde eden ve döndüren ortak bir uygulamayı paylaşır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1643">The two `GetEnumerator` methods share a common implementation that acquires and returns an available enumerator object.</span></span> <span data-ttu-id="4c8df-1644">Numaralandırıcı nesnesi, sıralanabilir nesne başlatıldığında bağımsız değişken değerleri ve örnek değeri ile başlatılır, aksi takdirde Numaralandırıcı nesnesi, [Numaralandırıcı nesnelerinde](classes.md#enumerator-objects)açıklanan şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1644">The enumerator object is initialized with the argument values and instance value saved when the enumerable object was initialized, but otherwise the enumerator object functions as described in [Enumerator objects](classes.md#enumerator-objects).</span></span>

### <a name="implementation-example"></a><span data-ttu-id="4c8df-1645">Uygulama örneği</span><span class="sxs-lookup"><span data-stu-id="4c8df-1645">Implementation example</span></span>

<span data-ttu-id="4c8df-1646">Bu bölümde, yineleyicilerin standart C# yapılar açısından olası bir uygulanması açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1646">This section describes a possible implementation of iterators in terms of standard C# constructs.</span></span> <span data-ttu-id="4c8df-1647">Burada açıklanan uygulama, Microsoft C# derleyicisi tarafından kullanılan ilkelere dayanır, ancak bu, bir uygulanan uygulamasına veya mümkün olan tek bir uygulama anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1647">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation or the only one possible.</span></span>

<span data-ttu-id="4c8df-1648">Aşağıdaki `Stack<T>` sınıfı, bir yineleyici kullanarak `GetEnumerator` metodunu uygular.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1648">The following `Stack<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="4c8df-1649">Yineleyici, yığının öğelerini üstten alta doğru sıralamayla numaralandırır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1649">The iterator enumerates the elements of the stack in top to bottom order.</span></span>

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

class Stack<T>: IEnumerable<T>
{
    T[] items;
    int count;

    public void Push(T item) {
        if (items == null) {
            items = new T[4];
        }
        else if (items.Length == count) {
            T[] newItems = new T[count * 2];
            Array.Copy(items, 0, newItems, 0, count);
            items = newItems;
        }
        items[count++] = item;
    }

    public T Pop() {
        T result = items[--count];
        items[count] = default(T);
        return result;
    }

    public IEnumerator<T> GetEnumerator() {
        for (int i = count - 1; i >= 0; --i) yield return items[i];
    }
}
```

<span data-ttu-id="4c8df-1650">`GetEnumerator` yöntemi, aşağıdaki şekilde gösterildiği gibi, yineleyici bloğunda kodu kapsülleyen derleyicinin ürettiği bir Numaralandırıcı sınıfının bir örneklemesine çevrilebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1650">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
class Stack<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1: IEnumerator<T>, IEnumerator
    {
        int __state;
        T __current;
        Stack<T> __this;
        int i;

        public __Enumerator1(Stack<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
                case 1: goto __state1;
                case 2: goto __state2;
            }
            i = __this.count - 1;
        __loop:
            if (i < 0) goto __state2;
            __current = __this.items[i];
            __state = 1;
            return true;
        __state1:
            --i;
            goto __loop;
        __state2:
            __state = 2;
            return false;
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="4c8df-1651">Yukarıdaki çeviride, yineleyici bloğundaki kod bir durum makinesine açılıp Numaralandırıcı sınıfının `MoveNext` yöntemine yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1651">In the preceding translation, the code in the iterator block is turned into a state machine and placed in the `MoveNext` method of the enumerator class.</span></span> <span data-ttu-id="4c8df-1652">Ayrıca, yerel değişken `i`, Numaralandırıcı nesnesindeki bir alana açık olduğundan, `MoveNext`çağırmaları arasında olmaya devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1652">Furthermore, the local variable `i` is turned into a field in the enumerator object so it can continue to exist across invocations of `MoveNext`.</span></span>

<span data-ttu-id="4c8df-1653">Aşağıdaki örnekte, 1 ile 10 arasındaki tamsayıların basit bir çarpma tablosu yazdırılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1653">The following example prints a simple multiplication table of the integers 1 through 10.</span></span> <span data-ttu-id="4c8df-1654">Örnekteki `FromTo` yöntemi, sıralanabilir bir nesne döndürüyor ve yineleyici kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1654">The `FromTo` method in the example returns an enumerable object and is implemented using an iterator.</span></span>

```csharp
using System;
using System.Collections.Generic;

class Test
{
    static IEnumerable<int> FromTo(int from, int to) {
        while (from <= to) yield return from++;
    }

    static void Main() {
        IEnumerable<int> e = FromTo(1, 10);
        foreach (int x in e) {
            foreach (int y in e) {
                Console.Write("{0,3} ", x * y);
            }
            Console.WriteLine();
        }
    }
}
```

<span data-ttu-id="4c8df-1655">`FromTo` yöntemi, aşağıdaki şekilde gösterildiği gibi, yineleyici bloğunda kodu sarmalayan derleyici tarafından oluşturulmuş bir sıralanabilir sınıfın bir örneklemesine çevrilebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1655">The `FromTo` method can be translated into an instantiation of a compiler-generated enumerable class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
using System;
using System.Threading;
using System.Collections;
using System.Collections.Generic;

class Test
{
    ...

    static IEnumerable<int> FromTo(int from, int to) {
        return new __Enumerable1(from, to);
    }

    class __Enumerable1:
        IEnumerable<int>, IEnumerable,
        IEnumerator<int>, IEnumerator
    {
        int __state;
        int __current;
        int __from;
        int from;
        int to;
        int i;

        public __Enumerable1(int __from, int to) {
            this.__from = __from;
            this.to = to;
        }

        public IEnumerator<int> GetEnumerator() {
            __Enumerable1 result = this;
            if (Interlocked.CompareExchange(ref __state, 1, 0) != 0) {
                result = new __Enumerable1(__from, to);
                result.__state = 1;
            }
            result.from = result.__from;
            return result;
        }

        IEnumerator IEnumerable.GetEnumerator() {
            return (IEnumerator)GetEnumerator();
        }

        public int Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
            case 1:
                if (from > to) goto case 2;
                __current = from++;
                __state = 1;
                return true;
            case 2:
                __state = 2;
                return false;
            default:
                throw new InvalidOperationException();
            }
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="4c8df-1656">Sıralanabilir sınıf hem numaralandırılabilir arabirimleri hem de Numaralandırıcı arabirimlerini uygular, bu sayede hem numaralandırılabilir hem de numaralandırıcı olarak işlev sağlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1656">The enumerable class implements both the enumerable interfaces and the enumerator interfaces, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="4c8df-1657">`GetEnumerator` yöntemi ilk kez çağrıldığında, sıralanabilir nesnenin kendisi döndürülür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1657">The first time the `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="4c8df-1658">Sıralanabilir nesnenin `GetEnumerator`sonraki etkinleştirmeleri, varsa, sıralanabilir nesnenin bir kopyasını döndürür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1658">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="4c8df-1659">Bu nedenle, döndürülen her Numaralandırıcı kendi durumuna sahiptir ve bir Numaralandırıcı içindeki değişiklikler başka bir Numaralandırıcı olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1659">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span> <span data-ttu-id="4c8df-1660">`Interlocked.CompareExchange` yöntemi, iş parçacığı güvenli işlemi sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1660">The `Interlocked.CompareExchange` method is used to ensure thread-safe operation.</span></span>

<span data-ttu-id="4c8df-1661">`from` ve `to` parametreleri, sıralanabilir sınıftaki alanlara açıktır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1661">The `from` and `to` parameters are turned into fields in the enumerable class.</span></span> <span data-ttu-id="4c8df-1662">Yineleyici bloğunda `from` değiştirildiğinden, her Numaralandırmadaki `from` için verilen ilk değeri tutmak üzere ek bir `__from` alanı tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1662">Because `from` is modified in the iterator block, an additional `__from` field is introduced to hold the initial value given to `from` in each enumerator.</span></span>

<span data-ttu-id="4c8df-1663">`MoveNext` yöntemi `__state` `0`olduğunda çağrılırsa bir `InvalidOperationException` oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1663">The `MoveNext` method throws an `InvalidOperationException` if it is called when `__state` is `0`.</span></span> <span data-ttu-id="4c8df-1664">Bu, önce `GetEnumerator`çağrılmadan Numaralandırıcı nesne olarak sıralanabilir nesnenin kullanılmasına karşı koruma sağlar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1664">This protects against use of the enumerable object as an enumerator object without first calling `GetEnumerator`.</span></span>

<span data-ttu-id="4c8df-1665">Aşağıdaki örnek bir basit ağaç sınıfını göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1665">The following example shows a simple tree class.</span></span> <span data-ttu-id="4c8df-1666">`Tree<T>` sınıfı, bir yineleyici kullanarak `GetEnumerator` metodunu uygular.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1666">The `Tree<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="4c8df-1667">Yineleyici, ağaç öğelerini geçersiz kılma sırasında sıralar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1667">The iterator enumerates the elements of the tree in infix order.</span></span>

```csharp
using System;
using System.Collections.Generic;

class Tree<T>: IEnumerable<T>
{
    T value;
    Tree<T> left;
    Tree<T> right;

    public Tree(T value, Tree<T> left, Tree<T> right) {
        this.value = value;
        this.left = left;
        this.right = right;
    }

    public IEnumerator<T> GetEnumerator() {
        if (left != null) foreach (T x in left) yield x;
        yield value;
        if (right != null) foreach (T x in right) yield x;
    }
}

class Program
{
    static Tree<T> MakeTree<T>(T[] items, int left, int right) {
        if (left > right) return null;
        int i = (left + right) / 2;
        return new Tree<T>(items[i], 
            MakeTree(items, left, i - 1),
            MakeTree(items, i + 1, right));
    }

    static Tree<T> MakeTree<T>(params T[] items) {
        return MakeTree(items, 0, items.Length - 1);
    }

    // The output of the program is:
    // 1 2 3 4 5 6 7 8 9
    // Mon Tue Wed Thu Fri Sat Sun

    static void Main() {
        Tree<int> ints = MakeTree(1, 2, 3, 4, 5, 6, 7, 8, 9);
        foreach (int i in ints) Console.Write("{0} ", i);
        Console.WriteLine();

        Tree<string> strings = MakeTree(
            "Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun");
        foreach (string s in strings) Console.Write("{0} ", s);
        Console.WriteLine();
    }
}
```

<span data-ttu-id="4c8df-1668">`GetEnumerator` yöntemi, aşağıdaki şekilde gösterildiği gibi, yineleyici bloğunda kodu kapsülleyen derleyicinin ürettiği bir Numaralandırıcı sınıfının bir örneklemesine çevrilebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1668">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
class Tree<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1 : IEnumerator<T>, IEnumerator
    {
        Node<T> __this;
        IEnumerator<T> __left, __right;
        int __state;
        T __current;

        public __Enumerator1(Node<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            try {
                switch (__state) {

                case 0:
                    __state = -1;
                    if (__this.left == null) goto __yield_value;
                    __left = __this.left.GetEnumerator();
                    goto case 1;

                case 1:
                    __state = -2;
                    if (!__left.MoveNext()) goto __left_dispose;
                    __current = __left.Current;
                    __state = 1;
                    return true;

                __left_dispose:
                    __state = -1;
                    __left.Dispose();

                __yield_value:
                    __current = __this.value;
                    __state = 2;
                    return true;

                case 2:
                    __state = -1;
                    if (__this.right == null) goto __end;
                    __right = __this.right.GetEnumerator();
                    goto case 3;

                case 3:
                    __state = -3;
                    if (!__right.MoveNext()) goto __right_dispose;
                    __current = __right.Current;
                    __state = 3;
                    return true;

                __right_dispose:
                    __state = -1;
                    __right.Dispose();

                __end:
                    __state = 4;
                    break;

                }
            }
            finally {
                if (__state < 0) Dispose();
            }
            return false;
        }

        public void Dispose() {
            try {
                switch (__state) {

                case 1:
                case -2:
                    __left.Dispose();
                    break;

                case 3:
                case -3:
                    __right.Dispose();
                    break;

                }
            }
            finally {
                __state = 4;
            }
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="4c8df-1669">`foreach` deyimlerde kullanılan derleyicinin oluşturduğu geçiciler, Numaralandırıcı nesnesinin `__left` ve `__right` alanlarına yükseltilmemiş.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1669">The compiler generated temporaries used in the `foreach` statements are lifted into the `__left` and `__right` fields of the enumerator object.</span></span> <span data-ttu-id="4c8df-1670">Bir özel durum oluşturulursa doğru `Dispose()` yönteminin doğru şekilde çağrılabilmesi için, Numaralandırıcı nesnesinin `__state` alanı dikkatle güncellenir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1670">The `__state` field of the enumerator object is carefully updated so that the correct `Dispose()` method will be called correctly if an exception is thrown.</span></span> <span data-ttu-id="4c8df-1671">Çevrilmiş kodu basit `foreach` deyimleriyle yazmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1671">Note that it is not possible to write the translated code with simple `foreach` statements.</span></span>

## <a name="async-functions"></a><span data-ttu-id="4c8df-1672">Zaman uyumsuz işlevler</span><span class="sxs-lookup"><span data-stu-id="4c8df-1672">Async functions</span></span>

<span data-ttu-id="4c8df-1673">`async` değiştiricisine sahip bir yönteme ([yöntemlere](classes.md#methods)) veya anonim Işleve ([anonim işlev ifadelerine](expressions.md#anonymous-function-expressions)) ***zaman uyumsuz işlev***denir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1673">A method ([Methods](classes.md#methods)) or anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) with the `async` modifier is called an ***async function***.</span></span> <span data-ttu-id="4c8df-1674">Genel olarak, ***zaman uyumsuz*** terimi `async` değiştiricisine sahip herhangi bir tür işlevi tanımlamakta kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1674">In general, the term ***async*** is used to describe any kind of function that has the `async` modifier.</span></span>

<span data-ttu-id="4c8df-1675">Herhangi bir `ref` veya `out` parametresi belirtmek için zaman uyumsuz bir işlevin biçimsel parametre listesi için derleme zamanı hatası.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1675">It is a compile-time error for the formal parameter list of an async function to specify any `ref` or `out` parameters.</span></span>

<span data-ttu-id="4c8df-1676">Zaman uyumsuz bir metodun *return_type* `void` ya da bir ***görev türü***olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1676">The *return_type* of an async method must be either `void` or a ***task type***.</span></span> <span data-ttu-id="4c8df-1677">Görev türleri `System.Threading.Tasks.Task` ve `System.Threading.Tasks.Task<T>`oluşturulan türlerdir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1677">The task types are `System.Threading.Tasks.Task` and types constructed from `System.Threading.Tasks.Task<T>`.</span></span> <span data-ttu-id="4c8df-1678">Breçekimi 'nin sake 'ı için, bu bölümde sırasıyla `Task` ve `Task<T>`olarak başvurulur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1678">For the sake of brevity, in this chapter these types are referenced as `Task` and `Task<T>`, respectively.</span></span> <span data-ttu-id="4c8df-1679">Bir görev türü döndüren zaman uyumsuz bir yöntem, görev döndüren olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1679">An async method returning a task type is said to be task-returning.</span></span>

<span data-ttu-id="4c8df-1680">Görev türlerinin tam tanımı uygulama tanımlı, ancak dilin bir görev türü görüntüleme noktasından tamamlanmamış, başarılı veya hatalı durumlardan birinde.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1680">The exact definition of the task types is implementation defined, but from the language's point of view a task type is in one of the states incomplete, succeeded or faulted.</span></span> <span data-ttu-id="4c8df-1681">Hatalı bir görev ilgili özel durumu kaydeder.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1681">A faulted task records a pertinent exception.</span></span> <span data-ttu-id="4c8df-1682">Başarılı bir `Task<T>` `T`türünde bir sonuç kaydeder.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1682">A succeeded `Task<T>` records a result of type `T`.</span></span> <span data-ttu-id="4c8df-1683">Görev türleri beklenebilir ve bu nedenle await ifadelerinin işlenenleri ([await ifadeleri](expressions.md#await-expressions)) olabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1683">Task types are awaitable, and can therefore be the operands of await expressions ([Await expressions](expressions.md#await-expressions)).</span></span>

<span data-ttu-id="4c8df-1684">Zaman uyumsuz işlev çağırma, kendi gövdesinde await ifadeleri ([await ifadeleri](expressions.md#await-expressions)) yoluyla değerlendirmeyi askıya alabilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1684">An async function invocation has the ability to suspend evaluation by means of await expressions ([Await expressions](expressions.md#await-expressions)) in its body.</span></span> <span data-ttu-id="4c8df-1685">Değerlendirme daha sonra, bir ***sürdürme temsilcisi***tarafından askıya alma await ifadesinin noktasında devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1685">Evaluation may later be resumed at the point of the suspending await expression by means of a ***resumption delegate***.</span></span> <span data-ttu-id="4c8df-1686">Sürdürme temsilcisi `System.Action`türüdür ve çağrıldığında, zaman uyumsuz işlev çağrısının değerlendirmesi, kaldığı bekleme ifadesinden sürdürülecek.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1686">The resumption delegate is of type `System.Action`, and when it is invoked, evaluation of the async function invocation will resume from the await expression where it left off.</span></span> <span data-ttu-id="4c8df-1687">Zaman uyumsuz işlev çağrısının ***geçerli çağıranı*** , işlev çağrısı hiçbir zaman askıya alınmadıysa veya sürdürme temsilcisinin en son çağıranı yoksa, özgün çağırıcı olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1687">The ***current caller*** of an async function invocation is the original caller if the function invocation has never been suspended, or the most recent caller of the resumption delegate otherwise.</span></span>

### <a name="evaluation-of-a-task-returning-async-function"></a><span data-ttu-id="4c8df-1688">Bir görev döndüren zaman uyumsuz işlev değerlendirmesi</span><span class="sxs-lookup"><span data-stu-id="4c8df-1688">Evaluation of a task-returning async function</span></span>

<span data-ttu-id="4c8df-1689">Bir görev döndüren zaman uyumsuz işlevin çağrılması döndürülen görev türünün bir örneğinin oluşturulmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1689">Invocation of a task-returning async function causes an instance of the returned task type to be generated.</span></span> <span data-ttu-id="4c8df-1690">Bu, Async işlevinin ***Return görevi*** olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1690">This is called the ***return task*** of the async function.</span></span> <span data-ttu-id="4c8df-1691">Görev başlangıçta tamamlanmamış bir durumda.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1691">The task is initially in an incomplete state.</span></span>

<span data-ttu-id="4c8df-1692">Zaman uyumsuz işlev gövdesi, askıya alınana kadar (bir await ifadesine ulaşılarak) ya da sona erene kadar değerlendirilir, bu da dönüş göreviyle birlikte çağrı yapana döndürülür.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1692">The async function body is then evaluated until it is either suspended (by reaching an await expression) or terminates, at which point control is returned to the caller, along with the return task.</span></span>

<span data-ttu-id="4c8df-1693">Async işlevinin gövdesi sonlandırıldığında, geri dönüş görevi tamamlanmamış durumdan çıkarılır:</span><span class="sxs-lookup"><span data-stu-id="4c8df-1693">When the body of the async function terminates, the return task is moved out of the incomplete state:</span></span>

*  <span data-ttu-id="4c8df-1694">İşlev gövdesi bir dönüş ifadesine veya gövdenin sonuna ulaşılması sonucu olarak sonlandığında, herhangi bir sonuç değeri, başarılı durumuna yerleştirilen dönüş görevine kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1694">If the function body terminates as the result of reaching a return statement or the end of the body, any result value is recorded in the return task, which is put into a succeeded state.</span></span>
*  <span data-ttu-id="4c8df-1695">İşlev gövdesi yakalanamayan bir özel durum ([throw](statements.md#the-throw-statement)) sonucu olarak sonlandığında, özel durum hatalı duruma yerleştirilen geri dönüş görevine kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1695">If the function body terminates as the result of an uncaught exception ([The throw statement](statements.md#the-throw-statement)) the exception is recorded in the return task which is put into a faulted state.</span></span>

### <a name="evaluation-of-a-void-returning-async-function"></a><span data-ttu-id="4c8df-1696">Void döndüren zaman uyumsuz bir işlevin değerlendirmesi</span><span class="sxs-lookup"><span data-stu-id="4c8df-1696">Evaluation of a void-returning async function</span></span>

<span data-ttu-id="4c8df-1697">Zaman uyumsuz işlevin dönüş türü `void`, değerlendirme yukarıdan aşağıdaki şekilde farklılık gösterir: hiçbir görev döndürülmediği için, işlev, geçerli iş parçacığının ***eşitleme bağlamına***tamamlanma ve özel durumları iletişim kurar.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1697">If the return type of the async function is `void`, evaluation differs from the above in the following way: Because no task is returned, the function instead communicates completion and exceptions to the current thread's ***synchronization context***.</span></span> <span data-ttu-id="4c8df-1698">Eşitleme bağlamının tam tanımı uygulamaya bağımlıdır, ancak geçerli iş parçacığının çalıştırıldığı "nerede" gösterimidir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1698">The exact definition of synchronization context is implementation-dependent, but is a representation of "where" the current thread is running.</span></span> <span data-ttu-id="4c8df-1699">Bir void döndüren zaman uyumsuz işlevin değerlendirilmesi, başarıyla tamamlandığında veya yakalanamayan bir özel durumun oluşturulmasına neden olduğunda eşitleme bağlamına bildirim gönderilir.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1699">The synchronization context is notified when evaluation of a void-returning async function commences, completes successfully, or causes an uncaught exception to be thrown.</span></span>

<span data-ttu-id="4c8df-1700">Bu, bağlamın altında kaç tane void döndüren zaman uyumsuz işlev çalıştığını izlemeye ve bunlardan gelen özel durumların nasıl yayabileceğine karar vermesine olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="4c8df-1700">This allows the context to keep track of how many void-returning async functions are running under it, and to decide how to propagate exceptions coming out of them.</span></span>
