# <a name="classes"></a><span data-ttu-id="6e059-101">Sınıflar</span><span class="sxs-lookup"><span data-stu-id="6e059-101">Classes</span></span>

<span data-ttu-id="6e059-102">Bir sınıf veri üyeleri (sabitleri ve alanları), işlev üyeleri (yöntemler, özellikler, olaylar, dizin oluşturucular, işleçler, örnek oluşturucuları, yok ediciler ve statik oluşturucular) ve iç içe türler içerebilecek bir veri yapısıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-102">A class is a data structure that may contain data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="6e059-103">Sınıf türleri, devralma, yapabildiği türetilmiş bir sınıf genişletin ve bir temel sınıf uzmanlaşmış bir mekanizma destekler.</span><span class="sxs-lookup"><span data-stu-id="6e059-103">Class types support inheritance, a mechanism whereby a derived class can extend and specialize a base class.</span></span>

## <a name="class-declarations"></a><span data-ttu-id="6e059-104">Sınıf bildirimleri</span><span class="sxs-lookup"><span data-stu-id="6e059-104">Class declarations</span></span>

<span data-ttu-id="6e059-105">A *class_declaration* olduğu bir *type_declaration* ([tür bildirimleri](namespaces.md#type-declarations)), yeni bir sınıfı bildirir.</span><span class="sxs-lookup"><span data-stu-id="6e059-105">A *class_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new class.</span></span>

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

<span data-ttu-id="6e059-106">A *class_declaration* isteğe bağlı bir kümesinden oluşur *öznitelikleri* ([öznitelikleri](attributes.md)) ve ardından isteğe bağlı bir dizi *class_modifier*s ([sınıfı değiştiricileri](classes.md#class-modifiers)) ve ardından isteğe bağlı `partial` değiştiricisi anahtar sözcüğü ve ardından, `class` ve *tanımlayıcı* ardından sınıf adları bir İsteğe bağlı *type_parameter_list* ([tür parametrelerindeki](classes.md#type-parameters)) ve ardından isteğe bağlı *class_base* belirtimi ([temel sınıfı belirtimi](classes.md#class-base-specification)) ve ardından isteğe bağlı bir dizi *type_parameter_constraints_clause*s ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) ve ardından bir *class_body*  ([Sınıfı gövdesi](classes.md#class-body)), noktalı virgül tarafından izlenen isteğe bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="6e059-106">A *class_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *class_modifier*s ([Class modifiers](classes.md#class-modifiers)), followed by an optional `partial` modifier, followed by the keyword `class` and an *identifier* that names the class, followed by an optional *type_parameter_list* ([Type parameters](classes.md#type-parameters)), followed by an optional *class_base* specification ([Class base specification](classes.md#class-base-specification)) , followed by an optional set of *type_parameter_constraints_clause*s ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *class_body* ([Class body](classes.md#class-body)), optionally followed by a semicolon.</span></span>

<span data-ttu-id="6e059-107">Bir sınıf bildiriminde sağlayamazsınız *type_parameter_constraints_clause*s ayrıca sağlanmadıkça bir *type_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="6e059-107">A class declaration cannot supply *type_parameter_constraints_clause*s unless it also supplies a *type_parameter_list*.</span></span>

<span data-ttu-id="6e059-108">Sağlayan bir sınıf bildirimi bir *type_parameter_list* olduğu bir ***genel sınıf bildirimi***.</span><span class="sxs-lookup"><span data-stu-id="6e059-108">A class declaration that supplies a *type_parameter_list* is a ***generic class declaration***.</span></span> <span data-ttu-id="6e059-109">Ayrıca bu yana oluşturulan bir tür oluşturmak için kapsayan tür için tür parametreleri sağlanmalıdır bir genel sınıf bildirimi veya genel yapı bildirimi içinde herhangi bir sınıfın kendisi bir genel sınıf bildirimi sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6e059-109">Additionally, any class nested inside a generic class declaration or a generic struct declaration is itself a generic class declaration, since type parameters for the containing type must be supplied to create a constructed type.</span></span>

### <a name="class-modifiers"></a><span data-ttu-id="6e059-110">Sınıfı değiştiricilere</span><span class="sxs-lookup"><span data-stu-id="6e059-110">Class modifiers</span></span>

<span data-ttu-id="6e059-111">A *class_declaration* isteğe bağlı olarak bir dizi sınıfı değiştiricilere içerebilir:</span><span class="sxs-lookup"><span data-stu-id="6e059-111">A *class_declaration* may optionally include a sequence of class modifiers:</span></span>

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

<span data-ttu-id="6e059-112">Aynı değiştiricisi bir sınıf bildirimi içinde birden çok kez görünmesi için bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-112">It is a compile-time error for the same modifier to appear multiple times in a class declaration.</span></span>

<span data-ttu-id="6e059-113">`new` Değiştiricisi iç içe sınıflarda izin verilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-113">The `new` modifier is permitted on nested classes.</span></span> <span data-ttu-id="6e059-114">Bölümünde anlatıldığı gibi sınıfı aynı ada göre devralınmış bir üyeyi gizler belirtir [yeni değiştiricisini](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="6e059-114">It specifies that the class hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span> <span data-ttu-id="6e059-115">Bir derleme zamanı hata için `new` iç içe geçmiş sınıf bildirimi olmayan bir sınıf bildirimi üzerinde görünecek değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-115">It is a compile-time error for the `new` modifier to appear on a class declaration that is not a nested class declaration.</span></span>

<span data-ttu-id="6e059-116">`public`, `protected`, `internal`, Ve `private` sınıf erişilebilirlik değiştiricileri denetim.</span><span class="sxs-lookup"><span data-stu-id="6e059-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the class.</span></span> <span data-ttu-id="6e059-117">Sınıf bildiriminin oluştuğu bağlama bağlı olarak bu değiştiriciler bazıları değil izin verilir ([erişilebilirlik bildirilen](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="6e059-117">Depending on the context in which the class declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="6e059-118">`abstract`, `sealed` Ve `static` değiştiriciler, aşağıdaki bölümlerde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="6e059-118">The `abstract`, `sealed` and `static` modifiers are discussed in the following sections.</span></span>

#### <a name="abstract-classes"></a><span data-ttu-id="6e059-119">Soyut sınıflar</span><span class="sxs-lookup"><span data-stu-id="6e059-119">Abstract classes</span></span>

<span data-ttu-id="6e059-120">`abstract` Değiştiricisi bir sınıf eksik olduğunu ve yalnızca temel sınıf olarak kullanılmak üzere tasarlanmış olduğunu belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-120">The `abstract` modifier is used to indicate that a class is incomplete and that it is intended to be used only as a base class.</span></span> <span data-ttu-id="6e059-121">Bir Özet sınıf, Özet olmayan bir sınıftan aşağıdaki yollarla farklıdır:</span><span class="sxs-lookup"><span data-stu-id="6e059-121">An abstract class differs from a non-abstract class in the following ways:</span></span>

*  <span data-ttu-id="6e059-122">Soyut bir sınıf doğrudan oluşturulamaz ve kullanmak için bir derleme zamanı hata `new` işlecinin bir Özet sınıf.</span><span class="sxs-lookup"><span data-stu-id="6e059-122">An abstract class cannot be instantiated directly, and it is a compile-time error to use the `new` operator on an abstract class.</span></span> <span data-ttu-id="6e059-123">Değişkenler ve değerleri, derleme zamanı türü soyut mümkün olsa da, bu tür değişkenlere ve değerlere mutlaka olacaktır `null` veya soyut türlerden türetilmiş soyut olmayan sınıflar örneklerini başvurular içeriyor.</span><span class="sxs-lookup"><span data-stu-id="6e059-123">While it is possible to have variables and values whose compile-time types are abstract, such variables and values will necessarily either be `null` or contain references to instances of non-abstract classes derived from the abstract types.</span></span>
*  <span data-ttu-id="6e059-124">Bir soyut sınıfı izin verilir (ancak gerekli değildir) soyut üye içermesi.</span><span class="sxs-lookup"><span data-stu-id="6e059-124">An abstract class is permitted (but not required) to contain abstract members.</span></span>
*  <span data-ttu-id="6e059-125">Bir soyut sınıfı korumalı olamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-125">An abstract class cannot be sealed.</span></span>

<span data-ttu-id="6e059-126">Bir soyut sınıftan türetilmiş soyut olmayan sınıf, soyut olmayan sınıf gerçek uygulamaları böylece bu soyut üyelerini geçersiz kılarak tüm devralınan soyut üye içermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-126">When a non-abstract class is derived from an abstract class, the non-abstract class must include actual implementations of all inherited abstract members, thereby overriding those abstract members.</span></span> <span data-ttu-id="6e059-127">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-127">In the example</span></span>
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
<span data-ttu-id="6e059-128">soyut sınıf `A` soyut bir yöntem sunar `F`.</span><span class="sxs-lookup"><span data-stu-id="6e059-128">the abstract class `A` introduces an abstract method `F`.</span></span> <span data-ttu-id="6e059-129">Sınıf `B` ek bir yöntem sunar `G`, ancak bu yana uygulaması sağlamaz `F`, `B` da soyut bildirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-129">Class `B` introduces an additional method `G`, but since it doesn't provide an implementation of `F`, `B` must also be declared abstract.</span></span> <span data-ttu-id="6e059-130">Sınıf `C` geçersiz kılmalar `F` ve gerçek bir uygulamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="6e059-130">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="6e059-131">Soyut üye yok olduğundan `C`, `C` izin verilir (ancak gerekli değildir) Özet olmayan olacak.</span><span class="sxs-lookup"><span data-stu-id="6e059-131">Since there are no abstract members in `C`, `C` is permitted (but not required) to be non-abstract.</span></span>

#### <a name="sealed-classes"></a><span data-ttu-id="6e059-132">Sealed sınıfları</span><span class="sxs-lookup"><span data-stu-id="6e059-132">Sealed classes</span></span>

<span data-ttu-id="6e059-133">`sealed` Değiştiricisi, bir sınıftan türetme önlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-133">The `sealed` modifier is used to prevent derivation from a class.</span></span> <span data-ttu-id="6e059-134">Korumalı sınıf başka bir sınıfın temel sınıf olarak belirtilirse bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="6e059-134">A compile-time error occurs if a sealed class is specified as the base class of another class.</span></span>

<span data-ttu-id="6e059-135">Korumalı sınıf bir soyut sınıfı da olamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-135">A sealed class cannot also be an abstract class.</span></span>

<span data-ttu-id="6e059-136">`sealed` Değiştiricisi istenmeyen türetme önlemek için öncelikli olarak kullanılır, ancak ayrıca bazı çalışma zamanı iyileştirmeleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="6e059-136">The `sealed` modifier is primarily used to prevent unintended derivation, but it also enables certain run-time optimizations.</span></span> <span data-ttu-id="6e059-137">Özellikle, korumalı bir sınıfın tüm türetilmiş sınıfları için hiçbir zaman bilindiğinden, sanal olmayan çağrılarını korumalı sınıf örneklerinde sanal işlev üyesi çağrılarını dönüştürme mümkündür.</span><span class="sxs-lookup"><span data-stu-id="6e059-137">In particular, because a sealed class is known to never have any derived classes, it is possible to transform virtual function member invocations on sealed class instances into non-virtual invocations.</span></span>

#### <a name="static-classes"></a><span data-ttu-id="6e059-138">Statik sınıflar</span><span class="sxs-lookup"><span data-stu-id="6e059-138">Static classes</span></span>

<span data-ttu-id="6e059-139">`static` Değiştirici olarak bildirilen bir sınıfı işaretlemek için kullanılan bir ***statik sınıf***.</span><span class="sxs-lookup"><span data-stu-id="6e059-139">The `static` modifier is used to mark the class being declared as a ***static class***.</span></span> <span data-ttu-id="6e059-140">Statik sınıf oluşturulamaz, bir tür olarak kullanılamaz ve yalnızca statik üyeleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-140">A static class cannot be instantiated, cannot be used as a type and can contain only static members.</span></span> <span data-ttu-id="6e059-141">Yalnızca bir statik sınıf genişletme yöntemleri, bildirimler içerebilir ([genişletme yöntemleri](classes.md#extension-methods)).</span><span class="sxs-lookup"><span data-stu-id="6e059-141">Only a static class can contain declarations of extension methods ([Extension methods](classes.md#extension-methods)).</span></span>

<span data-ttu-id="6e059-142">Bir statik sınıf bildirimi aşağıdaki sınırlamalara tabi şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="6e059-142">A static class declaration is subject to the following restrictions:</span></span>

*  <span data-ttu-id="6e059-143">Statik sınıf içermiyor bir `sealed` veya `abstract` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-143">A static class may not include a `sealed` or `abstract` modifier.</span></span> <span data-ttu-id="6e059-144">Ancak, bir statik sınıf örneği veya türetilmiş olduğundan, mühürlendi ve soyut gibi davranır olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="6e059-144">Note, however, that since a static class cannot be instantiated or derived from, it behaves as if it was both sealed and abstract.</span></span>
*  <span data-ttu-id="6e059-145">Statik sınıf içermiyor bir *class_base* belirtimi ([sınıf temel belirtimine](classes.md#class-base-specification)) ve bir temel sınıf veya uygulanan arabirimleri listesini açıkça belirtemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="6e059-145">A static class may not include a *class_base* specification ([Class base specification](classes.md#class-base-specification)) and cannot explicitly specify a base class or a list of implemented interfaces.</span></span> <span data-ttu-id="6e059-146">Statik sınıf örtük tür tarafından devralındığında `object`.</span><span class="sxs-lookup"><span data-stu-id="6e059-146">A static class implicitly inherits from type `object`.</span></span>
*  <span data-ttu-id="6e059-147">Statik sınıf yalnızca statik üyeleri içerebilir ([statik ve örnek üyeleri](classes.md#static-and-instance-members)).</span><span class="sxs-lookup"><span data-stu-id="6e059-147">A static class can only contain static members ([Static and instance members](classes.md#static-and-instance-members)).</span></span> <span data-ttu-id="6e059-148">Sabitler ve iç içe geçmiş türler statik üyeleri olarak sınıflandırıldığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="6e059-148">Note that constants and nested types are classified as static members.</span></span>
*  <span data-ttu-id="6e059-149">Statik sınıf üyeleri ile olamaz `protected` veya `protected internal` erişilebilirlik bildirilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-149">A static class cannot have members with `protected` or `protected internal` declared accessibility.</span></span>

<span data-ttu-id="6e059-150">Tüm bu kısıtlamalar ihlal eden bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-150">It is a compile-time error to violate any of these restrictions.</span></span>

<span data-ttu-id="6e059-151">Statik sınıf örneği oluşturucusu yok.</span><span class="sxs-lookup"><span data-stu-id="6e059-151">A static class has no instance constructors.</span></span> <span data-ttu-id="6e059-152">Bir statik sınıftaki örnek oluşturucusu ve yok varsayılan örnek oluşturucusu bildirmek mümkün değildir ([varsayılan oluşturucular](classes.md#default-constructors)) statik bir sınıf için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="6e059-152">It is not possible to declare an instance constructor in a static class, and no default instance constructor ([Default constructors](classes.md#default-constructors)) is provided for a static class.</span></span>

<span data-ttu-id="6e059-153">Statik sınıf üyeleri otomatik olarak statik olmayan ve üye bildirimleri açıkça içermelidir bir `static` değiştirici (hariç, sabitleri ve iç içe geçmiş türler).</span><span class="sxs-lookup"><span data-stu-id="6e059-153">The members of a static class are not automatically static, and the member declarations must explicitly include a `static` modifier (except for constants and nested types).</span></span> <span data-ttu-id="6e059-154">Bir sınıfın statik bir dış sınıf içinde iç içe geçmiş, açıkça içermedikçe iç içe geçmiş sınıf bir statik sınıf değil bir `static` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-154">When a class is nested within a static outer class, the nested class is not a static class unless it explicitly includes a `static` modifier.</span></span>

<span data-ttu-id="6e059-155">__Statik sınıf türlerine başvurma__</span><span class="sxs-lookup"><span data-stu-id="6e059-155">__Referencing static class types__</span></span>

<span data-ttu-id="6e059-156">A *namespace_or_type_name* ([Namespace ve tür adları](basic-concepts.md#namespace-and-type-names)), bir statik sınıf başvurmak için izin verilir</span><span class="sxs-lookup"><span data-stu-id="6e059-156">A *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="6e059-157">*Namespace_or_type_name* olduğu `T` içinde bir *namespace_or_type_name* formun `T.I`, veya</span><span class="sxs-lookup"><span data-stu-id="6e059-157">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="6e059-158">*Namespace_or_type_name* olduğu `T` içinde bir *typeof_expression* ([bağımsız değişken listeleri](expressions.md#argument-lists)1) biçiminde `typeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="6e059-158">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

<span data-ttu-id="6e059-159">A *primary_expression* ([işlev üyeleri](expressions.md#function-members)), bir statik sınıf başvurmak için izin verilir</span><span class="sxs-lookup"><span data-stu-id="6e059-159">A *primary_expression* ([Function members](expressions.md#function-members)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="6e059-160">*Primary_expression* olduğu `E` içinde bir *member_access* ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) biçiminde `E.I`.</span><span class="sxs-lookup"><span data-stu-id="6e059-160">The *primary_expression* is the `E` in a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) of the form `E.I`.</span></span>

<span data-ttu-id="6e059-161">Başka bir bağlamda, statik sınıf başvurmak için bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-161">In any other context it is a compile-time error to reference a static class.</span></span> <span data-ttu-id="6e059-162">Örneğin, bir temel sınıf bağlı bir türü olarak kullanılacak bir statik sınıf için bir hata olduğu ([türlerin](classes.md#nested-types)) üyesi, bir genel tür bağımsız değişkeni ya da bir tür parametresi kısıtlaması.</span><span class="sxs-lookup"><span data-stu-id="6e059-162">For example, it is an error for a static class to be used as a base class, a constituent type ([Nested types](classes.md#nested-types)) of a member, a generic type argument, or a type parameter constraint.</span></span> <span data-ttu-id="6e059-163">Benzer şekilde, bir statik sınıf bir dizi türü, işaretçi türünde kullanılamaz bir `new` ifade, bir atama ifadesi bir `is` ifadesi, bir `as` ifadesi, bir `sizeof` ifadesi veya varsayılan değer ifadesi.</span><span class="sxs-lookup"><span data-stu-id="6e059-163">Likewise, a static class cannot be used in an array type, a pointer type, a `new` expression, a cast expression, an `is` expression, an `as` expression, a `sizeof` expression, or a default value expression.</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="6e059-164">Partial değiştiricisi</span><span class="sxs-lookup"><span data-stu-id="6e059-164">Partial modifier</span></span>

<span data-ttu-id="6e059-165">`partial` Değiştiricisi belirtmek için kullanılır, bu *class_declaration* kısmi türü bildirimi.</span><span class="sxs-lookup"><span data-stu-id="6e059-165">The `partial` modifier is used to indicate that this *class_declaration* is a partial type declaration.</span></span> <span data-ttu-id="6e059-166">Form bir türü bildirimi için birleştirme kapsayan bir ad alanı veya tür bildirimi içinde aynı ada sahip birden fazla kısmi tür bildirimleri, kuralları aşağıdaki belirtilen [kısmi türlerinden](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="6e059-166">Multiple partial type declarations with the same name within an enclosing namespace or type declaration combine to form one type declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

<span data-ttu-id="6e059-167">Bu kesimler üretilen veya farklı bağlamlardaki tutulan ayrı program metni parçalarını dağıtılan bir sınıf bildirimi sahip yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-167">Having the declaration of a class distributed over separate segments of program text can be useful if these segments are produced or maintained in different contexts.</span></span> <span data-ttu-id="6e059-168">Örneğin, diğer el ile yazılan ise sınıf bildiriminin bir parçası oluşturulan makine olabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-168">For instance, one part of a class declaration may be machine generated, whereas the other is manually authored.</span></span> <span data-ttu-id="6e059-169">İki metin ayrımı güncelleştirmeleri tarafından diğer güncelleştirmelerle çakışan birinden tarafından engeller.</span><span class="sxs-lookup"><span data-stu-id="6e059-169">Textual separation of the two prevents updates by one from conflicting with updates by the other.</span></span>

### <a name="type-parameters"></a><span data-ttu-id="6e059-170">Tür parametreleri</span><span class="sxs-lookup"><span data-stu-id="6e059-170">Type parameters</span></span>

<span data-ttu-id="6e059-171">Bir tür parametresi oluşturulan bir tür oluşturmak için sağlanan bir tür bağımsız değişkeni için bir yer tutucu gösteren basit bir tanımlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-171">A type parameter is a simple identifier that denotes a placeholder for a type argument supplied to create a constructed type.</span></span> <span data-ttu-id="6e059-172">Bir tür parametresi daha sonra sunulacaktır bir tür için resmi bir yer tutucudur.</span><span class="sxs-lookup"><span data-stu-id="6e059-172">A type parameter is a formal placeholder for a type that will be supplied later.</span></span> <span data-ttu-id="6e059-173">Bunun aksine, bir tür bağımsız değişkeni olarak ([tür bağımsız değişkenleri](types.md#type-arguments)) oluşturulmuş bir türü oluşturulduğunda, tür parametresi yerine koyulan gerçek türü.</span><span class="sxs-lookup"><span data-stu-id="6e059-173">By contrast, a type argument ([Type arguments](types.md#type-arguments)) is the actual type that is substituted for the type parameter when a constructed type is created.</span></span>

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

<span data-ttu-id="6e059-174">Her bir sınıf bildiriminde tür parametresi bildirim alanında ad tanımlar ([bildirimleri](basic-concepts.md#declarations)) söz konusu sınıfın.</span><span class="sxs-lookup"><span data-stu-id="6e059-174">Each type parameter in a class declaration defines a name in the declaration space ([Declarations](basic-concepts.md#declarations)) of that class.</span></span> <span data-ttu-id="6e059-175">Bu nedenle, başka bir tür parametresiyle aynı ada sahip olamaz veya bu sınıfta bir üye bildirildi.</span><span class="sxs-lookup"><span data-stu-id="6e059-175">Thus, it cannot have the same name as another type parameter or a member declared in that class.</span></span> <span data-ttu-id="6e059-176">Bir tür parametresi türü ile aynı ada sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-176">A type parameter cannot have the same name as the type itself.</span></span>

### <a name="class-base-specification"></a><span data-ttu-id="6e059-177">Temel sınıfı belirtimi</span><span class="sxs-lookup"><span data-stu-id="6e059-177">Class base specification</span></span>

<span data-ttu-id="6e059-178">Bir sınıf bildirimi içerebilir bir *class_base* tanımlayan doğrudan temel sınıf sınıf ve arabirim belirtimi ([arabirimleri](interfaces.md)) doğrudan sınıf tarafından uygulanan.</span><span class="sxs-lookup"><span data-stu-id="6e059-178">A class declaration may include a *class_base* specification, which defines the direct base class of the class and the interfaces ([Interfaces](interfaces.md)) directly implemented by the class.</span></span>

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

<span data-ttu-id="6e059-179">Oluşturulan sınıf türü bir sınıf bildiriminde belirtilen taban sınıfı olabilir ([oluşturulan türler](types.md#constructed-types)).</span><span class="sxs-lookup"><span data-stu-id="6e059-179">The base class specified in a class declaration can be a constructed class type ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="6e059-180">Kapsam türü parametreler içerebilir ancak bir taban sınıfı kendi kendine, bir tür parametresi olamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-180">A base class cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span>

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a><span data-ttu-id="6e059-181">Temel sınıflar</span><span class="sxs-lookup"><span data-stu-id="6e059-181">Base classes</span></span>

<span data-ttu-id="6e059-182">Olduğunda bir *class_type* dahil *class_base*, bildirilen sınıf doğrudan temel sınıfını belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-182">When a *class_type* is included in the *class_base*, it specifies the direct base class of the class being declared.</span></span> <span data-ttu-id="6e059-183">Bir sınıf bildirimi Hayır varsa *class_base*, veya *class_base* listeleri yalnızca arabirim türleri, doğrudan temel sınıf olarak kabul edilir `object`.</span><span class="sxs-lookup"><span data-stu-id="6e059-183">If a class declaration has no *class_base*, or if the *class_base* lists only interface types, the direct base class is assumed to be `object`.</span></span> <span data-ttu-id="6e059-184">Bölümünde anlatıldığı gibi bir sınıf üyelerini doğrudan kendi temel sınıfından devralır [devralma](classes.md#inheritance).</span><span class="sxs-lookup"><span data-stu-id="6e059-184">A class inherits members from its direct base class, as described in [Inheritance](classes.md#inheritance).</span></span>

<span data-ttu-id="6e059-185">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-185">In the example</span></span>
```csharp
class A {}

class B: A {}
```
<span data-ttu-id="6e059-186">sınıf `A` doğrudan temel sınıfını olduğu söylenir `B`, ve `B` nesnesinden türetilmesi söylenir `A`.</span><span class="sxs-lookup"><span data-stu-id="6e059-186">class `A` is said to be the direct base class of `B`, and `B` is said to be derived from `A`.</span></span> <span data-ttu-id="6e059-187">Bu yana `A` mu doğrudan temel sınıfının açıkça belirtin, doğrudan temel sınıfı örtülü olarak `object`.</span><span class="sxs-lookup"><span data-stu-id="6e059-187">Since `A` does not explicitly specify a direct base class, its direct base class is implicitly `object`.</span></span>

<span data-ttu-id="6e059-188">Genel sınıf bildiriminde bir temel sınıf belirtilmişse oluşturulan sınıf türü için oluşturulan tür taban sınıfı, her biri için değiştirerek elde edilen *type_parameter* karşılık gelen taban sınıf bildiriminde *type_argument* oluşturulan türü.</span><span class="sxs-lookup"><span data-stu-id="6e059-188">For a constructed class type, if a base class is specified in the generic class declaration, the base class of the constructed type is obtained by substituting, for each *type_parameter* in the base class declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="6e059-189">Genel sınıf bildirimleri verildiğinde</span><span class="sxs-lookup"><span data-stu-id="6e059-189">Given the generic class declarations</span></span>
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
<span data-ttu-id="6e059-190">oluşturulan tür temel sınıfını `G<int>` olacaktır `B<string,int[]>`.</span><span class="sxs-lookup"><span data-stu-id="6e059-190">the base class of the constructed type `G<int>` would be `B<string,int[]>`.</span></span>

<span data-ttu-id="6e059-191">Bir sınıf türünün doğrudan temel sınıf en az sınıf türü kendisini olarak erişilebilir olmalıdır ([erişilebilirlik etki alanları](basic-concepts.md#accessibility-domains)).</span><span class="sxs-lookup"><span data-stu-id="6e059-191">The direct base class of a class type must be at least as accessible as the class type itself ([Accessibility domains](basic-concepts.md#accessibility-domains)).</span></span> <span data-ttu-id="6e059-192">Örneğin, bir derleme zamanı hatası için olduğu bir `public` öğesinden türetilen sınıfın bir `private` veya `internal` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="6e059-192">For example, it is a compile-time error for a `public` class to derive from a `private` or `internal` class.</span></span>

<span data-ttu-id="6e059-193">Bir sınıf türünün doğrudan temel sınıf aşağıdaki türlerinden herhangi birini olmamalıdır: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, veya `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="6e059-193">The direct base class of a class type must not be any of the following types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="6e059-194">Ayrıca, bir genel sınıf bildiriminde kullanılamaz `System.Attribute` doğrudan veya dolaylı bir temel sınıf olarak.</span><span class="sxs-lookup"><span data-stu-id="6e059-194">Furthermore, a generic class declaration cannot use `System.Attribute` as a direct or indirect base class.</span></span>

<span data-ttu-id="6e059-195">Doğrudan temel sınıf belirtimi anlamını belirlemek çalışırken `A` bir sınıfın `B`, doğrudan temel sınıfını `B` geçici olarak kabul edilir `object`.</span><span class="sxs-lookup"><span data-stu-id="6e059-195">While determining the meaning of the direct base class specification `A` of a class `B`, the direct base class of `B` is temporarily assumed to be `object`.</span></span> <span data-ttu-id="6e059-196">Bunun anlamı bir temel sınıf belirtimi, özyinelemeli olarak olamaz sezgisel sağlar kendisine bağımlı.</span><span class="sxs-lookup"><span data-stu-id="6e059-196">Intuitively this ensures that the meaning of a base class specification cannot recursively depend on itself.</span></span> <span data-ttu-id="6e059-197">Örnek:</span><span class="sxs-lookup"><span data-stu-id="6e059-197">The example:</span></span>
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
<span data-ttu-id="6e059-198">temel sınıf belirtiminde hata beri bulunduğu `A<C.B>` doğrudan temel sınıfını `C` olduğu kabul edilir `object`ve bu nedenle (kurallarına göre [Namespace ve tür adları](basic-concepts.md#namespace-and-type-names)) `C` olmadığı kabul edilir bir üyeye sahip `B`.</span><span class="sxs-lookup"><span data-stu-id="6e059-198">is in error since in the base class specification `A<C.B>` the direct base class of `C` is considered to be `object`, and hence (by the rules of [Namespace and type names](basic-concepts.md#namespace-and-type-names))  `C` is not considered to have a member `B`.</span></span>

<span data-ttu-id="6e059-199">Temel bir sınıf türünün doğrudan temel sınıfı ve temel sınıfları sınıflardır.</span><span class="sxs-lookup"><span data-stu-id="6e059-199">The base classes of a class type are the direct base class and its base classes.</span></span> <span data-ttu-id="6e059-200">Diğer bir deyişle, temel sınıfların doğrudan temel sınıf ilişkisinin geçişli kapatma kümesidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-200">In other words, the set of base classes is the transitive closure of the direct base class relationship.</span></span> <span data-ttu-id="6e059-201">Yukarıdaki örnekte, temel sınıflarını başvuran `B` olan `A` ve `object`.</span><span class="sxs-lookup"><span data-stu-id="6e059-201">Referring to the example above, the base classes of `B` are `A` and `object`.</span></span> <span data-ttu-id="6e059-202">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-202">In the example</span></span>
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
<span data-ttu-id="6e059-203">Taban sınıflarını `D<int>` olan `C<int[]>`, `B<IComparable<int[]>>`, `A`, ve `object`.</span><span class="sxs-lookup"><span data-stu-id="6e059-203">the base classes of `D<int>` are `C<int[]>`, `B<IComparable<int[]>>`, `A`, and `object`.</span></span>

<span data-ttu-id="6e059-204">Sınıf dışında `object`, her sınıf türünde tam olarak bir doğrudan temel sınıf.</span><span class="sxs-lookup"><span data-stu-id="6e059-204">Except for class `object`, every class type has exactly one direct base class.</span></span> <span data-ttu-id="6e059-205">`object` Sınıf doğrudan temel sınıfa sahip ve diğer sınıfların ultimate temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="6e059-205">The `object` class has no direct base class and is the ultimate base class of all other classes.</span></span>

<span data-ttu-id="6e059-206">Bir sınıf, `B` bir sınıftan türetilen `A`, bir derleme zamanı hata için `A` bağlıdır `B`.</span><span class="sxs-lookup"><span data-stu-id="6e059-206">When a class `B` derives from a class `A`, it is a compile-time error for `A` to depend on `B`.</span></span> <span data-ttu-id="6e059-207">Bir sınıf ***doğrudan bağlı*** doğrudan temel sınıfı (varsa) ve ***doğrudan bağlı*** içinde bunu hemen iç içe geçmiş (varsa) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="6e059-207">A class ***directly depends on*** its direct base class (if any) and ***directly depends on*** the class within which it is immediately nested (if any).</span></span> <span data-ttu-id="6e059-208">Bu tanımı verildiğinde, tam bir sınıf olduğu sınıf yansıma ve geçişli kapatılmasını kümesidir ***doğrudan bağlı*** ilişki.</span><span class="sxs-lookup"><span data-stu-id="6e059-208">Given this definition, the complete set of classes upon which a class depends is the reflexive and transitive closure of the ***directly depends on*** relationship.</span></span>

<span data-ttu-id="6e059-209">Örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-209">The example</span></span>
```csharp
class A: A {}
```
<span data-ttu-id="6e059-210">sınıf kendisine güveniyor hatalı olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-210">is erroneous because the class depends on itself.</span></span> <span data-ttu-id="6e059-211">Benzer şekilde, örneğin</span><span class="sxs-lookup"><span data-stu-id="6e059-211">Likewise, the example</span></span>
```csharp
class A: B {}
class B: C {}
class C: A {}
```
<span data-ttu-id="6e059-212">hata, sınıfları döngüsel olarak kendilerini üzerinde bağımlı olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-212">is in error because the classes circularly depend on themselves.</span></span> <span data-ttu-id="6e059-213">Son olarak, örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-213">Finally, the example</span></span>
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
<span data-ttu-id="6e059-214">sonuçları bir derleme zamanı hatası nedeniyle `A` bağlıdır `B.C` (doğrudan temel sınıfı), bağımlı olan `B` (kapsayan sınıfı) döngüsel olarak bağımlı olan `A`.</span><span class="sxs-lookup"><span data-stu-id="6e059-214">results in a compile-time error because `A` depends on `B.C` (its direct base class), which depends on `B` (its immediately enclosing class), which circularly depends on `A`.</span></span>

<span data-ttu-id="6e059-215">Bir sınıf içinde iç içe sınıflarda benzemez unutmayın.</span><span class="sxs-lookup"><span data-stu-id="6e059-215">Note that a class does not depend on the classes that are nested within it.</span></span> <span data-ttu-id="6e059-216">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-216">In the example</span></span>
```csharp
class A
{
    class B: A {}
}
```
<span data-ttu-id="6e059-217">`B` bağımlı `A` (çünkü `A` hem doğrudan temel sınıfı, hem de kapsayan sınıfı), ancak `A` bağlı değildir `B` (bu yana `B` bir temel sınıf ya da kapsayan bir sınıfı değil `A` ).</span><span class="sxs-lookup"><span data-stu-id="6e059-217">`B` depends on `A` (because `A` is both its direct base class and its immediately enclosing class), but `A` does not depend on `B` (since `B` is neither a base class nor an enclosing class of `A`).</span></span> <span data-ttu-id="6e059-218">Bu nedenle, örneğin geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="6e059-218">Thus, the example is valid.</span></span>

<span data-ttu-id="6e059-219">Öğesinden türetilen mümkün değil bir `sealed` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="6e059-219">It is not possible to derive from a `sealed` class.</span></span> <span data-ttu-id="6e059-220">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-220">In the example</span></span>
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
<span data-ttu-id="6e059-221">sınıf `B` hata türetmek çalışır çünkü `sealed` sınıfı `A`.</span><span class="sxs-lookup"><span data-stu-id="6e059-221">class `B` is in error because it attempts to derive from the `sealed` class `A`.</span></span>

#### <a name="interface-implementations"></a><span data-ttu-id="6e059-222">Arabirim uygulamaları</span><span class="sxs-lookup"><span data-stu-id="6e059-222">Interface implementations</span></span>

<span data-ttu-id="6e059-223">A *class_base* belirtimi içinde çalışması sınıfı söyledi doğrudan belirli bir arabirim türleri uygulamak için arabirim türlerinin bir listesini içerebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-223">A *class_base* specification may include a list of interface types, in which case the class is said to directly implement the given interface types.</span></span> <span data-ttu-id="6e059-224">Arabirim uygulamalarına açıklanmıştır daha ayrıntılı olarak [arabirimi uygulamaları](interfaces.md#interface-implementations).</span><span class="sxs-lookup"><span data-stu-id="6e059-224">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="type-parameter-constraints"></a><span data-ttu-id="6e059-225">Tür parametresi kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="6e059-225">Type parameter constraints</span></span>

<span data-ttu-id="6e059-226">Genel tür ve yöntem bildirimlerinde isteğe bağlı olarak belirtebilirsiniz tür parametresi kısıtlamaları dahil ederek *type_parameter_constraints_clause*s.</span><span class="sxs-lookup"><span data-stu-id="6e059-226">Generic type and method declarations can optionally specify type parameter constraints by including *type_parameter_constraints_clause*s.</span></span>

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

<span data-ttu-id="6e059-227">Her *type_parameter_constraints_clause* belirtecini oluşur `where`, bir tür parametre adından önce gelen, ardından bir iki nokta üst üste ve bu tür parametresi kısıtlamaları listesi.</span><span class="sxs-lookup"><span data-stu-id="6e059-227">Each *type_parameter_constraints_clause* consists of the token `where`, followed by the name of a type parameter, followed by a colon and the list of constraints for that type parameter.</span></span> <span data-ttu-id="6e059-228">En fazla bir olabilir `where` her tür parametresi için yan tümcesi ve `where` yan tümceleri herhangi bir sırada listelenen.</span><span class="sxs-lookup"><span data-stu-id="6e059-228">There can be at most one `where` clause for each type parameter, and the `where` clauses can be listed in any order.</span></span> <span data-ttu-id="6e059-229">Gibi `get` ve `set` bir özellik erişimcisi belirteçlerinde `where` belirteci bir anahtar değil.</span><span class="sxs-lookup"><span data-stu-id="6e059-229">Like the `get` and `set` tokens in a property accessor, the `where` token is not a keyword.</span></span>

<span data-ttu-id="6e059-230">Verilen kısıtlamalar listesine bir `where` yan tümcesi, bu sırada aşağıdaki bileşenlerden birini içerebilir: tek bir birincil kısıtlaması, bir veya daha fazla ikincil kısıtlamalar ve oluşturucu kısıtlaması `new()`.</span><span class="sxs-lookup"><span data-stu-id="6e059-230">The list of constraints given in a `where` clause can include any of the following components, in this order: a single primary constraint, one or more secondary constraints, and the constructor constraint, `new()`.</span></span>

<span data-ttu-id="6e059-231">Birincil kısıtlaması bir sınıf türü olabilir veya ***başvuru türü kısıtlaması*** `class` veya ***değer türü kısıtlaması*** `struct`.</span><span class="sxs-lookup"><span data-stu-id="6e059-231">A primary constraint can be a class type or the ***reference type constraint*** `class` or the ***value type constraint*** `struct`.</span></span> <span data-ttu-id="6e059-232">İkincil bir kısıtlama olabilir bir *type_parameter* veya *INTERFACE_TYPE*.</span><span class="sxs-lookup"><span data-stu-id="6e059-232">A secondary constraint can be a *type_parameter* or *interface_type*.</span></span>

<span data-ttu-id="6e059-233">Başvuru türü kısıtlaması tür parametresi için kullanılan bir tür bağımsız değişkeni bir başvuru türü olması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-233">The reference type constraint specifies that a type argument used for the type parameter must be a reference type.</span></span> <span data-ttu-id="6e059-234">Tüm sınıf türleri, arabirim türlerinde, temsilci türleri, dizi türleri ve tür parametreleri bir başvuru türü (yukarıda tanımlandığı şekilde) olarak bilinen bu kısıtlamasını.</span><span class="sxs-lookup"><span data-stu-id="6e059-234">All class types, interface types, delegate types, array types, and type parameters known to be a reference type (as defined below) satisfy this constraint.</span></span>

<span data-ttu-id="6e059-235">Değer türü kısıtlaması, tür parametresi için kullanılan bir tür bağımsız değişkeni null olmayan bir değer türü olması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-235">The value type constraint specifies that a type argument used for the type parameter must be a non-nullable value type.</span></span> <span data-ttu-id="6e059-236">Tüm NULL olmayan bir yapı türleri, sabit listesi türleri ve değer türü kısıtlamasına sahip tür parametreleri bu kısıtlamasını.</span><span class="sxs-lookup"><span data-stu-id="6e059-236">All non-nullable struct types, enum types, and type parameters having the value type constraint satisfy this constraint.</span></span> <span data-ttu-id="6e059-237">Bir değer türü, null yapılabilir bir tür sınıflandırılan rağmen unutmayın ([boş değer atanabilir türler](types.md#nullable-types)) değer türü kısıtlamasını karşılamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-237">Note that although classified as a value type, a nullable type ([Nullable types](types.md#nullable-types)) does not satisfy the value type constraint.</span></span> <span data-ttu-id="6e059-238">Değer türü kısıtlamasına sahip bir tür parametresi de olamaz *constructor_constraint*.</span><span class="sxs-lookup"><span data-stu-id="6e059-238">A type parameter having the value type constraint cannot also have the *constructor_constraint*.</span></span>

<span data-ttu-id="6e059-239">İşaretçi türleri, tür bağımsız değişkenleri olarak hiçbir zaman izin verilir ve her iki başvuru türü veya değer türü kısıtlamaları karşılamak için dikkate alınmaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-239">Pointer types are never allowed to be type arguments and are not considered to satisfy either the reference type or value type constraints.</span></span>

<span data-ttu-id="6e059-240">Bir kısıtlaması bir sınıf türü, bir arabirim türü veya tür parametresi olduğundan, bu tür ", bu tür parametresi için kullanılan her tür bağımsız değişkeni desteklemesi gereken bir minimum temel tür" belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-240">If a constraint is a class type, an interface type, or a type parameter, that type specifies a minimal "base type" that every type argument used for that type parameter must support.</span></span> <span data-ttu-id="6e059-241">Oluşturulan tür ya da genel yöntemin kullanıldığı her tür bağımsız değişkeni derleme zamanında tür parametresi kısıtlamaları karşılaştırılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-241">Whenever a constructed type or generic method is used, the type argument is checked against the constraints on the type parameter at compile-time.</span></span> <span data-ttu-id="6e059-242">Sağlanan tür bağımsız değişkeni açıklanan koşulları karşılaması gerekir [kısıtlamaları karşılamadığınızı](types.md#satisfying-constraints).</span><span class="sxs-lookup"><span data-stu-id="6e059-242">The type argument supplied must satisfy the conditions described in [Satisfying constraints](types.md#satisfying-constraints).</span></span>

<span data-ttu-id="6e059-243">A *class_type* kısıtlamasını aşağıdaki kurallar gerekir:</span><span class="sxs-lookup"><span data-stu-id="6e059-243">A *class_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="6e059-244">Türü, sınıf türünde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-244">The type must be a class type.</span></span>
*  <span data-ttu-id="6e059-245">Türü olmamalıdır `sealed`.</span><span class="sxs-lookup"><span data-stu-id="6e059-245">The type must not be `sealed`.</span></span>
*  <span data-ttu-id="6e059-246">Türü aşağıdaki türlerden biri olmamalıdır: `System.Array`, `System.Delegate`, `System.Enum`, veya `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="6e059-246">The type must not be one of the following types: `System.Array`, `System.Delegate`, `System.Enum`, or `System.ValueType`.</span></span>
*  <span data-ttu-id="6e059-247">Türü olmamalıdır `object`.</span><span class="sxs-lookup"><span data-stu-id="6e059-247">The type must not be `object`.</span></span> <span data-ttu-id="6e059-248">Türetilen tüm türler için `object`, bu izin veriyorsa, bu tür bir kısıtlama hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="6e059-248">Because all types derive from `object`, such a constraint would have no effect if it were permitted.</span></span>
*  <span data-ttu-id="6e059-249">En fazla bir kısıtlaması belirtilen tür parametresi için bir sınıf türü olabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-249">At most one constraint for a given type parameter can be a class type.</span></span>

<span data-ttu-id="6e059-250">Bir tür olarak belirtilen bir *INTERFACE_TYPE* kısıtlamasını aşağıdaki kurallar gerekir:</span><span class="sxs-lookup"><span data-stu-id="6e059-250">A type specified as an *interface_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="6e059-251">Türü bir arabirim türü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-251">The type must be an interface type.</span></span>
*  <span data-ttu-id="6e059-252">Bir türü birden fazla kez belirtilmesi gerekir değil bir verilen `where` yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="6e059-252">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="6e059-253">Her iki durumda da kısıtlaması herhangi bir türü parametre türüyle veya kapsamında oluşturulan tür yöntem bildiriminde içerebilir ve bildirilen türü içerebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-253">In either case, the constraint can involve any of the type parameters of the associated type or method declaration as part of a constructed type, and can involve the type being declared.</span></span>

<span data-ttu-id="6e059-254">Bir tür parametresi kısıtlaması en az olarak erişilebilir olarak belirtilen herhangi bir sınıf veya arabirim türü ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) genel tür veya yöntem bildirilen.</span><span class="sxs-lookup"><span data-stu-id="6e059-254">Any class or interface type specified as a type parameter constraint must be at least as accessible ([Accessibility constraints](basic-concepts.md#accessibility-constraints)) as the generic type or method being declared.</span></span>

<span data-ttu-id="6e059-255">Bir tür olarak belirtilen bir *type_parameter* kısıtlamasını aşağıdaki kurallar gerekir:</span><span class="sxs-lookup"><span data-stu-id="6e059-255">A type specified as a *type_parameter* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="6e059-256">Türü bir tür parametresi olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-256">The type must be a type parameter.</span></span>
*  <span data-ttu-id="6e059-257">Bir türü birden fazla kez belirtilmesi gerekir değil bir verilen `where` yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="6e059-257">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="6e059-258">Buna ek olarak olmalıdır hiçbir döngü bağımlılık tarafından tanımlanan geçişli bir ilişki olduğu tür parametrelerinin bağımlılık grafiğinde:</span><span class="sxs-lookup"><span data-stu-id="6e059-258">In addition there must be no cycles in the dependency graph of type parameters, where dependency is a transitive relation defined by:</span></span>

*  <span data-ttu-id="6e059-259">Bir tür parametresi, `T` tür parametresi için bir kısıtlama olarak kullanılan `S` ardından `S` ***bağlıdır*** `T`.</span><span class="sxs-lookup"><span data-stu-id="6e059-259">If a type parameter `T` is used as a constraint for type parameter `S` then `S` ***depends on*** `T`.</span></span>
*  <span data-ttu-id="6e059-260">Bir tür parametresi, `S` bir tür parametresine bağlı `T` ve `T` bir tür parametresine bağlı `U` ardından `S` ***bağlıdır*** `U`.</span><span class="sxs-lookup"><span data-stu-id="6e059-260">If a type parameter `S` depends on a type parameter `T` and `T` depends on a type parameter `U` then `S` ***depends on*** `U`.</span></span>

<span data-ttu-id="6e059-261">Bu ilişki göz önünde bulundurulduğunda, kendisine bağımlı (doğrudan veya dolaylı olarak) için bir tür parametresi için bir derleme zamanı hatası olduğundan.</span><span class="sxs-lookup"><span data-stu-id="6e059-261">Given this relation, it is a compile-time error for a type parameter to depend on itself (directly or indirectly).</span></span>

<span data-ttu-id="6e059-262">Kısıtlamalardan bağımlı tür parametreleri arasında tutarlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-262">Any constraints must be consistent among dependent type parameters.</span></span> <span data-ttu-id="6e059-263">Tür parametresi `S` tür parametresine bağlı `T` sonra:</span><span class="sxs-lookup"><span data-stu-id="6e059-263">If type parameter `S` depends on type parameter `T` then:</span></span>

*  <span data-ttu-id="6e059-264">`T` değer türü kısıtlaması olamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-264">`T` must not have the value type constraint.</span></span> <span data-ttu-id="6e059-265">Aksi takdirde, `T` etkili bir şekilde bunu korumalı `S` aynı türde olacak şekilde zorlanması `T`, iki tür parametreleri gereği ortadan kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-265">Otherwise, `T` is effectively sealed so `S` would be forced to be the same type as `T`, eliminating the need for two type parameters.</span></span>
*  <span data-ttu-id="6e059-266">Varsa `S` sonra değer türü kısıtlamasına sahip `T` değil olmalıdır bir *class_type* kısıtlaması.</span><span class="sxs-lookup"><span data-stu-id="6e059-266">If `S` has the value type constraint then `T` must not have a *class_type* constraint.</span></span>
*  <span data-ttu-id="6e059-267">Varsa `S` sahip bir *class_type* kısıtlaması `A` ve `T` sahip bir *class_type* kısıtlaması `B` olmalıdır sonra bir kimlik dönüştürme ya da örtük başvuru dönüştürmesi `A` için `B` veya bir örtük bir başvuru dönüştürme `B` için `A`.</span><span class="sxs-lookup"><span data-stu-id="6e059-267">If `S` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>
*  <span data-ttu-id="6e059-268">Varsa `S` ayrıca tür parametresine bağlı `U` ve `U` sahip bir *class_type* kısıtlaması `A` ve `T` sahip bir *class_type* kısıtlaması`B` olmalıdır sonra bir kimlik dönüştürme ya da örtük bir başvuru dönüştürmesi `A` için `B` veya bir örtük bir başvuru dönüştürme `B` için `A`.</span><span class="sxs-lookup"><span data-stu-id="6e059-268">If `S` also depends on type parameter `U` and `U` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>

<span data-ttu-id="6e059-269">İçin geçerli olduğundan `S` değer türü kısıtlaması olmasını ve `T` başvuru türü kısıtlamasına sahip.</span><span class="sxs-lookup"><span data-stu-id="6e059-269">It is valid for `S` to have the value type constraint and `T` to have the reference type constraint.</span></span> <span data-ttu-id="6e059-270">Etkili bir şekilde bu sınırlar `T` türlerine `System.Object`, `System.ValueType`, `System.Enum`ve herhangi bir arabirim türü.</span><span class="sxs-lookup"><span data-stu-id="6e059-270">Effectively this limits `T` to the types `System.Object`, `System.ValueType`, `System.Enum`, and any interface type.</span></span>

<span data-ttu-id="6e059-271">Varsa `where` yan tümcesi bir tür parametresi için bir oluşturucu kısıtlaması içerir (formu olan `new()`), kullanmak mümkün mü `new` türünün örneğini oluşturmak için işleç ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="6e059-271">If the `where` clause for a type parameter includes a constructor constraint (which has the form `new()`), it is possible to use the `new` operator to create instances of the type ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span> <span data-ttu-id="6e059-272">Kullanılan bağımsız değişkeni girin Oluşturucu kısıtlaması tür parametreyle (Bu oluşturucu örtük olarak mevcut herhangi bir değer türü için) ortak bir parametresiz oluşturucusu olmalıdır veya değer türü kısıtlaması veya Oluşturucu kısıtlaması (bkz: sahip bir tür parametresi olmalıdır [Tür parametresi kısıtlamaları](classes.md#type-parameter-constraints) Ayrıntılar için).</span><span class="sxs-lookup"><span data-stu-id="6e059-272">Any type argument used for a type parameter with a constructor constraint must have a public parameterless constructor (this constructor implicitly exists for any value type) or be a type parameter having the value type constraint or constructor constraint (see [Type parameter constraints](classes.md#type-parameter-constraints) for details).</span></span>

<span data-ttu-id="6e059-273">Kısıtlamaları örnekleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="6e059-273">The following are examples of constraints:</span></span>
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

<span data-ttu-id="6e059-274">Aşağıdaki örnekte hata çünkü tür parametrelerinin bağımlılık grafiğinde bir döngü neden olur:</span><span class="sxs-lookup"><span data-stu-id="6e059-274">The following example is in error because it causes a circularity in the dependency graph of the type parameters:</span></span>
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

<span data-ttu-id="6e059-275">Aşağıdaki örnekler, ek geçersiz durumlar gösterir:</span><span class="sxs-lookup"><span data-stu-id="6e059-275">The following examples illustrate additional invalid situations:</span></span>
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

<span data-ttu-id="6e059-276">***Etkin bir temel sınıf*** bir tür parametresini `T` şu şekilde tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="6e059-276">The ***effective base class*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="6e059-277">Varsa `T` birincil hiçbir kısıtlama veya tür parametresi kısıtlamaları, etkili temel sınıfı `object`.</span><span class="sxs-lookup"><span data-stu-id="6e059-277">If `T` has no primary constraints or type parameter constraints, its effective base class is `object`.</span></span>
*  <span data-ttu-id="6e059-278">Varsa `T` değer türü kısıtlaması vardır, geçerli bir temel sınıf `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="6e059-278">If `T` has the value type constraint, its effective base class is `System.ValueType`.</span></span>
*  <span data-ttu-id="6e059-279">Varsa `T` sahip bir *class_type* kısıtlaması `C` ancak hiçbir *type_parameter* kısıtlamaları, etkili temel sınıfı olan `C`.</span><span class="sxs-lookup"><span data-stu-id="6e059-279">If `T` has a *class_type* constraint `C` but no *type_parameter* constraints, its effective base class is `C`.</span></span>
*  <span data-ttu-id="6e059-280">Varsa `T` hiçbir *class_type* kısıtlama ancak sahip bir veya daha fazla *type_parameter* kısıtlamaları, etkili temel sınıfındaki en encompassed türüdür ([yükseltilmiş dönüştürme işleçler](conversions.md#lifted-conversion-operators)) etkili temel sınıfları kümesi kendi *type_parameter* kısıtlamaları.</span><span class="sxs-lookup"><span data-stu-id="6e059-280">If `T` has no *class_type* constraint but has one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set of effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="6e059-281">Tutarlık kuralları en encompassed bir tür olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="6e059-281">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="6e059-282">Varsa `T` hem de bir *class_type* kısıtlaması ve bir veya daha fazla *type_parameter* kısıtlamaları, etkili temel sınıfındaki en encompassed türüdür ([yükseltilmiş dönüştürme işleçler](conversions.md#lifted-conversion-operators)) oluşan kümesindeki *class_type* kısıtlamasını `T` ve etkili temel sınıfları, *type_parameter* kısıtlamaları.</span><span class="sxs-lookup"><span data-stu-id="6e059-282">If `T` has both a *class_type* constraint and one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set consisting of the *class_type* constraint of `T` and the effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="6e059-283">Tutarlık kuralları en encompassed bir tür olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="6e059-283">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="6e059-284">Varsa `T` başvuru türü kısıtlaması ancak Hayır *class_type* kısıtlamaları, etkili temel sınıfı olan `object`.</span><span class="sxs-lookup"><span data-stu-id="6e059-284">If `T` has the reference type constraint but no *class_type* constraints, its effective base class is `object`.</span></span>

<span data-ttu-id="6e059-285">T bir kısıtlama varsa bu kurallar amacıyla `V` diğer bir deyişle bir *value_type*, bunun yerine en belirgin taban türünü kullanın `V` diğer bir deyişle bir *class_type*.</span><span class="sxs-lookup"><span data-stu-id="6e059-285">For the purpose of these rules, if T has a constraint `V` that is a *value_type*, use instead the most specific base type of `V` that is a *class_type*.</span></span> <span data-ttu-id="6e059-286">Hiçbir zaman içinde açıkça belirtilen bir kısıtlama olabilir, ancak kısıtlamaları dahilinde bir genel yöntem, örtük olarak geçersiz kılan bir yöntem bildiriminde veya bir arabirim yönteminin açık bir uygulama tarafından devralınır oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-286">This can never happen in an explicitly given constraint, but may occur when the constraints of a generic method are implicitly inherited by an overriding method declaration or an explicit implementation of an interface method.</span></span>

<span data-ttu-id="6e059-287">Bu kurallar etkin bir temel sınıf her zaman olduğundan emin olun. bir *class_type*.</span><span class="sxs-lookup"><span data-stu-id="6e059-287">These rules ensure that the effective base class is always a *class_type*.</span></span>

<span data-ttu-id="6e059-288">***Etkili arabirimi kümesi*** bir tür parametresini `T` şu şekilde tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="6e059-288">The ***effective interface set*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="6e059-289">Varsa `T` hiçbir *secondary_constraints*, etkili arabirimi kendine boştur.</span><span class="sxs-lookup"><span data-stu-id="6e059-289">If `T` has no *secondary_constraints*, its effective interface set is empty.</span></span>
*  <span data-ttu-id="6e059-290">Varsa `T` sahip *INTERFACE_TYPE* kısıtlamaları ancak Hayır *type_parameter* kısıtlamaları, etkili arabirimi kendine olan bir dizi *INTERFACE_TYPE* kısıtlamaları.</span><span class="sxs-lookup"><span data-stu-id="6e059-290">If `T` has *interface_type* constraints but no *type_parameter* constraints, its effective interface set is its set of *interface_type* constraints.</span></span>
*  <span data-ttu-id="6e059-291">Varsa `T` hiçbir *INTERFACE_TYPE* ancak kısıtlamalara *type_parameter* kısıtlamaları, etkili arabirimi kendine olan etkili arabirimi kümesi birleşimi, *type_ parametre* kısıtlamaları.</span><span class="sxs-lookup"><span data-stu-id="6e059-291">If `T` has no *interface_type* constraints but has *type_parameter* constraints, its effective interface set is the union of the effective interface sets of its *type_parameter* constraints.</span></span>
*  <span data-ttu-id="6e059-292">Varsa `T` hem de *INTERFACE_TYPE* kısıtlamaları ve *type_parameter* kısıtlamaları, etkili arabirimi kendine olan bir dizi birleşimi *INTERFACE_TYPE* kısıtlamalar ve geçerli arabirimi kümeleri kendi *type_parameter* kısıtlamaları.</span><span class="sxs-lookup"><span data-stu-id="6e059-292">If `T` has both *interface_type* constraints and *type_parameter* constraints, its effective interface set is the union of its set of *interface_type* constraints and the effective interface sets of its *type_parameter* constraints.</span></span>

<span data-ttu-id="6e059-293">Bir tür parametresi ***bir başvuru türü olduğu bilinen*** başvuru türü kısıtlaması olan veya bunun geçerli bir taban sınıf değil `object` veya `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="6e059-293">A type parameter is ***known to be a reference type*** if it has the reference type constraint or its effective base class is not `object` or `System.ValueType`.</span></span>

<span data-ttu-id="6e059-294">Kısıtlanmış bir tür parametre türü değerlerinin kısıtlamalar tarafından kapsanan örneği üyelerine erişmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-294">Values of a constrained type parameter type can be used to access the instance members implied by the constraints.</span></span> <span data-ttu-id="6e059-295">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-295">In the example</span></span>
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
<span data-ttu-id="6e059-296">yöntemlerinin `IPrintable` doğrudan çağrılabilir `x` çünkü `T` her zaman uygulamak için kısıtlı `IPrintable`.</span><span class="sxs-lookup"><span data-stu-id="6e059-296">the methods of `IPrintable` can be invoked directly on `x` because `T` is constrained to always implement `IPrintable`.</span></span>

### <a name="class-body"></a><span data-ttu-id="6e059-297">Sınıfın gövdesi</span><span class="sxs-lookup"><span data-stu-id="6e059-297">Class body</span></span>

<span data-ttu-id="6e059-298">*Class_body* söz konusu sınıfın üyeleri bir sınıfı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="6e059-298">The *class_body* of a class defines the members of that class.</span></span>

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a><span data-ttu-id="6e059-299">Kısmi türler</span><span class="sxs-lookup"><span data-stu-id="6e059-299">Partial types</span></span>

<span data-ttu-id="6e059-300">Birden çok tür bildirimi bölünebilir ***kısmi tür bildirimleri***.</span><span class="sxs-lookup"><span data-stu-id="6e059-300">A type declaration can be split across multiple ***partial type declarations***.</span></span> <span data-ttu-id="6e059-301">Program derleme zamanı ve çalışma zamanı işlenmesi sırasında kalan tek bir bildirimi olarak kabul edilir kullanılması türü bildirimi bu bölümde, kurallara, parça oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6e059-301">The type declaration is constructed from its parts by following the rules in this section, whereupon it is treated as a single declaration during the remainder of the compile-time and run-time processing of the program.</span></span>

<span data-ttu-id="6e059-302">A *class_declaration*, *struct_declaration* veya *interface_declaration* içeriyorsa, kısmi türü bildirimi temsil eden bir `partial` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-302">A *class_declaration*, *struct_declaration* or *interface_declaration* represents a partial type declaration if it includes a `partial` modifier.</span></span> <span data-ttu-id="6e059-303">`partial` bir anahtar sözcük değildir ve anahtar sözcükleri hemen birini önce görünürse yalnızca bir değiştirici olarak davranan `class`, `struct` veya `interface` tür bildiriminde veya türünden önce `void` yöntem bildiriminde.</span><span class="sxs-lookup"><span data-stu-id="6e059-303">`partial` is not a keyword, and only acts as a modifier if it appears immediately before one of the keywords `class`, `struct` or `interface` in a type declaration, or before the type `void` in a method declaration.</span></span> <span data-ttu-id="6e059-304">Diğer bağlamlarda Bu, normal bir tanımlayıcı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-304">In other contexts it can be used as a normal identifier.</span></span>

<span data-ttu-id="6e059-305">Her bir parçasının kısmi türü bildirimi içermelidir bir `partial` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-305">Each part of a partial type declaration must include a `partial` modifier.</span></span> <span data-ttu-id="6e059-306">Aynı ada sahip olmalıdır ve aynı ad alanı veya tür bildirimi diğer bölümleri olarak bildirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-306">It must have the same name  and be declared in the same namespace or type declaration as the other parts.</span></span> <span data-ttu-id="6e059-307">`partial` Değiştiricisi belirten ek bölümlerini türü bildirimi başka bir yerde bulunabilir, ancak bu tür bölümler varlığını zorunlu değildir; eklemek bir tür için tek bir bildirimde ile geçerli `partial` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-307">The `partial` modifier indicates that additional parts of the type declaration may exist elsewhere, but the existence of such additional parts is not a requirement; it is valid for a type with a single declaration to include the `partial` modifier.</span></span>

<span data-ttu-id="6e059-308">Parçaları bir tek tür bildirimi derleme zamanında birleştirilebilir, kısmi bir türde tüm parçalarını birlikte derlenmelidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-308">All parts of a partial type must be compiled together such that the parts can be merged at compile-time into a single type declaration.</span></span> <span data-ttu-id="6e059-309">Kısmi türler özellikle genişletilmesi önceden derlenmiş türler izin vermez.</span><span class="sxs-lookup"><span data-stu-id="6e059-309">Partial types specifically do not allow already compiled types to be extended.</span></span>

<span data-ttu-id="6e059-310">İç içe geçmiş türler belirtilebilir birden çok bölümlerinde kullanarak `partial` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-310">Nested types may be declared in multiple parts by using the `partial` modifier.</span></span> <span data-ttu-id="6e059-311">Genellikle, kapsayan tür kullanarak bildirildiği `partial` iyi ve iç içe türün her bir parçası olarak bildirilen gibi farklı bir kapsayan tür parçası.</span><span class="sxs-lookup"><span data-stu-id="6e059-311">Typically, the containing type is declared using `partial` as well, and each part of the nested type is declared in a different part of the containing type.</span></span>

<span data-ttu-id="6e059-312">`partial` Değiştiricisi temsilci veya sabit listesi bildirimlerinde izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="6e059-312">The `partial` modifier is not permitted on delegate or enum declarations.</span></span>

### <a name="attributes"></a><span data-ttu-id="6e059-313">Öznitelikler</span><span class="sxs-lookup"><span data-stu-id="6e059-313">Attributes</span></span>

<span data-ttu-id="6e059-314">Kısmi bir türün öznitelikleri, belirsiz bir sırayla her biriyle özniteliklerini bir araya getirerek belirlenir.</span><span class="sxs-lookup"><span data-stu-id="6e059-314">The attributes of a partial type are determined by combining, in an unspecified order, the attributes of each of the parts.</span></span> <span data-ttu-id="6e059-315">Özniteliğin birden çok parça için de yerleştirilirse, öznitelik türünde birden çok kez belirtmeye eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="6e059-315">If an attribute is placed on multiple parts, it is equivalent to specifying the attribute multiple times on the type.</span></span> <span data-ttu-id="6e059-316">Örneğin, iki bölümü:</span><span class="sxs-lookup"><span data-stu-id="6e059-316">For example, the two parts:</span></span>

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
<span data-ttu-id="6e059-317">gibi bir bildirime eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="6e059-317">are equivalent to a declaration such as:</span></span>
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

<span data-ttu-id="6e059-318">Tür parametrelerindeki öznitelikleri benzer bir şekilde birleştirin.</span><span class="sxs-lookup"><span data-stu-id="6e059-318">Attributes on type parameters combine in a similar fashion.</span></span>

### <a name="modifiers"></a><span data-ttu-id="6e059-319">Değiştiriciler</span><span class="sxs-lookup"><span data-stu-id="6e059-319">Modifiers</span></span>

<span data-ttu-id="6e059-320">Ne zaman bir kısmi tür bildirimi içeren bir erişilebilirlik belirtimi ( `public`, `protected`, `internal`, ve `private` değiştiriciler) erişilebilirlik belirtimi içeren diğer tüm bölümleri ile kabul etmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-320">When a partial type declaration includes an accessibility specification (the `public`, `protected`, `internal`, and `private` modifiers) it must agree with all other parts that include an accessibility specification.</span></span> <span data-ttu-id="6e059-321">Kısmi bir türde bir parçası erişilebilirlik belirtimi içeriyorsa, türü uygun varsayılan erişilebilirlik verilir ([erişilebilirlik bildirilen](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="6e059-321">If no part of a partial type includes an accessibility specification, the type is given the appropriate default accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="6e059-322">İç içe türün bir veya daha fazla kısmi bildirimleri içeriyorsa bir `new` değiştiricisi, iç içe türün devralınan bir üyeyi gizler, hiçbir uyarı bildirilen ([devralma yoluyla gizleme](basic-concepts.md#hiding-through-inheritance)).</span><span class="sxs-lookup"><span data-stu-id="6e059-322">If one or more partial declarations of a nested type include a `new` modifier, no warning is reported if the nested type hides an inherited member ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

<span data-ttu-id="6e059-323">Bir sınıfın bir veya daha fazla kısmi bildirimleri içeriyorsa bir `abstract` değiştiricisi, bir sınıf olarak kabul edilir soyut ([soyut sınıflar](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="6e059-323">If one or more partial declarations of a class include an `abstract` modifier, the class is considered abstract ([Abstract classes](classes.md#abstract-classes)).</span></span> <span data-ttu-id="6e059-324">Aksi takdirde, sınıf, soyut olmayan olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-324">Otherwise, the class is considered non-abstract.</span></span>

<span data-ttu-id="6e059-325">Bir sınıfın bir veya daha fazla kısmi bildirimleri içeriyorsa bir `sealed` değiştiricisi, sınıfın korumalı kabul edilir ([sınıflar korumalı](classes.md#sealed-classes)).</span><span class="sxs-lookup"><span data-stu-id="6e059-325">If one or more partial declarations of a class include a `sealed` modifier, the class is considered sealed ([Sealed classes](classes.md#sealed-classes)).</span></span> <span data-ttu-id="6e059-326">Aksi takdirde, sınıf olarak kabul edilir korumasız.</span><span class="sxs-lookup"><span data-stu-id="6e059-326">Otherwise, the class is considered unsealed.</span></span>

<span data-ttu-id="6e059-327">Bir sınıf hem soyut hem korumalı olamaz unutmayın.</span><span class="sxs-lookup"><span data-stu-id="6e059-327">Note that a class cannot be both abstract and sealed.</span></span>

<span data-ttu-id="6e059-328">Zaman `unsafe` değiştiricisi, bir kısmi tür bildiriminde kullanıldığında, yalnızca güvenli olmayan bir bağlam, belirli bir parçası olarak kabul edilir ([güvensiz bir bağlamı](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="6e059-328">When the `unsafe` modifier is used on a partial type declaration, only that particular part is considered an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

### <a name="type-parameters-and-constraints"></a><span data-ttu-id="6e059-329">Tür parametreleri ve kısıtlamalar</span><span class="sxs-lookup"><span data-stu-id="6e059-329">Type parameters and constraints</span></span>

<span data-ttu-id="6e059-330">Bir genel türü birden çok bölümlerinde bildirirse, her parça türü parametreleri belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-330">If a generic type is declared in multiple parts, each part must state the type parameters.</span></span> <span data-ttu-id="6e059-331">Her parça sırada tür parametreleri aynı sayıda ve her tür parametresi için aynı ada sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-331">Each part must have the same number of type parameters, and the same name for each type parameter, in order.</span></span>

<span data-ttu-id="6e059-332">Ne zaman bir kısmi genel tür bildirimi içeren kısıtlamaları (`where` yan tümceleri), kısıtlamaları kısıtlamaları içeren diğer tüm bölümleri ile kabul etmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-332">When a partial generic type declaration includes constraints (`where` clauses), the constraints must agree with all other parts that include constraints.</span></span> <span data-ttu-id="6e059-333">Özellikle, aynı tür parametrelerinin kısıtlamaları kısıtlamaları içeren her bir parçası olması gerekir ve her tür parametresi için kümeleri birincil, ikincil ve Oluşturucusu kısıtlamaları eşdeğer olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-333">Specifically, each part that includes constraints must have constraints for the same set of type parameters, and for each type parameter the sets of primary, secondary, and constructor constraints must be equivalent.</span></span> <span data-ttu-id="6e059-334">Bunlar aynı üyeleri içeriyorsa, iki kısıtlamaları kümesini eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="6e059-334">Two sets of constraints are equivalent if they contain the same members.</span></span> <span data-ttu-id="6e059-335">Tür parametreleri olarak kabul edilir, hiçbir bölümü kısmi ve genel bir türün tür parametresi kısıtlamaları belirtiyorsa, sınırlandırılmamış.</span><span class="sxs-lookup"><span data-stu-id="6e059-335">If no part of a partial generic type specifies type parameter constraints, the type parameters are considered unconstrained.</span></span>

<span data-ttu-id="6e059-336">Örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-336">The example</span></span>
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
<span data-ttu-id="6e059-337">etkili bir şekilde kısıtlamaları (ilk iki) içeren bu bölümleri aynı birincil, ikincil ayarlayın ve aynı türde parametreler kümesi için oluşturucu kısıtlamaları belirttiğinden sırasıyla doğrudur.</span><span class="sxs-lookup"><span data-stu-id="6e059-337">is correct because those parts that include constraints (the first two) effectively specify the same set of primary, secondary, and constructor constraints for the same set of type parameters, respectively.</span></span>

### <a name="base-class"></a><span data-ttu-id="6e059-338">Temel sınıf</span><span class="sxs-lookup"><span data-stu-id="6e059-338">Base class</span></span>

<span data-ttu-id="6e059-339">Kısmi sınıf bildiriminin bir temel sınıf belirtimi içerdiğinde bir temel sınıf belirtimi içeren diğer tüm bölümleri ile kabul etmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-339">When a partial class declaration includes a base class specification it must agree with all other parts that include a base class specification.</span></span> <span data-ttu-id="6e059-340">Hiçbir bölümü, kısmi bir sınıfın temel sınıf belirtimi içeriyorsa, temel sınıf olur `System.Object` ([temel sınıflar](classes.md#base-classes)).</span><span class="sxs-lookup"><span data-stu-id="6e059-340">If no part of a partial class includes a base class specification, the base class becomes `System.Object` ([Base classes](classes.md#base-classes)).</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="6e059-341">Temel arabirimleri</span><span class="sxs-lookup"><span data-stu-id="6e059-341">Base interfaces</span></span>

<span data-ttu-id="6e059-342">Her bölüm üzerinde belirtilen temel arabirimleri birleşimi birden fazla bölümü içinde bildirilen bir tür için temel arabirimleri kümesidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-342">The set of base interfaces for a type declared in multiple parts is the union of the base interfaces specified on each part.</span></span> <span data-ttu-id="6e059-343">Belirli bir taban arabirimi her bölümü yalnızca bir kez adlandırılabilir, ancak aynı temel arabirimlerini adı birden fazla bölümü için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-343">A particular base interface may only be named once on each part, but it is permitted for multiple parts to name the same base interface(s).</span></span> <span data-ttu-id="6e059-344">Yalnızca bir adet herhangi belirli temel arabirim üyeleri olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-344">There must only be one implementation of the members of any given base interface.</span></span>

<span data-ttu-id="6e059-345">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-345">In the example</span></span>
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
<span data-ttu-id="6e059-346">sınıf için temel arabirimleri kümesi `C` olduğu `IA`, `IB`, ve `IC`.</span><span class="sxs-lookup"><span data-stu-id="6e059-346">the set of base interfaces for class `C` is `IA`, `IB`, and `IC`.</span></span>

<span data-ttu-id="6e059-347">Genellikle, her bölümü bu bölümünde bildirilen arabirimlere uygulaması sağlar. Ancak, bu bir gereksinim değildir.</span><span class="sxs-lookup"><span data-stu-id="6e059-347">Typically, each part provides an implementation of the interface(s) declared on that part; however, this is not a requirement.</span></span> <span data-ttu-id="6e059-348">Bir bölümü üzerinde farklı bir parçası olarak bildirilen bir arabirim uygulamasını sağlayabilir:</span><span class="sxs-lookup"><span data-stu-id="6e059-348">A part may provide the implementation for an interface declared on a different part:</span></span>
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

### <a name="members"></a><span data-ttu-id="6e059-349">Üyeler</span><span class="sxs-lookup"><span data-stu-id="6e059-349">Members</span></span>

<span data-ttu-id="6e059-350">Kısmi yöntemler dışında ([kısmi yöntemler](classes.md#partial-methods)), birden fazla bölümü içinde bildirilen bir türün üyeleri yalnızca her bir parçası olarak bildirilen üyeler kümesi birleşimi kümesidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-350">With the exception of partial methods ([Partial methods](classes.md#partial-methods)), the set of members of a type declared in multiple parts is simply the union of the set of members declared in each part.</span></span> <span data-ttu-id="6e059-351">Tüm bölümlerinde tür bildirimi gövdesi aynı bildirim alanına paylaşma ([bildirimleri](basic-concepts.md#declarations)) ve her üye kapsamını ([kapsamları](basic-concepts.md#scopes)) tüm bölümlerin gövdeleri için genişletir.</span><span class="sxs-lookup"><span data-stu-id="6e059-351">The bodies of all parts of the type declaration share the same declaration space ([Declarations](basic-concepts.md#declarations)), and the scope of each member ([Scopes](basic-concepts.md#scopes)) extends to the bodies of all the parts.</span></span> <span data-ttu-id="6e059-352">Erişilebilirlik etki alanı herhangi bir üyenin, her zaman kapsayan türdeki tüm parçaları içerir; bir `private` bir bölümünde bildirilen üye, başka bir kısmından ücretsiz olarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-352">The accessibility domain of any member always includes all the parts of the enclosing type; a `private` member declared in one part is freely accessible from another part.</span></span> <span data-ttu-id="6e059-353">Bu üyenin türü ile olmadığı sürece aynı türde birden fazla bölümü üye bildirmek için bir derleme zamanı hatası olduğu `partial` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-353">It is a compile-time error to declare the same member in more than one part of the type, unless that member is a type with the `partial` modifier.</span></span>

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

<span data-ttu-id="6e059-354">Bir tür içindeki üyelerden sıralama nadiren C# kodu için önemlidir, ancak diğer diller ve ortamları ile arabirim olduğunda önemli olabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-354">The ordering of members within a type is rarely significant to C# code, but may be significant when interfacing with other languages and environments.</span></span> <span data-ttu-id="6e059-355">Bu gibi durumlarda birden fazla bölümü içinde bildirilen bir tür içindeki üyelerden sırası tanımlanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="6e059-355">In these cases, the ordering of members within a type declared in multiple parts is undefined.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="6e059-356">Kısmi yöntemler</span><span class="sxs-lookup"><span data-stu-id="6e059-356">Partial methods</span></span>

<span data-ttu-id="6e059-357">Kısmi yöntemler tür bildirimi bir parçası tanımlanabilir ve başka bir programda uygulanır.</span><span class="sxs-lookup"><span data-stu-id="6e059-357">Partial methods can be defined in one part of a type declaration and implemented in another.</span></span> <span data-ttu-id="6e059-358">Uygulama isteğe bağlıdır; hiçbir bölümü kısmi yöntem uygularsa, kısmi yöntem bildiriminde ve tüm çağrıları bölümleri birleşiminden elde edilen tür bildirimi çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-358">The implementation is optional; if no part implements the partial method, the partial method declaration and all calls to it are removed from the type declaration resulting from the combination of the parts.</span></span>

<span data-ttu-id="6e059-359">Kısmi yöntemler erişim değiştiricileri tanımlayamazsınız ancak örtük olarak olan `private`.</span><span class="sxs-lookup"><span data-stu-id="6e059-359">Partial methods cannot define access modifiers, but are implicitly `private`.</span></span> <span data-ttu-id="6e059-360">Kendi dönüş türü olmalıdır `void`, ve bunların parametreleri olamaz `out` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-360">Their return type must be `void`, and their parameters cannot have the `out` modifier.</span></span> <span data-ttu-id="6e059-361">Tanımlayıcı `partial` yalnızca hemen önce görünürse, özel bir anahtar sözcüğü bir yöntem bildiriminde olarak değerlendirilmiştir `void` yazın; Aksi halde normal bir tanımlayıcı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-361">The identifier `partial` is recognized as a special keyword in a method declaration only if it appears right before the `void` type; otherwise it can be used as a normal identifier.</span></span> <span data-ttu-id="6e059-362">Kısmi bir yöntemin, arabirim yöntemleri olarak açıkça uygulayamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-362">A partial method cannot explicitly implement interface methods.</span></span>

<span data-ttu-id="6e059-363">Kısmi yöntem bildiriminin iki tür vardır: yöntem bildiriminde gövdesi bir noktalı virgül, bildirimi olarak kabul edilir bir ***kısmi yöntem bildiriminde tanımlama***.</span><span class="sxs-lookup"><span data-stu-id="6e059-363">There are two kinds of partial method declarations: If the body of the method declaration is a semicolon, the declaration is said to be a ***defining partial method declaration***.</span></span> <span data-ttu-id="6e059-364">Gövde olarak belirtilen bir *blok*, bildirimi olarak kabul edilir bir ***kısmi yöntem bildiriminin yürüten***.</span><span class="sxs-lookup"><span data-stu-id="6e059-364">If the body is given as a *block*, the declaration is said to be an ***implementing partial method declaration***.</span></span> <span data-ttu-id="6e059-365">Bir tür bildirimi bölümleri arasında yalnızca bir olabilir kısmi yöntem bildiriminde verilen imzayla tanımlama ve yalnızca bir kısmi yöntem bildiriminde verilen imzayla uygulama olabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-365">Across the parts of a type declaration there can be only one defining partial method declaration with a given signature, and there can be only one implementing partial method declaration with a given signature.</span></span> <span data-ttu-id="6e059-366">Uygulama kısmi yöntem bildirimi verilir, kısmi yöntem bildiriminde tanımlama karşılık gelen bulunmalı ve bildirimleri olarak eşleşmelidir aşağıdaki konumlarda arar:</span><span class="sxs-lookup"><span data-stu-id="6e059-366">If an implementing partial method declaration is given, a corresponding defining partial method declaration must exist, and the declarations must match as specified in the following:</span></span>

* <span data-ttu-id="6e059-367">(Mutlaka aynı sırada rağmen) bildirimleri aynı değiştiricilere sahip olmalıdır, yöntem adı, türü parametre sayısı ve parametre sayısı.</span><span class="sxs-lookup"><span data-stu-id="6e059-367">The declarations must have the same modifiers (although not necessarily in the same order), method name, number of type parameters and number of parameters.</span></span>
* <span data-ttu-id="6e059-368">Parametrelerini ilgili bildirimleri aynı değiştiricilere (mutlaka aynı sırada rağmen) olmalıdır ve aynı türleri (modül tür parametresi adlarına farklılıkları).</span><span class="sxs-lookup"><span data-stu-id="6e059-368">Corresponding parameters in the declarations must have the same modifiers (although not necessarily in the same order) and the same types (modulo differences in type parameter names).</span></span>
* <span data-ttu-id="6e059-369">Karşılık gelen tür parametreleri bildirimlerinde aynı kısıtlamalara (modül tür parametresi adlarına farklılıkları) olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-369">Corresponding type parameters in the declarations must have the same constraints (modulo differences in type parameter names).</span></span>

<span data-ttu-id="6e059-370">Uygulama kısmi yöntem bildirimi aynı bölümünde karşılık gelen tanımlama kısmi yöntem bildirimi olarak görünebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-370">An implementing partial method declaration can appear in the same part as the corresponding defining partial method declaration.</span></span>

<span data-ttu-id="6e059-371">Yalnızca tanımlayan kısmi bir yöntemin aşırı yükleme çözümlemesinde rol oynar.</span><span class="sxs-lookup"><span data-stu-id="6e059-371">Only a defining partial method participates in overload resolution.</span></span> <span data-ttu-id="6e059-372">Bu nedenle, uygulama bildirimi verilen olsun veya olmasın, çağrı ifadeleri kısmi yöntem çağrılarına çözülebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-372">Thus, whether or not an implementing declaration is given, invocation expressions may resolve to invocations of the partial method.</span></span> <span data-ttu-id="6e059-373">Kısmi bir yöntem her zaman döndürüldüğünden `void`, tür çağrı ifadeleri ifade deyimleri her zaman olur.</span><span class="sxs-lookup"><span data-stu-id="6e059-373">Because a partial method always returns `void`, such invocation expressions will always be expression statements.</span></span> <span data-ttu-id="6e059-374">Ayrıca, kısmi bir yöntemin örtük olarak olduğundan `private`, bu tür deyimler her zaman içinde kısmi yöntem bildirimi türü bildirimin parçalardan biri içinde gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="6e059-374">Furthermore, because a partial method is implicitly `private`, such statements will always occur within one of the parts of the type declaration within which the partial method is declared.</span></span>

<span data-ttu-id="6e059-375">Kısmi türü bildirimi hiçbir bölümü, belirli bir kısmi yönteminin uygulama bildirimi içeriyorsa, onu çağırır herhangi bir ifade deyimi yalnızca Birleşik türü bildirimden kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="6e059-375">If no part of a partial type declaration contains an implementing declaration for a given partial method, any expression statement invoking it is simply removed from the combined type declaration.</span></span> <span data-ttu-id="6e059-376">Bu nedenle çağrısı ifadesi, bağlı tüm ifadeleri de dahil olmak üzere çalışma zamanında bir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="6e059-376">Thus the invocation expression, including any constituent expressions, has no effect at run-time.</span></span> <span data-ttu-id="6e059-377">Kısmi yöntem da kaldırılır ve birleşik tür bildirimi üyesi olur.</span><span class="sxs-lookup"><span data-stu-id="6e059-377">The partial method itself is also removed and will not be a member of the combined type declaration.</span></span>

<span data-ttu-id="6e059-378">Uygulama bildirimi için belirtilen bir kısmi yöntem yoksa, kısmi yöntem çağrılarını korunur.</span><span class="sxs-lookup"><span data-stu-id="6e059-378">If an implementing declaration exist for a given partial method, the invocations of the partial methods are retained.</span></span> <span data-ttu-id="6e059-379">Kısmi yöntem artış olduğunda uygulama kısmi yöntem bildirimi aşağıdaki istisnalar dışında benzer bir yöntem bildiriminde sağlar:</span><span class="sxs-lookup"><span data-stu-id="6e059-379">The partial method gives rise to a method declaration similar to the implementing partial method declaration except for the following:</span></span>

* <span data-ttu-id="6e059-380">`partial` Değiştiricisi dahil değildir</span><span class="sxs-lookup"><span data-stu-id="6e059-380">The `partial` modifier is not included</span></span>
* <span data-ttu-id="6e059-381">Sonuçta elde edilen yöntem bildiriminde öznitelikleri birleştirilmiş tanımlama ve uygulama kısmi yöntem bildiriminde belirtilmemiş sırayla öznitelikleridir.</span><span class="sxs-lookup"><span data-stu-id="6e059-381">The attributes in the resulting method declaration are the combined attributes of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="6e059-382">Yinelenenler kaldırılamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-382">Duplicates are not removed.</span></span>
* <span data-ttu-id="6e059-383">Sonuçta elde edilen yöntem bildiriminde parametrelerinin özniteliklerinde birleştirilmiş tanımlama ve uygulama kısmi yöntem bildiriminde ilgili parametreleri belirtilmemiş sırayla öznitelikleridir.</span><span class="sxs-lookup"><span data-stu-id="6e059-383">The attributes on the parameters of the resulting method declaration are the combined attributes of the corresponding parameters of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="6e059-384">Yinelenenler kaldırılamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-384">Duplicates are not removed.</span></span>

<span data-ttu-id="6e059-385">Bir tanımlama bildirimi ancak bir uygulama bildirimi verilmiştir, kısmi bir yöntem için M aşağıdaki kısıtlamalar uygulanır:</span><span class="sxs-lookup"><span data-stu-id="6e059-385">If a defining declaration but not an implementing declaration is given for a partial method M, the following restrictions apply:</span></span>

* <span data-ttu-id="6e059-386">Yöntemi temsilci oluşturmak için bir derleme zamanı hata ([temsilci oluşturma ifadeleri](expressions.md#delegate-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="6e059-386">It is a compile-time error to create a delegate to method ([Delegate creation expressions](expressions.md#delegate-creation-expressions)).</span></span>
* <span data-ttu-id="6e059-387">Başvurmak için bir derleme zamanı hata `M` bir ifade ağacı türüne dönüştürülmüş anonim bir işlevin içindeki ([ifade ağacı türlerine dönüştürme işlemi anonim İşlev değerlendirmesi](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="6e059-387">It is a compile-time error to refer to `M` inside an anonymous function that is converted to an expression tree type ([Evaluation of anonymous function conversions to expression tree types](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span></span>
* <span data-ttu-id="6e059-388">Çağrısından bir parçası olarak gerçekleşen ifadeleri `M` belirli atama onayına durumunu etkilemez ([belirli atama onayına](variables.md#definite-assignment)), bu potansiyel olarak yol açabilir. derleme zamanı hatalarına.</span><span class="sxs-lookup"><span data-stu-id="6e059-388">Expressions occurring as part of an invocation of `M` do not affect the definite assignment state ([Definite assignment](variables.md#definite-assignment)), which can potentially lead to compile-time errors.</span></span>
* <span data-ttu-id="6e059-389">`M` bir uygulama için giriş noktası olamaz ([uygulama başlatma](basic-concepts.md#application-startup)).</span><span class="sxs-lookup"><span data-stu-id="6e059-389">`M` cannot be the entry point for an application ([Application Startup](basic-concepts.md#application-startup)).</span></span>

<span data-ttu-id="6e059-390">Kısmi yöntemler sağlayan bir araç tarafından oluşturulan bir örneğin, başka bir bölümü davranışını özelleştirmek için bir tür bildiriminde kullanıldığında bir parçası için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-390">Partial methods are useful for allowing one part of a type declaration to customize the behavior of another part, e.g., one that is generated by a tool.</span></span> <span data-ttu-id="6e059-391">Aşağıdaki kısmi sınıf bildirimi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="6e059-391">Consider the following partial class declaration:</span></span>
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

<span data-ttu-id="6e059-392">Bu sınıf diğer tüm bölümler derlenir tanımlayan kısmi yöntem bildiriminin ve bunların çağrılarını kaldırılacak ve sonuçta elde edilen birleştirilmiş sınıf bildiriminin aşağıdaki eşdeğer olacaktır:</span><span class="sxs-lookup"><span data-stu-id="6e059-392">If this class is compiled without any other parts, the defining partial method declarations and their invocations will be removed, and the resulting combined class declaration will be equivalent to the following:</span></span>
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

<span data-ttu-id="6e059-393">Başka bir bölümü, ancak uygulama bildirimi olan kısmi yöntemler sağlayan verilen varsayın:</span><span class="sxs-lookup"><span data-stu-id="6e059-393">Assume that another part is given, however, which provides implementing declarations of the partial methods:</span></span>
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

<span data-ttu-id="6e059-394">Sonuçta elde edilen birleştirilmiş sınıf bildiriminin ardından aşağıdaki denk olacaktır:</span><span class="sxs-lookup"><span data-stu-id="6e059-394">Then the resulting combined class declaration will be equivalent to the following:</span></span>
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

### <a name="name-binding"></a><span data-ttu-id="6e059-395">Bağlama adı</span><span class="sxs-lookup"><span data-stu-id="6e059-395">Name binding</span></span>

<span data-ttu-id="6e059-396">Her bir parçasının genişletilebilir bir tür aynı ad alanı içinde bildirilmelidir olsa da, parça genellikle farklı bir ad alanı bildirimi içinde yazılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-396">Although each part of an extensible type must be declared within the same namespace, the parts are typically written within different namespace declarations.</span></span> <span data-ttu-id="6e059-397">Bu nedenle, farklı `using` yönergeleri ([yönergeleri kullanarak](namespaces.md#using-directives)) için her bir bölümü bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-397">Thus, different `using` directives ([Using directives](namespaces.md#using-directives)) may be present for each part.</span></span> <span data-ttu-id="6e059-398">Basit adları yorumlarken ([anlam çıkarma](expressions.md#type-inference)) bir bölümü, yalnızca içinde `using` yönergeleri bu bölümü kapsayan ad alanı bildirimlerinden biri olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-398">When interpreting simple names ([Type inference](expressions.md#type-inference)) within one part, only the `using` directives of the namespace declaration(s) enclosing that part are considered.</span></span> <span data-ttu-id="6e059-399">Bu, farklı bölümleri farklı anlamlara sahip aynı tanımlayıcıda neden olabilir:</span><span class="sxs-lookup"><span data-stu-id="6e059-399">This may result in the same identifier having different meanings in different parts:</span></span>
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

## <a name="class-members"></a><span data-ttu-id="6e059-400">Sınıf üyeleri</span><span class="sxs-lookup"><span data-stu-id="6e059-400">Class members</span></span>

<span data-ttu-id="6e059-401">Bir sınıfın üyeleri tarafından tanıtılan üyeleri oluşur, *class_member_declaration*s ve üyelerini doğrudan temel sınıftan devralınan.</span><span class="sxs-lookup"><span data-stu-id="6e059-401">The members of a class consist of the members introduced by its *class_member_declaration*s and the members inherited from the direct base class.</span></span>

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

<span data-ttu-id="6e059-402">Bir sınıf türünün üyeleri aşağıdaki kategorilere ayrılır:</span><span class="sxs-lookup"><span data-stu-id="6e059-402">The members of a class type are divided into the following categories:</span></span>

*  <span data-ttu-id="6e059-403">Sınıf ile ilişkili sabit değerleri temsil eden sabitleri ([sabitleri](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="6e059-403">Constants, which represent constant values associated with the class ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="6e059-404">Sınıf değişkenler alanları ([alanları](classes.md#fields)).</span><span class="sxs-lookup"><span data-stu-id="6e059-404">Fields, which are the variables of the class ([Fields](classes.md#fields)).</span></span>
*  <span data-ttu-id="6e059-405">Sınıfı tarafından gerçekleştirilen eylemler ve hesaplamalar uygulayan yöntemler ([yöntemleri](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="6e059-405">Methods, which implement the computations and actions that can be performed by the class ([Methods](classes.md#methods)).</span></span>
*  <span data-ttu-id="6e059-406">Tanımlama özellikleri adlı özellik ve okuma ve bu özellikleri yazma ile ilişkili eylem ([özellikleri](classes.md#properties)).</span><span class="sxs-lookup"><span data-stu-id="6e059-406">Properties, which define named characteristics and the actions associated with reading and writing those characteristics ([Properties](classes.md#properties)).</span></span>
*  <span data-ttu-id="6e059-407">Sınıfı tarafından oluşturulan bildirimleri tanımlayan olayları ([olayları](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="6e059-407">Events, which define notifications that can be generated by the class ([Events](classes.md#events)).</span></span>
*  <span data-ttu-id="6e059-408">Aynı şekilde sıralanması sınıfının örneklerini izin dizin oluşturucular (sözdizimi kurallarına göre) olarak diziler ([dizin oluşturucular](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="6e059-408">Indexers, which permit instances of the class to be indexed in the same way (syntactically) as arrays ([Indexers](classes.md#indexers)).</span></span>
*  <span data-ttu-id="6e059-409">İşleçler, sınıf örnekleri için uygulanabilir ifade operatörlerini tanımlayın ([işleçleri](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="6e059-409">Operators, which define the expression operators that can be applied to instances of the class ([Operators](classes.md#operators)).</span></span>
*  <span data-ttu-id="6e059-410">Örnek sınıf örnekleri başlatmak için gerekli eylemleri uygulayan oluşturucular ([örnek oluşturucular](classes.md#instance-constructors))</span><span class="sxs-lookup"><span data-stu-id="6e059-410">Instance constructors, which implement the actions required to initialize instances of the class ([Instance constructors](classes.md#instance-constructors))</span></span>
*  <span data-ttu-id="6e059-411">Sınıf örneğini kalıcı olarak atılmadan önce gerçekleştirilecek eylemleri uygulayan Yıkıcılar ([yok ediciler](classes.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="6e059-411">Destructors, which implement the actions to be performed before instances of the class are permanently discarded ([Destructors](classes.md#destructors)).</span></span>
*  <span data-ttu-id="6e059-412">Sınıfını başlatmak için gerekli eylemleri uygulayan statik oluşturucular ([statik oluşturucular](classes.md#static-constructors)).</span><span class="sxs-lookup"><span data-stu-id="6e059-412">Static constructors, which implement the actions required to initialize the class itself ([Static constructors](classes.md#static-constructors)).</span></span>
*  <span data-ttu-id="6e059-413">Sınıf için yerel türleri temsil eden türleri ([türlerin](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="6e059-413">Types, which represent the types that are local to the class ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="6e059-414">Yürütülebilir kod içerebilecek üyeleri olarak bilinir topluca *işlev üyeleri* sınıf türü.</span><span class="sxs-lookup"><span data-stu-id="6e059-414">Members that can contain executable code are collectively known as the *function members* of the class type.</span></span> <span data-ttu-id="6e059-415">Bir sınıf türünün işlev üyeleri, yöntemleri, özellikleri, olayları, Dizinleyicileri, işleçleri, örnek oluşturucuları, yok ediciler ve bu sınıf türünün statik oluşturucular ' dir.</span><span class="sxs-lookup"><span data-stu-id="6e059-415">The function members of a class type are the methods, properties, events, indexers, operators, instance constructors,  destructors, and static constructors of that class type.</span></span>

<span data-ttu-id="6e059-416">A *class_declaration* yeni bir bildirim alanı oluşturur ([bildirimleri](basic-concepts.md#declarations)) ve *class_member_declaration*hemen içerdiği s *sınıfı _declaration* bu bildirim alanına yeni üye tanıtır.</span><span class="sxs-lookup"><span data-stu-id="6e059-416">A *class_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *class_member_declaration*s immediately contained by the *class_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="6e059-417">Aşağıdaki kurallar geçerli *class_member_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="6e059-417">The following rules apply to *class_member_declaration*s:</span></span>

*  <span data-ttu-id="6e059-418">Örnek oluşturucuları ve yıkıcıları statik oluşturucular kapsayan sınıf adıyla aynı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-418">Instance constructors, destructors and static constructors must have the same name as the immediately enclosing class.</span></span> <span data-ttu-id="6e059-419">Diğer tüm üyeleri hemen kapsayan sınıfın adından farklı adlara sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-419">All other members must have names that differ from the name of the immediately enclosing class.</span></span>
*  <span data-ttu-id="6e059-420">Bir sabit, alan, özelliği, olay veya türü adı aynı sınıfta bildirilen tüm diğer üyelerinin adları farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-420">The name of a constant, field, property, event, or type must differ from the names of all other members declared in the same class.</span></span>
*  <span data-ttu-id="6e059-421">Bir yöntemin adını, diğer tüm olmayan-aynı sınıfta bildirilen yöntemlerin adlarından farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-421">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="6e059-422">Ayrıca, imza ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)), bir yöntem imzaları aynı sınıfta bildirilen tüm diğer yöntemlerden farklı olmalıdır ve yalnızca farklı imzaları aynı sınıfta bildirilen iki yöntem olamaz tarafından `ref` ve `out`.</span><span class="sxs-lookup"><span data-stu-id="6e059-422">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="6e059-423">Bir örnek oluşturucusunda imzası aynı sınıfta bildirilen tüm diğer örnek oluşturucuları imzalarını farklı olmalıdır ve sadece onun tarafından farklı imzaları aynı sınıfta bildirilen iki Oluşturucu olmayabilir `ref` ve `out`.</span><span class="sxs-lookup"><span data-stu-id="6e059-423">The signature of an instance constructor must differ from the signatures of all other instance constructors declared in the same class, and two constructors declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="6e059-424">Bir dizin oluşturucu imzasının aynı sınıfta bildirilen tüm diğer dizin imzalarını farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-424">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>
*  <span data-ttu-id="6e059-425">Bir işleç imzası, imzaları aynı sınıfta bildirilen tüm diğer işleçlerin kadar farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-425">The signature of an operator must differ from the signatures of all other operators declared in the same class.</span></span>

<span data-ttu-id="6e059-426">Devralınan bir sınıf türü üyeleri ([devralma](classes.md#inheritance)) bir sınıf bildirimi alanının parçası değildir.</span><span class="sxs-lookup"><span data-stu-id="6e059-426">The inherited members of a class type ([Inheritance](classes.md#inheritance)) are not part of the declaration space of a class.</span></span> <span data-ttu-id="6e059-427">Bu nedenle, türetilmiş bir sınıf olarak (Bu aslında devralınan üyeyi gizler) devralınmış bir üyeyi aynı ad veya imzaya sahip bir üye bildirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-427">Thus, a derived class is allowed to declare a member with the same name or signature as an inherited member (which in effect hides the inherited member).</span></span>

### <a name="the-instance-type"></a><span data-ttu-id="6e059-428">Örnek türü</span><span class="sxs-lookup"><span data-stu-id="6e059-428">The instance type</span></span>

<span data-ttu-id="6e059-429">Her sınıf bildirimi ilişkili ilişkili bir türü vardır ([bağlı ve türleri ilişkisiz](types.md#bound-and-unbound-types)), ***örnek türünü***.</span><span class="sxs-lookup"><span data-stu-id="6e059-429">Each class declaration has an associated bound type ([Bound and unbound types](types.md#bound-and-unbound-types)), the ***instance type***.</span></span> <span data-ttu-id="6e059-430">Genel sınıf bildirimi, örnek türüne oluşturulan bir tür oluşturarak biçimlendirilmiş ([oluşturulan türler](types.md#constructed-types)) türü bildirimden sağlanan tür her biriyle ilgili olan bağımsız değişkenleri tür parametresi.</span><span class="sxs-lookup"><span data-stu-id="6e059-430">For a generic class declaration, the instance type is formed by creating a constructed type ([Constructed types](types.md#constructed-types)) from the type declaration, with each of the supplied type arguments being the corresponding type parameter.</span></span> <span data-ttu-id="6e059-431">Örnek türü tür parametreleri kullandığından, yalnızca tür parametreleri kapsamda nerede kullanılabilir; diğer bir deyişle, sınıf bildirimi içinde.</span><span class="sxs-lookup"><span data-stu-id="6e059-431">Since the instance type uses the type parameters, it can only be used where the type parameters are in scope; that is, inside the class declaration.</span></span> <span data-ttu-id="6e059-432">Örnek türü türüdür `this` sınıf bildirimi içinde yazılan kod için.</span><span class="sxs-lookup"><span data-stu-id="6e059-432">The instance type is the type of `this` for code written inside the class declaration.</span></span> <span data-ttu-id="6e059-433">Genel olmayan sınıflar için örneği yalnızca bildirilen sınıf türüdür.</span><span class="sxs-lookup"><span data-stu-id="6e059-433">For non-generic classes, the instance type is simply the declared class.</span></span> <span data-ttu-id="6e059-434">Aşağıdaki örnek türleri ile birlikte birkaç sınıf bildirimi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="6e059-434">The following shows several class declarations along with their instance types:</span></span> 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a><span data-ttu-id="6e059-435">Oluşturulan türler üyeleri</span><span class="sxs-lookup"><span data-stu-id="6e059-435">Members of constructed types</span></span>

<span data-ttu-id="6e059-436">Oluşturulan tür olmayan devralınan üyeleri, her biri için değiştirerek elde edilen *type_parameter* karşılık gelen üye bildiriminde *type_argument* oluşturulan türü.</span><span class="sxs-lookup"><span data-stu-id="6e059-436">The non-inherited members of a constructed type are obtained by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="6e059-437">Değiştirme işlemi anlam tür bildirimleri üzerinde temel alır ve yalnızca metin değiştirme değil.</span><span class="sxs-lookup"><span data-stu-id="6e059-437">The substitution process is based on the semantic meaning of type declarations, and is not simply textual substitution.</span></span>

<span data-ttu-id="6e059-438">Örneğin, genel bir sınıf bildiriminde belirtilen</span><span class="sxs-lookup"><span data-stu-id="6e059-438">For example, given the generic class declaration</span></span>
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
<span data-ttu-id="6e059-439">oluşturulan tür `Gen<int[],IComparable<string>>` aşağıdaki üyeleri içerir:</span><span class="sxs-lookup"><span data-stu-id="6e059-439">the constructed type `Gen<int[],IComparable<string>>` has the following members:</span></span>
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

<span data-ttu-id="6e059-440">Üyenin türü `a` genel sınıf bildiriminde `Gen` olan "iki boyutlu dizi `T`", bu nedenle üyenin türü `a` oluşturulmuş yukarıdaki "iki boyutlu dizi tek boyutlu dizi türüdür`int`", veya `int[,][]`.</span><span class="sxs-lookup"><span data-stu-id="6e059-440">The type of the member `a` in the generic class declaration `Gen` is "two-dimensional array of `T`", so the type of the member `a` in the constructed type above is "two-dimensional array of one-dimensional array of `int`", or `int[,][]`.</span></span>

<span data-ttu-id="6e059-441">Örnek işlev üyeleri, tür içinde `this` örneği türü ([örnek türü](classes.md#the-instance-type)) içeren bildirimi.</span><span class="sxs-lookup"><span data-stu-id="6e059-441">Within instance function members, the type of `this` is the instance type ([The instance type](classes.md#the-instance-type)) of the containing declaration.</span></span>

<span data-ttu-id="6e059-442">Bir genel sınıfın tüm üyeleri, doğrudan veya oluşturulan tür bir parçası olarak herhangi bir kapsayan sınıf türü parametreleri kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6e059-442">All members of a generic class can use type parameters from any enclosing class, either directly or as part of a constructed type.</span></span> <span data-ttu-id="6e059-443">Belirli bir kapatıldığı türü oluşturulan ([açık ve kapalı türleri](types.md#open-and-closed-types)) kullanılan çalışma zamanında, her bir tür parametresi kullanımının oluşturulan tür için sağlanan gerçek tür bağımsız değişken ile değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-443">When a particular closed constructed type ([Open and closed types](types.md#open-and-closed-types)) is used at run-time, each use of a type parameter is replaced with the actual type argument supplied to the constructed type.</span></span> <span data-ttu-id="6e059-444">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6e059-444">For example:</span></span>
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

### <a name="inheritance"></a><span data-ttu-id="6e059-445">Devralma</span><span class="sxs-lookup"><span data-stu-id="6e059-445">Inheritance</span></span>

<span data-ttu-id="6e059-446">Bir sınıf ***devralan*** doğrudan temel sınıf türü üyeleri.</span><span class="sxs-lookup"><span data-stu-id="6e059-446">A class ***inherits*** the members of its direct base class type.</span></span> <span data-ttu-id="6e059-447">Devralma, bir sınıf örtük olarak örnek oluşturucuları ve yıkıcıları temel sınıfın statik oluşturucular dışında doğrudan temel sınıf türünün tüm üyelerini içeren anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="6e059-447">Inheritance means that a class implicitly contains all members of its direct base class type, except for the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="6e059-448">Devralma önemli bazı yönleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="6e059-448">Some important aspects of inheritance are:</span></span>

*  <span data-ttu-id="6e059-449">Devralma geçişlidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-449">Inheritance is transitive.</span></span> <span data-ttu-id="6e059-450">Varsa `C` türetilir `B`, ve `B` türetilir `A`, ardından `C` bildirilen üyelerini devralır `B` bildirilen üyeler yanı sıra `A`.</span><span class="sxs-lookup"><span data-stu-id="6e059-450">If `C` is derived from `B`, and `B` is derived from `A`, then `C` inherits the members declared in `B` as well as the members declared in `A`.</span></span>
*  <span data-ttu-id="6e059-451">Türetilmiş bir sınıf doğrudan temel sınıfı genişletir.</span><span class="sxs-lookup"><span data-stu-id="6e059-451">A derived class extends its direct base class.</span></span> <span data-ttu-id="6e059-452">Türetilmiş bir sınıf devralır bu yeni üyeler ekleyebilirsiniz, ancak devralınan bir üyeyi tanımını kaldırılamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-452">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span>
*  <span data-ttu-id="6e059-453">Örnek oluşturucuları ve yıkıcıları statik oluşturucular devralınmaz var, ancak diğer tüm üyeleri, bağımsız olarak kendi bildirilen erişilebilirliği ([üye erişimi](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="6e059-453">Instance constructors, destructors, and static constructors are not inherited, but all other members are, regardless of their declared accessibility ([Member access](basic-concepts.md#member-access)).</span></span> <span data-ttu-id="6e059-454">Ancak, kendi bildirilen erişilebilirliği bağlı olarak, devralınan üyeler türetilen bir sınıfta erişilebilir olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-454">However, depending on their declared accessibility, inherited members might not be accessible in a derived class.</span></span>
*  <span data-ttu-id="6e059-455">Türetilmiş sınıf ***Gizle*** ([devralma yoluyla gizleme](basic-concepts.md#hiding-through-inheritance)) aynı ad veya imzaya sahip yeni üyeler bildirerek devralınan üyeleri.</span><span class="sxs-lookup"><span data-stu-id="6e059-455">A derived class can ***hide*** ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)) inherited members by declaring new members with the same name or signature.</span></span> <span data-ttu-id="6e059-456">Devralınmış bir üyeyi gizlemek, ancak bu üyede kaldırmaz unutmayın; yalnızca o üyesi türetilmiş sınıf üzerinden doğrudan erişilemez olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="6e059-456">Note however that hiding an inherited member does not remove that member—it merely makes that member inaccessible directly through the derived class.</span></span>
*  <span data-ttu-id="6e059-457">Tüm örnek alanları sınıfı ve temel sınıflarının ve örtük olarak bildirilen bir dizi bir sınıf örneğini içerir ([örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions)) bir türetilmiş sınıf türünden temel sınıf türlerinden birinin bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="6e059-457">An instance of a class contains a set of all instance fields declared in the class and its base classes, and an implicit conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from a derived class type to any of its base class types.</span></span> <span data-ttu-id="6e059-458">Bu nedenle, bazı türetilmiş sınıfın bir örneğine başvuru temel sınıflarını herhangi bir örneğe bir başvuru olarak işlenebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-458">Thus, a reference to an instance of some derived class can be treated as a reference to an instance of any of its base classes.</span></span>
*  <span data-ttu-id="6e059-459">Bir sınıf sanal yöntemler, özellikler ve dizin oluşturucular bildirebilir ve türetilen sınıflar bu işlev üyeler uygulamasını geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-459">A class can declare virtual methods, properties, and indexers, and derived classes can override the implementation of these function members.</span></span> <span data-ttu-id="6e059-460">Bu müşteriyle üzerinden işlevi üyenin çağrıldığı örnek çalışma zamanı türüne bağlı olarak değişir işlevi üye çağrısı tarafından gerçekleştirilen eylemlerin polimorfik davranışlar sergilemesine izin sınıflar sağlar.</span><span class="sxs-lookup"><span data-stu-id="6e059-460">This enables classes to exhibit polymorphic behavior wherein the actions performed by a function member invocation varies depending on the run-time type of the instance through which that function member is invoked.</span></span>

<span data-ttu-id="6e059-461">Oluşturulan sınıf türü devralınan üye olan hemen temel sınıf türünün üyeleri ([temel sınıflar](classes.md#base-classes)), hangi oluşturulan tür tür bağımsız değişkenleri karşılık gelen türü her geçtiği için değiştirerek bulunur parametrelerinde *class_base* belirtimi.</span><span class="sxs-lookup"><span data-stu-id="6e059-461">The inherited member of a constructed class type are the members of the immediate base class type ([Base classes](classes.md#base-classes)), which is found by substituting the type arguments of the constructed type for each occurrence of the corresponding type parameters in the *class_base* specification.</span></span> <span data-ttu-id="6e059-462">Bu üyeleri, buna karşılık, her biri için getirilmesiyle dönüştürülür *type_parameter* karşılık gelen üye bildiriminde *type_argument* , *class_base* belirtimi.</span><span class="sxs-lookup"><span data-stu-id="6e059-462">These members, in turn, are transformed by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the *class_base* specification.</span></span>

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

<span data-ttu-id="6e059-463">Yukarıdaki örnekte, yapılandırılmış türdedir `D<int>` olmayan devralınan bir üyenin `public int G(string s)` tür bağımsız değişkeni değiştirerek elde `int` tür parametresi için `T`.</span><span class="sxs-lookup"><span data-stu-id="6e059-463">In the above example, the constructed type `D<int>` has a non-inherited member `public int G(string s)` obtained by substituting the type argument `int` for the type parameter `T`.</span></span> <span data-ttu-id="6e059-464">`D<int>` devralınan bir sınıf bildiriminde üye de sahip `B`.</span><span class="sxs-lookup"><span data-stu-id="6e059-464">`D<int>` also has an inherited member from the class declaration `B`.</span></span> <span data-ttu-id="6e059-465">Bu devralınan üye, temel sınıf türünü belirleyerek belirlenir `B<int[]>` , `D<int>` değiştirerek tarafından `int` için `T` temel sınıf belirtiminde `B<T[]>`.</span><span class="sxs-lookup"><span data-stu-id="6e059-465">This inherited member is determined by first determining the base class type `B<int[]>` of `D<int>` by substituting `int` for `T` in the base class specification `B<T[]>`.</span></span> <span data-ttu-id="6e059-466">Ardından, bir tür bağımsız değişkeni olarak `B`, `int[]` için yerine `U` içinde `public U F(long index)`, devralınan üye sonuçlanmıyor `public int[] F(long index)`.</span><span class="sxs-lookup"><span data-stu-id="6e059-466">Then, as a type argument to `B`, `int[]` is substituted for `U` in `public U F(long index)`, yielding the inherited member `public int[] F(long index)`.</span></span>

### <a name="the-new-modifier"></a><span data-ttu-id="6e059-467">New değiştiricisi</span><span class="sxs-lookup"><span data-stu-id="6e059-467">The new modifier</span></span>

<span data-ttu-id="6e059-468">A *class_member_declaration* devralınmış bir üyeyi olarak aynı ad veya imzaya sahip bir üye bildirmek için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-468">A *class_member_declaration* is permitted to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="6e059-469">Bu durumda, türetilmiş sınıf üyesi edilir ***Gizle*** temel sınıf üyesi.</span><span class="sxs-lookup"><span data-stu-id="6e059-469">When this occurs, the derived class member is said to ***hide*** the base class member.</span></span> <span data-ttu-id="6e059-470">Devralınmış bir üyeyi gizlemek hata olarak kabul edilmez, ancak derleyici bir uyarı neden olmaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-470">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="6e059-471">Bu uyarının gösterilmemesi için türetilmiş sınıf üyesinin bildirimi içerebilir bir `new` türetilen üye temel üyeyi Gizle amaçlandığını belirtmek için değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-471">To suppress the warning, the declaration of the derived class member can include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="6e059-472">Bu konunun ele alındığı daha ayrıntılı olarak [devralma yoluyla gizleme](basic-concepts.md#hiding-through-inheritance).</span><span class="sxs-lookup"><span data-stu-id="6e059-472">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="6e059-473">Varsa bir `new` değiştiricisi devralınan bir üyeyi gizlemek olmayan bir bildiriminde dahil, bir uyarı belirlememişse bu bağlamda verilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-473">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning to that effect is issued.</span></span> <span data-ttu-id="6e059-474">Kaldırarak bu uyarı bastırılır `new` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-474">This warning is suppressed by removing the `new` modifier.</span></span>

### <a name="access-modifiers"></a><span data-ttu-id="6e059-475">Erişim değiştiricileri</span><span class="sxs-lookup"><span data-stu-id="6e059-475">Access modifiers</span></span>

<span data-ttu-id="6e059-476">A *class_member_declaration* bildirilen erişilebilirliği beş olası tür herhangi biri olabilir ([erişilebilirlik bildirilen](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal` , veya `private`.</span><span class="sxs-lookup"><span data-stu-id="6e059-476">A *class_member_declaration* can have any one of the five possible kinds of declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`, or `private`.</span></span> <span data-ttu-id="6e059-477">Dışında `protected internal` birleşimi, birden fazla erişim değiştiricisi belirtmek için bir derleme zamanı hatası olduğundan.</span><span class="sxs-lookup"><span data-stu-id="6e059-477">Except for the `protected internal` combination, it is a compile-time error to specify more than one access modifier.</span></span> <span data-ttu-id="6e059-478">Olduğunda bir *class_member_declaration* herhangi bir erişim değiştiricileri içermez `private` varsayılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-478">When a *class_member_declaration* does not include any access modifiers, `private` is assumed.</span></span>

### <a name="constituent-types"></a><span data-ttu-id="6e059-479">Bağlı türleri</span><span class="sxs-lookup"><span data-stu-id="6e059-479">Constituent types</span></span>

<span data-ttu-id="6e059-480">Bu üye bağlı tür üyesi bildiriminde kullanılan türler çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-480">Types that are used in the declaration of a member are called the constituent types of that member.</span></span> <span data-ttu-id="6e059-481">Olası bağlı bir sabit, alan, özelliği, olay veya dizin oluşturucu türü, bir yöntem veya işlecin dönüş türü ve parametre türleri, yöntem, dizin oluşturucu, işleci veya örnek oluşturucusu türleridir.</span><span class="sxs-lookup"><span data-stu-id="6e059-481">Possible constituent types are the type of a constant, field, property, event, or indexer, the return type of a method or operator, and the parameter types of a method, indexer, operator, or instance constructor.</span></span> <span data-ttu-id="6e059-482">Bağlı üye türleri en az bu üye olarak erişilebilir olmalıdır ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6e059-482">The constituent types of a member must be at least as accessible as that member itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

### <a name="static-and-instance-members"></a><span data-ttu-id="6e059-483">Statik ve örnek üyeleri</span><span class="sxs-lookup"><span data-stu-id="6e059-483">Static and instance members</span></span>

<span data-ttu-id="6e059-484">Bir sınıf üyesi olan ya da ***statik üyeleri*** veya ***örnek üyeleri***.</span><span class="sxs-lookup"><span data-stu-id="6e059-484">Members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="6e059-485">Genel olarak bakıldığında, statik üyeler sınıf türleri ve (sınıf türlerinin örneklerini) nesnelere ait örnek üyeleri ait olarak düşünmek kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-485">Generally speaking, it is useful to think of static members as belonging to class types and instance members as belonging to objects (instances of class types).</span></span>

<span data-ttu-id="6e059-486">Zaman alan, yöntem, özellik, olay, işleci veya Oluşturucu bildirimi içeren bir `static` değiştiricisi, statik bir üye bildirir.</span><span class="sxs-lookup"><span data-stu-id="6e059-486">When a field, method, property, event, operator, or constructor declaration includes a `static` modifier, it declares a static member.</span></span> <span data-ttu-id="6e059-487">Ayrıca, bir sabit değer veya tür bildirimi statik üyesi örtük olarak bildiriyor.</span><span class="sxs-lookup"><span data-stu-id="6e059-487">In addition, a constant or type declaration implicitly declares a static member.</span></span> <span data-ttu-id="6e059-488">Statik üyeleri aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="6e059-488">Static members have the following characteristics:</span></span>

*  <span data-ttu-id="6e059-489">Statik üye `M` başvurulduğundan bir *member_access* ([üye erişimi](expressions.md#member-access)) biçiminde `E.M`, `E` içeren türü belirtmek `M`.</span><span class="sxs-lookup"><span data-stu-id="6e059-489">When a static member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote a type containing `M`.</span></span> <span data-ttu-id="6e059-490">Bir derleme zamanı hata için `E` örneğini belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="6e059-490">It is a compile-time error for `E` to denote an instance.</span></span>
*  <span data-ttu-id="6e059-491">Statik alan belirli bir kapalı sınıf türünün tüm örnekleri tarafından paylaşılan tek bir depolama konumu belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-491">A static field identifies exactly one storage location to be shared by all instances of a given closed class type.</span></span> <span data-ttu-id="6e059-492">Belirli bir kapalı sınıf türü kaç örneklerin oluşturulduğu ne olursa olsun, yalnızca bir kopyasını statik bir alan yoktur.</span><span class="sxs-lookup"><span data-stu-id="6e059-492">No matter how many instances of a given closed class type are created, there is only ever one copy of a static field.</span></span>
*  <span data-ttu-id="6e059-493">Statik işlev üyesi (yöntem, özellik, olay, işleci veya Oluşturucu) belirli bir örneği üzerinde çalışmaz ve başvurmak için bir derleme zamanı hata `this` işlevi gibi bir üyesi olarak.</span><span class="sxs-lookup"><span data-stu-id="6e059-493">A static function member (method, property, event, operator, or constructor) does not operate on a specific instance, and it is a compile-time error to refer to `this` in such a function member.</span></span>

<span data-ttu-id="6e059-494">Zaman alan, yöntemi, özelliği, olay, dizin oluşturucu, oluşturucu veya yıkıcı bildirimi içermez bir `static` değiştiricisi, bir örnek üye bildirir.</span><span class="sxs-lookup"><span data-stu-id="6e059-494">When a field, method, property, event, indexer, constructor, or destructor declaration does not include a `static` modifier, it declares an instance member.</span></span> <span data-ttu-id="6e059-495">(Bir örnek üye statik olmayan üye adlandırılır.) Örnek üyeleri şu özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="6e059-495">(An instance member is sometimes called a non-static member.) Instance members have the following characteristics:</span></span>

*  <span data-ttu-id="6e059-496">Bir örnek üyesi olduğunda `M` başvurulduğundan bir *member_access* ([üye erişimi](expressions.md#member-access)) biçiminde `E.M`, `E` içerenbirtürünbirörneğinibelirtmekgerekir`M`.</span><span class="sxs-lookup"><span data-stu-id="6e059-496">When an instance member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote an instance of a type containing `M`.</span></span> <span data-ttu-id="6e059-497">Bir bağlama-time hata için `E` bir tür belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="6e059-497">It is a binding-time error for `E` to denote a type.</span></span>
*  <span data-ttu-id="6e059-498">Bir sınıfın her örneği ayrı bir sınıfın tüm örnek alanları kümesi içerir.</span><span class="sxs-lookup"><span data-stu-id="6e059-498">Every instance of a class contains a separate set of all instance fields of the class.</span></span>
*  <span data-ttu-id="6e059-499">Örnek işlevi üyesine (yöntemi, özelliği, dizin oluşturucu, örnek oluşturucusu veya yıkıcısı) belirli bir sınıf örneği üzerinde çalışır ve bu örnek olarak erişilebilen `this` ([bu erişim](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="6e059-499">An instance function member (method, property, indexer, instance constructor, or destructor) operates on a given instance of the class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="6e059-500">Aşağıdaki örnek, statik erişmek için kuralları ve örnek üyeleri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="6e059-500">The following example illustrates the rules for accessing static and instance members:</span></span>
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

<span data-ttu-id="6e059-501">`F` Yöntemi gösterir, bir örnek işlevi üyesi içinde bir *simple_name* ([basit adları](expressions.md#simple-names)) örnek üyeleri hem statik üyeleri erişmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-501">The `F` method shows that in an instance function member, a *simple_name* ([Simple names](expressions.md#simple-names)) can be used to access both instance members and static members.</span></span> <span data-ttu-id="6e059-502">`G` Yöntemi gösterir statik işlev üyesi onu aracılığıyla örnek üyesine erişmek için bir derleme zamanı hatası olduğu bir *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="6e059-502">The `G` method shows that in a static function member, it is a compile-time error to access an instance member through a *simple_name*.</span></span> <span data-ttu-id="6e059-503">`Main` Yöntemi gösterir, bir *member_access* ([üye erişimi](expressions.md#member-access)) örnekleri örnek üyeleri erişilen ve türleri statik üye erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-503">The `Main` method shows that in a *member_access* ([Member access](expressions.md#member-access)), instance members must be accessed through instances, and static members must be accessed through types.</span></span>

### <a name="nested-types"></a><span data-ttu-id="6e059-504">İç içe geçmiş türler</span><span class="sxs-lookup"><span data-stu-id="6e059-504">Nested types</span></span>

<span data-ttu-id="6e059-505">Bir sınıf veya yapı bildirimi içinde bildirilen bir tür olarak adlandırılan bir ***iç içe türü***.</span><span class="sxs-lookup"><span data-stu-id="6e059-505">A type declared within a class or struct declaration is called a ***nested type***.</span></span> <span data-ttu-id="6e059-506">Bir derleme biriminde veya ad alanı içinde bildirilen bir tür olarak adlandırılan bir ***iç içe türü***.</span><span class="sxs-lookup"><span data-stu-id="6e059-506">A type that is declared within a compilation unit or namespace is called a ***non-nested type***.</span></span>

<span data-ttu-id="6e059-507">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-507">In the example</span></span>
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
<span data-ttu-id="6e059-508">sınıf `B` sınıfı içinde bildirildiği için iç içe geçmiş bir türdür `A`ve sınıfı `A` bir derleme birimi içinde bildirildiği için bir iç içe olmayan türüdür.</span><span class="sxs-lookup"><span data-stu-id="6e059-508">class `B` is a nested type because it is declared within class `A`, and class `A` is a non-nested type because it is declared within a compilation unit.</span></span>

#### <a name="fully-qualified-name"></a><span data-ttu-id="6e059-509">Tam adı</span><span class="sxs-lookup"><span data-stu-id="6e059-509">Fully qualified name</span></span>

<span data-ttu-id="6e059-510">Tam adı ([adları tam](basic-concepts.md#fully-qualified-names)) iç içe geçmiş bir tür için `S.N` burada `S` hangi türdeki türünün tam adı `N` bildirilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-510">The fully qualified name ([Fully qualified names](basic-concepts.md#fully-qualified-names)) for a nested type is `S.N` where `S` is the fully qualified name of the type in which type `N` is declared.</span></span>

#### <a name="declared-accessibility"></a><span data-ttu-id="6e059-511">Bildirilen erişilebilirliği</span><span class="sxs-lookup"><span data-stu-id="6e059-511">Declared accessibility</span></span>

<span data-ttu-id="6e059-512">İç içe olmayan türleri olabilir `public` veya `internal` bildirilen erişilebilirliği ve sahip `internal` erişilebilirlik varsayılan olarak bildirilmiş.</span><span class="sxs-lookup"><span data-stu-id="6e059-512">Non-nested types can have `public` or `internal` declared accessibility and have `internal` declared accessibility by default.</span></span> <span data-ttu-id="6e059-513">İç içe geçmiş türler biçimlerinden birini bildirilen erişilebilirliği çok, artı içeren türün bir sınıf veya yapı olup olmadığına bağlı olarak bildirilen erişilebilirliği, bir veya daha fazla başka biçimlerde olabilir:</span><span class="sxs-lookup"><span data-stu-id="6e059-513">Nested types can have these forms of declared accessibility too, plus one or more additional forms of declared accessibility, depending on whether the containing type is a class or struct:</span></span>

*  <span data-ttu-id="6e059-514">Bir sınıfta bildirilen iç içe türü bildirilen erişilebilirliği, beş biçimlerden birini olabilir (`public`, `protected internal`, `protected`, `internal`, veya `private`) ve diğer sınıf üyeleri gibi varsayılan olarak `private` bildirildi Erişilebilirlik.</span><span class="sxs-lookup"><span data-stu-id="6e059-514">A nested type that is declared in a class can have any of five forms of declared accessibility (`public`, `protected internal`, `protected`, `internal`, or `private`) and, like other class members, defaults to `private` declared accessibility.</span></span>
*  <span data-ttu-id="6e059-515">Bir yapı içinde bildirilen iç içe türü bildirilen erişilebilirliği, üç biçimlerden birini olabilir (`public`, `internal`, veya `private`) ve diğer yapı üyeleri gibi varsayılan olarak `private` erişilebilirlik bildirilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-515">A nested type that is declared in a struct can have any of three forms of declared accessibility (`public`, `internal`, or `private`) and, like other struct members, defaults to `private` declared accessibility.</span></span>

<span data-ttu-id="6e059-516">Örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-516">The example</span></span>
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
<span data-ttu-id="6e059-517">özel iç içe geçmiş bir sınıfı bildirir `Node`.</span><span class="sxs-lookup"><span data-stu-id="6e059-517">declares a private nested class `Node`.</span></span>

#### <a name="hiding"></a><span data-ttu-id="6e059-518">Gizleme</span><span class="sxs-lookup"><span data-stu-id="6e059-518">Hiding</span></span>

<span data-ttu-id="6e059-519">İç içe geçmiş bir tür gizle ([ad gizleme](basic-concepts.md#name-hiding)) temel üye.</span><span class="sxs-lookup"><span data-stu-id="6e059-519">A nested type may hide ([Name hiding](basic-concepts.md#name-hiding)) a base member.</span></span> <span data-ttu-id="6e059-520">`new` Değiştiricisi, iç içe geçmiş tür bildirimlerinde verilir, böylece gizleme açıkça ifade edilebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-520">The `new` modifier is permitted on nested type declarations so that hiding can be expressed explicitly.</span></span> <span data-ttu-id="6e059-521">Örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-521">The example</span></span>
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
<span data-ttu-id="6e059-522">iç içe geçmiş bir sınıfı göstermektedir `M` , yöntemini gizliyor `M` tanımlanan `Base`.</span><span class="sxs-lookup"><span data-stu-id="6e059-522">shows a nested class `M` that hides the method `M` defined in `Base`.</span></span>

#### <a name="this-access"></a><span data-ttu-id="6e059-523">Bu erişim</span><span class="sxs-lookup"><span data-stu-id="6e059-523">this access</span></span>

<span data-ttu-id="6e059-524">İç içe geçmiş bir tür ve kapsayan türü ile özel bir ilişkisi olmayan *this_access* ([bu erişim](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="6e059-524">A nested type and its containing type do not have a special relationship with regard to *this_access* ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="6e059-525">Özellikle, `this` kapsayan türdeki örneği üyelerine başvuru için bir iç içe geçmiş bir tür içinde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-525">Specifically, `this` within a nested type cannot be used to refer to instance members of the containing type.</span></span> <span data-ttu-id="6e059-526">İç içe türü, kapsayan türün örnek üyeleri için erişim gereken yere durumlarda sağlayarak erişim sağlanabilir `this` olarak iç içe geçmiş tür için oluşturucu bağımsız değişkeni bir kapsayan tür örneği için.</span><span class="sxs-lookup"><span data-stu-id="6e059-526">In cases where a nested type needs access to the instance members of its containing type, access can be provided by providing the `this` for the instance of the containing type as a constructor argument for the nested type.</span></span> <span data-ttu-id="6e059-527">Aşağıdaki örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-527">The following example</span></span>
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
<span data-ttu-id="6e059-528">Bu teknik gösterir.</span><span class="sxs-lookup"><span data-stu-id="6e059-528">shows this technique.</span></span> <span data-ttu-id="6e059-529">Örneği `C` örneği oluşturur `Nested` ve kendi geçirir `this` için `Nested`'s sonraki erişim sağlamak için oluşturucu `C`ait örnek üyeleri.</span><span class="sxs-lookup"><span data-stu-id="6e059-529">An instance of `C` creates an instance of `Nested` and passes its own `this` to `Nested`'s constructor in order to provide subsequent access to `C`'s instance members.</span></span>

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a><span data-ttu-id="6e059-530">Kapsayan türdeki özel ve korumalı üyelerine erişimi</span><span class="sxs-lookup"><span data-stu-id="6e059-530">Access to private and protected members of the containing type</span></span>

<span data-ttu-id="6e059-531">İç içe türü kapsayan tür sahip üyeleri dahil olmak üzere, kapsadığı tür için erişilebilir olan üyelerin tümüne erişebilir `private` ve `protected` erişilebilirlik bildirilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-531">A nested type has access to all of the members that are accessible to its containing type, including members of the containing type that have `private` and `protected` declared accessibility.</span></span> <span data-ttu-id="6e059-532">Örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-532">The example</span></span>
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
<span data-ttu-id="6e059-533">bir sınıfı göstermektedir `C` iç içe geçmiş bir sınıf içeren `Nested`.</span><span class="sxs-lookup"><span data-stu-id="6e059-533">shows a class `C` that contains a nested class `Nested`.</span></span> <span data-ttu-id="6e059-534">İçinde `Nested`, yöntem `G` statik metodu çağırır `F` tanımlanan `C`, ve `F` özel erişilebilirlik bildirdiği.</span><span class="sxs-lookup"><span data-stu-id="6e059-534">Within `Nested`, the method `G` calls the static method `F` defined in `C`, and `F` has private declared accessibility.</span></span>

<span data-ttu-id="6e059-535">İç içe türü, kapsadığı tür temel bir türde tanımlanan korumalı üyeler da erişebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6e059-535">A nested type also may access protected members defined in a base type of its containing type.</span></span> <span data-ttu-id="6e059-536">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-536">In the example</span></span>
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
<span data-ttu-id="6e059-537">iç içe geçmiş sınıf `Derived.Nested` korumalı yöntem erişen `F` tanımlanan `Derived`ait temel sınıfı, `Base`kullanarak, bir örneği üzerinden çağırma `Derived`.</span><span class="sxs-lookup"><span data-stu-id="6e059-537">the nested class `Derived.Nested` accesses the protected method `F` defined in `Derived`'s base class, `Base`, by calling through an instance of `Derived`.</span></span>

#### <a name="nested-types-in-generic-classes"></a><span data-ttu-id="6e059-538">Genel sınıflar içinde iç içe geçmiş türler</span><span class="sxs-lookup"><span data-stu-id="6e059-538">Nested types in generic classes</span></span>

<span data-ttu-id="6e059-539">Genel sınıf bildirimi, iç içe geçmiş tür bildirimleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-539">A generic class declaration can contain nested type declarations.</span></span> <span data-ttu-id="6e059-540">Tür parametreleri kapsayan sınıfın içinde iç içe geçmiş türleri kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-540">The type parameters of the enclosing class can be used within the nested types.</span></span> <span data-ttu-id="6e059-541">Bir iç içe tür bildiriminde kullanıldığında, yalnızca iç içe türü için geçerli olan ek tür parametreleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-541">A nested type declaration can contain additional type parameters that apply only to the nested type.</span></span>

<span data-ttu-id="6e059-542">Genel sınıf bildirimi içinde yer alan her tür bildirim örtük olarak bir genel tür bildirimidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-542">Every type declaration contained within a generic class declaration is implicitly a generic type declaration.</span></span> <span data-ttu-id="6e059-543">Genel tür içinde iç içe bir türe başvuru yazma, tür bağımsız değişkenlerinden dahil olmak üzere kapsayan bir oluşturulmuş tür adlandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-543">When writing a reference to a type nested within a generic type, the containing constructed type, including its type arguments, must be named.</span></span> <span data-ttu-id="6e059-544">Ancak, gelen dış sınıf içinde iç içe türün nitelenmeden kullanılabilir; Dış sınıfın örnek türüne örtük olarak iç içe türün oluşturulurken kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-544">However, from within the outer class, the nested type can be used without qualification; the instance type of the outer class can be implicitly used when constructing the nested type.</span></span> <span data-ttu-id="6e059-545">Aşağıdaki örnek, oluşturulan oluşturulan bir tür belirtmek için üç farklı doğru şekilde gösterir. `Inner`; ilk iki eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="6e059-545">The following example shows three different correct ways to refer to a constructed type created from `Inner`; the first two are equivalent:</span></span>
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

<span data-ttu-id="6e059-546">Hatalı olmasına rağmen stili programlama bir tür parametresi iç içe bir türde üye gizleyebilir veya dış türde bildirilen parametre türü:</span><span class="sxs-lookup"><span data-stu-id="6e059-546">Although it is bad programming style, a type parameter in a nested type can hide a member or type parameter declared in the outer type:</span></span>
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a><span data-ttu-id="6e059-547">Ayrılmış üye adları</span><span class="sxs-lookup"><span data-stu-id="6e059-547">Reserved member names</span></span>

<span data-ttu-id="6e059-548">Uygulama, temel C# çalışma zamanı uygulaması için bir özellik, olay veya dizin oluşturucu, her kaynak üye bildirimi kolaylaştırmak için iki yöntem imzaları üye bildirimi, adını ve türünü türüne göre ayırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-548">To facilitate the underlying C# run-time implementation, for each source member declaration that is a property, event, or indexer, the implementation must reserve two method signatures based on the kind of the member declaration, its name, and its type.</span></span> <span data-ttu-id="6e059-549">Bir derleme zamanı hata imzası olan, bunlardan birini ile eşleşen bir üye imzaları, ayrılmış temel alınan çalışma zamanı uygulama kullanmayan bile bildirmek bir program için bu ayırmalar kullanın.</span><span class="sxs-lookup"><span data-stu-id="6e059-549">It is a compile-time error for a program to declare a member whose signature matches one of these reserved signatures, even if the underlying run-time implementation does not make use of these reservations.</span></span>

<span data-ttu-id="6e059-550">Ayrılmış adlar bildirimleri İstemediğimiz, bu nedenle bunlar içinde üye araması katılmaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-550">The reserved names do not introduce declarations, thus they do not participate in member lookup.</span></span> <span data-ttu-id="6e059-551">Ancak, bir bildirim ayrılmış yöntem imzaları Devralmada katılmak ilişkili ([devralma](classes.md#inheritance)) ve ile gizli `new` değiştirici ([yeni değiştiricisini](classes.md#the-new-modifier)).</span><span class="sxs-lookup"><span data-stu-id="6e059-551">However, a declaration's associated reserved method signatures do participate in inheritance ([Inheritance](classes.md#inheritance)), and can be hidden with the `new` modifier ([The new modifier](classes.md#the-new-modifier)).</span></span>

<span data-ttu-id="6e059-552">Bu adlar ayırma üç amaca hizmet eder:</span><span class="sxs-lookup"><span data-stu-id="6e059-552">The reservation of these names serves three purposes:</span></span>

*  <span data-ttu-id="6e059-553">Get için bir yöntem adı olarak normal bir tanımlayıcı kullanın veya erişim için C# dil özelliği ayarlamak temel uygulama izin verme.</span><span class="sxs-lookup"><span data-stu-id="6e059-553">To allow the underlying implementation to use an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="6e059-554">Diğer diller, bir yöntem adı get için normal bir tanımlayıcı ile birlikte çalışmak veya erişim için C# dil özelliği ayarlamak izin verme.</span><span class="sxs-lookup"><span data-stu-id="6e059-554">To allow other languages to interoperate using an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="6e059-555">Kaynak bir uyumlu derleyici tarafından kabul sağlamaya yardımcı olmak için başkası tarafından ayrılmış üyeyle ayrıntılarını adları tutarlı tüm C# uygulamaları arasında yaparak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-555">To help ensure that the source accepted by one conforming compiler is accepted by another, by making the specifics of reserved member names consistent across all C# implementations.</span></span>

<span data-ttu-id="6e059-556">Bir yok edici bildirimini ([yok ediciler](classes.md#destructors)) de ayrılacak imza neden olur ([üye adları yok ediciler için ayrılmış](classes.md#member-names-reserved-for-destructors)).</span><span class="sxs-lookup"><span data-stu-id="6e059-556">The declaration of a destructor ([Destructors](classes.md#destructors)) also causes a signature to be reserved ([Member names reserved for destructors](classes.md#member-names-reserved-for-destructors)).</span></span>

#### <a name="member-names-reserved-for-properties"></a><span data-ttu-id="6e059-557">Üye adları ayrılmış özellikleri</span><span class="sxs-lookup"><span data-stu-id="6e059-557">Member names reserved for properties</span></span>

<span data-ttu-id="6e059-558">Bir özellik için `P` ([özellikleri](classes.md#properties)) türü `T`, aşağıdaki imzalar ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="6e059-558">For a property `P` ([Properties](classes.md#properties)) of type `T`, the following signatures are reserved:</span></span>

```csharp
T get_P();
void set_P(T value);
```

<span data-ttu-id="6e059-559">Salt okunur veya salt yazılır özelliği olsa bile, her iki imzaları ayrılmıştır.</span><span class="sxs-lookup"><span data-stu-id="6e059-559">Both signatures are reserved, even if the property is read-only or write-only.</span></span>

<span data-ttu-id="6e059-560">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-560">In the example</span></span>
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
<span data-ttu-id="6e059-561">bir sınıf `A` salt okunur özelliği tanımlar `P`, bu nedenle imzaları ayırma `get_P` ve `set_P` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="6e059-561">a class `A` defines a read-only property `P`, thus reserving signatures for `get_P` and `set_P` methods.</span></span> <span data-ttu-id="6e059-562">Bir sınıf `B` türetildiği `A` ve bu ayrılmış imzalar her ikisi de gizler.</span><span class="sxs-lookup"><span data-stu-id="6e059-562">A class `B` derives from `A` and hides both of these reserved signatures.</span></span> <span data-ttu-id="6e059-563">Örnek çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="6e059-563">The example produces the output:</span></span>
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a><span data-ttu-id="6e059-564">Üye adları ayrılmış olayları</span><span class="sxs-lookup"><span data-stu-id="6e059-564">Member names reserved for events</span></span>

<span data-ttu-id="6e059-565">Bir olayın `E` ([olayları](classes.md#events)) temsilci türünün `T`, aşağıdaki imzalar ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="6e059-565">For an event `E` ([Events](classes.md#events)) of delegate type `T`, the following signatures are reserved:</span></span>
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a><span data-ttu-id="6e059-566">Dizin oluşturucular için ayrılmış üye adları</span><span class="sxs-lookup"><span data-stu-id="6e059-566">Member names reserved for indexers</span></span>

<span data-ttu-id="6e059-567">Bir dizin oluşturucu için ([dizin oluşturucular](classes.md#indexers)) türü `T` parametre listesiyle `L`, aşağıdaki imzalar ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="6e059-567">For an indexer ([Indexers](classes.md#indexers)) of type `T` with parameter-list `L`, the following signatures are reserved:</span></span>
```csharp
T get_Item(L);
void set_Item(L, T value);
```

<span data-ttu-id="6e059-568">Dizin Oluşturucu salt okunur veya salt yazılır olsa bile, her iki imzaları ayrılmıştır.</span><span class="sxs-lookup"><span data-stu-id="6e059-568">Both signatures are reserved, even if the indexer is read-only or write-only.</span></span>

<span data-ttu-id="6e059-569">Ayrıca üye adı `Item` ayrılmıştır.</span><span class="sxs-lookup"><span data-stu-id="6e059-569">Furthermore the member name `Item` is reserved.</span></span>

#### <a name="member-names-reserved-for-destructors"></a><span data-ttu-id="6e059-570">Üye adları ayrılmış için Yıkıcılar</span><span class="sxs-lookup"><span data-stu-id="6e059-570">Member names reserved for destructors</span></span>

<span data-ttu-id="6e059-571">Yıkıcı içeren bir sınıf için ([yok ediciler](classes.md#destructors)), aşağıdaki imzası ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="6e059-571">For a class containing a destructor ([Destructors](classes.md#destructors)), the following signature is reserved:</span></span>
```csharp
void Finalize();
```

## <a name="constants"></a><span data-ttu-id="6e059-572">Sabitler</span><span class="sxs-lookup"><span data-stu-id="6e059-572">Constants</span></span>

<span data-ttu-id="6e059-573">A ***sabit*** sabit bir değeri temsil eden bir sınıf üyesi: derleme zamanında hesaplanabilir bir değer.</span><span class="sxs-lookup"><span data-stu-id="6e059-573">A ***constant*** is a class member that represents a constant value: a value that can be computed at compile-time.</span></span> <span data-ttu-id="6e059-574">A *constant_declaration* belirli bir türün bir veya daha fazla sabitleri tanıtır.</span><span class="sxs-lookup"><span data-stu-id="6e059-574">A *constant_declaration* introduces one or more constants of a given type.</span></span>

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

<span data-ttu-id="6e059-575">A *constant_declaration* bir dizi içerebilir *öznitelikleri* ([öznitelikleri](attributes.md)), `new` değiştirici ([yeni değiştiricisini](classes.md#the-new-modifier)), Geçerli birlikte dört erişim değiştiricileri ([erişim değiştiricilerine](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="6e059-575">A *constant_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span> <span data-ttu-id="6e059-576">Üyeleri tarafından bildirilen tüm öznitelikler ve değiştiriciler uygulamak *constant_declaration*.</span><span class="sxs-lookup"><span data-stu-id="6e059-576">The attributes and modifiers apply to all of the members declared by the *constant_declaration*.</span></span> <span data-ttu-id="6e059-577">Sabit statik üyeleri olarak kabul edilir olsa bile bir *constant_declaration* gerektirir veya sağlar bir `static` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-577">Even though constants are considered static members, a *constant_declaration* neither requires nor allows a `static` modifier.</span></span> <span data-ttu-id="6e059-578">Aynı değiştiricisi sabit bir bildirimde birden çok kez görünmesi için bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="6e059-578">It is an error for the same modifier to appear multiple times in a constant declaration.</span></span>

<span data-ttu-id="6e059-579">*Türü* , bir *constant_declaration* bildirim tarafından tanıtılan üyelerin türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-579">The *type* of a *constant_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="6e059-580">Tür listesi tarafından izlenir *constant_declarator*s, her biri yeni bir üye tanıtır.</span><span class="sxs-lookup"><span data-stu-id="6e059-580">The type is followed by a list of *constant_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="6e059-581">A *constant_declarator* oluşan bir *tanımlayıcı* ardından üye adları bir "`=`" belirteci ve ardından bir *constant_expression* ([ Sabit ifadeler](expressions.md#constant-expressions)), üyenin değeri verir.</span><span class="sxs-lookup"><span data-stu-id="6e059-581">A *constant_declarator* consists of an *identifier* that names the member, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the member.</span></span>

<span data-ttu-id="6e059-582">*Türü* Sabitte belirtilen bildirimi olmalıdır `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`e *enum_type*, veya bir *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="6e059-582">The *type* specified in a constant declaration must be `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, an *enum_type*, or a *reference_type*.</span></span> <span data-ttu-id="6e059-583">Her *constant_expression* hedef türünün veya hedef türe örtük bir dönüştürme tarafından dönüştürülebilir bir türün bir değeri yield gerekir ([örtük dönüştürmelerin](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="6e059-583">Each *constant_expression* must yield a value of the target type or of a type that can be converted to the target type by an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>

<span data-ttu-id="6e059-584">*Türü* bir sabit'ın en az sabit olarak olarak erişilebilir olmalıdır ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6e059-584">The *type* of a constant must be at least as accessible as the constant itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6e059-585">Bir sabit değeri bir ifade kullanarak elde edilen bir *simple_name* ([basit adları](expressions.md#simple-names)) veya bir *member_access* ([üye erişimi](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="6e059-585">The value of a constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="6e059-586">Bir sabit kendisini katılabilir bir *constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="6e059-586">A constant can itself participate in a *constant_expression*.</span></span> <span data-ttu-id="6e059-587">Bu nedenle, bir sabit gerektiren herhangi bir yapı içinde kullanılabilir bir *constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="6e059-587">Thus, a constant may be used in any construct that requires a *constant_expression*.</span></span> <span data-ttu-id="6e059-588">Bu tür yapıları örneklerindendir `case` etiketleri, `goto case` deyimleri `enum` üye bildirimleri, öznitelikler ve diğer sabit bildirimleri.</span><span class="sxs-lookup"><span data-stu-id="6e059-588">Examples of such constructs include `case` labels, `goto case` statements, `enum` member declarations, attributes, and other constant declarations.</span></span>

<span data-ttu-id="6e059-589">Bölümünde anlatıldığı gibi [sabit ifadeler](expressions.md#constant-expressions), *constant_expression* , derleme zamanında tam olarak değerlendirilebilecek bir ifadedir.</span><span class="sxs-lookup"><span data-stu-id="6e059-589">As described in [Constant expressions](expressions.md#constant-expressions), a *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span> <span data-ttu-id="6e059-590">Tek yolu, bir null olmayan değer oluşturmak için bu yana bir *reference_type* dışında `string` uygulamaktır `new` işleci ve bu yana `new` işleci izin verilmez bir *constant_ ifade*, sabitleri için tek olası değer *reference_type*s dışında `string` olduğu `null`.</span><span class="sxs-lookup"><span data-stu-id="6e059-590">Since the only way to create a non-null value of a *reference_type* other than `string` is to apply the `new` operator, and since the `new` operator is not permitted in a *constant_expression*, the only possible value for constants of *reference_type*s other than `string` is `null`.</span></span>

<span data-ttu-id="6e059-591">Sabit değer için bir simgesel ad ne zaman isterseniz, ancak, bu değerin türü Sabit bildiriminde izin verilmez veya ne zaman değeri derleme zamanında deltanın hesaplanamaması bir *constant_expression*, `readonly` alan () [Salt okunur alanların](classes.md#readonly-fields)) bunun yerine kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-591">When a symbolic name for a constant value is desired, but when the type of that value is not permitted in a constant declaration, or when the value cannot be computed at compile-time by a *constant_expression*, a `readonly` field ([Readonly fields](classes.md#readonly-fields)) may be used instead.</span></span>

<span data-ttu-id="6e059-592">Birden fazla sabit bildirir sabit bir bildirimde öznitelikler, değiştiriciler ve türe sahip tek sabitlerin birden fazla bildirimi eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="6e059-592">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="6e059-593">Örneğin</span><span class="sxs-lookup"><span data-stu-id="6e059-593">For example</span></span>
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
<span data-ttu-id="6e059-594">eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="6e059-594">is equivalent to</span></span>
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

<span data-ttu-id="6e059-595">Sabitler, diğer sabitleri aynı programda bağımlılıkları döngüsel doğasını olmayan sürece bağımlı izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="6e059-595">Constants are permitted to depend on other constants within the same program as long as the dependencies are not of a circular nature.</span></span> <span data-ttu-id="6e059-596">Derleyici, sabit bildirimleri doğru sırada değerlendirilecek otomatik olarak düzenler.</span><span class="sxs-lookup"><span data-stu-id="6e059-596">The compiler automatically arranges to evaluate the constant declarations in the appropriate order.</span></span> <span data-ttu-id="6e059-597">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-597">In the example</span></span>
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
<span data-ttu-id="6e059-598">Derleyici ilk değerlendirir `A.Y`, sonra değerlendirir `B.Z`ve son olarak değerlendirilirse `A.X`, değerleri üreten `10`, `11`, ve `12`.</span><span class="sxs-lookup"><span data-stu-id="6e059-598">the compiler first evaluates `A.Y`, then evaluates `B.Z`, and finally evaluates `A.X`, producing the values `10`, `11`, and `12`.</span></span> <span data-ttu-id="6e059-599">Sabit bildirimi programlardan sabitleri bağlı olabilir, ancak bu tür bağımlılıkları sadece tek yöndedir mümkündür.</span><span class="sxs-lookup"><span data-stu-id="6e059-599">Constant declarations may depend on constants from other programs, but such dependencies are only possible in one direction.</span></span> <span data-ttu-id="6e059-600">Yukarıdaki örnekte, başvuran `A` ve `B` bildirilen ayrı programlarında mümkün olacaktır `A.X` bağlıdır `B.Z`, ancak `B.Z` sonra değil, aynı anda bağımlı `A.Y`.</span><span class="sxs-lookup"><span data-stu-id="6e059-600">Referring to the example above, if `A` and `B` were declared in separate programs, it would be possible for `A.X` to depend on `B.Z`, but `B.Z` could then not simultaneously depend on `A.Y`.</span></span>

## <a name="fields"></a><span data-ttu-id="6e059-601">Alanlar</span><span class="sxs-lookup"><span data-stu-id="6e059-601">Fields</span></span>

<span data-ttu-id="6e059-602">A ***alan*** bir nesneyle veya sınıfla ilişkili bir değişkeni temsil eden bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-602">A ***field*** is a member that represents a variable associated with an object or class.</span></span> <span data-ttu-id="6e059-603">A *field_declaration* belirli bir türün bir veya daha fazla alan sunar.</span><span class="sxs-lookup"><span data-stu-id="6e059-603">A *field_declaration* introduces one or more fields of a given type.</span></span>

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

<span data-ttu-id="6e059-604">A *field_declaration* bir dizi içerebilir *öznitelikleri* ([öznitelikleri](attributes.md)), `new` değiştirici ([yeni değiştiricisini](classes.md#the-new-modifier)), Geçerli birleşimi dört erişim değiştiricileri ([erişim değiştiricilerine](classes.md#access-modifiers)) ve `static` değiştirici ([statik ve örnek alanları](classes.md#static-and-instance-fields)).</span><span class="sxs-lookup"><span data-stu-id="6e059-604">A *field_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and a `static` modifier ([Static and instance fields](classes.md#static-and-instance-fields)).</span></span> <span data-ttu-id="6e059-605">Ayrıca, bir *field_declaration* içerebilir bir `readonly` değiştiricisi ([salt okunur alanların](classes.md#readonly-fields)) veya bir `volatile` değiştiricisi ([geçici alanları](classes.md#volatile-fields)) ikisini birden belirtmeyin.</span><span class="sxs-lookup"><span data-stu-id="6e059-605">In addition, a *field_declaration* may include a `readonly` modifier ([Readonly fields](classes.md#readonly-fields)) or a `volatile` modifier ([Volatile fields](classes.md#volatile-fields)) but not both.</span></span> <span data-ttu-id="6e059-606">Üyeleri tarafından bildirilen tüm öznitelikler ve değiştiriciler uygulamak *field_declaration*.</span><span class="sxs-lookup"><span data-stu-id="6e059-606">The attributes and modifiers apply to all of the members declared by the *field_declaration*.</span></span> <span data-ttu-id="6e059-607">V deklaraci pole birden çok kez görünmesini aynı değiştiricisi için bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="6e059-607">It is an error for the same modifier to appear multiple times in a field declaration.</span></span>

<span data-ttu-id="6e059-608">*Türü* , bir *field_declaration* bildirim tarafından tanıtılan üyelerin türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-608">The *type* of a *field_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="6e059-609">Tür listesi tarafından izlenir *variable_declarator*s, her biri yeni bir üye tanıtır.</span><span class="sxs-lookup"><span data-stu-id="6e059-609">The type is followed by a list of *variable_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="6e059-610">A *variable_declarator* oluşan bir *tanımlayıcı* ardından isteğe bağlı olarak, bu üye adları bir "`=`" belirteci ve *variable_initializer* ([ Değişken başlatıcılar](classes.md#variable-initializers)), bu üyenin ilk değeri verir.</span><span class="sxs-lookup"><span data-stu-id="6e059-610">A *variable_declarator* consists of an *identifier* that names that member, optionally followed by an "`=`" token and a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)) that gives the initial value of that member.</span></span>

<span data-ttu-id="6e059-611">*Türü* alanı en az alan olarak olarak erişilebilir olmalıdır ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6e059-611">The *type* of a field must be at least as accessible as the field itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6e059-612">Bir alanın değeri bir ifade kullanarak alınan bir *simple_name* ([basit adları](expressions.md#simple-names)) veya bir *member_access* ([üye erişimi](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="6e059-612">The value of a field is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="6e059-613">Kullanarak salt okunur olmayan bir alan değerini değiştiren bir *atama* ([atama işleçleri](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="6e059-613">The value of a non-readonly field is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="6e059-614">Salt okunur olmayan bir alan değeri, değiştirilmiş ve elde edilen hem de sonek artırma ve azaltma işleçleri olabilir ([sonek arttırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators)) ve önek arttırma ve azaltma işleçleri ([ön eki artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="6e059-614">The value of a non-readonly field can be both obtained and modified using postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)) and prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="6e059-615">Birden çok alan bildiren bir alan bildirimi tek alanların özniteliklerini, değiştiriciler ve türe sahip birden fazla bildirimi eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="6e059-615">A field declaration that declares multiple fields is equivalent to multiple declarations of single fields with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="6e059-616">Örneğin</span><span class="sxs-lookup"><span data-stu-id="6e059-616">For example</span></span>
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
<span data-ttu-id="6e059-617">eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="6e059-617">is equivalent to</span></span>
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a><span data-ttu-id="6e059-618">Statik ve örnek alanları</span><span class="sxs-lookup"><span data-stu-id="6e059-618">Static and instance fields</span></span>

<span data-ttu-id="6e059-619">Ne zaman bir alan bildirimi içeren bir `static` değiştiricisi bildirim tarafından tanıtılan alanlar ***statik alanlar***.</span><span class="sxs-lookup"><span data-stu-id="6e059-619">When a field declaration includes a `static` modifier, the fields introduced by the declaration are ***static fields***.</span></span> <span data-ttu-id="6e059-620">Hiçbir `static` değiştirici mevcut, alanlar bildirim tarafından tanıtılan ***örnek alanları***.</span><span class="sxs-lookup"><span data-stu-id="6e059-620">When no `static` modifier is present, the fields introduced by the declaration are ***instance fields***.</span></span> <span data-ttu-id="6e059-621">Statik ve örnek alanları olan iki değişkenleri çeşitli türlerde ([değişkenleri](variables.md)) C# tarafından desteklenen ve bazen kullanıcılar olarak adlandırılır ***statik değişkenler*** ve ***örnek değişkenleri*** sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="6e059-621">Static fields and instance fields are two of the several kinds of variables ([Variables](variables.md)) supported by C#, and at times they are referred to as ***static variables*** and ***instance variables***, respectively.</span></span>

<span data-ttu-id="6e059-622">Statik bir alan belirli bir örneğine bir parçası değil; Bunun yerine, tüm örneklerini kapalı bir tür arasında paylaşılan ([açık ve kapalı türleri](types.md#open-and-closed-types)).</span><span class="sxs-lookup"><span data-stu-id="6e059-622">A static field is not part of a specific instance; instead, it is shared amongst all instances of a closed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="6e059-623">Bir kapalı sınıf türünün kaç örneklerin oluşturulduğu ne olursa olsun, yalnızca bir kopyasını ilişkili uygulama etki alanı için statik bir alan yoktur.</span><span class="sxs-lookup"><span data-stu-id="6e059-623">No matter how many instances of a closed class type are created, there is only ever one copy of a static field for the associated application domain.</span></span>

<span data-ttu-id="6e059-624">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6e059-624">For example:</span></span>
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

<span data-ttu-id="6e059-625">Bir örnek alanıyla örneğine ait.</span><span class="sxs-lookup"><span data-stu-id="6e059-625">An instance field belongs to an instance.</span></span> <span data-ttu-id="6e059-626">Özellikle, bir sınıfın her örneği, bu sınıfın tüm örnek alanları ayrı bir kümesini içerir.</span><span class="sxs-lookup"><span data-stu-id="6e059-626">Specifically, every instance of a class contains a separate set of all the instance fields of that class.</span></span>

<span data-ttu-id="6e059-627">Ne zaman bir alan başvuru olarak bir *member_access* ([üye erişimi](expressions.md#member-access)) biçiminde `E.M`, `M` bir statik alanı `E` içeren türü belirtmek `M` ve eğer `M` bir örnek alanı E türünü içeren bir örneğini belirtmek gerekir `M`.</span><span class="sxs-lookup"><span data-stu-id="6e059-627">When a field is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static field, `E` must denote a type containing `M`, and if `M` is an instance field, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="6e059-628">Statik arasındaki farklar ve örnek üyeleri ele alınmıştır daha ayrıntılı olarak [statik ve örnek üyeleri](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="6e059-628">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="readonly-fields"></a><span data-ttu-id="6e059-629">Salt okunur alanları</span><span class="sxs-lookup"><span data-stu-id="6e059-629">Readonly fields</span></span>

<span data-ttu-id="6e059-630">Olduğunda bir *field_declaration* içeren bir `readonly` değiştiricisi bildirim tarafından tanıtılan alanlar ***salt okunur alanların***.</span><span class="sxs-lookup"><span data-stu-id="6e059-630">When a *field_declaration* includes a `readonly` modifier, the fields introduced by the declaration are ***readonly fields***.</span></span> <span data-ttu-id="6e059-631">Salt okunur alanlar doğrudan atamaları, yalnızca bu bildirimin ya da bir örnek oluşturucusu veya aynı sınıftaki statik Oluşturucusu bir parçası olarak gerçekleşebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-631">Direct assignments to readonly fields can only occur as part of that declaration or in an instance constructor or static constructor in the same class.</span></span> <span data-ttu-id="6e059-632">(Salt okunur bir alan için birden çok kez bu bağlamda atanabilir.) Özellikle, doğrudan atamaları bir `readonly` alanı yalnızca şu bağlamlarda verilir:</span><span class="sxs-lookup"><span data-stu-id="6e059-632">(A readonly field can be assigned to multiple times in these contexts.) Specifically, direct assignments to a `readonly` field are permitted only in the following contexts:</span></span>

*  <span data-ttu-id="6e059-633">İçinde *variable_declarator* alanı sunan (ekleyerek bir *variable_initializer* bildiriminde).</span><span class="sxs-lookup"><span data-stu-id="6e059-633">In the *variable_declarator* that introduces the field (by including a *variable_initializer* in the declaration).</span></span>
*  <span data-ttu-id="6e059-634">Bir örnek alan için sınıf örneği oluşturucularda alan bildirimi içerir; sınıfın statik oluşturucuda statik bir alan için alan bildirimini içerir.</span><span class="sxs-lookup"><span data-stu-id="6e059-634">For an instance field, in the instance constructors of the class that contains the field declaration; for a static field, in the static constructor of the class that contains the field declaration.</span></span> <span data-ttu-id="6e059-635">Olduğu geçirmek için geçerli tek bağlamı da bunlar bir `readonly` olarak alan bir `out` veya `ref` parametresi.</span><span class="sxs-lookup"><span data-stu-id="6e059-635">These are also the only contexts in which it is valid to pass a `readonly` field as an `out` or `ref` parameter.</span></span>

<span data-ttu-id="6e059-636">İçin atanmaya çalışılırken bir `readonly` olarak geçirmek veya alanı bir `out` veya `ref` başka bir bağlam parametresi olan bir derleme zamanı hatası.</span><span class="sxs-lookup"><span data-stu-id="6e059-636">Attempting to assign to a `readonly` field or pass it as an `out` or `ref` parameter in any other context is a compile-time error.</span></span>

#### <a name="using-static-readonly-fields-for-constants"></a><span data-ttu-id="6e059-637">Sabitler için statik salt okunur alanları kullanma</span><span class="sxs-lookup"><span data-stu-id="6e059-637">Using static readonly fields for constants</span></span>

<span data-ttu-id="6e059-638">A `static readonly` ne zaman bir sabit değer için bir simgesel ad isteniyorsa, ancak ne zaman değer türü olarak izin verilmez alan yararlı bir `const` bildirimi veya ne zaman kullanılamaz hesaplanan değer derleme zamanında.</span><span class="sxs-lookup"><span data-stu-id="6e059-638">A `static readonly` field is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a `const` declaration, or when the value cannot be computed at compile-time.</span></span> <span data-ttu-id="6e059-639">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-639">In the example</span></span>
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
<span data-ttu-id="6e059-640">`Black`, `White`, `Red`, `Green`, ve `Blue` üyeleri olarak bildirilemez `const` üyelerinin değerlerini derleme zamanında hesaplanamıyor çünkü.</span><span class="sxs-lookup"><span data-stu-id="6e059-640">the `Black`, `White`, `Red`, `Green`, and `Blue` members cannot be declared as `const` members because their values cannot be computed at compile-time.</span></span> <span data-ttu-id="6e059-641">Ancak, bunları bildirme `static readonly` yerine çok aynı etkiye sahiptir.</span><span class="sxs-lookup"><span data-stu-id="6e059-641">However, declaring them `static readonly` instead has much the same effect.</span></span>

#### <a name="versioning-of-constants-and-static-readonly-fields"></a><span data-ttu-id="6e059-642">Sabitler ve statik salt okunur alanların sürüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="6e059-642">Versioning of constants and static readonly fields</span></span>

<span data-ttu-id="6e059-643">Sabitler ve salt okunur alanları farklı ikili sürüm semantiklere sahip.</span><span class="sxs-lookup"><span data-stu-id="6e059-643">Constants and readonly fields have different binary versioning semantics.</span></span> <span data-ttu-id="6e059-644">Bir sabit bir ifade başvuruda bulunduğunda, derleme zamanında sabit değeri elde edilir, ancak salt okunur bir alan bir ifade başvuruda bulunduğunda, alanın değerini kadar çalışma zamanı elde değil.</span><span class="sxs-lookup"><span data-stu-id="6e059-644">When an expression references a constant, the value of the constant is obtained at compile-time, but when an expression references a readonly field, the value of the field is not obtained until run-time.</span></span> <span data-ttu-id="6e059-645">İki ayrı programlardan oluşan bir uygulamayı düşünün:</span><span class="sxs-lookup"><span data-stu-id="6e059-645">Consider an application that consists of two separate programs:</span></span>
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

<span data-ttu-id="6e059-646">`Program1` Ve `Program2` ad alanları belirtmek ayrı olarak derlenen iki program.</span><span class="sxs-lookup"><span data-stu-id="6e059-646">The `Program1` and `Program2` namespaces denote two programs that are compiled separately.</span></span> <span data-ttu-id="6e059-647">Çünkü `Program1.Utils.X` değeri çıktı tarafından statik salt okunur bir alan olarak bildirilen `Console.WriteLine` deyimi, derleme zamanında bilinmiyor ancak bunun yerine, çalışma zamanında elde edilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-647">Because `Program1.Utils.X` is declared as a static readonly field, the value output by the `Console.WriteLine` statement is not known at compile-time, but rather is obtained at run-time.</span></span> <span data-ttu-id="6e059-648">Bu nedenle, varsa değerini `X` değiştirilir ve `Program1` derlenir, `Console.WriteLine` deyimi, yeni değer olsa bile çıktı `Program2` yeniden derlenen değil.</span><span class="sxs-lookup"><span data-stu-id="6e059-648">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` isn't recompiled.</span></span> <span data-ttu-id="6e059-649">Ancak sahip `X` bir sabit değeri olan `X` zaman almış olmanız `Program2` derlenen ve değişikliklerinden etkilenmeyen kalır `Program1` kadar `Program2` derlenir.</span><span class="sxs-lookup"><span data-stu-id="6e059-649">However, had `X` been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would remain unaffected by changes in `Program1` until `Program2` is recompiled.</span></span>

### <a name="volatile-fields"></a><span data-ttu-id="6e059-650">Volatile alanları</span><span class="sxs-lookup"><span data-stu-id="6e059-650">Volatile fields</span></span>

<span data-ttu-id="6e059-651">Olduğunda bir *field_declaration* içeren bir `volatile` değiştiricisi, bu bildirim tarafından tanıtılan alanlar ***geçici alanları***.</span><span class="sxs-lookup"><span data-stu-id="6e059-651">When a *field_declaration* includes a `volatile` modifier, the fields introduced by that declaration are ***volatile fields***.</span></span>

<span data-ttu-id="6e059-652">Geçici olmayan alanlar için yönergeler yeniden sıralama iyileştirme teknikleri tarafından sağlanan gibi eşitleme olmadan alanlarına erişmek çok iş parçacıklı programlarda beklenmeyen ve beklenmeyen sonuçlara neden olabilecek *lock_statement*  ([Lock deyiminin](statements.md#the-lock-statement)).</span><span class="sxs-lookup"><span data-stu-id="6e059-652">For non-volatile fields, optimization techniques that reorder instructions can lead to unexpected and unpredictable results in multi-threaded programs that access fields without synchronization such as that provided by the *lock_statement* ([The lock statement](statements.md#the-lock-statement)).</span></span> <span data-ttu-id="6e059-653">Bu iyileştirmeler, derleyici tarafından çalışma zamanı sistemi tarafından veya donanım tarafından gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-653">These optimizations can be performed by the compiler, by the run-time system, or by hardware.</span></span> <span data-ttu-id="6e059-654">Volatile alanlar için sipariş gibi iyileştirmeler sınırlıdır:</span><span class="sxs-lookup"><span data-stu-id="6e059-654">For volatile fields, such reordering optimizations are restricted:</span></span>

*  <span data-ttu-id="6e059-655">Geçici bir alan okuma adlı bir ***geçici okuma***.</span><span class="sxs-lookup"><span data-stu-id="6e059-655">A read of a volatile field is called a ***volatile read***.</span></span> <span data-ttu-id="6e059-656">Geçici okuma "semantiği Al"; var diğer bir deyişle, bundan sonra yönerge sırada gerçekleşen tüm başvurularını bellek önce gerçekleşmesi için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="6e059-656">A volatile read has "acquire semantics"; that is, it is guaranteed to occur prior to any references to memory that occur after it in the instruction sequence.</span></span>
*  <span data-ttu-id="6e059-657">Geçici bir alan yazma adlı bir ***geçici yazma***.</span><span class="sxs-lookup"><span data-stu-id="6e059-657">A write of a volatile field is called a ***volatile write***.</span></span> <span data-ttu-id="6e059-658">Geçici bir yazma "semantik sürüm"; var diğer bir deyişle, tüm bellek başvuruları yazma yönerge yönerge dizisindeki önce sonra gerçekleşecek garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-658">A volatile write has "release semantics"; that is, it is guaranteed to happen after any memory references prior to the write instruction in the instruction sequence.</span></span>

<span data-ttu-id="6e059-659">Bu kısıtlamalar, tüm iş parçacıkları tarafından gerçekleştirilen sırada başka bir iş parçacığı tarafından gerçekleştirilen geçici yazma dikkate aldığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="6e059-659">These restrictions ensure that all threads will observe volatile writes performed by any other thread in the order in which they were performed.</span></span> <span data-ttu-id="6e059-660">Uyumlu bir uygulama, bir tek toplam yürütme tüm iş parçacıklarından görülen olarak geçici yazma sıralamasını sağlamak için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="6e059-660">A conforming implementation is not required to provide a single total ordering of volatile writes as seen from all threads of execution.</span></span> <span data-ttu-id="6e059-661">Geçici bir alan türü aşağıdakilerden biri olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="6e059-661">The type of a volatile field must be one of the following:</span></span>

*  <span data-ttu-id="6e059-662">A *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="6e059-662">A *reference_type*.</span></span>
*  <span data-ttu-id="6e059-663">Türü `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, veya` System.UIntPtr`.</span><span class="sxs-lookup"><span data-stu-id="6e059-663">The type `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, or` System.UIntPtr`.</span></span>
*  <span data-ttu-id="6e059-664">Bir *enum_type* temel bir enum türü olan `byte`, `sbyte`, `short`, `ushort`, `int`, veya `uint`.</span><span class="sxs-lookup"><span data-stu-id="6e059-664">An *enum_type* having an enum base type of `byte`, `sbyte`, `short`, `ushort`, `int`, or `uint`.</span></span>

<span data-ttu-id="6e059-665">Örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-665">The example</span></span>
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
<span data-ttu-id="6e059-666">çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="6e059-666">produces the output:</span></span>
```
result = 143
```

<span data-ttu-id="6e059-667">Bu örnekte, yöntem `Main` yöntemi çalıştıran yeni bir iş parçacığı başlattığını `Thread2`.</span><span class="sxs-lookup"><span data-stu-id="6e059-667">In this example, the method `Main` starts a new thread that runs the method `Thread2`.</span></span> <span data-ttu-id="6e059-668">Bu yöntem bir değer olarak adlandırılan bir geçici olmayan alana depolar `result`, ardından depolar `true` geçici bir alan olarak `finished`.</span><span class="sxs-lookup"><span data-stu-id="6e059-668">This method stores a value into a non-volatile field called `result`, then stores `true` in the volatile field `finished`.</span></span> <span data-ttu-id="6e059-669">Alan için ana iş parçacığı bekler `finished` ayarlanması `true`, alan olarak görülür `result`.</span><span class="sxs-lookup"><span data-stu-id="6e059-669">The main thread waits for the field `finished` to be set to `true`, then reads the field `result`.</span></span> <span data-ttu-id="6e059-670">Bu yana `finished` bildirilmiş `volatile`, ana iş parçacığı değeri okumalıdır `143` alanından `result`.</span><span class="sxs-lookup"><span data-stu-id="6e059-670">Since `finished` has been declared `volatile`, the main thread must read the value `143` from the field `result`.</span></span> <span data-ttu-id="6e059-671">Alanın `finished` değil bildirilmiş `volatile`, depoya için izin verilen sonra `result` deposuna sonra ana iş parçacığı için görünür olmasını `finished`ve bu nedenle değerini okumak ana iş parçacığı için `0` gelen alan `result`.</span><span class="sxs-lookup"><span data-stu-id="6e059-671">If the field `finished` had not been declared `volatile`, then it would be permissible for the store to `result` to be visible to the main thread after the store to `finished`, and hence for the main thread to read the value `0` from the field `result`.</span></span> <span data-ttu-id="6e059-672">Bildirme `finished` olarak bir `volatile` alan herhangi bir tutarsızlık engeller.</span><span class="sxs-lookup"><span data-stu-id="6e059-672">Declaring `finished` as a `volatile` field prevents any such inconsistency.</span></span>

### <a name="field-initialization"></a><span data-ttu-id="6e059-673">Alan başlatma</span><span class="sxs-lookup"><span data-stu-id="6e059-673">Field initialization</span></span>

<span data-ttu-id="6e059-674">Bir alanın başlangıç değerini bir statik alanı veya bir örnek alanıyla oluşmasından varsayılan değerdir ([varsayılan değerler](variables.md#default-values)) alanın türü.</span><span class="sxs-lookup"><span data-stu-id="6e059-674">The initial value of a field, whether it be a static field or an instance field, is the default value ([Default values](variables.md#default-values)) of the field's type.</span></span> <span data-ttu-id="6e059-675">Bu varsayılan başlatma oluştu ve bir alan bu nedenle hiçbir zaman "başlatılmamış" önce bir alanın değerini gözlemlemek mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="6e059-675">It is not possible to observe the value of a field before this default initialization has occurred, and a field is thus never "uninitialized".</span></span> <span data-ttu-id="6e059-676">Örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-676">The example</span></span>
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
<span data-ttu-id="6e059-677">çıktıyı üretir</span><span class="sxs-lookup"><span data-stu-id="6e059-677">produces the output</span></span>
```
b = False, i = 0
```
<span data-ttu-id="6e059-678">çünkü `b` ve `i` hem de varsayılan değerleri otomatik olarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-678">because `b` and `i` are both automatically initialized to default values.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="6e059-679">Değişken başlatıcıları</span><span class="sxs-lookup"><span data-stu-id="6e059-679">Variable initializers</span></span>

<span data-ttu-id="6e059-680">Alan bildirimler içerebilir *variable_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="6e059-680">Field declarations may include *variable_initializer*s.</span></span> <span data-ttu-id="6e059-681">Statik alanlar için değişken başlatıcılar sınıf başlatma sırasında yürütülen atama deyimleri karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="6e059-681">For static fields, variable initializers correspond to assignment statements that are executed during class initialization.</span></span> <span data-ttu-id="6e059-682">Örneğin alanları, değişken başlatıcılar sınıfının bir örneği oluşturulduğunda, yürütülen atama deyimleri karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="6e059-682">For instance fields, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span>

<span data-ttu-id="6e059-683">Örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-683">The example</span></span>
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
<span data-ttu-id="6e059-684">çıktıyı üretir</span><span class="sxs-lookup"><span data-stu-id="6e059-684">produces the output</span></span>
```
x = 1.4142135623731, i = 100, s = Hello
```
<span data-ttu-id="6e059-685">çünkü atama `x` gerçekleşir zaman statik alan başlatıcıları çalıştırmak ve atamalarını `i` ve `s` örnek alan başlatıcıları çalıştırmak oluşur.</span><span class="sxs-lookup"><span data-stu-id="6e059-685">because an assignment to `x` occurs when static field initializers execute and assignments to `i` and `s` occur when the instance field initializers execute.</span></span>

<span data-ttu-id="6e059-686">Açıklanan varsayılan değer başlatma [alan başlatma](classes.md#field-initialization) değişken başlatıcıları olan alanların dahil olmak üzere tüm alanlar için gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="6e059-686">The default value initialization described in [Field initialization](classes.md#field-initialization) occurs for all fields, including fields that have variable initializers.</span></span> <span data-ttu-id="6e059-687">Bu nedenle, bir sınıf başlatıldığında, bu sınıfın tüm statik alanları varsayılan değerlerine önce başlatılır ve statik alan başlatıcıları metinsel sırayla sonra yürütülür.</span><span class="sxs-lookup"><span data-stu-id="6e059-687">Thus, when a class is initialized, all static fields in that class are first initialized to their default values, and then the static field initializers are executed in textual order.</span></span> <span data-ttu-id="6e059-688">Benzer şekilde, bir sınıfın bir örneği oluşturulduğunda, bu örnekteki tüm örnek alanları varsayılan değerlerine önce başlatılır ve ardından örnek alan başlatıcıları metinsel sırayla yürütülür.</span><span class="sxs-lookup"><span data-stu-id="6e059-688">Likewise, when an instance of a class is created, all instance fields in that instance are first initialized to their default values, and then the instance field initializers are executed in textual order.</span></span>

<span data-ttu-id="6e059-689">Varsayılan değer durumlarını gözlenecek değişken başlatıcıları olan statik alanlar mümkündür.</span><span class="sxs-lookup"><span data-stu-id="6e059-689">It is possible for static fields with variable initializers to be observed in their default value state.</span></span> <span data-ttu-id="6e059-690">Ancak, bu stil sağlasa da kesinlikle önerilmez.</span><span class="sxs-lookup"><span data-stu-id="6e059-690">However, this is strongly discouraged as a matter of style.</span></span> <span data-ttu-id="6e059-691">Örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-691">The example</span></span>
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
<span data-ttu-id="6e059-692">Bu davranışı sergiler.</span><span class="sxs-lookup"><span data-stu-id="6e059-692">exhibits this behavior.</span></span> <span data-ttu-id="6e059-693">Döngüsel tanımları rağmen bir ve b, program geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-693">Despite the circular definitions of a and b, the program is valid.</span></span> <span data-ttu-id="6e059-694">Çıktı olur</span><span class="sxs-lookup"><span data-stu-id="6e059-694">It results in the output</span></span>
```
a = 1, b = 2
```
<span data-ttu-id="6e059-695">çünkü statik alanlar `a` ve `b` başlatılır `0` (varsayılan değeri `int`) kendi başlatıcılar yürütülmeden önce.</span><span class="sxs-lookup"><span data-stu-id="6e059-695">because the static fields `a` and `b` are initialized to `0` (the default value for `int`) before their initializers are executed.</span></span> <span data-ttu-id="6e059-696">Zaman için Başlatıcı `a` çalıştığında, değerini `b` sıfır ve bu nedenle `a` değerine ayarlanır `1`.</span><span class="sxs-lookup"><span data-stu-id="6e059-696">When the initializer for `a` runs, the value of `b` is zero, and so `a` is initialized to `1`.</span></span> <span data-ttu-id="6e059-697">Zaman için Başlatıcı `b` çalıştığında, değerini `a` zaten `1`ve bu nedenle `b` değerine ayarlanır `2`.</span><span class="sxs-lookup"><span data-stu-id="6e059-697">When the initializer for `b` runs, the value of `a` is already `1`, and so `b` is initialized to `2`.</span></span>

#### <a name="static-field-initialization"></a><span data-ttu-id="6e059-698">Statik alan başlatma</span><span class="sxs-lookup"><span data-stu-id="6e059-698">Static field initialization</span></span>

<span data-ttu-id="6e059-699">Bir sınıfın statik alan değişken başlatıcıları atamaları sınıfı bildiriminde göründükleri metinsel sırayla yürütülen bir dizi karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="6e059-699">The static field variable initializers of a class correspond to a sequence of assignments that are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="6e059-700">Bir statik Oluşturucu ([statik oluşturucular](classes.md#static-constructors)) var. sınıfında statik alan başlatıcıları yürütülmesi, statik Oluşturucusu yürütülmeden hemen önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="6e059-700">If a static constructor ([Static constructors](classes.md#static-constructors)) exists in the class, execution of the static field initializers occurs immediately prior to executing that static constructor.</span></span> <span data-ttu-id="6e059-701">Aksi takdirde, statik alan başlatıcıları, söz konusu sınıfın statik alanının ilk kez kullanıyorsanız önce bir uygulama bağımlıdır zamanında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="6e059-701">Otherwise, the static field initializers are executed at an implementation-dependent time prior to the first use of a static field of that class.</span></span> <span data-ttu-id="6e059-702">Örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-702">The example</span></span>
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
<span data-ttu-id="6e059-703">Çıkış ortaya çıkmasına neden olabilir:</span><span class="sxs-lookup"><span data-stu-id="6e059-703">might produce either the output:</span></span>
```
Init A
Init B
1 1
```
<span data-ttu-id="6e059-704">veya çıktı:</span><span class="sxs-lookup"><span data-stu-id="6e059-704">or the output:</span></span>
```
Init B
Init A
1 1
```
<span data-ttu-id="6e059-705">çünkü yürütülmesini `X`'s başlatıcı ve `Y`'s Başlatıcı, herhangi bir sırada olabilir; bunlar yalnızca bu alanlara başvurular önce gerçekleşmesi için kısıtlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="6e059-705">because the execution of `X`'s initializer and `Y`'s initializer could occur in either order; they are only constrained to occur before the references to those fields.</span></span> <span data-ttu-id="6e059-706">Ancak, örnekte:</span><span class="sxs-lookup"><span data-stu-id="6e059-706">However, in the example:</span></span>
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
<span data-ttu-id="6e059-707">Çıkış olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="6e059-707">the output must be:</span></span>
```
Init B
Init A
1 1
```
<span data-ttu-id="6e059-708">çünkü zaman statik oluşturucular yürütmek için kuralları (sınıfında tanımlandığı gibi [statik oluşturucular](classes.md#static-constructors)) sağlayan `B`'s statik Oluşturucusu (ve bu nedenle `B`'s statik alan başlatıcıları) önce çalıştırmanızgerekir`A`'s statik oluşturucusu ve alan başlatıcıları.</span><span class="sxs-lookup"><span data-stu-id="6e059-708">because the rules for when static constructors execute (as defined in [Static constructors](classes.md#static-constructors)) provide that `B`'s static constructor (and hence `B`'s static field initializers) must run before `A`'s static constructor and field initializers.</span></span>

#### <a name="instance-field-initialization"></a><span data-ttu-id="6e059-709">Örnek alanı başlatma</span><span class="sxs-lookup"><span data-stu-id="6e059-709">Instance field initialization</span></span>

<span data-ttu-id="6e059-710">Bir sınıfın örnek alan değişken başlatıcıları bir dizi hemen sonra Anonime örnek oluşturucuları herhangi birine çalıştırılan atamaları karşılık gelir ([Oluşturucu başlatıcıları](classes.md#constructor-initializers)) söz konusu sınıfın.</span><span class="sxs-lookup"><span data-stu-id="6e059-710">The instance field variable initializers of a class correspond to a sequence of assignments that are executed immediately upon entry to any one of the instance constructors ([Constructor initializers](classes.md#constructor-initializers)) of that class.</span></span> <span data-ttu-id="6e059-711">Değişken başlatıcılar, sınıf bildirimi içinde göründükleri metinsel sırayla yürütülür.</span><span class="sxs-lookup"><span data-stu-id="6e059-711">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="6e059-712">Sınıf örneği oluşturma ve başlatma işlemi daha ayrıntılı açıklanır [örnek oluşturucular](classes.md#instance-constructors).</span><span class="sxs-lookup"><span data-stu-id="6e059-712">The class instance creation and initialization process is described further in [Instance constructors](classes.md#instance-constructors).</span></span>

<span data-ttu-id="6e059-713">Değişken başlatıcı bir örnek alanıyla için oluşturulan örnek başvuramaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-713">A variable initializer for an instance field cannot reference the instance being created.</span></span> <span data-ttu-id="6e059-714">Bu nedenle, başvuru için bir derleme zamanı hatası olduğu `this` bir değişken başlatıcısında olarak olan bir derleme zamanı hatası için herhangi bir örnek üyesi ile başvurmak değişken başlatıcı bir *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="6e059-714">Thus, it is a compile-time error to reference `this` in a variable initializer, as it is a compile-time error for a variable initializer to reference any instance member through a *simple_name*.</span></span> <span data-ttu-id="6e059-715">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-715">In the example</span></span>
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
<span data-ttu-id="6e059-716">değişken için Başlatıcı `y` oluşturulan örnek üyesi başvuruda bulunduğundan bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="6e059-716">the variable initializer for `y` results in a compile-time error because it references a member of the instance being created.</span></span>

## <a name="methods"></a><span data-ttu-id="6e059-717">Yöntemler</span><span class="sxs-lookup"><span data-stu-id="6e059-717">Methods</span></span>

<span data-ttu-id="6e059-718">A ***yöntemi*** hesaplama veya bir nesne veya sınıf tarafından gerçekleştirilen eylem uygulayan bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-718">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="6e059-719">Yöntemleri kullanarak bildirilir *method_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="6e059-719">Methods are declared using *method_declaration*s:</span></span>

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

<span data-ttu-id="6e059-720">A *method_declaration* bir dizi içerebilir *öznitelikleri* ([öznitelikleri](attributes.md)) ve dört erişim değiştiricilere ait geçerli bir bileşim ([erişim değiştiricileri ](classes.md#access-modifiers)), `new` ([yeni değiştiricisini](classes.md#the-new-modifier)), `static` ([statik ve örnek yöntemler](classes.md#static-and-instance-methods)), `virtual` ([sanalyöntemleri](classes.md#virtual-methods)), `override` ([Geçersiz kılma yöntemleri](classes.md#override-methods)), `sealed` ([yöntemleri korumalı](classes.md#sealed-methods)), `abstract` ([soyut yöntemler](classes.md#abstract-methods)), ve `extern` ([Dış yöntemleri](classes.md#external-methods)) değiştiriciler.</span><span class="sxs-lookup"><span data-stu-id="6e059-720">A *method_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="6e059-721">Aşağıdakilerin tümü doğru olduğunda bir bildirimi, değiştiricilere ait geçerli bir bileşim sahiptir:</span><span class="sxs-lookup"><span data-stu-id="6e059-721">A declaration has a valid combination of modifiers if all of the following are true:</span></span>

*  <span data-ttu-id="6e059-722">Erişim değiştiricileri geçerli bir bileşimin bildirimi içerir ([erişim değiştiricilerine](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="6e059-722">The declaration includes a valid combination of access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span>
*  <span data-ttu-id="6e059-723">Bildirimi, birden çok kez aynı değiştiricisi içermez.</span><span class="sxs-lookup"><span data-stu-id="6e059-723">The declaration does not include the same modifier multiple times.</span></span>
*  <span data-ttu-id="6e059-724">Aşağıdaki değiştiriciler en fazla bir bildirim içerir: `static`, `virtual`, ve `override`.</span><span class="sxs-lookup"><span data-stu-id="6e059-724">The declaration includes at most one of the following modifiers: `static`, `virtual`, and `override`.</span></span>
*  <span data-ttu-id="6e059-725">Aşağıdaki değiştiriciler en fazla bir bildirim içerir: `new` ve `override`.</span><span class="sxs-lookup"><span data-stu-id="6e059-725">The declaration includes at most one of the following modifiers: `new` and `override`.</span></span>
*  <span data-ttu-id="6e059-726">Bildirimi içeriyorsa `abstract` değiştiricisi, sonra bildirimi içermez herhangi birini aşağıdaki değiştiriciler: `static`, `virtual`, `sealed` veya `extern`.</span><span class="sxs-lookup"><span data-stu-id="6e059-726">If the declaration includes the `abstract` modifier, then the declaration does not include any of the following modifiers: `static`, `virtual`, `sealed` or `extern`.</span></span>
*  <span data-ttu-id="6e059-727">Bildirimi içeriyorsa `private` değiştiricisi, sonra bildirimi içermez herhangi birini aşağıdaki değiştiriciler: `virtual`, `override`, veya `abstract`.</span><span class="sxs-lookup"><span data-stu-id="6e059-727">If the declaration includes the `private` modifier, then the declaration does not include any of the following modifiers: `virtual`, `override`, or `abstract`.</span></span>
*  <span data-ttu-id="6e059-728">Bildirimi içeriyorsa `sealed` değiştiricisi, sonra bildirimi de içerir `override` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-728">If the declaration includes the `sealed` modifier, then the declaration also includes the `override` modifier.</span></span>
*  <span data-ttu-id="6e059-729">Bildirimi içeriyorsa `partial` değiştiricisi, ardından içermez herhangi birini aşağıdaki değiştiriciler: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override` , `abstract`, veya `extern`.</span><span class="sxs-lookup"><span data-stu-id="6e059-729">If the declaration includes the `partial` modifier, then it does not include any of the following modifiers: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`, or `extern`.</span></span>

<span data-ttu-id="6e059-730">Bir yöntemin `async` değiştiricisi bir zaman uyumsuz işlev ve açıklanan kurallara [zaman uyumsuz işlevleri](classes.md#async-functions).</span><span class="sxs-lookup"><span data-stu-id="6e059-730">A method that has the `async` modifier is an async function and follows the rules described in [Async functions](classes.md#async-functions).</span></span>

<span data-ttu-id="6e059-731">*Döndür_tür* bir metodun bildirimi hesaplanır ve yöntem tarafından döndürülen değerin türü belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-731">The *return_type* of a method declaration specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="6e059-732">*Döndür_tür* olduğu `void` yöntemi bir değer döndürmezse.</span><span class="sxs-lookup"><span data-stu-id="6e059-732">The *return_type* is `void` if the method does not return a value.</span></span> <span data-ttu-id="6e059-733">Bildirimi içeriyorsa `partial` değiştiricisi, sonra da dönüş türü olmalıdır `void`.</span><span class="sxs-lookup"><span data-stu-id="6e059-733">If the declaration includes the `partial` modifier, then the return type must be `void`.</span></span>

<span data-ttu-id="6e059-734">*Member_name* yöntem adını belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-734">The *member_name* specifies the name of the method.</span></span> <span data-ttu-id="6e059-735">Açık arabirim üyesi uygulaması yöntemi olmadığı sürece ([açık arabirim üyesi uygulamalarını](interfaces.md#explicit-interface-member-implementations)), *member_name* olan yalnızca bir *tanımlayıcı*.</span><span class="sxs-lookup"><span data-stu-id="6e059-735">Unless the method is an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="6e059-736">Bir açık arabirim üyesi uygulaması *member_name* oluşan bir *INTERFACE_TYPE* arkasından bir "`.`" ve *tanımlayıcı*.</span><span class="sxs-lookup"><span data-stu-id="6e059-736">For an explicit interface member implementation, the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="6e059-737">İsteğe bağlı *type_parameter_list* yönteminin tür parametreleri belirtir ([tür parametrelerindeki](classes.md#type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="6e059-737">The optional *type_parameter_list* specifies the type parameters of the method ([Type parameters](classes.md#type-parameters)).</span></span> <span data-ttu-id="6e059-738">Varsa bir *type_parameter_list* yöntemi belirtilen bir ***genel yöntem***.</span><span class="sxs-lookup"><span data-stu-id="6e059-738">If a *type_parameter_list* is specified the method is a ***generic method***.</span></span> <span data-ttu-id="6e059-739">Yöntemi varsa bir `extern` değiştiricisi, bir *type_parameter_list* belirtilemez.</span><span class="sxs-lookup"><span data-stu-id="6e059-739">If the method has an `extern` modifier, a *type_parameter_list* cannot be specified.</span></span>

<span data-ttu-id="6e059-740">İsteğe bağlı *formal_parameter_list* yönteminin parametreleri belirtir ([yöntem parametreleri](classes.md#method-parameters)).</span><span class="sxs-lookup"><span data-stu-id="6e059-740">The optional *formal_parameter_list* specifies the parameters of the method ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="6e059-741">İsteğe bağlı *type_parameter_constraints_clause*s belirtin tek tek tür parametrelerindeki kısıtlamalar ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) ve varsa belirtilebilir bir *type_parameter_ Liste* da sağlanır ve yöntemi olmayan bir `override` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-741">The optional *type_parameter_constraints_clause*s specify constraints on individual type parameters ([Type parameter constraints](classes.md#type-parameter-constraints)) and may only be specified if a *type_parameter_list* is also supplied, and the method does not have an `override` modifier.</span></span>

<span data-ttu-id="6e059-742">*Döndür_tür* ve her başvurulan tür *formal_parameter_list* bir yöntemi en az yöntemi olarak olarak erişilebilir olmalıdır ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6e059-742">The *return_type* and each of the types referenced in the *formal_parameter_list* of a method must be at least as accessible as the method itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6e059-743">*Method_body* ya da bir noktalı virgüldür bir ***deyim gövdesi*** veya ***ifade gövdesi***.</span><span class="sxs-lookup"><span data-stu-id="6e059-743">The *method_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="6e059-744">Deyim gövdesi oluşan bir *blok*, yöntem çağrıldığında çalıştırılacak deyimleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-744">A statement body consists of a *block*, which specifies the statements to execute when the method is invoked.</span></span> <span data-ttu-id="6e059-745">İfade gövdesi oluşan `=>` arkasından bir *ifade* ve noktalı virgül ve tek bir ifade yöntemi çağrıldığında gerçekleştirilecek gösterir.</span><span class="sxs-lookup"><span data-stu-id="6e059-745">An expression body consists of `=>` followed by an *expression* and a semicolon, and denotes a single expression to perform when the method is invoked.</span></span> 

<span data-ttu-id="6e059-746">İçin `abstract` ve `extern` yöntemleri *method_body* yalnızca noktalı virgül içerir.</span><span class="sxs-lookup"><span data-stu-id="6e059-746">For `abstract` and `extern` methods, the *method_body* consists simply of a semicolon.</span></span> <span data-ttu-id="6e059-747">İçin `partial` yöntemleri *method_body* ya da noktalı virgül, bir blok gövdesi veya bir ifade gövdesi oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-747">For `partial` methods the *method_body* may consist of either a semicolon, a block body or an expression body.</span></span> <span data-ttu-id="6e059-748">Diğer tüm yöntemler için *method_body* blok gövdesi ya da bir deyim gövdesi.</span><span class="sxs-lookup"><span data-stu-id="6e059-748">For all other methods, the *method_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="6e059-749">Varsa *method_body* bildirimi içermiyor sonra bir virgül, içerir `async` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-749">If the *method_body* consists of a semicolon, then the declaration may not include the `async` modifier.</span></span>

<span data-ttu-id="6e059-750">Ad, tür parametresi listesi ve biçimsel parametre listesi bir yöntemin imzası tanımlayın ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)) yöntem.</span><span class="sxs-lookup"><span data-stu-id="6e059-750">The name, the type parameter list and the formal parameter list of a method define the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the method.</span></span> <span data-ttu-id="6e059-751">Özellikle, yöntemin imzası, adı, tür parametreleri ve sayısı, değiştiriciler ve biçimsel parametrelerinin türleri sayısı oluşur.</span><span class="sxs-lookup"><span data-stu-id="6e059-751">Specifically, the signature of a method consists of its name, the number of type parameters and the number, modifiers, and types of its formal parameters.</span></span> <span data-ttu-id="6e059-752">Bu amaçlar için oluşan bir biçimsel parametre türü yöntemin herhangi bir tür parametre adına göre değil, ancak türü bağımsız değişken listesindeki sıralı konumuna yöntem tarafından tanımlanır. Dönüş türü, yönetim imzasının bir parçası değil veya tür parametreleri veya biçimsel parametre adları.</span><span class="sxs-lookup"><span data-stu-id="6e059-752">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.The return type is not part of a method's signature, nor are the names of the type parameters or the formal parameters.</span></span>

<span data-ttu-id="6e059-753">Bir yöntemin adını, diğer tüm olmayan-aynı sınıfta bildirilen yöntemlerin adlarından farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-753">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="6e059-754">Ayrıca, yöntemin imzası aynı sınıfta bildirilen tüm diğer yöntemler imzalarını farklı gerekir ve aynı sınıfta bildirilen iki yöntem sadece onun tarafından farklı imzaları olmayabilir `ref` ve `out`.</span><span class="sxs-lookup"><span data-stu-id="6e059-754">In addition, the signature of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>

<span data-ttu-id="6e059-755">Yöntemin *type_parameter*lar kapsamdadır *method_declaration*ve bu kapsamda boyunca form türleri için kullanılabilir *Döndür_tür*, *method_body*, ve *type_parameter_constraints_clause*s fakat *öznitelikleri*.</span><span class="sxs-lookup"><span data-stu-id="6e059-755">The method's *type_parameter*s are in scope throughout the *method_declaration*, and can be used to form types throughout that scope in *return_type*, *method_body*, and *type_parameter_constraints_clause*s but not in *attributes*.</span></span>

<span data-ttu-id="6e059-756">Tüm biçimsel parametreler ve tür parametreleri farklı adlara sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-756">All formal parameters and type parameters must have different names.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="6e059-757">Yöntem parametreleri</span><span class="sxs-lookup"><span data-stu-id="6e059-757">Method parameters</span></span>

<span data-ttu-id="6e059-758">Bir yöntemin parametre varsa, yöntemin tarafından bildirilir *formal_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="6e059-758">The parameters of a method, if any, are declared by the method's *formal_parameter_list*.</span></span>

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

<span data-ttu-id="6e059-759">Biçimsel parametre listesi hangi yalnızca son olabilir bir veya daha fazla virgülle ayrılmış parametrelerinin oluşan bir *parameter_array*.</span><span class="sxs-lookup"><span data-stu-id="6e059-759">The formal parameter list consists of one or more comma-separated parameters of which only the last may be a *parameter_array*.</span></span>

<span data-ttu-id="6e059-760">A *fixed_parameter* isteğe bağlı bir kümesinden oluşur *öznitelikleri* ([öznitelikleri](attributes.md)), isteğe bağlı `ref`, `out` veya `this` değiştiricisi, bir *türü*e *tanımlayıcı* ve isteğe bağlı *default_argument*.</span><span class="sxs-lookup"><span data-stu-id="6e059-760">A *fixed_parameter* consists of an optional set of *attributes* ([Attributes](attributes.md)), an optional `ref`, `out` or `this` modifier, a *type*, an *identifier* and an optional *default_argument*.</span></span> <span data-ttu-id="6e059-761">Her *fixed_parameter* verilen ada sahip belirtilen türde bir parametre bildirir.</span><span class="sxs-lookup"><span data-stu-id="6e059-761">Each *fixed_parameter* declares a parameter of the given type with the given name.</span></span> <span data-ttu-id="6e059-762">`this` Değiştirici yöntem bir genişletme yöntemi belirler ve yalnızca bir statik yöntem ilk parametrede yer izin verilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-762">The `this` modifier designates the method as an extension method and is only allowed on the first parameter of a static method.</span></span> <span data-ttu-id="6e059-763">Genişletme yöntemleri bölümünde daha ayrıntılı açıklanmıştır [genişletme yöntemleri](classes.md#extension-methods).</span><span class="sxs-lookup"><span data-stu-id="6e059-763">Extension methods are further described in [Extension methods](classes.md#extension-methods).</span></span>

<span data-ttu-id="6e059-764">A *fixed_parameter* ile bir *default_argument* olarak bilinen bir ***isteğe bağlı parametre***bilgileriyse bir *fixed_parameter* bir olmadan*default_argument* olduğu bir ***gerekli parametre***.</span><span class="sxs-lookup"><span data-stu-id="6e059-764">A *fixed_parameter* with a *default_argument* is known as an ***optional parameter***, whereas a *fixed_parameter* without a *default_argument* is a ***required parameter***.</span></span> <span data-ttu-id="6e059-765">Gerekli bir parametre, isteğe bağlı bir parametre sonra görünmeyebilir bir *formal_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="6e059-765">A required parameter may not appear after an optional parameter in a *formal_parameter_list*.</span></span>

<span data-ttu-id="6e059-766">A `ref` veya `out` parametresi olamaz bir *default_argument*.</span><span class="sxs-lookup"><span data-stu-id="6e059-766">A `ref` or `out` parameter cannot have a *default_argument*.</span></span> <span data-ttu-id="6e059-767">*İfade* içinde bir *default_argument* aşağıdakilerden biri olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="6e059-767">The *expression* in a *default_argument* must be one of the following:</span></span>

*  <span data-ttu-id="6e059-768">bir *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="6e059-768">a *constant_expression*</span></span>
*  <span data-ttu-id="6e059-769">bir ifade formun `new S()` burada `S` bir değer türü</span><span class="sxs-lookup"><span data-stu-id="6e059-769">an expression of the form `new S()` where `S` is a value type</span></span>
*  <span data-ttu-id="6e059-770">bir ifade formun `default(S)` burada `S` bir değer türü</span><span class="sxs-lookup"><span data-stu-id="6e059-770">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="6e059-771">*İfade* bir kimlik veya parametresinin türü için boş değer atanabilir dönüştürme tarafından örtük olarak dönüştürülebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-771">The *expression* must be implicitly convertible by an identity or nullable conversion to the type of the parameter.</span></span>

<span data-ttu-id="6e059-772">İsteğe bağlı parametreler uygulayan bir kısmi yöntem bildiriminde gerçekleşmesi durumunda ([kısmi yöntemler](classes.md#partial-methods)), üye açık arabirim uygulaması ([açık arabirim üyesi uygulamalarını](interfaces.md#explicit-interface-member-implementations)) veya bir tek parametre dizin oluşturucu bildirimi ([dizin oluşturucular](classes.md#indexers)) bu üyeleri hiçbir zaman atlanacak bağımsız değişkenler veren bir şekilde çağrılabilir olduğundan derleyici bir uyarı vermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-772">If optional parameters occur in an implementing partial method declaration ([Partial methods](classes.md#partial-methods)) , an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)) or in a single-parameter indexer declaration ([Indexers](classes.md#indexers)) the compiler should give a warning, since these members can never be invoked in a way that permits arguments to be omitted.</span></span>

<span data-ttu-id="6e059-773">A *parameter_array* isteğe bağlı bir kümesinden oluşur *öznitelikleri* ([öznitelikleri](attributes.md)), `params` değiştiricisi, bir *array_type*, ve bir *tanımlayıcı*.</span><span class="sxs-lookup"><span data-stu-id="6e059-773">A *parameter_array* consists of an optional set of *attributes* ([Attributes](attributes.md)), a `params` modifier, an *array_type*, and an *identifier*.</span></span> <span data-ttu-id="6e059-774">Bir parametre dizisi, belirtilen ada sahip belirtilen dizi türünde tek bir parametre bildirir.</span><span class="sxs-lookup"><span data-stu-id="6e059-774">A parameter array declares a single parameter of the given array type with the given name.</span></span> <span data-ttu-id="6e059-775">*Array_type* parametre dizisi tek boyutlu dizi türü olmalıdır ([dizisi, türlerini](arrays.md#array-types)).</span><span class="sxs-lookup"><span data-stu-id="6e059-775">The *array_type* of a parameter array must be a single-dimensional array type ([Array types](arrays.md#array-types)).</span></span> <span data-ttu-id="6e059-776">Bir yöntem çağırmayla ya da tek bir bağımsız değişken belirtilmesi için belirtilen dizi türünde bir parametre dizisi izin verir veya dizi öğesi türünün belirtilmesi için sıfır veya daha fazla bağımsız değişken verir.</span><span class="sxs-lookup"><span data-stu-id="6e059-776">In a method invocation, a parameter array permits either a single argument of the given array type to be specified, or it permits zero or more arguments of the array element type to be specified.</span></span> <span data-ttu-id="6e059-777">Parametre dizileri daha ayrıntılı açıklanır [parametre dizileri](classes.md#parameter-arrays).</span><span class="sxs-lookup"><span data-stu-id="6e059-777">Parameter arrays are described further in [Parameter arrays](classes.md#parameter-arrays).</span></span>

<span data-ttu-id="6e059-778">A *parameter_array* sonra isteğe bağlı bir parametre oluşabilir, ancak--atlama bağımsız değişken için bir varsayılan değere sahip olamaz bir *parameter_array* boş bir dizi oluşturma bunun yerine neden olur.</span><span class="sxs-lookup"><span data-stu-id="6e059-778">A *parameter_array* may occur after an optional parameter, but cannot have a default value -- the omission of arguments for a *parameter_array* would instead result in the creation of an empty array.</span></span>

<span data-ttu-id="6e059-779">Aşağıdaki örnekte, farklı türde parametreler gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="6e059-779">The following example illustrates different kinds of parameters:</span></span>
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

<span data-ttu-id="6e059-780">İçinde *formal_parameter_list* için `M`, `i` gerekli ref parametresi `d` gerekli değer parametresi `b`, `s`, `o` ve `t` İsteğe bağlı değeri parametreleri ve `a` bir parametre dizisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-780">In the *formal_parameter_list* for `M`, `i` is a required ref parameter, `d` is a required value parameter, `b`, `s`, `o` and `t` are optional value parameters and `a` is a parameter array.</span></span>

<span data-ttu-id="6e059-781">Yöntem bildiriminde parametre, tür parametreleri ve yerel değişkenler için ayrı bildirimi boşluk oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6e059-781">A method declaration creates a separate declaration space for parameters, type parameters and local variables.</span></span> <span data-ttu-id="6e059-782">Tür parametresi listesi ve yöntem biçimsel parametre listesi ve yerel değişken bildirimlerinde adları bu bildirim alanına tanıtılır *blok* yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6e059-782">Names are introduced into this declaration space by the type parameter list and the formal parameter list of the method and by local variable declarations in the *block* of the method.</span></span> <span data-ttu-id="6e059-783">Aynı ada sahip iki üyesi bir yöntem bildirim alanı için bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="6e059-783">It is an error for two members of a method declaration space to have the same name.</span></span> <span data-ttu-id="6e059-784">Yöntemi bildirimini alan ve yerel değişken bildiriminde alan aynı ada sahip öğeleri içeren bir iç içe geçmiş bir bildirim alanı için bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="6e059-784">It is an error for the method declaration space and the local variable declaration space of a nested declaration space to contain elements with the same name.</span></span>

<span data-ttu-id="6e059-785">Yöntem çağırma ([yöntem çağrıları](expressions.md#method-invocations)) bu çağrıya belirli bir kopyasını oluşturur, biçimsel parametreler ve yöntemin yerel değişkenleri ve çağırma bağımsız değişken listesi değerlerini veya değişken başvuruları atar. biçimsel parametre yeni oluşturulmuş.</span><span class="sxs-lookup"><span data-stu-id="6e059-785">A method invocation ([Method invocations](expressions.md#method-invocations)) creates a copy, specific to that invocation, of the formal parameters and local variables of the method, and the argument list of the invocation assigns values or variable references to the newly created formal parameters.</span></span> <span data-ttu-id="6e059-786">İçinde *blok* yöntemi, biçimsel parametre kendi tanımlayıcıları tarafından başvurulabilir *simple_name* ifadeleri ([basit adları](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="6e059-786">Within the *block* of a method, formal parameters can be referenced by their identifiers in *simple_name* expressions ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="6e059-787">Biçimsel parametre dört çeşit vardır:</span><span class="sxs-lookup"><span data-stu-id="6e059-787">There are four kinds of formal parameters:</span></span>

*  <span data-ttu-id="6e059-788">Tüm değiştiricilere bildirilen değer parametreleri.</span><span class="sxs-lookup"><span data-stu-id="6e059-788">Value parameters, which are declared without any modifiers.</span></span>
*  <span data-ttu-id="6e059-789">Başvuru ile bildirilen parametreleri `ref` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-789">Reference parameters, which are declared with the `ref` modifier.</span></span>
*  <span data-ttu-id="6e059-790">İle bildirilen çıkış parametreleri `out` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-790">Output parameters, which are declared with the `out` modifier.</span></span>
*  <span data-ttu-id="6e059-791">İle bildirilen parametre dizileri `params` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-791">Parameter arrays, which are declared with the `params` modifier.</span></span>

<span data-ttu-id="6e059-792">Bölümünde anlatıldığı gibi [imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading), `ref` ve `out` değiştiriciler yöntemin imzası bir parçasıdır ancak `params` değiştiricisi değil.</span><span class="sxs-lookup"><span data-stu-id="6e059-792">As described in [Signatures and overloading](basic-concepts.md#signatures-and-overloading), the `ref` and `out` modifiers are part of a method's signature, but the `params` modifier is not.</span></span>

#### <a name="value-parameters"></a><span data-ttu-id="6e059-793">Değer parametreleri</span><span class="sxs-lookup"><span data-stu-id="6e059-793">Value parameters</span></span>

<span data-ttu-id="6e059-794">Hiçbir değiştiricileri ile bildirilen bir parametre değeri bir parametredir.</span><span class="sxs-lookup"><span data-stu-id="6e059-794">A parameter declared with no modifiers is a value parameter.</span></span> <span data-ttu-id="6e059-795">Bir değer parametresini karşılık gelen bir yerel değişkene yöntemi çağrısını sağlanan karşılık gelen bağımsız değişken bunun başlangıçtaki değerini alır.</span><span class="sxs-lookup"><span data-stu-id="6e059-795">A value parameter corresponds to a local variable that gets its initial value from the corresponding argument supplied in the method invocation.</span></span>

<span data-ttu-id="6e059-796">Biçimsel parametre bir değer parametresini olduğunda, karşılık gelen bağımsız değişken bir yöntem çağrısı örtük olarak dönüştürülebilir bir ifade olmalıdır ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) biçimsel parametre türü.</span><span class="sxs-lookup"><span data-stu-id="6e059-796">When a formal parameter is a value parameter, the corresponding argument in a method invocation must be an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the formal parameter type.</span></span>

<span data-ttu-id="6e059-797">Bir yöntem için bir değer parametresini yeni değerler atamanıza izin verilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-797">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="6e059-798">Bu tür atamalar yalnızca değer parametresi tarafından temsil edilen yerel depolama konumunu etkileyen — bunlar yöntem çağırma verilen gerçek bağımsız değişken üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="6e059-798">Such assignments only affect the local storage location represented by the value parameter—they have no effect on the actual argument given in the method invocation.</span></span>

#### <a name="reference-parameters"></a><span data-ttu-id="6e059-799">Başvuru parametreleri</span><span class="sxs-lookup"><span data-stu-id="6e059-799">Reference parameters</span></span>

<span data-ttu-id="6e059-800">Bir parametre ile bildirilen bir `ref` bir başvuru parametresi bir değiştiricidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-800">A parameter declared with a `ref` modifier is a reference parameter.</span></span> <span data-ttu-id="6e059-801">Bir değer parametresini, yeni bir depolama konumuna bir başvuru parametresi oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-801">Unlike a value parameter, a reference parameter does not create a new storage location.</span></span> <span data-ttu-id="6e059-802">Bunun yerine, bir başvuru parametresi aynı depolama konumu olarak yöntem çağırma bağımsız değişken olarak verilen değişkeni temsil eder.</span><span class="sxs-lookup"><span data-stu-id="6e059-802">Instead, a reference parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="6e059-803">Biçimsel parametre bir başvuru parametresi olduğunda, karşılık gelen bağımsız değişken bir yöntem çağrısı anahtar sözcüğünü oluşmalıdır `ref` arkasından bir *variable_reference* ([kesin kurallar belirlemek için belirli atama onayına](variables.md#precise-rules-for-determining-definite-assignment)) biçimsel parametresi ile aynı türde.</span><span class="sxs-lookup"><span data-stu-id="6e059-803">When a formal parameter is a reference parameter, the corresponding argument in a method invocation must consist of the keyword `ref` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="6e059-804">Bir başvuru parametresi geçirilebilir önce bir değişkeni kesinlikle atanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-804">A variable must be definitely assigned before it can be passed as a reference parameter.</span></span>

<span data-ttu-id="6e059-805">Bir yöntem içinde bir başvuru parametresi her zaman kesin atanmış olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-805">Within a method, a reference parameter is always considered definitely assigned.</span></span>

<span data-ttu-id="6e059-806">Bir yöntem bir yineleyici bildirilen ([yineleyiciler](classes.md#iterators)) başvuru parametrelere sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-806">A method declared as an iterator ([Iterators](classes.md#iterators)) cannot have reference parameters.</span></span>

<span data-ttu-id="6e059-807">Örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-807">The example</span></span>
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
<span data-ttu-id="6e059-808">çıktıyı üretir</span><span class="sxs-lookup"><span data-stu-id="6e059-808">produces the output</span></span>
```
i = 2, j = 1
```

<span data-ttu-id="6e059-809">Çalıştırılışı için `Swap` içinde `Main`, `x` temsil `i` ve `y` temsil `j`.</span><span class="sxs-lookup"><span data-stu-id="6e059-809">For the invocation of `Swap` in `Main`, `x` represents `i` and `y` represents `j`.</span></span> <span data-ttu-id="6e059-810">Bu nedenle, çağırma değerlerini değiştirme etkisi vardır `i` ve `j`.</span><span class="sxs-lookup"><span data-stu-id="6e059-810">Thus, the invocation has the effect of swapping the values of `i` and `j`.</span></span>

<span data-ttu-id="6e059-811">Bir yöntemde, aynı depolama konumunu göstermek birden çok ad mümkündür başvuru parametreleri alır.</span><span class="sxs-lookup"><span data-stu-id="6e059-811">In a method that takes reference parameters it is possible for multiple names to represent the same storage location.</span></span> <span data-ttu-id="6e059-812">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-812">In the example</span></span>
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
<span data-ttu-id="6e059-813">davranışınızın `F` içinde `G` bir başvuru geçirir `s` hem `a` ve `b`.</span><span class="sxs-lookup"><span data-stu-id="6e059-813">the invocation of `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="6e059-814">Bu nedenle, bu çağrı, adları için `s`, `a`, ve `b` tümü aynı depolama konumuna bakın ve örnek alanı tüm üç atamaları değiştirmek `s`.</span><span class="sxs-lookup"><span data-stu-id="6e059-814">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance field `s`.</span></span>

#### <a name="output-parameters"></a><span data-ttu-id="6e059-815">Çıktı parametreleri</span><span class="sxs-lookup"><span data-stu-id="6e059-815">Output parameters</span></span>

<span data-ttu-id="6e059-816">Bir parametre ile bildirilen bir `out` değiştiricidir çıkış parametresi.</span><span class="sxs-lookup"><span data-stu-id="6e059-816">A parameter declared with an `out` modifier is an output parameter.</span></span> <span data-ttu-id="6e059-817">Bir başvuru parametresi için benzer bir çıktı parametresi yeni bir depolama konumuna oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-817">Similar to a reference parameter, an output parameter does not create a new storage location.</span></span> <span data-ttu-id="6e059-818">Bunun yerine, bir çıkış parametresi aynı depolama konumu olarak yöntem çağırma bağımsız değişken olarak verilen değişkeni temsil eder.</span><span class="sxs-lookup"><span data-stu-id="6e059-818">Instead, an output parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="6e059-819">Bir biçimsel parametresi bir output parametresi olduğunda, karşılık gelen bağımsız değişken bir yöntem çağrısı anahtar sözcüğünü oluşmalıdır `out` arkasından bir *variable_reference* ([kesin kurallar belirlemek için belirli atama onayına](variables.md#precise-rules-for-determining-definite-assignment)) biçimsel parametresi ile aynı türde.</span><span class="sxs-lookup"><span data-stu-id="6e059-819">When a formal parameter is an output parameter, the corresponding argument in a method invocation must consist of the keyword `out` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="6e059-820">Bir değişken kesinlikle bir output parametresi olarak geçirildi, ancak burada bir değişken bir output parametresi olarak geçirildi bir çağrısını değişkeni kesinlikle atanan kabul önce atanmamış.</span><span class="sxs-lookup"><span data-stu-id="6e059-820">A variable need not be definitely assigned before it can be passed as an output parameter, but following an invocation where a variable was passed as an output parameter, the variable is considered definitely assigned.</span></span>

<span data-ttu-id="6e059-821">Bir yöntem içinde olduğu gibi yerel bir değişken, bir çıkış parametresi ilk olarak kabul edilir atanmamış ve değeri kullanılmadan önce kesinlikle atanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-821">Within a method, just like a local variable, an output parameter is initially considered unassigned and must be definitely assigned before its value is used.</span></span>

<span data-ttu-id="6e059-822">Yöntem döndürmeden önce her bir yöntemin çıkış parametresi kesinlikle atanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-822">Every output parameter of a method must be definitely assigned before the method returns.</span></span>

<span data-ttu-id="6e059-823">Bir yöntem kısmi bir yöntem bildirilen ([kısmi yöntemler](classes.md#partial-methods)) ya da bir yineleyici ([yineleyiciler](classes.md#iterators)) çıktı parametreleri olamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-823">A method declared as a partial method ([Partial methods](classes.md#partial-methods)) or an iterator ([Iterators](classes.md#iterators)) cannot have output parameters.</span></span>

<span data-ttu-id="6e059-824">Çıkış parametreleri, genellikle birden çok değer oluşturan yöntemleri kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-824">Output parameters are typically used in methods that produce multiple return values.</span></span> <span data-ttu-id="6e059-825">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6e059-825">For example:</span></span>
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

<span data-ttu-id="6e059-826">Örnek çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="6e059-826">The example produces the output:</span></span>
```
c:\Windows\System\
hello.txt
```

<span data-ttu-id="6e059-827">Unutmayın `dir` ve `name` için geçirilmeden önce değişkenleri atanmamış olabilir `SplitPath`, ve bunlar çağrıyı izleyen kesinlikle atanmış olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-827">Note that the `dir` and `name` variables can be unassigned before they are passed to `SplitPath`, and that they are considered definitely assigned following the call.</span></span>

#### <a name="parameter-arrays"></a><span data-ttu-id="6e059-828">Parametre dizileri</span><span class="sxs-lookup"><span data-stu-id="6e059-828">Parameter arrays</span></span>

<span data-ttu-id="6e059-829">Bir parametre ile bildirilen bir `params` değiştiricidir bir parametre dizisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-829">A parameter declared with a `params` modifier is a parameter array.</span></span> <span data-ttu-id="6e059-830">Bir parametre dizisi biçimsel parametre listesi içeriyorsa, listesindeki son parametre olmalıdır ve bir tek boyutlu dizi türünde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-830">If a formal parameter list includes a parameter array, it must be the last parameter in the list and it must be of a single-dimensional array type.</span></span> <span data-ttu-id="6e059-831">Örneğin, türleri `string[]` ve `string[][]` parametre dizi türü, ancak türü kullanılabilir `string[,]` gerçekleştiremezsiniz.</span><span class="sxs-lookup"><span data-stu-id="6e059-831">For example, the types `string[]` and `string[][]` can be used as the type of a parameter array, but the type `string[,]` can not.</span></span> <span data-ttu-id="6e059-832">Birleştirmek mümkün değildir `params` değiştiriciler değiştiriciyle `ref` ve `out`.</span><span class="sxs-lookup"><span data-stu-id="6e059-832">It is not possible to combine the `params` modifier with the modifiers `ref` and `out`.</span></span>

<span data-ttu-id="6e059-833">Bir parametre dizisi bağımsız değişken bir yöntem çağrısı iki yoldan biriyle belirtilmesi izin verir:</span><span class="sxs-lookup"><span data-stu-id="6e059-833">A parameter array permits arguments to be specified in one of two ways in a method invocation:</span></span>

*  <span data-ttu-id="6e059-834">Bir parametre dizisi örtük olarak dönüştürülebilir tek bir ifade olabilir için verilen bağımsız değişken ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) parametre dizi türü.</span><span class="sxs-lookup"><span data-stu-id="6e059-834">The argument given for a parameter array can be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="6e059-835">Bu durumda, parametre dizisi, tam olarak bir değer parametresini gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="6e059-835">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="6e059-836">Alternatif olarak, çağırma örtük olarak dönüştürülebilir bir ifade bulunduğu her bağımsız değişken, bir parametre dizisi için sıfır veya daha fazla bağımsız değişken belirtebilirsiniz ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) parametre dizinin öğe türü.</span><span class="sxs-lookup"><span data-stu-id="6e059-836">Alternatively, the invocation can specify zero or more arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="6e059-837">Bu durumda, çağırma bağımsız değişken sayısı için karşılık gelen uzunluğu parametre dizi türü örneği oluşturur, belirtilen bağımsız değişken değerleri dizi örneği başlatır ve yeni oluşturulan bir dizi örneği fiili kullanır bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="6e059-837">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="6e059-838">Değişken sayıda bağımsız değişken bir çağrıda izin vererek dışında bir parametre dizisi için bir değer parametresini tam olarak eşdeğerdir ([değer parametreleri](classes.md#value-parameters)) aynı türde.</span><span class="sxs-lookup"><span data-stu-id="6e059-838">Except for allowing a variable number of arguments in an invocation, a parameter array is precisely equivalent to a value parameter ([Value parameters](classes.md#value-parameters)) of the same type.</span></span>

<span data-ttu-id="6e059-839">Örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-839">The example</span></span>
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
<span data-ttu-id="6e059-840">çıktıyı üretir</span><span class="sxs-lookup"><span data-stu-id="6e059-840">produces the output</span></span>
```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="6e059-841">İlk çağrısıysa `F` yalnızca dizi geçirir `a` değer parametre olarak.</span><span class="sxs-lookup"><span data-stu-id="6e059-841">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="6e059-842">İkinci çağırmayı `F` dört öğeli otomatik olarak oluşturur `int[]` verilen öğe değerlerini ve dizi örneği bir değer parametresi olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="6e059-842">The second invocation of `F` automatically creates a four-element `int[]` with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="6e059-843">Benzer şekilde, üçüncü çağırmayı `F` sıfır öğeli oluşturur `int[]` ve bu örneğe bir değer parametre olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="6e059-843">Likewise, the third invocation of `F` creates a zero-element `int[]` and passes that instance as a value parameter.</span></span> <span data-ttu-id="6e059-844">İkinci ve üçüncü çağrıları, yazma tam olarak eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="6e059-844">The second and third invocations are precisely equivalent to writing:</span></span>
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

<span data-ttu-id="6e059-845">Aşırı yükleme çözümlemesi yaparken, bir parametre dizisi olan bir yöntem normal biçimde veya genişletilmiş biçimde uygun olabilir ([geçerli işlev üyesi](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="6e059-845">When performing overload resolution, a method with a parameter array may be applicable either in its normal form or in its expanded form ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="6e059-846">Bir yöntemin genişletilmiş form, yalnızca normal yöntem biçiminin geçerli değilse ve yalnızca genişletilmiş formun aynı imzaya sahip geçerli bir yöntem zaten aynı türden bildirilmedi varsa kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-846">The expanded form of a method is available only if the normal form of the method is not applicable and only if an applicable method with the same signature as the expanded form is not already declared in the same type.</span></span>

<span data-ttu-id="6e059-847">Örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-847">The example</span></span>
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
<span data-ttu-id="6e059-848">çıktıyı üretir</span><span class="sxs-lookup"><span data-stu-id="6e059-848">produces the output</span></span>
```
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

<span data-ttu-id="6e059-849">Örnekte, iki parametre dizisi yöntemiyle olası genişletilmiş biçimlerinin zaten sınıfında normal yöntemleri olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-849">In the example, two of the possible expanded forms of the method with a parameter array are already included in the class as regular methods.</span></span> <span data-ttu-id="6e059-850">Bu Genişletilmiş formlar bu nedenle aşırı yükleme çözümlemesi yaparken dikkate alınmaz ve birinci ve üçüncü yöntem çağrıları, böylece normal yöntemleri seçin.</span><span class="sxs-lookup"><span data-stu-id="6e059-850">These expanded forms are therefore not considered when performing overload resolution, and the first and third method invocations thus select the regular methods.</span></span> <span data-ttu-id="6e059-851">Bir yöntem parametre dizisi olan bir sınıfı bildirir, bazı normal yöntemler olarak genişletilmiş forms de sık karşılaşılan bir durum değil.</span><span class="sxs-lookup"><span data-stu-id="6e059-851">When a class declares a method with a parameter array, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="6e059-852">Bir dizinin ayırma önlemek için mümkün olacak şekilde yaparak bir yöntemin parametre dizisi olan Genişletilmiş bir form oluşur örneği çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-852">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a parameter array is invoked.</span></span>

<span data-ttu-id="6e059-853">Bir parametre dizi türü olduğunda `object[]`, tüketim form için tek bir yöntem normal biçiminin arasındaki olası bir belirsizliğe ortaya `object` parametresi.</span><span class="sxs-lookup"><span data-stu-id="6e059-853">When the type of a parameter array is `object[]`, a potential ambiguity arises between the normal form of the method and the expended form for a single `object` parameter.</span></span> <span data-ttu-id="6e059-854">Belirsizlik nedeni bir `object[]` kendisi türüne örtük olarak dönüştürülebilir değildir `object`.</span><span class="sxs-lookup"><span data-stu-id="6e059-854">The reason for the ambiguity is that an `object[]` is itself implicitly convertible to type `object`.</span></span> <span data-ttu-id="6e059-855">Gerekirse bir yayın ekleyerek çözülebilir beri belirsizlik hiç sorun değil, ancak sunar.</span><span class="sxs-lookup"><span data-stu-id="6e059-855">The ambiguity presents no problem, however, since it can be resolved by inserting a cast if needed.</span></span>

<span data-ttu-id="6e059-856">Örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-856">The example</span></span>
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
<span data-ttu-id="6e059-857">çıktıyı üretir</span><span class="sxs-lookup"><span data-stu-id="6e059-857">produces the output</span></span>
```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="6e059-858">İlk ve son çağrılarını içinde `F`, normal şeklinde `F` örtük bir dönüştürme için parametre türü bağımsız değişken türünden varolduğundan geçerlidir (her ikisi de türlerinin `object[]`).</span><span class="sxs-lookup"><span data-stu-id="6e059-858">In the first and last invocations of `F`, the normal form of `F` is applicable because an implicit conversion exists from the argument type to the parameter type (both are of type `object[]`).</span></span> <span data-ttu-id="6e059-859">Bu nedenle, aşırı yükleme çözünürlüğü normal şeklinde seçer `F`, ve normal değer parametre olarak geçirilen bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="6e059-859">Thus, overload resolution selects the normal form of `F`, and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="6e059-860">İkinci ve üçüncü çağrılarını normal şeklinde içinde `F` örtük dönüştürme için parametre türü bağımsız değişken türü olduğundan geçerli değildir (tür `object` türüne örtük olarak dönüştürülemez `object[]`).</span><span class="sxs-lookup"><span data-stu-id="6e059-860">In the second and third invocations, the normal form of `F` is not applicable because no implicit conversion exists from the argument type to the parameter type (type `object` cannot be implicitly converted to type `object[]`).</span></span> <span data-ttu-id="6e059-861">Ancak, genişletilmiş biçiminin `F` aşırı yükleme çözünürlüğü tarafından Seçili olmaması için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-861">However, the expanded form of `F` is applicable, so it is selected by overload resolution.</span></span> <span data-ttu-id="6e059-862">Sonuç olarak, bir öğe `object[]` çağrı tarafından oluşturulur ve belirtilen bağımsız değişken değeri ile başlatılmış dizinin tek öğesi (kendisi bir başvurudur bir `object[]`).</span><span class="sxs-lookup"><span data-stu-id="6e059-862">As a result, a one-element `object[]` is created by the invocation, and the single element of the array is initialized with the given argument value (which itself is a reference to an `object[]`).</span></span>

### <a name="static-and-instance-methods"></a><span data-ttu-id="6e059-863">Statik ve örnek yöntemleri</span><span class="sxs-lookup"><span data-stu-id="6e059-863">Static and instance methods</span></span>

<span data-ttu-id="6e059-864">Ne zaman bir yöntem bildiriminde içeren bir `static` değiştiricisi, yöntemi statik yöntem olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-864">When a method declaration includes a `static` modifier, that method is said to be a static method.</span></span> <span data-ttu-id="6e059-865">Hiçbir `static` değiştirici mevcut olduğundan, yöntemi bir örnek yöntemi olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-865">When no `static` modifier is present, the method is said to be an instance method.</span></span>

<span data-ttu-id="6e059-866">Statik bir yöntemi de belirli bir örneği üzerinde çalışmaz ve başvurmak için bir derleme zamanı hata `this` statik yöntemde.</span><span class="sxs-lookup"><span data-stu-id="6e059-866">A static method does not operate on a specific instance, and it is a compile-time error to refer to `this` in a static method.</span></span>

<span data-ttu-id="6e059-867">Bir örnek yöntemi, bir sınıfın belirli bir örneği üzerinde çalışır ve bu örnek olarak erişilebilen `this` ([bu erişim](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="6e059-867">An instance method operates on a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="6e059-868">Ne zaman bir yöntem başvuruluyor bir *member_access* ([üye erişimi](expressions.md#member-access)) biçiminde `E.M`, `M` statik bir yöntem `E` içerenbirtürbelirtmekgerekir`M`ve eğer `M` bir örnek yöntemi olduğundan `E` türünü içeren bir örneğini belirtmek gerekir `M`.</span><span class="sxs-lookup"><span data-stu-id="6e059-868">When a method is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static method, `E` must denote a type containing `M`, and if `M` is an instance method, `E` must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="6e059-869">Statik arasındaki farklar ve örnek üyeleri ele alınmıştır daha ayrıntılı olarak [statik ve örnek üyeleri](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="6e059-869">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-methods"></a><span data-ttu-id="6e059-870">Sanal yöntemler</span><span class="sxs-lookup"><span data-stu-id="6e059-870">Virtual methods</span></span>

<span data-ttu-id="6e059-871">Ne zaman bir örnek yöntemi bildirimi içeren bir `virtual` değiştiricisi, yöntemi sanal bir yöntem olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-871">When an instance method declaration includes a `virtual` modifier, that method is said to be a virtual method.</span></span> <span data-ttu-id="6e059-872">Hiçbir `virtual` değiştirici mevcut olduğundan, yöntemi sanal olmayan bir yöntem olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-872">When no `virtual` modifier is present, the method is said to be a non-virtual method.</span></span>

<span data-ttu-id="6e059-873">Sanal olmayan bir yöntemin uygulanmasını sabit: yöntemi sınıfının bir örneği üzerinde çağrıldıktan uygulaması aynı olup, bildirilen veya türetilmiş bir sınıfın bir örneği.</span><span class="sxs-lookup"><span data-stu-id="6e059-873">The implementation of a non-virtual method is invariant: The implementation is the same whether the method is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="6e059-874">Buna karşılık, bir sanal yöntemin uygulanmasını türetilmiş sınıflar tarafından yenisiyle değiştirilmiş.</span><span class="sxs-lookup"><span data-stu-id="6e059-874">In contrast, the implementation of a virtual method can be superseded by derived classes.</span></span> <span data-ttu-id="6e059-875">Devralınan bir sanal yöntemin uygulanmasını yerini alma işlemi olarak bilinir ***geçersiz kılma*** bu yöntem ([geçersiz kılma yöntemleri](classes.md#override-methods)).</span><span class="sxs-lookup"><span data-stu-id="6e059-875">The process of superseding the implementation of an inherited virtual method is known as ***overriding*** that method ([Override methods](classes.md#override-methods)).</span></span>

<span data-ttu-id="6e059-876">Bir sanal yöntem çağrısı ***çalışma zamanı tür*** bu çağırma aldığı örneğini yerde çağrılacak gerçek yöntem uygulaması belirler.</span><span class="sxs-lookup"><span data-stu-id="6e059-876">In a virtual method invocation, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="6e059-877">Bir sanal olmayan bir yöntem çağrısı ***derleme zamanı tür*** belirleyici faktör örneğidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-877">In a non-virtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span> <span data-ttu-id="6e059-878">Kesin terimleri, zaman adlı yöntemi `N` bir bağımsız değişken listesi ile çağrılan `A` ile bir derleme zamanı tür örneği üzerinde `C` ve çalışma zamanı tür `R` (burada `R` ya da `C` veya türetilmiş bir sınıf gelen `C`), çağırma şu şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="6e059-878">In precise terms, when a method named `N` is invoked with an argument list `A` on an instance with a compile-time type `C` and a run-time type `R` (where `R` is either `C` or a class derived from `C`), the invocation is processed as follows:</span></span>

*  <span data-ttu-id="6e059-879">İlk olarak, aşırı yükleme çözünürlüğü uygulanan `C`, `N`, ve `A`, belirli bir yöntemi seçmek için `M` bildirilen ve tarafından devralınan yöntemleri kümesinden `C`.</span><span class="sxs-lookup"><span data-stu-id="6e059-879">First, overload resolution is applied to `C`, `N`, and `A`, to select a specific method `M` from the set of methods declared in and inherited by `C`.</span></span> <span data-ttu-id="6e059-880">Bu açıklanan [yöntem çağrıları](expressions.md#method-invocations).</span><span class="sxs-lookup"><span data-stu-id="6e059-880">This is described in [Method invocations](expressions.md#method-invocations).</span></span>
*  <span data-ttu-id="6e059-881">Ardından, eğer `M` sanal olmayan bir yöntem `M` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-881">Then, if `M` is a non-virtual method, `M` is invoked.</span></span>
*  <span data-ttu-id="6e059-882">Aksi takdirde, `M` sanal bir yöntem ve en çok türetilen uygulamasını `M` ilişkilendirilebilmesi için `R` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-882">Otherwise, `M` is a virtual method, and the most derived implementation of `M` with respect to `R` is invoked.</span></span>

<span data-ttu-id="6e059-883">Bildirilen veya devralınan bir sınıf tarafından her sanal yöntem için mevcut bir ***uygulama'en çok türetilene*** o sınıfa göre yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6e059-883">For every virtual method declared in or inherited by a class, there exists a ***most derived implementation*** of the method with respect to that class.</span></span> <span data-ttu-id="6e059-884">Sanal bir yöntemin en türetilen uygulamaya `M` bir sınıfa göre `R` şu şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="6e059-884">The most derived implementation of a virtual method `M` with respect to a class `R` is determined as follows:</span></span>

*  <span data-ttu-id="6e059-885">Varsa `R` Tanıtımı içeren `virtual` bildirimi `M`, sonra da bu en çok türetilen uygulamasıdır `M`.</span><span class="sxs-lookup"><span data-stu-id="6e059-885">If `R` contains the introducing `virtual` declaration of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="6e059-886">Aksi takdirde `R` içeren bir `override` , `M`, sonra da bu en çok türetilen uygulamasıdır `M`.</span><span class="sxs-lookup"><span data-stu-id="6e059-886">Otherwise, if `R` contains an `override` of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="6e059-887">Aksi takdirde, en iyi uygulaması türetilen `M` ilişkilendirilebilmesi için `R` en çok türetilen uygulamasını aynı `M` doğrudan temel sınıfını göre `R`.</span><span class="sxs-lookup"><span data-stu-id="6e059-887">Otherwise, the most derived implementation of `M` with respect to `R` is the same as the most derived implementation of `M` with respect to the direct base class of `R`.</span></span>

<span data-ttu-id="6e059-888">Aşağıdaki örnekte, sanal ve sanal olmayan yöntemleri arasındaki farklar gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="6e059-888">The following example illustrates the differences between virtual and non-virtual methods:</span></span>
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

<span data-ttu-id="6e059-889">Örnekte, `A` sanal olmayan bir yöntem sunar `F` ve sanal bir yöntem `G`.</span><span class="sxs-lookup"><span data-stu-id="6e059-889">In the example, `A` introduces a non-virtual method `F` and a virtual method `G`.</span></span> <span data-ttu-id="6e059-890">Sınıf `B` sanal olmayan bir yöntem sunar `F`, bu nedenle Devralınanı gizleme `F`ve ayrıca devralınan yöntemi geçersiz kılar `G`.</span><span class="sxs-lookup"><span data-stu-id="6e059-890">The class `B` introduces a new non-virtual method `F`, thus hiding the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="6e059-891">Örnek çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="6e059-891">The example produces the output:</span></span>
```
A.F
B.F
B.G
B.G
```

<span data-ttu-id="6e059-892">Dikkat deyim `a.G()` çağırır `B.G`değil `A.G`.</span><span class="sxs-lookup"><span data-stu-id="6e059-892">Notice that the statement `a.G()` invokes `B.G`, not `A.G`.</span></span> <span data-ttu-id="6e059-893">Çalışma zamanı örneğini yazın olmasıdır (olduğu `B`), derleme zamanı türü örneğinin değil (olduğu `A`), çağrılacak gerçek yöntem uygulaması belirler.</span><span class="sxs-lookup"><span data-stu-id="6e059-893">This is because the run-time type of the instance (which is `B`), not the compile-time type of the instance (which is `A`), determines the actual method implementation to invoke.</span></span>

<span data-ttu-id="6e059-894">Devralınan yöntemleri gizlemek için yöntemleri izin verildiğinden, aynı imzaya sahip birden çok sanal yöntemler içeren sınıf için mümkündür.</span><span class="sxs-lookup"><span data-stu-id="6e059-894">Because methods are allowed to hide inherited methods, it is possible for a class to contain several virtual methods with the same signature.</span></span> <span data-ttu-id="6e059-895">En çok türetilen metot dışındaki tüm gizlenmiş olduğundan bu bir belirsizlik sorunu sunmaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-895">This does not present an ambiguity problem, since all but the most derived method are hidden.</span></span> <span data-ttu-id="6e059-896">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-896">In the example</span></span>
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
<span data-ttu-id="6e059-897">`C` ve `D` sınıflar aynı imzaya sahip iki sanal yöntem bulunur: tarafından sunulan bir `A` tarafından sunulan bir `C`.</span><span class="sxs-lookup"><span data-stu-id="6e059-897">the `C` and `D` classes contain two virtual methods with the same signature: The one introduced by `A` and the one introduced by `C`.</span></span> <span data-ttu-id="6e059-898">Yöntemi tarafından tanıtılan `C` devralınan yöntemini gizliyor `A`.</span><span class="sxs-lookup"><span data-stu-id="6e059-898">The method introduced by `C` hides the method inherited from `A`.</span></span> <span data-ttu-id="6e059-899">Bu nedenle, geçersiz kılma bildiriminde `D` tarafından tanıtılan yöntemini geçersiz kılan `C`, ve mümkün değil `D` tarafından tanıtılan yöntemi geçersiz kılmak için `A`.</span><span class="sxs-lookup"><span data-stu-id="6e059-899">Thus, the override declaration in `D` overrides the method introduced by `C`, and it is not possible for `D` to override the method introduced by `A`.</span></span> <span data-ttu-id="6e059-900">Örnek çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="6e059-900">The example produces the output:</span></span>
```
B.F
B.F
D.F
D.F
```

<span data-ttu-id="6e059-901">Örneği erişerek gizli sanal yöntemini çağırmak mümkün olmadığını unutmayın `D` daha az türetilmiş tür, yöntem gizlenmediğinden.</span><span class="sxs-lookup"><span data-stu-id="6e059-901">Note that it is possible to invoke the hidden virtual method by accessing an instance of `D` through a less derived type in which the method is not hidden.</span></span>

### <a name="override-methods"></a><span data-ttu-id="6e059-902">Geçersiz kılma yöntemleri</span><span class="sxs-lookup"><span data-stu-id="6e059-902">Override methods</span></span>

<span data-ttu-id="6e059-903">Ne zaman bir örnek yöntemi bildirimi içeren bir `override` değiştiricisi, yöntem olduğu söylenir bir ***yöntemi yok sayın***.</span><span class="sxs-lookup"><span data-stu-id="6e059-903">When an instance method declaration includes an `override` modifier, the method is said to be an ***override method***.</span></span> <span data-ttu-id="6e059-904">Bir geçersiz kılma yöntemi, aynı imzaya sahip devralınan sanal yöntemi geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="6e059-904">An override method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="6e059-905">Bir geçersiz kılma yöntemi bildirimini sanal yöntem bildiriminde bir yöntem sunar ancak bu yöntem yeni bir uygulamasını sağlayarak mevcut devralınan sanal yöntemi uzmanlaşmış.</span><span class="sxs-lookup"><span data-stu-id="6e059-905">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="6e059-906">Yöntem tarafından geçersiz kılınan bir `override` bildirimi olarak bilinen ***temel yöntemi geçersiz kılmışsa***.</span><span class="sxs-lookup"><span data-stu-id="6e059-906">The method overridden by an `override` declaration is known as the ***overridden base method***.</span></span> <span data-ttu-id="6e059-907">Bir geçersiz kılma yöntemi için `M` bir sınıfta bildirilen `C`, geçersiz kılınan taban yöntemi her temel sınıf türünü incelenerek belirlenir `C`doğrudan temel sınıf türü ile başlayan `C` ve her birinin art arda gelen bir verilen temel sınıf türünde en az bir erişilebilir yöntemi, bulunan kadar doğrudan temel sınıf türünde aynı imzaya `M` tür bağımsız değişkenlerini değiştirme sonra.</span><span class="sxs-lookup"><span data-stu-id="6e059-907">For an override method `M` declared in a class `C`, the overridden base method is determined by examining each base class type of `C`, starting with the direct base class type of `C` and continuing with each successive direct base class type, until in a given base class type at least one accessible method is located which has the same signature as `M` after substitution of type arguments.</span></span> <span data-ttu-id="6e059-908">Geçersiz kılınan taban yöntemini bulma amacıyla, bir yöntem ise erişilebilir kabul `public`, bu ise `protected`, bu ise `protected internal`, ya da ise `internal` ve aynı programın içinde bildirilen `C`.</span><span class="sxs-lookup"><span data-stu-id="6e059-908">For the purposes of locating the overridden base method, a method is considered accessible if it is `public`, if it is `protected`, if it is `protected internal`, or if it is `internal` and declared in the same program as `C`.</span></span>

<span data-ttu-id="6e059-909">Aşağıdakilerin tümü için bir geçersiz kılma bildirimi doğru olduğu sürece, bir derleme zamanı hatası oluşur:</span><span class="sxs-lookup"><span data-stu-id="6e059-909">A compile-time error occurs unless all of the following are true for an override declaration:</span></span>

*  <span data-ttu-id="6e059-910">Yukarıda açıklanan şekilde geçersiz kılınan bir taban yöntemi bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-910">An overridden base method can be located as described above.</span></span>
*  <span data-ttu-id="6e059-911">Tam olarak bir tür geçersiz kılınan taban yöntemi yoktur.</span><span class="sxs-lookup"><span data-stu-id="6e059-911">There is exactly one such overridden base method.</span></span> <span data-ttu-id="6e059-912">Yalnızca temel sınıf türünü nerede tür bağımsız değişkenleri değiştirme iki yöntem imzası aynı yapar oluşturulan tür ise, bu kısıtlama etkiye sahiptir.</span><span class="sxs-lookup"><span data-stu-id="6e059-912">This restriction has effect only if the base class type is a constructed type where the substitution of type arguments makes the signature of two methods the same.</span></span>
*  <span data-ttu-id="6e059-913">Geçersiz kılınan taban yöntemi sanal, Özet ya da yöntemi yok sayın.</span><span class="sxs-lookup"><span data-stu-id="6e059-913">The overridden base method is a virtual, abstract, or override method.</span></span> <span data-ttu-id="6e059-914">Diğer bir deyişle, geçersiz kılınan taban yöntemi sanal olmayan ya da statik olamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-914">In other words, the overridden base method cannot be static or non-virtual.</span></span>
*  <span data-ttu-id="6e059-915">Geçersiz kılınan taban yöntemi, korumalı bir yöntem değildir.</span><span class="sxs-lookup"><span data-stu-id="6e059-915">The overridden base method is not a sealed method.</span></span>
*  <span data-ttu-id="6e059-916">Aşırı yükleme yöntemi ve geçersiz kılınan taban yöntemi aynı dönüş türüne sahip.</span><span class="sxs-lookup"><span data-stu-id="6e059-916">The override method and the overridden base method have the same return type.</span></span>
*  <span data-ttu-id="6e059-917">Aynı bildirilen erişilebilirliği ve geçersiz kılma bildirimi geçersiz kılınan taban yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="6e059-917">The override declaration and the overridden base method have the same declared accessibility.</span></span> <span data-ttu-id="6e059-918">Diğer bir deyişle, bir geçersiz kılma bildirimi sanal yöntemin erişilebilirliği değiştiremezsiniz.</span><span class="sxs-lookup"><span data-stu-id="6e059-918">In other words, an override declaration cannot change the accessibility of the virtual method.</span></span> <span data-ttu-id="6e059-919">Ancak, geçersiz kılınan taban yöntemi dahili korunuyorsa ve daha sonra geçersiz kılma yöntemin aşırı yükleme yöntemini içeren derlemenin bildirilen olandan farklı bir derlemede bildirilmiş erişilebilirlik korunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-919">However, if the overridden base method is protected internal and it is declared in a different assembly than the assembly containing the override method then the override method's declared accessibility must be protected.</span></span>
*  <span data-ttu-id="6e059-920">Geçersiz kılma bildirimi tür parametresi kısıtlamaları tümceleri belirtmiyor.</span><span class="sxs-lookup"><span data-stu-id="6e059-920">The override declaration does not specify type-parameter-constraints-clauses.</span></span> <span data-ttu-id="6e059-921">Bunun yerine kısıtlamaları geçersiz kılınan taban yöntemden devralınır.</span><span class="sxs-lookup"><span data-stu-id="6e059-921">Instead the constraints are inherited from the overridden base method.</span></span> <span data-ttu-id="6e059-922">Devralınan kısıtlamasında tür bağımsız değişkeni tarafından geçersiz kılınan yönteminin tür parametreleri olan kısıtlamaları değiştirilebilir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="6e059-922">Note that constraints that are type parameters in the overridden method may be replaced by type arguments in the inherited constraint.</span></span> <span data-ttu-id="6e059-923">Bu açıkça, değer türleri veya sealed türlerde gibi belirtildiğinde yasal olmayan kısıtlamaları neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-923">This can lead to constraints that are not legal when explicitly specified, such as value types or sealed types.</span></span>

<span data-ttu-id="6e059-924">Aşağıdaki örnek, geçersiz kılma kuralları için Genel sınıflar nasıl çalıştığını gösterir:</span><span class="sxs-lookup"><span data-stu-id="6e059-924">The following example demonstrates how the overriding rules work for generic classes:</span></span>
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

<span data-ttu-id="6e059-925">Geçersiz kılınan taban yöntemini kullanarak bir geçersiz kılma bildirimi erişip bir *base_access* ([temel erişim](expressions.md#base-access)).</span><span class="sxs-lookup"><span data-stu-id="6e059-925">An override declaration can access the overridden base method using a *base_access* ([Base access](expressions.md#base-access)).</span></span> <span data-ttu-id="6e059-926">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-926">In the example</span></span>
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
<span data-ttu-id="6e059-927">`base.PrintFields()` çağrısını `B` çağırır `PrintFields` yöntemi içinde bildirilen `A`.</span><span class="sxs-lookup"><span data-stu-id="6e059-927">the `base.PrintFields()` invocation in `B` invokes the `PrintFields` method declared in `A`.</span></span> <span data-ttu-id="6e059-928">A *base_access* sanal çağırma mekanizması devre dışı bırakır ve yalnızca temel yöntemi sanal olmayan bir yöntem olarak değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="6e059-928">A *base_access* disables the virtual invocation mechanism and simply treats the base method as a non-virtual method.</span></span> <span data-ttu-id="6e059-929">Çağırma gerekiyordu `B` edilmiş yazılan `((A)this).PrintFields()`, yinelemeli olarak olduğu çağırma `PrintFields` yöntemi içinde bildirilen `B`, bildirilen bir `A`, bu yana `PrintFields` sanal çalışmazamanıtürü`((A)this)` olduğu `B`.</span><span class="sxs-lookup"><span data-stu-id="6e059-929">Had the invocation in `B` been written `((A)this).PrintFields()`, it would recursively invoke the `PrintFields` method declared in `B`, not the one declared in `A`, since `PrintFields` is virtual and the run-time type of `((A)this)` is `B`.</span></span>

<span data-ttu-id="6e059-930">Ekleyerek yalnızca bir `override` değiştiricisi can bir yöntem başka bir yöntemi geçersiz kılın.</span><span class="sxs-lookup"><span data-stu-id="6e059-930">Only by including an `override` modifier can a method override another method.</span></span> <span data-ttu-id="6e059-931">Diğer durumlarda, bir devralınan yönteme aynı imzaya sahip bir yöntemi yalnızca devralınan yöntemi gizler.</span><span class="sxs-lookup"><span data-stu-id="6e059-931">In all other cases, a method with the same signature as an inherited method simply hides the inherited method.</span></span> <span data-ttu-id="6e059-932">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-932">In the example</span></span>
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
<span data-ttu-id="6e059-933">`F` yönteminde `B` içermemesi bir `override` değiştiricisi ve bu nedenle geçersiz `F` yönteminde `A`.</span><span class="sxs-lookup"><span data-stu-id="6e059-933">the `F` method in `B` does not include an `override` modifier and therefore does not override the `F` method in `A`.</span></span> <span data-ttu-id="6e059-934">Bunun yerine, `F` yönteminde `B` yöntemi gizler `A`, ve bir uyarı bildirimi içermez çünkü bildirilen bir `new` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-934">Rather, the `F` method in `B` hides the method in `A`, and a warning is reported because the declaration does not include a `new` modifier.</span></span>

<span data-ttu-id="6e059-935">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-935">In the example</span></span>
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
<span data-ttu-id="6e059-936">`F` yönteminde `B` sanal gizler `F` yöntemi öğesinden devralınan `A`.</span><span class="sxs-lookup"><span data-stu-id="6e059-936">the `F` method in `B` hides the virtual `F` method inherited from `A`.</span></span> <span data-ttu-id="6e059-937">Bu yana yeni `F` içinde `B` özel erişimi olduğunu, yalnızca sınıf gövdesinin kapsamının `B` ve kapsamaz `C`.</span><span class="sxs-lookup"><span data-stu-id="6e059-937">Since the new `F` in `B` has private access, its scope only includes the class body of `B` and does not extend to `C`.</span></span> <span data-ttu-id="6e059-938">Bu nedenle, bildirimi `F` içinde `C` geçersiz kılmak için izin verilen `F` devralınan `A`.</span><span class="sxs-lookup"><span data-stu-id="6e059-938">Therefore, the declaration of `F` in `C` is permitted to override the `F` inherited from `A`.</span></span>

### <a name="sealed-methods"></a><span data-ttu-id="6e059-939">Sealed yöntemleri</span><span class="sxs-lookup"><span data-stu-id="6e059-939">Sealed methods</span></span>

<span data-ttu-id="6e059-940">Ne zaman bir örnek yöntemi bildirimi içeren bir `sealed` değiştiricisi, yöntemi olarak kabul edilir bir ***yöntem korumalı***.</span><span class="sxs-lookup"><span data-stu-id="6e059-940">When an instance method declaration includes a `sealed` modifier, that method is said to be a ***sealed method***.</span></span> <span data-ttu-id="6e059-941">Bir örnek yöntem bildiriminde içeriyorsa `sealed` değiştiricisi, aynı zamanda içermelidir `override` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-941">If an instance method declaration includes the  `sealed` modifier, it must also include the `override` modifier.</span></span> <span data-ttu-id="6e059-942">Kullanım `sealed` değiştiricisi türetilmiş bir sınıf başka bir yöntemi geçersiz kılmasını önler.</span><span class="sxs-lookup"><span data-stu-id="6e059-942">Use of the `sealed` modifier prevents a derived class from further overriding the method.</span></span>

<span data-ttu-id="6e059-943">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-943">In the example</span></span>
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
<span data-ttu-id="6e059-944">sınıf `B` yöntemleri iki geçersiz kılma sağlar: bir `F` yöntemin `sealed` değiştiricisi ve `G` yok yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6e059-944">the class `B` provides two override methods: an `F` method that has the `sealed` modifier and a `G` method that does not.</span></span> <span data-ttu-id="6e059-945">`B`kullanıcının kullanın korumalı `modifier` engeller `C` daha fazla geçersiz kılmasını `F`.</span><span class="sxs-lookup"><span data-stu-id="6e059-945">`B`'s use of the sealed `modifier` prevents `C` from further overriding `F`.</span></span>

### <a name="abstract-methods"></a><span data-ttu-id="6e059-946">Soyut metotlar</span><span class="sxs-lookup"><span data-stu-id="6e059-946">Abstract methods</span></span>

<span data-ttu-id="6e059-947">Ne zaman bir örnek yöntemi bildirimi içeren bir `abstract` değiştiricisi, yöntemi olarak kabul edilir bir ***yöntemi soyut***.</span><span class="sxs-lookup"><span data-stu-id="6e059-947">When an instance method declaration includes an `abstract` modifier, that method is said to be an ***abstract method***.</span></span> <span data-ttu-id="6e059-948">Soyut Metoda örtük olarak da sanal bir yöntem olmasına karşın, değiştiricisine sahip olamaz `virtual`.</span><span class="sxs-lookup"><span data-stu-id="6e059-948">Although an abstract method is implicitly also a virtual method, it cannot have the modifier `virtual`.</span></span>

<span data-ttu-id="6e059-949">Bir soyut yöntem bildiriminde sanal bir yöntem sunar ancak bu yöntemin bir uygulaması, sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-949">An abstract method declaration introduces a new virtual method but does not provide an implementation of that method.</span></span> <span data-ttu-id="6e059-950">Bunun yerine, soyut olmayan türetilen sınıfların bu yöntemi geçersiz kılarak, kendi uygulama sağlamak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-950">Instead, non-abstract derived classes are required to provide their own implementation by overriding that method.</span></span> <span data-ttu-id="6e059-951">Bir soyut yönteminden hiçbir kullanımınız sağladığından *method_body* soyut bir yöntemi yalnızca noktalı virgül içerir.</span><span class="sxs-lookup"><span data-stu-id="6e059-951">Because an abstract method provides no actual implementation, the *method_body* of an abstract method simply consists of a semicolon.</span></span>

<span data-ttu-id="6e059-952">Soyut yöntem bildirimleri yalnızca soyut sınıfları izin ([soyut sınıflar](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="6e059-952">Abstract method declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="6e059-953">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-953">In the example</span></span>
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
<span data-ttu-id="6e059-954">`Shape` sınıfı soyut kavramı kendi boyama yapabileceğiniz bir geometrik şekil nesnesi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="6e059-954">the `Shape` class defines the abstract notion of a geometrical shape object that can paint itself.</span></span> <span data-ttu-id="6e059-955">`Paint` Yöntemdir soyut olduğu için anlamlı varsayılan uygulaması.</span><span class="sxs-lookup"><span data-stu-id="6e059-955">The `Paint` method is abstract because there is no meaningful default implementation.</span></span> <span data-ttu-id="6e059-956">`Ellipse` Ve `Box` sınıflar somut `Shape` uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="6e059-956">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="6e059-957">Bu sınıflar, soyut olmayan olduğundan, bunlar geçersiz kılmak için gerekli `Paint` yöntemi ve bir gerçek uygulanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="6e059-957">Because these classes are non-abstract, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="6e059-958">Bir derleme zamanı hata için bir *base_access* ([temel erişim](expressions.md#base-access)) bir soyut yönteminden başvurmak için.</span><span class="sxs-lookup"><span data-stu-id="6e059-958">It is a compile-time error for a *base_access* ([Base access](expressions.md#base-access)) to reference an abstract method.</span></span> <span data-ttu-id="6e059-959">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-959">In the example</span></span>
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
<span data-ttu-id="6e059-960">bir derleme zamanı hatası için bildirilen `base.F()` çağırma soyut Metoda başvurduğundan.</span><span class="sxs-lookup"><span data-stu-id="6e059-960">a compile-time error is reported for the `base.F()` invocation because it references an abstract method.</span></span>

<span data-ttu-id="6e059-961">Bir soyut yöntem bildiriminde sanal bir yöntemi geçersiz kılmak için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-961">An abstract method declaration is permitted to override a virtual method.</span></span> <span data-ttu-id="6e059-962">Bu yöntemin türetilmiş sınıflarda yeniden uygulanmasını zorlamak bir Özet sınıf sağlar ve yöntemin asıl uygulamasına kullanılamaz hale getirir.</span><span class="sxs-lookup"><span data-stu-id="6e059-962">This allows an abstract class to force re-implementation of the method in derived classes, and makes the original implementation of the method unavailable.</span></span> <span data-ttu-id="6e059-963">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-963">In the example</span></span>
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
<span data-ttu-id="6e059-964">sınıf `A` sanal yöntem, sınıf `B` bir soyut metot ve sınıf bu metodu geçersiz kılar `C` kendi uygulamasını sağlamak için soyut yöntemini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="6e059-964">class `A` declares a virtual method, class `B` overrides this method with an abstract method, and class `C` overrides the abstract method to provide its own implementation.</span></span>

### <a name="external-methods"></a><span data-ttu-id="6e059-965">Dış yöntemleri</span><span class="sxs-lookup"><span data-stu-id="6e059-965">External methods</span></span>

<span data-ttu-id="6e059-966">Ne zaman bir yöntem bildiriminde içeren bir `extern` değiştiricisi, yöntemi olarak kabul edilir bir ***dış yöntem***.</span><span class="sxs-lookup"><span data-stu-id="6e059-966">When a method declaration includes an `extern` modifier, that method is said to be an ***external method***.</span></span> <span data-ttu-id="6e059-967">Dış yöntem, genellikle C# dışında bir dil kullanarak harici olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="6e059-967">External methods are implemented externally, typically using a language other than C#.</span></span> <span data-ttu-id="6e059-968">Bir dış yöntem bildiriminde yok kullanımınız sağladığından *method_body* dış bir Metoda yalnızca noktalı virgül içerir.</span><span class="sxs-lookup"><span data-stu-id="6e059-968">Because an external method declaration provides no actual implementation, the *method_body* of an external method simply consists of a semicolon.</span></span> <span data-ttu-id="6e059-969">Dış yöntem genel olamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-969">An external method may not be generic.</span></span>

<span data-ttu-id="6e059-970">`extern` Değiştiricisi ile birlikte kullanılan genellikle bir `DllImport` özniteliği ([bileşenlerle, COM ve Win32 birlikte çalışması](attributes.md#interoperation-with-com-and-win32-components)), dış yöntemleri (dinamik bağlantı kitaplıklarını) DLL tarafından uygulanmak üzere izin verme.</span><span class="sxs-lookup"><span data-stu-id="6e059-970">The `extern` modifier is typically used in conjunction with a `DllImport` attribute ([Interoperation with COM and Win32 components](attributes.md#interoperation-with-com-and-win32-components)), allowing external methods to be implemented by DLLs (Dynamic Link Libraries).</span></span> <span data-ttu-id="6e059-971">Yürütme Ortamı ile dış yöntem uygulamaları sağlanabilir başka mekanizmalar destekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-971">The execution environment may support other mechanisms whereby implementations of external methods can be provided.</span></span>

<span data-ttu-id="6e059-972">Ne zaman dış bir Metoda içeren bir `DllImport` özniteliği, yöntem bildiriminde içermelidir ayrıca bir `static` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-972">When an external method includes a `DllImport` attribute, the method declaration must also include a `static` modifier.</span></span> <span data-ttu-id="6e059-973">Bu örnek kullanımını gösterir `extern` değiştiricisi ve `DllImport` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="6e059-973">This example demonstrates the use of the `extern` modifier and the `DllImport` attribute:</span></span>
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

### <a name="partial-methods-recap"></a><span data-ttu-id="6e059-974">Kısmi yöntemler (özeti)</span><span class="sxs-lookup"><span data-stu-id="6e059-974">Partial methods (recap)</span></span>

<span data-ttu-id="6e059-975">Ne zaman bir yöntem bildiriminde içeren bir `partial` değiştiricisi, yöntemi olarak kabul edilir bir ***kısmi yöntem***.</span><span class="sxs-lookup"><span data-stu-id="6e059-975">When a method declaration includes a `partial` modifier, that method is said to be a ***partial method***.</span></span> <span data-ttu-id="6e059-976">Kısmi yöntemler yalnızca kısmi türlerinden bir üyesi olarak bildirilebilir ([kısmi türlerinden](classes.md#partial-types)) ve kısıtlama sayısı tabidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-976">Partial methods can only be declared as members of partial types ([Partial types](classes.md#partial-types)), and are subject to a number of restrictions.</span></span> <span data-ttu-id="6e059-977">Kısmi yöntemler bölümünde daha ayrıntılı açıklanmıştır [kısmi yöntemler](classes.md#partial-methods).</span><span class="sxs-lookup"><span data-stu-id="6e059-977">Partial methods are further described in [Partial methods](classes.md#partial-methods).</span></span>

### <a name="extension-methods"></a><span data-ttu-id="6e059-978">Genişletme yöntemleri</span><span class="sxs-lookup"><span data-stu-id="6e059-978">Extension methods</span></span>

<span data-ttu-id="6e059-979">Yönteminin ilk parametresinin zaman içerir `this` değiştiricisi, yöntemi olarak kabul edilir bir ***genişletme yöntemi***.</span><span class="sxs-lookup"><span data-stu-id="6e059-979">When the first parameter of a method includes the `this` modifier, that method is said to be an ***extension method***.</span></span> <span data-ttu-id="6e059-980">Genişletme yöntemleri, genel olmayan, iç içe olmayan statik sınıflarda yalnızca bildirilebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-980">Extension methods can only be declared in non-generic, non-nested static classes.</span></span> <span data-ttu-id="6e059-981">Genişletme yönteminin ilk parametresi dışında hiçbir değiştiriciler olabilir `this`, ve parametre türü bir işaretçi türü olamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-981">The first parameter of an extension method can have no modifiers other than `this`, and the parameter type cannot be a pointer type.</span></span>

<span data-ttu-id="6e059-982">Aşağıdaki iki uzantı yöntemleri bildiren bir statik sınıfının bir örneğidir:</span><span class="sxs-lookup"><span data-stu-id="6e059-982">The following is an example of a static class that declares two extension methods:</span></span>
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

<span data-ttu-id="6e059-983">Bir genişletme yöntemi bir normal statik yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-983">An extension method is a regular static method.</span></span> <span data-ttu-id="6e059-984">Kapsayan statik sınıfıyla kapsamında olduğunda, ayrıca, bir genişletme yöntemi örnek yöntem çağırma söz dizimi kullanılarak çağrılabilir ([uzantısı yöntem çağrıları](expressions.md#extension-method-invocations)), ilk bağımsız değişken olarak alıcı ifade kullanarak.</span><span class="sxs-lookup"><span data-stu-id="6e059-984">In addition, where its enclosing static class is in scope, an extension method can be invoked using instance method invocation syntax ([Extension method invocations](expressions.md#extension-method-invocations)), using the receiver expression as the first argument.</span></span>

<span data-ttu-id="6e059-985">Aşağıdaki program, yukarıda belirtilen genişletme yöntemleri kullanır:</span><span class="sxs-lookup"><span data-stu-id="6e059-985">The following program uses the extension methods declared above:</span></span>
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

<span data-ttu-id="6e059-986">`Slice` Yöntemi edinilebilir `string[]`ve `ToInt32` yöntemi edinilebilir `string`, genişletme yöntemleri bildirilmedi.</span><span class="sxs-lookup"><span data-stu-id="6e059-986">The `Slice` method is available on the `string[]`, and the `ToInt32` method is available on `string`, because they have been declared as extension methods.</span></span> <span data-ttu-id="6e059-987">Program anlamını aşağıdaki, kullanarak normal statik yöntem çağrılarını aynıdır:</span><span class="sxs-lookup"><span data-stu-id="6e059-987">The meaning of the program is the same as the following, using ordinary static method calls:</span></span>
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

### <a name="method-body"></a><span data-ttu-id="6e059-988">Yöntem gövdesi</span><span class="sxs-lookup"><span data-stu-id="6e059-988">Method body</span></span>

<span data-ttu-id="6e059-989">*Method_body* yöntemi bildirimi ya da bir blok gövdesi, bir deyim gövdesinin veya noktalı virgül oluşur.</span><span class="sxs-lookup"><span data-stu-id="6e059-989">The *method_body* of a method declaration consists of either a block body, an expression body or a semicolon.</span></span>

<span data-ttu-id="6e059-990">***Sonuç türü*** bir yöntemidir `void` dönüş türü ise `void`, veya zaman uyumsuz yöntem ise ve dönüş türü `System.Threading.Tasks.Task`.</span><span class="sxs-lookup"><span data-stu-id="6e059-990">The ***result type*** of a method is `void` if the return type is `void`, or if the method is async and the return type is `System.Threading.Tasks.Task`.</span></span> <span data-ttu-id="6e059-991">Aksi takdirde, sonuç türü olmayan zaman uyumsuz yöntemin dönüş türü ve zaman uyumsuz bir yöntemin dönüş türü ile sonuç türü olan `System.Threading.Tasks.Task<T>` olduğu `T`.</span><span class="sxs-lookup"><span data-stu-id="6e059-991">Otherwise, the result type of a non-async method is its return type, and the result type of an async method with return type `System.Threading.Tasks.Task<T>` is `T`.</span></span>

<span data-ttu-id="6e059-992">Bir yöntem olduğunda bir `void` sonuç türü ve bir blok gövdesi `return` deyimleri ([return deyiminin](statements.md#the-return-statement)) bloğunda bir ifadeyi belirtmek için izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="6e059-992">When a method has a `void` result type and a block body, `return` statements ([The return statement](statements.md#the-return-statement)) in the block are not permitted to specify an expression.</span></span> <span data-ttu-id="6e059-993">Void bir yöntem bloğu yürütülmesini normalde tamamlandıktan, (yani, Denetim Akışları yöntem gövdesinin sonuna kapalı), yöntemi yalnızca geçerli arayanına döner.</span><span class="sxs-lookup"><span data-stu-id="6e059-993">If execution of the block of a void method completes normally (that is, control flows off the end of the method body), that method simply returns to its current caller.</span></span>
    
<span data-ttu-id="6e059-994">Bir yöntem olduğunda bir `void` sonuç ve ifade bir deyim gövdesinin `E` olmalıdır bir *statement_expression*, ve gövde tam olarak bir form için blok gövdesi eşdeğerdir `{ E; }`.</span><span class="sxs-lookup"><span data-stu-id="6e059-994">When a method has a `void` result and an expression body, the expression `E` must be a *statement_expression*, and the body is exactly equivalent to a block body of the form `{ E; }`.</span></span>
    
<span data-ttu-id="6e059-995">Bir yöntem bir sonucu void olmayan türe sahip olduğunda ve bir blok gövdesi, her `return` deyim bloğu içindeki sonuç türüne örtük olarak dönüştürülebilir bir ifade belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-995">When a method has a non-void result type and a block body, each `return` statement in the block must specify an expression that is implicitly convertible to the result type.</span></span> <span data-ttu-id="6e059-996">Bir blok gövdesi değerini döndüren yöntemi uç noktası erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-996">The endpoint of a block body of a value-returning method must not be reachable.</span></span> <span data-ttu-id="6e059-997">Diğer bir deyişle, bir değer döndüren yöntemi ile bir blok gövdesi, denetim devre dışı yöntem gövdesinin sonuna akış izin verilmiyor.</span><span class="sxs-lookup"><span data-stu-id="6e059-997">In other words, in a value-returning method with a block body, control is not permitted to flow off the end of the method body.</span></span>
    
<span data-ttu-id="6e059-998">Bir yöntemi void olmayan sonuç türü ve bir deyim gövdesi olan, ifade için sonuç türü örtük olarak dönüştürülebilir olmalıdır ve gövde tam olarak bir form için blok gövdesi eşdeğerdir `{ return E; }`.</span><span class="sxs-lookup"><span data-stu-id="6e059-998">When a method has a non-void result type and an expression body, the expression must be implicitly convertible to the result type, and the body is exactly equivalent to a block body of the form `{ return E; }`.</span></span>
    
<span data-ttu-id="6e059-999">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-999">In the example</span></span>
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
<span data-ttu-id="6e059-1000">değer döndürme `F` yöntem, yöntem gövdesinin sonuna devre dışı akış denetimi için bir derleme zamanı hatası sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1000">the value-returning `F` method results in a compile-time error because control can flow off the end of the method body.</span></span> <span data-ttu-id="6e059-1001">`G` Ve `H` dönüş değeri belirten bir return deyimi içinde tüm olası yürütme yolları sonlandırmak için yöntemleri doğru.</span><span class="sxs-lookup"><span data-stu-id="6e059-1001">The `G` and `H` methods are correct because all possible execution paths end in a return statement that specifies a return value.</span></span> <span data-ttu-id="6e059-1002">`I` Yöntemi olduğundan doğru gövdesinde bir deyim bloğunu yalnızca tek bir dönüş deyimi içindeki ile eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1002">The `I` method is correct, because its body is equivalent to a statement block with just a single return statement in it.</span></span>

### <a name="method-overloading"></a><span data-ttu-id="6e059-1003">Yöntemi aşırı yüklemesi</span><span class="sxs-lookup"><span data-stu-id="6e059-1003">Method overloading</span></span>

<span data-ttu-id="6e059-1004">Yöntemi aşırı yükleme çözünürlüğü kuralları açıklanan [anlam çıkarma](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="6e059-1004">The method overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="properties"></a><span data-ttu-id="6e059-1005">Özellikler</span><span class="sxs-lookup"><span data-stu-id="6e059-1005">Properties</span></span>

<span data-ttu-id="6e059-1006">A ***özelliği*** bir nesnenin ya da bir sınıf bir karakteristik erişim sağlayan bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1006">A ***property*** is a member that provides access to a characteristic of an object or a class.</span></span> <span data-ttu-id="6e059-1007">Örnekler Özellikler penceresinde, bir müşterinin adını Başlığı yazı tipi boyutu bir dizenin uzunluğunu içeren ve benzeri.</span><span class="sxs-lookup"><span data-stu-id="6e059-1007">Examples of properties include the length of a string, the size of a font, the caption of a window, the name of a customer, and so on.</span></span> <span data-ttu-id="6e059-1008">Özellikleri alanlarının doğal bir uzantısı olan — hem de üyeleri ile ilişkilendirilmiş türler olarak adlandırılır ve alanlar ve Özellikler erişmek için sözdizimi aynıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1008">Properties are a natural extension of fields—both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="6e059-1009">Ancak, alanlar özellikleri depolama konumları belirtmek değil.</span><span class="sxs-lookup"><span data-stu-id="6e059-1009">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="6e059-1010">Bunun yerine, özelliklere sahip ***erişimcileri*** değerlerine okunabilir veya yazılabilir bırakıldığında yürütülecek deyimler belirtin.</span><span class="sxs-lookup"><span data-stu-id="6e059-1010">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span> <span data-ttu-id="6e059-1011">Özellikler, bu nedenle Eylemler okuma ve yazma nesnenin öznitelikleri ile ilişkilendirmek için bir mekanizma sağlar; Ayrıca, hesaplanan değer gibi özniteliklere izin.</span><span class="sxs-lookup"><span data-stu-id="6e059-1011">Properties thus provide a mechanism for associating actions with the reading and writing of an object's attributes; furthermore, they permit such attributes to be computed.</span></span>

<span data-ttu-id="6e059-1012">Özellikleri kullanarak bildirilir *property_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="6e059-1012">Properties are declared using *property_declaration*s:</span></span>

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

<span data-ttu-id="6e059-1013">A *property_declaration* bir dizi içerebilir *öznitelikleri* ([öznitelikleri](attributes.md)) ve dört erişim değiştiricilere ait geçerli bir bileşim ([erişim değiştiricileri ](classes.md#access-modifiers)), `new` ([yeni değiştiricisini](classes.md#the-new-modifier)), `static` ([statik ve örnek yöntemler](classes.md#static-and-instance-methods)), `virtual` ([sanalyöntemleri](classes.md#virtual-methods)), `override` ([Geçersiz kılma yöntemleri](classes.md#override-methods)), `sealed` ([yöntemleri korumalı](classes.md#sealed-methods)), `abstract` ([soyut yöntemler](classes.md#abstract-methods)), ve `extern` ([Dış yöntemleri](classes.md#external-methods)) değiştiriciler.</span><span class="sxs-lookup"><span data-stu-id="6e059-1013">A *property_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="6e059-1014">Özellik bildirimleri aynı kurallara göre yöntem bildirimleri tabidir ([yöntemleri](classes.md#methods)) değiştiriciler geçerli birleşimlerini onaylamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-1014">Property declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="6e059-1015">*Türü* özelliği bildirimi bildirim tarafından tanıtılan özelliğin türünü belirtir ve *member_name* özelliğin adını belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1015">The *type* of a property declaration specifies the type of the property introduced by the declaration, and the *member_name* specifies the name of the property.</span></span> <span data-ttu-id="6e059-1016">Açık arabirim üyesi uygulaması, özelliği olmadığı sürece *member_name* olan yalnızca bir *tanımlayıcı*.</span><span class="sxs-lookup"><span data-stu-id="6e059-1016">Unless the property is an explicit interface member implementation, the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="6e059-1017">Açık arabirim üyesi uygulaması için ([açık arabirim üyesi uygulamalarını](interfaces.md#explicit-interface-member-implementations)), *member_name* oluşan bir *INTERFACE_TYPE* arkasından bir " `.`"ve *tanımlayıcı*.</span><span class="sxs-lookup"><span data-stu-id="6e059-1017">For an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="6e059-1018">*Türü* bir özelliği en az özelliği olarak olarak erişilebilir olmalıdır ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1018">The *type* of a property must be at least as accessible as the property itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6e059-1019">A *property_body* herhangi birini içerebilir bir ***erişimcisinin gövdesi*** veya ***ifade gövdesi***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1019">A *property_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="6e059-1020">Bir erişimci gövdesindeki *accessor_declarations*, hangi alınmalıdır içinde "`{`"ve"`}`" belirteçleri, erişimci bildirir ([erişimcileri](classes.md#accessors)) özelliği.</span><span class="sxs-lookup"><span data-stu-id="6e059-1020">In an accessor body,  *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="6e059-1021">Erişimciler okuma ve yazma özelliği ilişkili yürütülebilir deyimleri belirtin.</span><span class="sxs-lookup"><span data-stu-id="6e059-1021">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="6e059-1022">Oluşan bir deyim gövdesinin `=>` arkasından bir *ifade* `E` ve noktalı virgül deyim gövdesi için tam olarak eşdeğerdir `{ get { return E; } }`ve bu nedenle yalnızca salt okuyucu belirtmek için kullanılabilir tek bir ifade tarafından sonucu alıcının burada verilen özellikleri.</span><span class="sxs-lookup"><span data-stu-id="6e059-1022">An expression body consisting of `=>` followed by an *expression* `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only properties where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="6e059-1023">A *property_initializer* otomatik olarak uygulanan bir özellik için yalnızca verilen ([Özellikleri'otomatik olarak uygulanan](classes.md#automatically-implemented-properties)) ve başlatma gibi temel alınan alanının neden olur özellikleri tarafından verilen değeri ile *ifade*.</span><span class="sxs-lookup"><span data-stu-id="6e059-1023">A *property_initializer* may only be given for an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)), and causes the initialization of the underlying field of such properties with the value given by the *expression*.</span></span>

<span data-ttu-id="6e059-1024">Bir özelliğe erişmek için söz dizimi aynı alan için olsa da, bir değişken olarak bir özellik sınıflandırılmaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-1024">Even though the syntax for accessing a property is the same as that for a field, a property is not classified as a variable.</span></span> <span data-ttu-id="6e059-1025">Bu nedenle, bir özellik olarak geçirmek mümkün değil bir `ref` veya `out` bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="6e059-1025">Thus, it is not possible to pass a property as a `ref` or `out` argument.</span></span>

<span data-ttu-id="6e059-1026">Ne zaman bir özellik bildirimi içeren bir `extern` değiştiricisi, özellik olduğu söylenir bir ***dış özellik***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1026">When a property declaration includes an `extern` modifier, the property is said to be an ***external property***.</span></span> <span data-ttu-id="6e059-1027">Hiçbir gerçek uygulama, her biri bir dış özellik bildiriminde sağladığından, *accessor_declarations* noktalı virgül içerir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1027">Because an external property declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

### <a name="static-and-instance-properties"></a><span data-ttu-id="6e059-1028">Statik ve örnek özellikleri</span><span class="sxs-lookup"><span data-stu-id="6e059-1028">Static and instance properties</span></span>

<span data-ttu-id="6e059-1029">Ne zaman bir özellik bildirimi içeren bir `static` değiştiricisi, özellik olduğu söylenir bir ***statik özellik***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1029">When a property declaration includes a `static` modifier, the property is said to be a ***static property***.</span></span> <span data-ttu-id="6e059-1030">Hiçbir `static` değiştiricisi, özellik olduğu söylenir bir ***örnek özellik***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1030">When no `static` modifier is present, the property is said to be an ***instance property***.</span></span>

<span data-ttu-id="6e059-1031">Statik bir özellik, belirli bir örneği ile ilişkili değil ve başvurmak için bir derleme zamanı hata `this` Erişimcilerde statik özellik.</span><span class="sxs-lookup"><span data-stu-id="6e059-1031">A static property is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static property.</span></span>

<span data-ttu-id="6e059-1032">Bir örnek özelliği belirli bir sınıf örneği ile ilişkili olduğunu ve bu örnek olarak erişilebilen `this` ([bu erişim](expressions.md#this-access)), bu özelliğin erişimcileri.</span><span class="sxs-lookup"><span data-stu-id="6e059-1032">An instance property is associated with a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that property.</span></span>

<span data-ttu-id="6e059-1033">Ne zaman bir özelliği başvuru olarak bir *member_access* ([üye erişimi](expressions.md#member-access)) biçiminde `E.M`, `M` statik bir özellik `E` içerenbirtürbelirtmekgerekir`M`ve eğer `M` bir örnek özelliği E türünü içeren bir örneğini belirtmek gerekir `M`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1033">When a property is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static property, `E` must denote a type containing `M`, and if `M` is an instance property, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="6e059-1034">Statik arasındaki farklar ve örnek üyeleri ele alınmıştır daha ayrıntılı olarak [statik ve örnek üyeleri](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="6e059-1034">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="accessors"></a><span data-ttu-id="6e059-1035">Erişimciler</span><span class="sxs-lookup"><span data-stu-id="6e059-1035">Accessors</span></span>

<span data-ttu-id="6e059-1036">*Accessor_declarations* okuma ve bu özelliğe yazma ile ilişkili yürütülebilir deyimlerin bir özelliğini belirtin.</span><span class="sxs-lookup"><span data-stu-id="6e059-1036">The *accessor_declarations* of a property specify the executable statements associated with reading and writing that property.</span></span>

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

<span data-ttu-id="6e059-1037">Erişimcisi bildirimlerine oluşan bir *get_accessor_declaration*, *set_accessor_declaration*, veya her ikisini de.</span><span class="sxs-lookup"><span data-stu-id="6e059-1037">The accessor declarations consist of a *get_accessor_declaration*, a *set_accessor_declaration*, or both.</span></span> <span data-ttu-id="6e059-1038">Belirteci her erişimci bildirimi oluşur `get` veya `set` bir isteğe bağlı olarak izlenen *accessor_modifier* ve *accessor_body*.</span><span class="sxs-lookup"><span data-stu-id="6e059-1038">Each accessor declaration consists of the token `get` or `set` followed by an optional *accessor_modifier* and an *accessor_body*.</span></span>

<span data-ttu-id="6e059-1039">Kullanımını *accessor_modifier*s tarafından aşağıdaki kısıtlamalara tabidir:</span><span class="sxs-lookup"><span data-stu-id="6e059-1039">The use of *accessor_modifier*s is governed by the following restrictions:</span></span>

*  <span data-ttu-id="6e059-1040">Bir *accessor_modifier* bir arabirim veya üye açık arabirim uygulaması kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-1040">An *accessor_modifier* may not be used in an interface or in an explicit interface member implementation.</span></span>
*  <span data-ttu-id="6e059-1041">Bir özellik veya hiçbir dizin oluşturucu için `override` değiştiricisi, bir *accessor_modifier* yalnızca özellik veya dizin oluşturucusu hem de varsa izin verilir bir `get` ve `set` erişimci ve ardından yalnızca birinde bu izin verilir erişimcileri.</span><span class="sxs-lookup"><span data-stu-id="6e059-1041">For a property or indexer that has no `override` modifier, an *accessor_modifier* is permitted only if the property or indexer has both a `get` and `set` accessor, and then is permitted only on one of those accessors.</span></span>
*  <span data-ttu-id="6e059-1042">Bir özellik veya içeren dizin oluşturucu için bir `override` değiştiricisi, bir erişimci eşleşmelidir *accessor_modifier*varsa kılınmasını erişimcisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1042">For a property or indexer that includes an `override` modifier, an accessor must match the *accessor_modifier*, if any, of the accessor being overridden.</span></span>
*  <span data-ttu-id="6e059-1043">*Accessor_modifier* özellik veya dizin oluşturucu kendisini bildirilen erişilebilirliği kesinlikle daha kısıtlayıcı bir erişilebilirlik bildirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1043">The *accessor_modifier* must declare an accessibility that is strictly more restrictive than the declared accessibility of the property or indexer itself.</span></span> <span data-ttu-id="6e059-1044">Kesin olarak için:</span><span class="sxs-lookup"><span data-stu-id="6e059-1044">To be precise:</span></span>
   * <span data-ttu-id="6e059-1045">Özellik veya dizin oluşturucu bir bildirilen erişilebilirliği varsa `public`, *accessor_modifier* olabilir `protected internal`, `internal`, `protected`, veya `private`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1045">If the property or indexer has a declared accessibility of `public`, the *accessor_modifier* may be either `protected internal`, `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="6e059-1046">Özellik veya dizin oluşturucu bir bildirilen erişilebilirliği varsa `protected internal`, *accessor_modifier* olabilir `internal`, `protected`, veya `private`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1046">If the property or indexer has a declared accessibility of `protected internal`, the *accessor_modifier* may be either `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="6e059-1047">Özellik veya dizin oluşturucu bir bildirilen erişilebilirliği varsa `internal` veya `protected`, *accessor_modifier* olmalıdır `private`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1047">If the property or indexer has a declared accessibility of `internal` or `protected`, the *accessor_modifier* must be `private`.</span></span>
   * <span data-ttu-id="6e059-1048">Özellik veya dizin oluşturucu bir bildirilen erişilebilirliği varsa `private`, *accessor_modifier* kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1048">If the property or indexer has a declared accessibility of `private`, no *accessor_modifier* may be used.</span></span>

<span data-ttu-id="6e059-1049">İçin `abstract` ve `extern` özellikleri *accessor_body* için belirtilen her bir erişimci noktalı yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1049">For `abstract` and `extern` properties, the *accessor_body* for each accessor specified is simply a semicolon.</span></span> <span data-ttu-id="6e059-1050">Her bir soyut olmayan, extern olmayan özelliğe sahip *accessor_body* noktalı olabilir, bu durumda olduğu bir ***özelliği'otomatik olarak uygulanan*** ([Özellikleri'otomatik olarak uygulanan ](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1050">A non-abstract, non-extern property may have each *accessor_body* be a semicolon, in which case it is an ***automatically implemented property*** ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="6e059-1051">Otomatik olarak uygulanan bir özellik en az bir get erişimcisine sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1051">An automatically implemented property must have at least a get accessor.</span></span> <span data-ttu-id="6e059-1052">Herhangi diğer soyut olmayan, extern olmayan özellik erişimcileri için *accessor_body* olduğu bir *blok* karşılık gelen erişimci çağrıldığında çalıştırılacak deyimleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1052">For the accessors of any other non-abstract, non-extern property, the *accessor_body* is a *block* which specifies the statements to be executed when the corresponding accessor is invoked.</span></span>

<span data-ttu-id="6e059-1053">A `get` bir parametresiz yöntemin dönüş değerini özellik türü ile erişimci karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1053">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="6e059-1054">Bir özellik bir ifadede başvurulduğunda, atama hedefi olarak dışında `get` özelliğin değerini hesaplamak için erişimci özelliğin çağırılır ([ifadelerin değerleri](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1054">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property ([Values of expressions](expressions.md#values-of-expressions)).</span></span> <span data-ttu-id="6e059-1055">Gövdesi bir `get` erişimci değeri döndürmek için kurallara uygun olmalıdır açıklanan yöntemlerden [yöntem gövdesini](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="6e059-1055">The body of a `get` accessor must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="6e059-1056">Özellikle, tüm `return` deyimleri gövdesinde bir `get` erişimci özelliği türüne örtük olarak dönüştürülebilir bir ifade belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1056">In particular, all `return` statements in the body of a `get` accessor must specify an expression that is implicitly convertible to the property type.</span></span> <span data-ttu-id="6e059-1057">Ayrıca, uç noktası bir `get` erişimci erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1057">Furthermore, the endpoint of a `get` accessor must not be reachable.</span></span>

<span data-ttu-id="6e059-1058">A `set` erişimci karşılık gelen bir tek değer özellik türü parametresi olan bir yönteme ve `void` dönüş türü.</span><span class="sxs-lookup"><span data-stu-id="6e059-1058">A `set` accessor corresponds to a method with a single value parameter of the property type and a `void` return type.</span></span> <span data-ttu-id="6e059-1059">Örtük parametresi bir `set` erişimci adlı her zaman `value`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1059">The implicit parameter of a `set` accessor is always named `value`.</span></span> <span data-ttu-id="6e059-1060">Bir özellik atama hedefi başvurulan ne zaman ([atama işleçleri](expressions.md#assignment-operators)), veya işleneni olarak `++` veya `--` ([sonek arttırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators), [ Önek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)), `set` erişimci bağımsız değişkenlerle çağrılır (değeri ataması sağ tarafında veya işleneni olan `++` veya `--` işleci), Yeni bir değer sağlar ([basit atama](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1060">When a property is referenced as the target of an assignment ([Assignment operators](expressions.md#assignment-operators)), or as the operand of `++` or `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), the `set` accessor is invoked with an argument (whose value is that of the right-hand side of the assignment or the operand of the `++` or `--` operator) that provides the new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="6e059-1061">Gövdesi bir `set` erişimcisi için kurallara uygun olmalıdır `void` açıklanan yöntemlerden [yöntem gövdesini](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="6e059-1061">The body of a `set` accessor must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="6e059-1062">Özellikle, `return` deyimlerinde `set` erişimcisinin gövdesi bir ifadeyi belirtmek izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="6e059-1062">In particular, `return` statements in the `set` accessor body are not permitted to specify an expression.</span></span> <span data-ttu-id="6e059-1063">Bu yana bir `set` erişimci örtük olarak adlandırılan bir parametresi sahip `value`, bir derleme zamanı hata için bir yerel değişken veya Sabit bildiriminde bir `set` erişimci bu ada sahip.</span><span class="sxs-lookup"><span data-stu-id="6e059-1063">Since a `set` accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declaration in a `set` accessor to have that name.</span></span>

<span data-ttu-id="6e059-1064">Varlığı veya yokluğu tabanlı `get` ve `set` erişimcileri, bir özellik gibi Sınıflandırması:</span><span class="sxs-lookup"><span data-stu-id="6e059-1064">Based on the presence or absence of the `get` and `set` accessors, a property is classified as follows:</span></span>

*  <span data-ttu-id="6e059-1065">Her ikisini de içeren bir özellik bir `get` erişimci ve `set` erişimci söyledi olacak şekilde bir ***okuma-yazma*** özelliği.</span><span class="sxs-lookup"><span data-stu-id="6e059-1065">A property that includes both a `get` accessor and a `set` accessor is said to be a ***read-write*** property.</span></span>
*  <span data-ttu-id="6e059-1066">Yalnızca bir özellik bir `get` erişimci söyledi olacak şekilde bir ***salt okunur*** özelliği.</span><span class="sxs-lookup"><span data-stu-id="6e059-1066">A property that has only a `get` accessor is said to be a ***read-only*** property.</span></span> <span data-ttu-id="6e059-1067">Atama hedefi olmasını salt okunur bir özellik için bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1067">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>
*  <span data-ttu-id="6e059-1068">Yalnızca bir özellik bir `set` erişimci söyledi olacak şekilde bir ***salt yazılır*** özelliği.</span><span class="sxs-lookup"><span data-stu-id="6e059-1068">A property that has only a `set` accessor is said to be a ***write-only*** property.</span></span> <span data-ttu-id="6e059-1069">Dışında bir atama hedefi olarak, bir ifadede bir salt yazılır özellik başvurmak için bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1069">Except as the target of an assignment, it is a compile-time error to reference a write-only property in an expression.</span></span>

<span data-ttu-id="6e059-1070">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-1070">In the example</span></span>
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
<span data-ttu-id="6e059-1071">`Button` denetimini bildirir genel `Caption` özelliği.</span><span class="sxs-lookup"><span data-stu-id="6e059-1071">the `Button` control declares a public `Caption` property.</span></span> <span data-ttu-id="6e059-1072">`get` Erişimcisine `Caption` özelliği, özel depolanan bir dize döndürür `caption` alan.</span><span class="sxs-lookup"><span data-stu-id="6e059-1072">The `get` accessor of the `Caption` property returns the string stored in the private `caption` field.</span></span> <span data-ttu-id="6e059-1073">`set` Erişimcisi varsa yeni değerin geçerli değerden farklı ve bu durumda, yeni bir değer depolar. denetler ve denetimi halinde yeniden boyar.</span><span class="sxs-lookup"><span data-stu-id="6e059-1073">The `set` accessor checks if the new value is different from the current value, and if so, it stores the new value and repaints the control.</span></span> <span data-ttu-id="6e059-1074">Özellikleri, yukarıda gösterilen deseni genellikle izleyin: `get` erişimcisi yalnızca özel bir alanda depolanmış bir değer döndürür ve `set` erişimcisi bu özel alan değiştirir ve ardından tam durumu güncelleştirmek için gereken ek eylemleri gerçekleştirir nesne.</span><span class="sxs-lookup"><span data-stu-id="6e059-1074">Properties often follow the pattern shown above: The `get` accessor simply returns a value stored in a private field, and the `set` accessor modifies that private field and then performs any additional actions required to fully update the state of the object.</span></span>

<span data-ttu-id="6e059-1075">Verilen `Button` sınıfında above, aşağıdaki örneğidir kullanımını `Caption` özelliği:</span><span class="sxs-lookup"><span data-stu-id="6e059-1075">Given the `Button` class above, the following is an example of use of the `Caption` property:</span></span>
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

<span data-ttu-id="6e059-1076">Burada, `set` erişimci özelliği için bir değer atama tarafından çağrılır ve `get` erişimci özelliği bir ifadede başvurarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1076">Here, the `set` accessor is invoked by assigning a value to the property, and the `get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="6e059-1077">`get` Ve `set` özellik erişimcileri ayrı üyeleri olmayan ve bir özelliğin erişimcileri ayrı olarak bildirmek mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1077">The `get` and `set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="6e059-1078">Bu nedenle, farklı erişilebilirliğe sahip iki erişimci okuma-yazma özelliği için mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1078">As such, it is not possible for the two accessors of a read-write property to have different accessibility.</span></span> <span data-ttu-id="6e059-1079">Örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-1079">The example</span></span>
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
<span data-ttu-id="6e059-1080">tek bir okuma-yazma özelliği bildirmiyor.</span><span class="sxs-lookup"><span data-stu-id="6e059-1080">does not declare a single read-write property.</span></span> <span data-ttu-id="6e059-1081">Bunun yerine, bu iki özellik aynı ada sahip, bir salt okunur bildirir ve bir salt yazılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1081">Rather, it declares two properties with the same name, one read-only and one write-only.</span></span> <span data-ttu-id="6e059-1082">Örnek aynı sınıfta bildirilen iki üye aynı ada sahip olamaz olduğundan, bir derleme zamanı hatası oluşmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1082">Since two members declared in the same class cannot have the same name, the example causes a compile-time error to occur.</span></span>

<span data-ttu-id="6e059-1083">Türetilmiş bir sınıf, devralınmış bir özellik olarak aynı adı taşıyan bir özelliği bildirir, hem okumaya hem yazmaya göre devralınan özelliği türetilmiş özelliğini gizler.</span><span class="sxs-lookup"><span data-stu-id="6e059-1083">When a derived class declares a property by the same name as an inherited property, the derived property hides the inherited property with respect to both reading and writing.</span></span> <span data-ttu-id="6e059-1084">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-1084">In the example</span></span>
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
<span data-ttu-id="6e059-1085">`P` özelliğinde `B` gizler `P` özelliğinde `A` hem okuma ve yazma ile ilgili.</span><span class="sxs-lookup"><span data-stu-id="6e059-1085">the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing.</span></span> <span data-ttu-id="6e059-1086">Bu nedenle, ifadeler</span><span class="sxs-lookup"><span data-stu-id="6e059-1086">Thus, in the statements</span></span>
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
<span data-ttu-id="6e059-1087">atamayı `b.P` salt okunur beri bildirilmesini bir derleme zamanı hatasına neden olur `P` özelliğinde `B` salt yazılır gizler `P` özelliğinde `A`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1087">the assignment to `b.P` causes a compile-time error to be reported, since the read-only `P` property in `B` hides the write-only `P` property in `A`.</span></span> <span data-ttu-id="6e059-1088">Ancak, bir yayın gizli erişmek için kullanılabileceğini unutmayın `P` özelliği.</span><span class="sxs-lookup"><span data-stu-id="6e059-1088">Note, however, that a cast can be used to access the hidden `P` property.</span></span>

<span data-ttu-id="6e059-1089">Özellikler, ortak alanları, bir nesnenin iç durumu ve ortak arabirimi arasında bir ayrım sağlar.</span><span class="sxs-lookup"><span data-stu-id="6e059-1089">Unlike public fields, properties provide a separation between an object's internal state and its public interface.</span></span> <span data-ttu-id="6e059-1090">Örneği göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="6e059-1090">Consider the example:</span></span>
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

<span data-ttu-id="6e059-1091">Burada, `Label` sınıfını kullanan iki `int` alanları `x` ve `y`konumunu saklamak için.</span><span class="sxs-lookup"><span data-stu-id="6e059-1091">Here, the `Label` class uses two `int` fields, `x` and `y`, to store its location.</span></span> <span data-ttu-id="6e059-1092">Herkese açık şekilde konumdur her ikisi de olarak kullanıma sunulan bir `X` ve `Y` özelliği ve bir `Location` türünün özelliği `Point`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1092">The location is publicly exposed both as an `X` and a `Y` property and as a `Location` property of type `Point`.</span></span> <span data-ttu-id="6e059-1093">Eğer, gelecek bir sürümünde `Label`, konumu olarak depolamak daha kullanışlı hale gelir bir `Point` sınıfının ortak arabirimi etkilemeden değişiklik dahili olarak, yapılabilir:</span><span class="sxs-lookup"><span data-stu-id="6e059-1093">If, in a future version of `Label`, it becomes more convenient to store the location as a `Point` internally, the change can be made without affecting the public interface of the class:</span></span>
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

<span data-ttu-id="6e059-1094">Vardı `x` ve `y` bunun yerine silinmiş `public readonly` alanları, olabilirdi böyle bir değişiklik yapmak mümkün `Label` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="6e059-1094">Had `x` and `y` instead been `public readonly` fields, it would have been impossible to make such a change to the `Label` class.</span></span>

<span data-ttu-id="6e059-1095">Durum özellikleri aracılığıyla kullanıma sunmak, alanlar doğrudan sunmaktan mutlaka daha az verimli değil.</span><span class="sxs-lookup"><span data-stu-id="6e059-1095">Exposing state through properties is not necessarily any less efficient than exposing fields directly.</span></span> <span data-ttu-id="6e059-1096">Özellikle, bir özellik sanal olmayan ve yalnızca küçük miktarda kod içerdiğinde yürütme ortamı erişimcileri gerçek koduyla erişimcileri çağrıları değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1096">In particular, when a property is non-virtual and contains only a small amount of code, the execution environment may replace calls to accessors with the actual code of the accessors.</span></span> <span data-ttu-id="6e059-1097">Bu işlem olarak bilinir ***inlining'i***, özellik erişimi olarak alan erişimini verimli hale getirir henüz özelliklerinin daha fazla esneklik korur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1097">This process is known as ***inlining***, and it makes property access as efficient as field access, yet preserves the increased flexibility of properties.</span></span>

<span data-ttu-id="6e059-1098">Çağrılırken bu yana bir `get` erişimci eşdeğerdir bir alanın değerini okumak için kavramsal olarak, hatalı programlama stili olarak kabul edilir `get` gözlemlenebilir bir yan etkileri olduğu erişimcileri.</span><span class="sxs-lookup"><span data-stu-id="6e059-1098">Since invoking a `get` accessor is conceptually equivalent to reading the value of a field, it is considered bad programming style for `get` accessors to have observable side-effects.</span></span> <span data-ttu-id="6e059-1099">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-1099">In the example</span></span>
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
<span data-ttu-id="6e059-1100">Değerini `Next` özelliği özelliği daha önce erişilmeden kez sayısına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1100">the value of the `Next` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="6e059-1101">Bu nedenle, özelliğine erişmek için gözlemlenebilir bir yan etkisi üretir ve özellik yöntemi olarak bunun yerine uygulanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1101">Thus, accessing the property produces an observable side-effect, and the property should be implemented as a method instead.</span></span>

<span data-ttu-id="6e059-1102">İçin "hiçbir yan etkiler" kuralı `get` erişimcileri, ortalama değil `get` erişimcileri her zaman yazılması alanlarında depolanan değerleri döndürülecek.</span><span class="sxs-lookup"><span data-stu-id="6e059-1102">The "no side-effects" convention for `get` accessors doesn't mean that `get` accessors should always be written to simply return values stored in fields.</span></span> <span data-ttu-id="6e059-1103">Aslında, `get` erişimcileri birden çok alan erişme veya yöntemlerini çağırmaktan bir özelliğin değerini genellikle işlem.</span><span class="sxs-lookup"><span data-stu-id="6e059-1103">Indeed, `get` accessors often compute the value of a property by accessing multiple fields or invoking methods.</span></span> <span data-ttu-id="6e059-1104">Ancak, düzgün bir şekilde tasarlanmış `get` erişimci gözlemlenebilir değişiklikleri nesne durumunda neden herhangi bir eylem gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1104">However, a properly designed `get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="6e059-1105">İlk başvurulan şu kadar olan bir kaynağın başlatma gecikme özellikleri kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1105">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="6e059-1106">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6e059-1106">For example:</span></span>
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

<span data-ttu-id="6e059-1107">`Console` Sınıfı üç özellik içerir `In`, `Out`, ve `Error`, temsil eden standart giriş, çıkış ve hata cihazları, sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="6e059-1107">The `Console` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="6e059-1108">Bu üye özellikleri olarak gösterme tarafından `Console` gerçekten kullanılana kadar sınıf başlatma gecikme.</span><span class="sxs-lookup"><span data-stu-id="6e059-1108">By exposing these members as properties, the `Console` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="6e059-1109">Örneğin, ilk başvuran bağlı `Out` görüldüğü özelliği</span><span class="sxs-lookup"><span data-stu-id="6e059-1109">For example, upon first referencing the `Out` property, as in</span></span>
```csharp
Console.Out.WriteLine("hello, world");
```
<span data-ttu-id="6e059-1110">arka plandaki `TextWriter` çıktı cihazına oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1110">the underlying `TextWriter` for the output device is created.</span></span> <span data-ttu-id="6e059-1111">Ancak uygulama başvuru yaparsa `In` ve `Error` özellikleri ve herhangi bir nesnenin bu cihazlar için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1111">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="6e059-1112">Otomatik olarak uygulanan özellikler</span><span class="sxs-lookup"><span data-stu-id="6e059-1112">Automatically implemented properties</span></span>

<span data-ttu-id="6e059-1113">Otomatik olarak uygulanan bir özellik (veya ***otomatik-özellik*** kısaca), yalnızca noktalı virgül erişimcisinin gövdesi ile soyut olmayan extern olmayan özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1113">An automatically implemented property (or ***auto-property*** for short), is a non-abstract non-extern property with semicolon-only accessor bodies.</span></span> <span data-ttu-id="6e059-1114">Otomatik özelliklerde bir get erişimcisine sahip olmalıdır ve isteğe bağlı olarak ayarlama erişimcisine sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1114">Auto-properties must have a get accessor and can optionally have a set accessor.</span></span>

<span data-ttu-id="6e059-1115">Bir özelliği otomatik olarak uygulanan bir özellik olarak belirtildiğinde, gizli destek alanı özelliği için otomatik olarak kullanılabilir ve erişimcileri, okuma ve yazma ilgili yedekleme alanı için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1115">When a property is specified as an automatically implemented property, a hidden backing field is automatically available for the property, and the accessors are implemented to read from and write to that backing field.</span></span> <span data-ttu-id="6e059-1116">Otomatik-özellik set erişimcisine sahip değildir, yedekleme alanını kabul edildiği `readonly` ([salt okunur alanların](classes.md#readonly-fields)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1116">If the auto-property has no set accessor, the backing field is considered `readonly` ([Readonly fields](classes.md#readonly-fields)).</span></span> <span data-ttu-id="6e059-1117">Olduğu gibi bir `readonly` alan, bir yalnızca otomatik özellik de atanabilen kapsayan sınıfın bir oluşturucu gövdesinde.</span><span class="sxs-lookup"><span data-stu-id="6e059-1117">Just like a `readonly` field, a getter-only auto-property can also be assigned to in the body of a constructor of the enclosing class.</span></span> <span data-ttu-id="6e059-1118">Bu tür bir atama doğrudan salt okunur yedekleme alanına özelliğinin atar.</span><span class="sxs-lookup"><span data-stu-id="6e059-1118">Such an assignment assigns directly to the readonly backing field of the property.</span></span>

<span data-ttu-id="6e059-1119">Otomatik özellik isteğe bağlı olarak olabilir bir *property_initializer*, doğrudan yedekleme alanı olarak için uygulanan bir *variable_initializer* ([değişken başlatıcılar](classes.md#variable-initializers)) .</span><span class="sxs-lookup"><span data-stu-id="6e059-1119">An auto-property may optionally have a *property_initializer*, which is applied directly to the backing field as a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)).</span></span>

<span data-ttu-id="6e059-1120">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="6e059-1120">The following example:</span></span>
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
<span data-ttu-id="6e059-1121">Aşağıdaki bildirime eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="6e059-1121">is equivalent to the following declaration:</span></span>
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

<span data-ttu-id="6e059-1122">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="6e059-1122">The following example:</span></span>
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
<span data-ttu-id="6e059-1123">Aşağıdaki bildirime eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="6e059-1123">is equivalent to the following declaration:</span></span>
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

<span data-ttu-id="6e059-1124">Oluşturucu içinde ortaya olduğundan salt okunur alan atamalarını yasal, olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6e059-1124">Notice that the assignments to the readonly field are legal, because they occur within the constructor.</span></span>


### <a name="accessibility"></a><span data-ttu-id="6e059-1125">Erişilebilirlik</span><span class="sxs-lookup"><span data-stu-id="6e059-1125">Accessibility</span></span>

<span data-ttu-id="6e059-1126">Erişimci varsa bir *accessor_modifier*, Erişilebilirlik etki alanı ([erişilebilirlik etki alanı](basic-concepts.md#accessibility-domains)) erişimcisine öğesinin bildirilen erişilebilirliği belirlenir *accessor_modifier* .</span><span class="sxs-lookup"><span data-stu-id="6e059-1126">If an accessor has an *accessor_modifier*, the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the accessor is determined using the declared accessibility of the *accessor_modifier*.</span></span> <span data-ttu-id="6e059-1127">Erişimci yoksa bir *accessor_modifier*, özelliğin veya dizin oluşturucu bildirilen erişilebilirliği erişimcisinin erişilebilirlik etki alanı belirlenir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1127">If an accessor does not have an *accessor_modifier*, the accessibility domain of the accessor is determined from the declared accessibility of the property or indexer.</span></span>

<span data-ttu-id="6e059-1128">Varlığı bir *accessor_modifier* hiç üye araması etkiler ([işleçleri](expressions.md#operators)) veya aşırı yükleme çözümlemesi ([aşırı yükleme çözünürlüğü](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1128">The presence of an *accessor_modifier* never affects member lookup ([Operators](expressions.md#operators)) or overload resolution ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="6e059-1129">Değiştiriciler özelliği veya dizin oluşturucu hangi özelliğin veya dizin oluşturucu bağımsız olarak erişim bağlamında bağlı olduğu her zaman belirleyin.</span><span class="sxs-lookup"><span data-stu-id="6e059-1129">The modifiers on the property or indexer always determine which property or indexer is bound to, regardless of the context of the access.</span></span>

<span data-ttu-id="6e059-1130">Belirli bir özellik veya dizin oluşturucu seçildikten sonra ilgili özel erişimciler erişilebilirlik etki alanları, kullanım geçerli olup olmadığını belirlemek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="6e059-1130">Once a particular property or indexer has been selected, the accessibility domains of the specific accessors involved are used to determine if that usage is valid:</span></span>

*  <span data-ttu-id="6e059-1131">Kullanım bir değer olarak ise ([ifadelerin değerleri](expressions.md#values-of-expressions)), `get` erişimci mevcut ve erişilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1131">If the usage is as a value ([Values of expressions](expressions.md#values-of-expressions)), the `get` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="6e059-1132">Kullanımı basit atama hedefi olarak ayarlanırsa ([basit atama](expressions.md#simple-assignment)), `set` erişimci mevcut ve erişilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1132">If the usage is as the target of a simple assignment ([Simple assignment](expressions.md#simple-assignment)), the `set` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="6e059-1133">Kullanım bileşik atama hedefi olarak olup olmadığını ([bileşik atama](expressions.md#compound-assignment)), veya hedefi olarak `++` veya `--` işleçleri ([işlev üyeleri](expressions.md#function-members).9, [ Çağrı ifadeleri](expressions.md#invocation-expressions)), her iki `get` erişimcileri ve `set` erişimci mevcut ve erişilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1133">If the usage is as the target of compound assignment ([Compound assignment](expressions.md#compound-assignment)), or as the target of the `++` or `--` operators ([Function members](expressions.md#function-members).9, [Invocation expressions](expressions.md#invocation-expressions)), both the `get` accessors and the `set` accessor must exist and be accessible.</span></span>

<span data-ttu-id="6e059-1134">Aşağıdaki örnekte, özellik `A.Text` özelliği tarafından gizleniyor `B.Text`yalnızca olduğunda bile bağlamlarda `set` erişimci çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1134">In the following example, the property `A.Text` is hidden by the property `B.Text`, even in contexts where only the `set` accessor is called.</span></span> <span data-ttu-id="6e059-1135">Buna karşılık, özellik `B.Count` sınıfına erişilemiyor `M`, bu nedenle erişilebilir özellik `A.Count` yerine kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1135">In contrast, the property `B.Count` is not accessible to class `M`, so the accessible property `A.Count` is used instead.</span></span>

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

<span data-ttu-id="6e059-1136">Bir arabirim uygulamak için kullanılan bir erişimci olmayabilir bir *accessor_modifier*.</span><span class="sxs-lookup"><span data-stu-id="6e059-1136">An accessor that is used to implement an interface may not have an *accessor_modifier*.</span></span> <span data-ttu-id="6e059-1137">Yalnızca bir erişimci bir arabirim uygulamak için kullanılan diğer erişimcisi ile bildirilebilir bir *accessor_modifier*:</span><span class="sxs-lookup"><span data-stu-id="6e059-1137">If only one accessor is used to implement an interface, the other accessor may be declared with an *accessor_modifier*:</span></span>
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a><span data-ttu-id="6e059-1138">Korumalı sanal, geçersiz kılma ve Özet özellik erişimcileri</span><span class="sxs-lookup"><span data-stu-id="6e059-1138">Virtual, sealed, override, and abstract property accessors</span></span>

<span data-ttu-id="6e059-1139">A `virtual` özellik bildiriminde özelliğin erişimcileri sanal olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1139">A `virtual` property declaration specifies that the accessors of the property are virtual.</span></span> <span data-ttu-id="6e059-1140">`virtual` Değiştiricisi, bir okuma-yazma özellik erişenleri için uygular — sanal yalnızca bir erişimci okuma-yazma özelliği için mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1140">The `virtual` modifier applies to both accessors of a read-write property—it is not possible for only one accessor of a read-write property to be virtual.</span></span>

<span data-ttu-id="6e059-1141">Bir `abstract` özellik bildiriminde özellik erişimcileri sanal ancak gerçek erişimcileri uygulanışı sağlamaz belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1141">An `abstract` property declaration specifies that the accessors of the property are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="6e059-1142">Bunun yerine, türetilen soyut olmayan sınıflar özellik geçersiz kılarak erişimcileri için kendi uygulama sağlamak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1142">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the property.</span></span> <span data-ttu-id="6e059-1143">Bir soyut özellik bildiriminde bir erişimci hiçbir kullanımınız sağladığından, *accessor_body* yalnızca noktalı virgül içerir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1143">Because an accessor for an abstract property declaration provides no actual implementation, its *accessor_body* simply consists of a semicolon.</span></span>

<span data-ttu-id="6e059-1144">Her ikisini de içeren bir özellik bildirimi `abstract` ve `override` değiştiriciler özelliği soyuttur ve bir temel özelliğini geçersiz kılar belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1144">A property declaration that includes both the `abstract` and `override` modifiers specifies that the property is abstract and overrides a base property.</span></span> <span data-ttu-id="6e059-1145">Böyle bir özellik erişimcisi de soyuttur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1145">The accessors of such a property are also abstract.</span></span>

<span data-ttu-id="6e059-1146">Soyut özellik bildirimleri yalnızca soyut sınıfları izin ([soyut sınıflar](classes.md#abstract-classes)). Devralınmış bir sanal özellik erişimcileri belirten bir özellik bildirimi dahil olmak üzere türetilen bir sınıfta kılınabilir bir `override` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1146">Abstract property declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).The accessors of an inherited virtual property can be overridden in a derived class by including a property declaration that specifies an `override` directive.</span></span> <span data-ttu-id="6e059-1147">Bu olarak bilinen bir ***özellik bildiriminde geçersiz kılma***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1147">This is known as an ***overriding property declaration***.</span></span> <span data-ttu-id="6e059-1148">Bir geçersiz kılma özelliği bildirimi, yeni bir özellik bildirmiyor.</span><span class="sxs-lookup"><span data-stu-id="6e059-1148">An overriding property declaration does not declare a new property.</span></span> <span data-ttu-id="6e059-1149">Bunun yerine, mevcut sanal özelliğin erişimcileri, uygulamaları yalnızca uzmanlaşmış.</span><span class="sxs-lookup"><span data-stu-id="6e059-1149">Instead, it simply specializes the implementations of the accessors of an existing virtual property.</span></span>

<span data-ttu-id="6e059-1150">Bir geçersiz kılma özellik bildiriminde tam aynı erişilebilirlik değiştiricileri, tür ve ad devralınan özelliği olarak belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1150">An overriding property declaration must specify the exact same accessibility modifiers, type, and name as the inherited property.</span></span> <span data-ttu-id="6e059-1151">Varsa (yani, devralınan özelliği salt okunur veya salt yazılır) devralınan özelliği yalnızca tek bir erişimci sahipse, geçersiz kılma özelliği yalnızca bir erişimci içermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1151">If the inherited property has only a single accessor (i.e., if the inherited property is read-only or write-only), the overriding property must include only that accessor.</span></span> <span data-ttu-id="6e059-1152">Varsa (yani, devralınan özelliği salt okunur ise), her iki erişimcisi devralınan özelliği içerir, geçersiz kılma özelliği tek bir erişimci veya her iki erişimcisi içerebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1152">If the inherited property includes both accessors (i.e., if the inherited property is read-write), the overriding property can include either a single accessor or both accessors.</span></span>

<span data-ttu-id="6e059-1153">Bir geçersiz kılma özelliği bildirimi içerebilir `sealed` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1153">An overriding property declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="6e059-1154">Bu değiştirici kullanımını türetilmiş bir sınıf, daha fazla özellik geçersiz kılmasını önler.</span><span class="sxs-lookup"><span data-stu-id="6e059-1154">Use of this modifier prevents a derived class from further overriding the property.</span></span> <span data-ttu-id="6e059-1155">Ayrıca korumalı bir korumalı özelliğin erişimcileri.</span><span class="sxs-lookup"><span data-stu-id="6e059-1155">The accessors of a sealed property are also sealed.</span></span>

<span data-ttu-id="6e059-1156">Bildirim ve çağırma farklılıkları dışında sözdizimi, sanal, sealed bir geçersiz kılma ve soyut erişimcileri tam olarak sanal, sealed bir geçersiz kılma ve Özet yöntemler gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1156">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="6e059-1157">Kuralları açıklanan özellikle [sanal yöntemleri](classes.md#virtual-methods), [geçersiz kılma yöntemleri](classes.md#override-methods), [yöntemleri korumalı](classes.md#sealed-methods), ve [soyut yöntemler](classes.md#abstract-methods) uygulamak gibi erişimciler yöntemleri karşılık gelen bir formun şöyleydi:</span><span class="sxs-lookup"><span data-stu-id="6e059-1157">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form:</span></span>

*  <span data-ttu-id="6e059-1158">A `get` bir parametresiz yöntemin dönüş değeri içeren özellik olarak aynı değiştiricilere ve özellik türü ile erişimci karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1158">A `get` accessor corresponds to a parameterless method with a return value of the property type and the same modifiers as the containing property.</span></span>
*  <span data-ttu-id="6e059-1159">A `set` erişimci karşılık gelen bir tek değer özellik türü parametresi olan bir yönteme bir `void` dönüş türü ve aynı değiştiricilere kapsayan özelliği olarak.</span><span class="sxs-lookup"><span data-stu-id="6e059-1159">A `set` accessor corresponds to a method with a single value parameter of the property type, a `void` return type, and the same modifiers as the containing property.</span></span>

<span data-ttu-id="6e059-1160">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-1160">In the example</span></span>
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
<span data-ttu-id="6e059-1161">`X` bir sanal salt okunur özelliği `Y` sanal bir okuma-yazma özelliği ve `Z` soyut bir okuma-yazma özelliği.</span><span class="sxs-lookup"><span data-stu-id="6e059-1161">`X` is a virtual read-only property, `Y` is a virtual read-write property, and `Z` is an abstract read-write property.</span></span> <span data-ttu-id="6e059-1162">Çünkü `Z` soyuttur, kapsayan sınıfı `A` da soyut bildirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1162">Because `Z` is abstract, the containing class `A` must also be declared abstract.</span></span>

<span data-ttu-id="6e059-1163">Türetilen bir sınıf `A` aşağıdaki gösterisi:</span><span class="sxs-lookup"><span data-stu-id="6e059-1163">A class that derives from `A` is show below:</span></span>
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

<span data-ttu-id="6e059-1164">Burada, bildirimlerini `X`, `Y`, ve `Z` özellik bildirimleri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="6e059-1164">Here, the declarations of `X`, `Y`, and `Z` are overriding property declarations.</span></span> <span data-ttu-id="6e059-1165">Her özellik bildiriminde erişilebilirlik değiştiricileri, türü ve karşılık gelen devralınan özelliğin adını tam olarak eşleşir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1165">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="6e059-1166">`get` Erişimcisine `X` ve `set` erişimcisine `Y` kullanın `base` devralınan erişimcileri erişmek için anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="6e059-1166">The `get` accessor of `X` and the `set` accessor of `Y` use the `base` keyword to access the inherited accessors.</span></span> <span data-ttu-id="6e059-1167">Bildirimi `Z` soyut her iki erişimcisi geçersiz kılar; bu nedenle, hiçbir bekleyen soyut işlev üyeleri vardır `B`, ve `B` soyut olmayan bir sınıf olması için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1167">The declaration of `Z` overrides both abstract accessors—thus, there are no outstanding abstract function members in `B`, and `B` is permitted to be a non-abstract class.</span></span>

<span data-ttu-id="6e059-1168">Ne zaman bir özelliği olarak bildirilen bir `override`, herhangi bir geçersiz kılınan erişimcileri geçersiz kılma koda erişilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1168">When a property is declared as an `override`, any overridden accessors must be accessible to the overriding code.</span></span> <span data-ttu-id="6e059-1169">Ayrıca, özellik veya dizin oluşturucu kendisini ve erişimcileri öğesinin bildirilen erişilebilirliği erişimcileri ve geçersiz kılınan üyesiyle eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1169">In addition, the declared accessibility of both the property or indexer itself, and of the accessors, must match that of the overridden member and accessors.</span></span> <span data-ttu-id="6e059-1170">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6e059-1170">For example:</span></span>
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

## <a name="events"></a><span data-ttu-id="6e059-1171">Olaylar</span><span class="sxs-lookup"><span data-stu-id="6e059-1171">Events</span></span>

<span data-ttu-id="6e059-1172">Bir ***olay*** bir nesne veya sınıf bildirimleri sağlamak için sağlayan bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1172">An ***event*** is a member that enables an object or class to provide notifications.</span></span> <span data-ttu-id="6e059-1173">İstemciler, olaylar için yürütülebilir kodu sağlayarak iliştirebilirsiniz ***olay işleyicileri***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1173">Clients can attach executable code for events by supplying ***event handlers***.</span></span>

<span data-ttu-id="6e059-1174">Olayları kullanarak bildirilir *event_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="6e059-1174">Events are declared using *event_declaration*s:</span></span>

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

<span data-ttu-id="6e059-1175">Bir *event_declaration* bir dizi içerebilir *öznitelikleri* ([öznitelikleri](attributes.md)) ve geçerli bir bileşimin dört erişim değiştiricileri ([erişim değiştiricileri ](classes.md#access-modifiers)), `new` ([yeni değiştiricisini](classes.md#the-new-modifier)), `static` ([statik ve örnek yöntemler](classes.md#static-and-instance-methods)), `virtual` ([sanalyöntemleri](classes.md#virtual-methods)), `override` ([Geçersiz kılma yöntemleri](classes.md#override-methods)), `sealed` ([yöntemleri korumalı](classes.md#sealed-methods)), `abstract` ([soyut yöntemler](classes.md#abstract-methods)), ve `extern` ([Dış yöntemleri](classes.md#external-methods)) değiştiriciler.</span><span class="sxs-lookup"><span data-stu-id="6e059-1175">An *event_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="6e059-1176">Yöntem bildirimleri gibi aynı kurallara olay bildirimlerine tabidir ([yöntemleri](classes.md#methods)) değiştiriciler geçerli birleşimlerini onaylamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-1176">Event declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="6e059-1177">*Türü* olay bildirimi olmalıdır bir *delegate_type* ([başvuru türleri](types.md#reference-types)) ve *delegate_type* en az olarak olay olarak erişilebilir ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1177">The *type* of an event declaration must be a *delegate_type* ([Reference types](types.md#reference-types)), and that *delegate_type* must be at least as accessible as the event itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6e059-1178">Bir olay bildirimi içerebilir *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="6e059-1178">An event declaration may include *event_accessor_declarations*.</span></span> <span data-ttu-id="6e059-1179">Olmayan-extern, soyut olmayan olaylar içermiyorsa, ancak derleyicinin bunları otomatik olarak sağlar ([alan benzeri olaylara](classes.md#field-like-events)); extern olaylar için erişimciler harici olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1179">However, if it does not, for non-extern, non-abstract events, the compiler supplies them automatically ([Field-like events](classes.md#field-like-events)); for extern events, the accessors are provided externally.</span></span>

<span data-ttu-id="6e059-1180">Olay bildiriminde atlar *event_accessor_declarations* bir veya daha fazla olayı tanımlar — için birer tane *variable_declarator*s.</span><span class="sxs-lookup"><span data-stu-id="6e059-1180">An event declaration that omits *event_accessor_declarations* defines one or more events—one for each of the *variable_declarator*s.</span></span> <span data-ttu-id="6e059-1181">Tüm gibi tarafından bildirilen üyeler değiştiriciler ve öznitelikleri geçerli bir *event_declaration*.</span><span class="sxs-lookup"><span data-stu-id="6e059-1181">The attributes and modifiers apply to all of the members declared by such an *event_declaration*.</span></span>

<span data-ttu-id="6e059-1182">Bir derleme zamanı hata için bir *event_declaration* hem de içerecek şekilde `abstract` değiştiricisi ve küme ayracı ayrılmış *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="6e059-1182">It is a compile-time error for an *event_declaration* to include both the `abstract` modifier and brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="6e059-1183">Ne zaman bir olay bildirimi içeren bir `extern` değiştiricisi, bir olay olarak kabul edilir bir ***dış olay***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1183">When an event declaration includes an `extern` modifier, the event is said to be an ***external event***.</span></span> <span data-ttu-id="6e059-1184">Bir dış olay bildirimi gerçek uygulaması sağladığından, her ikisi de içerecek şekilde için bir hata olduğu `extern` değiştiricisi ve *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="6e059-1184">Because an external event declaration provides no actual implementation, it is an error for it to include both the `extern` modifier and *event_accessor_declarations*.</span></span>

<span data-ttu-id="6e059-1185">Bir derleme zamanı hata için bir *variable_declarator* ile bir olay bildiriminin bir `abstract` veya `external` içerecek şekilde değiştiricisi bir *variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="6e059-1185">It is a compile-time error for a *variable_declarator* of an event declaration with an `abstract` or `external` modifier to include a *variable_initializer*.</span></span>

<span data-ttu-id="6e059-1186">Bir olay sol işleneni kullanılabilir `+=` ve `-=` işleçleri ([olay ataması](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1186">An event can be used as the left-hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="6e059-1187">Bu işleçler, sırasıyla, olay işleyicileri eklemek veya bir olay, olay işleyicilerini kaldırmak için kullanılır ve bu işlemler izin bağlamları olayın erişim değiştiricileri denetleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6e059-1187">These operators are used, respectively, to attach event handlers to or to remove event handlers from an event, and the access modifiers of the event control the contexts in which such operations are permitted.</span></span>

<span data-ttu-id="6e059-1188">Bu yana `+=` ve `-=` bildirir olay, dış kod türü dışında bir olay için izin verilen işlemler ekleyin ve bir olay işleyicilerini kaldırmak ancak olamaz diğer herhangi bir yolla edinebilir veya temel alınan olay listesini değiştirmek olan işleyicileri.</span><span class="sxs-lookup"><span data-stu-id="6e059-1188">Since `+=` and `-=` are the only operations that are permitted on an event outside the type that declares the event, external code can add and remove handlers for an event, but cannot in any other way obtain or modify the underlying list of event handlers.</span></span>

<span data-ttu-id="6e059-1189">Formun işleminde `x += y` veya `x -= y`, `x` bir olaydır ve bildirimini içeren türü dışında bir yerde başvuru alan `x`, işlemin sonucunu türüne sahip `void` (aksine sahip türünü `x`, değeriyle `x` Atama işleminden sonra).</span><span class="sxs-lookup"><span data-stu-id="6e059-1189">In an operation of the form `x += y` or `x -= y`, when `x` is an event and the reference takes place outside the type that contains the declaration of `x`, the result of the operation has type `void` (as opposed to having the type of `x`, with the value of `x` after the assignment).</span></span> <span data-ttu-id="6e059-1190">Bu kural, bir olayın temel temsilci dolaylı olarak İnceleme dış kod yasaklar.</span><span class="sxs-lookup"><span data-stu-id="6e059-1190">This rule prohibits external code from indirectly examining the underlying delegate of an event.</span></span>

<span data-ttu-id="6e059-1191">Aşağıdaki örnek, olay işleyicileri örneklerine bağlı nasıl gösterir `Button` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="6e059-1191">The following example shows how event handlers are attached to instances of the `Button` class:</span></span>
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

<span data-ttu-id="6e059-1192">Burada, `LoginDialog` örnek oluşturucusu oluşturur iki `Button` örnekler ve ekler için olay işleyicileri `Click` olayları.</span><span class="sxs-lookup"><span data-stu-id="6e059-1192">Here, the `LoginDialog` instance constructor creates two `Button` instances and attaches event handlers to the `Click` events.</span></span>

### <a name="field-like-events"></a><span data-ttu-id="6e059-1193">Alan benzeri olaylara</span><span class="sxs-lookup"><span data-stu-id="6e059-1193">Field-like events</span></span>

<span data-ttu-id="6e059-1194">Sınıf veya bir olay bildirimini içeren yapı öğesinin program metni alanlar gibi belirli olaylar kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1194">Within the program text of the class or struct that contains the declaration of an event, certain events can be used like fields.</span></span> <span data-ttu-id="6e059-1195">Bu şekilde kullanılması için bir olay değil olmalıdır `abstract` veya `extern`ve açıkça içermemelidir *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="6e059-1195">To be used in this way, an event must not be `abstract` or `extern`, and must not explicitly include *event_accessor_declarations*.</span></span> <span data-ttu-id="6e059-1196">Böyle bir olaya izin veren bir alan her bağlamda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1196">Such an event can be used in any context that permits a field.</span></span> <span data-ttu-id="6e059-1197">Alanın bir temsilci içerir ([Temsilciler](delegates.md)) eklenmiş olan olaya olay işleyicileri listesine başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="6e059-1197">The field contains a delegate ([Delegates](delegates.md)) which refers to the list of event handlers that have been added to the event.</span></span> <span data-ttu-id="6e059-1198">Hiçbir olay işleyicileri eklenmişse içeren alan `null`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1198">If no event handlers have been added, the field contains `null`.</span></span>

<span data-ttu-id="6e059-1199">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-1199">In the example</span></span>
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
<span data-ttu-id="6e059-1200">`Click` bir alanı olarak kullanılan `Button` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="6e059-1200">`Click` is used as a field within the `Button` class.</span></span> <span data-ttu-id="6e059-1201">Örnekte de gösterildiği gibi alanın incelenir, değiştirilebilir ve temsilci çağırma ifadelerinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1201">As the example demonstrates, the field can be examined, modified, and used in delegate invocation expressions.</span></span> <span data-ttu-id="6e059-1202">`OnClick` Yönteminde `Button` sınıf "harekete geçirirse" `Click` olay.</span><span class="sxs-lookup"><span data-stu-id="6e059-1202">The `OnClick` method in the `Button` class "raises" the `Click` event.</span></span> <span data-ttu-id="6e059-1203">Olay bildirmek, kavram olay tarafından temsil edilen temsilci çağırmak için tam olarak eşittir; Bu nedenle, olayları oluşturma için hiçbir özel dil yapıları vardır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1203">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span> <span data-ttu-id="6e059-1204">Temsilci çağrısı null olmayan bir temsilci sağlar bir denetimi tarafından bulunduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6e059-1204">Note that the delegate invocation is preceded by a check that ensures the delegate is non-null.</span></span>

<span data-ttu-id="6e059-1205">Bildirimi dışındaki `Button` sınıfı `Click` üye yalnızca sol tarafında, kullanılabilir `+=` ve `-=` görüldüğü işleçleri</span><span class="sxs-lookup"><span data-stu-id="6e059-1205">Outside the declaration of the `Button` class, the `Click` member can only be used on the left-hand side of the `+=` and `-=` operators, as in</span></span>
```csharp
b.Click += new EventHandler(...);
```
<span data-ttu-id="6e059-1206">çağırma listesine bir temsilci ekler `Click` olay ve</span><span class="sxs-lookup"><span data-stu-id="6e059-1206">which appends a delegate to the invocation list of the `Click` event, and</span></span>
```csharp
b.Click -= new EventHandler(...);
```
<span data-ttu-id="6e059-1207">çağırma listesinden bir temsilciyi kaldırır `Click` olay.</span><span class="sxs-lookup"><span data-stu-id="6e059-1207">which removes a delegate from the invocation list of the `Click` event.</span></span>

<span data-ttu-id="6e059-1208">Alan benzeri olay derlerken, derleyici otomatik olarak temsilci tutmak için depolama oluşturur ve temsilci alanı için olay işleyicileri ekleyip erişimcileri olayı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1208">When compiling a field-like event, the compiler automatically creates storage to hold the delegate, and creates accessors for the event that add or remove event handlers to the delegate field.</span></span> <span data-ttu-id="6e059-1209">Ekleme ve kaldırma işlemlerinin iş parçacığı güvenlidir ve olabilir (ancak gerekli değildir) olması biraz yanıtlar bulunduran kilit ([lock deyiminin](statements.md#the-lock-statement)) içeren bir nesne için bir örnek olay veya tür nesnesi ([anonim Nesne oluşturma ifadeleri](expressions.md#anonymous-object-creation-expressions)) statik bir olay için.</span><span class="sxs-lookup"><span data-stu-id="6e059-1209">The addition and removal operations are thread safe, and may (but are not required to) be done while holding the lock ([The lock statement](statements.md#the-lock-statement)) on the containing object for an instance event, or the type object ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) for a static event.</span></span>

<span data-ttu-id="6e059-1210">Bu nedenle, bir örnek olay bildirimi formun:</span><span class="sxs-lookup"><span data-stu-id="6e059-1210">Thus, an instance event declaration of the form:</span></span>
```csharp
class X
{
    public event D Ev;
}
```
<span data-ttu-id="6e059-1211">eşdeğer bir şey derlenecek:</span><span class="sxs-lookup"><span data-stu-id="6e059-1211">will be compiled to something equivalent to:</span></span>
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
<span data-ttu-id="6e059-1212">Sınıf içindeki `X`, başvurular `Ev` sol tarafındaki `+=` ve `-=` işleçler neden ekleme ve kaldırma erişimcileri çağrılacak.</span><span class="sxs-lookup"><span data-stu-id="6e059-1212">Within the class `X`, references to `Ev` on the left-hand side of the `+=` and `-=` operators cause the add and remove accessors to be invoked.</span></span> <span data-ttu-id="6e059-1213">Tüm başvuruları `Ev` gizli alan başvurmak için derlenmiş `__Ev` bunun yerine ([üye erişimi](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1213">All other references to `Ev` are compiled to reference the hidden field `__Ev` instead ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="6e059-1214">Adı "`__Ev`" rastgeledir; gizli alanı hiç herhangi bir ad veya ad olabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1214">The name "`__Ev`" is arbitrary; the hidden field could have any name or no name at all.</span></span>

### <a name="event-accessors"></a><span data-ttu-id="6e059-1215">Olay erişimcileri</span><span class="sxs-lookup"><span data-stu-id="6e059-1215">Event accessors</span></span>

<span data-ttu-id="6e059-1216">Olay bildirimleri genellikle atlamak *event_accessor_declarations*, olarak `Button` Yukarıdaki örneği.</span><span class="sxs-lookup"><span data-stu-id="6e059-1216">Event declarations typically omit *event_accessor_declarations*, as in the `Button` example above.</span></span> <span data-ttu-id="6e059-1217">Bunu yapmak için bir durum kadar olay başına tek bir alanın depolama maliyeti kabul edilebilir değil çalışması içerir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1217">One situation for doing so involves the case in which the storage cost of one field per event is not acceptable.</span></span> <span data-ttu-id="6e059-1218">Bu gibi durumlarda, bir sınıf içerebilir *event_accessor_declarations* ve olay işleyicileri listesi depolamakla özel bir mekanizma kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6e059-1218">In such cases, a class can include *event_accessor_declarations* and use a private mechanism for storing the list of event handlers.</span></span>

<span data-ttu-id="6e059-1219">*Event_accessor_declarations* ekleme ve kaldırma olay işleyicileri ile ilişkili yürütülebilir deyimleri bir olay belirtin.</span><span class="sxs-lookup"><span data-stu-id="6e059-1219">The *event_accessor_declarations* of an event specify the executable statements associated with adding and removing event handlers.</span></span>

<span data-ttu-id="6e059-1220">Erişimcisi bildirimlerine oluşan bir *add_accessor_declaration* ve *remove_accessor_declaration*.</span><span class="sxs-lookup"><span data-stu-id="6e059-1220">The accessor declarations consist of an *add_accessor_declaration* and a *remove_accessor_declaration*.</span></span> <span data-ttu-id="6e059-1221">Belirteci her erişimci bildirimi oluşur `add` veya `remove` arkasından bir *blok*.</span><span class="sxs-lookup"><span data-stu-id="6e059-1221">Each accessor declaration consists of the token `add` or `remove` followed by a *block*.</span></span> <span data-ttu-id="6e059-1222">*Blok* ile ilişkili bir *add_accessor_declaration* bir olay işleyicisi eklendiğinde çalıştırılacak deyimleri belirtir ve *blok* bir ileilişkili*remove_accessor_declaration* bir olay işleyicisi kaldırıldığında çalıştırılacak deyimleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1222">The *block* associated with an *add_accessor_declaration* specifies the statements to execute when an event handler is added, and the *block* associated with a *remove_accessor_declaration* specifies the statements to execute when an event handler is removed.</span></span>

<span data-ttu-id="6e059-1223">Her *add_accessor_declaration* ve *remove_accessor_declaration* karşılık gelen bir olay türü tek değer parametresi olan bir yönteme ve `void` dönüş türü.</span><span class="sxs-lookup"><span data-stu-id="6e059-1223">Each *add_accessor_declaration* and *remove_accessor_declaration* corresponds to a method with a single value parameter of the event type and a `void` return type.</span></span> <span data-ttu-id="6e059-1224">Örtük bir olay erişimcisinin parametresi adlı `value`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1224">The implicit parameter of an event accessor is named `value`.</span></span> <span data-ttu-id="6e059-1225">Bir olay Olay atama kullanıldığında, ilgili olay erişimcisi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1225">When an event is used in an event assignment, the appropriate event accessor is used.</span></span> <span data-ttu-id="6e059-1226">Özellikle de atama işleci ise `+=` ekleme erişimcisi kullanılır ve atama işleci ise `-=` ait kaldırma erişimcisi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1226">Specifically, if the assignment operator is `+=` then the add accessor is used, and if the assignment operator is `-=` then the remove accessor is used.</span></span> <span data-ttu-id="6e059-1227">Her iki durumda da atama işlecinin sağ işleneni, bağımsız değişkeni olarak bir olay erişimcisi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1227">In either case, the right-hand operand of the assignment operator is used as the argument to the event accessor.</span></span> <span data-ttu-id="6e059-1228">Bloğunu bir *add_accessor_declaration* veya *remove_accessor_declaration* kurallarına uymalıdır `void` açıklanan yöntemlerden [yöntem gövdesini](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="6e059-1228">The block of an *add_accessor_declaration* or a *remove_accessor_declaration* must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="6e059-1229">Özellikle, `return` ifadeleri gibi bir blok içinde bir ifade belirtin izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="6e059-1229">In particular, `return` statements in such a block are not permitted to specify an expression.</span></span>

<span data-ttu-id="6e059-1230">Bir olay erişimcisi örtük olarak adlı bir parametre olduğundan `value`, yerel bir değişken veya sabit bildirilen bu ada sahip bir olay erişimcisi için bir derleme zamanı hatası olduğu.</span><span class="sxs-lookup"><span data-stu-id="6e059-1230">Since an event accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declared in an event accessor to have that name.</span></span>

<span data-ttu-id="6e059-1231">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-1231">In the example</span></span>
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
<span data-ttu-id="6e059-1232">`Control` sınıfı olaylar için bir iç depolama mekanizması uygular.</span><span class="sxs-lookup"><span data-stu-id="6e059-1232">the `Control` class implements an internal storage mechanism for events.</span></span> <span data-ttu-id="6e059-1233">`AddEventHandler` Yöntemi temsilci değer bir anahtar ile ilişkilendirir `GetEventHandler` yöntem şu anda bir anahtar ile ilişkilendirilmiş olan metot temsilcisinin döndürür ve `RemoveEventHandler` yöntemi, belirtilen olay için bir olay işleyicisi olarak bir temsilci kaldırır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1233">The `AddEventHandler` method associates a delegate value with a key, the `GetEventHandler` method returns the delegate currently associated with a key, and the `RemoveEventHandler` method removes a delegate as an event handler for the specified event.</span></span> <span data-ttu-id="6e059-1234">Yok ilişkilendirmek için ücretsiz, temel depolama mekanizması muhtemelen, tasarlanmış bir `null` değeri bir anahtar ile temsilci olarak seçmeyi ve bu nedenle hiçbir depolama işlenmemiş olayları kullanma.</span><span class="sxs-lookup"><span data-stu-id="6e059-1234">Presumably, the underlying storage mechanism is designed such that there is no cost for associating a `null` delegate value with a key, and thus unhandled events consume no storage.</span></span>

### <a name="static-and-instance-events"></a><span data-ttu-id="6e059-1235">Statik ve örnek olaylar</span><span class="sxs-lookup"><span data-stu-id="6e059-1235">Static and instance events</span></span>

<span data-ttu-id="6e059-1236">Ne zaman bir olay bildirimi içeren bir `static` değiştiricisi, bir olay olarak kabul edilir bir ***statik olay***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1236">When an event declaration includes a `static` modifier, the event is said to be a ***static event***.</span></span> <span data-ttu-id="6e059-1237">Hiçbir `static` değiştirici mevcut, olay olarak kabul edilir bir ***örnek olay***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1237">When no `static` modifier is present, the event is said to be an ***instance event***.</span></span>

<span data-ttu-id="6e059-1238">Statik olay belirli bir örneği ile ilişkili değil ve başvurmak için bir derleme zamanı hata `this` statik bir olayın Erişimcilerde.</span><span class="sxs-lookup"><span data-stu-id="6e059-1238">A static event is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static event.</span></span>

<span data-ttu-id="6e059-1239">Bir örnek olay bir sınıfın belirli bir örneği ile ilişkilendirilir ve bu örnek olarak erişilebilen `this` ([bu erişim](expressions.md#this-access)) Bu olayın Erişimcilerde.</span><span class="sxs-lookup"><span data-stu-id="6e059-1239">An instance event is associated with a given instance of a class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that event.</span></span>

<span data-ttu-id="6e059-1240">Ne zaman bir olay başvuruluyor bir *member_access* ([üye erişimi](expressions.md#member-access)) biçiminde `E.M`, `M` statik bir olay `E` içerenbirtürbelirtmekgerekir`M`ve eğer `M` bir örnek olay E türünü içeren bir örneğini belirtmek gerekir `M`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1240">When an event is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static event, `E` must denote a type containing `M`, and if `M` is an instance event, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="6e059-1241">Statik arasındaki farklar ve örnek üyeleri ele alınmıştır daha ayrıntılı olarak [statik ve örnek üyeleri](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="6e059-1241">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a><span data-ttu-id="6e059-1242">Korumalı sanal, geçersiz kılma ve soyut olay erişimcileri</span><span class="sxs-lookup"><span data-stu-id="6e059-1242">Virtual, sealed, override, and abstract event accessors</span></span>

<span data-ttu-id="6e059-1243">A `virtual` olay bildirimi olay erişimcileri sanal olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1243">A `virtual` event declaration specifies that the accessors of that event are virtual.</span></span> <span data-ttu-id="6e059-1244">`virtual` Değiştiricisi, bir olay erişenleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1244">The `virtual` modifier applies to both accessors of an event.</span></span>

<span data-ttu-id="6e059-1245">Bir `abstract` olay bildirimi, olay erişimcileri sanal ancak gerçek erişimcileri uygulanışı sağlamaz belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1245">An `abstract` event declaration specifies that the accessors of the event are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="6e059-1246">Bunun yerine, türetilen soyut olmayan sınıflar, geçersiz kılarak olay erişimcileri için kendi uygulamasını sağlamak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1246">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the event.</span></span> <span data-ttu-id="6e059-1247">Bir soyut olay bildirimi gerçek uygulaması sağladığından, küme ayracı ayrılmış sağlayamaz *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="6e059-1247">Because an abstract event declaration provides no actual implementation, it cannot provide brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="6e059-1248">Her ikisini de içeren bir olay bildirimi `abstract` ve `override` değiştiriciler olay soyuttur ve temel olayı geçersiz kılmaları belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1248">An event declaration that includes both the `abstract` and `override` modifiers specifies that the event is abstract and overrides a base event.</span></span> <span data-ttu-id="6e059-1249">Böyle bir olay erişimci de soyut.</span><span class="sxs-lookup"><span data-stu-id="6e059-1249">The accessors of such an event are also abstract.</span></span>

<span data-ttu-id="6e059-1250">Soyut olay bildirimleri yalnızca soyut sınıfları izin ([soyut sınıflar](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1250">Abstract event declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="6e059-1251">Devralınan sanal olay erişimcileri belirten bir olay bildirimi dahil olmak üzere türetilen bir sınıfta kılınabilir bir `override` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1251">The accessors of an inherited virtual event can be overridden in a derived class by including an event declaration that specifies an `override` modifier.</span></span> <span data-ttu-id="6e059-1252">Bu olarak bilinen bir ***olay bildirimi geçersiz kılma***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1252">This is known as an ***overriding event declaration***.</span></span> <span data-ttu-id="6e059-1253">Yeni bir olay Olay bildiriminde geçersiz kılma bildirmiyor.</span><span class="sxs-lookup"><span data-stu-id="6e059-1253">An overriding event declaration does not declare a new event.</span></span> <span data-ttu-id="6e059-1254">Bunun yerine, mevcut bir sanal olay erişimcileri, uygulamaları yalnızca uzmanlaşmış.</span><span class="sxs-lookup"><span data-stu-id="6e059-1254">Instead, it simply specializes the implementations of the accessors of an existing virtual event.</span></span>

<span data-ttu-id="6e059-1255">Olay bildiriminde geçersiz kılma tam aynı erişilebilirlik değiştiricileri, türü ve adı geçersiz kılınan olay belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1255">An overriding event declaration must specify the exact same accessibility modifiers, type, and name as the overridden event.</span></span>

<span data-ttu-id="6e059-1256">Bir geçersiz kılma olay bildirimi içerebilir `sealed` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1256">An overriding event declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="6e059-1257">Bu değiştirici kullanımını türetilmiş bir sınıf daha fazla olay geçersiz kılmasını önler.</span><span class="sxs-lookup"><span data-stu-id="6e059-1257">Use of this modifier prevents a derived class from further overriding the event.</span></span> <span data-ttu-id="6e059-1258">Ayrıca korumalı bir korumalı olay erişimcileri.</span><span class="sxs-lookup"><span data-stu-id="6e059-1258">The accessors of a sealed event are also sealed.</span></span>

<span data-ttu-id="6e059-1259">Bir derleme zamanı hata dahil etmek bir geçersiz kılma olay bildirimi için bir `new` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1259">It is a compile-time error for an overriding event declaration to include a `new` modifier.</span></span>

<span data-ttu-id="6e059-1260">Bildirim ve çağırma farklılıkları dışında sözdizimi, sanal, sealed bir geçersiz kılma ve soyut erişimcileri tam olarak sanal, sealed bir geçersiz kılma ve Özet yöntemler gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1260">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="6e059-1261">Kuralları açıklanan özellikle [sanal yöntemleri](classes.md#virtual-methods), [geçersiz kılma yöntemleri](classes.md#override-methods), [yöntemleri korumalı](classes.md#sealed-methods), ve [soyut yöntemler](classes.md#abstract-methods) uygulamak gibi erişimciler yöntemleri karşılık gelen bir formun yoktu.</span><span class="sxs-lookup"><span data-stu-id="6e059-1261">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form.</span></span> <span data-ttu-id="6e059-1262">Olay türü, bir tek değer parametresi olan bir yönteme her erişimci karşılık gelen bir `void` dönüş türü ve içeren olayla aynı değiştiriciler.</span><span class="sxs-lookup"><span data-stu-id="6e059-1262">Each accessor corresponds to a method with a single value parameter of the event type, a `void` return type, and the same modifiers as the containing event.</span></span>

## <a name="indexers"></a><span data-ttu-id="6e059-1263">Dizin Oluşturucular</span><span class="sxs-lookup"><span data-stu-id="6e059-1263">Indexers</span></span>

<span data-ttu-id="6e059-1264">Bir ***dizin oluşturucu*** bir nesne dizisi olarak aynı şekilde dizinlenmesini sağlar bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1264">An ***indexer*** is a member that enables an object to be indexed in the same way as an array.</span></span> <span data-ttu-id="6e059-1265">Dizin oluşturucular kullanarak bildirilir *indexer_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="6e059-1265">Indexers are declared using *indexer_declaration*s:</span></span>

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

<span data-ttu-id="6e059-1266">Bir *indexer_declaration* bir dizi içerebilir *öznitelikleri* ([öznitelikleri](attributes.md)) ve geçerli bir bileşimin dört erişim değiştiricileri ([erişim değiştiricileri ](classes.md#access-modifiers)), `new` ([yeni değiştiricisini](classes.md#the-new-modifier)), `virtual` ([sanal yöntemleri](classes.md#virtual-methods)), `override` ([geçersiz kılma yöntemleri](classes.md#override-methods) ), `sealed` ([Yöntemleri korumalı](classes.md#sealed-methods)), `abstract` ([soyut yöntemler](classes.md#abstract-methods)), ve `extern` ([dış yöntemleri](classes.md#external-methods)) değiştiriciler.</span><span class="sxs-lookup"><span data-stu-id="6e059-1266">An *indexer_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="6e059-1267">Dizin Oluşturucu bildirimlerdir yöntem bildirimleri gibi aynı kurallara tabi ([yöntemleri](classes.md#methods)) değiştiriciler geçerli birleşimlerini onaylamaz, statik değiştirici olan bir özel durumla bir dizin oluşturucu bildiriminde izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="6e059-1267">Indexer declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers, with the one exception being that the static modifier is not permitted on an indexer declaration.</span></span>

<span data-ttu-id="6e059-1268">Değiştiriciler `virtual`, `override`, ve `abstract` bir durumda dışında kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-1268">The modifiers `virtual`, `override`, and `abstract` are mutually exclusive except in one case.</span></span> <span data-ttu-id="6e059-1269">`abstract` Ve `override` değiştiriciler kullanılabilir birlikte böylece sanal bir soyut bir dizin oluşturucu geçersiz kılabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6e059-1269">The `abstract` and `override` modifiers may be used together so that an abstract indexer can override a virtual one.</span></span>

<span data-ttu-id="6e059-1270">*Türü* bir dizin oluşturucu bildirim bildirim tarafından tanıtılan dizin oluşturucu öğenin türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1270">The *type* of an indexer declaration specifies the element type of the indexer introduced by the declaration.</span></span> <span data-ttu-id="6e059-1271">Açık arabirim üyesi uygulaması, dizin oluşturucu olmadığı sürece *türü* anahtar sözcüğü tarafından izlenen `this`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1271">Unless the indexer is an explicit interface member implementation, the *type* is followed by the keyword `this`.</span></span> <span data-ttu-id="6e059-1272">Bir açık arabirim üyesi uygulaması *türü* tarafından izlenen bir *INTERFACE_TYPE*, "`.`" ve anahtar sözcüğü `this`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1272">For an explicit interface member implementation, the *type* is followed by an *interface_type*, a "`.`", and the keyword `this`.</span></span> <span data-ttu-id="6e059-1273">Diğer üyeleri farklı olarak, dizin oluşturucular kullanıcı tanımlı adları yok.</span><span class="sxs-lookup"><span data-stu-id="6e059-1273">Unlike other members, indexers do not have user-defined names.</span></span>

<span data-ttu-id="6e059-1274">*Formal_parameter_list* dizin oluşturucunun parametreleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1274">The *formal_parameter_list* specifies the parameters of the indexer.</span></span> <span data-ttu-id="6e059-1275">Bir dizin oluşturucu biçimsel parametre listesi, yöntemin karşılık gelir ([yöntem parametreleri](classes.md#method-parameters)) en az bir parametresi belirtilmelidir, dışında ve `ref` ve `out` parametre değiştiricilere izin verilmez .</span><span class="sxs-lookup"><span data-stu-id="6e059-1275">The formal parameter list of an indexer corresponds to that of a method ([Method parameters](classes.md#method-parameters)), except that at least one parameter must be specified, and that the `ref` and `out` parameter modifiers are not permitted.</span></span>

<span data-ttu-id="6e059-1276">*Türü* bir dizin oluşturucu ve her başvurulan tür *formal_parameter_list* en az Indexer olarak erişilebilir olmalıdır ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1276">The *type* of an indexer and each of the types referenced in the *formal_parameter_list* must be at least as accessible as the indexer itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6e059-1277">Bir *indexer_body* herhangi birini içerebilir bir ***erişimcisinin gövdesi*** veya ***ifade gövdesi***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1277">An *indexer_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="6e059-1278">Bir erişimci gövdesindeki *accessor_declarations*, hangi alınmalıdır içinde "`{`"ve"`}`" belirteçleri, erişimci bildirir ([erişimcileri](classes.md#accessors)) özelliği.</span><span class="sxs-lookup"><span data-stu-id="6e059-1278">In an accessor body, *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="6e059-1279">Erişimciler okuma ve yazma özelliği ilişkili yürütülebilir deyimleri belirtin.</span><span class="sxs-lookup"><span data-stu-id="6e059-1279">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="6e059-1280">Oluşan bir deyim gövdesinin "`=>`" bir ifade tarafından izlenen `E` ve noktalı virgül deyim gövdesi için tam olarak eşdeğerdir `{ get { return E; } }`ve bu nedenle yalnızca yalnızca dizin oluşturucular belirtmek için alıcının sonucu olduğu kullanılabilir tek bir ifade belirler.</span><span class="sxs-lookup"><span data-stu-id="6e059-1280">An expression body consisting of "`=>`" followed by an expression `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only indexers where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="6e059-1281">Dizin Oluşturucu öğenin erişmek için söz dizimi aynı dizi öğesi için olsa da, bir değişken olarak bir dizin oluşturucu öğenin sınıflandırılmaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-1281">Even though the syntax for accessing an indexer element is the same as that for an array element, an indexer element is not classified as a variable.</span></span> <span data-ttu-id="6e059-1282">Bu nedenle, dizin oluşturucu öğenin olarak geçirmek mümkün değil bir `ref` veya `out` bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="6e059-1282">Thus, it is not possible to pass an indexer element as a `ref` or `out` argument.</span></span>

<span data-ttu-id="6e059-1283">İmza biçimsel parametre listesinde bir dizin oluşturucu tanımlar ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)) dizin oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="6e059-1283">The formal parameter list of an indexer defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the indexer.</span></span> <span data-ttu-id="6e059-1284">Özellikle, bir dizin oluşturucu imzası, sayı ve biçimsel parametrelerinin türleri oluşur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1284">Specifically, the signature of an indexer consists of the number and types of its formal parameters.</span></span> <span data-ttu-id="6e059-1285">Öğe türü ve biçimsel parametrelerinin adları bir oluşturucunun imzasının parçası değildir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1285">The element type and names of the formal parameters are not part of an indexer's signature.</span></span>

<span data-ttu-id="6e059-1286">Bir dizin oluşturucu imzasının aynı sınıfta bildirilen tüm diğer dizin imzalarını farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1286">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>

<span data-ttu-id="6e059-1287">Dizin oluşturucular ve özellikler çok benzer, ancak şu bakımlardan ayrılır:</span><span class="sxs-lookup"><span data-stu-id="6e059-1287">Indexers and properties are very similar in concept, but differ in the following ways:</span></span>

*  <span data-ttu-id="6e059-1288">Bir dizin oluşturucu imzası tarafından tanımlanan ise özellik sunucu adına göre tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1288">A property is identified by its name, whereas an indexer is identified by its signature.</span></span>
*  <span data-ttu-id="6e059-1289">Bir özellik üzerinden erişilen bir *simple_name* ([basit adları](expressions.md#simple-names)) veya bir *member_access* ([üye erişimi](expressions.md#member-access)), ancak bir dizin oluşturucu öğesi aracılığıyla erişilen bir *element_access* ([dizinleyici erişimi](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1289">A property is accessed through a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)), whereas an indexer element is accessed through an *element_access* ([Indexer access](expressions.md#indexer-access)).</span></span>
*  <span data-ttu-id="6e059-1290">Bir özellik ayarlanabilir bir `static` üyesi, ancak dizin oluşturucunun her zaman bir örnek üyesi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1290">A property can be a `static` member, whereas an indexer is always an instance member.</span></span>
*  <span data-ttu-id="6e059-1291">A `get` özellik erişimcisi karşılık gelen bir yönteme parametre olmadan, ancak bir `get` erişimci bir dizin oluşturucu aynı dizin oluşturucu biçimsel parametre listesine sahip bir yöntemi karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1291">A `get` accessor of a property corresponds to a method with no parameters, whereas a `get` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer.</span></span>
*  <span data-ttu-id="6e059-1292">A `set` özellik erişimcisi karşılık gelen bir yönteme adlı tek bir parametre `value`bilgileriyse bir `set` bir yöntemle aynı biçimsel parametre listesi dizin oluşturucu yanı sıra, ek bir parametre olarak bir dizin oluşturucu erişimcisine karşılık gelir adlı `value`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1292">A `set` accessor of a property corresponds to a method with a single parameter named `value`, whereas a `set` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer, plus an additional parameter named `value`.</span></span>
*  <span data-ttu-id="6e059-1293">Dizin Oluşturucu parametresi olarak aynı ada sahip yerel bir değişken bildirmek bir dizin oluşturucu erişimcisi için bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1293">It is a compile-time error for an indexer accessor to declare a local variable with the same name as an indexer parameter.</span></span>
*  <span data-ttu-id="6e059-1294">Bir geçersiz kılma özelliği bildiriminde devralınan özelliği söz dizimi kullanılarak erişilen `base.P`burada `P` özellik adı.</span><span class="sxs-lookup"><span data-stu-id="6e059-1294">In an overriding property declaration, the inherited property is accessed using the syntax `base.P`, where `P` is the property name.</span></span> <span data-ttu-id="6e059-1295">Geçersiz kılan bir dizin oluşturucu bildiriminde devralınan dizin oluşturucu sözdizimi kullanılarak erişilir `base[E]`burada `E` ifadelerin virgülle ayrılmış listesidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1295">In an overriding indexer declaration, the inherited indexer is accessed using the syntax `base[E]`, where `E` is a comma separated list of expressions.</span></span>
*  <span data-ttu-id="6e059-1296">"Otomatik olarak uygulanan dizin oluşturucu" kavramı yoktur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1296">There is no concept of an "automatically implemented indexer".</span></span> <span data-ttu-id="6e059-1297">Noktalı virgül erişimcileri ile soyut olmayan, dış olmayan bir dizinleyici olması hatadır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1297">It is an error to have a non-abstract, non-external indexer with semicolon accessors.</span></span>

<span data-ttu-id="6e059-1298">Tüm kuralları aşağıdaki farklar dışında tanımlanan [erişimcileri](classes.md#accessors) ve [Özellikleri'otomatik olarak uygulanan](classes.md#automatically-implemented-properties) dizin oluşturucu erişimcileri de özellik erişimcileri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1298">Aside from these differences, all rules defined in [Accessors](classes.md#accessors) and [Automatically implemented properties](classes.md#automatically-implemented-properties) apply to indexer accessors as well as to property accessors.</span></span>

<span data-ttu-id="6e059-1299">Ne zaman bir dizin oluşturucu bildirimi içeren bir `extern` değiştiricisi, dizin oluşturucu olduğu söylenir bir ***dış dizin oluşturucu***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1299">When an indexer declaration includes an `extern` modifier, the indexer is said to be an ***external indexer***.</span></span> <span data-ttu-id="6e059-1300">Hiçbir gerçek uygulama, her biri bir dış dizin oluşturucu bildirimi sağladığından, *accessor_declarations* noktalı virgül içerir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1300">Because an external indexer declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

<span data-ttu-id="6e059-1301">Aşağıdaki örnekte bildiren bir `BitArray` bit dizideki her biti erişmek için bir dizin oluşturucu uygulayan sınıf.</span><span class="sxs-lookup"><span data-stu-id="6e059-1301">The example below declares a `BitArray` class that implements an indexer for accessing the individual bits in the bit array.</span></span>
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

<span data-ttu-id="6e059-1302">Örneği `BitArray` sınıfı, karşılık gelen daha önemli ölçüde daha az bellek tüketir `bool[]` (önceki her değeri yerine bir bit kapladığı olduğundan ikinci bir bayt kullanıcının), ancak aynı işlemlere izin bir `bool[]`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1302">An instance of the `BitArray` class consumes substantially less memory than a corresponding `bool[]` (since each value of the former occupies only one bit instead of the latter's one byte), but it permits the same operations as a `bool[]`.</span></span>

<span data-ttu-id="6e059-1303">Aşağıdaki `CountPrimes` sınıfını kullanan bir `BitArray` ve belirli en fazla 1 ila primes sayısını hesaplamak için Klasik olan "sieve" algoritması:</span><span class="sxs-lookup"><span data-stu-id="6e059-1303">The following `CountPrimes` class uses a `BitArray` and the classical "sieve" algorithm to compute the number of primes between 1 and a given maximum:</span></span>
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

<span data-ttu-id="6e059-1304">Unutmayın öğeleri erişmek için söz dizimi `BitArray` tam olarak aynı olan bir `bool[]`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1304">Note that the syntax for accessing elements of the `BitArray` is precisely the same as for a `bool[]`.</span></span>

<span data-ttu-id="6e059-1305">Aşağıdaki örnek, iki parametre ile bir dizin oluşturucu sahip bir 26 \* 10 kılavuz sınıfı gösterir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1305">The following example shows a 26 \* 10 grid class that has an indexer with two parameters.</span></span> <span data-ttu-id="6e059-1306">İlk parametre bir üst - veya A-Z aralığındaki küçük harf olması gerekir ve ikinci 0-9 aralığında bir tamsayı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1306">The first parameter is required to be an upper- or lowercase letter in the range A-Z, and the second is required to be an integer in the range 0-9.</span></span>

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

### <a name="indexer-overloading"></a><span data-ttu-id="6e059-1307">Dizin oluşturucu aşırı yüklemesi</span><span class="sxs-lookup"><span data-stu-id="6e059-1307">Indexer overloading</span></span>

<span data-ttu-id="6e059-1308">Dizin oluşturucu aşırı yükleme çözünürlüğü kuralları açıklanan [anlam çıkarma](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="6e059-1308">The indexer overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="operators"></a><span data-ttu-id="6e059-1309">İşleçler</span><span class="sxs-lookup"><span data-stu-id="6e059-1309">Operators</span></span>

<span data-ttu-id="6e059-1310">Bir ***işleci*** sınıf örnekleri için uygulanabilir bir ifade işleci anlamını tanımlayan bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1310">An ***operator*** is a member that defines the meaning of an expression operator that can be applied to instances of the class.</span></span> <span data-ttu-id="6e059-1311">İşleçleri kullanarak bildirilir *operator_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="6e059-1311">Operators are declared using *operator_declaration*s:</span></span>

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
    | 'right_shift' | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
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

<span data-ttu-id="6e059-1312">Fazla yüklenebilir işleçler üç kategorisi vardır: tekli işleçler ([birli işleçler](classes.md#unary-operators)), ikili işleçler ([ikili işleçler](classes.md#binary-operators)) ve dönüştürme işleçleri ([dönüştürme işleçleri ](classes.md#conversion-operators)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1312">There are three categories of overloadable operators: Unary operators ([Unary operators](classes.md#unary-operators)), binary operators ([Binary operators](classes.md#binary-operators)), and conversion operators ([Conversion operators](classes.md#conversion-operators)).</span></span>

<span data-ttu-id="6e059-1313">*Operator_body* ya da bir noktalı virgüldür bir ***deyim gövdesi*** veya ***ifade gövdesi***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1313">The *operator_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="6e059-1314">Deyim gövdesi oluşan bir *blok*, işleci çağrıldığında çalıştırılacak deyimleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1314">A statement body consists of a *block*, which specifies the statements to execute when the operator is invoked.</span></span> <span data-ttu-id="6e059-1315">*Blok* değeri döndürmek için kurallarına uymalıdır açıklanan yöntemlerden [yöntem gövdesini](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="6e059-1315">The *block* must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="6e059-1316">İfade gövdesi oluşan `=>` bir ifade ve noktalı virgül tarafından izlenen ve tek bir ifade işleci çağrıldığında gerçekleştirilecek gösterir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1316">An expression body consists of `=>` followed by an expression and a semicolon, and denotes a single expression to perform when the operator is invoked.</span></span>

<span data-ttu-id="6e059-1317">İçin `extern` işleçleri *operator_body* yalnızca noktalı virgül içerir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1317">For `extern` operators, the *operator_body* consists simply of a semicolon.</span></span> <span data-ttu-id="6e059-1318">Diğer işleçler için *operator_body* blok gövdesi ya da bir deyim gövdesi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1318">For all other operators, the *operator_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="6e059-1319">Tüm işleci bildirimi aşağıdaki kurallar geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="6e059-1319">The following rules apply to all operator declarations:</span></span>

*  <span data-ttu-id="6e059-1320">İşleç bildirimi hem de içermelidir bir `public` ve `static` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1320">An operator declaration must include both a `public` and a `static` modifier.</span></span>
*  <span data-ttu-id="6e059-1321">Parametre bir işlecin değeri parametreler olmalıdır ([değer parametreleri](variables.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1321">The parameter(s) of an operator must be value parameters ([Value parameters](variables.md#value-parameters)).</span></span> <span data-ttu-id="6e059-1322">Bir derleme zamanı hata belirtmek bir işleç bildirimi için `ref` veya `out` parametreleri.</span><span class="sxs-lookup"><span data-stu-id="6e059-1322">It is a compile-time error for an operator declaration to specify `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="6e059-1323">Bir işleç imzası ([birli işleçler](classes.md#unary-operators), [ikili işleçler](classes.md#binary-operators), [dönüştürme işleçleri](classes.md#conversion-operators)) bildirilen tüm diğer işleçlerin imzalarını farklı olmalıdır aynı sınıf.</span><span class="sxs-lookup"><span data-stu-id="6e059-1323">The signature of an operator ([Unary operators](classes.md#unary-operators), [Binary operators](classes.md#binary-operators), [Conversion operators](classes.md#conversion-operators)) must differ from the signatures of all other operators declared in the same class.</span></span>
*  <span data-ttu-id="6e059-1324">Bir operatörü bildiriminde başvurulan tüm türleri en azından operatör olarak olarak erişilebilir olmalıdır ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1324">All types referenced in an operator declaration must be at least as accessible as the operator itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>
*  <span data-ttu-id="6e059-1325">Aynı değiştiricisi bir operatörü bildiriminde birden çok kez görünmesi için bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1325">It is an error for the same modifier to appear multiple times in an operator declaration.</span></span>

<span data-ttu-id="6e059-1326">Her işleç kategorisi, aşağıdaki bölümlerde açıklandığı gibi ek kısıtlamalar getirir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1326">Each operator category imposes additional restrictions, as described in the following sections.</span></span>

<span data-ttu-id="6e059-1327">Diğer üyeler gibi bir temel sınıfta bildirilen işleçleri türetilmiş sınıflar tarafından devralınır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1327">Like other members, operators declared in a base class are inherited by derived classes.</span></span> <span data-ttu-id="6e059-1328">İşleç bildirimi sınıfın veya yapının işlecin işleci imzasında katılmak bildirildiği her zaman gerekli olduğundan, bir temel sınıfta bildirilen bir işleç gizlemek türetilen bir sınıfta bildirilen bir işleç için mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1328">Because operator declarations always require the class or struct in which the operator is declared to participate in the signature of the operator, it is not possible for an operator declared in a derived class to hide an operator declared in a base class.</span></span> <span data-ttu-id="6e059-1329">Bu nedenle, `new` değiştiricisi hiçbir zaman gerekli ve bu nedenle hiçbir zaman, bir işleç bildiriminde izin verilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1329">Thus, the `new` modifier is never required, and therefore never permitted, in an operator declaration.</span></span>

<span data-ttu-id="6e059-1330">Tekli veya ikili işleçler hakkında daha fazla bilgi bulunabilir [işleçleri](expressions.md#operators).</span><span class="sxs-lookup"><span data-stu-id="6e059-1330">Additional information on unary and binary operators can be found in [Operators](expressions.md#operators).</span></span>

<span data-ttu-id="6e059-1331">Dönüştürme işleçleri hakkında daha fazla bilgi bulunabilir [kullanıcı tanımlı dönüşümler](conversions.md#user-defined-conversions).</span><span class="sxs-lookup"><span data-stu-id="6e059-1331">Additional information on conversion operators can be found in [User-defined conversions](conversions.md#user-defined-conversions).</span></span>

### <a name="unary-operators"></a><span data-ttu-id="6e059-1332">Birli işleçler</span><span class="sxs-lookup"><span data-stu-id="6e059-1332">Unary operators</span></span>

<span data-ttu-id="6e059-1333">Birli işleç bildirimi, aşağıdaki kurallar geçerli olduğu `T` sınıfın veya yapının işleç bildirimi içeren örnek türünü gösterir:</span><span class="sxs-lookup"><span data-stu-id="6e059-1333">The following rules apply to unary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="6e059-1334">Bir birli `+`, `-`, `!`, veya `~` işleci türünde tek bir parametre gerçekleştirmeniz gereken `T` veya `T?` ve her türlü döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1334">A unary `+`, `-`, `!`, or `~` operator must take a single parameter of type `T` or `T?` and can return any type.</span></span>
*  <span data-ttu-id="6e059-1335">Bir birli `++` veya `--` işleci türünde tek bir parametre gerçekleştirmeniz gereken `T` veya `T?` ve aynı türü veya tür bundan türetilmiş döndürmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1335">A unary `++` or `--` operator must take a single parameter of type `T` or `T?` and must return that same type or a type derived from it.</span></span>
*  <span data-ttu-id="6e059-1336">Bir birli `true` veya `false` işleci türünde tek bir parametre gerçekleştirmeniz gereken `T` veya `T?` ve türü döndürmelidir `bool`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1336">A unary `true` or `false` operator must take a single parameter of type `T` or `T?` and must return type `bool`.</span></span>

<span data-ttu-id="6e059-1337">İmza birli işleç işleç belirtecini oluşur (`+`, `-`, `!`, `~`, `++`, `--`, `true`, veya `false`) ve tek bir biçimsel parametrenin türü.</span><span class="sxs-lookup"><span data-stu-id="6e059-1337">The signature of a unary operator consists of the operator token (`+`, `-`, `!`, `~`, `++`, `--`, `true`, or `false`) and the type of the single formal parameter.</span></span> <span data-ttu-id="6e059-1338">Dönüş türü bir birli işlecin imzasının parçası değil veya biçimsel parametrenin adı değil.</span><span class="sxs-lookup"><span data-stu-id="6e059-1338">The return type is not part of a unary operator's signature, nor is the name of the formal parameter.</span></span>

<span data-ttu-id="6e059-1339">`true` Ve `false` birli işleçler pair-wise bildirimi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1339">The `true` and `false` unary operators require pair-wise declaration.</span></span> <span data-ttu-id="6e059-1340">Bir sınıf diğerini de bildirmeden bu işleçlerden birini bildiriyorsa, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1340">A compile-time error occurs if a class declares one of these operators without also declaring the other.</span></span> <span data-ttu-id="6e059-1341">`true` Ve `false` işleçler şunlardır: daha ayrıntılı açıklanır [kullanıcı tanımlı koşullu mantıksal işleçler](expressions.md#user-defined-conditional-logical-operators) ve [Boolean ifadeler](expressions.md#boolean-expressions).</span><span class="sxs-lookup"><span data-stu-id="6e059-1341">The `true` and `false` operators are described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators) and [Boolean expressions](expressions.md#boolean-expressions).</span></span>

<span data-ttu-id="6e059-1342">Aşağıdaki örnek, bir uygulama ve sonraki kullanımını gösterir. `operator ++` bir tamsayı vektörü sınıfı için:</span><span class="sxs-lookup"><span data-stu-id="6e059-1342">The following example shows an implementation and subsequent usage of `operator ++` for an integer vector class:</span></span>
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

<span data-ttu-id="6e059-1343">Ne işleci yöntemi yalnızca sonek artırma gibi işlenenin 1 ekleyerek üretilen değerini döndürür unutmayın ve azaltma işleçleri ([sonek arttırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators)) ve ön ek arttırma ve azaltma işleçler ([önek arttırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1343">Note how the operator method returns the value produced by adding 1 to the operand, just like the  postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)), and the prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="6e059-1344">Farklı C++'da, bu yöntem, işleneninin değeri doğrudan değişiklik.</span><span class="sxs-lookup"><span data-stu-id="6e059-1344">Unlike in C++, this method need not modify the value of its operand directly.</span></span> <span data-ttu-id="6e059-1345">Aslında, işlenenin değerinin değiştirme sonek artırma işlecini standart semantiği ihlal ediyor.</span><span class="sxs-lookup"><span data-stu-id="6e059-1345">In fact, modifying the operand value would violate the standard semantics of the postfix increment operator.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="6e059-1346">İkili işleçler</span><span class="sxs-lookup"><span data-stu-id="6e059-1346">Binary operators</span></span>

<span data-ttu-id="6e059-1347">İkili işleç bildirimi, aşağıdaki kurallar geçerli olduğu `T` sınıfın veya yapının işleç bildirimi içeren örnek türünü gösterir:</span><span class="sxs-lookup"><span data-stu-id="6e059-1347">The following rules apply to binary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="6e059-1348">Bir ikili olmayan kaydırma işleci, en az biri türünde olmalıdır, iki parametre almalıdır `T` veya `T?`ve her türlü döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1348">A binary non-shift operator must take two parameters, at least one of which must have type `T` or `T?`, and can return any type.</span></span>
*  <span data-ttu-id="6e059-1349">Bir ikili `<<` veya `>>` işleci iki parametre ilki türünde olmalıdır, gerçekleştirmeniz gereken `T` veya `T?` ve ikinci türüne sahip olmalıdır `int` veya `int?`ve her türlü döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1349">A binary `<<` or `>>` operator must take two parameters, the first of which must have type `T` or `T?` and the second of which must have type `int` or `int?`, and can return any type.</span></span>

<span data-ttu-id="6e059-1350">İkili işleç imzası işleç belirteci oluşur (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, veya `<=`) ve iki biçimsel parametre türleri.</span><span class="sxs-lookup"><span data-stu-id="6e059-1350">The signature of a binary operator consists of the operator token (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, or `<=`) and the types of the two formal parameters.</span></span> <span data-ttu-id="6e059-1351">Dönüş türünü ve biçimsel parametrelerinin adları bir ikili işlecin imzasının parçası değildir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1351">The return type and the names of the formal parameters are not part of a binary operator's signature.</span></span>

<span data-ttu-id="6e059-1352">Bazı ikili işleçlerin pair-wise bildirimi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1352">Certain binary operators require pair-wise declaration.</span></span> <span data-ttu-id="6e059-1353">Her bir çifti ya da işleç bildirimi için eşleşen bir bildirim çiftinin başka bir işleç olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1353">For every declaration of either operator of a pair, there must be a matching declaration of the other operator of the pair.</span></span> <span data-ttu-id="6e059-1354">Aynı dönüş türü ve her parametre için aynı türe sahip olduğunda iki işleç bildirimi eşleştirin.</span><span class="sxs-lookup"><span data-stu-id="6e059-1354">Two operator declarations match when they have the same return type and the same type for each parameter.</span></span> <span data-ttu-id="6e059-1355">Aşağıdaki işleçleri pair-wise bildirimi gerektirir:</span><span class="sxs-lookup"><span data-stu-id="6e059-1355">The following operators require pair-wise declaration:</span></span>

*  <span data-ttu-id="6e059-1356">`operator ==` ve `operator !=`</span><span class="sxs-lookup"><span data-stu-id="6e059-1356">`operator ==` and `operator !=`</span></span>
*  <span data-ttu-id="6e059-1357">`operator >` ve `operator <`</span><span class="sxs-lookup"><span data-stu-id="6e059-1357">`operator >` and `operator <`</span></span>
*  <span data-ttu-id="6e059-1358">`operator >=` ve `operator <=`</span><span class="sxs-lookup"><span data-stu-id="6e059-1358">`operator >=` and `operator <=`</span></span>

### <a name="conversion-operators"></a><span data-ttu-id="6e059-1359">Dönüştürme işleçleri</span><span class="sxs-lookup"><span data-stu-id="6e059-1359">Conversion operators</span></span>

<span data-ttu-id="6e059-1360">Bir dönüştürme işleci bildirimi sunar bir ***kullanıcı tanımlı dönüştürme*** ([kullanıcı tanımlı dönüşümler](conversions.md#user-defined-conversions)) önceden tanımlı örtük ve açık dönüştürmeler artırmaktadır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1360">A conversion operator declaration introduces a ***user-defined conversion*** ([User-defined conversions](conversions.md#user-defined-conversions)) which augments the pre-defined implicit and explicit conversions.</span></span>

<span data-ttu-id="6e059-1361">İçeren bir dönüştürme işleci bildirimi `implicit` anahtar sözcüğü, bir kullanıcı tanımlı bir örtük dönüştürme tanıtır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1361">A conversion operator declaration that includes the `implicit` keyword introduces a user-defined implicit conversion.</span></span> <span data-ttu-id="6e059-1362">Örtük dönüştürmeleri durumlarda, işlev üyesi çağrılarını cast ifadeleri ve atamaları da dahil olmak üzere çeşitli içinde ortaya çıkabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1362">Implicit conversions can occur in a variety of situations, including function member invocations, cast expressions, and assignments.</span></span> <span data-ttu-id="6e059-1363">Daha ayrıntılı açıklanır budur [örtük dönüştürmelerin](conversions.md#implicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="6e059-1363">This is described further in [Implicit conversions](conversions.md#implicit-conversions).</span></span>

<span data-ttu-id="6e059-1364">İçeren bir dönüştürme işleci bildirimi `explicit` anahtar sözcüğü, bir kullanıcı tanımlı açık dönüştürme tanıtır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1364">A conversion operator declaration that includes the `explicit` keyword introduces a user-defined explicit conversion.</span></span> <span data-ttu-id="6e059-1365">Açık dönüştürmeler cast ifadeleri oluşabilir ve daha ayrıntılı açıklanır [açık dönüştürmeler](conversions.md#explicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="6e059-1365">Explicit conversions can occur in cast expressions, and are described further in [Explicit conversions](conversions.md#explicit-conversions).</span></span>

<span data-ttu-id="6e059-1366">Bir kaynak türünden dönüştürme işlecinin dönüş türü tarafından belirtilen bir hedef türüne dönüştürme işlecinin parametre türü tarafından belirtilen bir dönüştürme operatörünün dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="6e059-1366">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span>

<span data-ttu-id="6e059-1367">Belirtilen kaynak türü için `S` ve hedef türü `T`, `S` veya `T` boş değer atanabilir türler izin `S0` ve `T0` Aksi takdirde, temel alınan türler için başvuru `S0` ve `T0` olan eşit `S` ve `T` sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="6e059-1367">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="6e059-1368">Bir kaynak türünden dönüştürme bildirmek için bir sınıf veya yapı izin `S` hedef türüyle `T` yalnızca aşağıdakilerin tümü doğru olduğunda:</span><span class="sxs-lookup"><span data-stu-id="6e059-1368">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="6e059-1369">`S0` ve `T0` farklı tür aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1369">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="6e059-1370">Ya da `S0` veya `T0` işleç bildirimi yer alan sınıf veya yapı türü.</span><span class="sxs-lookup"><span data-stu-id="6e059-1370">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="6e059-1371">Ne `S0` ya da `T0` olduğu bir *INTERFACE_TYPE*.</span><span class="sxs-lookup"><span data-stu-id="6e059-1371">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="6e059-1372">Kullanıcı tanımlı dönüşümler, bir dönüştürme yok gelen `S` için `T` veya `T` için `S`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1372">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="6e059-1373">Bu kurallar amaçları için ilişkili parametreler girin `S` veya `T` diğer tür devralma ilişkisi yok ve bu tür parametreleri yok sayılır tüm kısıtlamalar benzersiz tür olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1373">For the purposes of these rules, any type parameters associated with `S` or `T` are considered to be unique types that have no inheritance relationship with other types, and any constraints on those type parameters are ignored.</span></span>

<span data-ttu-id="6e059-1374">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-1374">In the example</span></span>
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
<span data-ttu-id="6e059-1375">ilk iki işleç bildirimleri için izin verilen amacıyla [dizin oluşturucular](classes.md#indexers).3, `T` ve `int` ve `string` sırasıyla ile hiçbir ilişkisi benzersiz tür olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1375">the first two operator declarations are permitted because, for the purposes of [Indexers](classes.md#indexers).3, `T` and `int` and `string` respectively are considered unique types with no relationship.</span></span> <span data-ttu-id="6e059-1376">Ancak, üçüncü bir hata nedeniyle işlecidir `C<T>` taban sınıfıdır `D<T>`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1376">However, the third operator is an error because `C<T>` is the base class of `D<T>`.</span></span>

<span data-ttu-id="6e059-1377">İçin veya işlecin bildirildiği sınıf veya yapı türü, takip eden ikinci kuralından bir dönüştürme operatörünün dönüştürmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1377">From the second rule it follows that a conversion operator must convert either to or from the class or struct type in which the operator is declared.</span></span> <span data-ttu-id="6e059-1378">Örneğin, bir sınıf veya yapı türü için olası `C` dönüştürme tanımlamak için `C` için `int` ve `int` için `C`, ancak `int` için `bool`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1378">For example, it is possible for a class or struct type `C` to define a conversion from `C` to `int` and from `int` to `C`, but not from `int` to `bool`.</span></span>

<span data-ttu-id="6e059-1379">Önceden tanımlı bir dönüştürme doğrudan tanımlanacak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1379">It is not possible to directly redefine a pre-defined conversion.</span></span> <span data-ttu-id="6e059-1380">Bu nedenle, dönüştürme işleçleri veya dönüştürmeye izin verilmez `object` arasında örtük ve açık dönüştürmeler olduğundan `object` ve diğer tüm türleri.</span><span class="sxs-lookup"><span data-stu-id="6e059-1380">Thus, conversion operators are not allowed to convert from or to `object` because implicit and explicit conversions already exist between `object` and all other types.</span></span> <span data-ttu-id="6e059-1381">Benzer şekilde, kaynak ya da hedef tür bir dönüştürme dönüştürme ardından zaten var olduğundan, diğer bir taban türü olabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1381">Likewise, neither the source nor the target types of a conversion can be a base type of the other, since a conversion would then already exist.</span></span>

<span data-ttu-id="6e059-1382">Ancak, belirttiğiniz, belirli tür bağımsız değişkenleri, zaten dönüştürmeleri önceden tanımlı dönüştürmeler genel türlerde işleçleri bildirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="6e059-1382">However, it is possible to declare operators on generic types that, for particular type arguments, specify conversions that already exist as pre-defined conversions.</span></span> <span data-ttu-id="6e059-1383">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-1383">In the example</span></span>
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
<span data-ttu-id="6e059-1384">yazdığınızda `object` bir tür bağımsız değişkeni olarak belirtilen `T`, zaten bir dönüştürme ikinci işleç bildirir (örtük ve bu nedenle ayrıca açık, her tür için türünden dönüştürme var `object`).</span><span class="sxs-lookup"><span data-stu-id="6e059-1384">when type `object` is specified as a type argument for `T`, the second operator declares a conversion that already exists (an implicit, and therefore also an explicit, conversion exists from any type to type `object`).</span></span>

<span data-ttu-id="6e059-1385">Önceden tanımlı bir dönüştürme iki tür arasında mevcut olduğu durumlarda, tüm bu türler arasında kullanıcı tanımlı dönüştürme göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1385">In cases where a pre-defined conversion exists between two types, any user-defined conversions between those types are ignored.</span></span> <span data-ttu-id="6e059-1386">Özellikle:</span><span class="sxs-lookup"><span data-stu-id="6e059-1386">Specifically:</span></span>

*  <span data-ttu-id="6e059-1387">Önceden tanımlanmış bir örtük dönüştürme varsa ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) türüne `S` türüne `T`, tüm kullanıcı tanımlı Dönüşümler (örtük veya açık) öğesinden `S` için `T` göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1387">If a pre-defined implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from type `S` to type `T`, all user-defined conversions (implicit or explicit) from `S` to `T` are ignored.</span></span>
*  <span data-ttu-id="6e059-1388">Önceden tanımlı açık bir dönüştürme ise ([açık dönüştürmeler](conversions.md#explicit-conversions)) türüne `S` türüne `T`, kullanıcı tanımlı hiçbir açık dönüşümlerse `S` için `T` göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1388">If a pre-defined explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) exists from type `S` to type `T`, any user-defined explicit conversions from `S` to `T` are ignored.</span></span> <span data-ttu-id="6e059-1389">Ayrıca:</span><span class="sxs-lookup"><span data-stu-id="6e059-1389">Furthermore:</span></span>

<span data-ttu-id="6e059-1390">Varsa `T` bir arabirim türüne örtük dönüşümlere kullanıcı tanımlı olan `S` için `T` göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1390">If `T` is an interface type, user-defined implicit conversions from `S` to `T` are ignored.</span></span>

<span data-ttu-id="6e059-1391">Aksi takdirde, kullanıcı tanımlı örtük dönüştürmelerine `S` için `T` olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1391">Otherwise, user-defined implicit conversions from `S` to `T` are still considered.</span></span>

<span data-ttu-id="6e059-1392">Tüm türleri ancak `object`, işleçler tarafından bildirilen `Convertible<T>` türü yukarıdaki önceden tanımlı dönüştürmeler çakışma yok.</span><span class="sxs-lookup"><span data-stu-id="6e059-1392">For all types but `object`, the operators declared by the `Convertible<T>` type above do not conflict with pre-defined conversions.</span></span> <span data-ttu-id="6e059-1393">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6e059-1393">For example:</span></span>
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

<span data-ttu-id="6e059-1394">Ancak, türünün `object`, önceden tanımlı dönüştürmeler tüm durumlarda ancak bir kullanıcı tanımlı dönüşümler Gizle:</span><span class="sxs-lookup"><span data-stu-id="6e059-1394">However, for type `object`, pre-defined conversions hide the user-defined conversions in all cases but one:</span></span>

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

<span data-ttu-id="6e059-1395">Kullanıcı tanımlı Dönüştürmelere izin veya dönüştürülecek verilmez *INTERFACE_TYPE*s.</span><span class="sxs-lookup"><span data-stu-id="6e059-1395">User-defined conversions are not allowed to convert from or to *interface_type*s.</span></span> <span data-ttu-id="6e059-1396">Özellikle, bu kısıtlama hiçbir kullanıcı tanımlı dönüştürme dönüştürülürken gerçekleşmesini sağlar. bir *INTERFACE_TYPE*ve dönüştürme bir *INTERFACE_TYPE* yalnızca başarılı nesnesi Dönüştürülen gerçekten uygulayan belirtilen *INTERFACE_TYPE*.</span><span class="sxs-lookup"><span data-stu-id="6e059-1396">In particular, this restriction ensures that no user-defined transformations occur when converting to an *interface_type*, and that a conversion to an *interface_type* succeeds only if the object being converted actually implements the specified *interface_type*.</span></span>

<span data-ttu-id="6e059-1397">Kaynak türü ve hedef türü bir dönüştürme operatörünün imzası oluşur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1397">The signature of a conversion operator consists of the source type and the target type.</span></span> <span data-ttu-id="6e059-1398">(Bu yalnızca formun kendisi için dönüş türü imzada katıldığı üyesi olduğunu unutmayın.) `implicit` Veya `explicit` sınıflandırma dönüştürme operatörü işlecin imzasının parçası değil.</span><span class="sxs-lookup"><span data-stu-id="6e059-1398">(Note that this is the only form of member for which the return type participates in the signature.) The `implicit` or `explicit` classification of a conversion operator is not part of the operator's signature.</span></span> <span data-ttu-id="6e059-1399">Bu nedenle, bir sınıf veya yapı hem bildirimini yapamazsınız bir `implicit` ve `explicit` dönüştürme işleci ile aynı kaynak ve hedef türleri.</span><span class="sxs-lookup"><span data-stu-id="6e059-1399">Thus, a class or struct cannot declare both an `implicit` and an `explicit` conversion operator with the same source and target types.</span></span>

<span data-ttu-id="6e059-1400">Genel olarak, kullanıcı tanımlı örtük dönüştürmeler, hiçbir zaman özel durumlar ve hiçbir zaman bilgi kaybetmek tasarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1400">In general, user-defined implicit conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="6e059-1401">Bir kullanıcı tanımlı dönüştürme (örneğin, kaynak bağımsız değişkeni aralık dışında olduğundan) özel durumları ortaya çıkmasına neden verebilir veya kaybı bilgi (örneğin, yüksek sıra bitleri atılıyor) ve ardından bu dönüştürme açık bir dönüştürme tanımlanması gerekir</span><span class="sxs-lookup"><span data-stu-id="6e059-1401">If a user-defined conversion can give rise to exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as an explicit conversion.</span></span>

<span data-ttu-id="6e059-1402">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-1402">In the example</span></span>
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
<span data-ttu-id="6e059-1403">Dönüştürme `Digit` için `byte` hiçbir zaman istisnalar fırlatıyorsa veya bilgiler, ancak bu dönüştürme kaybeder örtük olarak `byte` için `Digit` beri açıktır `Digit` yalnızca bir alt kümesini olası temsil edebilir değerleri bir `byte`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1403">the conversion from `Digit` to `byte` is implicit because it never throws exceptions or loses information, but the conversion from `byte` to `Digit` is explicit since `Digit` can only represent a subset of the possible values of a `byte`.</span></span>

## <a name="instance-constructors"></a><span data-ttu-id="6e059-1404">Örnek oluşturucuları</span><span class="sxs-lookup"><span data-stu-id="6e059-1404">Instance constructors</span></span>

<span data-ttu-id="6e059-1405">Bir ***örnek oluşturucusu*** uygulayan bir sınıf örneği başlatmak için gerekli eylemleri bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1405">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="6e059-1406">Örnek oluşturucuları kullanarak bildirilir *constructor_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="6e059-1406">Instance constructors are declared using *constructor_declaration*s:</span></span>

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

<span data-ttu-id="6e059-1407">A *constructor_declaration* bir dizi içerebilir *öznitelikleri* ([öznitelikleri](attributes.md)), geçerli bir bileşimin dört erişim değiştiricileri ([erişim değiştiricilerine ](classes.md#access-modifiers)) ve bir `extern` ([dış yöntemleri](classes.md#external-methods)) değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1407">A *constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and an `extern` ([External methods](classes.md#external-methods)) modifier.</span></span> <span data-ttu-id="6e059-1408">Birden çok kez aynı değiştiricisi içerecek şekilde bir oluşturucu bildirimi izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="6e059-1408">A constructor declaration is not permitted to include the same modifier multiple times.</span></span>

<span data-ttu-id="6e059-1409">*Tanımlayıcı* , bir *constructor_declarator* örnek oluşturucusu bildirilmiş sınıfı adı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1409">The *identifier* of a *constructor_declarator* must name the class in which the instance constructor is declared.</span></span> <span data-ttu-id="6e059-1410">Herhangi bir ad belirtilmezse, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1410">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="6e059-1411">İsteğe bağlı *formal_parameter_list* örneği aynı kurallara tabi oluşturucudur *formal_parameter_list* bir yöntemin ([yöntemleri](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1411">The optional *formal_parameter_list* of an instance constructor is subject to the same rules as the *formal_parameter_list* of a method ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="6e059-1412">İmza biçimsel parametre listesi tanımlar ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)), bir örnek oluşturucusu ve işlem yapabildiği yöneten aşırı yükleme çözümlemesi ([anlam çıkarma](expressions.md#type-inference)) belirli bir seçer bir çağrı örnek oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="6e059-1412">The formal parameter list defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of an instance constructor and governs the process whereby overload resolution ([Type inference](expressions.md#type-inference)) selects a particular instance constructor in an invocation.</span></span>

<span data-ttu-id="6e059-1413">Başvuru türlerinin her biri *formal_parameter_list* örneğini Oluşturucusu en az Oluşturucu olarak olarak erişilebilir olmalıdır ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1413">Each of the types referenced in the *formal_parameter_list* of an instance constructor must be at least as accessible as the constructor itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6e059-1414">İsteğe bağlı *constructor_initializer* verilen deyimleri yürütmeden önce çağrılacak başka bir örnek oluşturucusu belirtir *constructor_body* , bu örnek oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="6e059-1414">The optional *constructor_initializer* specifies another instance constructor to invoke before executing the statements given in the *constructor_body* of this instance constructor.</span></span> <span data-ttu-id="6e059-1415">Daha ayrıntılı açıklanır budur [Oluşturucu başlatıcıları](classes.md#constructor-initializers).</span><span class="sxs-lookup"><span data-stu-id="6e059-1415">This is described further in [Constructor initializers](classes.md#constructor-initializers).</span></span>

<span data-ttu-id="6e059-1416">Ne zaman bir oluşturucu bildirimi içeren bir `extern` değiştiricisi, kurucusu olduğu söylenir bir ***dış Oluşturucu***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1416">When a constructor declaration includes an `extern` modifier, the constructor is said to be an ***external constructor***.</span></span> <span data-ttu-id="6e059-1417">Bir dış Oluşturucu bildirimi yok kullanımınız sağladığından, *constructor_body* noktalı virgül içerir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1417">Because an external constructor declaration provides no actual implementation, its *constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="6e059-1418">Diğer tüm oluşturucular için *constructor_body* oluşan bir *blok* sınıfının yeni bir örneğini başlatmak için belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1418">For all other constructors, the *constructor_body* consists of a *block* which specifies the statements to initialize a new instance of the class.</span></span> <span data-ttu-id="6e059-1419">Bu tam olarak karşılık geldiğinden *blok* ile bir örnek yöntemi, bir `void` dönüş türü ([yöntem gövdesini](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1419">This corresponds exactly to the *block* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="6e059-1420">Örnek oluşturucuları devralınmaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-1420">Instance constructors are not inherited.</span></span> <span data-ttu-id="6e059-1421">Bu nedenle, bir sınıf gerçekten sınıfta bildirilen dışındaki örnek oluşturucusu yok.</span><span class="sxs-lookup"><span data-stu-id="6e059-1421">Thus, a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="6e059-1422">Bir sınıf örneği hiçbir oluşturucu bildirimleri içeriyorsa, varsayılan örnek oluşturucusu otomatik olarak sağlanır ([varsayılan oluşturucular](classes.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1422">If a class contains no instance constructor declarations, a default instance constructor is automatically provided ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="6e059-1423">Örnek Oluşturucuları tarafından çağrılır *object_creation_expression*s ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)) ve ile *constructor_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="6e059-1423">Instance constructors are invoked by *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)) and through *constructor_initializer*s.</span></span>

### <a name="constructor-initializers"></a><span data-ttu-id="6e059-1424">Oluşturucu başlatıcıları</span><span class="sxs-lookup"><span data-stu-id="6e059-1424">Constructor initializers</span></span>

<span data-ttu-id="6e059-1425">Tüm oluşturucular örnek (sınıfı için hariç `object`) örtük olarak başka bir örnek oluşturucusu çağrısından hemen önce dahil *constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="6e059-1425">All instance constructors (except those for class `object`) implicitly include an invocation of another instance constructor immediately before the *constructor_body*.</span></span> <span data-ttu-id="6e059-1426">Örtük olarak çağrılacak oluşturucunun belirlenir *constructor_initializer*:</span><span class="sxs-lookup"><span data-stu-id="6e059-1426">The constructor to implicitly invoke is determined by the *constructor_initializer*:</span></span>

*  <span data-ttu-id="6e059-1427">Form örneği oluşturucu başlatıcı `base(argument_list)` veya `base()` çağrılacak doğrudan temel sınıftan bir örnek oluşturucusunda neden olur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1427">An instance constructor initializer of the form `base(argument_list)` or `base()` causes an instance constructor from the direct base class to be invoked.</span></span> <span data-ttu-id="6e059-1428">Bu oluşturucu kullanılarak seçilen *argument_list* , mevcut ve aşırı yükleme çözünürlüğü kurallarına [aşırı yükleme çözünürlüğü](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="6e059-1428">That constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="6e059-1429">Tüm erişilebilir örnek oluşturucular doğrudan temel sınıfta yer alan veya varsayılan oluşturucu aday örnek oluşturucuları kümesini oluşur ([varsayılan oluşturucular](classes.md#default-constructors)), hiçbir örnek oluşturucuları bildirilmiş olan doğrudan temel sınıf.</span><span class="sxs-lookup"><span data-stu-id="6e059-1429">The set of candidate instance constructors consists of all accessible instance constructors contained in the direct base class, or the default constructor ([Default constructors](classes.md#default-constructors)), if no instance constructors are declared in the direct base class.</span></span> <span data-ttu-id="6e059-1430">Bu kümenin boş olup olmadığını veya tek bir en iyi örnek oluşturucusu tanımladıysanız, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1430">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span>
*  <span data-ttu-id="6e059-1431">Form örneği oluşturucu başlatıcı `this(argument-list)` veya `this()` sınıfın kendisi çağrılacak bir örnek oluşturucusunda neden olur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1431">An instance constructor initializer of the form `this(argument-list)` or `this()` causes an instance constructor from the class itself to be invoked.</span></span> <span data-ttu-id="6e059-1432">Oluşturucu kullanılarak seçilen *argument_list* , mevcut ve aşırı yükleme çözünürlüğü kurallarına [aşırı yükleme çözünürlüğü](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="6e059-1432">The constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="6e059-1433">Örnek oluşturucuları aday kümesini sınıfta bildirilen tüm erişilebilir örnek oluşturucular oluşur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1433">The set of candidate instance constructors consists of all accessible instance constructors declared in the class itself.</span></span> <span data-ttu-id="6e059-1434">Bu kümenin boş olup olmadığını veya tek bir en iyi örnek oluşturucusu tanımladıysanız, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1434">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span> <span data-ttu-id="6e059-1435">Bir kopya Oluşturucusu bildirimi Oluşturucusu kendisini çağıran bir oluşturucu başlatıcı içeriyorsa, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1435">If an instance constructor declaration includes a constructor initializer that invokes the constructor itself, a compile-time error occurs.</span></span>

<span data-ttu-id="6e059-1436">Bir örnek oluşturucusunda bir oluşturucu başlatıcı formun hiçbir oluşturucu başlatıcı varsa `base()` örtük olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1436">If an instance constructor has no constructor initializer, a constructor initializer of the form `base()` is implicitly provided.</span></span> <span data-ttu-id="6e059-1437">Bu nedenle, bir kopya Oluşturucusu bildirimi form</span><span class="sxs-lookup"><span data-stu-id="6e059-1437">Thus, an instance constructor declaration of the form</span></span>
```csharp
C(...) {...}
```
<span data-ttu-id="6e059-1438">tam olarak eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="6e059-1438">is exactly equivalent to</span></span>
```csharp
C(...): base() {...}
```

<span data-ttu-id="6e059-1439">Tarafından belirtilen parametrelerle kapsamını *formal_parameter_list* örnek oluşturucusu bu bildirimin oluşturucu başlatıcı bildirimi içerir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1439">The scope of the parameters given by the *formal_parameter_list* of an instance constructor declaration includes the constructor initializer of that declaration.</span></span> <span data-ttu-id="6e059-1440">Bu nedenle, bir oluşturucu başlatıcı oluşturucunun parametreler erişim izin verilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1440">Thus, a constructor initializer is permitted to access the parameters of the constructor.</span></span> <span data-ttu-id="6e059-1441">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6e059-1441">For example:</span></span>
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

<span data-ttu-id="6e059-1442">Bir örneği oluşturucu başlatıcı oluşturulan örnek erişemez.</span><span class="sxs-lookup"><span data-stu-id="6e059-1442">An instance constructor initializer cannot access the instance being created.</span></span> <span data-ttu-id="6e059-1443">Bu nedenle, başvuru bir derleme zamanı hatasıdır `this` oluşturucu başlatıcı bir bağımsız değişken ifadede gibidir, bir derleme zamanı hatası aracılığıyla herhangi bir örnek üyesi başvurmak bir bağımsız değişken ifadesi için bir *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="6e059-1443">Therefore it is a compile-time error to reference `this` in an argument expression of the constructor initializer, as is it a compile-time error for an argument expression to reference any instance member through a *simple_name*.</span></span>

### <a name="instance-variable-initializers"></a><span data-ttu-id="6e059-1444">Örnek değişken başlatıcıları</span><span class="sxs-lookup"><span data-stu-id="6e059-1444">Instance variable initializers</span></span>

<span data-ttu-id="6e059-1445">Ne zaman bir örnek oluşturucusunda oluşturucu başlatıcı içermiyor veya form Oluşturucu başlatıcısına sahip `base(...)`, o Oluşturucu örtük olarak belirtilen başlatmalar gerçekleştirir *variable_initializer*, s örnek alanları, sınıfta bildirilen.</span><span class="sxs-lookup"><span data-stu-id="6e059-1445">When an instance constructor has no constructor initializer, or it has a constructor initializer of the form `base(...)`, that constructor implicitly performs the initializations specified by the *variable_initializer*s of the instance fields declared in its class.</span></span> <span data-ttu-id="6e059-1446">Bu bir dizi hemen sonra Anonime oluşturucusuna ve doğrudan temel sınıf oluşturucu örtük çağırmadan önce yürütülür atamaları karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1446">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor and before the implicit invocation of the direct base class constructor.</span></span> <span data-ttu-id="6e059-1447">Değişken başlatıcılar, sınıf bildirimi içinde göründükleri metinsel sırayla yürütülür.</span><span class="sxs-lookup"><span data-stu-id="6e059-1447">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span>

### <a name="constructor-execution"></a><span data-ttu-id="6e059-1448">Oluşturucusu yürütme</span><span class="sxs-lookup"><span data-stu-id="6e059-1448">Constructor execution</span></span>

<span data-ttu-id="6e059-1449">Değişken başlatıcılar atama deyimleri dönüştürülür ve bu atama deyimleri, temel sınıf örneği oluşturucunun çağrılacağını önce yürütülür.</span><span class="sxs-lookup"><span data-stu-id="6e059-1449">Variable initializers are transformed into assignment statements, and these assignment statements are executed before the invocation of the base class instance constructor.</span></span> <span data-ttu-id="6e059-1450">Bu sıralama, söz konusu örneğine erişimi olan herhangi bir deyim yürütülmeden önce tüm örnek alanları, değişken başlatıcılar tarafından başlatılır sağlar.</span><span class="sxs-lookup"><span data-stu-id="6e059-1450">This ordering ensures that all instance fields are initialized by their variable initializers before any statements that have access to that instance are executed.</span></span>

<span data-ttu-id="6e059-1451">Verilen örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-1451">Given the example</span></span>
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
<span data-ttu-id="6e059-1452">Zaman `new B()` bir örneğini oluşturmak için kullanılan `B`, aşağıdaki çıkış üretilir:</span><span class="sxs-lookup"><span data-stu-id="6e059-1452">when `new B()` is used to create an instance of `B`, the following output is produced:</span></span>
```
x = 1, y = 0
```

<span data-ttu-id="6e059-1453">Değerini `x` temel sınıfının örnek oluşturucusu çağırılmadan önce değişken başlatıcı çalıştırıldığı için 1'dir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1453">The value of `x` is 1 because the variable initializer is executed before the base class instance constructor is invoked.</span></span> <span data-ttu-id="6e059-1454">Ancak, değerini `y` 0'dır (varsayılan değer olan bir `int`) çünkü atamaya `y` temel sınıf oluşturucusunu döndürdükten sonra kadar yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="6e059-1454">However, the value of `y` is 0 (the default value of an `int`) because the assignment to `y` is not executed until after the base class constructor returns.</span></span>

<span data-ttu-id="6e059-1455">Örnek değişken başlatıcılar ve oluşturucu başlatıcıları önce otomatik olarak eklenen deyimleri düşünmek faydalıdır *constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="6e059-1455">It is useful to think of instance variable initializers and constructor initializers as statements that are automatically inserted before the *constructor_body*.</span></span> <span data-ttu-id="6e059-1456">Örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-1456">The example</span></span>
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
<span data-ttu-id="6e059-1457">birden fazla değişken başlatıcılar içerir. Ayrıca, her iki Oluşturucu başlatıcıları içerir (`base` ve `this`).</span><span class="sxs-lookup"><span data-stu-id="6e059-1457">contains several variable initializers; it also contains constructor initializers of both forms (`base` and `this`).</span></span> <span data-ttu-id="6e059-1458">Örnek aşağıda gösterilen koda karşılık gelen burada açıklamaları (otomatik olarak eklenen Oluşturucu çağrıları için kullanılan sözdizimi geçerli değil, ancak yalnızca mekanizması göstermek için kullanılır) otomatik olarak eklenen bir ifade belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1458">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement (the syntax used for the automatically inserted constructor invocations isn't valid, but merely serves to illustrate the mechanism).</span></span>

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

### <a name="default-constructors"></a><span data-ttu-id="6e059-1459">Varsayılan Oluşturucu</span><span class="sxs-lookup"><span data-stu-id="6e059-1459">Default constructors</span></span>

<span data-ttu-id="6e059-1460">Bir sınıf örneği hiçbir oluşturucu bildirimleri içeriyorsa, varsayılan örnek oluşturucusu otomatik olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1460">If a class contains no instance constructor declarations, a default instance constructor is automatically provided.</span></span> <span data-ttu-id="6e059-1461">Bu varsayılan oluşturucu yalnızca doğrudan temel sınıf parametresiz oluşturucu çağırır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1461">That default constructor simply invokes the parameterless constructor of the direct base class.</span></span> <span data-ttu-id="6e059-1462">Ardından sınıf soyuttur varsayılan oluşturucusu için bildirilen erişilebilirliği korunmaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-1462">If the class is abstract then the declared accessibility for the default constructor is protected.</span></span> <span data-ttu-id="6e059-1463">Aksi takdirde, varsayılan oluşturucusu için bildirilen erişilebilirliği herkes tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1463">Otherwise, the declared accessibility for the default constructor is public.</span></span> <span data-ttu-id="6e059-1464">Bu nedenle, varsayılan oluşturucu her zaman biçimindedir</span><span class="sxs-lookup"><span data-stu-id="6e059-1464">Thus, the default constructor is always of the form</span></span>

```csharp
protected C(): base() {}
```
<span data-ttu-id="6e059-1465">veya</span><span class="sxs-lookup"><span data-stu-id="6e059-1465">or</span></span>
```csharp
public C(): base() {}
```
<span data-ttu-id="6e059-1466">Burada `C` sınıf adıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1466">where `C` is the name of the class.</span></span> <span data-ttu-id="6e059-1467">Ardından aşırı yükleme çözünürlüğü için temel sınıf oluşturucu başlatıcı benzersiz bir en iyi aday belirlenemiyor ise bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1467">If overload resolution is unable to determine a unique best candidate for the base class constructor initializer then a compile-time error occurs.</span></span>

<span data-ttu-id="6e059-1468">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-1468">In the example</span></span>
```csharp
class Message
{
    object sender;
    string text;
}
```
<span data-ttu-id="6e059-1469">hiçbir örnek oluşturucusu bildirimleri sınıfı içerdiği için varsayılan bir oluşturucu sağlanır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1469">a default constructor is provided because the class contains no instance constructor declarations.</span></span> <span data-ttu-id="6e059-1470">Bu nedenle, örneğin tam olarak eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="6e059-1470">Thus, the example is precisely equivalent to</span></span>
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a><span data-ttu-id="6e059-1471">Özel oluşturucular</span><span class="sxs-lookup"><span data-stu-id="6e059-1471">Private constructors</span></span>

<span data-ttu-id="6e059-1472">Bir sınıf, `T` öğesinin program metni dışında sınıflar için olası değil, yalnızca özel örnek oluşturucuları bildirir `T` türetmek için `T` veya doğrudan örneklerini oluşturmak için `T`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1472">When a class `T` declares only private instance constructors, it is not possible for classes outside the program text of `T` to derive from `T` or to directly create instances of `T`.</span></span> <span data-ttu-id="6e059-1473">Bir sınıf yalnızca statik üyeleri içerir ve örneği için tasarlanmamıştır, bu nedenle, boş özel örnek oluşturucusu ekleme örneğini oluşturmada engelleyecek.</span><span class="sxs-lookup"><span data-stu-id="6e059-1473">Thus, if a class contains only static members and isn't intended to be instantiated, adding an empty private instance constructor will prevent instantiation.</span></span> <span data-ttu-id="6e059-1474">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6e059-1474">For example:</span></span>
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

<span data-ttu-id="6e059-1475">`Trig` Sınıfı için ilgili yöntemler ve sabitler gruplarını, ancak bu örneği için tasarlanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1475">The `Trig` class groups related methods and constants, but is not intended to be instantiated.</span></span> <span data-ttu-id="6e059-1476">Bu nedenle tek boş özel örnek oluşturucusu bildirir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1476">Therefore it declares a single empty private instance constructor.</span></span> <span data-ttu-id="6e059-1477">Varsayılan Oluşturucu otomatik olarak oluşturulmasını engellemek için en az bir örnek oluşturucusu bildirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1477">At least one instance constructor must be declared to suppress the automatic generation of a default constructor.</span></span>

### <a name="optional-instance-constructor-parameters"></a><span data-ttu-id="6e059-1478">İsteğe bağlı örnek oluşturucusu parametreleri</span><span class="sxs-lookup"><span data-stu-id="6e059-1478">Optional instance constructor parameters</span></span>

<span data-ttu-id="6e059-1479">`this(...)` Oluşturucu başlatıcı biçimi yaygın olarak aşırı yüklemesi ile birlikte isteğe bağlı örnek oluşturucusu parametreleri uygulamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1479">The `this(...)` form of constructor initializer is commonly used in conjunction with overloading to implement optional instance constructor parameters.</span></span> <span data-ttu-id="6e059-1480">Örnekte</span><span class="sxs-lookup"><span data-stu-id="6e059-1480">In the example</span></span>
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
<span data-ttu-id="6e059-1481">ilk iki örnek oluşturucuları, yalnızca varsayılan değerleri eksik bağımsız değişkenleri sağlayın.</span><span class="sxs-lookup"><span data-stu-id="6e059-1481">the first two instance constructors merely provide the default values for the missing arguments.</span></span> <span data-ttu-id="6e059-1482">İkisi de bir `this(...)` gerçekten çalışır yeni örneği başlatılıyor, üçüncü örnek oluşturucusu çağırmak için oluşturucu başlatıcı.</span><span class="sxs-lookup"><span data-stu-id="6e059-1482">Both use a `this(...)` constructor initializer to invoke the third instance constructor, which actually does the work of initializing the new instance.</span></span> <span data-ttu-id="6e059-1483">Etkisi isteğe bağlı Oluşturucu parametrelerinin olan:</span><span class="sxs-lookup"><span data-stu-id="6e059-1483">The effect is that of optional constructor parameters:</span></span>
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a><span data-ttu-id="6e059-1484">Statik oluşturucular</span><span class="sxs-lookup"><span data-stu-id="6e059-1484">Static constructors</span></span>

<span data-ttu-id="6e059-1485">A ***statik Oluşturucu*** kapalı sınıf türünü başlatmak için gerekli eylemleri uygulayan bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1485">A ***static constructor*** is a member that implements the actions required to initialize a closed class type.</span></span> <span data-ttu-id="6e059-1486">Statik oluşturucular kullanarak bildirilir *static_constructor_declaration*: %s</span><span class="sxs-lookup"><span data-stu-id="6e059-1486">Static constructors are declared using *static_constructor_declaration*s:</span></span>

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

<span data-ttu-id="6e059-1487">A *static_constructor_declaration* bir dizi içerebilir *öznitelikleri* ([öznitelikleri](attributes.md)) ve bir `extern` değiştirici ([dışyöntemleri](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1487">A *static_constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and an `extern` modifier ([External methods](classes.md#external-methods)).</span></span>

<span data-ttu-id="6e059-1488">*Tanımlayıcı* , bir *static_constructor_declaration* statik Oluşturucu bildirilmiş sınıfı adı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1488">The *identifier* of a *static_constructor_declaration* must name the class in which the static constructor is declared.</span></span> <span data-ttu-id="6e059-1489">Herhangi bir ad belirtilmezse, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1489">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="6e059-1490">Ne zaman bir statik Oluşturucu bildirimi içeren bir `extern` değiştiricisi, statik oluşturucunun olduğu söylenir bir ***dış statik Oluşturucu***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1490">When a static constructor declaration includes an `extern` modifier, the static constructor is said to be an ***external static constructor***.</span></span> <span data-ttu-id="6e059-1491">Bir dış statik Oluşturucu bildirimi yok kullanımınız sağladığından, *static_constructor_body* noktalı virgül içerir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1491">Because an external static constructor declaration provides no actual implementation, its *static_constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="6e059-1492">Diğer statik Oluşturucu bildirimleri *static_constructor_body* oluşan bir *blok* sınıf başlatma çalıştırılacak deyimleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1492">For all other static constructor declarations, the *static_constructor_body* consists of a *block* which specifies the statements to execute in order to initialize the class.</span></span> <span data-ttu-id="6e059-1493">Bu tam olarak karşılık geldiğinden *method_body* ile statik bir yöntemin bir `void` dönüş türü ([yöntem gövdesini](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1493">This corresponds exactly to the *method_body* of a static method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="6e059-1494">Statik oluşturucular değil devralınır ve doğrudan çağrılamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-1494">Static constructors are not inherited, and cannot be called directly.</span></span>

<span data-ttu-id="6e059-1495">Statik Oluşturucu bir kapalı sınıf türü için en fazla bir kez bir verilen uygulama etki alanında yürütür.</span><span class="sxs-lookup"><span data-stu-id="6e059-1495">The static constructor for a closed class type executes at most once in a given application domain.</span></span> <span data-ttu-id="6e059-1496">Statik Oluşturucu yürütülmesi, bir uygulama etki alanı içinde gerçekleşmesi için ilk aşağıdaki olaylardan biri tarafından tetiklenir:</span><span class="sxs-lookup"><span data-stu-id="6e059-1496">The execution of a static constructor is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="6e059-1497">Sınıf türünün bir örneği oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1497">An instance of the class type is created.</span></span>
*  <span data-ttu-id="6e059-1498">Statik üyeler sınıf türünden birini başvurulur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1498">Any of the static members of the class type are referenced.</span></span>

<span data-ttu-id="6e059-1499">Bir sınıf içeriyorsa `Main` yöntemi ([uygulama başlatma](basic-concepts.md#application-startup)) hangi yürütmeyi başlatır, statik Oluşturucu için önce o sınıfın yürütür, `Main` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1499">If a class contains the `Main` method ([Application Startup](basic-concepts.md#application-startup)) in which execution begins, the static constructor for that class executes before the `Main` method is called.</span></span>

<span data-ttu-id="6e059-1500">Yeni bir kapalı sınıf türü statik alanları yeni bir dizi ilk kez başlatmak için ([statik ve örnek alanları](classes.md#static-and-instance-fields)), belirli kapalı bir tür oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1500">To initialize a new closed class type, first a new set of static fields ([Static and instance fields](classes.md#static-and-instance-fields)) for that particular closed type is created.</span></span> <span data-ttu-id="6e059-1501">Statik alanların her biri varsayılan değer olarak başlatılır ([varsayılan değerler](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1501">Each of the static fields is initialized to its default value ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="6e059-1502">Ardından, statik alan başlatıcıları ([statik alan başlatma](classes.md#static-field-initialization)) statik alanlara yürütülür.</span><span class="sxs-lookup"><span data-stu-id="6e059-1502">Next, the static field initializers ([Static field initialization](classes.md#static-field-initialization)) are executed for those static fields.</span></span> <span data-ttu-id="6e059-1503">Son olarak, statik oluşturucunun yürütülür.</span><span class="sxs-lookup"><span data-stu-id="6e059-1503">Finally, the static constructor is executed.</span></span>

<span data-ttu-id="6e059-1504">Örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-1504">The example</span></span>
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
<span data-ttu-id="6e059-1505">Çıkış üretmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="6e059-1505">must produce the output:</span></span>
```
Init A
A.F
Init B
B.F
```
<span data-ttu-id="6e059-1506">çünkü yürütülmesini `A`'s statik oluşturucu çağrısı tarafından tetiklendiğini `A.F`ve yürütülmesini `B`'s statik oluşturucu çağrısı tarafından tetiklenir `B.F`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1506">because the execution of `A`'s static constructor is triggered by the call to `A.F`, and the execution of `B`'s static constructor is triggered by the call to `B.F`.</span></span>

<span data-ttu-id="6e059-1507">Varsayılan değer durumlarını gözlenecek değişken başlatıcıları olan statik alanlar izin döngüsel bağımlılıklar oluşturmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="6e059-1507">It is possible to construct circular dependencies that allow static fields with variable initializers to be observed in their default value state.</span></span>

<span data-ttu-id="6e059-1508">Örnek</span><span class="sxs-lookup"><span data-stu-id="6e059-1508">The example</span></span>
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
<span data-ttu-id="6e059-1509">çıktıyı üretir</span><span class="sxs-lookup"><span data-stu-id="6e059-1509">produces the output</span></span>
```
X = 1, Y = 2
```

<span data-ttu-id="6e059-1510">Yürütülecek `Main` yöntemi, sistemin ilk için Başlatıcı çalıştırır `B.Y`, sınıfa önceki `B`'s statik Oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="6e059-1510">To execute the `Main` method, the system first runs the initializer for `B.Y`, prior to class `B`'s static constructor.</span></span> <span data-ttu-id="6e059-1511">`Y`ın Başlatıcı neden olur `A`ait olduğundan çalıştırılacak statik Oluşturucu değerini `A.X` başvuruluyor.</span><span class="sxs-lookup"><span data-stu-id="6e059-1511">`Y`'s initializer causes `A`'s static constructor to be run because the value of `A.X` is referenced.</span></span> <span data-ttu-id="6e059-1512">Statik oluşturucusunun `A` değerini hesaplamak için sırayla geçer `X`ve bunu öğesinden varsayılan değerini `Y`, sıfır olan.</span><span class="sxs-lookup"><span data-stu-id="6e059-1512">The static constructor of `A` in turn proceeds to compute the value of `X`, and in doing so fetches the default value of `Y`, which is zero.</span></span> <span data-ttu-id="6e059-1513">`A.X` Bu nedenle, 1 olarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1513">`A.X` is thus initialized to 1.</span></span> <span data-ttu-id="6e059-1514">Çalışan işlemi `A`ilk değerini hesaplaması için döndüren tamamlandıktan sonra statik alan başlatıcıları ve statik Oluşturucu `Y`, 2 sonucunu olur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1514">The process of running `A`'s static field initializers and static constructor then completes, returning to the calculation of the initial value of `Y`, the result of which becomes 2.</span></span>

<span data-ttu-id="6e059-1515">Statik Oluşturucu her kapalı oluşturulan sınıf türü için tam bir kez yürütülür çünkü bu iade edilemez kısıtlamaları aracılığıyla derleme zamanında tür parametresi üzerinde çalışma zamanı denetimleri zorunlu kılmak için kullanışlı bir yerdir ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1515">Because the static constructor is executed exactly once for each closed constructed class type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="6e059-1516">Örneğin, şu tür, tür bağımsız değişkeni bir sabit listesi olduğunu zorlamak için bir statik Oluşturucu kullanır:</span><span class="sxs-lookup"><span data-stu-id="6e059-1516">For example, the following type uses a static constructor to enforce that the type argument is an enum:</span></span>
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

## <a name="destructors"></a><span data-ttu-id="6e059-1517">Yıkıcılar</span><span class="sxs-lookup"><span data-stu-id="6e059-1517">Destructors</span></span>

<span data-ttu-id="6e059-1518">A ***yok Edicisi*** uygulayan bir sınıf örneği yok etmek için gereken eylemler bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1518">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="6e059-1519">Bir yok edici kullanılarak bildirilen bir *destructor_declaration*:</span><span class="sxs-lookup"><span data-stu-id="6e059-1519">A destructor is declared using a *destructor_declaration*:</span></span>

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

<span data-ttu-id="6e059-1520">A *destructor_declaration* bir dizi içerebilir *öznitelikleri* ([öznitelikleri](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1520">A *destructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="6e059-1521">*Tanımlayıcı* , bir *destructor_declaration* yok edici bildirilmiş sınıfı adı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1521">The *identifier* of a *destructor_declaration* must name the class in which the destructor is declared.</span></span> <span data-ttu-id="6e059-1522">Herhangi bir ad belirtilmezse, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1522">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="6e059-1523">Ne zaman bir yok edici bildirimi içeren bir `extern` değiştiricisi, yok edici olduğu söylenir bir ***dış yok Edicisi***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1523">When a destructor declaration includes an `extern` modifier, the destructor is said to be an ***external destructor***.</span></span> <span data-ttu-id="6e059-1524">Bir dış yok Edicisi bildirimi yok kullanımınız sağladığından, *destructor_body* noktalı virgül içerir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1524">Because an external destructor declaration provides no actual implementation, its *destructor_body* consists of a semicolon.</span></span> <span data-ttu-id="6e059-1525">Diğer yıkıcı için *destructor_body* oluşan bir *blok* sınıfının bir örneğini yok etmek çalıştırılacak deyimleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1525">For all other destructors, the *destructor_body* consists of a *block* which specifies the statements to execute in order to destruct an instance of the class.</span></span> <span data-ttu-id="6e059-1526">A *destructor_body* tam olarak karşılık geldiğinden *method_body* ile bir örnek yöntemi, bir `void` dönüş türü ([yöntem gövdesini](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1526">A *destructor_body* corresponds exactly to the *method_body* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="6e059-1527">Yok ediciler devralınmaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-1527">Destructors are not inherited.</span></span> <span data-ttu-id="6e059-1528">Bu nedenle, bir sınıfın yok edicileri bu sınıfta bildirilen farklı vardır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1528">Thus, a class has no destructors other than the one which may be declared in that class.</span></span>

<span data-ttu-id="6e059-1529">Hiçbir parametrelere sahip olacak şekilde bir yıkıcı gerekli olduğundan, bir sınıf, en fazla bir yıkıcı çalışabilmeniz için aşırı yüklenmiş, olamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-1529">Since a destructor is required to have no parameters, it cannot be overloaded, so a class can have, at most, one destructor.</span></span>

<span data-ttu-id="6e059-1530">Yıkıcılar, otomatik olarak çağrılır ve açıkça çağrılamaz.</span><span class="sxs-lookup"><span data-stu-id="6e059-1530">Destructors are invoked automatically, and cannot be invoked explicitly.</span></span> <span data-ttu-id="6e059-1531">Artık bu örneği kullanmak herhangi bir kod için mümkün olduğunda örneği yok etme için uygun hale gelir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1531">An instance becomes eligible for destruction when it is no longer possible for any code to use that instance.</span></span> <span data-ttu-id="6e059-1532">Örnek yok Edicisi yürütülmesini örneği yok etme için uygun hale geldikten sonra herhangi bir zamanda meydana gelebilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1532">Execution of the destructor for the instance may occur at any time after the instance becomes eligible for destruction.</span></span> <span data-ttu-id="6e059-1533">Bir örneği imha edildiğinde yok ediciler o örneğin devralma zincirinde, sırayla çağrılır için türetilmiş az türetilmiş en iyi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1533">When an instance is destructed, the destructors in that instance's inheritance chain are called, in order, from most derived to least derived.</span></span> <span data-ttu-id="6e059-1534">Bir yok edici herhangi bir iş parçacığı üzerinde çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1534">A destructor may be executed on any thread.</span></span> <span data-ttu-id="6e059-1535">Daha ayrıntılı bir açıklaması ne zaman ve yok edici nasıl yürütüleceğini yöneten kurallar için [otomatik bellek yönetimi](basic-concepts.md#automatic-memory-management).</span><span class="sxs-lookup"><span data-stu-id="6e059-1535">For further discussion of the rules that govern when and how a destructor is executed, see [Automatic memory management](basic-concepts.md#automatic-memory-management).</span></span>

<span data-ttu-id="6e059-1536">Örnek çıktısı</span><span class="sxs-lookup"><span data-stu-id="6e059-1536">The output of the example</span></span>
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
<span data-ttu-id="6e059-1537">is</span><span class="sxs-lookup"><span data-stu-id="6e059-1537">is</span></span>
```
B's destructor
A's destructor
```
<span data-ttu-id="6e059-1538">yok ediciler bir devralma zincirindeki sırayla çağrılır beri için türetilmiş az türetilmiş en iyi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1538">since destructors in an inheritance chain are called in order, from most derived to least derived.</span></span>

<span data-ttu-id="6e059-1539">Yıkıcılar, sanal yöntemini geçersiz kılarak uygulanır `Finalize` üzerinde `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1539">Destructors are implemented by overriding the virtual method `Finalize` on `System.Object`.</span></span> <span data-ttu-id="6e059-1540">C# programları (ya da geçersiz kılmalarına) çağırın ya da bu yöntemi yok sayın izin verilmez doğrudan.</span><span class="sxs-lookup"><span data-stu-id="6e059-1540">C# programs are not permitted to override this method or call it (or overrides of it) directly.</span></span> <span data-ttu-id="6e059-1541">Örneğin, program</span><span class="sxs-lookup"><span data-stu-id="6e059-1541">For instance, the program</span></span>
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
<span data-ttu-id="6e059-1542">iki hatalar içeriyor.</span><span class="sxs-lookup"><span data-stu-id="6e059-1542">contains two errors.</span></span>

<span data-ttu-id="6e059-1543">Derleyici bu yöntem ve geçersiz kılmalar, tüm yok gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1543">The compiler behaves as if this method, and overrides of it, do not exist at all.</span></span> <span data-ttu-id="6e059-1544">Bu nedenle, bu program:</span><span class="sxs-lookup"><span data-stu-id="6e059-1544">Thus, this program:</span></span>
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
<span data-ttu-id="6e059-1545">geçerli olduğunu ve yöntemini gizliyor gösterilen `System.Object`'s `Finalize` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1545">is valid, and the method shown hides `System.Object`'s `Finalize` method.</span></span>

<span data-ttu-id="6e059-1546">Bir yok ediciden bir özel durum oluştuğunda, davranış hakkında ayrıntılı bilgi için bkz. [özel durumların nasıl işleneceğini](exceptions.md#how-exceptions-are-handled).</span><span class="sxs-lookup"><span data-stu-id="6e059-1546">For a discussion of the behavior when an exception is thrown from a destructor, see [How exceptions are handled](exceptions.md#how-exceptions-are-handled).</span></span>

## <a name="iterators"></a><span data-ttu-id="6e059-1547">Yineleyiciler</span><span class="sxs-lookup"><span data-stu-id="6e059-1547">Iterators</span></span>

<span data-ttu-id="6e059-1548">İşlev üyesi ([işlev üyeleri](expressions.md#function-members)) yineleyici bloğu kullanılarak uygulanan ([blokları](statements.md#blocks)) olarak adlandırılır bir ***yineleyici***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1548">A function member ([Function members](expressions.md#function-members)) implemented using an iterator block ([Blocks](statements.md#blocks)) is called an ***iterator***.</span></span>

<span data-ttu-id="6e059-1549">Dönüş türü karşılık gelen işlevi üyesinin Numaralandırıcı arabirimleri biri olduğu sürece, bir yineleyici bloğu işlevi üyesinin gövdesi olarak kullanılabilir ([Numaralandırıcı arabirimleri](classes.md#enumerator-interfaces)) veya numaralandırılabilir arabirimler ([Numaralandırılabilir arabirimler](classes.md#enumerable-interfaces)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1549">An iterator block may be used as the body of a function member as long as the return type of the corresponding function member is one of the enumerator interfaces ([Enumerator interfaces](classes.md#enumerator-interfaces)) or one of the enumerable interfaces ([Enumerable interfaces](classes.md#enumerable-interfaces)).</span></span> <span data-ttu-id="6e059-1550">Olarak gerçekleşebilir bir *method_body*, *operator_body* veya *accessor_body*bilgileriyse olayları, örnek oluşturucuları, statik oluşturucular ve Yıkıcılar olamaz yineleyiciler uygulanır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1550">It can occur as a *method_body*, *operator_body* or *accessor_body*, whereas events, instance constructors, static constructors and destructors cannot be implemented as iterators.</span></span>

<span data-ttu-id="6e059-1551">Yineleyici bloğu kullanarak bir işlev üyesi uygulandığında belirtmek için işlev üyesi biçimsel parametre listesi için bir derleme zamanı hatası olduğu `ref` veya `out` parametreleri.</span><span class="sxs-lookup"><span data-stu-id="6e059-1551">When a function member is implemented using an iterator block, it is a compile-time error for the formal parameter list of the function member to specify any `ref` or `out` parameters.</span></span>

### <a name="enumerator-interfaces"></a><span data-ttu-id="6e059-1552">Numaralandırıcı arabirimleri</span><span class="sxs-lookup"><span data-stu-id="6e059-1552">Enumerator interfaces</span></span>

<span data-ttu-id="6e059-1553">***Numaralandırıcı arabirimleri*** genel olmayan arabirimi `System.Collections.IEnumerator` ve tüm genel arabirim örneklemeleri `System.Collections.Generic.IEnumerator<T>`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1553">The ***enumerator interfaces*** are the non-generic interface `System.Collections.IEnumerator` and all instantiations of the generic interface `System.Collections.Generic.IEnumerator<T>`.</span></span> <span data-ttu-id="6e059-1554">Konuyu uzatmamak amacıyla, bu bölümde bu arabirimleri olarak başvurulan `IEnumerator` ve `IEnumerator<T>`sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="6e059-1554">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerator` and `IEnumerator<T>`, respectively.</span></span>

### <a name="enumerable-interfaces"></a><span data-ttu-id="6e059-1555">Numaralandırılabilir arabirimler</span><span class="sxs-lookup"><span data-stu-id="6e059-1555">Enumerable interfaces</span></span>

<span data-ttu-id="6e059-1556">***Numaralandırılabilir arabirimler*** genel olmayan arabirimi `System.Collections.IEnumerable` ve tüm genel arabirim örneklemeleri `System.Collections.Generic.IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1556">The ***enumerable interfaces*** are the non-generic interface `System.Collections.IEnumerable` and all instantiations of the generic interface `System.Collections.Generic.IEnumerable<T>`.</span></span> <span data-ttu-id="6e059-1557">Konuyu uzatmamak amacıyla, bu bölümde bu arabirimleri olarak başvurulan `IEnumerable` ve `IEnumerable<T>`sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="6e059-1557">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerable` and `IEnumerable<T>`, respectively.</span></span>

### <a name="yield-type"></a><span data-ttu-id="6e059-1558">Yield türü</span><span class="sxs-lookup"><span data-stu-id="6e059-1558">Yield type</span></span>

<span data-ttu-id="6e059-1559">Tümü aynı türdeki değerler bir yineleyici oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1559">An iterator produces a sequence of values, all of the same type.</span></span> <span data-ttu-id="6e059-1560">Bu tür adında ***yield türü*** yineleyici.</span><span class="sxs-lookup"><span data-stu-id="6e059-1560">This type is called the ***yield type*** of the iterator.</span></span>

*  <span data-ttu-id="6e059-1561">Yield türü döndüren bir yineleyicinin `IEnumerator` veya `IEnumerable` olduğu `object`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1561">The yield type of an iterator that returns `IEnumerator` or `IEnumerable` is `object`.</span></span>
*  <span data-ttu-id="6e059-1562">Yield türü döndüren bir yineleyicinin `IEnumerator<T>` veya `IEnumerable<T>` olduğu `T`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1562">The yield type of an iterator that returns `IEnumerator<T>` or `IEnumerable<T>` is `T`.</span></span>

### <a name="enumerator-objects"></a><span data-ttu-id="6e059-1563">Numaralandırıcı nesne</span><span class="sxs-lookup"><span data-stu-id="6e059-1563">Enumerator objects</span></span>

<span data-ttu-id="6e059-1564">Yineleyici bloğu kullanarak bir numaralandırıcı arabirim türü döndüren bir işlev üyesi uygulandığında işlevi üye çağırma hemen kod yineleyici bloğunda yürütmez.</span><span class="sxs-lookup"><span data-stu-id="6e059-1564">When a function member returning an enumerator interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="6e059-1565">Bunun yerine, bir ***Numaralandırıcı nesnesi*** oluşturulan ve döndürdü.</span><span class="sxs-lookup"><span data-stu-id="6e059-1565">Instead, an ***enumerator object*** is created and returned.</span></span> <span data-ttu-id="6e059-1566">Bu nesne yineleyici bloğunda belirtilen kodu kapsüller ve yineleyici bloğu içindeki kod yürütülmesi gerçekleşir, numaralandırıcı nesnenin `MoveNext` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1566">This object encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="6e059-1567">Numaralandırıcı nesnesi, aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="6e059-1567">An enumerator object has the following characteristics:</span></span>

*  <span data-ttu-id="6e059-1568">Bunu uygulayan `IEnumerator` ve `IEnumerator<T>`burada `T` yield yineleyici türüdür.</span><span class="sxs-lookup"><span data-stu-id="6e059-1568">It implements `IEnumerator` and `IEnumerator<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="6e059-1569">Bunu uygulayan `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1569">It implements `System.IDisposable`.</span></span>
*  <span data-ttu-id="6e059-1570">Bağımsız değişken değerlerini bir kopyasını (varsa) başlatılır ve işlev üyesine geçirilen örnek değer.</span><span class="sxs-lookup"><span data-stu-id="6e059-1570">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>
*  <span data-ttu-id="6e059-1571">Olası durumlar dört sahip ***önce***, ***çalıştıran***, ***askıya***, ve ***sonra***ve başlangıçta ***önce***  durumu.</span><span class="sxs-lookup"><span data-stu-id="6e059-1571">It has four potential states, ***before***, ***running***, ***suspended***, and ***after***, and is initially in the ***before*** state.</span></span>

<span data-ttu-id="6e059-1572">Numaralandırıcı nesnesi genellikle yineleyici bloğu içindeki kod kapsüller ve Numaralandırıcı arabirimleri uygulayan bir numaralandırıcı derleyici tarafından oluşturulan sınıfın bir örneği olduğu halde uygulamasının diğer yöntemler kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1572">An enumerator object is typically an instance of a compiler-generated enumerator class that encapsulates the code in the iterator block and implements the enumerator interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="6e059-1573">Derleyici tarafından oluşturulan bir numaralandırıcı sınıfı, bu sınıfı iç içe olamaz, doğrudan veya dolaylı olarak işlevi üye içeren sınıfın özel erişilebilirlik sahip olur ve derleyici kullanılmak üzere ayrılmış bir adı olacaktır ([tanımlayıcıları ](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1573">If an enumerator class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="6e059-1574">Numaralandırıcı nesnesi yukarıda belirtilen olandan daha fazla arabirim uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1574">An enumerator object may implement more interfaces than those specified above.</span></span>

<span data-ttu-id="6e059-1575">Aşağıdaki bölümlerde tam davranışını `MoveNext`, `Current`, ve `Dispose` üyeleri `IEnumerable` ve `IEnumerable<T>` arabirimi bir sabit listesi nesnesi tarafından sağlanan uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="6e059-1575">The following sections describe the exact behavior of the `MoveNext`, `Current`, and `Dispose` members of the `IEnumerable` and `IEnumerable<T>` interface implementations provided by an enumerator object.</span></span>

<span data-ttu-id="6e059-1576">Numaralandırıcı nesne desteklemeyen unutmayın `IEnumerator.Reset` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1576">Note that enumerator objects do not support the `IEnumerator.Reset` method.</span></span> <span data-ttu-id="6e059-1577">Bu yöntem çağırma neden olan bir `System.NotSupportedException` oluşturulması için.</span><span class="sxs-lookup"><span data-stu-id="6e059-1577">Invoking this method causes a `System.NotSupportedException` to be thrown.</span></span>

#### <a name="the-movenext-method"></a><span data-ttu-id="6e059-1578">MoveNext yöntemi</span><span class="sxs-lookup"><span data-stu-id="6e059-1578">The MoveNext method</span></span>

<span data-ttu-id="6e059-1579">`MoveNext` Yöntemi bir sabit listesi nesnesi, bir yineleyici bloğu kod kapsüller.</span><span class="sxs-lookup"><span data-stu-id="6e059-1579">The `MoveNext` method of an enumerator object encapsulates the code of an iterator block.</span></span> <span data-ttu-id="6e059-1580">Çağırma `MoveNext` yöntemi kümeleri ve yineleyici bloğu içinde kod yürütülmeden `Current` uygun şekilde Numaralandırıcı nesnesi bir özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1580">Invoking the `MoveNext` method executes code in the iterator block and sets the `Current` property of the enumerator object as appropriate.</span></span> <span data-ttu-id="6e059-1581">Tarafından gerçekleştirilen kesin eylemi `MoveNext` Numaralandırıcı nesnesi durumuna bağlıdır, `MoveNext` çağrılır:</span><span class="sxs-lookup"><span data-stu-id="6e059-1581">The precise action performed by `MoveNext` depends on the state of the enumerator object when `MoveNext` is invoked:</span></span>

*  <span data-ttu-id="6e059-1582">Numaralandırıcı nesne durumunu ise ***önce***, çağrılıyor `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="6e059-1582">If the state of the enumerator object is ***before***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="6e059-1583">Durumu ***çalıştıran***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1583">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="6e059-1584">Parametreleri başlatır (dahil olmak üzere `this`) yineleyici bloğun bağımsız değişken değerlerini ve örnek değeri Numaralandırıcı nesnesi başlatıldığında kaydedildi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1584">Initializes the parameters (including `this`) of the iterator block to the argument values and instance value saved when the enumerator object was initialized.</span></span>
   * <span data-ttu-id="6e059-1585">Yürütme (aşağıda açıklandığı gibi) kesilene kadar yineleyici bloğu baştan yürütür.</span><span class="sxs-lookup"><span data-stu-id="6e059-1585">Executes the iterator block from the beginning until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="6e059-1586">Numaralandırıcı nesne durumunu ise ***çalıştıran***, çağırma sonucu `MoveNext` belirtilmemiş.</span><span class="sxs-lookup"><span data-stu-id="6e059-1586">If the state of the enumerator object is ***running***, the result of invoking `MoveNext` is unspecified.</span></span>
*  <span data-ttu-id="6e059-1587">Numaralandırıcı nesne durumunu ise ***askıya***, çağrılıyor `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="6e059-1587">If the state of the enumerator object is ***suspended***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="6e059-1588">Durumu ***çalıştıran***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1588">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="6e059-1589">Tüm yerel değişkenleri ve parametreleri (Bu dahil) değerlerini yineleyici bloğu yürütülmesini son askıya alındığı zaman kaydedilmiş değerleri geri yükler.</span><span class="sxs-lookup"><span data-stu-id="6e059-1589">Restores the values of all local variables and parameters (including this) to the values saved when execution of the iterator block was last suspended.</span></span> <span data-ttu-id="6e059-1590">Bu değişkenler tarafından başvurulan tüm nesneleri içeriğini önceki MoveNext çağrısı beri değişmiş olabilir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="6e059-1590">Note that the contents of any objects referenced by these variables may have changed since the previous call to MoveNext.</span></span>
   * <span data-ttu-id="6e059-1591">Hemen yineleyici bloğu yürütülmesini sürdürür `yield return` (aşağıda açıklandığı gibi) yürütülmesi yarıda kadar devam eder ve yürütmeyi askıya alınması neden deyimi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1591">Resumes execution of the iterator block immediately following the `yield return` statement that caused the suspension of execution and continues until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="6e059-1592">Numaralandırıcı nesne durumunu ise ***sonra***, çağrılıyor `MoveNext` döndürür `false`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1592">If the state of the enumerator object is ***after***, invoking `MoveNext` returns `false`.</span></span>


<span data-ttu-id="6e059-1593">Zaman `MoveNext` yineleyici bloğun yürütme dört yollarla kesintiye: tarafından bir `yield return` deyimi tarafından bir `yield break` deyimi, bir özel durum ve yineleyici bloğunun sonu karşılaşıldığında tarafından oluşturulur ve tanesi yayılan yineleyici bloğu.</span><span class="sxs-lookup"><span data-stu-id="6e059-1593">When `MoveNext` executes the iterator block, execution can be interrupted in four ways: By a `yield return` statement, by a `yield break` statement, by encountering the end of the iterator block, and by an exception being thrown and propagated out of the iterator block.</span></span>

*  <span data-ttu-id="6e059-1594">Olduğunda bir `yield return` deyimi karşılaşıldığında ([yield deyimi](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="6e059-1594">When a `yield return` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="6e059-1595">Deyiminde belirtilen ifade değerlendirilir, örtük olarak yield türüne dönüştürülerek ve atanan `Current` Numaralandırıcı nesnesi bir özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1595">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
   * <span data-ttu-id="6e059-1596">Yineleyici gövdenin yürütülmesi askıya alınır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1596">Execution of the iterator body is suspended.</span></span> <span data-ttu-id="6e059-1597">Tüm yerel değişkenlerin ve parametrelerin değerlerini (dahil olmak üzere `this`) kaydedilir, bu konumu olarak `yield return` deyimi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1597">The values of all local variables and parameters (including `this`) are saved, as is the location of this `yield return` statement.</span></span> <span data-ttu-id="6e059-1598">Varsa `yield return` deyimi, bir veya daha fazla içinde `try` engeller, ilişkili `finally` blokları şu anda yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="6e059-1598">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
   * <span data-ttu-id="6e059-1599">Numaralandırıcı nesne durumunu değiştirilecek ***askıya***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1599">The state of the enumerator object is changed to ***suspended***.</span></span>
   * <span data-ttu-id="6e059-1600">`MoveNext` Yöntemi döndürür `true` yineleme sonraki değere başarıyla Gelişmiş belirten arayanına için.</span><span class="sxs-lookup"><span data-stu-id="6e059-1600">The `MoveNext` method returns `true` to its caller, indicating that the iteration successfully advanced to the next value.</span></span>
*  <span data-ttu-id="6e059-1601">Olduğunda bir `yield break` deyimi karşılaşıldığında ([yield deyimi](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="6e059-1601">When a `yield break` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="6e059-1602">Varsa `yield break` deyimi, bir veya daha fazla içinde `try` engeller, ilişkili `finally` bloğu yürütülür.</span><span class="sxs-lookup"><span data-stu-id="6e059-1602">If the `yield break` statement is within one or more `try` blocks, the associated `finally` blocks are executed.</span></span>
   * <span data-ttu-id="6e059-1603">Numaralandırıcı nesne durumunu değiştirilecek ***sonra***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1603">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="6e059-1604">`MoveNext` Yöntemi döndürür `false` yineleme tamamlandığını gösteren arayanına için.</span><span class="sxs-lookup"><span data-stu-id="6e059-1604">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="6e059-1605">Yineleyici gövdenin sonuna ne zaman karşılaştı:</span><span class="sxs-lookup"><span data-stu-id="6e059-1605">When the end of the iterator body is encountered:</span></span>
   * <span data-ttu-id="6e059-1606">Numaralandırıcı nesne durumunu değiştirilecek ***sonra***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1606">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="6e059-1607">`MoveNext` Yöntemi döndürür `false` yineleme tamamlandığını gösteren arayanına için.</span><span class="sxs-lookup"><span data-stu-id="6e059-1607">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="6e059-1608">Ne zaman bir özel durum ve yineleyici bloğu dışında yayılır:</span><span class="sxs-lookup"><span data-stu-id="6e059-1608">When an exception is thrown and propagated out of the iterator block:</span></span>
   * <span data-ttu-id="6e059-1609">Uygun `finally` yineleyici gövdenin bloklarında özel durum yayma tarafından yürütülen.</span><span class="sxs-lookup"><span data-stu-id="6e059-1609">Appropriate `finally` blocks in the iterator body will have been executed by the exception propagation.</span></span>
   * <span data-ttu-id="6e059-1610">Numaralandırıcı nesne durumunu değiştirilecek ***sonra***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1610">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="6e059-1611">Özel durum yayma çağırana devam `MoveNext` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1611">The exception propagation continues to the caller of the `MoveNext` method.</span></span>

#### <a name="the-current-property"></a><span data-ttu-id="6e059-1612">Geçerli özelliği</span><span class="sxs-lookup"><span data-stu-id="6e059-1612">The Current property</span></span>

<span data-ttu-id="6e059-1613">Numaralandırıcı nesne `Current` özelliği tarafından etkilenir `yield return` yineleyici bloğu içindeki deyimler.</span><span class="sxs-lookup"><span data-stu-id="6e059-1613">An enumerator object's `Current` property is affected by `yield return` statements in the iterator block.</span></span>

<span data-ttu-id="6e059-1614">Numaralandırıcı nesnesi olduğunda ***askıya*** durumunda değerini `Current` önceki çağrı tarafından ayarlanan değer `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1614">When an enumerator object is in the ***suspended*** state, the value of `Current` is the value set by the previous call to `MoveNext`.</span></span> <span data-ttu-id="6e059-1615">Numaralandırıcı nesnesi olduğunda ***önce***, ***çalıştıran***, veya ***sonra*** durumlarını erişme sonucunu `Current` belirtilmemiş.</span><span class="sxs-lookup"><span data-stu-id="6e059-1615">When an enumerator object is in the ***before***, ***running***, or ***after*** states, the result of accessing `Current` is unspecified.</span></span>

<span data-ttu-id="6e059-1616">Bir yield ile bir yineleyici için yazın dışında `object`, erişme sonucunu `Current` Numaralandırıcı nesnenin aracılığıyla `IEnumerable` uygulama karşılık gelen erişimi `Current` Numaralandırıcı nesnenin aracılığıyla `IEnumerator<T>` Uygulama ve sonucu atama `object`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1616">For an iterator with a yield type other than `object`, the result of accessing `Current` through the enumerator object's `IEnumerable` implementation corresponds to accessing `Current` through the enumerator object's `IEnumerator<T>` implementation and casting the result to `object`.</span></span>

#### <a name="the-dispose-method"></a><span data-ttu-id="6e059-1617">Dispose yöntemi</span><span class="sxs-lookup"><span data-stu-id="6e059-1617">The Dispose method</span></span>

<span data-ttu-id="6e059-1618">`Dispose` Yöntemi tarafından Numaralandırıcı nesnesi getirmek yineleme temizlemek için kullanılan ***sonra*** durumu.</span><span class="sxs-lookup"><span data-stu-id="6e059-1618">The `Dispose` method is used to clean up the iteration by bringing the enumerator object to the ***after*** state.</span></span>

*  <span data-ttu-id="6e059-1619">Numaralandırıcı nesne durumunu ise ***önce***, çağrılıyor `Dispose` duruma geçmesi ***sonra***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1619">If the state of the enumerator object is ***before***, invoking `Dispose` changes the state to ***after***.</span></span>
*  <span data-ttu-id="6e059-1620">Numaralandırıcı nesne durumunu ise ***çalıştıran***, çağırma sonucu `Dispose` belirtilmemiş.</span><span class="sxs-lookup"><span data-stu-id="6e059-1620">If the state of the enumerator object is ***running***, the result of invoking `Dispose` is unspecified.</span></span>
*  <span data-ttu-id="6e059-1621">Numaralandırıcı nesne durumunu ise ***askıya***, çağrılıyor `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="6e059-1621">If the state of the enumerator object is ***suspended***, invoking `Dispose`:</span></span>
   * <span data-ttu-id="6e059-1622">Durumu ***çalıştıran***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1622">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="6e059-1623">Son olarak çalıştırıldığında herhangi finally blokları yürütür `yield return` ifade edilen bir `yield break` deyimi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1623">Executes any finally blocks as if the last executed `yield return` statement were a `yield break` statement.</span></span> <span data-ttu-id="6e059-1624">Bu durum ve yineleyici gövdenin dışında yayılan bir özel durum neden olursa, numaralandırıcı nesne durumunu kümesine ***sonra*** ve özel durum çağırana yayılır `Dispose` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1624">If this causes an exception to be thrown and propagated out of the iterator body, the state of the enumerator object is set to ***after*** and the exception is propagated to the caller of the `Dispose` method.</span></span>
   * <span data-ttu-id="6e059-1625">Durumu ***sonra***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1625">Changes the state to ***after***.</span></span>
*  <span data-ttu-id="6e059-1626">Numaralandırıcı nesne durumunu ise ***sonra***, çağrılıyor `Dispose` herhangi bir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1626">If the state of the enumerator object is ***after***, invoking `Dispose` has no affect.</span></span>

### <a name="enumerable-objects"></a><span data-ttu-id="6e059-1627">Numaralandırılabilir nesneleri</span><span class="sxs-lookup"><span data-stu-id="6e059-1627">Enumerable objects</span></span>

<span data-ttu-id="6e059-1628">Sıralanabilir arabirim türü döndüren bir işlev üyesi yineleyici bloğu kullanarak uygulandığında işlevi üye çağırma hemen kod yineleyici bloğunda yürütmez.</span><span class="sxs-lookup"><span data-stu-id="6e059-1628">When a function member returning an enumerable interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="6e059-1629">Bunun yerine, bir ***numaralandırma nesnesi*** oluşturulan ve döndürdü.</span><span class="sxs-lookup"><span data-stu-id="6e059-1629">Instead, an ***enumerable object*** is created and returned.</span></span> <span data-ttu-id="6e059-1630">Numaralandırılabilir nesnenin `GetEnumerator` yöntemi döndürür yineleyici bloğunda kodu kapsülleyen bir sabit listesi nesnesi belirtilen ve yineleyici bloğu içindeki kod yürütülmesi gerçekleşir, numaralandırıcı nesnenin `MoveNext` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1630">The enumerable object's `GetEnumerator` method returns an enumerator object that encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="6e059-1631">Bir numaralandırma nesnesi, aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="6e059-1631">An enumerable object has the following characteristics:</span></span>

*  <span data-ttu-id="6e059-1632">Bunu uygulayan `IEnumerable` ve `IEnumerable<T>`burada `T` yield yineleyici türüdür.</span><span class="sxs-lookup"><span data-stu-id="6e059-1632">It implements `IEnumerable` and `IEnumerable<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="6e059-1633">Bağımsız değişken değerlerini bir kopyasını (varsa) başlatılır ve işlev üyesine geçirilen örnek değer.</span><span class="sxs-lookup"><span data-stu-id="6e059-1633">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>

<span data-ttu-id="6e059-1634">Bir numaralandırma nesnesi genellikle bir yineleyici bloğu içindeki kod kapsüller ve numaralandırılabilir arabirimler uygulayan bir sınıfın derleyicinin ürettiği numaralandırılabilir örneğidir, ancak uygulama, diğer yöntemler kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1634">An enumerable object is typically an instance of a compiler-generated enumerable class that encapsulates the code in the iterator block and implements the enumerable interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="6e059-1635">Numaralandırılabilir bir sınıf, derleyici tarafından oluşturulan, o sınıfı iç içe olamaz, doğrudan veya dolaylı olarak işlevi üye içeren sınıfın özel erişilebilirlik sahip olur ve derleyici kullanılmak üzere ayrılmış bir adı olacaktır ([tanımlayıcıları ](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1635">If an enumerable class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="6e059-1636">Bir numaralandırma nesnesi yukarıda belirtilen olandan daha fazla arabirim uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1636">An enumerable object may implement more interfaces than those specified above.</span></span> <span data-ttu-id="6e059-1637">Özellikle, bir numaralandırma nesnesi de uygulayabilir `IEnumerator` ve `IEnumerator<T>`, bir numaralandırılabilir hem bir numaralandırıcı olarak hizmet edecek şekilde etkinleştirme.</span><span class="sxs-lookup"><span data-stu-id="6e059-1637">In particular, an enumerable object may also implement `IEnumerator` and `IEnumerator<T>`, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="6e059-1638">Bu tür uygulama, ilk kez bir nesnenin numaralandırılabilir `GetEnumerator` yöntemi çağrılır, numaralandırılabilir nesne döndürülür.</span><span class="sxs-lookup"><span data-stu-id="6e059-1638">In that type of implementation, the first time an enumerable object's `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="6e059-1639">Sonraki çağrılar nesnenin numaralandırılabilir `GetEnumerator`herhangi biri, döndürmek, numaralandırılabilir nesnesinin bir kopyasını.</span><span class="sxs-lookup"><span data-stu-id="6e059-1639">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="6e059-1640">Bu nedenle, her Numaralayıcı kendi durumunu sahiptir ve başka bir numaralandırıcı değişiklikleri etkilemez döndürdü.</span><span class="sxs-lookup"><span data-stu-id="6e059-1640">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span>

#### <a name="the-getenumerator-method"></a><span data-ttu-id="6e059-1641">GetEnumerator yöntemi</span><span class="sxs-lookup"><span data-stu-id="6e059-1641">The GetEnumerator method</span></span>

<span data-ttu-id="6e059-1642">Bir numaralandırma nesnesi bir uygulamasını sağlar `GetEnumerator` yöntemlerinin `IEnumerable` ve `IEnumerable<T>` arabirimleri.</span><span class="sxs-lookup"><span data-stu-id="6e059-1642">An enumerable object provides an implementation of the `GetEnumerator` methods of the `IEnumerable` and `IEnumerable<T>` interfaces.</span></span> <span data-ttu-id="6e059-1643">İki `GetEnumerator` yöntemleri alır ve bir kullanılabilir sabit listesi nesnesi döndüren ortak bir uygulama paylaşın.</span><span class="sxs-lookup"><span data-stu-id="6e059-1643">The two `GetEnumerator` methods share a common implementation that acquires and returns an available enumerator object.</span></span> <span data-ttu-id="6e059-1644">Numaralandırıcı nesnesi bağımsız değişken değerleri başlatılır ve başlatıldı ancak aksi numaralandırma nesnesi, kaydedilen değeri açıklandığı Numaralandırıcı nesnesi işlevleri örnek [Numaralandırıcı nesneleri](classes.md#enumerator-objects).</span><span class="sxs-lookup"><span data-stu-id="6e059-1644">The enumerator object is initialized with the argument values and instance value saved when the enumerable object was initialized, but otherwise the enumerator object functions as described in [Enumerator objects](classes.md#enumerator-objects).</span></span>

### <a name="implementation-example"></a><span data-ttu-id="6e059-1645">Uygulama örneği</span><span class="sxs-lookup"><span data-stu-id="6e059-1645">Implementation example</span></span>

<span data-ttu-id="6e059-1646">Bu bölümde, standart C# yapılarını açısından Yineleyicilerin olası bir uygulaması açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1646">This section describes a possible implementation of iterators in terms of standard C# constructs.</span></span> <span data-ttu-id="6e059-1647">Burada açıklanan uygulama, Microsoft C# derleyicisi tarafından kullanılan aynı ilkelere dayalı ancak mandated uygulaması veya yalnızca bir mümkün olmadığı göre Hayır değil.</span><span class="sxs-lookup"><span data-stu-id="6e059-1647">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation or the only one possible.</span></span>

<span data-ttu-id="6e059-1648">Aşağıdaki `Stack<T>` sınıfının uygular, `GetEnumerator` yöntemi kullanarak bir yineleyici.</span><span class="sxs-lookup"><span data-stu-id="6e059-1648">The following `Stack<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="6e059-1649">Yineleyici yığınının alt sipariş üstteki öğeleri sıralar.</span><span class="sxs-lookup"><span data-stu-id="6e059-1649">The iterator enumerates the elements of the stack in top to bottom order.</span></span>

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

<span data-ttu-id="6e059-1650">`GetEnumerator` Yöntemi dönüştürülür örneklemesi yineleyici bloğu içindeki kod kapsülleyen bir numaralandırıcı derleyici tarafından oluşturulan sınıfın içine aşağıdaki gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1650">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="6e059-1651">Önceki çeviri yineleyici bloğu içindeki kod bir Durum makinesi açık ve bulundukları `MoveNext` Numaralandırıcı sınıfının yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1651">In the preceding translation, the code in the iterator block is turned into a state machine and placed in the `MoveNext` method of the enumerator class.</span></span> <span data-ttu-id="6e059-1652">Ayrıca, yerel değişken `i` Numaralandırıcı nesnesi bir alana çağrıları arasında mevcut devam edebilmesi açık `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1652">Furthermore, the local variable `i` is turned into a field in the enumerator object so it can continue to exist across invocations of `MoveNext`.</span></span>

<span data-ttu-id="6e059-1653">Aşağıdaki örnek, basit bir çarpan tablosunu 1-10 arasındaki tamsayılar yazdırır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1653">The following example prints a simple multiplication table of the integers 1 through 10.</span></span> <span data-ttu-id="6e059-1654">`FromTo` Örnek yönteminde sıralanabilir bir nesne döndürür ve bir yineleyici kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1654">The `FromTo` method in the example returns an enumerable object and is implemented using an iterator.</span></span>

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

<span data-ttu-id="6e059-1655">`FromTo` Yöntemi dönüştürülür örneklemesi derleyici tarafından oluşturulan ve yineleyici bloğu içindeki kod kapsülleyen bir numaralandırılabilir sınıfı içine aşağıdaki gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1655">The `FromTo` method can be translated into an instantiation of a compiler-generated enumerable class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="6e059-1656">Numaralandırılabilir arabirimler hem bir numaralandırılabilir hem bir numaralandırıcı olarak hizmet edecek şekilde etkinleştirme Numaralandırıcı arabirimleri numaralandırılabilir sınıfı uygular.</span><span class="sxs-lookup"><span data-stu-id="6e059-1656">The enumerable class implements both the enumerable interfaces and the enumerator interfaces, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="6e059-1657">İlk kez `GetEnumerator` yöntemi çağrılır, numaralandırılabilir nesne döndürülür.</span><span class="sxs-lookup"><span data-stu-id="6e059-1657">The first time the `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="6e059-1658">Sonraki çağrılar nesnenin numaralandırılabilir `GetEnumerator`herhangi biri, döndürmek, numaralandırılabilir nesnesinin bir kopyasını.</span><span class="sxs-lookup"><span data-stu-id="6e059-1658">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="6e059-1659">Bu nedenle, her Numaralayıcı kendi durumunu sahiptir ve başka bir numaralandırıcı değişiklikleri etkilemez döndürdü.</span><span class="sxs-lookup"><span data-stu-id="6e059-1659">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span> <span data-ttu-id="6e059-1660">`Interlocked.CompareExchange` Yöntemi iş parçacığı açısından güvenli çalışmasını sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6e059-1660">The `Interlocked.CompareExchange` method is used to ensure thread-safe operation.</span></span>

<span data-ttu-id="6e059-1661">`from` Ve `to` parametreleri alanlara numaralandırılabilir sınıfında etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1661">The `from` and `to` parameters are turned into fields in the enumerable class.</span></span> <span data-ttu-id="6e059-1662">Çünkü `from` yineleyici bloğunda, ek bir değişiklik `__from` alan için verilen ilk değer tutacak sunulmuştur `from` içinde her Numaralandırıcı.</span><span class="sxs-lookup"><span data-stu-id="6e059-1662">Because `from` is modified in the iterator block, an additional `__from` field is introduced to hold the initial value given to `from` in each enumerator.</span></span>

<span data-ttu-id="6e059-1663">`MoveNext` Yöntem bir `InvalidOperationException` ne zaman çağrılırsa `__state` olduğu `0`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1663">The `MoveNext` method throws an `InvalidOperationException` if it is called when `__state` is `0`.</span></span> <span data-ttu-id="6e059-1664">Bu ilk çağırmadan bir sabit listesi nesnesi olarak numaralandırma nesnesi kullanımını karşı korur `GetEnumerator`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1664">This protects against use of the enumerable object as an enumerator object without first calling `GetEnumerator`.</span></span>

<span data-ttu-id="6e059-1665">Aşağıdaki örnek, bir basit ağaç sınıfı gösterir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1665">The following example shows a simple tree class.</span></span> <span data-ttu-id="6e059-1666">`Tree<T>` Sınıfının uygular, `GetEnumerator` yöntemi kullanarak bir yineleyici.</span><span class="sxs-lookup"><span data-stu-id="6e059-1666">The `Tree<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="6e059-1667">Yineleyici içtakı sırayla ağacı öğeleri sıralar.</span><span class="sxs-lookup"><span data-stu-id="6e059-1667">The iterator enumerates the elements of the tree in infix order.</span></span>

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

<span data-ttu-id="6e059-1668">`GetEnumerator` Yöntemi dönüştürülür örneklemesi yineleyici bloğu içindeki kod kapsülleyen bir numaralandırıcı derleyici tarafından oluşturulan sınıfın içine aşağıdaki gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1668">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="6e059-1669">Kullanılan derleyicinin ürettiği değerlendirmesidir `foreach` deyimleri yükseltilmiş içine `__left` ve `__right` Numaralandırıcı nesnesi alanları.</span><span class="sxs-lookup"><span data-stu-id="6e059-1669">The compiler generated temporaries used in the `foreach` statements are lifted into the `__left` and `__right` fields of the enumerator object.</span></span> <span data-ttu-id="6e059-1670">`__state` Numaralandırıcı nesnesi alanının dikkatli bir şekilde güncelleştirilmiş böylece doğru `Dispose()` yöntemin çağrılacağı doğru değilse bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1670">The `__state` field of the enumerator object is carefully updated so that the correct `Dispose()` method will be called correctly if an exception is thrown.</span></span> <span data-ttu-id="6e059-1671">Not ile basit kod yazmayı mümkün değildir, `foreach` deyimleri.</span><span class="sxs-lookup"><span data-stu-id="6e059-1671">Note that it is not possible to write the translated code with simple `foreach` statements.</span></span>

## <a name="async-functions"></a><span data-ttu-id="6e059-1672">Zaman uyumsuz işlevleri</span><span class="sxs-lookup"><span data-stu-id="6e059-1672">Async functions</span></span>

<span data-ttu-id="6e059-1673">Bir yöntem ([yöntemleri](classes.md#methods)) veya anonim işlevi ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) ile `async` değiştiricisi çağrıldığında bir ***zaman uyumsuz işlev***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1673">A method ([Methods](classes.md#methods)) or anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) with the `async` modifier is called an ***async function***.</span></span> <span data-ttu-id="6e059-1674">Genel olarak, terim ***zaman uyumsuz*** herhangi bir tür olan işlev açıklamak için kullanılan `async` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="6e059-1674">In general, the term ***async*** is used to describe any kind of function that has the `async` modifier.</span></span>

<span data-ttu-id="6e059-1675">Bir derleme zamanı hata belirtmek için bir zaman uyumsuz işlev biçimsel parametre listesinde `ref` veya `out` parametreleri.</span><span class="sxs-lookup"><span data-stu-id="6e059-1675">It is a compile-time error for the formal parameter list of an async function to specify any `ref` or `out` parameters.</span></span>

<span data-ttu-id="6e059-1676">*Döndür_tür* zaman uyumsuz bir yöntem ya da olmalıdır `void` veya ***görev türü***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1676">The *return_type* of an async method must be either `void` or a ***task type***.</span></span> <span data-ttu-id="6e059-1677">Görev türleri `System.Threading.Tasks.Task` ve türleri oluşturulan gelen `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1677">The task types are `System.Threading.Tasks.Task` and types constructed from `System.Threading.Tasks.Task<T>`.</span></span> <span data-ttu-id="6e059-1678">Konuyu uzatmamak amacıyla, bu bölümde bu tür olarak başvurulan `Task` ve `Task<T>`sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="6e059-1678">For the sake of brevity, in this chapter these types are referenced as `Task` and `Task<T>`, respectively.</span></span> <span data-ttu-id="6e059-1679">Görev türü döndüren bir zaman uyumsuz yöntem, görev döndüren olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1679">An async method returning a task type is said to be task-returning.</span></span>

<span data-ttu-id="6e059-1680">Görev türleri tam tanımı tanımlanan uygulama, ancak dil açısından bakıldığında görev türü durumlardan birinde eksik başarılı veya hatalı.</span><span class="sxs-lookup"><span data-stu-id="6e059-1680">The exact definition of the task types is implementation defined, but from the language's point of view a task type is in one of the states incomplete, succeeded or faulted.</span></span> <span data-ttu-id="6e059-1681">İlgili bir özel durum hatalı bir görev kaydeder.</span><span class="sxs-lookup"><span data-stu-id="6e059-1681">A faulted task records a pertinent exception.</span></span> <span data-ttu-id="6e059-1682">Başarılı bir `Task<T>` türünün bir sonucunun kayıtları `T`.</span><span class="sxs-lookup"><span data-stu-id="6e059-1682">A succeeded `Task<T>` records a result of type `T`.</span></span> <span data-ttu-id="6e059-1683">Görev türü beklenebilir ve bu nedenle, işlenenler olabilir await ifadeleri ([Await ifadeleri](expressions.md#await-expressions)).</span><span class="sxs-lookup"><span data-stu-id="6e059-1683">Task types are awaitable, and can therefore be the operands of await expressions ([Await expressions](expressions.md#await-expressions)).</span></span>

<span data-ttu-id="6e059-1684">Bir zaman uyumsuz işlev çağrısını değerlendirme askıya alma olanağı vardır await ifadeleri yoluyla ([Await ifadeleri](expressions.md#await-expressions)) gövdesinde.</span><span class="sxs-lookup"><span data-stu-id="6e059-1684">An async function invocation has the ability to suspend evaluation by means of await expressions ([Await expressions](expressions.md#await-expressions)) in its body.</span></span> <span data-ttu-id="6e059-1685">Değerlendirme daha sonra sürdürüldü askıya alma noktasında await ifadesi yoluyla bir ***sürdürme temsilci***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1685">Evaluation may later be resumed at the point of the suspending await expression by means of a ***resumption delegate***.</span></span> <span data-ttu-id="6e059-1686">Sürdürme temsilci türünde `System.Action`, ve çağrıldığında, zaman uyumsuz işlev çağrısının değerlendirme seçeneğindeki await ifadesine kaldığı yerden devam eder.</span><span class="sxs-lookup"><span data-stu-id="6e059-1686">The resumption delegate is of type `System.Action`, and when it is invoked, evaluation of the async function invocation will resume from the await expression where it left off.</span></span> <span data-ttu-id="6e059-1687">***Geçerli arayan*** bir zaman uyumsuz çağırma özgün çağıran işlev çağrısını hiçbir zaman askıya alınırsa veya sürdürme temsilci en son çağıran aksi işlevidir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1687">The ***current caller*** of an async function invocation is the original caller if the function invocation has never been suspended, or the most recent caller of the resumption delegate otherwise.</span></span>

### <a name="evaluation-of-a-task-returning-async-function"></a><span data-ttu-id="6e059-1688">Bir görev döndüren zaman uyumsuz İşlev değerlendirmesi</span><span class="sxs-lookup"><span data-stu-id="6e059-1688">Evaluation of a task-returning async function</span></span>

<span data-ttu-id="6e059-1689">Bir görev döndüren zaman uyumsuz işlev çağırmayı oluşturulacak döndürülen görevin türü örneği neden olur.</span><span class="sxs-lookup"><span data-stu-id="6e059-1689">Invocation of a task-returning async function causes an instance of the returned task type to be generated.</span></span> <span data-ttu-id="6e059-1690">Bu adlandırılır ***dönüş görev*** zaman uyumsuz işlev.</span><span class="sxs-lookup"><span data-stu-id="6e059-1690">This is called the ***return task*** of the async function.</span></span> <span data-ttu-id="6e059-1691">Görev başlangıçta bir eksik durumda.</span><span class="sxs-lookup"><span data-stu-id="6e059-1691">The task is initially in an incomplete state.</span></span>

<span data-ttu-id="6e059-1692">(Bir await deyimindeki ulaşma) ya da askıya alındı veya sonlandırılırsa, kadar zaman uyumsuz işlev gövdesi sonra hangi noktası denetim görevi Döndür birlikte çağırana döndürülen değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1692">The async function body is then evaluated until it is either suspended (by reaching an await expression) or terminates, at which point control is returned to the caller, along with the return task.</span></span>

<span data-ttu-id="6e059-1693">Zaman uyumsuz işlev gövdesini sonlandırıldığında dönüş görev tamamlanmamış durumundan taşınır:</span><span class="sxs-lookup"><span data-stu-id="6e059-1693">When the body of the async function terminates, the return task is moved out of the incomplete state:</span></span>

*  <span data-ttu-id="6e059-1694">İşlev gövdesi bir return deyimi veya gövdesinin sonuna ulaşmadan sonucu olarak sona ererse, herhangi bir sonuç değeri başarılı bir duruma getirilir dönüş görev kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1694">If the function body terminates as the result of reaching a return statement or the end of the body, any result value is recorded in the return task, which is put into a succeeded state.</span></span>
*  <span data-ttu-id="6e059-1695">İşlev gövdesi yakalanmayan bir özel durum sonucu olarak kesilip kesilmediğini ([fırlatma](statements.md#the-throw-statement)) özel durum, hatalı bir duruma getirilir dönüş görev kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1695">If the function body terminates as the result of an uncaught exception ([The throw statement](statements.md#the-throw-statement)) the exception is recorded in the return task which is put into a faulted state.</span></span>

### <a name="evaluation-of-a-void-returning-async-function"></a><span data-ttu-id="6e059-1696">Void döndüren zaman uyumsuz işlev değerlendirme</span><span class="sxs-lookup"><span data-stu-id="6e059-1696">Evaluation of a void-returning async function</span></span>

<span data-ttu-id="6e059-1697">Zaman uyumsuz işlev dönüş türü ise `void`, değerlendirme farklı Yukarıdakilerden şu şekilde: hiçbir görev döndürdüğünden işlevi yerine tamamlama ve geçerli iş parçacığının özel durumlar iletişim kuran ***eşitleme bağlam***.</span><span class="sxs-lookup"><span data-stu-id="6e059-1697">If the return type of the async function is `void`, evaluation differs from the above in the following way: Because no task is returned, the function instead communicates completion and exceptions to the current thread's ***synchronization context***.</span></span> <span data-ttu-id="6e059-1698">Eşitleme bağlamı tam tanımı uygulamaya bağlıdır, ancak bir gösterimiyse, geçerli iş parçacığı "nerede" çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="6e059-1698">The exact definition of synchronization context is implementation-dependent, but is a representation of "where" the current thread is running.</span></span> <span data-ttu-id="6e059-1699">Void döndüren zaman uyumsuz işlev değerlendirme yenilenirse, başarıyla tamamlandığında veya yakalanmayan bir özel durum oluşturulmasına neden olur, eşitleme bağlamı bildirilir.</span><span class="sxs-lookup"><span data-stu-id="6e059-1699">The synchronization context is notified when evaluation of a void-returning async function commences, completes successfully, or causes an uncaught exception to be thrown.</span></span>

<span data-ttu-id="6e059-1700">Bu, kaç void döndüren zaman uyumsuz işlevleri altında çalışan izler ve bunların dışında özel durum yayma nasıl karar vermek için bağlam sağlar.</span><span class="sxs-lookup"><span data-stu-id="6e059-1700">This allows the context to keep track of how many void-returning async functions are running under it, and to decide how to propagate exceptions coming out of them.</span></span>
