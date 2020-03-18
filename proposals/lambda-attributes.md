---
ms.openlocfilehash: 9647fff40a1e45bef917f140612ae4e91abea958
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484575"
---
# <a name="lambda-attributes"></a>Lambda öznitelikleri

* [x] önerilir
* [] Prototipi
* [] Uygulama
* [] Belirtimi

## <a name="summary"></a>Özet
[summary]: #summary

Özniteliklerin Lambdalar (ve anonim yöntemler) ve lambda/anonim yöntem parametrelerine (normal metotlarda olabilecek) uygulanmasına izin verin.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

İki birincil motive:

1. Meta verileri derleme zamanında çözümleyiciler için görünür hale sağlamak.
2. Çalışma zamanında yansıma ve araç için görünür hale, meta verileri sağlamak için.

Örnek olarak, (1): performans duyarlı kod Için, kapanışlara ve temsilcilerin, durumu kapatan Lambdalar için ayrılmakta olduğu bir çözümleyici olması yararlı olacaktır.  Genellikle, bu kodun bir geliştiricisi, herhangi bir durumu yakalamaya engel olmak için bir veya kendi yolunda kalacak, böylece derleyicinin Yöntem için bir statik yöntem ve önbelleklenebilir bir temsilci oluşturabilmesi, ya da geliştirici, en azından bir kapanış nesnesinin ayrılmasına engel olmak için `this`olduğundan emin olmanızı sağlar.  Ancak, nelerin yakalanabileceğini kısıtlamak için dil desteği olmadan, yanlışlıkla daha kolay bir şekilde kapatılabilir.  Bir geliştirici, bir geliştiricinin neleri kapatmasına izin verildiğini belirtmek için özniteliklere sahip Lambdalar eklemek için yararlı olacaktır, örneğin:

```csharp
[CaptureNone] // can't close over any instance state
[CaptureThis] // can only capture `this` and no other instance state
[CaptureAny] // can close over any instance state
```

Daha sonra, durum yanlış yakalandıktan sonra bir çözümleyici yazılabilir, örneğin:

```csharp
var results = collection.Select([CaptureNone](i) => Process(item)); // Analyzer error: [CaptureNone] lambdas captures `this`
...
private U Process(T item) { ... }
```

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

- Normal yöntemlerle aynı öznitelik söz dizimini kullanarak, öznitelikler bir lambda veya anonim yöntemin başlangıcında uygulanabilir, örneğin:

```csharp
[SomeAttribute(...)] () => { ... }
[SomeAttribute(...)] delegate (int i) { ... }
```

- Bir özniteliğin lambda yöntemine veya bağımsız değişkenlerden birine uygulanıp uygulanmayacağı konusunda belirsizliğe engel olmak için, öznitelikler yalnızca herhangi bir bağımsız değişken etrafında kullanıldığı zaman kullanılabilir; örneğin:

```csharp
[SomeAttribute] i => { ... } // ERROR
[SomeAttribute] (i) => { ... } // Ok
[SomeAttribute] (int i) => { ... } // Ok
```

- Anonim yöntemlerle, `delegate` anahtar sözcüğünden önce yönteme bir öznitelik uygulamak için Parens gerekmez, örneğin:

```csharp
[SomeAttribute] delegate { ... } // Ok
[SomeAttribute] delegate (int i) => { ... } // Ok
```

- Birden çok öznitelik, standart virgülle ayrılmış sözdizimi veya tam öznitelik sözdizimi aracılığıyla uygulanabilir. Örneğin:

```csharp
[FirstAttribute, SecondAttribute] (i) => { ... } // Ok
[FirstAttribute] [SecondAttribute] (i) => { .... } // Ok
```

- Öznitelikler anonim bir yönteme veya lambda parametrelerine uygulanabilir, ancak yalnızca parülar her bağımsız değişken etrafında kullanıldığında, örneğin:

```csharp
[SomeAttribute] i => { ... } // ERROR
([SomeAttribute] i) => { .... } // Ok
([SomeAttribute] int i) => { ... } // Ok
([SomeAttribute] i, [SomeOtherAttribute] j) => { ... } // Ok
```

- Bir anonim metodun veya lambda parametrelerine, virgülle ayrılmış veya tam öznitelik sözdizimi kullanılarak birden çok öznitelik uygulanabilir. Örneğin:

```csharp
([FirstAttribute, SecondAttribute] i) => { ... } // Ok
([FirstAttribute] [SecondAttribute] i) => { ... } // Ok
```

- `return`hedefli öznitelikler Lambdalar üzerinde de kullanılabilir, örneğin:

```csharp
([return: SomeAttribute] (i) => { ... }) // Ok
```

- Derleyici, öznitelikleri oluşturulan Yöntem ve bağımsız değişkenler üzerinde diğer herhangi bir yöntem için olduğu gibi bu yöntemlere verir.

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

yok

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

yok

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

yok

## <a name="design-meetings"></a>Tasarım toplantıları

yok