---
ms.openlocfilehash: c3b716e6eb331be2ee33fffeb42c1e2406cd3a5c
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488765"
---
# <a name="enums"></a><span data-ttu-id="d7ca3-101">Numaralandırmalar</span><span class="sxs-lookup"><span data-stu-id="d7ca3-101">Enums</span></span>

<span data-ttu-id="d7ca3-102">Bir ***sabit listesi türü*** ayrı değer türü ([değer türleri](types.md#value-types)), adlandırılmış sabitler kümesini bildirir.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-102">An ***enum type*** is a distinct value type ([Value types](types.md#value-types)) that declares a set of named constants.</span></span>

<span data-ttu-id="d7ca3-103">Örnek</span><span class="sxs-lookup"><span data-stu-id="d7ca3-103">The example</span></span>

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="d7ca3-104">adlı bir sabit listesi türü bildirir `Color` üyeleriyle `Red`, `Green`, ve `Blue`.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-104">declares an enum type named `Color` with members `Red`, `Green`, and `Blue`.</span></span>

## <a name="enum-declarations"></a><span data-ttu-id="d7ca3-105">Enum bildirimleri</span><span class="sxs-lookup"><span data-stu-id="d7ca3-105">Enum declarations</span></span>

<span data-ttu-id="d7ca3-106">Bir numaralandırma bildirimi yeni bir sabit listesi türü bildirir.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-106">An enum declaration declares a new enum type.</span></span> <span data-ttu-id="d7ca3-107">With anahtar sözcüğü bir numaralandırma bildirimi başlar `enum`, adı, erişilebilirlik, temel alınan türü ve numaralandırma üyeleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-107">An enum declaration begins with the keyword `enum`, and defines the name, accessibility, underlying type, and members of the enum.</span></span>

```antlr
enum_declaration
    : attributes? enum_modifier* 'enum' identifier enum_base? enum_body ';'?
    ;

enum_base
    : ':' integral_type
    ;

enum_body
    : '{' enum_member_declarations? '}'
    | '{' enum_member_declarations ',' '}'
    ;
```

<span data-ttu-id="d7ca3-108">Her bir numaralandırma türünün adlı karşılık gelen bir integral türü olan ***temel alınan türü*** sabit listesi türü.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-108">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="d7ca3-109">Bu temel türünü numaralandırmada tanımlanan tüm Numaralayıcı değerleri temsil etmesi mümkün olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-109">This underlying type must be able to represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="d7ca3-110">Bir numaralandırma bildirimi bir temel alınan türü açıkça bildirebilir `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` veya `ulong`.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-110">An enum declaration may explicitly declare an underlying type of `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="d7ca3-111">Unutmayın `char` bir temel türü kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-111">Note that `char` cannot be used as an underlying type.</span></span> <span data-ttu-id="d7ca3-112">Bir temel türü açıkça bildirmiyor bir numaralandırma bildirimi bir temel türüne sahip `int`.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-112">An enum declaration that does not explicitly declare an underlying type has an underlying type of `int`.</span></span>

<span data-ttu-id="d7ca3-113">Örnek</span><span class="sxs-lookup"><span data-stu-id="d7ca3-113">The example</span></span>

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="d7ca3-114">temel alınan bir türü ile bir sabit listesi bildirir `long`.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-114">declares an enum with an underlying type of `long`.</span></span> <span data-ttu-id="d7ca3-115">Bir geliştirici, temel alınan bir türünü kullanmayı tercih edebilirsiniz `long`aralığında olan değerleri kullanımını etkinleştirmek için örnekte gibi `long` ancak aralığını `int`, veya gelecek için bu seçeneği korumak için.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-115">A developer might choose to use an underlying type of `long`, as in the example, to enable the use of values that are in the range of `long` but not in the range of `int`, or to preserve this option for the future.</span></span>

## <a name="enum-modifiers"></a><span data-ttu-id="d7ca3-116">Enum değiştiriciler</span><span class="sxs-lookup"><span data-stu-id="d7ca3-116">Enum modifiers</span></span>

<span data-ttu-id="d7ca3-117">Bir *enum_declaration* isteğe bağlı olarak bir dizi sabit listesi değiştiriciler içerebilir:</span><span class="sxs-lookup"><span data-stu-id="d7ca3-117">An *enum_declaration* may optionally include a sequence of enum modifiers:</span></span>

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

<span data-ttu-id="d7ca3-118">Aynı değiştiricisi bir sabit listesi bildiriminde birden çok kez görünmesi için bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-118">It is a compile-time error for the same modifier to appear multiple times in an enum declaration.</span></span>

<span data-ttu-id="d7ca3-119">Bu sınıf bildirimi aynı anlamı, bir numaralandırma bildirimi, değiştiricilere sahip ([sınıfı değiştiricileri](classes.md#class-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="d7ca3-119">The modifiers of an enum declaration have the same meaning as those of a class declaration ([Class modifiers](classes.md#class-modifiers)).</span></span> <span data-ttu-id="d7ca3-120">Ancak, unutmayın, `abstract` ve `sealed` değiştiriciler bir sabit listesi bildiriminde izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-120">Note, however, that the `abstract` and `sealed` modifiers are not permitted in an enum declaration.</span></span> <span data-ttu-id="d7ca3-121">Numaralandırmalar, soyut olamaz ve türetme tanımaz.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-121">Enums cannot be abstract and do not permit derivation.</span></span>

## <a name="enum-members"></a><span data-ttu-id="d7ca3-122">Numaralandırma üyeleri</span><span class="sxs-lookup"><span data-stu-id="d7ca3-122">Enum members</span></span>

<span data-ttu-id="d7ca3-123">Bir numaralandırma türü bildirimi gövdesi adlandırılmış sabitler sabit listesi türü, sıfır veya daha fazla numaralandırma üyeleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-123">The body of an enum type declaration defines zero or more enum members, which are the named constants of the enum type.</span></span> <span data-ttu-id="d7ca3-124">İki sabit listesi üye aynı ada sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-124">No two enum members can have the same name.</span></span>

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

<span data-ttu-id="d7ca3-125">Her sabit listesi üyesi, ilişkili bir sabit değere sahip.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-125">Each enum member has an associated constant value.</span></span> <span data-ttu-id="d7ca3-126">Bu değer içeren numaralandırması için temeldeki türü türüdür.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-126">The type of this value is the underlying type for the containing enum.</span></span> <span data-ttu-id="d7ca3-127">Her sabit listesi üyesi için sabit değerin numaralandırması için temeldeki türü aralığında olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-127">The constant value for each enum member must be in the range of the underlying type for the enum.</span></span> <span data-ttu-id="d7ca3-128">Örnek</span><span class="sxs-lookup"><span data-stu-id="d7ca3-128">The example</span></span>

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

<span data-ttu-id="d7ca3-129">sonuçları bir derleme zamanı hatası nedeniyle sabit değerler `-1`, `-2`, ve `-3` integral türünün temel alınan bir aralıkta yer `uint`.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-129">results in a compile-time error because the constant values `-1`, `-2`, and `-3` are not in the range of the underlying integral type `uint`.</span></span>

<span data-ttu-id="d7ca3-130">Birden çok sabit listesi üye aynı ilişkili değer paylaşabilir.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-130">Multiple enum members may share the same associated value.</span></span> <span data-ttu-id="d7ca3-131">Örnek</span><span class="sxs-lookup"><span data-stu-id="d7ca3-131">The example</span></span>

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

<span data-ttu-id="d7ca3-132">içinde iki numaralandırma üyeleri--enum gösterir `Blue` ve `Max` --aynı değeri ilişkilendirdiniz.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-132">shows an enum in which two enum members -- `Blue` and `Max` -- have the same associated value.</span></span>

<span data-ttu-id="d7ca3-133">İlişkili değer enum üyesini örtük veya açık olarak atanır.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-133">The associated value of an enum member is assigned either implicitly or explicitly.</span></span> <span data-ttu-id="d7ca3-134">Sabit listesi üyesi bildirimi varsa bir *constant_expression* Başlatıcı, temel alınan numaralandırma türüne örtük olarak dönüştürülen bu sabit ifade değerini sabit listesi üyesi ilişkili değeridir.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-134">If the declaration of the enum member has a *constant_expression* initializer, the value of that constant expression, implicitly converted to the underlying type of the enum, is the associated value of the enum member.</span></span> <span data-ttu-id="d7ca3-135">Sabit listesi üyesi bildirimi Başlatıcı varsa, ilişkili değeri örtük olarak, şu şekilde ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="d7ca3-135">If the declaration of the enum member has no initializer, its associated value is set implicitly, as follows:</span></span>

*  <span data-ttu-id="d7ca3-136">Sabit listesi üyesi enum türünde bildirilen ilk sabit listesi üyesi ise, ilişkili değeri sıfırdır.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-136">If the enum member is the first enum member declared in the enum type, its associated value is zero.</span></span>
*  <span data-ttu-id="d7ca3-137">Aksi takdirde, sabit listesi üyesi ilişkili değerini bir metin içeriğini eklemek önceki sabit listesi üyesi ilişkili değerini artırarak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-137">Otherwise, the associated value of the enum member is obtained by increasing the associated value of the textually preceding enum member by one.</span></span> <span data-ttu-id="d7ca3-138">Daha fazla bu değer temel alınan türü tarafından temsil edilebilir değerler aralığında olması gerekir, aksi takdirde bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-138">This increased value must be within the range of values that can be represented by the underlying type, otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="d7ca3-139">Örnek</span><span class="sxs-lookup"><span data-stu-id="d7ca3-139">The example</span></span>

```csharp
using System;

enum Color
{
    Red,
    Green = 10,
    Blue
}

class Test
{
    static void Main() {
        Console.WriteLine(StringFromColor(Color.Red));
        Console.WriteLine(StringFromColor(Color.Green));
        Console.WriteLine(StringFromColor(Color.Blue));
    }

    static string StringFromColor(Color c) {
        switch (c) {
            case Color.Red: 
                return String.Format("Red = {0}", (int) c);

            case Color.Green:
                return String.Format("Green = {0}", (int) c);

            case Color.Blue:
                return String.Format("Blue = {0}", (int) c);

            default:
                return "Invalid color";
        }
    }
}
```

<span data-ttu-id="d7ca3-140">sabit listesi üye adları ve ilişkili değerleri yazdırır.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-140">prints out the enum member names and their associated values.</span></span> <span data-ttu-id="d7ca3-141">Çıktı.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-141">The output is:</span></span>

```
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="d7ca3-142">Aşağıdaki nedenlerle:</span><span class="sxs-lookup"><span data-stu-id="d7ca3-142">for the following reasons:</span></span>

*  <span data-ttu-id="d7ca3-143">sabit listesi üyesi `Red` değeri sıfır (bunu Başlatıcı içermiyor ve ilk sabit listesi üyesi olduğundan); otomatik olarak atanır</span><span class="sxs-lookup"><span data-stu-id="d7ca3-143">the enum member `Red` is automatically assigned the value zero (since it has no initializer and is the first enum member);</span></span>
*  <span data-ttu-id="d7ca3-144">sabit listesi üyesi `Green` değeri açıkça verilir `10`;</span><span class="sxs-lookup"><span data-stu-id="d7ca3-144">the enum member `Green` is explicitly given the value `10`;</span></span>
*  <span data-ttu-id="d7ca3-145">ve sabit listesi üyesi `Blue` değeri metin içeriğini eklemek kendisinden üyenin büyük bir otomatik olarak atanır.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-145">and the enum member `Blue` is automatically assigned the value one greater than the member that textually precedes it.</span></span>

<span data-ttu-id="d7ca3-146">İlişkili değeri bir sabit listesi üyesi olabilir değil, doğrudan veya dolaylı olarak kendi ilişkili sabit listesi üyesi değerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-146">The associated value of an enum member may not, directly or indirectly, use the value of its own associated enum member.</span></span> <span data-ttu-id="d7ca3-147">Bu döngü kısıtlama dışında sabit listesi üyesi başlatıcıları metinsel konumlarına bakmaksızın diğer sabit listesi üyesi başlatıcıları serbestçe başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-147">Other than this circularity restriction, enum member initializers may freely refer to other enum member initializers, regardless of their textual position.</span></span> <span data-ttu-id="d7ca3-148">Böylece yayınları diğer enum üyelerine başvuru yaparken gerekli olmayan bir sabit listesi üyesi başlatıcısı içinde diğer sabit listesi üyelerinin değerlerini her zaman kendi temel türü türünü varmış gibi davranılır.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-148">Within an enum member initializer, values of other enum members are always treated as having the type of their underlying type, so that casts are not necessary when referring to other enum members.</span></span>

<span data-ttu-id="d7ca3-149">Örnek</span><span class="sxs-lookup"><span data-stu-id="d7ca3-149">The example</span></span>

```csharp
enum Circular
{
    A = B,
    B
}
```

<span data-ttu-id="d7ca3-150">sonuçları bir derleme zamanı hatası nedeniyle bildirimlerini `A` ve `B` döngüsel olan.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-150">results in a compile-time error because the declarations of `A` and `B` are circular.</span></span> <span data-ttu-id="d7ca3-151">`A` bağımlı `B` açıkça ve `B` bağlıdır `A` örtük olarak.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-151">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

<span data-ttu-id="d7ca3-152">Numaralandırma üyeleri adlı ve sınıfları alanları tam olarak benzer bir şekilde kapsamlı.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-152">Enum members are named and scoped in a manner exactly analogous to fields within classes.</span></span> <span data-ttu-id="d7ca3-153">Sabit listesi üyesi kapsamını içeren enum türü gövdesidir.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-153">The scope of an enum member is the body of its containing enum type.</span></span> <span data-ttu-id="d7ca3-154">Bu kapsam içinde numaralandırma üyeleri için basit adlarına göre adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-154">Within that scope, enum members can be referred to by their simple name.</span></span> <span data-ttu-id="d7ca3-155">Diğer tüm koddan bir sabit listesi üyesi adını numaralandırma türü adı ile nitelenmelidir.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-155">From all other code, the name of an enum member must be qualified with the name of its enum type.</span></span> <span data-ttu-id="d7ca3-156">Numaralandırma üyeleri tüm bildirilen erişilebilirliği yok--sabit listesi üyesi, kapsayan bir enum türü erişilemiyorsa erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-156">Enum members do not have any declared accessibility -- an enum member is accessible if its containing enum type is accessible.</span></span>

## <a name="the-systemenum-type"></a><span data-ttu-id="d7ca3-157">System.Enum Türü</span><span class="sxs-lookup"><span data-stu-id="d7ca3-157">The System.Enum type</span></span>

<span data-ttu-id="d7ca3-158">Türü `System.Enum` soyut temel sınıf (Bu, ayrı ve sabit listesi türü temel alınan türünden farklı) tüm sabit listesi türleri ve üyeleri devralınan `System.Enum` herhangi bir sabit listesi türü kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-158">The type `System.Enum` is the abstract base class of all enum types (this is distinct and different from the underlying type of the enum type), and the members inherited from `System.Enum` are available in any enum type.</span></span> <span data-ttu-id="d7ca3-159">Kutulama dönüştürmesi ([kutulama dönüştürmeler](types.md#boxing-conversions)) herhangi bir sabit listesi Türü alanından mevcut `System.Enum`ve bir paket açma dönüşümünün ([kutudan çıkarma dönüştürme](types.md#unboxing-conversions)) öğesinden var. `System.Enum` herhangi bir sabit listesi türü için.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-159">A boxing conversion ([Boxing conversions](types.md#boxing-conversions)) exists from any enum type to `System.Enum`, and an unboxing conversion ([Unboxing conversions](types.md#unboxing-conversions)) exists from `System.Enum` to any enum type.</span></span>

<span data-ttu-id="d7ca3-160">Unutmayın `System.Enum` kendisi değil bir *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-160">Note that `System.Enum` is not itself an *enum_type*.</span></span> <span data-ttu-id="d7ca3-161">Bunun yerine, olan bir *class_type* tüm gelen *enum_type*s türetilir.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-161">Rather, it is a *class_type* from which all *enum_type*s are derived.</span></span> <span data-ttu-id="d7ca3-162">Türü `System.Enum` tür tarafından devralındığında `System.ValueType` ([System.ValueType türü](types.md#the-systemvaluetype-type)), buna karşılık, tür tarafından devralındığında `object`.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-162">The type `System.Enum` inherits from the type `System.ValueType` ([The System.ValueType type](types.md#the-systemvaluetype-type)), which, in turn, inherits from type `object`.</span></span> <span data-ttu-id="d7ca3-163">Çalışma zamanında, türünde bir değer `System.Enum` olabilir `null` veya paketlenmiş değere herhangi bir enum tür başvurusu.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-163">At run-time, a value of type `System.Enum` can be `null` or a reference to a boxed value of any enum type.</span></span>

## <a name="enum-values-and-operations"></a><span data-ttu-id="d7ca3-164">Sabit listesi değerlerinin ve işlemler</span><span class="sxs-lookup"><span data-stu-id="d7ca3-164">Enum values and operations</span></span>

<span data-ttu-id="d7ca3-165">Her bir numaralandırma türünün farklı bir tür tanımlar; Açık numaralandırma dönüştürme ([açık numaralandırma dönüştürmeler](conversions.md#explicit-enumeration-conversions)) enum türü ve integral türünde arasında veya iki sabit listesi türleri arasında dönüştürme için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-165">Each enum type defines a distinct type; an explicit enumeration conversion ([Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)) is required to convert between an enum type and an integral type, or between two enum types.</span></span> <span data-ttu-id="d7ca3-166">Enum türü alan değerleri kümesi numaralandırma üyelerini sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-166">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="d7ca3-167">Özellikle, bir sabit listesi türünü temel herhangi bir değerini numaralandırma türüne dönüştürülebilen ve sabit listesi türü farklı geçerli bir değer.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-167">In particular, any value of the underlying type of an enum can be cast to the enum type, and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="d7ca3-168">Numaralandırma üyeleri kendi içeren numaralandırma türünün sahiptir (dışındaki diğer sabit listesi üyesi başlatıcıları içinde: bkz [numaralandırma üyeleri](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="d7ca3-168">Enum members have the type of their containing enum type (except within other enum member initializers: see [Enum members](enums.md#enum-members)).</span></span> <span data-ttu-id="d7ca3-169">Sabit listesi üyesi değerini sabit listesi türünde bildirilen `E` ilişkili değeriyle `v` olduğu `(E)v`.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-169">The value of an enum member declared in enum type `E` with associated value `v` is `(E)v`.</span></span>

<span data-ttu-id="d7ca3-170">Aşağıdaki işleçleri, değerleri sabit listesi türleri üzerinde kullanılabilir: `==`, `!=`, `<`, `>`, `<=`, `>=`  ([numaralandırma Karşılaştırma işleçleri](expressions.md#enumeration-comparison-operators)) , ikili `+`  ([Toplama işleci](expressions.md#addition-operator)), ikili `-`  ([çıkarma işleci](expressions.md#subtraction-operator)), `^`, `&` , `|`  ([Numaralandırma mantıksal işleçler](expressions.md#enumeration-logical-operators)), `~`  ([bit düzeyinde tamamlama işleci](expressions.md#bitwise-complement-operator)), `++` ve `--`  ([Sonek arttırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators) ve [önek arttırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="d7ca3-170">The following operators can be used on values of enum types: `==`, `!=`, `<`, `>`, `<=`, `>=` ([Enumeration comparison operators](expressions.md#enumeration-comparison-operators)), binary `+` ([Addition operator](expressions.md#addition-operator)), binary `-` ([Subtraction operator](expressions.md#subtraction-operator)), `^`, `&`, `|` ([Enumeration logical operators](expressions.md#enumeration-logical-operators)), `~` ([Bitwise complement operator](expressions.md#bitwise-complement-operator)), `++` and `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="d7ca3-171">Her sabit listesi türü otomatik olarak sınıfından türetilen `System.Enum` (, sırasıyla türetilir `System.ValueType` ve `object`).</span><span class="sxs-lookup"><span data-stu-id="d7ca3-171">Every enum type automatically derives from the class `System.Enum` (which, in turn, derives from `System.ValueType` and `object`).</span></span> <span data-ttu-id="d7ca3-172">Bu nedenle, devralınan yöntemler ve özellikler bu sınıfın bir numaralandırma türünün değerleri üzerinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d7ca3-172">Thus, inherited methods and properties of this class can be used on values of an enum type.</span></span>
