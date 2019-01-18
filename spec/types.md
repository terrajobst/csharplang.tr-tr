---
ms.openlocfilehash: a28397b1ce97dbead6d5014e2b20e108a1018502
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229971"
---
# <a name="types"></a><span data-ttu-id="f0c6a-101">Türler</span><span class="sxs-lookup"><span data-stu-id="f0c6a-101">Types</span></span>

<span data-ttu-id="f0c6a-102">C# dili türlerini iki ana kategoriye ayrılır: ***değer türleri*** ve ***başvuru türleri***.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-102">The types of the C# language are divided into two main categories: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="f0c6a-103">Hem değer türleri ve başvuru türleri olabilir ***genel türler***, bir veya daha fazla almakta ***tür parametrelerindeki***.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-103">Both value types and reference types may be ***generic types***, which take one or more ***type parameters***.</span></span> <span data-ttu-id="f0c6a-104">Tür parametreleri, her iki değer türleri belirlemek ve başvuru türleri.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-104">Type parameters can designate both value types and reference types.</span></span>

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

<span data-ttu-id="f0c6a-105">Son kategori türlerinin işaretçileri, yalnızca güvenli olmayan kod içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-105">The final category of types, pointers, is available only in unsafe code.</span></span> <span data-ttu-id="f0c6a-106">Bunu ele alınan ayrıntılı olarak [işaretçi türleri](unsafe-code.md#pointer-types).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-106">This is discussed further in [Pointer types](unsafe-code.md#pointer-types).</span></span>

<span data-ttu-id="f0c6a-107">Değer türleri farklı başvuru türleri, değer türlerinin değişkenleri kendi verilerini doğrudan içerdiği değişkenleri başvuru türleri ise depolama ***başvuruları*** ikinci olarak bilinen verilerine, ***nesneleri***.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-107">Value types differ from reference types in that variables of the value types directly contain their data, whereas variables of the reference types store ***references*** to their data, the latter being known as ***objects***.</span></span> <span data-ttu-id="f0c6a-108">Başvuru türleri ile bu iki değişken aynı nesneye başvurmak mümkün ve dolayısıyla işlemler diğer değişkenin başvurduğu nesneyi etkileyebilir bir değişken üzerinde mümkün olur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-108">With reference types, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="f0c6a-109">Değer türleri ile her değişkenleri kendi veri kopyası vardır ve yapılan işlemlerin diğerini etkilemesi olanaklı birinde değil.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-109">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span>

<span data-ttu-id="f0c6a-110">C# tür sistemi, herhangi bir türde bir değer bir nesne olarak davranılıp şekilde birleşiktir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-110">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="f0c6a-111">C# ' de her tür doğrudan veya dolaylı olarak türetir `object` sınıf türü, ve `object` tüm türlerin ultimate temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-111">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="f0c6a-112">Başvuru türlerindeki değerleri nesneler olarak değer türü olarak yalnızca görüntüleyerek edilir `object`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-112">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="f0c6a-113">Değer türlerinin değerleri nesneler olarak kutulama ve kutudan çıkarma işlemleri gerçekleştirerek edilir ([kutulama ve kutudan çıkarma](types.md#boxing-and-unboxing)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-113">Values of value types are treated as objects by performing boxing and unboxing operations ([Boxing and unboxing](types.md#boxing-and-unboxing)).</span></span>

## <a name="value-types"></a><span data-ttu-id="f0c6a-114">Değer türleri</span><span class="sxs-lookup"><span data-stu-id="f0c6a-114">Value types</span></span>

<span data-ttu-id="f0c6a-115">Bir değer türü veya bir yapı türü, hem de bir numaralandırma türü değil.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-115">A value type is either a struct type or an enumeration type.</span></span> <span data-ttu-id="f0c6a-116">C# adlı önceden tanımlanmış bir yapı türleri kümesi sağlar ***basit türler***.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-116">C# provides a set of predefined struct types called the ***simple types***.</span></span> <span data-ttu-id="f0c6a-117">Basit türler ayrılmış sözcükler tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-117">The simple types are identified through reserved words.</span></span>

```antlr
value_type
    : struct_type
    | enum_type
    ;

struct_type
    : type_name
    | simple_type
    | nullable_type
    ;

simple_type
    : numeric_type
    | 'bool'
    ;

numeric_type
    : integral_type
    | floating_point_type
    | 'decimal'
    ;

integral_type
    : 'sbyte'
    | 'byte'
    | 'short'
    | 'ushort'
    | 'int'
    | 'uint'
    | 'long'
    | 'ulong'
    | 'char'
    ;

floating_point_type
    : 'float'
    | 'double'
    ;

nullable_type
    : non_nullable_value_type '?'
    ;

non_nullable_value_type
    : type
    ;

enum_type
    : type_name
    ;
```

<span data-ttu-id="f0c6a-118">Bir değişkeni başvuru türünde bir değer türünün bir değişkeni değer içerebilir `null` yalnızca değer türü boş değer atanabilir bir tür ise.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-118">Unlike a variable of a reference type, a variable of a value type can contain the value `null` only if the value type is a nullable type.</span></span>  <span data-ttu-id="f0c6a-119">Her değer atanamayan değer türü için aynı değerleri artı değer kümesini belirten karşılık gelen null yapılabilir değer türü var. `null`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-119">For every non-nullable value type there is a corresponding nullable value type denoting the same set of values plus the value `null`.</span></span>

<span data-ttu-id="f0c6a-120">Bir değer türü bir değişkene atama atanan değerin bir kopyasını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-120">Assignment to a variable of a value type creates a copy of the value being assigned.</span></span> <span data-ttu-id="f0c6a-121">Bu başvuru ancak başvuru tarafından tanımlanan nesnesi değil kopyalar bir başvuru türündeki bir değişkene atamadan farklıdır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-121">This differs from assignment to a variable of a reference type, which copies the reference but not the object identified by the reference.</span></span>

### <a name="the-systemvaluetype-type"></a><span data-ttu-id="f0c6a-122">System.ValueType türü</span><span class="sxs-lookup"><span data-stu-id="f0c6a-122">The System.ValueType type</span></span>

<span data-ttu-id="f0c6a-123">Tüm değer türleri örtülü olarak sınıftan `System.ValueType`, sırasıyla sınıfından devralan `object`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-123">All value types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="f0c6a-124">Bir değer türünden türetilen herhangi bir tür için mümkün değildir ve değer türleri böylece türetme ([sınıflar korumalı](classes.md#sealed-classes)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-124">It is not possible for any type to derive from a value type, and value types are thus implicitly sealed ([Sealed classes](classes.md#sealed-classes)).</span></span>

<span data-ttu-id="f0c6a-125">Unutmayın `System.ValueType` kendisi değil bir *value_type*.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-125">Note that `System.ValueType` is not itself a *value_type*.</span></span> <span data-ttu-id="f0c6a-126">Bunun yerine, olan bir *class_type* tüm gelen *value_type*s otomatik olarak türetilmiş.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-126">Rather, it is a *class_type* from which all *value_type*s are automatically derived.</span></span>

### <a name="default-constructors"></a><span data-ttu-id="f0c6a-127">Varsayılan Oluşturucu</span><span class="sxs-lookup"><span data-stu-id="f0c6a-127">Default constructors</span></span>

<span data-ttu-id="f0c6a-128">Tüm değer türleri örtülü olarak adlı bir Ortak parametresiz örnek oluşturucusu bildirme ***varsayılan oluşturucu***.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-128">All value types implicitly declare a public parameterless instance constructor called the ***default constructor***.</span></span> <span data-ttu-id="f0c6a-129">Varsayılan oluşturucu olarak bilinen sıfır başlatılmayan bir örnek döndürür ***varsayılan değer*** değer türü için:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-129">The default constructor returns a zero-initialized instance known as the ***default value*** for the value type:</span></span>

*  <span data-ttu-id="f0c6a-130">Tüm *simple_type*s, varsayılan değer: değerin sıfır bit deseni tarafından üretilen:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-130">For all *simple_type*s, the default value is the value produced by a bit pattern of all zeros:</span></span>
    * <span data-ttu-id="f0c6a-131">İçin `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, ve `ulong`, varsayılan değer `0`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-131">For `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, and `ulong`, the default value is `0`.</span></span>
    * <span data-ttu-id="f0c6a-132">İçin `char`, varsayılan değer `'\x0000'`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-132">For `char`, the default value is `'\x0000'`.</span></span>
    * <span data-ttu-id="f0c6a-133">İçin `float`, varsayılan değer `0.0f`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-133">For `float`, the default value is `0.0f`.</span></span>
    * <span data-ttu-id="f0c6a-134">İçin `double`, varsayılan değer `0.0d`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-134">For `double`, the default value is `0.0d`.</span></span>
    * <span data-ttu-id="f0c6a-135">İçin `decimal`, varsayılan değer `0.0m`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-135">For `decimal`, the default value is `0.0m`.</span></span>
    * <span data-ttu-id="f0c6a-136">İçin `bool`, varsayılan değer `false`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-136">For `bool`, the default value is `false`.</span></span>
*  <span data-ttu-id="f0c6a-137">İçin bir *enum_type* `E`, varsayılan değer `0`, türe dönüştürülebilir `E`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-137">For an *enum_type* `E`, the default value is `0`, converted to the type `E`.</span></span>
*  <span data-ttu-id="f0c6a-138">İçin bir *struct_type*, varsayılan değer tür alanları için tüm değer tür alanları varsayılan değerlerine ve tüm başvuru ayarlayarak üretilen `null`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-138">For a *struct_type*, the default value is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>
*  <span data-ttu-id="f0c6a-139">İçin bir *nullable_type* örneği, varsayılan değer: `HasValue` özelliği false ve `Value` özelliği tanımsız.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-139">For a *nullable_type* the default value is an instance for which the `HasValue` property is false and the `Value` property is undefined.</span></span> <span data-ttu-id="f0c6a-140">Varsayılan değer olarak da bilinir: ***null değer*** boş değer atanabilir tür.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-140">The default value is also known as the ***null value*** of the nullable type.</span></span>

<span data-ttu-id="f0c6a-141">Diğer örnek oluşturucusu gibi bir değer türünün varsayılan oluşturucu kullanılarak çağrılan `new` işleci.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-141">Like any other instance constructor, the default constructor of a value type is invoked using the `new` operator.</span></span> <span data-ttu-id="f0c6a-142">Verimliliği nedeniyle, bu gereksinim aslında bir oluşturucu çağrı oluşturma uygulama sağlamak için tasarlanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-142">For efficiency reasons, this requirement is not intended to actually have the implementation generate a constructor call.</span></span> <span data-ttu-id="f0c6a-143">Değişkenleri aşağıdaki örnekte `i` ve `j` her ikisi de sıfır olarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-143">In the example below, variables `i` and `j` are both initialized to zero.</span></span>

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

<span data-ttu-id="f0c6a-144">Her değer türü örtük olarak bir Ortak parametresiz örnek oluşturucusu olduğundan, bir açık parametresiz bir oluşturucu bildirimi içeren bir yapı türü için mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-144">Because every value type implicitly has a public parameterless instance constructor, it is not possible for a struct type to contain an explicit declaration of a parameterless constructor.</span></span> <span data-ttu-id="f0c6a-145">Bir yapı türü, ancak parametreli örnek oluşturucuları bildirme izni ([oluşturucular](structs.md#constructors)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-145">A struct type is however permitted to declare parameterized instance constructors ([Constructors](structs.md#constructors)).</span></span>

### <a name="struct-types"></a><span data-ttu-id="f0c6a-146">Yapı türleri</span><span class="sxs-lookup"><span data-stu-id="f0c6a-146">Struct types</span></span>

<span data-ttu-id="f0c6a-147">Bir yapı türü sabitleri, alanları, yöntemleri, özellikleri, Dizinleyicileri, işleçleri, örnek oluşturucuları, statik oluşturucular ve iç içe geçmiş türler bildirebilirsiniz bir değer türüdür.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-147">A struct type is a value type that can declare constants, fields, methods, properties, indexers, operators, instance constructors, static constructors, and nested types.</span></span> <span data-ttu-id="f0c6a-148">Yapı türleri bildirimi açıklanan [yapı bildirimleri](structs.md#struct-declarations).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-148">The declaration of struct types is described in [Struct declarations](structs.md#struct-declarations).</span></span>

### <a name="simple-types"></a><span data-ttu-id="f0c6a-149">Basit türler</span><span class="sxs-lookup"><span data-stu-id="f0c6a-149">Simple types</span></span>

<span data-ttu-id="f0c6a-150">C# adlı önceden tanımlanmış bir yapı türleri kümesi sağlar ***basit türler***.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-150">C# provides a set of predefined struct types called the ***simple types***.</span></span> <span data-ttu-id="f0c6a-151">Basit türler ayrılmış sözcükler tanımlanır, ancak yalnızca önceden tanımlanmış bir yapı türleri için diğer adlar şu ayrılmış sözcüklerdir `System` aşağıdaki tabloda açıklandığı gibi ad alanı.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-151">The simple types are identified through reserved words, but these reserved words are simply aliases for predefined struct types in the `System` namespace, as described in the table below.</span></span>


| <span data-ttu-id="f0c6a-152">__Ayrılmış bir sözcük__</span><span class="sxs-lookup"><span data-stu-id="f0c6a-152">__Reserved word__</span></span> | <span data-ttu-id="f0c6a-153">__Diğer adlı türü__</span><span class="sxs-lookup"><span data-stu-id="f0c6a-153">__Aliased type__</span></span> |
|-------------------|------------------|
| `sbyte`           | `System.SByte`   | 
| `byte`            | `System.Byte`    | 
| `short`           | `System.Int16`   | 
| `ushort`          | `System.UInt16`  | 
| `int`             | `System.Int32`   | 
| `uint`            | `System.UInt32`  | 
| `long`            | `System.Int64`   | 
| `ulong`           | `System.UInt64`  | 
| `char`            | `System.Char`    | 
| `float`           | `System.Single`  | 
| `double`          | `System.Double`  | 
| `bool`            | `System.Boolean` | 
| `decimal`         | `System.Decimal` | 

<span data-ttu-id="f0c6a-154">Bir yapı türü diğer adlar basit bir tür olduğundan, her bir basit tür üyeleri var.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-154">Because a simple type aliases a struct type, every simple type has members.</span></span> <span data-ttu-id="f0c6a-155">Örneğin, `int` bildirilmiş üyeleri `System.Int32` ve üyeleri devralınan `System.Object`, ve aşağıdaki deyimleri izin verilir:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-155">For example, `int` has the members declared in `System.Int32` and the members inherited from `System.Object`, and the following statements are permitted:</span></span>

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

<span data-ttu-id="f0c6a-156">Basit türler, bazı ek işlemler izin vermek, diğer yapı türlerden farklılık gösterir:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-156">The simple types differ from other struct types in that they permit certain additional operations:</span></span>

*  <span data-ttu-id="f0c6a-157">En basit türler değerleri yazılarak oluşturulmak izin *değişmez değerleri* ([değişmez değerleri](lexical-structure.md#literals)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-157">Most simple types permit values to be created by writing *literals* ([Literals](lexical-structure.md#literals)).</span></span> <span data-ttu-id="f0c6a-158">Örneğin, `123` bir sabit değer türü olan `int` ve `'a'` bir sabit değer türü olan `char`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-158">For example, `123` is a literal of type `int` and `'a'` is a literal of type `char`.</span></span> <span data-ttu-id="f0c6a-159">C# değişmez değerler için hiçbir sağlama yapı türleri, genel yapar ve diğer yapı türlerinin varsayılan olmayan değerleri sonuçta her zaman bu yapı türü örnek oluşturucuları oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-159">C# makes no provision for literals of struct types in general, and non-default values of other struct types are ultimately always created through instance constructors of those struct types.</span></span>
*  <span data-ttu-id="f0c6a-160">Bir ifadenin işlenenleri tüm basit tür sabit olduğunda, derleyici derleme zamanında ifadeyi değerlendirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-160">When the operands of an expression are all simple type constants, it is possible for the compiler to evaluate the expression at compile-time.</span></span> <span data-ttu-id="f0c6a-161">Böyle bir ifade olarak da bilinen bir *constant_expression* ([sabit ifadeler](expressions.md#constant-expressions)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-161">Such an expression is known as a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)).</span></span> <span data-ttu-id="f0c6a-162">Diğer yapı türleri tarafından tanımlanan işleçleri içeren ifadeler sabit ifadeler olması için dikkate alınmaz.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-162">Expressions involving operators defined by other struct types are not considered to be constant expressions.</span></span>
*  <span data-ttu-id="f0c6a-163">Aracılığıyla `const` bildirimleri mümkündür basit türler sabitler ([sabitleri](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-163">Through `const` declarations it is possible to declare constants of the simple types ([Constants](classes.md#constants)).</span></span> <span data-ttu-id="f0c6a-164">Diğer yapı tür sabitleri mümkün değildir, ancak benzer bir etkisi tarafından sağlanan `static readonly` alanları.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-164">It is not possible to have constants of other struct types, but a similar effect is provided by `static readonly` fields.</span></span>
*  <span data-ttu-id="f0c6a-165">Basit türler içeren dönüştürmeler diğer yapı türleri tarafından tanımlanan dönüşüm işleçleri değerlendirmesini katılıyor, ancak kullanıcı tanımlı dönüştürme işleci hiçbir zaman içinde başka bir kullanıcı tanımlı işleç değerlendirme katılabilir ([değerlendirmesi Kullanıcı tanımlı dönüşümler](conversions.md#evaluation-of-user-defined-conversions)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-165">Conversions involving simple types can participate in evaluation of conversion operators defined by other struct types, but a user-defined conversion operator can never participate in evaluation of another user-defined operator ([Evaluation of user-defined conversions](conversions.md#evaluation-of-user-defined-conversions)).</span></span>

### <a name="integral-types"></a><span data-ttu-id="f0c6a-166">Tam sayı türleri</span><span class="sxs-lookup"><span data-stu-id="f0c6a-166">Integral types</span></span>

<span data-ttu-id="f0c6a-167">C# dokuz tamsayı türlerini destekler: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, ve `char`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-167">C# supports nine integral types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, and `char`.</span></span> <span data-ttu-id="f0c6a-168">Aşağıdaki boyutları ve değerler aralığı tam sayı türleri vardır:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-168">The integral types have the following sizes and ranges of values:</span></span>

*  <span data-ttu-id="f0c6a-169">`sbyte` İmzalı -128 ile 127 arasında değerlerle 8 bit tamsayı türünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-169">The `sbyte` type represents signed 8-bit integers with values between -128 and 127.</span></span>
*  <span data-ttu-id="f0c6a-170">`byte` Türünü işaretsiz 8 bit tam sayı 0 ile 255 arasında değerlerle temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-170">The `byte` type represents unsigned 8-bit integers with values between 0 and 255.</span></span>
*  <span data-ttu-id="f0c6a-171">`short` Türü temsil imzalı -32768 ile 32767 arasında değerlerle 16 bitlik tamsayı.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-171">The `short` type represents signed 16-bit integers with values between -32768 and 32767.</span></span>
*  <span data-ttu-id="f0c6a-172">`ushort` Türünü işaretsiz 16 bit tam sayı 0 ile 65535 arasında değerlerle temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-172">The `ushort` type represents unsigned 16-bit integers with values between 0 and 65535.</span></span>
*  <span data-ttu-id="f0c6a-173">`int` İmzalı 32-bit tamsayı değerleri -2147483648 ile 2147483647 arasında olan türü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-173">The `int` type represents signed 32-bit integers with values between -2147483648 and 2147483647.</span></span>
*  <span data-ttu-id="f0c6a-174">`uint` Türünü işaretsiz 32-bit tamsayı değerleri 0 ve 4294967295 arasında ile temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-174">The `uint` type represents unsigned 32-bit integers with values between 0 and 4294967295.</span></span>
*  <span data-ttu-id="f0c6a-175">`long` İmzalı -9223372036854775808 ile 9223372036854775807 arasındaki değerleri ile 64 bit tamsayı türü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-175">The `long` type represents signed 64-bit integers with values between -9223372036854775808 and 9223372036854775807.</span></span>
*  <span data-ttu-id="f0c6a-176">`ulong` Türünü işaretsiz 64-bit tamsayı değerleri 0 ile 18446744073709551615 arasında ile temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-176">The `ulong` type represents unsigned 64-bit integers with values between 0 and 18446744073709551615.</span></span>
*  <span data-ttu-id="f0c6a-177">`char` Türünü işaretsiz 16 bit tam sayı 0 ile 65535 arasında değerlerle temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-177">The `char` type represents unsigned 16-bit integers with values between 0 and 65535.</span></span> <span data-ttu-id="f0c6a-178">İçin olası değerler kümesini `char` UNICODE karakter kümesine karşılık gelen türü.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-178">The set of possible values for the `char` type corresponds to the Unicode character set.</span></span> <span data-ttu-id="f0c6a-179">Ancak `char` aynı gösterime sahip `ushort`, diğer tüm işlemler bir yola izin izin verilir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-179">Although `char` has the same representation as `ushort`, not all operations permitted on one type are permitted on the other.</span></span>

<span data-ttu-id="f0c6a-180">İkili işleçler ve integral türünden birli imzalı 32-bit duyarlığa, işaretsiz 32-bit duyarlığa, imzalı 64-bit duyarlığa veya işaretsiz 64-bit kesinliği ile her zaman çalışır:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-180">The integral-type unary and binary operators always operate with signed 32-bit precision, unsigned 32-bit precision, signed 64-bit precision, or unsigned 64-bit precision:</span></span>

*  <span data-ttu-id="f0c6a-181">Birli için `+` ve `~` işleçleri, işlenenin türüne dönüştürülür `T`burada `T` rehberin `int`, `uint`, `long`, ve `ulong` tam olarak temsil edebilen tüm Olası değerler işlenenin.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-181">For the unary `+` and `~` operators, the operand is converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="f0c6a-182">Duyarlık türü kullanarak işlemi gerçekleştirilir `T`, ve sonuç türü `T`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-182">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span>
*  <span data-ttu-id="f0c6a-183">Birli için `-` işleci, işlenenin türüne dönüştürülür `T`burada `T` rehberin `int` ve `long` tam olarak temsil edebilen işlenenin tüm olası değerler.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-183">For the unary `-` operator, the operand is converted to type `T`, where `T` is the first of `int` and `long` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="f0c6a-184">Duyarlık türü kullanarak işlemi gerçekleştirilir `T`, ve sonuç türü `T`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-184">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span> <span data-ttu-id="f0c6a-185">Birli `-` işleci türündeki işlenenlere uygulanamaz `ulong`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-185">The unary `-` operator cannot be applied to operands of type `ulong`.</span></span>
*  <span data-ttu-id="f0c6a-186">İkili için `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`, ve `<=` işleçler işlenenlerin türüne dönüştürülür `T`burada `T` rehberin `int`, `uint`, `long`, ve `ulong` tam olarak temsil edebilen tüm olası Her iki işlenen de değerleri.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-186">For the binary `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`, and `<=` operators, the operands are converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of both operands.</span></span> <span data-ttu-id="f0c6a-187">Duyarlık türü kullanarak işlemi gerçekleştirilir `T`, ve sonuç türü `T` (veya `bool` operatörler için).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-187">The operation is then performed using the precision of type `T`, and the type of the result is `T` (or `bool` for the relational operators).</span></span> <span data-ttu-id="f0c6a-188">Bir işlenen türünde olmasını verilmeyen `long` ve diğer türünde olmasını `ulong` ikili işleçlere sahip.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-188">It is not permitted for one operand to be of type `long` and the other to be of type `ulong` with the binary operators.</span></span>
*  <span data-ttu-id="f0c6a-189">İkili için `<<` ve `>>` işleçleri, sol işlenen türüne dönüştürülür `T`burada `T` rehberin `int`, `uint`, `long`, ve `ulong` tam olarak temsil edebilen tüm Olası değerler işlenenin.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-189">For the binary `<<` and `>>` operators, the left operand is converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="f0c6a-190">Duyarlık türü kullanarak işlemi gerçekleştirilir `T`, ve sonuç türü `T`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-190">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span>

<span data-ttu-id="f0c6a-191">`char` Türü bir tam sayı türü olarak sınıflandırılan, ancak diğer tamsayı türlerinin iki şekilde farklıdır:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-191">The `char` type is classified as an integral type, but it differs from the other integral types in two ways:</span></span>

*  <span data-ttu-id="f0c6a-192">Diğer türlerinden herhangi bir örtük dönüştürme vardır `char` türü.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-192">There are no implicit conversions from other types to the `char` type.</span></span> <span data-ttu-id="f0c6a-193">Özellikle, olsa bile `sbyte`, `byte`, ve `ushort` türlerine sahip tam olarak gösterilebilir kullanarak olan değerleri aralığı `char` türüne örtük dönüşümlere `sbyte`, `byte`, veya `ushort` için `char` mevcut değil.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-193">In particular, even though the `sbyte`, `byte`, and `ushort` types have ranges of values that are fully representable using the `char` type, implicit conversions from `sbyte`, `byte`, or `ushort` to `char` do not exist.</span></span>
*  <span data-ttu-id="f0c6a-194">Sabitleri `char` türü olarak yazılmalıdır *character_literal*s veya as *integer_literal*yazmanız için bir yayın birlikte s `char`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-194">Constants of the `char` type must be written as *character_literal*s or as *integer_literal*s in combination with a cast to type `char`.</span></span> <span data-ttu-id="f0c6a-195">Örneğin, `(char)10` aynı `'\x000A'`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-195">For example, `(char)10` is the same as `'\x000A'`.</span></span>

<span data-ttu-id="f0c6a-196">`checked` Ve `unchecked` işleçler ve ifadeler Tamsayı türünde aritmetik işlemler ve dönüştürmeler için denetleme taşma denetlemek için kullanılır ([checked ve unchecked işleçleri](expressions.md#the-checked-and-unchecked-operators)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-196">The `checked` and `unchecked` operators and statements are used to control overflow checking for integral-type arithmetic operations and conversions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span> <span data-ttu-id="f0c6a-197">İçinde bir `checked` bağlamı, taşma bir derleme zamanı hatası oluşturur veya neden olan bir `System.OverflowException` oluşturulması için.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-197">In a `checked` context, an overflow produces a compile-time error or causes a `System.OverflowException` to be thrown.</span></span> <span data-ttu-id="f0c6a-198">İçinde bir `unchecked` bağlamını, taşan yok sayılır ve hedef türüne uygun değil herhangi bir yüksek sıra bitleri atılır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-198">In an `unchecked` context, overflows are ignored and any high-order bits that do not fit in the destination type are discarded.</span></span>

### <a name="floating-point-types"></a><span data-ttu-id="f0c6a-199">Kayan nokta türleri</span><span class="sxs-lookup"><span data-stu-id="f0c6a-199">Floating point types</span></span>

<span data-ttu-id="f0c6a-200">C# destekleyen iki kayan nokta türleri: `float` ve `double`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-200">C# supports two floating point types: `float` and `double`.</span></span> <span data-ttu-id="f0c6a-201">`float` Ve `double` türleri, aşağıdaki değerleri kümesi sağlayan 32-bit tek duyarlıklı ve 64-bit çift duyarlıklı IEEE 754 biçimlerini kullanarak gösterilir:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-201">The `float` and `double` types are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats, which provide the following sets of values:</span></span>

*  <span data-ttu-id="f0c6a-202">Pozitif sıfır ve sıfır negatif.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-202">Positive zero and negative zero.</span></span> <span data-ttu-id="f0c6a-203">Çoğu durumda, pozitif sıfır ve negatif sıfır aynı şekilde davranır basit değer sıfır, ancak belirli işlemleri ikisi arasında ayrım ([bölme işleci](expressions.md#division-operator)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-203">In most situations, positive zero and negative zero behave identically as the simple value zero, but certain operations distinguish between the two ([Division operator](expressions.md#division-operator)).</span></span>
*  <span data-ttu-id="f0c6a-204">Pozitif sonsuz ve negatif sonsuz.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-204">Positive infinity and negative infinity.</span></span> <span data-ttu-id="f0c6a-205">Sonsuz, sıfır olmayan bir sayı sıfıra bölme olarak bu işlemler tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-205">Infinities are produced by such operations as dividing a non-zero number by zero.</span></span> <span data-ttu-id="f0c6a-206">Örneğin, `1.0 / 0.0` Pozitif sonsuz verir ve `-1.0 / 0.0` konusundaki negatif sonsuz.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-206">For example, `1.0 / 0.0` yields positive infinity, and `-1.0 / 0.0` yields negative infinity.</span></span>
*  <span data-ttu-id="f0c6a-207">***Sayı değil*** genellikle kısaltılmış NaN değeri.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-207">The ***Not-a-Number*** value, often abbreviated NaN.</span></span> <span data-ttu-id="f0c6a-208">NaN'ler sıfır olarak sıfıra bölme gibi geçersiz kayan nokta işlemleri tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-208">NaNs are produced by invalid floating-point operations, such as dividing zero by zero.</span></span>
*  <span data-ttu-id="f0c6a-209">Sıfır olmayan değerler biçiminin sınırlı kümesini `s * m * 2^e`burada `s` 1 veya -1'dir ve `m` ve `e` belirli kayan nokta türüne göre belirlenir: İçin `float`, `0 < m < 2^24` ve `-149 <= e <= 104`ve `double`, `0 < m < 2^53` ve `1075 <= e <= 970`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-209">The finite set of non-zero values of the form `s * m * 2^e`, where `s` is 1 or -1, and `m` and `e` are determined by the particular floating-point type: For `float`, `0 < m < 2^24` and `-149 <= e <= 104`, and for `double`, `0 < m < 2^53` and `1075 <= e <= 970`.</span></span> <span data-ttu-id="f0c6a-210">Normalleştirilmemiş bir kayan noktalı sayılar, geçerli sıfır olmayan değerler olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-210">Denormalized floating-point numbers are considered valid non-zero values.</span></span>

<span data-ttu-id="f0c6a-211">`float` Türü yaklaşık aralığındaki değerleri temsil eden `1.5 * 10^-45` için `3.4 * 10^38` 7 basamak kesinliği ile.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-211">The `float` type can represent values ranging from approximately `1.5 * 10^-45` to `3.4 * 10^38` with a precision of 7 digits.</span></span>

<span data-ttu-id="f0c6a-212">`double` Türü yaklaşık aralığındaki değerleri temsil eden `5.0 * 10^-324` için `1.7 × 10^308` 15-16 basamak kesinliği ile.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-212">The `double` type can represent values ranging from approximately `5.0 * 10^-324` to `1.7 × 10^308` with a precision of 15-16 digits.</span></span>

<span data-ttu-id="f0c6a-213">İkili işlecinin işlenenleri kayan nokta türü ise, daha sonra diğer işlenen bir tamsayı türü veya bir kayan nokta türü olmalıdır ve işlem gibi değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-213">If one of the operands of a binary operator is of a floating-point type, then the other operand must be of an integral type or a floating-point type, and the operation is evaluated as follows:</span></span>

*  <span data-ttu-id="f0c6a-214">Ardından biri bir tamsayı türü ise, işlenen diğer işlenen kayan nokta türüne dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-214">If one of the operands is of an integral type, then that operand is converted to the floating-point type of the other operand.</span></span>
*  <span data-ttu-id="f0c6a-215">Daha sonra ya da işlenen türü ise `double`, diğer işlenen dönüştürülür `double`, en az kullanma işlemin gerçekleştirilmesinden `double` aralık ve duyarlık ve sonuç türü `double` (veya `bool` için İlişkisel işleçler:).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-215">Then, if either of the operands is of type `double`, the other operand is converted to `double`, the operation is performed using at least `double` range and precision, and the type of the result is `double` (or `bool` for the relational operators).</span></span>
*  <span data-ttu-id="f0c6a-216">Aksi takdirde işlemi en az kullanılarak gerçekleştirilir `float` aralık ve duyarlık ve sonuç türü `float` (veya `bool` operatörler için).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-216">Otherwise, the operation is performed using at least `float` range and precision, and the type of the result is `float` (or `bool` for the relational operators).</span></span>

<span data-ttu-id="f0c6a-217">Atama İşleçleri kayan nokta işleçlerin, hiçbir zaman özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-217">The floating-point operators, including the assignment operators, never produce exceptions.</span></span> <span data-ttu-id="f0c6a-218">Bunun yerine, olağanüstü durumlarda, kayan nokta işlemleri sıfır, sonsuzluk ve NaN, aşağıda açıklandığı gibi oluşturmak:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-218">Instead, in exceptional situations, floating-point operations produce zero, infinity, or NaN, as described below:</span></span>

*  <span data-ttu-id="f0c6a-219">Kayan noktalı bir işlemin sonucunu hedef biçimi depolayamayacak kadar küçükse, işlemin sonucunu pozitif sıfır veya negatif sıfır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-219">If the result of a floating-point operation is too small for the destination format, the result of the operation becomes positive zero or negative zero.</span></span>
*  <span data-ttu-id="f0c6a-220">Kayan noktalı bir işlemin sonucunu hedef biçimi çok büyük ise, işlemin sonucunu, Pozitif sonsuz veya negatif sonsuz olur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-220">If the result of a floating-point operation is too large for the destination format, the result of the operation becomes positive infinity or negative infinity.</span></span>
*  <span data-ttu-id="f0c6a-221">Kayan noktalı bir işlemin geçersiz ise, NaN işleminin sonucu olur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-221">If a floating-point operation is invalid, the result of the operation becomes NaN.</span></span>
*  <span data-ttu-id="f0c6a-222">İşlemin sonucu bir veya iki işlenenin de kayan noktalı bir işlemin NaN ise, NaN haline gelir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-222">If one or both operands of a floating-point operation is NaN, the result of the operation becomes NaN.</span></span>

<span data-ttu-id="f0c6a-223">Kayan nokta işlemleri, işlemin sonuç türü daha yüksek duyarlık ile gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-223">Floating-point operations may be performed with higher precision than the result type of the operation.</span></span> <span data-ttu-id="f0c6a-224">Örneğin, bazı donanım mimarileri büyük aralık ve duyarlık daha ile bir "uzun" veya "long double" kayan nokta türü desteği `double` yazın ve örtük olarak bu daha yüksek duyarlılık türünü kullanan tüm kayan nokta işlemleri gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-224">For example, some hardware architectures support an "extended" or "long double" floating-point type with greater range and precision than the `double` type, and implicitly perform all floating-point operations using this higher precision type.</span></span> <span data-ttu-id="f0c6a-225">Yalnızca ücret ödemeden aşırı performans gibi donanım mimarileri daha az hassas kayan nokta işlemleri gerçekleştirmek için yapılabilir ve C# daha yüksek duyarlılık türünün olmasını gerektiren bir uygulama hem performans hem de duyarlık kaybeder yerine sağlar tüm kayan nokta işlemleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-225">Only at excessive cost in performance can such hardware architectures be made to perform floating-point operations with less precision, and rather than require an implementation to forfeit both performance and precision, C# allows a higher precision type to be used for all floating-point operations.</span></span> <span data-ttu-id="f0c6a-226">Daha kesin sonuç sağlama dışında bu nadiren herhangi ölçülebilir etkilere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-226">Other than delivering more precise results, this rarely has any measurable effects.</span></span> <span data-ttu-id="f0c6a-227">Bununla birlikte, formun ifadelerde `x * y / z`nerede çarpma dışında bir sonuç üretir `double` aralığı, ancak sonraki bölme geçici sonucu geri getirir `double` aralık ifade olgusu değerlendirilen daha yüksek bir aralık içinde sonsuzluk yerine üretilecek sınırlı bir sonuç biçimi neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-227">However, in expressions of the form `x * y / z`, where the multiplication produces a result that is outside the `double` range, but the subsequent division brings the temporary result back into the `double` range, the fact that the expression is evaluated in a higher range format may cause a finite result to be produced instead of an infinity.</span></span>

### <a name="the-decimal-type"></a><span data-ttu-id="f0c6a-228">Decimal türü</span><span class="sxs-lookup"><span data-stu-id="f0c6a-228">The decimal type</span></span>

<span data-ttu-id="f0c6a-229">`decimal` Türüdür finansal ve parasal hesaplamalar için uygun bir 128-bit veri türü.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-229">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span> <span data-ttu-id="f0c6a-230">`decimal` Türü arasında değişen değerlerini temsil eden `1.0 * 10^-28` için yaklaşık `7.9 * 10^28` 28-29 belirgin basamağı ile.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-230">The `decimal` type can represent values ranging from `1.0 * 10^-28` to approximately `7.9 * 10^28` with 28-29 significant digits.</span></span>

<span data-ttu-id="f0c6a-231">Sınırlı tür değerleri kümesi `decimal` biçimindedir `(-1)^s * c * 10^-e`burada oturum `s` 0 veya 1, katsayısı `c` tarafından verilen `0 <= *c* < 2^96`ve ölçek `e` olduğu gibi `0 <= e <= 28`. `decimal` İmzalı sıfır, sonsuz veya NaN'ın türü desteklemiyor.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-231">The finite set of values of type `decimal` are of the form `(-1)^s * c * 10^-e`, where the sign `s` is 0 or 1, the coefficient `c` is given by `0 <= *c* < 2^96`, and the scale `e` is such that `0 <= e <= 28`.The `decimal` type does not support signed zeros, infinities, or NaN's.</span></span> <span data-ttu-id="f0c6a-232">A `decimal` yaklaşık on kuvveti Ölçeklendirildi 96 bitlik bir tamsayı olarak temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-232">A `decimal` is represented as a 96-bit integer scaled by a power of ten.</span></span> <span data-ttu-id="f0c6a-233">İçin `decimal`s mutlak bir değer ile kısa `1.0m`, 28 ondalık basamak için tam, ancak başka değerdir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-233">For `decimal`s with an absolute value less than `1.0m`, the value is exact to the 28th decimal place, but no further.</span></span> <span data-ttu-id="f0c6a-234">İçin `decimal`mutlak bir değer büyüktür veya eşittir s `1.0m`, 28 ya da 29 basamağa kadar kesin bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-234">For `decimal`s with an absolute value greater than or equal to `1.0m`, the value is exact to 28 or 29 digits.</span></span> <span data-ttu-id="f0c6a-235">Contrary için `float` ve `double` veri türlerini ondalık kesirli sayılar gibi 0,1 gösterilebileceği tam olarak `decimal` gösterimi.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-235">Contrary to the `float` and `double` data types, decimal fractional numbers such as 0.1 can be represented exactly in the `decimal` representation.</span></span> <span data-ttu-id="f0c6a-236">İçinde `float` ve `double` temsilleri böyle sayılardır genellikle sonsuz kesir bu gösterimler yuvarlama daha açık hale getirme hataları.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-236">In the `float` and `double` representations, such numbers are often infinite fractions, making those representations more prone to round-off errors.</span></span>

<span data-ttu-id="f0c6a-237">İkili işlecinin işlenenleri birini türde ise `decimal`, diğer işlenen bir tamsayı türü veya tür olmalıdır `decimal`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-237">If one of the operands of a binary operator is of type `decimal`, then the other operand must be of an integral type or of type `decimal`.</span></span> <span data-ttu-id="f0c6a-238">Bir tamsayı türü işlenen mevcutsa dönüştürülür `decimal` işlemi gerçekleştirilmeden önce.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-238">If an integral type operand is present, it is converted to `decimal` before the operation is performed.</span></span>

<span data-ttu-id="f0c6a-239">Türünün değerleri üzerinde bir işlemin sonucunu `decimal` olduğundan, tam bir sonucu (her işleç tanımlanmış olarak koruyucu ölçeklendirme) hesaplama ve ardından gösterimi uyacak şekilde yuvarlama sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-239">The result of an operation on values of type `decimal` is that which would result from calculating an exact result (preserving scale, as defined for each operator) and then rounding to fit the representation.</span></span> <span data-ttu-id="f0c6a-240">Sonuç yuvarlanır en yakın gösterilebilir değere ve bir sonuç yakın iki temsil edilebilir değerler, bir çift sayı olan en az önemli basamak konumu (Bu bilinen "banker yuvarlama gibi") değerine eşit olduğunda.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-240">Results are rounded to the nearest representable value, and, when a result is equally close to two representable values, to the value that has an even number in the least significant digit position (this is known as "banker's rounding").</span></span> <span data-ttu-id="f0c6a-241">Sıfır sonuç her zaman 0'ın bir oturum ve ölçeği 0 sahiptir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-241">A zero result always has a sign of 0 and a scale of 0.</span></span>

<span data-ttu-id="f0c6a-242">Eşit veya daha düşük bir değere ondalık bir aritmetik işlemi neden oluyorsa `5 * 10^-29` mutlak değeri, işlemin sonucunu sıfır olur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-242">If a decimal arithmetic operation produces a value less than or equal to `5 * 10^-29` in absolute value, the result of the operation becomes zero.</span></span> <span data-ttu-id="f0c6a-243">Varsa bir `decimal` aritmetik işlemi için çok büyük bir sonuç üretir `decimal` biçiminde bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-243">If a `decimal` arithmetic operation produces a result that is too large for the `decimal` format, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="f0c6a-244">`decimal` Türünde daha fazla duyarlık ancak daha küçük aralığından daha kayan nokta türleri.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-244">The `decimal` type has greater precision but smaller range than the floating-point types.</span></span> <span data-ttu-id="f0c6a-245">Bu nedenle, kayan nokta türlerinden dönüşümler `decimal` taşması özel durumları ve dönüşümlerse üretebilir `decimal` kayan nokta türleri için duyarlık kaybına neden.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-245">Thus, conversions from the floating-point types to `decimal` might produce overflow exceptions, and conversions from `decimal` to the floating-point types might cause loss of precision.</span></span> <span data-ttu-id="f0c6a-246">Bu nedenlerle, kayan nokta türleri arasında örtük dönüştürme işlemi yok mevcut ve `decimal`, ve açık atamaları kayan nokta karıştırmak mümkün değildir ve `decimal` işlenenler aynı ifadede.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-246">For these reasons, no implicit conversions exist between the floating-point types and `decimal`, and without explicit casts, it is not possible to mix floating-point and `decimal` operands in the same expression.</span></span>

### <a name="the-bool-type"></a><span data-ttu-id="f0c6a-247">Bool türü</span><span class="sxs-lookup"><span data-stu-id="f0c6a-247">The bool type</span></span>

<span data-ttu-id="f0c6a-248">`bool` Türünü, boolean mantıksal miktarlar temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-248">The `bool` type represents boolean logical quantities.</span></span> <span data-ttu-id="f0c6a-249">Olası değerler türü `bool` olan `true` ve `false`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-249">The possible values of type `bool` are `true` and `false`.</span></span>

<span data-ttu-id="f0c6a-250">Standart dönüştürme arasındaki farklara `bool` ve diğer türleri.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-250">No standard conversions exist between `bool` and other types.</span></span> <span data-ttu-id="f0c6a-251">Özellikle, `bool` türüdür ve tam sayı türlerinden ayrı ve `bool` değeri bir tamsayı değeri yerine ve bunun tersi de kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-251">In particular, the `bool` type is distinct and separate from the integral types, and a `bool` value cannot be used in place of an integral value, and vice versa.</span></span>

<span data-ttu-id="f0c6a-252">C ve C++ dillerinde, sıfır ayrılmaz veya kayan nokta değeri ya da bir null işaretçiyse boolean değerine dönüştürülebilir `false`, ve sıfır olmayan bir tamsayı veya kayan nokta değer veya null olmayan bir işaretçi boolean değerine dönüştürülebilir `true`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-252">In the C and C++ languages, a zero integral or floating-point value, or a null pointer can be converted to the boolean value `false`, and a non-zero integral or floating-point value, or a non-null pointer can be converted to the boolean value `true`.</span></span> <span data-ttu-id="f0c6a-253">C# içinde bu tür dönüştürmeler açıkça bir tamsayı veya kayan nokta değeri sıfıra karşılaştırma ya da açıkça bir nesne başvurusu karşılaştırma gerçekleştirilir `null`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-253">In C#, such conversions are accomplished by explicitly comparing an integral or floating-point value to zero, or by explicitly comparing an object reference to `null`.</span></span>

### <a name="enumeration-types"></a><span data-ttu-id="f0c6a-254">Numaralandırma türleri</span><span class="sxs-lookup"><span data-stu-id="f0c6a-254">Enumeration types</span></span>

<span data-ttu-id="f0c6a-255">Bir numaralandırma türü, adlandırılmış sabitler ile farklı bir türdür.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-255">An enumeration type is a distinct type with named constants.</span></span> <span data-ttu-id="f0c6a-256">Her sabit listesi türünde olmalıdır bir temel türü `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` veya `ulong`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-256">Every enumeration type has an underlying type, which must be `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="f0c6a-257">Numaralandırma türü değerleri kümesi, temel alınan tür değerleri kümesi ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-257">The set of values of the enumeration type is the same as the set of values of the underlying type.</span></span> <span data-ttu-id="f0c6a-258">Numaralandırma türünün değerlerini adlandırılmış sabitlerin değerleri için sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-258">Values of the enumeration type are not restricted to the values of the named constants.</span></span> <span data-ttu-id="f0c6a-259">Numaralandırma türleri numaralandırma bildirimleri tanımlanır ([Enum bildirimleri](enums.md#enum-declarations)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-259">Enumeration types are defined through enumeration declarations ([Enum declarations](enums.md#enum-declarations)).</span></span>

### <a name="nullable-types"></a><span data-ttu-id="f0c6a-260">Boş değer atanabilir tipler</span><span class="sxs-lookup"><span data-stu-id="f0c6a-260">Nullable types</span></span>

<span data-ttu-id="f0c6a-261">Boş değer atanabilir bir tür tüm değerleri temsil edebilen kendi ***temel alınan türü*** artı ek bir null değer.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-261">A nullable type can represent all values of its ***underlying type*** plus an additional null value.</span></span> <span data-ttu-id="f0c6a-262">Boş değer atanabilir bir tür yazılan `T?`burada `T` temel alınan türü.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-262">A nullable type is written `T?`, where `T` is the underlying type.</span></span> <span data-ttu-id="f0c6a-263">İçin Toplu özellik bu sözdizimidir `System.Nullable<T>`, ve iki birbirlerinin yerine kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-263">This syntax is shorthand for `System.Nullable<T>`, and the two forms can be used interchangeably.</span></span>

<span data-ttu-id="f0c6a-264">A ***NULL olmayan değer türü*** tersine dışında herhangi bir değer türü olan `System.Nullable<T>` ve kendi toplu `T?` (herhangi `T`), bir NULL olmayan değer türü (diğer bir deyişle, tüm kısıtlanmış bir tür parametresi artı tür parametresi ile bir `struct` kısıtlama).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-264">A ***non-nullable value type*** conversely is any value type other than `System.Nullable<T>` and its shorthand `T?` (for any `T`), plus any type parameter that is constrained to be a non-nullable value type (that is, any type parameter with a `struct` constraint).</span></span> <span data-ttu-id="f0c6a-265">`System.Nullable<T>` Türü için değer türü kısıtlaması belirtir `T` ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)), herhangi bir değer atanamayan değer türünün temel türünü null yapılabilir bir tür olabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-265">The `System.Nullable<T>` type specifies the value type constraint for `T` ([Type parameter constraints](classes.md#type-parameter-constraints)), which means that the underlying type of a nullable type can be any non-nullable value type.</span></span> <span data-ttu-id="f0c6a-266">Boş değer atanabilir bir tür temel türü, null yapılabilir bir tür veya bir başvuru türü olamaz.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-266">The underlying type of a nullable type cannot be a nullable type or a reference type.</span></span> <span data-ttu-id="f0c6a-267">Örneğin, `int??` ve `string?` geçersiz türleridir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-267">For example, `int??` and `string?` are invalid types.</span></span>

<span data-ttu-id="f0c6a-268">Boş değer atanabilir bir tür örneği `T?` iki genel salt okunur özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-268">An instance of a nullable type `T?` has two public read-only properties:</span></span>

*  <span data-ttu-id="f0c6a-269">A `HasValue` türünün özelliği `bool`</span><span class="sxs-lookup"><span data-stu-id="f0c6a-269">A `HasValue` property of type `bool`</span></span>
*  <span data-ttu-id="f0c6a-270">A `Value` türünün özelliği `T`</span><span class="sxs-lookup"><span data-stu-id="f0c6a-270">A `Value` property of type `T`</span></span>

<span data-ttu-id="f0c6a-271">Bir örneğin `HasValue` olan NULL olmayan söyledi true.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-271">An instance for which `HasValue` is true is said to be non-null.</span></span> <span data-ttu-id="f0c6a-272">Bilinen bir değerle null olmayan bir örnek içerir ve `Value` değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-272">A non-null instance contains a known value and `Value` returns that value.</span></span>

<span data-ttu-id="f0c6a-273">Bir örneğin `HasValue` olan false söyledi null olması.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-273">An instance for which `HasValue` is false is said to be null.</span></span> <span data-ttu-id="f0c6a-274">Null bir örnek, tanımlanmamış bir değere sahip.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-274">A null instance has an undefined value.</span></span> <span data-ttu-id="f0c6a-275">Okumaya `Value` null bir örneğini neden olan bir `System.InvalidOperationException` oluşturulması için.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-275">Attempting to read the `Value` of a null instance causes a `System.InvalidOperationException` to be thrown.</span></span> <span data-ttu-id="f0c6a-276">Erişme işlemi `Value` özelliği boş değer atanabilir bir örneği olarak adlandırılır ***açma***.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-276">The process of accessing the `Value` property of a nullable instance is referred to as ***unwrapping***.</span></span>

<span data-ttu-id="f0c6a-277">Varsayılan Oluşturucu, her bir boş değer atanabilir tür yanı sıra `T?` türünde tek bir bağımsız değişken bir ortak yapıcıya sahip `T`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-277">In addition to the default constructor, every nullable type `T?` has a public constructor that takes a single argument of type `T`.</span></span> <span data-ttu-id="f0c6a-278">Verilen bir değer `x` türü `T`, formun bir oluşturucu çağrı</span><span class="sxs-lookup"><span data-stu-id="f0c6a-278">Given a value `x` of type `T`, a constructor invocation of the form</span></span>

```csharp
new T?(x)
```
<span data-ttu-id="f0c6a-279">null olmayan bir örneğini oluşturur `T?` hangi `Value` özelliği `x`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-279">creates a non-null instance of `T?` for which the `Value` property is `x`.</span></span> <span data-ttu-id="f0c6a-280">Belirli bir değeri olarak adlandırılır için boş değer atanabilir bir tür null olmayan bir örneğini oluşturma işlemini ***sarmalama***.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-280">The process of creating a non-null instance of a nullable type for a given value is referred to as ***wrapping***.</span></span>

<span data-ttu-id="f0c6a-281">Örtük dönüştürmeleri web'da `null` literal `T?` ([Null sabit değer dönüştürme](conversions.md#null-literal-conversions)) ve `T` için `T?` ([boş değer atanabilir örtük dönüştürmelerin](conversions.md#implicit-nullable-conversions)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-281">Implicit conversions are available from the `null` literal to `T?` ([Null literal conversions](conversions.md#null-literal-conversions)) and from `T` to `T?` ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)).</span></span>

## <a name="reference-types"></a><span data-ttu-id="f0c6a-282">Başvuru türleri</span><span class="sxs-lookup"><span data-stu-id="f0c6a-282">Reference types</span></span>

<span data-ttu-id="f0c6a-283">Bir başvuru sınıfı türü, bir arabirim türü, bir dizi türü veya bir temsilci türü türüdür.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-283">A reference type is a class type, an interface type, an array type, or a delegate type.</span></span>

```antlr
reference_type
    : class_type
    | interface_type
    | array_type
    | delegate_type
    ;

class_type
    : type_name
    | 'object'
    | 'dynamic'
    | 'string'
    ;

interface_type
    : type_name
    ;

array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;

delegate_type
    : type_name
    ;
```

<span data-ttu-id="f0c6a-284">Bir başvuru bir başvuru türü değeri yanlış bir ***örneği*** türü olarak ikinci bilinen bir ***nesne***.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-284">A reference type value is a reference to an ***instance*** of the type, the latter known as an ***object***.</span></span> <span data-ttu-id="f0c6a-285">Özel değeri `null` tüm başvuru türleri ile uyumludur ve olmaması örneğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-285">The special value `null` is compatible with all reference types and indicates the absence of an instance.</span></span>

### <a name="class-types"></a><span data-ttu-id="f0c6a-286">Sınıf türleri</span><span class="sxs-lookup"><span data-stu-id="f0c6a-286">Class types</span></span>

<span data-ttu-id="f0c6a-287">Bir sınıf türü, veri üyeleri (sabitleri ve alanları), işlev üyeleri (yöntemler, özellikler, olaylar, dizin oluşturucular, işleçler, örnek oluşturucuları, yok ediciler ve statik oluşturucular) ve iç içe geçmiş türler içeren bir veri yapısını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-287">A class type defines a data structure that contains data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="f0c6a-288">Sınıf türleri, devralma, yapabildiği türetilmiş sınıfları genişletmek ve temel sınıflar uzmanlaşmış bir mekanizma destekler.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-288">Class types support inheritance, a mechanism whereby derived classes can extend and specialize base classes.</span></span> <span data-ttu-id="f0c6a-289">Sınıf türlerinin örneklerini kullanılarak oluşturulur *object_creation_expression*s ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-289">Instances of class types are created using *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>

<span data-ttu-id="f0c6a-290">Sınıf türleri açıklanmıştır [sınıfları](classes.md).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-290">Class types are described in [Classes](classes.md).</span></span>

<span data-ttu-id="f0c6a-291">Belirli önceden tanımlanmış sınıf türleri, aşağıdaki tabloda açıklandığı gibi C# dili, özel bir anlamı yoktur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-291">Certain predefined class types have special meaning in the C# language, as described in the table below.</span></span>


| <span data-ttu-id="f0c6a-292">__Sınıf türü__</span><span class="sxs-lookup"><span data-stu-id="f0c6a-292">__Class type__</span></span>     | <span data-ttu-id="f0c6a-293">__Açıklama__</span><span class="sxs-lookup"><span data-stu-id="f0c6a-293">__Description__</span></span>                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | <span data-ttu-id="f0c6a-294">Diğer tüm türlerin ultimate temel sınıf.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-294">The ultimate base class of all other types.</span></span> <span data-ttu-id="f0c6a-295">Bkz: [nesne türü](types.md#the-object-type).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-295">See [The object type](types.md#the-object-type).</span></span> | 
| `System.String`    | <span data-ttu-id="f0c6a-296">C# dilinin string türü.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-296">The string type of the C# language.</span></span> <span data-ttu-id="f0c6a-297">Bkz: [dize türü](types.md#the-string-type).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-297">See [The string type](types.md#the-string-type).</span></span>         |
| `System.ValueType` | <span data-ttu-id="f0c6a-298">Tüm değer türlerinin bir taban sınıf.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-298">The base class of all value types.</span></span> <span data-ttu-id="f0c6a-299">Bkz: [System.ValueType türü](types.md#the-systemvaluetype-type).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-299">See [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>          |
| `System.Enum`      | <span data-ttu-id="f0c6a-300">Tüm sabit listesi türleri temel sınıf.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-300">The base class of all enum types.</span></span> <span data-ttu-id="f0c6a-301">Bkz: [numaralandırmalar](enums.md).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-301">See [Enums](enums.md).</span></span>              |
| `System.Array`     | <span data-ttu-id="f0c6a-302">Tüm dizi türleri temel sınıf.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-302">The base class of all array types.</span></span> <span data-ttu-id="f0c6a-303">Bkz: [diziler](arrays.md).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-303">See [Arrays](arrays.md).</span></span>             |
| `System.Delegate`  | <span data-ttu-id="f0c6a-304">Tüm temsilci türlerinin bir taban sınıf.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-304">The base class of all delegate types.</span></span> <span data-ttu-id="f0c6a-305">Bkz: [Temsilciler](delegates.md).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-305">See [Delegates](delegates.md).</span></span>          |
| `System.Exception` | <span data-ttu-id="f0c6a-306">Tüm özel durum türlerini temel sınıfı.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-306">The base class of all exception types.</span></span> <span data-ttu-id="f0c6a-307">Bkz: [özel durumları](exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-307">See [Exceptions](exceptions.md).</span></span>         |

### <a name="the-object-type"></a><span data-ttu-id="f0c6a-308">Nesne türü</span><span class="sxs-lookup"><span data-stu-id="f0c6a-308">The object type</span></span>

<span data-ttu-id="f0c6a-309">`object` Sınıf türü, diğer tüm türlerin ultimate temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-309">The `object` class type is the ultimate base class of all other types.</span></span> <span data-ttu-id="f0c6a-310">C# ' de her tür doğrudan veya dolaylı olarak türetir `object` sınıf türü.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-310">Every type in C# directly or indirectly derives from the `object` class type.</span></span>

<span data-ttu-id="f0c6a-311">Anahtar sözcüğü `object` yalnızca önceden tanımlanmış sınıf için bir diğer addır `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-311">The keyword `object` is simply an alias for the predefined class `System.Object`.</span></span>

### <a name="the-dynamic-type"></a><span data-ttu-id="f0c6a-312">Dinamik tür</span><span class="sxs-lookup"><span data-stu-id="f0c6a-312">The dynamic type</span></span>

<span data-ttu-id="f0c6a-313">`dynamic` Yazın, gibi `object`, herhangi bir nesneye başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-313">The `dynamic` type, like `object`, can reference any object.</span></span> <span data-ttu-id="f0c6a-314">İşleçler türündeki ifadeler için uygulandığı zaman `dynamic`, bunların çözümü program çalıştırılıncaya kadar ertelenir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-314">When operators are applied to expressions of type `dynamic`, their resolution is deferred until the program is run.</span></span> <span data-ttu-id="f0c6a-315">Bu nedenle, işlecin yasal başvurulan nesneye uygulanamaz, derleme sırasında herhangi bir hata verilir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-315">Thus, if the operator cannot legally be applied to the referenced object, no error is given during compilation.</span></span> <span data-ttu-id="f0c6a-316">Çözümleme işlecinin çalışma zamanında başarısız olduğunda, bunun yerine bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-316">Instead an exception will be thrown when resolution of the operator fails at run-time.</span></span>

<span data-ttu-id="f0c6a-317">Amacı, ayrıntılı olarak açıklanan dinamik bağlama izin vermektir [dinamik bağlama](expressions.md#dynamic-binding).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-317">Its purpose is to allow dynamic binding, which is described in detail in [Dynamic binding](expressions.md#dynamic-binding).</span></span>

<span data-ttu-id="f0c6a-318">`dynamic` aynı olarak kabul edilir `object` aşağıdaki yönden hariç:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-318">`dynamic` is considered identical to `object` except in the following respects:</span></span>

*  <span data-ttu-id="f0c6a-319">Türündeki ifade üzerinde işlemler `dynamic` dinamik olarak bağlı olabilir ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-319">Operations on expressions of type `dynamic` can be dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span>
*  <span data-ttu-id="f0c6a-320">Anlam çıkarma ([anlam çıkarma](expressions.md#type-inference)) tercih eder `dynamic` üzerinden `object` hem de adayları olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-320">Type inference ([Type inference](expressions.md#type-inference)) will prefer `dynamic` over `object` if both are candidates.</span></span>

<span data-ttu-id="f0c6a-321">Bu denkliğin nedeniyle aşağıdakileri içerir:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-321">Because of this equivalence, the following holds:</span></span>

*  <span data-ttu-id="f0c6a-322">Bir örtük kimlik dönüştürme arasında `object` ve `dynamic`, aynı değiştirilirken olan oluşturulan türler arasında `dynamic` ile `object`</span><span class="sxs-lookup"><span data-stu-id="f0c6a-322">There is an implicit identity conversion between `object` and `dynamic`, and between constructed types that are the same when replacing `dynamic` with `object`</span></span>
*  <span data-ttu-id="f0c6a-323">Örtük ve açık dönüştürmeler gelen ve giden `object` ve ondan de geçerli `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-323">Implicit and explicit conversions to and from `object` also apply to and from `dynamic`.</span></span>
*  <span data-ttu-id="f0c6a-324">Aynı değiştirilirken olan yöntem imzaları `dynamic` ile `object` aynı imzaya olarak kabul edilir</span><span class="sxs-lookup"><span data-stu-id="f0c6a-324">Method signatures that are the same when replacing `dynamic` with `object` are considered the same signature</span></span>
*  <span data-ttu-id="f0c6a-325">Türü `dynamic` döndürsün olan `object` çalışma zamanında.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-325">The type `dynamic` is indistinguishable from `object` at run-time.</span></span>
*  <span data-ttu-id="f0c6a-326">Türündeki bir ifade `dynamic` şeklinde adlandırılan bir ***dinamik ifade***.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-326">An expression of the type `dynamic` is referred to as a ***dynamic expression***.</span></span>

### <a name="the-string-type"></a><span data-ttu-id="f0c6a-327">Dize türü</span><span class="sxs-lookup"><span data-stu-id="f0c6a-327">The string type</span></span>

<span data-ttu-id="f0c6a-328">`string` Türüdür doğrudan devralan bir korumalı sınıf türü `object`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-328">The `string` type is a sealed class type that inherits directly from `object`.</span></span> <span data-ttu-id="f0c6a-329">Örneklerini `string` sınıfı Unicode karakter dizeleri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-329">Instances of the `string` class represent Unicode character strings.</span></span>

<span data-ttu-id="f0c6a-330">Değerleri `string` tür dize değişmez değer olarak yazılabilir ([dize değişmez değerleri](lexical-structure.md#string-literals)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-330">Values of the `string` type can be written as string literals ([String literals](lexical-structure.md#string-literals)).</span></span>

<span data-ttu-id="f0c6a-331">Anahtar sözcüğü `string` yalnızca önceden tanımlanmış sınıf için bir diğer addır `System.String`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-331">The keyword `string` is simply an alias for the predefined class `System.String`.</span></span>

### <a name="interface-types"></a><span data-ttu-id="f0c6a-332">Arabirim türleri</span><span class="sxs-lookup"><span data-stu-id="f0c6a-332">Interface types</span></span>

<span data-ttu-id="f0c6a-333">Bir arabirim bir sözleşmeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-333">An interface defines a contract.</span></span> <span data-ttu-id="f0c6a-334">Bir sınıf ya da bir arabirimi uygulayan yapı bu sözleşmeye uymalıdır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-334">A class or struct that implements an interface must adhere to its contract.</span></span> <span data-ttu-id="f0c6a-335">Birden fazla temel Ara birimden arabirim devralabilir ve bir sınıf veya yapı birden fazla arabirim uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-335">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="f0c6a-336">Arabirim türlerinde açıklanmıştır [arabirimleri](interfaces.md).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-336">Interface types are described in [Interfaces](interfaces.md).</span></span>

### <a name="array-types"></a><span data-ttu-id="f0c6a-337">Dizi türleri</span><span class="sxs-lookup"><span data-stu-id="f0c6a-337">Array types</span></span>

<span data-ttu-id="f0c6a-338">Bir dizi hesaplanan dizinlerini erişilen sıfır veya daha fazla değişken içeren bir veri yapısıdır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-338">An array is a data structure that contains zero or more variables which are accessed through computed indices.</span></span> <span data-ttu-id="f0c6a-339">Dizinin öğeleri olarak da bilinen bir dizi içindeki tüm aynı türdeki değişkenlerdir ve bu tür dizinin öğe türü olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-339">The variables contained in an array, also called the elements of the array, are all of the same type, and this type is called the element type of the array.</span></span>

<span data-ttu-id="f0c6a-340">Dizi türleri açıklanmıştır [diziler](arrays.md).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-340">Array types are described in [Arrays](arrays.md).</span></span>

### <a name="delegate-types"></a><span data-ttu-id="f0c6a-341">Temsilci türleri</span><span class="sxs-lookup"><span data-stu-id="f0c6a-341">Delegate types</span></span>

<span data-ttu-id="f0c6a-342">Bir temsilci, bir veya daha fazla yönteme başvuran bir veri yapısıdır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-342">A delegate is a data structure that refers to one or more methods.</span></span> <span data-ttu-id="f0c6a-343">Örneği için yöntemleri, ayrıca karşılık gelen nesne örneklerini anlamına gelmektedir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-343">For instance methods, it also refers to their corresponding object instances.</span></span>

<span data-ttu-id="f0c6a-344">En yakın bir Temsilcide C veya C++ işlev işaretçisi eşdeğerdir, ancak bir işlev işaretçisine, yalnızca statik işlevler başvurabilir gelirken, bir temsilci hem statik başvuru ve yöntemleri örneği.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-344">The closest equivalent of a delegate in C or C++ is a function pointer, but whereas a function pointer can only reference static functions, a delegate can reference both static and instance methods.</span></span> <span data-ttu-id="f0c6a-345">İkinci durumda, yalnızca metodun giriş noktası bir başvuru, aynı zamanda Pro instanci objektu yöntemi çağırmak için bir başvuru temsilci depolar.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-345">In the latter case, the delegate stores not only a reference to the method's entry point, but also a reference to the object instance on which to invoke the method.</span></span>

<span data-ttu-id="f0c6a-346">Temsilci türleri açıklanmıştır [Temsilciler](delegates.md).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-346">Delegate types are described in [Delegates](delegates.md).</span></span>

## <a name="boxing-and-unboxing"></a><span data-ttu-id="f0c6a-347">Kutulama ve kutudan çıkarma</span><span class="sxs-lookup"><span data-stu-id="f0c6a-347">Boxing and unboxing</span></span>

<span data-ttu-id="f0c6a-348">Kutulama ve kutudan çıkarma kavramı, C# tür sistemi taşımaktadır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-348">The concept of boxing and unboxing is central to C#'s type system.</span></span> <span data-ttu-id="f0c6a-349">Arasında bir köprü sağlar *value_type*s ve *reference_type*s tarafından herhangi bir değerini izin veren bir *value_type* türü gelen ve giden dönüştürülecek `object`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-349">It provides a bridge between *value_type*s and *reference_type*s by permitting any value of a *value_type* to be converted to and from type `object`.</span></span> <span data-ttu-id="f0c6a-350">Kutulama ve kutudan çıkarma gibi herhangi bir türde bir değer sonuçta bir nesne olarak davranılıp tür sistemi birleşik bir görünümünü sağlar.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-350">Boxing and unboxing enables a unified view of the type system wherein a value of any type can ultimately be treated as an object.</span></span>

### <a name="boxing-conversions"></a><span data-ttu-id="f0c6a-351">Kutulama dönüştürmeler</span><span class="sxs-lookup"><span data-stu-id="f0c6a-351">Boxing conversions</span></span>

<span data-ttu-id="f0c6a-352">Kutulama dönüştürmesi izin veren bir *value_type* için örtük olarak dönüştürülmesine bir *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-352">A boxing conversion permits a *value_type* to be implicitly converted to a *reference_type*.</span></span> <span data-ttu-id="f0c6a-353">Aşağıdaki kutulama dönüştürmeler mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-353">The following boxing conversions exist:</span></span>

*  <span data-ttu-id="f0c6a-354">Herhangi *value_type* türüne `object`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-354">From any *value_type* to the type `object`.</span></span>
*  <span data-ttu-id="f0c6a-355">Herhangi *value_type* türüne `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-355">From any *value_type* to the type `System.ValueType`.</span></span>
*  <span data-ttu-id="f0c6a-356">Herhangi *non_nullable_value_type* herhangi *INTERFACE_TYPE* tarafından uygulanan *value_type*.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-356">From any *non_nullable_value_type* to any *interface_type* implemented by the *value_type*.</span></span>
*  <span data-ttu-id="f0c6a-357">Herhangi *nullable_type* herhangi *INTERFACE_TYPE* temel alınan türü tarafından uygulanan *nullable_type*.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-357">From any *nullable_type* to any *interface_type* implemented by the underlying type of the *nullable_type*.</span></span>
*  <span data-ttu-id="f0c6a-358">Herhangi *enum_type* türüne `System.Enum`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-358">From any *enum_type* to the type `System.Enum`.</span></span>
*  <span data-ttu-id="f0c6a-359">Herhangi *nullable_type* bir temel ile *enum_type* türüne `System.Enum`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-359">From any *nullable_type* with an underlying *enum_type* to the type `System.Enum`.</span></span>
*  <span data-ttu-id="f0c6a-360">Çalışma zamanında bir değer türünden bir başvuru türüne dönüştürme yukarı sona ererse örtük bir dönüştürme türü parametresi bir paketleme dönüştürmesi yürütülecek unutmayın ([tür parametreleri içeren örtük dönüştürmelerin](conversions.md#implicit-conversions-involving-type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-360">Note that an implicit conversion from a type parameter will be executed as a boxing conversion if at run-time it ends up converting from a value type to a reference type ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters)).</span></span>

<span data-ttu-id="f0c6a-361">Değerini kutulama bir *non_nullable_value_type* nesne örneğini tahsis etme ve kopyalama oluşur *non_nullable_value_type* değerini söz konusu örneğine dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-361">Boxing a value of a *non_nullable_value_type* consists of allocating an object instance and copying the *non_nullable_value_type* value into that instance.</span></span>

<span data-ttu-id="f0c6a-362">Değerini kutulama bir *nullable_type* null başvuru ise üretir `null` değeri (`HasValue` olan `false`), veya çözülme ve temeldeki değeri aksi kutulama sonucu.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-362">Boxing a value of a *nullable_type* produces a null reference if it is the `null` value (`HasValue` is `false`), or the result of unwrapping and boxing the underlying value otherwise.</span></span>

<span data-ttu-id="f0c6a-363">Gerçek değerini kutulama işlemini bir *non_nullable_value_type* en iyi genel varlığını imagining tarafından açıklanan ***kutulama sınıfı***, şu şekilde bildirilir yokmuş gibi davranır:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-363">The actual process of boxing a value of a *non_nullable_value_type* is best explained by imagining the existence of a generic ***boxing class***, which behaves as if it were declared as follows:</span></span>

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

<span data-ttu-id="f0c6a-364">Bir değer olarak kutulama `v` türü `T` ifadeyi yürüten artık oluşur `new Box<T>(v)`ve elde edilen örnek türünde bir değer döndüren `object`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-364">Boxing of a value `v` of type `T` now consists of executing the expression `new Box<T>(v)`, and returning the resulting instance as a value of type `object`.</span></span> <span data-ttu-id="f0c6a-365">Bu nedenle, deyimleri</span><span class="sxs-lookup"><span data-stu-id="f0c6a-365">Thus, the statements</span></span>
```csharp
int i = 123;
object box = i;
```
<span data-ttu-id="f0c6a-366">kavramsal olarak karşılık gelir</span><span class="sxs-lookup"><span data-stu-id="f0c6a-366">conceptually correspond to</span></span>
```csharp
int i = 123;
object box = new Box<int>(i);
```

<span data-ttu-id="f0c6a-367">Kutulama sınıfı gibi `Box<T>` yukarıdaki gerçekten yok ve aslında bir sınıf türünün kutulanmış değer dinamik türü desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-367">A boxing class like `Box<T>` above doesn't actually exist and the dynamic type of a boxed value isn't actually a class type.</span></span> <span data-ttu-id="f0c6a-368">Bunun yerine, kutulanmış bir değer türü `T` dinamik türünde `T`ve dinamik tür denetimi kullanılarak `is` işleci yalnızca türü başvuru `T`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-368">Instead, a boxed value of type `T` has the dynamic type `T`, and a dynamic type check using the `is` operator can simply reference type `T`.</span></span> <span data-ttu-id="f0c6a-369">Örneğin,</span><span class="sxs-lookup"><span data-stu-id="f0c6a-369">For example,</span></span>
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
<span data-ttu-id="f0c6a-370">dize çıkarır "`Box contains an int`" konsolunda.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-370">will output the string "`Box contains an int`" on the console.</span></span>

<span data-ttu-id="f0c6a-371">Kutulama dönüştürmesi Kutulu değer kopyalayarak anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-371">A boxing conversion implies making a copy of the value being boxed.</span></span> <span data-ttu-id="f0c6a-372">Bu dönüştürme farklı bir *reference_type* türüne `object`hangi değeri aynı örneğine başvurmak devam eder ve yalnızca daha az türetilmiş bir türü olarak kabul edilir `object`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-372">This is different from a conversion of a *reference_type* to type `object`, in which the value continues to reference the same instance and simply is regarded as the less derived type `object`.</span></span> <span data-ttu-id="f0c6a-373">Örneğin, bildirimi verilen</span><span class="sxs-lookup"><span data-stu-id="f0c6a-373">For example, given the declaration</span></span>
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
<span data-ttu-id="f0c6a-374">aşağıdaki deyimleri</span><span class="sxs-lookup"><span data-stu-id="f0c6a-374">the following statements</span></span>
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
<span data-ttu-id="f0c6a-375">' % s'değeri 10 konsolunda çünkü çıkış atamasını içinde oluşan örtük kutulama işlemi `p` için `box` değeri neden olur `p` kopyalanacak.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-375">will output the value 10 on the console because the implicit boxing operation that occurs in the assignment of `p` to `box` causes the value of `p` to be copied.</span></span> <span data-ttu-id="f0c6a-376">Vardı `Point` edilmiş bildirilen bir `class` bunun yerine, ' % s'değeri 20 çıkış olurdu, çünkü `p` ve `box` aynı örneğine başvuru.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-376">Had `Point` been declared a `class` instead, the value 20 would be output because `p` and `box` would reference the same instance.</span></span>

### <a name="unboxing-conversions"></a><span data-ttu-id="f0c6a-377">Kutudan çıkarma dönüştürme</span><span class="sxs-lookup"><span data-stu-id="f0c6a-377">Unboxing conversions</span></span>

<span data-ttu-id="f0c6a-378">Bir paketi açma dönüştürmesi izin veren bir *reference_type* açıkça dönüştürülmesini bir *value_type*.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-378">An unboxing conversion permits a *reference_type* to be explicitly converted to a *value_type*.</span></span> <span data-ttu-id="f0c6a-379">Aşağıdaki kutudan çıkarma dönüştürme vardır:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-379">The following unboxing conversions exist:</span></span>

*  <span data-ttu-id="f0c6a-380">Türünden `object` herhangi *value_type*.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-380">From the type `object` to any *value_type*.</span></span>
*  <span data-ttu-id="f0c6a-381">Türünden `System.ValueType` herhangi *value_type*.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-381">From the type `System.ValueType` to any *value_type*.</span></span>
*  <span data-ttu-id="f0c6a-382">Herhangi *INTERFACE_TYPE* herhangi *non_nullable_value_type* uygulayan *INTERFACE_TYPE*.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-382">From any *interface_type* to any *non_nullable_value_type* that implements the *interface_type*.</span></span>
*  <span data-ttu-id="f0c6a-383">Herhangi *INTERFACE_TYPE* herhangi *nullable_type* temel türü uygulayan *INTERFACE_TYPE*.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-383">From any *interface_type* to any *nullable_type* whose underlying type implements the *interface_type*.</span></span>
*  <span data-ttu-id="f0c6a-384">Türünden `System.Enum` herhangi *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-384">From the type `System.Enum` to any *enum_type*.</span></span>
*  <span data-ttu-id="f0c6a-385">Türünden `System.Enum` herhangi *nullable_type* bir temel ile *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-385">From the type `System.Enum` to any *nullable_type* with an underlying *enum_type*.</span></span>
*  <span data-ttu-id="f0c6a-386">Çalışma zamanında bir değer türüne başvuru türünden dönüştürme yukarı sona ererse açık bir dönüştürme için bir tür parametresi bir paketi açma dönüştürmesi yürütülecek unutmayın ([açık dinamik dönüştürmeler](conversions.md#explicit-dynamic-conversions)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-386">Note that an explicit conversion to a type parameter will be executed as an unboxing conversion if at run-time it ends up converting from a reference type to a value type ([Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions)).</span></span>

<span data-ttu-id="f0c6a-387">Bir kutudan çıkarma işlemi için bir *non_nullable_value_type* ilk nesne örneği, kutulanmış bir değer olan denetimini oluşur verilen *non_nullable_value_type*ve ardından değeri / kopyalama örneği.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-387">An unboxing operation to a *non_nullable_value_type* consists of first checking that the object instance is a boxed value of the given *non_nullable_value_type*, and then copying the value out of the instance.</span></span>

<span data-ttu-id="f0c6a-388">Kutudan çıkarma için bir *nullable_type* null değerini üretir *nullable_type* kaynak işlenen `null`, ya da temel alınan türü için nesne örneği kutudan çıkarma Sarmalanan sonucu *nullable_type* Aksi takdirde.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-388">Unboxing to a *nullable_type* produces the null value of the *nullable_type* if the source operand is `null`, or the wrapped result of unboxing the object instance to the underlying type of the *nullable_type* otherwise.</span></span>

<span data-ttu-id="f0c6a-389">Bir nesnenin bir paketi açma dönüştürmesi önceki bölümde açıklanan sanal kutulama sınıfa başvuran `box` için bir *value_type* `T` ifadeyi yürüten oluşur `((Box<T>)box).value`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-389">Referring to the imaginary boxing class described in the previous section, an unboxing conversion of an object `box` to a *value_type* `T` consists of executing the expression `((Box<T>)box).value`.</span></span> <span data-ttu-id="f0c6a-390">Bu nedenle, deyimleri</span><span class="sxs-lookup"><span data-stu-id="f0c6a-390">Thus, the statements</span></span>
```csharp
object box = 123;
int i = (int)box;
```
<span data-ttu-id="f0c6a-391">kavramsal olarak karşılık gelir</span><span class="sxs-lookup"><span data-stu-id="f0c6a-391">conceptually correspond to</span></span>
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

<span data-ttu-id="f0c6a-392">Bir kutudan çıkarma dönüştürme için bir verilen *non_nullable_value_type* çalışma zamanında başarılı olması için kaynak işlecinin değer, söz konusu paketlenmiş değere bir başvuru olmalıdır *non_nullable_value_type*.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-392">For an unboxing conversion to a given *non_nullable_value_type* to succeed at run-time, the value of the source operand must be a reference to a boxed value of that *non_nullable_value_type*.</span></span> <span data-ttu-id="f0c6a-393">Kaynak işlenen `null`, `System.NullReferenceException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-393">If the source operand is `null`, a `System.NullReferenceException` is thrown.</span></span> <span data-ttu-id="f0c6a-394">Kaynak işlenen uyumsuz bir nesneye bir başvuru ise bir `System.InvalidCastException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-394">If the source operand is a reference to an incompatible object, a `System.InvalidCastException` is thrown.</span></span>

<span data-ttu-id="f0c6a-395">Bir kutudan çıkarma dönüştürme için bir verilen *nullable_type* çalışma zamanında başarılı olması için kaynak işlecinin değer olmalıdır `null` veya arka plandaki paketlenmiş değere bir başvuru *non_nullable_value_type* , *nullable_type*.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-395">For an unboxing conversion to a given *nullable_type* to succeed at run-time, the value of the source operand must be either `null` or a reference to a boxed value of the underlying *non_nullable_value_type* of the *nullable_type*.</span></span> <span data-ttu-id="f0c6a-396">Kaynak işlenen uyumsuz bir nesneye bir başvuru ise bir `System.InvalidCastException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-396">If the source operand is a reference to an incompatible object, a `System.InvalidCastException` is thrown.</span></span>

## <a name="constructed-types"></a><span data-ttu-id="f0c6a-397">Oluşturulan türler</span><span class="sxs-lookup"><span data-stu-id="f0c6a-397">Constructed types</span></span>

<span data-ttu-id="f0c6a-398">Bir genel tür bildirimi kendisi tarafından yapıttan bir ***genel tür ilişkisiz*** "plan" uygulama yoluyla, birçok farklı türler oluşturmak için kullanılan ***tür bağımsız değişkenleri***.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-398">A generic type declaration, by itself, denotes an ***unbound generic type*** that is used as a "blueprint" to form many different types, by way of applying ***type arguments***.</span></span> <span data-ttu-id="f0c6a-399">Tür bağımsız değişkenleri köşeli ayraç içinde yazılan (`<` ve `>`) genel tür adını takip.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-399">The type arguments are written within angle brackets (`<` and `>`) immediately following the name of the generic type.</span></span> <span data-ttu-id="f0c6a-400">En az bir tür bağımsız değişkeni bir tür olarak adlandırılan bir ***oluşturulan türü***.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-400">A type that includes at least one type argument is called a ***constructed type***.</span></span> <span data-ttu-id="f0c6a-401">Oluşturulan bir tür içinde bir tür adı görünebilir dil birçok yerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-401">A constructed type can be used in most places in the language in which a type name can appear.</span></span> <span data-ttu-id="f0c6a-402">İlişkisiz bir genel tür içinde yalnızca kullanılabilir bir *typeof_expression* ([typeof işleci](expressions.md#the-typeof-operator)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-402">An unbound generic type can only be used within a *typeof_expression* ([The typeof operator](expressions.md#the-typeof-operator)).</span></span>

<span data-ttu-id="f0c6a-403">Oluşturulan türler de kullanılabilir ifadelerinde basit adları ([basit adları](expressions.md#simple-names)) veya bir üye erişirken ([üye erişimi](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-403">Constructed types can also be used in expressions as simple names ([Simple names](expressions.md#simple-names)) or when accessing a member ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="f0c6a-404">Olduğunda bir *namespace_or_type_name* Değerlendirilmiş, yalnızca genel türlerinin doğru sayısıyla türü parametreleri olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-404">When a *namespace_or_type_name* is evaluated, only generic types with the correct number of type parameters are considered.</span></span> <span data-ttu-id="f0c6a-405">Bu nedenle, farklı sayıda tür parametreleri türlerine sahip olduğu sürece farklı türlerini tanımlamak üzere aynı tanımlayıcıyı kullanın mümkündür.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-405">Thus, it is possible to use the same identifier to identify different types, as long as the types have different numbers of type parameters.</span></span> <span data-ttu-id="f0c6a-406">Bu, genel ve genel olmayan sınıflar aynı programda kullanırken yararlıdır:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-406">This is useful when mixing generic and non-generic classes in the same program:</span></span>

```csharp
namespace Widgets
{
    class Queue {...}
    class Queue<TElement> {...}
}

namespace MyApplication
{
    using Widgets;

    class X
    {
        Queue q1;            // Non-generic Widgets.Queue
        Queue<int> q2;       // Generic Widgets.Queue
    }
}
```

<span data-ttu-id="f0c6a-407">A *type_name* oluşturulan tür, tür parametreleri doğrudan belirtmeyen olsa da tanımlayabilir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-407">A *type_name* might identify a constructed type even though it doesn't specify type parameters directly.</span></span> <span data-ttu-id="f0c6a-408">Burada bir tür genel bir sınıf bildirimi içinde iç içe geçmiş ve yönelik ad araması içeren bildirim örnek türü örtük olarak kullanılan oluşabilir ([türlerin genel sınıflardaki](classes.md#nested-types-in-generic-classes)):</span><span class="sxs-lookup"><span data-stu-id="f0c6a-408">This can occur where a type is nested within a generic class declaration, and the instance type of the containing declaration is implicitly used for name lookup ([Nested types in generic classes](classes.md#nested-types-in-generic-classes)):</span></span>

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

<span data-ttu-id="f0c6a-409">Güvenli olmayan kod içinde oluşturulmuş bir türü olarak kullanılamaz bir *unmanaged_type* ([işaretçi türleri](unsafe-code.md#pointer-types)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-409">In unsafe code, a constructed type cannot be used as an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

### <a name="type-arguments"></a><span data-ttu-id="f0c6a-410">Tür bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="f0c6a-410">Type arguments</span></span>

<span data-ttu-id="f0c6a-411">Tür bağımsız değişken listesindeki her bağımsız değişken teknolojidir bir *türü*.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-411">Each argument in a type argument list is simply a *type*.</span></span>

```antlr
type_argument_list
    : '<' type_arguments '>'
    ;

type_arguments
    : type_argument (',' type_argument)*
    ;

type_argument
    : type
    ;
```

<span data-ttu-id="f0c6a-412">Güvenli olmayan kod ([güvenli olmayan kod](unsafe-code.md)), *type_argument* bir işaretçi türü olabilir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-412">In unsafe code ([Unsafe code](unsafe-code.md)), a *type_argument* may not be a pointer type.</span></span> <span data-ttu-id="f0c6a-413">Her tür bağımsız değişkeni tür parametresi buna karşılık gelen tüm kısıtlamalar karşılaması gerekir ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-413">Each type argument must satisfy any constraints on the corresponding type parameter ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>

### <a name="open-and-closed-types"></a><span data-ttu-id="f0c6a-414">Açık ve kapalı türleri</span><span class="sxs-lookup"><span data-stu-id="f0c6a-414">Open and closed types</span></span>

<span data-ttu-id="f0c6a-415">Tüm türleri olarak sınıflandırılabilir ***açın türleri*** veya ***kapalı türleri***.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-415">All types can be classified as either ***open types*** or ***closed types***.</span></span> <span data-ttu-id="f0c6a-416">Tür parametreleri içeren bir tür bir açık türdür.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-416">An open type is a type that involves type parameters.</span></span> <span data-ttu-id="f0c6a-417">Daha ayrıntılı belirtmek gerekirse:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-417">More specifically:</span></span>

*  <span data-ttu-id="f0c6a-418">Bir tür parametresi açık bir tür tanımlar.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-418">A type parameter defines an open type.</span></span>
*  <span data-ttu-id="f0c6a-419">Öğe türünün bir açık türdür ve yalnızca, bir dizi türü bir açık türdür.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-419">An array type is an open type if and only if its element type is an open type.</span></span>
*  <span data-ttu-id="f0c6a-420">Bir veya daha fazla tür bağımsız değişkenlerinden bir açık türdür ve yalnızca, oluşturulan tür bir açık türdür.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-420">A constructed type is an open type if and only if one or more of its type arguments is an open type.</span></span> <span data-ttu-id="f0c6a-421">Bir veya daha fazla tür bağımsız değişkenlerinden veya tür bağımsız değişkenlerini içeren kendi türünde bir açık türdür ve yalnızca, oluşturulan iç içe geçmiş tür bir açık türdür.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-421">A constructed nested type is an open type if and only if one or more of its type arguments or the type arguments of its containing type(s) is an open type.</span></span>

<span data-ttu-id="f0c6a-422">Kapalı bir tür açık bir tür değil bir türdür.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-422">A closed type is a type that is not an open type.</span></span>

<span data-ttu-id="f0c6a-423">Çalışma zamanında, tüm genel tür bildirimi içinde kod yürütülür tür bağımsız değişkenleri için genel bildirimi uygulama tarafından oluşturulan kapalı bir oluşturulmuş tür bağlamında.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-423">At run-time, all of the code within a generic type declaration is executed in the context of a closed constructed type that was created by applying type arguments to the generic declaration.</span></span> <span data-ttu-id="f0c6a-424">Her tür parametresi, genel tür içinde belirli bir çalışma zamanı türüne bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-424">Each type parameter within the generic type is bound to a particular run-time type.</span></span> <span data-ttu-id="f0c6a-425">Çalışma zamanı, tüm ifadeleri ve ifadeler her zaman kapalı türleriyle işleneceğini ve açık türler yalnızca derleme zamanı sırasında işlem oluşur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-425">The run-time processing of all statements and expressions always occurs with closed types, and open types occur only during compile-time processing.</span></span>

<span data-ttu-id="f0c6a-426">Her kapalı bir oluşturulmuş tür kendi herhangi diğer kapalı oluşturulan türler ile paylaşılmayan statik değişkenler kümesine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-426">Each closed constructed type has its own set of static variables, which are not shared with any other closed constructed types.</span></span> <span data-ttu-id="f0c6a-427">Açık bir tür çalışma zamanında mevcut olmadığından, açık bir türü ile ilişkili hiçbir statik değişkenler vardır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-427">Since an open type does not exist at run-time, there are no static variables associated with an open type.</span></span> <span data-ttu-id="f0c6a-428">İki kapalı oluşturulan türler, bunlar aynı ilişkisiz genel türünden oluşturulduğu ve bunların karşılık gelen tür bağımsız değişkenleri aynı türdeyse aynı türde olan.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-428">Two closed constructed types are the same type if they are constructed from the same unbound generic type, and their corresponding type arguments are the same type.</span></span>

### <a name="bound-and-unbound-types"></a><span data-ttu-id="f0c6a-429">İlişkili ve ilişkisiz türleri</span><span class="sxs-lookup"><span data-stu-id="f0c6a-429">Bound and unbound types</span></span>

<span data-ttu-id="f0c6a-430">Terim ***ilişkisiz türü*** genel olmayan bir tür veya ilişkisiz bir genel tür belirtir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-430">The term ***unbound type*** refers to a non-generic type or an unbound generic type.</span></span> <span data-ttu-id="f0c6a-431">Terim ***bağlı türü*** genel olmayan bir tür veya oluşturulan tür anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-431">The term ***bound type*** refers to a non-generic type or a constructed type.</span></span>

<span data-ttu-id="f0c6a-432">İlişkisiz bir türü bir tür bildirimi tarafından bildirilen bir varlığa başvurur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-432">An unbound type refers to the entity declared by a type declaration.</span></span> <span data-ttu-id="f0c6a-433">İlişkisiz bir genel tür kendisi bir tür değildir ve bir değişken, bağımsız değişken veya dönüş değeri türünü veya bir taban türü olarak kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-433">An unbound generic type is not itself a type, and cannot be used as the type of a variable, argument or return value, or as a base type.</span></span> <span data-ttu-id="f0c6a-434">Hangi ilişkisiz bir genel tür başvurulabilir yalnızca yapıdır `typeof` ifade ([typeof işleci](expressions.md#the-typeof-operator)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-434">The only construct in which an unbound generic type can be referenced is the `typeof` expression ([The typeof operator](expressions.md#the-typeof-operator)).</span></span>

### <a name="satisfying-constraints"></a><span data-ttu-id="f0c6a-435">Tatmin kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="f0c6a-435">Satisfying constraints</span></span>

<span data-ttu-id="f0c6a-436">Oluşturulan tür ya da genel yöntemin başvurduğu her sağlanan tür bağımsız değişkeni genel tür veya yöntem üzerinde bildirilen tür parametresi kısıtlamaları karşı denetlenir ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-436">Whenever a constructed type or generic method is referenced, the supplied type arguments are checked against the type parameter constraints declared on the generic type or method ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="f0c6a-437">Her `where` yan tümcesi, tür bağımsız değişkeni `A` karşılık gelen adlandırılmış tür parametresi işaretli karşı her kısıtlama şu şekilde:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-437">For each `where` clause, the type argument `A` that corresponds to the named type parameter is checked against each constraint as follows:</span></span>

*  <span data-ttu-id="f0c6a-438">Bir sınıf türü, bir arabirim türü veya tür parametresi kısıtlaması ise `C` kısıtlamasındaki görünen herhangi bir tür parametre kısıtlaması sağlanan tür bağımsız değişkenleri ile yerine temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-438">If the constraint is a class type, an interface type, or a type parameter, let `C` represent that constraint with the supplied type arguments substituted for any type parameters that appear in the constraint.</span></span> <span data-ttu-id="f0c6a-439">Kısıtlamasını karşılamak için bunu yazın çalışması olmalıdır `A` türüne dönüştürülebilir olduğundan `C` aşağıdakilerden biri olarak:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-439">To satisfy the constraint, it must be the case that type `A` is convertible to type `C` by one of the following:</span></span>
    * <span data-ttu-id="f0c6a-440">Bir kimlik dönüştürme ([kimlik dönüştürme](conversions.md#identity-conversion))</span><span class="sxs-lookup"><span data-stu-id="f0c6a-440">An identity conversion ([Identity conversion](conversions.md#identity-conversion))</span></span>
    * <span data-ttu-id="f0c6a-441">Örtük bir başvuru dönüştürmesi ([örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions))</span><span class="sxs-lookup"><span data-stu-id="f0c6a-441">An implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions))</span></span>
    * <span data-ttu-id="f0c6a-442">Kutulama dönüştürmesi ([kutulama dönüştürmeler](conversions.md#boxing-conversions)), tür A NULL olmayan bir değer türü olması şartıyla.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-442">A boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)), provided that type A is a non-nullable value type.</span></span>
    * <span data-ttu-id="f0c6a-443">Bir tür parametresi arasında'örtük bir başvuru, kutulama veya tür parametresi dönüştürme `A` için `C`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-443">An implicit reference, boxing or type parameter conversion from a type parameter `A` to `C`.</span></span>
*  <span data-ttu-id="f0c6a-444">Başvuru türü kısıtlaması kısıtlaması varsa (`class`), türü `A` aşağıdakilerden birini karşılaması gerekir:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-444">If the constraint is the reference type constraint (`class`), the type `A` must satisfy one of the following:</span></span>
    * <span data-ttu-id="f0c6a-445">`A` bir arabirim türü, sınıf türü, temsilci türü veya dizi türü olabilir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-445">`A` is an interface type, class type, delegate type or array type.</span></span> <span data-ttu-id="f0c6a-446">Unutmayın `System.ValueType` ve `System.Enum` bu kısıtlamasını başvuru türleridir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-446">Note that `System.ValueType` and `System.Enum` are reference types that satisfy this constraint.</span></span>
    * <span data-ttu-id="f0c6a-447">`A` bir başvuru türü olarak bilinen bir tür parametresi ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-447">`A` is a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="f0c6a-448">Değer türü kısıtlaması kısıtlaması varsa (`struct`), türü `A` aşağıdakilerden birini karşılaması gerekir:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-448">If the constraint is the value type constraint (`struct`), the type `A` must satisfy one of the following:</span></span>
    * <span data-ttu-id="f0c6a-449">`A` bir yapı türü veya sabit listesi türünde, ancak null yapılabilir bir tür değildir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-449">`A` is a struct type or enum type, but not a nullable type.</span></span> <span data-ttu-id="f0c6a-450">Unutmayın `System.ValueType` ve `System.Enum` bu kısıtlamasını karşılamaz başvuru türleridir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-450">Note that `System.ValueType` and `System.Enum` are reference types that do not satisfy this constraint.</span></span>
    * <span data-ttu-id="f0c6a-451">`A` değer türü kısıtlamasına sahip bir tür parametresi ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-451">`A` is a type parameter having the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="f0c6a-452">Kısıtlama Oluşturucu kısıtlaması ise `new()`, türü `A` olmamalıdır `abstract` ve ortak bir parametresiz oluşturucuya sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-452">If the constraint is the constructor constraint `new()`, the type `A` must not be `abstract` and must have a public parameterless constructor.</span></span> <span data-ttu-id="f0c6a-453">Aşağıdakilerden biri doğruysa uyulmuş olur:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-453">This is satisfied if one of the following is true:</span></span>
    * <span data-ttu-id="f0c6a-454">`A` tüm değer türleri ortak varsayılan oluşturucuya sahip bir değer türü olduğu ([varsayılan oluşturucular](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-454">`A` is a value type, since all value types have a public default constructor ([Default constructors](types.md#default-constructors)).</span></span>
    * <span data-ttu-id="f0c6a-455">`A` Oluşturucu kısıtlaması sahip bir tür parametresi ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-455">`A` is a type parameter having the constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
    * <span data-ttu-id="f0c6a-456">`A` değer türü kısıtlamasına sahip bir tür parametresi ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-456">`A` is a type parameter having the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
    * <span data-ttu-id="f0c6a-457">`A` olmayan bir sınıf olan `abstract` ve açıkça bildirilirse içeren `public` parametresiz oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-457">`A` is a class that is not `abstract` and contains an explicitly declared `public` constructor with no parameters.</span></span>
    * <span data-ttu-id="f0c6a-458">`A` değil `abstract` ve varsayılan bir oluşturucu vardır ([varsayılan oluşturucular](classes.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-458">`A` is not `abstract` and has a default constructor ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="f0c6a-459">Bir veya daha fazla tür parametresinin kısıtlamalarını tarafından belirtilen tür bağımsız değişkenlerini tatmin edici değil, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-459">A compile-time error occurs if one or more of a type parameter's constraints are not satisfied by the given type arguments.</span></span>

<span data-ttu-id="f0c6a-460">Tür parametreleri devralınmaz olduğundan, hiçbir zaman kısıtlamaları olan ya da devralındı.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-460">Since type parameters are not inherited, constraints are never inherited either.</span></span> <span data-ttu-id="f0c6a-461">Aşağıdaki örnekte `D` kısıtlaması, tür parametresinin belirtilmesi gerekiyor `T` böylece `T` taban sınıfı tarafından uygulanan kısıtlamasına `B<T>`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-461">In the example below, `D` needs to specify the constraint on its type parameter `T` so that `T` satisfies the constraint imposed by the base class `B<T>`.</span></span> <span data-ttu-id="f0c6a-462">Buna karşılık, sınıfının `E` bir kısıtlama nedeniyle belirtmemeniz gerekir `List<T>` uygulayan `IEnumerable` herhangi `T`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-462">In contrast, class `E` need not specify a constraint, because `List<T>` implements `IEnumerable` for any `T`.</span></span>

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a><span data-ttu-id="f0c6a-463">Tür parametreleri</span><span class="sxs-lookup"><span data-stu-id="f0c6a-463">Type parameters</span></span>

<span data-ttu-id="f0c6a-464">Bir tür parametresi, bir değer türü veya parametresi çalışma zamanında bağlı olduğu başvuru türüne atamak bir tanımlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-464">A type parameter is an identifier designating a value type or reference type that the parameter is bound to at run-time.</span></span>

```antlr
type_parameter
    : identifier
    ;
```

<span data-ttu-id="f0c6a-465">Bir tür parametresi, birçok farklı gerçek tür bağımsız değişkenleri ile oluşturulabilir olduğundan, Tür parametrelerine biraz daha farklı işlemler ve diğer türleri kısıtlamalara sahip.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-465">Since a type parameter can be instantiated with many different actual type arguments, type parameters have slightly different operations and restrictions than other types.</span></span> <span data-ttu-id="f0c6a-466">Bu güncelleştirmeler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-466">These include:</span></span>

*  <span data-ttu-id="f0c6a-467">Bir tür parametresi, doğrudan temel sınıf bildirmek için kullanılamaz ([temel sınıfı](classes.md#base-class)) veya arabirimi ([değişken türü parametre listeleri](interfaces.md#variant-type-parameter-lists)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-467">A type parameter cannot be used directly to declare a base class ([Base class](classes.md#base-class)) or interface ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)).</span></span>
*  <span data-ttu-id="f0c6a-468">Tür parametresi için parametreleri kısıtlamalar varsa, bağımlı türündeki üye araması kuralları uygulanır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-468">The rules for member lookup on type parameters depend on the constraints, if any, applied to the type parameter.</span></span> <span data-ttu-id="f0c6a-469">İçinde ayrıntılı [üye araması](expressions.md#member-lookup).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-469">They are detailed in [Member lookup](expressions.md#member-lookup).</span></span>
*  <span data-ttu-id="f0c6a-470">Bağımlı bir tür parametresi için kısıtlamalar varsa, kullanılabilir dönüştürmeler için tür parametresi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-470">The available conversions for a type parameter depend on the constraints, if any, applied to the type parameter.</span></span> <span data-ttu-id="f0c6a-471">İçinde ayrıntılı [tür parametreleri içeren örtük dönüştürmelerin](conversions.md#implicit-conversions-involving-type-parameters) ve [açık dinamik dönüştürmeler](conversions.md#explicit-dynamic-conversions).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-471">They are detailed in [Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters) and [Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions).</span></span>
*  <span data-ttu-id="f0c6a-472">Değişmez değer `null` tür parametresi bir başvuru türü olması biliniyorsa dışında bir tür parametresi tarafından belirtilen bir türe dönüştürülemez ([tür parametreleri içeren örtük dönüştürmelerin](conversions.md#implicit-conversions-involving-type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-472">The literal `null` cannot be converted to a type given by a type parameter, except if the type parameter is known to be a reference type ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters)).</span></span> <span data-ttu-id="f0c6a-473">Ancak, bir `default` ifade ([varsayılan değer ifadeleri](expressions.md#default-value-expressions)) bunun yerine kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-473">However, a `default` expression ([Default value expressions](expressions.md#default-value-expressions)) can be used instead.</span></span> <span data-ttu-id="f0c6a-474">Ayrıca, bir değer türü parametre tarafından sağlanan bir türü ile karşılaştırılabilir `null` kullanarak `==` ve `!=` ([başvuru türü eşitlik işleçleri](expressions.md#reference-type-equality-operators)) değer türü kısıtlaması tür parametresi sahip olmadığı sürece.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-474">In addition, a value with a type given by a type parameter can be compared with `null` using `==` and `!=` ([Reference type equality operators](expressions.md#reference-type-equality-operators)) unless the type parameter has the value type constraint.</span></span>
*  <span data-ttu-id="f0c6a-475">A `new` ifade ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)) tarafından tür parametresi kısıtlı yalnızca bir tür parametreyle kullanılabilir bir *constructor_constraint* veya kısıtlama (türüdeğeri[ Tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-475">A `new` expression ([Object creation expressions](expressions.md#object-creation-expressions)) can only be used with a type parameter if the type parameter is constrained by a *constructor_constraint* or the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="f0c6a-476">Bir tür parametresi, her yerde bir öznitelik içinde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-476">A type parameter cannot be used anywhere within an attribute.</span></span>
*  <span data-ttu-id="f0c6a-477">Bir tür parametresi bir üye erişimi kullanılamaz ([üye erişimi](expressions.md#member-access)) veya tür adı ([Namespace ve tür adları](basic-concepts.md#namespace-and-type-names)) statik bir üyeye veya iç içe geçmiş bir tür tanımlamak için.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-477">A type parameter cannot be used in a member access ([Member access](expressions.md#member-access)) or type name ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) to identify a static member or a nested type.</span></span>
*  <span data-ttu-id="f0c6a-478">Güvenli olmayan kod içinde bir tür parametresi olarak kullanılamaz bir *unmanaged_type* ([işaretçi türleri](unsafe-code.md#pointer-types)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-478">In unsafe code, a type parameter cannot be used as an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

<span data-ttu-id="f0c6a-479">Bir türü olarak yalnızca bir derleme zamanı yapısı tür parametreleri var.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-479">As a type, type parameters are purely a compile-time construct.</span></span> <span data-ttu-id="f0c6a-480">Çalışma zamanında, her tür parametresi tür bağımsız değişkeni genel tür bildirimine sağlanarak belirtilen çalışma zamanı türüne bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-480">At run-time, each type parameter is bound to a run-time type that was specified by supplying a type argument to the generic type declaration.</span></span> <span data-ttu-id="f0c6a-481">Bu nedenle, kapalı bir oluşturulmuş tür olabilir bir tür parametresi gerçekleştirilse çalışma zamanı ile bir değişken türü bildirilen ([açık ve kapalı türleri](types.md#open-and-closed-types)).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-481">Thus, the type of a variable declared with a type parameter will, at run-time, be a closed constructed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="f0c6a-482">Çalışma zamanı yürütme tüm ifadeleri ve tür parametreleri içeren ifadeler sağlanmadı gerçek tür, parametre türü bağımsız değişkeni olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-482">The run-time execution of all statements and expressions involving type parameters uses the actual type that was supplied as the type argument for that parameter.</span></span>

## <a name="expression-tree-types"></a><span data-ttu-id="f0c6a-483">İfade ağacı türleri</span><span class="sxs-lookup"><span data-stu-id="f0c6a-483">Expression tree types</span></span>

<span data-ttu-id="f0c6a-484">***İfade ağaçları*** lambda ifadeleri olarak yürütülebilir kod yerine veri yapılarını temsil edilmesini izin verir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-484">***Expression trees*** permit lambda expressions to be represented as data structures instead of executable code.</span></span> <span data-ttu-id="f0c6a-485">İfade ağaçları olan değerleri ***ifade ağacı türleri*** formun `System.Linq.Expressions.Expression<D>`burada `D` herhangi bir temsilci türü.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-485">Expression trees are values of ***expression tree types*** of the form `System.Linq.Expressions.Expression<D>`, where `D` is any delegate type.</span></span> <span data-ttu-id="f0c6a-486">Bu belirtim geri kalanı için biz kısaltmalar kullanarak bu türlerine başvuracaktır `Expression<D>`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-486">For the remainder of this specification we will refer to these types using the shorthand `Expression<D>`.</span></span>

<span data-ttu-id="f0c6a-487">Bir temsilci türüne dönüştürme bir lambda ifadesinden varsa `D`, bir dönüştürme da ifade ağaç türüne olduğunu `Expression<D>`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-487">If a conversion exists from a lambda expression to a delegate type `D`, a conversion also exists to the expression tree type `Expression<D>`.</span></span> <span data-ttu-id="f0c6a-488">Dönüştürme bir temsilci türü için bir lambda ifadesinin lambda ifadesi için yürütülebilir koda başvuran bir temsilci davranırken bir ifade ağacı türüne dönüştürme, lambda ifadesi bir ifade ağacı temsilini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-488">Whereas the conversion of a lambda expression to a delegate type generates a delegate that references executable code for the lambda expression, conversion to an expression tree type creates an expression tree representation of the lambda expression.</span></span>

<span data-ttu-id="f0c6a-489">İfade ağaçları lambda ifadeleri verimli bellek içi verilerin temsillerini olan ve lambda ifadesinin yapısı saydam ve açık hale getirir.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-489">Expression trees are efficient in-memory data representations of lambda expressions and make the structure of the lambda expression transparent and explicit.</span></span>

<span data-ttu-id="f0c6a-490">Bir temsilci türü'olduğu gibi `D`, `Expression<D>` parametre ve dönüş türleri için kabul edilir, aynı olduğu `D`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-490">Just like a delegate type `D`, `Expression<D>` is said to have parameter and return types, which are the same as those of `D`.</span></span>

<span data-ttu-id="f0c6a-491">Aşağıdaki örnek, bir lambda ifadesi yürütülebilir kod hem bir ifade ağacı olarak temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-491">The following example represents a lambda expression both as executable code and as an expression tree.</span></span> <span data-ttu-id="f0c6a-492">Bir dönüştürme olmadığından `Func<int,int>`, dönüştürme için de mevcut `Expression<Func<int,int>>`:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-492">Because a conversion exists to `Func<int,int>`, a conversion also exists to `Expression<Func<int,int>>`:</span></span>

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

<span data-ttu-id="f0c6a-493">Bu atamaları, temsilci aşağıdaki `del` döndüren bir yöntemin başvurduğu `x + 1`ve ifade ağacı `exp` ifade tanımlayan bir veri yapısı başvuran `x => x + 1`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-493">Following these assignments, the delegate `del` references a method that returns `x + 1`, and the expression tree `exp` references a data structure that describes the expression `x => x + 1`.</span></span>

<span data-ttu-id="f0c6a-494">Genel türün tam tanımı `Expression<D>` bir lambda ifadesi bir ifade ağacı türüne dönüştürülürken bir ifade ağacı oluşturmak için kesin yanı sıra, her ikisi de bu belirtim kapsamı dışında kurallardır.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-494">The exact definition of the generic type `Expression<D>` as well as the precise rules for constructing an expression tree when a lambda expression is converted to an expression tree type, are both outside the scope of this specification.</span></span>

<span data-ttu-id="f0c6a-495">İki şey açık hale getirmek önemlidir:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-495">Two things are important to make explicit:</span></span>

*  <span data-ttu-id="f0c6a-496">Tüm lambda ifadeleri ifade ağaçlarına dönüştürülemez.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-496">Not all lambda expressions can be converted to expression trees.</span></span> <span data-ttu-id="f0c6a-497">Örneğin, deyim gövdesi olan lambda ifadeleri ve atama ifadeleri içeren lambda ifadeleri ifade edilemez.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-497">For instance, lambda expressions with statement bodies, and lambda expressions containing assignment expressions cannot be represented.</span></span> <span data-ttu-id="f0c6a-498">Bu gibi durumlarda, bir dönüştürme hala var, ancak derleme zamanında başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-498">In these cases, a conversion still exists, but will fail at compile-time.</span></span> <span data-ttu-id="f0c6a-499">Bu özel durum ayrıntıları [anonim işlev dönüştürmeler](conversions.md#anonymous-function-conversions).</span><span class="sxs-lookup"><span data-stu-id="f0c6a-499">These exceptions are detailed in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>
*   <span data-ttu-id="f0c6a-500">`Expression<D>` bir örnek yöntemi sunar `Compile` bir temsilci türü üretir `D`:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-500">`Expression<D>` offers an instance method `Compile` which produces a delegate of type `D`:</span></span>

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    <span data-ttu-id="f0c6a-501">Bu temsilci çağırma yürütülecek bir ifade ağacı tarafından temsil edilen koda neden olur.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-501">Invoking this delegate causes the code represented by the expression tree to be executed.</span></span> <span data-ttu-id="f0c6a-502">Bu nedenle, yukarıdaki tanımları göz önünde bulundurulduğunda, del ve del2 eşdeğerdir ve aşağıdaki iki deyimi aynı etkiye sahip olur:</span><span class="sxs-lookup"><span data-stu-id="f0c6a-502">Thus, given the definitions above, del and del2 are equivalent, and the following two statements will have the same effect:</span></span>

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    <span data-ttu-id="f0c6a-503">Bu kod çalıştırıldıktan sonra `i1` ve `i2` hem de değeri alacaktır. `2`.</span><span class="sxs-lookup"><span data-stu-id="f0c6a-503">After executing this code,  `i1` and `i2` will both have the value `2`.</span></span>

