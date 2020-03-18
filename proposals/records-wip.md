---
ms.openlocfilehash: 25756c1811d5e6dc97512ce70f99ab7fefa91c4a
ms.sourcegitcommit: 2a6dffb60718065ece95df75e1cc7eb509e48a8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/01/2020
ms.locfileid: "79485240"
---
# <a name="records-work-in-progress"></a><span data-ttu-id="37c6b-101">Süren Iş kayıtları</span><span class="sxs-lookup"><span data-stu-id="37c6b-101">Records Work-in-Progress</span></span>

<span data-ttu-id="37c6b-102">Diğer kayıt tekliflerinin aksine, bu bir teklif değildir, ancak kayıtlar özelliği için konsensus tasarım kararlarını kaydetmek üzere tasarlanan bir süren iş devam ediyor.</span><span class="sxs-lookup"><span data-stu-id="37c6b-102">Unlike the other records proposals, this is not a proposal in itself, but a work-in-progress designed to record consensus design decisions for the records feature.</span></span> <span data-ttu-id="37c6b-103">Teknik ayrıntılar, soruları çözümlemek için gereken şekilde eklenecektir.</span><span class="sxs-lookup"><span data-stu-id="37c6b-103">Specification detail will be added as necessary to resolve questions.</span></span>

<span data-ttu-id="37c6b-104">Bir kaydın sözdizimi aşağıdaki şekilde eklenmek üzere önerilir:</span><span class="sxs-lookup"><span data-stu-id="37c6b-104">The syntax for a record is proposed to be added as follows:</span></span>

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

<span data-ttu-id="37c6b-105">Terminal dışı `attributes`, `data`yeni bir bağlamsal özniteliğe da izin verir.</span><span class="sxs-lookup"><span data-stu-id="37c6b-105">The `attributes` non-terminal will also permit a new contextual attribute, `data`.</span></span>

<span data-ttu-id="37c6b-106">Bir parametre listesi veya `data` değiştiricisi ile belirtilen bir sınıf (struct), biri kayıt türü olan bir kayıt sınıfı (kayıt yapısı) olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="37c6b-106">A class (struct) declared with a parameter list or `data` modifier is called a record class (record struct), either of which is a record type.</span></span>

<span data-ttu-id="37c6b-107">Yalnızca bir parametre listesi ve `data` değiştiricisi olmadan bir kayıt türü bildirmek hatadır.</span><span class="sxs-lookup"><span data-stu-id="37c6b-107">It is an error to declare a record type without both a parameter list and the `data` modifier.</span></span>

## <a name="members-of-a-record-type"></a><span data-ttu-id="37c6b-108">Bir kayıt türünün üyeleri</span><span class="sxs-lookup"><span data-stu-id="37c6b-108">Members of a record type</span></span>

<span data-ttu-id="37c6b-109">Sınıf gövdesinde belirtilen üyelere ek olarak, bir kayıt türü aşağıdaki ek üyelere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="37c6b-109">In addition to the members declared in the class-body, a record type has the following additional members:</span></span>

### <a name="primary-constructor"></a><span data-ttu-id="37c6b-110">Birincil Oluşturucu</span><span class="sxs-lookup"><span data-stu-id="37c6b-110">Primary Constructor</span></span>

<span data-ttu-id="37c6b-111">Bir kayıt türünün, imzası tür bildiriminin değer parametrelerine karşılık gelen bir ortak Oluşturucusu vardır.</span><span class="sxs-lookup"><span data-stu-id="37c6b-111">A record type has a public constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="37c6b-112">Bu, türü için birincil Oluşturucu olarak adlandırılır ve örtük olarak belirtilen varsayılan oluşturucunun gizlenmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="37c6b-112">This is called the primary constructor for the type, and causes the implicitly declared default constructor to be suppressed.</span></span> <span data-ttu-id="37c6b-113">Bir birincil oluşturucuya ve sınıfta zaten aynı imzaya sahip bir oluşturucuya sahip olmak bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="37c6b-113">It is an error to have a primary constructor and a constructor with the same signature already present in the class.</span></span>
<span data-ttu-id="37c6b-114">Çalışma zamanında birincil Oluşturucu</span><span class="sxs-lookup"><span data-stu-id="37c6b-114">At runtime the primary constructor</span></span> 

1. <span data-ttu-id="37c6b-115">sınıf gövdesinde görünen örnek alanı başlatıcıları 'nı yürütür; ve sonra bağımsız değişken olmadan temel sınıf oluşturucusunu çağırır.</span><span class="sxs-lookup"><span data-stu-id="37c6b-115">executes the instance field initializers appearing in the class-body; and then  invokes the base class constructor with no arguments.</span></span>

1. <span data-ttu-id="37c6b-116">değer parametrelerine karşılık gelen özellikler için derleyici tarafından oluşturulan yedekleme alanlarını başlatır (Bu özellikler derleyici tarafından sağlanmışsa), bkz. [sentezlenmiş Özellikler](#Synthesized Properties))</span><span class="sxs-lookup"><span data-stu-id="37c6b-116">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; see [Synthesized properties](#Synthesized Properties))</span></span>


<span data-ttu-id="37c6b-117">[] TODO: temel çağrı sözdizimi ve aşırı yükleme çözümlemesi aracılığıyla temel Oluşturucu seçme hakkında belirtim ekleyin</span><span class="sxs-lookup"><span data-stu-id="37c6b-117">[ ] TODO: add base call syntax and specification about choosing base constructor through overload resolution</span></span>

### <a name="properties"></a><span data-ttu-id="37c6b-118">Özellikler</span><span class="sxs-lookup"><span data-stu-id="37c6b-118">Properties</span></span>

<span data-ttu-id="37c6b-119">Bir kayıt türü bildiriminin her bir kayıt parametresi için, Name ve Type değeri Parameter bildiriminden alınan karşılık gelen bir ortak özellik üyesi vardır.</span><span class="sxs-lookup"><span data-stu-id="37c6b-119">For each record parameter of a record type declaration there is a corresponding public property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="37c6b-120">Bir get erişimcisine sahip ve bu ada ve türe sahip bir somut (soyut olmayan) özelliği yoksa, derleyici tarafından aşağıdaki gibi üretilir:</span><span class="sxs-lookup"><span data-stu-id="37c6b-120">If no concrete (i.e. non-abstract) property with a get accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

<span data-ttu-id="37c6b-121">Bir kayıt yapısı veya kayıt sınıfı için:</span><span class="sxs-lookup"><span data-stu-id="37c6b-121">For a record struct or a record class:</span></span>

* <span data-ttu-id="37c6b-122">Ortak bir salt-al otomatik özelliği oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="37c6b-122">A public get-only auto-property is created.</span></span> <span data-ttu-id="37c6b-123">Değeri, oluşturma sırasında karşılık gelen birincil Oluşturucu parametresinin değeri ile başlatılır.</span><span class="sxs-lookup"><span data-stu-id="37c6b-123">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span> <span data-ttu-id="37c6b-124">Her "eşleşen" devralınan soyut özelliğin get erişimcisi geçersiz kılındı.</span><span class="sxs-lookup"><span data-stu-id="37c6b-124">Each "matching" inherited abstract property's get accessor is overridden.</span></span>

### <a name="equality-members"></a><span data-ttu-id="37c6b-125">Eşitlik üyeleri</span><span class="sxs-lookup"><span data-stu-id="37c6b-125">Equality members</span></span>

<span data-ttu-id="37c6b-126">Kayıt türleri aşağıdaki yöntemler için birleştirilmiş uygulamalar üretir:</span><span class="sxs-lookup"><span data-stu-id="37c6b-126">Record types produce synthesized implementations for the following methods:</span></span>

* <span data-ttu-id="37c6b-127">korumalı veya Kullanıcı tarafından sağlanmadığı müddetçe `object.GetHashCode()` geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="37c6b-127">`object.GetHashCode()` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="37c6b-128">korumalı veya Kullanıcı tarafından sağlanmadığı müddetçe `object.Equals(object)` geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="37c6b-128">`object.Equals(object)` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="37c6b-129">`T Equals(T)` yöntemi, `T` geçerli türdür</span><span class="sxs-lookup"><span data-stu-id="37c6b-129">`T Equals(T)` method, where `T` is the current type</span></span>

<span data-ttu-id="37c6b-130">`T Equals(T)`, her birincil Oluşturucu parametresi ile aynı ada sahip özelliği diğer türün karşılık gelen özelliği ile karşılaştıran değer eşitlik gerçekleştirmek için belirtilir.</span><span class="sxs-lookup"><span data-stu-id="37c6b-130">`T Equals(T)` is specified to perform value equality, comparing the property with same name as each primary constructor parameter to the corresponding property of the other type.</span></span>
<span data-ttu-id="37c6b-131">`object.Equals` eşdeğerini gerçekleştirir</span><span class="sxs-lookup"><span data-stu-id="37c6b-131">`object.Equals` performs the equivalent of</span></span>

```C#
override Equals(object o) => Equals(o as T);
```
