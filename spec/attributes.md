# <a name="attributes"></a><span data-ttu-id="9de6f-101">Öznitelikler</span><span class="sxs-lookup"><span data-stu-id="9de6f-101">Attributes</span></span>

<span data-ttu-id="9de6f-102">C# dili çoğunu programda tanımlanmış varlıkları hakkında tanımlayıcı bilgileri belirtmek Programcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="9de6f-102">Much of the C# language enables the programmer to specify declarative information about the entities defined in the program.</span></span> <span data-ttu-id="9de6f-103">Örneğin, bir sınıf içinde bir yöntemin erişilebilirliği ile tasarlayarak belirtilen *method_modifier*s `public`, `protected`, `internal`, ve `private`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-103">For example, the accessibility of a method in a class is specified by decorating it with the *method_modifier*s `public`, `protected`, `internal`, and `private`.</span></span>

<span data-ttu-id="9de6f-104">C# programcıları yeni tür adında bildirim temelli bilgileri imkan sağlayan ***öznitelikleri***.</span><span class="sxs-lookup"><span data-stu-id="9de6f-104">C# enables programmers to invent new kinds of declarative information, called ***attributes***.</span></span> <span data-ttu-id="9de6f-105">Programcıları sonra çeşitli program varlıklara öznitelikleri eklemek ve bir çalışma zamanı ortamında öznitelik bilgileri alınamıyor.</span><span class="sxs-lookup"><span data-stu-id="9de6f-105">Programmers can then attach attributes to various program entities, and retrieve attribute information in a run-time environment.</span></span> <span data-ttu-id="9de6f-106">Örneğin, bir çerçeve tanımlayabilir bir `HelpAttribute` belirli program öğelerini (örneğin, sınıflar ve metotlar) üzerinde bu program öğelerini belgelerinin bir eşleme sağlamak üzere yerleştirilebilecek bir özniteliği.</span><span class="sxs-lookup"><span data-stu-id="9de6f-106">For instance, a framework might define a `HelpAttribute` attribute that can be placed on certain program elements (such as classes and methods) to provide a mapping from those program elements to their documentation.</span></span>

<span data-ttu-id="9de6f-107">Öznitelikleri, öznitelik sınıflarının bildirimi tanımlanır ([öznitelik sınıfları](attributes.md#attribute-classes)), konumsal ve adlandırılmış parametreleri olabilir ([konumsal ve adlandırılmış parametreleri](attributes.md#positional-and-named-parameters)).</span><span class="sxs-lookup"><span data-stu-id="9de6f-107">Attributes are defined through the declaration of attribute classes ([Attribute classes](attributes.md#attribute-classes)), which may have positional and named parameters ([Positional and named parameters](attributes.md#positional-and-named-parameters)).</span></span> <span data-ttu-id="9de6f-108">Öznitelikleri, öznitelik özelliklerini kullanarak bir C# programı varlıklara eklenir ([öznitelik belirtimi](attributes.md#attribute-specification)) ve çalışma zamanında öznitelik örnekleri alınabilir ([öznitelik örneklerini](attributes.md#attribute-instances)).</span><span class="sxs-lookup"><span data-stu-id="9de6f-108">Attributes are attached to entities in a C# program using attribute specifications ([Attribute specification](attributes.md#attribute-specification)), and can be retrieved at run-time as attribute instances ([Attribute instances](attributes.md#attribute-instances)).</span></span>

## <a name="attribute-classes"></a><span data-ttu-id="9de6f-109">Öznitelik sınıfları</span><span class="sxs-lookup"><span data-stu-id="9de6f-109">Attribute classes</span></span>

<span data-ttu-id="9de6f-110">Soyut sınıftan türeyen bir sınıf `System.Attribute`, doğrudan veya dolaylı olarak olan bir ***öznitelik sınıfını***.</span><span class="sxs-lookup"><span data-stu-id="9de6f-110">A class that derives from the abstract class `System.Attribute`, whether directly or indirectly, is an ***attribute class***.</span></span> <span data-ttu-id="9de6f-111">Öznitelik sınıfı bildirimi tür yeni tanımlar ***özniteliği*** bir bildiriminde yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-111">The declaration of an attribute class defines a new kind of ***attribute*** that can be placed on a declaration.</span></span> <span data-ttu-id="9de6f-112">Kural gereği, öznitelik sınıfları bir sonekiyle adlandırılır `Attribute`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-112">By convention, attribute classes are named with a suffix of `Attribute`.</span></span> <span data-ttu-id="9de6f-113">Kullanan bir öznitelik eklemek veya bu sonek atlayın.</span><span class="sxs-lookup"><span data-stu-id="9de6f-113">Uses of an attribute may either include or omit this suffix.</span></span>

### <a name="attribute-usage"></a><span data-ttu-id="9de6f-114">Öznitelik kullanımı</span><span class="sxs-lookup"><span data-stu-id="9de6f-114">Attribute usage</span></span>

<span data-ttu-id="9de6f-115">Öznitelik `AttributeUsage` ([AttributeUsage özniteliği](attributes.md#the-attributeusage-attribute)) bir öznitelik sınıfı nasıl kullanılabileceğini tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-115">The attribute `AttributeUsage` ([The AttributeUsage attribute](attributes.md#the-attributeusage-attribute)) is used to describe how an attribute class can be used.</span></span>

<span data-ttu-id="9de6f-116">`AttributeUsage` konumsal bir parametreye sahiptir ([konumsal ve adlandırılmış parametreleri](attributes.md#positional-and-named-parameters)) üzerinde kullanılabileceği bildirimleri türlerini belirtmek bir öznitelik sınıfı sağlar.</span><span class="sxs-lookup"><span data-stu-id="9de6f-116">`AttributeUsage` has a positional parameter ([Positional and named parameters](attributes.md#positional-and-named-parameters)) that enables an attribute class to specify the kinds of declarations on which it can be used.</span></span> <span data-ttu-id="9de6f-117">Örnek</span><span class="sxs-lookup"><span data-stu-id="9de6f-117">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Interface)]
public class SimpleAttribute: Attribute 
{
    ...
}
```

<span data-ttu-id="9de6f-118">adlı bir öznitelik sınıfı tanımlar `SimpleAttribute` üzerinde yerleştirilebileceğini *class_declaration*s ve *interface_declaration*yalnızca s.</span><span class="sxs-lookup"><span data-stu-id="9de6f-118">defines an attribute class named `SimpleAttribute` that can be placed on *class_declaration*s and *interface_declaration*s only.</span></span> <span data-ttu-id="9de6f-119">Örnek</span><span class="sxs-lookup"><span data-stu-id="9de6f-119">The example</span></span>

```csharp
[Simple] class Class1 {...}

[Simple] interface Interface1 {...}
```

<span data-ttu-id="9de6f-120">bazı kullanımlarını gösterir `Simple` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="9de6f-120">shows several uses of the `Simple` attribute.</span></span> <span data-ttu-id="9de6f-121">Bu öznitelik adı ile tanımlanır ancak `SimpleAttribute`, bu öznitelik kullanıldığında `Attribute` soneki atlanabilir, kısa adı elde `Simple`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-121">Although this attribute is defined with the name `SimpleAttribute`, when this attribute is used, the `Attribute` suffix may be omitted, resulting in the short name `Simple`.</span></span> <span data-ttu-id="9de6f-122">Bu nedenle, yukarıdaki örnek aşağıdaki anlam olarak eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="9de6f-122">Thus, the example above is semantically equivalent to the following:</span></span>

```csharp
[SimpleAttribute] class Class1 {...}

[SimpleAttribute] interface Interface1 {...}
```

<span data-ttu-id="9de6f-123">`AttributeUsage` adlandırılmış bir parametreye sahiptir ([konumsal ve adlandırılmış parametreleri](attributes.md#positional-and-named-parameters)) olarak adlandırılan `AllowMultiple`, öznitelik belirli bir varlık için birden çok kez belirtilip belirtilemeyeceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-123">`AttributeUsage` has a named parameter ([Positional and named parameters](attributes.md#positional-and-named-parameters)) called `AllowMultiple`, which indicates whether the attribute can be specified more than once for a given entity.</span></span> <span data-ttu-id="9de6f-124">Varsa `AllowMultiple` sınıfı bir öznitelik için geçerlidir ve ardından bu öznitelik sınıfı olduğu bir ***çok kullanımı öznitelik sınıfı***ve bir varlık üzerinde birden çok kez belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-124">If `AllowMultiple` for an attribute class is true, then that attribute class is a ***multi-use attribute class***, and can be specified more than once on an entity.</span></span> <span data-ttu-id="9de6f-125">Varsa `AllowMultiple` sınıfı için bir öznitelik false veya belirtilmemiş ve ardından bu öznitelik sınıfı olduğu bir ***tek kullanımlık öznitelik sınıfı***ve bir varlık üzerinde en fazla bir kez belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-125">If `AllowMultiple` for an attribute class is false or it is unspecified, then that attribute class is a ***single-use attribute class***, and can be specified at most once on an entity.</span></span>

<span data-ttu-id="9de6f-126">Örnek</span><span class="sxs-lookup"><span data-stu-id="9de6f-126">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class, AllowMultiple = true)]
public class AuthorAttribute: Attribute
{
    private string name;

    public AuthorAttribute(string name) {
        this.name = name;
    }

    public string Name {
        get { return name; }
    }
}
```

<span data-ttu-id="9de6f-127">adında çok kullanım özniteliği bir sınıf tanımlar `AuthorAttribute`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-127">defines a multi-use attribute class named `AuthorAttribute`.</span></span> <span data-ttu-id="9de6f-128">Örnek</span><span class="sxs-lookup"><span data-stu-id="9de6f-128">The example</span></span>

```csharp
[Author("Brian Kernighan"), Author("Dennis Ritchie")] 
class Class1
{
    ...
}
```

<span data-ttu-id="9de6f-129">bir sınıf bildirimiyle birlikte iki kullanımlarını gösterir `Author` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="9de6f-129">shows a class declaration with two uses of the `Author` attribute.</span></span>

<span data-ttu-id="9de6f-130">`AttributeUsage` sahip olarak adlandırılan başka bir adlandırılmış parametre `Inherited`, öznitelik bir temel sınıf belirtildiğinde, o temel sınıftan türetilmiş sınıflar tarafından ayrıca devralınır olup olmadığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-130">`AttributeUsage` has another named parameter called `Inherited`, which indicates whether the attribute, when specified on a base class, is also inherited by classes that derive from that base class.</span></span> <span data-ttu-id="9de6f-131">Varsa `Inherited` sınıfı bir öznitelik için geçerlidir ve ardından bu özniteliği devralınır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-131">If `Inherited` for an attribute class is true, then that attribute is inherited.</span></span> <span data-ttu-id="9de6f-132">Varsa `Inherited` sınıfı için bir öznitelik false sonra bu öznitelik devralınmaz.</span><span class="sxs-lookup"><span data-stu-id="9de6f-132">If `Inherited` for an attribute class is false then that attribute is not inherited.</span></span> <span data-ttu-id="9de6f-133">Belirtilmemişse, kendi varsayılan değer True'dur.</span><span class="sxs-lookup"><span data-stu-id="9de6f-133">If it is unspecified, its default value is true.</span></span>

<span data-ttu-id="9de6f-134">Öznitelik sınıfı `X` olmaması bir `AttributeUsage` özniteliği olarak kendisine bağlı</span><span class="sxs-lookup"><span data-stu-id="9de6f-134">An attribute class `X` not having an `AttributeUsage` attribute attached to it, as in</span></span>

```csharp
using System;

class X: Attribute {...}
```

<span data-ttu-id="9de6f-135">Aşağıdakine eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="9de6f-135">is equivalent to the following:</span></span>

```csharp
using System;

[AttributeUsage(
    AttributeTargets.All,
    AllowMultiple = false,
    Inherited = true)
]
class X: Attribute {...}
```

### <a name="positional-and-named-parameters"></a><span data-ttu-id="9de6f-136">Konumsal ve adlandırılmış parametreleri</span><span class="sxs-lookup"><span data-stu-id="9de6f-136">Positional and named parameters</span></span>

<span data-ttu-id="9de6f-137">Öznitelik sınıfları olabilir ***konumsal parametreler*** ve ***adlandırılmış parametreleri***.</span><span class="sxs-lookup"><span data-stu-id="9de6f-137">Attribute classes can have ***positional parameters*** and ***named parameters***.</span></span> <span data-ttu-id="9de6f-138">Her ortak örnek oluşturucusu bir öznitelik sınıfı için konumsal parametreleri, öznitelik sınıfı için geçerli bir dizi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="9de6f-138">Each public instance constructor for an attribute class defines a valid sequence of positional parameters for that attribute class.</span></span> <span data-ttu-id="9de6f-139">Öznitelik sınıfı için adlandırılmış bir parametre, her bir statik olmayan ortak okuma-yazma alan ve öznitelik sınıfı özelliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="9de6f-139">Each non-static public read-write field and property for an attribute class defines a named parameter for the attribute class.</span></span>

<span data-ttu-id="9de6f-140">Örnek</span><span class="sxs-lookup"><span data-stu-id="9de6f-140">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpAttribute: Attribute
{
    public HelpAttribute(string url) {        // Positional parameter
        ...
    }

    public string Topic {                     // Named parameter
        get {...}
        set {...}
    }

    public string Url {
        get {...}
    }
}
```

<span data-ttu-id="9de6f-141">adlı bir öznitelik sınıfı tanımlar `HelpAttribute` konumsal bir parametreye sahip `url`ve bir adlandırılmış parametre `Topic`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-141">defines an attribute class named `HelpAttribute` that has one positional parameter, `url`, and one named parameter, `Topic`.</span></span> <span data-ttu-id="9de6f-142">Statik olmayan ve ortak, özelliği, olmasına rağmen `Url` okuma-yazma olmadığından adlandırılmış bir parametre tanımlamıyor.</span><span class="sxs-lookup"><span data-stu-id="9de6f-142">Although it is non-static and public, the property `Url` does not define a named parameter, since it is not read-write.</span></span>

<span data-ttu-id="9de6f-143">Bu öznitelik sınıfı aşağıdaki gibi kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="9de6f-143">This attribute class might be used as follows:</span></span>

```csharp
[Help("http://www.mycompany.com/.../Class1.htm")]
class Class1
{
    ...
}

[Help("http://www.mycompany.com/.../Misc.htm", Topic = "Class2")]
class Class2
{
    ...
}
```

### <a name="attribute-parameter-types"></a><span data-ttu-id="9de6f-144">Öznitelik parametre türleri</span><span class="sxs-lookup"><span data-stu-id="9de6f-144">Attribute parameter types</span></span>

<span data-ttu-id="9de6f-145">Öznitelik sınıfı için konumsal ve adlandırılmış parametrelerden tür sınırlı ***öznitelik parametre türleri***, hangi:</span><span class="sxs-lookup"><span data-stu-id="9de6f-145">The types of positional and named parameters for an attribute class are limited to the ***attribute parameter types***, which are:</span></span>

*  <span data-ttu-id="9de6f-146">Aşağıdaki türlerden biri: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-146">One of the following types: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.</span></span>
*  <span data-ttu-id="9de6f-147">Türü `object`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-147">The type `object`.</span></span>
*  <span data-ttu-id="9de6f-148">Türü `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-148">The type `System.Type`.</span></span>
*  <span data-ttu-id="9de6f-149">Bir sabit listesi türünde sağlanan ortak erişilebilirlik vardır ve ortak erişilebilirlik, onu iç içe (varsa) türler de ([öznitelik belirtimi](attributes.md#attribute-specification)).</span><span class="sxs-lookup"><span data-stu-id="9de6f-149">An enum type, provided it has public accessibility and the types in which it is nested (if any) also have public accessibility ([Attribute specification](attributes.md#attribute-specification)).</span></span>
*  <span data-ttu-id="9de6f-150">Tek boyutlu diziler yukarıdaki türleri.</span><span class="sxs-lookup"><span data-stu-id="9de6f-150">Single-dimensional arrays of the above types.</span></span>
*  <span data-ttu-id="9de6f-151">Bir öznitelik belirtiminde konumsal veya adlandırılmış bir parametre olarak bir oluşturucu bağımsız değişkeni ya da bu türlerden birinde olmayan ortak alan kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="9de6f-151">A constructor argument or public field which does not have one of these types, cannot be used as a positional or named parameter in an attribute specification.</span></span>

## <a name="attribute-specification"></a><span data-ttu-id="9de6f-152">Öznitelik belirtimi</span><span class="sxs-lookup"><span data-stu-id="9de6f-152">Attribute specification</span></span>

<span data-ttu-id="9de6f-153">***Öznitelik belirtimi*** önceden tanımlanmış bir öznitelik için bir bildirim uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-153">***Attribute specification*** is the application of a previously defined attribute to a declaration.</span></span> <span data-ttu-id="9de6f-154">Bir öznitelik bildirimi belirtilen ek bildirim temelli bilgi parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-154">An attribute is a piece of additional declarative information that is specified for a declaration.</span></span> <span data-ttu-id="9de6f-155">Öznitelikler (içeren derleme veya modül öznitelikleri belirtmek için), genel kapsamda belirtilebilir ve *type_declaration*s ([tür bildirimleri](namespaces.md#type-declarations)), *class_member_declaration* s ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)), *interface_member_declaration*s ([arabirim üyeleri](interfaces.md#interface-members)), *struct_member _declaration*s ([Yapı üyeleri](structs.md#struct-members)), *enum_member_declaration*s ([numaralandırma üyeleri](enums.md#enum-members)), *accessor_declarations*  ([Erişimcileri](classes.md#accessors)), *event_accessor_declarations* ([alan benzeri olaylara](classes.md#field-like-events)), ve *formal_parameter_list*s ([yöntem parametreleri](classes.md#method-parameters)).</span><span class="sxs-lookup"><span data-stu-id="9de6f-155">Attributes can be specified at global scope (to specify attributes on the containing assembly or module) and for *type_declaration*s ([Type declarations](namespaces.md#type-declarations)), *class_member_declaration*s ([Type parameter constraints](classes.md#type-parameter-constraints)), *interface_member_declaration*s ([Interface members](interfaces.md#interface-members)), *struct_member_declaration*s ([Struct members](structs.md#struct-members)), *enum_member_declaration*s ([Enum members](enums.md#enum-members)), *accessor_declarations* ([Accessors](classes.md#accessors)), *event_accessor_declarations* ([Field-like events](classes.md#field-like-events)), and *formal_parameter_list*s ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="9de6f-156">Öznitelikleri belirtilir ***bölümlerde öznitelik***.</span><span class="sxs-lookup"><span data-stu-id="9de6f-156">Attributes are specified in ***attribute sections***.</span></span> <span data-ttu-id="9de6f-157">Bir öznitelik bölümü bir veya daha fazla öznitelik virgülle ayrılmış bir listesini çevreleyen köşeli ayraçların bir çiftinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="9de6f-157">An attribute section consists of a pair of square brackets, which surround a comma-separated list of one or more attributes.</span></span> <span data-ttu-id="9de6f-158">Böyle bir listede öznitelikleri belirtildiğinde ve bölümler aynı program varlığa ekli sırasını düzenlenir sırası önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-158">The order in which attributes are specified in such a list, and the order in which sections attached to the same program entity are arranged, is not significant.</span></span> <span data-ttu-id="9de6f-159">Örneğin, öznitelik özelliklerini `[A][B]`, `[B][A]`, `[A,B]`, ve `[B,A]` eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-159">For instance, the attribute specifications `[A][B]`, `[B][A]`, `[A,B]`, and `[B,A]` are equivalent.</span></span>

```antlr
global_attributes
    : global_attribute_section+
    ;

global_attribute_section
    : '[' global_attribute_target_specifier attribute_list ']'
    | '[' global_attribute_target_specifier attribute_list ',' ']'
    ;

global_attribute_target_specifier
    : global_attribute_target ':'
    ;

global_attribute_target
    : 'assembly'
    | 'module'
    ;

attributes
    : attribute_section+
    ;

attribute_section
    : '[' attribute_target_specifier? attribute_list ']'
    | '[' attribute_target_specifier? attribute_list ',' ']'
    ;

attribute_target_specifier
    : attribute_target ':'
    ;

attribute_target
    : 'field'
    | 'event'
    | 'method'
    | 'param'
    | 'property'
    | 'return'
    | 'type'
    ;

attribute_list
    : attribute (',' attribute)*
    ;

attribute
    : attribute_name attribute_arguments?
    ;

attribute_name
    : type_name
    ;

attribute_arguments
    : '(' positional_argument_list? ')'
    | '(' positional_argument_list ',' named_argument_list ')'
    | '(' named_argument_list ')'
    ;

positional_argument_list
    : positional_argument (',' positional_argument)*
    ;

positional_argument
    : attribute_argument_expression
    ;

named_argument_list
    : named_argument (','  named_argument)*
    ;

named_argument
    : identifier '=' attribute_argument_expression
    ;

attribute_argument_expression
    : expression
    ;
```

<span data-ttu-id="9de6f-160">Bir öznitelik oluşan bir *attrıbute_name* ve konumsal ve adlandırılmış bağımsız değişken isteğe bağlı bir liste.</span><span class="sxs-lookup"><span data-stu-id="9de6f-160">An attribute consists of an *attribute_name* and an optional list of positional and named arguments.</span></span> <span data-ttu-id="9de6f-161">Adlandırılmış bağımsız değişkenler konumsal bağımsız değişkenler (varsa) koyun.</span><span class="sxs-lookup"><span data-stu-id="9de6f-161">The positional arguments (if any) precede the named arguments.</span></span> <span data-ttu-id="9de6f-162">Konumsal bağımsız değişken içeren bir *attribute_argument_expression*; ardından bir eşittir işareti tarafından izlenen bir ad, adlandırılmış bir bağımsız değişken oluşan bir *attribute_argument_expression*, birlikte , basit atama gibi aynı kurallara göre kısıtlanır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-162">A positional argument consists of an *attribute_argument_expression*; a named argument consists of a name, followed by an equal sign, followed by an *attribute_argument_expression*, which, together, are constrained by the same rules as simple assignment.</span></span> <span data-ttu-id="9de6f-163">Adlandırılmış bağımsız değişkenlerin sırası önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-163">The order of named arguments is not significant.</span></span>

<span data-ttu-id="9de6f-164">*Attrıbute_name* öznitelik sınıfı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="9de6f-164">The *attribute_name* identifies an attribute class.</span></span> <span data-ttu-id="9de6f-165">Varsa biçiminde *attrıbute_name* olduğu *type_name* sonra bu adı için bir öznitelik sınıfı başvurmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-165">If the form of *attribute_name* is *type_name* then this name must refer to an attribute class.</span></span> <span data-ttu-id="9de6f-166">Aksi takdirde, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="9de6f-166">Otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="9de6f-167">Örnek</span><span class="sxs-lookup"><span data-stu-id="9de6f-167">The example</span></span>

```csharp
class Class1 {}

[Class1] class Class2 {}    // Error
```

<span data-ttu-id="9de6f-168">sonuçları bir derleme zamanı hatası kullanmaya çalışır çünkü `Class1` öznitelik olarak sınıf `Class1` öznitelik sınıfı değil.</span><span class="sxs-lookup"><span data-stu-id="9de6f-168">results in a compile-time error because it attempts to use `Class1` as an attribute class when `Class1` is not an attribute class.</span></span>

<span data-ttu-id="9de6f-169">Belirli bağlamlarda birden fazla hedef üzerinde bir öznitelik belirtimi izin verir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-169">Certain contexts permit the specification of an attribute on more than one target.</span></span> <span data-ttu-id="9de6f-170">Bir program açıkça hedef dahil ederek belirtebilirsiniz bir *attribute_target_specifier*.</span><span class="sxs-lookup"><span data-stu-id="9de6f-170">A program can explicitly specify the target by including an *attribute_target_specifier*.</span></span> <span data-ttu-id="9de6f-171">Bir öznitelik genel düzeyde yerleştirildiğinde bir *global_attribute_target_specifier* gereklidir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-171">When an attribute is placed at the global level, a *global_attribute_target_specifier* is required.</span></span> <span data-ttu-id="9de6f-172">Diğer tüm konumlarda makul bir varsayılan uygulanır, ancak bir *attribute_target_specifier* onaylayabilir veya belirsiz belirli durumlarda varsayılan geçersiz kılma (veya varsayılan Belirsiz olmayan bir durumda yalnızca onaylayabilir) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-172">In all other locations, a reasonable default is applied, but an *attribute_target_specifier* can be used to affirm or override the default in certain ambiguous cases (or to just affirm the default in non-ambiguous cases).</span></span> <span data-ttu-id="9de6f-173">Bu nedenle, genellikle *attribute_target_specifier*s atlanabilir dışında genel düzeyde.</span><span class="sxs-lookup"><span data-stu-id="9de6f-173">Thus, typically, *attribute_target_specifier*s can be omitted except at the global level.</span></span> <span data-ttu-id="9de6f-174">Belirsiz olası bağlamları gibi çözümlenir:</span><span class="sxs-lookup"><span data-stu-id="9de6f-174">The potentially ambiguous contexts are resolved as follows:</span></span>

*  <span data-ttu-id="9de6f-175">Genel kapsamda belirtilen bir özniteliği hedef derleme veya hedef modülü uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9de6f-175">An attribute specified at global scope can apply either to the target assembly or the target module.</span></span> <span data-ttu-id="9de6f-176">Varsayılan bu nedenle bu bağlam için mevcut bir *attribute_target_specifier* bu bağlamda her zaman gereklidir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-176">No default exists for this context, so an *attribute_target_specifier* is always required in this context.</span></span> <span data-ttu-id="9de6f-177">Varlığını `assembly` *attribute_target_specifier* öznitelik hedefi için geçerli olduğunu gösterir derleme; varlığını `module` *attribute_target_specifier* öznitelik hedef modülü için geçerli olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-177">The presence of the `assembly` *attribute_target_specifier* indicates that the attribute applies to the target assembly; the presence of the `module` *attribute_target_specifier* indicates that the attribute applies to the target module.</span></span>
*  <span data-ttu-id="9de6f-178">Delegate bildiriminde belirtilen öznitelik bildirilen temsilci veya dönüş değeri uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9de6f-178">An attribute specified on a delegate declaration can apply either to the delegate being declared or to its return value.</span></span> <span data-ttu-id="9de6f-179">Mevcut olmadığında bir *attribute_target_specifier*, temsilciye özniteliğini uygular.</span><span class="sxs-lookup"><span data-stu-id="9de6f-179">In the absence of an *attribute_target_specifier*, the attribute applies to the delegate.</span></span> <span data-ttu-id="9de6f-180">Varlığını `type` *attribute_target_specifier* öznitelik temsilciye; geçerli olduğunu gösterir varlığını `return` *attribute_target_specifier* öznitelik dönüş değerini geçerli olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-180">The presence of the `type` *attribute_target_specifier* indicates that the attribute applies to the delegate; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="9de6f-181">Özniteliğin yöntem bildiriminde belirtilen bildirilen yöntemi veya dönüş değeri uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9de6f-181">An attribute specified on a method declaration can apply either to the method being declared or to its return value.</span></span> <span data-ttu-id="9de6f-182">Mevcut olmadığında bir *attribute_target_specifier*, yönteme özniteliğini uygular.</span><span class="sxs-lookup"><span data-stu-id="9de6f-182">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="9de6f-183">Varlığını `method` *attribute_target_specifier* öznitelik yönteme; geçerli olduğunu gösterir varlığını `return` *attribute_target_specifier* gösterir öznitelik dönüş değerini olmasını uygular.</span><span class="sxs-lookup"><span data-stu-id="9de6f-183">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="9de6f-184">Bir operatörü bildiriminde belirtilen öznitelik bildirilen işleci veya dönüş değerine uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9de6f-184">An attribute specified on an operator declaration can apply either to the operator being declared or to its return value.</span></span> <span data-ttu-id="9de6f-185">Mevcut olmadığında bir *attribute_target_specifier*, öznitelik işlecini uygular.</span><span class="sxs-lookup"><span data-stu-id="9de6f-185">In the absence of an *attribute_target_specifier*, the attribute applies to the operator.</span></span> <span data-ttu-id="9de6f-186">Varlığını `method` *attribute_target_specifier* öznitelik işleci; geçerli olduğunu gösterir varlığını `return` *attribute_target_specifier* öznitelik dönüş değerini geçerli olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-186">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the operator; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="9de6f-187">Olay erişimcileri atlar olay bildiriminde belirtilen bir öznitelik, bildirilen olay, (olay soyut değilse) ilişkilendirilmiş alan veya ilişkili Ekle uygulamak ve yöntemleri kaldırın.</span><span class="sxs-lookup"><span data-stu-id="9de6f-187">An attribute specified on an event declaration that omits event accessors can apply to the event being declared, to the associated field (if the event is not abstract), or to the associated add and remove methods.</span></span> <span data-ttu-id="9de6f-188">Mevcut olmadığında bir *attribute_target_specifier*, özniteliği olaya uygulanır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-188">In the absence of an *attribute_target_specifier*, the attribute applies to the event.</span></span> <span data-ttu-id="9de6f-189">Varlığını `event` *attribute_target_specifier* öznitelik olayı için geçerli olduğunu gösterir varlığını `field` *attribute_target_specifier* gösterir öznitelik alanı uyguladığı; ve varlığından destek alan `method` *attribute_target_specifier* öznitelik yöntemleri için geçerli olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-189">The presence of the `event` *attribute_target_specifier* indicates that the attribute applies to the event; the presence of the `field` *attribute_target_specifier* indicates that the attribute applies to the field; and the presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the methods.</span></span>
*  <span data-ttu-id="9de6f-190">Get erişimcisi bildiriminde bir özellik veya dizin oluşturucu bildirimi için belirtilen bir öznitelik veya ilişkili yöntem dönüş değerine uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9de6f-190">An attribute specified on a get accessor declaration for a property or indexer declaration can apply either to the associated method or to its return value.</span></span> <span data-ttu-id="9de6f-191">Mevcut olmadığında bir *attribute_target_specifier*, yönteme özniteliğini uygular.</span><span class="sxs-lookup"><span data-stu-id="9de6f-191">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="9de6f-192">Varlığını `method` *attribute_target_specifier* öznitelik yönteme; geçerli olduğunu gösterir varlığını `return` *attribute_target_specifier* gösterir öznitelik dönüş değerini olmasını uygular.</span><span class="sxs-lookup"><span data-stu-id="9de6f-192">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="9de6f-193">İlişkili yöntem veya silmenizin, örtük parametresinin ayarlama erişimcisine bir özellik veya dizin oluşturucu bildirimi için belirtilen bir öznitelik uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9de6f-193">An attribute specified on a set accessor for a property or indexer declaration can apply either to the associated method or to its lone implicit parameter.</span></span> <span data-ttu-id="9de6f-194">Mevcut olmadığında bir *attribute_target_specifier*, yönteme özniteliğini uygular.</span><span class="sxs-lookup"><span data-stu-id="9de6f-194">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="9de6f-195">Varlığını `method` *attribute_target_specifier* öznitelik yönteme; geçerli olduğunu gösterir varlığını `param` *attribute_target_specifier* gösterir öznitelik parametre uyguladığı; varlığını `return` *attribute_target_specifier* öznitelik dönüş değerini geçerli olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-195">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `param` *attribute_target_specifier* indicates that the attribute applies to the parameter; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="9de6f-196">İlişkili yöntem veya silmenizin, parametresinin bir olay bildirimi uygulayabilirsiniz için bir ekleme veya kaldırma erişimcisi bildiriminde belirtilen bir öznitelik.</span><span class="sxs-lookup"><span data-stu-id="9de6f-196">An attribute specified on an add or remove accessor declaration for an event declaration can apply either to the associated method or to its lone parameter.</span></span> <span data-ttu-id="9de6f-197">Mevcut olmadığında bir *attribute_target_specifier*, yönteme özniteliğini uygular.</span><span class="sxs-lookup"><span data-stu-id="9de6f-197">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="9de6f-198">Varlığını `method` *attribute_target_specifier* öznitelik yönteme; geçerli olduğunu gösterir varlığını `param` *attribute_target_specifier* gösterir öznitelik parametre uyguladığı; varlığını `return` *attribute_target_specifier* öznitelik dönüş değerini geçerli olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-198">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `param` *attribute_target_specifier* indicates that the attribute applies to the parameter; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>

<span data-ttu-id="9de6f-199">Diğer bağlamlarda eklenmesi, bir *attribute_target_specifier* izin verilen ancak gereksiz.</span><span class="sxs-lookup"><span data-stu-id="9de6f-199">In other contexts, inclusion of an *attribute_target_specifier* is permitted but unnecessary.</span></span> <span data-ttu-id="9de6f-200">Örneğin, bir sınıf bildirimi dahil etmek veya belirticisini atlayan `type`:</span><span class="sxs-lookup"><span data-stu-id="9de6f-200">For instance, a class declaration may either include or omit the specifier `type`:</span></span>

```csharp
[type: Author("Brian Kernighan")]
class Class1 {}

[Author("Dennis Ritchie")]
class Class2 {}
```

<span data-ttu-id="9de6f-201">Geçersiz bir belirtmek için bir hata olduğunu *attribute_target_specifier*.</span><span class="sxs-lookup"><span data-stu-id="9de6f-201">It is an error to specify an invalid *attribute_target_specifier*.</span></span> <span data-ttu-id="9de6f-202">Örneğin, belirleyici `param` bir sınıf bildiriminde kullanılamaz:</span><span class="sxs-lookup"><span data-stu-id="9de6f-202">For instance, the specifier `param` cannot be used on a class declaration:</span></span>

```csharp
[param: Author("Brian Kernighan")]        // Error
class Class1 {}
```

<span data-ttu-id="9de6f-203">Kural gereği, öznitelik sınıfları bir sonekiyle adlandırılır `Attribute`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-203">By convention, attribute classes are named with a suffix of `Attribute`.</span></span> <span data-ttu-id="9de6f-204">Bir *attrıbute_name* formun *type_name* , bu ekini ya da içerir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-204">An *attribute_name* of the form *type_name* may either include or omit this suffix.</span></span> <span data-ttu-id="9de6f-205">Öznitelik sınıfı ile ve bu son ek olmadan bulunursa, bir belirsizlik var ve derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="9de6f-205">If an attribute class is found both with and without this suffix, an ambiguity is present, and a compile-time error results.</span></span> <span data-ttu-id="9de6f-206">Varsa *attrıbute_name* yazıldığından emin olacak şekilde, en sağdaki *tanımlayıcısı* verbatim bir tanımlayıcıdır ([tanımlayıcıları](lexical-structure.md#identifiers)), yalnızca bir öznitelik bir soneki olmadan eşleşen sonra Bu nedenle böyle bir belirsizlik çözülmesi etkinleştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="9de6f-206">If the *attribute_name* is spelled such that its right-most *identifier* is a verbatim identifier ([Identifiers](lexical-structure.md#identifiers)), then only an attribute without a suffix is matched, thus enabling such an ambiguity to be resolved.</span></span> <span data-ttu-id="9de6f-207">Örnek</span><span class="sxs-lookup"><span data-stu-id="9de6f-207">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class X: Attribute
{}

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Error: ambiguity
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Refers to X
class Class3 {}

[@XAttribute]           // Refers to XAttribute
class Class4 {}
```

<span data-ttu-id="9de6f-208">iki öznitelik sınıflar adlandırılan gösterir `X` ve `XAttribute`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-208">shows two attribute classes named `X` and `XAttribute`.</span></span> <span data-ttu-id="9de6f-209">Öznitelik `[X]` ya da başvurabileceğiniz beri belirsiz `X` veya `XAttribute`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-209">The attribute `[X]` is ambiguous, since it could refer to either `X` or `XAttribute`.</span></span> <span data-ttu-id="9de6f-210">Tam tanımlayıcı kullanarak tam hedefi gibi nadir durumlarda belirtilmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="9de6f-210">Using a verbatim identifier allows the exact intent to be specified in such rare cases.</span></span> <span data-ttu-id="9de6f-211">Öznitelik `[XAttribute]` belirsiz değil (adlandırılmış bir öznitelik sınıfı olup olmadığını olacak olsa da `XAttributeAttribute`!).</span><span class="sxs-lookup"><span data-stu-id="9de6f-211">The attribute `[XAttribute]` is not ambiguous (although it would be if there was an attribute class named `XAttributeAttribute`!).</span></span> <span data-ttu-id="9de6f-212">Sınıf bildirimi `X` kaldırılır, ardından her iki öznitelik adlı öznitelik sınıfına başvurmak `XAttribute`gibi:</span><span class="sxs-lookup"><span data-stu-id="9de6f-212">If the declaration for class `X` is removed, then both attributes refer to the attribute class named `XAttribute`, as follows:</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Refers to XAttribute
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Error: no attribute named "X"
class Class3 {}
```

<span data-ttu-id="9de6f-213">Bu bir tek kullanımlık öznitelik sınıfı aynı varlığın birden çok kez kullanmak için bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-213">It is a compile-time error to use a single-use attribute class more than once on the same entity.</span></span> <span data-ttu-id="9de6f-214">Örnek</span><span class="sxs-lookup"><span data-stu-id="9de6f-214">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpStringAttribute: Attribute
{
    string value;

    public HelpStringAttribute(string value) {
        this.value = value;
    }

    public string Value {
        get {...}
    }
}

[HelpString("Description of Class1")]
[HelpString("Another description of Class1")]
public class Class1 {}
```

<span data-ttu-id="9de6f-215">sonuçları bir derleme zamanı hatası kullanmaya çalışır çünkü `HelpString`, olduğu bir tek kullanımlık öznitelik sınıfı birden çok kez beyanı `Class1`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-215">results in a compile-time error because it attempts to use `HelpString`, which is a single-use attribute class, more than once on the declaration of `Class1`.</span></span>

<span data-ttu-id="9de6f-216">Bir ifade `E` olduğu bir *attribute_argument_expression* tümü aşağıdaki deyimleri doğruysa:</span><span class="sxs-lookup"><span data-stu-id="9de6f-216">An expression `E` is an *attribute_argument_expression* if all of the following statements are true:</span></span>

*  <span data-ttu-id="9de6f-217">Türünü `E` bir öznitelik parametresi türü ([öznitelik parametre türleri](attributes.md#attribute-parameter-types)).</span><span class="sxs-lookup"><span data-stu-id="9de6f-217">The type of `E` is an attribute parameter type ([Attribute parameter types](attributes.md#attribute-parameter-types)).</span></span>
*  <span data-ttu-id="9de6f-218">Derleme zamanında, değerini `E` aşağıdakilerden birini çözülebilir:</span><span class="sxs-lookup"><span data-stu-id="9de6f-218">At compile-time, the value of `E` can be resolved to one of the following:</span></span>
   * <span data-ttu-id="9de6f-219">Sabit bir değer.</span><span class="sxs-lookup"><span data-stu-id="9de6f-219">A constant value.</span></span>
   * <span data-ttu-id="9de6f-220">A `System.Type` nesne.</span><span class="sxs-lookup"><span data-stu-id="9de6f-220">A `System.Type` object.</span></span>
   * <span data-ttu-id="9de6f-221">Tek boyutlu bir dizi *attribute_argument_expression*s.</span><span class="sxs-lookup"><span data-stu-id="9de6f-221">A one-dimensional array of *attribute_argument_expression*s.</span></span>

<span data-ttu-id="9de6f-222">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="9de6f-222">For example:</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class TestAttribute: Attribute
{
    public int P1 {
        get {...}
        set {...}
    }

    public Type P2 {
        get {...}
        set {...}
    }

    public object P3 {
        get {...}
        set {...}
    }
}

[Test(P1 = 1234, P3 = new int[] {1, 3, 5}, P2 = typeof(float))]
class MyClass {}
```

<span data-ttu-id="9de6f-223">A *typeof_expression* ([typeof işleci](expressions.md#the-typeof-operator)) bir öznitelik bağımsız değişken ifadesi genel olmayan bir tür, kapalı bir oluşturulmuş tür ya da bağlanmamış bir genel tür başvurabilir, ancak başvuruda bulunulamaz olarak kullanılan bir açık tür.</span><span class="sxs-lookup"><span data-stu-id="9de6f-223">A *typeof_expression* ([The typeof operator](expressions.md#the-typeof-operator)) used as an attribute argument expression can reference a non-generic type, a closed constructed type, or an unbound generic type, but it cannot reference an open type.</span></span> <span data-ttu-id="9de6f-224">Bu ifadenin derleme zamanında çözülebilir sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-224">This is to ensure that the expression can be resolved at compile-time.</span></span>

```csharp
class A: Attribute
{
    public A(Type t) {...}
}

class G<T>
{
    [A(typeof(T))] T t;                  // Error, open type in attribute
}

class X
{
    [A(typeof(List<int>))] int x;        // Ok, closed constructed type
    [A(typeof(List<>))] int y;           // Ok, unbound generic type
}
```

## <a name="attribute-instances"></a><span data-ttu-id="9de6f-225">Öznitelik örnekleri</span><span class="sxs-lookup"><span data-stu-id="9de6f-225">Attribute instances</span></span>

<span data-ttu-id="9de6f-226">Bir ***öznitelik örneği*** çalışma zamanında bir özniteliği temsil eder bir örneğidir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-226">An ***attribute instance*** is an instance that represents an attribute at run-time.</span></span> <span data-ttu-id="9de6f-227">Bir öznitelik konumsal bağımsız değişkenler, bir öznitelik sınıfı ile tanımlanır ve adlandırılmış bağımsız değişkenler.</span><span class="sxs-lookup"><span data-stu-id="9de6f-227">An attribute is defined with an attribute class, positional arguments, and named arguments.</span></span> <span data-ttu-id="9de6f-228">Bir öznitelik örneği konumsal ve adlandırılmış bağımsız değişkenler ile başlatılan sınıfının bir örneğidir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-228">An attribute instance is an instance of the attribute class that is initialized with the positional and named arguments.</span></span>

<span data-ttu-id="9de6f-229">Bir öznitelik örneği alınmasını, aşağıdaki bölümlerde açıklandığı gibi hem derleme zamanı ve çalışma zamanı işleme içerir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-229">Retrieval of an attribute instance involves both compile-time and run-time processing, as described in the following sections.</span></span>

### <a name="compilation-of-an-attribute"></a><span data-ttu-id="9de6f-230">Bir özniteliğin derleme</span><span class="sxs-lookup"><span data-stu-id="9de6f-230">Compilation of an attribute</span></span>

<span data-ttu-id="9de6f-231">Derlemesi bir *özniteliği* öznitelik sınıfı ile `T`, *positional_argument_list* `P` ve *named_argument_list* `N`, aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="9de6f-231">The compilation of an *attribute* with attribute class `T`, *positional_argument_list* `P` and *named_argument_list* `N`, consists of the following steps:</span></span>

*  <span data-ttu-id="9de6f-232">Derleme için derleme zamanı işleme adımları bir *object_creation_expression* formun `new T(P)`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-232">Follow the compile-time processing steps for compiling an *object_creation_expression* of the form `new T(P)`.</span></span> <span data-ttu-id="9de6f-233">Bu adımları neden bir derleme zamanı hatası ya da bir örnek oluşturucusunda belirlemek `C` üzerinde `T` , çağrılacak çalışma zamanında.</span><span class="sxs-lookup"><span data-stu-id="9de6f-233">These steps either result in a compile-time error, or determine an instance constructor `C` on `T` that can be invoked at run-time.</span></span>
*  <span data-ttu-id="9de6f-234">Varsa `C` bir derleme zamanı hatası oluşursa, ortak erişilebilirlik yok.</span><span class="sxs-lookup"><span data-stu-id="9de6f-234">If `C` does not have public accessibility, then a compile-time error occurs.</span></span>
*  <span data-ttu-id="9de6f-235">Her *named_argument* `Arg` içinde `N`:</span><span class="sxs-lookup"><span data-stu-id="9de6f-235">For each *named_argument* `Arg` in `N`:</span></span>
   * <span data-ttu-id="9de6f-236">İzin `Name` olması *tanımlayıcı* , *named_argument* `Arg`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-236">Let `Name` be the *identifier* of the *named_argument* `Arg`.</span></span>
   * <span data-ttu-id="9de6f-237">`Name` Statik olmayan okuma-yazma ortak alan veya özellik üzerinde tanımlamalıdır `T`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-237">`Name` must identify a non-static read-write public field or property on `T`.</span></span> <span data-ttu-id="9de6f-238">Varsa `T` böyle bir alan veya özellik, bir derleme zamanı hatası oluşursa sahiptir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-238">If `T` has no such field or property, then a compile-time error occurs.</span></span>
*  <span data-ttu-id="9de6f-239">Özniteliğin çalışma zamanı örneklemesi için aşağıdaki bilgileri tutun: öznitelik sınıfı `T`, örnek oluşturucusu `C` üzerinde `T`, *positional_argument_list* `P` ve *named_argument_list* `N`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-239">Keep the following information for run-time instantiation of the attribute: the attribute class `T`, the instance constructor `C` on `T`, the *positional_argument_list* `P` and the *named_argument_list* `N`.</span></span>

### <a name="run-time-retrieval-of-an-attribute-instance"></a><span data-ttu-id="9de6f-240">Çalışma zamanı alınmasını öznitelik örneği</span><span class="sxs-lookup"><span data-stu-id="9de6f-240">Run-time retrieval of an attribute instance</span></span>

<span data-ttu-id="9de6f-241">Derleme, bir *özniteliği* öznitelik sınıfı verir `T`, örnek oluşturucusu `C` üzerinde `T`, *positional_argument_list* `P`ve *named_argument_list* `N`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-241">Compilation of an *attribute* yields an attribute class `T`, an instance constructor `C` on `T`, a *positional_argument_list* `P`, and a *named_argument_list* `N`.</span></span> <span data-ttu-id="9de6f-242">Bu bilgiler verildiğinde, bir öznitelik örneği aşağıdaki adımları kullanarak zamanında alınabilir:</span><span class="sxs-lookup"><span data-stu-id="9de6f-242">Given this information, an attribute instance can be retrieved at run-time using the following steps:</span></span>

*  <span data-ttu-id="9de6f-243">Yürütme için çalışma zamanı işleme adımları bir *object_creation_expression* formun `new T(P)`, örnek Oluşturucusu kullanarak `C` derleme zamanında belirlendiği.</span><span class="sxs-lookup"><span data-stu-id="9de6f-243">Follow the run-time processing steps for executing an *object_creation_expression* of the form `new T(P)`, using the instance constructor `C` as determined at compile-time.</span></span> <span data-ttu-id="9de6f-244">Bu adımları bir özel durum neden ya da bir örnek `O` , `T`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-244">These steps either result in an exception, or produce an instance `O` of `T`.</span></span>
*  <span data-ttu-id="9de6f-245">Her *named_argument* `Arg` içinde `N`, sırayla:</span><span class="sxs-lookup"><span data-stu-id="9de6f-245">For each *named_argument* `Arg` in `N`, in order:</span></span>
   * <span data-ttu-id="9de6f-246">İzin `Name` olması *tanımlayıcı* , *named_argument* `Arg`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-246">Let `Name` be the *identifier* of the *named_argument* `Arg`.</span></span> <span data-ttu-id="9de6f-247">Varsa `Name` bir statik olmayan, genel okuma-yazma alan veya özellik üzerinde tanımlamaz `O`, sonra da bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9de6f-247">If `Name` does not identify a non-static public read-write field or property on `O`, then an exception is thrown.</span></span>
   * <span data-ttu-id="9de6f-248">İzin `Value` değerlendirme sonucu *attribute_argument_expression* , `Arg`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-248">Let `Value` be the result of evaluating the *attribute_argument_expression* of `Arg`.</span></span>
   * <span data-ttu-id="9de6f-249">Varsa `Name` üzerinde bir alan tanımlar `O`, ardından bu alan kümesine `Value`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-249">If `Name` identifies a field on `O`, then set this field to `Value`.</span></span>
   * <span data-ttu-id="9de6f-250">Aksi takdirde, `Name` üzerindeki bir özelliğini tanımlar `O`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-250">Otherwise, `Name` identifies a property on `O`.</span></span> <span data-ttu-id="9de6f-251">Bu özellik kümesine `Value`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-251">Set this property to `Value`.</span></span>
   * <span data-ttu-id="9de6f-252">Sonuç `O`, öznitelik sınıfının bir örneğini `T` ile başlatıldıysa *positional_argument_list* `P` ve *named_argument_list* `N`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-252">The result is `O`, an instance of the attribute class `T` that has been initialized with the *positional_argument_list* `P` and the *named_argument_list* `N`.</span></span>

## <a name="reserved-attributes"></a><span data-ttu-id="9de6f-253">Ayrılmış öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="9de6f-253">Reserved attributes</span></span>

<span data-ttu-id="9de6f-254">Öznitelikleri az sayıda dil şekilde etkiler.</span><span class="sxs-lookup"><span data-stu-id="9de6f-254">A small number of attributes affect the language in some way.</span></span> <span data-ttu-id="9de6f-255">Bu öznitelikler içerir:</span><span class="sxs-lookup"><span data-stu-id="9de6f-255">These attributes include:</span></span>

*  <span data-ttu-id="9de6f-256">`System.AttributeUsageAttribute` ([AttributeUsage özniteliği](attributes.md#the-attributeusage-attribute)), öznitelik sınıfı kullanılabilecek yöntemleri tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-256">`System.AttributeUsageAttribute` ([The AttributeUsage attribute](attributes.md#the-attributeusage-attribute)), which is used to describe the ways in which an attribute class can be used.</span></span>
*  <span data-ttu-id="9de6f-257">`System.Diagnostics.ConditionalAttribute` ([Conditional özniteliği](attributes.md#the-conditional-attribute)), koşullu yöntemler tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-257">`System.Diagnostics.ConditionalAttribute` ([The Conditional attribute](attributes.md#the-conditional-attribute)), which is used to define conditional methods.</span></span>
*  <span data-ttu-id="9de6f-258">`System.ObsoleteAttribute` ([Geçersiz öznitelik](attributes.md#the-obsolete-attribute)), üye artık kullanılmıyor olarak işaretlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-258">`System.ObsoleteAttribute` ([The Obsolete attribute](attributes.md#the-obsolete-attribute)), which is used to mark a member as obsolete.</span></span>
*  <span data-ttu-id="9de6f-259">`System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` ve `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([arayan bilgileri öznitelikleri](attributes.md#caller-info-attributes)), isteğe bağlı parametreler için arama bağlamı hakkında bilgi sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-259">`System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` and `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([Caller info attributes](attributes.md#caller-info-attributes)), which are used to supply information about the calling context to optional parameters.</span></span>

### <a name="the-attributeusage-attribute"></a><span data-ttu-id="9de6f-260">AttributeUsage özniteliği</span><span class="sxs-lookup"><span data-stu-id="9de6f-260">The AttributeUsage attribute</span></span>

<span data-ttu-id="9de6f-261">Öznitelik `AttributeUsage` öznitelik sınıfı kullanılabilecek şekilde tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-261">The attribute `AttributeUsage` is used to describe the manner in which the attribute class can be used.</span></span>

<span data-ttu-id="9de6f-262">İle donatılmış sınıf `AttributeUsage` özniteliği türetilmesi gereken `System.Attribute`, doğrudan veya dolaylı olarak.</span><span class="sxs-lookup"><span data-stu-id="9de6f-262">A class that is decorated with the `AttributeUsage` attribute must derive from `System.Attribute`, either directly or indirectly.</span></span> <span data-ttu-id="9de6f-263">Aksi takdirde, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="9de6f-263">Otherwise, a compile-time error occurs.</span></span>

```csharp
namespace System
{
    [AttributeUsage(AttributeTargets.Class)]
    public class AttributeUsageAttribute: Attribute
    {
        public AttributeUsageAttribute(AttributeTargets validOn) {...}
        public virtual bool AllowMultiple { get {...} set {...} }
        public virtual bool Inherited { get {...} set {...} }
        public virtual AttributeTargets ValidOn { get {...} }
    }

    public enum AttributeTargets
    {
        Assembly     = 0x0001,
        Module       = 0x0002,
        Class        = 0x0004,
        Struct       = 0x0008,
        Enum         = 0x0010,
        Constructor  = 0x0020,
        Method       = 0x0040,
        Property     = 0x0080,
        Field        = 0x0100,
        Event        = 0x0200,
        Interface    = 0x0400,
        Parameter    = 0x0800,
        Delegate     = 0x1000,
        ReturnValue  = 0x2000,

        All = Assembly | Module | Class | Struct | Enum | Constructor | 
            Method | Property | Field | Event | Interface | Parameter | 
            Delegate | ReturnValue
    }
}
```

### <a name="the-conditional-attribute"></a><span data-ttu-id="9de6f-264">Conditional özniteliği</span><span class="sxs-lookup"><span data-stu-id="9de6f-264">The Conditional attribute</span></span>

<span data-ttu-id="9de6f-265">Öznitelik `Conditional` tanımını sağlar ***koşullu yöntemler*** ve ***koşullu öznitelik sınıfları***.</span><span class="sxs-lookup"><span data-stu-id="9de6f-265">The attribute `Conditional` enables the definition of ***conditional methods*** and ***conditional attribute classes***.</span></span>

```csharp
namespace System.Diagnostics
{
    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Class, AllowMultiple = true)]
    public class ConditionalAttribute: Attribute
    {
        public ConditionalAttribute(string conditionString) {...}
        public string ConditionString { get {...} }
    }
}
```

#### <a name="conditional-methods"></a><span data-ttu-id="9de6f-266">Koşullu yöntemler</span><span class="sxs-lookup"><span data-stu-id="9de6f-266">Conditional methods</span></span>

<span data-ttu-id="9de6f-267">Bir yöntem ile donatılmış `Conditional` özniteliği koşullu bir yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-267">A method decorated with the `Conditional` attribute is a conditional method.</span></span> <span data-ttu-id="9de6f-268">`Conditional` Özniteliği, koşullu derleme sembol test ederek bir koşulu belirtir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-268">The `Conditional` attribute indicates a condition by testing a conditional compilation symbol.</span></span> <span data-ttu-id="9de6f-269">Bir koşullu metot çağrıları dahil veya bu sembol çağrısı noktasında tanımlı olup olmadığı bağlı olarak atlanmış.</span><span class="sxs-lookup"><span data-stu-id="9de6f-269">Calls to a conditional method are either included or omitted depending on whether this symbol is defined at the point of the call.</span></span> <span data-ttu-id="9de6f-270">Simge tanımlanmışsa çağrısı bulunur; Aksi takdirde (alıcı değerlendirmesini ve çağrının parametreleri dahil) çağrısı atlandı.</span><span class="sxs-lookup"><span data-stu-id="9de6f-270">If the symbol is defined, the call is included; otherwise, the call (including evaluation of the receiver and parameters of the call) is omitted.</span></span>

<span data-ttu-id="9de6f-271">Aşağıdaki sınırlamalara tabi buna koşullu bir yöntemdir:</span><span class="sxs-lookup"><span data-stu-id="9de6f-271">A conditional method is subject to the following restrictions:</span></span>

*  <span data-ttu-id="9de6f-272">Koşullu yöntem bir yöntemin olmalıdır bir *class_declaration* veya *struct_declaration*.</span><span class="sxs-lookup"><span data-stu-id="9de6f-272">The conditional method must be a method in a *class_declaration* or *struct_declaration*.</span></span> <span data-ttu-id="9de6f-273">Bir derleme zamanı hatası oluşur `Conditional` öznitelik, bir Metoda bir arabirim bildirimi belirtilir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-273">A compile-time error occurs if the `Conditional` attribute is specified on a method in an interface declaration.</span></span>
*  <span data-ttu-id="9de6f-274">Koşullu yöntem dönüş türüne sahip olmalıdır `void`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-274">The conditional method must have a return type of `void`.</span></span>
*  <span data-ttu-id="9de6f-275">Koşullu yöntemi ile işaretlenmemelidir `override` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="9de6f-275">The conditional method must not be marked with the `override` modifier.</span></span> <span data-ttu-id="9de6f-276">Koşullu bir yöntem ile işaretlenmiş olabilir `virtual` değiştiricisi, ancak.</span><span class="sxs-lookup"><span data-stu-id="9de6f-276">A conditional method may be marked with the `virtual` modifier, however.</span></span> <span data-ttu-id="9de6f-277">Böyle bir yöntemi geçersiz kılmaları örtük olarak koşullu ve açıkça ile işaretlenmemelidir bir `Conditional` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="9de6f-277">Overrides of such a method are implicitly conditional, and must not be explicitly marked with a `Conditional` attribute.</span></span>
*  <span data-ttu-id="9de6f-278">Koşullu yöntem bir arabirim yöntemin bir uygulaması, olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-278">The conditional method must not be an implementation of an interface method.</span></span> <span data-ttu-id="9de6f-279">Aksi takdirde, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="9de6f-279">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="9de6f-280">Ayrıca, koşullu yöntem kullanılırsa derleme zamanı hatası oluşur. bir *delegate_creation_expression*.</span><span class="sxs-lookup"><span data-stu-id="9de6f-280">In addition, a compile-time error occurs if a conditional method is used in a *delegate_creation_expression*.</span></span> <span data-ttu-id="9de6f-281">Örnek</span><span class="sxs-lookup"><span data-stu-id="9de6f-281">The example</span></span>

```csharp
#define DEBUG

using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void M() {
        Console.WriteLine("Executed Class1.M");
    }
}

class Class2
{
    public static void Test() {
        Class1.M();
    }
}
```

<span data-ttu-id="9de6f-282">bildirir `Class1.M` koşullu bir yöntem olarak.</span><span class="sxs-lookup"><span data-stu-id="9de6f-282">declares `Class1.M` as a conditional method.</span></span> <span data-ttu-id="9de6f-283">`Class2`ın `Test` yöntemi bu yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-283">`Class2`'s `Test` method calls this method.</span></span> <span data-ttu-id="9de6f-284">Koşullu derleme sembol beri `DEBUG` durumunda tanımlanır `Class2.Test` olan çağrılır, bunu çağıracak `M`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-284">Since the conditional compilation symbol `DEBUG` is defined, if `Class2.Test` is called, it will call `M`.</span></span> <span data-ttu-id="9de6f-285">Sembol `DEBUG` , ardından tanımlanmış olmayan `Class2.Test` değil çağırırsınız `Class1.M`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-285">If the symbol `DEBUG` had not been defined, then `Class2.Test` would not call `Class1.M`.</span></span>

<span data-ttu-id="9de6f-286">Ekleme veya dışlama koşullu bir yöntem çağrısına çağrısı noktasında koşullu derleme simgeleri tarafından denetlendiğini unutmayın önemlidir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-286">It is important to note that the inclusion or exclusion of a call to a conditional method is controlled by the conditional compilation symbols at the point of the call.</span></span> <span data-ttu-id="9de6f-287">Örnekte</span><span class="sxs-lookup"><span data-stu-id="9de6f-287">In the example</span></span>

<span data-ttu-id="9de6f-288">Dosya `class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="9de6f-288">File `class1.cs`:</span></span>

```csharp
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void F() {
        Console.WriteLine("Executed Class1.F");
    }
}
```

<span data-ttu-id="9de6f-289">Dosya `class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="9de6f-289">File `class2.cs`:</span></span>

```csharp
#define DEBUG

class Class2
{
    public static void G() {
        Class1.F();                // F is called
    }
}
```

<span data-ttu-id="9de6f-290">Dosya `class3.cs`:</span><span class="sxs-lookup"><span data-stu-id="9de6f-290">File `class3.cs`:</span></span>

```csharp
#undef DEBUG

class Class3
{
    public static void H() {
        Class1.F();                // F is not called
    }
}
```

<span data-ttu-id="9de6f-291">sınıfları `Class2` ve `Class3` her içeren koşullu yöntemine yönelik çağrılar `Class1.F`, koşullu olduğu olmamasına tabanlı `DEBUG` tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-291">the classes `Class2` and `Class3` each contain calls to the conditional method `Class1.F`, which is conditional based on whether or not `DEBUG` is defined.</span></span> <span data-ttu-id="9de6f-292">Bu sembol bağlamında tanımlamış `Class2` ama `Class3`, çağrı `F` içinde `Class2` çağrısı sırasında bulunan `F` içinde `Class3` atlanır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-292">Since this symbol is defined in the context of `Class2` but not `Class3`, the call to `F` in `Class2` is included, while the call to `F` in `Class3` is omitted.</span></span>

<span data-ttu-id="9de6f-293">Devralma zincirinde koşullu yöntemler kullanımını kafa karıştırıcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-293">The use of conditional methods in an inheritance chain can be confusing.</span></span> <span data-ttu-id="9de6f-294">Koşullu bir yöntem aracılığıyla yapılan çağrıları `base`, formun `base.M`, normal bir koşullu metot çağrısı kurallarına tabidir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-294">Calls made to a conditional method through `base`, of the form `base.M`, are subject to the normal conditional method call rules.</span></span> <span data-ttu-id="9de6f-295">Örnekte</span><span class="sxs-lookup"><span data-stu-id="9de6f-295">In the example</span></span>

<span data-ttu-id="9de6f-296">Dosya `class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="9de6f-296">File `class1.cs`:</span></span>

```csharp
using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public virtual void M() {
        Console.WriteLine("Class1.M executed");
    }
}
```

<span data-ttu-id="9de6f-297">Dosya `class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="9de6f-297">File `class2.cs`:</span></span>

```csharp
using System;

class Class2: Class1
{
    public override void M() {
        Console.WriteLine("Class2.M executed");
        base.M();                        // base.M is not called!
    }
}
```

<span data-ttu-id="9de6f-298">Dosya `class3.cs`:</span><span class="sxs-lookup"><span data-stu-id="9de6f-298">File `class3.cs`:</span></span>

```csharp
#define DEBUG

using System;

class Class3
{
    public static void Test() {
        Class2 c = new Class2();
        c.M();                            // M is called
    }
}
```

<span data-ttu-id="9de6f-299">`Class2` bir çağrı içerdiğine `M` , temel sınıfta tanımlanan.</span><span class="sxs-lookup"><span data-stu-id="9de6f-299">`Class2` includes a call to the `M` defined in its base class.</span></span> <span data-ttu-id="9de6f-300">Taban yönteminin sembolün varlığını temel alarak koşullu olduğu için bu çağrıyı atlanırsa `DEBUG`, tanımsız olduğu.</span><span class="sxs-lookup"><span data-stu-id="9de6f-300">This call is omitted because the base method is conditional based on the presence of the symbol `DEBUG`, which is undefined.</span></span> <span data-ttu-id="9de6f-301">Bu nedenle, yöntem, konsola yazar "`Class2.M executed`" yalnızca.</span><span class="sxs-lookup"><span data-stu-id="9de6f-301">Thus, the method writes to the console "`Class2.M executed`" only.</span></span> <span data-ttu-id="9de6f-302">Kullanmalıdır *pp_declaration*s, bu tür sorunları ortadan kaldırabilir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-302">Judicious use of *pp_declaration*s can eliminate such problems.</span></span>

#### <a name="conditional-attribute-classes"></a><span data-ttu-id="9de6f-303">Conditional özniteliği sınıfları</span><span class="sxs-lookup"><span data-stu-id="9de6f-303">Conditional attribute classes</span></span>

<span data-ttu-id="9de6f-304">Öznitelik sınıfı ([öznitelik sınıfları](attributes.md#attribute-classes)) biri veya daha fazlası ile donatılmış `Conditional` öznitelikleri bir ***koşullu öznitelik sınıfı***.</span><span class="sxs-lookup"><span data-stu-id="9de6f-304">An attribute class ([Attribute classes](attributes.md#attribute-classes)) decorated with one or more `Conditional` attributes is a ***conditional attribute class***.</span></span> <span data-ttu-id="9de6f-305">Conditional özniteliği sınıfı ile bildirilen koşullu derleme simgeleri nedenle ilişkilidir, `Conditional` öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="9de6f-305">A conditional attribute class is thus associated with the conditional compilation symbols declared in its `Conditional` attributes.</span></span> <span data-ttu-id="9de6f-306">Bu örnekte:</span><span class="sxs-lookup"><span data-stu-id="9de6f-306">This example:</span></span>

```csharp
using System;
using System.Diagnostics;
[Conditional("ALPHA")]
[Conditional("BETA")]
public class TestAttribute : Attribute {}
```

<span data-ttu-id="9de6f-307">bildirir `TestAttribute` koşullu derleme simgeleri ile ilişkili bir koşullu öznitelik sınıfı olarak `ALPHA` ve `BETA`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-307">declares `TestAttribute` as a conditional attribute class associated with the conditional compilations symbols `ALPHA` and `BETA`.</span></span>

<span data-ttu-id="9de6f-308">Öznitelik belirtimleri ([öznitelik belirtimi](attributes.md#attribute-specification)) bir veya daha fazla kendi ilişkili koşullu derleme simgeleri belirtimi noktasında, aksi takdirde tanımlanmışsa conditional özniteliği dahil edilen özniteliği belirtimi atlanır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-308">Attribute specifications ([Attribute specification](attributes.md#attribute-specification)) of a conditional attribute are included if one or more of its associated conditional compilation symbols is defined at the point of specification, otherwise the attribute specification is omitted.</span></span>

<span data-ttu-id="9de6f-309">Ekleme veya çıkarma bir öznitelik belirtimi bir conditional özniteliği sınıfın belirtimi noktasında koşullu derleme simgeleri tarafından denetlendiğini unutmayın önemlidir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-309">It is important to note that the inclusion or exclusion of an attribute specification of a conditional attribute class is controlled by the conditional compilation symbols at the point of the specification.</span></span> <span data-ttu-id="9de6f-310">Örnekte</span><span class="sxs-lookup"><span data-stu-id="9de6f-310">In the example</span></span>

<span data-ttu-id="9de6f-311">Dosya `test.cs`:</span><span class="sxs-lookup"><span data-stu-id="9de6f-311">File `test.cs`:</span></span>

```csharp
using System;
using System.Diagnostics;

[Conditional("DEBUG")]

public class TestAttribute : Attribute {}
```

<span data-ttu-id="9de6f-312">Dosya `class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="9de6f-312">File `class1.cs`:</span></span>

```csharp
#define DEBUG

[Test]                // TestAttribute is specified

class Class1 {}
```

<span data-ttu-id="9de6f-313">Dosya `class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="9de6f-313">File `class2.cs`:</span></span>

```csharp
#undef DEBUG

[Test]                 // TestAttribute is not specified

class Class2 {}
```

<span data-ttu-id="9de6f-314">sınıfları `Class1` ve `Class2` olan her özniteliği ile donatılmış `Test`, koşullu olduğu olmamasına tabanlı `DEBUG` tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-314">the classes `Class1` and `Class2` are each decorated with attribute `Test`, which is conditional based on whether or not `DEBUG` is defined.</span></span> <span data-ttu-id="9de6f-315">Bu sembol bağlamında tanımlamış `Class1` ama `Class2`, belirtimi `Test` özniteliği `Class1` , belirtimi sırasında bulunan `Test` özniteliği `Class2` atlanır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-315">Since this symbol is defined in the context of `Class1` but not `Class2`, the specification of the `Test` attribute on `Class1` is included, while the specification of the `Test` attribute on `Class2` is omitted.</span></span>

### <a name="the-obsolete-attribute"></a><span data-ttu-id="9de6f-316">Geçersiz öznitelik</span><span class="sxs-lookup"><span data-stu-id="9de6f-316">The Obsolete attribute</span></span>

<span data-ttu-id="9de6f-317">Öznitelik `Obsolete` türleri ve üyeleri artık kullanılması gereken türleri işaretlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-317">The attribute `Obsolete` is used to mark types and members of types that should no longer be used.</span></span>

```csharp
namespace System
{
    [AttributeUsage(
        AttributeTargets.Class | 
        AttributeTargets.Struct |
        AttributeTargets.Enum | 
        AttributeTargets.Interface | 
        AttributeTargets.Delegate |
        AttributeTargets.Method | 
        AttributeTargets.Constructor |
        AttributeTargets.Property | 
        AttributeTargets.Field |
        AttributeTargets.Event,
        Inherited = false)
    ]
    public class ObsoleteAttribute: Attribute
    {
        public ObsoleteAttribute() {...}
        public ObsoleteAttribute(string message) {...}
        public ObsoleteAttribute(string message, bool error) {...}
        public string Message { get {...} }
        public bool IsError { get {...} }
    }
}
```

<span data-ttu-id="9de6f-318">Bir program türü veya ile donatılmış üye kullanıp kullanmadığını `Obsolete` özniteliği, derleyici sorunları bir uyarı veya hata.</span><span class="sxs-lookup"><span data-stu-id="9de6f-318">If a program uses a type or member that is decorated with the `Obsolete` attribute, the compiler issues a warning or an error.</span></span> <span data-ttu-id="9de6f-319">Özellikle, derleyici hatası parametre sağlanmazsa veya hata parametresi sağlanır ve değere sahipse bir uyarı verir `false`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-319">Specifically, the compiler issues a warning if no error parameter is provided, or if the error parameter is provided and has the value `false`.</span></span> <span data-ttu-id="9de6f-320">Hata parametresi belirtilmişse ve değerine sahip, derleyici bir hata verir `true`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-320">The compiler issues an error if the error parameter is specified and has the value `true`.</span></span>

<span data-ttu-id="9de6f-321">Örnekte</span><span class="sxs-lookup"><span data-stu-id="9de6f-321">In the example</span></span>

```csharp
[Obsolete("This class is obsolete; use class B instead")]
class A
{
    public void F() {}
}

class B
{
    public void F() {}
}

class Test
{
    static void Main() {
        A a = new A();         // Warning
        a.F();
    }
}
```

<span data-ttu-id="9de6f-322">sınıf `A` ile donatılmış `Obsolete` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="9de6f-322">the class `A` is decorated with the `Obsolete` attribute.</span></span> <span data-ttu-id="9de6f-323">Her kullanımının `A` içinde `Main` belirtilen iletiyi içeren bir uyarı sonuçları, "Bu sınıf artık kullanılmıyor; B sınıfı kullanın."</span><span class="sxs-lookup"><span data-stu-id="9de6f-323">Each use of `A` in `Main` results in a warning that includes the specified message, "This class is obsolete; use class B instead."</span></span>

### <a name="caller-info-attributes"></a><span data-ttu-id="9de6f-324">Arayan bilgileri öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="9de6f-324">Caller info attributes</span></span>

<span data-ttu-id="9de6f-325">Günlüğe kaydetme ve raporlama gibi amaçlarla belirli çağıran kod derleme zamanı bilgilerini almak işlev üyesi bazen kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-325">For purposes such as logging and reporting, it is sometimes useful for a function member to obtain certain compile-time information about the calling code.</span></span> <span data-ttu-id="9de6f-326">Arayan bilgileri öznitelikleri gibi bilgileri şeffaf bir şekilde geçirmek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="9de6f-326">The caller info attributes provide a way to pass such information transparently.</span></span>

<span data-ttu-id="9de6f-327">İsteğe bağlı parametresi arayan bilgileri öznitelikleri biriyle açıklandığında bir çağrıda karşılık gelen bağımsız değişken atlama mutlaka yerine kullanılacak varsayılan parametre değeri neden olmaz.</span><span class="sxs-lookup"><span data-stu-id="9de6f-327">When an optional parameter is annotated with one of the caller info attributes, omitting the corresponding argument in a call does not necessarily cause the default parameter value to be substituted.</span></span> <span data-ttu-id="9de6f-328">Bunun yerine, arama bağlamı hakkında belirli bilgi varsa, bu bilgileri bağımsız değişken değeri geçirilir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-328">Instead, if the specified information about the calling context is available, that information will be passed as the argument value.</span></span>

<span data-ttu-id="9de6f-329">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="9de6f-329">For example:</span></span>

```csharp
using System.Runtime.CompilerServices

...

public void Log(
    [CallerLineNumber] int line = -1,
    [CallerFilePath]   string path = null,
    [CallerMemberName] string name = null
)
{
    Console.WriteLine((line < 0) ? "No line" : "Line "+ line);
    Console.WriteLine((path == null) ? "No file path" : path);
    Console.WriteLine((name == null) ? "No member name" : name);
}
```

<span data-ttu-id="9de6f-330">Bir çağrı `Log()` bağımsız değişken olmadan satır numarasını ve dosya yolu çağrısının yanı sıra, çağrı içinde gerçekleştiği üyesinin adı yazdırır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-330">A call to `Log()` with no arguments would print the line number and file path of the call, as well as the name of the member within which the call occurred.</span></span>

<span data-ttu-id="9de6f-331">Arayan bilgisi öznitelikleri, temsilci bildirimlerinde de dahil olmak üzere herhangi bir yerde, isteğe bağlı parametreler ortaya çıkabilir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-331">Caller info attributes can occur on optional parameters anywhere, including in delegate declarations.</span></span> <span data-ttu-id="9de6f-332">Ancak belirli arayan bilgileri öznitelikleri her zaman olmayacaktır parametre türü bir yedek değerinden örtük dönüştürmeleri böylece bunlar özniteliği parametre türlerine kısıtlamaları vardır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-332">However, the specific caller info attributes have restrictions on the types of the parameters they can attribute, so that there will always be an implicit conversion from a substituted value to the parameter type.</span></span>

<span data-ttu-id="9de6f-333">Bu hem tanımlama ve kısmi yöntem bildiriminin bir parçası uygulama parametresi aynı çağıran bilgisi özniteliği için bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-333">It is an error to have the same caller info attribute on a parameter of both the defining and implementing part of a partial method declaration.</span></span> <span data-ttu-id="9de6f-334">Arayan bilgileri öznitelikleri yalnızca uygulama bölümünde gerçekleşen göz ardı edilir ancak yalnızca arayan bilgisi özniteliklerini tanımlayan bir parçası olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-334">Only caller info attributes in the defining part are applied, whereas caller info attributes occurring only in the implementing part are ignored.</span></span>

<span data-ttu-id="9de6f-335">Arayan bilgileri aşırı yükleme çözünürlüğü etkilemez.</span><span class="sxs-lookup"><span data-stu-id="9de6f-335">Caller information does not affect overload resolution.</span></span> <span data-ttu-id="9de6f-336">Öznitelikli isteğe bağlı parametreler hala çağıran kaynak kodundan atlanır gibi aşırı yükleme çözünürlüğü Bu parametre belirtilmemişse diğer isteğe bağlı parametreleri yoksayar aynı şekilde yok sayar. ([aşırı yükleme çözünürlüğü](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="9de6f-336">As the attributed optional parameters are still omitted from the source code of the caller, overload resolution ignores those parameters in the same way it ignores other omitted optional parameters ([Overload resolution](expressions.md#overload-resolution)).</span></span>

<span data-ttu-id="9de6f-337">Kaynak kodunda açıkça bir işlev çağrıldığında arayan bilgileri yalnızca konur.</span><span class="sxs-lookup"><span data-stu-id="9de6f-337">Caller information is only substituted when a function is explicitly invoked in source code.</span></span> <span data-ttu-id="9de6f-338">Örtük çağrıları örtük üst Oluşturucu çağrıları gibi bir kaynak konumu yok ve arayan bilgileri alternatif değildir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-338">Implicit invocations such as implicit parent constructor calls do not have a source location and will not substitute caller information.</span></span> <span data-ttu-id="9de6f-339">Ayrıca, dinamik olarak bağlı çağrıları arayan bilgileri yerine değil.</span><span class="sxs-lookup"><span data-stu-id="9de6f-339">Also, calls that are dynamically bound will not substitute caller information.</span></span> <span data-ttu-id="9de6f-340">Parametrenin belirtilen varsayılan değeri, bu gibi durumlarda öznitelikli parametre atlanırsa bir arayan bilgisi, bunun yerine kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-340">When a caller info attributed parameter is omitted in such cases, the specified default value of the parameter is used instead.</span></span>

<span data-ttu-id="9de6f-341">Sorgu ifadeleri bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-341">One exception is query-expressions.</span></span> <span data-ttu-id="9de6f-342">Bu söz dizimi genişletmeleri kabul edilir ve çağrıları bunlar arayan bilgileri öznitelikleri isteğe bağlı parametrelerle atlamak için genişletirseniz, arayan bilgileri değiştirilecektir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-342">These are considered syntactic expansions, and if the calls they expand to omit optional parameters with caller info attributes, caller information will be substituted.</span></span> <span data-ttu-id="9de6f-343">Kullanılan konumu çağrı oluşturulan sorgu yan tümcesi konumudur.</span><span class="sxs-lookup"><span data-stu-id="9de6f-343">The location used is the location of the query clause which the call was generated from.</span></span>

<span data-ttu-id="9de6f-344">Verilen bir parametre üzerinde birden fazla çağıran bilgisi özniteliği belirtilmezse, bunlar şu sırayla tercih edilen: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.</span><span class="sxs-lookup"><span data-stu-id="9de6f-344">If more than one caller info attribute is specified on a given parameter, they are preferred in the following order: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.</span></span>

#### <a name="the-callerlinenumber-attribute"></a><span data-ttu-id="9de6f-345">CallerLineNumber özniteliği</span><span class="sxs-lookup"><span data-stu-id="9de6f-345">The CallerLineNumber attribute</span></span>

<span data-ttu-id="9de6f-346">`System.Runtime.CompilerServices.CallerLineNumberAttribute` Standart bir örtük dönüştürme olduğunda isteğe bağlı parametrelere izin ([standart örtük dönüştürmeler](conversions.md#standard-implicit-conversions)) sabit değerinden `int.MaxValue` parametrenin türü.</span><span class="sxs-lookup"><span data-stu-id="9de6f-346">The `System.Runtime.CompilerServices.CallerLineNumberAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from the constant value `int.MaxValue` to the parameter's type.</span></span> <span data-ttu-id="9de6f-347">Bu, bu değer kadar herhangi bir negatif olmayan satır numarasına hatasız geçirilebilir sağlar.</span><span class="sxs-lookup"><span data-stu-id="9de6f-347">This ensures that any non-negative line number up to that value can be passed without error.</span></span>

<span data-ttu-id="9de6f-348">İsteğe bağlı bir parametre ile bir konumda bir kaynak kodu bir işlev çağırmasından çıkarırsa `CallerLineNumberAttribute`, o konumun satır numarasını temsil eden sayısal bir değişmez değeri varsayılan parametre değeri yerine çağırma bağımsız değişkeni olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-348">If a function invocation from a location in source code omits an optional parameter with the `CallerLineNumberAttribute`, then a numeric literal representing that location's line number is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="9de6f-349">Çok satırlı çağırma yayılmış durumdaysa de seçilen satıra uygulamaya bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-349">If the invocation spans multiple lines, the line chosen is implementation-dependent.</span></span>

<span data-ttu-id="9de6f-350">Satır numarası etkilenebilir Not `#line` yönergeleri ([satır yönergeleri](lexical-structure.md#line-directives)).</span><span class="sxs-lookup"><span data-stu-id="9de6f-350">Note that the line number may be affected by `#line` directives ([Line directives](lexical-structure.md#line-directives)).</span></span>

#### <a name="the-callerfilepath-attribute"></a><span data-ttu-id="9de6f-351">CallerFilePath özniteliği</span><span class="sxs-lookup"><span data-stu-id="9de6f-351">The CallerFilePath attribute</span></span>

<span data-ttu-id="9de6f-352">`System.Runtime.CompilerServices.CallerFilePathAttribute` Standart bir örtük dönüştürme olduğunda isteğe bağlı parametrelere izin ([standart örtük dönüştürmeler](conversions.md#standard-implicit-conversions)) öğesinden `string` parametrenin türü.</span><span class="sxs-lookup"><span data-stu-id="9de6f-352">The `System.Runtime.CompilerServices.CallerFilePathAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from `string` to the parameter's type.</span></span>

<span data-ttu-id="9de6f-353">İsteğe bağlı bir parametre ile bir konumda bir kaynak kodu bir işlev çağırmasından çıkarırsa `CallerFilePathAttribute`, o konumun yolunu temsil eden bir dize sabit değeri varsayılan parametre değeri yerine çağırma bağımsız değişkeni olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-353">If a function invocation from a location in source code omits an optional parameter with the `CallerFilePathAttribute`, then a string literal representing that location's file path is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="9de6f-354">Dosya yolunun biçimi uygulamaya bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-354">The format of the file path is implementation-dependent.</span></span>

<span data-ttu-id="9de6f-355">Dosya yolu etkilenebilir Not `#line` yönergeleri ([satır yönergeleri](lexical-structure.md#line-directives)).</span><span class="sxs-lookup"><span data-stu-id="9de6f-355">Note that the file path may be affected by `#line` directives ([Line directives](lexical-structure.md#line-directives)).</span></span>

#### <a name="the-callermembername-attribute"></a><span data-ttu-id="9de6f-356">CallerMemberName özniteliği</span><span class="sxs-lookup"><span data-stu-id="9de6f-356">The CallerMemberName attribute</span></span>

<span data-ttu-id="9de6f-357">`System.Runtime.CompilerServices.CallerMemberNameAttribute` Standart bir örtük dönüştürme olduğunda isteğe bağlı parametrelere izin ([standart örtük dönüştürmeler](conversions.md#standard-implicit-conversions)) öğesinden `string` parametrenin türü.</span><span class="sxs-lookup"><span data-stu-id="9de6f-357">The `System.Runtime.CompilerServices.CallerMemberNameAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from `string` to the parameter's type.</span></span>

<span data-ttu-id="9de6f-358">Kendisi veya kendi dönüş türü, parametre veya tür parametrelerinde bir konuma bir öznitelik veya işlevi üyesinin gövdesi içinde bir işlev çağırmasından işlevi üyeye uygulanan, kaynak kodu ile isteğe bağlı bir parametre atlar `CallerMemberNameAttribute`, bir Bu üyenin adını temsil eden bir dize sabit değeri varsayılan parametre değeri yerine çağırma bağımsız değişkeni olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-358">If a function invocation from a location within the body of a function member or within an attribute applied to the function member itself or its return type, parameters or type parameters in source code omits an optional parameter with the `CallerMemberNameAttribute`, then a string literal representing the name of that member is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="9de6f-359">Genel yöntemler içinde gerçekleşen çağrılarını yöntemi adın tamamı yalnızca, tür parametresi listesi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-359">For invocations that occur within generic methods, only the method name itself is used, without the type parameter list.</span></span>

<span data-ttu-id="9de6f-360">Açık arabirim üyesi uygulamalarını içinde gerçekleşen çağrılarını yöntemi adın tamamı yalnızca, önceki arabirimi niteleme olmadan kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-360">For invocations that occur within explicit interface member implementations, only the method name itself is used, without the preceding interface qualification.</span></span>

<span data-ttu-id="9de6f-361">Özellik veya olay erişimcileri içinde gerçekleşen çağrıları için kullanılan üyesi, özelliğin veya olayın kendisinin addır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-361">For invocations that occur within property or event accessors, the member name used is that of the property or event itself.</span></span>

<span data-ttu-id="9de6f-362">Dizin Oluşturucu erişimcileri içinde gerçekleşen çağrıları için kullanılan üye tarafından sağlanan addır bir `IndexerNameAttribute` ([IndexerName özniteliği](attributes.md#the-indexername-attribute)) varsa, dizin oluşturucu üye veya varsayılan adı `Item` Aksi takdirde.</span><span class="sxs-lookup"><span data-stu-id="9de6f-362">For invocations that occur within indexer accessors, the member name used is that supplied by an `IndexerNameAttribute` ([The IndexerName attribute](attributes.md#the-indexername-attribute)) on the indexer member, if present, or the default name `Item` otherwise.</span></span>

<span data-ttu-id="9de6f-363">Örnek oluşturucuları, statik Oluşturucular, Yıkıcılar ve işleçler bildirimlerinde, üye ortaya çağrıları için kullanılan ad uygulamaya bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-363">For invocations that occur within declarations of instance constructors, static constructors, destructors and operators the member name used is implementation-dependent.</span></span>

## <a name="attributes-for-interoperation"></a><span data-ttu-id="9de6f-364">Birlikte çalışma için öznitelikler</span><span class="sxs-lookup"><span data-stu-id="9de6f-364">Attributes for Interoperation</span></span>

<span data-ttu-id="9de6f-365">Not: Bu bölüm, yalnızca Microsoft .NET uygulaması için geçerlidir C#.</span><span class="sxs-lookup"><span data-stu-id="9de6f-365">Note: This section is applicable only to the Microsoft .NET implementation of C#.</span></span>

### <a name="interoperation-with-com-and-win32-components"></a><span data-ttu-id="9de6f-366">COM ve Win32 bileşenleri ile birlikte çalışma</span><span class="sxs-lookup"><span data-stu-id="9de6f-366">Interoperation with COM and Win32 components</span></span>

<span data-ttu-id="9de6f-367">.NET çalışma zamanı çok sayıda COM ve Win32 DLL'leri kullanılarak yazılmış bileşenler ile çalışmak C# programları etkinleştirme öznitelikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="9de6f-367">The .NET run-time provides a large number of attributes that enable C# programs to interoperate with components written using COM and Win32 DLLs.</span></span> <span data-ttu-id="9de6f-368">Örneğin, `DllImport` özniteliği kullanılabilir bir `static extern` bir Win32 DLL içinde bulunacak yöntemin uygulanmasını olduğunu belirtmek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9de6f-368">For example, the `DllImport` attribute can be used on a `static extern` method to indicate that the implementation of the method is to be found in a Win32 DLL.</span></span> <span data-ttu-id="9de6f-369">Bu öznitelikler bulunan `System.Runtime.InteropServices` ad alanı ve bu öznitelikler için ayrıntılı belgelere .NET çalışma zamanı belgelerinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="9de6f-369">These attributes are found in the `System.Runtime.InteropServices` namespace, and detailed documentation for these attributes is found in the .NET runtime documentation.</span></span>

### <a name="interoperation-with-other-net-languages"></a><span data-ttu-id="9de6f-370">Diğer .NET dilleri ile birlikte çalışma</span><span class="sxs-lookup"><span data-stu-id="9de6f-370">Interoperation with other .NET languages</span></span>

#### <a name="the-indexername-attribute"></a><span data-ttu-id="9de6f-371">IndexerName özniteliği</span><span class="sxs-lookup"><span data-stu-id="9de6f-371">The IndexerName attribute</span></span>

<span data-ttu-id="9de6f-372">Dizin oluşturucular dizinli özellikler kullanılarak. NET'te uygulanır ve .NET meta verilerde bir ada sahip.</span><span class="sxs-lookup"><span data-stu-id="9de6f-372">Indexers are implemented in .NET using indexed properties, and have a name in the .NET metadata.</span></span> <span data-ttu-id="9de6f-373">Hayır ise `IndexerName` özniteliktir bir dizin oluşturucu ve ardından adı için mevcut `Item` varsayılan olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9de6f-373">If no `IndexerName` attribute is present for an indexer, then the name `Item` is used by default.</span></span> <span data-ttu-id="9de6f-374">`IndexerName` Özniteliği sağlayan bir geliştirici bu varsayılanı geçersiz kılmak ve farklı bir ad belirtin.</span><span class="sxs-lookup"><span data-stu-id="9de6f-374">The `IndexerName` attribute enables a developer to override this default and specify a different name.</span></span>

```csharp
namespace System.Runtime.CompilerServices.CSharp
{
    [AttributeUsage(AttributeTargets.Property)]
    public class IndexerNameAttribute: Attribute
    {
        public IndexerNameAttribute(string indexerName) {...}
        public string Value { get {...} } 
    }
}
```
