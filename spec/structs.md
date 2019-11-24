---
ms.openlocfilehash: 6dd1dde67597b2125de9a1aa2fab9144128d533f
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704037"
---
# <a name="structs"></a><span data-ttu-id="da82c-101">Yapılar</span><span class="sxs-lookup"><span data-stu-id="da82c-101">Structs</span></span>

<span data-ttu-id="da82c-102">Yapıları, veri üyeleri ve işlev üyeleri içerebilen veri yapılarını temsil ettikleri sınıflara benzerdir.</span><span class="sxs-lookup"><span data-stu-id="da82c-102">Structs are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="da82c-103">Ancak, sınıfların aksine yapılar değer türlerdir ve yığın ayırmayı gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="da82c-103">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="da82c-104">Yapı türünün bir değişkeni doğrudan yapı verilerini içerir, ancak bir sınıf türünün değişkeni, ikinci olarak bir nesne olarak bilinen veri başvurusunu içerir.</span><span class="sxs-lookup"><span data-stu-id="da82c-104">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span>

<span data-ttu-id="da82c-105">Yapılar, özellikle değer semantiği olan küçük veri yapıları için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="da82c-105">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="da82c-106">Karmaşık sayılar, bir koordinat sistemindeki işaret veya bir sözlükte anahtar-değer çiftleri, yapı birimlerinin iyi örnekleridir.</span><span class="sxs-lookup"><span data-stu-id="da82c-106">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="da82c-107">Bu veri yapılarına yönelik anahtar, çok sayıda veri üyesine sahip olduğundan, devralma veya başvuru kimliği kullanımını gerektirmediğinden ve atamanın başvuru yerine değeri kopyalayan değer semantiğinin kullanılmasıyla kolayca uygulanabilirler.</span><span class="sxs-lookup"><span data-stu-id="da82c-107">Key to these data structures is that they have few data members, that they do not require use of inheritance or referential identity, and that they can be conveniently implemented using value semantics where assignment copies the value instead of the reference.</span></span>

<span data-ttu-id="da82c-108">[Basit türlerde](types.md#simple-types)açıklandığı gibi, tarafından C#sunulan `int`, `double`ve `bool`gibi basit türler aslında tüm yapı türleridir.</span><span class="sxs-lookup"><span data-stu-id="da82c-108">As described in [Simple types](types.md#simple-types), the simple types provided by C#, such as `int`, `double`, and `bool`, are in fact all struct types.</span></span> <span data-ttu-id="da82c-109">Bu öntanımlı türlerin yapıları olduğu gibi, C# dilde yeni "temel" türler uygulamak için yapılar ve işleç aşırı yüklemesi de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="da82c-109">Just as these predefined types are structs, it is also possible to use structs and operator overloading to implement new "primitive" types in the C# language.</span></span> <span data-ttu-id="da82c-110">Bu bölümün sonunda bu türden iki örnek verilmiştir ([struct örnekleri](structs.md#struct-examples)).</span><span class="sxs-lookup"><span data-stu-id="da82c-110">Two examples of such types are given at the end of this chapter ([Struct examples](structs.md#struct-examples)).</span></span>

## <a name="struct-declarations"></a><span data-ttu-id="da82c-111">Struct bildirimleri</span><span class="sxs-lookup"><span data-stu-id="da82c-111">Struct declarations</span></span>

<span data-ttu-id="da82c-112">*Struct_declaration* , yeni bir yapı bildiren bir *type_declaration* ([tür bildirimleri](namespaces.md#type-declarations)):</span><span class="sxs-lookup"><span data-stu-id="da82c-112">A *struct_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new struct:</span></span>

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

<span data-ttu-id="da82c-113">*Struct_declaration* , isteğe bağlı bir dizi *öznitelik* ([öznitelik](attributes.md) *struct_modifier*) ve ardından isteğe bağlı bir `partial` değiştiricisi ve ardından anahtar `struct`[](structs.md#struct-modifiers)sözcüğünün ardından isteğe bağlı bir *type_parameter_list* belirtimi (tür parametreleri) ve *ardından isteğe bağlı* bir Struct_interfaces belirtimi ([tür parametreleri](classes.md#type-parameters)) ve ardından isteğe bağlı bir belirtimi (kısmi değiştirici) içerir.[ ](structs.md#partial-modifier)) ve ardından isteğe bağlı bir *type_parameter_constraints_clause*s belirtimi ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) ve ardından bir *struct_body* ([struct Body](structs.md#struct-body)), isteğe bağlı olarak noktalı virgül gelir.</span><span class="sxs-lookup"><span data-stu-id="da82c-113">A *struct_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *struct_modifier*s ([Struct modifiers](structs.md#struct-modifiers)), followed by an optional `partial` modifier, followed by the keyword `struct` and an *identifier* that names the struct, followed by an optional *type_parameter_list* specification ([Type parameters](classes.md#type-parameters)), followed by an optional *struct_interfaces* specification ([Partial modifier](structs.md#partial-modifier)) ), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *struct_body* ([Struct body](structs.md#struct-body)), optionally followed by a semicolon.</span></span>

### <a name="struct-modifiers"></a><span data-ttu-id="da82c-114">Yapı değiştiricileri</span><span class="sxs-lookup"><span data-stu-id="da82c-114">Struct modifiers</span></span>

<span data-ttu-id="da82c-115">*Struct_declaration* , isteğe bağlı olarak bir yapı değiştiricileri dizisi içerebilir:</span><span class="sxs-lookup"><span data-stu-id="da82c-115">A *struct_declaration* may optionally include a sequence of struct modifiers:</span></span>

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

<span data-ttu-id="da82c-116">Aynı değiştiricinin bir struct bildiriminde birden çok kez görünmesi için derleme zamanı hatası vardır.</span><span class="sxs-lookup"><span data-stu-id="da82c-116">It is a compile-time error for the same modifier to appear multiple times in a struct declaration.</span></span>

<span data-ttu-id="da82c-117">Bir struct bildiriminin değiştiricileri, bir sınıf bildirimiyle ([sınıf bildirimleri](classes.md#class-declarations)) aynı anlama sahiptir.</span><span class="sxs-lookup"><span data-stu-id="da82c-117">The modifiers of a struct declaration have the same meaning as those of a class declaration ([Class declarations](classes.md#class-declarations)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="da82c-118">Kısmi değiştirici</span><span class="sxs-lookup"><span data-stu-id="da82c-118">Partial modifier</span></span>

<span data-ttu-id="da82c-119">`partial` değiştirici, bu *struct_declaration* kısmi bir tür bildirimi olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="da82c-119">The `partial` modifier indicates that this *struct_declaration* is a partial type declaration.</span></span> <span data-ttu-id="da82c-120">Bir kapsayan ad alanı veya tür bildiriminde aynı ada sahip birden çok kısmi struct bildirimi, [kısmi türlerde](classes.md#partial-types)belirtilen kurallara göre tek bir struct bildirimi oluşturmak için birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="da82c-120">Multiple partial struct declarations with the same name within an enclosing namespace or type declaration combine to form one struct declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="struct-interfaces"></a><span data-ttu-id="da82c-121">Struct arabirimleri</span><span class="sxs-lookup"><span data-stu-id="da82c-121">Struct interfaces</span></span>

<span data-ttu-id="da82c-122">Yapı bildirimi *struct_interfaces* bir belirtim içerebilir ve bu durumda yapının verilen arabirim türlerini doğrudan uygulaması söylenemez.</span><span class="sxs-lookup"><span data-stu-id="da82c-122">A struct declaration may include a *struct_interfaces* specification, in which case the struct is said to directly implement the given interface types.</span></span>

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

<span data-ttu-id="da82c-123">Arabirim Uygulamaları, [arabirim uygulamalarında](interfaces.md#interface-implementations)daha ayrıntılı bir şekilde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="da82c-123">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="struct-body"></a><span data-ttu-id="da82c-124">Yapı gövdesi</span><span class="sxs-lookup"><span data-stu-id="da82c-124">Struct body</span></span>

<span data-ttu-id="da82c-125">Bir yapının *struct_body* yapının üyelerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="da82c-125">The *struct_body* of a struct defines the members of the struct.</span></span>

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a><span data-ttu-id="da82c-126">Yapı üyeleri</span><span class="sxs-lookup"><span data-stu-id="da82c-126">Struct members</span></span>

<span data-ttu-id="da82c-127">Bir yapının üyeleri, *struct_member_declaration*s tarafından tanıtılan üyelerden ve `System.ValueType`türünden devralınan üyelere oluşur.</span><span class="sxs-lookup"><span data-stu-id="da82c-127">The members of a struct consist of the members introduced by its *struct_member_declaration*s and the members inherited from the type `System.ValueType`.</span></span>

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

<span data-ttu-id="da82c-128">[Sınıf ve yapı farklılıklarında](structs.md#class-and-struct-differences)belirtilen farklar dışında, [sınıf üyeleri için](classes.md#class-members) [yineleyiciler](classes.md#iterators) aracılığıyla sunulan sınıf üyelerinin açıklamaları da yapı üyelerine uygulanır.</span><span class="sxs-lookup"><span data-stu-id="da82c-128">Except for the differences noted in [Class and struct differences](structs.md#class-and-struct-differences), the descriptions of class members provided in [Class members](classes.md#class-members) through [Iterators](classes.md#iterators) apply to struct members as well.</span></span>

## <a name="class-and-struct-differences"></a><span data-ttu-id="da82c-129">Sınıf ve yapı farkları</span><span class="sxs-lookup"><span data-stu-id="da82c-129">Class and struct differences</span></span>

<span data-ttu-id="da82c-130">Yapılar çeşitli önemli yollarla sınıflardan farklıdır:</span><span class="sxs-lookup"><span data-stu-id="da82c-130">Structs differ from classes in several important ways:</span></span>

*  <span data-ttu-id="da82c-131">Yapılar değer türlerdir ([değer semantiği](structs.md#value-semantics)).</span><span class="sxs-lookup"><span data-stu-id="da82c-131">Structs are value types ([Value semantics](structs.md#value-semantics)).</span></span>
*  <span data-ttu-id="da82c-132">Tüm yapı türleri dolaylı olarak `System.ValueType` ([Devralma](structs.md#inheritance)) sınıfından devralınır.</span><span class="sxs-lookup"><span data-stu-id="da82c-132">All struct types implicitly inherit from the class `System.ValueType` ([Inheritance](structs.md#inheritance)).</span></span>
*  <span data-ttu-id="da82c-133">Struct türündeki bir değişkene atama, atanmakta olan değerin ([atama](structs.md#assignment)) bir kopyasını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="da82c-133">Assignment to a variable of a struct type creates a copy of the value being assigned ([Assignment](structs.md#assignment)).</span></span>
*  <span data-ttu-id="da82c-134">Bir yapının varsayılan değeri, tüm değer türü alanları varsayılan değerlerine ve tüm başvuru türü alanlarına `null` ([varsayılan değerler](structs.md#default-values)) olarak ayarlanarak oluşturulan değerdir.</span><span class="sxs-lookup"><span data-stu-id="da82c-134">The default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null` ([Default values](structs.md#default-values)).</span></span>
*  <span data-ttu-id="da82c-135">Kutulama ve kutudan çıkarma işlemleri bir yapı türü ve `object` ([kutulama ve kutudan](structs.md#boxing-and-unboxing)çıkarma) arasında dönüştürmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="da82c-135">Boxing and unboxing operations are used to convert between a struct type and `object` ([Boxing and unboxing](structs.md#boxing-and-unboxing)).</span></span>
*  <span data-ttu-id="da82c-136">`this` anlamı yapılar için farklıdır ([Bu erişim](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="da82c-136">The meaning of `this` is different for structs ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="da82c-137">Bir yapının örnek alanı bildirimlerine değişken başlatıcıları ([alan başlatıcıları](structs.md#field-initializers)) ekleme izni verilmez.</span><span class="sxs-lookup"><span data-stu-id="da82c-137">Instance field declarations for a struct are not permitted to include variable initializers ([Field initializers](structs.md#field-initializers)).</span></span>
*  <span data-ttu-id="da82c-138">Bir yapının parametresiz örnek Oluşturucusu ([oluşturucular](structs.md#constructors)) bildirme izni yok.</span><span class="sxs-lookup"><span data-stu-id="da82c-138">A struct is not permitted to declare a parameterless instance constructor ([Constructors](structs.md#constructors)).</span></span>
*  <span data-ttu-id="da82c-139">Bir yapının yıkıcı ([Yıkıcılar](structs.md#destructors)) bildirme izni yok.</span><span class="sxs-lookup"><span data-stu-id="da82c-139">A struct is not permitted to declare a destructor ([Destructors](structs.md#destructors)).</span></span>

### <a name="value-semantics"></a><span data-ttu-id="da82c-140">Değer semantiği</span><span class="sxs-lookup"><span data-stu-id="da82c-140">Value semantics</span></span>

<span data-ttu-id="da82c-141">Yapılar değer türlerdir ([değer türleri](types.md#value-types)) ve değer semantiğini kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="da82c-141">Structs are value types ([Value types](types.md#value-types)) and are said to have value semantics.</span></span> <span data-ttu-id="da82c-142">Diğer yandan sınıflar, başvuru türleridir ([başvuru türleri](types.md#reference-types)) ve başvuru semantiklerine sahip olarak söylenir.</span><span class="sxs-lookup"><span data-stu-id="da82c-142">Classes, on the other hand, are reference types ([Reference types](types.md#reference-types)) and are said to have reference semantics.</span></span>

<span data-ttu-id="da82c-143">Yapı türünün bir değişkeni doğrudan yapı verilerini içerir, ancak bir sınıf türünün değişkeni, ikinci olarak bir nesne olarak bilinen veri başvurusunu içerir.</span><span class="sxs-lookup"><span data-stu-id="da82c-143">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span> <span data-ttu-id="da82c-144">Bir struct `B` `A` türünde bir örnek alanı içerdiğinde ve `A` bir yapı türü ise, `A` `B` veya `B`oluşturulan bir türe bağlı olarak bir derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="da82c-144">When a struct `B` contains an instance field of type `A` and `A` is a struct type, it is a compile-time error for `A` to depend on `B` or a type constructed from `B`.</span></span> <span data-ttu-id="da82c-145">Bir struct `X`, `X` `Y`bir örnek alanı içeriyorsa, bir struct `Y` ***doğrudan bağlıdır*** .</span><span class="sxs-lookup"><span data-stu-id="da82c-145">A struct `X` ***directly depends on*** a struct `Y` if `X` contains an instance field of type `Y`.</span></span> <span data-ttu-id="da82c-146">Bu tanım verildiğinde, bir yapının bağlı olduğu yapı kümesinin tamamı, ilişkiye bağlı olarak, ***doğrudan '*** nin geçişli kapanışı olur.</span><span class="sxs-lookup"><span data-stu-id="da82c-146">Given this definition, the complete set of structs upon which a struct depends is the transitive closure of the ***directly depends on*** relationship.</span></span>  <span data-ttu-id="da82c-147">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="da82c-147">For example</span></span>
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
<span data-ttu-id="da82c-148">`Node`, kendi türünün bir örnek alanı içerdiğinden bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="da82c-148">is an error because `Node` contains an instance field of its own type.</span></span>  <span data-ttu-id="da82c-149">Başka bir örnek</span><span class="sxs-lookup"><span data-stu-id="da82c-149">Another example</span></span>
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
<span data-ttu-id="da82c-150">Her türden `A`, `B`ve `C` birbirlerine bağlı olduğundan bir hata oluştu.</span><span class="sxs-lookup"><span data-stu-id="da82c-150">is an error because each of the types `A`, `B`, and `C` depend on each other.</span></span>

<span data-ttu-id="da82c-151">Sınıflar ile, iki değişkenin aynı nesneye başvurması ve bu nedenle bir değişkende işlemler için diğer değişken tarafından başvurulan nesneyi etkilemesi mümkündür.</span><span class="sxs-lookup"><span data-stu-id="da82c-151">With classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="da82c-152">Yapılar ile, her birinin verilerin kendi kopyası vardır (`ref` ve `out` parametre değişkenleri olması dışında) ve birindeki işlemler diğerini etkilemeyebilir.</span><span class="sxs-lookup"><span data-stu-id="da82c-152">With structs, the variables each have their own copy of the data (except in the case of `ref` and `out` parameter variables), and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="da82c-153">Ayrıca, yapılar başvuru türleri olmadığından, yapı türünün değerlerinin `null`olması mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="da82c-153">Furthermore, because structs are not reference types, it is not possible for values of a struct type to be `null`.</span></span>

<span data-ttu-id="da82c-154">Bildirim verildi</span><span class="sxs-lookup"><span data-stu-id="da82c-154">Given the declaration</span></span>
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
<span data-ttu-id="da82c-155">kod parçası</span><span class="sxs-lookup"><span data-stu-id="da82c-155">the code fragment</span></span>
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
<span data-ttu-id="da82c-156">`10`değerini verir.</span><span class="sxs-lookup"><span data-stu-id="da82c-156">outputs the value `10`.</span></span> <span data-ttu-id="da82c-157">`a` `b` atama, değerin bir kopyasını oluşturur ve bu nedenle `b` `a.x`atamasından etkilenmez.</span><span class="sxs-lookup"><span data-stu-id="da82c-157">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="da82c-158">Bunun yerine bir sınıf olarak bildirildiği `Point`, `a` ve `b` aynı nesneye başvuracağından çıkış `100`.</span><span class="sxs-lookup"><span data-stu-id="da82c-158">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>

### <a name="inheritance"></a><span data-ttu-id="da82c-159">Devralma</span><span class="sxs-lookup"><span data-stu-id="da82c-159">Inheritance</span></span>

<span data-ttu-id="da82c-160">Tüm yapı türleri dolaylı olarak `System.ValueType`sınıfından devralır, bu, sırasıyla sınıf `object`devralır.</span><span class="sxs-lookup"><span data-stu-id="da82c-160">All struct types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="da82c-161">Bir struct bildirimi uygulanan arabirimlerin bir listesini belirtebilir, ancak bir struct bildiriminin bir temel sınıf belirtmesi mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="da82c-161">A struct declaration may specify a list of implemented interfaces, but it is not possible for a struct declaration to specify a base class.</span></span>

<span data-ttu-id="da82c-162">Yapı türleri hiçbir zaman soyut değildir ve her zaman örtük olarak mühürlenmez.</span><span class="sxs-lookup"><span data-stu-id="da82c-162">Struct types are never abstract and are always implicitly sealed.</span></span> <span data-ttu-id="da82c-163">Bu nedenle `abstract` ve `sealed` değiştiricilerine bir struct bildiriminde izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="da82c-163">The `abstract` and `sealed` modifiers are therefore not permitted in a struct declaration.</span></span>

<span data-ttu-id="da82c-164">Yapılar için devralma desteklenmediğinden, bir struct üyesinin tanımlanmış erişilebilirliği `protected` veya `protected internal`olamaz.</span><span class="sxs-lookup"><span data-stu-id="da82c-164">Since inheritance isn't supported for structs, the declared accessibility of a struct member cannot be `protected` or `protected internal`.</span></span>

<span data-ttu-id="da82c-165">Bir yapıdaki işlev üyeleri `abstract` veya `virtual`olamaz ve `override` değiştiricisine yalnızca `System.ValueType`devralınan yöntemleri geçersiz kılmak için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="da82c-165">Function members in a struct cannot be `abstract` or `virtual`, and the `override` modifier is allowed only to override methods inherited from `System.ValueType`.</span></span>

### <a name="assignment"></a><span data-ttu-id="da82c-166">Atama</span><span class="sxs-lookup"><span data-stu-id="da82c-166">Assignment</span></span>

<span data-ttu-id="da82c-167">Struct türündeki bir değişkene atama, atanmakta olan değerin bir kopyasını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="da82c-167">Assignment to a variable of a struct type creates a copy of the value being assigned.</span></span> <span data-ttu-id="da82c-168">Bu, atamadan bir sınıf türü değişkenine farklılık gösterir. Bu, başvuruyu kopyalayan ancak başvuru tarafından tanımlanan nesneye değil.</span><span class="sxs-lookup"><span data-stu-id="da82c-168">This differs from assignment to a variable of a class type, which copies the reference but not the object identified by the reference.</span></span>

<span data-ttu-id="da82c-169">Bir atamaya benzer şekilde, bir struct bir değer parametresi olarak geçirildiğinde veya bir işlev üyesinin sonucu olarak döndürüldüğünde, yapının bir kopyası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="da82c-169">Similar to an assignment, when a struct is passed as a value parameter or returned as the result of a function member, a copy of the struct is created.</span></span> <span data-ttu-id="da82c-170">Bir struct, bir `ref` veya `out` parametresi kullanılarak bir işlev üyesine başvuru ile geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="da82c-170">A struct may be passed by reference to a function member using a `ref` or `out` parameter.</span></span>

<span data-ttu-id="da82c-171">Bir yapının özelliği veya Dizin Oluşturucusu bir atamanın hedefi olduğunda, özellik veya Dizin Oluşturucu erişimi ile ilişkili örnek ifadesi bir değişken olarak sınıflandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="da82c-171">When a property or indexer of a struct is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="da82c-172">Örnek ifadesi bir değer olarak sınıflandırıldığında, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="da82c-172">If the instance expression is classified as a value, a compile-time error occurs.</span></span> <span data-ttu-id="da82c-173">Bu, [basit atamada](expressions.md#simple-assignment)daha ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="da82c-173">This is described in further detail in [Simple assignment](expressions.md#simple-assignment).</span></span>

### <a name="default-values"></a><span data-ttu-id="da82c-174">Varsayılan değerler</span><span class="sxs-lookup"><span data-stu-id="da82c-174">Default values</span></span>

<span data-ttu-id="da82c-175">[Varsayılan değerler](variables.md#default-values)' de açıklandığı gibi çeşitli değişkenler, oluşturuldukları sırada varsayılan değerlerine otomatik olarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="da82c-175">As described in [Default values](variables.md#default-values), several kinds of variables are automatically initialized to their default value when they are created.</span></span> <span data-ttu-id="da82c-176">Sınıf türleri ve diğer başvuru türleri değişkenleri için, bu varsayılan değer `null`.</span><span class="sxs-lookup"><span data-stu-id="da82c-176">For variables of class types and other reference types, this default value is `null`.</span></span> <span data-ttu-id="da82c-177">Ancak, yapılar `null`değer türleri olduğundan, yapının varsayılan değeri, tüm değer türü alanları varsayılan değerlerine ve tüm başvuru türü `null`alanlarına ayarlanarak oluşturulan değerdir.</span><span class="sxs-lookup"><span data-stu-id="da82c-177">However, since structs are value types that cannot be `null`, the default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="da82c-178">Yukarıda belirtilen `Point` yapısına başvurma, örnek</span><span class="sxs-lookup"><span data-stu-id="da82c-178">Referring to the `Point` struct declared above, the example</span></span>
```csharp
Point[] a = new Point[100];
```
<span data-ttu-id="da82c-179">dizideki her `Point`, `x` ve `y` alanları sıfıra ayarlanarak oluşturulan değere başlatır.</span><span class="sxs-lookup"><span data-stu-id="da82c-179">initializes each `Point` in the array to the value produced by setting the `x` and `y` fields to zero.</span></span>

<span data-ttu-id="da82c-180">Bir yapının varsayılan değeri, yapının varsayılan oluşturucusunun ([Varsayılan oluşturucular](types.md#default-constructors)) döndürdüğü değere karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="da82c-180">The default value of a struct corresponds to the value returned by the default constructor of the struct ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="da82c-181">Bir sınıfın aksine, bir yapının parametresiz örnek Oluşturucusu bildirmesine izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="da82c-181">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="da82c-182">Bunun yerine, her yapının örtük olarak, tüm değer türü alanlarını varsayılan değerlerine ve tüm başvuru türü alanlarına `null`için ayarlamalarından elde edilen değeri döndüren, parametresiz bir örnek Oluşturucusu vardır.</span><span class="sxs-lookup"><span data-stu-id="da82c-182">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="da82c-183">Yapılar, varsayılan başlatma durumunu geçerli bir durum olacak şekilde göz önünde bulundurmanız için tasarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="da82c-183">Structs should be designed to consider the default initialization state a valid state.</span></span> <span data-ttu-id="da82c-184">örnekte</span><span class="sxs-lookup"><span data-stu-id="da82c-184">In the example</span></span>
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
<span data-ttu-id="da82c-185">Kullanıcı tanımlı örnek Oluşturucu yalnızca açıkça çağrıldığı yerde null değerlere karşı koruma sağlar.</span><span class="sxs-lookup"><span data-stu-id="da82c-185">the user-defined instance constructor protects against null values only where it is explicitly called.</span></span> <span data-ttu-id="da82c-186">Bir `KeyValuePair` değişkeninin varsayılan değer başlatmasına tabi olduğu durumlarda, `key` ve `value` alanları null olur ve yapının bu durumu işlemek için hazırlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="da82c-186">In cases where a `KeyValuePair` variable is subject to default value initialization, the `key` and `value` fields will be null, and the struct must be prepared to handle this state.</span></span>

### <a name="boxing-and-unboxing"></a><span data-ttu-id="da82c-187">Kutulama ve kutudan çıkarma</span><span class="sxs-lookup"><span data-stu-id="da82c-187">Boxing and unboxing</span></span>

<span data-ttu-id="da82c-188">Bir sınıf türünün değeri, başvuruya yalnızca derleme zamanında başka bir tür olarak davranarak sınıf tarafından uygulanan bir arabirim türü `object` veya türü türüne dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="da82c-188">A value of a class type can be converted to type `object` or to an interface type that is implemented by the class simply by treating the reference as another type at compile-time.</span></span> <span data-ttu-id="da82c-189">Benzer şekilde, `object` veya bir arabirim türünün değeri bir değeri, başvuru değiştirilmeden bir sınıf türüne geri dönüştürülebilir (ancak bu durumda bir çalışma zamanı tür denetimi gereklidir).</span><span class="sxs-lookup"><span data-stu-id="da82c-189">Likewise, a value of type `object` or a value of an interface type can be converted back to a class type without changing the reference (but of course a run-time type check is required in this case).</span></span>

<span data-ttu-id="da82c-190">Yapılar başvuru türleri olmadığından, bu işlemler yapı türleri için farklı şekilde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="da82c-190">Since structs are not reference types, these operations are implemented differently for struct types.</span></span> <span data-ttu-id="da82c-191">Yapı türünde bir değer `object` türüne veya yapı tarafından uygulanan bir arabirim türüne dönüştürüldüğünde, kutulama işlemi gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="da82c-191">When a value of a struct type is converted to type `object` or to an interface type that is implemented by the struct, a boxing operation takes place.</span></span> <span data-ttu-id="da82c-192">Benzer şekilde, `object` türü veya bir arabirim türünün değeri bir yapı türüne geri dönüştürüldüğünde, kutudan çıkarma işlemi gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="da82c-192">Likewise, when a value of type `object` or a value of an interface type is converted back to a struct type, an unboxing operation takes place.</span></span> <span data-ttu-id="da82c-193">Sınıf türlerinde aynı işlemlerden önemli bir farklılık, kutulama ve kutudan çıkarma, yapı değerini kutulanmış örneğin içine veya dışına kopyaladır.</span><span class="sxs-lookup"><span data-stu-id="da82c-193">A key difference from the same operations on class types is that boxing and unboxing copies the struct value either into or out of the boxed instance.</span></span> <span data-ttu-id="da82c-194">Bu nedenle, kutulama veya kutudan çıkarma işleminin ardından kutulanmış yapıda yapılan değişiklikler kutulanmış yapıda yansıtılmaz.</span><span class="sxs-lookup"><span data-stu-id="da82c-194">Thus, following a boxing or unboxing operation, changes made to the unboxed struct are not reflected in the boxed struct.</span></span>

<span data-ttu-id="da82c-195">Bir struct türü `System.Object` devralınan sanal bir yöntemi geçersiz kıldığında (`Equals`, `GetHashCode`veya `ToString`gibi), yapı türünün bir örneği aracılığıyla sanal yöntemin çağrılması kutulama oluşmasına neden olmaz.</span><span class="sxs-lookup"><span data-stu-id="da82c-195">When a struct type overrides a virtual method inherited from `System.Object` (such as `Equals`, `GetHashCode`, or `ToString`), invocation of the virtual method through an instance of the struct type does not cause boxing to occur.</span></span> <span data-ttu-id="da82c-196">Bu, struct bir tür parametresi olarak kullanıldığında ve çağrı tür parametre türünün bir örneği aracılığıyla gerçekleşse bile geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="da82c-196">This is true even when the struct is used as a type parameter and the invocation occurs through an instance of the type parameter type.</span></span> <span data-ttu-id="da82c-197">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="da82c-197">For example:</span></span>
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

<span data-ttu-id="da82c-198">Programın çıkışı:</span><span class="sxs-lookup"><span data-stu-id="da82c-198">The output of the program is:</span></span>
```console
1
2
3
```

<span data-ttu-id="da82c-199">`ToString` yan etkileri olması için hatalı stil olsa da, örnek üç `x.ToString()`çağırma için kutulanmanın olmadığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="da82c-199">Although it is bad style for `ToString` to have side effects, the example demonstrates that no boxing occurred for the three invocations of `x.ToString()`.</span></span>

<span data-ttu-id="da82c-200">Benzer şekilde, kısıtlı bir tür parametresindeki bir üyeye erişirken paketleme hiçbir zaman açıkça gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="da82c-200">Similarly, boxing never implicitly occurs when accessing a member on a constrained type parameter.</span></span> <span data-ttu-id="da82c-201">Örneğin, bir arabirimin, bir değeri değiştirmek için kullanılabilecek bir yöntem `Increment` `ICounter` olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="da82c-201">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="da82c-202">`ICounter` bir kısıtlama olarak kullanılıyorsa, `Increment` yönteminin uygulanması, `Increment` çağrılan değişkene bir başvuru ile çağrılır, hiçbir koşulda paketlenmiş bir kopya.</span><span class="sxs-lookup"><span data-stu-id="da82c-202">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, never a boxed copy.</span></span>

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

<span data-ttu-id="da82c-203">İlk `Increment` çağrısı, `x`değişken içindeki değeri değiştirir.</span><span class="sxs-lookup"><span data-stu-id="da82c-203">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="da82c-204">Bu, `x`paketlenmiş bir kopyasında değeri değiştiren `Increment`ikinci çağrısıyla eşdeğer değildir.</span><span class="sxs-lookup"><span data-stu-id="da82c-204">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="da82c-205">Bu nedenle, programın çıktısı şu şekilde olur:</span><span class="sxs-lookup"><span data-stu-id="da82c-205">Thus, the output of the program is:</span></span>
```console
0
1
1
```

<span data-ttu-id="da82c-206">Kutulama ve kutudan çıkarma hakkında daha fazla bilgi için bkz. [kutulama ve kutudan](types.md#boxing-and-unboxing)çıkarma.</span><span class="sxs-lookup"><span data-stu-id="da82c-206">For further details on boxing and unboxing, see [Boxing and unboxing](types.md#boxing-and-unboxing).</span></span>

### <a name="meaning-of-this"></a><span data-ttu-id="da82c-207">Bunun anlamı</span><span class="sxs-lookup"><span data-stu-id="da82c-207">Meaning of this</span></span>

<span data-ttu-id="da82c-208">Bir sınıfın örnek Oluşturucu veya örnek işlev üyesi içinde `this`, bir değer olarak sınıflandırıldı.</span><span class="sxs-lookup"><span data-stu-id="da82c-208">Within an instance constructor or instance function member of a class, `this` is classified as a value.</span></span> <span data-ttu-id="da82c-209">Bu nedenle `this`, işlev üyesinin çağrıldığı örneğe başvurmak için kullanılabilir ancak, bir sınıfın işlev üyesinde `this` atamak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="da82c-209">Thus, while `this` can be used to refer to the instance for which the function member was invoked, it is not possible to assign to `this` in a function member of a class.</span></span>

<span data-ttu-id="da82c-210">Bir yapının örnek Oluşturucusu içinde `this`, yapı türünün bir `out` parametresine karşılık gelir ve bir yapının örnek işlev üyesi `this`, yapı türünün `ref` parametresine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="da82c-210">Within an instance constructor of a struct, `this` corresponds to an `out` parameter of the struct type, and within an instance function member of a struct, `this` corresponds to a `ref` parameter of the struct type.</span></span> <span data-ttu-id="da82c-211">Her iki durumda da, `this` bir değişken olarak sınıflandırıldı ve işlev üyesinin `this` atama ile veya `ref` ya da `out` parametresi olarak geçirerek çağrıldığı yapının tamamını değiştirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="da82c-211">In both cases, `this` is classified as a variable, and it is possible to modify the entire struct for which the function member was invoked by assigning to `this` or by passing this as a `ref` or `out` parameter.</span></span>

### <a name="field-initializers"></a><span data-ttu-id="da82c-212">Alan başlatıcıları</span><span class="sxs-lookup"><span data-stu-id="da82c-212">Field initializers</span></span>

<span data-ttu-id="da82c-213">[Varsayılan değerler](structs.md#default-values)bölümünde açıklandığı gibi, bir yapının varsayılan değeri, tüm değer türü alanlarını varsayılan değerlerine ve tüm başvuru türü alanlarına `null`.</span><span class="sxs-lookup"><span data-stu-id="da82c-213">As described in [Default values](structs.md#default-values), the default value of a struct consists of the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span> <span data-ttu-id="da82c-214">Bu nedenle, bir yapı, örnek alan bildirimlerine değişken başlatıcıları dahil etmek için izin vermez.</span><span class="sxs-lookup"><span data-stu-id="da82c-214">For this reason, a struct does not permit instance field declarations to include variable initializers.</span></span> <span data-ttu-id="da82c-215">Bu kısıtlama yalnızca örnek alanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="da82c-215">This restriction applies only to instance fields.</span></span> <span data-ttu-id="da82c-216">Yapının statik alanlarının değişken başlatıcıları içermesi için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="da82c-216">Static fields of a struct are permitted to include variable initializers.</span></span>

<span data-ttu-id="da82c-217">Örnek</span><span class="sxs-lookup"><span data-stu-id="da82c-217">The example</span></span>
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
<span data-ttu-id="da82c-218">örnek alan bildirimleri değişken başlatıcılar içerdiğinden hata oluştu.</span><span class="sxs-lookup"><span data-stu-id="da82c-218">is in error because the instance field declarations include variable initializers.</span></span>

### <a name="constructors"></a><span data-ttu-id="da82c-219">Oluşturucular</span><span class="sxs-lookup"><span data-stu-id="da82c-219">Constructors</span></span>

<span data-ttu-id="da82c-220">Bir sınıfın aksine, bir yapının parametresiz örnek Oluşturucusu bildirmesine izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="da82c-220">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="da82c-221">Bunun yerine, her yapının örtük olarak, tüm değer türü alanlarını varsayılan değerlerine ve tüm başvuru türü alanlarına null ([Varsayılan oluşturucular](types.md#default-constructors)) olarak ayarlamalarından elde edilen değeri döndüren, parametresiz bir örnek Oluşturucusu vardır.</span><span class="sxs-lookup"><span data-stu-id="da82c-221">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to null ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="da82c-222">Bir struct, parametrelere sahip örnek oluşturucularını bildirebilir.</span><span class="sxs-lookup"><span data-stu-id="da82c-222">A struct can declare instance constructors having parameters.</span></span> <span data-ttu-id="da82c-223">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="da82c-223">For example</span></span>
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

<span data-ttu-id="da82c-224">Yukarıdaki bildirim ve deyimler verildiğinde</span><span class="sxs-lookup"><span data-stu-id="da82c-224">Given the above declaration, the statements</span></span>
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
<span data-ttu-id="da82c-225">her ikisi de `x` ve `y` sıfırdan `Point` oluşturun.</span><span class="sxs-lookup"><span data-stu-id="da82c-225">both create a `Point` with `x` and `y` initialized to zero.</span></span>

<span data-ttu-id="da82c-226">Bir struct Instance oluşturucusunun `base(...)`form Oluşturucu başlatıcısı içerme izni yoktur.</span><span class="sxs-lookup"><span data-stu-id="da82c-226">A struct instance constructor is not permitted to include a constructor initializer of the form `base(...)`.</span></span>

<span data-ttu-id="da82c-227">Yapı örneği Oluşturucusu bir Oluşturucu başlatıcısı belirtmezse, `this` değişkeni yapı türünün bir `out` parametresine karşılık gelir ve bir `out` parametresine benzer şekilde, oluşturucunun döndürdüğü her yerde `this` kesinlikle atanmalıdır ([kesin atama](variables.md#definite-assignment)).</span><span class="sxs-lookup"><span data-stu-id="da82c-227">If the struct instance constructor doesn't specify a constructor initializer, the `this` variable corresponds to an `out` parameter of the struct type, and similar to an `out` parameter, `this` must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at every location where the constructor returns.</span></span> <span data-ttu-id="da82c-228">Yapı örneği Oluşturucusu bir Oluşturucu başlatıcısı belirtiyorsa, `this` değişkeni yapı türünün bir `ref` parametresine karşılık gelir ve bir `ref` parametresine benzer `this`, Oluşturucu gövdesine kesin olarak atanmış olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="da82c-228">If the struct instance constructor specifies a constructor initializer, the `this` variable corresponds to a `ref` parameter of the struct type, and similar to a `ref` parameter, `this` is considered definitely assigned on entry to the constructor body.</span></span> <span data-ttu-id="da82c-229">Örnek Oluşturucu uygulamasını aşağıdan göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="da82c-229">Consider the instance constructor implementation below:</span></span>
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

<span data-ttu-id="da82c-230">Oluşturulan yapının tüm alanları kesinlikle atanana kadar, örnek üye işlevi (`X` ve `Y`için küme erişimcileri dahil) çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="da82c-230">No instance member function (including the set accessors for the properties `X` and `Y`) can be called until all fields of the struct being constructed have been definitely assigned.</span></span> <span data-ttu-id="da82c-231">Tek özel durum otomatik olarak uygulanan Özellikler ([Otomatik uygulanan özellikler](classes.md#automatically-implemented-properties)) içerir.</span><span class="sxs-lookup"><span data-stu-id="da82c-231">The only exception involves automatically implemented properties ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="da82c-232">Kesin atama kuralları ([basit atama ifadeleri](variables.md#simple-assignment-expressions)), bu yapı türünün örnek Oluşturucusu içindeki bir yapı türünün bir otomatik özelliğine atamayı özel olarak muaf bulundurmaktır: böyle bir atama, otomatik özelliğin gizli yedekleme alanının kesin bir atamasını kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="da82c-232">The definite assignment rules ([Simple assignment expressions](variables.md#simple-assignment-expressions)) specifically exempt assignment to an auto-property of a struct type within an instance constructor of that struct type: such an assignment is considered a definite assignment of the hidden backing field of the auto-property.</span></span> <span data-ttu-id="da82c-233">Bu nedenle aşağıdakilere izin verilir:</span><span class="sxs-lookup"><span data-stu-id="da82c-233">Thus, the following is allowed:</span></span>

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

### <a name="destructors"></a><span data-ttu-id="da82c-234">Yıkıcılar</span><span class="sxs-lookup"><span data-stu-id="da82c-234">Destructors</span></span>

<span data-ttu-id="da82c-235">Bir yapının yıkıcı bildirme izni yok.</span><span class="sxs-lookup"><span data-stu-id="da82c-235">A struct is not permitted to declare a destructor.</span></span>

### <a name="static-constructors"></a><span data-ttu-id="da82c-236">Statik oluşturucular</span><span class="sxs-lookup"><span data-stu-id="da82c-236">Static constructors</span></span>

<span data-ttu-id="da82c-237">Yapılar için statik oluşturucular, sınıflarla aynı kuralların çoğunu izler.</span><span class="sxs-lookup"><span data-stu-id="da82c-237">Static constructors for structs follow most of the same rules as for classes.</span></span> <span data-ttu-id="da82c-238">Bir yapı türü için statik oluşturucunun yürütülmesi, aşağıdaki olayların ilki tarafından bir uygulama etki alanında gerçekleşecek şekilde tetiklenir:</span><span class="sxs-lookup"><span data-stu-id="da82c-238">The execution of a static constructor for a struct type is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="da82c-239">Struct türündeki statik bir üyeye başvurulur.</span><span class="sxs-lookup"><span data-stu-id="da82c-239">A static member of the struct type is referenced.</span></span>
*  <span data-ttu-id="da82c-240">Struct türünün açıkça tanımlanmış bir Oluşturucusu çağırılır.</span><span class="sxs-lookup"><span data-stu-id="da82c-240">An explicitly declared constructor of the struct type is called.</span></span>

<span data-ttu-id="da82c-241">Yapı türlerinin varsayılan değerlerinin ([varsayılan değerler](structs.md#default-values)) oluşturulması statik oluşturucuyu tetiklemez.</span><span class="sxs-lookup"><span data-stu-id="da82c-241">The creation of default values ([Default values](structs.md#default-values)) of struct types does not trigger the static constructor.</span></span> <span data-ttu-id="da82c-242">(Bir dizideki öğelerin ilk değeri bu bir örnektir.)</span><span class="sxs-lookup"><span data-stu-id="da82c-242">(An example of this is the initial value of elements in an array.)</span></span>

## <a name="struct-examples"></a><span data-ttu-id="da82c-243">Struct örnekleri</span><span class="sxs-lookup"><span data-stu-id="da82c-243">Struct examples</span></span>

<span data-ttu-id="da82c-244">Aşağıda, dilin önceden tanımlanmış türlerine benzer şekilde kullanılabilecek türler oluşturmak için `struct` türlerini kullanmanın iki önemli örneği gösterilmektedir, ancak değiştirme semantiği vardır.</span><span class="sxs-lookup"><span data-stu-id="da82c-244">The following shows two significant examples of using `struct` types to create types that can be used similarly to the predefined types of the language, but with modified semantics.</span></span>

### <a name="database-integer-type"></a><span data-ttu-id="da82c-245">Veritabanı tamsayı türü</span><span class="sxs-lookup"><span data-stu-id="da82c-245">Database integer type</span></span>

<span data-ttu-id="da82c-246">Aşağıdaki `DBInt` yapısı, `int` türünün tüm değer kümesini temsil eden bir tamsayı türü ve bilinmeyen bir değeri belirten ek bir durum uygular.</span><span class="sxs-lookup"><span data-stu-id="da82c-246">The `DBInt` struct below implements an integer type that can represent the complete set of values of the `int` type, plus an additional state that indicates an unknown value.</span></span> <span data-ttu-id="da82c-247">Bu özelliklere sahip bir tür veritabanlarında yaygın olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="da82c-247">A type with these characteristics is commonly used in databases.</span></span>

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

### <a name="database-boolean-type"></a><span data-ttu-id="da82c-248">Veritabanı Boole türü</span><span class="sxs-lookup"><span data-stu-id="da82c-248">Database boolean type</span></span>

<span data-ttu-id="da82c-249">Aşağıdaki `DBBool` yapısı üç değerli bir mantıksal tür uygular.</span><span class="sxs-lookup"><span data-stu-id="da82c-249">The `DBBool` struct below implements a three-valued logical type.</span></span> <span data-ttu-id="da82c-250">Bu türün olası değerleri, `Null` üyesinin bilinmeyen bir değeri gösterdiği `DBBool.True`, `DBBool.False`ve `DBBool.Null`.</span><span class="sxs-lookup"><span data-stu-id="da82c-250">The possible values of this type are `DBBool.True`, `DBBool.False`, and `DBBool.Null`, where the `Null` member indicates an unknown value.</span></span> <span data-ttu-id="da82c-251">Bu üç değerli mantıksal türler genellikle veritabanlarında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="da82c-251">Such three-valued logical types are commonly used in databases.</span></span>

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
