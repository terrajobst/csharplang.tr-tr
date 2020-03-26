---
ms.openlocfilehash: f000dda7eeb1c4f17c26f94c326a12a9d0014288
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281976"
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

## <a name="specification"></a>Min
[design]: #detailed-design

Yeni bir sözdizimsel form, *object_creation_expression* *target_typed_new* , *türün* isteğe bağlı olduğu kabul edilir.

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    | target_typed_new
    ;
target_typed_new
    : 'new' '(' argument_list? ')' object_or_collection_initializer?
    ;
```

*Target_typed_new* ifadesinin türü yok. Ancak, ifadeden örtük dönüştürme olan yeni bir *nesne oluşturma dönüştürmesi* vardır. bu bir target_typed_new, her türe bir *target_typed_new* vardır.

Hedef türü `T`verildiğinde tür `T0`, `T` `System.Nullable`bir örneğidir `T`temel türüdür. Aksi takdirde `T0` `T`. `T` türüne dönüştürülen *target_typed_new* ifadesinin anlamı, türü olarak `T0` belirten karşılık gelen bir *object_creation_expression* anlamı ile aynıdır.

Bir *target_typed_new* birli veya ikili işlecin işleneni olarak kullanılıyorsa ya da bir *nesne oluşturma dönüştürmesinin*konusu olmadığı durumlarda kullanılıyorsa, derleme zamanı hatasıdır.

> **Açık sorun:** temsilcilerin ve tanımlama gruplarının hedef tür olarak izin vermesi gerekir mi?

Yukarıdaki kurallar temsilcileri (bir başvuru türü) ve tanımlama gruplarını (bir struct türü) içerir. Her iki tür de oluşturulabilir olsa da, tür ınable ise anonim bir işlev veya bir tanımlama grubu değişmez değeri zaten kullanılabilir.
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // OK; same as (0, 0)
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a>Çeşitli

Aşağıda, belirtimin sonuçları verilmiştir:

- `throw new()` izin veriliyor (hedef tür `System.Exception`)
- Target-Typed `new` ikili işleçlerle kullanılamaz.
- Target için tür olmadığında bu izin verilmez: Birli İşleçler, `foreach`koleksiyonu bir `using`, bir `await` ifadesinde, `sizeof`işlecinin sol işleneni olarak `fixed` işlecinin işleneni olarak, bir`new().field`bildiriminde bir`someDynamic.Method(new())`deyimindeki bir`new { Prop = new() }`) anonim tür özelliği (`is`) olarak, bir `??` deyimi içinde `lock`, işleci içindeki bir deyiminde, bir ifadesinde ,  ...
- Ayrıca, `ref`olarak da izin verilmez.
- Dönüştürme hedefleri olarak aşağıdaki tür türlere izin verilmez
  - **Sabit listesi türleri:** `new()` çalışacaktır (`new Enum()` varsayılan değer vermek için çalışır), ancak `new(1)` iş numaralama türlerinde bir oluşturucuya sahip değildir.
  - **Arabirim türleri:** Bu, COM türleri için ilgili oluşturma ifadesiyle aynı şekilde çalışır.
  - **Dizi türleri:** dizilerin uzunluğu sağlamak için özel bir sözdizimi olması gerekir.    
  - **dinamik:** `new dynamic()`izin vermedik, bu nedenle hedef tür olarak `dynamic` `new()` izin vermedik.
  - **Tanımlama grupları:** Bunlar, temel alınan türü kullanarak bir nesne oluşturma ile aynı anlama sahiptir.
  - *Object_creation_expression* izin verilmeyen tüm diğer türler Ayrıca, örneğin işaretçi türleri hariç tutulur.   

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
- [LDM-2020-03-25](https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-03-25.md)
