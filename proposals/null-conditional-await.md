---
ms.openlocfilehash: 3d10cacef98e802333c8cbe65edb93c19c74cabf
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484498"
---
# <a name="null-conditional-await"></a>null-koşullu await

* [x] önerilir
* [] Prototipi: yok
* [] Uygulama: yok
* [] Belirtim: başlatıldı, aşağıdan

## <a name="summary"></a>Özet
[summary]: #summary

`await? e`form ifadesini destekler, bu, null olmayan bir `e` bekler, aksi takdirde `null`sonuçlanır.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Bu ortak bir kodlama modelidir ve bu özellik, var olan null yayan ve null birleşim işleçleriyle iyi bir şekilde eşitleniyor.

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

*Await_expression*yeni bir form ekleyeceğiz:

```antlr
await_expression
    : 'await' '?' unary_expression
    ;
```

Null koşullu `await` işleci, yalnızca bu işlenen null olmadığında işleneni bekler. Aksi takdirde işleci uygulamanın sonucu null olur.

Sonucun türü, [null koşullu işlecin kuralları](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator)kullanılarak hesaplanır.

> **Note:** `e` `Task`tür ise, `e` `null``await? e;` hiçbir şey yapmaz ve bu `e` `null`bekler.
>
> `e`, `K` bir değer türü olduğu `Task<K>` tür ise, `await? e` `K?`türünde bir değer verir.

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Herhangi bir dil özelliğinde olduğu gibi, bu dilin ek karmaşıklığının, özellikten faydalanabilecek C# programlar gövdesine sunulan ek açıklıkla bir repaıp olup olmadığını sormalıdır.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

Bazı demirbaş kod gerektirse de, bu işlecin kullanımları genellikle `(e == null) ? null : await e` veya `if (e != null) await e`gibi bir deyim ile değiştirilebilir.

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

- [] LDM incelemesi gerektiriyor

## <a name="design-meetings"></a>Tasarım toplantıları

Yok.
