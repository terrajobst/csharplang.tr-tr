---
ms.openlocfilehash: 032cb8590a0b6e83f8ab6201e10720f1b254c605
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484533"
---
# <a name="nullable-enhanced-common-type"></a>Null yapılabilir-gelişmiş ortak tür

* [x] önerilir
* [] Prototipi: yok
* [] Uygulama: yok
* [] Belirtimi: aşağıya bakın

## <a name="summary"></a>Özet
[summary]: #summary

Geçerli ortak tür algoritması sonuçlarının sayaç sezgisel olduğu bir durum vardır ve programcının koda gereksiz bir şekilde saçılması gibi ne kadar fekine bir ekleme işlemi yaptığını ortaya çıkarır. Bu değişiklik ile `condition ? 1 : null` gibi bir ifade `int?`türünde bir değer ve `x` `condition ? x : 1.0` gibi bir ifade `int?` türünde bir değere neden olur.`double?`

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Bu, programcının gereksiz bir demirbaş kodu gibi ne kadar çok daha yaygın bir nedendir.

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

Aşağıdaki durumları etkilemek için [bir dizi ifadenin en iyi ortak türünü bulma](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) belirtimini değiştirdik:

- Bir ifade null yapılamayan bir değer türü `T` ve diğeri null bir sabit değerdir, sonuç `T?`türündedir.
- Bir ifade null yapılabilir bir değer türü `T?` ve diğeri bir değer türü `U`ve `U``T` ' den örtük bir dönüştürme varsa, sonuç `U?`türüdür.

Bu, dilin aşağıdaki yönlerini etkilemek için beklenir:

- [Üçlü ifade](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)
- örtük olarak yazılan [dizi oluşturma ifadesi](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)
- Tür çıkarımı için [lambdanın dönüş türünü](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) erteleme
- `M(1, null)`olarak `M<T>(T a, T b)` çağırma gibi genel türler içeren durumlar.

Daha kesin olarak, belirtimin aşağıdaki bölümlerini (kalın yazı tipiyle eklenenler, üstü çizili) değiştiririz:

> #### <a name="output-type-inferences"></a>Çıkış türü ında
> 
> Aşağıdaki şekilde *`E` bir ifadeden* *bir tür* `T` bir *Çıkış türü çıkarımı* yapılır:
> 
> *  `E`, `U` çıkartılan dönüş türü olan bir anonim işlevse[(çıkartılan dönüş türü](expressions.md#inferred-return-type)) ve `T`, dönüş türü `Tb`olan bir temsilci türü veya ifade ağacı türü ise *, `U` ile `Tb`* *arasında* daha *düşük bağlantılı bir çıkarım* ([alt sınır](expressions.md#lower-bound-inferences)) yapılır.
> *  Aksi takdirde, `E` bir yöntem grubu ise ve `T`, parametre türleri `T1...Tk` ve dönüş türü `Tb`olan bir temsilci türü ya da ifade ağacı türü `E` ve `T1...Tk` bir `U`dönüş türü ile tek bir yöntem ortaya çıkardı `U` *, `Tb`ile* arasında *daha düşük* bir çıkarma yapılır. *from*
> *  \* * Aksi takdirde, `E` Nullable değer türü `U?`olan bir ifadedir, *daha* *sonra `U` '* *den* `T` ve null bir *sınır* `T`eklenir. **
> *  Aksi takdirde, `E` `U`türünde bir ifade ise, `U` ' *den* `T` *'ye* *daha düşük bir çıkarım* yapılır.
> *  **Aksi takdirde, `E` `null`değeri olan sabit bir ifade ise, null bir *sınırı* `T` eklenir** 
> *  Aksi takdirde, hiçbir Inse yapılmaz.

> #### <a name="fixing"></a>Düzelttikten
> 
> Bir dizi sınırı olan `Xi` *sabit olmayan* bir tür değişkeni aşağıdaki gibi *düzeltilir* :
> 
> *  `Uj` *aday türleri* kümesi, `Xi`sınırları kümesindeki tüm türlerin kümesi olarak başlatılır.
> *  Sonra her bir `Xi` için her birini sırayla inceliyoruz `U`: `U` aynı olmayan tüm türler `Uj` `Xi` aday kümesinden kaldırılır. Tüm `Xi` `U` her bir alt sınır için `U` örtük bir dönüştürme *olmadığı* `Uj` tüm türler için aday kümesinden kaldırılır. `U` örtük bir dönüştürme *olmadığı* `Uj` tüm türlerin `Xi` `U` her üst sınır için aday kümesinden kaldırılır.
> *  Kalan aday türleri arasında `Uj`, tüm diğer aday türlerine örtük bir dönüştürme olan `V` benzersiz bir tür vardır ve ~~`Xi` `V`için düzeltilir.~~
>     -  **`V` bir değer türüdür ve `Xi`için bir *null sınırı* varsa `Xi` düzeltilir `V?`**
>     -  **Aksi takdirde `Xi` `V` için düzeltilir**
> *  Aksi takdirde tür çıkarımı başarısız olur.

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Bu teklif tarafından sunulan bazı uyumsuzluklar olabilir.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

Yok.

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

- [] Bu teklifin tanıtılmasıyla ilgili uyumsuzluğun önem derecesi nedir ve nasıl dağıtılabilir?

## <a name="design-meetings"></a>Tasarım toplantıları

Yok.
