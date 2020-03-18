---
ms.openlocfilehash: 25e95b3ab8c384a7e66e59a7f9422cc9699074d7
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484722"
---
# <a name="infer-tuple-names-aka-tuple-projection-initializers"></a>Tanımlama grubu adlarını (aka) çıkarın. demet projeksiyon başlatıcıları)

## <a name="summary"></a>Özet
[summary]: #summary

Birçok yaygın durumda, bu özellik demet öğesi adlarının atlanmasýný ve bunun yerine çıkarlanmasýný sağlar. Örneğin, `(f1: x.f1, f2: x?.f2)`yazmak yerine "F1" ve "F2" öğe adları `(x.f1, x?.f2)`çıkarsanamıyor.

Bu, oluşturma sırasında üye adlarının kullanılmasına izin veren anonim türlerin davranışını paraleldir. Örneğin, `new { x.f1, y?.f2 }` "F1" ve "F2" üyelerini bildirir.

Bu, LINQ içinde tanımlama grupları kullanırken özellikle yararlıdır:

```csharp
// "c" and "result" have element names "f1" and "f2"
var result = list.Select(c => (c.f1, c.f2)).Where(t => t.f2 == 1); 
```

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

Değişikliğin iki bölümü vardır:

1.  Açık adı olmayan her demet öğesi için bir aday adı çıkarımı deneyin:
    -   Anonim türler için ad çıkarımı ile aynı kuralların kullanılması.
        - ' C#De, bu üç durumda izin verir: `y` (tanımlayıcı), `x.y` (basit üye erişimi) ve `x?.y` (koşullu erişim).
        - VB 'de, bu `x.y()`gibi ek durumlar sağlar.
    -   Yasak veya zaten örtük olduklarından ayrılmış demet adlarını ( C#büyük/küçük harfe duyarlı, vb 'de büyük/küçük harfe duyarlı) reddetme. Örneğin, `ItemN`, `Rest`ve `ToString`.
    -   Herhangi bir aday adı, tüm kayıt kapsamındaki (VB 'de C#büyük/küçük harfe duyarlı) yineleniyorlarsa, bu adayları düşürüyoruz.
2.  Dönüştürmeler sırasında (demet sabit değerlerinde adları bırakma ve bu bilgileri uyarma), çıkartılan adlar hiçbir uyarı oluşturmaz. Bu, mevcut demet kodunun kesilmesini önler.

Yinelenenleri işleme kuralının anonim türler için olandan farklı olduğunu unutmayın. Örneğin, `new { x.f1, x.f1 }` bir hata üretir, ancak `(x.f1, x.f1)` yine de izin verilir (herhangi bir Çıkarsanan ad olmadan). Bu, mevcut demet kodunun kesilmesini önler.

Tutarlılık için, aynı durum, yinelenenleri kaldırma (içinde) tarafından üretilen diziler için de C#geçerlidir:

```csharp
// tuple has element names "f1" and "f2" 
var tuple = ((x.f1, x?.f2) = (1, 2));
```

Aynı zamanda, ifadenin adı ve büyük/küçük harf duyarsız ad karşılaştırmaları için VB 'ye özgü kuralları kullanarak VB tanımlama grupları için de geçerlidir.

Dil sürümü " C# 7,0" olan 7,1 derleyicisini (veya sonraki bir sürümünü) kullanırken, öğe adları çıkarsolacaktır (özellik kullanılabilir olmasa da), ancak bunlara erişmeye çalışmak için bir kullanım sitesi hatası olacaktır. Bu, daha sonra uyumluluk sorununa (aşağıda açıklanmıştır) dönük yeni kod eklemelerini sınırlandırır.

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Bunun ana dezavantajı, bunun 7,0 'den C# bir uyumluluk kesmesi sunmaıdır:

```csharp
Action y = () => M();
var t = (x: x, y);
t.y(); // this might have previously picked up an extension method called “y”, but would now call the lambda.
```

Uyumluluk Council bu kesmeyi kabul ettiği için, sınırlandırılacak ve (7,0 ' de C# ), başlıkların gönderildiği ve zaman penceresinin bu yana kabul edilebilir olduğunu belirledi.

## <a name="references"></a>Başvurular
- [LDM 4 Nisan 2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md#tuple-names)
- [GitHub tartışması](https://github.com/dotnet/csharplang/issues/370) (Bu sorunu getirmek için @alrz teşekkürler)
- [Tanımlama grubu tasarımı](https://github.com/dotnet/roslyn/blob/master/docs/features/tuples.md)
