---
ms.openlocfilehash: d2064ec1637e50962262c9380281abd5e1711402
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484582"
---
# <a name="declaration-expressions"></a><span data-ttu-id="3d3f5-101">Bildirim ifadeleri</span><span class="sxs-lookup"><span data-stu-id="3d3f5-101">Declaration expressions</span></span>

<span data-ttu-id="3d3f5-102">Deyim olarak bildirim atamalarını destekleme.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-102">Support declaration assignments as expressions.</span></span>

## <a name="motivation"></a><span data-ttu-id="3d3f5-103">Amacı</span><span class="sxs-lookup"><span data-stu-id="3d3f5-103">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="3d3f5-104">Daha fazla durumda, kodu basitleştirecek ve `var` kullanılmasına izin vererek bildirim noktasında başlatmaya izin verin.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-104">Allow initialization at the point of declaration in more cases, simplifying code, and allowing `var` to be used.</span></span>

```csharp
SpecialType ReferenceType =>
    (var st = _type.SpecialType).IsValueType() ? SpecialType.None : st;
```

<span data-ttu-id="3d3f5-105">`out var`benzer `ref` bağımsız değişkenler için bildirimlere izin verin.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-105">Allow declarations for `ref` arguments, similar to `out var`.</span></span>

```csharp
Convert(source, destination, ref List<Diagnostic> diagnostics = null);
```

## <a name="detailed-design"></a><span data-ttu-id="3d3f5-106">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="3d3f5-106">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="3d3f5-107">İfadeler, bildirim atamasını içerecek şekilde genişletilir.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-107">Expressions are extended to include declaration assignment.</span></span> <span data-ttu-id="3d3f5-108">Öncelik atama ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-108">Precedence is the same as assignment.</span></span>

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

<span data-ttu-id="3d3f5-109">Bildirim ataması tek bir yereldir.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-109">The declaration assignment is of a single local.</span></span>

<span data-ttu-id="3d3f5-110">Bildirim atama ifadesinin türü bildirimin türüdür.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-110">The type of a declaration assignment expression is the type of the declaration.</span></span>
<span data-ttu-id="3d3f5-111">Tür `var`ise, çıkarılan tür başlatılıyor ifadesinin türüdür.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-111">If the type is `var`, the inferred type is the type of the initializing expression.</span></span> 

<span data-ttu-id="3d3f5-112">Bildirim atama ifadesi, belirli bir `ref` bağımsız değişken değeri için bir l değeri olabilir.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-112">The declaration assignment expression may be an l-value, for `ref` argument values in particular.</span></span>

<span data-ttu-id="3d3f5-113">Bildirim atama ifadesi bir değer türü bildiriyorsa ve ifade bir r-değeri ise, ifadenin değeri bir kopyadır.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-113">If the declaration assignment expression declares a value type, and the expression is an r-value, the value of the expression is a copy.</span></span>

<span data-ttu-id="3d3f5-114">Bildirim atama ifadesi bir `ref` yerel olarak bildirebilir.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-114">The declaration assignment expression may declare a `ref` local.</span></span>
<span data-ttu-id="3d3f5-115">Bir `ref` bağımsız değişkeninde bildirim ifadesi için `ref` kullanıldığında bir belirsizlik vardır.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-115">There is an ambiguity when `ref` is used for a declaration expression in a `ref` argument.</span></span>
<span data-ttu-id="3d3f5-116">Yerel değişken başlatıcısı, bildirimin bir `ref` yerel olup olmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-116">The local variable initializer determines whether the declaration is a `ref` local.</span></span>

```csharp
F(ref int x = IntFunc());    // int x;
F(ref int y = RefIntFunc()); // ref int y;
```

<span data-ttu-id="3d3f5-117">Bildirim atama ifadelerinde belirtilen Yereller kapsamı, C # 7.0 öğesine karşılık gelen bildirim ifadelerinin kapsamıdır.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-117">The scope of locals declared in declaration assignment expressions is the same the scope of corresponding declaration expressions from C#7.0.</span></span>

<span data-ttu-id="3d3f5-118">Bu bir derleme zamanı hatası, bildirim ifadesinden önce gelen metinde yerel olarak ifade edilir.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-118">It is a compile time error to refer to a local in text preceding the declaration expression.</span></span>

## <a name="alternatives"></a><span data-ttu-id="3d3f5-119">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="3d3f5-119">Alternatives</span></span>
[alternatives]: #alternatives
<span data-ttu-id="3d3f5-120">Değişiklik yok.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-120">No change.</span></span> <span data-ttu-id="3d3f5-121">Bu özellik, yalnızca sözdizimsel bir toplu özelliktir.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-121">This feature is just syntactic shorthand after all.</span></span>

<span data-ttu-id="3d3f5-122">Daha fazla genel dizi ifadesi: bkz. [#377](https://github.com/dotnet/csharplang/issues/377).</span><span class="sxs-lookup"><span data-stu-id="3d3f5-122">More general sequence expressions: see [#377](https://github.com/dotnet/csharplang/issues/377).</span></span>

<span data-ttu-id="3d3f5-123">Daha fazla durumda `var` kullanımına izin vermek için, `var` Yereller için ayrı bildirim ve atamaya izin verin ve türü tüm kod yollarındaki atamalardan çıkarın.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-123">To allow use of `var` in more cases, allow separate declaration and assignment of `var` locals, and infer the type from assignments from all code paths.</span></span>

## <a name="see-also"></a><span data-ttu-id="3d3f5-124">Ayrıca bkz.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-124">See also</span></span>
[see-also]: #see-also
<span data-ttu-id="3d3f5-125">Bkz. [#595](https://github.com/dotnet/csharplang/issues/595)temel bildirim ifadesi.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-125">See Basic Declaration Expression in [#595](https://github.com/dotnet/csharplang/issues/595).</span></span>

<span data-ttu-id="3d3f5-126">Bkz. [oluşturma](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) özelliği Içinde ayrıştırma bildirimi.</span><span class="sxs-lookup"><span data-stu-id="3d3f5-126">See Deconstruction Declaration in the [deconstruction](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) feature.</span></span>
