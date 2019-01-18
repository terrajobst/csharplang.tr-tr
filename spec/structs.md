---
ms.openlocfilehash: 72d17175dfb8ef284dce6cf7e5837420fa06f16a
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229960"
---
# <a name="structs"></a><span data-ttu-id="5e468-101">Yapılar</span><span class="sxs-lookup"><span data-stu-id="5e468-101">Structs</span></span>

<span data-ttu-id="5e468-102">Veri üyeleri ve işlev üyeleri içerebilir veri yapılarını temsil ettikleri yapılar için sınıflar benzer.</span><span class="sxs-lookup"><span data-stu-id="5e468-102">Structs are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="5e468-103">Ancak, farklı sınıflar, yapılar değer türleri ve yığın ayırma gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="5e468-103">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="5e468-104">Bir sınıf türünün bir değişkeni verilerin ikinci bilinen bir nesne olarak başvuru içerirken bir yapı türünün değişkenini doğrudan, struct'ın verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="5e468-104">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span>

<span data-ttu-id="5e468-105">Yapılar değer semantiklere sahip küçük veri yapıları için özellikle yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="5e468-105">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="5e468-106">Karmaşık sayılar, koordinat sisteminde noktaları veya anahtar-değer çiftlerinin bir sözlükteki tüm iyi yapılar örnekleridir.</span><span class="sxs-lookup"><span data-stu-id="5e468-106">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="5e468-107">Bu veri yapıları için anahtar, birkaç veri üyeleri, devralma veya başvuru kimliği kullanımı gerektirmez ve bunların rahatça atama değerin başvurusu yerine nereye kopyalayacağını değeri semantiği kullanıyor uygulanabilir olduğunu sahip olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="5e468-107">Key to these data structures is that they have few data members, that they do not require use of inheritance or referential identity, and that they can be conveniently implemented using value semantics where assignment copies the value instead of the reference.</span></span>

<span data-ttu-id="5e468-108">Bölümünde anlatıldığı gibi [basit türler](types.md#simple-types), sağlanan C# kullanarak gibi basit türler `int`, `double`, ve `bool`, hatta tüm yapı türleri şunlardır.</span><span class="sxs-lookup"><span data-stu-id="5e468-108">As described in [Simple types](types.md#simple-types), the simple types provided by C#, such as `int`, `double`, and `bool`, are in fact all struct types.</span></span> <span data-ttu-id="5e468-109">Yalnızca bu önceden tanımlanmış türler yapı birimleridir gibi ayrıca yapıları ve yeni "temel" türler C# dilinde uygulamak için işleç aşırı yüklemesi kullanmak da mümkündür.</span><span class="sxs-lookup"><span data-stu-id="5e468-109">Just as these predefined types are structs, it is also possible to use structs and operator overloading to implement new "primitive" types in the C# language.</span></span> <span data-ttu-id="5e468-110">İki tür örnekleri, tür, bu bölümün sonunda verilmiştir ([yapı örnekler](structs.md#struct-examples)).</span><span class="sxs-lookup"><span data-stu-id="5e468-110">Two examples of such types are given at the end of this chapter ([Struct examples](structs.md#struct-examples)).</span></span>

## <a name="struct-declarations"></a><span data-ttu-id="5e468-111">Yapı bildirimleri</span><span class="sxs-lookup"><span data-stu-id="5e468-111">Struct declarations</span></span>

<span data-ttu-id="5e468-112">A *struct_declaration* olduğu bir *type_declaration* ([tür bildirimleri](namespaces.md#type-declarations)), yeni bir yapı bildirir:</span><span class="sxs-lookup"><span data-stu-id="5e468-112">A *struct_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new struct:</span></span>

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

<span data-ttu-id="5e468-113">A *struct_declaration* isteğe bağlı bir kümesinden oluşur *öznitelikleri* ([öznitelikleri](attributes.md)) ve ardından isteğe bağlı bir dizi *struct_modifier*s ([yapı değiştiriciler](structs.md#struct-modifiers)) ve ardından isteğe bağlı `partial` değiştiricisi anahtar sözcüğü ve ardından, `struct` ve *tanımlayıcı* ardından yapısı adları bir İsteğe bağlı *type_parameter_list* belirtimi ([tür parametrelerindeki](classes.md#type-parameters)) ve ardından isteğe bağlı *struct_interfaces* belirtimi ([Partial değiştiricisi](structs.md#partial-modifier))) ve ardından isteğe bağlı *type_parameter_constraints_clause*s belirtimi ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) ve ardından bir *struct_body* ([yapı gövdesi](structs.md#struct-body)), noktalı virgül tarafından izlenen isteğe bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="5e468-113">A *struct_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *struct_modifier*s ([Struct modifiers](structs.md#struct-modifiers)), followed by an optional `partial` modifier, followed by the keyword `struct` and an *identifier* that names the struct, followed by an optional *type_parameter_list* specification ([Type parameters](classes.md#type-parameters)), followed by an optional *struct_interfaces* specification ([Partial modifier](structs.md#partial-modifier)) ), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *struct_body* ([Struct body](structs.md#struct-body)), optionally followed by a semicolon.</span></span>

### <a name="struct-modifiers"></a><span data-ttu-id="5e468-114">Yapı değiştiriciler</span><span class="sxs-lookup"><span data-stu-id="5e468-114">Struct modifiers</span></span>

<span data-ttu-id="5e468-115">A *struct_declaration* isteğe bağlı olarak bir dizi yapı değiştiriciler içerebilir:</span><span class="sxs-lookup"><span data-stu-id="5e468-115">A *struct_declaration* may optionally include a sequence of struct modifiers:</span></span>

```antlr
struct_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | struct_modifier_unsafe
    ;
```

<span data-ttu-id="5e468-116">Aynı değiştiricisi bir struct bildiriminde birden çok kez görünmesi için bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="5e468-116">It is a compile-time error for the same modifier to appear multiple times in a struct declaration.</span></span>

<span data-ttu-id="5e468-117">Bu sınıf bildirimi aynı anlamı, bir yapı bildirimi, değiştiricilere sahip ([sınıf bildirimleri](classes.md#class-declarations)).</span><span class="sxs-lookup"><span data-stu-id="5e468-117">The modifiers of a struct declaration have the same meaning as those of a class declaration ([Class declarations](classes.md#class-declarations)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="5e468-118">Partial değiştiricisi</span><span class="sxs-lookup"><span data-stu-id="5e468-118">Partial modifier</span></span>

<span data-ttu-id="5e468-119">`partial` Değiştiricisi gösteren bu *struct_declaration* kısmi türü bildirimi.</span><span class="sxs-lookup"><span data-stu-id="5e468-119">The `partial` modifier indicates that this *struct_declaration* is a partial type declaration.</span></span> <span data-ttu-id="5e468-120">Kapsayan ad alanı veya tür bildirimi içinde aynı ada sahip birden fazla kısmi sınıfının veya yapı bildirimi birleştiren bir yapı bildirimi oluşturmak için aşağıdaki kuralları belirtilen [kısmi türlerinden](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="5e468-120">Multiple partial struct declarations with the same name within an enclosing namespace or type declaration combine to form one struct declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="struct-interfaces"></a><span data-ttu-id="5e468-121">Yapı arabirimleri</span><span class="sxs-lookup"><span data-stu-id="5e468-121">Struct interfaces</span></span>

<span data-ttu-id="5e468-122">Yapı bildirimi içerebilir bir *struct_interfaces* içinde çalışması struct söyledi doğrudan belirli bir arabirim türleri uygulamak için belirtimi.</span><span class="sxs-lookup"><span data-stu-id="5e468-122">A struct declaration may include a *struct_interfaces* specification, in which case the struct is said to directly implement the given interface types.</span></span>

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

<span data-ttu-id="5e468-123">Arabirim uygulamalarına açıklanmıştır daha ayrıntılı olarak [arabirimi uygulamaları](interfaces.md#interface-implementations).</span><span class="sxs-lookup"><span data-stu-id="5e468-123">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="struct-body"></a><span data-ttu-id="5e468-124">Yapı gövdesi</span><span class="sxs-lookup"><span data-stu-id="5e468-124">Struct body</span></span>

<span data-ttu-id="5e468-125">*Struct_body* struct'ın Yapı üyeleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="5e468-125">The *struct_body* of a struct defines the members of the struct.</span></span>

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a><span data-ttu-id="5e468-126">Yapı üyeleri</span><span class="sxs-lookup"><span data-stu-id="5e468-126">Struct members</span></span>

<span data-ttu-id="5e468-127">Yapı üyeleri tarafından tanıtılan üyeleri oluşur, *struct_member_declaration*s ve üyeleri devralınan bir türden `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="5e468-127">The members of a struct consist of the members introduced by its *struct_member_declaration*s and the members inherited from the type `System.ValueType`.</span></span>

```antlr
struct_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | static_constructor_declaration
    | type_declaration
    | struct_member_declaration_unsafe
    ;
```

<span data-ttu-id="5e468-128">Belirtilen farklılıklar dışında [sınıf ile yapı farklar](structs.md#class-and-struct-differences), sağlanan sınıf üyeleri açıklamalarını [sınıf üyeleri](classes.md#class-members) aracılığıyla [yineleyiciler](classes.md#iterators) yapısı için geçerlidir Üyeler de.</span><span class="sxs-lookup"><span data-stu-id="5e468-128">Except for the differences noted in [Class and struct differences](structs.md#class-and-struct-differences), the descriptions of class members provided in [Class members](classes.md#class-members) through [Iterators](classes.md#iterators) apply to struct members as well.</span></span>

## <a name="class-and-struct-differences"></a><span data-ttu-id="5e468-129">Sınıf ile yapı farkları</span><span class="sxs-lookup"><span data-stu-id="5e468-129">Class and struct differences</span></span>

<span data-ttu-id="5e468-130">Yapılar, birkaç önemli şekilde sınıflardan farklılık gösterir:</span><span class="sxs-lookup"><span data-stu-id="5e468-130">Structs differ from classes in several important ways:</span></span>

*  <span data-ttu-id="5e468-131">Değer türleri yapı birimleridir ([değer semantiği](structs.md#value-semantics)).</span><span class="sxs-lookup"><span data-stu-id="5e468-131">Structs are value types ([Value semantics](structs.md#value-semantics)).</span></span>
*  <span data-ttu-id="5e468-132">Tüm yapı türleri örtülü olarak sınıftan `System.ValueType` ([devralma](structs.md#inheritance)).</span><span class="sxs-lookup"><span data-stu-id="5e468-132">All struct types implicitly inherit from the class `System.ValueType` ([Inheritance](structs.md#inheritance)).</span></span>
*  <span data-ttu-id="5e468-133">Bir yapı türü bir değişkene atanması, atanan değerin bir kopyasını oluşturur ([atama](structs.md#assignment)).</span><span class="sxs-lookup"><span data-stu-id="5e468-133">Assignment to a variable of a struct type creates a copy of the value being assigned ([Assignment](structs.md#assignment)).</span></span>
*  <span data-ttu-id="5e468-134">Bir yapı varsayılan değer tür alanları için tüm değer tür alanları varsayılan değerlerine ve tüm başvuru ayarlayarak üretilen değeridir `null` ([varsayılan değerler](structs.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="5e468-134">The default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null` ([Default values](structs.md#default-values)).</span></span>
*  <span data-ttu-id="5e468-135">Kutulama ve kutudan çıkarma işlemleri, bir yapı türü arasında dönüştürmek için kullanılır ve `object` ([kutulama ve kutudan çıkarma](structs.md#boxing-and-unboxing)).</span><span class="sxs-lookup"><span data-stu-id="5e468-135">Boxing and unboxing operations are used to convert between a struct type and `object` ([Boxing and unboxing](structs.md#boxing-and-unboxing)).</span></span>
*  <span data-ttu-id="5e468-136">Anlamını `this` yapılar için farklı ([bu erişim](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="5e468-136">The meaning of `this` is different for structs ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="5e468-137">Bir yapı için örnek alanı bildirimleri, değişken başlatıcılar dahil izin verilmez ([alan başlatıcıları](structs.md#field-initializers)).</span><span class="sxs-lookup"><span data-stu-id="5e468-137">Instance field declarations for a struct are not permitted to include variable initializers ([Field initializers](structs.md#field-initializers)).</span></span>
*  <span data-ttu-id="5e468-138">Bir yapının bir parametresiz örnek oluşturucusu bildirmek için izin verilmiyor ([oluşturucular](structs.md#constructors)).</span><span class="sxs-lookup"><span data-stu-id="5e468-138">A struct is not permitted to declare a parameterless instance constructor ([Constructors](structs.md#constructors)).</span></span>
*  <span data-ttu-id="5e468-139">Bir yapının bir yıkıcı bildirmek için izin verilmiyor ([yok ediciler](structs.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="5e468-139">A struct is not permitted to declare a destructor ([Destructors](structs.md#destructors)).</span></span>

### <a name="value-semantics"></a><span data-ttu-id="5e468-140">Değer semantiği</span><span class="sxs-lookup"><span data-stu-id="5e468-140">Value semantics</span></span>

<span data-ttu-id="5e468-141">Değer türleri yapı birimleridir ([değer türleri](types.md#value-types)) ve değer semantiği olduğu söylenir.</span><span class="sxs-lookup"><span data-stu-id="5e468-141">Structs are value types ([Value types](types.md#value-types)) and are said to have value semantics.</span></span> <span data-ttu-id="5e468-142">Sınıflar, diğer taraftan, başvuru türleridir ([başvuru türleri](types.md#reference-types)) ve başvuru semantiği olduğu söylenir.</span><span class="sxs-lookup"><span data-stu-id="5e468-142">Classes, on the other hand, are reference types ([Reference types](types.md#reference-types)) and are said to have reference semantics.</span></span>

<span data-ttu-id="5e468-143">Bir sınıf türünün bir değişkeni verilerin ikinci bilinen bir nesne olarak başvuru içerirken bir yapı türünün değişkenini doğrudan, struct'ın verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="5e468-143">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span> <span data-ttu-id="5e468-144">Bir struct olduğunda `B` bir örnek alan türü içeren `A` ve `A` bir yapı türü için bir derleme zamanı hata `A` bağlıdır `B` veya bir tür oluşturulan `B`.</span><span class="sxs-lookup"><span data-stu-id="5e468-144">When a struct `B` contains an instance field of type `A` and `A` is a struct type, it is a compile-time error for `A` to depend on `B` or a type constructed from `B`.</span></span> <span data-ttu-id="5e468-145">Bir yapı `X` ***doğrudan bağlı*** yapı `Y` varsa `X` bir örnek alan türü içeren `Y`.</span><span class="sxs-lookup"><span data-stu-id="5e468-145">A struct `X` ***directly depends on*** a struct `Y` if `X` contains an instance field of type `Y`.</span></span> <span data-ttu-id="5e468-146">Bu tanımı verildiğinde, tam bir yapı olduğu yapıları geçişli kapatılmasını kümesidir ***doğrudan bağlı*** ilişki.</span><span class="sxs-lookup"><span data-stu-id="5e468-146">Given this definition, the complete set of structs upon which a struct depends is the transitive closure of the ***directly depends on*** relationship.</span></span>  <span data-ttu-id="5e468-147">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5e468-147">For example</span></span>
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
<span data-ttu-id="5e468-148">bir hata olduğundan `Node` kendi türünde bir örnek alanı içerir.</span><span class="sxs-lookup"><span data-stu-id="5e468-148">is an error because `Node` contains an instance field of its own type.</span></span>  <span data-ttu-id="5e468-149">Başka bir örnek</span><span class="sxs-lookup"><span data-stu-id="5e468-149">Another example</span></span>
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
<span data-ttu-id="5e468-150">bir hata olduğundan türlerinin her biri `A`, `B`, ve `C` birbirine bağımlı.</span><span class="sxs-lookup"><span data-stu-id="5e468-150">is an error because each of the types `A`, `B`, and `C` depend on each other.</span></span>

<span data-ttu-id="5e468-151">Sınıfları ile bu iki değişken aynı nesneye başvurmak mümkün ve dolayısıyla işlemler diğer değişkenin başvurduğu nesneyi etkileyebilir bir değişken üzerinde mümkün olur.</span><span class="sxs-lookup"><span data-stu-id="5e468-151">With classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="5e468-152">Yapılar ile her değişkenleri kendi veri kopyası vardır (dışındaki durumunda `ref` ve `out` parametresi değişkenleri), ve işlemlerin bir diğerini etkilemesi olanaklı değildir.</span><span class="sxs-lookup"><span data-stu-id="5e468-152">With structs, the variables each have their own copy of the data (except in the case of `ref` and `out` parameter variables), and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="5e468-153">Yapılar, başvuru türleri değil, ayrıca, mümkün olması için bir yapı türü değerleri için değildir, çünkü `null`.</span><span class="sxs-lookup"><span data-stu-id="5e468-153">Furthermore, because structs are not reference types, it is not possible for values of a struct type to be `null`.</span></span>

<span data-ttu-id="5e468-154">Verilen bildirimi</span><span class="sxs-lookup"><span data-stu-id="5e468-154">Given the declaration</span></span>
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="5e468-155">kod parçası</span><span class="sxs-lookup"><span data-stu-id="5e468-155">the code fragment</span></span>
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
<span data-ttu-id="5e468-156">değeri çıkarır `10`.</span><span class="sxs-lookup"><span data-stu-id="5e468-156">outputs the value `10`.</span></span> <span data-ttu-id="5e468-157">Atamasını `a` için `b` değeri bir kopyasını oluşturur ve `b` böylece atamaya tarafından etkilenmez `a.x`.</span><span class="sxs-lookup"><span data-stu-id="5e468-157">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="5e468-158">Vardı `Point` bunun yerine silinmiş bir sınıf olarak bildirilen, çıktı olacaktır `100` çünkü `a` ve `b` aynı nesneye başvuru.</span><span class="sxs-lookup"><span data-stu-id="5e468-158">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>

### <a name="inheritance"></a><span data-ttu-id="5e468-159">Devralma</span><span class="sxs-lookup"><span data-stu-id="5e468-159">Inheritance</span></span>

<span data-ttu-id="5e468-160">Tüm yapı türleri örtülü olarak sınıftan `System.ValueType`, sırasıyla sınıfından devralan `object`.</span><span class="sxs-lookup"><span data-stu-id="5e468-160">All struct types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="5e468-161">Yapı bildirimi uygulanan arabirimleri listesini belirtebilir, ancak bir taban sınıfı belirtmek bir yapı bildirimi için mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="5e468-161">A struct declaration may specify a list of implemented interfaces, but it is not possible for a struct declaration to specify a base class.</span></span>

<span data-ttu-id="5e468-162">Yapı türleri, hiçbir zaman soyuttur ve her zaman örtük olarak korumalı.</span><span class="sxs-lookup"><span data-stu-id="5e468-162">Struct types are never abstract and are always implicitly sealed.</span></span> <span data-ttu-id="5e468-163">`abstract` Ve `sealed` değiştiriciler bu nedenle izin verilmiyor bir struct bildiriminde.</span><span class="sxs-lookup"><span data-stu-id="5e468-163">The `abstract` and `sealed` modifiers are therefore not permitted in a struct declaration.</span></span>

<span data-ttu-id="5e468-164">Öğesinin bildirilen erişilebilirliği yapı üyesi devralma yapılar için desteklenmiyor. bu yana olamaz `protected` veya `protected internal`.</span><span class="sxs-lookup"><span data-stu-id="5e468-164">Since inheritance isn't supported for structs, the declared accessibility of a struct member cannot be `protected` or `protected internal`.</span></span>

<span data-ttu-id="5e468-165">Bir yapı içinde işlev üyeleri olamaz `abstract` veya `virtual`ve `override` değiştiriciye izin yalnızca devralınan yöntemleri geçersiz kılmak için `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="5e468-165">Function members in a struct cannot be `abstract` or `virtual`, and the `override` modifier is allowed only to override methods inherited from `System.ValueType`.</span></span>

### <a name="assignment"></a><span data-ttu-id="5e468-166">Atama</span><span class="sxs-lookup"><span data-stu-id="5e468-166">Assignment</span></span>

<span data-ttu-id="5e468-167">Bir yapı türü bir değişkene atanması, atanan değerin bir kopyasını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5e468-167">Assignment to a variable of a struct type creates a copy of the value being assigned.</span></span> <span data-ttu-id="5e468-168">Bu başvuru ancak başvuru tarafından tanımlanan nesnesi değil kopyalar bir sınıf türünde bir değişken için bir atama farklıdır.</span><span class="sxs-lookup"><span data-stu-id="5e468-168">This differs from assignment to a variable of a class type, which copies the reference but not the object identified by the reference.</span></span>

<span data-ttu-id="5e468-169">Benzer şekilde bir struct olduğunda veya değer parametre olarak geçen işlevi üyesi bir sonuç döndürdü atama, struct'ın bir kopyası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5e468-169">Similar to an assignment, when a struct is passed as a value parameter or returned as the result of a function member, a copy of the struct is created.</span></span> <span data-ttu-id="5e468-170">Bir yapı kullanarak bir işlev üyeye başvuruya göre geçirilebilir bir `ref` veya `out` parametresi.</span><span class="sxs-lookup"><span data-stu-id="5e468-170">A struct may be passed by reference to a function member using a `ref` or `out` parameter.</span></span>

<span data-ttu-id="5e468-171">Bir özellik veya dizin oluşturucu bir yapının atama hedefi olduğunda özellik veya dizin oluşturucu erişimiyle ilişkili örnek ifade bir değişken olarak sınıflandırılan gerekir.</span><span class="sxs-lookup"><span data-stu-id="5e468-171">When a property or indexer of a struct is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="5e468-172">Örnek ifade bir değer olarak sınıflandırılır, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="5e468-172">If the instance expression is classified as a value, a compile-time error occurs.</span></span> <span data-ttu-id="5e468-173">Bu daha ayrıntılı anlatılan [basit atama](expressions.md#simple-assignment).</span><span class="sxs-lookup"><span data-stu-id="5e468-173">This is described in further detail in [Simple assignment](expressions.md#simple-assignment).</span></span>

### <a name="default-values"></a><span data-ttu-id="5e468-174">Varsayılan değerler</span><span class="sxs-lookup"><span data-stu-id="5e468-174">Default values</span></span>

<span data-ttu-id="5e468-175">Bölümünde anlatıldığı gibi [varsayılan değerler](variables.md#default-values), oluşturulduğunda varsayılan değerlerine çeşitli türlerde değişkenleri otomatik olarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="5e468-175">As described in [Default values](variables.md#default-values), several kinds of variables are automatically initialized to their default value when they are created.</span></span> <span data-ttu-id="5e468-176">Sınıf türleri ve diğer başvuru türlerinin değişkenleri için bu varsayılan değerdir `null`.</span><span class="sxs-lookup"><span data-stu-id="5e468-176">For variables of class types and other reference types, this default value is `null`.</span></span> <span data-ttu-id="5e468-177">Ancak, yapılar olamaz değer türleri olduğundan `null`, bir yapı varsayılan değer tür alanları için tüm değer tür alanları varsayılan değerlerine ve tüm başvuru ayarlayarak üretilen değeridir `null`.</span><span class="sxs-lookup"><span data-stu-id="5e468-177">However, since structs are value types that cannot be `null`, the default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="5e468-178">Başvuran `Point` yapısı, yukarıdaki örnekte bildirilen</span><span class="sxs-lookup"><span data-stu-id="5e468-178">Referring to the `Point` struct declared above, the example</span></span>
```csharp
Point[] a = new Point[100];
```
<span data-ttu-id="5e468-179">Her başlatır `Point` değerine ayarlayarak üretilen dizideki `x` ve `y` sıfıra alanları.</span><span class="sxs-lookup"><span data-stu-id="5e468-179">initializes each `Point` in the array to the value produced by setting the `x` and `y` fields to zero.</span></span>

<span data-ttu-id="5e468-180">Bir yapı varsayılan değerini struct'ın varsayılan oluşturucu tarafından döndürülen değer karşılık gelir ([varsayılan oluşturucular](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="5e468-180">The default value of a struct corresponds to the value returned by the default constructor of the struct ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="5e468-181">Bir sınıf, yapı, parametresiz örnek oluşturucusu bildirmek için izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="5e468-181">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="5e468-182">Bunun yerine, her yapı her zaman tüm değer tür alanları varsayılan değerlerine ve tüm başvuru türü alanlarını ayarlama özelliğinden sonucunda değeri döndüren bir parametresiz örnek oluşturucusu örtük olarak sahip `null`.</span><span class="sxs-lookup"><span data-stu-id="5e468-182">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="5e468-183">Yapılar varsayılan başlatma durumu geçerli bir durum düşünün şekilde tasarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5e468-183">Structs should be designed to consider the default initialization state a valid state.</span></span> <span data-ttu-id="5e468-184">Örnekte</span><span class="sxs-lookup"><span data-stu-id="5e468-184">In the example</span></span>
```csharp
using System;

struct KeyValuePair
{
    string key;
    string value;

    public KeyValuePair(string key, string value) {
        if (key == null || value == null) throw new ArgumentException();
        this.key = key;
        this.value = value;
    }
}
```
<span data-ttu-id="5e468-185">örnek kullanıcı tanımlı oluşturucusu yalnızca burada açıkça çağrıldığı null değerler karşı korur.</span><span class="sxs-lookup"><span data-stu-id="5e468-185">the user-defined instance constructor protects against null values only where it is explicitly called.</span></span> <span data-ttu-id="5e468-186">Durumlarda burada bir `KeyValuePair` değişkeni ise varsayılan değer başlatma tabi `key` ve `value` alanları null olacaktır ve struct bu durumunu işlemek için hazırlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="5e468-186">In cases where a `KeyValuePair` variable is subject to default value initialization, the `key` and `value` fields will be null, and the struct must be prepared to handle this state.</span></span>

### <a name="boxing-and-unboxing"></a><span data-ttu-id="5e468-187">Kutulama ve kutudan çıkarma</span><span class="sxs-lookup"><span data-stu-id="5e468-187">Boxing and unboxing</span></span>

<span data-ttu-id="5e468-188">Bir sınıf türünde bir değer türüne dönüştürülebilir `object` veya başka bir türü derleme zamanında yalnızca başvuru düşünerek sınıfı tarafından uygulanan bir arabirim türü.</span><span class="sxs-lookup"><span data-stu-id="5e468-188">A value of a class type can be converted to type `object` or to an interface type that is implemented by the class simply by treating the reference as another type at compile-time.</span></span> <span data-ttu-id="5e468-189">Benzer şekilde, bir değer türü `object` veya bir arabirim türü değeri başvurusu değiştirmeden bir sınıf türüne dönüştürülebilir (ancak Elbette bir çalışma zamanı tür denetimi bu durumda gereklidir).</span><span class="sxs-lookup"><span data-stu-id="5e468-189">Likewise, a value of type `object` or a value of an interface type can be converted back to a class type without changing the reference (but of course a run-time type check is required in this case).</span></span>

<span data-ttu-id="5e468-190">Yapılar başvuru türleri olmadığından, bu işlemleri farklı yapı türleri için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="5e468-190">Since structs are not reference types, these operations are implemented differently for struct types.</span></span> <span data-ttu-id="5e468-191">Ne zaman bir yapı türünün bir değer türüne dönüştürülür `object` veya struct tarafından uygulanan bir arabirim türü için bir paketleme işlemi gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="5e468-191">When a value of a struct type is converted to type `object` or to an interface type that is implemented by the struct, a boxing operation takes place.</span></span> <span data-ttu-id="5e468-192">Benzer şekilde, bir değer türü `object` veya bir yapı türünü bir arabirim türü değeri dönüştürülür, bir kutudan çıkarma işlemi gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="5e468-192">Likewise, when a value of type `object` or a value of an interface type is converted back to a struct type, an unboxing operation takes place.</span></span> <span data-ttu-id="5e468-193">Sınıf türleri aynı işlemleri önemli bir fark, kutulama ve kutudan çıkarma yapı değeri veya paketlenmiş örneği dışından kopyalar ' dir.</span><span class="sxs-lookup"><span data-stu-id="5e468-193">A key difference from the same operations on class types is that boxing and unboxing copies the struct value either into or out of the boxed instance.</span></span> <span data-ttu-id="5e468-194">Bu nedenle, kutulama veya kutudan çıkarma işlemi, kutulanmış struct kutulanmamış struct için yapılan değişiklikler yansıtılmaz.</span><span class="sxs-lookup"><span data-stu-id="5e468-194">Thus, following a boxing or unboxing operation, changes made to the unboxed struct are not reflected in the boxed struct.</span></span>

<span data-ttu-id="5e468-195">Bir yapı türü devralınan sanal bir yöntem olduğunda geçersiz kılmalar `System.Object` (gibi `Equals`, `GetHashCode`, veya `ToString`), yapı türünün bir örneği üzerinden sanal yöntemi çağırmayı kutulama oluşmasına neden olmaz.</span><span class="sxs-lookup"><span data-stu-id="5e468-195">When a struct type overrides a virtual method inherited from `System.Object` (such as `Equals`, `GetHashCode`, or `ToString`), invocation of the virtual method through an instance of the struct type does not cause boxing to occur.</span></span> <span data-ttu-id="5e468-196">Bu yapı türü parametre olarak kullanılır ve çağırma parametre türü bir örneği üzerinden gerçekleşir bile geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="5e468-196">This is true even when the struct is used as a type parameter and the invocation occurs through an instance of the type parameter type.</span></span> <span data-ttu-id="5e468-197">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5e468-197">For example:</span></span>
```csharp
using System;

struct Counter
{
    int value;

    public override string ToString() {
        value++;
        return value.ToString();
    }
}

class Program
{
    static void Test<T>() where T: new() {
        T x = new T();
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
    }

    static void Main() {
        Test<Counter>();
    }
}
```

<span data-ttu-id="5e468-198">Program çıktısı şöyledir:</span><span class="sxs-lookup"><span data-stu-id="5e468-198">The output of the program is:</span></span>
```
1
2
3
```

<span data-ttu-id="5e468-199">Hatalı stili olmasına rağmen `ToString` yan etkileri olduğu için örnek hiçbir kutulama için üç çağrılarını oluştuğunu gösterir `x.ToString()`.</span><span class="sxs-lookup"><span data-stu-id="5e468-199">Although it is bad style for `ToString` to have side effects, the example demonstrates that no boxing occurred for the three invocations of `x.ToString()`.</span></span>

<span data-ttu-id="5e468-200">Benzer şekilde, hiçbir zaman örtük olarak kutulama kısıtlanmış bir tür parametresi üzerindeki bir üyeye erişilmesi meydana gelir.</span><span class="sxs-lookup"><span data-stu-id="5e468-200">Similarly, boxing never implicitly occurs when accessing a member on a constrained type parameter.</span></span> <span data-ttu-id="5e468-201">Örneğin, bir arabirim varsayalım `ICounter` bir yöntem içerir `Increment` değeri değiştirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e468-201">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="5e468-202">Varsa `ICounter` yürütmesinin kısıtlama olarak kullanılan `Increment` değişken başvuru ile yöntemi çağrıldığında, `Increment` paketlenmiş bir kopyasını, hiçbir zaman çağrıldı.</span><span class="sxs-lookup"><span data-stu-id="5e468-202">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, never a boxed copy.</span></span>

```csharp
using System;

interface ICounter
{
    void Increment();
}

struct Counter: ICounter
{
    int value;

    public override string ToString() {
        return value.ToString();
    }

    void ICounter.Increment() {
        value++;
    }
}

class Program
{
    static void Test<T>() where T: ICounter, new() {
        T x = new T();
        Console.WriteLine(x);
        x.Increment();                    // Modify x
        Console.WriteLine(x);
        ((ICounter)x).Increment();        // Modify boxed copy of x
        Console.WriteLine(x);
    }

    static void Main() {
        Test<Counter>();
    }
}
```

<span data-ttu-id="5e468-203">İlk çağrıda `Increment` değişken içindeki değeri değiştirir `x`.</span><span class="sxs-lookup"><span data-stu-id="5e468-203">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="5e468-204">Bu ikinci çağrının eşdeğer değildir `Increment`, paketlenmiş bir kopyasını değeri değiştirir `x`.</span><span class="sxs-lookup"><span data-stu-id="5e468-204">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="5e468-205">Bu nedenle, program çıktısı şöyledir:</span><span class="sxs-lookup"><span data-stu-id="5e468-205">Thus, the output of the program is:</span></span>
```
0
1
1
```

<span data-ttu-id="5e468-206">Kutulama ve kutudan çıkarma hakkında daha fazla bilgi için bkz. [kutulama ve kutudan çıkarma](types.md#boxing-and-unboxing).</span><span class="sxs-lookup"><span data-stu-id="5e468-206">For further details on boxing and unboxing, see [Boxing and unboxing](types.md#boxing-and-unboxing).</span></span>

### <a name="meaning-of-this"></a><span data-ttu-id="5e468-207">Bunun anlamı</span><span class="sxs-lookup"><span data-stu-id="5e468-207">Meaning of this</span></span>

<span data-ttu-id="5e468-208">Bir örnek oluşturucusu veya örnek işlevi bir sınıf üyesi `this` bir değer olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="5e468-208">Within an instance constructor or instance function member of a class, `this` is classified as a value.</span></span> <span data-ttu-id="5e468-209">Bu nedenle, while `this` örneğine başvurmak için kullanılabilir olan işlevi üyenin çağrıldığı için atamak mümkün değildir `this` bir sınıf üyesi işlevi olarak.</span><span class="sxs-lookup"><span data-stu-id="5e468-209">Thus, while `this` can be used to refer to the instance for which the function member was invoked, it is not possible to assign to `this` in a function member of a class.</span></span>

<span data-ttu-id="5e468-210">Bir örnek oluşturucusunda bir yapı içinde `this` karşılık gelen bir `out` parametresi yapı türünün ve örnek işlev üyesi bir yapı içinde `this` karşılık gelen bir `ref` yapı türünde parametre.</span><span class="sxs-lookup"><span data-stu-id="5e468-210">Within an instance constructor of a struct, `this` corresponds to an `out` parameter of the struct type, and within an instance function member of a struct, `this` corresponds to a `ref` parameter of the struct type.</span></span> <span data-ttu-id="5e468-211">Her iki durumda da `this` bir değişken olarak sınıflandırılır ve kendisi için işlev üyenin çağrıldığı atayarak yapının tamamını değiştirmek mümkündür `this` veya olarak geçirerek bir `ref` veya `out` parametresi.</span><span class="sxs-lookup"><span data-stu-id="5e468-211">In both cases, `this` is classified as a variable, and it is possible to modify the entire struct for which the function member was invoked by assigning to `this` or by passing this as a `ref` or `out` parameter.</span></span>

### <a name="field-initializers"></a><span data-ttu-id="5e468-212">Alan başlatıcıları</span><span class="sxs-lookup"><span data-stu-id="5e468-212">Field initializers</span></span>

<span data-ttu-id="5e468-213">Bölümünde anlatıldığı gibi [varsayılan değerler](structs.md#default-values), bir yapı varsayılan değer tür alanları için tüm değer tür alanları varsayılan değerlerine ve tüm başvuru ayarından sonucunda değeri oluşur `null`.</span><span class="sxs-lookup"><span data-stu-id="5e468-213">As described in [Default values](structs.md#default-values), the default value of a struct consists of the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span> <span data-ttu-id="5e468-214">Bu nedenle, bir yapı değişken başlatıcılar eklemek için örnek alanı bildirimleri izin vermez.</span><span class="sxs-lookup"><span data-stu-id="5e468-214">For this reason, a struct does not permit instance field declarations to include variable initializers.</span></span> <span data-ttu-id="5e468-215">Bu kısıtlama yalnızca örnek alanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="5e468-215">This restriction applies only to instance fields.</span></span> <span data-ttu-id="5e468-216">Bir yapının statik alanlar, değişken başlatıcılar içerecek şekilde izin verilir.</span><span class="sxs-lookup"><span data-stu-id="5e468-216">Static fields of a struct are permitted to include variable initializers.</span></span>

<span data-ttu-id="5e468-217">Örnek</span><span class="sxs-lookup"><span data-stu-id="5e468-217">The example</span></span>
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
<span data-ttu-id="5e468-218">örnek alanı bildirimleri değişken başlatıcılar içerdiğinden hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="5e468-218">is in error because the instance field declarations include variable initializers.</span></span>

### <a name="constructors"></a><span data-ttu-id="5e468-219">Oluşturucular</span><span class="sxs-lookup"><span data-stu-id="5e468-219">Constructors</span></span>

<span data-ttu-id="5e468-220">Bir sınıf, yapı, parametresiz örnek oluşturucusu bildirmek için izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="5e468-220">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="5e468-221">Bunun yerine, her yapı her zaman null tür alanları tüm değer tür alanları varsayılan değerlerine ve tüm başvuru ayarından sonucunda değeri döndüren bir parametresiz örnek oluşturucusu örtük olarak sahip ([varsayılan oluşturucular](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="5e468-221">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to null ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="5e468-222">Örnek oluşturucuları parametreleri olan bir yapı bildirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e468-222">A struct can declare instance constructors having parameters.</span></span> <span data-ttu-id="5e468-223">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5e468-223">For example</span></span>
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

<span data-ttu-id="5e468-224">Yukarıdaki bildirim, deyimleri verilen</span><span class="sxs-lookup"><span data-stu-id="5e468-224">Given the above declaration, the statements</span></span>
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
<span data-ttu-id="5e468-225">her ikisini de oluşturma bir `Point` ile `x` ve `y` sıfır olarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="5e468-225">both create a `Point` with `x` and `y` initialized to zero.</span></span>

<span data-ttu-id="5e468-226">Bir yapı örnek oluşturucusu biçiminde bir oluşturucu başlatıcı eklemek için izin verilmiyor `base(...)`.</span><span class="sxs-lookup"><span data-stu-id="5e468-226">A struct instance constructor is not permitted to include a constructor initializer of the form `base(...)`.</span></span>

<span data-ttu-id="5e468-227">Yapı örnek oluşturucusu bir oluşturucu başlatıcı belirtmiyorsa `this` değişken karşılık gelen bir `out` parametresi yapı türünün ve benzer bir `out` parametresi `this` kesinlikle atanmalıdır ( [Belirli atama onayına](variables.md#definite-assignment)) Oluşturucu döndüğü her konumda.</span><span class="sxs-lookup"><span data-stu-id="5e468-227">If the struct instance constructor doesn't specify a constructor initializer, the `this` variable corresponds to an `out` parameter of the struct type, and similar to an `out` parameter, `this` must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at every location where the constructor returns.</span></span> <span data-ttu-id="5e468-228">Yapı örnek oluşturucusu bir oluşturucu başlatıcı belirtiyorsa `this` değişken karşılık gelen bir `ref` parametresi yapı türünün ve benzer bir `ref` parametresi `this` atanan kesin olarak kabul edilir Oluşturucu body girişi.</span><span class="sxs-lookup"><span data-stu-id="5e468-228">If the struct instance constructor specifies a constructor initializer, the `this` variable corresponds to a `ref` parameter of the struct type, and similar to a `ref` parameter, `this` is considered definitely assigned on entry to the constructor body.</span></span> <span data-ttu-id="5e468-229">Aşağıdaki örnek oluşturucusu uygulaması göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5e468-229">Consider the instance constructor implementation below:</span></span>
```csharp
struct Point
{
    int x, y;

    public int X {
        set { x = value; }
    }

    public int Y {
        set { y = value; }
    }

    public Point(int x, int y) {
        X = x;        // error, this is not yet definitely assigned
        Y = y;        // error, this is not yet definitely assigned
    }
}
```

<span data-ttu-id="5e468-230">Hiçbir örnek üyesi işlevini (set erişimcisine özellikleri dahil olmak üzere `X` ve `Y`) yapılandırılmakta yapının tüm alanlarını kesinlikle atanmış kadar çağrılamaz.</span><span class="sxs-lookup"><span data-stu-id="5e468-230">No instance member function (including the set accessors for the properties `X` and `Y`) can be called until all fields of the struct being constructed have been definitely assigned.</span></span> <span data-ttu-id="5e468-231">Tek özel durum otomatik olarak uygulanan özellikler içerir ([Özellikleri'otomatik olarak uygulanan](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="5e468-231">The only exception involves automatically implemented properties ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="5e468-232">Belirli atama onayına kuralları ([basit atama ifadeleri](variables.md#simple-assignment-expressions)) özellikle atama otomatik özellik için bir yapı türünün bir örneği oluşturucu içinde yapı türü muaf: Böyle bir atama bir kesin olarak kabul edilir Otomatik-özellik gizli yedekleme alanının atama.</span><span class="sxs-lookup"><span data-stu-id="5e468-232">The definite assignment rules ([Simple assignment expressions](variables.md#simple-assignment-expressions)) specifically exempt assignment to an auto-property of a struct type within an instance constructor of that struct type: such an assignment is considered a definite assignment of the hidden backing field of the auto-property.</span></span> <span data-ttu-id="5e468-233">Bu nedenle, aşağıdaki izin verilir:</span><span class="sxs-lookup"><span data-stu-id="5e468-233">Thus, the following is allowed:</span></span>

```csharp
struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y) {
        X = x;      // allowed, definitely assigns backing field
        Y = y;      // allowed, definitely assigns backing field
    }
```

### <a name="destructors"></a><span data-ttu-id="5e468-234">Yıkıcılar</span><span class="sxs-lookup"><span data-stu-id="5e468-234">Destructors</span></span>

<span data-ttu-id="5e468-235">Bir yapının bir yıkıcı bildirmek için izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="5e468-235">A struct is not permitted to declare a destructor.</span></span>

### <a name="static-constructors"></a><span data-ttu-id="5e468-236">Statik oluşturucular</span><span class="sxs-lookup"><span data-stu-id="5e468-236">Static constructors</span></span>

<span data-ttu-id="5e468-237">Yapılar için statik oluşturucular sınıfları olduğu gibi aynı kurallara çoğunu izleyin.</span><span class="sxs-lookup"><span data-stu-id="5e468-237">Static constructors for structs follow most of the same rules as for classes.</span></span> <span data-ttu-id="5e468-238">Bir yapı türü için bir statik Oluşturucu yürütülmesi, bir uygulama etki alanı içinde gerçekleşmesi için ilk aşağıdaki olaylardan biri tarafından tetiklenir:</span><span class="sxs-lookup"><span data-stu-id="5e468-238">The execution of a static constructor for a struct type is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="5e468-239">Yapı türünün statik üyesi başvuruluyor.</span><span class="sxs-lookup"><span data-stu-id="5e468-239">A static member of the struct type is referenced.</span></span>
*  <span data-ttu-id="5e468-240">Yapı türünün açıkça bildirilen bir oluşturucu çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5e468-240">An explicitly declared constructor of the struct type is called.</span></span>

<span data-ttu-id="5e468-241">Varsayılan değerleri oluşturulmasını ([varsayılan değerler](structs.md#default-values)) struct'ın türleri statik Oluşturucu tetiklemez.</span><span class="sxs-lookup"><span data-stu-id="5e468-241">The creation of default values ([Default values](structs.md#default-values)) of struct types does not trigger the static constructor.</span></span> <span data-ttu-id="5e468-242">(Buna örnek olarak bir dizideki öğelerin ilk değeri var.)</span><span class="sxs-lookup"><span data-stu-id="5e468-242">(An example of this is the initial value of elements in an array.)</span></span>

## <a name="struct-examples"></a><span data-ttu-id="5e468-243">Yapı örnekleri</span><span class="sxs-lookup"><span data-stu-id="5e468-243">Struct examples</span></span>

<span data-ttu-id="5e468-244">Aşağıdakileri kullanarak iki önemli örnek gösterir `struct` dilinin ancak değiştirilen semantiğine sahip önceden tanımlanmış türlerine benzer şekilde kullanılabilir türleri oluşturma türleri.</span><span class="sxs-lookup"><span data-stu-id="5e468-244">The following shows two significant examples of using `struct` types to create types that can be used similarly to the predefined types of the language, but with modified semantics.</span></span>

### <a name="database-integer-type"></a><span data-ttu-id="5e468-245">Veritabanı tamsayı türü</span><span class="sxs-lookup"><span data-stu-id="5e468-245">Database integer type</span></span>

<span data-ttu-id="5e468-246">`DBInt` Yapısı aşağıdaki değerleri tam kümesini temsil edebilen bir tamsayı türü uygulayan `int` türü yanı sıra, bilinmeyen bir değere gösteren bir ek durumu.</span><span class="sxs-lookup"><span data-stu-id="5e468-246">The `DBInt` struct below implements an integer type that can represent the complete set of values of the `int` type, plus an additional state that indicates an unknown value.</span></span> <span data-ttu-id="5e468-247">Bu özelliklere sahip bir tür veritabanlarında yaygın olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5e468-247">A type with these characteristics is commonly used in databases.</span></span>

```csharp
using System;

public struct DBInt
{
    // The Null member represents an unknown DBInt value.

    public static readonly DBInt Null = new DBInt();

    // When the defined field is true, this DBInt represents a known value
    // which is stored in the value field. When the defined field is false,
    // this DBInt represents an unknown value, and the value field is 0.

    int value;
    bool defined;

    // Private instance constructor. Creates a DBInt with a known value.

    DBInt(int value) {
        this.value = value;
        this.defined = true;
    }

    // The IsNull property is true if this DBInt represents an unknown value.

    public bool IsNull { get { return !defined; } }

    // The Value property is the known value of this DBInt, or 0 if this
    // DBInt represents an unknown value.

    public int Value { get { return value; } }

    // Implicit conversion from int to DBInt.

    public static implicit operator DBInt(int x) {
        return new DBInt(x);
    }

    // Explicit conversion from DBInt to int. Throws an exception if the
    // given DBInt represents an unknown value.

    public static explicit operator int(DBInt x) {
        if (!x.defined) throw new InvalidOperationException();
        return x.value;
    }

    public static DBInt operator +(DBInt x) {
        return x;
    }

    public static DBInt operator -(DBInt x) {
        return x.defined ? -x.value : Null;
    }

    public static DBInt operator +(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value + y.value: Null;
    }

    public static DBInt operator -(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value - y.value: Null;
    }

    public static DBInt operator *(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value * y.value: Null;
    }

    public static DBInt operator /(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value / y.value: Null;
    }

    public static DBInt operator %(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value % y.value: Null;
    }

    public static DBBool operator ==(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value == y.value: DBBool.Null;
    }

    public static DBBool operator !=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value != y.value: DBBool.Null;
    }

    public static DBBool operator >(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value > y.value: DBBool.Null;
    }

    public static DBBool operator <(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value < y.value: DBBool.Null;
    }

    public static DBBool operator >=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value >= y.value: DBBool.Null;
    }

    public static DBBool operator <=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value <= y.value: DBBool.Null;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBInt)) return false;
        DBInt x = (DBInt)obj;
        return value == x.value && defined == x.defined;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        return defined? value.ToString(): "DBInt.Null";
    }
}
```

### <a name="database-boolean-type"></a><span data-ttu-id="5e468-248">Veritabanı Boole türü</span><span class="sxs-lookup"><span data-stu-id="5e468-248">Database boolean type</span></span>

<span data-ttu-id="5e468-249">`DBBool` Yapısı aşağıdaki üç değerli mantıksal türü uygular.</span><span class="sxs-lookup"><span data-stu-id="5e468-249">The `DBBool` struct below implements a three-valued logical type.</span></span> <span data-ttu-id="5e468-250">Bu tür olası değerler şunlardır: `DBBool.True`, `DBBool.False`, ve `DBBool.Null`burada `Null` üye bilinmeyen bir değere gösterir.</span><span class="sxs-lookup"><span data-stu-id="5e468-250">The possible values of this type are `DBBool.True`, `DBBool.False`, and `DBBool.Null`, where the `Null` member indicates an unknown value.</span></span> <span data-ttu-id="5e468-251">Bu üç değerli mantıksal türler veritabanlarında yaygın olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5e468-251">Such three-valued logical types are commonly used in databases.</span></span>

```csharp
using System;

public struct DBBool
{
    // The three possible DBBool values.

    public static readonly DBBool Null = new DBBool(0);
    public static readonly DBBool False = new DBBool(-1);
    public static readonly DBBool True = new DBBool(1);

    // Private field that stores -1, 0, 1 for False, Null, True.

    sbyte value;

    // Private instance constructor. The value parameter must be -1, 0, or 1.

    DBBool(int value) {
        this.value = (sbyte)value;
    }

    // Properties to examine the value of a DBBool. Return true if this
    // DBBool has the given value, false otherwise.

    public bool IsNull { get { return value == 0; } }

    public bool IsFalse { get { return value < 0; } }

    public bool IsTrue { get { return value > 0; } }

    // Implicit conversion from bool to DBBool. Maps true to DBBool.True and
    // false to DBBool.False.

    public static implicit operator DBBool(bool x) {
        return x? True: False;
    }

    // Explicit conversion from DBBool to bool. Throws an exception if the
    // given DBBool is Null, otherwise returns true or false.

    public static explicit operator bool(DBBool x) {
        if (x.value == 0) throw new InvalidOperationException();
        return x.value > 0;
    }

    // Equality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator ==(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value == y.value? True: False;
    }

    // Inequality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator !=(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value != y.value? True: False;
    }

    // Logical negation operator. Returns True if the operand is False, Null
    // if the operand is Null, or False if the operand is True.

    public static DBBool operator !(DBBool x) {
        return new DBBool(-x.value);
    }

    // Logical AND operator. Returns False if either operand is False,
    // otherwise Null if either operand is Null, otherwise True.

    public static DBBool operator &(DBBool x, DBBool y) {
        return new DBBool(x.value < y.value? x.value: y.value);
    }

    // Logical OR operator. Returns True if either operand is True, otherwise
    // Null if either operand is Null, otherwise False.

    public static DBBool operator |(DBBool x, DBBool y) {
        return new DBBool(x.value > y.value? x.value: y.value);
    }

    // Definitely true operator. Returns true if the operand is True, false
    // otherwise.

    public static bool operator true(DBBool x) {
        return x.value > 0;
    }

    // Definitely false operator. Returns true if the operand is False, false
    // otherwise.

    public static bool operator false(DBBool x) {
        return x.value < 0;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBBool)) return false;
        return value == ((DBBool)obj).value;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        if (value > 0) return "DBBool.True";
        if (value < 0) return "DBBool.False";
        return "DBBool.Null";
    }
}
```
