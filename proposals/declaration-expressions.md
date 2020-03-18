---
ms.openlocfilehash: d2064ec1637e50962262c9380281abd5e1711402
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484582"
---
# <a name="declaration-expressions"></a>Bildirim ifadeleri

Deyim olarak bildirim atamalarını destekleme.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Daha fazla durumda, kodu basitleştirecek ve `var` kullanılmasına izin vererek bildirim noktasında başlatmaya izin verin.

```csharp
SpecialType ReferenceType =>
    (var st = _type.SpecialType).IsValueType() ? SpecialType.None : st;
```

`out var`benzer `ref` bağımsız değişkenler için bildirimlere izin verin.

```csharp
Convert(source, destination, ref List<Diagnostic> diagnostics = null);
```

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

İfadeler, bildirim atamasını içerecek şekilde genişletilir. Öncelik atama ile aynıdır.

```antlr
expression
    : non_assignment_expression
    | assignment
    | declaration_assignment_expression // new
    ;
declaration_assignment_expression // new
    : declaration_expression '=' local_variable_initializer
    ;
declaration_expression // C# 7.0
    | type variable_designation
    ;
```

Bildirim ataması tek bir yereldir.

Bildirim atama ifadesinin türü bildirimin türüdür.
Tür `var`ise, çıkarılan tür başlatılıyor ifadesinin türüdür. 

Bildirim atama ifadesi, belirli bir `ref` bağımsız değişken değeri için bir l değeri olabilir.

Bildirim atama ifadesi bir değer türü bildiriyorsa ve ifade bir r-değeri ise, ifadenin değeri bir kopyadır.

Bildirim atama ifadesi bir `ref` yerel olarak bildirebilir.
Bir `ref` bağımsız değişkeninde bildirim ifadesi için `ref` kullanıldığında bir belirsizlik vardır.
Yerel değişken başlatıcısı, bildirimin bir `ref` yerel olup olmadığını belirler.

```csharp
F(ref int x = IntFunc());    // int x;
F(ref int y = RefIntFunc()); // ref int y;
```

Bildirim atama ifadelerinde belirtilen Yereller kapsamı, C # 7.0 öğesine karşılık gelen bildirim ifadelerinin kapsamıdır.

Bu bir derleme zamanı hatası, bildirim ifadesinden önce gelen metinde yerel olarak ifade edilir.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives
Değişiklik yok. Bu özellik, yalnızca sözdizimsel bir toplu özelliktir.

Daha fazla genel dizi ifadesi: bkz. [#377](https://github.com/dotnet/csharplang/issues/377).

Daha fazla durumda `var` kullanımına izin vermek için, `var` Yereller için ayrı bildirim ve atamaya izin verin ve türü tüm kod yollarındaki atamalardan çıkarın.

## <a name="see-also"></a>Ayrıca bkz.
[see-also]: #see-also
Bkz. [#595](https://github.com/dotnet/csharplang/issues/595)temel bildirim ifadesi.

Bkz. [oluşturma](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) özelliği Içinde ayrıştırma bildirimi.
