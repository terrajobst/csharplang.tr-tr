---
ms.openlocfilehash: 088c4a77cecde490c556c44c239a3496f896582e
ms.sourcegitcommit: 4ddf18d000734c1b6d0a48127bf338086fc3f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73616135"
---
# <a name="types"></a><span data-ttu-id="e7f76-101">Türler</span><span class="sxs-lookup"><span data-stu-id="e7f76-101">Types</span></span>

<span data-ttu-id="e7f76-102">C# Dilin türleri iki ana kategoriye ayrılmıştır: ***değer türleri*** ve ***başvuru türleri***.</span><span class="sxs-lookup"><span data-stu-id="e7f76-102">The types of the C# language are divided into two main categories: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="e7f76-103">Değer türleri ve başvuru türleri, bir veya daha fazla ***tür parametresi***alacak ***Genel türler***olabilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-103">Both value types and reference types may be ***generic types***, which take one or more ***type parameters***.</span></span> <span data-ttu-id="e7f76-104">Tür parametreleri hem değer türlerini hem de başvuru türlerini belirleyebilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-104">Type parameters can designate both value types and reference types.</span></span>

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

<span data-ttu-id="e7f76-105">Türlerin son kategorisi, işaretçiler, yalnızca güvenli olmayan kodda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-105">The final category of types, pointers, is available only in unsafe code.</span></span> <span data-ttu-id="e7f76-106">Bu, [işaretçi türlerinde](unsafe-code.md#pointer-types)daha fazla ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-106">This is discussed further in [Pointer types](unsafe-code.md#pointer-types).</span></span>

<span data-ttu-id="e7f76-107">Değer türleri, değer türlerinin değişkenlerinin verilerini doğrudan içerdiği başvuru türlerinden farklıdır, ancak başvuru türündeki değişkenler, ikinci olarak ***nesneler***olarak bilinmekte olan verilerine ***başvuruları*** depolar.</span><span class="sxs-lookup"><span data-stu-id="e7f76-107">Value types differ from reference types in that variables of the value types directly contain their data, whereas variables of the reference types store ***references*** to their data, the latter being known as ***objects***.</span></span> <span data-ttu-id="e7f76-108">Başvuru türleriyle, iki değişken aynı nesneye başvurabilir ve bu nedenle bir değişkende işlemler, diğer değişken tarafından başvurulan nesneyi etkilemek için mümkündür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-108">With reference types, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="e7f76-109">Değer türleriyle, her birinin kendi verilerinin bir kopyasına sahip olduğu ve bir üzerindeki işlemlerin diğerini etkilemesi mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-109">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span>

<span data-ttu-id="e7f76-110">C#tür sistemi, herhangi bir tür değeri bir nesne olarak işlenemeyeceği şekilde birleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-110">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="e7f76-111">Her tür doğrudan C# veya dolaylı olarak `object` sınıf türünden türetilir ve `object` tüm türlerin en son temel sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-111">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="e7f76-112">Başvuru türlerinin değerleri, `object`türü olarak değerleri görüntüleyerek nesne olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-112">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="e7f76-113">Değer türlerinin değerleri, kutulama ve kutudan çıkarma işlemleri ([kutulama ve kutudan](types.md#boxing-and-unboxing)çıkarma) gerçekleştirerek nesneler olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-113">Values of value types are treated as objects by performing boxing and unboxing operations ([Boxing and unboxing](types.md#boxing-and-unboxing)).</span></span>

## <a name="value-types"></a><span data-ttu-id="e7f76-114">Değer türleri</span><span class="sxs-lookup"><span data-stu-id="e7f76-114">Value types</span></span>

<span data-ttu-id="e7f76-115">Değer türü bir struct türü ya da bir numaralandırma türüdür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-115">A value type is either a struct type or an enumeration type.</span></span> <span data-ttu-id="e7f76-116">C#***basit türler***adlı önceden tanımlanmış bir yapı türleri kümesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="e7f76-116">C# provides a set of predefined struct types called the ***simple types***.</span></span> <span data-ttu-id="e7f76-117">Basit türler, ayrılmış sözcükler aracılığıyla tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-117">The simple types are identified through reserved words.</span></span>

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

<span data-ttu-id="e7f76-118">Bir başvuru türü değişkeninin aksine, bir değer türü değişkeni, yalnızca değer türü null yapılabilir bir tür ise `null` değerini içerebilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-118">Unlike a variable of a reference type, a variable of a value type can contain the value `null` only if the value type is a nullable type.</span></span>  <span data-ttu-id="e7f76-119">Null yapılamayan her değer türü için, aynı değer kümesini ve `null`değeri belirten karşılık gelen null yapılabilir bir değer türü vardır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-119">For every non-nullable value type there is a corresponding nullable value type denoting the same set of values plus the value `null`.</span></span>

<span data-ttu-id="e7f76-120">Değer türünde bir değişkene atama, atanmakta olan değerin bir kopyasını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-120">Assignment to a variable of a value type creates a copy of the value being assigned.</span></span> <span data-ttu-id="e7f76-121">Bu, atamadan bir başvuru türü değişkenine farklılık gösterir ve başvuruyu, başvuru tarafından tanımlanan nesneyi kopyalar.</span><span class="sxs-lookup"><span data-stu-id="e7f76-121">This differs from assignment to a variable of a reference type, which copies the reference but not the object identified by the reference.</span></span>

### <a name="the-systemvaluetype-type"></a><span data-ttu-id="e7f76-122">System. ValueType türü</span><span class="sxs-lookup"><span data-stu-id="e7f76-122">The System.ValueType type</span></span>

<span data-ttu-id="e7f76-123">Tüm değer türleri dolaylı olarak `System.ValueType`sınıfından devralır, bu, sırasıyla sınıf `object`devralır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-123">All value types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="e7f76-124">Herhangi bir türün bir değer türünden türemesini mümkün değildir ve değer türleri örtülü olarak mühürlenebilir ([korumalı sınıflardır](classes.md#sealed-classes)).</span><span class="sxs-lookup"><span data-stu-id="e7f76-124">It is not possible for any type to derive from a value type, and value types are thus implicitly sealed ([Sealed classes](classes.md#sealed-classes)).</span></span>

<span data-ttu-id="e7f76-125">`System.ValueType` kendisinin bir *value_type*olmadığı unutulmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-125">Note that `System.ValueType` is not itself a *value_type*.</span></span> <span data-ttu-id="e7f76-126">Bunun yerine, tüm *value_type*s 'nin otomatik olarak türetildiği bir *class_type* .</span><span class="sxs-lookup"><span data-stu-id="e7f76-126">Rather, it is a *class_type* from which all *value_type*s are automatically derived.</span></span>

### <a name="default-constructors"></a><span data-ttu-id="e7f76-127">Varsayılan oluşturucular</span><span class="sxs-lookup"><span data-stu-id="e7f76-127">Default constructors</span></span>

<span data-ttu-id="e7f76-128">Tüm değer türleri örtük olarak ***Varsayılan Oluşturucu***adlı ortak parametresiz bir örnek Oluşturucusu bildirir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-128">All value types implicitly declare a public parameterless instance constructor called the ***default constructor***.</span></span> <span data-ttu-id="e7f76-129">Varsayılan Oluşturucu değer türü için ***varsayılan değer*** olarak bilinen sıfır ile başlatılmış bir örnek döndürür:</span><span class="sxs-lookup"><span data-stu-id="e7f76-129">The default constructor returns a zero-initialized instance known as the ***default value*** for the value type:</span></span>

*  <span data-ttu-id="e7f76-130">Tüm *Simple_Type*s için, varsayılan değer tüm sıfırlardan oluşan bir bit düzeniyle oluşturulan değerdir:</span><span class="sxs-lookup"><span data-stu-id="e7f76-130">For all *simple_type*s, the default value is the value produced by a bit pattern of all zeros:</span></span>
    * <span data-ttu-id="e7f76-131">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`ve `ulong`için varsayılan değer `0`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-131">For `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, and `ulong`, the default value is `0`.</span></span>
    * <span data-ttu-id="e7f76-132">`char`için varsayılan değer `'\x0000'`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-132">For `char`, the default value is `'\x0000'`.</span></span>
    * <span data-ttu-id="e7f76-133">`float`için varsayılan değer `0.0f`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-133">For `float`, the default value is `0.0f`.</span></span>
    * <span data-ttu-id="e7f76-134">`double`için varsayılan değer `0.0d`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-134">For `double`, the default value is `0.0d`.</span></span>
    * <span data-ttu-id="e7f76-135">`decimal`için varsayılan değer `0.0m`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-135">For `decimal`, the default value is `0.0m`.</span></span>
    * <span data-ttu-id="e7f76-136">`bool`için varsayılan değer `false`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-136">For `bool`, the default value is `false`.</span></span>
*  <span data-ttu-id="e7f76-137">Bir *enum_type* `E`için, varsayılan değer `E`türüne dönüştürülür `0`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-137">For an *enum_type* `E`, the default value is `0`, converted to the type `E`.</span></span>
*  <span data-ttu-id="e7f76-138">Bir *struct_type*için varsayılan değer, tüm değer türü alanları varsayılan değerlerine ve tüm başvuru türü `null`alanlarına ayarlanarak oluşturulan değerdir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-138">For a *struct_type*, the default value is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>
*  <span data-ttu-id="e7f76-139">Bir *nullable_type* için varsayılan değer `HasValue` özelliğinin false olduğu ve `Value` özelliğinin tanımsız olduğu bir örneğidir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-139">For a *nullable_type* the default value is an instance for which the `HasValue` property is false and the `Value` property is undefined.</span></span> <span data-ttu-id="e7f76-140">Varsayılan değer null yapılabilir türün ***null değeri*** olarak da bilinir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-140">The default value is also known as the ***null value*** of the nullable type.</span></span>

<span data-ttu-id="e7f76-141">Diğer örnek Oluşturucu gibi, bir değer türünün varsayılan Oluşturucusu `new` işleci kullanılarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-141">Like any other instance constructor, the default constructor of a value type is invoked using the `new` operator.</span></span> <span data-ttu-id="e7f76-142">Verimlilik açısından, bu gereksinim aslında uygulamanın bir Oluşturucu çağrısı oluşturması için tasarlanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-142">For efficiency reasons, this requirement is not intended to actually have the implementation generate a constructor call.</span></span> <span data-ttu-id="e7f76-143">Aşağıdaki örnekte, `i` ve `j` değişkenleri sıfır olarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-143">In the example below, variables `i` and `j` are both initialized to zero.</span></span>

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

<span data-ttu-id="e7f76-144">Her değer türünün örtük olarak ortak parametresiz bir örnek Oluşturucusu olduğundan, bir yapı türünün parametresiz bir oluşturucunun açık bir bildirimini içermesi mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-144">Because every value type implicitly has a public parameterless instance constructor, it is not possible for a struct type to contain an explicit declaration of a parameterless constructor.</span></span> <span data-ttu-id="e7f76-145">Yapı türüne ancak parametreli örnek oluşturucuları ([oluşturucular](structs.md#constructors)) bildirme izni verilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-145">A struct type is however permitted to declare parameterized instance constructors ([Constructors](structs.md#constructors)).</span></span>

### <a name="struct-types"></a><span data-ttu-id="e7f76-146">Yapı türleri</span><span class="sxs-lookup"><span data-stu-id="e7f76-146">Struct types</span></span>

<span data-ttu-id="e7f76-147">Yapı türü, sabitler, alanlar, Yöntemler, özellikler, Dizin oluşturucular, işleçler, örnek oluşturucular, statik oluşturucular ve iç içe geçmiş türleri bildirebilen bir değer türüdür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-147">A struct type is a value type that can declare constants, fields, methods, properties, indexers, operators, instance constructors, static constructors, and nested types.</span></span> <span data-ttu-id="e7f76-148">Struct türlerinin bildirimi [struct bildirimlerinde](structs.md#struct-declarations)açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-148">The declaration of struct types is described in [Struct declarations](structs.md#struct-declarations).</span></span>

### <a name="simple-types"></a><span data-ttu-id="e7f76-149">Basit türler</span><span class="sxs-lookup"><span data-stu-id="e7f76-149">Simple types</span></span>

<span data-ttu-id="e7f76-150">C#***basit türler***adlı önceden tanımlanmış bir yapı türleri kümesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="e7f76-150">C# provides a set of predefined struct types called the ***simple types***.</span></span> <span data-ttu-id="e7f76-151">Basit türler ayrılmış sözcükler aracılığıyla tanımlanır, ancak aşağıdaki tabloda açıklandığı gibi, bu ayrılmış sözcükler `System` ad alanındaki önceden tanımlanmış yapı türleri için diğer adlardır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-151">The simple types are identified through reserved words, but these reserved words are simply aliases for predefined struct types in the `System` namespace, as described in the table below.</span></span>


| <span data-ttu-id="e7f76-152">__Ayrılmış sözcük__</span><span class="sxs-lookup"><span data-stu-id="e7f76-152">__Reserved word__</span></span> | <span data-ttu-id="e7f76-153">__Diğer ad türü__</span><span class="sxs-lookup"><span data-stu-id="e7f76-153">__Aliased type__</span></span> |
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

<span data-ttu-id="e7f76-154">Basit bir tür bir struct türü olarak diğer ad olduğundan, her basit türün üyeleri vardır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-154">Because a simple type aliases a struct type, every simple type has members.</span></span> <span data-ttu-id="e7f76-155">Örneğin, `int`, `System.Int32` ve `System.Object`devralınmış Üyeler ve aşağıdaki deyimlerde belirtilen üyelere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="e7f76-155">For example, `int` has the members declared in `System.Int32` and the members inherited from `System.Object`, and the following statements are permitted:</span></span>

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

<span data-ttu-id="e7f76-156">Basit türler, bazı ek işlemlere izin veren diğer yapı türlerinden farklıdır:</span><span class="sxs-lookup"><span data-stu-id="e7f76-156">The simple types differ from other struct types in that they permit certain additional operations:</span></span>

*  <span data-ttu-id="e7f76-157">Çoğu basit tür değerleri *değişmez* değerler ([değişmez](lexical-structure.md#literals)değerler) yazılarak oluşturulacak şekilde izin verir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-157">Most simple types permit values to be created by writing *literals* ([Literals](lexical-structure.md#literals)).</span></span> <span data-ttu-id="e7f76-158">Örneğin, `123` `int` türünde bir değişmez değerdir ve `'a'` `char`türünde bir sabit değerdir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-158">For example, `123` is a literal of type `int` and `'a'` is a literal of type `char`.</span></span> <span data-ttu-id="e7f76-159">C#, genel olarak yapı türlerinin değişmez değerleri için bir provizyon yapmaz ve diğer yapı türlerinin varsayılan olmayan değerleri, sonuçta bu yapı türlerinin örnek oluşturucular aracılığıyla her zaman oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-159">C# makes no provision for literals of struct types in general, and non-default values of other struct types are ultimately always created through instance constructors of those struct types.</span></span>
*  <span data-ttu-id="e7f76-160">Bir ifadenin işlenenleri tüm basit tür sabitlersiyse, derleyicinin derlemeyi derleme zamanında değerlendirmesi mümkündür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-160">When the operands of an expression are all simple type constants, it is possible for the compiler to evaluate the expression at compile-time.</span></span> <span data-ttu-id="e7f76-161">Böyle bir ifade, *constant_expression* ([sabit ifadeler](expressions.md#constant-expressions)) olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-161">Such an expression is known as a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)).</span></span> <span data-ttu-id="e7f76-162">Diğer yapı türleri tarafından tanımlanan işleçleri içeren ifadeler sabit ifadeler olarak kabul edilmez.</span><span class="sxs-lookup"><span data-stu-id="e7f76-162">Expressions involving operators defined by other struct types are not considered to be constant expressions.</span></span>
*  <span data-ttu-id="e7f76-163">`const` bildirimleri aracılığıyla basit türlerin ([sabitler](classes.md#constants)) sabitlerini bildirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-163">Through `const` declarations it is possible to declare constants of the simple types ([Constants](classes.md#constants)).</span></span> <span data-ttu-id="e7f76-164">Diğer yapı türlerinin sabitlerinin olması mümkün değildir, ancak `static readonly` alanlar tarafından benzer bir efekt sağlanır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-164">It is not possible to have constants of other struct types, but a similar effect is provided by `static readonly` fields.</span></span>
*  <span data-ttu-id="e7f76-165">Basit türler içeren dönüştürmeler, diğer yapı türleri tarafından tanımlanan dönüştürme işleçleri değerlendirmesinde yer alabilir, ancak kullanıcı tanımlı bir dönüştürme işleci hiçbir şekilde Kullanıcı tanımlı başka bir işlecin değerlendirilmesine katılamaz ([değerlendirmesi Kullanıcı tanımlı dönüştürmeler](conversions.md#evaluation-of-user-defined-conversions)).</span><span class="sxs-lookup"><span data-stu-id="e7f76-165">Conversions involving simple types can participate in evaluation of conversion operators defined by other struct types, but a user-defined conversion operator can never participate in evaluation of another user-defined operator ([Evaluation of user-defined conversions](conversions.md#evaluation-of-user-defined-conversions)).</span></span>

### <a name="integral-types"></a><span data-ttu-id="e7f76-166">Integral türleri</span><span class="sxs-lookup"><span data-stu-id="e7f76-166">Integral types</span></span>

<span data-ttu-id="e7f76-167">C#Dokuz integral türünü destekler: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`ve `char`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-167">C# supports nine integral types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, and `char`.</span></span> <span data-ttu-id="e7f76-168">İntegral türleri aşağıdaki boyutlara ve değer aralıklarına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="e7f76-168">The integral types have the following sizes and ranges of values:</span></span>

*  <span data-ttu-id="e7f76-169">`sbyte` türü,-128 ile 127 arasında değerleri olan işaretli 8 bit tamsayıları temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e7f76-169">The `sbyte` type represents signed 8-bit integers with values between -128 and 127.</span></span>
*  <span data-ttu-id="e7f76-170">`byte` türü, 0 ile 255 arasında değerler içeren işaretsiz 8 bit tamsayılar temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e7f76-170">The `byte` type represents unsigned 8-bit integers with values between 0 and 255.</span></span>
*  <span data-ttu-id="e7f76-171">`short` türü,-32768 ile 32767 arasında değerleri olan işaretli 16 bit tamsayıları temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e7f76-171">The `short` type represents signed 16-bit integers with values between -32768 and 32767.</span></span>
*  <span data-ttu-id="e7f76-172">`ushort` türü, 0 ile 65535 arasında değerler içeren işaretsiz 16 bit tamsayılar temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e7f76-172">The `ushort` type represents unsigned 16-bit integers with values between 0 and 65535.</span></span>
*  <span data-ttu-id="e7f76-173">`int` türü,-2147483648 ve 2147483647 değerlerini içeren imzalı 32 bitlik tamsayılar temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e7f76-173">The `int` type represents signed 32-bit integers with values between -2147483648 and 2147483647.</span></span>
*  <span data-ttu-id="e7f76-174">`uint` türü, 0 ile 4294967295 arasındaki değerlere sahip işaretsiz 32 bitlik tamsayılar temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e7f76-174">The `uint` type represents unsigned 32-bit integers with values between 0 and 4294967295.</span></span>
*  <span data-ttu-id="e7f76-175">`long` türü,-9223372036854775808 ve 9223372036854775807 değerlerini içeren imzalı 64 bitlik tamsayıları temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e7f76-175">The `long` type represents signed 64-bit integers with values between -9223372036854775808 and 9223372036854775807.</span></span>
*  <span data-ttu-id="e7f76-176">`ulong` türü, 0 ile 18446744073709551615 arasında değerler içeren işaretsiz 64 bitlik tamsayılar temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e7f76-176">The `ulong` type represents unsigned 64-bit integers with values between 0 and 18446744073709551615.</span></span>
*  <span data-ttu-id="e7f76-177">`char` türü, 0 ile 65535 arasında değerler içeren işaretsiz 16 bit tamsayılar temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e7f76-177">The `char` type represents unsigned 16-bit integers with values between 0 and 65535.</span></span> <span data-ttu-id="e7f76-178">`char` türü için olası değerler kümesi Unicode karakter kümesine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-178">The set of possible values for the `char` type corresponds to the Unicode character set.</span></span> <span data-ttu-id="e7f76-179">`char`, `ushort`aynı gösterimine sahip olsa da, bir tür üzerinde izin verilen tüm işlemlere izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="e7f76-179">Although `char` has the same representation as `ushort`, not all operations permitted on one type are permitted on the other.</span></span>

<span data-ttu-id="e7f76-180">Tam sayı türü birli ve ikili işleçler her zaman imzalanmış 32 bit duyarlık, imzasız 32 bit duyarlık, imzalı 64 bit duyarlık veya imzasız 64-bit duyarlık ile çalışır:</span><span class="sxs-lookup"><span data-stu-id="e7f76-180">The integral-type unary and binary operators always operate with signed 32-bit precision, unsigned 32-bit precision, signed 64-bit precision, or unsigned 64-bit precision:</span></span>

*  <span data-ttu-id="e7f76-181">Birli `+` ve `~` işleçleri için, işlenen `T`türüne dönüştürülür, burada `T`, işlenenin tüm olası değerlerini tam olarak temsil eden `int`, `uint`, `long`ve `ulong`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-181">For the unary `+` and `~` operators, the operand is converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="e7f76-182">Daha sonra işlem `T`türü duyarlık kullanılarak gerçekleştirilir ve sonucun türü `T`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-182">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span>
*  <span data-ttu-id="e7f76-183">Birli `-` işleci için, işlenen `T`türüne dönüştürülür; burada `T`, işlenenin tüm olası değerlerini tam olarak temsil eden `int` ve `long`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-183">For the unary `-` operator, the operand is converted to type `T`, where `T` is the first of `int` and `long` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="e7f76-184">Daha sonra işlem `T`türü duyarlık kullanılarak gerçekleştirilir ve sonucun türü `T`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-184">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span> <span data-ttu-id="e7f76-185">Birli `-` işleci `ulong`türündeki işlenenlere uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="e7f76-185">The unary `-` operator cannot be applied to operands of type `ulong`.</span></span>
*  <span data-ttu-id="e7f76-186">İkili `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`ve `<=` işleçleri için işlenen, her iki işlenenin olası tüm değerlerini tam olarak temsil eden `int`, `uint`, `long`ve `ulong` ilk `T` `T`türüne dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-186">For the binary `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`, and `<=` operators, the operands are converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of both operands.</span></span> <span data-ttu-id="e7f76-187">Daha sonra işlem, `T`türü duyarlık kullanılarak gerçekleştirilir ve sonucun türü `T` (veya ilişkisel işleçler için `bool`).</span><span class="sxs-lookup"><span data-stu-id="e7f76-187">The operation is then performed using the precision of type `T`, and the type of the result is `T` (or `bool` for the relational operators).</span></span> <span data-ttu-id="e7f76-188">Bir işlenenin `long` türü ve diğeri de ikili işleçlerle `ulong` türünden olması için izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="e7f76-188">It is not permitted for one operand to be of type `long` and the other to be of type `ulong` with the binary operators.</span></span>
*  <span data-ttu-id="e7f76-189">İkili `<<` ve `>>` işleçleri için, sol işlenen `T`türüne dönüştürülür; burada `T` ilki, `int`, `uint`ve `long`, işlenenin tüm olası değerlerini tam olarak temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-189">For the binary `<<` and `>>` operators, the left operand is converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="e7f76-190">Daha sonra işlem `T`türü duyarlık kullanılarak gerçekleştirilir ve sonucun türü `T`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-190">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span>

<span data-ttu-id="e7f76-191">`char` türü bir integral türü olarak sınıflandırıldı, ancak diğer integral türlerinden iki şekilde farklı olur:</span><span class="sxs-lookup"><span data-stu-id="e7f76-191">The `char` type is classified as an integral type, but it differs from the other integral types in two ways:</span></span>

*  <span data-ttu-id="e7f76-192">Diğer türlerden `char` türüne örtük dönüştürme yok.</span><span class="sxs-lookup"><span data-stu-id="e7f76-192">There are no implicit conversions from other types to the `char` type.</span></span> <span data-ttu-id="e7f76-193">Özellikle, `sbyte`, `byte`ve `ushort` türleri `char` türünü kullanarak tam olarak gösterilemeyen değer aralıklarına sahip olsa da, `sbyte`, `byte`veya `ushort` ' ten örtülü dönüştürmeler yok `char`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-193">In particular, even though the `sbyte`, `byte`, and `ushort` types have ranges of values that are fully representable using the `char` type, implicit conversions from `sbyte`, `byte`, or `ushort` to `char` do not exist.</span></span>
*  <span data-ttu-id="e7f76-194">`char` türünün sabitleri, `char`türüne dönüştürme ile birlikte *character_literal*s veya as *integer_literal*s olarak yazılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-194">Constants of the `char` type must be written as *character_literal*s or as *integer_literal*s in combination with a cast to type `char`.</span></span> <span data-ttu-id="e7f76-195">Örneğin, `(char)10` `'\x000A'`aynıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-195">For example, `(char)10` is the same as `'\x000A'`.</span></span>

<span data-ttu-id="e7f76-196">`checked` ve `unchecked` işleçleri ve deyimleri, tam sayı türü aritmetik işlemler ve dönüştürmeler için taşma denetimini denetlemek için kullanılır ([denetlenen ve işaretlenmemiş işleçler](expressions.md#the-checked-and-unchecked-operators)).</span><span class="sxs-lookup"><span data-stu-id="e7f76-196">The `checked` and `unchecked` operators and statements are used to control overflow checking for integral-type arithmetic operations and conversions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span> <span data-ttu-id="e7f76-197">`checked` bağlamında, taşma bir derleme zamanı hatası üretir veya bir `System.OverflowException` oluşturulmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-197">In a `checked` context, an overflow produces a compile-time error or causes a `System.OverflowException` to be thrown.</span></span> <span data-ttu-id="e7f76-198">`unchecked` bağlamında, taşmalar yok sayılır ve hedef türüne uymayan tüm yüksek sıralı bitler atılır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-198">In an `unchecked` context, overflows are ignored and any high-order bits that do not fit in the destination type are discarded.</span></span>

### <a name="floating-point-types"></a><span data-ttu-id="e7f76-199">Kayan nokta türleri</span><span class="sxs-lookup"><span data-stu-id="e7f76-199">Floating point types</span></span>

<span data-ttu-id="e7f76-200">C#iki kayan nokta türünü destekler: `float` ve `double`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-200">C# supports two floating point types: `float` and `double`.</span></span> <span data-ttu-id="e7f76-201">`float` ve `double` türleri, aşağıdaki değer kümelerini sağlayan 32 bitlik tek duyarlıklı ve 64 bit çift duyarlıklı IEEE 754 biçimleri kullanılarak temsil edilir:</span><span class="sxs-lookup"><span data-stu-id="e7f76-201">The `float` and `double` types are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats, which provide the following sets of values:</span></span>

*  <span data-ttu-id="e7f76-202">Pozitif sıfır ve negatif sıfır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-202">Positive zero and negative zero.</span></span> <span data-ttu-id="e7f76-203">Çoğu durumda, pozitif sıfır ve negatif sıfır değeri basit değer sıfır ile aynı şekilde davranır, ancak belirli işlemler iki ([bölme işleci](expressions.md#division-operator)) arasında ayrım yapar.</span><span class="sxs-lookup"><span data-stu-id="e7f76-203">In most situations, positive zero and negative zero behave identically as the simple value zero, but certain operations distinguish between the two ([Division operator](expressions.md#division-operator)).</span></span>
*  <span data-ttu-id="e7f76-204">Pozitif sonsuzluk ve negatif sonsuz.</span><span class="sxs-lookup"><span data-stu-id="e7f76-204">Positive infinity and negative infinity.</span></span> <span data-ttu-id="e7f76-205">Sonsuz sayısı sıfır olmayan bir sayı sıfıra bölünerek bu işlemler tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-205">Infinities are produced by such operations as dividing a non-zero number by zero.</span></span> <span data-ttu-id="e7f76-206">Örneğin, `1.0 / 0.0` pozitif sonsuz verir ve `-1.0 / 0.0` negatif sonsuz verir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-206">For example, `1.0 / 0.0` yields positive infinity, and `-1.0 / 0.0` yields negative infinity.</span></span>
*  <span data-ttu-id="e7f76-207">***Bir sayı değil*** değeri genellikle kısaltılmış Nan.</span><span class="sxs-lookup"><span data-stu-id="e7f76-207">The ***Not-a-Number*** value, often abbreviated NaN.</span></span> <span data-ttu-id="e7f76-208">NaNs 'ler, sıfıra bölme gibi geçersiz kayan nokta işlemleri tarafından üretilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-208">NaNs are produced by invalid floating-point operations, such as dividing zero by zero.</span></span>
*  <span data-ttu-id="e7f76-209">`s * m * 2^e`, `s` 1 veya-1 olduğu ve `m` ve `e` belirli kayan nokta türü tarafından belirlendiği şekilde, sıfır olmayan değerler kümesi: `float`, `0 < m < 2^24` ve `-149 <= e <= 104`ve `double`, `0 < m < 2^53` ve `-1075 <= e <= 970`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-209">The finite set of non-zero values of the form `s * m * 2^e`, where `s` is 1 or -1, and `m` and `e` are determined by the particular floating-point type: For `float`, `0 < m < 2^24` and `-149 <= e <= 104`, and for `double`, `0 < m < 2^53` and `-1075 <= e <= 970`.</span></span> <span data-ttu-id="e7f76-210">Normalleştirilmemiş kayan noktalı sayılar geçerli sıfır olmayan değerler olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-210">Denormalized floating-point numbers are considered valid non-zero values.</span></span>

<span data-ttu-id="e7f76-211">`float` türü, yaklaşık `1.5 * 10^-45` 7 basamaklı bir duyarlık ile `3.4 * 10^38` kadar olan değerleri temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-211">The `float` type can represent values ranging from approximately `1.5 * 10^-45` to `3.4 * 10^38` with a precision of 7 digits.</span></span>

<span data-ttu-id="e7f76-212">`double` türü, 15-16 basamaklı bir duyarlık ile `1.7 × 10^308` kadar yaklaşık `5.0 * 10^-324` değişen değerleri temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-212">The `double` type can represent values ranging from approximately `5.0 * 10^-324` to `1.7 × 10^308` with a precision of 15-16 digits.</span></span>

<span data-ttu-id="e7f76-213">Bir ikili işlecin işlenenlerinden biri kayan nokta türünde ise, diğer işlenen bir integral türü veya kayan nokta türünde olmalıdır ve işlem aşağıdaki gibi değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="e7f76-213">If one of the operands of a binary operator is of a floating-point type, then the other operand must be of an integral type or a floating-point type, and the operation is evaluated as follows:</span></span>

*  <span data-ttu-id="e7f76-214">İşlenenden biri integral türünde ise, bu işlenen diğer işlenenin kayan nokta türüne dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-214">If one of the operands is of an integral type, then that operand is converted to the floating-point type of the other operand.</span></span>
*  <span data-ttu-id="e7f76-215">Ardından, işlenenlerden biri `double`türünde ise, diğer işlenen `double`dönüştürülür, işlem en az `double` Aralık ve duyarlık kullanılarak yapılır ve sonucun türü `double` (veya ilişkisel işleçler için `bool`).</span><span class="sxs-lookup"><span data-stu-id="e7f76-215">Then, if either of the operands is of type `double`, the other operand is converted to `double`, the operation is performed using at least `double` range and precision, and the type of the result is `double` (or `bool` for the relational operators).</span></span>
*  <span data-ttu-id="e7f76-216">Aksi takdirde, işlem en az `float` Aralık ve duyarlık kullanılarak gerçekleştirilir ve sonucun türü `float` (veya ilişkisel işleçler için `bool`).</span><span class="sxs-lookup"><span data-stu-id="e7f76-216">Otherwise, the operation is performed using at least `float` range and precision, and the type of the result is `float` (or `bool` for the relational operators).</span></span>

<span data-ttu-id="e7f76-217">Atama işleçleri de dahil olmak üzere kayan nokta işleçleri hiçbir şekilde özel durum oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="e7f76-217">The floating-point operators, including the assignment operators, never produce exceptions.</span></span> <span data-ttu-id="e7f76-218">Bunun yerine, bazı durumlarda kayan nokta işlemleri, aşağıda açıklandığı gibi sıfır, sonsuz veya NaN üretir:</span><span class="sxs-lookup"><span data-stu-id="e7f76-218">Instead, in exceptional situations, floating-point operations produce zero, infinity, or NaN, as described below:</span></span>

*  <span data-ttu-id="e7f76-219">Kayan noktalı bir işlemin sonucu hedef biçim için çok küçükse, işlemin sonucu pozitif sıfır veya negatif sıfır olur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-219">If the result of a floating-point operation is too small for the destination format, the result of the operation becomes positive zero or negative zero.</span></span>
*  <span data-ttu-id="e7f76-220">Kayan noktalı bir işlemin sonucu hedef biçim için çok büyükse, işlemin sonucu pozitif sonsuzluk veya negatif sonsuzluk olur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-220">If the result of a floating-point operation is too large for the destination format, the result of the operation becomes positive infinity or negative infinity.</span></span>
*  <span data-ttu-id="e7f76-221">Kayan noktalı bir işlem geçersizse, işlemin sonucu NaN olur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-221">If a floating-point operation is invalid, the result of the operation becomes NaN.</span></span>
*  <span data-ttu-id="e7f76-222">Kayan noktalı bir işlemin bir veya her iki işleneni de NaN ise, işlemin sonucu NaN olur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-222">If one or both operands of a floating-point operation is NaN, the result of the operation becomes NaN.</span></span>

<span data-ttu-id="e7f76-223">Kayan nokta işlemleri, işlemin sonuç türünden daha yüksek bir duyarlıkla gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-223">Floating-point operations may be performed with higher precision than the result type of the operation.</span></span> <span data-ttu-id="e7f76-224">Örneğin, bazı donanım mimarileri, `double` türünden daha fazla Aralık ve duyarlık içeren bir "genişletilmiş" veya "uzun çift" kayan nokta türü destekler ve bu daha yüksek duyarlık türünü kullanarak tüm kayan nokta işlemlerini örtülü olarak gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-224">For example, some hardware architectures support an "extended" or "long double" floating-point type with greater range and precision than the `double` type, and implicitly perform all floating-point operations using this higher precision type.</span></span> <span data-ttu-id="e7f76-225">Yalnızca yüksek maliyetli performans, bu tür donanım mimarileri daha az duyarlılıkla kayan nokta işlemleri gerçekleştirmek için yapılabilir ve hem performans hem de duyarlık sağlamak için bir uygulama istemek yerine, daha yüksek bir duyarlık türüne C# izin verir Tüm kayan nokta işlemleri için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-225">Only at excessive cost in performance can such hardware architectures be made to perform floating-point operations with less precision, and rather than require an implementation to forfeit both performance and precision, C# allows a higher precision type to be used for all floating-point operations.</span></span> <span data-ttu-id="e7f76-226">Daha kesin sonuçlar sunmaya kıyasla, bu nadiren de ölçülebilir etkileri vardır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-226">Other than delivering more precise results, this rarely has any measurable effects.</span></span> <span data-ttu-id="e7f76-227">Bununla birlikte, çarpma 'nın `double` aralığın dışında kalan bir sonuç oluşturduğu `x * y / z`, ancak sonraki bölme geçici sonucu `double` aralığına geri getirirse, ifadenin bir daha yüksek Aralık biçimi sonsuz bir sonucun bir sonsuzluk yerine üretilmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-227">However, in expressions of the form `x * y / z`, where the multiplication produces a result that is outside the `double` range, but the subsequent division brings the temporary result back into the `double` range, the fact that the expression is evaluated in a higher range format may cause a finite result to be produced instead of an infinity.</span></span>

### <a name="the-decimal-type"></a><span data-ttu-id="e7f76-228">Ondalık tür</span><span class="sxs-lookup"><span data-stu-id="e7f76-228">The decimal type</span></span>

<span data-ttu-id="e7f76-229">`decimal` türü, finansal ve parasal hesaplamalar için uygun olan 128 bitlik bir veri türüdür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-229">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span> <span data-ttu-id="e7f76-230">`decimal` türü, 28-29 önemli bir basamakla `1.0 * 10^-28` yaklaşık olarak `7.9 * 10^28` kadar olan değerleri temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-230">The `decimal` type can represent values ranging from `1.0 * 10^-28` to approximately `7.9 * 10^28` with 28-29 significant digits.</span></span>

<span data-ttu-id="e7f76-231">`decimal` türü sınırlı değerler kümesi, işaret `s` 0 veya 1 olduğunda, katsayı `c` `0 <= *c* < 2^96`tarafından verildiği ve ölçek `e` `0 <= e <= 28`gibi bir `(-1)^s * c * 10^-e`formundadır. `decimal` türü, imzalanmış sıfırları, sonsuz veya NaN 'yi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="e7f76-231">The finite set of values of type `decimal` are of the form `(-1)^s * c * 10^-e`, where the sign `s` is 0 or 1, the coefficient `c` is given by `0 <= *c* < 2^96`, and the scale `e` is such that `0 <= e <= 28`.The `decimal` type does not support signed zeros, infinities, or NaN's.</span></span> <span data-ttu-id="e7f76-232">Bir `decimal`, on ' un bir kuvvetine ölçeklenerek 96 bitlik bir tamsayı olarak temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-232">A `decimal` is represented as a 96-bit integer scaled by a power of ten.</span></span> <span data-ttu-id="e7f76-233">`1.0m`sıfırdan küçük bir değer olan `decimal`s için, değer tamamen 28 ondalık hanesiz, ancak başka bir değer değildir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-233">For `decimal`s with an absolute value less than `1.0m`, the value is exact to the 28th decimal place, but no further.</span></span> <span data-ttu-id="e7f76-234">`1.0m`sıfırdan büyük veya buna eşit bir mutlak değere sahip `decimal`s için, değer 28 veya 29 haneye eşittir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-234">For `decimal`s with an absolute value greater than or equal to `1.0m`, the value is exact to 28 or 29 digits.</span></span> <span data-ttu-id="e7f76-235">`float` ve `double` veri türlerinin aksine, 0,1 gibi ondalık kesirli sayılar tam olarak `decimal` gösteriminde temsil edilebilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-235">Contrary to the `float` and `double` data types, decimal fractional numbers such as 0.1 can be represented exactly in the `decimal` representation.</span></span> <span data-ttu-id="e7f76-236">`float` ve `double` temsilleri genellikle sonsuz kesirler olur ve bu temsiller, yuvarlama hatalarına karşı daha açıktır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-236">In the `float` and `double` representations, such numbers are often infinite fractions, making those representations more prone to round-off errors.</span></span>

<span data-ttu-id="e7f76-237">Bir ikili işlecin işlenenlerinden biri `decimal`türünde ise, diğer işlenenin bir integral türünde veya `decimal`türünde olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-237">If one of the operands of a binary operator is of type `decimal`, then the other operand must be of an integral type or of type `decimal`.</span></span> <span data-ttu-id="e7f76-238">İntegral tür işleneni varsa, işlem gerçekleştirilmeden önce `decimal` dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-238">If an integral type operand is present, it is converted to `decimal` before the operation is performed.</span></span>

<span data-ttu-id="e7f76-239">`decimal` türündeki değerler üzerindeki bir işlemin sonucu, tam bir sonucun hesaplanmasının (ölçeği koruma, her operatör için tanımlanan şekilde koruma) ve ardından temsili sığacak şekilde yuvarlamasının sonuçlandığı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-239">The result of an operation on values of type `decimal` is that which would result from calculating an exact result (preserving scale, as defined for each operator) and then rounding to fit the representation.</span></span> <span data-ttu-id="e7f76-240">Sonuçlar en yakın gösterilemeyen değere yuvarlanır ve bir sonuç iki gösterilemeyen değere eşit bir şekilde yakınsa, en az önemli basamak konumunda çift sayı olan değere ("banker 'in yuvarlanması" olarak bilinir).</span><span class="sxs-lookup"><span data-stu-id="e7f76-240">Results are rounded to the nearest representable value, and, when a result is equally close to two representable values, to the value that has an even number in the least significant digit position (this is known as "banker's rounding").</span></span> <span data-ttu-id="e7f76-241">Sıfır sonucun her zaman 0 ve Ölçeği 0 olan bir imzası vardır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-241">A zero result always has a sign of 0 and a scale of 0.</span></span>

<span data-ttu-id="e7f76-242">Ondalık aritmetik işlem mutlak değerde `5 * 10^-29` değerinden küçük veya buna eşit bir değer üretirse, işlemin sonucu sıfır olur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-242">If a decimal arithmetic operation produces a value less than or equal to `5 * 10^-29` in absolute value, the result of the operation becomes zero.</span></span> <span data-ttu-id="e7f76-243">Bir `decimal` aritmetik işlemi `decimal` biçimi için çok büyük bir sonuç üretirse, bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-243">If a `decimal` arithmetic operation produces a result that is too large for the `decimal` format, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="e7f76-244">`decimal` türünün daha fazla duyarlığı ancak kayan nokta türlerinden daha küçük bir aralığı vardır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-244">The `decimal` type has greater precision but smaller range than the floating-point types.</span></span> <span data-ttu-id="e7f76-245">Bu nedenle, kayan nokta türlerinden `decimal` dönüşümler, taşma özel durumları üretebilir ve `decimal` ' dan kayan nokta türlerine dönüşümler duyarlık kaybına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-245">Thus, conversions from the floating-point types to `decimal` might produce overflow exceptions, and conversions from `decimal` to the floating-point types might cause loss of precision.</span></span> <span data-ttu-id="e7f76-246">Bu nedenlerden dolayı, kayan nokta türleri ve `decimal`arasında örtük dönüştürmeler yoktur ve açık yayınlar olmadan, kayan nokta ve `decimal` işlenenlerini aynı ifadede karıştırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-246">For these reasons, no implicit conversions exist between the floating-point types and `decimal`, and without explicit casts, it is not possible to mix floating-point and `decimal` operands in the same expression.</span></span>

### <a name="the-bool-type"></a><span data-ttu-id="e7f76-247">Bool türü</span><span class="sxs-lookup"><span data-stu-id="e7f76-247">The bool type</span></span>

<span data-ttu-id="e7f76-248">`bool` türü, Boolean mantıksal miktarları temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e7f76-248">The `bool` type represents boolean logical quantities.</span></span> <span data-ttu-id="e7f76-249">`bool` türündeki olası değerler `true` ve `false`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-249">The possible values of type `bool` are `true` and `false`.</span></span>

<span data-ttu-id="e7f76-250">`bool` ve diğer türler arasında standart dönüştürme yok.</span><span class="sxs-lookup"><span data-stu-id="e7f76-250">No standard conversions exist between `bool` and other types.</span></span> <span data-ttu-id="e7f76-251">Özellikle, `bool` türü benzersiz ve integral türlerden ayrıdır ve bir `bool` değeri bir tam sayı değeri yerine kullanılamaz ve tam tersi de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-251">In particular, the `bool` type is distinct and separate from the integral types, and a `bool` value cannot be used in place of an integral value, and vice versa.</span></span>

<span data-ttu-id="e7f76-252">C ve C++ dillerde sıfır tamsayı veya kayan nokta değeri ya da null işaretçisi, boolean değer `false`, sıfır olmayan bir tamsayı veya kayan nokta değeri veya null olmayan bir işaretçi, `true`Boole değerine dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-252">In the C and C++ languages, a zero integral or floating-point value, or a null pointer can be converted to the boolean value `false`, and a non-zero integral or floating-point value, or a non-null pointer can be converted to the boolean value `true`.</span></span> <span data-ttu-id="e7f76-253">' C#De, bu tür dönüştürmeler, tam olarak bir integral veya kayan nokta değeri sıfıra karşılaştırılarak veya bir nesne başvurusunu `null`açıkça karşılaştırarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-253">In C#, such conversions are accomplished by explicitly comparing an integral or floating-point value to zero, or by explicitly comparing an object reference to `null`.</span></span>

### <a name="enumeration-types"></a><span data-ttu-id="e7f76-254">Numaralandırma türleri</span><span class="sxs-lookup"><span data-stu-id="e7f76-254">Enumeration types</span></span>

<span data-ttu-id="e7f76-255">Sabit listesi türü, adlandırılmış sabitleri olan ayrı bir türdür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-255">An enumeration type is a distinct type with named constants.</span></span> <span data-ttu-id="e7f76-256">Her numaralandırma türünün, `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` veya `ulong`olması gereken temel bir türü vardır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-256">Every enumeration type has an underlying type, which must be `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="e7f76-257">Numaralandırma türünün değer kümesi, temel alınan türün değerleri kümesiyle aynıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-257">The set of values of the enumeration type is the same as the set of values of the underlying type.</span></span> <span data-ttu-id="e7f76-258">Numaralandırma türünün değerleri, adlandırılmış sabitlerin değerleriyle sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-258">Values of the enumeration type are not restricted to the values of the named constants.</span></span> <span data-ttu-id="e7f76-259">Numaralandırma türleri, numaralandırma bildirimleri ([enum bildirimleri](enums.md#enum-declarations)) aracılığıyla tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-259">Enumeration types are defined through enumeration declarations ([Enum declarations](enums.md#enum-declarations)).</span></span>

### <a name="nullable-types"></a><span data-ttu-id="e7f76-260">Boş değer atanabilir tipler</span><span class="sxs-lookup"><span data-stu-id="e7f76-260">Nullable types</span></span>

<span data-ttu-id="e7f76-261">Null yapılabilir bir tür, ***temel türünün*** tüm değerlerini ve ek bir null değeri temsil edebilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-261">A nullable type can represent all values of its ***underlying type*** plus an additional null value.</span></span> <span data-ttu-id="e7f76-262">Null yapılabilir bir tür `T?`yazılır; burada `T` temel tür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-262">A nullable type is written `T?`, where `T` is the underlying type.</span></span> <span data-ttu-id="e7f76-263">Bu sözdizimi `System.Nullable<T>`için toplu bir özelliktir ve iki form birbirinin yerine kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-263">This syntax is shorthand for `System.Nullable<T>`, and the two forms can be used interchangeably.</span></span>

<span data-ttu-id="e7f76-264">***Null yapılamayan bir değer türü*** , `System.Nullable<T>` dışında herhangi bir değer türü ve onun toplu `T?` (herhangi bir `T`için) ve null yapılamayan bir değer türü (yani, `struct` bir tür parametresi) olarak kısıtlanan herhangi bir tür parametresi. kısıtlama).</span><span class="sxs-lookup"><span data-stu-id="e7f76-264">A ***non-nullable value type*** conversely is any value type other than `System.Nullable<T>` and its shorthand `T?` (for any `T`), plus any type parameter that is constrained to be a non-nullable value type (that is, any type parameter with a `struct` constraint).</span></span> <span data-ttu-id="e7f76-265">`System.Nullable<T>` türü, `T` ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) için değer türü kısıtlamasını belirtir, yani null yapılabilir bir türün temel alınan türünün hiçbir null yapılamayan değer türü olabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-265">The `System.Nullable<T>` type specifies the value type constraint for `T` ([Type parameter constraints](classes.md#type-parameter-constraints)), which means that the underlying type of a nullable type can be any non-nullable value type.</span></span> <span data-ttu-id="e7f76-266">Null yapılabilir bir türün temel alınan türü null yapılabilir bir tür veya başvuru türü olamaz.</span><span class="sxs-lookup"><span data-stu-id="e7f76-266">The underlying type of a nullable type cannot be a nullable type or a reference type.</span></span> <span data-ttu-id="e7f76-267">Örneğin, `int??` ve `string?` geçersiz türlerdir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-267">For example, `int??` and `string?` are invalid types.</span></span>

<span data-ttu-id="e7f76-268">Null yapılabilir bir `T?` tür örneğinin iki ortak salt okuma özelliği vardır:</span><span class="sxs-lookup"><span data-stu-id="e7f76-268">An instance of a nullable type `T?` has two public read-only properties:</span></span>

*  <span data-ttu-id="e7f76-269">`bool` türünde bir `HasValue` özelliği</span><span class="sxs-lookup"><span data-stu-id="e7f76-269">A `HasValue` property of type `bool`</span></span>
*  <span data-ttu-id="e7f76-270">`T` türünde bir `Value` özelliği</span><span class="sxs-lookup"><span data-stu-id="e7f76-270">A `Value` property of type `T`</span></span>

<span data-ttu-id="e7f76-271">`HasValue` true olan bir örnek, null olmayan olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-271">An instance for which `HasValue` is true is said to be non-null.</span></span> <span data-ttu-id="e7f76-272">Null olmayan bir örnek bilinen bir değer içerir ve `Value` bu değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-272">A non-null instance contains a known value and `Value` returns that value.</span></span>

<span data-ttu-id="e7f76-273">`HasValue` false olan bir örnek null olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-273">An instance for which `HasValue` is false is said to be null.</span></span> <span data-ttu-id="e7f76-274">Null bir örnek tanımsız bir değere sahip.</span><span class="sxs-lookup"><span data-stu-id="e7f76-274">A null instance has an undefined value.</span></span> <span data-ttu-id="e7f76-275">Null örneğinin `Value` okunmaya çalışılması `System.InvalidOperationException` oluşturulmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-275">Attempting to read the `Value` of a null instance causes a `System.InvalidOperationException` to be thrown.</span></span> <span data-ttu-id="e7f76-276">Null yapılabilir bir örneğin `Value` özelliğine erişme işlemi, ***sarmalama***olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-276">The process of accessing the `Value` property of a nullable instance is referred to as ***unwrapping***.</span></span>

<span data-ttu-id="e7f76-277">Varsayılan oluşturucuya ek olarak, her Nullable tür `T?` `T`türünde tek bir bağımsız değişken alan bir ortak oluşturucuya sahiptir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-277">In addition to the default constructor, every nullable type `T?` has a public constructor that takes a single argument of type `T`.</span></span> <span data-ttu-id="e7f76-278">`T`türünde bir değer `x` verildiğinde, formun Oluşturucu çağrısı</span><span class="sxs-lookup"><span data-stu-id="e7f76-278">Given a value `x` of type `T`, a constructor invocation of the form</span></span>

```csharp
new T?(x)
```
<span data-ttu-id="e7f76-279">`Value` özelliğinin `x``T?` null olmayan bir örneğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-279">creates a non-null instance of `T?` for which the `Value` property is `x`.</span></span> <span data-ttu-id="e7f76-280">Verili bir değer için null değer olmayan bir türün null olmayan bir örneğini oluşturma işlemi ***sarmalama***olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-280">The process of creating a non-null instance of a nullable type for a given value is referred to as ***wrapping***.</span></span>

<span data-ttu-id="e7f76-281">Örtük dönüştürmeler, `null` değişmez değerinden `T?` ([null değişmez değer dönüştürmeleri](conversions.md#null-literal-conversions)) ve `T` ile `T?` ([örtük Nullable dönüşümler](conversions.md#implicit-nullable-conversions)) arasında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-281">Implicit conversions are available from the `null` literal to `T?` ([Null literal conversions](conversions.md#null-literal-conversions)) and from `T` to `T?` ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)).</span></span>

## <a name="reference-types"></a><span data-ttu-id="e7f76-282">Başvuru türleri</span><span class="sxs-lookup"><span data-stu-id="e7f76-282">Reference types</span></span>

<span data-ttu-id="e7f76-283">Başvuru türü bir sınıf türü, arabirim türü, bir dizi türü veya temsilci türüdür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-283">A reference type is a class type, an interface type, an array type, or a delegate type.</span></span>

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

<span data-ttu-id="e7f76-284">Başvuru türü değeri, ikinci bir ***nesne***olarak bilinen türünün bir ***örneğine*** başvurudur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-284">A reference type value is a reference to an ***instance*** of the type, the latter known as an ***object***.</span></span> <span data-ttu-id="e7f76-285">`null` özel değer tüm başvuru türleriyle uyumludur ve bir örneğin yokluğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-285">The special value `null` is compatible with all reference types and indicates the absence of an instance.</span></span>

### <a name="class-types"></a><span data-ttu-id="e7f76-286">Sınıf türleri</span><span class="sxs-lookup"><span data-stu-id="e7f76-286">Class types</span></span>

<span data-ttu-id="e7f76-287">Bir sınıf türü veri üyelerini (sabitler ve alanlar), işlev üyelerini (Yöntemler, özellikler, olaylar, Dizin oluşturucular, işleçler, örnek oluşturucular, Yıkıcılar ve statik oluşturucular) ve iç içe geçmiş türleri içeren bir veri yapısını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="e7f76-287">A class type defines a data structure that contains data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="e7f76-288">Sınıf türleri, türetilmiş sınıfların temel sınıfları genişletebilen ve özelleştireceği bir mekanizma olan devralmayı destekler.</span><span class="sxs-lookup"><span data-stu-id="e7f76-288">Class types support inheritance, a mechanism whereby derived classes can extend and specialize base classes.</span></span> <span data-ttu-id="e7f76-289">Sınıf türlerinin örnekleri, *object_creation_expression*s ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)) kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-289">Instances of class types are created using *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>

<span data-ttu-id="e7f76-290">Sınıf türleri [sınıflarda](classes.md)açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-290">Class types are described in [Classes](classes.md).</span></span>

<span data-ttu-id="e7f76-291">Önceden tanımlanmış bazı sınıf türleri, aşağıdaki tabloda açıklandığı C# gibi dilde özel anlamlara sahiptir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-291">Certain predefined class types have special meaning in the C# language, as described in the table below.</span></span>


| <span data-ttu-id="e7f76-292">__Sınıf türü__</span><span class="sxs-lookup"><span data-stu-id="e7f76-292">__Class type__</span></span>     | <span data-ttu-id="e7f76-293">__Açıklama__</span><span class="sxs-lookup"><span data-stu-id="e7f76-293">__Description__</span></span>                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | <span data-ttu-id="e7f76-294">Diğer tüm türlerin Ultimate temel sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e7f76-294">The ultimate base class of all other types.</span></span> <span data-ttu-id="e7f76-295">Bkz. [nesne türü](types.md#the-object-type).</span><span class="sxs-lookup"><span data-stu-id="e7f76-295">See [The object type](types.md#the-object-type).</span></span> | 
| `System.String`    | <span data-ttu-id="e7f76-296">C# Dilin dize türü.</span><span class="sxs-lookup"><span data-stu-id="e7f76-296">The string type of the C# language.</span></span> <span data-ttu-id="e7f76-297">[Dize türü](types.md#the-string-type)' ne bakın.</span><span class="sxs-lookup"><span data-stu-id="e7f76-297">See [The string type](types.md#the-string-type).</span></span>         |
| `System.ValueType` | <span data-ttu-id="e7f76-298">Tüm değer türlerinin temel sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e7f76-298">The base class of all value types.</span></span> <span data-ttu-id="e7f76-299">Bkz. [System. ValueType türü](types.md#the-systemvaluetype-type).</span><span class="sxs-lookup"><span data-stu-id="e7f76-299">See [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>          |
| `System.Enum`      | <span data-ttu-id="e7f76-300">Tüm sabit listesi türlerinin temel sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e7f76-300">The base class of all enum types.</span></span> <span data-ttu-id="e7f76-301">Bkz. [numaralandırmalar](enums.md).</span><span class="sxs-lookup"><span data-stu-id="e7f76-301">See [Enums](enums.md).</span></span>              |
| `System.Array`     | <span data-ttu-id="e7f76-302">Tüm dizi türlerinin temel sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e7f76-302">The base class of all array types.</span></span> <span data-ttu-id="e7f76-303">Bkz. [diziler](arrays.md).</span><span class="sxs-lookup"><span data-stu-id="e7f76-303">See [Arrays](arrays.md).</span></span>             |
| `System.Delegate`  | <span data-ttu-id="e7f76-304">Tüm temsilci türlerinin temel sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e7f76-304">The base class of all delegate types.</span></span> <span data-ttu-id="e7f76-305">Bkz. [Temsilciler](delegates.md).</span><span class="sxs-lookup"><span data-stu-id="e7f76-305">See [Delegates](delegates.md).</span></span>          |
| `System.Exception` | <span data-ttu-id="e7f76-306">Tüm özel durum türlerinin temel sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e7f76-306">The base class of all exception types.</span></span> <span data-ttu-id="e7f76-307">Bkz. [özel durumlar](exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="e7f76-307">See [Exceptions](exceptions.md).</span></span>         |

### <a name="the-object-type"></a><span data-ttu-id="e7f76-308">Nesne türü</span><span class="sxs-lookup"><span data-stu-id="e7f76-308">The object type</span></span>

<span data-ttu-id="e7f76-309">`object` sınıf türü, diğer tüm türlerin en son temel sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-309">The `object` class type is the ultimate base class of all other types.</span></span> <span data-ttu-id="e7f76-310">Her tür doğrudan C# veya dolaylı olarak `object` sınıf türünden türetilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-310">Every type in C# directly or indirectly derives from the `object` class type.</span></span>

<span data-ttu-id="e7f76-311">Anahtar sözcüğü `object` önceden tanımlanmış sınıf `System.Object`için yalnızca bir diğer addır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-311">The keyword `object` is simply an alias for the predefined class `System.Object`.</span></span>

### <a name="the-dynamic-type"></a><span data-ttu-id="e7f76-312">Dinamik tür</span><span class="sxs-lookup"><span data-stu-id="e7f76-312">The dynamic type</span></span>

<span data-ttu-id="e7f76-313">`object`gibi `dynamic` türü herhangi bir nesneye başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-313">The `dynamic` type, like `object`, can reference any object.</span></span> <span data-ttu-id="e7f76-314">İşleçler `dynamic`türündeki ifadelere uygulandığında, çözüm program çalıştırılıncaya kadar ertelenir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-314">When operators are applied to expressions of type `dynamic`, their resolution is deferred until the program is run.</span></span> <span data-ttu-id="e7f76-315">Bu nedenle, işleç başvurulan nesneye yasal olarak uygulanamazlar, derleme sırasında herhangi bir hata verilmez.</span><span class="sxs-lookup"><span data-stu-id="e7f76-315">Thus, if the operator cannot legally be applied to the referenced object, no error is given during compilation.</span></span> <span data-ttu-id="e7f76-316">Bunun yerine, işlecin çözümlenmesi çalışma zamanında başarısız olduğunda bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-316">Instead an exception will be thrown when resolution of the operator fails at run-time.</span></span>

<span data-ttu-id="e7f76-317">Amacı [, dinamik bağlamada](expressions.md#dynamic-binding)ayrıntılı olarak açıklanan dinamik bağlamaya izin versağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-317">Its purpose is to allow dynamic binding, which is described in detail in [Dynamic binding](expressions.md#dynamic-binding).</span></span>

<span data-ttu-id="e7f76-318">`dynamic`, aşağıdaki durumlar dışında `object` ile aynı kabul edilir:</span><span class="sxs-lookup"><span data-stu-id="e7f76-318">`dynamic` is considered identical to `object` except in the following respects:</span></span>

*  <span data-ttu-id="e7f76-319">`dynamic` türündeki ifadelerde işlemler, dinamik olarak bağlanabilir ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="e7f76-319">Operations on expressions of type `dynamic` can be dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span>
*  <span data-ttu-id="e7f76-320">Tür çıkarımı ([tür çıkarımı](expressions.md#type-inference)), her ikisi de aday ise `object` `dynamic` tercih eder.</span><span class="sxs-lookup"><span data-stu-id="e7f76-320">Type inference ([Type inference](expressions.md#type-inference)) will prefer `dynamic` over `object` if both are candidates.</span></span>

<span data-ttu-id="e7f76-321">Bu denklik nedeniyle aşağıdakiler şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="e7f76-321">Because of this equivalence, the following holds:</span></span>

*  <span data-ttu-id="e7f76-322">`object` ve `dynamic`arasında ve `dynamic` değiştirirken aynı olan oluşturulmuş türler arasında örtük bir kimlik dönüştürmesi vardır `object`</span><span class="sxs-lookup"><span data-stu-id="e7f76-322">There is an implicit identity conversion between `object` and `dynamic`, and between constructed types that are the same when replacing `dynamic` with `object`</span></span>
*  <span data-ttu-id="e7f76-323">`object` ve öğesinden örtük ve açık dönüştürmeler, `dynamic`için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-323">Implicit and explicit conversions to and from `object` also apply to and from `dynamic`.</span></span>
*  <span data-ttu-id="e7f76-324">`dynamic` `object` ile değiştirme sırasında aynı olan yöntem imzaları aynı imza olarak değerlendirilir</span><span class="sxs-lookup"><span data-stu-id="e7f76-324">Method signatures that are the same when replacing `dynamic` with `object` are considered the same signature</span></span>
*  <span data-ttu-id="e7f76-325">Tür `dynamic`, çalışma zamanında `object` ayırt edilemez.</span><span class="sxs-lookup"><span data-stu-id="e7f76-325">The type `dynamic` is indistinguishable from `object` at run-time.</span></span>
*  <span data-ttu-id="e7f76-326">`dynamic` türünün bir ifadesi, ***dinamik ifade***olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-326">An expression of the type `dynamic` is referred to as a ***dynamic expression***.</span></span>

### <a name="the-string-type"></a><span data-ttu-id="e7f76-327">Dize türü</span><span class="sxs-lookup"><span data-stu-id="e7f76-327">The string type</span></span>

<span data-ttu-id="e7f76-328">`string` türü, doğrudan `object`devralan kapalı bir sınıf türüdür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-328">The `string` type is a sealed class type that inherits directly from `object`.</span></span> <span data-ttu-id="e7f76-329">`string` sınıfının örnekleri Unicode karakter dizelerini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e7f76-329">Instances of the `string` class represent Unicode character strings.</span></span>

<span data-ttu-id="e7f76-330">`string` türünün değerleri, dize değişmez değerleri ([dize sabit](lexical-structure.md#string-literals)değerleri) olarak yazılabilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-330">Values of the `string` type can be written as string literals ([String literals](lexical-structure.md#string-literals)).</span></span>

<span data-ttu-id="e7f76-331">Anahtar sözcüğü `string` önceden tanımlanmış sınıf `System.String`için yalnızca bir diğer addır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-331">The keyword `string` is simply an alias for the predefined class `System.String`.</span></span>

### <a name="interface-types"></a><span data-ttu-id="e7f76-332">Arabirim türleri</span><span class="sxs-lookup"><span data-stu-id="e7f76-332">Interface types</span></span>

<span data-ttu-id="e7f76-333">Arabirim, bir sözleşmeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="e7f76-333">An interface defines a contract.</span></span> <span data-ttu-id="e7f76-334">Arabirim uygulayan bir sınıf veya yapı, sözleşmesine uymalıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-334">A class or struct that implements an interface must adhere to its contract.</span></span> <span data-ttu-id="e7f76-335">Bir arabirim birden çok temel arabirimden devralınabilir ve bir sınıf ya da yapı birden çok arabirim uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-335">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="e7f76-336">Arabirim türleri [arabirimlerde](interfaces.md)açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-336">Interface types are described in [Interfaces](interfaces.md).</span></span>

### <a name="array-types"></a><span data-ttu-id="e7f76-337">Dizi türleri</span><span class="sxs-lookup"><span data-stu-id="e7f76-337">Array types</span></span>

<span data-ttu-id="e7f76-338">Dizi, hesaplanan dizinler üzerinden erişilen sıfır veya daha fazla değişken içeren bir veri yapısıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-338">An array is a data structure that contains zero or more variables which are accessed through computed indices.</span></span> <span data-ttu-id="e7f76-339">Bir dizide bulunan değişkenler, dizi öğeleri de denir, hepsi aynı türdür ve bu tür dizinin öğe türü olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-339">The variables contained in an array, also called the elements of the array, are all of the same type, and this type is called the element type of the array.</span></span>

<span data-ttu-id="e7f76-340">Dizi türleri [diziler](arrays.md)içinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-340">Array types are described in [Arrays](arrays.md).</span></span>

### <a name="delegate-types"></a><span data-ttu-id="e7f76-341">Temsilci türleri</span><span class="sxs-lookup"><span data-stu-id="e7f76-341">Delegate types</span></span>

<span data-ttu-id="e7f76-342">Bir temsilci, bir veya daha fazla yönteme başvuran bir veri yapısıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-342">A delegate is a data structure that refers to one or more methods.</span></span> <span data-ttu-id="e7f76-343">Örnek yöntemleri için de ilgili nesne örneklerine başvurur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-343">For instance methods, it also refers to their corresponding object instances.</span></span>

<span data-ttu-id="e7f76-344">C veya C++ bir işlev işaretçisidir, ancak bir işlev işaretçisi yalnızca statik işlevlere başvurabilir, bir temsilci hem statik hem de örnek yöntemlerine başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-344">The closest equivalent of a delegate in C or C++ is a function pointer, but whereas a function pointer can only reference static functions, a delegate can reference both static and instance methods.</span></span> <span data-ttu-id="e7f76-345">İkinci durumda, temsilci yalnızca metodun giriş noktasına bir başvuru değil, aynı zamanda yöntemi çağırmak için bir nesne örneğine de bir başvuru içerir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-345">In the latter case, the delegate stores not only a reference to the method's entry point, but also a reference to the object instance on which to invoke the method.</span></span>

<span data-ttu-id="e7f76-346">Temsilci türleri [Temsilcilerde](delegates.md)açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-346">Delegate types are described in [Delegates](delegates.md).</span></span>

## <a name="boxing-and-unboxing"></a><span data-ttu-id="e7f76-347">Kutulama ve kutudan çıkarma</span><span class="sxs-lookup"><span data-stu-id="e7f76-347">Boxing and unboxing</span></span>

<span data-ttu-id="e7f76-348">Kutulama ve kutudan çıkarma kavramı, Merkezi C#'nin tür sistemidir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-348">The concept of boxing and unboxing is central to C#'s type system.</span></span> <span data-ttu-id="e7f76-349">*Value_type* herhangi bir değerin `object`türüne ve türüne dönüştürülmesine izin vererek *value_type*s ve *reference_type*s arasında bir köprü sağlar.</span><span class="sxs-lookup"><span data-stu-id="e7f76-349">It provides a bridge between *value_type*s and *reference_type*s by permitting any value of a *value_type* to be converted to and from type `object`.</span></span> <span data-ttu-id="e7f76-350">Kutulama ve kutudan çıkarma, herhangi bir türde bir değer olan tür sisteminin birleştirilmiş bir görünümünü, sonunda bir nesne olarak kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-350">Boxing and unboxing enables a unified view of the type system wherein a value of any type can ultimately be treated as an object.</span></span>

### <a name="boxing-conversions"></a><span data-ttu-id="e7f76-351">Paketleme dönüştürmeleri</span><span class="sxs-lookup"><span data-stu-id="e7f76-351">Boxing conversions</span></span>

<span data-ttu-id="e7f76-352">Paketleme dönüştürmesi, bir *value_type* örtük olarak bir *reference_type*'e dönüştürülmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-352">A boxing conversion permits a *value_type* to be implicitly converted to a *reference_type*.</span></span> <span data-ttu-id="e7f76-353">Aşağıdaki kutulama dönüştürmeleri mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="e7f76-353">The following boxing conversions exist:</span></span>

*  <span data-ttu-id="e7f76-354">Herhangi bir *value_type* türünden `object`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-354">From any *value_type* to the type `object`.</span></span>
*  <span data-ttu-id="e7f76-355">Herhangi bir *value_type* türünden `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-355">From any *value_type* to the type `System.ValueType`.</span></span>
*  <span data-ttu-id="e7f76-356">Herhangi bir *non_nullable_value_type* , *value_type*tarafından uygulanan herhangi bir *interface_type* .</span><span class="sxs-lookup"><span data-stu-id="e7f76-356">From any *non_nullable_value_type* to any *interface_type* implemented by the *value_type*.</span></span>
*  <span data-ttu-id="e7f76-357">Herhangi bir *nullable_type* , *nullable_type*temel türü tarafından uygulanan herhangi bir *interface_type* .</span><span class="sxs-lookup"><span data-stu-id="e7f76-357">From any *nullable_type* to any *interface_type* implemented by the underlying type of the *nullable_type*.</span></span>
*  <span data-ttu-id="e7f76-358">Herhangi bir *enum_type* türünden `System.Enum`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-358">From any *enum_type* to the type `System.Enum`.</span></span>
*  <span data-ttu-id="e7f76-359">Herhangi bir *nullable_type* , temel alınan *enum_type* `System.Enum`türüne sahiptir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-359">From any *nullable_type* with an underlying *enum_type* to the type `System.Enum`.</span></span>
*  <span data-ttu-id="e7f76-360">Bir tür parametresindeki örtük dönüştürmenin, çalışma zamanında bir değer türünden bir başvuru türüne ([tür parametreleri Içeren örtük dönüştürmeler](conversions.md#implicit-conversions-involving-type-parameters)) dönüştürülmesini istiyorsanız kutulama dönüştürmesi olarak yürütüleceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="e7f76-360">Note that an implicit conversion from a type parameter will be executed as a boxing conversion if at run-time it ends up converting from a value type to a reference type ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters)).</span></span>

<span data-ttu-id="e7f76-361">Bir *non_nullable_value_type* değerini paketleme bir nesne örneği ayırmayı ve *non_nullable_value_type* değerini bu örneğe kopyalamayı içerir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-361">Boxing a value of a *non_nullable_value_type* consists of allocating an object instance and copying the *non_nullable_value_type* value into that instance.</span></span>

<span data-ttu-id="e7f76-362">Bir *nullable_type* değerinin kutulanması, `null` değer ise (`HasValue` `false`) veya temel alınan değeri sarmalamayı geri Kutulamadan ve Kutulamadan elde edilen null bir başvuru üretir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-362">Boxing a value of a *nullable_type* produces a null reference if it is the `null` value (`HasValue` is `false`), or the result of unwrapping and boxing the underlying value otherwise.</span></span>

<span data-ttu-id="e7f76-363">Bir *non_nullable_value_type* değeri Kutulamak için gerçek işlem, aşağıdaki gibi bir genel ***paketleme sınıfının***mevcut olduğu gibi davranan, en iyi şekilde açıklanmıştır:</span><span class="sxs-lookup"><span data-stu-id="e7f76-363">The actual process of boxing a value of a *non_nullable_value_type* is best explained by imagining the existence of a generic ***boxing class***, which behaves as if it were declared as follows:</span></span>

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

<span data-ttu-id="e7f76-364">`T` türündeki bir değerin `v` kutulenmesi, artık ifade `new Box<T>(v)`yürütmekten ve sonuçta elde edilen örneği `object`türünde bir değer olarak döndürmekten oluşur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-364">Boxing of a value `v` of type `T` now consists of executing the expression `new Box<T>(v)`, and returning the resulting instance as a value of type `object`.</span></span> <span data-ttu-id="e7f76-365">Bu nedenle, deyimler</span><span class="sxs-lookup"><span data-stu-id="e7f76-365">Thus, the statements</span></span>
```csharp
int i = 123;
object box = i;
```
<span data-ttu-id="e7f76-366">kavramsal olarak karşılık gelen</span><span class="sxs-lookup"><span data-stu-id="e7f76-366">conceptually correspond to</span></span>
```csharp
int i = 123;
object box = new Box<int>(i);
```

<span data-ttu-id="e7f76-367">Yukarıdaki `Box<T>` gibi bir paketleme sınıfı aslında yoktur ve paketlenmiş değerin dinamik türü aslında bir sınıf türü değildir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-367">A boxing class like `Box<T>` above doesn't actually exist and the dynamic type of a boxed value isn't actually a class type.</span></span> <span data-ttu-id="e7f76-368">Bunun yerine, `T` türündeki paketlenmiş bir değer dinamik tür `T`ve `is` işleci kullanılarak dinamik bir tür denetimi yalnızca tür `T`başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-368">Instead, a boxed value of type `T` has the dynamic type `T`, and a dynamic type check using the `is` operator can simply reference type `T`.</span></span> <span data-ttu-id="e7f76-369">Örneğin,</span><span class="sxs-lookup"><span data-stu-id="e7f76-369">For example,</span></span>
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
<span data-ttu-id="e7f76-370">, konsolundaki "`Box contains an int`" dizesini çıktısını alır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-370">will output the string "`Box contains an int`" on the console.</span></span>

<span data-ttu-id="e7f76-371">Kutulama dönüştürme, kutulanmış değerin bir kopyasını yapmayı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-371">A boxing conversion implies making a copy of the value being boxed.</span></span> <span data-ttu-id="e7f76-372">Bu, değerin aynı örneğe başvurmaya devam ettiği ve yalnızca daha az türetilmiş tür `object`olarak kabul edilen `object`türünde bir *reference_type* dönüştürmesinden farklıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-372">This is different from a conversion of a *reference_type* to type `object`, in which the value continues to reference the same instance and simply is regarded as the less derived type `object`.</span></span> <span data-ttu-id="e7f76-373">Örneğin, bildirim verildiğinde</span><span class="sxs-lookup"><span data-stu-id="e7f76-373">For example, given the declaration</span></span>
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
<span data-ttu-id="e7f76-374">Aşağıdaki deyimler</span><span class="sxs-lookup"><span data-stu-id="e7f76-374">the following statements</span></span>
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
<span data-ttu-id="e7f76-375">`box` `p` atamasında oluşan örtük paketleme işlemi, `p` değerinin kopyalanmasına neden olduğundan, konsolda 10 değerini çıktı olarak alır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-375">will output the value 10 on the console because the implicit boxing operation that occurs in the assignment of `p` to `box` causes the value of `p` to be copied.</span></span> <span data-ttu-id="e7f76-376">`Point` `class` olarak bildirildiği için, `p` ve `box` aynı örneğe başvuracağından, 20 değeri çıktı olur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-376">Had `Point` been declared a `class` instead, the value 20 would be output because `p` and `box` would reference the same instance.</span></span>

### <a name="unboxing-conversions"></a><span data-ttu-id="e7f76-377">Kutudan çıkarma dönüştürmeleri</span><span class="sxs-lookup"><span data-stu-id="e7f76-377">Unboxing conversions</span></span>

<span data-ttu-id="e7f76-378">Kutudan çıkarma dönüştürmesi, bir *reference_type* açıkça bir *value_type*'e dönüştürülmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-378">An unboxing conversion permits a *reference_type* to be explicitly converted to a *value_type*.</span></span> <span data-ttu-id="e7f76-379">Aşağıdaki kutudan çıkarma dönüştürmeleri mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="e7f76-379">The following unboxing conversions exist:</span></span>

*  <span data-ttu-id="e7f76-380">`object` türünden herhangi bir *value_type*.</span><span class="sxs-lookup"><span data-stu-id="e7f76-380">From the type `object` to any *value_type*.</span></span>
*  <span data-ttu-id="e7f76-381">`System.ValueType` türünden herhangi bir *value_type*.</span><span class="sxs-lookup"><span data-stu-id="e7f76-381">From the type `System.ValueType` to any *value_type*.</span></span>
*  <span data-ttu-id="e7f76-382">Herhangi bir *interface_type* , *interface_type*uygulayan herhangi bir *non_nullable_value_type* .</span><span class="sxs-lookup"><span data-stu-id="e7f76-382">From any *interface_type* to any *non_nullable_value_type* that implements the *interface_type*.</span></span>
*  <span data-ttu-id="e7f76-383">Herhangi bir *interface_type* , temel alınan türü *interface_type*uygulayan herhangi bir *nullable_type* .</span><span class="sxs-lookup"><span data-stu-id="e7f76-383">From any *interface_type* to any *nullable_type* whose underlying type implements the *interface_type*.</span></span>
*  <span data-ttu-id="e7f76-384">`System.Enum` türünden herhangi bir *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="e7f76-384">From the type `System.Enum` to any *enum_type*.</span></span>
*  <span data-ttu-id="e7f76-385">`System.Enum` türü, temel alınan bir *enum_type*ile herhangi bir *nullable_type* .</span><span class="sxs-lookup"><span data-stu-id="e7f76-385">From the type `System.Enum` to any *nullable_type* with an underlying *enum_type*.</span></span>
*  <span data-ttu-id="e7f76-386">Bir tür parametresine açık dönüştürme, bir başvuru türünden bir değer türüne ([Açık dinamik dönüştürmeler](conversions.md#explicit-dynamic-conversions)) dönüştürme sona erdiğinde, bir paket açma dönüştürmesi olarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-386">Note that an explicit conversion to a type parameter will be executed as an unboxing conversion if at run-time it ends up converting from a reference type to a value type ([Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions)).</span></span>

<span data-ttu-id="e7f76-387">Bir *non_nullable_value_type* kutudan çıkarma işlemi, nesne örneğinin verilen *non_nullable_value_type*'ın paketlenmiş bir değeri olup olmadığını kontrol ederek değeri örnekten dışarı kopyalamaktan oluşur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-387">An unboxing operation to a *non_nullable_value_type* consists of first checking that the object instance is a boxed value of the given *non_nullable_value_type*, and then copying the value out of the instance.</span></span>

<span data-ttu-id="e7f76-388">Bir *nullable_type* öğesinin kutudan çıkarma işlemi, kaynak işleneni `null`veya nesne örneğini bir alt nesnenin temel alınan türüne *(Aksi takdirde* ) bir nesne örneğini kutudan çıkarma sonucu olarak, *nullable_type* null değerini üretir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-388">Unboxing to a *nullable_type* produces the null value of the *nullable_type* if the source operand is `null`, or the wrapped result of unboxing the object instance to the underlying type of the *nullable_type* otherwise.</span></span>

<span data-ttu-id="e7f76-389">Önceki bölümde açıklanan sanal paketleme sınıfına işaret eden bir nesne için bir *value_type* `T` bir kutudan çıkarma dönüştürmesi, `((Box<T>)box).value`ifade `box` oluşur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-389">Referring to the imaginary boxing class described in the previous section, an unboxing conversion of an object `box` to a *value_type* `T` consists of executing the expression `((Box<T>)box).value`.</span></span> <span data-ttu-id="e7f76-390">Bu nedenle, deyimler</span><span class="sxs-lookup"><span data-stu-id="e7f76-390">Thus, the statements</span></span>
```csharp
object box = 123;
int i = (int)box;
```
<span data-ttu-id="e7f76-391">kavramsal olarak karşılık gelen</span><span class="sxs-lookup"><span data-stu-id="e7f76-391">conceptually correspond to</span></span>
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

<span data-ttu-id="e7f76-392">Belirli bir *non_nullable_value_type* , çalışma zamanında başarılı olmak için bir kutudan çıkarma dönüştürmesi için, kaynak işleneni değeri, bu *non_nullable_value_type*'ın paketlenmiş değerine bir başvuru olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-392">For an unboxing conversion to a given *non_nullable_value_type* to succeed at run-time, the value of the source operand must be a reference to a boxed value of that *non_nullable_value_type*.</span></span> <span data-ttu-id="e7f76-393">Kaynak işleneni `null`, bir `System.NullReferenceException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-393">If the source operand is `null`, a `System.NullReferenceException` is thrown.</span></span> <span data-ttu-id="e7f76-394">Kaynak işleneni uyumsuz bir nesneye başvuru ise, bir `System.InvalidCastException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-394">If the source operand is a reference to an incompatible object, a `System.InvalidCastException` is thrown.</span></span>

<span data-ttu-id="e7f76-395">Belirli bir *nullable_type* , çalışma zamanında başarılı olmak için bir kutudan çıkarma dönüştürmesi için, kaynak işlenenin değeri `null` ya da *nullable_type*temeldeki *non_nullable_value_type* kutulanmış bir değere başvuru olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-395">For an unboxing conversion to a given *nullable_type* to succeed at run-time, the value of the source operand must be either `null` or a reference to a boxed value of the underlying *non_nullable_value_type* of the *nullable_type*.</span></span> <span data-ttu-id="e7f76-396">Kaynak işleneni uyumsuz bir nesneye başvuru ise, bir `System.InvalidCastException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-396">If the source operand is a reference to an incompatible object, a `System.InvalidCastException` is thrown.</span></span>

## <a name="constructed-types"></a><span data-ttu-id="e7f76-397">Oluşturulmuş türler</span><span class="sxs-lookup"><span data-stu-id="e7f76-397">Constructed types</span></span>

<span data-ttu-id="e7f76-398">Genel tür bildirimi, tek başına, ***tür bağımsız değişkenleri***uygulama yoluyla birçok farklı tür oluşturmak için "Blueprint" olarak kullanılan ***ilişkisiz genel bir türü*** gösterir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-398">A generic type declaration, by itself, denotes an ***unbound generic type*** that is used as a "blueprint" to form many different types, by way of applying ***type arguments***.</span></span> <span data-ttu-id="e7f76-399">Tür bağımsız değişkenleri, genel türün adının hemen ardından gelen açılı ayraç içinde (`<` ve `>`) yazılır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-399">The type arguments are written within angle brackets (`<` and `>`) immediately following the name of the generic type.</span></span> <span data-ttu-id="e7f76-400">En az bir tür bağımsız değişkeni içeren bir türe ***oluşturulmuş tür***denir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-400">A type that includes at least one type argument is called a ***constructed type***.</span></span> <span data-ttu-id="e7f76-401">Oluşturulmuş bir tür, bir tür adının görünebileceği dilde çoğu yerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-401">A constructed type can be used in most places in the language in which a type name can appear.</span></span> <span data-ttu-id="e7f76-402">İlişkisiz genel tür yalnızca bir *typeof_expression* içinde kullanılabilir ([typeof işleci](expressions.md#the-typeof-operator)).</span><span class="sxs-lookup"><span data-stu-id="e7f76-402">An unbound generic type can only be used within a *typeof_expression* ([The typeof operator](expressions.md#the-typeof-operator)).</span></span>

<span data-ttu-id="e7f76-403">Oluşturulan türler, ifadelerde basit adlar ([basit adlar](expressions.md#simple-names)) veya bir üyeye ([üye erişimi](expressions.md#member-access)) erişimi olarak da kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-403">Constructed types can also be used in expressions as simple names ([Simple names](expressions.md#simple-names)) or when accessing a member ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="e7f76-404">Bir *namespace_or_type_name* değerlendirildiğinde, yalnızca doğru sayıda tür parametrelerine sahip genel türler kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-404">When a *namespace_or_type_name* is evaluated, only generic types with the correct number of type parameters are considered.</span></span> <span data-ttu-id="e7f76-405">Bu nedenle, türlerin tür parametrelerine sahip olduğu sürece farklı türleri tanımlamak için aynı tanımlayıcıyı kullanmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-405">Thus, it is possible to use the same identifier to identify different types, as long as the types have different numbers of type parameters.</span></span> <span data-ttu-id="e7f76-406">Bu, genel ve genel olmayan sınıfları aynı programda karıştırdığınızda yararlı olur:</span><span class="sxs-lookup"><span data-stu-id="e7f76-406">This is useful when mixing generic and non-generic classes in the same program:</span></span>

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

<span data-ttu-id="e7f76-407">Bir *type_name* , doğrudan tür parametreleri belirtmese de, oluşturulmuş bir tür tanımlayabilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-407">A *type_name* might identify a constructed type even though it doesn't specify type parameters directly.</span></span> <span data-ttu-id="e7f76-408">Bu, bir türün genel sınıf bildiriminde iç içe kullanıldığı ve kapsayan bildirimin örnek türünün, ad araması için örtük olarak kullanıldığı bir durum olabilir ([genel sınıflarda Iç içe türler](classes.md#nested-types-in-generic-classes)):</span><span class="sxs-lookup"><span data-stu-id="e7f76-408">This can occur where a type is nested within a generic class declaration, and the instance type of the containing declaration is implicitly used for name lookup ([Nested types in generic classes](classes.md#nested-types-in-generic-classes)):</span></span>

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

<span data-ttu-id="e7f76-409">Güvenli olmayan kodda, oluşturulan tür bir *unmanaged_type* ([işaretçi türleri](unsafe-code.md#pointer-types)) olarak kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="e7f76-409">In unsafe code, a constructed type cannot be used as an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

### <a name="type-arguments"></a><span data-ttu-id="e7f76-410">Tür bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="e7f76-410">Type arguments</span></span>

<span data-ttu-id="e7f76-411">Bir tür bağımsız değişkeni listesindeki her bağımsız değişken yalnızca bir *türdür*.</span><span class="sxs-lookup"><span data-stu-id="e7f76-411">Each argument in a type argument list is simply a *type*.</span></span>

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

<span data-ttu-id="e7f76-412">Güvenli olmayan kodda ([güvenli olmayan kod](unsafe-code.md)), bir *type_argument* işaretçi türü olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-412">In unsafe code ([Unsafe code](unsafe-code.md)), a *type_argument* may not be a pointer type.</span></span> <span data-ttu-id="e7f76-413">Her tür bağımsız değişkeni, karşılık gelen tür parametresindeki ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) tüm kısıtlamalara uymalıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-413">Each type argument must satisfy any constraints on the corresponding type parameter ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>

### <a name="open-and-closed-types"></a><span data-ttu-id="e7f76-414">Açık ve kapalı türler</span><span class="sxs-lookup"><span data-stu-id="e7f76-414">Open and closed types</span></span>

<span data-ttu-id="e7f76-415">Tüm türler ***Açık türler*** veya ***kapalı türler***olarak sınıflandırılabilirler.</span><span class="sxs-lookup"><span data-stu-id="e7f76-415">All types can be classified as either ***open types*** or ***closed types***.</span></span> <span data-ttu-id="e7f76-416">Açık tür, tür parametrelerini içeren bir türdür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-416">An open type is a type that involves type parameters.</span></span> <span data-ttu-id="e7f76-417">Daha özel olarak:</span><span class="sxs-lookup"><span data-stu-id="e7f76-417">More specifically:</span></span>

*  <span data-ttu-id="e7f76-418">Bir tür parametresi bir açık türü tanımlar.</span><span class="sxs-lookup"><span data-stu-id="e7f76-418">A type parameter defines an open type.</span></span>
*  <span data-ttu-id="e7f76-419">Bir dizi türü, ve yalnızca kendi öğe türü açık bir tür ise açık bir türdür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-419">An array type is an open type if and only if its element type is an open type.</span></span>
*  <span data-ttu-id="e7f76-420">Oluşturulmuş bir tür, yalnızca bir veya daha fazla bağımsız değişkeni açık bir tür ise açık bir türdür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-420">A constructed type is an open type if and only if one or more of its type arguments is an open type.</span></span> <span data-ttu-id="e7f76-421">Oluşturulmuş iç içe bir tür, yalnızca bir veya daha fazla tür bağımsız değişkeni ya da kapsayan tür bağımsız değişkenleri bir açık tür ise açık bir türdür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-421">A constructed nested type is an open type if and only if one or more of its type arguments or the type arguments of its containing type(s) is an open type.</span></span>

<span data-ttu-id="e7f76-422">Kapalı bir tür, açık tür olmayan bir türdür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-422">A closed type is a type that is not an open type.</span></span>

<span data-ttu-id="e7f76-423">Çalışma zamanında, genel bir tür bildirimi içindeki tüm kod, genel bildirime tür bağımsız değişkenleri uygulanarak oluşturulan kapalı oluşturulmuş bir tür bağlamında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-423">At run-time, all of the code within a generic type declaration is executed in the context of a closed constructed type that was created by applying type arguments to the generic declaration.</span></span> <span data-ttu-id="e7f76-424">Genel tür içindeki her tür parametresi, belirli bir çalışma zamanı türüne bağlanır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-424">Each type parameter within the generic type is bound to a particular run-time type.</span></span> <span data-ttu-id="e7f76-425">Tüm deyimlerin ve ifadelerin çalışma zamanı işleme her zaman kapalı türlerle oluşur ve açık türler yalnızca derleme zamanı işleme sırasında oluşur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-425">The run-time processing of all statements and expressions always occurs with closed types, and open types occur only during compile-time processing.</span></span>

<span data-ttu-id="e7f76-426">Her kapatılan oluşturulmuş türün kendi statik değişkenleri kümesi vardır ve bu, başka bir kapalı oluşturulmuş türle paylaşılmaz.</span><span class="sxs-lookup"><span data-stu-id="e7f76-426">Each closed constructed type has its own set of static variables, which are not shared with any other closed constructed types.</span></span> <span data-ttu-id="e7f76-427">Çalışma zamanında açık bir tür olmadığından, bir açık türle ilişkili statik değişken yoktur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-427">Since an open type does not exist at run-time, there are no static variables associated with an open type.</span></span> <span data-ttu-id="e7f76-428">Aynı ilişkisiz genel türden oluşturulmuşsa ve ilgili tür bağımsız değişkenleri aynı türde ise, bu iki adet oluşturulmuş tür oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-428">Two closed constructed types are the same type if they are constructed from the same unbound generic type, and their corresponding type arguments are the same type.</span></span>

### <a name="bound-and-unbound-types"></a><span data-ttu-id="e7f76-429">Bağlı ve ilişkisiz türler</span><span class="sxs-lookup"><span data-stu-id="e7f76-429">Bound and unbound types</span></span>

<span data-ttu-id="e7f76-430">İlişkisiz terim ***türü*** genel olmayan bir türe veya ilişkisiz genel bir türe başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="e7f76-430">The term ***unbound type*** refers to a non-generic type or an unbound generic type.</span></span> <span data-ttu-id="e7f76-431">Terim ***bağlantılı türü*** genel olmayan bir türe veya oluşturulmuş bir türe başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="e7f76-431">The term ***bound type*** refers to a non-generic type or a constructed type.</span></span>

<span data-ttu-id="e7f76-432">İlişkisiz bir tür, bir tür bildirimiyle belirtilen varlığı ifade eder.</span><span class="sxs-lookup"><span data-stu-id="e7f76-432">An unbound type refers to the entity declared by a type declaration.</span></span> <span data-ttu-id="e7f76-433">İlişkisiz genel tür bir tür değildir ve bir değişken, bağımsız değişken veya dönüş değeri ya da bir temel tür olarak kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="e7f76-433">An unbound generic type is not itself a type, and cannot be used as the type of a variable, argument or return value, or as a base type.</span></span> <span data-ttu-id="e7f76-434">İlişkisiz genel bir türün başvurabilecebileceği tek yapı `typeof` ifadedir ([typeof işleci](expressions.md#the-typeof-operator)).</span><span class="sxs-lookup"><span data-stu-id="e7f76-434">The only construct in which an unbound generic type can be referenced is the `typeof` expression ([The typeof operator](expressions.md#the-typeof-operator)).</span></span>

### <a name="satisfying-constraints"></a><span data-ttu-id="e7f76-435">Yer çağıran kısıtlamalar</span><span class="sxs-lookup"><span data-stu-id="e7f76-435">Satisfying constraints</span></span>

<span data-ttu-id="e7f76-436">Oluşturulmuş bir tür veya genel yönteme başvurulduğunda, sağlanan tür bağımsız değişkenleri genel tür veya yöntemde ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) belirtilen tür parametresi kısıtlamalarına göre denetlenir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-436">Whenever a constructed type or generic method is referenced, the supplied type arguments are checked against the type parameter constraints declared on the generic type or method ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="e7f76-437">Her bir `where` yan tümcesi için, adlandırılmış tür parametresine karşılık gelen `A` tür bağımsız değişkeni her bir kısıtlamaya göre aşağıdaki gibi denetlenir:</span><span class="sxs-lookup"><span data-stu-id="e7f76-437">For each `where` clause, the type argument `A` that corresponds to the named type parameter is checked against each constraint as follows:</span></span>

*  <span data-ttu-id="e7f76-438">Kısıtlama bir sınıf türü, bir arabirim türü veya bir tür parametresi ise, kısıtlamada görünen tür parametrelerinin yerine sağlanan tür bağımsız değişkenleriyle bu kısıtlamayı `C`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-438">If the constraint is a class type, an interface type, or a type parameter, let `C` represent that constraint with the supplied type arguments substituted for any type parameters that appear in the constraint.</span></span> <span data-ttu-id="e7f76-439">Kısıtlamayı karşılamak için, tür `A`, aşağıdakilerden biri `C` türüne dönüştürülebilir olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="e7f76-439">To satisfy the constraint, it must be the case that type `A` is convertible to type `C` by one of the following:</span></span>
    * <span data-ttu-id="e7f76-440">Bir kimlik dönüştürme ([kimlik dönüştürme](conversions.md#identity-conversion))</span><span class="sxs-lookup"><span data-stu-id="e7f76-440">An identity conversion ([Identity conversion](conversions.md#identity-conversion))</span></span>
    * <span data-ttu-id="e7f76-441">Örtük başvuru dönüştürmesi ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions))</span><span class="sxs-lookup"><span data-stu-id="e7f76-441">An implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions))</span></span>
    * <span data-ttu-id="e7f76-442">Bir kutulama dönüştürmesi ([kutulama dönüşümleri](conversions.md#boxing-conversions)), türü null yapılamayan bir değer türüdür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-442">A boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)), provided that type A is a non-nullable value type.</span></span>
    * <span data-ttu-id="e7f76-443">Bir tür parametresinden dolaylı başvuru, paketleme veya tür parametresi dönüştürme `C``A`.</span><span class="sxs-lookup"><span data-stu-id="e7f76-443">An implicit reference, boxing or type parameter conversion from a type parameter `A` to `C`.</span></span>
*  <span data-ttu-id="e7f76-444">Kısıtlama başvuru türü kısıtlamasıdır (`class`) `A` tür, aşağıdakilerden birini karşılamalıdır:</span><span class="sxs-lookup"><span data-stu-id="e7f76-444">If the constraint is the reference type constraint (`class`), the type `A` must satisfy one of the following:</span></span>
    * <span data-ttu-id="e7f76-445">`A` bir arabirim türü, sınıf türü, temsilci türü veya dizi türüdür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-445">`A` is an interface type, class type, delegate type or array type.</span></span> <span data-ttu-id="e7f76-446">`System.ValueType` ve `System.Enum` bu kısıtlamayı karşılayan başvuru türleri olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="e7f76-446">Note that `System.ValueType` and `System.Enum` are reference types that satisfy this constraint.</span></span>
    * <span data-ttu-id="e7f76-447">`A`, başvuru türü olarak bilinen bir tür parametresidir ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="e7f76-447">`A` is a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="e7f76-448">Kısıtlama değer türü kısıtlamasıdır (`struct`) `A` tür, aşağıdakilerden birini karşılamalıdır:</span><span class="sxs-lookup"><span data-stu-id="e7f76-448">If the constraint is the value type constraint (`struct`), the type `A` must satisfy one of the following:</span></span>
    * <span data-ttu-id="e7f76-449">`A`, bir struct türü veya Enum türüdür, ancak null yapılabilir bir tür değildir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-449">`A` is a struct type or enum type, but not a nullable type.</span></span> <span data-ttu-id="e7f76-450">`System.ValueType` ve `System.Enum` bu kısıtlamayı karşılamayan başvuru türleri olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="e7f76-450">Note that `System.ValueType` and `System.Enum` are reference types that do not satisfy this constraint.</span></span>
    * <span data-ttu-id="e7f76-451">`A`, değer türü kısıtlamasına ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) sahip bir tür parametresidir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-451">`A` is a type parameter having the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="e7f76-452">Kısıtlama Oluşturucu kısıtlaması `new()`, tür `A` `abstract` olmamalı ve ortak parametresiz bir oluşturucuya sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-452">If the constraint is the constructor constraint `new()`, the type `A` must not be `abstract` and must have a public parameterless constructor.</span></span> <span data-ttu-id="e7f76-453">Aşağıdakilerden biri doğruysa, bu karşılanır:</span><span class="sxs-lookup"><span data-stu-id="e7f76-453">This is satisfied if one of the following is true:</span></span>
    * <span data-ttu-id="e7f76-454">`A`, tüm değer türleri ortak bir varsayılan oluşturucuya ([Varsayılan oluşturucular](types.md#default-constructors)) sahip olduğundan bir değer türüdür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-454">`A` is a value type, since all value types have a public default constructor ([Default constructors](types.md#default-constructors)).</span></span>
    * <span data-ttu-id="e7f76-455">`A`, Oluşturucu kısıtlamasına sahip bir tür parametresidir ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="e7f76-455">`A` is a type parameter having the constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
    * <span data-ttu-id="e7f76-456">`A`, değer türü kısıtlamasına ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) sahip bir tür parametresidir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-456">`A` is a type parameter having the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
    * <span data-ttu-id="e7f76-457">`A`, `abstract` olmayan ve açıkça tanımlanmış ve parametresi olmayan bir `public` Oluşturucusu içeren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-457">`A` is a class that is not `abstract` and contains an explicitly declared `public` constructor with no parameters.</span></span>
    * <span data-ttu-id="e7f76-458">`A` `abstract` değildir ve varsayılan bir oluşturucuya ([Varsayılan oluşturucular](classes.md#default-constructors)) sahiptir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-458">`A` is not `abstract` and has a default constructor ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="e7f76-459">Bir veya daha fazla tür parametresi kısıtlaması verilen tür bağımsız değişkenleri tarafından karşılanmıyorsa, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-459">A compile-time error occurs if one or more of a type parameter's constraints are not satisfied by the given type arguments.</span></span>

<span data-ttu-id="e7f76-460">Tür parametreleri devralınmadığından, kısıtlamalar hiçbir şekilde devralınmaz.</span><span class="sxs-lookup"><span data-stu-id="e7f76-460">Since type parameters are not inherited, constraints are never inherited either.</span></span> <span data-ttu-id="e7f76-461">Aşağıdaki örnekte `D`, `T` temel `B<T>`sınıf tarafından uygulanan kısıtlamayı karşılayacak şekilde, `T` tür parametresinde kısıtlamayı belirtmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-461">In the example below, `D` needs to specify the constraint on its type parameter `T` so that `T` satisfies the constraint imposed by the base class `B<T>`.</span></span> <span data-ttu-id="e7f76-462">Buna karşılık, `List<T>` herhangi bir `T`için `IEnumerable` uyguladığı için, sınıf `E` bir kısıtlama belirtmemelidir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-462">In contrast, class `E` need not specify a constraint, because `List<T>` implements `IEnumerable` for any `T`.</span></span>

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a><span data-ttu-id="e7f76-463">Tür parametreleri</span><span class="sxs-lookup"><span data-stu-id="e7f76-463">Type parameters</span></span>

<span data-ttu-id="e7f76-464">Tür parametresi, parametrenin çalışma zamanında bağlandığı bir değer türü veya başvuru türü atayarak bir tanıtıcıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-464">A type parameter is an identifier designating a value type or reference type that the parameter is bound to at run-time.</span></span>

```antlr
type_parameter
    : identifier
    ;
```

<span data-ttu-id="e7f76-465">Bir tür parametresi birçok farklı gerçek tür bağımsız değişkeni ile örneklenmiş olduğundan, tür parametrelerinin farklı işlemleri ve kısıtlamaları diğer türlerden farklıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-465">Since a type parameter can be instantiated with many different actual type arguments, type parameters have slightly different operations and restrictions than other types.</span></span> <span data-ttu-id="e7f76-466">Bu güncelleştirmeler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="e7f76-466">These include:</span></span>

*  <span data-ttu-id="e7f76-467">Bir tür parametresi doğrudan bir taban sınıf ([temel sınıf](classes.md#base-class)) veya arabirim ([değişken türü parametre listeleri](interfaces.md#variant-type-parameter-lists)) bildirmek için kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="e7f76-467">A type parameter cannot be used directly to declare a base class ([Base class](classes.md#base-class)) or interface ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)).</span></span>
*  <span data-ttu-id="e7f76-468">Tür parametrelerine üye arama kuralları, varsa tür parametresine uygulanan kısıtlamalara bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-468">The rules for member lookup on type parameters depend on the constraints, if any, applied to the type parameter.</span></span> <span data-ttu-id="e7f76-469">Bunlar [üye aramada](expressions.md#member-lookup)ayrıntılıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-469">They are detailed in [Member lookup](expressions.md#member-lookup).</span></span>
*  <span data-ttu-id="e7f76-470">Bir tür parametresi için kullanılabilir dönüştürmeler, varsa tür parametresine uygulanan kısıtlamalara bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-470">The available conversions for a type parameter depend on the constraints, if any, applied to the type parameter.</span></span> <span data-ttu-id="e7f76-471">Bunlar tür parametreleri ve [Açık dinamik dönüştürmeleri](conversions.md#explicit-dynamic-conversions) [içeren örtük dönüştürmelerde](conversions.md#implicit-conversions-involving-type-parameters) ayrıntılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-471">They are detailed in [Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters) and [Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions).</span></span>
*  <span data-ttu-id="e7f76-472">Tür parametresi bir başvuru türü ([tür parametreleri Içeren örtük dönüştürmeler](conversions.md#implicit-conversions-involving-type-parameters)) olarak bilinmediği sürece, sabit `null` tür parametresi tarafından verilen bir türe dönüştürülemez.</span><span class="sxs-lookup"><span data-stu-id="e7f76-472">The literal `null` cannot be converted to a type given by a type parameter, except if the type parameter is known to be a reference type ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters)).</span></span> <span data-ttu-id="e7f76-473">Ancak, bunun yerine bir `default` ifadesi ([varsayılan değer ifadeleri](expressions.md#default-value-expressions)) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-473">However, a `default` expression ([Default value expressions](expressions.md#default-value-expressions)) can be used instead.</span></span> <span data-ttu-id="e7f76-474">Ayrıca, bir tür parametresi tarafından verilen bir değer, tür parametresi değer türü kısıtlamasına sahip olmadığı takdirde, `==` ve `!=` ([başvuru türü eşitlik işleçleri](expressions.md#reference-type-equality-operators)) kullanılarak `null` karşılaştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-474">In addition, a value with a type given by a type parameter can be compared with `null` using `==` and `!=` ([Reference type equality operators](expressions.md#reference-type-equality-operators)) unless the type parameter has the value type constraint.</span></span>
*  <span data-ttu-id="e7f76-475">`new` ifadesi ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)) yalnızca tür parametresi bir *constructor_constraint* veya değer türü kısıtlaması ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) tarafından kısıtlanmamışsa bir tür parametresiyle birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-475">A `new` expression ([Object creation expressions](expressions.md#object-creation-expressions)) can only be used with a type parameter if the type parameter is constrained by a *constructor_constraint* or the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="e7f76-476">Bir tür parametresi bir öznitelik içinde herhangi bir yerde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="e7f76-476">A type parameter cannot be used anywhere within an attribute.</span></span>
*  <span data-ttu-id="e7f76-477">Bir statik üye veya iç içe bir tür tanımlamak için bir tür parametresi, üye erişimi ([üye erişimi](expressions.md#member-access)) veya tür adı ([ad alanı ve tür adları](basic-concepts.md#namespace-and-type-names)) içinde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="e7f76-477">A type parameter cannot be used in a member access ([Member access](expressions.md#member-access)) or type name ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) to identify a static member or a nested type.</span></span>
*  <span data-ttu-id="e7f76-478">Güvenli olmayan kodda, bir tür parametresi bir *unmanaged_type* ([işaretçi türleri](unsafe-code.md#pointer-types)) olarak kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="e7f76-478">In unsafe code, a type parameter cannot be used as an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

<span data-ttu-id="e7f76-479">Tür olarak, tür parametreleri yalnızca derleme zamanı yapısıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-479">As a type, type parameters are purely a compile-time construct.</span></span> <span data-ttu-id="e7f76-480">Çalışma zamanında, her tür parametresi genel tür bildirimine bir tür bağımsız değişkeni sağlanarak belirtilen bir çalışma zamanı türüne bağlanır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-480">At run-time, each type parameter is bound to a run-time type that was specified by supplying a type argument to the generic type declaration.</span></span> <span data-ttu-id="e7f76-481">Bu nedenle, bir tür parametresiyle belirtilen değişkenin türü, çalışma zamanında, Kapalı oluşturulmuş bir tür ([açık ve kapalı türler](types.md#open-and-closed-types)) olur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-481">Thus, the type of a variable declared with a type parameter will, at run-time, be a closed constructed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="e7f76-482">Tüm deyimlerin ve tür parametreleri içeren ifadelerin çalışma zamanı yürütmesi, bu parametre için tür bağımsız değişkeni olarak sağlanan gerçek türü kullanır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-482">The run-time execution of all statements and expressions involving type parameters uses the actual type that was supplied as the type argument for that parameter.</span></span>

## <a name="expression-tree-types"></a><span data-ttu-id="e7f76-483">İfade ağacı türleri</span><span class="sxs-lookup"><span data-stu-id="e7f76-483">Expression tree types</span></span>

<span data-ttu-id="e7f76-484">***İfade ağaçları*** , lambda ifadelerinin yürütülebilir kod yerine veri yapıları olarak gösterilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-484">***Expression trees*** permit lambda expressions to be represented as data structures instead of executable code.</span></span> <span data-ttu-id="e7f76-485">İfade ağaçları `System.Linq.Expressions.Expression<D>`form ***ifade ağacı türlerinin*** değerlerdir; burada `D` herhangi bir temsilci türüdür.</span><span class="sxs-lookup"><span data-stu-id="e7f76-485">Expression trees are values of ***expression tree types*** of the form `System.Linq.Expressions.Expression<D>`, where `D` is any delegate type.</span></span> <span data-ttu-id="e7f76-486">Bu şartın geri kalanında, bu türlere toplu `Expression<D>`başvuracağız.</span><span class="sxs-lookup"><span data-stu-id="e7f76-486">For the remainder of this specification we will refer to these types using the shorthand `Expression<D>`.</span></span>

<span data-ttu-id="e7f76-487">Lambda ifadesinden bir temsilci türü `D`bir dönüştürme varsa, ifade ağacı türü `Expression<D>`bir dönüştürme de mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-487">If a conversion exists from a lambda expression to a delegate type `D`, a conversion also exists to the expression tree type `Expression<D>`.</span></span> <span data-ttu-id="e7f76-488">Bir lambda ifadesinin bir temsilci türüne dönüştürülmesi, lambda ifadesinin yürütülebilir koduna başvuran bir temsilci oluşturduğunda, bir ifade ağacı türüne dönüştürme lambda ifadesinin ifade ağacı gösterimini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-488">Whereas the conversion of a lambda expression to a delegate type generates a delegate that references executable code for the lambda expression, conversion to an expression tree type creates an expression tree representation of the lambda expression.</span></span>

<span data-ttu-id="e7f76-489">İfade ağaçları, lambda ifadelerinin verimli bellek içi veri temsilleridir ve lambda ifadesinin yapısını saydam ve açık hale getirir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-489">Expression trees are efficient in-memory data representations of lambda expressions and make the structure of the lambda expression transparent and explicit.</span></span>

<span data-ttu-id="e7f76-490">Bir temsilci türü `D`benzer şekilde, `Expression<D>`, `D`ile aynı olan parametre ve dönüş türlerine sahip olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="e7f76-490">Just like a delegate type `D`, `Expression<D>` is said to have parameter and return types, which are the same as those of `D`.</span></span>

<span data-ttu-id="e7f76-491">Aşağıdaki örnek, hem yürütülebilir kod hem de bir ifade ağacı olarak bir lambda ifadesini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e7f76-491">The following example represents a lambda expression both as executable code and as an expression tree.</span></span> <span data-ttu-id="e7f76-492">`Func<int,int>`bir dönüştürme olduğundan, `Expression<Func<int,int>>`için de bir dönüştürme de mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="e7f76-492">Because a conversion exists to `Func<int,int>`, a conversion also exists to `Expression<Func<int,int>>`:</span></span>

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

<span data-ttu-id="e7f76-493">Bu atamaları izleyerek temsilci `del` `x + 1`döndüren bir yönteme başvurur ve ifade ağacı `exp` ifade `x => x + 1`tanımlayan bir veri yapısına başvurur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-493">Following these assignments, the delegate `del` references a method that returns `x + 1`, and the expression tree `exp` references a data structure that describes the expression `x => x + 1`.</span></span>

<span data-ttu-id="e7f76-494">Genel tür `Expression<D>` tam tanımı ve bir lambda ifadesi bir ifade ağacı türüne dönüştürüldüğünde bir ifade ağacı oluşturmak için kesin kurallar, her ikisi de bu belirtim kapsamının dışındadır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-494">The exact definition of the generic type `Expression<D>` as well as the precise rules for constructing an expression tree when a lambda expression is converted to an expression tree type, are both outside the scope of this specification.</span></span>

<span data-ttu-id="e7f76-495">Açık hale getirmek için iki şey önemlidir:</span><span class="sxs-lookup"><span data-stu-id="e7f76-495">Two things are important to make explicit:</span></span>

*  <span data-ttu-id="e7f76-496">Tüm lambda ifadeleri ifade ağaçlarına dönüştürülemez.</span><span class="sxs-lookup"><span data-stu-id="e7f76-496">Not all lambda expressions can be converted to expression trees.</span></span> <span data-ttu-id="e7f76-497">Örneğin, deyim gövdeleriyle lambda ifadeleri ve atama ifadeleri içeren lambda ifadeleri temsil edilemez.</span><span class="sxs-lookup"><span data-stu-id="e7f76-497">For instance, lambda expressions with statement bodies, and lambda expressions containing assignment expressions cannot be represented.</span></span> <span data-ttu-id="e7f76-498">Bu durumlarda, bir dönüştürme hala vardır ancak derleme zamanında başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-498">In these cases, a conversion still exists, but will fail at compile-time.</span></span> <span data-ttu-id="e7f76-499">Bu özel durumlar [anonim işlev dönüştürmelerinde](conversions.md#anonymous-function-conversions)ayrıntılıdır.</span><span class="sxs-lookup"><span data-stu-id="e7f76-499">These exceptions are detailed in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>
*   <span data-ttu-id="e7f76-500">`Expression<D>`, `D`türünde bir temsilci üreten bir örnek yöntemi `Compile` sunar:</span><span class="sxs-lookup"><span data-stu-id="e7f76-500">`Expression<D>` offers an instance method `Compile` which produces a delegate of type `D`:</span></span>

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    <span data-ttu-id="e7f76-501">Bu temsilciyi çağırmak, ifade ağacının gösterdiği kodun yürütülmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-501">Invoking this delegate causes the code represented by the expression tree to be executed.</span></span> <span data-ttu-id="e7f76-502">Bu nedenle, yukarıdaki tanımlar, del ve del2 eşdeğerdir ve aşağıdaki iki deyim aynı etkiye sahip olacaktır:</span><span class="sxs-lookup"><span data-stu-id="e7f76-502">Thus, given the definitions above, del and del2 are equivalent, and the following two statements will have the same effect:</span></span>

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    <span data-ttu-id="e7f76-503">Bu kodu yürüttükten sonra, `i1` ve `i2` her ikisi de `2`değeri olur.</span><span class="sxs-lookup"><span data-stu-id="e7f76-503">After executing this code,  `i1` and `i2` will both have the value `2`.</span></span>

