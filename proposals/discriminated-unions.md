---
ms.openlocfilehash: 2e4a3a32def6900797c151264c984378b09b4988
ms.sourcegitcommit: 5983461e05be62f39c77383cb7857539518cb04f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2019
ms.locfileid: "79485128"
---

# <a name="discriminated-unions--enum-class"></a><span data-ttu-id="cb7d5-101">Ayırt edici birleşimler/`enum class`</span><span class="sxs-lookup"><span data-stu-id="cb7d5-101">Discriminated unions / `enum class`</span></span>

<span data-ttu-id="cb7d5-102">`enum class`es, bazen ayrılmış birleşimler olarak da adlandırılan, her olası örneğin türün listelendiği ve her örnek çakışmayan bir tür bildirimidir.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-102">`enum class`es are a new kind of type declaration, sometimes referred to as discriminated unions, where each every possible instance the type is listed, and each instance is non-overlapping.</span></span>

<span data-ttu-id="cb7d5-103">Aşağıdaki sözdizimi kullanılarak bir `enum class` tanımlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="cb7d5-103">An `enum class` is defined using the following syntax:</span></span>

```antlr
enum_class
    : 'partial'? 'enum class' identifier type_parameter_list? type_parameter_constraints_clause* 
      '{' enum_class_body '}'
    ;

enum_class_body
    : enum_class_cases?
    | enum_class_cases ','
    ;

enum_class_cases
    : enum_class_case
    | enum_class_case ',' enum_class_cases
    ;

enum_class_case
    : enum_class
    | class_declaration
    | identifier type_parameter_list? '(' formal_parameter_list? ')'
    | identifier
    ;

```

<span data-ttu-id="cb7d5-104">Örnek sözdizimi:</span><span class="sxs-lookup"><span data-stu-id="cb7d5-104">Sample syntax:</span></span>

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius),
}
```

## <a name="semantics"></a><span data-ttu-id="cb7d5-105">İçeriyor</span><span class="sxs-lookup"><span data-stu-id="cb7d5-105">Semantics</span></span>

<span data-ttu-id="cb7d5-106">`enum class` tanımı, `enum class` bildirimiyle aynı ada sahip bir soyut sınıf olan kök türünü ve her birinin kök türünün bir alt türü olan bir türü olan bir üye kümesini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-106">An `enum class` definition defines a root type, which is an abstract class of the same name as the `enum class` declaration, and a set of members, each of which has a type which is a subtype of the root type.</span></span> <span data-ttu-id="cb7d5-107">Birden çok `partial enum class` tanımı varsa, tüm Üyeler enum sınıfı tanımının üyeleri olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-107">If there are multiple `partial enum class` definitions, all members will be considered members of the enum class definition.</span></span> <span data-ttu-id="cb7d5-108">Kullanıcı tanımlı soyut sınıf tanımından farklı olarak, `enum class` kök türü varsayılan olarak kısmen olur ve varsayılan *özel* parametre-daha az oluşturucuya sahip olacak şekilde tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-108">Unlike a user-defined abstract class definition, the `enum class` root type is partial by default and defined to have a default *private* parameter-less constructor.</span></span>

<span data-ttu-id="cb7d5-109">Kök türü kısmi bir soyut sınıf olmak üzere tanımlandığından, *kök türünün* kısmi tanımlarının de eklenebilir olması, bu da bir sınıf gövdesinin standart sözdizimi formlarına izin verileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-109">Note that, since the root type is defined to be a partial abstract class, partial definitions of the *root type* may also be added, where standard syntax forms for a class body are allowed.</span></span>
<span data-ttu-id="cb7d5-110">Ancak, hiçbir tür, hiçbir bildirimde doğrudan kök türünden devralınabilir ve bu, `enum class` üyeleri olarak belirtilenlerden farklı olabilir.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-110">However, no types may directly inherit from the root type in any declaration, aside from those specified as `enum class` members.</span></span> <span data-ttu-id="cb7d5-111">Ayrıca, kök türü için Kullanıcı tanımlı oluşturuculara izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-111">In addition, no user-defined constructors are permitted for the root type.</span></span>

<span data-ttu-id="cb7d5-112">Dört tür `enum class` üye bildirimi vardır:</span><span class="sxs-lookup"><span data-stu-id="cb7d5-112">There are four kinds of `enum class` member declarations:</span></span>

* <span data-ttu-id="cb7d5-113">Basit sınıf üyeleri</span><span class="sxs-lookup"><span data-stu-id="cb7d5-113">simple class members</span></span>

* <span data-ttu-id="cb7d5-114">karmaşık sınıf üyeleri</span><span class="sxs-lookup"><span data-stu-id="cb7d5-114">complex class members</span></span>

* <span data-ttu-id="cb7d5-115">`enum class` Üyeler</span><span class="sxs-lookup"><span data-stu-id="cb7d5-115">`enum class` members</span></span>

* <span data-ttu-id="cb7d5-116">değer üyeleri.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-116">value members.</span></span>

### <a name="simple-class-members"></a><span data-ttu-id="cb7d5-117">Basit sınıf üyeleri</span><span class="sxs-lookup"><span data-stu-id="cb7d5-117">Simple class members</span></span>

<span data-ttu-id="cb7d5-118">Basit bir sınıf üyesi bildirimi, aynı ada sahip yeni bir iç içe "kayıt" sınıfını (Bu belgede kasıtlı olarak ayrıldı) tanımlar.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-118">A simple class member declaration defines a new nested "record" class (intentionally left undefined in this document) with the same name.</span></span> <span data-ttu-id="cb7d5-119">İç içe yerleştirilmiş sınıf kök türünden devralır.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-119">The nested class inherits from the root type.</span></span>

<span data-ttu-id="cb7d5-120">Yukarıdaki örnek kod verildiğinde,</span><span class="sxs-lookup"><span data-stu-id="cb7d5-120">Given the sample code above,</span></span>

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius)
}
```

<span data-ttu-id="cb7d5-121">`enum class` bildiriminde semantikler aşağıdaki bildirime eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="cb7d5-121">the `enum class` declaration has semantics equivalent to the following declaration</span></span>

```C#
partial abstract class Shape
{
    public data class Rectangle(float Width, float Length) : Shape,
    public data class Circle(float Radius) : Shape
}
```

### <a name="complex-class-members"></a><span data-ttu-id="cb7d5-122">Karmaşık sınıf üyeleri</span><span class="sxs-lookup"><span data-stu-id="cb7d5-122">Complex class members</span></span>

<span data-ttu-id="cb7d5-123">Tüm sınıf bildirimini bir `enum class` bildiriminin altına de iç içe yerleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-123">You can also nest an entire class declaration below an `enum class` declaration.</span></span> <span data-ttu-id="cb7d5-124">Kök türünde iç içe geçmiş bir sınıf olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-124">It will be treated as a nested class of the root type.</span></span> <span data-ttu-id="cb7d5-125">Söz dizimi tüm sınıf bildirimine izin verir, ancak karmaşık sınıf üyesinin doğrudan içeren `enum class` bildiriminden devralması için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-125">The syntax allows any class declaration, but it is required for the complex class member to inherit from the direct containing `enum class` declaration.</span></span> 

### <a name="enum-class-members"></a><span data-ttu-id="cb7d5-126">`enum class` Üyeler</span><span class="sxs-lookup"><span data-stu-id="cb7d5-126">`enum class` members</span></span>

<span data-ttu-id="cb7d5-127">`enum classes` birbirlerine iç içe eklenebilir, ör.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-127">`enum classes` can be nested under each other, e.g.</span></span>

```C#
enum class Expr
{
    enum class Binary
    {
        Addition(Expr left, Expr right),
        Multiplication(Expr left, Expr right)
    }
}
```

<span data-ttu-id="cb7d5-128">Bu, üst düzey bir `enum class`ile neredeyse aynıdır, iç içe enum sınıfı iç içe geçmiş bir kök türü tanımlar ve iç içe enum sınıfının altındaki her şey, üst düzey kök türü yerine iç içe geçmiş kök türünün bir alt türüdür.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-128">This is almost identical to the semantics of a top-level `enum class`, except that the nested enum class defines a nested root type, and everything below the nested enum class is a subtype of the nested root type, instead of the top-level root type.</span></span>

```C#
partial abstract class Expr
{
    partial abstract class Binary : Expr
    {
        public data class Addition(Expr left, Expr right) : Binary,
        public data class Multiplication(Expr left, Expr right) : Binary
    }
}
```

### <a name="value-members"></a><span data-ttu-id="cb7d5-129">Değer üyeleri</span><span class="sxs-lookup"><span data-stu-id="cb7d5-129">Value members</span></span>

<span data-ttu-id="cb7d5-130">`enum classes`, değer üyelerini de içerebilir.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-130">`enum classes` can also contain value members.</span></span> <span data-ttu-id="cb7d5-131">Değer üyeleri kök türünde kök türü de döndüren genel salt Al statik özelliklerini tanımlar, örn.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-131">Value members define public get-only static properties on the root type that also return the root type, e.g.</span></span>

```C#
enum class Color
{
    Red,
    Green
}
```

<span data-ttu-id="cb7d5-132">ile eşdeğer özellikler vardır</span><span class="sxs-lookup"><span data-stu-id="cb7d5-132">has properties equivalent to</span></span>

```C#
partial abstract class Color
{
    public static Color Red => ...;
    public static Color Green => ...;
}
```

<span data-ttu-id="cb7d5-133">Tüm semantikler bir uygulama ayrıntısı olarak değerlendirilir, ancak her bir özellik için bir benzersiz örnek döndürüldüğünden ve yinelenen etkinleştirmeleri üzerinde aynı örnek döndürülmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-133">The complete semantics are considered an implementation detail, but it is guaranteed that one unique instance will be returned for each property, and the same instance will be returned on repeated invocations.</span></span>


### <a name="switch-expression-and-patterns"></a><span data-ttu-id="cb7d5-134">Anahtar ifadesi ve desenleri</span><span class="sxs-lookup"><span data-stu-id="cb7d5-134">Switch expression and patterns</span></span>

<span data-ttu-id="cb7d5-135">`enum classes`işlemek için, model eşleştirme ve switch ifadesinin bazı önerilmiş ayarlamaları vardır.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-135">There are some proposed adjustments to pattern matching and the switch expression to handle `enum classes`.</span></span> <span data-ttu-id="cb7d5-136">Anahtar ifadeleri türleri değişken düzeniyle zaten eşleştirebilir, ancak şu anda başvuru türleri için, anahtar ifadesinde anahtar kolları kümesi, bağımsız değişkenin statik türü veya bir alt tür için eşleşme dışında tamamlanmış olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-136">Switch expressions can already match types through the variable pattern, but for currently for reference types, no set of switch arms in the switch expression are considered complete, except for matching against the static type of the argument, or a subtype.</span></span>

<span data-ttu-id="cb7d5-137">Anahtar ifadeleri, bir `enum class` kök türü, anahtar ifadesi için bağımsız değişkenin statik türü ise ve sabit listesinin tüm üyeleriyle eşleşen bir desen kümesi varsa, anahtar geniş olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-137">Switch expressions would be changed such that, if the root type of an `enum class` is the static type of the argument to the switch expression, and there is a set of patterns matching all members of the enum, then the switch will be considered exhaustive.</span></span>

<span data-ttu-id="cb7d5-138">Değer üyeleri sabit olmadığından ve yeni statik türler tanımlamadıklarından, şu anda bu öğeler düzeniyle eşleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-138">Since value members are not constants and do not define new static types, they currently cannot be matched by pattern.</span></span> <span data-ttu-id="cb7d5-139">Bunu mümkün kılmak için, `enum class` değer üyelerine eşleştirmeye izin vermek üzere sabit model sözdizimi kullanan yeni bir model eklenecektir.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-139">To make this possible, a new pattern using the constant pattern syntax will be added to allow match against `enum class` value members.</span></span> <span data-ttu-id="cb7d5-140">Eşleşme, yalnızca model eşleşmesi için bağımsız değişken ve `enum class` Value üyesi tarafından döndürülen değer eşitse, bu denetimi gerçekleştirmek için gerekli olmasa da, başarılı olarak tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-140">The match is defined to succeed if and only if the argument to the pattern match and the value returned by the `enum class` value member would be reference equal, although the implementation is not required to perform this check.</span></span>


## <a name="open-questions"></a><span data-ttu-id="cb7d5-141">Açık sorular</span><span class="sxs-lookup"><span data-stu-id="cb7d5-141">Open questions</span></span>

- <span data-ttu-id="cb7d5-142">[] Ortak tür algoritması `enum class` üye hakkında ne söylüyor?</span><span class="sxs-lookup"><span data-stu-id="cb7d5-142">[ ] What does the common type algorithm say about `enum class` members?</span></span> <span data-ttu-id="cb7d5-143">Bu geçerli bir kod mi?</span><span class="sxs-lookup"><span data-stu-id="cb7d5-143">Is this valid code?</span></span>
    * `var x = b ? new Shape.Rectangle(...) : new Shape.Circle(...)`

- <span data-ttu-id="cb7d5-144">[] Yalnızca değer üyeleri için yeni bir model eklemek ağır bir şekilde görünür.</span><span class="sxs-lookup"><span data-stu-id="cb7d5-144">[ ] Adding a new pattern just for value members seems heavy handed.</span></span> <span data-ttu-id="cb7d5-145">Daha anlamlı bir sürüm oluşturma var mı?</span><span class="sxs-lookup"><span data-stu-id="cb7d5-145">Is there a more general version construction that makes sense?</span></span>
    - <span data-ttu-id="cb7d5-146">[] Değer üyeleri, bu nedenle bir paralel iç içe sınıf oluşturmaya de uygun şekilde eşlenmiyor</span><span class="sxs-lookup"><span data-stu-id="cb7d5-146">[ ] Value members also do not map well to a parallel nested class construction because of this</span></span>

- <span data-ttu-id="cb7d5-147">[] Sabit bir `enum class` statik bir türe sahip bir bağımsız değişkene geçiş yapıyor mu?</span><span class="sxs-lookup"><span data-stu-id="cb7d5-147">[ ] Is switching against an argument with an `enum class` static type guaranteed to be constant-time?</span></span>

- <span data-ttu-id="cb7d5-148">[] Anahtar ifadesinde `enum class`es 'nin tamamlanmış olarak kabul edilmediğinden emin olmak için bir yol olması gerekir mi?</span><span class="sxs-lookup"><span data-stu-id="cb7d5-148">[ ] Should there be a way to make `enum class`es not be considered complete in the switch expression?</span></span> <span data-ttu-id="cb7d5-149">`virtual`önek mi?</span><span class="sxs-lookup"><span data-stu-id="cb7d5-149">Prefix with `virtual`?</span></span>

- <span data-ttu-id="cb7d5-150">[] `enum class`değiştiricilere ne izin verilmelidir?</span><span class="sxs-lookup"><span data-stu-id="cb7d5-150">[ ] What modifiers should be permitted on `enum class`?</span></span>