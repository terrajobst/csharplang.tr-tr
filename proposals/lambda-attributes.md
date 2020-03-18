---
ms.openlocfilehash: 9647fff40a1e45bef917f140612ae4e91abea958
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484575"
---
# <a name="lambda-attributes"></a><span data-ttu-id="b485d-101">Lambda öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="b485d-101">Lambda Attributes</span></span>

* <span data-ttu-id="b485d-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="b485d-102">[x] Proposed</span></span>
* <span data-ttu-id="b485d-103">[] Prototipi</span><span class="sxs-lookup"><span data-stu-id="b485d-103">[ ] Prototype</span></span>
* <span data-ttu-id="b485d-104">[] Uygulama</span><span class="sxs-lookup"><span data-stu-id="b485d-104">[ ] Implementation</span></span>
* <span data-ttu-id="b485d-105">[] Belirtimi</span><span class="sxs-lookup"><span data-stu-id="b485d-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="b485d-106">Özet</span><span class="sxs-lookup"><span data-stu-id="b485d-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="b485d-107">Özniteliklerin Lambdalar (ve anonim yöntemler) ve lambda/anonim yöntem parametrelerine (normal metotlarda olabilecek) uygulanmasına izin verin.</span><span class="sxs-lookup"><span data-stu-id="b485d-107">Allow attributes to be applied to lambdas (and anonymous methods) and to lambda / anonymous method parameters, as they can be on regular methods.</span></span>

## <a name="motivation"></a><span data-ttu-id="b485d-108">Amacı</span><span class="sxs-lookup"><span data-stu-id="b485d-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="b485d-109">İki birincil motive:</span><span class="sxs-lookup"><span data-stu-id="b485d-109">Two primary motivations:</span></span>

1. <span data-ttu-id="b485d-110">Meta verileri derleme zamanında çözümleyiciler için görünür hale sağlamak.</span><span class="sxs-lookup"><span data-stu-id="b485d-110">To provide metadata visible to analyzers at compile-time.</span></span>
2. <span data-ttu-id="b485d-111">Çalışma zamanında yansıma ve araç için görünür hale, meta verileri sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="b485d-111">To provide metadata visible to reflection and tooling at run-time.</span></span>

<span data-ttu-id="b485d-112">Örnek olarak, (1): performans duyarlı kod Için, kapanışlara ve temsilcilerin, durumu kapatan Lambdalar için ayrılmakta olduğu bir çözümleyici olması yararlı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="b485d-112">As an example of (1): For performance-sensitive code, it is helpful to be able to have an analyzer that flags when closures and delegates are being allocated for lambdas that close over state.</span></span>  <span data-ttu-id="b485d-113">Genellikle, bu kodun bir geliştiricisi, herhangi bir durumu yakalamaya engel olmak için bir veya kendi yolunda kalacak, böylece derleyicinin Yöntem için bir statik yöntem ve önbelleklenebilir bir temsilci oluşturabilmesi, ya da geliştirici, en azından bir kapanış nesnesinin ayrılmasına engel olmak için `this`olduğundan emin olmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="b485d-113">Often a developer of such code will go out of his or her way to avoid capturing any state, so that the compiler can generate a static method and a cacheable delegate for the method, or the developer will ensure that the only state being closed over is `this`, allowing the compiler at least to avoid allocating a closure object.</span></span>  <span data-ttu-id="b485d-114">Ancak, nelerin yakalanabileceğini kısıtlamak için dil desteği olmadan, yanlışlıkla daha kolay bir şekilde kapatılabilir.</span><span class="sxs-lookup"><span data-stu-id="b485d-114">But, without language support for limiting what may be captured, it is all too easy to accidentally close over state.</span></span>  <span data-ttu-id="b485d-115">Bir geliştirici, bir geliştiricinin neleri kapatmasına izin verildiğini belirtmek için özniteliklere sahip Lambdalar eklemek için yararlı olacaktır, örneğin:</span><span class="sxs-lookup"><span data-stu-id="b485d-115">It would be valuable if a developer could annotate lambdas with attributes to indicate what state they're allowed to close over, for example:</span></span>

```csharp
[CaptureNone] // can't close over any instance state
[CaptureThis] // can only capture `this` and no other instance state
[CaptureAny] // can close over any instance state
```

<span data-ttu-id="b485d-116">Daha sonra, durum yanlış yakalandıktan sonra bir çözümleyici yazılabilir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="b485d-116">Then an analyzer can be written to flag when state is captured incorrectly, for example:</span></span>

```csharp
var results = collection.Select([CaptureNone](i) => Process(item)); // Analyzer error: [CaptureNone] lambdas captures `this`
...
private U Process(T item) { ... }
```

## <a name="detailed-design"></a><span data-ttu-id="b485d-117">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="b485d-117">Detailed design</span></span>
[design]: #detailed-design

- <span data-ttu-id="b485d-118">Normal yöntemlerle aynı öznitelik söz dizimini kullanarak, öznitelikler bir lambda veya anonim yöntemin başlangıcında uygulanabilir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="b485d-118">Using the same attribute syntax as on normal methods, attributes may be applied at the beginning of a lambda or anonymous method, for example:</span></span>

```csharp
[SomeAttribute(...)] () => { ... }
[SomeAttribute(...)] delegate (int i) { ... }
```

- <span data-ttu-id="b485d-119">Bir özniteliğin lambda yöntemine veya bağımsız değişkenlerden birine uygulanıp uygulanmayacağı konusunda belirsizliğe engel olmak için, öznitelikler yalnızca herhangi bir bağımsız değişken etrafında kullanıldığı zaman kullanılabilir; örneğin:</span><span class="sxs-lookup"><span data-stu-id="b485d-119">To avoid ambiguity as to whether an attribute applies to the lambda method or to one of the arguments, attributes may only be used when parens are used around any arguments, for example:</span></span>

```csharp
[SomeAttribute] i => { ... } // ERROR
[SomeAttribute] (i) => { ... } // Ok
[SomeAttribute] (int i) => { ... } // Ok
```

- <span data-ttu-id="b485d-120">Anonim yöntemlerle, `delegate` anahtar sözcüğünden önce yönteme bir öznitelik uygulamak için Parens gerekmez, örneğin:</span><span class="sxs-lookup"><span data-stu-id="b485d-120">With anonymous methods, parens are not needed in order to apply an attribute to the method before the `delegate` keyword, for example:</span></span>

```csharp
[SomeAttribute] delegate { ... } // Ok
[SomeAttribute] delegate (int i) => { ... } // Ok
```

- <span data-ttu-id="b485d-121">Birden çok öznitelik, standart virgülle ayrılmış sözdizimi veya tam öznitelik sözdizimi aracılığıyla uygulanabilir. Örneğin:</span><span class="sxs-lookup"><span data-stu-id="b485d-121">Multiple attributes may be applied, either via standard comma-delimited syntax or via full-attribute syntax, for example:</span></span>

```csharp
[FirstAttribute, SecondAttribute] (i) => { ... } // Ok
[FirstAttribute] [SecondAttribute] (i) => { .... } // Ok
```

- <span data-ttu-id="b485d-122">Öznitelikler anonim bir yönteme veya lambda parametrelerine uygulanabilir, ancak yalnızca parülar her bağımsız değişken etrafında kullanıldığında, örneğin:</span><span class="sxs-lookup"><span data-stu-id="b485d-122">Attributes may be applied to the parameters to an anonymous method or lambda, but only when parens are used around any arguments, for example:</span></span>

```csharp
[SomeAttribute] i => { ... } // ERROR
([SomeAttribute] i) => { .... } // Ok
([SomeAttribute] int i) => { ... } // Ok
([SomeAttribute] i, [SomeOtherAttribute] j) => { ... } // Ok
```

- <span data-ttu-id="b485d-123">Bir anonim metodun veya lambda parametrelerine, virgülle ayrılmış veya tam öznitelik sözdizimi kullanılarak birden çok öznitelik uygulanabilir. Örneğin:</span><span class="sxs-lookup"><span data-stu-id="b485d-123">Multiple attributes may be applied to the parameters of an anonymous method or lambda, using either the comma-delimited or full-attribute syntax, for example:</span></span>

```csharp
([FirstAttribute, SecondAttribute] i) => { ... } // Ok
([FirstAttribute] [SecondAttribute] i) => { ... } // Ok
```

- <span data-ttu-id="b485d-124">`return`hedefli öznitelikler Lambdalar üzerinde de kullanılabilir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="b485d-124">`return`-targeted attributes may also be used on lambdas, for example:</span></span>

```csharp
([return: SomeAttribute] (i) => { ... }) // Ok
```

- <span data-ttu-id="b485d-125">Derleyici, öznitelikleri oluşturulan Yöntem ve bağımsız değişkenler üzerinde diğer herhangi bir yöntem için olduğu gibi bu yöntemlere verir.</span><span class="sxs-lookup"><span data-stu-id="b485d-125">The compiler outputs the attributes onto the generated method and arguments to those methods as it would for any other method.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="b485d-126">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="b485d-126">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="b485d-127">yok</span><span class="sxs-lookup"><span data-stu-id="b485d-127">n/a</span></span>

## <a name="alternatives"></a><span data-ttu-id="b485d-128">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="b485d-128">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="b485d-129">yok</span><span class="sxs-lookup"><span data-stu-id="b485d-129">n/a</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="b485d-130">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="b485d-130">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="b485d-131">yok</span><span class="sxs-lookup"><span data-stu-id="b485d-131">n/a</span></span>

## <a name="design-meetings"></a><span data-ttu-id="b485d-132">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="b485d-132">Design meetings</span></span>

<span data-ttu-id="b485d-133">yok</span><span class="sxs-lookup"><span data-stu-id="b485d-133">n/a</span></span>