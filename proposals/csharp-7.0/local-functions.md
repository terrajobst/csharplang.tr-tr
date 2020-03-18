---
ms.openlocfilehash: a4b0fbbc600eaf1e705ad8e6bd9fcecb44100761
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484442"
---
# <a name="local-functions"></a><span data-ttu-id="79c6d-101">Yerel işlevler</span><span class="sxs-lookup"><span data-stu-id="79c6d-101">Local functions</span></span>

<span data-ttu-id="79c6d-102">Blok kapsamındaki C# işlevlerin bildirimini destekleyecek şekilde genişlettik.</span><span class="sxs-lookup"><span data-stu-id="79c6d-102">We extend C# to support the declaration of functions in block scope.</span></span> <span data-ttu-id="79c6d-103">Yerel işlevler kapsayan kapsamdaki değişkenleri kullanabilir (yakalayabilir).</span><span class="sxs-lookup"><span data-stu-id="79c6d-103">Local functions may use (capture) variables from the enclosing scope.</span></span>

<span data-ttu-id="79c6d-104">Derleyici, bir değeri atamadan önce yerel bir işlevin kullandığı değişkenleri algılamak için akış analizini kullanır.</span><span class="sxs-lookup"><span data-stu-id="79c6d-104">The compiler uses flow analysis to detect which variables a local function uses before assigning it a value.</span></span> <span data-ttu-id="79c6d-105">İşlevin her çağrısı, bu tür değişkenlerin kesinlikle atanmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="79c6d-105">Every call of the function requires such variables to be definitely assigned.</span></span> <span data-ttu-id="79c6d-106">Benzer şekilde, derleyici hangi değişkenlerin dönüşte kesin olarak atandığını belirler.</span><span class="sxs-lookup"><span data-stu-id="79c6d-106">Similarly the compiler determines which variables are definitely assigned on return.</span></span> <span data-ttu-id="79c6d-107">Bu tür değişkenler, yerel işlev çağrıldıktan sonra kesin olarak atanır olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="79c6d-107">Such variables are considered definitely assigned after the local function is invoked.</span></span>

<span data-ttu-id="79c6d-108">Yerel işlevler, tanımdan önce bir sözlü noktadan çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="79c6d-108">Local functions may be called from a lexical point before its definition.</span></span> <span data-ttu-id="79c6d-109">Yerel işlev bildirimi deyimleri, erişilebilir olmadığında bir uyarıya neden olmaz.</span><span class="sxs-lookup"><span data-stu-id="79c6d-109">Local function declaration statements do not cause a warning when they are not reachable.</span></span>

<span data-ttu-id="79c6d-110">TODO: _yazma özelliği_</span><span class="sxs-lookup"><span data-stu-id="79c6d-110">TODO: _WRITE SPEC_</span></span>

## <a name="syntax-grammar"></a><span data-ttu-id="79c6d-111">Sözdizimi dilbilgisi</span><span class="sxs-lookup"><span data-stu-id="79c6d-111">Syntax grammar</span></span>

<span data-ttu-id="79c6d-112">Bu dilbilgisi geçerli belirtim dilbilgisinde fark olarak temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="79c6d-112">This grammar is represented as a diff from the current spec grammar.</span></span>

```diff
declaration-statement
    : local-variable-declaration ';'
    | local-constant-declaration ';'
+   | local-function-declaration
    ;

+local-function-declaration
+   : local-function-header local-function-body
+   ;

+local-function-header
+   : local-function-modifiers? return-type identifier type-parameter-list?
+       ( formal-parameter-list? ) type-parameter-constraints-clauses
+   ;

+local-function-modifiers
+   : (async | unsafe)
+   ;

+local-function-body
+   : block
+   | arrow-expression-body
+   ;
```

<span data-ttu-id="79c6d-113">Yerel işlevler kapsayan kapsamda tanımlanan değişkenleri kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="79c6d-113">Local functions may use variables defined in the enclosing scope.</span></span> <span data-ttu-id="79c6d-114">Geçerli uygulama, yerel bir işlev içinde okunan her değişkenin, kendi tanım noktasında yürütülerek kesinlikle atanmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="79c6d-114">The current implementation requires that every variable read inside a local function be definitely assigned, as if executing the local function at its point of definition.</span></span> <span data-ttu-id="79c6d-115">Ayrıca, yerel işlev tanımının herhangi bir kullanım noktasında "yürütülmüş" olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="79c6d-115">Also, the local function definition must have been "executed" at any use point.</span></span>

<span data-ttu-id="79c6d-116">Bu bit ile denemeler yaptıktan sonra (örneğin, birbirini dışlayan iki özyinelemeli yerel işlev tanımlamak mümkün değildir), bu tarihten sonra kesin atamanın nasıl çalışmasını istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="79c6d-116">After experimenting with that a bit (for example, it is not possible to define two mutually recursive local functions), we've since revised how we want the definite assignment to work.</span></span> <span data-ttu-id="79c6d-117">Düzeltme (henüz uygulanmadı), yerel bir işlevde okunan tüm yerel değişkenlerin her yerel işlev çağrısında kesinlikle atanmasını sağlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="79c6d-117">The revision (not yet implemented) is that all local variables read in a local function must be definitely assigned at each invocation of the local function.</span></span> <span data-ttu-id="79c6d-118">Bu aslında BT seslerinden daha hafif olur ve çalışır duruma getirmek için bir miktar kalan iş vardır.</span><span class="sxs-lookup"><span data-stu-id="79c6d-118">That's actually more subtle than it sounds, and there is a bunch of work remaining to make it work.</span></span> <span data-ttu-id="79c6d-119">İşlem tamamlandıktan sonra, yerel işlevlerinizi kapsayan bloğunun sonuna taşıyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="79c6d-119">Once it is done you'll be able to move your local functions to the end of its enclosing block.</span></span>

<span data-ttu-id="79c6d-120">Yeni kesin atama kuralları, yerel bir işlevin dönüş türünü ele almasıyla uyumlu değildir, bu nedenle dönüş türünü destekleme desteğini kaldıracağız.</span><span class="sxs-lookup"><span data-stu-id="79c6d-120">The new definite assignment rules are incompatible with inferring the return type of a local function, so we'll likely be removing support for inferring the return type.</span></span>

<span data-ttu-id="79c6d-121">Yerel bir işlevi bir temsilciye dönüştürmediğiniz takdirde, yakalama değer türleri olan çerçevelere yapılır.</span><span class="sxs-lookup"><span data-stu-id="79c6d-121">Unless you convert a local function to a delegate, capturing is done into frames that are value types.</span></span> <span data-ttu-id="79c6d-122">Diğer bir deyişle, yakalama ile yerel işlevler kullanmanın herhangi bir GC basıncını edinmezsiniz.</span><span class="sxs-lookup"><span data-stu-id="79c6d-122">That means you don't get any GC pressure from using local functions with capturing.</span></span>

### <a name="reachability"></a><span data-ttu-id="79c6d-123">Erişilebilirlik</span><span class="sxs-lookup"><span data-stu-id="79c6d-123">Reachability</span></span>

<span data-ttu-id="79c6d-124">Spec 'e ekliyoruz</span><span class="sxs-lookup"><span data-stu-id="79c6d-124">We add to the spec</span></span>

> <span data-ttu-id="79c6d-125">Deyim-Bodied lambda ifadesinin veya yerel işlevin gövdesi erişilebilir olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="79c6d-125">The body of a statement-bodied lambda expression or local function is considered reachable.</span></span>
