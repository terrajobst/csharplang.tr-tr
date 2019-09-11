---
ms.openlocfilehash: 94346034a667ad4af26796c0c4bbc96d6ed79aba
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876843"
---
# <a name="statements"></a><span data-ttu-id="1ee89-101">Deyimler</span><span class="sxs-lookup"><span data-stu-id="1ee89-101">Statements</span></span>

<span data-ttu-id="1ee89-102">C#çeşitli deyimler sağlar.</span><span class="sxs-lookup"><span data-stu-id="1ee89-102">C# provides a variety of statements.</span></span> <span data-ttu-id="1ee89-103">Bu deyimlerin çoğu, C ve C++' de programlanan geliştiricilere tanıdık gelecektir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-103">Most of these statements will be familiar to developers who have programmed in C and C++.</span></span>

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

<span data-ttu-id="1ee89-104">*Embedded_statement* & gt, diğer deyimler içinde görünen deyimler için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-104">The *embedded_statement* nonterminal is used for statements that appear within other statements.</span></span> <span data-ttu-id="1ee89-105">Yerine *embedded_statement* kullanımı *deyimi* , bu bağlamlarda bildirim deyimlerinin ve etiketli deyimlerin kullanımını dışlar.</span><span class="sxs-lookup"><span data-stu-id="1ee89-105">The use of *embedded_statement* rather than *statement* excludes the use of declaration statements and labeled statements in these contexts.</span></span> <span data-ttu-id="1ee89-106">Örnek</span><span class="sxs-lookup"><span data-stu-id="1ee89-106">The example</span></span>
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
<span data-ttu-id="1ee89-107">bir `if` deyimin If dalı için bir *bildirim* yerine bir *embedded_statement* gerektirdiğinden derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-107">results in a compile-time error because an `if` statement requires an *embedded_statement* rather than a *statement* for its if branch.</span></span> <span data-ttu-id="1ee89-108">Bu koda izin verildiğinde, değişken `i` bildirilebilecek, ancak hiçbir şekilde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="1ee89-108">If this code were permitted, then the variable `i` would be declared, but it could never be used.</span></span> <span data-ttu-id="1ee89-109">Ancak, bir blokta bildirimi yerleştirilerek `i`örneğin geçerli olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="1ee89-109">Note, however, that by placing `i`'s declaration in a block, the example is valid.</span></span>

## <a name="end-points-and-reachability"></a><span data-ttu-id="1ee89-110">Bitiş noktaları ve ulaşılabilirlik</span><span class="sxs-lookup"><span data-stu-id="1ee89-110">End points and reachability</span></span>

<span data-ttu-id="1ee89-111">Her deyimin bir ***uç noktası***vardır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-111">Every statement has an ***end point***.</span></span> <span data-ttu-id="1ee89-112">Sezgisel koşullarda, bir deyimin bitiş noktası, deyimden hemen sonraki konumdur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-112">In intuitive terms, the end point of a statement is the location that immediately follows the statement.</span></span> <span data-ttu-id="1ee89-113">Bileşik deyimler için yürütme kuralları (katıştırılmış deyimler içeren deyimler), denetimin gömülü bir deyimin bitiş noktasına ulaştığında gerçekleştirilecek eylemi belirtir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-113">The execution rules for composite statements (statements that contain embedded statements) specify the action that is taken when control reaches the end point of an embedded statement.</span></span> <span data-ttu-id="1ee89-114">Örneğin, denetim bir blok içindeki bir deyimin bitiş noktasına ulaştığında denetim bloğunda bir sonraki ifadeye aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-114">For example, when control reaches the end point of a statement in a block, control is transferred to the next statement in the block.</span></span>

<span data-ttu-id="1ee89-115">Bir ifadeye, yürütme tarafından büyük olasılıkla ulaşılırsa, ifadeye ***ulaşılabilir***denir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-115">If a statement can possibly be reached by execution, the statement is said to be ***reachable***.</span></span> <span data-ttu-id="1ee89-116">Buna karşılık, bir deyimin yürütülemeyecek bir olasılık yoksa, ifadeye ***ulaşılamıyor***denir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-116">Conversely, if there is no possibility that a statement will be executed, the statement is said to be ***unreachable***.</span></span>

<span data-ttu-id="1ee89-117">Örnekte</span><span class="sxs-lookup"><span data-stu-id="1ee89-117">In the example</span></span>
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
<span data-ttu-id="1ee89-118">ifadesinin yürütülebilmesi olası bir `Console.WriteLine` olasılık olmadığından, öğesinin ikinci çağrısı ulaşılamaz.</span><span class="sxs-lookup"><span data-stu-id="1ee89-118">the second invocation of `Console.WriteLine` is unreachable because there is no possibility that the statement will be executed.</span></span>

<span data-ttu-id="1ee89-119">Derleyici bir deyimin ulaşılamaz olduğunu belirlerse bir uyarı bildirilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-119">A warning is reported if the compiler determines that a statement is unreachable.</span></span> <span data-ttu-id="1ee89-120">Bir deyimin ulaşılamaz olması için özellikle bir hata değildir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-120">It is specifically not an error for a statement to be unreachable.</span></span>

<span data-ttu-id="1ee89-121">Belirli bir deyimin veya bitiş noktasının erişilebilir olup olmadığını anlamak için, derleyici her bir bildirimde tanımlanan erişilebilir kurallara göre Akış Analizi gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-121">To determine whether a particular statement or end point is reachable, the compiler performs flow analysis according to the reachability rules defined for each statement.</span></span> <span data-ttu-id="1ee89-122">Akış Analizi, deyimlerin davranışını denetleyen sabit ifadelerin ([sabit ifadeler](expressions.md#constant-expressions)) değerlerini hesaba alır, ancak sabit olmayan ifadelerin olası değerleri dikkate alınmaz.</span><span class="sxs-lookup"><span data-stu-id="1ee89-122">The flow analysis takes into account the values of constant expressions ([Constant expressions](expressions.md#constant-expressions)) that control the behavior of statements, but the possible values of non-constant expressions are not considered.</span></span> <span data-ttu-id="1ee89-123">Diğer bir deyişle, denetim akışı analizinin amaçları doğrultusunda, belirli bir türün sabit olmayan bir ifadesi söz konusu türde herhangi bir olası değere sahip olduğu kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-123">In other words, for purposes of control flow analysis, a non-constant expression of a given type is considered to have any possible value of that type.</span></span>

<span data-ttu-id="1ee89-124">Örnekte</span><span class="sxs-lookup"><span data-stu-id="1ee89-124">In the example</span></span>
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
<span data-ttu-id="1ee89-125">`==` işlecin Boolean ifadesi `if` sabit bir ifadedir çünkü her iki işleç işlenenleri de sabitler.</span><span class="sxs-lookup"><span data-stu-id="1ee89-125">the boolean expression of the `if` statement is a constant expression because both operands of the `==` operator are constants.</span></span> <span data-ttu-id="1ee89-126">Sabit ifade derleme zamanında değerlendirildiğinden, değeri `false` `Console.WriteLine` üreten çağrının ulaşılamaz olduğu kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-126">As the constant expression is evaluated at compile-time, producing the value `false`, the `Console.WriteLine` invocation is considered unreachable.</span></span> <span data-ttu-id="1ee89-127">Ancak, `i` yerel bir değişken olarak değiştirilirse</span><span class="sxs-lookup"><span data-stu-id="1ee89-127">However, if `i` is changed to be a local variable</span></span>
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
<span data-ttu-id="1ee89-128">`Console.WriteLine` çağrının erişilebilir olduğu kabul edilir, ancak gerçekte hiçbir şekilde yürütülmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-128">the `Console.WriteLine` invocation is considered reachable, even though, in reality, it will never be executed.</span></span>

<span data-ttu-id="1ee89-129">Bir işlev üyesinin *bloğu* her zaman ulaşılabilir olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-129">The *block* of a function member is always considered reachable.</span></span> <span data-ttu-id="1ee89-130">Bir blok içindeki her bir deyimin erişilebilirlik kurallarını kapsamlı bir şekilde değerlendirerek, belirli bir deyimin erişilebilirliği belirlenebilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-130">By successively evaluating the reachability rules of each statement in a block, the reachability of any given statement can be determined.</span></span>

<span data-ttu-id="1ee89-131">Örnekte</span><span class="sxs-lookup"><span data-stu-id="1ee89-131">In the example</span></span>
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
<span data-ttu-id="1ee89-132">İkincinin `Console.WriteLine` ulaşılabilirlik aşağıdaki gibi belirlenir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-132">the reachability of the second `Console.WriteLine` is determined as follows:</span></span>

*  <span data-ttu-id="1ee89-133">Metodun`F` bloğu `Console.WriteLine` erişilebilir olduğundan ilk ifade deyimine erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-133">The first `Console.WriteLine` expression statement is reachable because the block of the `F` method is reachable.</span></span>
*  <span data-ttu-id="1ee89-134">İlk `Console.WriteLine` ifade deyiminin bitiş noktasına ulaşılabilir çünkü bu deyim erişilebilir durumda.</span><span class="sxs-lookup"><span data-stu-id="1ee89-134">The end point of the first `Console.WriteLine` expression statement is reachable because that statement is reachable.</span></span>
*  <span data-ttu-id="1ee89-135">`if` İlk`Console.WriteLine` ifade deyiminin bitiş noktası erişilebilir olduğundan deyim erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-135">The `if` statement is reachable because the end point of the first `Console.WriteLine` expression statement is reachable.</span></span>
*  <span data-ttu-id="1ee89-136">Deyimin Boole `Console.WriteLine` ifadesi `false`sabit değere sahip olmadığından ikinci ifade deyimine erişilebilir. `if`</span><span class="sxs-lookup"><span data-stu-id="1ee89-136">The second `Console.WriteLine` expression statement is reachable because the boolean expression of the `if` statement does not have the constant value `false`.</span></span>

<span data-ttu-id="1ee89-137">Bir deyimin bitiş noktasında erişilebilir olması için derleme zamanı hatası olduğu iki durum vardır:</span><span class="sxs-lookup"><span data-stu-id="1ee89-137">There are two situations in which it is a compile-time error for the end point of a statement to be reachable:</span></span>

*  <span data-ttu-id="1ee89-138">`switch` İfade, bir switch bölümünün bir sonraki Switch bölümüne "Fall" olarak geçmesine izin vermediğinden, bir switch bölümünün bildirim listesinin bitiş noktası için bir derleme zamanı hatası, erişilebilir olacaktır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-138">Because the `switch` statement does not permit a switch section to "fall through" to the next switch section, it is a compile-time error for the end point of the statement list of a switch section to be reachable.</span></span> <span data-ttu-id="1ee89-139">Bu hata oluşursa, genellikle bir `break` deyimin eksik olduğunun bir göstergesidir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-139">If this error occurs, it is typically an indication that a `break` statement is missing.</span></span>
*  <span data-ttu-id="1ee89-140">Bu, erişilebilen bir değeri hesaplayan bir işlev üyesinin bloğunun bitiş noktası için derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-140">It is a compile-time error for the end point of the block of a function member that computes a value to be reachable.</span></span> <span data-ttu-id="1ee89-141">Bu hata oluşursa, genellikle bir `return` deyimin eksik olduğunun göstergesidir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-141">If this error occurs, it typically is an indication that a `return` statement is missing.</span></span>

## <a name="blocks"></a><span data-ttu-id="1ee89-142">Bloklar</span><span class="sxs-lookup"><span data-stu-id="1ee89-142">Blocks</span></span>

<span data-ttu-id="1ee89-143">Bir *blok* , tek bir ifadeye izin verilen bağlamlarda birden çok deyimin yazılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-143">A *block* permits multiple statements to be written in contexts where a single statement is allowed.</span></span>

```antlr
block
    : '{' statement_list? '}'
    ;
```

<span data-ttu-id="1ee89-144">Bir *blok* , küme ayraçları içine alınmış bir isteğe bağlı *statement_list* ([ifade listeleri](statements.md#statement-lists)) içerir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-144">A *block* consists of an optional *statement_list* ([Statement lists](statements.md#statement-lists)), enclosed in braces.</span></span> <span data-ttu-id="1ee89-145">Ekstre listesi atlanırsa, blok boş olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-145">If the statement list is omitted, the block is said to be empty.</span></span>

<span data-ttu-id="1ee89-146">Bir blok, bildirim deyimleri içerebilir ([bildirim deyimleri](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="1ee89-146">A block may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="1ee89-147">Bir blok içinde belirtilen yerel bir değişken veya sabit kapsam, bloğudur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-147">The scope of a local variable or constant declared in a block is the block.</span></span>

<span data-ttu-id="1ee89-148">Bir blok aşağıdaki gibi yürütülür:</span><span class="sxs-lookup"><span data-stu-id="1ee89-148">A block is executed as follows:</span></span>

*  <span data-ttu-id="1ee89-149">Blok boşsa denetim, bloğun bitiş noktasına aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-149">If the block is empty, control is transferred to the end point of the block.</span></span>
*  <span data-ttu-id="1ee89-150">Blok boş değilse denetim, ifade listesine aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-150">If the block is not empty, control is transferred to the statement list.</span></span> <span data-ttu-id="1ee89-151">Ve denetimi, ekstre listesinin bitiş noktasına ulaştığında denetim, bloğun bitiş noktasına aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-151">When and if control reaches the end point of the statement list, control is transferred to the end point of the block.</span></span>

<span data-ttu-id="1ee89-152">Bloğun kendisi erişilebilir olduğunda bir bloğun ifade listesi erişilebilir olur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-152">The statement list of a block is reachable if the block itself is reachable.</span></span>

<span data-ttu-id="1ee89-153">Blok boşsa veya ekstre listesinin bitiş noktası ulaşılabilir ise bir bloğun bitiş noktası erişilebilir olur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-153">The end point of a block is reachable if the block is empty or if the end point of the statement list is reachable.</span></span>

<span data-ttu-id="1ee89-154">Bir veya daha fazla `yield` deyim ([Yield deyimi](statements.md#the-yield-statement)) içeren bir blok, yineleyici bloğu olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-154">A *block* that contains one or more `yield` statements ([The yield statement](statements.md#the-yield-statement)) is called an iterator block.</span></span> <span data-ttu-id="1ee89-155">Yineleyici blokları, işlev üyelerini yineleyiciler ([yineleyiciler](classes.md#iterators)) olarak uygulamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-155">Iterator blocks are used to implement function members as iterators ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="1ee89-156">Yineleyici blokları için bazı ek kısıtlamalar geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-156">Some additional restrictions apply to iterator blocks:</span></span>

*  <span data-ttu-id="1ee89-157">Bir `return` deyimin Yineleyici bloğunda görünmesi için derleme zamanı hatası (ancak `yield return` deyimlerine izin verilir).</span><span class="sxs-lookup"><span data-stu-id="1ee89-157">It is a compile-time error for a `return` statement to appear in an iterator block (but `yield return` statements are permitted).</span></span>
*  <span data-ttu-id="1ee89-158">Bir yineleyici bloğunun güvenli olmayan bir bağlam ([güvenli olmayan bağlamlar](unsafe-code.md#unsafe-contexts)) içermesi için derleme zamanı hatası.</span><span class="sxs-lookup"><span data-stu-id="1ee89-158">It is a compile-time error for an iterator block to contain an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="1ee89-159">Bir yineleyici bloğu, bildirimi güvenli olmayan bir bağlamda iç içe yerleştirilmiş olsa bile her zaman güvenli bir bağlam tanımlar.</span><span class="sxs-lookup"><span data-stu-id="1ee89-159">An iterator block always defines a safe context, even when its declaration is nested in an unsafe context.</span></span>

### <a name="statement-lists"></a><span data-ttu-id="1ee89-160">Ekstre listeleri</span><span class="sxs-lookup"><span data-stu-id="1ee89-160">Statement lists</span></span>

<span data-ttu-id="1ee89-161">Deyim listesi, sırayla yazılan bir veya daha fazla ***deyimden*** oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-161">A ***statement list*** consists of one or more statements written in sequence.</span></span> <span data-ttu-id="1ee89-162">İfade listeleri, *blok*s ([bloklar](statements.md#blocks)) ve *switch_block*s ([Switch ifadesinde](statements.md#the-switch-statement)) içinde oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-162">Statement lists occur in *block*s ([Blocks](statements.md#blocks)) and in *switch_block*s ([The switch statement](statements.md#the-switch-statement)).</span></span>

```antlr
statement_list
    : statement+
    ;
```

<span data-ttu-id="1ee89-163">Bir ifade listesi, denetim ilk ifadeye aktarılarak yürütülür.</span><span class="sxs-lookup"><span data-stu-id="1ee89-163">A statement list is executed by transferring control to the first statement.</span></span> <span data-ttu-id="1ee89-164">Ve denetimi bir deyimin bitiş noktasına ulaştığında, Denetim sonraki deyime aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-164">When and if control reaches the end point of a statement, control is transferred to the next statement.</span></span> <span data-ttu-id="1ee89-165">Denetim, son deyimin bitiş noktasına ulaştığında denetim, ekstre listesinin bitiş noktasına aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-165">When and if control reaches the end point of the last statement, control is transferred to the end point of the statement list.</span></span>

<span data-ttu-id="1ee89-166">Aşağıdakilerden en az biri doğru ise, bir ifade listesindeki deyime erişilebilir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-166">A statement in a statement list is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="1ee89-167">İfade ilk ifadedir ve ekstre listesinin kendisi erişilebilir olur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-167">The statement is the first statement and the statement list itself is reachable.</span></span>
*  <span data-ttu-id="1ee89-168">Önceki deyimin bitiş noktasına ulaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-168">The end point of the preceding statement is reachable.</span></span>
*  <span data-ttu-id="1ee89-169">İfade etiketli bir ifadedir ve etikete erişilebilir `goto` bir ifade tarafından başvuruluyor.</span><span class="sxs-lookup"><span data-stu-id="1ee89-169">The statement is a labeled statement and the label is referenced by a reachable `goto` statement.</span></span>

<span data-ttu-id="1ee89-170">Listedeki son deyimin bitiş noktası ulaşılabilir ise, bir ekstre listesinin bitiş noktası erişilebilir olur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-170">The end point of a statement list is reachable if the end point of the last statement in the list is reachable.</span></span>

## <a name="the-empty-statement"></a><span data-ttu-id="1ee89-171">Boş ifade</span><span class="sxs-lookup"><span data-stu-id="1ee89-171">The empty statement</span></span>

<span data-ttu-id="1ee89-172">Bir *empty_statement* hiçbir şey yapmaz.</span><span class="sxs-lookup"><span data-stu-id="1ee89-172">An *empty_statement* does nothing.</span></span>

```antlr
empty_statement
    : ';'
    ;
```

<span data-ttu-id="1ee89-173">Boş bir ifade, bir deyimin gerekli olduğu bir bağlamda gerçekleştirilecek bir işlem olmadığında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-173">An empty statement is used when there are no operations to perform in a context where a statement is required.</span></span>

<span data-ttu-id="1ee89-174">Boş bir deyimin yürütülmesi, denetimi yalnızca deyimin bitiş noktasına aktarır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-174">Execution of an empty statement simply transfers control to the end point of the statement.</span></span> <span data-ttu-id="1ee89-175">Bu nedenle, boş deyimin erişilebilir olması halinde boş bir deyimin bitiş noktasına ulaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-175">Thus, the end point of an empty statement is reachable if the empty statement is reachable.</span></span>

<span data-ttu-id="1ee89-176">Boş bir ifade, null gövdesi olan bir `while` ifade yazılırken kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-176">An empty statement can be used when writing a `while` statement with a null body:</span></span>
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

<span data-ttu-id="1ee89-177">Ayrıca, bir bloğun "`}`" kapanışından hemen önce bir etiketi bildirmek için boş bir ifade kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-177">Also, an empty statement can be used to declare a label just before the closing "`}`" of a block:</span></span>
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a><span data-ttu-id="1ee89-178">Etiketli deyimler</span><span class="sxs-lookup"><span data-stu-id="1ee89-178">Labeled statements</span></span>

<span data-ttu-id="1ee89-179">Bir *labeled_statement* , bir deyimin ön eki olarak belirtilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-179">A *labeled_statement* permits a statement to be prefixed by a label.</span></span> <span data-ttu-id="1ee89-180">Etiketli deyimlerde bloklara izin verilir, ancak gömülü deyimler olarak izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="1ee89-180">Labeled statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

<span data-ttu-id="1ee89-181">Etiketli bir ifade, *tanımlayıcı*tarafından verilen ada sahip bir etiket bildirir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-181">A labeled statement declares a label with the name given by the *identifier*.</span></span> <span data-ttu-id="1ee89-182">Bir etiketin kapsamı, iç içe geçmiş bloklar dahil olmak üzere, etiketin bildirildiği tüm bloğudur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-182">The scope of a label is the whole block in which the label is declared, including any nested blocks.</span></span> <span data-ttu-id="1ee89-183">Çakışan kapsamlara sahip olmak için aynı ada sahip iki etiket için derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-183">It is a compile-time error for two labels with the same name to have overlapping scopes.</span></span>

<span data-ttu-id="1ee89-184">Etiketin kapsamı içinde deyimlerden `goto` ([goto deyimi](statements.md#the-goto-statement)) bir etikete başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-184">A label can be referenced from `goto` statements ([The goto statement](statements.md#the-goto-statement)) within the scope of the label.</span></span> <span data-ttu-id="1ee89-185">Bu, `goto` deyimlerin blokları bloklar içinde ve blok dışında, ancak hiçbir şekilde bloklara aktarabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-185">This means that `goto` statements can transfer control within blocks and out of blocks, but never into blocks.</span></span>

<span data-ttu-id="1ee89-186">Etiketler kendi bildirim alanına sahiptir ve diğer tanımlayıcılarla karışmaz.</span><span class="sxs-lookup"><span data-stu-id="1ee89-186">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="1ee89-187">Örnek</span><span class="sxs-lookup"><span data-stu-id="1ee89-187">The example</span></span>
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
<span data-ttu-id="1ee89-188">geçerli olur ve adı `x` hem parametre hem de etiket olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-188">is valid and uses the name `x` as both a parameter and a label.</span></span>

<span data-ttu-id="1ee89-189">Etiketli bir deyimin yürütülmesi, etiketi izleyen deyimin yürütülmesi için tam olarak karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-189">Execution of a labeled statement corresponds exactly to execution of the statement following the label.</span></span>

<span data-ttu-id="1ee89-190">Normal denetim akışı tarafından sağlanmış olan erişilebilirlik 'e ek olarak, etikete erişilebilir `goto` bir bildirimde başvuruluyorsa etiketli ifadeye erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-190">In addition to the reachability provided by normal flow of control, a labeled statement is reachable if the label is referenced by a reachable `goto` statement.</span></span> <span data-ttu-id="1ee89-191">Duruma `finally` `try` `finally` Bir ifade bir blok içeren bir `try` içinde ise ve etiketli ifade öğesinin dışındaysa ve bloğun bitiş noktası ulaşılamaz durumdaysa, etiketli ifadeye öğesinden ulaşılamıyor `goto` Bu `goto` ifade.)</span><span class="sxs-lookup"><span data-stu-id="1ee89-191">(Exception: If a `goto` statement is inside a `try` that includes a `finally` block, and the labeled statement is outside the `try`, and the end point of the `finally` block is unreachable, then the labeled statement is not reachable from that `goto` statement.)</span></span>

## <a name="declaration-statements"></a><span data-ttu-id="1ee89-192">Bildirim deyimleri</span><span class="sxs-lookup"><span data-stu-id="1ee89-192">Declaration statements</span></span>

<span data-ttu-id="1ee89-193">Bir *declaration_statement* yerel bir değişken veya sabit bildirir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-193">A *declaration_statement* declares a local variable or constant.</span></span> <span data-ttu-id="1ee89-194">Bildirim deyimlerinin bloklara izin verilir, ancak gömülü deyimler olarak izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="1ee89-194">Declaration statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a><span data-ttu-id="1ee89-195">Yerel değişken bildirimleri</span><span class="sxs-lookup"><span data-stu-id="1ee89-195">Local variable declarations</span></span>

<span data-ttu-id="1ee89-196">Bir *local_variable_declaration* , bir veya daha fazla yerel değişken bildirir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-196">A *local_variable_declaration* declares one or more local variables.</span></span>

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

<span data-ttu-id="1ee89-197">Bir *local_variable_declaration* 'ın `var` *local_variable_type* , bildirim tarafından tanıtılan değişkenlerin türünü doğrudan belirtir veya bir tür ile bir izer.</span><span class="sxs-lookup"><span data-stu-id="1ee89-197">The *local_variable_type* of a *local_variable_declaration* either directly specifies the type of the variables introduced by the declaration, or indicates with the identifier `var` that the type should be inferred based on an initializer.</span></span> <span data-ttu-id="1ee89-198">Türün ardından, her biri yeni bir değişken tanıtan bir *local_variable_declarator*s listesi gelir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-198">The type is followed by a list of *local_variable_declarator*s, each of which introduces a new variable.</span></span> <span data-ttu-id="1ee89-199">Bir *local_variable_declarator* değişkeni, isteğe bağlı olarak bir "`=`" belirteci ve değişkenin başlangıç değerini veren bir *local_variable_initializer* tarafından kullanılan bir *tanımlayıcıdan* oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-199">A *local_variable_declarator* consists of an *identifier* that names the variable, optionally followed by an "`=`" token and a *local_variable_initializer* that gives the initial value of the variable.</span></span>

<span data-ttu-id="1ee89-200">Yerel bir değişken bildirimi bağlamında, tanımlayıcı var bir bağlamsal anahtar sözcük ([anahtar sözcük](lexical-structure.md#keywords)) olarak davranır. *Local_variable_type* `var` olarak belirtildiğinde ve adlandırılmış `var` hiçbir tür kapsamda olduğunda, bildirim ***örtük olarak yazılmış bir yerel değişken bildirimidir***ve türü ilişkili Başlatıcı türünden çıkarsandır ifadesini.</span><span class="sxs-lookup"><span data-stu-id="1ee89-200">In the context of a local variable declaration, the identifier var acts as a contextual keyword ([Keywords](lexical-structure.md#keywords)).When the *local_variable_type* is specified as `var` and no type named `var` is in scope, the declaration is an ***implicitly typed local variable declaration***, whose type is inferred from the type of the associated initializer expression.</span></span> <span data-ttu-id="1ee89-201">Örtük olarak yazılan yerel değişken bildirimleri aşağıdaki kısıtlamalara tabidir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-201">Implicitly typed local variable declarations are subject to the following restrictions:</span></span>

*  <span data-ttu-id="1ee89-202">*Local_variable_declaration* birden çok *local_variable_declarator*s içeremez.</span><span class="sxs-lookup"><span data-stu-id="1ee89-202">The *local_variable_declaration* cannot include multiple *local_variable_declarator*s.</span></span>
*  <span data-ttu-id="1ee89-203">*Local_variable_declarator* bir *local_variable_initializer*içermelidir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-203">The *local_variable_declarator* must include a *local_variable_initializer*.</span></span>
*  <span data-ttu-id="1ee89-204">*Local_variable_initializer* bir *ifade*olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-204">The *local_variable_initializer* must be an *expression*.</span></span>
*  <span data-ttu-id="1ee89-205">Başlatıcı *ifadesi* bir derleme zamanı türüne sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-205">The initializer *expression* must have a compile-time type.</span></span>
*  <span data-ttu-id="1ee89-206">Başlatıcı *ifadesi* , belirtilen değişkenin kendine başvuramaz</span><span class="sxs-lookup"><span data-stu-id="1ee89-206">The initializer *expression* cannot refer to the declared variable itself</span></span>

<span data-ttu-id="1ee89-207">Aşağıda, türü kesin olarak belirlenmiş geçersiz yerel değişken bildirimlerinin örnekleri verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-207">The following are examples of incorrect implicitly typed local variable declarations:</span></span>

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

<span data-ttu-id="1ee89-208">Yerel bir değişkenin değeri, bir *simple_name* ([basit adlar](expressions.md#simple-names)) kullanılarak bir ifadede alınır ve yerel bir değişkenin değeri *atama* ([atama işleçleri](expressions.md#assignment-operators)) kullanılarak değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-208">The value of a local variable is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)), and the value of a local variable is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="1ee89-209">Yerel değişken, değerinin alındığı her konumda kesinlikle atanmalıdır ([kesin atama](variables.md#definite-assignment)).</span><span class="sxs-lookup"><span data-stu-id="1ee89-209">A local variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at each location where its value is obtained.</span></span>

<span data-ttu-id="1ee89-210">Bir *local_variable_declaration* içinde belirtilen yerel değişkenin kapsamı, bildirimin gerçekleştiği bloğudur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-210">The scope of a local variable declared in a *local_variable_declaration* is the block in which the declaration occurs.</span></span> <span data-ttu-id="1ee89-211">Yerel değişkenin *local_variable_declarator* önündeki bir metinsel konumdaki yerel değişkene başvurabileceğiniz bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-211">It is an error to refer to a local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="1ee89-212">Yerel bir değişken kapsamında, aynı ada sahip başka bir yerel değişken veya sabit bildirmek için derleme zamanı hatası vardır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-212">Within the scope of a local variable, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="1ee89-213">Birden çok değişken bildiren bir yerel değişken bildirimi, aynı türe sahip tek değişkenlerin birden çok bildirimi ile eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-213">A local variable declaration that declares multiple variables is equivalent to multiple declarations of single variables with the same type.</span></span> <span data-ttu-id="1ee89-214">Ayrıca, yerel bir değişken bildirimindeki bir değişken başlatıcısı, bildirimden hemen sonra eklenen bir atama bildirimine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-214">Furthermore, a variable initializer in a local variable declaration corresponds exactly to an assignment statement that is inserted immediately after the declaration.</span></span>

<span data-ttu-id="1ee89-215">Örnek</span><span class="sxs-lookup"><span data-stu-id="1ee89-215">The example</span></span>
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
<span data-ttu-id="1ee89-216">tam olarak öğesine karşılık gelir</span><span class="sxs-lookup"><span data-stu-id="1ee89-216">corresponds exactly to</span></span>
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

<span data-ttu-id="1ee89-217">Örtük olarak yazılmış bir yerel değişken bildiriminde, bildirildiği yerel değişkenin türü, değişkeni başlatmak için kullanılan ifadenin türüyle aynı olacak şekilde alınır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-217">In an implicitly typed local variable declaration, the type of the local variable being declared is taken to be the same as the type of the expression used to initialize the variable.</span></span> <span data-ttu-id="1ee89-218">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1ee89-218">For example:</span></span>
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

<span data-ttu-id="1ee89-219">Yukarıdaki örtük olarak yazılan yerel değişken bildirimleri aşağıdaki açıkça belirlenmiş bildirimlerle tam olarak eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-219">The implicitly typed local variable declarations above are precisely equivalent to the following explicitly typed declarations:</span></span>
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a><span data-ttu-id="1ee89-220">Yerel sabit bildirimler</span><span class="sxs-lookup"><span data-stu-id="1ee89-220">Local constant declarations</span></span>

<span data-ttu-id="1ee89-221">Bir *local_constant_declaration* bir veya daha fazla yerel sabit bildirir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-221">A *local_constant_declaration* declares one or more local constants.</span></span>

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

<span data-ttu-id="1ee89-222">Bir *local_constant_declaration* *türü* , bildirim tarafından tanıtılan sabitlerin türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-222">The *type* of a *local_constant_declaration* specifies the type of the constants introduced by the declaration.</span></span> <span data-ttu-id="1ee89-223">Türün ardından, her biri yeni bir sabit sunan bir *constant_declarator*s listesi bulunur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-223">The type is followed by a list of *constant_declarator*s, each of which introduces a new constant.</span></span> <span data-ttu-id="1ee89-224">Bir *constant_declarator* , sabit değerli bir *tanımlayıcıdan* , ardından bir "`=`" belirteci ve ardından sabit değeri veren bir *constant_expression* ([sabit ifadeler](expressions.md#constant-expressions)) ile oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-224">A *constant_declarator* consists of an *identifier* that names the constant, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the constant.</span></span>

<span data-ttu-id="1ee89-225">Yerel bir sabit bildirimin *türü* ve *constant_expression* , sabit üye bildirimiyle ([sabitler](classes.md#constants)) aynı kurallara uymalıdır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-225">The *type* and *constant_expression* of a local constant declaration must follow the same rules as those of a constant member declaration ([Constants](classes.md#constants)).</span></span>

<span data-ttu-id="1ee89-226">Yerel bir sabitin değeri, *simple_name* ([basit adlar](expressions.md#simple-names)) kullanılarak bir ifadede elde edilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-226">The value of a local constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="1ee89-227">Yerel bir sabitin kapsamı, bildirimin gerçekleştiği bloğudur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-227">The scope of a local constant is the block in which the declaration occurs.</span></span> <span data-ttu-id="1ee89-228">*Constant_declarator*önünde bir metinsel konumda yerel bir Sabitte başvurmak hatadır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-228">It is an error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span> <span data-ttu-id="1ee89-229">Yerel bir sabit kapsamında, aynı ada sahip başka bir yerel değişken veya sabit bildirmek için derleme zamanı hatası vardır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-229">Within the scope of a local constant, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="1ee89-230">Birden çok sabiti bildiren yerel bir sabit bildirim, aynı türe sahip tek sabitlerin birden çok bildirimine eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-230">A local constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same type.</span></span>

## <a name="expression-statements"></a><span data-ttu-id="1ee89-231">İfade deyimleri</span><span class="sxs-lookup"><span data-stu-id="1ee89-231">Expression statements</span></span>

<span data-ttu-id="1ee89-232">Bir *expression_statement* verilen bir ifadeyi değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-232">An *expression_statement* evaluates a given expression.</span></span> <span data-ttu-id="1ee89-233">Varsa, ifade tarafından hesaplanan değer atılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-233">The value computed by the expression, if any, is discarded.</span></span>

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

<span data-ttu-id="1ee89-234">Tüm ifadelere deyim olarak izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="1ee89-234">Not all expressions are permitted as statements.</span></span> <span data-ttu-id="1ee89-235">Özellikle, `x + y` ve `x == 1` gibi yalnızca bir değeri hesaplama (atılacak) gibi ifadeler, deyimler olarak izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="1ee89-235">In particular, expressions such as `x + y` and `x == 1` that merely compute a value (which will be discarded), are not permitted as statements.</span></span>

<span data-ttu-id="1ee89-236">Bir *expression_statement* öğesinin yürütülmesi içerilen ifadeyi değerlendirir ve sonra denetimi *expression_statement*bitiş noktasına aktarır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-236">Execution of an *expression_statement* evaluates the contained expression and then transfers control to the end point of the *expression_statement*.</span></span> <span data-ttu-id="1ee89-237">Bu *expression_statement* ulaşılabilir olduğunda bir *expression_statement* bitiş noktasına ulaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-237">The end point of an *expression_statement* is reachable if that *expression_statement* is reachable.</span></span>

## <a name="selection-statements"></a><span data-ttu-id="1ee89-238">Seçim deyimleri</span><span class="sxs-lookup"><span data-stu-id="1ee89-238">Selection statements</span></span>

<span data-ttu-id="1ee89-239">Seçim deyimleri, bazı deyimlerin değerine göre yürütme için bir dizi olası deyimden birini seçer.</span><span class="sxs-lookup"><span data-stu-id="1ee89-239">Selection statements select one of a number of possible statements for execution based on the value of some expression.</span></span>

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a><span data-ttu-id="1ee89-240">If ekstresi</span><span class="sxs-lookup"><span data-stu-id="1ee89-240">The if statement</span></span>

<span data-ttu-id="1ee89-241">Deyimi `if` , bir Boolean ifadesinin değerine göre yürütme için bir deyim seçer.</span><span class="sxs-lookup"><span data-stu-id="1ee89-241">The `if` statement selects a statement for execution based on the value of a boolean expression.</span></span>

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="1ee89-242">Bir `else` bölüm, söz dizimi tarafından izin verilen, `if` en yakın olan sözcüme ile ilişkilidir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-242">An `else` part is associated with the lexically nearest preceding `if` that is allowed by the syntax.</span></span> <span data-ttu-id="1ee89-243">Bu nedenle, `if` formun bir ifadesidir</span><span class="sxs-lookup"><span data-stu-id="1ee89-243">Thus, an `if` statement of the form</span></span>
```csharp
if (x) if (y) F(); else G();
```
<span data-ttu-id="1ee89-244">eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="1ee89-244">is equivalent to</span></span>
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

<span data-ttu-id="1ee89-245">Bir `if` ifade aşağıdaki gibi yürütülür:</span><span class="sxs-lookup"><span data-stu-id="1ee89-245">An `if` statement is executed as follows:</span></span>

*  <span data-ttu-id="1ee89-246">*Boolean_expression* ([Boolean ifadeler](expressions.md#boolean-expressions)) değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-246">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="1ee89-247">Boolean ifadesi `true`uygunsa denetim ilk eklenmiş ifadeye aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-247">If the boolean expression yields `true`, control is transferred to the first embedded statement.</span></span> <span data-ttu-id="1ee89-248">Ve denetimi bu deyimin bitiş noktasına ulaştığında, Denetim `if` deyimin bitiş noktasına aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-248">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="1ee89-249">Boolean ifadesi `false` varsa ve bir `else` bölüm varsa, denetim ikinci katıştırılmış deyime aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-249">If the boolean expression yields `false` and if an `else` part is present, control is transferred to the second embedded statement.</span></span> <span data-ttu-id="1ee89-250">Ve denetimi bu deyimin bitiş noktasına ulaştığında, Denetim `if` deyimin bitiş noktasına aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-250">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="1ee89-251">Boolean ifadesi `false` varsa ve bir `else` bölüm yoksa, Denetim `if` deyimin bitiş noktasına aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-251">If the boolean expression yields `false` and if an `else` part is not present, control is transferred to the end point of the `if` statement.</span></span>

<span data-ttu-id="1ee89-252">Deyim erişilebilir olduğunda ve Boolean ifadesinde `if` sabit değer `false`yoksa, bir `if` deyimin ilk ekli deyimine erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-252">The first embedded statement of an `if` statement is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="1ee89-253">Varsa, bir `if` deyimin ikinci gömülü deyimi, `if` deyimi erişilebilir olduğunda ve Boolean ifadesinde sabit değer `true`yoksa erişilebilir olur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-253">The second embedded statement of an `if` statement, if present, is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

<span data-ttu-id="1ee89-254">Bir `if` deyimin bitiş noktası, katıştırılmış deyimlerinden en az birinin bitiş noktası ulaşılabilir ise erişilebilir olur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-254">The end point of an `if` statement is reachable if the end point of at least one of its embedded statements is reachable.</span></span> <span data-ttu-id="1ee89-255">Ayrıca, `if` deyim erişilebilir olduğunda ve Boole ifadesinde `if` sabit değer `true`yoksa `else` , Bölüm içermeyen bir deyimin bitiş noktasına ulaşılamaz.</span><span class="sxs-lookup"><span data-stu-id="1ee89-255">In addition, the end point of an `if` statement with no `else` part is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-switch-statement"></a><span data-ttu-id="1ee89-256">Switch deyimleri</span><span class="sxs-lookup"><span data-stu-id="1ee89-256">The switch statement</span></span>

<span data-ttu-id="1ee89-257">Switch deyimi, anahtar ifadesinin değerine karşılık gelen ilişkili bir anahtar etiketine sahip bir deyim listesini yürütmeye yönelik seçer.</span><span class="sxs-lookup"><span data-stu-id="1ee89-257">The switch statement selects for execution a statement list having an associated switch label that corresponds to the value of the switch expression.</span></span>

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

<span data-ttu-id="1ee89-258">Bir *switch_statement* , öğesinden sonra parantez `switch`içine alınmış bir ifade (switch ifadesi olarak adlandırılır) ve ardından bir *switch_block*olan anahtar sözcüğünden oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-258">A *switch_statement* consists of the keyword `switch`, followed by a parenthesized expression (called the switch expression), followed by a *switch_block*.</span></span> <span data-ttu-id="1ee89-259">*Switch_block* , küme ayraçları içine alınmış sıfır veya daha fazla *switch_section*s içerir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-259">The *switch_block* consists of zero or more *switch_section*s, enclosed in braces.</span></span> <span data-ttu-id="1ee89-260">Her *switch_section* , bir veya daha fazla *switch_label*s öğesinden sonra bir *statement_list* ([ifade listesi](statements.md#statement-lists)) içerir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-260">Each *switch_section* consists of one or more *switch_label*s followed by a *statement_list* ([Statement lists](statements.md#statement-lists)).</span></span>

<span data-ttu-id="1ee89-261">Bir`switch` deyimin ***yöneten türü*** , switch ifadesi tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-261">The ***governing type*** of a `switch` statement is established by the switch expression.</span></span>

*  <span data-ttu-id="1ee89-262">Anahtar `sbyte`ifadesinin türü ,`uint` ,,`char`,,, ,,`ulong`, ,`string`veya `byte` `short` `ushort` `long` `int` `bool`  *enum_type*ya da bu türlerden birine karşılık gelen null yapılabilir türde ise, bu `switch` bildirimin yöneten türüdür.</span><span class="sxs-lookup"><span data-stu-id="1ee89-262">If the type of the switch expression is `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, or an *enum_type*, or if it is the nullable type corresponding to one of these types, then that is the governing type of the `switch` statement.</span></span>
*  <span data-ttu-id="1ee89-263">Aksi halde, tek bir Kullanıcı tanımlı örtük dönüştürme ([Kullanıcı tanımlı dönüştürmeler](conversions.md#user-defined-conversions)), anahtar ifadesinin türünden aşağıdaki olası bir tür düzenleyen türlerden `sbyte`birine varolmalıdır:, `byte`,, `ushort` `short` `int` ,,`ulong`, ,,`char`, veya,butürlerdenbirinekarşılıkgelennullyapılabilirbirtür`string`. `uint` `long`</span><span class="sxs-lookup"><span data-stu-id="1ee89-263">Otherwise, exactly one user-defined implicit conversion ([User-defined conversions](conversions.md#user-defined-conversions)) must exist from the type of the switch expression to one of the following possible governing types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, or,  a nullable type corresponding to one of those types.</span></span>
*  <span data-ttu-id="1ee89-264">Aksi takdirde, böyle bir örtük dönüştürme yoksa veya birden fazla örtük dönüştürme varsa, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-264">Otherwise, if no such implicit conversion exists, or if more than one such implicit conversion exists, a compile-time error occurs.</span></span>

<span data-ttu-id="1ee89-265">Her `case` bir etiketin sabit ifadesi örtük olarak dönüştürülebilir bir değeri ([örtük dönüştürmeler](conversions.md#implicit-conversions)) `switch` deyimin yöneten türüne göre belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-265">The constant expression of each `case` label must denote a value that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the `switch` statement.</span></span> <span data-ttu-id="1ee89-266">`case` Aynı`switch` deyimdeki iki veya daha fazla etiket aynı sabit değeri belirtmediğinde bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-266">A compile-time error occurs if two or more `case` labels in the same `switch` statement specify the same constant value.</span></span>

<span data-ttu-id="1ee89-267">Switch ifadesinde en fazla bir `default` etiket olabilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-267">There can be at most one `default` label in a switch statement.</span></span>

<span data-ttu-id="1ee89-268">Bir `switch` ifade aşağıdaki gibi yürütülür:</span><span class="sxs-lookup"><span data-stu-id="1ee89-268">A `switch` statement is executed as follows:</span></span>

*  <span data-ttu-id="1ee89-269">Anahtar ifadesi değerlendirilir ve yöneten türe dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="1ee89-269">The switch expression is evaluated and converted to the governing type.</span></span>
*  <span data-ttu-id="1ee89-270">Aynı `case` `case` deyimdeki bir etikette belirtilen sabitlerden biri, switch ifadesinin değerine eşitse, denetim eşleşen etiketten sonraki deyim listesine aktarılır. `switch`</span><span class="sxs-lookup"><span data-stu-id="1ee89-270">If one of the constants specified in a `case` label in the same `switch` statement is equal to the value of the switch expression, control is transferred to the statement list following the matched `case` label.</span></span>
*  <span data-ttu-id="1ee89-271">Aynı `case` `default` `default` deyimde Etiketler içinde belirtilen sabitlerden hiçbiri anahtar ifadesinin değerine eşit değilse ve bir etiket mevcutsa, Denetim aşağıdaki ifadeyi izleyen deyim listesine aktarılır. `switch` etiketin.</span><span class="sxs-lookup"><span data-stu-id="1ee89-271">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if a `default` label is present, control is transferred to the statement list following the `default` label.</span></span>
*  <span data-ttu-id="1ee89-272">Aynı `case` `default` `switch` deyimdeki etiketlerde belirtilen sabitlerin hiçbiri anahtar ifadesinin değerine eşit değilse ve bir etiket yoksa, denetim deyimin bitiş noktasına aktarılır. `switch`</span><span class="sxs-lookup"><span data-stu-id="1ee89-272">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if no `default` label is present, control is transferred to the end point of the `switch` statement.</span></span>

<span data-ttu-id="1ee89-273">Bir anahtar bölümünün ekstre listesinin bitiş noktasına ulaşılabiliyorsa, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-273">If the end point of the statement list of a switch section is reachable, a compile-time error occurs.</span></span> <span data-ttu-id="1ee89-274">Bu, "hiçbir düşüş olmadan" kuralı olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-274">This is known as the "no fall through" rule.</span></span> <span data-ttu-id="1ee89-275">Örnek</span><span class="sxs-lookup"><span data-stu-id="1ee89-275">The example</span></span>
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
<span data-ttu-id="1ee89-276">, Switch bölümünün erişilebilir bir uç noktasına sahip olmadığı için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-276">is valid because no switch section has a reachable end point.</span></span> <span data-ttu-id="1ee89-277">C ve C++' nin aksine, bir switch bölümünün yürütülmesi bir sonraki anahtar bölümüne "dönemez" ve örnek</span><span class="sxs-lookup"><span data-stu-id="1ee89-277">Unlike C and C++, execution of a switch section is not permitted to "fall through" to the next switch section, and the example</span></span>
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
<span data-ttu-id="1ee89-278">derleme zamanı hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-278">results in a compile-time error.</span></span> <span data-ttu-id="1ee89-279">Bir switch bölümünün yürütülmesi başka bir anahtar bölümünün yürütülmesi tarafından izlenmesinin ardından açık `goto case` veya `goto default` bir deyimin kullanılması gerekir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-279">When execution of a switch section is to be followed by execution of another switch section, an explicit `goto case` or `goto default` statement must be used:</span></span>
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

<span data-ttu-id="1ee89-280">Bir *switch_section*içinde birden çok etikete izin verilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-280">Multiple labels are permitted in a *switch_section*.</span></span> <span data-ttu-id="1ee89-281">Örnek</span><span class="sxs-lookup"><span data-stu-id="1ee89-281">The example</span></span>
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
<span data-ttu-id="1ee89-282">geçerli.</span><span class="sxs-lookup"><span data-stu-id="1ee89-282">is valid.</span></span> <span data-ttu-id="1ee89-283">Bu örnek, Etiketler `case 2:` ve `default:` aynı *switch_section*bir parçası olduğundan "hiçbir düşüş olmadan" kuralını ihlal etmez.</span><span class="sxs-lookup"><span data-stu-id="1ee89-283">The example does not violate the "no fall through" rule because the labels `case 2:` and `default:` are part of the same *switch_section*.</span></span>

<span data-ttu-id="1ee89-284">"Fall yok" kuralı, C 'de oluşan ve C++ `break` deyimler yanlışlıkla atlandığında ortak bir hata sınıfını engeller.</span><span class="sxs-lookup"><span data-stu-id="1ee89-284">The "no fall through" rule prevents a common class of bugs that occur in C and C++ when `break` statements are accidentally omitted.</span></span> <span data-ttu-id="1ee89-285">Ayrıca, bu kural nedeniyle, bir `switch` deyimin anahtar bölümleri deyimin davranışını etkilemeden rastgele bir şekilde yeniden düzenlenebilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-285">In addition, because of this rule, the switch sections of a `switch` statement can be arbitrarily rearranged without affecting the behavior of the statement.</span></span> <span data-ttu-id="1ee89-286">Örneğin, yukarıdaki `switch` deyimin bölümleri, deyimin davranışı etkilenmeden ters alınabilir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-286">For example, the sections of the `switch` statement above can be reversed without affecting the behavior of the statement:</span></span>
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

<span data-ttu-id="1ee89-287">Bir switch bölümünün ifade listesi genellikle bir `break`, `goto case`veya `goto default` ifadesinde sona erer, ancak bildirim listesinin ulaşılamaz bitiş noktasını işleyen herhangi bir yapı için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-287">The statement list of a switch section typically ends in a `break`, `goto case`, or `goto default` statement, but any construct that renders the end point of the statement list unreachable is permitted.</span></span> <span data-ttu-id="1ee89-288">Örneğin, Boolean ifadesinin `while` `true` denetimindeki bir deyim, uç noktasına hiçbir şekilde ulaşamadığı bilinmektedir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-288">For example, a `while` statement controlled by the boolean expression `true` is known to never reach its end point.</span></span> <span data-ttu-id="1ee89-289">Benzer şekilde, `throw` bir `return` veya deyimleri her zaman denetimi başka bir yere aktarır ve bitiş noktasına hiçbir zaman ulaşmaz.</span><span class="sxs-lookup"><span data-stu-id="1ee89-289">Likewise, a `throw` or `return` statement always transfers control elsewhere and never reaches its end point.</span></span> <span data-ttu-id="1ee89-290">Bu nedenle, aşağıdaki örnek geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-290">Thus, the following example is valid:</span></span>
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

<span data-ttu-id="1ee89-291">Bir `switch` deyimin yöneten türü tür `string`olabilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-291">The governing type of a `switch` statement may be the type `string`.</span></span> <span data-ttu-id="1ee89-292">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1ee89-292">For example:</span></span>
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

<span data-ttu-id="1ee89-293">Dize eşitlik işleçleri ([dize eşitlik işleçleri](expressions.md#string-equality-operators)) gibi, `switch` deyim büyük/küçük harfe duyarlıdır ve yalnızca anahtar ifadesi dizesi bir `case` etiket sabiti ile tam olarak eşleşiyorsa verilen bir anahtar bölümünü yürütür.</span><span class="sxs-lookup"><span data-stu-id="1ee89-293">Like the string equality operators ([String equality operators](expressions.md#string-equality-operators)), the `switch` statement is case sensitive and will execute a given switch section only if the switch expression string exactly matches a `case` label constant.</span></span>

<span data-ttu-id="1ee89-294">Bir `switch` deyiminyöneten`null` türü olduğunda ,değerebirCaseetiketisabitiolarakizinverilir.`string`</span><span class="sxs-lookup"><span data-stu-id="1ee89-294">When the governing type of a `switch` statement is `string`, the value `null` is permitted as a case label constant.</span></span>

<span data-ttu-id="1ee89-295">Bir *switch_block* *statement_list*s, bildirim deyimlerini ([bildirim deyimleri](statements.md#declaration-statements)) içerebilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-295">The *statement_list*s of a *switch_block* may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="1ee89-296">Anahtar bloğunda belirtilen yerel bir değişken veya sabit kapsam, anahtar bloğudur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-296">The scope of a local variable or constant declared in a switch block is the switch block.</span></span>

<span data-ttu-id="1ee89-297">Belirli bir switch bölümünün ifade listesi, `switch` ifadeye ulaşılabiliyorsa ve aşağıdakilerden en az biri geçerliyse erişilebilir olur:</span><span class="sxs-lookup"><span data-stu-id="1ee89-297">The statement list of a given switch section is reachable if the `switch` statement is reachable and at least one of the following is true:</span></span>

*  <span data-ttu-id="1ee89-298">Switch ifadesi sabit olmayan bir değer.</span><span class="sxs-lookup"><span data-stu-id="1ee89-298">The switch expression is a non-constant value.</span></span>
*  <span data-ttu-id="1ee89-299">Switch ifadesi, Switch bölümündeki bir `case` etiketle eşleşen sabit bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-299">The switch expression is a constant value that matches a `case` label in the switch section.</span></span>
*  <span data-ttu-id="1ee89-300">Switch ifadesi herhangi `case` bir etiketle eşleşmeyen sabit bir değerdir ve Switch bölümü `default` etiketi içerir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-300">The switch expression is a constant value that doesn't match any `case` label, and the switch section contains the `default` label.</span></span>
*  <span data-ttu-id="1ee89-301">Switch bölümünün anahtar etiketine erişilebilir `goto case` veya `goto default` bildirim tarafından başvuruluyor.</span><span class="sxs-lookup"><span data-stu-id="1ee89-301">A switch label of the switch section is referenced by a reachable `goto case` or `goto default` statement.</span></span>

<span data-ttu-id="1ee89-302">Aşağıdakilerden en az biri doğru `switch` ise, bir deyimin bitiş noktasına ulaşılabilir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-302">The end point of a `switch` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="1ee89-303">`switch` İfade `break` , ifadesiyle`switch` çıkış yapan erişilebilir bir ifade içeriyor.</span><span class="sxs-lookup"><span data-stu-id="1ee89-303">The `switch` statement contains a reachable `break` statement that exits the `switch` statement.</span></span>
*  <span data-ttu-id="1ee89-304">Deyim erişilebilir, anahtar ifadesi sabit olmayan bir değer `default` ve etiket yok. `switch`</span><span class="sxs-lookup"><span data-stu-id="1ee89-304">The `switch` statement is reachable, the switch expression is a non-constant value, and no `default` label is present.</span></span>
*  <span data-ttu-id="1ee89-305">Deyim erişilebilir, anahtar ifadesi hiçbir `case` etiketle eşleşmeyen bir sabit değerdir ve hiçbir `default` etiket yoktur. `switch`</span><span class="sxs-lookup"><span data-stu-id="1ee89-305">The `switch` statement is reachable, the switch expression is a constant value that doesn't match any `case` label, and no `default` label is present.</span></span>

## <a name="iteration-statements"></a><span data-ttu-id="1ee89-306">Yineleme deyimleri</span><span class="sxs-lookup"><span data-stu-id="1ee89-306">Iteration statements</span></span>

<span data-ttu-id="1ee89-307">Yineleme deyimleri, art arda gömülü bir deyimi yürütür.</span><span class="sxs-lookup"><span data-stu-id="1ee89-307">Iteration statements repeatedly execute an embedded statement.</span></span>

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a><span data-ttu-id="1ee89-308">While ekstresi</span><span class="sxs-lookup"><span data-stu-id="1ee89-308">The while statement</span></span>

<span data-ttu-id="1ee89-309">İfade `while` , gömülü bir ifadeyi sıfır veya daha fazla kez koşullu olarak yürütür.</span><span class="sxs-lookup"><span data-stu-id="1ee89-309">The `while` statement conditionally executes an embedded statement zero or more times.</span></span>

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

<span data-ttu-id="1ee89-310">Bir `while` ifade aşağıdaki gibi yürütülür:</span><span class="sxs-lookup"><span data-stu-id="1ee89-310">A `while` statement is executed as follows:</span></span>

*  <span data-ttu-id="1ee89-311">*Boolean_expression* ([Boolean ifadeler](expressions.md#boolean-expressions)) değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-311">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="1ee89-312">Boolean ifadesi `true`uygunsa denetim katıştırılmış deyime aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-312">If the boolean expression yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="1ee89-313">Ve denetimi katıştırılmış deyimin bitiş noktasına ulaşırsa (muhtemelen bir `continue` deyimin yürütülmesinden), Denetim `while` deyimin başlangıcına aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-313">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), control is transferred to the beginning of the `while` statement.</span></span>
*  <span data-ttu-id="1ee89-314">Boolean ifadesi `false`uygunsa denetim `while` deyimin bitiş noktasına aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-314">If the boolean expression yields `false`, control is transferred to the end point of the `while` statement.</span></span>

<span data-ttu-id="1ee89-315">Bir `while` deyimin gömülü ifadesinde `break` , `while` denetimi deyimin bitiş noktasına (Bu nedenle katıştırılmış deyimin sonuna kadar)[](statements.md#the-break-statement) aktarmakiçinbirifade(breakdeyimleri)kullanılabilir.`continue` deyimin ([devam eden bildiri](statements.md#the-continue-statement)), denetimi katıştırılmış deyimin bitiş noktasına aktarmak için kullanılabilir (Bu nedenle `while` deyimin başka bir yinelemesi gerçekleştiriliyor).</span><span class="sxs-lookup"><span data-stu-id="1ee89-315">Within the embedded statement of a `while` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `while` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus performing another iteration of the `while` statement).</span></span>

<span data-ttu-id="1ee89-316">Deyim erişilebilir olduğunda ve Boolean `while` ifadesinde sabit değer `false`yoksa, bir deyimin gömülü deyimine erişilebilir. `while`</span><span class="sxs-lookup"><span data-stu-id="1ee89-316">The embedded statement of a `while` statement is reachable if the `while` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="1ee89-317">Aşağıdakilerden en az biri doğru `while` ise, bir deyimin bitiş noktasına ulaşılabilir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-317">The end point of a `while` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="1ee89-318">`while` İfade `break` , ifadesiyle`while` çıkış yapan erişilebilir bir ifade içeriyor.</span><span class="sxs-lookup"><span data-stu-id="1ee89-318">The `while` statement contains a reachable `break` statement that exits the `while` statement.</span></span>
*  <span data-ttu-id="1ee89-319">Deyimine erişilebilir ve Boole ifadesi sabit değere `true`sahip değil. `while`</span><span class="sxs-lookup"><span data-stu-id="1ee89-319">The `while` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-do-statement"></a><span data-ttu-id="1ee89-320">Do ekstresi</span><span class="sxs-lookup"><span data-stu-id="1ee89-320">The do statement</span></span>

<span data-ttu-id="1ee89-321">İfade `do` , gömülü bir ifadeyi bir veya daha fazla kez koşullu olarak yürütür.</span><span class="sxs-lookup"><span data-stu-id="1ee89-321">The `do` statement conditionally executes an embedded statement one or more times.</span></span>

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

<span data-ttu-id="1ee89-322">Bir `do` ifade aşağıdaki gibi yürütülür:</span><span class="sxs-lookup"><span data-stu-id="1ee89-322">A `do` statement is executed as follows:</span></span>

*  <span data-ttu-id="1ee89-323">Denetim katıştırılmış deyime aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-323">Control is transferred to the embedded statement.</span></span>
*  <span data-ttu-id="1ee89-324">Ve denetimi katıştırılmış deyimin bitiş noktasına ulaşırsa (muhtemelen bir `continue` deyimin yürütülmesinden), *Boolean_expression* ([Boolean ifadeler](expressions.md#boolean-expressions)) değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-324">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span> <span data-ttu-id="1ee89-325">Boolean ifadesi `true`uygunsa denetim `do` deyimin başlangıcına aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-325">If the boolean expression yields `true`, control is transferred to the beginning of the `do` statement.</span></span> <span data-ttu-id="1ee89-326">Aksi takdirde denetim, `do` deyimin bitiş noktasına aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-326">Otherwise, control is transferred to the end point of the `do` statement.</span></span>

<span data-ttu-id="1ee89-327">Bir `do` deyimin gömülü ifadesinde `break` , `do` denetimi deyimin bitiş noktasına (Bu nedenle katıştırılmış deyimin sonuna kadar)[](statements.md#the-break-statement) aktarmakiçinbirifade(breakdeyimleri)kullanılabilir.`continue` bildirim ([Continue bildirisi](statements.md#the-continue-statement)), denetimi katıştırılmış deyimin bitiş noktasına aktarmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-327">Within the embedded statement of a `do` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `do` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement.</span></span>

<span data-ttu-id="1ee89-328">Deyimden ulaşılabilir olduğunda bir `do` deyimin gömülü ifadesine ulaşılabilir. `do`</span><span class="sxs-lookup"><span data-stu-id="1ee89-328">The embedded statement of a `do` statement is reachable if the `do` statement is reachable.</span></span>

<span data-ttu-id="1ee89-329">Aşağıdakilerden en az biri doğru `do` ise, bir deyimin bitiş noktasına ulaşılabilir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-329">The end point of a `do` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="1ee89-330">`do` İfade `break` , ifadesiyle`do` çıkış yapan erişilebilir bir ifade içeriyor.</span><span class="sxs-lookup"><span data-stu-id="1ee89-330">The `do` statement contains a reachable `break` statement that exits the `do` statement.</span></span>
*  <span data-ttu-id="1ee89-331">Katıştırılmış deyimin bitiş noktasına ulaşılabilir ve Boole ifadesi sabit değere `true`sahip değil.</span><span class="sxs-lookup"><span data-stu-id="1ee89-331">The end point of the embedded statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-for-statement"></a><span data-ttu-id="1ee89-332">For deyimleri</span><span class="sxs-lookup"><span data-stu-id="1ee89-332">The for statement</span></span>

<span data-ttu-id="1ee89-333">`for` Deyimi, bir dizi başlatma ifadesi değerlendirir ve sonra bir koşul true olduğunda, gömülü bir deyimi tekrar tekrar yürütür ve bir dizi yineleme ifadesini değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-333">The `for` statement evaluates a sequence of initialization expressions and then, while a condition is true, repeatedly executes an embedded statement and evaluates a sequence of iteration expressions.</span></span>

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

<span data-ttu-id="1ee89-334">*For_initializer*, varsa, virgülle ayrılmış bir *local_variable_declaration* ([yerel değişken bildirimleri](statements.md#local-variable-declarations)) ya da bir *statement_expression*s ([ifade deyimleri](statements.md#expression-statements)) listesinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-334">The *for_initializer*, if present, consists of either a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) or a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span> <span data-ttu-id="1ee89-335">*For_initializer* tarafından belirtilen bir yerel değişkenin kapsamı, değişken için *local_variable_declarator* başlar ve ekli deyimin sonuna genişletilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-335">The scope of a local variable declared by a *for_initializer* starts at the *local_variable_declarator* for the variable and extends to the end of the embedded statement.</span></span> <span data-ttu-id="1ee89-336">Kapsam *for_condition* ve *for_iterator*içerir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-336">The scope includes the *for_condition* and the *for_iterator*.</span></span>

<span data-ttu-id="1ee89-337">*For_condition*, varsa, bir *Boolean_expression* ([Boolean ifadeleri](expressions.md#boolean-expressions)) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-337">The *for_condition*, if present, must be a *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)).</span></span>

<span data-ttu-id="1ee89-338">Varsa *for_iterator*, virgülle ayrılmış bir *Statement_expression*s ([Expression deyimleri](statements.md#expression-statements)) listesinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-338">The *for_iterator*, if present, consists of a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span>

<span data-ttu-id="1ee89-339">A for deyimleri aşağıdaki gibi yürütülür:</span><span class="sxs-lookup"><span data-stu-id="1ee89-339">A for statement is executed as follows:</span></span>

*  <span data-ttu-id="1ee89-340">Bir *for_initializer* varsa, değişken başlatıcıları veya deyim ifadeleri yazıldığı sırada yürütülür.</span><span class="sxs-lookup"><span data-stu-id="1ee89-340">If a *for_initializer* is present, the variable initializers or statement expressions are executed in the order they are written.</span></span> <span data-ttu-id="1ee89-341">Bu adım yalnızca bir kez gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-341">This step is only performed once.</span></span>
*  <span data-ttu-id="1ee89-342">Bir *for_condition* varsa, değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-342">If a *for_condition* is present, it is evaluated.</span></span>
*  <span data-ttu-id="1ee89-343">*For_condition* yoksa veya değerlendirme `true`uygunsa denetim katıştırılmış deyime aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-343">If the *for_condition* is not present or if the evaluation yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="1ee89-344">Ve denetimi katıştırılmış deyimin bitiş noktasına ulaştığında (büyük olasılıkla bir `continue` deyimin yürütülmesinden), varsa, *for_iterator*ifadeleri sırayla değerlendirilir ve ardından, ile başlayan başka bir yineleme yapılır. Yukarıdaki adımda *for_condition* değerlendirmesi.</span><span class="sxs-lookup"><span data-stu-id="1ee89-344">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the expressions of the *for_iterator*, if any, are evaluated in sequence, and then another iteration is performed, starting with evaluation of the *for_condition* in the step above.</span></span>
*  <span data-ttu-id="1ee89-345">*For_condition* varsa ve değerlendirme `false`uygunsa denetim, `for` deyimin bitiş noktasına aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-345">If the *for_condition* is present and the evaluation yields `false`, control is transferred to the end point of the `for` statement.</span></span>

<span data-ttu-id="1ee89-346">Bir `for` deyimin gömülü ifadesinde `break` , `for` denetimi deyimin bitiş noktasına (Bu nedenle katıştırılmış deyimin sonuna kadar)[](statements.md#the-break-statement) aktarmakiçinbirifade(breakdeyimleri)kullanılabilir.`continue` deyimin ([Continue deyimleri](statements.md#the-continue-statement)), denetimi katıştırılmış deyimin bitiş noktasına aktarmak için kullanılabilir (Bu nedenle, *for_iterator* öğesini yürüten ve `for` *for_condition*).</span><span class="sxs-lookup"><span data-stu-id="1ee89-346">Within the embedded statement of a `for` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `for` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus executing the *for_iterator* and performing another iteration of the `for` statement, starting with the *for_condition*).</span></span>

<span data-ttu-id="1ee89-347">Aşağıdakilerden biri doğruysa, bir `for` deyimin gömülü ifadesine ulaşılabilir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-347">The embedded statement of a `for` statement is reachable if one of the following is true:</span></span>

*  <span data-ttu-id="1ee89-348">İfadeye ulaşılamıyor ve hiçbir for_condition yok. `for`</span><span class="sxs-lookup"><span data-stu-id="1ee89-348">The `for` statement is reachable and no *for_condition* is present.</span></span>
*  <span data-ttu-id="1ee89-349">Deyime erişilebilir ve bir *for_condition* var ve sabit değere `false`sahip değil. `for`</span><span class="sxs-lookup"><span data-stu-id="1ee89-349">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `false`.</span></span>

<span data-ttu-id="1ee89-350">Aşağıdakilerden en az biri doğru `for` ise, bir deyimin bitiş noktasına ulaşılabilir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-350">The end point of a `for` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="1ee89-351">`for` İfade `break` , ifadesiyle`for` çıkış yapan erişilebilir bir ifade içeriyor.</span><span class="sxs-lookup"><span data-stu-id="1ee89-351">The `for` statement contains a reachable `break` statement that exits the `for` statement.</span></span>
*  <span data-ttu-id="1ee89-352">Deyime erişilebilir ve bir *for_condition* var ve sabit değere `true`sahip değil. `for`</span><span class="sxs-lookup"><span data-stu-id="1ee89-352">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `true`.</span></span>

### <a name="the-foreach-statement"></a><span data-ttu-id="1ee89-353">Foreach ifadesi</span><span class="sxs-lookup"><span data-stu-id="1ee89-353">The foreach statement</span></span>

<span data-ttu-id="1ee89-354">`foreach` İfade, koleksiyonun öğelerinin her öğesi için gömülü bir ifade yürüten bir koleksiyonun öğelerini numaralandırır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-354">The `foreach` statement enumerates the elements of a collection, executing an embedded statement for each element of the collection.</span></span>

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

<span data-ttu-id="1ee89-355">`foreach` Deyimin *türü* ve *tanımlayıcısı* deyimin ***yineleme değişkenini*** bildirir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-355">The *type* and *identifier* of a `foreach` statement declare the ***iteration variable*** of the statement.</span></span> <span data-ttu-id="1ee89-356">Tanımlayıcı, *local_variable_type*olarak verilirse ve adında `var` bir tür varsa, yineleme değişkeni ***örtük olarak yazılmış bir yineleme değişkeni***olarak adlandırılır ve türü, öğe türü olan `var` `foreach` ifadesini aşağıda belirtildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="1ee89-356">If the `var` identifier is given as the *local_variable_type*, and no type named `var` is in scope, the iteration variable is said to be an ***implicitly typed iteration variable***, and its type is taken to be the element type of the `foreach` statement, as specified below.</span></span> <span data-ttu-id="1ee89-357">Yineleme değişkeni, katıştırılmış deyimin kapsamını genişleten bir salt okunurdur yerel değişkene karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-357">The iteration variable corresponds to a read-only local variable with a scope that extends over the embedded statement.</span></span> <span data-ttu-id="1ee89-358">Bir `foreach` deyimin yürütülmesi sırasında yineleme değişkeni, bir yinelemenin Şu anda gerçekleştirildiği koleksiyon öğesini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="1ee89-358">During execution of a `foreach` statement, the iteration variable represents the collection element for which an iteration is currently being performed.</span></span> <span data-ttu-id="1ee89-359">Katıştırılmış deyimin yineleme değişkenini `++` (atama ya da ve `--` işleçler aracılığıyla) değiştirmeye veya yineleme değişkenini bir `ref` veya `out` parametresi olarak geçirmeye çalışırsa bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-359">A compile-time error occurs if the embedded statement attempts to modify the iteration variable (via assignment or the `++` and `--` operators) or pass the iteration variable as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="1ee89-360">`IEnumerable`Aşağıda, kısaltma `IEnumerator` `IEnumerator<T>` `System.Collections` için,, vead`System.Collections.Generic`alanları ve içindeki ilgili türlere bakın. `IEnumerable<T>`</span><span class="sxs-lookup"><span data-stu-id="1ee89-360">In the following, for brevity, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` and `IEnumerator<T>` refer to the corresponding types in the namespaces `System.Collections` and `System.Collections.Generic`.</span></span>

<span data-ttu-id="1ee89-361">Bir foreach deyiminin derleme zamanı işleme öncelikle deyimin ***koleksiyon türünü***, ***Numaralandırıcı türünü*** ve ***öğe türünü*** belirler.</span><span class="sxs-lookup"><span data-stu-id="1ee89-361">The compile-time processing of a foreach statement first determines the ***collection type***, ***enumerator type*** and ***element type*** of the expression.</span></span> <span data-ttu-id="1ee89-362">Bu belirleme işlemi aşağıdaki gibi gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-362">This determination proceeds as follows:</span></span>

*  <span data-ttu-id="1ee89-363">`X` *İfade* türü bir dizi türü ise, `X` `IEnumerable` arabirime bir örtük başvuru dönüştürmesi vardır (Bu arabirimden bu yana `System.Array` ).</span><span class="sxs-lookup"><span data-stu-id="1ee89-363">If the type `X` of *expression* is an array type then there is an implicit reference conversion from `X` to the `IEnumerable` interface (since `System.Array` implements this interface).</span></span> <span data-ttu-id="1ee89-364">***Koleksiyon türü*** `IEnumerable` arabirimdir, ***Numaralandırıcı türü*** `IEnumerator` arayüztür ve ***öğe*** türü dizi türünün `X`öğe türüdür.</span><span class="sxs-lookup"><span data-stu-id="1ee89-364">The ***collection type*** is the `IEnumerable` interface, the ***enumerator type*** is the `IEnumerator` interface and the ***element type*** is the element type of the array type `X`.</span></span>
*  <span data-ttu-id="1ee89-365">`X` `IEnumerable` *İfadenin* [](conversions.md#implicit-dynamic-conversions)türü daha sonra deyimden arabirime (örtük dinamik dönüştürmeler) örtük bir dönüşüm varsa. `dynamic`</span><span class="sxs-lookup"><span data-stu-id="1ee89-365">If the type `X` of *expression* is `dynamic` then there is an implicit conversion from *expression* to the `IEnumerable` interface ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)).</span></span> <span data-ttu-id="1ee89-366">***Koleksiyon türü*** `IEnumerable` arayüztür ve `IEnumerator` ***Numaralandırıcı türü arayüztür*** .</span><span class="sxs-lookup"><span data-stu-id="1ee89-366">The ***collection type*** is the `IEnumerable` interface and the ***enumerator type*** is the `IEnumerator` interface.</span></span> <span data-ttu-id="1ee89-367">`dynamic` `object` Tanımlayıcı local_variable_type olarak verilirse öğe türü olur, aksi durumda. `var`</span><span class="sxs-lookup"><span data-stu-id="1ee89-367">If the `var` identifier is given as the *local_variable_type* then the ***element type*** is `dynamic`, otherwise it is `object`.</span></span>
*  <span data-ttu-id="1ee89-368">Aksi takdirde, türün `X` uygun `GetEnumerator` bir yönteme sahip olup olmadığını saptayın:</span><span class="sxs-lookup"><span data-stu-id="1ee89-368">Otherwise, determine whether the type `X` has an appropriate `GetEnumerator` method:</span></span>
   * <span data-ttu-id="1ee89-369">`X` Tanımlayıcı`GetEnumerator` ve tür bağımsız değişkeni olmayan tür üzerinde üye araması gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="1ee89-369">Perform member lookup on the type `X` with identifier `GetEnumerator` and no type arguments.</span></span> <span data-ttu-id="1ee89-370">Üye arama bir eşleşme oluşturmuyorsa veya bir belirsizlik üretir ya da bir yöntem grubu olmayan bir eşleşme üretirse, aşağıda açıklandığı gibi sıralanabilir bir arabirim olup olmadığını kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="1ee89-370">If the member lookup does not produce a match, or it produces an ambiguity, or produces a match that is not a method group, check for an enumerable interface as described below.</span></span> <span data-ttu-id="1ee89-371">Üye arama yöntemi bir yöntem grubu veya eşleşme dışında bir şey üretirse bir uyarı verilmesi önerilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-371">It is recommended that a warning be issued if member lookup produces anything except a method group or no match.</span></span>
   * <span data-ttu-id="1ee89-372">Elde edilen yöntem grubunu ve boş bir bağımsız değişken listesini kullanarak aşırı yükleme çözümlemesi gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="1ee89-372">Perform overload resolution using the resulting method group and an empty argument list.</span></span> <span data-ttu-id="1ee89-373">Aşırı yükleme çözümlemesi uygulanabilir bir yöntem olmadan sonuçlanırsa, belirsizliğe neden olur veya tek bir en iyi yöntemle sonuçlanır, ancak bu yöntem statik veya genel değil, aşağıda açıklandığı gibi sıralanabilir bir arabirim olup olmadığını kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="1ee89-373">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, check for an enumerable interface as described below.</span></span> <span data-ttu-id="1ee89-374">Aşırı yükleme çözümlemesi, belirsiz bir ortak örnek yöntemi veya uygulanabilir Yöntemler dışında bir şey üretirse bir uyarı verilmesi önerilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-374">It is recommended that a warning be issued if overload resolution produces anything except an unambiguous public instance method or no applicable methods.</span></span>
   * <span data-ttu-id="1ee89-375">`GetEnumerator` Yöntemin dönüş türü `E` bir sınıf, yapı veya arabirim türü değilse, bir hata oluşturulur ve başka bir adım alınmaz.</span><span class="sxs-lookup"><span data-stu-id="1ee89-375">If the return type `E` of the `GetEnumerator` method is not a class, struct or interface type, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="1ee89-376">Üye arama, tanımlayıcısı `E` `Current` ve tür bağımsız değişkenleri olmadan üzerinde gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-376">Member lookup is performed on `E` with the identifier `Current` and no type arguments.</span></span> <span data-ttu-id="1ee89-377">Üye arama hiçbir eşleşme oluşturmazsa, sonuç bir hatadır veya sonuç, okumaya izin veren bir ortak örnek özelliği dışında bir hata oluşturulur ve başka hiçbir adım alınmaz.</span><span class="sxs-lookup"><span data-stu-id="1ee89-377">If the member lookup produces no match, the result is an error, or the result is anything except a public instance property that permits reading, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="1ee89-378">Üye arama, tanımlayıcısı `E` `MoveNext` ve tür bağımsız değişkenleri olmadan üzerinde gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-378">Member lookup is performed on `E` with the identifier `MoveNext` and no type arguments.</span></span> <span data-ttu-id="1ee89-379">Üye arama hiçbir eşleşme oluşturmazsa, sonuç bir hatadır veya sonuç bir yöntem grubu dışında bir hata üretiyorsa, başka bir adım alınmaz.</span><span class="sxs-lookup"><span data-stu-id="1ee89-379">If the member lookup produces no match, the result is an error, or the result is anything except a method group, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="1ee89-380">Aşırı yükleme çözümlemesi, metot grubunda boş bir bağımsız değişken listesiyle gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-380">Overload resolution is performed on the method group with an empty argument list.</span></span> <span data-ttu-id="1ee89-381">Aşırı yükleme çözümlemesi uygulanabilir bir yöntem olmadan sonuçlanırsa, bir belirsizliğe neden olur ya da tek bir en iyi yöntem ile sonuçlanır, ancak bu yöntem statik veya genel değildir veya dönüş türü değildir `bool`, bir hata üretilir ve başka bir adım alınmaz.</span><span class="sxs-lookup"><span data-stu-id="1ee89-381">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, or its return type is not `bool`, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="1ee89-382">***Koleksiyon türü*** `X`, ***Numaralandırıcı türüdür*** `E` ***ve öğe türü*** özelliğintürüdür.`Current`</span><span class="sxs-lookup"><span data-stu-id="1ee89-382">The ***collection type*** is `X`, the ***enumerator type*** is `E`, and the ***element type*** is the type of the `Current` property.</span></span>

*  <span data-ttu-id="1ee89-383">Aksi takdirde, sıralanabilir bir arabirim olup olmadığını kontrol edin:</span><span class="sxs-lookup"><span data-stu-id="1ee89-383">Otherwise, check for an enumerable interface:</span></span>
   * <span data-ttu-id="1ee89-384">`Ti` `dynamic` `T` `Ti`'Den örtükbirdönüştürmeolantümtürlerarasında,olmayanbirtürüvardırvebununiçin`T` `X` `IEnumerable<Ti>` `IEnumerable<T>` öğesinden öğesine `IEnumerator<T>` `IEnumerable<T>` örtük dönüştürme, sonra koleksiyon türü arabirimdir, Numaralandırıcı türü arayüztür ve öğe türü `IEnumerable<Ti>` `T`.</span><span class="sxs-lookup"><span data-stu-id="1ee89-384">If among all the types `Ti` for which there is an implicit conversion from `X` to `IEnumerable<Ti>`, there is a unique type `T` such that `T` is not `dynamic` and for all the other `Ti` there is an implicit conversion from `IEnumerable<T>` to `IEnumerable<Ti>`, then the ***collection type*** is the interface `IEnumerable<T>`, the ***enumerator type*** is the interface `IEnumerator<T>`, and the ***element type*** is `T`.</span></span>
   * <span data-ttu-id="1ee89-385">Aksi takdirde, böyle birden çok tür `T`varsa, bir hata oluşturulur ve başka bir adım alınmaz.</span><span class="sxs-lookup"><span data-stu-id="1ee89-385">Otherwise, if there is more than one such type `T`, then an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="1ee89-386">Aksi `X` halde, `System.Collections.IEnumerable` arabirimine örtük bir dönüştürme varsa, ***koleksiyon türü*** bu arabirimdir, ***Numaralandırıcı türü arayüztür*** `System.Collections.IEnumerator`ve ***öğe türü*** `object`.</span><span class="sxs-lookup"><span data-stu-id="1ee89-386">Otherwise, if there is an implicit conversion from `X` to the `System.Collections.IEnumerable` interface, then the ***collection type*** is this interface, the ***enumerator type*** is the interface `System.Collections.IEnumerator`, and the ***element type*** is `object`.</span></span>
   * <span data-ttu-id="1ee89-387">Aksi halde, bir hata oluşturulur ve başka bir adım alınmaz.</span><span class="sxs-lookup"><span data-stu-id="1ee89-387">Otherwise, an error is produced and no further steps are taken.</span></span>

<span data-ttu-id="1ee89-388">Yukarıdaki adımlar başarılı olursa, kesin bir şekilde koleksiyon türü `C`, Numaralandırıcı türü `E` ve öğe türü `T`üretir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-388">The above steps, if successful, unambiguously produce a collection type `C`, enumerator type `E` and element type `T`.</span></span> <span data-ttu-id="1ee89-389">Formun foreach ifadesi</span><span class="sxs-lookup"><span data-stu-id="1ee89-389">A foreach statement of the form</span></span>
```csharp
foreach (V v in x) embedded_statement
```
<span data-ttu-id="1ee89-390">daha sonra şu şekilde genişletilir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-390">is then expanded to:</span></span>
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

<span data-ttu-id="1ee89-391">Değişken `e` , ifade `x` veya katıştırılmış deyim ya da programın başka bir kaynak kodu için görünür veya erişilebilir değil.</span><span class="sxs-lookup"><span data-stu-id="1ee89-391">The variable `e` is not visible to or accessible to the expression `x` or the embedded statement or any other source code of the program.</span></span> <span data-ttu-id="1ee89-392">Değişken `v` , gömülü ifadede salt okunurdur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-392">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="1ee89-393">( `T` *Öğe türü* )[](conversions.md#explicit-conversions) öğesinden(foreachdeyimindekilocal_variable_type)açıkbirdönüştürme(açıkdönüştürmeler)yoksa,birhataoluşturulurvebaşka`V` bir adım alınmaz.</span><span class="sxs-lookup"><span data-stu-id="1ee89-393">If there is not an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) from `T` (the element type) to `V` (the *local_variable_type* in the foreach statement), an error is produced and no further steps are taken.</span></span> <span data-ttu-id="1ee89-394">`System.NullReferenceException` Değeri `x` varsa`null`, çalışma zamanında bir oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-394">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

<span data-ttu-id="1ee89-395">Davranışın yukarıdaki genişlemeyle tutarlı olduğu sürece, bir uygulamanın belirli bir foreach-deyimin farklı şekilde uygulanmasına izin verilir; Örneğin performans nedenleriyle.</span><span class="sxs-lookup"><span data-stu-id="1ee89-395">An implementation is permitted to implement a given foreach-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="1ee89-396">While döngüsünün `v` içine yerleştirilmesi, *embedded_statement*içinde oluşan herhangi bir anonim işlev tarafından nasıl yakalandığından önemlidir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-396">The placement of `v` inside the while loop is important for how it is captured by any anonymous function occurring in the *embedded_statement*.</span></span>

<span data-ttu-id="1ee89-397">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1ee89-397">For example:</span></span>
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
<span data-ttu-id="1ee89-398">While döngüsünün dışında bildirilirse, bu, tüm yinelemeler arasında paylaşılır ve for döngüsünden sonraki değeri son `13`değer olacaktır, bu, ' nin `f` çağrılma değeridir. `v`</span><span class="sxs-lookup"><span data-stu-id="1ee89-398">If `v` was declared outside of the while loop, it would be shared among all iterations, and its value after the for loop would be the final value, `13`, which is what the invocation of `f` would print.</span></span> <span data-ttu-id="1ee89-399">Bunun yerine, her yineleme kendi değişkenine `v`sahip olduğundan, ilk yinelemede tarafından `f` yakalanan tek şey değeri `7`tutmaya devam eder, bu, yazdırılacak şeydir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-399">Instead, because each iteration has its own variable `v`, the one captured by `f` in the first iteration will continue to hold the value `7`, which is what will be printed.</span></span> <span data-ttu-id="1ee89-400">(Note: while döngüsünün dışında C# bildirildiği `v` önceki sürümleri.)</span><span class="sxs-lookup"><span data-stu-id="1ee89-400">(Note: earlier versions of C# declared `v` outside of the while loop.)</span></span>

<span data-ttu-id="1ee89-401">Finally bloğunun gövdesi aşağıdaki adımlara göre oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="1ee89-401">The body of the finally block is constructed according to the following steps:</span></span>

*  <span data-ttu-id="1ee89-402">`System.IDisposable` Arabiriminden örtülü bir dönüşüm `E` varsa,</span><span class="sxs-lookup"><span data-stu-id="1ee89-402">If there is an implicit conversion from `E` to the `System.IDisposable` interface, then</span></span>
   *  <span data-ttu-id="1ee89-403">`E` Null yapılamayan bir değer türü ise finally yan tümcesi, ' nin anlamsal eşdeğerine genişletilir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-403">If `E` is a non-nullable value type then the finally clause is expanded to the semantic equivalent  of:</span></span>

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  <span data-ttu-id="1ee89-404">Aksi halde finally yan tümcesi anlam eşdeğerine genişletilir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-404">Otherwise the finally clause is expanded to the semantic equivalent of:</span></span>

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   <span data-ttu-id="1ee89-405">bir değer türü veya bir değer türüne örneklenmiş tür parametresi olması dışında, ' `System.IDisposable` ın `e` ' a dönüştürme işlemi, kutulamayı oluşmasına neden olmaz. `E`</span><span class="sxs-lookup"><span data-stu-id="1ee89-405">except that if `E` is a value type, or a type parameter instantiated to a value type, then the cast of `e` to `System.IDisposable` will not cause boxing to occur.</span></span>

*  <span data-ttu-id="1ee89-406">Aksi halde, `E` korumalı bir tür ise finally yan tümcesi boş bir bloğa genişletilir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-406">Otherwise, if `E` is a sealed type, the finally clause is expanded to an empty block:</span></span>

   ```csharp
   finally {
   }
   ```

*  <span data-ttu-id="1ee89-407">Aksi takdirde, finally yan tümcesi şu şekilde genişletilir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-407">Otherwise, the finally clause is expanded to:</span></span>

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   <span data-ttu-id="1ee89-408">Yerel değişken `d` , herhangi bir kullanıcı koduna görünmez veya erişemez.</span><span class="sxs-lookup"><span data-stu-id="1ee89-408">The local variable `d` is not visible to or accessible to any user code.</span></span> <span data-ttu-id="1ee89-409">Özellikle, kapsamı finally bloğunu içeren başka herhangi bir değişkenle çakışmaz.</span><span class="sxs-lookup"><span data-stu-id="1ee89-409">In particular, it does not conflict with any other variable whose scope includes the finally block.</span></span>

<span data-ttu-id="1ee89-410">Bir dizinin öğelerinin taşındığı `foreach` sıra aşağıdaki gibidir: Tek boyutlu diziler öğeleri için dizin ile `0` başlayıp dizinle biten `Length - 1`Dizin sıralaması artılarak çapraz işlenir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-410">The order in which `foreach` traverses the elements of an array, is as follows: For single-dimensional arrays elements are traversed in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="1ee89-411">Çok boyutlu diziler için öğelere, en sağdaki boyutun dizinlerinin önce artması, sonra bir sonraki sol boyutun ve bu şekilde sol tarafta olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-411">For multi-dimensional arrays, elements are traversed such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span>

<span data-ttu-id="1ee89-412">Aşağıdaki örnek, her bir değeri öğe sırasıyla iki boyutlu bir dizide yazdırır:</span><span class="sxs-lookup"><span data-stu-id="1ee89-412">The following example prints out each value in a two-dimensional array, in element order:</span></span>
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
<span data-ttu-id="1ee89-413">Oluşturulan çıkış aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-413">The output produced is as follows:</span></span>
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

<span data-ttu-id="1ee89-414">Örnekte</span><span class="sxs-lookup"><span data-stu-id="1ee89-414">In the example</span></span>
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
<span data-ttu-id="1ee89-415">türü `n` olarak `int` algılanır`numbers`, öğesinin öğesi türü.</span><span class="sxs-lookup"><span data-stu-id="1ee89-415">the type of `n` is inferred to be `int`, the element type of `numbers`.</span></span>

## <a name="jump-statements"></a><span data-ttu-id="1ee89-416">Atlama deyimleri</span><span class="sxs-lookup"><span data-stu-id="1ee89-416">Jump statements</span></span>

<span data-ttu-id="1ee89-417">Geçiş deyimleri koşulsuz olmayan aktarma denetimi.</span><span class="sxs-lookup"><span data-stu-id="1ee89-417">Jump statements unconditionally transfer control.</span></span>

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

<span data-ttu-id="1ee89-418">Bir atma bildirimine aktarma denetimi, atlamanın ***hedefi*** olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-418">The location to which a jump statement transfers control is called the ***target*** of the jump statement.</span></span>

<span data-ttu-id="1ee89-419">Bir blok içinde bir atdeyim oluştuğunda ve bu sıçrama ifadesinin hedefi o bloğun dışında olduğunda, atlamanın bloğundan ***Çıkış*** olarak söylenir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-419">When a jump statement occurs within a block, and the target of that jump statement is outside that block, the jump statement is said to ***exit*** the block.</span></span> <span data-ttu-id="1ee89-420">Bir atlamayla denetimi bir bloğun dışına aktarabileceği sürece denetim bir bloğa hiçbir şekilde aktarılmaz.</span><span class="sxs-lookup"><span data-stu-id="1ee89-420">While a jump statement may transfer control out of a block, it can never transfer control into a block.</span></span>

<span data-ttu-id="1ee89-421">Sıçrama deyimlerinin yürütülmesi, aradaki `try` deyimlerin varlığı tarafından karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-421">Execution of jump statements is complicated by the presence of intervening `try` statements.</span></span> <span data-ttu-id="1ee89-422">Bu tür `try` deyimler yokluğunda, bir sıçrama deyimi, denetimi bir atlamanın hedefine koşulsuz olarak aktarır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-422">In the absence of such `try` statements, a jump statement unconditionally transfers control from the jump statement to its target.</span></span> <span data-ttu-id="1ee89-423">Bu tür bir araya giren `try` deyimler varsa, yürütme daha karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-423">In the presence of such intervening `try` statements, execution is more complex.</span></span> <span data-ttu-id="1ee89-424">Atif deyim ilişkili `finally` bloklarla bir veya `try` daha fazla `finally` bloktan çıkıldığında denetim başlangıçta en içteki `try` deyimin bloğuna aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-424">If the jump statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="1ee89-425">Ve denetimi bir `finally` bloğun bitiş noktasına ulaştığında, Denetim sonraki kapsayan `try` deyimin `finally` bloğuna aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-425">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="1ee89-426">Bu işlem, tüm araya `finally` `try` eklenen deyimlerin blokları yürütülene kadar yinelenir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-426">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>

<span data-ttu-id="1ee89-427">Örnekte</span><span class="sxs-lookup"><span data-stu-id="1ee89-427">In the example</span></span>
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
<span data-ttu-id="1ee89-428">`finally` iki`try` deyimle ilişkili bloklar, denetim, sıçrama ifadesinin hedefine aktarılmadan önce yürütülür.</span><span class="sxs-lookup"><span data-stu-id="1ee89-428">the `finally` blocks associated with two `try` statements are executed before control is transferred to the target of the jump statement.</span></span>

<span data-ttu-id="1ee89-429">Oluşturulan çıkış aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-429">The output produced is as follows:</span></span>
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a><span data-ttu-id="1ee89-430">Break ekstresi</span><span class="sxs-lookup"><span data-stu-id="1ee89-430">The break statement</span></span>

<span data-ttu-id="1ee89-431">`switch` İfadeen`while`yakın kapsayan,, `do` ,,`foreach` veya ifadesiyle çıkar. `for` `break`</span><span class="sxs-lookup"><span data-stu-id="1ee89-431">The `break` statement exits the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
break_statement
    : 'break' ';'
    ;
```

<span data-ttu-id="1ee89-432">`break` Bir deyimin hedefi, en yakın kapsayan `do` `switch`, `while`, `for`,, veya `foreach` ifadesinin bitiş noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-432">The target of a `break` statement is the end point of the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="1ee89-433">Bir `break` ifade bir `switch`, `while` ,,`foreach` veya ifadesiyle çevrelenemez, derleme zamanı hatası oluşur. `do` `for`</span><span class="sxs-lookup"><span data-stu-id="1ee89-433">If a `break` statement is not enclosed by a `switch`, `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="1ee89-434">Birden çok `switch`, `while`, `do` `break` ,, veya deyimleri`foreach` birbirine yuvalandığı zaman, bir deyim yalnızca en içteki deyim için geçerlidir. `for`</span><span class="sxs-lookup"><span data-stu-id="1ee89-434">When multiple `switch`, `while`, `do`, `for`, or `foreach` statements are nested within each other, a `break` statement applies only to the innermost statement.</span></span> <span data-ttu-id="1ee89-435">Birden çok iç içe düzeylerdeki denetimi aktarmak için bir `goto` deyimin ([goto deyimidir](statements.md#the-goto-statement)) kullanılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-435">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="1ee89-436">Bir `break` ifade bir `finally` bloğundan ([TRY ifadesiyle](statements.md#the-try-statement)) çıkabilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-436">A `break` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="1ee89-437">Bir `break` blok`finally` içinde bir ifade gerçekleştiğinde `break` , deyimin hedefi aynı `finally` blok içinde olmalıdır; Aksi takdirde, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-437">When a `break` statement occurs within a `finally` block, the target of the `break` statement must be within the same `finally` block; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="1ee89-438">Bir `break` ifade aşağıdaki gibi yürütülür:</span><span class="sxs-lookup"><span data-stu-id="1ee89-438">A `break` statement is executed as follows:</span></span>

*  <span data-ttu-id="1ee89-439">`try` `try` İfadeilişkili`finally`bloklarla bir veya daha fazla bloktan çıkıldığında denetim başlangıçta en içteki deyimin bloğuna aktarılır. `finally` `break`</span><span class="sxs-lookup"><span data-stu-id="1ee89-439">If the `break` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="1ee89-440">Ve denetimi bir `finally` bloğun bitiş noktasına ulaştığında, Denetim sonraki kapsayan `try` deyimin `finally` bloğuna aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-440">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="1ee89-441">Bu işlem, tüm araya `finally` `try` eklenen deyimlerin blokları yürütülene kadar yinelenir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-441">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="1ee89-442">Denetim, `break` deyimin hedefine aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-442">Control is transferred to the target of the `break` statement.</span></span>

<span data-ttu-id="1ee89-443">Bir `break` ifade denetimi başka bir yerde bir yere aktarmadığı için, bir `break` deyimin bitiş noktasına hiçbir şekilde ulaşılamıyor.</span><span class="sxs-lookup"><span data-stu-id="1ee89-443">Because a `break` statement unconditionally transfers control elsewhere, the end point of a `break` statement is never reachable.</span></span>

### <a name="the-continue-statement"></a><span data-ttu-id="1ee89-444">Continue ekstresi</span><span class="sxs-lookup"><span data-stu-id="1ee89-444">The continue statement</span></span>

<span data-ttu-id="1ee89-445">`while` `do` `for`Bu ifade, en yakın kapsayan,,, veya `foreach` bildiriminin yeni bir yinelemesini başlatır. `continue`</span><span class="sxs-lookup"><span data-stu-id="1ee89-445">The `continue` statement starts a new iteration of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
continue_statement
    : 'continue' ';'
    ;
```

<span data-ttu-id="1ee89-446">Bir `continue` deyimin hedefi, en yakın kapsayan `while`, `do`, `for`, veya `foreach` bildiriminin gömülü ifadesinin bitiş noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-446">The target of a `continue` statement is the end point of the embedded statement of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="1ee89-447">Bir `continue` ifade `while`, `do` ,,`foreach` veya ifadesiyle çevrelenemez, derleme zamanı hatası oluşur. `for`</span><span class="sxs-lookup"><span data-stu-id="1ee89-447">If a `continue` statement is not enclosed by a `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="1ee89-448">Birden çok `while`, `do`, `for`, `continue` veya `foreach` deyimleri birbirine yuvalandığı zaman, bir deyim yalnızca en içteki deyim için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-448">When multiple `while`, `do`, `for`, or `foreach` statements are nested within each other, a `continue` statement applies only to the innermost statement.</span></span> <span data-ttu-id="1ee89-449">Birden çok iç içe düzeylerdeki denetimi aktarmak için bir `goto` deyimin ([goto deyimidir](statements.md#the-goto-statement)) kullanılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-449">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="1ee89-450">Bir `continue` ifade bir `finally` bloğundan ([TRY ifadesiyle](statements.md#the-try-statement)) çıkabilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-450">A `continue` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="1ee89-451">Bir `continue` blok`finally` içinde bir ifade gerçekleştiğinde `continue` , deyimin hedefi aynı `finally` blok içinde olmalıdır; Aksi takdirde derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-451">When a `continue` statement occurs within a `finally` block, the target of the `continue` statement must be within the same `finally` block; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="1ee89-452">Bir `continue` ifade aşağıdaki gibi yürütülür:</span><span class="sxs-lookup"><span data-stu-id="1ee89-452">A `continue` statement is executed as follows:</span></span>

*  <span data-ttu-id="1ee89-453">`try` `try` İfadeilişkili`finally`bloklarla bir veya daha fazla bloktan çıkıldığında denetim başlangıçta en içteki deyimin bloğuna aktarılır. `finally` `continue`</span><span class="sxs-lookup"><span data-stu-id="1ee89-453">If the `continue` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="1ee89-454">Ve denetimi bir `finally` bloğun bitiş noktasına ulaştığında, Denetim sonraki kapsayan `try` deyimin `finally` bloğuna aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-454">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="1ee89-455">Bu işlem, tüm araya `finally` `try` eklenen deyimlerin blokları yürütülene kadar yinelenir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-455">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="1ee89-456">Denetim, `continue` deyimin hedefine aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-456">Control is transferred to the target of the `continue` statement.</span></span>

<span data-ttu-id="1ee89-457">Bir `continue` ifade denetimi başka bir yerde bir yere aktarmadığı için, bir `continue` deyimin bitiş noktasına hiçbir şekilde ulaşılamıyor.</span><span class="sxs-lookup"><span data-stu-id="1ee89-457">Because a `continue` statement unconditionally transfers control elsewhere, the end point of a `continue` statement is never reachable.</span></span>

### <a name="the-goto-statement"></a><span data-ttu-id="1ee89-458">Goto ekstresi</span><span class="sxs-lookup"><span data-stu-id="1ee89-458">The goto statement</span></span>

<span data-ttu-id="1ee89-459">İfade `goto` , denetimi bir etiketi tarafından işaretlenen bir ifadeye aktarır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-459">The `goto` statement transfers control to a statement that is marked by a label.</span></span>

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

<span data-ttu-id="1ee89-460">Bir `goto` *Identifier* ifadesinin hedefi, verilen etikete sahip etiketli deyimdir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-460">The target of a `goto` *identifier* statement is the labeled statement with the given label.</span></span> <span data-ttu-id="1ee89-461">Verilen ada sahip bir etiket geçerli işlev üyesinde yoksa veya `goto` ifade etiketin kapsamı içinde değilse, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-461">If a label with the given name does not exist in the current function member, or if the `goto` statement is not within the scope of the label, a compile-time error occurs.</span></span> <span data-ttu-id="1ee89-462">Bu kural, iç içe geçmiş bir `goto` kapsamın denetimini dışarı aktarmak için bir deyimin kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-462">This rule permits the use of a `goto` statement to transfer control out of a nested scope, but not into a nested scope.</span></span> <span data-ttu-id="1ee89-463">Örnekte</span><span class="sxs-lookup"><span data-stu-id="1ee89-463">In the example</span></span>
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
<span data-ttu-id="1ee89-464">bir `goto` ifade, iç içe bir kapsamın denetimini aktarmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-464">a `goto` statement is used to transfer control out of a nested scope.</span></span>

<span data-ttu-id="1ee89-465">Bir `goto case` deyimin hedefi, belirtilen sabit değere sahip bir `case` etiketi içeren hemen kapsayan `switch` deyimindeki ([Switch deyimindeki](statements.md#the-switch-statement)) ifade listesidir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-465">The target of a `goto case` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `case` label with the given constant value.</span></span> <span data-ttu-id="1ee89-466">[](conversions.md#implicit-conversions) `switch` İfade bir `switch` deyimden çevrede yoksa, constant_expression örtük olarak dönüştürülebilir (örtük dönüştürmeler), en yakın kapsayan deyimin yöneten türüne veya `goto case` En yakın kapsayan `switch` ifade verilen sabit değeri olan `case` bir etiket içermiyor, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-466">If the `goto case` statement is not enclosed by a `switch` statement, if the *constant_expression* is not implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the nearest enclosing `switch` statement, or if the nearest enclosing `switch` statement does not contain a `case` label with the given constant value, a compile-time error occurs.</span></span>

<span data-ttu-id="1ee89-467">Bir `goto default` deyimin hedefi, `switch` bir`default` etiketi içeren hemen kapsayan deyimindeki ([Switch deyimindeki](statements.md#the-switch-statement)) ifade listesidir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-467">The target of a `goto default` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `default` label.</span></span> <span data-ttu-id="1ee89-468">İfade bir `switch` deyimle çevrede yoksa veya en yakın kapsayan `switch` ifade bir `default` etiket içermiyorsa, derleme zamanı hatası oluşur. `goto default`</span><span class="sxs-lookup"><span data-stu-id="1ee89-468">If the `goto default` statement is not enclosed by a `switch` statement, or if the nearest enclosing `switch` statement does not contain a `default` label, a compile-time error occurs.</span></span>

<span data-ttu-id="1ee89-469">Bir `goto` ifade bir `finally` bloğundan ([TRY ifadesiyle](statements.md#the-try-statement)) çıkabilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-469">A `goto` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="1ee89-470">Bir `goto` blok`finally` içinde bir ifade gerçekleştiğinde `goto` , deyimin hedefi aynı `finally` blok içinde olmalıdır, aksi takdirde bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-470">When a `goto` statement occurs within a `finally` block, the target of the `goto` statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="1ee89-471">Bir `goto` ifade aşağıdaki gibi yürütülür:</span><span class="sxs-lookup"><span data-stu-id="1ee89-471">A `goto` statement is executed as follows:</span></span>

*  <span data-ttu-id="1ee89-472">`try` `try` İfadeilişkili`finally`bloklarla bir veya daha fazla bloktan çıkıldığında denetim başlangıçta en içteki deyimin bloğuna aktarılır. `finally` `goto`</span><span class="sxs-lookup"><span data-stu-id="1ee89-472">If the `goto` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="1ee89-473">Ve denetimi bir `finally` bloğun bitiş noktasına ulaştığında, Denetim sonraki kapsayan `try` deyimin `finally` bloğuna aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-473">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="1ee89-474">Bu işlem, tüm araya `finally` `try` eklenen deyimlerin blokları yürütülene kadar yinelenir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-474">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="1ee89-475">Denetim, `goto` deyimin hedefine aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-475">Control is transferred to the target of the `goto` statement.</span></span>

<span data-ttu-id="1ee89-476">Bir `goto` ifade denetimi başka bir yerde bir yere aktarmadığı için, bir `goto` deyimin bitiş noktasına hiçbir şekilde ulaşılamıyor.</span><span class="sxs-lookup"><span data-stu-id="1ee89-476">Because a `goto` statement unconditionally transfers control elsewhere, the end point of a `goto` statement is never reachable.</span></span>

### <a name="the-return-statement"></a><span data-ttu-id="1ee89-477">Return ekstresi</span><span class="sxs-lookup"><span data-stu-id="1ee89-477">The return statement</span></span>

<span data-ttu-id="1ee89-478">İfadesi, denetimi `return` deyimin göründüğü işlevin geçerli çağıranına döndürür. `return`</span><span class="sxs-lookup"><span data-stu-id="1ee89-478">The `return` statement returns control to the current caller of the function in which the `return` statement appears.</span></span>

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

<span data-ttu-id="1ee89-479">İfadesi `return` olmayan bir deyim yalnızca bir değer hesaplamayan bir işlev üyesinde, diğer bir deyişle, sonuç türü olan bir Yöntem ([Yöntem gövdesi](classes.md#method-body)) `void`, `set` bir özelliğin veya dizin oluşturucunun erişimcisi, `add` ve `remove` bir olayın erişimcileri, örnek Oluşturucu, statik oluşturucu ya da yok edicisi.</span><span class="sxs-lookup"><span data-stu-id="1ee89-479">A `return` statement with no expression can be used only in a function member that does not compute a value, that is, a method with the result type ([Method body](classes.md#method-body)) `void`, the `set` accessor of a property or indexer, the `add` and `remove` accessors of an event, an instance constructor, a static constructor, or a destructor.</span></span>

<span data-ttu-id="1ee89-480">İfadesi `return` içeren bir deyim yalnızca bir değeri hesaplayan bir işlev üyesinde, diğer bir deyişle, void olmayan sonuç türü olan bir yöntem `get` , bir özellik veya dizin oluşturucunun erişimcisi veya Kullanıcı tanımlı bir operatör ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-480">A `return` statement with an expression can only be used in a function member that computes a value, that is, a method with a non-void result type, the `get` accessor of a property or indexer, or a user-defined operator.</span></span> <span data-ttu-id="1ee89-481">Örtük dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)), ifadenin türünden kapsayan işlev üyesinin dönüş türüne sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-481">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression to the return type of the containing function member.</span></span>

<span data-ttu-id="1ee89-482">Return deyimleri, anonim işlev ifadelerinin gövdesinde de kullanılabilir ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) ve bu işlevler için hangi dönüştürmelerin var olduğunu belirlemeye katılabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1ee89-482">Return statements can also be used in the body of anonymous function expressions ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), and participate in determining which conversions exist for those functions.</span></span>

<span data-ttu-id="1ee89-483">Bir `return` deyimin `finally` bloğunda ([TRY ifadesinde](statements.md#the-try-statement)) görünmesi için derleme zamanı hatası.</span><span class="sxs-lookup"><span data-stu-id="1ee89-483">It is a compile-time error for a `return` statement to appear in a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="1ee89-484">Bir `return` ifade aşağıdaki gibi yürütülür:</span><span class="sxs-lookup"><span data-stu-id="1ee89-484">A `return` statement is executed as follows:</span></span>

*  <span data-ttu-id="1ee89-485">`return` Deyim bir ifade belirtiyorsa, ifade değerlendirilir ve elde edilen değer, örtük bir dönüştürme tarafından içerilen işlevin dönüş türüne dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="1ee89-485">If the `return` statement specifies an expression, the expression is evaluated and the resulting value is converted to the return type of the containing function by an implicit conversion.</span></span> <span data-ttu-id="1ee89-486">Dönüştürmenin sonucu, işlev tarafından üretilen sonuç değeri haline gelir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-486">The result of the conversion becomes the result value produced by the function.</span></span>
*  <span data-ttu-id="1ee89-487">`try` `catch` `try` `finally` Deyimle ilişkilendirilmiş`finally` blokları olan bir veya daha fazla ya da blok varsa, denetim başlangıçta en içteki deyimin bloğuna aktarılır. `return`</span><span class="sxs-lookup"><span data-stu-id="1ee89-487">If the `return` statement is enclosed by one or more `try` or `catch` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="1ee89-488">Ve denetimi bir `finally` bloğun bitiş noktasına ulaştığında, Denetim sonraki kapsayan `try` deyimin `finally` bloğuna aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-488">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="1ee89-489">Bu işlem, kapsayan `finally` `try` tüm deyimlerin blokları yürütülene kadar yinelenir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-489">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="1ee89-490">İçeren işlev bir zaman uyumsuz işlev değilse, denetim, içerilen işlevin çağıranına, varsa sonuç değeriyle birlikte döndürülür.</span><span class="sxs-lookup"><span data-stu-id="1ee89-490">If the containing function is not an async function, control is returned to the caller of the containing function along with the result value, if any.</span></span>
*  <span data-ttu-id="1ee89-491">İçeren işlev bir zaman uyumsuz işlevtiyse, Denetim geçerli çağırana döndürülür ve sonuç değeri varsa, ([Numaralandırıcı arabirimleri](classes.md#enumerator-interfaces)) bölümünde açıklandığı gibi Return görevinde kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-491">If the containing function is an async function, control is returned to the current caller, and the result value, if any, is recorded in the return task as described in ([Enumerator interfaces](classes.md#enumerator-interfaces)).</span></span>

<span data-ttu-id="1ee89-492">Bir `return` ifade denetimi başka bir yerde bir yere aktarmadığı için, bir `return` deyimin bitiş noktasına hiçbir şekilde ulaşılamıyor.</span><span class="sxs-lookup"><span data-stu-id="1ee89-492">Because a `return` statement unconditionally transfers control elsewhere, the end point of a `return` statement is never reachable.</span></span>

### <a name="the-throw-statement"></a><span data-ttu-id="1ee89-493">Throw deyimleri</span><span class="sxs-lookup"><span data-stu-id="1ee89-493">The throw statement</span></span>

<span data-ttu-id="1ee89-494">`throw` İfade bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-494">The `throw` statement throws an exception.</span></span>

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

<span data-ttu-id="1ee89-495">İfadesi `throw` içeren bir deyim, ifade hesaplanarak üretilen değeri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-495">A `throw` statement with an expression throws the value produced by evaluating the expression.</span></span> <span data-ttu-id="1ee89-496">İfade, etkin taban sınıfı olarak `System.Exception` `System.Exception` (veya alt sınıfı) olan bir tür parametre türünden veya ondan `System.Exception` türetilen bir sınıf türünün sınıf türünün bir değerini belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-496">The expression must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="1ee89-497">İfadenin `null`değerlendirmesi oluşursa bunun yerine bir `System.NullReferenceException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-497">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="1ee89-498">İfadesi `throw` olmayan bir deyim yalnızca bir `catch` blokta kullanılabilir, bu durumda deyim o `catch` blok tarafından işlenmekte olan özel durumu yeniden atar.</span><span class="sxs-lookup"><span data-stu-id="1ee89-498">A `throw` statement with no expression can be used only in a `catch` block, in which case that statement re-throws the exception that is currently being handled by that `catch` block.</span></span>

<span data-ttu-id="1ee89-499">Bir `throw` ifade denetimi başka bir yerde bir yere aktarmadığı için, bir `throw` deyimin bitiş noktasına hiçbir şekilde ulaşılamıyor.</span><span class="sxs-lookup"><span data-stu-id="1ee89-499">Because a `throw` statement unconditionally transfers control elsewhere, the end point of a `throw` statement is never reachable.</span></span>

<span data-ttu-id="1ee89-500">Bir özel durum oluştuğunda denetim, özel durumu işleyebilen bir kapsayan `catch` `try` ifadede ilk yan tümcesine aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-500">When an exception is thrown, control is transferred to the first `catch` clause in an enclosing `try` statement that can handle the exception.</span></span> <span data-ttu-id="1ee89-501">Oluşturulan özel durumun noktasından, uygun bir özel durum işleyicisine denetim aktarma noktasına gerçekleşen işlem, ***özel durum yayma***olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-501">The process that takes place from the point of the exception being thrown to the point of transferring control to a suitable exception handler is known as ***exception propagation***.</span></span> <span data-ttu-id="1ee89-502">Bir özel durumun yayılması, özel durumla eşleşen bir `catch` yan tümce bulunana kadar aşağıdaki adımları tekrar tekrar değerlendirmeden oluşur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-502">Propagation of an exception consists of repeatedly evaluating the following steps until a `catch` clause that matches the exception is found.</span></span> <span data-ttu-id="1ee89-503">Bu açıklamada, ***throw noktası*** başlangıçta özel durumun oluşturulduğu konumdur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-503">In this description, the ***throw point*** is initially the location at which the exception is thrown.</span></span>

*  <span data-ttu-id="1ee89-504">Geçerli işlev üyesinde, throw noktasını kapsayan `try` her bir ifade incelenir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-504">In the current function member, each `try` statement that encloses the throw point is examined.</span></span> <span data-ttu-id="1ee89-505">Her bir bildirimde `S`, en içteki `try` bildirimiyle başlayıp en dıştaki `try` ifadesiyle sona ermek üzere aşağıdaki adımlar değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-505">For each statement `S`, starting with the innermost `try` statement and ending with the outermost `try` statement, the following steps are evaluated:</span></span>

   * <span data-ttu-id="1ee89-506">Bloğu bir `catch`veyadaha fazla yan tümce içeriyorsa ve bu blok birveyadahafazlayantümceiçeriyorsa,yantümceleri,'debelirtilenkurallaragöreözeldurumiçinuygunbirişleyicininyerinibulacakşekildeincelenir.`catch` `S` `try` [TRY ifadesinin](statements.md#the-try-statement)bölümü.</span><span class="sxs-lookup"><span data-stu-id="1ee89-506">If the `try` block of `S` encloses the throw point and if S has one or more `catch` clauses, the `catch` clauses are examined in order of appearance to locate a suitable handler for the exception, according to the rules specified in Section [The try statement](statements.md#the-try-statement).</span></span> <span data-ttu-id="1ee89-507">Eşleşen `catch` bir yan tümce varsa, özel durum yayma, denetim bu `catch` yan tümce bloğuna aktarılarak tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-507">If a matching `catch` clause is located, the exception propagation is completed by transferring control to the block of that `catch` clause.</span></span>

   * <span data-ttu-id="1ee89-508">Aksi takdirde `try` , blok veya bir `catch` bloğu `S` throw noktasını barındırır ve eğer bir `finally` blok içeriyorsa `S` denetim `finally` bloğa aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-508">Otherwise, if the `try` block or a `catch` block of `S` encloses the throw point and if `S` has a `finally` block, control is transferred to the `finally` block.</span></span> <span data-ttu-id="1ee89-509">`finally` Blok başka bir özel durum oluşturursa, geçerli özel durumun işlenmesi sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-509">If the `finally` block throws another exception, processing of the current exception is terminated.</span></span> <span data-ttu-id="1ee89-510">Aksi takdirde, Denetim `finally` bloğun bitiş noktasına ulaştığında, geçerli özel durumun işlenmesi devam eder.</span><span class="sxs-lookup"><span data-stu-id="1ee89-510">Otherwise, when control reaches the end point of the `finally` block, processing of the current exception is continued.</span></span>

*  <span data-ttu-id="1ee89-511">Geçerli işlev çağrısında bir özel durum işleyicisi bulunmuyorsa, işlev çağırma sonlandırılır ve aşağıdakilerden biri oluşur:</span><span class="sxs-lookup"><span data-stu-id="1ee89-511">If an exception handler was not located in the current function invocation, the function invocation is terminated, and one of the following occurs:</span></span>

   * <span data-ttu-id="1ee89-512">Geçerli işlev zaman uyumsuz ise, yukarıdaki adımlar işlev üyesinin çağrıldığı ifadeye karşılık gelen bir throw noktasıyla işlevin çağıranı için yinelenir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-512">If the current function is non-async, the steps above are repeated for the caller of the function with a throw point corresponding to the statement from which the function member was invoked.</span></span>

   * <span data-ttu-id="1ee89-513">Geçerli işlev zaman uyumsuz ve görev döndürüyor ise, özel durum, [Numaralandırıcı arabirimlerinde](classes.md#enumerator-interfaces)açıklandığı şekilde hatalı veya iptal edilmiş duruma yerleştirilen geri dönüş görevine kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-513">If the current function is async and task-returning, the exception is recorded in the return task, which is put into a faulted or cancelled state as described in [Enumerator interfaces](classes.md#enumerator-interfaces).</span></span>

   * <span data-ttu-id="1ee89-514">Geçerli işlev zaman uyumsuz ve void döndürüyorsa, geçerli iş parçacığının eşitleme bağlamı, [sıralanabilir arabirimlerde](classes.md#enumerable-interfaces)açıklandığı gibi bilgilendirilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-514">If the current function is async and void-returning, the synchronization context of the current thread is notified as described in [Enumerable interfaces](classes.md#enumerable-interfaces).</span></span>

*  <span data-ttu-id="1ee89-515">Özel durum işleme geçerli iş parçacığında tüm işlev üyesi çağırmaları sonlandırdığında, iş parçacığının özel durum için bir işleyici olmadığını belirten bir iş parçacığının kendisi sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-515">If the exception processing terminates all function member invocations in the current thread, indicating that the thread has no handler for the exception, then the thread is itself terminated.</span></span> <span data-ttu-id="1ee89-516">Bu sonlandırmanın etkisi, uygulama tanımlı ' dır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-516">The impact of such termination is implementation-defined.</span></span>

## <a name="the-try-statement"></a><span data-ttu-id="1ee89-517">TRY deyimleri</span><span class="sxs-lookup"><span data-stu-id="1ee89-517">The try statement</span></span>

<span data-ttu-id="1ee89-518">İfade `try` , bir bloğun yürütülmesi sırasında oluşan özel durumları yakalamak için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="1ee89-518">The `try` statement provides a mechanism for catching exceptions that occur during execution of a block.</span></span> <span data-ttu-id="1ee89-519">Ayrıca,, `try` denetimi `try` deyimden çıktığında her zaman yürütülen bir kod bloğunu belirtme özelliği de sağlar.</span><span class="sxs-lookup"><span data-stu-id="1ee89-519">Furthermore, the `try` statement provides the ability to specify a block of code that is always executed when control leaves the `try` statement.</span></span>

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

<span data-ttu-id="1ee89-520">Üç olası `try` deyim biçimi vardır:</span><span class="sxs-lookup"><span data-stu-id="1ee89-520">There are three possible forms of `try` statements:</span></span>

*  <span data-ttu-id="1ee89-521">Bir `try` blok ve ardından bir veya daha `catch` fazla blok gelir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-521">A `try` block followed by one or more `catch` blocks.</span></span>
*  <span data-ttu-id="1ee89-522">Bir `try` blok ve ardından bir `finally` blok gelir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-522">A `try` block followed by a `finally` block.</span></span>
*  <span data-ttu-id="1ee89-523">Bir blok ve ardından bir blok tarafından `catch` `finally` izlenen bir veya daha fazla blok. `try`</span><span class="sxs-lookup"><span data-stu-id="1ee89-523">A `try` block followed by one or more `catch` blocks followed by a `finally` block.</span></span>

<span data-ttu-id="1ee89-524">Bir `catch` yan tümce bir *exception_specifier*belirttiğinde, türü `System.Exception`, geçerli temel sınıfı olarak `System.Exception` (veya bir alt `System.Exception` sınıfı) içeren tür parametre türünden veya türetilen bir tür olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-524">When a `catch` clause specifies an *exception_specifier*, the type must be `System.Exception`, a type that derives from `System.Exception` or a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span>

<span data-ttu-id="1ee89-525">Bir `catch` yan tümce her ikisi de *tanımlayıcı*içeren bir *exception_specifier* belirttiğinde, verilen ad ve tür için bir ***özel durum değişkeni*** bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-525">When a `catch` clause specifies both an *exception_specifier* with an *identifier*, an ***exception variable*** of the given name and type is declared.</span></span> <span data-ttu-id="1ee89-526">Özel durum değişkeni, `catch` yan tümcesini genişleten bir kapsama sahip yerel bir değişkene karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-526">The exception variable corresponds to a local variable with a scope that extends over the `catch` clause.</span></span> <span data-ttu-id="1ee89-527">*Exception_filter* ve *bloğunun*yürütülmesi sırasında, özel durum değişkeni işlenmekte olan özel durumu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="1ee89-527">During execution of the *exception_filter* and *block*, the exception variable represents the exception currently being handled.</span></span> <span data-ttu-id="1ee89-528">Kesin atama denetimi amacıyla, özel durum değişkeni tüm kapsamda kesin olarak atanır olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-528">For purposes of definite assignment checking, the exception variable is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="1ee89-529">Bir `catch` yan tümce bir özel durum değişkeni adı içermiyorsa, filtre ve `catch` bloktaki özel durum nesnesine erişmek olanaksız olur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-529">Unless a `catch` clause includes an exception variable name, it is impossible to access the exception object in the filter and `catch` block.</span></span>

<span data-ttu-id="1ee89-530">Bir `catch` *exception_specifier* belirtmeyen bir yan tümce genel `catch` yan tümce olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-530">A `catch` clause that does not specify an *exception_specifier* is called a general `catch` clause.</span></span>

<span data-ttu-id="1ee89-531">Bazı programlama dilleri, ' den `System.Exception`türetilmiş bir nesne olarak gösterilemeyen özel durumları destekleyebilir, ancak bu tür özel durumlar hiçbir şekilde kod tarafından C# üretilmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-531">Some programming languages may support exceptions that are not representable as an object derived from `System.Exception`, although such exceptions could never be generated by C# code.</span></span> <span data-ttu-id="1ee89-532">Bu tür `catch` özel durumları yakalamak için genel bir yan tümce kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-532">A general `catch` clause may be used to catch such exceptions.</span></span> <span data-ttu-id="1ee89-533">Bu nedenle, genel `catch` bir yan tümce türü `System.Exception`belirten bir değer olan anlam, diğer bir deyişle diğer dillerdeki özel durumları da yakalayabiliriz.</span><span class="sxs-lookup"><span data-stu-id="1ee89-533">Thus, a general `catch` clause is semantically different from one that specifies the type `System.Exception`, in that the former may also catch exceptions from other languages.</span></span>

<span data-ttu-id="1ee89-534">Bir özel durum için işleyicinin yerini bulmak için, `catch` yan tümceler sözlü sırada incelenir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-534">In order to locate a handler for an exception, `catch` clauses are examined in lexical order.</span></span> <span data-ttu-id="1ee89-535">Bir `catch` yan tümce bir tür belirtiyorsa, ancak özel durum filtresi yoksa, bu tür ile aynı veya ondan türetilmiş bir `catch` türü belirtmek için aynı `try` deyimdeki sonraki yan tümce için derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-535">If a `catch` clause specifies a type but no exception filter, it is a compile-time error for a later `catch` clause in the same `try` statement to specify a type that is the same as, or is derived from, that type.</span></span> <span data-ttu-id="1ee89-536">Bir `catch` yan tümce hiçbir tür ve filtre yoksa, bu `try` deyimin son `catch` yan tümcesi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-536">If a `catch` clause specifies no type and no filter, it must be the last `catch` clause for that `try` statement.</span></span>

<span data-ttu-id="1ee89-537">Bir `catch` blok içinde, ifadesi `throw` olmayan bir deyim ([throw deyimi](statements.md#the-throw-statement)), `catch` blok tarafından yakalanan özel durumu yeniden oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-537">Within a `catch` block, a `throw` statement ([The throw statement](statements.md#the-throw-statement)) with no expression can be used to re-throw the exception that was caught by the `catch` block.</span></span> <span data-ttu-id="1ee89-538">Bir özel durum değişkenine atamalar yeniden oluşturulan özel durumu değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="1ee89-538">Assignments to an exception variable do not alter the exception that is re-thrown.</span></span>

<span data-ttu-id="1ee89-539">Örnekte</span><span class="sxs-lookup"><span data-stu-id="1ee89-539">In the example</span></span>
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
<span data-ttu-id="1ee89-540">yöntemi `F` bir özel durumu yakalar, konsola bazı tanılama bilgileri yazar, özel durum değişkenini değiştirir ve özel durumu yeniden oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-540">the method `F` catches an exception, writes some diagnostic information to the console, alters the exception variable, and re-throws the exception.</span></span> <span data-ttu-id="1ee89-541">Yeniden oluşturulan özel durum özgün özel durumdur, bu nedenle oluşturulan çıkış şu şekilde yapılır:</span><span class="sxs-lookup"><span data-stu-id="1ee89-541">The exception that is re-thrown is the original exception, so the output produced is:</span></span>
```
Exception in F: G
Exception in Main: G
```

<span data-ttu-id="1ee89-542">İlk catch bloğu `e` geçerli özel durumu yeniden oluşturmak yerine oluşursa, üretilen çıkış aşağıdaki gibi olacaktır:</span><span class="sxs-lookup"><span data-stu-id="1ee89-542">If the first catch block had thrown `e` instead of rethrowing the current exception, the output produced would be as follows:</span></span>
```
Exception in F: G
Exception in Main: F
```

<span data-ttu-id="1ee89-543">Bir `break` blokdışına`finally` denetim aktarmak için, `continue`veya `goto` bildiriminde derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-543">It is a compile-time error for a `break`, `continue`, or `goto` statement to transfer control out of a `finally` block.</span></span> <span data-ttu-id="1ee89-544">`break`Bir `continue` `finally` blok içinde bir, `goto` veya ifade oluştuğunda, deyimin hedefi aynı blok içinde olmalıdır, aksi takdirde bir derleme zamanı hatası oluşur. `finally`</span><span class="sxs-lookup"><span data-stu-id="1ee89-544">When a `break`, `continue`, or `goto` statement occurs in a `finally` block, the target of the statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="1ee89-545">Bir `return` deyimin bir `finally` blokta oluşması için derleme zamanı hatası.</span><span class="sxs-lookup"><span data-stu-id="1ee89-545">It is a compile-time error for a `return` statement to occur in a `finally` block.</span></span>

<span data-ttu-id="1ee89-546">Bir `try` ifade aşağıdaki gibi yürütülür:</span><span class="sxs-lookup"><span data-stu-id="1ee89-546">A `try` statement is executed as follows:</span></span>

*  <span data-ttu-id="1ee89-547">Denetim, `try` bloğa aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-547">Control is transferred to the `try` block.</span></span>
*  <span data-ttu-id="1ee89-548">Denetim, `try` bloğunun bitiş noktasına ulaştığında:</span><span class="sxs-lookup"><span data-stu-id="1ee89-548">When and if control reaches the end point of the `try` block:</span></span>
   *  <span data-ttu-id="1ee89-549">Deyimde bir `finally` blok varsa, `finally` blok yürütülür. `try`</span><span class="sxs-lookup"><span data-stu-id="1ee89-549">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
   *  <span data-ttu-id="1ee89-550">Denetim, `try` deyimin bitiş noktasına aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-550">Control is transferred to the end point of the `try` statement.</span></span>

*  <span data-ttu-id="1ee89-551">Bloğun`try` yürütülmesi sırasında `try` ifadeye bir özel durum yayıldığında:</span><span class="sxs-lookup"><span data-stu-id="1ee89-551">If an exception is propagated to the `try` statement during execution of the `try` block:</span></span>
   *  <span data-ttu-id="1ee89-552">`catch` Yan tümceleri, varsa, özel durum için uygun bir işleyicinin yerini bulmak üzere görünüm sırasına göre incelenir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-552">The `catch` clauses, if any, are examined in order of appearance to locate a suitable handler for the exception.</span></span> <span data-ttu-id="1ee89-553">Bir `catch` yan tümce bir tür belirtmezse veya özel durum türünü ya da özel durum türünün temel türünü belirtir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-553">If a `catch` clause does not specify a type, or specifies the exception type or a base type of the exception type:</span></span>
      *  <span data-ttu-id="1ee89-554">`catch` Yan tümce bir özel durum değişkeni bildiriyorsa, özel durum nesnesi özel durum değişkenine atanır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-554">If the `catch` clause declares an exception variable, the exception object is assigned to the exception variable.</span></span>
      *  <span data-ttu-id="1ee89-555">`catch` Yan tümce bir özel durum filtresi bildirirse, filtre değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-555">If the `catch` clause declares an exception filter, the filter is evaluated.</span></span> <span data-ttu-id="1ee89-556">Olarak `false`değerlendirilirse, catch yan tümcesi bir eşleşme değildir ve arama uygun bir işleyici için sonraki `catch` yan tümcelerde devam eder.</span><span class="sxs-lookup"><span data-stu-id="1ee89-556">If it evaluates to `false`, the catch clause is not a match, and the search continues through any subsequent `catch` clauses for a suitable handler.</span></span>
      *  <span data-ttu-id="1ee89-557">Aksi takdirde, `catch` yan tümce eşleşme olarak değerlendirilir ve denetim eşleşen `catch` bloğa aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-557">Otherwise, the `catch` clause is considered a match, and control is transferred to the matching `catch` block.</span></span>
      *  <span data-ttu-id="1ee89-558">Denetim, `catch` bloğunun bitiş noktasına ulaştığında:</span><span class="sxs-lookup"><span data-stu-id="1ee89-558">When and if control reaches the end point of the `catch` block:</span></span>
         * <span data-ttu-id="1ee89-559">Deyimde bir `finally` blok varsa, `finally` blok yürütülür. `try`</span><span class="sxs-lookup"><span data-stu-id="1ee89-559">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         * <span data-ttu-id="1ee89-560">Denetim, `try` deyimin bitiş noktasına aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-560">Control is transferred to the end point of the `try` statement.</span></span>
      *  <span data-ttu-id="1ee89-561">Bloğun`catch` yürütülmesi sırasında `try` ifadeye bir özel durum yayıldığında:</span><span class="sxs-lookup"><span data-stu-id="1ee89-561">If an exception is propagated to the `try` statement during execution of the `catch` block:</span></span>
         *  <span data-ttu-id="1ee89-562">Deyimde bir `finally` blok varsa, `finally` blok yürütülür. `try`</span><span class="sxs-lookup"><span data-stu-id="1ee89-562">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         *  <span data-ttu-id="1ee89-563">Özel durum bir sonraki kapsayan `try` ifadeye yayılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-563">The exception is propagated to the next enclosing `try` statement.</span></span>
   *  <span data-ttu-id="1ee89-564">Deyimin hiçbir yan tümcesi yoksa `catch` veya hiçbir `catch` yan tümce özel durumla eşleşmez: `try`</span><span class="sxs-lookup"><span data-stu-id="1ee89-564">If the `try` statement has no `catch` clauses or if no `catch` clause matches the exception:</span></span>
      *  <span data-ttu-id="1ee89-565">Deyimde bir `finally` blok varsa, `finally` blok yürütülür. `try`</span><span class="sxs-lookup"><span data-stu-id="1ee89-565">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
      *  <span data-ttu-id="1ee89-566">Özel durum bir sonraki kapsayan `try` ifadeye yayılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-566">The exception is propagated to the next enclosing `try` statement.</span></span>

<span data-ttu-id="1ee89-567">Bir `finally` bloğun deyimleri her zaman denetim bir `try` ifadeden ayrıldığında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="1ee89-567">The statements of a `finally` block are always executed when control leaves a `try` statement.</span></span> <span data-ttu-id="1ee89-568">Bu, denetim aktarımının normal yürütmenin sonucu olarak,,, `break`veya `return` ifadesinin yürütülmesi `continue` `goto` `try` veya bir özel durumun bir özel durum yaymasından kaynaklanan bir sonuç olarak oluşup oluşmadığını belirtir. Ekstre.</span><span class="sxs-lookup"><span data-stu-id="1ee89-568">This is true whether the control transfer occurs as a result of normal execution, as a result of executing a `break`, `continue`, `goto`, or `return` statement, or as a result of propagating an exception out of the `try` statement.</span></span>

<span data-ttu-id="1ee89-569">Bir `finally` bloğun yürütülmesi sırasında bir özel durum oluşturulursa ve aynı finally bloğu içinde yakalanmadığında, özel durum sonraki kapsayan `try` ifadeye yayılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-569">If an exception is thrown during execution of a `finally` block, and is not caught within the same finally block, the exception is propagated to the next enclosing `try` statement.</span></span> <span data-ttu-id="1ee89-570">Yayılmakta olan işlemde başka bir özel durum varsa, bu özel durum kaybedilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-570">If another exception was in the process of being propagated, that exception is lost.</span></span> <span data-ttu-id="1ee89-571">Bir özel durumu yayma işlemi, `throw` deyimin açıklamasında ([throw deyimidir](statements.md#the-throw-statement)) daha ayrıntılı bir şekilde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-571">The process of propagating an exception is discussed further in the description of the `throw` statement ([The throw statement](statements.md#the-throw-statement)).</span></span>

<span data-ttu-id="1ee89-572">Deyimin erişilebilir olması durumunda `try` bir `try` deyimin `try` bloğuna erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-572">The `try` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="1ee89-573">Deyimde erişilebilir olduğunda `try` bir `catch` deyimin bloğu `try` erişilebilir olur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-573">A `catch` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="1ee89-574">Deyimin erişilebilir olması durumunda `try` bir `finally` deyimin `try` bloğuna erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-574">The `finally` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="1ee89-575">Aşağıdakilerin her ikisi de doğruysa `try` , bir deyimin bitiş noktasına ulaşılabilir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-575">The end point of a `try` statement is reachable if both of the following are true:</span></span>

*  <span data-ttu-id="1ee89-576">`try` Bloğun bitiş noktasına ulaşılabilir veya en az bir `catch` bloğun bitiş noktası erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-576">The end point of the `try` block is reachable or the end point of at least one `catch` block is reachable.</span></span>
*  <span data-ttu-id="1ee89-577">Bir `finally` blok varsa, `finally` bloğun bitiş noktasına ulaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-577">If a `finally` block is present, the end point of the `finally` block is reachable.</span></span>

## <a name="the-checked-and-unchecked-statements"></a><span data-ttu-id="1ee89-578">Checked ve unchecked deyimleri</span><span class="sxs-lookup"><span data-stu-id="1ee89-578">The checked and unchecked statements</span></span>

<span data-ttu-id="1ee89-579">`checked` Ve`unchecked` deyimleri, tamsayı türü aritmetik işlemler ve dönüştürmeler için ***taşma denetimi bağlamını*** denetlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-579">The `checked` and `unchecked` statements are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

<span data-ttu-id="1ee89-580">Deyimi, *bloktaki* tüm ifadelerin işaretlenmiş bir `unchecked` bağlamda değerlendirilmesini sağlar ve deyim, bloktaki tüm ifadelerin işaretlenmemiş bir bağlamda değerlendirilmesini sağlar. `checked`</span><span class="sxs-lookup"><span data-stu-id="1ee89-580">The `checked` statement causes all expressions in the *block* to be evaluated in a checked context, and the `unchecked` statement causes all expressions in the *block* to be evaluated in an unchecked context.</span></span>

<span data-ttu-id="1ee89-581">`checked` `unchecked` Ve deyimleri, ve işleçleri ([Checked ve unchecked işleçleri](expressions.md#the-checked-and-unchecked-operators)), ifadeler yerine bloklar üzerinde çalıştıkları durumlar hariç, ve işleçlerine tam olarak eşdeğerdir. `unchecked` `checked`</span><span class="sxs-lookup"><span data-stu-id="1ee89-581">The `checked` and `unchecked` statements are precisely equivalent to the `checked` and `unchecked` operators ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), except that they operate on blocks instead of expressions.</span></span>

## <a name="the-lock-statement"></a><span data-ttu-id="1ee89-582">Lock deyimleri</span><span class="sxs-lookup"><span data-stu-id="1ee89-582">The lock statement</span></span>

<span data-ttu-id="1ee89-583">`lock` İfade, belirli bir nesne için karşılıklı dışlama kilidini edinir, bir ifade yürütür ve sonra kilidi serbest bırakır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-583">The `lock` statement obtains the mutual-exclusion lock for a given object, executes a statement, and then releases the lock.</span></span>

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

<span data-ttu-id="1ee89-584">Bir `lock` deyimin ifadesi, *reference_type*olarak bilinen bir türün değerini belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-584">The expression of a `lock` statement must denote a value of a type known to be a *reference_type*.</span></span> <span data-ttu-id="1ee89-585">[](conversions.md#boxing-conversions) Bir`lock` deyimin ifadesi için hiçbir örtük paketleme dönüştürmesi (kutulama dönüştürmesi) yapılmaz ve bu nedenle, ifadenin bir *value_type*değerini belirtmek için derleme zamanı hatası vardır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-585">No implicit boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)) is ever performed for the expression of a `lock` statement, and thus it is a compile-time error for the expression to denote a value of a *value_type*.</span></span>

<span data-ttu-id="1ee89-586">Formun `lock` bir açıklaması</span><span class="sxs-lookup"><span data-stu-id="1ee89-586">A `lock` statement of the form</span></span>
```csharp
lock (x) ...
```
<span data-ttu-id="1ee89-587">, reference_type 'in bir ifadesi olduğu için tam olarak eşdeğerdir `x`</span><span class="sxs-lookup"><span data-stu-id="1ee89-587">where `x` is an expression of a *reference_type*, is precisely equivalent to</span></span>
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
<span data-ttu-id="1ee89-588">`x` hariç yalnızca bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-588">except that `x` is only evaluated once.</span></span>

<span data-ttu-id="1ee89-589">Karşılıklı dışlama kilidi tutulurken, aynı yürütme iş parçacığında yürütülen kod da kilidi alabilir ve serbest bırakabilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-589">While a mutual-exclusion lock is held, code executing in the same execution thread can also obtain and release the lock.</span></span> <span data-ttu-id="1ee89-590">Ancak, diğer iş parçacıklarında yürütülen kodun kilit serbest bırakılana kadar kilidi almasını engellenir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-590">However, code executing in other threads is blocked from obtaining the lock until the lock is released.</span></span>

<span data-ttu-id="1ee89-591">Statik `System.Type` verilere erişimin eşitlenmesi için nesneleri kilitleme önerilmez.</span><span class="sxs-lookup"><span data-stu-id="1ee89-591">Locking `System.Type` objects in order to synchronize access to static data is not recommended.</span></span> <span data-ttu-id="1ee89-592">Diğer kod aynı tür üzerinde kilitlenebilir, bu da kilitlenmeye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-592">Other code might lock on the same type, which can result in deadlock.</span></span> <span data-ttu-id="1ee89-593">Özel bir statik nesneyi kilitleyerek statik verilere erişimi eşitlememeye daha iyi bir yaklaşım.</span><span class="sxs-lookup"><span data-stu-id="1ee89-593">A better approach is to synchronize access to static data by locking a private static object.</span></span> <span data-ttu-id="1ee89-594">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1ee89-594">For example:</span></span>
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

## <a name="the-using-statement"></a><span data-ttu-id="1ee89-595">Using deyimi</span><span class="sxs-lookup"><span data-stu-id="1ee89-595">The using statement</span></span>

<span data-ttu-id="1ee89-596">`using` İfade bir veya daha fazla kaynak edinir, bir ifade yürütür ve sonra kaynağı atar.</span><span class="sxs-lookup"><span data-stu-id="1ee89-596">The `using` statement obtains one or more resources, executes a statement, and then disposes of the resource.</span></span>

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

<span data-ttu-id="1ee89-597">***Kaynak*** , adlı `System.IDisposable` `Dispose`tek bir parametresiz yöntemi içeren, uygulayan bir sınıf veya yapı olur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-597">A ***resource*** is a class or struct that implements `System.IDisposable`, which includes a single parameterless method named `Dispose`.</span></span> <span data-ttu-id="1ee89-598">Kaynak kullanan kod, kaynağın artık gerekli olmadığını `Dispose` belirtmek için çağrı yapabilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-598">Code that is using a resource can call `Dispose` to indicate that the resource is no longer needed.</span></span> <span data-ttu-id="1ee89-599">`Dispose` Çağrılırsa, çöp toplamanın bir sonucu olarak otomatik çıkarma işlemi gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-599">If `Dispose` is not called, then automatic disposal eventually occurs as a consequence of garbage collection.</span></span>

<span data-ttu-id="1ee89-600">*Resource_acquisition* biçimi *local_variable_declaration* ise *local_variable_declaration* türü ya da `dynamic` örtük olarak dönüştürülebilir `System.IDisposable`bir tür olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-600">If the form of *resource_acquisition* is *local_variable_declaration* then the type of the *local_variable_declaration* must be either `dynamic` or a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="1ee89-601">*Resource_acquisition* , *ifadesi* ise, bu ifade `System.IDisposable`örtülü olarak dönüştürülebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-601">If the form of *resource_acquisition* is *expression* then this expression must be implicitly convertible to `System.IDisposable`.</span></span>

<span data-ttu-id="1ee89-602">Bir *resource_acquisition* içinde belirtilen yerel değişkenler salt okunurdur ve bir başlatıcı içermelidir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-602">Local variables declared in a *resource_acquisition* are read-only, and must include an initializer.</span></span> <span data-ttu-id="1ee89-603">Katıştırılmış ifade bu yerel değişkenleri değiştirmeye çalışırsa (atama `++` veya ve `--` işleçler aracılığıyla) bir derleme zamanı hatası oluşur, bunların adresini alın veya ya da parametreleri veya `out` parametreleri olarak `ref` geçirin.</span><span class="sxs-lookup"><span data-stu-id="1ee89-603">A compile-time error occurs if the embedded statement attempts to modify these local variables (via assignment or the `++` and `--` operators) , take the address of them, or pass them as `ref` or `out` parameters.</span></span>

<span data-ttu-id="1ee89-604">Bir `using` ifade üç parçaya çevrilir: alma, kullanım ve çıkarma.</span><span class="sxs-lookup"><span data-stu-id="1ee89-604">A `using` statement is translated into three parts: acquisition, usage, and disposal.</span></span> <span data-ttu-id="1ee89-605">Kaynağın kullanımı dolaylı olarak bir `try` `finally` yan tümce içeren bir ifadeye alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-605">Usage of the resource is implicitly enclosed in a `try` statement that includes a `finally` clause.</span></span> <span data-ttu-id="1ee89-606">Bu `finally` yan tümce kaynağı ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-606">This `finally` clause disposes of the resource.</span></span> <span data-ttu-id="1ee89-607">Bir `null` kaynak elde alınırsa, hiçbir `Dispose` çağrı yapılmaz ve hiçbir özel durum oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="1ee89-607">If a `null` resource is acquired, then no call to `Dispose` is made, and no exception is thrown.</span></span> <span data-ttu-id="1ee89-608">Kaynak türtür `dynamic` ise, dönüştürmenin kullanımdan önce başarılı olduğundan emin olmak için, alma `IDisposable` sırasında dinamik bir dinamik dönüştürme ([örtük dinamik dönüştürmeler](conversions.md#implicit-dynamic-conversions)) üzerinden dinamik olarak dönüştürülür. elden.</span><span class="sxs-lookup"><span data-stu-id="1ee89-608">If the resource is of type `dynamic` it is dynamically converted through an implicit dynamic conversion ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)) to `IDisposable` during acquisition in order to ensure that the conversion is successful before the usage and disposal.</span></span>

<span data-ttu-id="1ee89-609">Formun `using` bir açıklaması</span><span class="sxs-lookup"><span data-stu-id="1ee89-609">A `using` statement of the form</span></span>
```csharp
using (ResourceType resource = expression) statement
```
<span data-ttu-id="1ee89-610">olası üç Genişlemeden birine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-610">corresponds to one of three possible expansions.</span></span> <span data-ttu-id="1ee89-611">Null `ResourceType` yapılamayan bir değer türü olduğunda, genişletme</span><span class="sxs-lookup"><span data-stu-id="1ee89-611">When `ResourceType` is a non-nullable value type, the expansion is</span></span>
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

<span data-ttu-id="1ee89-612">Aksi halde, `ResourceType` null yapılabilir bir değer türü veya dışında `dynamic`bir başvuru türü olduğunda, genişletme</span><span class="sxs-lookup"><span data-stu-id="1ee89-612">Otherwise, when `ResourceType` is a nullable value type or a reference type other than `dynamic`, the expansion is</span></span>
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

<span data-ttu-id="1ee89-613">Aksi halde, `ResourceType` ne `dynamic`zaman, genişletme</span><span class="sxs-lookup"><span data-stu-id="1ee89-613">Otherwise, when `ResourceType` is `dynamic`, the expansion is</span></span>
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

<span data-ttu-id="1ee89-614">Her iki genişlede `resource` de değişken gömülü ifadede salt okunurdur `d` ve değişkenine, gömülü ifadeye ve görünmez.</span><span class="sxs-lookup"><span data-stu-id="1ee89-614">In either expansion, the `resource` variable is read-only in the embedded statement, and the `d` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="1ee89-615">Davranışın yukarıdaki genişlemeyle tutarlı olduğu sürece, bir uygulamanın belirli bir using ifadesini farklı bir şekilde uygulaması için, örneğin performans nedenleriyle, bir uygulamaya izin verilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-615">An implementation is permitted to implement a given using-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="1ee89-616">Formun `using` bir açıklaması</span><span class="sxs-lookup"><span data-stu-id="1ee89-616">A `using` statement of the form</span></span>
```csharp
using (expression) statement
```
<span data-ttu-id="1ee89-617">, mümkün olan üç genişleme sahiptir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-617">has the same three possible expansions.</span></span> <span data-ttu-id="1ee89-618">Bu durumda `ResourceType` , varsa, `expression`öğesinin derleme zamanı türü örtülü olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1ee89-618">In this case `ResourceType` is implicitly the compile-time type of the `expression`, if it has one.</span></span> <span data-ttu-id="1ee89-619">Aksi takdirde, `IDisposable` `ResourceType`arabirimin kendisi olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-619">Otherwise the interface `IDisposable` itself is used as the `ResourceType`.</span></span> <span data-ttu-id="1ee89-620">`resource` Değişkenine, gömülü ifadeye, ve görünmez olarak erişilemez.</span><span class="sxs-lookup"><span data-stu-id="1ee89-620">The `resource` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="1ee89-621">Bir *resource_acquisition* , bir *local_variable_declaration*formunu aldığında, belirli bir türün birden çok kaynağını elde etmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="1ee89-621">When a *resource_acquisition* takes the form of a *local_variable_declaration*, it is possible to acquire multiple resources of a given type.</span></span> <span data-ttu-id="1ee89-622">Formun `using` bir açıklaması</span><span class="sxs-lookup"><span data-stu-id="1ee89-622">A `using` statement of the form</span></span>
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
<span data-ttu-id="1ee89-623">, iç içe geçmiş `using` deyimlerin dizisine tam olarak eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="1ee89-623">is precisely equivalent to a sequence of nested `using` statements:</span></span>
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

<span data-ttu-id="1ee89-624">Aşağıdaki örnek adlı `log.txt` bir dosya oluşturur ve dosyaya iki satırlık metin yazar.</span><span class="sxs-lookup"><span data-stu-id="1ee89-624">The example below creates a file named `log.txt` and writes two lines of text to the file.</span></span> <span data-ttu-id="1ee89-625">Örnek daha sonra bu dosyayı okumak için aynı dosyayı açar ve içerilen metin satırlarını konsola kopyalar.</span><span class="sxs-lookup"><span data-stu-id="1ee89-625">The example then opens that same file for reading and copies the contained lines of text to the console.</span></span>
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

<span data-ttu-id="1ee89-626">Ve sınıfları arabirimini kullandığından, örnek, temel alınan dosyanın yazma veya okuma işlemlerinden sonra düzgün şekilde kapatılmasını sağlamak için deyimlerini kullanabilir. `using` `TextWriter` `TextReader` `IDisposable`</span><span class="sxs-lookup"><span data-stu-id="1ee89-626">Since the `TextWriter` and `TextReader` classes implement the `IDisposable` interface, the example can use `using` statements to ensure that the underlying file is properly closed following the write or read operations.</span></span>

## <a name="the-yield-statement"></a><span data-ttu-id="1ee89-627">Yield ekstresi</span><span class="sxs-lookup"><span data-stu-id="1ee89-627">The yield statement</span></span>

<span data-ttu-id="1ee89-628">İfade, bir yineleyici bloğunda ([bloklar](statements.md#blocks)), bir yineleyicinin Numaralandırıcı nesnesine ([Numaralandırıcı nesneleri](classes.md#enumerator-objects)) veya sıralanabilir nesne ([numaralandırılabilir nesneler](classes.md#enumerable-objects)) bir değer vermek veya yinelemenin sonuna işaret etmek için kullanılır. `yield`</span><span class="sxs-lookup"><span data-stu-id="1ee89-628">The `yield` statement is used in an iterator block ([Blocks](statements.md#blocks)) to yield a value to the enumerator object ([Enumerator objects](classes.md#enumerator-objects)) or enumerable object ([Enumerable objects](classes.md#enumerable-objects)) of an iterator or to signal the end of the iteration.</span></span>

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

<span data-ttu-id="1ee89-629">`yield`ayrılmış bir sözcük değil; yalnızca bir `return` veya `break` anahtar sözcüğünden hemen önce kullanıldığında özel anlamı vardır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-629">`yield` is not a reserved word; it has special meaning only when used immediately before a `return` or `break` keyword.</span></span> <span data-ttu-id="1ee89-630">Diğer bağlamlarda `yield` tanımlayıcı olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-630">In other contexts, `yield` can be used as an identifier.</span></span>

<span data-ttu-id="1ee89-631">Aşağıda açıklandığı gibi bir `yield` deyimin görünebileceği çeşitli kısıtlamalar vardır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-631">There are several restrictions on where a `yield` statement can appear, as described in the following.</span></span>

*  <span data-ttu-id="1ee89-632">Bir `yield` deyimin (herhangi bir biçimde) bir *method_body*, *operator_body* veya *accessor_body* dışında görünmesi için derleme zamanı hatası</span><span class="sxs-lookup"><span data-stu-id="1ee89-632">It is a compile-time error for a `yield` statement (of either form) to appear outside a *method_body*, *operator_body* or *accessor_body*</span></span>
*  <span data-ttu-id="1ee89-633">Bir deyimin (herhangi bir `yield` biçimde) anonim bir işlev içinde görünmesi için derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-633">It is a compile-time error for a `yield` statement (of either form) to appear inside an anonymous function.</span></span>
*  <span data-ttu-id="1ee89-634">Bir deyimin `yield` `finally` yan tümcesinde `try` görünmesi için (her bir biçimde) bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-634">It is a compile-time error for a `yield` statement (of either form) to appear in the `finally` clause of a `try` statement.</span></span>
*  <span data-ttu-id="1ee89-635">Bir `yield return` deyimin, `try` herhangi`catch` bir yan tümce içeren bir ifadede görünmesini sağlayan derleme zamanı hatası.</span><span class="sxs-lookup"><span data-stu-id="1ee89-635">It is a compile-time error for a `yield return` statement to appear anywhere in a `try` statement that contains any `catch` clauses.</span></span>

<span data-ttu-id="1ee89-636">Aşağıdaki örnekte, bazı geçerli ve geçersiz `yield` deyim kullanımları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-636">The following example shows some valid and invalid uses of `yield` statements.</span></span>

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

<span data-ttu-id="1ee89-637">Bir örtük dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)), `yield return` deyimdeki ifade türünün, yineleyicinin yield türüne ([yield türü](classes.md#yield-type)) sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-637">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression in the `yield return` statement to the yield type ([Yield type](classes.md#yield-type)) of the iterator.</span></span>

<span data-ttu-id="1ee89-638">Bir `yield return` ifade aşağıdaki gibi yürütülür:</span><span class="sxs-lookup"><span data-stu-id="1ee89-638">A `yield return` statement is executed as follows:</span></span>

*  <span data-ttu-id="1ee89-639">Deyimde verilen ifade değerlendirilir, örtülü olarak yield türüne dönüştürülür ve Numaralandırıcı nesnesinin `Current` özelliğine atanır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-639">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
*  <span data-ttu-id="1ee89-640">Yineleyici bloğunun yürütülmesi askıya alındı.</span><span class="sxs-lookup"><span data-stu-id="1ee89-640">Execution of the iterator block is suspended.</span></span> <span data-ttu-id="1ee89-641">İfade bir veya daha fazla `try` blok içindeyse, ilişkili `finally` bloklar Şu anda yürütülmez. `yield return`</span><span class="sxs-lookup"><span data-stu-id="1ee89-641">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
*  <span data-ttu-id="1ee89-642">Numaralandırıcı nesnesinin `true` yöntemi, Numaralandırıcı nesnesinin bir sonraki öğeye başarıyla ilerlemediğini belirten, çağırana döner. `MoveNext`</span><span class="sxs-lookup"><span data-stu-id="1ee89-642">The `MoveNext` method of the enumerator object returns `true` to its caller, indicating that the enumerator object successfully advanced to the next item.</span></span>

<span data-ttu-id="1ee89-643">Numaralandırıcı nesnesinin `MoveNext` yönteminin sonraki çağrısı, yineleyici bloğunun son askıya alındığı yerden yürütülmesini sürdürür.</span><span class="sxs-lookup"><span data-stu-id="1ee89-643">The next call to the enumerator object's `MoveNext` method resumes execution of the iterator block from where it was last suspended.</span></span>

<span data-ttu-id="1ee89-644">Bir `yield break` ifade aşağıdaki gibi yürütülür:</span><span class="sxs-lookup"><span data-stu-id="1ee89-644">A `yield break` statement is executed as follows:</span></span>

*  <span data-ttu-id="1ee89-645">`try` Deyimle`finally` `try` ilişkili`finally` blokları olan bir veya daha fazla blok varsa, denetim başlangıçta en içteki deyimin bloğuna aktarılır. `yield break`</span><span class="sxs-lookup"><span data-stu-id="1ee89-645">If the `yield break` statement is enclosed by one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="1ee89-646">Ve denetimi bir `finally` bloğun bitiş noktasına ulaştığında, Denetim sonraki kapsayan `try` deyimin `finally` bloğuna aktarılır.</span><span class="sxs-lookup"><span data-stu-id="1ee89-646">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="1ee89-647">Bu işlem, kapsayan `finally` `try` tüm deyimlerin blokları yürütülene kadar yinelenir.</span><span class="sxs-lookup"><span data-stu-id="1ee89-647">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="1ee89-648">Denetim, yineleyici bloğunun çağıranına döndürülür.</span><span class="sxs-lookup"><span data-stu-id="1ee89-648">Control is returned to the caller of the iterator block.</span></span> <span data-ttu-id="1ee89-649">Bu, Numaralandırıcı `Dispose` nesnesinin yöntemiyadayöntemidir.`MoveNext`</span><span class="sxs-lookup"><span data-stu-id="1ee89-649">This is either the `MoveNext` method or `Dispose` method of the enumerator object.</span></span>

<span data-ttu-id="1ee89-650">Bir `yield break` ifade denetimi başka bir yerde bir yere aktarmadığı için, bir `yield break` deyimin bitiş noktasına hiçbir şekilde ulaşılamıyor.</span><span class="sxs-lookup"><span data-stu-id="1ee89-650">Because a `yield break` statement unconditionally transfers control elsewhere, the end point of a `yield break` statement is never reachable.</span></span>
