---
ms.openlocfilehash: fdd858c895d56a7a6b410e6ea7be3032e4851fd6
ms.sourcegitcommit: 5a88d5432d32c690c6b870fc4be32cf26cadd76f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2019
ms.locfileid: "79484960"
---
# <a name="null-coalescing-assignment"></a>null birleşim ataması

* [x] önerilir
* [] Prototipi: başlatılmadı
* [] Uygulama: başlatılmadı
* [] Belirtimi: aşağıda

## <a name="summary"></a>Özet
[summary]: #summary

Null ise bir değişkene bir değer atandığında ortak bir kodlama modelini basitleştirir.

Bu teklifin bir parçası olarak, türü, sol tarafta kullanılacak kısıtlanmış olmayan bir tür parametresi olan bir ifadeye izin vermek için `??` tür gereksinimlerini da gevyoruz.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Formun kodunu görmek yaygındır.

```csharp
if (variable == null)
{
    variable = expression;
}
```

Bu teklif, bu işlevi gerçekleştiren dile aşırı yüklenebilir olmayan bir ikili işleç ekler.

Bu özellik için en az sekiz ayrı topluluk isteği vardı.

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

Yeni bir atama operatörü formu ekledik

``` antlr
assignment_operator
    : '??='
    ;
```

[Bileşik atama işleçleri için var olan anlam kurallarını](../../spec/expressions.md#compound-assignment)izleyen bu, sol taraftaki boş değer değilse atamayı tam olarak gerçekleştirmemiz dışında. Bu özelliğin kuralları aşağıdaki gibidir.

`a ??= b`, `A` `a`türüdür, `B` `b`türüdür ve `A0` null yapılabilir bir değer türü ise `A` temel türüdür:`A`

1. `A` yoksa veya null yapılamayan bir değer türü ise, derleme zamanı hatası oluşur.
2. `B` örtük olarak `A` veya `A0` 'e dönüştürülemez (`A0` varsa), bir derleme zamanı hatası oluşur.
3. `A0` varsa ve `B` örtük olarak `A0`'e dönüştürülebilir ve `B` dinamik değilse, `a ??= b` türü `A0`olur. `a ??= b` çalışma zamanında şu şekilde değerlendirilir:
   ```C#
   var tmp = a.GetValueOrDefault();
   if (!a.HasValue) { tmp = b; a = tmp; }
   tmp
   ```
   `a` hariç, yalnızca bir kez değerlendirilir.
4. Aksi takdirde, `a ??= b` türü `A`. `a ??= b` çalışma zamanında `a ?? (a = b)`olarak değerlendirilir, ancak `a` yalnızca bir kez değerlendirilir.


`??`'nin tür gereksinimlerinin bir kısmını, `a ?? b`verilen, `A` `a`türü olduğu belirtilen şekilde güncelleştirmekte olan belirtimi güncelleştiririz:

> 1. Varsa ve null yapılabilir bir tür ya da bir başvuru türü değilse, derleme zamanı hatası oluşur.

Bu gereksinimi şu şekilde rahatlıyoruz:

1. Bir varsa ve null yapılamayan bir değer türü ise, derleme zamanı hatası oluşur.

Bu, sınırlandırılmamış tür parametresi var olduğundan, null olabilen tür parametreleri üzerinde çalışır, ancak bir başvuru türü değildir.

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Herhangi bir dil özelliğinde olduğu gibi, bu dilin ek karmaşıklığının, özellikten faydalanabilecek C# programlar gövdesine sunulan ek açıklıkla bir repaıp olup olmadığını sormalıdır.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

Programcı `(x = x ?? y)`, `if (x == null) x = y;`veya `x ?? (x = y)` el ile yazabilir.

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

- [] LDM incelemesi gerektiriyor
- [] `&&=` ve `||=` işleçlerini da destekliyoruz.

## <a name="design-meetings"></a>Tasarım toplantıları

Yok.
