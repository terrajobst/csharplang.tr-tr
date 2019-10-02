---
ms.openlocfilehash: 3b142d7dbda8a94a4cf2c973a2e380065dcbf5ee
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703962"
---
# <a name="enums"></a><span data-ttu-id="a57a4-101">Numaralandırmalar</span><span class="sxs-lookup"><span data-stu-id="a57a4-101">Enums</span></span>

<span data-ttu-id="a57a4-102">***Sabit listesi türü*** , adlandırılmış sabitler kümesini bildiren ayrı bir değer türüdür ([değer türleri](types.md#value-types)).</span><span class="sxs-lookup"><span data-stu-id="a57a4-102">An ***enum type*** is a distinct value type ([Value types](types.md#value-types)) that declares a set of named constants.</span></span>

<span data-ttu-id="a57a4-103">Örnek</span><span class="sxs-lookup"><span data-stu-id="a57a4-103">The example</span></span>

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="a57a4-104">-1, `Green` ve `Blue` @no__t üyelere sahip `Color` adlı bir sabit listesi türü bildirir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-104">declares an enum type named `Color` with members `Red`, `Green`, and `Blue`.</span></span>

## <a name="enum-declarations"></a><span data-ttu-id="a57a4-105">Sabit Listesi bildirimleri</span><span class="sxs-lookup"><span data-stu-id="a57a4-105">Enum declarations</span></span>

<span data-ttu-id="a57a4-106">Sabit listesi bildirimi yeni bir sabit listesi türü bildirir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-106">An enum declaration declares a new enum type.</span></span> <span data-ttu-id="a57a4-107">Enum bildirimi `enum` anahtar sözcüğüyle başlar ve ad, erişilebilirlik, temel türü ve sabit listesinin üyelerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="a57a4-107">An enum declaration begins with the keyword `enum`, and defines the name, accessibility, underlying type, and members of the enum.</span></span>

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

<span data-ttu-id="a57a4-108">Her sabit listesi türünün, enum türünün ***temel alınan türü*** olarak adlandırılan karşılık gelen bir integral türü vardır.</span><span class="sxs-lookup"><span data-stu-id="a57a4-108">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="a57a4-109">Bu temel tür, numaralandırmada tanımlanan tüm Numaralandırıcı değerlerini temsil edebilmelidir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-109">This underlying type must be able to represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="a57a4-110">Sabit listesi bildirimi, `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` veya `ulong` türündeki temel bir türü açıkça bildirebilir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-110">An enum declaration may explicitly declare an underlying type of `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="a57a4-111">@No__t-0 ' ın temel alınan bir tür olarak kullanılamayacağını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="a57a4-111">Note that `char` cannot be used as an underlying type.</span></span> <span data-ttu-id="a57a4-112">Temel alınan bir türü açıkça bildirmiyor bir numaralandırma bildirimi, temel alınan bir tür `int` ' dır.</span><span class="sxs-lookup"><span data-stu-id="a57a4-112">An enum declaration that does not explicitly declare an underlying type has an underlying type of `int`.</span></span>

<span data-ttu-id="a57a4-113">Örnek</span><span class="sxs-lookup"><span data-stu-id="a57a4-113">The example</span></span>

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="a57a4-114">`long` temel türüne sahip bir sabit listesi bildirir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-114">declares an enum with an underlying type of `long`.</span></span> <span data-ttu-id="a57a4-115">Geliştirici, örneğin `long` aralığında olan ancak `int` aralığında olmayan değerlerin kullanımını etkinleştirmek ya da gelecekte bu seçeneği korumak için `long` ' ın temel bir türünü kullanmayı tercih edebilir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-115">A developer might choose to use an underlying type of `long`, as in the example, to enable the use of values that are in the range of `long` but not in the range of `int`, or to preserve this option for the future.</span></span>

## <a name="enum-modifiers"></a><span data-ttu-id="a57a4-116">Sabit Listesi değiştiricileri</span><span class="sxs-lookup"><span data-stu-id="a57a4-116">Enum modifiers</span></span>

<span data-ttu-id="a57a4-117">Bir *enum_declaration* isteğe bağlı olarak bir sabit listesi değiştiricileri sırası içerebilir:</span><span class="sxs-lookup"><span data-stu-id="a57a4-117">An *enum_declaration* may optionally include a sequence of enum modifiers:</span></span>

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

<span data-ttu-id="a57a4-118">Aynı değiştiricinin bir numaralandırma bildiriminde birden çok kez görünmesi için derleme zamanı hatası vardır.</span><span class="sxs-lookup"><span data-stu-id="a57a4-118">It is a compile-time error for the same modifier to appear multiple times in an enum declaration.</span></span>

<span data-ttu-id="a57a4-119">Bir numaralandırma bildiriminin değiştiricileri, bir sınıf bildirimiyle ([sınıf değiştiricileri](classes.md#class-modifiers)) aynı anlama sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-119">The modifiers of an enum declaration have the same meaning as those of a class declaration ([Class modifiers](classes.md#class-modifiers)).</span></span> <span data-ttu-id="a57a4-120">Ancak, `abstract` ve `sealed` değiştiricilerine bir numaralandırma bildiriminde izin verilmeyeceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="a57a4-120">Note, however, that the `abstract` and `sealed` modifiers are not permitted in an enum declaration.</span></span> <span data-ttu-id="a57a4-121">Numaralandırmalar özet olamaz ve türetmeye izin vermez.</span><span class="sxs-lookup"><span data-stu-id="a57a4-121">Enums cannot be abstract and do not permit derivation.</span></span>

## <a name="enum-members"></a><span data-ttu-id="a57a4-122">Sabit listesi üyeleri</span><span class="sxs-lookup"><span data-stu-id="a57a4-122">Enum members</span></span>

<span data-ttu-id="a57a4-123">Enum türü bildiriminin gövdesi, sabit listesi türünün adlandırılmış sabitleri olan sıfır veya daha fazla numaralandırma üyesini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="a57a4-123">The body of an enum type declaration defines zero or more enum members, which are the named constants of the enum type.</span></span> <span data-ttu-id="a57a4-124">İki numaralandırma üyesi aynı ada sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="a57a4-124">No two enum members can have the same name.</span></span>

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

<span data-ttu-id="a57a4-125">Her sabit listesi üyesinin ilişkili bir sabit değeri vardır.</span><span class="sxs-lookup"><span data-stu-id="a57a4-125">Each enum member has an associated constant value.</span></span> <span data-ttu-id="a57a4-126">Bu değerin türü, kapsayan Enum için temel alınan türdür.</span><span class="sxs-lookup"><span data-stu-id="a57a4-126">The type of this value is the underlying type for the containing enum.</span></span> <span data-ttu-id="a57a4-127">Her bir sabit listesi üyesinin sabit değeri, sabit listesinin temel alınan türünün aralığında olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a57a4-127">The constant value for each enum member must be in the range of the underlying type for the enum.</span></span> <span data-ttu-id="a57a4-128">Örnek</span><span class="sxs-lookup"><span data-stu-id="a57a4-128">The example</span></span>

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

<span data-ttu-id="a57a4-129">`-1`, `-2` ve `-3` sabit değerleri temel alınan integral türü `uint` aralığında olmadığından, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="a57a4-129">results in a compile-time error because the constant values `-1`, `-2`, and `-3` are not in the range of the underlying integral type `uint`.</span></span>

<span data-ttu-id="a57a4-130">Birden çok enum üyesi aynı ilişkili değeri paylaşabilir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-130">Multiple enum members may share the same associated value.</span></span> <span data-ttu-id="a57a4-131">Örnek</span><span class="sxs-lookup"><span data-stu-id="a57a4-131">The example</span></span>

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

<span data-ttu-id="a57a4-132">iki sabit listesi üyesinin--`Blue` ve `Max` ile aynı ilişkili değere sahip bir Enum gösterir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-132">shows an enum in which two enum members -- `Blue` and `Max` -- have the same associated value.</span></span>

<span data-ttu-id="a57a4-133">Sabit Listesi üyesinin ilişkili değeri örtük veya açık olarak atanır.</span><span class="sxs-lookup"><span data-stu-id="a57a4-133">The associated value of an enum member is assigned either implicitly or explicitly.</span></span> <span data-ttu-id="a57a4-134">Enum üyesinin bildiriminde bir *constant_expression* başlatıcısı varsa, bu sabit ifadenin değeri örtük olarak numaralandırmanın temel alınan türüne dönüştürüldü, enum üyesinin ilişkili değeridir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-134">If the declaration of the enum member has a *constant_expression* initializer, the value of that constant expression, implicitly converted to the underlying type of the enum, is the associated value of the enum member.</span></span> <span data-ttu-id="a57a4-135">Enum üyesinin bildiriminde Başlatıcı yoksa, ilişkili değeri örtük olarak aşağıdaki gibi ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="a57a4-135">If the declaration of the enum member has no initializer, its associated value is set implicitly, as follows:</span></span>

*  <span data-ttu-id="a57a4-136">Sabit listesi üyesi, numaralandırma türünde belirtilen ilk enum üyesiyse, ilişkili değeri sıfırdır.</span><span class="sxs-lookup"><span data-stu-id="a57a4-136">If the enum member is the first enum member declared in the enum type, its associated value is zero.</span></span>
*  <span data-ttu-id="a57a4-137">Aksi takdirde, enum üyesinin ilişkili değeri, metin içeriğini eklemek önceki enum üyesinin ilişkili değeri bir ile arttırılarak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-137">Otherwise, the associated value of the enum member is obtained by increasing the associated value of the textually preceding enum member by one.</span></span> <span data-ttu-id="a57a4-138">Bu artış değeri, temel alınan tür tarafından gösterilebilen değer aralığı içinde olmalıdır, aksi takdirde bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="a57a4-138">This increased value must be within the range of values that can be represented by the underlying type, otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="a57a4-139">Örnek</span><span class="sxs-lookup"><span data-stu-id="a57a4-139">The example</span></span>

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

<span data-ttu-id="a57a4-140">sabit listesi üye adlarını ve bunlarla ilişkili değerleri yazdırır.</span><span class="sxs-lookup"><span data-stu-id="a57a4-140">prints out the enum member names and their associated values.</span></span> <span data-ttu-id="a57a4-141">Çıktı:</span><span class="sxs-lookup"><span data-stu-id="a57a4-141">The output is:</span></span>

```console
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="a57a4-142">Aşağıdaki nedenlerle:</span><span class="sxs-lookup"><span data-stu-id="a57a4-142">for the following reasons:</span></span>

*  <span data-ttu-id="a57a4-143">`Red` sabit listesi üyesi, otomatik olarak sıfır değerine atanır (Başlatıcı olmadığından ve ilk sabit listesi üyesi olduğundan);</span><span class="sxs-lookup"><span data-stu-id="a57a4-143">the enum member `Red` is automatically assigned the value zero (since it has no initializer and is the first enum member);</span></span>
*  <span data-ttu-id="a57a4-144">`Green` sabit listesi üyesi, `10`; değerine açıkça verilir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-144">the enum member `Green` is explicitly given the value `10`;</span></span>
*  <span data-ttu-id="a57a4-145">`Blue` enum üyesi, metin içeriğini eklemek kendisinden önce gelen Üyeden bir daha büyük değere otomatik olarak atanır.</span><span class="sxs-lookup"><span data-stu-id="a57a4-145">and the enum member `Blue` is automatically assigned the value one greater than the member that textually precedes it.</span></span>

<span data-ttu-id="a57a4-146">Enum üyesinin ilişkili değeri, doğrudan veya dolaylı olarak kendi ilişkili enum üyesinin değerini kullanamaz.</span><span class="sxs-lookup"><span data-stu-id="a57a4-146">The associated value of an enum member may not, directly or indirectly, use the value of its own associated enum member.</span></span> <span data-ttu-id="a57a4-147">Bu döngü kısıtlamasından farklı olarak, sabit listesi üye başlatıcıları, metinsel konumlarından bağımsız olarak diğer sabit listesi üye başlatıcılarına serbestçe başvurabilirler.</span><span class="sxs-lookup"><span data-stu-id="a57a4-147">Other than this circularity restriction, enum member initializers may freely refer to other enum member initializers, regardless of their textual position.</span></span> <span data-ttu-id="a57a4-148">Bir sabit listesi üye başlatıcısı içinde, diğer sabit listesi üyelerinin değerleri her zaman kendi temel türlerinin türüne sahip olarak değerlendirilir ve bu sayede diğer sabit listesi üyelerine başvuru yaparken yapılan yayınlar gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-148">Within an enum member initializer, values of other enum members are always treated as having the type of their underlying type, so that casts are not necessary when referring to other enum members.</span></span>

<span data-ttu-id="a57a4-149">Örnek</span><span class="sxs-lookup"><span data-stu-id="a57a4-149">The example</span></span>

```csharp
enum Circular
{
    A = B,
    B
}
```

<span data-ttu-id="a57a4-150">`A` ve `B` bildirimleri dairesel olduğundan derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="a57a4-150">results in a compile-time error because the declarations of `A` and `B` are circular.</span></span> <span data-ttu-id="a57a4-151">`A` `B` ' ye bağlıdır ve `B` ' ye örtülü olarak @no__t bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="a57a4-151">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

<span data-ttu-id="a57a4-152">Sabit listesi üyeleri, sınıfların içindeki alanlara tam olarak benzer şekilde adlandırılır ve kapsamlandırılır.</span><span class="sxs-lookup"><span data-stu-id="a57a4-152">Enum members are named and scoped in a manner exactly analogous to fields within classes.</span></span> <span data-ttu-id="a57a4-153">Bir sabit listesi üyesinin kapsamı, kapsayan enum türünün gövdesidir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-153">The scope of an enum member is the body of its containing enum type.</span></span> <span data-ttu-id="a57a4-154">Bu kapsam içinde, sabit listesi üyeleri kendi basit adlarıyla adlandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-154">Within that scope, enum members can be referred to by their simple name.</span></span> <span data-ttu-id="a57a4-155">Diğer tüm koddan, bir sabit listesi üyesinin adı, sabit listesi türünün adı ile nitelenmelidir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-155">From all other code, the name of an enum member must be qualified with the name of its enum type.</span></span> <span data-ttu-id="a57a4-156">Sabit listesi üyelerinin tanımlanmış bir erişilebilirliği yoktur--bir numaralandırma üyesine, içerdiği numaralandırma türü erişilebilir ise erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-156">Enum members do not have any declared accessibility -- an enum member is accessible if its containing enum type is accessible.</span></span>

## <a name="the-systemenum-type"></a><span data-ttu-id="a57a4-157">System. Enum türü</span><span class="sxs-lookup"><span data-stu-id="a57a4-157">The System.Enum type</span></span>

<span data-ttu-id="a57a4-158">@No__t-0 türü, tüm numaralandırma türlerinin soyut temel sınıfıdır (Bu, sabit listesi türünün temel alınan türünden farklıdır) ve `System.Enum` ' den devralınan Üyeler herhangi bir numaralandırma türünde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-158">The type `System.Enum` is the abstract base class of all enum types (this is distinct and different from the underlying type of the enum type), and the members inherited from `System.Enum` are available in any enum type.</span></span> <span data-ttu-id="a57a4-159">Herhangi bir numaralandırma türünden `System.Enum` ' e ([kutulama dönüşümleri](types.md#boxing-conversions)), `System.Enum` ' ten herhangi bir numaralandırma türüne ([kutudan](types.md#unboxing-conversions)çıkarma dönüştürmesi) var.</span><span class="sxs-lookup"><span data-stu-id="a57a4-159">A boxing conversion ([Boxing conversions](types.md#boxing-conversions)) exists from any enum type to `System.Enum`, and an unboxing conversion ([Unboxing conversions](types.md#unboxing-conversions)) exists from `System.Enum` to any enum type.</span></span>

<span data-ttu-id="a57a4-160">@No__t-0 ' ın kendisinin bir *enum_type*olmadığı unutulmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="a57a4-160">Note that `System.Enum` is not itself an *enum_type*.</span></span> <span data-ttu-id="a57a4-161">Bunun yerine, tüm *enum_type*s 'nin türetildiği bir *class_type* .</span><span class="sxs-lookup"><span data-stu-id="a57a4-161">Rather, it is a *class_type* from which all *enum_type*s are derived.</span></span> <span data-ttu-id="a57a4-162">@No__t-0 türü `System.ValueType` ([System. ValueType türü](types.md#the-systemvaluetype-type)) türünden devralır, bu, sırasıyla `object` türünden devralır.</span><span class="sxs-lookup"><span data-stu-id="a57a4-162">The type `System.Enum` inherits from the type `System.ValueType` ([The System.ValueType type](types.md#the-systemvaluetype-type)), which, in turn, inherits from type `object`.</span></span> <span data-ttu-id="a57a4-163">Çalışma zamanında, `System.Enum` türünde bir değer `null` olabilir ya da herhangi bir numaralandırma türündeki paketlenmiş bir değere başvuru olabilir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-163">At run-time, a value of type `System.Enum` can be `null` or a reference to a boxed value of any enum type.</span></span>

## <a name="enum-values-and-operations"></a><span data-ttu-id="a57a4-164">Sabit listesi değerleri ve işlemleri</span><span class="sxs-lookup"><span data-stu-id="a57a4-164">Enum values and operations</span></span>

<span data-ttu-id="a57a4-165">Her enum türü farklı bir tür tanımlar; bir numaralandırma türü ile integral türü arasında veya iki sabit listesi türü arasında dönüştürme yapmak için açık bir numaralandırma dönüştürmesi ([açık numaralandırma dönüştürmeleri](conversions.md#explicit-enumeration-conversions)) gereklidir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-165">Each enum type defines a distinct type; an explicit enumeration conversion ([Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)) is required to convert between an enum type and an integral type, or between two enum types.</span></span> <span data-ttu-id="a57a4-166">Bir numaralandırma türünün üzerinde sürebelirlenen değerler kümesi, sabit listesi üyeleri tarafından sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-166">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="a57a4-167">Özellikle, bir sabit listesinin temel alınan türünün herhangi bir değeri, sabit listesi türüne dönüşebilir ve bu sabit listesi türünün ayrı bir geçerli değeridir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-167">In particular, any value of the underlying type of an enum can be cast to the enum type, and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="a57a4-168">Sabit listesi üyeleri, kendi kapsayan enum türünde (diğer sabit listesi üye başlatıcıları dışında: [enum üyelerine](enums.md#enum-members)bakın) türü vardır.</span><span class="sxs-lookup"><span data-stu-id="a57a4-168">Enum members have the type of their containing enum type (except within other enum member initializers: see [Enum members](enums.md#enum-members)).</span></span> <span data-ttu-id="a57a4-169">@No__t-1 ilişkili değeri ile `E` numaralandırma türünde belirtilen bir sabit listesi üyesinin değeri `(E)v` ' dir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-169">The value of an enum member declared in enum type `E` with associated value `v` is `(E)v`.</span></span>

<span data-ttu-id="a57a4-170">Aşağıdaki işleçler, enum türlerinin değerleri üzerinde kullanılabilir: `==`, `!=`, `<`, `>`, `<=`, `>=` @ no__t-6 ([numaralandırma karşılaştırma işleçleri](expressions.md#enumeration-comparison-operators)), ikili `+` @ no__t-9 ([toplama işleci](expressions.md#addition-operator)), ikili 1 @ no__ t-12 ([çıkarma işleci](expressions.md#subtraction-operator)), 4, 5, 6 @ no__t-17 ([numaralandırma mantıksal işleçler](expressions.md#enumeration-logical-operators)), 9 @ No__t-20 ([bit düzeyinde tamamlama işleci](expressions.md#bitwise-complement-operator)), 2 ve 3 @ no__t-24 ([Sonek artışı ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators) ve [önek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="a57a4-170">The following operators can be used on values of enum types: `==`, `!=`, `<`, `>`, `<=`, `>=` ([Enumeration comparison operators](expressions.md#enumeration-comparison-operators)), binary `+` ([Addition operator](expressions.md#addition-operator)), binary `-` ([Subtraction operator](expressions.md#subtraction-operator)), `^`, `&`, `|` ([Enumeration logical operators](expressions.md#enumeration-logical-operators)), `~` ([Bitwise complement operator](expressions.md#bitwise-complement-operator)), `++` and `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="a57a4-171">Her numaralandırma türü otomatik olarak `System.Enum` sınıfından türetilir (Bu, sırasıyla `System.ValueType` ve `object` ' den türetilir).</span><span class="sxs-lookup"><span data-stu-id="a57a4-171">Every enum type automatically derives from the class `System.Enum` (which, in turn, derives from `System.ValueType` and `object`).</span></span> <span data-ttu-id="a57a4-172">Bu nedenle, bu sınıfın devralınmış yöntemleri ve özellikleri, bir sabit listesi türünün değerleri üzerinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a57a4-172">Thus, inherited methods and properties of this class can be used on values of an enum type.</span></span>
