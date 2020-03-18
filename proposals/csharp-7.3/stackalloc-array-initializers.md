---
ms.openlocfilehash: 7ea62713416ef82034963aef06f3cb11703342ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484610"
---
# <a name="stackalloc-array-initializers"></a>Stackalloc dizi başlatıcılar

## <a name="summary"></a>Özet
[summary]: #summary

Dizi başlatıcısı sözdiziminin `stackalloc`birlikte kullanılmasına izin verin.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Sıradan diziler, öğeleri oluşturma sırasında başlatılabilir. `stackalloc` durumunda buna izin vermek mantıklı bir şekilde görünür.

Bu tür sözdiziminin neden çok sık `stackalloc` izin verilmediğine ilişkin sorunuz.  
Bkz. Örneğin, [#1112](https://github.com/dotnet/csharplang/issues/1112)

## <a name="detailed-design"></a>Ayrıntılı tasarım

Sıradan diziler aşağıdaki sözdizimi kullanılarak oluşturulabilir:

```csharp
new int[3]
new int[3] { 1, 2, 3 }
new int[] { 1, 2, 3 }
new[] { 1, 2, 3 }
```

Yığın ayrılmış dizilerinin şu şekilde oluşturulmasını izin vereceğiz:  

```csharp
stackalloc int[3]               // currently allowed
stackalloc int[3] { 1, 2, 3 }
stackalloc int[] { 1, 2, 3 }
stackalloc[] { 1, 2, 3 }
```

Tüm durumların semantiği, dizilerle aynı şekilde kabaca aynıdır.  
Örneğin: öğe türünün başlatıcıdan çıkarılmış olması ve "yönetilmeyen" bir tür olması gerekir.

Not: Özellik hedefe bağlı değil `Span<T>`. `T*` durumda olduğu gibi bir durumdur. bu nedenle, `Span<T>` durumunda bu durum koşulunu sağlamak mantıklı görünmüyor.  

## <a name="translation"></a>Çeviri

Naïve uygulama, diziyi bir dizi öğe temelinde atamalar aracılığıyla oluşturmadan hemen başlatabilir.  

Ayrıca, diziler ile aynı şekilde, öğelerin tümünün veya çoğunun blittable türler olduğu ve tüm sabit öğelerin önceden oluşturulmuş durumuna kopyalanarak daha verimli teknikler kullanıldığı durumları algılamak mümkün olabilir ve istenebilir. 

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

Bu bir kullanışlı özelliktir. Yalnızca bir şey yapmak mümkündür.

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>Tasarım toplantıları

Henüz yok. 
