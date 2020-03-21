---
ms.openlocfilehash: 38740069a2e105f920fa275c443f4560055e2901
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108382"
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

### Miscellaneous

`throw new()` is disallowed.

Target-typed `new` is not allowed with binary operators.

It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...

It is also disallowed as a `ref`.

## Drawbacks
[drawbacks]: #drawbacks

There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.

## Alternatives
[alternatives]: #alternatives

Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.

## Questions
[questions]: #questions

- Should we forbid usages in expression trees? (no)
- How the feature interacts with `dynamic` arguments? (no special treatment)
- How IntelliSense should work with `new()`? (only when there is a single target-type)

## Design meetings

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
