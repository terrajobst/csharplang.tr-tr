---
ms.openlocfilehash: b8d975a8fc95af6980feaae6be35160646fe2cd2
ms.sourcegitcommit: 32abf01f2e43be29114bfcf8f8ed1cc4e3eaded2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2019
ms.locfileid: "79485142"
---
# <a name="permit-stackalloc-in-nested-contexts"></a><span data-ttu-id="e2b8e-101">İç içe bağlamlarda `stackalloc` izin ver</span><span class="sxs-lookup"><span data-stu-id="e2b8e-101">Permit `stackalloc` in nested contexts</span></span>

### <a name="stack-allocation"></a><span data-ttu-id="e2b8e-102">Yığın ayırma</span><span class="sxs-lookup"><span data-stu-id="e2b8e-102">Stack allocation</span></span>

<span data-ttu-id="e2b8e-103">Bir `stackalloc` ifadesi görünebilen yerleri rahat hale C# getirme için dil belirtiminin bölüm [*yığın ayırmayı*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation) değiştiririz.</span><span class="sxs-lookup"><span data-stu-id="e2b8e-103">We modify the section [*Stack allocation*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation) of the C# language specification to relax the places when a `stackalloc` expression may appear.</span></span> <span data-ttu-id="e2b8e-104">Sileriz</span><span class="sxs-lookup"><span data-stu-id="e2b8e-104">We delete</span></span>

``` antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="e2b8e-105">ve ile değiştirin</span><span class="sxs-lookup"><span data-stu-id="e2b8e-105">and replace them with</span></span>

``` antlr
primary_no_array_creation_expression
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression? ']' array_initializer?
    | 'stackalloc' '[' expression? ']' array_initializer
    ;
```

<span data-ttu-id="e2b8e-106">*Stackalloc_initializer* için bir *array_initializer* eklemenin (ve Dizin ifadesinin isteğe bağlı olarak) [7,3 C# ' de bir uzantı](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md) olduğunu ve burada açıklanmadığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="e2b8e-106">Note that the addition of an *array_initializer* to *stackalloc_initializer* (and making the index expression optional) was an [extension in C# 7.3](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md) and is not described here.</span></span>

<span data-ttu-id="e2b8e-107">`stackalloc` ifadesinin *öğe türü* , varsa stackalloc ifadesinde (varsa) veya *array_initializer* öğeleri arasında ortak tür olarak adlandırılan *unmanaged_type* .</span><span class="sxs-lookup"><span data-stu-id="e2b8e-107">The *element type* of the `stackalloc` expression is the *unmanaged_type* named in the stackalloc expression, if any, or the common type among the elements of the *array_initializer* otherwise.</span></span>

<span data-ttu-id="e2b8e-108">*Öğe türü* `K` *stackalloc_initializer* türü, sözdizimsel bağlamına bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="e2b8e-108">The type of the *stackalloc_initializer* with *element type* `K` depends on its syntactic context:</span></span>
- <span data-ttu-id="e2b8e-109">*Stackalloc_initializer* doğrudan bir *local_variable_declaration* deyimin veya bir *for_initializer* *local_variable_initializer* olarak görünürse, türü `K*`olur.</span><span class="sxs-lookup"><span data-stu-id="e2b8e-109">If the *stackalloc_initializer* appears directly as the *local_variable_initializer* of a *local_variable_declaration* statement or a *for_initializer*, then its type is `K*`.</span></span>
- <span data-ttu-id="e2b8e-110">Aksi takdirde, türü `System.Span<K>`.</span><span class="sxs-lookup"><span data-stu-id="e2b8e-110">Otherwise its type is `System.Span<K>`.</span></span>

### <a name="stackalloc-conversion"></a><span data-ttu-id="e2b8e-111">Stackalloc dönüştürme</span><span class="sxs-lookup"><span data-stu-id="e2b8e-111">Stackalloc Conversion</span></span>

<span data-ttu-id="e2b8e-112">*Stackalloc dönüşümü* , ifadeden yeni bir yerleşik örtük dönüşümtür.</span><span class="sxs-lookup"><span data-stu-id="e2b8e-112">The *stackalloc conversion* is a new built-in implicit conversion from expression.</span></span> <span data-ttu-id="e2b8e-113">*Stackalloc_initializer* türü `K*`olduğunda, *stackalloc_initializer* türüne `System.Span<K>`örtük bir *stackalloc dönüştürmesi* vardır.</span><span class="sxs-lookup"><span data-stu-id="e2b8e-113">When the type of a *stackalloc_initializer* is `K*`, there is an implicit *stackalloc conversion* from the *stackalloc_initializer* to the type `System.Span<K>`.</span></span>
