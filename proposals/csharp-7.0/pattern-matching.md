---
ms.openlocfilehash: 3df21c5816be90387a6cd9242e99ba11f43dfd1c
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485170"
---
# <a name="pattern-matching-for-c-7"></a><span data-ttu-id="44e4c-101">7 için C# model eşleştirme</span><span class="sxs-lookup"><span data-stu-id="44e4c-101">Pattern Matching for C# 7</span></span>

<span data-ttu-id="44e4c-102">İçin C# model eşleştirme uzantıları, işlev dillerinden çok sayıda avantajı ve işlevsel dillerin örüntüsünün eşleşmesini sağlayan, ancak temel alınan dilin hisleriyle sorunsuz bir şekilde tümleşen birçok avantaj sağlayan bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="44e4c-102">Pattern matching extensions for C# enable many of the benefits of algebraic data types and pattern matching from functional languages, but in a way that smoothly integrates with the feel of the underlying language.</span></span> <span data-ttu-id="44e4c-103">Temel özellikler şunlardır: anlam anlamı verilerin şekli tarafından tanımlanan türler olan [kayıt türleri](https://github.com/dotnet/csharplang/blob/master/proposals/records.md); ve, bu veri türlerinin son derece daha çok daha fazla düzeyde ayrışmalarını sağlayan yeni bir ifade biçimi olan kalıp eşleme.</span><span class="sxs-lookup"><span data-stu-id="44e4c-103">The basic features are: [record types](https://github.com/dotnet/csharplang/blob/master/proposals/records.md), which are types whose semantic meaning is described by the shape of the data; and pattern matching, which is a new expression form that enables extremely concise multilevel decomposition of these data types.</span></span> <span data-ttu-id="44e4c-104">Bu yaklaşımın öğeleri, programlama dillerinde [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Hafif bir dil aracılığıyla Genişletilebilir kalıp eşleme") ve [Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Desenler Ile eşleşen nesneler")'daki ilgili özelliklerle ilham.</span><span class="sxs-lookup"><span data-stu-id="44e4c-104">Elements of this approach are inspired by related features in the programming languages [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Extensible Pattern Matching Via a Lightweight Language") and [Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Matching Objects With Patterns").</span></span>

## <a name="is-expression"></a><span data-ttu-id="44e4c-105">İfade ifadesi</span><span class="sxs-lookup"><span data-stu-id="44e4c-105">Is expression</span></span>

<span data-ttu-id="44e4c-106">`is` işleci, bir ifadeyi bir *düzene*karşı test etmek için genişletilir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-106">The `is` operator is extended to test an expression against a *pattern*.</span></span>

```antlr
relational_expression
    : relational_expression 'is' pattern
    ;
```

<span data-ttu-id="44e4c-107">Bu *relational_expression* biçimi, C# belirtideki mevcut formlara ek niteliğindedir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-107">This form of *relational_expression* is in addition to the existing forms in the C# specification.</span></span> <span data-ttu-id="44e4c-108">`is` belirtecinin solundaki *relational_expression* bir değer belirlememez veya bir tür yoksa bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-108">It is a compile-time error if the *relational_expression* to the left of the `is` token does not designate a value or does not have a type.</span></span>

<span data-ttu-id="44e4c-109">Düzenin her *tanımlayıcısı* , `is` işleci `true` sonra *kesinlikle atanan* yeni bir yerel değişken sunar (yani, *doğru olduğunda, kesin olarak atanır*).</span><span class="sxs-lookup"><span data-stu-id="44e4c-109">Every *identifier* of the pattern introduces a new local variable that is *definitely assigned* after the `is` operator is `true` (i.e. *definitely assigned when true*).</span></span>

> <span data-ttu-id="44e4c-110">Note: `is-expression` ve *constant_pattern* *türü* arasında teknik bir belirsizlik vardır; bunlardan biri, tam bir tanımlayıcının geçerli bir ayrıştırmasından kaynaklanabilir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-110">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="44e4c-111">Bunu, dilin önceki sürümleriyle uyumluluk için bir tür olarak bağlamaya çalışırız; yalnızca bu başarısız olursa, diğer bağlamlarda yaptığımız gibi, bulduğu ilk şeyi (sabit veya bir tür olması gerekir) çözeceğiz.</span><span class="sxs-lookup"><span data-stu-id="44e4c-111">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="44e4c-112">Bu belirsizlik yalnızca `is` ifadesinin sağ tarafında bulunur.</span><span class="sxs-lookup"><span data-stu-id="44e4c-112">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

## <a name="patterns"></a><span data-ttu-id="44e4c-113">Desenler</span><span class="sxs-lookup"><span data-stu-id="44e4c-113">Patterns</span></span>

<span data-ttu-id="44e4c-114">Desenler `is` işlecinde ve bir *switch_statement* , gelen verilerin karşılaştırılacağı verilerin şeklini ifade etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-114">Patterns are used in the `is` operator and in a *switch_statement* to express the shape of data against which incoming data is to be compared.</span></span> <span data-ttu-id="44e4c-115">Desenler, verilerin bölümlerinin alt desenlerle eşleştirileceği şekilde özyinelemeli olabilir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-115">Patterns may be recursive so that parts of the data may be matched against sub-patterns.</span></span>

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    ;

declaration_pattern
    : type simple_designation
    ;

constant_pattern
    : shift_expression
    ;

var_pattern
    : 'var' simple_designation
    ;
```

> <span data-ttu-id="44e4c-116">Note: `is-expression` ve *constant_pattern* *türü* arasında teknik bir belirsizlik vardır; bunlardan biri, tam bir tanımlayıcının geçerli bir ayrıştırmasından kaynaklanabilir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-116">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="44e4c-117">Bunu, dilin önceki sürümleriyle uyumluluk için bir tür olarak bağlamaya çalışırız; yalnızca bu başarısız olursa, diğer bağlamlarda yaptığımız gibi, bulduğu ilk şeyi (sabit veya bir tür olması gerekir) çözeceğiz.</span><span class="sxs-lookup"><span data-stu-id="44e4c-117">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="44e4c-118">Bu belirsizlik yalnızca `is` ifadesinin sağ tarafında bulunur.</span><span class="sxs-lookup"><span data-stu-id="44e4c-118">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

### <a name="declaration-pattern"></a><span data-ttu-id="44e4c-119">Bildirim kalıbı</span><span class="sxs-lookup"><span data-stu-id="44e4c-119">Declaration pattern</span></span>

<span data-ttu-id="44e4c-120">*Declaration_pattern* her ikisi de bir ifadenin verilen türden olduğunu sınar ve test başarılı olursa bu türe yayınlar.</span><span class="sxs-lookup"><span data-stu-id="44e4c-120">The *declaration_pattern* both tests that an expression is of a given type and casts it to that type if the test succeeds.</span></span> <span data-ttu-id="44e4c-121">*Simple_designation* bir tanımlayıcıse, verilen tanımlayıcı tarafından adlandırılan belirtilen türde yerel bir değişken tanıtır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-121">If the *simple_designation* is an identifier, it introduces a local variable of the given type named by the given identifier.</span></span> <span data-ttu-id="44e4c-122">Bu yerel değişken, bir kalıp eşleme işleminin sonucu true olduğunda *kesinlikle atanır* .</span><span class="sxs-lookup"><span data-stu-id="44e4c-122">That local variable is *definitely assigned* when the result of the pattern-matching operation is true.</span></span>

```antlr
declaration_pattern
    : type simple_designation
    ;
```

<span data-ttu-id="44e4c-123">Bu ifadenin çalışma zamanı anlam düzeyi, sol taraftaki *relational_expression* işlenenin çalışma zamanı türünü, düzendeki *türe* göre sınar.</span><span class="sxs-lookup"><span data-stu-id="44e4c-123">The runtime semantic of this expression is that it tests the runtime type of the left-hand *relational_expression* operand against the *type* in the pattern.</span></span> <span data-ttu-id="44e4c-124">Çalışma zamanı türü (veya bazı alt tür) ise, `is operator` sonucu `true`.</span><span class="sxs-lookup"><span data-stu-id="44e4c-124">If it is of that runtime type (or some subtype), the result of the `is operator` is `true`.</span></span> <span data-ttu-id="44e4c-125">Sonuç `true`, sol taraftaki işlenenin değerine atanan *tanımlayıcı* tarafından adlı yeni bir yerel değişken bildirir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-125">It declares a new local variable named by the *identifier* that is assigned the value of the left-hand operand when the result is `true`.</span></span>

<span data-ttu-id="44e4c-126">Sol taraftaki statik türdeki belirli birleşimler ve verilen tür uyumsuz olarak değerlendirilir ve derleme zamanı hatası ile sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-126">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="44e4c-127">`E` statik tür değeri, bir kimlik dönüştürmesi, örtük bir başvuru dönüştürmesi, paketleme dönüştürmesi, açık bir başvuru dönüştürmesi veya `E` ' den `T`' a bir kutudan çıkarma dönüştürmesi varsa, tür `T` ile *uyumlu* olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-127">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`.</span></span> <span data-ttu-id="44e4c-128">`E` türündeki bir ifade ile eşleştirildiği tür deseninin türüyle uyumlu kalıp yoksa, derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-128">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

> <span data-ttu-id="44e4c-129">Note: [7,1 C# içinde](../csharp-7.1/generics-pattern-match.md) , giriş türü veya tür `T` bir açık tür ise, bu, bir kalıp eşleme işlemine izin vermek için bunu genişlettik.</span><span class="sxs-lookup"><span data-stu-id="44e4c-129">Note: [In C# 7.1 we extend this](../csharp-7.1/generics-pattern-match.md) to permit a pattern-matching operation if either the input type or the type `T` is an open type.</span></span> <span data-ttu-id="44e4c-130">Bu paragraf şu şekilde değiştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="44e4c-130">This paragraph is replaced by the following:</span></span>
> 
> <span data-ttu-id="44e4c-131">Sol taraftaki statik türdeki belirli birleşimler ve verilen tür uyumsuz olarak değerlendirilir ve derleme zamanı hatası ile sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-131">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="44e4c-132">`E` statik tür değeri, bir kimlik dönüştürmesi, örtük bir başvuru dönüştürmesi, paketleme dönüştürmesi, açık bir başvuru dönüştürmesi veya `E` ' den `T`' e bir kutudan çıkarma dönüştürmesi ya da **`E` ya da `T` bir açık tür**ise, tür ile *uyumlu* bir değer `T` olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-132">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`, **or if either `E` or `T` is an open type**.</span></span> <span data-ttu-id="44e4c-133">`E` türündeki bir ifade ile eşleştirildiği tür deseninin türüyle uyumlu kalıp yoksa, derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-133">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

<span data-ttu-id="44e4c-134">Bildirim stili, başvuru türlerinin çalışma zamanı türü testlerini gerçekleştirmek için yararlıdır ve deyim yerini alır</span><span class="sxs-lookup"><span data-stu-id="44e4c-134">The declaration pattern is useful for performing run-time type tests of reference types, and replaces the idiom</span></span>

```csharp
var v = expr as Type;
if (v != null) { // code using v }
```

<span data-ttu-id="44e4c-135">Biraz daha kısa</span><span class="sxs-lookup"><span data-stu-id="44e4c-135">With the slightly more concise</span></span>

```csharp
if (expr is Type v) { // code using v }
```

<span data-ttu-id="44e4c-136">*Tür* null yapılabilir bir değer türünde ise bu bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-136">It is an error if *type* is a nullable value type.</span></span>

<span data-ttu-id="44e4c-137">Bildirim stili, null yapılabilir türlerin değerlerini test etmek için kullanılabilir: `Nullable<T>` (veya kutulanmış `T`) bir değer, değer null ise ve `T2` türü `T`veya bazı temel tür veya `T`arabirimse `T2 id` bir tür düzeniyle eşleşir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-137">The declaration pattern can be used to test values of nullable types: a value of type `Nullable<T>` (or a boxed `T`) matches a type pattern `T2 id` if the value is non-null and the type of `T2` is `T`, or some base type or interface of `T`.</span></span> <span data-ttu-id="44e4c-138">Örneğin, kod parçasında</span><span class="sxs-lookup"><span data-stu-id="44e4c-138">For example, in the code fragment</span></span>

```csharp
int? x = 3;
if (x is int v) { // code using v }
```

<span data-ttu-id="44e4c-139">`if` deyimin koşulu çalışma zamanında `true` ve `v` değişkeni, blok içindeki `int` türündeki `3` değeri tutar.</span><span class="sxs-lookup"><span data-stu-id="44e4c-139">The condition of the `if` statement is `true` at runtime and the variable `v` holds the value `3` of type `int` inside the block.</span></span>

### <a name="constant-pattern"></a><span data-ttu-id="44e4c-140">Sabit model</span><span class="sxs-lookup"><span data-stu-id="44e4c-140">Constant pattern</span></span>

```antlr
constant_pattern
    : shift_expression
    ;
```

<span data-ttu-id="44e4c-141">Sabit bir model, bir ifadenin değerini sabit bir değere göre sınar.</span><span class="sxs-lookup"><span data-stu-id="44e4c-141">A constant pattern tests the value of an expression against a constant value.</span></span> <span data-ttu-id="44e4c-142">Sabit, değişmez değer, belirtilen bir `const` değişkeninin adı ya da bir numaralandırma sabiti veya bir `typeof` ifadesi gibi herhangi bir sabit ifade olabilir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-142">The constant may be any constant expression, such as a literal, the name of a declared `const` variable, or an enumeration constant, or a `typeof` expression.</span></span>

<span data-ttu-id="44e4c-143">Hem *e* hem de *c* integral türtürsiyse, ifadenin sonucu `e == c` `true`ise, model eşleştirildiği kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-143">If both *e* and *c* are of integral types, the pattern is considered matched if the result of the expression `e == c` is `true`.</span></span>

<span data-ttu-id="44e4c-144">Aksi takdirde, `object.Equals(e, c)` `true`döndürürse, model eşleşen kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-144">Otherwise the pattern is considered matching if `object.Equals(e, c)` returns `true`.</span></span> <span data-ttu-id="44e4c-145">Bu durumda, statik *e* türü, sabit türü ile *uyumlu kalıp* değilse, derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="44e4c-145">In this case it is a compile-time error if the static type of *e* is not *pattern compatible* with the type of the constant.</span></span>

### <a name="var-pattern"></a><span data-ttu-id="44e4c-146">var deseninin</span><span class="sxs-lookup"><span data-stu-id="44e4c-146">Var pattern</span></span>

```antlr
var_pattern
    : 'var' simple_designation
    ;
```

<span data-ttu-id="44e4c-147">*E* ifadesi her zaman *var_pattern* eşleşir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-147">An expression *e* matches a *var_pattern* always.</span></span> <span data-ttu-id="44e4c-148">Diğer bir deyişle, var olan bir *model* ile eşleşme her zaman başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="44e4c-148">In other words, a match to a *var pattern* always succeeds.</span></span> <span data-ttu-id="44e4c-149">*Simple_designation* bir tanımlayıcıse, çalışma zamanında *e* değeri yeni tanıtılan bir yerel değişkene bağlanır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-149">If the *simple_designation* is an identifier, then at runtime the value of *e* is bound to a newly introduced local variable.</span></span> <span data-ttu-id="44e4c-150">Yerel değişkenin türü, *e*'nin statik türüdür.</span><span class="sxs-lookup"><span data-stu-id="44e4c-150">The type of the local variable is the static type of *e*.</span></span>

<span data-ttu-id="44e4c-151">Ad `var` bir türe bağlandığında bu bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-151">It is an error if the name `var` binds to a type.</span></span>

## <a name="switch-statement"></a><span data-ttu-id="44e4c-152">Switch deyimleri</span><span class="sxs-lookup"><span data-stu-id="44e4c-152">Switch statement</span></span>

<span data-ttu-id="44e4c-153">`switch` deyimi, *anahtar ifadesiyle*eşleşen ilişkili bir düzene sahip olan ilk bloğun yürütülmesi için seçilecek şekilde genişletilir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-153">The `switch` statement is extended to select for execution the first block having an associated pattern that matches the *switch expression*.</span></span>

```antlr
switch_label
    : 'case' complex_pattern case_guard? ':'
    | 'case' constant_expression case_guard? ':'
    | 'default' ':'
    ;

case_guard
    : 'when' expression
    ;
```

<span data-ttu-id="44e4c-154">Desenlerin eşleştirildiği sıra tanımlı değil.</span><span class="sxs-lookup"><span data-stu-id="44e4c-154">The order in which patterns are matched is not defined.</span></span> <span data-ttu-id="44e4c-155">Bir derleyicinin desenleri sıralamayla eşleşmesi ve diğer desenlerin eşleşme sonucunu hesaplamak için zaten eşleşen desenlerin sonuçlarını yeniden kullanmak için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-155">A compiler is permitted to match patterns out of order, and to reuse the results of already matched patterns to compute the result of matching of other patterns.</span></span>

<span data-ttu-id="44e4c-156">Bir *durum koruması* varsa, ifadesi `bool`türündedir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-156">If a *case-guard* is present, its expression is of type `bool`.</span></span> <span data-ttu-id="44e4c-157">Bu, büyük/küçük harf karşılanması durumunda karşılanması gereken ek bir koşul olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-157">It is evaluated as an additional condition that must be satisfied for the case to be considered satisfied.</span></span>

<span data-ttu-id="44e4c-158">*Switch_label* çalışma zamanında hiçbir etkisi yoksa, bu bir hatadır çünkü bu, kendi deseninin önceki durumlar tarafından bir alt grup haline getirilir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-158">It is an error if a *switch_label* can have no effect at runtime because its pattern is subsumed by previous cases.</span></span> <span data-ttu-id="44e4c-159">[TODO: derleyicinin bu kararya ulaşmak için kullanması gereken tekniklerin daha kesin olması gerekir.]</span><span class="sxs-lookup"><span data-stu-id="44e4c-159">[TODO: We should be more precise about the techniques the compiler is required to use to reach this judgment.]</span></span>

<span data-ttu-id="44e4c-160">Bir *switch_label* olarak belirtilen bir model değişkeni, yalnızca bu durum bloğu tam olarak bir *switch_label*içeriyorsa, büyük/küçük harf bloğunda kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-160">A pattern variable declared in a *switch_label* is definitely assigned in its case block if and only if that case block contains precisely one *switch_label*.</span></span>

<span data-ttu-id="44e4c-161">[TODO: bir *anahtar bloğunun* ne zaman ulaşılabilir olduğunu belirtmemiz gerekir.]</span><span class="sxs-lookup"><span data-stu-id="44e4c-161">[TODO: We should specify when a *switch block* is reachable.]</span></span>

### <a name="scope-of-pattern-variables"></a><span data-ttu-id="44e4c-162">Model değişkenlerinin kapsamı</span><span class="sxs-lookup"><span data-stu-id="44e4c-162">Scope of pattern variables</span></span>

<span data-ttu-id="44e4c-163">Bir düzende belirtilen değişkenin kapsamı aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="44e4c-163">The scope of a variable declared in a pattern is as follows:</span></span>

- <span data-ttu-id="44e4c-164">Desenler bir Case etiketse, değişkenin kapsamı *Case bloğu*olur.</span><span class="sxs-lookup"><span data-stu-id="44e4c-164">If the pattern is a case label, then the scope of the variable is the *case block*.</span></span>

<span data-ttu-id="44e4c-165">Aksi takdirde, değişken bir *is_pattern* ifadesinde, ve kapsamı, *is_pattern* ifadesini içeren ifadeyi hemen kapsayan yapıyı temel alır:</span><span class="sxs-lookup"><span data-stu-id="44e4c-165">Otherwise the variable is declared in an *is_pattern* expression, and its scope is based on the construct immediately enclosing the expression containing the *is_pattern* expression as follows:</span></span>

- <span data-ttu-id="44e4c-166">İfade bir ifade ifadesinde ise, kapsamı lambda gövdesidir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-166">If the expression is in an expression-bodied lambda, its scope is the body of the lambda.</span></span>
- <span data-ttu-id="44e4c-167">İfade bir ifade-Bodied yöntemi veya özelliği ise, kapsamı yöntemin veya özelliğin gövdesidir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-167">If the expression is in an expression-bodied method or property, its scope is the body of the method or property.</span></span>
- <span data-ttu-id="44e4c-168">İfade bir `catch` yan tümcesinin `when` yan tümcelerinde yer alıyorsa, kapsamı o `catch` yan tümcesine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-168">If the expression is in a `when` clause of a `catch` clause, its scope is that `catch` clause.</span></span>
- <span data-ttu-id="44e4c-169">İfade bir *iteration_statement*ise, kapsamı yalnızca o deyimdir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-169">If the expression is in an *iteration_statement*, its scope is just that statement.</span></span>
- <span data-ttu-id="44e4c-170">Aksi takdirde, ifade başka bir deyim formundadır, kapsamı deyimi içeren kapsamdır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-170">Otherwise if the expression is in some other statement form, its scope is the scope containing the statement.</span></span>

<span data-ttu-id="44e4c-171">Kapsamı belirlemek amacıyla bir *embedded_statement* kendi kapsamında olduğu kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-171">For the purpose of determining the scope, an *embedded_statement* is considered to be in its own scope.</span></span> <span data-ttu-id="44e4c-172">Örneğin, bir *if_statement* için dilbilgisi</span><span class="sxs-lookup"><span data-stu-id="44e4c-172">For example, the grammar for an *if_statement* is</span></span>

``` antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="44e4c-173">Bu nedenle bir *if_statement* denetimli bildiriminde bir model değişkeni bildiriliyorsa, kapsamı bu *embedded_statement*kısıtlıdır:</span><span class="sxs-lookup"><span data-stu-id="44e4c-173">So if the controlled statement of an *if_statement* declares a pattern variable, its scope is restricted to that *embedded_statement*:</span></span>

```csharp
if (x) M(y is var z);
```

<span data-ttu-id="44e4c-174">Bu durumda `z` kapsamı gömülü ifadedir `M(y is var z);`.</span><span class="sxs-lookup"><span data-stu-id="44e4c-174">In this case the scope of `z` is the embedded statement `M(y is var z);`.</span></span>

<span data-ttu-id="44e4c-175">Diğer nedenlerden dolayı diğer nedenlerle hatalar vardır (örneğin, bir parametrenin varsayılan değeri veya bir özniteliği, her ikisi de bir hata olan bu içerikler sabit bir ifade gerektirdiğinden).</span><span class="sxs-lookup"><span data-stu-id="44e4c-175">Other cases are errors for other reasons (e.g. in a parameter's default value or an attribute, both of which are an error because those contexts require a constant expression).</span></span>

> <span data-ttu-id="44e4c-176">[7,3 C# ' de,](../csharp-7.3/expression-variables-in-initializers.md) bir model değişkeninin bildirilebilecek aşağıdaki bağlamları ekledik:</span><span class="sxs-lookup"><span data-stu-id="44e4c-176">[In C# 7.3 we added the following contexts](../csharp-7.3/expression-variables-in-initializers.md) in which a pattern variable may be declared:</span></span>
> - <span data-ttu-id="44e4c-177">İfade bir *Oluşturucu başlatıcısında*ise, kapsamı *Oluşturucu başlatıcısı* ve oluşturucunun gövdesi olur.</span><span class="sxs-lookup"><span data-stu-id="44e4c-177">If the expression is in a *constructor initializer*, its scope is the *constructor initializer* and the constructor's body.</span></span>
> - <span data-ttu-id="44e4c-178">İfade bir alan başlatıcısında ise, kapsamı göründüğü *equals_value_clause* .</span><span class="sxs-lookup"><span data-stu-id="44e4c-178">If the expression is in a field initializer, its scope is the *equals_value_clause* in which it appears.</span></span>
> - <span data-ttu-id="44e4c-179">İfade bir lambda gövdesine çevrilebilmesi için belirtilen bir sorgu yan tümcesinde ise, kapsamı yalnızca o ifadedir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-179">If the expression is in a query clause that is specified to be translated into the body of a lambda, its scope is just that expression.</span></span>

## <a name="changes-to-syntactic-disambiguation"></a><span data-ttu-id="44e4c-180">Sözdizimsel Kesinleştirme değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="44e4c-180">Changes to syntactic disambiguation</span></span>

<span data-ttu-id="44e4c-181">C# Dilbilgisinin belirsiz olduğu ve dil belirtiminin bu belirsizlikleri nasıl çözümlendiğini belirten genel türler içeren durumlar vardır:</span><span class="sxs-lookup"><span data-stu-id="44e4c-181">There are situations involving generics where the C# grammar is ambiguous, and the language spec says how to resolve those ambiguities:</span></span>

> #### <a name="7652-grammar-ambiguities"></a><span data-ttu-id="44e4c-182">7.6.5.2 dilbilgisi belirsizlikleri</span><span class="sxs-lookup"><span data-stu-id="44e4c-182">7.6.5.2 Grammar ambiguities</span></span>
> <span data-ttu-id="44e4c-183">*Basit ad* (§ 7.6.3) ve *üye erişimi* (§ 7.6.5) için üretimler, ifadelerde dilbilgisi için verebilir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-183">The productions for *simple-name* (§7.6.3) and *member-access* (§7.6.5) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="44e4c-184">Örneğin, ifade:</span><span class="sxs-lookup"><span data-stu-id="44e4c-184">For example, the statement:</span></span>
> ```csharp
> F(G<A,B>(7));
> ```
> <span data-ttu-id="44e4c-185">, `G < A` ve `B > (7)`iki bağımsız değişkenle `F` çağrısı olarak yorumlanamaz.</span><span class="sxs-lookup"><span data-stu-id="44e4c-185">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="44e4c-186">Alternatif olarak, iki tür bağımsız değişkeni ve bir normal bağımsız değişkenle `G` genel bir yöntem çağrısı olan bir bağımsız değişkenle `F` çağrısı olarak yorumlanabilecek.</span><span class="sxs-lookup"><span data-stu-id="44e4c-186">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

> <span data-ttu-id="44e4c-187">Bir belirteç dizisi bir *basit-ad* (§ 7.6.3), *üye erişimi* (§ 7.6.5) veya bir *tür bağımsız değişkeni* (§ 18.5.2) ile biten *işaretçi-üye erişimi* (§ 4.4.1) olarak ayrıştırılamıyorsa, kapanış `>` belirtecini izleyen belirteç incelenir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-187">If a sequence of tokens can be parsed (in context) as a *simple-name* (§7.6.3), *member-access* (§7.6.5), or *pointer-member-access* (§18.5.2) ending with a *type-argument-list* (§4.4.1), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="44e4c-188">Bunlardan biri ise</span><span class="sxs-lookup"><span data-stu-id="44e4c-188">If it is one of</span></span>
> ```none
> (  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
> ```
> <span data-ttu-id="44e4c-189">daha sonra *tür bağımsız değişken listesi* , *basit ad*, *üye erişimi* veya *işaretçi-üye erişiminin* bir parçası olarak korunur ve belirteçleri dizisinin diğer olası ayrıştırması atılır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-189">then the *type-argument-list* is retained as part of the *simple-name*, *member-access* or *pointer-member-access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="44e4c-190">Aksi takdirde, *tür bağımsız değişken listesi* , belirteçlerin başka bir olası ayrıştırması olmasa bile, *basit ad*, *üye erişimi* veya > *işaretçi-üye erişiminin*parçası olarak kabul edilmez.</span><span class="sxs-lookup"><span data-stu-id="44e4c-190">Otherwise, the *type-argument-list* is not considered to be part of the *simple-name*, *member-access* or > *pointer-member-access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="44e4c-191">Bir *ad alanı-veya-tür adı* (§ 3,8) içinde bir *tür bağımsız değişken listesi* ayrıştırılırken bu kuralların uygulanmadığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="44e4c-191">Note that these rules are not applied when parsing a *type-argument-list* in a *namespace-or-type-name* (§3.8).</span></span> <span data-ttu-id="44e4c-192">İfade</span><span class="sxs-lookup"><span data-stu-id="44e4c-192">The statement</span></span>
> ```csharp
> F(G<A,B>(7));
> ```
> <span data-ttu-id="44e4c-193">Bu kurala göre, iki tür bağımsız değişkeni ve bir normal bağımsız değişkenle `G` genel bir yöntem çağrısı olan bir bağımsız değişkenle `F` çağrısı olarak yorumlanacaktır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-193">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="44e4c-194">Deyimler</span><span class="sxs-lookup"><span data-stu-id="44e4c-194">The statements</span></span>
> ```csharp
> F(G < A, B > 7);
> F(G < A, B >> 7);
> ```
> <span data-ttu-id="44e4c-195">her biri, iki bağımsız değişkenle `F` çağrısı olarak yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-195">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="44e4c-196">İfade</span><span class="sxs-lookup"><span data-stu-id="44e4c-196">The statement</span></span>
> ```csharp
> x = F < A > +y;
> ```
> <span data-ttu-id="44e4c-197">, bir *Type-argument-List* ile ardından bir ikili artı işleci ile bir *basit ad* yerine, ' den daha az işleç, büyüktür işleci ve birli Plus `x = (F < A) > (+y)`işleci olarak yorumlanacaktır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-197">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple-name* with a *type-argument-list* followed by a binary plus operator.</span></span> <span data-ttu-id="44e4c-198">İfadesinde</span><span class="sxs-lookup"><span data-stu-id="44e4c-198">In the statement</span></span>
> ```csharp
> x = y is C<T> + z;
> ```
> <span data-ttu-id="44e4c-199">belirteçler `C<T>` bir *ad alanı veya tür adı* olarak yorumlanır- *Argument-List*.</span><span class="sxs-lookup"><span data-stu-id="44e4c-199">the tokens `C<T>` are interpreted as a *namespace-or-type-name* with a *type-argument-list*.</span></span>

<span data-ttu-id="44e4c-200">7 ' de C# sunulan ve bu Kesinleştirme kurallarını dilin karmaşıklığını işlemeye yetecek kadar yeterli olmayan değişiklikler vardır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-200">There are a number of changes being introduced in C# 7 that make these disambiguation rules no longer sufficient to handle the complexity of the language.</span></span>

### <a name="out-variable-declarations"></a><span data-ttu-id="44e4c-201">Out değişken bildirimleri</span><span class="sxs-lookup"><span data-stu-id="44e4c-201">Out variable declarations</span></span>

<span data-ttu-id="44e4c-202">Artık bir out bağımsız değişkeninde bir değişken bildirmek mümkündür:</span><span class="sxs-lookup"><span data-stu-id="44e4c-202">It is now possible to declare a variable in an out argument:</span></span>

```csharp
M(out Type name);
```

<span data-ttu-id="44e4c-203">Ancak, tür genel olabilir:</span><span class="sxs-lookup"><span data-stu-id="44e4c-203">However, the type may be generic:</span></span> 

```csharp
M(out A<B> name);
```

<span data-ttu-id="44e4c-204">Bağımsız değişken için dil dilbilgisi *ifadesi*kullandığından, bu bağlam Kesinleştirme kuralına tabidir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-204">Since the language grammar for the argument uses *expression*, this context is subject to the disambiguation rule.</span></span> <span data-ttu-id="44e4c-205">Bu durumda, kapanış `>`, bir *tür bağımsız değişken listesi*olarak işlenmesine izin veren belirteçlerden biri olmayan bir *tanımlayıcı*tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-205">In this case the closing `>` is followed by an *identifier*, which is not one of the tokens that permits it to be treated as a *type-argument-list*.</span></span> <span data-ttu-id="44e4c-206">Bu nedenle **, bir *tür bağımsız değişken listesi*için belirsizliği tetikleyen belirteç kümesine *tanımlayıcı* eklemeyi önerin.**</span><span class="sxs-lookup"><span data-stu-id="44e4c-206">I therefore propose to **add *identifier* to the set of tokens that triggers the disambiguation to a *type-argument-list*.**</span></span>

### <a name="tuples-and-deconstruction-declarations"></a><span data-ttu-id="44e4c-207">Tanımlama grupları ve ayrıştırma bildirimleri</span><span class="sxs-lookup"><span data-stu-id="44e4c-207">Tuples and deconstruction declarations</span></span>

<span data-ttu-id="44e4c-208">Bir demet sabit değeri tamamen aynı sorun üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-208">A tuple literal runs into exactly the same issue.</span></span> <span data-ttu-id="44e4c-209">Tanımlama grubu ifadesini göz önünde bulundurun</span><span class="sxs-lookup"><span data-stu-id="44e4c-209">Consider the tuple expression</span></span>

```csharp
(A < B, C > D, E < F, G > H)
```

<span data-ttu-id="44e4c-210">Bağımsız değişken listesini C# ayrıştırmak için eski 6 kuralları altında, bu, ilk olarak `A < B` başlayarak dört öğe içeren bir tanımlama grubu olarak ayrıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-210">Under the old C# 6 rules for parsing an argument list, this would parse as a tuple with four elements, starting with `A < B` as the first.</span></span> <span data-ttu-id="44e4c-211">Bununla birlikte, bu, bir zaman oluşumunun solunda göründüğünde, yukarıdaki *tanımlayıcı* belirteç tarafından tetiklenerek, daha önce açıklandığı gibi kesinleştirilmesine istiyoruz:</span><span class="sxs-lookup"><span data-stu-id="44e4c-211">However, when this appears on the left of a deconstruction, we want the disambiguation triggered by the *identifier* token as described above:</span></span>

```csharp
(A<B,C> D, E<F,G> H) = e;
```

<span data-ttu-id="44e4c-212">Bu, ilki `A<B,C>` ve `D`adlı iki değişken bildiren bir deinşaat bildirimidir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-212">This is a deconstruction declaration which declares two variables, the first of which is of type `A<B,C>` and named `D`.</span></span> <span data-ttu-id="44e4c-213">Diğer bir deyişle, demet değişmez değeri her biri bir bildirim ifadesi olan iki ifade içerir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-213">In other words, the tuple literal contains two expressions, each of which is a declaration expression.</span></span>

<span data-ttu-id="44e4c-214">Belirtim ve derleyicinin basitliği için, bu demet sabit değerinin göründüğü yerde iki öğeli bir kayıt kümesi olarak ayrıştırılmasını (atamanın sol tarafında görünüp başlatılmayacağını) önerdim.</span><span class="sxs-lookup"><span data-stu-id="44e4c-214">For simplicity of the specification and compiler, I propose that this tuple literal be parsed as a two-element tuple wherever it appears (whether or not it appears on the left-hand-side of an assignment).</span></span> <span data-ttu-id="44e4c-215">Bu, önceki bölümde açıklanan belirsizinin doğal bir sonucu olacaktır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-215">That would be a natural result of the disambiguation described in the previous section.</span></span>

### <a name="pattern-matching"></a><span data-ttu-id="44e4c-216">Desenler eşleme</span><span class="sxs-lookup"><span data-stu-id="44e4c-216">Pattern-matching</span></span>

<span data-ttu-id="44e4c-217">Model eşleştirme, ifade türü belirsizliğin ortaya çıkarmakta olduğu yeni bir bağlam tanıtır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-217">Pattern matching introduces a new context where the expression-type ambiguity arises.</span></span> <span data-ttu-id="44e4c-218">Daha önce `is` işlecinin sağ tarafında bir tür vardı.</span><span class="sxs-lookup"><span data-stu-id="44e4c-218">Previously the right-hand-side of an `is` operator was a type.</span></span> <span data-ttu-id="44e4c-219">Artık bir tür veya ifade olabilir ve bir tür ise, bir tanımlayıcı gelebilir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-219">Now it can be a type or expression, and if it is a type it may be followed by an identifier.</span></span> <span data-ttu-id="44e4c-220">Teknik olarak, var olan kodun anlamını değiştirebilir:</span><span class="sxs-lookup"><span data-stu-id="44e4c-220">This can, technically, change the meaning of existing code:</span></span>

```csharp
var x = e is T < A > B;
```

<span data-ttu-id="44e4c-221">Bu, C# 6 kuralları altında şu şekilde ayrıştırılabilir</span><span class="sxs-lookup"><span data-stu-id="44e4c-221">This could be parsed under C#6 rules as</span></span>

```csharp
var x = ((e is T) < A) > B;
```

<span data-ttu-id="44e4c-222">ancak C# 7 kuralları altında (yukarıda önerilen Kesinleştirme ile birlikte)</span><span class="sxs-lookup"><span data-stu-id="44e4c-222">but under under C#7 rules (with the disambiguation proposed above) would be parsed as</span></span>

```csharp
var x = e is T<A> B;
```

<span data-ttu-id="44e4c-223">`T<A>`türünde bir değişken `B` bildiren.</span><span class="sxs-lookup"><span data-stu-id="44e4c-223">which declares a variable `B` of type `T<A>`.</span></span> <span data-ttu-id="44e4c-224">Neyse ki, yerel ve Roslyn derleyicileri, C# 6 kodunda sözdizimi hatası vertikleri bir hata içermektedir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-224">Fortunately, the native and Roslyn compilers have a bug whereby they give a syntax error on the C#6 code.</span></span> <span data-ttu-id="44e4c-225">Bu nedenle, bu son değişiklik bir sorun değildir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-225">Therefore this particular breaking change is not a concern.</span></span>

<span data-ttu-id="44e4c-226">Örüntü eşleme, bir türü seçmek için belirsizlik çözümünü destekleyen ek belirteçler sunar.</span><span class="sxs-lookup"><span data-stu-id="44e4c-226">Pattern-matching introduces additional tokens that should drive the ambiguity resolution toward selecting a type.</span></span> <span data-ttu-id="44e4c-227">Mevcut geçerli C# 6 kodunun aşağıdaki örnekleri, ek Kesinleştirme kuralları olmadan bozulur:</span><span class="sxs-lookup"><span data-stu-id="44e4c-227">The following examples of existing valid C#6 code would be broken without additional disambiguation rules:</span></span>

```csharp
var x = e is A<B> && f;            // &&
var x = e is A<B> || f;            // ||
var x = e is A<B> & f;             // &
var x = e is A<B>[];               // [
```

### <a name="proposed-change-to-the-disambiguation-rule"></a><span data-ttu-id="44e4c-228">Kesinleştirme kuralında önerilen değişiklik</span><span class="sxs-lookup"><span data-stu-id="44e4c-228">Proposed change to the disambiguation rule</span></span>

<span data-ttu-id="44e4c-229">Belirteçlerin, ayırt edici belirteç listesini değiştirme belirtimini</span><span class="sxs-lookup"><span data-stu-id="44e4c-229">I propose to revise the specification to change the list of disambiguating tokens from</span></span>

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```

<span data-ttu-id="44e4c-230">kime</span><span class="sxs-lookup"><span data-stu-id="44e4c-230">to</span></span>

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [
```

<span data-ttu-id="44e4c-231">Ve bazı bağlamlarda *tanımlayıcıyı* Kesinleştirme belirteci olarak değerlendiririz.</span><span class="sxs-lookup"><span data-stu-id="44e4c-231">And, in certain contexts, we treat *identifier* as a disambiguating token.</span></span> <span data-ttu-id="44e4c-232">Bu bağlamlar, ayırt edilen belirteçlerin sırasının kendisinden hemen önce `is`, `case`veya `out`ya da bir demet sabit öğesinin ilk öğesi ayrıştırılırken oluşur (Bu durumda, belirteçlerin önünde `(` veya `:`, daha sonra da tanımlayıcı bir `,`) veya bir dizi sabit değerinin sonraki bir öğesi tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-232">Those contexts are where the sequence of tokens being disambiguated is immediately preceded by one of the keywords `is`, `case`, or `out`, or arises while parsing the first element of a tuple literal (in which case the tokens are preceded by `(` or `:` and the identifier is followed by a `,`) or a subsequent element of a tuple literal.</span></span>

### <a name="modified-disambiguation-rule"></a><span data-ttu-id="44e4c-233">Değişiklik Kesinleştirme kuralı</span><span class="sxs-lookup"><span data-stu-id="44e4c-233">Modified disambiguation rule</span></span>

<span data-ttu-id="44e4c-234">Düzeltilen Kesinleştirme kuralı şuna benzer olacaktır</span><span class="sxs-lookup"><span data-stu-id="44e4c-234">The revised disambiguation rule would be something like this</span></span>

> <span data-ttu-id="44e4c-235">Bir belirteç dizisi bir *basit-ad* (§ 7.6.3), *üye erişimi* (§ 7.6.5) veya bir *tür bağımsız değişkeni* (§ 18.5.2) ile biten *işaretçi-üye-erişim* (§ 4.4.1) olarak ayrıştırılabiliyorsanız, kapatma `>` belirtecini izleyen belirteç incelenmekte olup olmadığını görmek için</span><span class="sxs-lookup"><span data-stu-id="44e4c-235">If a sequence of tokens can be parsed (in context) as a *simple-name* (§7.6.3), *member-access* (§7.6.5), or *pointer-member-access* (§18.5.2) ending with a *type-argument-list* (§4.4.1), the token immediately following the closing `>` token is examined, to see if it is</span></span>
> - <span data-ttu-id="44e4c-236">Birisi `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; veya</span><span class="sxs-lookup"><span data-stu-id="44e4c-236">One of `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; or</span></span>
> - <span data-ttu-id="44e4c-237">İlişkisel işleçlerden biri `<  >  <=  >=  is as`; veya</span><span class="sxs-lookup"><span data-stu-id="44e4c-237">One of the relational operators `<  >  <=  >=  is as`; or</span></span>
> - <span data-ttu-id="44e4c-238">Sorgu ifadesinin içinde görünen bağlamsal sorgu anahtar sözcüğü; veya</span><span class="sxs-lookup"><span data-stu-id="44e4c-238">A contextual query keyword appearing inside a query expression; or</span></span>
> - <span data-ttu-id="44e4c-239">Belirli bağlamlarda *tanımlayıcıyı* Kesinleştirme belirteci olarak değerlendiririz.</span><span class="sxs-lookup"><span data-stu-id="44e4c-239">In certain contexts, we treat *identifier* as a disambiguating token.</span></span> <span data-ttu-id="44e4c-240">Bu bağlamlar, ayırt edilen belirteçlerin sırasının, bir tanımlama sözcüğünün ilk öğesi ayrıştırılırken `is`, `case` veya `out`ya da ortaya çıkar. (Bu durumda, belirteçlerin önünde `(` veya `:`, daha sonra da tanımlayıcı bir `,`) veya bir dizi sabit değerinin sonraki bir öğesi gelir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-240">Those contexts are where the sequence of tokens being disambiguated is immediately preceded by one of the keywords `is`, `case` or `out`, or arises while parsing the first element of a tuple literal (in which case the tokens are preceded by `(` or `:` and the identifier is followed by a `,`) or a subsequent element of a tuple literal.</span></span>
> 
> <span data-ttu-id="44e4c-241">Aşağıdaki belirteç bu liste veya böyle bir bağlamdaki bir tanımlayıcı arasında ise, *tür bağımsız değişken listesi* *basit ad*, *üye erişimi* veya *işaretçi üye erişiminin* bir parçası olarak korunur ve belirteçleri dizisinin diğer olası ayrıştırması atılır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-241">If the following token is among this list, or an identifier in such a context, then the *type-argument-list* is retained as part of the *simple-name*, *member-access* or  *pointer-member-access* and any other possible parse of the sequence of tokens is discarded.</span></span>  <span data-ttu-id="44e4c-242">Aksi takdirde, *tür bağımsız değişken listesi* , belirteçlerin başka bir olası ayrıştırması olmasa bile, *basit ad*, *üye erişimi* veya *işaretçi-üye erişiminin*bir parçası olarak kabul edilmez.</span><span class="sxs-lookup"><span data-stu-id="44e4c-242">Otherwise, the *type-argument-list* is not considered to be part of the *simple-name*, *member-access* or *pointer-member-access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="44e4c-243">Bir *ad alanı-veya-tür adı* (§ 3,8) içinde bir *tür bağımsız değişken listesi* ayrıştırılırken bu kuralların uygulanmadığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="44e4c-243">Note that these rules are not applied when parsing a *type-argument-list* in a *namespace-or-type-name* (§3.8).</span></span>

### <a name="breaking-changes-due-to-this-proposal"></a><span data-ttu-id="44e4c-244">Bu tekliften kaynaklanan değişiklikler kesiliyor</span><span class="sxs-lookup"><span data-stu-id="44e4c-244">Breaking changes due to this proposal</span></span>

<span data-ttu-id="44e4c-245">Bu önerilen Kesinleştirme kuralı nedeniyle hiçbir bozucu değişiklik bilinmiyor.</span><span class="sxs-lookup"><span data-stu-id="44e4c-245">No breaking changes are known due to this proposed disambiguation rule.</span></span>

### <a name="interesting-examples"></a><span data-ttu-id="44e4c-246">İlginç örnekler</span><span class="sxs-lookup"><span data-stu-id="44e4c-246">Interesting examples</span></span>

<span data-ttu-id="44e4c-247">Bu Kesinleştirme kurallarının ilginç sonuçları aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="44e4c-247">Here are some interesting results of these disambiguation rules:</span></span>

<span data-ttu-id="44e4c-248">`(A < B, C > D)` ifade, her bir karşılaştırma olmak üzere iki öğe içeren bir tanımlama grubu.</span><span class="sxs-lookup"><span data-stu-id="44e4c-248">The expression `(A < B, C > D)` is a tuple with two elements, each a comparison.</span></span>

<span data-ttu-id="44e4c-249">İfade `(A<B,C> D, E)`, ilki bir bildirim ifadesi olan iki öğesi olan bir tanımlama grubu olur.</span><span class="sxs-lookup"><span data-stu-id="44e4c-249">The expression `(A<B,C> D, E)` is a tuple with two elements, the first of which is a declaration expression.</span></span>

<span data-ttu-id="44e4c-250">Çağırma `M(A < B, C > D, E)` üç bağımsız değişkene sahiptir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-250">The invocation `M(A < B, C > D, E)` has three arguments.</span></span>

<span data-ttu-id="44e4c-251">Çağırma `M(out A<B,C> D, E)`, ilki bir `out` bildirimi olan iki bağımsız değişkene sahiptir.</span><span class="sxs-lookup"><span data-stu-id="44e4c-251">The invocation `M(out A<B,C> D, E)` has two arguments, the first of which is an `out` declaration.</span></span>

<span data-ttu-id="44e4c-252">`e is A<B> C` ifadesi bir bildirim ifadesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-252">The expression `e is A<B> C` uses a declaration expression.</span></span>

<span data-ttu-id="44e4c-253">Case etiketi `case A<B> C:` bir bildirim ifadesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="44e4c-253">The case label `case A<B> C:` uses a declaration expression.</span></span>

## <a name="some-examples-of-pattern-matching"></a><span data-ttu-id="44e4c-254">Bazı kalıp eşleme örnekleri</span><span class="sxs-lookup"><span data-stu-id="44e4c-254">Some examples of pattern matching</span></span>

### <a name="is-as"></a><span data-ttu-id="44e4c-255">Farklı</span><span class="sxs-lookup"><span data-stu-id="44e4c-255">Is-As</span></span>

<span data-ttu-id="44e4c-256">Deyim yerini alacak</span><span class="sxs-lookup"><span data-stu-id="44e4c-256">We can replace the idiom</span></span>

```csharp
var v = expr as Type;   
if (v != null) {
    // code using v
}
```

<span data-ttu-id="44e4c-257">Biraz daha kısa ve doğrudan</span><span class="sxs-lookup"><span data-stu-id="44e4c-257">With the slightly more concise and direct</span></span>

```csharp
if (expr is Type v) {
    // code using v
}
```

### <a name="testing-nullable"></a><span data-ttu-id="44e4c-258">Null yapılabilir test</span><span class="sxs-lookup"><span data-stu-id="44e4c-258">Testing nullable</span></span>

<span data-ttu-id="44e4c-259">Deyim yerini alacak</span><span class="sxs-lookup"><span data-stu-id="44e4c-259">We can replace the idiom</span></span>

```csharp
Type? v = x?.y?.z;
if (v.HasValue) {
    var value = v.GetValueOrDefault();
    // code using value
}
```

<span data-ttu-id="44e4c-260">Biraz daha kısa ve doğrudan</span><span class="sxs-lookup"><span data-stu-id="44e4c-260">With the slightly more concise and direct</span></span>

```csharp
if (x?.y?.z is Type value) {
    // code using value
}
```

### <a name="arithmetic-simplification"></a><span data-ttu-id="44e4c-261">Aritmetik basitleştirme</span><span class="sxs-lookup"><span data-stu-id="44e4c-261">Arithmetic simplification</span></span>

<span data-ttu-id="44e4c-262">İfadeleri göstermek için özyinelemeli türler kümesi tanımladığımızda (ayrı bir teklife göre):</span><span class="sxs-lookup"><span data-stu-id="44e4c-262">Suppose we define a set of recursive types to represent expressions (per a separate proposal):</span></span>

```csharp
abstract class Expr;
class X() : Expr;
class Const(double Value) : Expr;
class Add(Expr Left, Expr Right) : Expr;
class Mult(Expr Left, Expr Right) : Expr;
class Neg(Expr Value) : Expr;
```

<span data-ttu-id="44e4c-263">Artık bir ifadenin türevini (azaltılan) hesaplamak için bir işlev tanımlayabiliriz:</span><span class="sxs-lookup"><span data-stu-id="44e4c-263">Now we can define a function to compute the (unreduced) derivative of an expression:</span></span>

```csharp
Expr Deriv(Expr e)
{
  switch (e) {
    case X(): return Const(1);
    case Const(*): return Const(0);
    case Add(var Left, var Right):
      return Add(Deriv(Left), Deriv(Right));
    case Mult(var Left, var Right):
      return Add(Mult(Deriv(Left), Right), Mult(Left, Deriv(Right)));
    case Neg(var Value):
      return Neg(Deriv(Value));
  }
}
```

<span data-ttu-id="44e4c-264">Bir ifade simplifier konumsal desenleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="44e4c-264">An expression simplifier demonstrates positional patterns:</span></span>

```csharp
Expr Simplify(Expr e)
{
  switch (e) {
    case Mult(Const(0), *): return Const(0);
    case Mult(*, Const(0)): return Const(0);
    case Mult(Const(1), var x): return Simplify(x);
    case Mult(var x, Const(1)): return Simplify(x);
    case Mult(Const(var l), Const(var r)): return Const(l*r);
    case Add(Const(0), var x): return Simplify(x);
    case Add(var x, Const(0)): return Simplify(x);
    case Add(Const(var l), Const(var r)): return Const(l+r);
    case Neg(Const(var k)): return Const(-k);
    default: return e;
  }
}
```
