---
ms.openlocfilehash: 258ae6865c5b2c3103a0cdf7e1e5a2cdee11e740
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281963"
---
# <a name="records-work-in-progress"></a><span data-ttu-id="e710f-101">Süren Iş kayıtları</span><span class="sxs-lookup"><span data-stu-id="e710f-101">Records Work-in-Progress</span></span>

<span data-ttu-id="e710f-102">Diğer kayıt tekliflerinin aksine, bu bir teklif değildir, ancak kayıtlar özelliği için konsensus tasarım kararlarını kaydetmek üzere tasarlanan bir süren iş devam ediyor.</span><span class="sxs-lookup"><span data-stu-id="e710f-102">Unlike the other records proposals, this is not a proposal in itself, but a work-in-progress designed to record consensus design decisions for the records feature.</span></span> <span data-ttu-id="e710f-103">Teknik ayrıntılar, soruları çözümlemek için gereken şekilde eklenecektir.</span><span class="sxs-lookup"><span data-stu-id="e710f-103">Specification detail will be added as necessary to resolve questions.</span></span>

<span data-ttu-id="e710f-104">Bir kaydın sözdizimi aşağıdaki şekilde eklenmek üzere önerilir:</span><span class="sxs-lookup"><span data-stu-id="e710f-104">The syntax for a record is proposed to be added as follows:</span></span>

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      parameter_list? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      parameter_list? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

<span data-ttu-id="e710f-105">Terminal dışı `attributes`, `data`yeni bir bağlamsal özniteliğe da izin verir.</span><span class="sxs-lookup"><span data-stu-id="e710f-105">The `attributes` non-terminal will also permit a new contextual attribute, `data`.</span></span>

<span data-ttu-id="e710f-106">Bir parametre listesi veya `data` değiştiricisi ile belirtilen bir sınıf (struct), biri kayıt türü olan bir kayıt sınıfı (kayıt yapısı) olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="e710f-106">A class (struct) declared with a parameter list or `data` modifier is called a record class (record struct), either of which is a record type.</span></span>

<span data-ttu-id="e710f-107">Yalnızca bir parametre listesi ve `data` değiştiricisi olmadan bir kayıt türü bildirmek hatadır.</span><span class="sxs-lookup"><span data-stu-id="e710f-107">It is an error to declare a record type without both a parameter list and the `data` modifier.</span></span>

## <a name="members-of-a-record-type"></a><span data-ttu-id="e710f-108">Bir kayıt türünün üyeleri</span><span class="sxs-lookup"><span data-stu-id="e710f-108">Members of a record type</span></span>

<span data-ttu-id="e710f-109">Sınıf gövdesinde belirtilen üyelere ek olarak, bir kayıt türü aşağıdaki ek üyelere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="e710f-109">In addition to the members declared in the class-body, a record type has the following additional members:</span></span>

### <a name="primary-constructor"></a><span data-ttu-id="e710f-110">Birincil Oluşturucu</span><span class="sxs-lookup"><span data-stu-id="e710f-110">Primary Constructor</span></span>

<span data-ttu-id="e710f-111">Bir kayıt türünün, imzası tür bildiriminin değer parametrelerine karşılık gelen bir ortak Oluşturucusu vardır.</span><span class="sxs-lookup"><span data-stu-id="e710f-111">A record type has a public constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="e710f-112">Bu, türü için birincil Oluşturucu olarak adlandırılır ve örtük olarak belirtilen varsayılan oluşturucunun gizlenmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="e710f-112">This is called the primary constructor for the type, and causes the implicitly declared default constructor to be suppressed.</span></span> <span data-ttu-id="e710f-113">Bir birincil oluşturucuya ve sınıfta zaten aynı imzaya sahip bir oluşturucuya sahip olmak bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="e710f-113">It is an error to have a primary constructor and a constructor with the same signature already present in the class.</span></span>
<span data-ttu-id="e710f-114">Çalışma zamanında birincil Oluşturucu</span><span class="sxs-lookup"><span data-stu-id="e710f-114">At runtime the primary constructor</span></span> 

1. <span data-ttu-id="e710f-115">sınıf gövdesinde görünen örnek alanı başlatıcıları 'nı yürütür; ve sonra bağımsız değişken olmadan temel sınıf oluşturucusunu çağırır.</span><span class="sxs-lookup"><span data-stu-id="e710f-115">executes the instance field initializers appearing in the class-body; and then  invokes the base class constructor with no arguments.</span></span>

1. <span data-ttu-id="e710f-116">değer parametrelerine karşılık gelen özellikler için derleyici tarafından oluşturulan yedekleme alanlarını başlatır (Bu özellikler derleyici tarafından sağlanmışsa), bkz. [sentezlenmiş Özellikler](#Synthesized Properties))</span><span class="sxs-lookup"><span data-stu-id="e710f-116">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; see [Synthesized properties](#Synthesized Properties))</span></span>


<span data-ttu-id="e710f-117">[] TODO: temel çağrı sözdizimi ve aşırı yükleme çözümlemesi aracılığıyla temel Oluşturucu seçme hakkında belirtim ekleyin</span><span class="sxs-lookup"><span data-stu-id="e710f-117">[ ] TODO: add base call syntax and specification about choosing base constructor through overload resolution</span></span>

### <a name="properties"></a><span data-ttu-id="e710f-118">Özellikler</span><span class="sxs-lookup"><span data-stu-id="e710f-118">Properties</span></span>

<span data-ttu-id="e710f-119">Bir kayıt türü bildiriminin her bir kayıt parametresi için, Name ve Type değeri Parameter bildiriminden alınan karşılık gelen bir ortak özellik üyesi vardır.</span><span class="sxs-lookup"><span data-stu-id="e710f-119">For each record parameter of a record type declaration there is a corresponding public property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="e710f-120">Bir get erişimcisine sahip ve bu ada ve türe sahip bir somut (soyut olmayan) özelliği yoksa, derleyici tarafından aşağıdaki gibi üretilir:</span><span class="sxs-lookup"><span data-stu-id="e710f-120">If no concrete (i.e. non-abstract) property with a get accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

<span data-ttu-id="e710f-121">Bir kayıt yapısı veya kayıt sınıfı için:</span><span class="sxs-lookup"><span data-stu-id="e710f-121">For a record struct or a record class:</span></span>

* <span data-ttu-id="e710f-122">Ortak bir salt-al otomatik özelliği oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e710f-122">A public get-only auto-property is created.</span></span> <span data-ttu-id="e710f-123">Değeri, oluşturma sırasında karşılık gelen birincil Oluşturucu parametresinin değeri ile başlatılır.</span><span class="sxs-lookup"><span data-stu-id="e710f-123">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span> <span data-ttu-id="e710f-124">Her "eşleşen" devralınan soyut özelliğin get erişimcisi geçersiz kılındı.</span><span class="sxs-lookup"><span data-stu-id="e710f-124">Each "matching" inherited abstract property's get accessor is overridden.</span></span>

### <a name="equality-members"></a><span data-ttu-id="e710f-125">Eşitlik üyeleri</span><span class="sxs-lookup"><span data-stu-id="e710f-125">Equality members</span></span>

<span data-ttu-id="e710f-126">Kayıt türleri aşağıdaki yöntemler için birleştirilmiş uygulamalar üretir:</span><span class="sxs-lookup"><span data-stu-id="e710f-126">Record types produce synthesized implementations for the following methods:</span></span>

* <span data-ttu-id="e710f-127">korumalı veya Kullanıcı tarafından sağlanmadığı müddetçe `object.GetHashCode()` geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="e710f-127">`object.GetHashCode()` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="e710f-128">korumalı veya Kullanıcı tarafından sağlanmadığı müddetçe `object.Equals(object)` geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="e710f-128">`object.Equals(object)` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="e710f-129">`T Equals(T)` yöntemi, `T` geçerli türdür</span><span class="sxs-lookup"><span data-stu-id="e710f-129">`T Equals(T)` method, where `T` is the current type</span></span>

<span data-ttu-id="e710f-130">`T Equals(T)`, her birincil Oluşturucu parametresi ile aynı ada sahip özelliği diğer türün karşılık gelen özelliği ile karşılaştıran değer eşitlik gerçekleştirmek için belirtilir.</span><span class="sxs-lookup"><span data-stu-id="e710f-130">`T Equals(T)` is specified to perform value equality, comparing the property with same name as each primary constructor parameter to the corresponding property of the other type.</span></span>
<span data-ttu-id="e710f-131">`object.Equals` eşdeğerini gerçekleştirir</span><span class="sxs-lookup"><span data-stu-id="e710f-131">`object.Equals` performs the equivalent of</span></span>

```C#
override Equals(object o) => Equals(o as T);
```

## <a name="with-expression"></a><span data-ttu-id="e710f-132">`with` ifadesi</span><span class="sxs-lookup"><span data-stu-id="e710f-132">`with` expression</span></span>

<span data-ttu-id="e710f-133">`with` ifadesi aşağıdaki sözdizimini kullanan yeni bir ifadedir.</span><span class="sxs-lookup"><span data-stu-id="e710f-133">A `with` expression is a new expression using the following syntax.</span></span>

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

<span data-ttu-id="e710f-134">`with` bir ifade, `anonymous_object_initializer`listelenen özelliklerde yapılan değişikliklerle alıcı ifadesinin bir kopyasını oluşturmak için tasarlanan "bozucu olmayan" bir yıkıcı "sağlar.</span><span class="sxs-lookup"><span data-stu-id="e710f-134">A `with` expression allows for "non-destructive mutation", designed to produce a copy of the receiver expression with modifications to properties listed in the `anonymous_object_initializer`.</span></span>

<span data-ttu-id="e710f-135">Geçerli bir `with` ifadesi void olmayan bir türe sahip bir alıcıya sahip.</span><span class="sxs-lookup"><span data-stu-id="e710f-135">A valid `with` expression has a receiver with a non-void type.</span></span> <span data-ttu-id="e710f-136">Alıcı türü, uygun parametreler ve dönüş türü ile `With` adlı erişilebilir bir örnek yöntem içermelidir.</span><span class="sxs-lookup"><span data-stu-id="e710f-136">The receiver type must contain an accessible instance method called `With` with the appropriate parameters and return type.</span></span> <span data-ttu-id="e710f-137">Birden çok geçersiz kılma olmayan `With` yöntemi varsa bu bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="e710f-137">It is an error if there are multiple non-override `With` methods.</span></span> <span data-ttu-id="e710f-138">Birden çok `With` geçersiz kılma varsa, hedef yöntemi olan geçersiz kılma olmayan bir `With` yöntemi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e710f-138">If there are multiple `With` overrides, there must be a non-override `With` method, which is the target method.</span></span> <span data-ttu-id="e710f-139">Aksi takdirde, tam olarak bir `With` yöntemi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e710f-139">Otherwise, there must be exactly one `With` method.</span></span>

<span data-ttu-id="e710f-140">`with` ifadesinin sağ tarafında, atamanın sol tarafındaki alıcının bir alanı veya özelliği ile bir dizi atamalarla birlikte bir `anonymous_object_initializer` ve sağ tarafta, sol taraftaki türe örtük olarak dönüştürülebilir bir rastgele ifade olan bir.</span><span class="sxs-lookup"><span data-stu-id="e710f-140">On the right hand side of the `with` expression is an `anonymous_object_initializer` with a sequence of assignments with a field or property of the receiver on the left-hand side of the assignment, and an arbitrary expression on the right-hand side which is implicitly convertible to the type of the left-hand side.</span></span>

<span data-ttu-id="e710f-141">Hedef `With` yöntemi verildiğinde, dönüş türü alıcı ifadesi türünün türü veya temel bir tür olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e710f-141">Given a target `With` method, the return type must be the type of the receiver expression type, or a base type thereof.</span></span> <span data-ttu-id="e710f-142">`With` yönteminin her parametresi için, aynı ada ve aynı türe sahip alıcı türünde bir erişilebilir karşılık gelen örnek alanı veya okunabilir özellik olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e710f-142">For each parameter of the `With` method, there must be an accessible corresponding instance field or readable property on the receiver type with the same name and the same type.</span></span> <span data-ttu-id="e710f-143">WITH ifadesinin sağ tarafındaki her bir özellik veya alan, `With` yönteminde aynı ada sahip bir parametreye de karşılık gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="e710f-143">Each property or field in the right-hand side of the With expression must also correspond to a parameter of the same name in the `With` method.</span></span>

<span data-ttu-id="e710f-144">Geçerli bir `With` yöntemi verildiğinde, bir `with` ifadesinin değerlendirmesi, `With` yöntemini, sol taraftaki özelliği ile aynı ada sahip parametre için değiştirilen `anonymous_object_initializer` ifadelerle çağırarak eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="e710f-144">Given a valid `With` method, the evaluation of a `with` expression is equivalent to calling the `With` method with the expressions in the `anonymous_object_initializer` substituted for the parameter of the same name as the property on the left hand side.</span></span> <span data-ttu-id="e710f-145">`anonymous_object_initializer`belirli bir parametre için eşleşen bir özellik yoksa, bağımsız değişken alıcı üzerinde aynı ada sahip alanın veya özelliğin değerlendirmesinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="e710f-145">If there is no matching property for a given parameter in the `anonymous_object_initializer`, the argument is the evaluation of the field or property of the same name on the receiver.</span></span>

<span data-ttu-id="e710f-146">Yan etkileri değerlendirmesi sırası, her bir ifade tam olarak bir kez hesaplandıktan sonra aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="e710f-146">The order of evaluation of side effects is as follows, with each expression evaluated exactly once:</span></span>

1. <span data-ttu-id="e710f-147">Alıcı ifadesi</span><span class="sxs-lookup"><span data-stu-id="e710f-147">Receiver expression</span></span>

2. <span data-ttu-id="e710f-148">`anonymous_object_initializer`, sözcük temelli sırada olan ifadeler</span><span class="sxs-lookup"><span data-stu-id="e710f-148">Expressions in the `anonymous_object_initializer`, in lexical order</span></span>

3. <span data-ttu-id="e710f-149">`With` yöntemi parametrelerinin tanımı sırasına göre `With` yöntemi parametreleriyle eşleşen özelliklerin değerlendirilmesi.</span><span class="sxs-lookup"><span data-stu-id="e710f-149">The evaluation of any properties matching the `With` method parameters, in order of definition of the `With` method parameters.</span></span>