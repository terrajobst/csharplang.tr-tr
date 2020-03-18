---
ms.openlocfilehash: a0d80afc47e9f0073237db9b8d7a4f0b045c1b0b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484512"
---
# <a name="out-variable-declarations"></a><span data-ttu-id="e778e-101">Out değişken bildirimleri</span><span class="sxs-lookup"><span data-stu-id="e778e-101">Out variable declarations</span></span>

<span data-ttu-id="e778e-102">*Out değişkeni bildirimi* özelliği, bir değişkenin `out` bağımsız değişken olarak geçirildiği konumda bildirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="e778e-102">The *out variable declaration* feature enables a variable to be declared at the location that it is being passed as an `out` argument.</span></span>

```antlr
argument_value
    : 'out' type identifier
    | ...
    ;
```

<span data-ttu-id="e778e-103">Bu şekilde bildirilmiştir bir değişken *Out değişkeni*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="e778e-103">A variable declared this way is called an *out variable*.</span></span> <span data-ttu-id="e778e-104">Değişken türü için `var` bağlamsal anahtar sözcüğünü kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e778e-104">You may use the contextual keyword `var` for the variable's type.</span></span> <span data-ttu-id="e778e-105">Kapsam, model eşleştirme ile tanıtılan bir *model değişkeni* ile aynı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="e778e-105">The scope will be the same as for a *pattern-variable* introduced via pattern-matching.</span></span>

<span data-ttu-id="e778e-106">Dil belirtimine göre (Bölüm 7.6.7 öğe erişimi) bir öğe erişimi (Dizin oluşturma ifadesi) bağımsız değişken listesi, ref veya out bağımsız değişkenleri içermez.</span><span class="sxs-lookup"><span data-stu-id="e778e-106">According to Language Specification (section 7.6.7 Element access) the argument-list of an element-access (indexing expression) does not contain ref or out arguments.</span></span> <span data-ttu-id="e778e-107">Ancak, çeşitli senaryolar için derleyici tarafından izin verilir, örneğin `out`kabul eden meta verilerde belirtilen dizin oluşturucular.</span><span class="sxs-lookup"><span data-stu-id="e778e-107">However, they are permitted by the compiler for various scenarios, for example indexers declared in metadata that accept `out`.</span></span>

<span data-ttu-id="e778e-108">Bir argument_value tarafından tanıtılan yerel bir değişken kapsamında, bu yerel değişkene, bildiriminden önce gelen metinsel bir konumda başvuruda bulunmak için derleme zamanı hatası vardır.</span><span class="sxs-lookup"><span data-stu-id="e778e-108">Within the scope of a local variable introduced by an argument_value, it is a compile-time error to refer to that local variable in a textual position that precedes its declaration.</span></span>

<span data-ttu-id="e778e-109">Ayrıca, kendi bildirimini içeren aynı bağımsız değişken listesinde örtük olarak yazılmış bir (§ 8.5.1) Out değişkenine başvurmak için de bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="e778e-109">It is also an error to reference an implicitly-typed (§8.5.1) out variable in the same argument list that immediately contains its declaration.</span></span>

<span data-ttu-id="e778e-110">Aşırı yükleme çözümlemesi aşağıdaki şekilde değiştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="e778e-110">Overload resolution is modified as follows:</span></span>

<span data-ttu-id="e778e-111">Yeni bir dönüştürme ekledik:</span><span class="sxs-lookup"><span data-stu-id="e778e-111">We add a new conversion:</span></span>

> <span data-ttu-id="e778e-112">Örtük olarak yazılmış bir değişken bildiriminden her türe bir *ifadeden dönüşüm* vardır.</span><span class="sxs-lookup"><span data-stu-id="e778e-112">There is a *conversion from expression* from an implicitly-typed out variable declaration to every type.</span></span>

<span data-ttu-id="e778e-113">Açabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="e778e-113">Also</span></span>

> <span data-ttu-id="e778e-114">Açıkça yazılmış bir out değişkeni bağımsız değişkeninin türü, belirtilen türdür.</span><span class="sxs-lookup"><span data-stu-id="e778e-114">The type of an explicitly-typed out variable argument is the declared type.</span></span>

<span data-ttu-id="e778e-115">ve</span><span class="sxs-lookup"><span data-stu-id="e778e-115">and</span></span>

> <span data-ttu-id="e778e-116">Örtük olarak yazılmış bir değişken bağımsız değişkeninin türü yok.</span><span class="sxs-lookup"><span data-stu-id="e778e-116">An implicitly-typed out variable argument has no type.</span></span>

<span data-ttu-id="e778e-117">Örtük olarak yazılmış bir değişken bildiriminden *ifadeden dönüştürme* , deyimden başka herhangi bir *dönüşümden*daha iyi düşünülmez.</span><span class="sxs-lookup"><span data-stu-id="e778e-117">The *conversion from expression* from an implicitly-typed out variable declaration is not considered better than any other *conversion from expression*.</span></span>

<span data-ttu-id="e778e-118">Örtük olarak yazılmış bir değişkenin türü, aşırı yükleme çözümlemesi tarafından seçilen yöntemin imzasında karşılık gelen parametrenin türüdür.</span><span class="sxs-lookup"><span data-stu-id="e778e-118">The type of an implicitly-typed out variable is the type of the corresponding parameter in the signature of the method selected by overload resolution.</span></span>

<span data-ttu-id="e778e-119">Yeni sözdizimi düğümü `DeclarationExpressionSyntax`, bildirimi bir out var bağımsız değişkeninde temsil edecek şekilde eklenir.</span><span class="sxs-lookup"><span data-stu-id="e778e-119">The new syntax node `DeclarationExpressionSyntax` is added to represent the declaration in an out var argument.</span></span>
