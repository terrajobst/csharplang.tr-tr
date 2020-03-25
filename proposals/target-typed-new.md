---
ms.openlocfilehash: 07b4afe4a3fcbf10c978f05e642dfd8a47d53ea5
ms.sourcegitcommit: 194a043db72b9244f8db45db326cc82de6cec965
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80217209"
---

# <a name="target-typed-new-expressions"></a>Hedef türü belirlenmiş `new` ifadeleri

* [x] önerilir
* [x] [prototipi](https://github.com/alrz/roslyn/tree/features/target-typed-new)
* [] Uygulama
* [] Belirtimi

## <a name="summary"></a>Özet
[summary]: #summary

Tür bilindiğinde oluşturucular için tür belirtimi gerektirmeyin. 

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Türü çoğaltmadan alan başlatılmasına izin verin.
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

Kullanımdan çıkarsanamıyor, türü dışarıda bırakabilirsiniz.
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

Türü yazarken bir nesne oluşturun.
```cs
private readonly static object s_syncObj = new();
```

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

*Object_creation_expression* sözdizimi, parantezler mevcut olduğunda *türü* isteğe bağlı hale getirmek için değiştirilir. Bu, *anonymous_object_creation_expression*belirsizliğin ele almak için gereklidir.
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```

Hedef türü belirlenmiş bir `new` herhangi bir türe dönüştürülebilir. Sonuç olarak, aşırı yükleme çözümüne katkıda bulunmaz. Bu, genellikle öngörülemeyen son değişikliklerden kaçınmaktır.

Bağımsız değişken listesi ve başlatıcı ifadeleri tür saptandıktan sonra bağlanacak.

İfadenin türü, aşağıdakilerden biri olması gereken hedef türden çıkarsanamıyor:

- **Herhangi bir struct türü** (demet türleri dahil)
- **Herhangi bir başvuru türü** (temsilci türleri dahil)
- Oluşturucu veya `struct` kısıtlaması olan **herhangi bir tür parametresi**

Aşağıdaki özel durumlarla birlikte:

- Sabit listesi türleri **:** tüm sabit listesi türleri sıfır sabiti içermez, bu nedenle açık numaralandırma üyesinin kullanılması istenebilir.
- **Arabirim türleri:** bu bir sunarak pazarların özelliğidir ve türün açıkça bahsetmek için tercih edilmelidir.
- **Dizi türleri:** dizilerin uzunluğu sağlamak için özel bir sözdizimi olması gerekir.
- **dinamik:** `new dynamic()`izin vermedik, bu nedenle hedef tür olarak `dynamic` `new()` izin vermedik.

*Object_creation_expression* izin verilmeyen tüm diğer türler Ayrıca, örneğin işaretçi türleri hariç tutulur.

Hedef türü null yapılabilir bir değer türü olduğunda, hedef türü belirlenmiş `new` null yapılabilir tür yerine temel alınan türe dönüştürülür.

> **Açık sorun:** temsilcilerin ve tanımlama gruplarının hedef tür olarak izin vermesi gerekir mi?

Yukarıdaki kurallar temsilcileri (bir başvuru türü) ve tanımlama gruplarını (bir struct türü) içerir. Her iki tür de oluşturulabilir olsa da, tür ınable ise anonim bir işlev veya bir tanımlama grubu değişmez değeri zaten kullanılabilir.
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a>Çeşitli

`throw new()` izin verilmiyor.

Target-Typed `new` ikili işleçlerle kullanılamaz.

Target için tür olmadığında bu izin verilmez: Birli İşleçler, `foreach`koleksiyonu bir `using`, bir `await` ifadesinde, `sizeof`işlecinin sol işleneni olarak `fixed` işlecinin işleneni olarak, bir`new().field`bildiriminde bir`someDynamic.Method(new())`deyimindeki bir`new { Prop = new() }`) anonim tür özelliği (`is`) olarak, bir `??` deyimi içinde `lock`, işleci içindeki bir deyiminde, bir ifadesinde ,  ...

Ayrıca, `ref`olarak da izin verilmez.

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Hedef türü bazı sorunlar vardı `new` yeni son değişiklikler kategorileri oluşturuyor, ancak zaten `null` ve `default`ve bu da önemli bir sorun olmadık.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

Alan başlatılmasında yinelenmek için çok uzun olan türlerin büyük bir kısmının türü, türün kendisi değil tür *bağımsız değişkenleri* hakkında bilgi verebiliriz. yalnızca `new Dictionary(...)` (veya benzeri) gibi tür bağımsız değişkenleri ve bağımsız değişkenlerden ya da koleksiyon başlatıcıdan yerel olarak tür bağımsız değişkenleri çıkarsız.

## <a name="questions"></a>UL
[questions]: #questions

- İfade ağaçlarında kullanımları planlıyoruz mı? eşleşen
- Özellik `dynamic` bağımsız değişkenlerle nasıl etkileşime girer? (özel bir işleme yok)
- IntelliSense `new()`ile nasıl çalışır? (yalnızca tek bir hedef türü olduğunda)

## <a name="design-meetings"></a>Tasarım toplantıları

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
