---
ms.openlocfilehash: b8d975a8fc95af6980feaae6be35160646fe2cd2
ms.sourcegitcommit: 32abf01f2e43be29114bfcf8f8ed1cc4e3eaded2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2019
ms.locfileid: "79485142"
---
# <a name="permit-stackalloc-in-nested-contexts"></a>İç içe bağlamlarda `stackalloc` izin ver

### <a name="stack-allocation"></a>Yığın ayırma

Bir `stackalloc` ifadesi görünebilen yerleri rahat hale C# getirme için dil belirtiminin bölüm [*yığın ayırmayı*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation) değiştiririz. Sileriz

``` antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

ve ile değiştirin

``` antlr
primary_no_array_creation_expression
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression? ']' array_initializer?
    | 'stackalloc' '[' expression? ']' array_initializer
    ;
```

*Stackalloc_initializer* için bir *array_initializer* eklemenin (ve Dizin ifadesinin isteğe bağlı olarak) [7,3 C# ' de bir uzantı](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md) olduğunu ve burada açıklanmadığını unutmayın.

`stackalloc` ifadesinin *öğe türü* , varsa stackalloc ifadesinde (varsa) veya *array_initializer* öğeleri arasında ortak tür olarak adlandırılan *unmanaged_type* .

*Öğe türü* `K` *stackalloc_initializer* türü, sözdizimsel bağlamına bağlıdır:
- *Stackalloc_initializer* doğrudan bir *local_variable_declaration* deyimin veya bir *for_initializer* *local_variable_initializer* olarak görünürse, türü `K*`olur.
- Aksi takdirde, türü `System.Span<K>`.

### <a name="stackalloc-conversion"></a>Stackalloc dönüştürme

*Stackalloc dönüşümü* , ifadeden yeni bir yerleşik örtük dönüşümtür. *Stackalloc_initializer* türü `K*`olduğunda, *stackalloc_initializer* türüne `System.Span<K>`örtük bir *stackalloc dönüştürmesi* vardır.
