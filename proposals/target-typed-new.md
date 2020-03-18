---
ms.openlocfilehash: 4e2a536bab00859b003e8d967cb1927a99a9fa21
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484540"
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

- **Herhangi bir struct türü**
- **Herhangi bir başvuru türü**
- Oluşturucu veya `struct` kısıtlaması olan **herhangi bir tür parametresi**

Aşağıdaki özel durumlarla birlikte:

- Sabit listesi türleri **:** tüm sabit listesi türleri sıfır sabiti içermez, bu nedenle açık numaralandırma üyesinin kullanılması istenebilir.
- **Arabirim türleri:** bu bir sunarak pazarların özelliğidir ve türün açıkça bahsetmek için tercih edilmelidir.
- **Dizi türleri:** dizilerin uzunluğu sağlamak için özel bir sözdizimi olması gerekir.
- **Struct varsayılan Oluşturucusu**: Bu, tüm ilkel türler ve çoğu değer türlerini kurallar. Bu tür türler için varsayılan değeri kullanmak isterseniz, bunun yerine `default` yazabilirsiniz.

*Object_creation_expression* izin verilmeyen tüm diğer türler Ayrıca, örneğin işaretçi türleri hariç tutulur.

> **Açık sorun:** temsilcilerin ve tanımlama gruplarının hedef tür olarak izin vermesi gerekir mi?

Yukarıdaki kurallar temsilcileri (bir başvuru türü) ve tanımlama gruplarını (bir struct türü) içerir. Her iki tür de oluşturulabilir olsa da, tür ınable ise anonim bir işlev veya bir tanımlama grubu değişmez değeri zaten kullanılabilir.
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

var x = new() == (1, 2); // ruled out by "use of struct default constructor"
var x = new(1, 2) == (1, 2) // "new" is redundant
```


> **Açık sorun:** hedef tür olarak `Exception` `throw new()` izin vermemiz gerekir mi?

Bugün `throw null`, ancak `throw default` değil (aynı etkiye sahip olabilir). Diğer taraftan, `throw new()` `throw new Exception(...)`için bir toplu olarak yararlı olabilir. Geçerli belirtim tarafından zaten izin verildiğini unutmayın. `Exception` bir başvuru türüdür ve throw deyiminin belirtimi, ifadenin `Exception`dönüştürüldüğünü belirtir.

> **Açık sorun:** Kullanıcı tanımlı karşılaştırma ve Aritmetik işleçlerle, hedef türü belirlenmiş `new` kullanımlarına izin vermemiz gerekir mi?

Karşılaştırma için `default` yalnızca eşitlik (Kullanıcı tanımlı ve yerleşik) işleçlerini destekler. Diğer işleçleri de `new()` destekletsin mi?

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Yok.

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
