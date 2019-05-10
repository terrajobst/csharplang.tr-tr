---
ms.openlocfilehash: 8f9551b9e7f70379836c23a60f0d37dc02f8e18e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488808"
---
# <a name="statements"></a><span data-ttu-id="3c16d-101">Deyimler</span><span class="sxs-lookup"><span data-stu-id="3c16d-101">Statements</span></span>

<span data-ttu-id="3c16d-102">C# ifadeleri çeşitli sağlar.</span><span class="sxs-lookup"><span data-stu-id="3c16d-102">C# provides a variety of statements.</span></span> <span data-ttu-id="3c16d-103">Bu deyimler çoğu C ve C++ içinde programlanmış geliştiriciler için tanıdık gelecektir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-103">Most of these statements will be familiar to developers who have programmed in C and C++.</span></span>

```antlr
statement
    : labeled_statement
    | declaration_statement
    | embedded_statement
    ;

embedded_statement
    : block
    | empty_statement
    | expression_statement
    | selection_statement
    | iteration_statement
    | jump_statement
    | try_statement
    | checked_statement
    | unchecked_statement
    | lock_statement
    | using_statement
    | yield_statement
    | embedded_statement_unsafe
    ;
```

<span data-ttu-id="3c16d-104">*Embedded_statement* bildirimlere diğer deyimleri içinde görünen deyimleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-104">The *embedded_statement* nonterminal is used for statements that appear within other statements.</span></span> <span data-ttu-id="3c16d-105">Kullanımını *embedded_statement* yerine *deyimi* bildirim deyimleri ve etiketli deyimler şu bağlamlarda dışlar.</span><span class="sxs-lookup"><span data-stu-id="3c16d-105">The use of *embedded_statement* rather than *statement* excludes the use of declaration statements and labeled statements in these contexts.</span></span> <span data-ttu-id="3c16d-106">Örnek</span><span class="sxs-lookup"><span data-stu-id="3c16d-106">The example</span></span>
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
<span data-ttu-id="3c16d-107">sonuçları bir derleme zamanı hatası nedeniyle bir `if` çalıştın ve bir *embedded_statement* yerine *deyimi* kendi IF için dal.</span><span class="sxs-lookup"><span data-stu-id="3c16d-107">results in a compile-time error because an `if` statement requires an *embedded_statement* rather than a *statement* for its if branch.</span></span> <span data-ttu-id="3c16d-108">Bu kod izin veriyorsa, ardından değişken `i` bildirilen, ancak hiçbir zaman kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-108">If this code were permitted, then the variable `i` would be declared, but it could never be used.</span></span> <span data-ttu-id="3c16d-109">Ancak, unutmayın, yerleştirerek `i`ait bildirimi bir blok içinde örnek geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-109">Note, however, that by placing `i`'s declaration in a block, the example is valid.</span></span>

## <a name="end-points-and-reachability"></a><span data-ttu-id="3c16d-110">Uç noktaları ve erişilebilirlik</span><span class="sxs-lookup"><span data-stu-id="3c16d-110">End points and reachability</span></span>

<span data-ttu-id="3c16d-111">Her deyim sahip bir ***uç noktası***.</span><span class="sxs-lookup"><span data-stu-id="3c16d-111">Every statement has an ***end point***.</span></span> <span data-ttu-id="3c16d-112">Sezgisel bağlamında, bir deyim bitiş noktasını deyim hemen izleyen konumdur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-112">In intuitive terms, the end point of a statement is the location that immediately follows the statement.</span></span> <span data-ttu-id="3c16d-113">Bileşik deyimler (katıştırılmış deyimleri içeren ifadeler) yürütme kuralları denetimi bir katıştırılmış deyim bitiş noktasını ulaştığında alınmış eylemi belirtin.</span><span class="sxs-lookup"><span data-stu-id="3c16d-113">The execution rules for composite statements (statements that contain embedded statements) specify the action that is taken when control reaches the end point of an embedded statement.</span></span> <span data-ttu-id="3c16d-114">Örneğin, denetim bir blok içinde bir ifade bitiş noktasını ulaştığında, denetim bloğunda sonraki deyime aktarılır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-114">For example, when control reaches the end point of a statement in a block, control is transferred to the next statement in the block.</span></span>

<span data-ttu-id="3c16d-115">Bir deyimi yürütme büyük olasılıkla ulaşılabilirse, deyim olduğu söylenir ***erişilebilir***.</span><span class="sxs-lookup"><span data-stu-id="3c16d-115">If a statement can possibly be reached by execution, the statement is said to be ***reachable***.</span></span> <span data-ttu-id="3c16d-116">Bir deyimi yürütülür olasılığı varsa, buna karşılık, deyim olduğu söylenir ***ulaşılamaz***.</span><span class="sxs-lookup"><span data-stu-id="3c16d-116">Conversely, if there is no possibility that a statement will be executed, the statement is said to be ***unreachable***.</span></span>

<span data-ttu-id="3c16d-117">Örnekte</span><span class="sxs-lookup"><span data-stu-id="3c16d-117">In the example</span></span>
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
<span data-ttu-id="3c16d-118">İkinci çağırmayı `Console.WriteLine` deyimi yürütülür olasılığı olduğundan erişilemiyor.</span><span class="sxs-lookup"><span data-stu-id="3c16d-118">the second invocation of `Console.WriteLine` is unreachable because there is no possibility that the statement will be executed.</span></span>

<span data-ttu-id="3c16d-119">Derleyici bir deyim ulaşılamaz olduğunu belirlerse, bir uyarı bildirilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-119">A warning is reported if the compiler determines that a statement is unreachable.</span></span> <span data-ttu-id="3c16d-120">Özellikle değil ulaşılamaz olarak bir deyim için hatadır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-120">It is specifically not an error for a statement to be unreachable.</span></span>

<span data-ttu-id="3c16d-121">Belirli bir deyimi veya uç noktası erişilebilir olup olmadığını belirlemek için akış analizi for each deyimi tanımlanan ulaşılabilirlik kurallara göre derleyici gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-121">To determine whether a particular statement or end point is reachable, the compiler performs flow analysis according to the reachability rules defined for each statement.</span></span> <span data-ttu-id="3c16d-122">Akış analizi, sabit ifadeler değerlerini dikkate alır ([sabit ifadeler](expressions.md#constant-expressions)) deyimleri davranışını kontrol eden, ancak sabit olmayan ifade olası değerlerin değil olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-122">The flow analysis takes into account the values of constant expressions ([Constant expressions](expressions.md#constant-expressions)) that control the behavior of statements, but the possible values of non-constant expressions are not considered.</span></span> <span data-ttu-id="3c16d-123">Diğer bir deyişle, denetim akışı analizi amacıyla herhangi bir olası değer türü olması, belirli bir türde bir sabit olmayan ifade değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-123">In other words, for purposes of control flow analysis, a non-constant expression of a given type is considered to have any possible value of that type.</span></span>

<span data-ttu-id="3c16d-124">Örnekte</span><span class="sxs-lookup"><span data-stu-id="3c16d-124">In the example</span></span>
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
<span data-ttu-id="3c16d-125">Boole ifadesi `if` deyimi olduğundan sabit bir ifade iki işleneni de `==` işleci değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-125">the boolean expression of the `if` statement is a constant expression because both operands of the `==` operator are constants.</span></span> <span data-ttu-id="3c16d-126">Derleme zamanında sabit ifade değerlendirmesinde gibi değer üreten `false`, `Console.WriteLine` çağırma ulaşılamaz olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-126">As the constant expression is evaluated at compile-time, producing the value `false`, the `Console.WriteLine` invocation is considered unreachable.</span></span> <span data-ttu-id="3c16d-127">Ancak, varsa `i` yerel bir değişken olarak değiştirilir</span><span class="sxs-lookup"><span data-stu-id="3c16d-127">However, if `i` is changed to be a local variable</span></span>
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
<span data-ttu-id="3c16d-128">`Console.WriteLine` gerçekte hiçbir zaman yürütülür olsa da, çağırma erişilebilir, değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-128">the `Console.WriteLine` invocation is considered reachable, even though, in reality, it will never be executed.</span></span>

<span data-ttu-id="3c16d-129">*Blok* işlevi üye her zaman erişilebilir olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-129">The *block* of a function member is always considered reachable.</span></span> <span data-ttu-id="3c16d-130">Her deyim bloğunda ulaşılabilirlik kurallarına sırayla değerlendirerek, herhangi bir deyimle erişilebilirliğini belirlenebilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-130">By successively evaluating the reachability rules of each statement in a block, the reachability of any given statement can be determined.</span></span>

<span data-ttu-id="3c16d-131">Örnekte</span><span class="sxs-lookup"><span data-stu-id="3c16d-131">In the example</span></span>
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
<span data-ttu-id="3c16d-132">İkinci erişilebilirliğini `Console.WriteLine` şu şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-132">the reachability of the second `Console.WriteLine` is determined as follows:</span></span>

*  <span data-ttu-id="3c16d-133">İlk `Console.WriteLine` ifade deyimi erişilebilir olduğundan bloğunu `F` yöntemi erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-133">The first `Console.WriteLine` expression statement is reachable because the block of the `F` method is reachable.</span></span>
*  <span data-ttu-id="3c16d-134">İlk uç noktası `Console.WriteLine` ifade deyimi, o ifadeyi erişilebilir olduğu için erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-134">The end point of the first `Console.WriteLine` expression statement is reachable because that statement is reachable.</span></span>
*  <span data-ttu-id="3c16d-135">`if` Deyimi, ilk uç noktası için erişilebilir `Console.WriteLine` ifade deyimi erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-135">The `if` statement is reachable because the end point of the first `Console.WriteLine` expression statement is reachable.</span></span>
*  <span data-ttu-id="3c16d-136">İkinci `Console.WriteLine` ifade deyimi erişilebilir olduğundan boolean ifadesi `if` ifadesi sabit değer yok `false`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-136">The second `Console.WriteLine` expression statement is reachable because the boolean expression of the `if` statement does not have the constant value `false`.</span></span>

<span data-ttu-id="3c16d-137">Bir derleme zamanı hatası erişilebilir olması için tablo uç noktası için olan iki durum vardır:</span><span class="sxs-lookup"><span data-stu-id="3c16d-137">There are two situations in which it is a compile-time error for the end point of a statement to be reachable:</span></span>

*  <span data-ttu-id="3c16d-138">Çünkü `switch` deyimi "geçiş için" anahtar bölüm sonraki switch bölümüne vermez, erişilebilir olması için bir anahtar bölümü deyim listesinin uç noktası için bir derleme zamanı hata.</span><span class="sxs-lookup"><span data-stu-id="3c16d-138">Because the `switch` statement does not permit a switch section to "fall through" to the next switch section, it is a compile-time error for the end point of the statement list of a switch section to be reachable.</span></span> <span data-ttu-id="3c16d-139">Bu hata meydana gelirse, genellikle bir göstergesi olduğu, bir `break` deyimi eksik.</span><span class="sxs-lookup"><span data-stu-id="3c16d-139">If this error occurs, it is typically an indication that a `break` statement is missing.</span></span>
*  <span data-ttu-id="3c16d-140">Bloğun erişilebilir olması için bir değer hesaplayan bir işlevi üyesinin uç noktası için bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-140">It is a compile-time error for the end point of the block of a function member that computes a value to be reachable.</span></span> <span data-ttu-id="3c16d-141">Bu hata meydana gelirse, genellikle bir göstergesi olduğu, bir `return` deyimi eksik.</span><span class="sxs-lookup"><span data-stu-id="3c16d-141">If this error occurs, it typically is an indication that a `return` statement is missing.</span></span>

## <a name="blocks"></a><span data-ttu-id="3c16d-142">Bloklar</span><span class="sxs-lookup"><span data-stu-id="3c16d-142">Blocks</span></span>

<span data-ttu-id="3c16d-143">A *blok* bağlamlarda yazılacak birden çok deyime burada tek bir deyimde izin verir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-143">A *block* permits multiple statements to be written in contexts where a single statement is allowed.</span></span>

```antlr
block
    : '{' statement_list? '}'
    ;
```

<span data-ttu-id="3c16d-144">A *blok* isteğe oluşur *statement_list* ([deyimi listeler](statements.md#statement-lists)) küme ayraçları içine alınan.</span><span class="sxs-lookup"><span data-stu-id="3c16d-144">A *block* consists of an optional *statement_list* ([Statement lists](statements.md#statement-lists)), enclosed in braces.</span></span> <span data-ttu-id="3c16d-145">İfade listesinin atlanırsa, blok boş olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-145">If the statement list is omitted, the block is said to be empty.</span></span>

<span data-ttu-id="3c16d-146">Bildirim deyimleri bir blok içerebilir ([bildirim deyimleri](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="3c16d-146">A block may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="3c16d-147">Bir yerel değişken veya sabit bir blokta bildirilen blok kapsamıdır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-147">The scope of a local variable or constant declared in a block is the block.</span></span>

<span data-ttu-id="3c16d-148">Bir blok gibi yürütülür:</span><span class="sxs-lookup"><span data-stu-id="3c16d-148">A block is executed as follows:</span></span>

*  <span data-ttu-id="3c16d-149">Blok boşsa, denetimi bloğun uç noktasına aktarılır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-149">If the block is empty, control is transferred to the end point of the block.</span></span>
*  <span data-ttu-id="3c16d-150">Blok boş değilse, Denetim deyimi listeye aktarılır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-150">If the block is not empty, control is transferred to the statement list.</span></span> <span data-ttu-id="3c16d-151">Zaman ve denetim deyim listesinin bitiş noktasını ulaşırsa, denetimi bloğun uç noktasına aktarılır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-151">When and if control reaches the end point of the statement list, control is transferred to the end point of the block.</span></span>

<span data-ttu-id="3c16d-152">Deyimi bir blok blok ulaşılıp ulaşılamadığını erişilebilir listesidir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-152">The statement list of a block is reachable if the block itself is reachable.</span></span>

<span data-ttu-id="3c16d-153">Uç noktası bir bloğun blok boş ise veya deyim listesinin bitiş noktasını ulaşılıp ulaşılamadığını erişilebiliyor.</span><span class="sxs-lookup"><span data-stu-id="3c16d-153">The end point of a block is reachable if the block is empty or if the end point of the statement list is reachable.</span></span>

<span data-ttu-id="3c16d-154">A *blok* bir veya daha fazla bilgi içeren `yield` deyimleri ([yield deyimi](statements.md#the-yield-statement)) yineleyici bloğu çağrılır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-154">A *block* that contains one or more `yield` statements ([The yield statement](statements.md#the-yield-statement)) is called an iterator block.</span></span> <span data-ttu-id="3c16d-155">Yineleyici bloğu yineleyiciler olarak işlev üyeleri uygulamak için kullanılır ([yineleyiciler](classes.md#iterators)).</span><span class="sxs-lookup"><span data-stu-id="3c16d-155">Iterator blocks are used to implement function members as iterators ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="3c16d-156">Yineleyici bloğu için bazı ek sınırlamalar:</span><span class="sxs-lookup"><span data-stu-id="3c16d-156">Some additional restrictions apply to iterator blocks:</span></span>

*  <span data-ttu-id="3c16d-157">Bir derleme zamanı hata için bir `return` yineleyici bloğu içinde görüntülenecek deyimi (ancak `yield return` deyimleri izin verilir).</span><span class="sxs-lookup"><span data-stu-id="3c16d-157">It is a compile-time error for a `return` statement to appear in an iterator block (but `yield return` statements are permitted).</span></span>
*  <span data-ttu-id="3c16d-158">Güvenli olmayan bir bağlam içerecek şekilde yineleyici bloğu için bir derleme zamanı hata ([güvensiz bir bağlamı](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="3c16d-158">It is a compile-time error for an iterator block to contain an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="3c16d-159">Bile bildiriminden güvenli olmayan bir bağlamda yer alan bir yineleyici bloğu her zaman güvenli bir bağlamı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="3c16d-159">An iterator block always defines a safe context, even when its declaration is nested in an unsafe context.</span></span>

### <a name="statement-lists"></a><span data-ttu-id="3c16d-160">Deyimi listeler</span><span class="sxs-lookup"><span data-stu-id="3c16d-160">Statement lists</span></span>

<span data-ttu-id="3c16d-161">A ***deyim listesinin*** sırada yazılmasını bir veya daha fazla deyim içerir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-161">A ***statement list*** consists of one or more statements written in sequence.</span></span> <span data-ttu-id="3c16d-162">Deyimi listeleri gerçekleştirilir *blok*s ([blokları](statements.md#blocks)) ve *switch_block*s ([switch deyimi](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="3c16d-162">Statement lists occur in *block*s ([Blocks](statements.md#blocks)) and in *switch_block*s ([The switch statement](statements.md#the-switch-statement)).</span></span>

```antlr
statement_list
    : statement+
    ;
```

<span data-ttu-id="3c16d-163">İfade listesinin denetim ilk deyime aktarılması yoluyla yürütülür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-163">A statement list is executed by transferring control to the first statement.</span></span> <span data-ttu-id="3c16d-164">Zaman ve Denetim deyiminin uç noktası ulaşırsa, Denetim sonraki deyime aktarılır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-164">When and if control reaches the end point of a statement, control is transferred to the next statement.</span></span> <span data-ttu-id="3c16d-165">Zaman ve denetim son deyim bitiş noktasını ulaşırsa, Denetim deyim listesinin son noktasına aktarılır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-165">When and if control reaches the end point of the last statement, control is transferred to the end point of the statement list.</span></span>

<span data-ttu-id="3c16d-166">Deyim listesinin deyiminde aşağıdakilerden en az biri doğruysa erişilebilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-166">A statement in a statement list is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="3c16d-167">Deyim ilk deyim ve deyim listesinin erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-167">The statement is the first statement and the statement list itself is reachable.</span></span>
*  <span data-ttu-id="3c16d-168">Önceki deyim bitiş noktasının erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-168">The end point of the preceding statement is reachable.</span></span>
*  <span data-ttu-id="3c16d-169">Etiketli deyim ifadesi ve etiket tarafından erişilebilir başvurulan `goto` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-169">The statement is a labeled statement and the label is referenced by a reachable `goto` statement.</span></span>

<span data-ttu-id="3c16d-170">Listedeki son deyim bitiş noktasını ulaşılıp ulaşılamadığını deyim listesinin bitiş noktasını erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-170">The end point of a statement list is reachable if the end point of the last statement in the list is reachable.</span></span>

## <a name="the-empty-statement"></a><span data-ttu-id="3c16d-171">Boş deyim</span><span class="sxs-lookup"><span data-stu-id="3c16d-171">The empty statement</span></span>

<span data-ttu-id="3c16d-172">Bir *empty_statement* hiçbir şey yapmaz.</span><span class="sxs-lookup"><span data-stu-id="3c16d-172">An *empty_statement* does nothing.</span></span>

```antlr
empty_statement
    : ';'
    ;
```

<span data-ttu-id="3c16d-173">Olduğunda bir bağlamda gerçekleştirmek için bir işlem bir deyim gerekli olduğu boş bir deyimi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-173">An empty statement is used when there are no operations to perform in a context where a statement is required.</span></span>

<span data-ttu-id="3c16d-174">Boş bir deyimi yürütme denetimi deyiminin uç noktasına yalnızca aktarır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-174">Execution of an empty statement simply transfers control to the end point of the statement.</span></span> <span data-ttu-id="3c16d-175">Bu nedenle, boş bir deyimi bitiş noktası boş deyim ulaşılıp ulaşılamadığını erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-175">Thus, the end point of an empty statement is reachable if the empty statement is reachable.</span></span>

<span data-ttu-id="3c16d-176">Boş bir deyimi yazılırken kullanılan bir `while` boş gövdesi olan deyimi:</span><span class="sxs-lookup"><span data-stu-id="3c16d-176">An empty statement can be used when writing a `while` statement with a null body:</span></span>
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

<span data-ttu-id="3c16d-177">Ayrıca, boş bir deyimi yalnızca kapatmadan önce bir etiket bildirmek için kullanılabilir "`}`" bir blok:</span><span class="sxs-lookup"><span data-stu-id="3c16d-177">Also, an empty statement can be used to declare a label just before the closing "`}`" of a block:</span></span>
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a><span data-ttu-id="3c16d-178">Etiketli deyimler</span><span class="sxs-lookup"><span data-stu-id="3c16d-178">Labeled statements</span></span>

<span data-ttu-id="3c16d-179">A *labeled_statement* bir ifade tarafından bir etiket öneki verir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-179">A *labeled_statement* permits a statement to be prefixed by a label.</span></span> <span data-ttu-id="3c16d-180">Etiketli deyimler bloklarında izin verilir, ancak katıştırılmış deyimler izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="3c16d-180">Labeled statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

<span data-ttu-id="3c16d-181">Etiketli deyim tarafından verilen ada sahip bir etiket bildirir *tanımlayıcı*.</span><span class="sxs-lookup"><span data-stu-id="3c16d-181">A labeled statement declares a label with the name given by the *identifier*.</span></span> <span data-ttu-id="3c16d-182">Tüm bloğu kapsamdır bir etiketin etiket bildirildiği içinde dahil iç içe bloklarınız.</span><span class="sxs-lookup"><span data-stu-id="3c16d-182">The scope of a label is the whole block in which the label is declared, including any nested blocks.</span></span> <span data-ttu-id="3c16d-183">Çakışan bir kapsam için aynı ada sahip iki etiket için bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-183">It is a compile-time error for two labels with the same name to have overlapping scopes.</span></span>

<span data-ttu-id="3c16d-184">Bir etiket başvurulabilir `goto` deyimleri ([goto deyimi](statements.md#the-goto-statement)) etiketin kapsamı içinde.</span><span class="sxs-lookup"><span data-stu-id="3c16d-184">A label can be referenced from `goto` statements ([The goto statement](statements.md#the-goto-statement)) within the scope of the label.</span></span> <span data-ttu-id="3c16d-185">Diğer bir deyişle `goto` deyimleri, denetim blokları içinde ve dışında blokları, ancak hiçbir zaman bloklara aktarabilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-185">This means that `goto` statements can transfer control within blocks and out of blocks, but never into blocks.</span></span>

<span data-ttu-id="3c16d-186">Etiketler, kendi bildirim alanına sahip ve diğer tanımlayıcıları ile müdahale etmez.</span><span class="sxs-lookup"><span data-stu-id="3c16d-186">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="3c16d-187">Örnek</span><span class="sxs-lookup"><span data-stu-id="3c16d-187">The example</span></span>
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
<span data-ttu-id="3c16d-188">geçerli olduğundan ve bu adı kullanan `x` hem bir parametre hem de bir etiket olarak.</span><span class="sxs-lookup"><span data-stu-id="3c16d-188">is valid and uses the name `x` as both a parameter and a label.</span></span>

<span data-ttu-id="3c16d-189">Etiketli deyim yürütülmesini etiketi takiben deyime sahip yürütme için tam olarak karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-189">Execution of a labeled statement corresponds exactly to execution of the statement following the label.</span></span>

<span data-ttu-id="3c16d-190">Normal denetim akışını tarafından sağlanan erişilebilirlik ek olarak etiketli bir deyim etiketi tarafından erişilebilir başvuruyorsa ulaşılabildiğinden `goto` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-190">In addition to the reachability provided by normal flow of control, a labeled statement is reachable if the label is referenced by a reachable `goto` statement.</span></span> <span data-ttu-id="3c16d-191">(Özel durum: Varsa bir `goto` deyimdir içinde bir `try` içeren bir `finally` blok ve etiketli deyim olduğu dışında `try`ve bitiş noktasını `finally` blok erişilemez ve etiketli deyim öğesinden erişilebilir değil `goto` deyimi.)</span><span class="sxs-lookup"><span data-stu-id="3c16d-191">(Exception: If a `goto` statement is inside a `try` that includes a `finally` block, and the labeled statement is outside the `try`, and the end point of the `finally` block is unreachable, then the labeled statement is not reachable from that `goto` statement.)</span></span>

## <a name="declaration-statements"></a><span data-ttu-id="3c16d-192">Bildirim deyimleri</span><span class="sxs-lookup"><span data-stu-id="3c16d-192">Declaration statements</span></span>

<span data-ttu-id="3c16d-193">A *declaration_statement* bir yerel değişken veya sabiti bildirir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-193">A *declaration_statement* declares a local variable or constant.</span></span> <span data-ttu-id="3c16d-194">Bildirim deyimleri bloklarında izin verilir, ancak katıştırılmış deyimler izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="3c16d-194">Declaration statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a><span data-ttu-id="3c16d-195">Yerel değişken bildirimleri</span><span class="sxs-lookup"><span data-stu-id="3c16d-195">Local variable declarations</span></span>

<span data-ttu-id="3c16d-196">A *local_variable_declaration* veya daha fazla yerel değişken bildirir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-196">A *local_variable_declaration* declares one or more local variables.</span></span>

```antlr
local_variable_declaration
    : local_variable_type local_variable_declarators
    ;

local_variable_type
    : type
    | 'var'
    ;

local_variable_declarators
    : local_variable_declarator
    | local_variable_declarators ',' local_variable_declarator
    ;

local_variable_declarator
    : identifier
    | identifier '=' local_variable_initializer
    ;

local_variable_initializer
    : expression
    | array_initializer
    | local_variable_initializer_unsafe
    ;
```

<span data-ttu-id="3c16d-197">*Local_variable_type* , bir *local_variable_declaration* doğrudan bildirim tarafından tanıtılan değişkenler türünü belirtir veya tanımlayıcısıyla gösterir `var` , bir başlatıcı üzerinde temel türü çıkarılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-197">The *local_variable_type* of a *local_variable_declaration* either directly specifies the type of the variables introduced by the declaration, or indicates with the identifier `var` that the type should be inferred based on an initializer.</span></span> <span data-ttu-id="3c16d-198">Tür listesi tarafından izlenir *local_variable_declarator*s, her biri yeni bir değişken tanıtır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-198">The type is followed by a list of *local_variable_declarator*s, each of which introduces a new variable.</span></span> <span data-ttu-id="3c16d-199">A *local_variable_declarator* oluşan bir *tanımlayıcı* ardından isteğe bağlı olarak, değişken adları bir "`=`" belirteci ve *local_variable_initializer* değişkenin başlangıç değerini vermektedir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-199">A *local_variable_declarator* consists of an *identifier* that names the variable, optionally followed by an "`=`" token and a *local_variable_initializer* that gives the initial value of the variable.</span></span>

<span data-ttu-id="3c16d-200">Bir yerel değişken bildiriminde bağlamında tanımlayıcı var bağlamsal anahtar sözcük işlevi görür. ([anahtar sözcükleri](lexical-structure.md#keywords)). Zaman *local_variable_type* olarak belirtilen `var` ve adlandırılmış tür `var` olduğu bildirimi kapsam içinde olan bir ***yerel değişken bildiriminde örtük***, türü olan ilişkili Başlatıcı ifadesinin türünden çıkarsanan.</span><span class="sxs-lookup"><span data-stu-id="3c16d-200">In the context of a local variable declaration, the identifier var acts as a contextual keyword ([Keywords](lexical-structure.md#keywords)).When the *local_variable_type* is specified as `var` and no type named `var` is in scope, the declaration is an ***implicitly typed local variable declaration***, whose type is inferred from the type of the associated initializer expression.</span></span> <span data-ttu-id="3c16d-201">Türü örtük olarak belirlenmiş yerel değişken bildirimlerini aşağıdaki kısıtlamalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="3c16d-201">Implicitly typed local variable declarations are subject to the following restrictions:</span></span>

*  <span data-ttu-id="3c16d-202">*Local_variable_declaration* birden çok içeremez *local_variable_declarator*s.</span><span class="sxs-lookup"><span data-stu-id="3c16d-202">The *local_variable_declaration* cannot include multiple *local_variable_declarator*s.</span></span>
*  <span data-ttu-id="3c16d-203">*Local_variable_declarator* içermelidir bir *local_variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="3c16d-203">The *local_variable_declarator* must include a *local_variable_initializer*.</span></span>
*  <span data-ttu-id="3c16d-204">*Local_variable_initializer* olmalıdır bir *ifade*.</span><span class="sxs-lookup"><span data-stu-id="3c16d-204">The *local_variable_initializer* must be an *expression*.</span></span>
*  <span data-ttu-id="3c16d-205">Başlatıcı *ifade* bir derleme zamanı türü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-205">The initializer *expression* must have a compile-time type.</span></span>
*  <span data-ttu-id="3c16d-206">Başlatıcı *ifade* bildirilmiş bir değişken için kendisine başvuramaz</span><span class="sxs-lookup"><span data-stu-id="3c16d-206">The initializer *expression* cannot refer to the declared variable itself</span></span>

<span data-ttu-id="3c16d-207">Yanlış türü örtük olarak belirlenmiş yerel değişken tanımlama örnekleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="3c16d-207">The following are examples of incorrect implicitly typed local variable declarations:</span></span>

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

<span data-ttu-id="3c16d-208">Yerel bir değişken değerini bir ifade kullanılarak elde edilir bir *simple_name* ([basit adları](expressions.md#simple-names)), ve kullanarak yerel bir değişken değerini değiştiren bir *atama* () [Atama işleçleri](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="3c16d-208">The value of a local variable is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)), and the value of a local variable is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="3c16d-209">Yerel bir değişken kesinlikle atanmalıdır ([belirli atama onayına](variables.md#definite-assignment)) her bir konumdaki değeri elde burada.</span><span class="sxs-lookup"><span data-stu-id="3c16d-209">A local variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at each location where its value is obtained.</span></span>

<span data-ttu-id="3c16d-210">Bildirilen yerel değişken kapsamını bir *local_variable_declaration* blok bildirimi oluştuğu içinde.</span><span class="sxs-lookup"><span data-stu-id="3c16d-210">The scope of a local variable declared in a *local_variable_declaration* is the block in which the declaration occurs.</span></span> <span data-ttu-id="3c16d-211">Önündeki değerinin metinsel bir konumda yerel bir değişkene başvurmak için bir hata olduğunu *local_variable_declarator* yerel değişkenin.</span><span class="sxs-lookup"><span data-stu-id="3c16d-211">It is an error to refer to a local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="3c16d-212">Yerel bir değişken kapsamında buna başka bir yerel değişken ya da aynı ada sahip sabit bildirmek için bir derleme zamanı hatası var.</span><span class="sxs-lookup"><span data-stu-id="3c16d-212">Within the scope of a local variable, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="3c16d-213">Birden fazla değişken bildiren bir yerel değişken bildiriminde tek değişkenlerin birden çok bildirimleri aynı türe ile eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-213">A local variable declaration that declares multiple variables is equivalent to multiple declarations of single variables with the same type.</span></span> <span data-ttu-id="3c16d-214">Ayrıca, yerel bir değişken bildirimi bir değişken başlatıcısında bildiriminden hemen sonra eklenen atama deyiminin tam olarak karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-214">Furthermore, a variable initializer in a local variable declaration corresponds exactly to an assignment statement that is inserted immediately after the declaration.</span></span>

<span data-ttu-id="3c16d-215">Örnek</span><span class="sxs-lookup"><span data-stu-id="3c16d-215">The example</span></span>
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
<span data-ttu-id="3c16d-216">tam olarak karşılık geldiğinden</span><span class="sxs-lookup"><span data-stu-id="3c16d-216">corresponds exactly to</span></span>
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

<span data-ttu-id="3c16d-217">Bir türü örtük olarak belirlenmiş yerel değişken bildiriminde bildirilen yerel değişken türünü değişkeni başlatmak için kullanılan ifade türü ile aynı olacak şekilde alınır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-217">In an implicitly typed local variable declaration, the type of the local variable being declared is taken to be the same as the type of the expression used to initialize the variable.</span></span> <span data-ttu-id="3c16d-218">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3c16d-218">For example:</span></span>
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

<span data-ttu-id="3c16d-219">Türü örtük olarak belirlenmiş yerel değişken bildirimlerini yukarıdaki aşağıdaki açıkça belirlenmiş bildirimleri için tam olarak eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-219">The implicitly typed local variable declarations above are precisely equivalent to the following explicitly typed declarations:</span></span>
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a><span data-ttu-id="3c16d-220">Yerel sabit bildirimleri</span><span class="sxs-lookup"><span data-stu-id="3c16d-220">Local constant declarations</span></span>

<span data-ttu-id="3c16d-221">A *local_constant_declaration* bir veya daha fazla yerel sabit bildirir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-221">A *local_constant_declaration* declares one or more local constants.</span></span>

```antlr
local_constant_declaration
    : 'const' type constant_declarators
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

<span data-ttu-id="3c16d-222">*Türü* , bir *local_constant_declaration* bildirim tarafından tanıtılan sabitleri türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-222">The *type* of a *local_constant_declaration* specifies the type of the constants introduced by the declaration.</span></span> <span data-ttu-id="3c16d-223">Tür listesi tarafından izlenir *constant_declarator*s, her biri yeni bir sabit tanıtır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-223">The type is followed by a list of *constant_declarator*s, each of which introduces a new constant.</span></span> <span data-ttu-id="3c16d-224">A *constant_declarator* oluşan bir *tanımlayıcı* sabiti adlarının arkasından bir "`=`" belirteci ve ardından bir *constant_expression* ([ Sabit ifadeler](expressions.md#constant-expressions)), sabit değerini verir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-224">A *constant_declarator* consists of an *identifier* that names the constant, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the constant.</span></span>

<span data-ttu-id="3c16d-225">*Türü* ve *constant_expression* yerel sabit bir bildirimde sabit üye bildirimi aynı kuralları izlemelidir ([sabitleri](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="3c16d-225">The *type* and *constant_expression* of a local constant declaration must follow the same rules as those of a constant member declaration ([Constants](classes.md#constants)).</span></span>

<span data-ttu-id="3c16d-226">Bir yerel sabit değeri bir ifade kullanarak elde edilen bir *simple_name* ([basit adları](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="3c16d-226">The value of a local constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="3c16d-227">Bir yerel sabit kapsamını bloğudur bildirimi oluştuğu içinde.</span><span class="sxs-lookup"><span data-stu-id="3c16d-227">The scope of a local constant is the block in which the declaration occurs.</span></span> <span data-ttu-id="3c16d-228">Önündeki değerinin metinsel bir konumda yerel bir sabit belirtmek için bir hata olduğunu, *constant_declarator*.</span><span class="sxs-lookup"><span data-stu-id="3c16d-228">It is an error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span> <span data-ttu-id="3c16d-229">Yerel sabit kapsamında, başka bir yerel değişken veya aynı ada sahip bir sabit bildirme bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-229">Within the scope of a local constant, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="3c16d-230">Birden fazla sabit bildiren bir yerel sabit bildiriminde tek sabitlerin aynı türde birden fazla bildirimi eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-230">A local constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same type.</span></span>

## <a name="expression-statements"></a><span data-ttu-id="3c16d-231">İfade deyimleri</span><span class="sxs-lookup"><span data-stu-id="3c16d-231">Expression statements</span></span>

<span data-ttu-id="3c16d-232">Bir *expression_statement* belirtilen ifadeyi hesaplar.</span><span class="sxs-lookup"><span data-stu-id="3c16d-232">An *expression_statement* evaluates a given expression.</span></span> <span data-ttu-id="3c16d-233">İfade tarafından hesaplanan değer, varsa, göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-233">The value computed by the expression, if any, is discarded.</span></span>

```antlr
expression_statement
    : statement_expression ';'
    ;

statement_expression
    : invocation_expression
    | null_conditional_invocation_expression
    | object_creation_expression
    | assignment
    | post_increment_expression
    | post_decrement_expression
    | pre_increment_expression
    | pre_decrement_expression
    | await_expression
    ;
```

<span data-ttu-id="3c16d-234">Tüm ifadeler deyimleri izin verilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-234">Not all expressions are permitted as statements.</span></span> <span data-ttu-id="3c16d-235">Gibi belirli, ifadelerde `x + y` ve `x == 1` , yalnızca işlem (atılır) bir değer, ifadeleri izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="3c16d-235">In particular, expressions such as `x + y` and `x == 1` that merely compute a value (which will be discarded), are not permitted as statements.</span></span>

<span data-ttu-id="3c16d-236">Yürütülmesini bir *expression_statement* kapsanan ifadeyi değerlendirir ve bitiş noktası için Denetim aktarır *expression_statement*.</span><span class="sxs-lookup"><span data-stu-id="3c16d-236">Execution of an *expression_statement* evaluates the contained expression and then transfers control to the end point of the *expression_statement*.</span></span> <span data-ttu-id="3c16d-237">Bitiş noktasını bir *expression_statement* erişilebilir olmadığını *expression_statement* erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-237">The end point of an *expression_statement* is reachable if that *expression_statement* is reachable.</span></span>

## <a name="selection-statements"></a><span data-ttu-id="3c16d-238">Seçim deyimleri</span><span class="sxs-lookup"><span data-stu-id="3c16d-238">Selection statements</span></span>

<span data-ttu-id="3c16d-239">Seçim deyimleri birkaç olası deyimler yürütme bazı ifadesinin değerine göre birini seçin.</span><span class="sxs-lookup"><span data-stu-id="3c16d-239">Selection statements select one of a number of possible statements for execution based on the value of some expression.</span></span>

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a><span data-ttu-id="3c16d-240">İf deyimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-240">The if statement</span></span>

<span data-ttu-id="3c16d-241">`if` Deyimi, bir boolean ifadesinin değerine göre yürütülecek bir deyim seçer.</span><span class="sxs-lookup"><span data-stu-id="3c16d-241">The `if` statement selects a statement for execution based on the value of a boolean expression.</span></span>

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="3c16d-242">Bir `else` parçasıdır sözcüksel olarak en yakın önceki ile ilişkili `if` sözdizimi tarafından verilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-242">An `else` part is associated with the lexically nearest preceding `if` that is allowed by the syntax.</span></span> <span data-ttu-id="3c16d-243">Bu nedenle, bir `if` formun deyimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-243">Thus, an `if` statement of the form</span></span>
```csharp
if (x) if (y) F(); else G();
```
<span data-ttu-id="3c16d-244">eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="3c16d-244">is equivalent to</span></span>
```csharp
if (x) {
    if (y) {
        F();
    }
    else {
        G();
    }
}
```

<span data-ttu-id="3c16d-245">Bir `if` deyim şu şekilde gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-245">An `if` statement is executed as follows:</span></span>

*  <span data-ttu-id="3c16d-246">*Boolean_expression* ([Boolean ifadeler](expressions.md#boolean-expressions)) değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-246">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="3c16d-247">Boole ifadesine döndürürse `true`, denetimi, ilk katıştırılmış deyime aktarılır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-247">If the boolean expression yields `true`, control is transferred to the first embedded statement.</span></span> <span data-ttu-id="3c16d-248">Zaman ve denetim bu bildirimi bitiş noktasını ulaşırsa, denetim için bitiş noktasını aktarılır `if` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-248">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="3c16d-249">Boole ifadesine döndürürse `false` ve bir `else` bölümü varsa, Denetim, ikinci katıştırılmış deyime aktarılır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-249">If the boolean expression yields `false` and if an `else` part is present, control is transferred to the second embedded statement.</span></span> <span data-ttu-id="3c16d-250">Zaman ve denetim bu bildirimi bitiş noktasını ulaşırsa, denetim için bitiş noktasını aktarılır `if` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-250">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="3c16d-251">Boole ifadesine döndürürse `false` ve bir `else` bölümü yoksa, denetim için bitiş noktasını aktarılan `if` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-251">If the boolean expression yields `false` and if an `else` part is not present, control is transferred to the end point of the `if` statement.</span></span>

<span data-ttu-id="3c16d-252">İlk katıştırılmış deyimi bir `if` deyimi erişilebilir değilse `if` ifadesi erişilebilir ve Boole ifadesi sabit değer olmayan `false`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-252">The first embedded statement of an `if` statement is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="3c16d-253">İkinci katıştırılmış deyimi bir `if` deyimi, varsa, erişilebilir değilse `if` ifadesi erişilebilir ve Boole ifadesi sabit değer olmayan `true`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-253">The second embedded statement of an `if` statement, if present, is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

<span data-ttu-id="3c16d-254">Bitiş noktasını bir `if` deyimi, bir katıştırılmış deyim en az bir uç noktası ulaşılıp ulaşılamadığını erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-254">The end point of an `if` statement is reachable if the end point of at least one of its embedded statements is reachable.</span></span> <span data-ttu-id="3c16d-255">Ayrıca, uç noktasını bir `if` deyimi olmadan `else` parçasıdır erişilebilir değilse `if` ifadesi erişilebilir ve Boole ifadesi sabit değer yok `true`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-255">In addition, the end point of an `if` statement with no `else` part is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-switch-statement"></a><span data-ttu-id="3c16d-256">Switch deyimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-256">The switch statement</span></span>

<span data-ttu-id="3c16d-257">Switch deyimi için yürütme switch ifadesinin değerine karşılık gelen bir ilişkili anahtar etiketine sahip bir deyim listesinin seçer.</span><span class="sxs-lookup"><span data-stu-id="3c16d-257">The switch statement selects for execution a statement list having an associated switch label that corresponds to the value of the switch expression.</span></span>

```antlr
switch_statement
    : 'switch' '(' expression ')' switch_block
    ;

switch_block
    : '{' switch_section* '}'
    ;

switch_section
    : switch_label+ statement_list
    ;

switch_label
    : 'case' constant_expression ':'
    | 'default' ':'
    ;
```

<span data-ttu-id="3c16d-258">A *switch_statement* anahtar sözcüğünü oluşur `switch`, parantezli ifade (switch ifadesi olarak adlandırılır) tarafından izlenen, arkasından bir *switch_block*.</span><span class="sxs-lookup"><span data-stu-id="3c16d-258">A *switch_statement* consists of the keyword `switch`, followed by a parenthesized expression (called the switch expression), followed by a *switch_block*.</span></span> <span data-ttu-id="3c16d-259">*Switch_block* sıfır veya daha fazla oluşur *switch_section*s, küme ayraçları içine alınmış.</span><span class="sxs-lookup"><span data-stu-id="3c16d-259">The *switch_block* consists of zero or more *switch_section*s, enclosed in braces.</span></span> <span data-ttu-id="3c16d-260">Her *switch_section* bir veya daha fazla oluşur *switch_label*s arkasından bir *statement_list* ([deyimi listeler](statements.md#statement-lists)).</span><span class="sxs-lookup"><span data-stu-id="3c16d-260">Each *switch_section* consists of one or more *switch_label*s followed by a *statement_list* ([Statement lists](statements.md#statement-lists)).</span></span>

<span data-ttu-id="3c16d-261">***Türü yöneten*** , bir `switch` ifadesi switch ifadesi kurulur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-261">The ***governing type*** of a `switch` statement is established by the switch expression.</span></span>

*  <span data-ttu-id="3c16d-262">Switch ifadesinin türü ise `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, veya bir *enum_type*, veya bu türlerden birine karşılık gelen null olabilen türü olan ardından yöneten olan türü `switch` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-262">If the type of the switch expression is `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, or an *enum_type*, or if it is the nullable type corresponding to one of these types, then that is the governing type of the `switch` statement.</span></span>
*  <span data-ttu-id="3c16d-263">Aksi halde, tam olarak bir kullanıcı tanımlı örtük dönüştürme ([kullanıcı tanımlı dönüşümler](conversions.md#user-defined-conversions)) switch ifadesinin türünden türleri yöneten aşağıdaki olası biri mevcut olmalıdır: `sbyte`, `byte`, `short` , `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, veya bu türlerden birine karşılık gelen null yapılabilir bir tür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-263">Otherwise, exactly one user-defined implicit conversion ([User-defined conversions](conversions.md#user-defined-conversions)) must exist from the type of the switch expression to one of the following possible governing types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, or,  a nullable type corresponding to one of those types.</span></span>
*  <span data-ttu-id="3c16d-264">Aksi takdirde, böyle bir örtük dönüştürme var ya da daha fazlası gibi örtük bir dönüştürme var, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-264">Otherwise, if no such implicit conversion exists, or if more than one such implicit conversion exists, a compile-time error occurs.</span></span>

<span data-ttu-id="3c16d-265">Her sabit ifade `case` etiket örtük olarak dönüştürülebilir bir değer belirtmek gerekir ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) değerlendirip türüne `switch` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-265">The constant expression of each `case` label must denote a value that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the `switch` statement.</span></span> <span data-ttu-id="3c16d-266">İki veya daha fazla, bir derleme zamanı hatası meydana gelir `case` etiketleri aynı `switch` deyimi aynı sabit değeri belirtin.</span><span class="sxs-lookup"><span data-stu-id="3c16d-266">A compile-time error occurs if two or more `case` labels in the same `switch` statement specify the same constant value.</span></span>

<span data-ttu-id="3c16d-267">En fazla bir olabilir `default` switch deyiminde etiketi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-267">There can be at most one `default` label in a switch statement.</span></span>

<span data-ttu-id="3c16d-268">A `switch` deyim şu şekilde gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-268">A `switch` statement is executed as follows:</span></span>

*  <span data-ttu-id="3c16d-269">Switch ifadesi değerlendirilir ve değerlendirip türüne dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-269">The switch expression is evaluated and converted to the governing type.</span></span>
*  <span data-ttu-id="3c16d-270">Sabitlerinden birini içinde belirtilen bir `case` aynı etiketi `switch` ifadesi switch ifadesinin değerine eşittir, Denetim, eşleşen aşağıdaki deyim listesinin aktarılır `case` etiketi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-270">If one of the constants specified in a `case` label in the same `switch` statement is equal to the value of the switch expression, control is transferred to the statement list following the matched `case` label.</span></span>
*  <span data-ttu-id="3c16d-271">Belirtilen sabitlerden hiçbiri `case` etiketleri aynı `switch` ifadesi switch ifadesinin değerine eşittir ve bir `default` etiket varsa, Denetim listesi deyime aktarılır `default` Etiket.</span><span class="sxs-lookup"><span data-stu-id="3c16d-271">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if a `default` label is present, control is transferred to the statement list following the `default` label.</span></span>
*  <span data-ttu-id="3c16d-272">Belirtilen sabitlerden hiçbiri `case` etiketleri aynı `switch` ifadesi switch ifadesinin değerine eşittir ve hiçbir `default` etiket varsa, denetim için bitiş noktasını aktarılan `switch` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-272">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if no `default` label is present, control is transferred to the end point of the `switch` statement.</span></span>

<span data-ttu-id="3c16d-273">İfade listesinin bir anahtar bölümünün bitiş noktasını erişilebiliyorsa, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-273">If the end point of the statement list of a switch section is reachable, a compile-time error occurs.</span></span> <span data-ttu-id="3c16d-274">Bu, "fall aracılığıyla" kuralı olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-274">This is known as the "no fall through" rule.</span></span> <span data-ttu-id="3c16d-275">Örnek</span><span class="sxs-lookup"><span data-stu-id="3c16d-275">The example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
default:
    CaseOthers();
    break;
}
```
<span data-ttu-id="3c16d-276">hiçbir anahtar bölümü erişilebilir bir uç noktaya sahip olduğundan geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="3c16d-276">is valid because no switch section has a reachable end point.</span></span> <span data-ttu-id="3c16d-277">C ve C++'ın aksine, bir anahtar bölümün yürütülmesi "geçiş" sonraki switch bölümüne ve örnek iznine sahip değil</span><span class="sxs-lookup"><span data-stu-id="3c16d-277">Unlike C and C++, execution of a switch section is not permitted to "fall through" to the next switch section, and the example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
case 1:
    CaseZeroOrOne();
default:
    CaseAny();
}
```
<span data-ttu-id="3c16d-278">bir derleme zamanı hatası sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-278">results in a compile-time error.</span></span> <span data-ttu-id="3c16d-279">Bir switch bölümüne yürütülmesini olduğunda başka bir anahtar bölümü, açık bir yürütülmesini tarafından izlenen `goto case` veya `goto default` deyimi kullanılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="3c16d-279">When execution of a switch section is to be followed by execution of another switch section, an explicit `goto case` or `goto default` statement must be used:</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    goto case 1;
case 1:
    CaseZeroOrOne();
    goto default;
default:
    CaseAny();
    break;
}
```

<span data-ttu-id="3c16d-280">Birden çok etiketle izin verilen bir *switch_section*.</span><span class="sxs-lookup"><span data-stu-id="3c16d-280">Multiple labels are permitted in a *switch_section*.</span></span> <span data-ttu-id="3c16d-281">Örnek</span><span class="sxs-lookup"><span data-stu-id="3c16d-281">The example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
case 2:
default:
    CaseTwo();
    break;
}
```
<span data-ttu-id="3c16d-282">geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="3c16d-282">is valid.</span></span> <span data-ttu-id="3c16d-283">Örneğin "fall aracılığıyla" kuralı nedeniyle ihlal etmemesini etiketleri `case 2:` ve `default:` aynı parçası olan *switch_section*.</span><span class="sxs-lookup"><span data-stu-id="3c16d-283">The example does not violate the "no fall through" rule because the labels `case 2:` and `default:` are part of the same *switch_section*.</span></span>

<span data-ttu-id="3c16d-284">Genel bir sınıf C ve C++ içinde oluşan hataların "fall aracılığıyla" kuralı engelleyen zaman `break` deyimleri yanlışlıkla atlanmış.</span><span class="sxs-lookup"><span data-stu-id="3c16d-284">The "no fall through" rule prevents a common class of bugs that occur in C and C++ when `break` statements are accidentally omitted.</span></span> <span data-ttu-id="3c16d-285">Ayrıca, bu kural, anahtar bölümlerini nedeniyle bir `switch` deyimi rasgele düzenlenmeyecek deyim davranışını etkilemeden.</span><span class="sxs-lookup"><span data-stu-id="3c16d-285">In addition, because of this rule, the switch sections of a `switch` statement can be arbitrarily rearranged without affecting the behavior of the statement.</span></span> <span data-ttu-id="3c16d-286">Örneğin, bölümlerini `switch` yukarıdaki ifade deyimi davranışını etkilemeden alınması:</span><span class="sxs-lookup"><span data-stu-id="3c16d-286">For example, the sections of the `switch` statement above can be reversed without affecting the behavior of the statement:</span></span>
```csharp
switch (i) {
default:
    CaseAny();
    break;
case 1:
    CaseZeroOrOne();
    goto default;
case 0:
    CaseZero();
    goto case 1;
}
```

<span data-ttu-id="3c16d-287">Bir switch bölümüne ifade listesi genellikle biten bir `break`, `goto case`, veya `goto default` deyimi, ancak ulaşılamaz ifade listesinin bitiş noktasını işleyen herhangi bir yapı izin verilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-287">The statement list of a switch section typically ends in a `break`, `goto case`, or `goto default` statement, but any construct that renders the end point of the statement list unreachable is permitted.</span></span> <span data-ttu-id="3c16d-288">Örneğin, bir `while` Boole ifadesine göre denetlenen deyim `true` uç noktasına hiçbir zaman için erişim verilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-288">For example, a `while` statement controlled by the boolean expression `true` is known to never reach its end point.</span></span> <span data-ttu-id="3c16d-289">Benzer şekilde, bir `throw` veya `return` ifade her zaman, başka bir yerde denetim aktarır ve hiçbir zaman uç noktasına ulaşır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-289">Likewise, a `throw` or `return` statement always transfers control elsewhere and never reaches its end point.</span></span> <span data-ttu-id="3c16d-290">Bu nedenle, aşağıdaki örnekte geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-290">Thus, the following example is valid:</span></span>
```csharp
switch (i) {
case 0:
    while (true) F();
case 1:
    throw new ArgumentException();
case 2:
    return;
}
```

<span data-ttu-id="3c16d-291">Değerlendirip türünü bir `switch` deyim türü olabilir `string`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-291">The governing type of a `switch` statement may be the type `string`.</span></span> <span data-ttu-id="3c16d-292">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3c16d-292">For example:</span></span>
```csharp
void DoCommand(string command) {
    switch (command.ToLower()) {
    case "run":
        DoRun();
        break;
    case "save":
        DoSave();
        break;
    case "quit":
        DoQuit();
        break;
    default:
        InvalidCommand(command);
        break;
    }
}
```

<span data-ttu-id="3c16d-293">Gibi dize eşitlik işleçleri ([dize eşitlik işleçleri](expressions.md#string-equality-operators)), `switch` deyimi büyük/küçük harfe duyarlıdır ve yalnızca switch ifadesi dize tam olarak eşleşmesi durumunda belirli bir anahtar bölüm yürütecek bir `case` etiketi sabit değer.</span><span class="sxs-lookup"><span data-stu-id="3c16d-293">Like the string equality operators ([String equality operators](expressions.md#string-equality-operators)), the `switch` statement is case sensitive and will execute a given switch section only if the switch expression string exactly matches a `case` label constant.</span></span>

<span data-ttu-id="3c16d-294">Düzenleyen zaman türü bir `switch` deyimi `string`, değer `null` durum etiketi sabit olarak izin verilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-294">When the governing type of a `switch` statement is `string`, the value `null` is permitted as a case label constant.</span></span>

<span data-ttu-id="3c16d-295">*Statement_list*sn bir *switch_block* bildirim deyimleri içerebilir ([bildirim deyimleri](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="3c16d-295">The *statement_list*s of a *switch_block* may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="3c16d-296">Bir yerel değişken veya sabit anahtar blokta bildirilen switch bloğu kapsamıdır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-296">The scope of a local variable or constant declared in a switch block is the switch block.</span></span>

<span data-ttu-id="3c16d-297">Belirli bir anahtar bölümündeki ifade listesinin erişilebilir olduğundan, `switch` ifadesi erişilebilir ve aşağıdakilerden en az birini doğrudur:</span><span class="sxs-lookup"><span data-stu-id="3c16d-297">The statement list of a given switch section is reachable if the `switch` statement is reachable and at least one of the following is true:</span></span>

*  <span data-ttu-id="3c16d-298">Switch ifadesi bir sabit değerdir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-298">The switch expression is a non-constant value.</span></span>
*  <span data-ttu-id="3c16d-299">Switch ifadesi ile eşleşen bir sabit değer olan bir `case` switch bölümüne etiketi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-299">The switch expression is a constant value that matches a `case` label in the switch section.</span></span>
*  <span data-ttu-id="3c16d-300">Switch ifadesi herhangi eşleşmeyen bir sabit değerdir `case` etiket ve anahtar bölümü içeren `default` etiketi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-300">The switch expression is a constant value that doesn't match any `case` label, and the switch section contains the `default` label.</span></span>
*  <span data-ttu-id="3c16d-301">Switch bölümüne bir anahtar etiketi tarafından erişilebilir başvurulan `goto case` veya `goto default` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-301">A switch label of the switch section is referenced by a reachable `goto case` or `goto default` statement.</span></span>

<span data-ttu-id="3c16d-302">Bitiş noktasını bir `switch` deyimdir aşağıdakilerden en az biri doğruysa erişilebilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-302">The end point of a `switch` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="3c16d-303">`switch` Deyimi içeren bir erişilebilir `break` çıkar deyimi `switch` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-303">The `switch` statement contains a reachable `break` statement that exits the `switch` statement.</span></span>
*  <span data-ttu-id="3c16d-304">`switch` Deyimi erişilebilir, switch ifadesi bir sabit olmayan değer ve Hayır `default` etiketi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-304">The `switch` statement is reachable, the switch expression is a non-constant value, and no `default` label is present.</span></span>
*  <span data-ttu-id="3c16d-305">`switch` Deyimi erişilebilir, switch ifadesi herhangi eşleşmeyen bir sabit değerdir `case` etiket ve Hayır `default` etiketi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-305">The `switch` statement is reachable, the switch expression is a constant value that doesn't match any `case` label, and no `default` label is present.</span></span>

## <a name="iteration-statements"></a><span data-ttu-id="3c16d-306">Yineleme deyimleri</span><span class="sxs-lookup"><span data-stu-id="3c16d-306">Iteration statements</span></span>

<span data-ttu-id="3c16d-307">Yineleme deyimleri, bir katıştırılmış deyim tekrar tekrar yürütün.</span><span class="sxs-lookup"><span data-stu-id="3c16d-307">Iteration statements repeatedly execute an embedded statement.</span></span>

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a><span data-ttu-id="3c16d-308">While ifadesi</span><span class="sxs-lookup"><span data-stu-id="3c16d-308">The while statement</span></span>

<span data-ttu-id="3c16d-309">`while` Deyimi, bir katıştırılmış deyim sıfır veya daha fazla kez koşullu yürütür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-309">The `while` statement conditionally executes an embedded statement zero or more times.</span></span>

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

<span data-ttu-id="3c16d-310">A `while` deyim şu şekilde gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-310">A `while` statement is executed as follows:</span></span>

*  <span data-ttu-id="3c16d-311">*Boolean_expression* ([Boolean ifadeler](expressions.md#boolean-expressions)) değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-311">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="3c16d-312">Boole ifadesine döndürürse `true`, Denetim, katıştırılmış deyime aktarılır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-312">If the boolean expression yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="3c16d-313">Zaman ve denetim gömülü deyim bitiş noktasını ulaşırsa (büyük olasılıkla yürütmesine bir `continue` deyimi), Denetim başlangıcına aktarılan `while` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-313">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), control is transferred to the beginning of the `while` statement.</span></span>
*  <span data-ttu-id="3c16d-314">Boole ifadesine döndürürse `false`, denetim için bitiş noktasını aktarılan `while` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-314">If the boolean expression yields `false`, control is transferred to the end point of the `while` statement.</span></span>

<span data-ttu-id="3c16d-315">Katıştırılmış deyimi içinde bir `while` deyimi, bir `break` deyimi ([break deyimi](statements.md#the-break-statement)) denetimi için bitiş noktasını aktarmak için kullanılabilir `while` (Bu nedenle Embedded yineleme bitiş deyimi deyimi) ve bir `continue` deyimi ([CONTINUE deyimi](statements.md#the-continue-statement)) uç noktasına katıştırılmış deyiminin denetim aktarmak için kullanılabilir (Bu nedenle, başka bir yineleme gerçekleştirme `while` deyimi).</span><span class="sxs-lookup"><span data-stu-id="3c16d-315">Within the embedded statement of a `while` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `while` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus performing another iteration of the `while` statement).</span></span>

<span data-ttu-id="3c16d-316">Katıştırılmış deyimi bir `while` deyimi erişilebilir değilse `while` ifadesi erişilebilir ve Boole ifadesi sabit değer olmayan `false`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-316">The embedded statement of a `while` statement is reachable if the `while` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="3c16d-317">Bitiş noktasını bir `while` deyimdir aşağıdakilerden en az biri doğruysa erişilebilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-317">The end point of a `while` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="3c16d-318">`while` Deyimi içeren bir erişilebilir `break` çıkar deyimi `while` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-318">The `while` statement contains a reachable `break` statement that exits the `while` statement.</span></span>
*  <span data-ttu-id="3c16d-319">`while` İfadesi erişilebilir ve Boole ifadesi sabit değer olmayan `true`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-319">The `while` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-do-statement"></a><span data-ttu-id="3c16d-320">Do deyimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-320">The do statement</span></span>

<span data-ttu-id="3c16d-321">`do` Deyimi, bir katıştırılmış deyimi bir veya daha fazla kez koşullu yürütür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-321">The `do` statement conditionally executes an embedded statement one or more times.</span></span>

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

<span data-ttu-id="3c16d-322">A `do` deyim şu şekilde gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-322">A `do` statement is executed as follows:</span></span>

*  <span data-ttu-id="3c16d-323">Denetim, katıştırılmış deyime aktarılır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-323">Control is transferred to the embedded statement.</span></span>
*  <span data-ttu-id="3c16d-324">Zaman ve denetim gömülü deyim bitiş noktasını ulaşırsa (büyük olasılıkla yürütülmesini gelen bir `continue` deyimi), *boolean_expression* ([Boolean ifadeler](expressions.md#boolean-expressions)) değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-324">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span> <span data-ttu-id="3c16d-325">Boole ifadesine döndürürse `true`, Denetim başlangıcına aktarılan `do` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-325">If the boolean expression yields `true`, control is transferred to the beginning of the `do` statement.</span></span> <span data-ttu-id="3c16d-326">Aksi takdirde, denetim için bitiş noktasını aktarılır `do` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-326">Otherwise, control is transferred to the end point of the `do` statement.</span></span>

<span data-ttu-id="3c16d-327">Katıştırılmış deyimi içinde bir `do` deyimi, bir `break` deyimi ([break deyimi](statements.md#the-break-statement)) denetimi için bitiş noktasını aktarmak için kullanılabilir `do` (Bu nedenle Embedded yineleme bitiş deyimi deyimi) ve bir `continue` deyimi ([CONTINUE deyimi](statements.md#the-continue-statement)) uç noktasına katıştırılmış deyiminin denetim aktarmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-327">Within the embedded statement of a `do` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `do` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement.</span></span>

<span data-ttu-id="3c16d-328">Katıştırılmış deyimi bir `do` deyimi erişilebilir değilse `do` deyimi erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-328">The embedded statement of a `do` statement is reachable if the `do` statement is reachable.</span></span>

<span data-ttu-id="3c16d-329">Bitiş noktasını bir `do` deyimdir aşağıdakilerden en az biri doğruysa erişilebilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-329">The end point of a `do` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="3c16d-330">`do` Deyimi içeren bir erişilebilir `break` çıkar deyimi `do` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-330">The `do` statement contains a reachable `break` statement that exits the `do` statement.</span></span>
*  <span data-ttu-id="3c16d-331">Gömülü deyim bitiş noktasının erişilebilir olduğundan ve Boole ifadesi sabit değer olmayan `true`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-331">The end point of the embedded statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-for-statement"></a><span data-ttu-id="3c16d-332">For deyimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-332">The for statement</span></span>

<span data-ttu-id="3c16d-333">`for` Deyimi bir dizi başlatma ifadeleri değerlendirir ve daha sonra bir koşul true olduğu sürece art arda katıştırılmış bir deyimi yürütür ve bir dizi yineleme ifadeleri değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-333">The `for` statement evaluates a sequence of initialization expressions and then, while a condition is true, repeatedly executes an embedded statement and evaluates a sequence of iteration expressions.</span></span>

```antlr
for_statement
    : 'for' '(' for_initializer? ';' for_condition? ';' for_iterator? ')' embedded_statement
    ;

for_initializer
    : local_variable_declaration
    | statement_expression_list
    ;

for_condition
    : boolean_expression
    ;

for_iterator
    : statement_expression_list
    ;

statement_expression_list
    : statement_expression (',' statement_expression)*
    ;
```

<span data-ttu-id="3c16d-334">*For_initializer*, varsa, birini oluşan bir *local_variable_declaration* ([yerel değişken bildirimlerini](statements.md#local-variable-declarations)) veya bir listesini *statement_ ifade*s ([ifade deyimleri](statements.md#expression-statements)) virgülle ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="3c16d-334">The *for_initializer*, if present, consists of either a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) or a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span> <span data-ttu-id="3c16d-335">Tarafından bildirilen yerel değişken kapsamını bir *for_initializer* başlar *local_variable_declarator* değişken için ve katıştırılmış deyim sonuna kadar genişletir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-335">The scope of a local variable declared by a *for_initializer* starts at the *local_variable_declarator* for the variable and extends to the end of the embedded statement.</span></span> <span data-ttu-id="3c16d-336">Kapsamının *for_condition* ve *for_iterator*.</span><span class="sxs-lookup"><span data-stu-id="3c16d-336">The scope includes the *for_condition* and the *for_iterator*.</span></span>

<span data-ttu-id="3c16d-337">*For_condition*, varsa, olmalıdır bir *boolean_expression* ([Boolean ifadeler](expressions.md#boolean-expressions)).</span><span class="sxs-lookup"><span data-stu-id="3c16d-337">The *for_condition*, if present, must be a *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)).</span></span>

<span data-ttu-id="3c16d-338">*For_iterator*, varsa, bir listesinden oluşur *statement_expression*s ([ifade deyimleri](statements.md#expression-statements)) virgülle ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="3c16d-338">The *for_iterator*, if present, consists of a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span>

<span data-ttu-id="3c16d-339">Deyimi için şu şekilde gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-339">A for statement is executed as follows:</span></span>

*  <span data-ttu-id="3c16d-340">Varsa bir *for_initializer* değişken başlatıcıları varsa veya deyim ifadeleri, bunlar yazılır sırayla yürütülür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-340">If a *for_initializer* is present, the variable initializers or statement expressions are executed in the order they are written.</span></span> <span data-ttu-id="3c16d-341">Bu adım yalnızca bir kez gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-341">This step is only performed once.</span></span>
*  <span data-ttu-id="3c16d-342">Varsa bir *for_condition* var, bunu değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-342">If a *for_condition* is present, it is evaluated.</span></span>
*  <span data-ttu-id="3c16d-343">Varsa *for_condition* yoksa veya değerlendirme verir durumunda değil `true`, Denetim, katıştırılmış deyime aktarılır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-343">If the *for_condition* is not present or if the evaluation yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="3c16d-344">Zaman ve denetim gömülü deyim bitiş noktasını ulaşırsa (büyük olasılıkla yürütülmesini gelen bir `continue` deyimi), ifadelerin *for_iterator*, varsa, sırayla değerlendirilir ve ardından başka bir yineleme değerlendirmesi ile başlayarak, gerçekleştirilen *for_condition* Yukarıdaki adımda.</span><span class="sxs-lookup"><span data-stu-id="3c16d-344">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the expressions of the *for_iterator*, if any, are evaluated in sequence, and then another iteration is performed, starting with evaluation of the *for_condition* in the step above.</span></span>
*  <span data-ttu-id="3c16d-345">Varsa *for_condition* var ve değerlendirme verir `false`, denetim için bitiş noktasını aktarılan `for` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-345">If the *for_condition* is present and the evaluation yields `false`, control is transferred to the end point of the `for` statement.</span></span>

<span data-ttu-id="3c16d-346">Katıştırılmış deyimi içinde bir `for` deyimi, bir `break` deyimi ([break deyimi](statements.md#the-break-statement)) denetimi için bitiş noktasını aktarmak için kullanılabilir `for` (Bu nedenle Embedded yineleme bitiş deyimi deyimi) ve bir `continue` deyimi ([CONTINUE deyimi](statements.md#the-continue-statement)) denetim gömülü deyim uç noktasına aktarmak için kullanılabilir (Bu nedenle yürütme *for_iterator* ve başka bir yinelemesini gerçekleştirme `for` deyimi ile başlayarak, *for_condition*).</span><span class="sxs-lookup"><span data-stu-id="3c16d-346">Within the embedded statement of a `for` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `for` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus executing the *for_iterator* and performing another iteration of the `for` statement, starting with the *for_condition*).</span></span>

<span data-ttu-id="3c16d-347">Katıştırılmış deyimi bir `for` deyimdir aşağıdakilerden biri doğruysa erişilebilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-347">The embedded statement of a `for` statement is reachable if one of the following is true:</span></span>

*  <span data-ttu-id="3c16d-348">`for` Deyimi erişilebilir ve hiçbir *for_condition* mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-348">The `for` statement is reachable and no *for_condition* is present.</span></span>
*  <span data-ttu-id="3c16d-349">`for` Deyimi erişilebilir ve *for_condition* varsa ve sabit değer olmayan `false`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-349">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `false`.</span></span>

<span data-ttu-id="3c16d-350">Bitiş noktasını bir `for` deyimdir aşağıdakilerden en az biri doğruysa erişilebilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-350">The end point of a `for` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="3c16d-351">`for` Deyimi içeren bir erişilebilir `break` çıkar deyimi `for` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-351">The `for` statement contains a reachable `break` statement that exits the `for` statement.</span></span>
*  <span data-ttu-id="3c16d-352">`for` Deyimi erişilebilir ve *for_condition* varsa ve sabit değer olmayan `true`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-352">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `true`.</span></span>

### <a name="the-foreach-statement"></a><span data-ttu-id="3c16d-353">Foreach deyimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-353">The foreach statement</span></span>

<span data-ttu-id="3c16d-354">`foreach` Deyimi koleksiyonundaki her öğe için bir katıştırılmış deyim yürütülürken, bir koleksiyonun öğeleri sıralar.</span><span class="sxs-lookup"><span data-stu-id="3c16d-354">The `foreach` statement enumerates the elements of a collection, executing an embedded statement for each element of the collection.</span></span>

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

<span data-ttu-id="3c16d-355">*Türü* ve *tanımlayıcı* , bir `foreach` declare deyimi ***yineleme değişkeni*** deyiminin.</span><span class="sxs-lookup"><span data-stu-id="3c16d-355">The *type* and *identifier* of a `foreach` statement declare the ***iteration variable*** of the statement.</span></span> <span data-ttu-id="3c16d-356">Varsa `var` tanımlayıcısı olarak verilir *local_variable_type*ve adlandırılmış tür `var` olan kapsamda yineleme değişkeni olarak kabul edilir bir ***örtük olarak yazılan yineleme değişkeni***, ve onun türü öğe türü olarak alınır `foreach` aşağıda belirtildiği gibi bir ifade.</span><span class="sxs-lookup"><span data-stu-id="3c16d-356">If the `var` identifier is given as the *local_variable_type*, and no type named `var` is in scope, the iteration variable is said to be an ***implicitly typed iteration variable***, and its type is taken to be the element type of the `foreach` statement, as specified below.</span></span> <span data-ttu-id="3c16d-357">Yineleme değişkeni salt okunur bir yerel değişken gömülü deyim genişleten bir kapsamla karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-357">The iteration variable corresponds to a read-only local variable with a scope that extends over the embedded statement.</span></span> <span data-ttu-id="3c16d-358">Yürütülmesi sırasında bir `foreach` deyimi, yineleme değişkeni, bir yineleme şu anda gerçekleştirildiği için koleksiyon öğeyi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="3c16d-358">During execution of a `foreach` statement, the iteration variable represents the collection element for which an iteration is currently being performed.</span></span> <span data-ttu-id="3c16d-359">Gömülü deyim yineleme değişkeni değiştirme girişiminde bulunursa, bir derleme zamanı hatası oluşur (atama aracılığıyla veya `++` ve `--` işleçleri) veya yineleme değişkeni olarak bir `ref` veya `out` parametresi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-359">A compile-time error occurs if the embedded statement attempts to modify the iteration variable (via assignment or the `++` and `--` operators) or pass the iteration variable as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="3c16d-360">Kısaltma, aşağıdaki `IEnumerable`, `IEnumerator`, `IEnumerable<T>` ve `IEnumerator<T>` ad alanlarındaki karşılık gelen türlerine başvuran `System.Collections` ve `System.Collections.Generic`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-360">In the following, for brevity, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` and `IEnumerator<T>` refer to the corresponding types in the namespaces `System.Collections` and `System.Collections.Generic`.</span></span>

<span data-ttu-id="3c16d-361">Foreach deyimi derleme zamanı işlenmesini önce belirler ***koleksiyon türü***, ***Numaralandırıcı türü*** ve ***öğe türü*** ifadesi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-361">The compile-time processing of a foreach statement first determines the ***collection type***, ***enumerator type*** and ***element type*** of the expression.</span></span> <span data-ttu-id="3c16d-362">Bu belirleme gibi çalışır:</span><span class="sxs-lookup"><span data-stu-id="3c16d-362">This determination proceeds as follows:</span></span>

*  <span data-ttu-id="3c16d-363">Varsa türü `X` , *ifade* bir örtük bir başvuru dönüştürme daha sonra bir dizi türü olduğu `X` için `IEnumerable` arabirimi (bu yana `System.Array` bu arabirimi uygulayan).</span><span class="sxs-lookup"><span data-stu-id="3c16d-363">If the type `X` of *expression* is an array type then there is an implicit reference conversion from `X` to the `IEnumerable` interface (since `System.Array` implements this interface).</span></span> <span data-ttu-id="3c16d-364">***Koleksiyon türü*** olduğu `IEnumerable` arabirimi ***Numaralandırıcı türü*** olduğu `IEnumerator` arabirimi ve ***öğe türü*** öğe türü dizi türü `X`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-364">The ***collection type*** is the `IEnumerable` interface, the ***enumerator type*** is the `IEnumerator` interface and the ***element type*** is the element type of the array type `X`.</span></span>
*  <span data-ttu-id="3c16d-365">Varsa türü `X` , *ifade* olduğu `dynamic` örtük bir dönüştürme daha sonra *ifade* için `IEnumerable` arabirimi ([örtük dinamik dönüştürmeleri](conversions.md#implicit-dynamic-conversions)).</span><span class="sxs-lookup"><span data-stu-id="3c16d-365">If the type `X` of *expression* is `dynamic` then there is an implicit conversion from *expression* to the `IEnumerable` interface ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)).</span></span> <span data-ttu-id="3c16d-366">***Koleksiyon türü*** olduğu `IEnumerable` arabirimi ve ***Numaralandırıcı türü*** olduğu `IEnumerator` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-366">The ***collection type*** is the `IEnumerable` interface and the ***enumerator type*** is the `IEnumerator` interface.</span></span> <span data-ttu-id="3c16d-367">Varsa `var` tanımlayıcısı olarak verilir *local_variable_type* sonra ***öğe türü*** olduğu `dynamic`, aksi halde kalır `object`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-367">If the `var` identifier is given as the *local_variable_type* then the ***element type*** is `dynamic`, otherwise it is `object`.</span></span>
*  <span data-ttu-id="3c16d-368">Aksi takdirde, belirlemek olmadığını türü `X` uygun bir sahip `GetEnumerator` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3c16d-368">Otherwise, determine whether the type `X` has an appropriate `GetEnumerator` method:</span></span>
   * <span data-ttu-id="3c16d-369">Türde üye araması gerçekleştirmek `X` tanımlayıcıyla `GetEnumerator` ve hiçbir tür bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="3c16d-369">Perform member lookup on the type `X` with identifier `GetEnumerator` and no type arguments.</span></span> <span data-ttu-id="3c16d-370">Üye araması bir eşleşme üretmez veya bir belirsizlik oluşturur veya bir yöntem grubu olmayan bir eşleşme üretir, numaralandırılabilir bir arabirim için aşağıda açıklandığı gibi kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="3c16d-370">If the member lookup does not produce a match, or it produces an ambiguity, or produces a match that is not a method group, check for an enumerable interface as described below.</span></span> <span data-ttu-id="3c16d-371">Üye araması bir yöntem grubu veya eşleşme dışında her şeyi oluşturursa uyarı verilmesi önerilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-371">It is recommended that a warning be issued if member lookup produces anything except a method group or no match.</span></span>
   * <span data-ttu-id="3c16d-372">Elde edilen yöntem grubu ve boş bağımsız değişken listesi kullanılarak aşırı yükleme çözümlemesi gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-372">Perform overload resolution using the resulting method group and an empty argument list.</span></span> <span data-ttu-id="3c16d-373">Aşırı yükleme çözünürlüğü geçerli hiçbir yöntemleri sonuçlanırsa, bir belirsizlik sonuçları veya yöntemi statik veya ortak değil, onay sıralanabilir arabirim için aşağıda açıklanan şekilde ancak bu, tek bir en iyi yöntem sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-373">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, check for an enumerable interface as described below.</span></span> <span data-ttu-id="3c16d-374">Aşırı yükleme çözümlemesi belirsiz ortak örnek yöntemi veya hiçbir uygun yöntemleri dışında her şeyi oluşturursa uyarı verilmesi önerilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-374">It is recommended that a warning be issued if overload resolution produces anything except an unambiguous public instance method or no applicable methods.</span></span>
   * <span data-ttu-id="3c16d-375">Dönüş türü `E` , `GetEnumerator` yöntemi bir sınıf değil, yapı veya arabirim türü, bir hata oluşturulur ve başka bir adım alınır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-375">If the return type `E` of the `GetEnumerator` method is not a class, struct or interface type, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="3c16d-376">Üye araması gerçekleştirilir `E` tanımlayıcıyla `Current` ve hiçbir tür bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="3c16d-376">Member lookup is performed on `E` with the identifier `Current` and no type arguments.</span></span> <span data-ttu-id="3c16d-377">Üye araması eşleşme üretir, sonuç hatadır veya okuma izin veren bir genel örnek özelliğini dışındaki sonucudur, bir hata oluşturulur ve başka bir adım alınır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-377">If the member lookup produces no match, the result is an error, or the result is anything except a public instance property that permits reading, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="3c16d-378">Üye araması gerçekleştirilir `E` tanımlayıcıyla `MoveNext` ve hiçbir tür bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="3c16d-378">Member lookup is performed on `E` with the identifier `MoveNext` and no type arguments.</span></span> <span data-ttu-id="3c16d-379">Üye araması eşleşme üretir, sonuç hatadır ve sonucu bir yöntem grubu dışında bir şey var mı, bir hata oluşturulur ve başka bir adım alınır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-379">If the member lookup produces no match, the result is an error, or the result is anything except a method group, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="3c16d-380">Aşırı yükleme çözünürlüğü yöntem grubu boş bağımsız değişken listesi ile gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-380">Overload resolution is performed on the method group with an empty argument list.</span></span> <span data-ttu-id="3c16d-381">Aşırı yükleme çözünürlüğü sonuçlarında hiçbir uygun yöntemleri, sonuçları bir belirsizlik veya tek bir en iyi yöntem, ancak bu yöntem sonuçları, statik veya ortak değil ya da dönüş türü değil `bool`, bir hata oluşturulur ve başka bir adım alınır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-381">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, or its return type is not `bool`, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="3c16d-382">***Koleksiyon türü*** olduğu `X`, ***Numaralandırıcı türü*** olduğu `E`ve ***öğe türü*** türü `Current` özelliği.</span><span class="sxs-lookup"><span data-stu-id="3c16d-382">The ***collection type*** is `X`, the ***enumerator type*** is `E`, and the ***element type*** is the type of the `Current` property.</span></span>

*  <span data-ttu-id="3c16d-383">Aksi takdirde, numaralandırılabilir bir arabirim için denetleyin:</span><span class="sxs-lookup"><span data-stu-id="3c16d-383">Otherwise, check for an enumerable interface:</span></span>
   * <span data-ttu-id="3c16d-384">Eğer tüm türleri arasında `Ti` var olan açık bir dönüştürme `X` için `IEnumerable<Ti>`, benzersiz bir tür `T` şekilde `T` değil `dynamic` ve diğer tüm `Ti` yoktur bir örtük dönüştürme `IEnumerable<T>` için `IEnumerable<Ti>`, ardından ***koleksiyon türü*** arabirim `IEnumerable<T>`, ***Numaralandırıcı türü*** arabirimi`IEnumerator<T>`ve ***öğe türü*** olduğu `T`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-384">If among all the types `Ti` for which there is an implicit conversion from `X` to `IEnumerable<Ti>`, there is a unique type `T` such that `T` is not `dynamic` and for all the other `Ti` there is an implicit conversion from `IEnumerable<T>` to `IEnumerable<Ti>`, then the ***collection type*** is the interface `IEnumerable<T>`, the ***enumerator type*** is the interface `IEnumerator<T>`, and the ***element type*** is `T`.</span></span>
   * <span data-ttu-id="3c16d-385">Aksi takdirde birden fazla tür ise `T`, ardından bir hata oluşturulur ve başka bir adım alınır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-385">Otherwise, if there is more than one such type `T`, then an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="3c16d-386">Aksi takdirde örtük bir dönüştürme ise `X` için `System.Collections.IEnumerable` arabirimi, ardından ***koleksiyon türü*** bu arabirim, ***Numaralandırıcı türü*** arabirimi`System.Collections.IEnumerator`ve ***öğe türü*** olduğu `object`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-386">Otherwise, if there is an implicit conversion from `X` to the `System.Collections.IEnumerable` interface, then the ***collection type*** is this interface, the ***enumerator type*** is the interface `System.Collections.IEnumerator`, and the ***element type*** is `object`.</span></span>
   * <span data-ttu-id="3c16d-387">Aksi takdirde bir hata oluşturulur ve başka bir adım alınır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-387">Otherwise, an error is produced and no further steps are taken.</span></span>

<span data-ttu-id="3c16d-388">Yukarıdaki adımları başarılı olursa, kesin bir şekilde bir koleksiyon türü üretmek `C`, numaralandırıcı türü `E` ve öğe türü `T`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-388">The above steps, if successful, unambiguously produce a collection type `C`, enumerator type `E` and element type `T`.</span></span> <span data-ttu-id="3c16d-389">Formun foreach deyimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-389">A foreach statement of the form</span></span>
```csharp
foreach (V v in x) embedded_statement
```
<span data-ttu-id="3c16d-390">ardından için genişletilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-390">is then expanded to:</span></span>
```csharp
{
    E e = ((C)(x)).GetEnumerator();
    try {
        while (e.MoveNext()) {
            V v = (V)(T)e.Current;
            embedded_statement
        }
    }
    finally {
        ... // Dispose e
    }
}
```

<span data-ttu-id="3c16d-391">Değişken `e` görülebilir ya da ifade erişilebilir değil `x` veya katıştırılmış deyim ya da programın başka bir kaynak kodu.</span><span class="sxs-lookup"><span data-stu-id="3c16d-391">The variable `e` is not visible to or accessible to the expression `x` or the embedded statement or any other source code of the program.</span></span> <span data-ttu-id="3c16d-392">Değişken `v` katıştırılmış deyiminde salt okunur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-392">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="3c16d-393">Açık bir dönüştürme ise ([açık dönüştürmeler](conversions.md#explicit-conversions)) öğesinden `T` (öğe türü) için `V` ( *local_variable_type* foreach deyiminde), bir hata oluşturulur ve başka bir adım alınır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-393">If there is not an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) from `T` (the element type) to `V` (the *local_variable_type* in the foreach statement), an error is produced and no further steps are taken.</span></span> <span data-ttu-id="3c16d-394">Varsa `x` değerine sahip `null`, `System.NullReferenceException` çalışma zamanında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-394">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

<span data-ttu-id="3c16d-395">Yukarıdaki genişletmeyle tutarlı bir davranıştır sürece bir uygulama belirli bir foreach deyimi, örneğin performans nedenleriyle desteklememesinden izin verilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-395">An implementation is permitted to implement a given foreach-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="3c16d-396">Yerleşimini `v` içinde while döngüsü nasıl gerçekleşen herhangi bir anonim işleve göre yakalandıktan için önemli *embedded_statement*.</span><span class="sxs-lookup"><span data-stu-id="3c16d-396">The placement of `v` inside the while loop is important for how it is captured by any anonymous function occurring in the *embedded_statement*.</span></span>

<span data-ttu-id="3c16d-397">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3c16d-397">For example:</span></span>
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
<span data-ttu-id="3c16d-398">Varsa `v` while dışında bildirildi döngü, tüm yinelemeleri ve sonra değeri arasında paylaşılabilir döngü son değer olacaktır için `13`, hangi çağırma olduğu `f` yazdırır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-398">If `v` was declared outside of the while loop, it would be shared among all iterations, and its value after the for loop would be the final value, `13`, which is what the invocation of `f` would print.</span></span> <span data-ttu-id="3c16d-399">Bunun yerine, her yineleme kendi değişken olduğundan `v`, bir yakalanan `f` ilk değerini tutmak yineleme devam edecek `7`, olduğu ne yazdırılır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-399">Instead, because each iteration has its own variable `v`, the one captured by `f` in the first iteration will continue to hold the value `7`, which is what will be printed.</span></span> <span data-ttu-id="3c16d-400">(Not: C# ' ın önceki sürümlerinde bildirilen `v` dışına while döngüsü.)</span><span class="sxs-lookup"><span data-stu-id="3c16d-400">(Note: earlier versions of C# declared `v` outside of the while loop.)</span></span>

<span data-ttu-id="3c16d-401">Gövdesi finally bloğu göre aşağıdaki adımları oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="3c16d-401">The body of the finally block is constructed according to the following steps:</span></span>

*  <span data-ttu-id="3c16d-402">Örtük bir dönüştürme ise `E` için `System.IDisposable` ardından arabirimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-402">If there is an implicit conversion from `E` to the `System.IDisposable` interface, then</span></span>
   *  <span data-ttu-id="3c16d-403">Varsa `E` NULL olmayan bir değer türü olan sonra finally yan tümcesi anlam denk genişletilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-403">If `E` is a non-nullable value type then the finally clause is expanded to the semantic equivalent  of:</span></span>

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  <span data-ttu-id="3c16d-404">Aksi takdirde finally yan tümcesi anlam denk genişletilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-404">Otherwise the finally clause is expanded to the semantic equivalent of:</span></span>

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   <span data-ttu-id="3c16d-405">dışındaki olması durumunda `E` bir değer türü veya tür parametresi bir değer yazın ve ardından atanacağını örneği `e` için `System.IDisposable` kutulama oluşmasına neden olmaz.</span><span class="sxs-lookup"><span data-stu-id="3c16d-405">except that if `E` is a value type, or a type parameter instantiated to a value type, then the cast of `e` to `System.IDisposable` will not cause boxing to occur.</span></span>

*  <span data-ttu-id="3c16d-406">Aksi takdirde `E` mühürlenmiş bir tür finally yan tümcesi için boş bir blok genişletilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-406">Otherwise, if `E` is a sealed type, the finally clause is expanded to an empty block:</span></span>

   ```csharp
   finally {
   }
   ```

*  <span data-ttu-id="3c16d-407">Aksi takdirde, finally yan tümcesi için genişletilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-407">Otherwise, the finally clause is expanded to:</span></span>

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   <span data-ttu-id="3c16d-408">Yerel değişken `d` görülebilir ya da herhangi bir kullanıcı kodu tarafından erişilebilir değil.</span><span class="sxs-lookup"><span data-stu-id="3c16d-408">The local variable `d` is not visible to or accessible to any user code.</span></span> <span data-ttu-id="3c16d-409">Özellikle, kapsamı içeren diğer değişkeni ile çakışmaz finally bloğunda.</span><span class="sxs-lookup"><span data-stu-id="3c16d-409">In particular, it does not conflict with any other variable whose scope includes the finally block.</span></span>

<span data-ttu-id="3c16d-410">Bir sırayı `foreach` bir dizinin öğeleri erişir, aşağıdaki gibidir: Tek boyutlu dizi öğeleri, artan dizin sırasıyla çapraz geçiş için ile dizininden başlayarak `0` ve dizin ile biten `Length - 1`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-410">The order in which `foreach` traverses the elements of an array, is as follows: For single-dimensional arrays elements are traversed in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="3c16d-411">En sağdaki boyutu dizinleri artan ilk sonra sonraki sol boyut vb. için sol olduğuna gibi çok boyutlu diziler için öğeleri geçirilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-411">For multi-dimensional arrays, elements are traversed such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span>

<span data-ttu-id="3c16d-412">Aşağıdaki örnek, her değer sırayla öğesi iki boyutlu bir dizi kullanıma yazdırır:</span><span class="sxs-lookup"><span data-stu-id="3c16d-412">The following example prints out each value in a two-dimensional array, in element order:</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        double[,] values = {
            {1.2, 2.3, 3.4, 4.5},
            {5.6, 6.7, 7.8, 8.9}
        };

        foreach (double elementValue in values)
            Console.Write("{0} ", elementValue);

        Console.WriteLine();
    }
}
```
<span data-ttu-id="3c16d-413">Oluşturulan çıktı şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-413">The output produced is as follows:</span></span>
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

<span data-ttu-id="3c16d-414">Örnekte</span><span class="sxs-lookup"><span data-stu-id="3c16d-414">In the example</span></span>
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
<span data-ttu-id="3c16d-415">türünü `n` olmasını çıkarılan `int`, öğe türü `numbers`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-415">the type of `n` is inferred to be `int`, the element type of `numbers`.</span></span>

## <a name="jump-statements"></a><span data-ttu-id="3c16d-416">Atlama deyimleri</span><span class="sxs-lookup"><span data-stu-id="3c16d-416">Jump statements</span></span>

<span data-ttu-id="3c16d-417">Atlama deyimleri koşulsuz olarak denetim aktarın.</span><span class="sxs-lookup"><span data-stu-id="3c16d-417">Jump statements unconditionally transfer control.</span></span>

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

<span data-ttu-id="3c16d-418">Atlama deyimi aktarır denetimi konumu adlı ***hedef*** atlama deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-418">The location to which a jump statement transfers control is called the ***target*** of the jump statement.</span></span>

<span data-ttu-id="3c16d-419">Bir blok içinde bir atlama deyimi gerçekleşir ve bu blok dışındaysa, atlama bildiriminin hedefi olan, atlama deyimi edilir ***çıkmak*** blok.</span><span class="sxs-lookup"><span data-stu-id="3c16d-419">When a jump statement occurs within a block, and the target of that jump statement is outside that block, the jump statement is said to ***exit*** the block.</span></span> <span data-ttu-id="3c16d-420">Atlama deyimi denetimi dışında bir blok aktarabilir olsa da, Denetim bloğunun içine hiçbir zaman aktarabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3c16d-420">While a jump statement may transfer control out of a block, it can never transfer control into a block.</span></span>

<span data-ttu-id="3c16d-421">Atlama deyimleri yürütülmesini müdahale cihazlarınız varlık tarafından karmaşık `try` deyimleri.</span><span class="sxs-lookup"><span data-stu-id="3c16d-421">Execution of jump statements is complicated by the presence of intervening `try` statements.</span></span> <span data-ttu-id="3c16d-422">Örneğin mevcut olmadığında `try` deyimleri, atlama deyimi koşulsuz olarak aktarır denetimi atlama deyimden hedefte.</span><span class="sxs-lookup"><span data-stu-id="3c16d-422">In the absence of such `try` statements, a jump statement unconditionally transfers control from the jump statement to its target.</span></span> <span data-ttu-id="3c16d-423">Bu tür müdahale cihazlarınız varsa `try` deyimlerini yürütme daha karmaşık.</span><span class="sxs-lookup"><span data-stu-id="3c16d-423">In the presence of such intervening `try` statements, execution is more complex.</span></span> <span data-ttu-id="3c16d-424">Atlama deyimi bir veya daha fazla çıkılıyorsa `try` bloklarla ilişkili `finally` denetim blokları için başlangıçta aktarılır `finally` en içteki bloğunu `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-424">If the jump statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="3c16d-425">Zaman ve denetim bitiş noktasını ulaşırsa bir `finally` denetim bloğu aktarılır `finally` sonraki kapsayan, blok `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-425">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="3c16d-426">Kadar bu işlem yinelenir `finally` tüm blokları müdahalede bulunan `try` deyimler yürütülür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-426">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>

<span data-ttu-id="3c16d-427">Örnekte</span><span class="sxs-lookup"><span data-stu-id="3c16d-427">In the example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        while (true) {
            try {
                try {
                    Console.WriteLine("Before break");
                    break;
                }
                finally {
                    Console.WriteLine("Innermost finally block");
                }
            }
            finally {
                Console.WriteLine("Outermost finally block");
            }
        }
        Console.WriteLine("After break");
    }
}
```
<span data-ttu-id="3c16d-428">`finally` iki ile ilişkili blokları `try` deyimleri denetimi atlama deyimi hedefe aktarılan önce yürütülür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-428">the `finally` blocks associated with two `try` statements are executed before control is transferred to the target of the jump statement.</span></span>

<span data-ttu-id="3c16d-429">Oluşturulan çıktı şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-429">The output produced is as follows:</span></span>
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a><span data-ttu-id="3c16d-430">Break deyimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-430">The break statement</span></span>

<span data-ttu-id="3c16d-431">`break` Deyimi en yakın kapsayan çıkar `switch`, `while`, `do`, `for`, veya `foreach` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-431">The `break` statement exits the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
break_statement
    : 'break' ';'
    ;
```

<span data-ttu-id="3c16d-432">Hedefi bir `break` deyimi en yakın kapsayan uç noktasıdır `switch`, `while`, `do`, `for`, veya `foreach` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-432">The target of a `break` statement is the end point of the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="3c16d-433">Varsa bir `break` deyimi tarafından içine alınmayan bir `switch`, `while`, `do`, `for`, veya `foreach` deyimi, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-433">If a `break` statement is not enclosed by a `switch`, `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="3c16d-434">Zaman birden çok `switch`, `while`, `do`, `for`, veya `foreach` ifadeleri iç içe birbirine içinde bir `break` bildirimi yalnızca en içteki deyimi için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-434">When multiple `switch`, `while`, `do`, `for`, or `foreach` statements are nested within each other, a `break` statement applies only to the innermost statement.</span></span> <span data-ttu-id="3c16d-435">Denetim birden çok iç içe geçme düzeyi arasında aktarmak için bir `goto` deyimi ([goto deyimi](statements.md#the-goto-statement)) kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-435">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="3c16d-436">A `break` olamaz exit deyimi bir `finally` blok ([try deyimi](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="3c16d-436">A `break` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="3c16d-437">Olduğunda bir `break` ifade oluştuğu yer bir `finally` bloğu hedefinin `break` deyimi içinde aynı olmalıdır `finally` engelle; Aksi takdirde, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-437">When a `break` statement occurs within a `finally` block, the target of the `break` statement must be within the same `finally` block; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="3c16d-438">A `break` deyim şu şekilde gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-438">A `break` statement is executed as follows:</span></span>

*  <span data-ttu-id="3c16d-439">Varsa `break` deyimi, bir veya daha fazla çıkar `try` bloklarla ilişkili `finally` denetim blokları için başlangıçta aktarılır `finally` en içteki bloğunu `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-439">If the `break` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="3c16d-440">Zaman ve denetim bitiş noktasını ulaşırsa bir `finally` denetim bloğu aktarılır `finally` sonraki kapsayan, blok `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-440">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="3c16d-441">Kadar bu işlem yinelenir `finally` tüm blokları müdahalede bulunan `try` deyimler yürütülür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-441">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="3c16d-442">Denetim hedefi için aktarılan `break` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-442">Control is transferred to the target of the `break` statement.</span></span>

<span data-ttu-id="3c16d-443">Çünkü bir `break` deyimi koşulsuz aktarımları denetimi başka bir yerde bitiş noktasını bir `break` deyimi erişilebilir hiçbir zaman.</span><span class="sxs-lookup"><span data-stu-id="3c16d-443">Because a `break` statement unconditionally transfers control elsewhere, the end point of a `break` statement is never reachable.</span></span>

### <a name="the-continue-statement"></a><span data-ttu-id="3c16d-444">Continue deyimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-444">The continue statement</span></span>

<span data-ttu-id="3c16d-445">`continue` Deyimi en yakın kapsayan, yeni bir yineleme başlatır `while`, `do`, `for`, veya `foreach` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-445">The `continue` statement starts a new iteration of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
continue_statement
    : 'continue' ';'
    ;
```

<span data-ttu-id="3c16d-446">Hedefi bir `continue` deyimdir katıştırılmış deyimi en yakın kapsayan bitiş noktasını `while`, `do`, `for`, veya `foreach` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-446">The target of a `continue` statement is the end point of the embedded statement of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="3c16d-447">Varsa bir `continue` deyimi tarafından içine alınmayan bir `while`, `do`, `for`, veya `foreach` deyimi, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-447">If a `continue` statement is not enclosed by a `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="3c16d-448">Zaman birden çok `while`, `do`, `for`, veya `foreach` ifadeleri iç içe birbirine içinde bir `continue` bildirimi yalnızca en içteki deyimi için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-448">When multiple `while`, `do`, `for`, or `foreach` statements are nested within each other, a `continue` statement applies only to the innermost statement.</span></span> <span data-ttu-id="3c16d-449">Denetim birden çok iç içe geçme düzeyi arasında aktarmak için bir `goto` deyimi ([goto deyimi](statements.md#the-goto-statement)) kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-449">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="3c16d-450">A `continue` olamaz exit deyimi bir `finally` blok ([try deyimi](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="3c16d-450">A `continue` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="3c16d-451">Olduğunda bir `continue` ifade oluştuğu yer bir `finally` bloğu hedefinin `continue` deyimi içinde aynı olmalıdır `finally` engelle; Aksi takdirde bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-451">When a `continue` statement occurs within a `finally` block, the target of the `continue` statement must be within the same `finally` block; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="3c16d-452">A `continue` deyim şu şekilde gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-452">A `continue` statement is executed as follows:</span></span>

*  <span data-ttu-id="3c16d-453">Varsa `continue` deyimi, bir veya daha fazla çıkar `try` bloklarla ilişkili `finally` denetim blokları için başlangıçta aktarılır `finally` en içteki bloğunu `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-453">If the `continue` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="3c16d-454">Zaman ve denetim bitiş noktasını ulaşırsa bir `finally` denetim bloğu aktarılır `finally` sonraki kapsayan, blok `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-454">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="3c16d-455">Kadar bu işlem yinelenir `finally` tüm blokları müdahalede bulunan `try` deyimler yürütülür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-455">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="3c16d-456">Denetim hedefi için aktarılan `continue` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-456">Control is transferred to the target of the `continue` statement.</span></span>

<span data-ttu-id="3c16d-457">Çünkü bir `continue` deyimi koşulsuz aktarımları denetimi başka bir yerde bitiş noktasını bir `continue` deyimi erişilebilir hiçbir zaman.</span><span class="sxs-lookup"><span data-stu-id="3c16d-457">Because a `continue` statement unconditionally transfers control elsewhere, the end point of a `continue` statement is never reachable.</span></span>

### <a name="the-goto-statement"></a><span data-ttu-id="3c16d-458">Goto deyimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-458">The goto statement</span></span>

<span data-ttu-id="3c16d-459">`goto` Deyimi denetimi etiket işaretlenmiş bir deyim aktarır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-459">The `goto` statement transfers control to a statement that is marked by a label.</span></span>

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

<span data-ttu-id="3c16d-460">Hedefi bir `goto` *tanımlayıcı* etiketli deyim belirli bir etikete sahip bir ifadedir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-460">The target of a `goto` *identifier* statement is the labeled statement with the given label.</span></span> <span data-ttu-id="3c16d-461">Belirtilen ada sahip etiket geçerli işlev üye mevcut değilse veya `goto` deyimi değil etiketin kapsamında bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-461">If a label with the given name does not exist in the current function member, or if the `goto` statement is not within the scope of the label, a compile-time error occurs.</span></span> <span data-ttu-id="3c16d-462">Bu kural kullanımına izin verir. bir `goto` denetimi bir iç içe kapsam dışında ancak iç içe bir kapsam aktarmak için deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-462">This rule permits the use of a `goto` statement to transfer control out of a nested scope, but not into a nested scope.</span></span> <span data-ttu-id="3c16d-463">Örnekte</span><span class="sxs-lookup"><span data-stu-id="3c16d-463">In the example</span></span>
```csharp
using System;

class Test
{
    static void Main(string[] args) {
        string[,] table = {
            {"Red", "Blue", "Green"},
            {"Monday", "Wednesday", "Friday"}
        };

        foreach (string str in args) {
            int row, colm;
            for (row = 0; row <= 1; ++row)
                for (colm = 0; colm <= 2; ++colm)
                    if (str == table[row,colm])
                         goto done;

            Console.WriteLine("{0} not found", str);
            continue;
    done:
            Console.WriteLine("Found {0} at [{1}][{2}]", str, row, colm);
        }
    }
}
```
<span data-ttu-id="3c16d-464">bir `goto` deyimi, denetimi bir iç içe kapsam dışına aktarmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-464">a `goto` statement is used to transfer control out of a nested scope.</span></span>

<span data-ttu-id="3c16d-465">Hedefi bir `goto case` deyimi listesinde hemen kapsayan bir ifadedir `switch` deyimi ([switch deyimi](statements.md#the-switch-statement)), içeren bir `case` verilen sabit değerine sahip etiket.</span><span class="sxs-lookup"><span data-stu-id="3c16d-465">The target of a `goto case` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `case` label with the given constant value.</span></span> <span data-ttu-id="3c16d-466">Varsa `goto case` deyimi tarafından içine alınmayan bir `switch` ifadesi varsa *constant_expression* örtük olarak dönüştürülebilir değil ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) değerlendirip türüne en yakın kapsayan `switch` deyimi, veya en yakın kapsayan `switch` deyim içermiyor bir `case` etiket ile verilen sabit değer, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-466">If the `goto case` statement is not enclosed by a `switch` statement, if the *constant_expression* is not implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the nearest enclosing `switch` statement, or if the nearest enclosing `switch` statement does not contain a `case` label with the given constant value, a compile-time error occurs.</span></span>

<span data-ttu-id="3c16d-467">Hedefi bir `goto default` deyimi listesinde hemen kapsayan bir ifadedir `switch` deyimi ([switch deyimi](statements.md#the-switch-statement)), içeren bir `default` etiketi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-467">The target of a `goto default` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `default` label.</span></span> <span data-ttu-id="3c16d-468">Varsa `goto default` deyimi tarafından içine alınmayan bir `switch` deyimi, veya en yakın kapsayan `switch` deyim içermiyor bir `default` etiketi, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-468">If the `goto default` statement is not enclosed by a `switch` statement, or if the nearest enclosing `switch` statement does not contain a `default` label, a compile-time error occurs.</span></span>

<span data-ttu-id="3c16d-469">A `goto` olamaz exit deyimi bir `finally` blok ([try deyimi](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="3c16d-469">A `goto` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="3c16d-470">Olduğunda bir `goto` ifade oluştuğu yer bir `finally` bloğu hedefinin `goto` deyimi içinde aynı olmalıdır `finally` bloğu veya aksi halde bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-470">When a `goto` statement occurs within a `finally` block, the target of the `goto` statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="3c16d-471">A `goto` deyim şu şekilde gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-471">A `goto` statement is executed as follows:</span></span>

*  <span data-ttu-id="3c16d-472">Varsa `goto` deyimi, bir veya daha fazla çıkar `try` bloklarla ilişkili `finally` denetim blokları için başlangıçta aktarılır `finally` en içteki bloğunu `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-472">If the `goto` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="3c16d-473">Zaman ve denetim bitiş noktasını ulaşırsa bir `finally` denetim bloğu aktarılır `finally` sonraki kapsayan, blok `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-473">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="3c16d-474">Kadar bu işlem yinelenir `finally` tüm blokları müdahalede bulunan `try` deyimler yürütülür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-474">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="3c16d-475">Denetim hedefi için aktarılan `goto` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-475">Control is transferred to the target of the `goto` statement.</span></span>

<span data-ttu-id="3c16d-476">Çünkü bir `goto` deyimi koşulsuz aktarımları denetimi başka bir yerde bitiş noktasını bir `goto` deyimi erişilebilir hiçbir zaman.</span><span class="sxs-lookup"><span data-stu-id="3c16d-476">Because a `goto` statement unconditionally transfers control elsewhere, the end point of a `goto` statement is never reachable.</span></span>

### <a name="the-return-statement"></a><span data-ttu-id="3c16d-477">Return deyimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-477">The return statement</span></span>

<span data-ttu-id="3c16d-478">`return` Deyimi geçerli çağıran işlevin hangi denetim döndürür `return` ifadesi görünür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-478">The `return` statement returns control to the current caller of the function in which the `return` statement appears.</span></span>

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

<span data-ttu-id="3c16d-479">A `return` herhangi bir ifade deyimi yalnızca bir değer, diğer bir deyişle, bir yöntem sonuç türü ile bir işlem değil bir işlev üyesi kullanılabilir ([yöntem gövdesini](classes.md#method-body)) `void`, `set` erişimci özelliğin veya Dizin Oluşturucu, `add` ve `remove` erişicilerini geçersiz bir olay, bir örnek oluşturucusu, bir statik oluşturucu veya yıkıcı.</span><span class="sxs-lookup"><span data-stu-id="3c16d-479">A `return` statement with no expression can be used only in a function member that does not compute a value, that is, a method with the result type ([Method body](classes.md#method-body)) `void`, the `set` accessor of a property or indexer, the `add` and `remove` accessors of an event, an instance constructor, a static constructor, or a destructor.</span></span>

<span data-ttu-id="3c16d-480">A `return` bir ifade deyimi yalnızca kullanılabilir diğer bir deyişle, bir void olmayan sonuç türü olan bir yöntem bir değer hesaplayan bir işlevi üyesinde `get` erişimci bir özellik veya dizin oluşturucu veya bir kullanıcı tanımlı işleç.</span><span class="sxs-lookup"><span data-stu-id="3c16d-480">A `return` statement with an expression can only be used in a function member that computes a value, that is, a method with a non-void result type, the `get` accessor of a property or indexer, or a user-defined operator.</span></span> <span data-ttu-id="3c16d-481">Örtük bir dönüştürme ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) ifadesinin türünden içeren işlev üyenin dönüş türü için mevcut olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-481">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression to the return type of the containing function member.</span></span>

<span data-ttu-id="3c16d-482">Dönüş deyimleri anonim işlev ifadeleri gövdesinde da kullanılabilir ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) ve hangi dönüştürmeler için bu işlevleri mevcut saptanırken katılın.</span><span class="sxs-lookup"><span data-stu-id="3c16d-482">Return statements can also be used in the body of anonymous function expressions ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), and participate in determining which conversions exist for those functions.</span></span>

<span data-ttu-id="3c16d-483">Bir derleme zamanı hata için bir `return` görünmesini deyimi bir `finally` blok ([try deyimi](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="3c16d-483">It is a compile-time error for a `return` statement to appear in a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="3c16d-484">A `return` deyim şu şekilde gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-484">A `return` statement is executed as follows:</span></span>

*  <span data-ttu-id="3c16d-485">Varsa `return` belirten bir ifade deyimi ifade değerlendirilir ve sonuç değerini içeren işlevin dönüş türüne örtük bir dönüştürme dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-485">If the `return` statement specifies an expression, the expression is evaluated and the resulting value is converted to the return type of the containing function by an implicit conversion.</span></span> <span data-ttu-id="3c16d-486">Dönüştürmenin sonucu işlev tarafından üretilen sonuç değeri olur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-486">The result of the conversion becomes the result value produced by the function.</span></span>
*  <span data-ttu-id="3c16d-487">Varsa `return` deyimi bir veya daha fazla içine `try` veya `catch` bloklarla ilişkili `finally` denetim blokları için başlangıçta aktarılır `finally` en içteki bloğunu `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-487">If the `return` statement is enclosed by one or more `try` or `catch` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="3c16d-488">Zaman ve denetim bitiş noktasını ulaşırsa bir `finally` denetim bloğu aktarılır `finally` sonraki kapsayan, blok `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-488">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="3c16d-489">Kadar bu işlem yinelenir `finally` tüm kapsayan, bloklarını `try` deyimler yürütülür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-489">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="3c16d-490">İçeren işlev bir zaman uyumsuz işlev değilse, Denetim çağırana içeren işlevin sonucu değerin yanı sıra varsa döndürülür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-490">If the containing function is not an async function, control is returned to the caller of the containing function along with the result value, if any.</span></span>
*  <span data-ttu-id="3c16d-491">İçeren işlev bir zaman uyumsuz işlev ise, Denetim, geçerli çağırana döndürülür ve sonuç değeri varsa, dönüş görevde açıklandığı kaydedilir ([Numaralandırıcı arabirimleri](classes.md#enumerator-interfaces)).</span><span class="sxs-lookup"><span data-stu-id="3c16d-491">If the containing function is an async function, control is returned to the current caller, and the result value, if any, is recorded in the return task as described in ([Enumerator interfaces](classes.md#enumerator-interfaces)).</span></span>

<span data-ttu-id="3c16d-492">Çünkü bir `return` deyimi koşulsuz aktarımları denetimi başka bir yerde bitiş noktasını bir `return` deyimi erişilebilir hiçbir zaman.</span><span class="sxs-lookup"><span data-stu-id="3c16d-492">Because a `return` statement unconditionally transfers control elsewhere, the end point of a `return` statement is never reachable.</span></span>

### <a name="the-throw-statement"></a><span data-ttu-id="3c16d-493">Throw deyimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-493">The throw statement</span></span>

<span data-ttu-id="3c16d-494">`throw` Deyimi bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-494">The `throw` statement throws an exception.</span></span>

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

<span data-ttu-id="3c16d-495">A `throw` değeri ifade tarafından değerlendirilen bir ifade deyimi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-495">A `throw` statement with an expression throws the value produced by evaluating the expression.</span></span> <span data-ttu-id="3c16d-496">İfade sınıf türünde bir değer belirtmek gerekir `System.Exception`, türetilen bir sınıf türünün `System.Exception` veya olan bir tür parametresi türü `System.Exception` (veya bir alt yapanın) etkili temel sınıfı olarak.</span><span class="sxs-lookup"><span data-stu-id="3c16d-496">The expression must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="3c16d-497">İfadenin değerlendirilmesi oluşturursa `null`, `System.NullReferenceException` yerine oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-497">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="3c16d-498">A `throw` herhangi bir ifade deyimi yalnızca kullanılabilir bir `catch` bloğunda bu durumda bu deyimi tarafından işlenmekte olan özel durumu yeniden oluşturur `catch` blok.</span><span class="sxs-lookup"><span data-stu-id="3c16d-498">A `throw` statement with no expression can be used only in a `catch` block, in which case that statement re-throws the exception that is currently being handled by that `catch` block.</span></span>

<span data-ttu-id="3c16d-499">Çünkü bir `throw` deyimi koşulsuz aktarımları denetimi başka bir yerde bitiş noktasını bir `throw` deyimi erişilebilir hiçbir zaman.</span><span class="sxs-lookup"><span data-stu-id="3c16d-499">Because a `throw` statement unconditionally transfers control elsewhere, the end point of a `throw` statement is never reachable.</span></span>

<span data-ttu-id="3c16d-500">Bir özel durum oluştuğunda denetim ilk aktarılır `catch` kapsayan içinde yan tümcesi `try` özel durumu işleyebilecek deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-500">When an exception is thrown, control is transferred to the first `catch` clause in an enclosing `try` statement that can handle the exception.</span></span> <span data-ttu-id="3c16d-501">Denetimi uygun özel durum işleyicisine aktarma noktasına oluşturulan özel durumun noktasından gerçekleşmeden işlemi olarak bilinir ***özel durum yayma***.</span><span class="sxs-lookup"><span data-stu-id="3c16d-501">The process that takes place from the point of the exception being thrown to the point of transferring control to a suitable exception handler is known as ***exception propagation***.</span></span> <span data-ttu-id="3c16d-502">Bir özel durum yayma oluşan aşağıdaki adımları kadar sürekli olarak değerlendirme bir `catch` eşleşen özel durum yan tümcesi bulundu.</span><span class="sxs-lookup"><span data-stu-id="3c16d-502">Propagation of an exception consists of repeatedly evaluating the following steps until a `catch` clause that matches the exception is found.</span></span> <span data-ttu-id="3c16d-503">Bu açıklama ***noktası throw*** başlangıçta özel durum oluşturulur konumdur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-503">In this description, the ***throw point*** is initially the location at which the exception is thrown.</span></span>

*  <span data-ttu-id="3c16d-504">Geçerli işlev üyesinde her `try` throw noktası kapsayan bir deyim incelenir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-504">In the current function member, each `try` statement that encloses the throw point is examined.</span></span> <span data-ttu-id="3c16d-505">For each deyimi `S`en içteki ile başlayan `try` deyimi ve en dıştaki görmektesiniz `try` deyimi, aşağıdaki adımları değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-505">For each statement `S`, starting with the innermost `try` statement and ending with the outermost `try` statement, the following steps are evaluated:</span></span>

   * <span data-ttu-id="3c16d-506">Varsa `try` bloğu `S` throw noktası barındırır ve S bir veya daha fazla `catch` yan tümceleri, `catch` yan tümceleri içinde belirtilen kurallara göre özel durum için uygun bir işleyici bulmaya sırada görünümünü incelenir Bölüm [try deyimi](statements.md#the-try-statement).</span><span class="sxs-lookup"><span data-stu-id="3c16d-506">If the `try` block of `S` encloses the throw point and if S has one or more `catch` clauses, the `catch` clauses are examined in order of appearance to locate a suitable handler for the exception, according to the rules specified in Section [The try statement](statements.md#the-try-statement).</span></span> <span data-ttu-id="3c16d-507">Eşleşen bir `catch` yan tümcesi bulunduğu, özel durum yayma denetimi, bloğunu aktararak tamamlanmış `catch` yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-507">If a matching `catch` clause is located, the exception propagation is completed by transferring control to the block of that `catch` clause.</span></span>

   * <span data-ttu-id="3c16d-508">Aksi takdirde `try` blok veya `catch` bloğu `S` throw noktası barındırır ve `S` sahip bir `finally` denetim bloğu aktarılır `finally` blok.</span><span class="sxs-lookup"><span data-stu-id="3c16d-508">Otherwise, if the `try` block or a `catch` block of `S` encloses the throw point and if `S` has a `finally` block, control is transferred to the `finally` block.</span></span> <span data-ttu-id="3c16d-509">Varsa `finally` blok başka bir özel durum oluşturursa, geçerli özel durumun işlenmesini sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-509">If the `finally` block throws another exception, processing of the current exception is terminated.</span></span> <span data-ttu-id="3c16d-510">Aksi takdirde denetim bitiş noktasını ulaştığında `finally` blok, geçerli özel durum işleme devam edilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-510">Otherwise, when control reaches the end point of the `finally` block, processing of the current exception is continued.</span></span>

*  <span data-ttu-id="3c16d-511">Geçerli işlev çağrısını bir özel durum işleyicisi bulunan değil, işlev çağrısını sonlandırılır ve aşağıdakilerden biri gerçekleşir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-511">If an exception handler was not located in the current function invocation, the function invocation is terminated, and one of the following occurs:</span></span>

   * <span data-ttu-id="3c16d-512">Geçerli işlev olmayan zaman uyumsuz ise, yukarıdaki adımları için işlev çağıran işlevi üyenin çağrıldığı ifadesine karşılık gelen bir throw noktasıyla yinelenir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-512">If the current function is non-async, the steps above are repeated for the caller of the function with a throw point corresponding to the statement from which the function member was invoked.</span></span>

   * <span data-ttu-id="3c16d-513">Geçerli işlev zaman uyumsuz ve görev döndüren ise özel durum açıklandığı bir hatalı veya iptal edilmiş duruma getirilir dönüş görev kaydedilir [Numaralandırıcı arabirimleri](classes.md#enumerator-interfaces).</span><span class="sxs-lookup"><span data-stu-id="3c16d-513">If the current function is async and task-returning, the exception is recorded in the return task, which is put into a faulted or cancelled state as described in [Enumerator interfaces](classes.md#enumerator-interfaces).</span></span>

   * <span data-ttu-id="3c16d-514">Geçerli işlev zaman uyumsuz ve void döndüren ise, geçerli iş parçacığının senkronizasyon bağlamı açıklandığı gibi bildirilir [numaralandırılabilir arabirimler](classes.md#enumerable-interfaces).</span><span class="sxs-lookup"><span data-stu-id="3c16d-514">If the current function is async and void-returning, the synchronization context of the current thread is notified as described in [Enumerable interfaces](classes.md#enumerable-interfaces).</span></span>

*  <span data-ttu-id="3c16d-515">Tüm işlev üyesi çağrılarını geçerli iş parçacığında özel durum işleme sona ererse, iş parçacığı için özel durum işleyici yok olduğunu belirten ardından kendisini sonlandırıldı iş parçacığıdır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-515">If the exception processing terminates all function member invocations in the current thread, indicating that the thread has no handler for the exception, then the thread is itself terminated.</span></span> <span data-ttu-id="3c16d-516">Bu tür sonlandırma uygulama tanımlı etkisidir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-516">The impact of such termination is implementation-defined.</span></span>

## <a name="the-try-statement"></a><span data-ttu-id="3c16d-517">Try deyimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-517">The try statement</span></span>

<span data-ttu-id="3c16d-518">`try` Deyim bloğu yürütülmesi sırasında oluşan özel durumları yakalama için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="3c16d-518">The `try` statement provides a mechanism for catching exceptions that occur during execution of a block.</span></span> <span data-ttu-id="3c16d-519">Ayrıca, `try` deyimi, denetimi terk ettiğinde, her zaman yürütülür bir kod bloğunu belirtme olanağı sağlar `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-519">Furthermore, the `try` statement provides the ability to specify a block of code that is always executed when control leaves the `try` statement.</span></span>

```antlr
try_statement
    : 'try' block catch_clause+
    | 'try' block finally_clause
    | 'try' block catch_clause+ finally_clause
    ;

catch_clause
    : 'catch' exception_specifier? exception_filter?  block
    ;

exception_specifier
    : '(' type identifier? ')'
    ;

exception_filter
    : 'when' '(' expression ')'
    ;

finally_clause
    : 'finally' block
    ;
```

<span data-ttu-id="3c16d-520">Üç olası biçimi vardır `try` ifadeleri:</span><span class="sxs-lookup"><span data-stu-id="3c16d-520">There are three possible forms of `try` statements:</span></span>

*  <span data-ttu-id="3c16d-521">A `try` blok izlenen bir veya daha fazla tarafından `catch` engeller.</span><span class="sxs-lookup"><span data-stu-id="3c16d-521">A `try` block followed by one or more `catch` blocks.</span></span>
*  <span data-ttu-id="3c16d-522">A `try` blok arkasından bir `finally` blok.</span><span class="sxs-lookup"><span data-stu-id="3c16d-522">A `try` block followed by a `finally` block.</span></span>
*  <span data-ttu-id="3c16d-523">A `try` blok izlenen bir veya daha fazla tarafından `catch` blokları arkasından bir `finally` blok.</span><span class="sxs-lookup"><span data-stu-id="3c16d-523">A `try` block followed by one or more `catch` blocks followed by a `finally` block.</span></span>

<span data-ttu-id="3c16d-524">Olduğunda bir `catch` yan tümcesi belirtir bir *exception_specifier*, türü olmalıdır `System.Exception`, türetilen bir tür `System.Exception` ya da olan bir tür parametresi türü `System.Exception` (veya bir alt yapanın), etkin olarak temel sınıf.</span><span class="sxs-lookup"><span data-stu-id="3c16d-524">When a `catch` clause specifies an *exception_specifier*, the type must be `System.Exception`, a type that derives from `System.Exception` or a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span>

<span data-ttu-id="3c16d-525">Olduğunda bir `catch` yan tümcesi hem belirtir bir *exception_specifier* ile bir *tanımlayıcı*, bir ***özel değişken*** verilen ad ve tür bildirilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-525">When a `catch` clause specifies both an *exception_specifier* with an *identifier*, an ***exception variable*** of the given name and type is declared.</span></span> <span data-ttu-id="3c16d-526">Özel durum değişkeni üzerinden genişleten bir kapsama sahip bir yerel değişken için karşılık gelen `catch` yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-526">The exception variable corresponds to a local variable with a scope that extends over the `catch` clause.</span></span> <span data-ttu-id="3c16d-527">Yürütülmesi sırasında *exception_filter* ve *blok*, özel durum değişkeni işlenmekte olan özel durumu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="3c16d-527">During execution of the *exception_filter* and *block*, the exception variable represents the exception currently being handled.</span></span> <span data-ttu-id="3c16d-528">Belirli atama onayına denetleme amaçları kesinlikle tüm kapsamında atanan özel değişken olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-528">For purposes of definite assignment checking, the exception variable is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="3c16d-529">Sürece bir `catch` yan tümcesi içeren bir özel durum değişken adı, özel durum nesnesi filtredeki erişmek mümkün değildir ve `catch` blok.</span><span class="sxs-lookup"><span data-stu-id="3c16d-529">Unless a `catch` clause includes an exception variable name, it is impossible to access the exception object in the filter and `catch` block.</span></span>

<span data-ttu-id="3c16d-530">A `catch` belirttiğinde yan tümcesi bir *exception_specifier* genel olarak adlandırılan `catch` yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-530">A `catch` clause that does not specify an *exception_specifier* is called a general `catch` clause.</span></span>

<span data-ttu-id="3c16d-531">Bazı programlama dilleri, türetilen bir nesne olarak gösterilebilir değil özel durumları destekleyebilir `System.Exception`, ancak bu tür özel durumların C# kodu tarafından asla oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-531">Some programming languages may support exceptions that are not representable as an object derived from `System.Exception`, although such exceptions could never be generated by C# code.</span></span> <span data-ttu-id="3c16d-532">Genel `catch` yan tümcesi, bu tür özel durumları yakalamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-532">A general `catch` clause may be used to catch such exceptions.</span></span> <span data-ttu-id="3c16d-533">Bu nedenle, genel `catch` yan tümcesi ise türünü belirten bir anlamsal olarak farklı `System.Exception`bu eski ayrıca diğer dillerdeki özel durumlara catch.</span><span class="sxs-lookup"><span data-stu-id="3c16d-533">Thus, a general `catch` clause is semantically different from one that specifies the type `System.Exception`, in that the former may also catch exceptions from other languages.</span></span>

<span data-ttu-id="3c16d-534">Bir özel durum işleyicisi bulmak için `catch` yan tümceleri sözcük sırayla incelenir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-534">In order to locate a handler for an exception, `catch` clauses are examined in lexical order.</span></span> <span data-ttu-id="3c16d-535">Varsa bir `catch` yan tümcesi, bir tür, ancak hiçbir özel durum filtresi belirtir, bir derleme zamanı hatası için bir sonraki `catch` yan tümcesinde aynı `try` ifade türü bir aynı veya ondan türetilmiş bir tür belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="3c16d-535">If a `catch` clause specifies a type but no exception filter, it is a compile-time error for a later `catch` clause in the same `try` statement to specify a type that is the same as, or is derived from, that type.</span></span> <span data-ttu-id="3c16d-536">Varsa bir `catch` hiçbir türü ve filtre yan tümcesi belirtir, son olmalıdır `catch` söz konusu yan tümcesi `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-536">If a `catch` clause specifies no type and no filter, it must be the last `catch` clause for that `try` statement.</span></span>

<span data-ttu-id="3c16d-537">İçinde bir `catch` bloğu bir `throw` deyimi ([fırlatma](statements.md#the-throw-statement)) tarafından yakalanan özel durumu yeniden harekete geçirileceğini herhangi bir ifade ile kullanılan `catch` blok.</span><span class="sxs-lookup"><span data-stu-id="3c16d-537">Within a `catch` block, a `throw` statement ([The throw statement](statements.md#the-throw-statement)) with no expression can be used to re-throw the exception that was caught by the `catch` block.</span></span> <span data-ttu-id="3c16d-538">Bir özel durum değişken atamaları yeniden oluşturulan özel durum değiştirmeyin.</span><span class="sxs-lookup"><span data-stu-id="3c16d-538">Assignments to an exception variable do not alter the exception that is re-thrown.</span></span>

<span data-ttu-id="3c16d-539">Örnekte</span><span class="sxs-lookup"><span data-stu-id="3c16d-539">In the example</span></span>
```csharp
using System;

class Test
{
    static void F() {
        try {
            G();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in F: " + e.Message);
            e = new Exception("F");
            throw;                // re-throw
        }
    }

    static void G() {
        throw new Exception("G");
    }

    static void Main() {
        try {
            F();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in Main: " + e.Message);
        }
    }
}
```
<span data-ttu-id="3c16d-540">yöntem `F` bir özel durumu yakalar, bazı tanı bilgilerini konsola yazar, özel durum değişkeni değiştirir ve özel durum yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-540">the method `F` catches an exception, writes some diagnostic information to the console, alters the exception variable, and re-throws the exception.</span></span> <span data-ttu-id="3c16d-541">Yeniden oluşturulan özel durum özgün özel durum olduğundan, oluşturulan çıktı şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-541">The exception that is re-thrown is the original exception, so the output produced is:</span></span>
```
Exception in F: G
Exception in Main: G
```

<span data-ttu-id="3c16d-542">Eğer ilk catch bloğu harekete `e` geçerli özel durumun yeniden atma yerine oluşturulan çıktı şu şekilde olacaktır:</span><span class="sxs-lookup"><span data-stu-id="3c16d-542">If the first catch block had thrown `e` instead of rethrowing the current exception, the output produced would be as follows:</span></span>
```csharp
Exception in F: G
Exception in Main: F
```

<span data-ttu-id="3c16d-543">Bir derleme zamanı hata için bir `break`, `continue`, veya `goto` kontrolünü dışarı aktarmak için deyimi bir `finally` blok.</span><span class="sxs-lookup"><span data-stu-id="3c16d-543">It is a compile-time error for a `break`, `continue`, or `goto` statement to transfer control out of a `finally` block.</span></span> <span data-ttu-id="3c16d-544">Olduğunda bir `break`, `continue`, veya `goto` deyimi oluşuyor bir `finally` deyiminin hedefi bloğu içinde aynı olmalıdır `finally` bloğu veya aksi halde bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-544">When a `break`, `continue`, or `goto` statement occurs in a `finally` block, the target of the statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="3c16d-545">Bir derleme zamanı hata için bir `return` deyimini oluşan bir `finally` blok.</span><span class="sxs-lookup"><span data-stu-id="3c16d-545">It is a compile-time error for a `return` statement to occur in a `finally` block.</span></span>

<span data-ttu-id="3c16d-546">A `try` deyim şu şekilde gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-546">A `try` statement is executed as follows:</span></span>

*  <span data-ttu-id="3c16d-547">Denetim aktarılır `try` blok.</span><span class="sxs-lookup"><span data-stu-id="3c16d-547">Control is transferred to the `try` block.</span></span>
*  <span data-ttu-id="3c16d-548">Zaman ve denetim bitiş noktasını ulaşırsa `try` engelle:</span><span class="sxs-lookup"><span data-stu-id="3c16d-548">When and if control reaches the end point of the `try` block:</span></span>
   *  <span data-ttu-id="3c16d-549">Varsa `try` deyimi bir `finally` bloğu `finally` bloğu yürütülür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-549">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
   *  <span data-ttu-id="3c16d-550">Denetim için bitiş noktasını aktarılan `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-550">Control is transferred to the end point of the `try` statement.</span></span>

*  <span data-ttu-id="3c16d-551">Bir özel durum yayılır, `try` deyimi yürütülürken `try` engelle:</span><span class="sxs-lookup"><span data-stu-id="3c16d-551">If an exception is propagated to the `try` statement during execution of the `try` block:</span></span>
   *  <span data-ttu-id="3c16d-552">`catch` Yan tümcesi varsa, sırayla incelenir uygun bir özel durum işleyicisi bulunacak görünümünü.</span><span class="sxs-lookup"><span data-stu-id="3c16d-552">The `catch` clauses, if any, are examined in order of appearance to locate a suitable handler for the exception.</span></span> <span data-ttu-id="3c16d-553">Varsa bir `catch` yan tümcesi bir tür belirtmiyor veya özel durum türü veya temel türde bir özel durum türünü belirtir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-553">If a `catch` clause does not specify a type, or specifies the exception type or a base type of the exception type:</span></span>
      *  <span data-ttu-id="3c16d-554">Varsa `catch` yan tümcesi bir özel durum değişkenini bildirir, özel durum nesnesi özel durum değişkenine atanır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-554">If the `catch` clause declares an exception variable, the exception object is assigned to the exception variable.</span></span>
      *  <span data-ttu-id="3c16d-555">Varsa `catch` yan tümcesi bir özel durum filtresi bildirir, filtre değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-555">If the `catch` clause declares an exception filter, the filter is evaluated.</span></span> <span data-ttu-id="3c16d-556">Değerlendirilirse `false`catch yan tümcesi bir eşleşme değil ve arama herhangi biri ile devam eder. sonraki `catch` yan tümceleri uygun bir işleyici.</span><span class="sxs-lookup"><span data-stu-id="3c16d-556">If it evaluates to `false`, the catch clause is not a match, and the search continues through any subsequent `catch` clauses for a suitable handler.</span></span>
      *  <span data-ttu-id="3c16d-557">Aksi takdirde, `catch` yan tümcesi bir eşleşme olarak kabul edilir ve denetim, eşleşen aktarılır `catch` blok.</span><span class="sxs-lookup"><span data-stu-id="3c16d-557">Otherwise, the `catch` clause is considered a match, and control is transferred to the matching `catch` block.</span></span>
      *  <span data-ttu-id="3c16d-558">Zaman ve denetim bitiş noktasını ulaşırsa `catch` engelle:</span><span class="sxs-lookup"><span data-stu-id="3c16d-558">When and if control reaches the end point of the `catch` block:</span></span>
         * <span data-ttu-id="3c16d-559">Varsa `try` deyimi bir `finally` bloğu `finally` bloğu yürütülür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-559">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         * <span data-ttu-id="3c16d-560">Denetim için bitiş noktasını aktarılan `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-560">Control is transferred to the end point of the `try` statement.</span></span>
      *  <span data-ttu-id="3c16d-561">Bir özel durum yayılır, `try` deyimi yürütülürken `catch` engelle:</span><span class="sxs-lookup"><span data-stu-id="3c16d-561">If an exception is propagated to the `try` statement during execution of the `catch` block:</span></span>
         *  <span data-ttu-id="3c16d-562">Varsa `try` deyimi bir `finally` bloğu `finally` bloğu yürütülür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-562">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         *  <span data-ttu-id="3c16d-563">Özel durum sonraki kapsayan yayılır `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-563">The exception is propagated to the next enclosing `try` statement.</span></span>
   *  <span data-ttu-id="3c16d-564">Varsa `try` deyimi yok `catch` yan tümceleri veya hiçbir `catch` eşleşiyorsa bir özel durum:</span><span class="sxs-lookup"><span data-stu-id="3c16d-564">If the `try` statement has no `catch` clauses or if no `catch` clause matches the exception:</span></span>
      *  <span data-ttu-id="3c16d-565">Varsa `try` deyimi bir `finally` bloğu `finally` bloğu yürütülür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-565">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
      *  <span data-ttu-id="3c16d-566">Özel durum sonraki kapsayan yayılır `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-566">The exception is propagated to the next enclosing `try` statement.</span></span>

<span data-ttu-id="3c16d-567">Deyimleri bir `finally` denetimden çıktığında, blok yürütülen her zaman bir `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-567">The statements of a `finally` block are always executed when control leaves a `try` statement.</span></span> <span data-ttu-id="3c16d-568">Olup yürütülmesi sonucunda normal yürütmenin, sonucunda denetim aktarımı gerçekleşir, böyle bir `break`, `continue`, `goto`, veya `return` deyimi veya bir özel durumun yayılmasını sonucunda `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-568">This is true whether the control transfer occurs as a result of normal execution, as a result of executing a `break`, `continue`, `goto`, or `return` statement, or as a result of propagating an exception out of the `try` statement.</span></span>

<span data-ttu-id="3c16d-569">Yürütülmesi sırasında bir özel durum oluşturulursa bir `finally` blok ve yakalanan değil aynı içinde finally bloğu, özel durum sonraki kapsayan yayılır `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-569">If an exception is thrown during execution of a `finally` block, and is not caught within the same finally block, the exception is propagated to the next enclosing `try` statement.</span></span> <span data-ttu-id="3c16d-570">Başka bir özel durum yayılmaz sürecinde olduysa, o özel durumu kaybolur.</span><span class="sxs-lookup"><span data-stu-id="3c16d-570">If another exception was in the process of being propagated, that exception is lost.</span></span> <span data-ttu-id="3c16d-571">Bir özel durum yayma işleminin ele alınan ayrıntılı açıklamasını olarak `throw` deyimi ([fırlatma](statements.md#the-throw-statement)).</span><span class="sxs-lookup"><span data-stu-id="3c16d-571">The process of propagating an exception is discussed further in the description of the `throw` statement ([The throw statement](statements.md#the-throw-statement)).</span></span>

<span data-ttu-id="3c16d-572">`try` Bloğu bir `try` deyimi erişilebilir değilse `try` deyimi erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-572">The `try` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="3c16d-573">A `catch` bloğu bir `try` deyimi erişilebilir değilse `try` deyimi erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-573">A `catch` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="3c16d-574">`finally` Bloğu bir `try` deyimi erişilebilir değilse `try` deyimi erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-574">The `finally` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="3c16d-575">Bitiş noktasını bir `try` ifadesi aşağıdakilerin her ikisi de doğruysa erişilebilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-575">The end point of a `try` statement is reachable if both of the following are true:</span></span>

*  <span data-ttu-id="3c16d-576">Bitiş noktasını `try` blok erişilebilir olduğundan veya en az biri son noktası `catch` blok erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-576">The end point of the `try` block is reachable or the end point of at least one `catch` block is reachable.</span></span>
*  <span data-ttu-id="3c16d-577">Varsa bir `finally` blok mevcutsa, bitiş noktasını `finally` blok erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-577">If a `finally` block is present, the end point of the `finally` block is reachable.</span></span>

## <a name="the-checked-and-unchecked-statements"></a><span data-ttu-id="3c16d-578">Checked ve unchecked deyimleri</span><span class="sxs-lookup"><span data-stu-id="3c16d-578">The checked and unchecked statements</span></span>

<span data-ttu-id="3c16d-579">`checked` Ve `unchecked` deyimleri denetlemek için kullanılan ***taşma denetimi bağlamını*** Tamsayı türünde aritmetik işlemler ve dönüştürmeler için.</span><span class="sxs-lookup"><span data-stu-id="3c16d-579">The `checked` and `unchecked` statements are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

<span data-ttu-id="3c16d-580">`checked` Deyimi neden olan tüm ifadeler *blok* denetlenen bir bağlamda değerlendirilecek ve `unchecked` deyimi neden olan tüm ifadeler *blok* uyumluluğunun değerlendirilebilmesi için bir denetlenmeyen bağlamı.</span><span class="sxs-lookup"><span data-stu-id="3c16d-580">The `checked` statement causes all expressions in the *block* to be evaluated in a checked context, and the `unchecked` statement causes all expressions in the *block* to be evaluated in an unchecked context.</span></span>

<span data-ttu-id="3c16d-581">`checked` Ve `unchecked` ifadeleri tam olarak eşdeğer `checked` ve `unchecked` işleçleri ([checked ve unchecked işleçleri](expressions.md#the-checked-and-unchecked-operators)) dışında bunlar bloklarındaki ifadeler yerine çalışır .</span><span class="sxs-lookup"><span data-stu-id="3c16d-581">The `checked` and `unchecked` statements are precisely equivalent to the `checked` and `unchecked` operators ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), except that they operate on blocks instead of expressions.</span></span>

## <a name="the-lock-statement"></a><span data-ttu-id="3c16d-582">Lock deyimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-582">The lock statement</span></span>

<span data-ttu-id="3c16d-583">`lock` Deyimi belirli bir nesne için karşılıklı dışlama kilidini alır, bir deyimi yürütür ve ardından kilidi serbest bırakır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-583">The `lock` statement obtains the mutual-exclusion lock for a given object, executes a statement, and then releases the lock.</span></span>

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

<span data-ttu-id="3c16d-584">İfade bir `lock` deyimi olarak bilinen bir türde bir değer belirtmek gerekir bir *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="3c16d-584">The expression of a `lock` statement must denote a value of a type known to be a *reference_type*.</span></span> <span data-ttu-id="3c16d-585">Örtük kutulama dönüştürme ([kutulama dönüştürmeler](conversions.md#boxing-conversions)) ifadesi için hiç olmadığı kadar gerçekleştirilen bir `lock` deyimi ve bu nedenle bu, ifade değeri belirtmek için bir derleme zamanı hatası bir *value_type*.</span><span class="sxs-lookup"><span data-stu-id="3c16d-585">No implicit boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)) is ever performed for the expression of a `lock` statement, and thus it is a compile-time error for the expression to denote a value of a *value_type*.</span></span>

<span data-ttu-id="3c16d-586">A `lock` formun deyimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-586">A `lock` statement of the form</span></span>
```csharp
lock (x) ...
```
<span data-ttu-id="3c16d-587">Burada `x` bir ifade olan bir *reference_type*, tam olarak eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="3c16d-587">where `x` is an expression of a *reference_type*, is precisely equivalent to</span></span>
```csharp
bool __lockWasTaken = false;
try {
    System.Threading.Monitor.Enter(x, ref __lockWasTaken);
    ...
}
finally {
    if (__lockWasTaken) System.Threading.Monitor.Exit(x);
}
```
<span data-ttu-id="3c16d-588">dışında `x` yalnızca bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-588">except that `x` is only evaluated once.</span></span>

<span data-ttu-id="3c16d-589">Karşılıklı dışlama kilidi açık tutulduğu sürece, kod içinde aynı yürütme iş parçacığını yürüten de alabilir ve kilidi serbest bırakır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-589">While a mutual-exclusion lock is held, code executing in the same execution thread can also obtain and release the lock.</span></span> <span data-ttu-id="3c16d-590">Ancak, diğer iş parçacıkları kodu yürüten kilidi serbest bırakılıncaya kadar kilitlemeye karşı engellenir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-590">However, code executing in other threads is blocked from obtaining the lock until the lock is released.</span></span>

<span data-ttu-id="3c16d-591">Kilitleme `System.Type` nesneleri statik veri erişimi eşitlemek için önerilmez.</span><span class="sxs-lookup"><span data-stu-id="3c16d-591">Locking `System.Type` objects in order to synchronize access to static data is not recommended.</span></span> <span data-ttu-id="3c16d-592">Diğer kod kilitlenmeyle sonuçlanabilir aynı tür kilitleyebilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-592">Other code might lock on the same type, which can result in deadlock.</span></span> <span data-ttu-id="3c16d-593">Özel statik nesne kilitleyerek statik veri erişimi eşitleme daha iyi bir yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-593">A better approach is to synchronize access to static data by locking a private static object.</span></span> <span data-ttu-id="3c16d-594">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3c16d-594">For example:</span></span>
```csharp
class Cache
{
    private static readonly object synchronizationObject = new object();

    public static void Add(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }

    public static void Remove(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }
}
```

## <a name="the-using-statement"></a><span data-ttu-id="3c16d-595">Using deyimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-595">The using statement</span></span>

<span data-ttu-id="3c16d-596">`using` Deyimi bir veya daha fazla kaynağı alır, bir deyimi yürütür ve ardından kaynağı siler.</span><span class="sxs-lookup"><span data-stu-id="3c16d-596">The `using` statement obtains one or more resources, executes a statement, and then disposes of the resource.</span></span>

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

<span data-ttu-id="3c16d-597">A ***kaynak*** sınıfı veya uygulayan yapı `System.IDisposable`, adlı tek bir parametresiz yöntemin içeren `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-597">A ***resource*** is a class or struct that implements `System.IDisposable`, which includes a single parameterless method named `Dispose`.</span></span> <span data-ttu-id="3c16d-598">Bir kaynağı kullanarak kodu çağırabilir `Dispose` kaynak artık gerekli olduğunu göstermek için.</span><span class="sxs-lookup"><span data-stu-id="3c16d-598">Code that is using a resource can call `Dispose` to indicate that the resource is no longer needed.</span></span> <span data-ttu-id="3c16d-599">Varsa `Dispose` otomatik çıkarma söz konusu kümelerdeki çöp toplamanın sonunda oluşursa, çağrılmaz.</span><span class="sxs-lookup"><span data-stu-id="3c16d-599">If `Dispose` is not called, then automatic disposal eventually occurs as a consequence of garbage collection.</span></span>

<span data-ttu-id="3c16d-600">Varsa biçiminde *resource_acquisition* olan *local_variable_declaration* türü *local_variable_declaration* olmalıdır `dynamic` veya türü örtük olarak dönüştürülebilir `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-600">If the form of *resource_acquisition* is *local_variable_declaration* then the type of the *local_variable_declaration* must be either `dynamic` or a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="3c16d-601">Varsa biçiminde *resource_acquisition* olduğu *ifade* Bu ifade için örtük olarak dönüştürülebilir olmalıdır `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-601">If the form of *resource_acquisition* is *expression* then this expression must be implicitly convertible to `System.IDisposable`.</span></span>

<span data-ttu-id="3c16d-602">Bildirilen yerel değişken bir *resource_acquisition* salt okunurdur ve Başlatıcı içermelidir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-602">Local variables declared in a *resource_acquisition* are read-only, and must include an initializer.</span></span> <span data-ttu-id="3c16d-603">Gömülü deyim, bu yerel değişkenleri değiştirme girişiminde bulunursa, bir derleme zamanı hatası oluşur (atama aracılığıyla veya `++` ve `--` işleçleri) adresi alınamaz veya olarak geçirileceğini `ref` veya `out` parametreleri.</span><span class="sxs-lookup"><span data-stu-id="3c16d-603">A compile-time error occurs if the embedded statement attempts to modify these local variables (via assignment or the `++` and `--` operators) , take the address of them, or pass them as `ref` or `out` parameters.</span></span>

<span data-ttu-id="3c16d-604">A `using` deyimi, üç bölüme çevrilir: alım, kullanım ve silinecek.</span><span class="sxs-lookup"><span data-stu-id="3c16d-604">A `using` statement is translated into three parts: acquisition, usage, and disposal.</span></span> <span data-ttu-id="3c16d-605">Kaynak kullanımını içine örtük olarak bir `try` içeren deyimi bir `finally` yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-605">Usage of the resource is implicitly enclosed in a `try` statement that includes a `finally` clause.</span></span> <span data-ttu-id="3c16d-606">Bu `finally` yan tümcesi, kaynağın siler.</span><span class="sxs-lookup"><span data-stu-id="3c16d-606">This `finally` clause disposes of the resource.</span></span> <span data-ttu-id="3c16d-607">Varsa bir `null` kaynak alınan, ardından hiçbir çağrı `Dispose` yapılmış ve hiçbir özel durum.</span><span class="sxs-lookup"><span data-stu-id="3c16d-607">If a `null` resource is acquired, then no call to `Dispose` is made, and no exception is thrown.</span></span> <span data-ttu-id="3c16d-608">Kaynak türü ise `dynamic` örtük bir dinamik dönüştürme dinamik olarak dönüştürülür ([dinamik örtük dönüştürmelerin](conversions.md#implicit-dynamic-conversions)) için `IDisposable` dönüştürme olmasını sağlamak için alma işlemi sırasında Kullanım ve elden önce başarılı.</span><span class="sxs-lookup"><span data-stu-id="3c16d-608">If the resource is of type `dynamic` it is dynamically converted through an implicit dynamic conversion ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)) to `IDisposable` during acquisition in order to ensure that the conversion is successful before the usage and disposal.</span></span>

<span data-ttu-id="3c16d-609">A `using` formun deyimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-609">A `using` statement of the form</span></span>
```csharp
using (ResourceType resource = expression) statement
```
<span data-ttu-id="3c16d-610">üç olası genişletmeleri birine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-610">corresponds to one of three possible expansions.</span></span> <span data-ttu-id="3c16d-611">Zaman `ResourceType` bir NULL olmayan değer türü olan genişletme</span><span class="sxs-lookup"><span data-stu-id="3c16d-611">When `ResourceType` is a non-nullable value type, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        ((IDisposable)resource).Dispose();
    }
}
```

<span data-ttu-id="3c16d-612">Aksi takdirde olduğunda `ResourceType` null bir değer türü veya bir başvuru türü dışında olan `dynamic`, genişletilmiş halidir</span><span class="sxs-lookup"><span data-stu-id="3c16d-612">Otherwise, when `ResourceType` is a nullable value type or a reference type other than `dynamic`, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        if (resource != null) ((IDisposable)resource).Dispose();
    }
}
```

<span data-ttu-id="3c16d-613">Aksi takdirde olduğunda `ResourceType` olduğu `dynamic`, genişletilmiş halidir</span><span class="sxs-lookup"><span data-stu-id="3c16d-613">Otherwise, when `ResourceType` is `dynamic`, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    IDisposable d = (IDisposable)resource;
    try {
        statement;
    }
    finally {
        if (d != null) d.Dispose();
    }
}
```

<span data-ttu-id="3c16d-614">Ya da genişletme içinde `resource` değişkendir katıştırılmış deyiminde salt okunur ve `d` değişken için katıştırılmış deyimi içinde erişilemez ve görünmez.</span><span class="sxs-lookup"><span data-stu-id="3c16d-614">In either expansion, the `resource` variable is read-only in the embedded statement, and the `d` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="3c16d-615">Yukarıdaki genişletmeyle tutarlı bir davranıştır sürece bir uygulama bir verilen kullanan-deyimi, örneğin performans nedenleriyle desteklememesinden izin verilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-615">An implementation is permitted to implement a given using-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="3c16d-616">A `using` formun deyimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-616">A `using` statement of the form</span></span>
```csharp
using (expression) statement
```
<span data-ttu-id="3c16d-617">aynı üç olası uzantılarına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-617">has the same three possible expansions.</span></span> <span data-ttu-id="3c16d-618">Bu durumda `ResourceType` derleme zamanı türü örtülü olarak başvuruluyor `expression`, varsa.</span><span class="sxs-lookup"><span data-stu-id="3c16d-618">In this case `ResourceType` is implicitly the compile-time type of the `expression`, if it has one.</span></span> <span data-ttu-id="3c16d-619">Aksi takdirde arabirimi `IDisposable` olarak tek başına kullanılan `ResourceType`.</span><span class="sxs-lookup"><span data-stu-id="3c16d-619">Otherwise the interface `IDisposable` itself is used as the `ResourceType`.</span></span> <span data-ttu-id="3c16d-620">`resource` Değişken için katıştırılmış deyimi içinde erişilemez ve görünmez.</span><span class="sxs-lookup"><span data-stu-id="3c16d-620">The `resource` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="3c16d-621">Olduğunda bir *resource_acquisition* biçimini alır bir *local_variable_declaration*, belirli bir türün birden çok kaynak elde mümkündür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-621">When a *resource_acquisition* takes the form of a *local_variable_declaration*, it is possible to acquire multiple resources of a given type.</span></span> <span data-ttu-id="3c16d-622">A `using` formun deyimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-622">A `using` statement of the form</span></span>
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
<span data-ttu-id="3c16d-623">tam olarak bir dizi denk iç içe `using` ifadeleri:</span><span class="sxs-lookup"><span data-stu-id="3c16d-623">is precisely equivalent to a sequence of nested `using` statements:</span></span>
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

<span data-ttu-id="3c16d-624">Aşağıdaki örnekte adlı bir dosya oluşturur `log.txt` ve iki satırlık bir metin dosyasına yazar.</span><span class="sxs-lookup"><span data-stu-id="3c16d-624">The example below creates a file named `log.txt` and writes two lines of text to the file.</span></span> <span data-ttu-id="3c16d-625">Örnek, aynı dosya okuma için açar ve içerdiği satırlık metin konsola kopyalar.</span><span class="sxs-lookup"><span data-stu-id="3c16d-625">The example then opens that same file for reading and copies the contained lines of text to the console.</span></span>
```csharp
using System;
using System.IO;

class Test
{
    static void Main() {
        using (TextWriter w = File.CreateText("log.txt")) {
            w.WriteLine("This is line one");
            w.WriteLine("This is line two");
        }

        using (TextReader r = File.OpenText("log.txt")) {
            string s;
            while ((s = r.ReadLine()) != null) {
                Console.WriteLine(s);
            }

        }
    }
}
```

<span data-ttu-id="3c16d-626">Bu yana `TextWriter` ve `TextReader` sınıfları uygulama `IDisposable` arabirimi, örnek kullanabilir `using` temel alınan dosya düzgün şekilde yazma aşağıdaki kapatıldığından emin olun veya okuma işlemleri için deyimleri.</span><span class="sxs-lookup"><span data-stu-id="3c16d-626">Since the `TextWriter` and `TextReader` classes implement the `IDisposable` interface, the example can use `using` statements to ensure that the underlying file is properly closed following the write or read operations.</span></span>

## <a name="the-yield-statement"></a><span data-ttu-id="3c16d-627">Yield deyimi</span><span class="sxs-lookup"><span data-stu-id="3c16d-627">The yield statement</span></span>

<span data-ttu-id="3c16d-628">`yield` Deyimi, bir yineleyici bloğunda kullanılır ([blokları](statements.md#blocks)) Numaralandırıcı nesnesi için bir değer elde etmek üzere ([Numaralandırıcı nesneleri](classes.md#enumerator-objects)) veya numaralandırma nesnesi ([numaralandırılabilir nesneleri](classes.md#enumerable-objects)) bir yineleyicinin veya yinelemenin sonunda, sinyal.</span><span class="sxs-lookup"><span data-stu-id="3c16d-628">The `yield` statement is used in an iterator block ([Blocks](statements.md#blocks)) to yield a value to the enumerator object ([Enumerator objects](classes.md#enumerator-objects)) or enumerable object ([Enumerable objects](classes.md#enumerable-objects)) of an iterator or to signal the end of the iteration.</span></span>

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

<span data-ttu-id="3c16d-629">`yield` ayrılmış bir sözcük değildir; yalnızca hemen önce kullanıldığında özel anlamı vardır bir `return` veya `break` anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="3c16d-629">`yield` is not a reserved word; it has special meaning only when used immediately before a `return` or `break` keyword.</span></span> <span data-ttu-id="3c16d-630">Diğer bağlamlarda `yield` tanımlayıcı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-630">In other contexts, `yield` can be used as an identifier.</span></span>

<span data-ttu-id="3c16d-631">Nereye birkaç kısıtlama vardır bir `yield` deyimi görüntülenebilir, aşağıda açıklandığı gibi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-631">There are several restrictions on where a `yield` statement can appear, as described in the following.</span></span>

*  <span data-ttu-id="3c16d-632">Bir derleme zamanı hata için bir `yield` deyimi (ya da formun) dışında görüntülenecek bir *method_body*, *operator_body* veya *accessor_body*</span><span class="sxs-lookup"><span data-stu-id="3c16d-632">It is a compile-time error for a `yield` statement (of either form) to appear outside a *method_body*, *operator_body* or *accessor_body*</span></span>
*  <span data-ttu-id="3c16d-633">Bir derleme zamanı hata için bir `yield` (ya da formun) deyimi anonim bir işlev içinde görüntülenecek.</span><span class="sxs-lookup"><span data-stu-id="3c16d-633">It is a compile-time error for a `yield` statement (of either form) to appear inside an anonymous function.</span></span>
*  <span data-ttu-id="3c16d-634">Bir derleme zamanı hata için bir `yield` deyimi (ya da formun) görünmesini `finally` yan tümcesi bir `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-634">It is a compile-time error for a `yield` statement (of either form) to appear in the `finally` clause of a `try` statement.</span></span>
*  <span data-ttu-id="3c16d-635">Bir derleme zamanı hata için bir `yield return` deyimini her yerde görünür bir `try` herhangi içeren ifade `catch` yan tümceleri.</span><span class="sxs-lookup"><span data-stu-id="3c16d-635">It is a compile-time error for a `yield return` statement to appear anywhere in a `try` statement that contains any `catch` clauses.</span></span>

<span data-ttu-id="3c16d-636">Aşağıdaki örnek, geçerli ve geçersiz bazı kullanımlarını gösterir `yield` deyimleri.</span><span class="sxs-lookup"><span data-stu-id="3c16d-636">The following example shows some valid and invalid uses of `yield` statements.</span></span>

```csharp
delegate IEnumerable<int> D();

IEnumerator<int> GetEnumerator() {
    try {
        yield return 1;        // Ok
        yield break;           // Ok
    }
    finally {
        yield return 2;        // Error, yield in finally
        yield break;           // Error, yield in finally
    }

    try {
        yield return 3;        // Error, yield return in try...catch
        yield break;           // Ok
    }
    catch {
        yield return 4;        // Error, yield return in try...catch
        yield break;           // Ok
    }

    D d = delegate { 
        yield return 5;        // Error, yield in an anonymous function
    }; 
}

int MyMethod() {
    yield return 1;            // Error, wrong return type for an iterator block
}
```

<span data-ttu-id="3c16d-637">Örtük bir dönüştürme ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) ifadede türünden bulunmalıdır `yield return` deyimi yield türüne ([Yield türü](classes.md#yield-type)) yineleyici.</span><span class="sxs-lookup"><span data-stu-id="3c16d-637">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression in the `yield return` statement to the yield type ([Yield type](classes.md#yield-type)) of the iterator.</span></span>

<span data-ttu-id="3c16d-638">A `yield return` deyim şu şekilde gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-638">A `yield return` statement is executed as follows:</span></span>

*  <span data-ttu-id="3c16d-639">Deyiminde belirtilen ifade değerlendirilir, örtük olarak yield türüne dönüştürülerek ve atanan `Current` Numaralandırıcı nesnesi bir özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="3c16d-639">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
*  <span data-ttu-id="3c16d-640">Yineleyici bloğu yürütülmesini askıya alınır.</span><span class="sxs-lookup"><span data-stu-id="3c16d-640">Execution of the iterator block is suspended.</span></span> <span data-ttu-id="3c16d-641">Varsa `yield return` deyimi, bir veya daha fazla içinde `try` engeller, ilişkili `finally` blokları şu anda yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="3c16d-641">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
*  <span data-ttu-id="3c16d-642">`MoveNext` Yöntemi, numaralandırıcı nesnesi döndürür `true` Numaralandırıcı nesnesi bir sonraki öğeye başarıyla Gelişmiş belirten arayanına için.</span><span class="sxs-lookup"><span data-stu-id="3c16d-642">The `MoveNext` method of the enumerator object returns `true` to its caller, indicating that the enumerator object successfully advanced to the next item.</span></span>

<span data-ttu-id="3c16d-643">Numaralandırıcı nesne yapılan sonraki çağrıda `MoveNext` yöntemi burada son askıya alındığı gelen yineleyici bloğu yürütülmesini sürdürür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-643">The next call to the enumerator object's `MoveNext` method resumes execution of the iterator block from where it was last suspended.</span></span>

<span data-ttu-id="3c16d-644">A `yield break` deyim şu şekilde gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="3c16d-644">A `yield break` statement is executed as follows:</span></span>

*  <span data-ttu-id="3c16d-645">Varsa `yield break` deyimi bir veya daha fazla içine `try` bloklarla ilişkili `finally` denetim blokları için başlangıçta aktarılır `finally` en içteki bloğunu `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-645">If the `yield break` statement is enclosed by one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="3c16d-646">Zaman ve denetim bitiş noktasını ulaşırsa bir `finally` denetim bloğu aktarılır `finally` sonraki kapsayan, blok `try` deyimi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-646">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="3c16d-647">Kadar bu işlem yinelenir `finally` tüm kapsayan, bloklarını `try` deyimler yürütülür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-647">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="3c16d-648">Yineleyici bloğu çağırana döndürülür.</span><span class="sxs-lookup"><span data-stu-id="3c16d-648">Control is returned to the caller of the iterator block.</span></span> <span data-ttu-id="3c16d-649">Bu `MoveNext` yöntemi veya `Dispose` Numaralandırıcı nesnesi yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3c16d-649">This is either the `MoveNext` method or `Dispose` method of the enumerator object.</span></span>

<span data-ttu-id="3c16d-650">Çünkü bir `yield break` deyimi koşulsuz aktarımları denetimi başka bir yerde bitiş noktasını bir `yield break` deyimi erişilebilir hiçbir zaman.</span><span class="sxs-lookup"><span data-stu-id="3c16d-650">Because a `yield break` statement unconditionally transfers control elsewhere, the end point of a `yield break` statement is never reachable.</span></span>
