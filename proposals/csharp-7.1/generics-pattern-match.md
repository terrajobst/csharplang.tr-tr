---
ms.openlocfilehash: 4f66c0f60d05ed6509a1d0843318a71d1b36c351
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484708"
---
# <a name="pattern-matching-with-generics"></a>Genel türler ile desenler eşleme

* [x] önerilir
* [] Prototipi:
* [] Uygulama:
* [] Belirtimi:

## <a name="summary"></a>Özet
[summary]: #summary

[ C# Mevcut as işlecinin](../../spec/expressions.md#the-as-operator) belirtimi, bir açık tür olduğunda işlenenin türü ve belirtilen tür arasında dönüştürme olmaması için izin verir. Ancak, 7 C# ' de `Type identifier` deseninin, girişin türü ve verilen tür arasında bir dönüştürme olması gerekir.

Bu işlemi C# rahat hale getirme ve `expression is Type identifier`değiştirme konusunda, `expression as Type` izin verildiğinde izin verilmesi için izin verilen koşullarda izin verilmesi önerilir. Özellikle, yeni durumlar ifade türünün veya belirtilen türün açık bir tür olduğu durumlardır. 

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Model eşleme 'nin şu anda derlenmesinin "açık" olmasına neden olması gerektiği durumlar. Bkz. Örneğin, https://github.com/dotnet/roslyn/issues/16195.

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

Paragraf eşleme belirtimindeki paragrafı değiştirdik (önerilen ekleme kalın olarak gösterilmiştir):

> Sol taraftaki statik türdeki belirli birleşimler ve verilen tür uyumsuz olarak değerlendirilir ve derleme zamanı hatası ile sonuçlanır. `E` statik tür değeri, bir kimlik dönüştürmesi, örtük bir başvuru dönüştürmesi, paketleme dönüştürmesi, açık bir başvuru dönüştürmesi veya `E` ' den `T`' e bir kutudan çıkarma dönüştürmesi ya da **`E` ya da `T` bir açık tür**ise, tür ile *uyumlu* bir değer `T` olarak kabul edilir. `E` türündeki bir ifade ile eşleştirildiği tür deseninin türüyle uyumlu kalıp yoksa, derleme zamanı hatasıdır.

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Yok.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

Yok.

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

Yok.

## <a name="design-meetings"></a>Tasarım toplantıları

LDM bu soruyu kabul eden ve keçeli bir hata çözme düzeyi değişikliği vardı. Bunu ayrı bir dil özelliği olarak kabul ediyoruz çünkü yalnızca dil yayımlandıktan sonra değişikliği yapmak ileri bir uyumsuzluk sağlar. Önerilen değişikliğin kullanılması, programcının dil sürümü 7,1 ' i belirtmesini gerektirir.
