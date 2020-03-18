---
ms.openlocfilehash: fad1425e2871f395a2eb1f39faccbc773d88d6a3
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2019
ms.locfileid: "79485065"
---
# <a name="recursive-pattern-matching"></a><span data-ttu-id="82faa-101">Özyinelemeli desenli eşleştirme</span><span class="sxs-lookup"><span data-stu-id="82faa-101">Recursive Pattern Matching</span></span>

## <a name="summary"></a><span data-ttu-id="82faa-102">Özet</span><span class="sxs-lookup"><span data-stu-id="82faa-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="82faa-103">İçin C# model eşleştirme uzantıları, işlev dillerinden çok sayıda avantajı ve işlevsel dillerin örüntüsünün eşleşmesini sağlayan, ancak temel alınan dilin hisleriyle sorunsuz bir şekilde tümleşen birçok avantaj sağlayan bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="82faa-103">Pattern matching extensions for C# enable many of the benefits of algebraic data types and pattern matching from functional languages, but in a way that smoothly integrates with the feel of the underlying language.</span></span> <span data-ttu-id="82faa-104">Bu yaklaşımın öğeleri, programlama dillerinde [F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "Hafif bir dil aracılığıyla Genişletilebilir kalıp eşleme") ve [Scala](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "Desenler, sayfa 273 Ile eşleşen nesneler")'daki ilgili özelliklerle ilham.</span><span class="sxs-lookup"><span data-stu-id="82faa-104">Elements of this approach are inspired by related features in the programming languages [F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "Extensible Pattern Matching Via a Lightweight Language") and [Scala](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "Matching Objects With Patterns, page 273").</span></span>

## <a name="detailed-design"></a><span data-ttu-id="82faa-105">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="82faa-105">Detailed design</span></span>
[design]: #detailed-design

### <a name="is-expression"></a><span data-ttu-id="82faa-106">Ifade Ifadesi</span><span class="sxs-lookup"><span data-stu-id="82faa-106">Is Expression</span></span>

<span data-ttu-id="82faa-107">`is` işleci, bir ifadeyi bir *düzene*karşı test etmek için genişletilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-107">The `is` operator is extended to test an expression against a *pattern*.</span></span>

```antlr
relational_expression
    : is_pattern_expression
    ;
is_pattern_expression
    : relational_expression 'is' pattern
    ;
```

<span data-ttu-id="82faa-108">Bu *relational_expression* biçimi, C# belirtideki mevcut formlara ek niteliğindedir.</span><span class="sxs-lookup"><span data-stu-id="82faa-108">This form of *relational_expression* is in addition to the existing forms in the C# specification.</span></span> <span data-ttu-id="82faa-109">`is` belirtecinin solundaki *relational_expression* bir değer belirlememez veya bir tür yoksa bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="82faa-109">It is a compile-time error if the *relational_expression* to the left of the `is` token does not designate a value or does not have a type.</span></span>

<span data-ttu-id="82faa-110">Düzenin her *tanımlayıcısı* , `is` işleci `true` sonra *kesinlikle atanan* yeni bir yerel değişken sunar (yani, *doğru olduğunda, kesin olarak atanır*).</span><span class="sxs-lookup"><span data-stu-id="82faa-110">Every *identifier* of the pattern introduces a new local variable that is *definitely assigned* after the `is` operator is `true` (i.e. *definitely assigned when true*).</span></span>

> <span data-ttu-id="82faa-111">Note: `is-expression` ve *constant_pattern* *türü* arasında teknik bir belirsizlik vardır; bunlardan biri, tam bir tanımlayıcının geçerli bir ayrıştırmasından kaynaklanabilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-111">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="82faa-112">Bunu, dilin önceki sürümleriyle uyumluluk için bir tür olarak bağlamaya çalışırız; yalnızca bu başarısız olursa, diğer bağlamlarda bir ifade yaptığımız (bir sabit veya bir tür olması gerekir) bunu çözeceğiz.</span><span class="sxs-lookup"><span data-stu-id="82faa-112">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do an expression in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="82faa-113">Bu belirsizlik yalnızca `is` ifadesinin sağ tarafında bulunur.</span><span class="sxs-lookup"><span data-stu-id="82faa-113">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

### <a name="patterns"></a><span data-ttu-id="82faa-114">Desenler</span><span class="sxs-lookup"><span data-stu-id="82faa-114">Patterns</span></span>

<span data-ttu-id="82faa-115">Desenler *is_pattern* işlecinde, bir *switch_statement*ve bir *switch_expression* içinde, gelen verilerin (giriş değerini çağırdığımız) veri şeklinin karşılaştırılacağı bir şekilde ifade etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="82faa-115">Patterns are used in the *is_pattern* operator, in a *switch_statement*, and in a *switch_expression* to express the shape of data against which incoming data  (which we call the input value) is to be compared.</span></span> <span data-ttu-id="82faa-116">Desenler, verilerin bölümlerinin alt desenlerle eşleştirileceği şekilde özyinelemeli olabilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-116">Patterns may be recursive so that parts of the data may be matched against sub-patterns.</span></span>

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    | positional_pattern
    | property_pattern
    | discard_pattern
    ;
declaration_pattern
    : type simple_designation
    ;
constant_pattern
    : expression
    ;
var_pattern
    : 'var' designation
    ;
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
property_subpattern
    : '{' subpatterns? '}'
    ;
property_pattern
    : type? property_subpattern simple_designation?
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
discard_pattern
    : '_'
    ;
```

#### <a name="declaration-pattern"></a><span data-ttu-id="82faa-117">Bildirim kalıbı</span><span class="sxs-lookup"><span data-stu-id="82faa-117">Declaration Pattern</span></span>

```antlr
declaration_pattern
    : type simple_designation
    ;
```

<span data-ttu-id="82faa-118">*Declaration_pattern* her ikisi de bir ifadenin verilen türden olduğunu sınar ve test başarılı olursa bu türe yayınlar.</span><span class="sxs-lookup"><span data-stu-id="82faa-118">The *declaration_pattern* both tests that an expression is of a given type and casts it to that type if the test succeeds.</span></span> <span data-ttu-id="82faa-119">Bu, atama bir *single_variable_designation*ise verilen tanımlayıcı tarafından verilen türün yerel bir değişkenini ortaya çıkarabilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-119">This may introduce a local variable of the given type named by the given identifier, if the designation is a *single_variable_designation*.</span></span> <span data-ttu-id="82faa-120">Bu yerel değişken, bir kalıp eşleme işleminin sonucu `true`, *kesinlikle atanır* .</span><span class="sxs-lookup"><span data-stu-id="82faa-120">That local variable is *definitely assigned* when the result of the pattern-matching operation is `true`.</span></span>

<span data-ttu-id="82faa-121">Bu ifadenin çalışma zamanı anlam düzeyi, sol taraftaki *relational_expression* işlenenin çalışma zamanı türünü, düzendeki *türe* göre sınar.</span><span class="sxs-lookup"><span data-stu-id="82faa-121">The runtime semantic of this expression is that it tests the runtime type of the left-hand *relational_expression* operand against the *type* in the pattern.</span></span>  <span data-ttu-id="82faa-122">Çalışma zamanı türü (veya bazı alt tür) ise, `null``is operator` sonucu `true`.</span><span class="sxs-lookup"><span data-stu-id="82faa-122">If it is of that runtime type (or some subtype) and not `null`, the result of the `is operator` is `true`.</span></span>

<span data-ttu-id="82faa-123">Sol taraftaki statik türdeki belirli birleşimler ve verilen tür uyumsuz olarak değerlendirilir ve derleme zamanı hatası ile sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="82faa-123">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="82faa-124">`E` statik tür değeri, bir kimlik dönüştürmesi, örtük bir başvuru dönüştürmesi, paketleme dönüştürmesi, açık bir başvuru dönüştürmesi veya `E` 'den `T`'ye bir paket oluşturma dönüştürmesi ya da bu türlerden biri bir açık tür ise, bir tür `T` bir tür ile *uyumlu* olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-124">A value of static type `E` is said to be *pattern-compatible* with a type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`, or if one of those types is an open type.</span></span> <span data-ttu-id="82faa-125">`E` türünde bir giriş ile eşleştirildiği tür deseninin *türüyle* *uyumlu* değilse, derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="82faa-125">It is a compile-time error if an input of type `E` is not *pattern-compatible* with the *type* in a type pattern that it is matched with.</span></span>

<span data-ttu-id="82faa-126">Tür stili, başvuru türlerinin çalışma zamanı türü testlerini gerçekleştirmek için yararlıdır ve deyim yerini alır</span><span class="sxs-lookup"><span data-stu-id="82faa-126">The type pattern is useful for performing run-time type tests of reference types, and replaces the idiom</span></span>

```csharp
var v = expr as Type;
if (v != null) { // code using v
```

<span data-ttu-id="82faa-127">Biraz daha kısa</span><span class="sxs-lookup"><span data-stu-id="82faa-127">With the slightly more concise</span></span>

```csharp
if (expr is Type v) { // code using v
```

<span data-ttu-id="82faa-128">*Tür* null yapılabilir bir değer türünde ise bu bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="82faa-128">It is an error if *type* is a nullable value type.</span></span>

<span data-ttu-id="82faa-129">Tür stili, null yapılabilir türlerin değerlerini test etmek için kullanılabilir: `Nullable<T>` (veya kutulanmış `T`) bir değer, bir tür düzeniyle eşleşir `T2 id` değer null değilse ve `T2` türü `T`veya bazı temel tür veya `T`arabirimi.</span><span class="sxs-lookup"><span data-stu-id="82faa-129">The type pattern can be used to test values of nullable types: a value of type `Nullable<T>` (or a boxed `T`) matches a type pattern `T2 id` if the value is non-null and the type of `T2` is `T`, or some base type or interface of `T`.</span></span> <span data-ttu-id="82faa-130">Örneğin, kod parçasında</span><span class="sxs-lookup"><span data-stu-id="82faa-130">For example, in the code fragment</span></span>

```csharp
int? x = 3;
if (x is int v) { // code using v
```

<span data-ttu-id="82faa-131">`if` deyimin koşulu çalışma zamanında `true` ve `v` değişkeni, blok içindeki `int` türündeki `3` değeri tutar.</span><span class="sxs-lookup"><span data-stu-id="82faa-131">The condition of the `if` statement is `true` at runtime and the variable `v` holds the value `3` of type `int` inside the block.</span></span> <span data-ttu-id="82faa-132">Blok `v`, değişken kapsam içinde ve kesin olarak atanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="82faa-132">After the block the variable `v` is in scope but not definitely assigned.</span></span>

#### <a name="constant-pattern"></a><span data-ttu-id="82faa-133">Sabit model</span><span class="sxs-lookup"><span data-stu-id="82faa-133">Constant Pattern</span></span>

```antlr
constant_pattern
    : constant_expression
    ;
```

<span data-ttu-id="82faa-134">Sabit bir model, bir ifadenin değerini sabit bir değere göre sınar.</span><span class="sxs-lookup"><span data-stu-id="82faa-134">A constant pattern tests the value of an expression against a constant value.</span></span> <span data-ttu-id="82faa-135">Sabit, değişmez değer, belirtilen bir `const` değişkeninin adı veya bir numaralandırma sabiti gibi herhangi bir sabit ifade olabilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-135">The constant may be any constant expression, such as a literal, the name of a declared `const` variable, or an enumeration constant.</span></span> <span data-ttu-id="82faa-136">Giriş değeri açık bir tür olmadığında, sabit ifade örtük olarak eşleşen ifadenin türüne dönüştürülür; giriş değerinin türü, sabit ifadenin türüyle *uyumlu* değilse, kalıp eşleme işlemi bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="82faa-136">When the input value is not an open type, the constant expression is implicitly converted to the type of the matched expression; if the type of the input value is not *pattern-compatible* with the type of the constant expression, the pattern-matching operation is an error.</span></span>

<span data-ttu-id="82faa-137">`object.Equals(c, e)`, *`true`* döndürecaksa, dönüştürülmüş giriş değeri *e* ile eşleşen olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-137">The pattern *c* is considered matching the converted input value *e* if `object.Equals(c, e)` would return `true`.</span></span>

<span data-ttu-id="82faa-138">Kullanıcı tanımlı bir `operator==`çağıracağından, yeni yazılmış koddaki `null` test etmenin en yaygın yolu olarak `e is null` görmeniz beklenir.</span><span class="sxs-lookup"><span data-stu-id="82faa-138">We expect to see `e is null` as the most common way to test for `null` in newly written code, as it cannot invoke a user-defined `operator==`.</span></span>

#### <a name="var-pattern"></a><span data-ttu-id="82faa-139">Var deseninin</span><span class="sxs-lookup"><span data-stu-id="82faa-139">Var Pattern</span></span>

```antlr
var_pattern
    : 'var' designation
    ;
designation
    : simple_designation
    | tuple_designation
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
single_variable_designation
    : identifier
    ;
discard_designation
    : _
    ;
tuple_designation
    : '(' designations? ')'
    ;
designations
    : designation
    | designations ',' designation
    ;
```

<span data-ttu-id="82faa-140">Eğer *atama* bir *simple_designation*ise, *e* ifadesi bir ifade ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="82faa-140">If the *designation* is a *simple_designation*, an expression *e* matches the pattern.</span></span> <span data-ttu-id="82faa-141">Diğer bir deyişle, bir *var düzeniyle* eşleşme *simple_designation*her zaman başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="82faa-141">In other words, a match to a *var pattern* always succeeds with a *simple_designation*.</span></span> <span data-ttu-id="82faa-142">*Simple_designation* bir *single_variable_designation*ise, *e* değeri yeni tanıtılan yerel değişkene göre sınırlar.</span><span class="sxs-lookup"><span data-stu-id="82faa-142">If the *simple_designation* is a *single_variable_designation*, the value of *e* is bounds to a newly introduced local variable.</span></span> <span data-ttu-id="82faa-143">Yerel değişkenin türü, *e*'nin statik türüdür.</span><span class="sxs-lookup"><span data-stu-id="82faa-143">The type of the local variable is the static type of *e*.</span></span>

<span data-ttu-id="82faa-144">Eğer *atama* bir *tuple_designation*ise, bu, *positional_pattern* , `(var` *atama*,... `)`, *tuple_designation*içinde bulunan *belirtimlerin*bulunduğu eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="82faa-144">If the *designation* is a *tuple_designation*, then the pattern is equivalent to a *positional_pattern* of the form `(var` *designation*, ... `)` where the *designation*s are those found within the *tuple_designation*.</span></span>  <span data-ttu-id="82faa-145">Örneğin, `var (x, (y, z))` `(var x, (var y, var z))`eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="82faa-145">For example, the pattern `var (x, (y, z))` is equivalent to `(var x, (var y, var z))`.</span></span>

<span data-ttu-id="82faa-146">Ad `var` bir türe bağlandığında bu bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="82faa-146">It is an error if the name `var` binds to a type.</span></span>

#### <a name="discard-pattern"></a><span data-ttu-id="82faa-147">Stili at</span><span class="sxs-lookup"><span data-stu-id="82faa-147">Discard Pattern</span></span>

```antlr
discard_pattern
    : '_'
    ;
```

<span data-ttu-id="82faa-148">Her zaman `_` bir ifade *e-* posta ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="82faa-148">An expression *e* matches the pattern `_` always.</span></span> <span data-ttu-id="82faa-149">Diğer bir deyişle, her ifade atma düzeniyle eşleşir.</span><span class="sxs-lookup"><span data-stu-id="82faa-149">In other words, every expression matches the discard pattern.</span></span>

<span data-ttu-id="82faa-150">Bir atma deseninin bir *is_pattern_expression*kalıbı olarak kullanılması kullanılamayabilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-150">A discard pattern may not be used as the pattern of an *is_pattern_expression*.</span></span>

#### <a name="positional-pattern"></a><span data-ttu-id="82faa-151">Konumsal model</span><span class="sxs-lookup"><span data-stu-id="82faa-151">Positional Pattern</span></span>

<span data-ttu-id="82faa-152">Konumsal bir model, giriş değerinin `null`olmadığını denetler, uygun bir `Deconstruct` yöntemini çağırır ve elde edilen değerlerde daha fazla model eşleştirmeyi gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="82faa-152">A positional pattern checks that the input value is not `null`, invokes an appropriate `Deconstruct` method, and performs further pattern matching on the resulting values.</span></span>  <span data-ttu-id="82faa-153">Ayrıca, giriş değerinin türü `Deconstruct`içeren türle aynı olduğunda ya da giriş değerinin türü bir tanımlama grubu türü ise ya da deyimin çalışma zamanı türü `ITuple`uygularsa `ITuple` `object`, demet benzeri bir model söz dizimini (Bu tür olmadan) destekler.</span><span class="sxs-lookup"><span data-stu-id="82faa-153">It also supports a tuple-like pattern syntax (without the type being provided) when the type of the input value is the same as the type containing `Deconstruct`, or if the type of the input value is a tuple type, or if the type of the input value is `object` or `ITuple` and the runtime type of the expression implements `ITuple`.</span></span>

```antlr
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
```

<span data-ttu-id="82faa-154">*Tür* atlanırsa, bu değeri giriş değerinin statik türü olacak şekilde sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="82faa-154">If the *type* is omitted, we take it to be the static type of the input value.</span></span>

<span data-ttu-id="82faa-155">`(` *subpattern_list* `)`bir giriş değeriyle eşleşme verildiğinde, `Deconstruct` *type* erişilebilir bildirimler için *tür* arayarak ve ayrıştırma bildirimiyle aynı kurallara göre seçim yaparak bir yöntem seçilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-155">Given a match of an input value to the pattern *type* `(` *subpattern_list* `)`, a method is selected by searching in *type* for accessible declarations of `Deconstruct` and selecting one among them using the same rules as for the deconstruction declaration.</span></span>

<span data-ttu-id="82faa-156">*Positional_pattern* türü atladığında bir hatadır, *tanımlayıcı*olmadan tek bir *alt model* varsa, *property_subpattern* yoktur ve *simple_designation*içermez.</span><span class="sxs-lookup"><span data-stu-id="82faa-156">It is an error if a *positional_pattern* omits the type, has a single *subpattern* without an *identifier*, has no *property_subpattern* and has no *simple_designation*.</span></span> <span data-ttu-id="82faa-157">Bu, parantez içine alınmış bir *constant_pattern* ve bir *positional_pattern*arasında ayırt edilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-157">This disambiguates between a *constant_pattern* that is parenthesized and a *positional_pattern*.</span></span>

<span data-ttu-id="82faa-158">Listedeki desenlerle eşleşecek değerleri ayıklamak için</span><span class="sxs-lookup"><span data-stu-id="82faa-158">In order to extract the values to match against the patterns in the list,</span></span>
- <span data-ttu-id="82faa-159">*Tür* atlanmışsa ve giriş değerinin türü bir demet türü ise, alt desenlerin sayısının, kayıt düzeninin kardinalitesiyle aynı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="82faa-159">If *type* was omitted and the input value's type is a tuple type, then the number of subpatterns is required to be the same as the cardinality of the tuple.</span></span> <span data-ttu-id="82faa-160">Her demet öğesi karşılık gelen *alt düzen*ile eşleştirilir ve bu başarılı olursa eşleşme başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="82faa-160">Each tuple element is matched against the corresponding *subpattern*, and the match succeeds if all of these succeed.</span></span> <span data-ttu-id="82faa-161">Herhangi bir *alt düzenin* bir *tanımlayıcısı*varsa, bu, demet türünde ilgili konumda bir demet öğesi adı vermelidir.</span><span class="sxs-lookup"><span data-stu-id="82faa-161">If any *subpattern* has an *identifier*, then that must name a tuple element at the corresponding position in the tuple type.</span></span>
- <span data-ttu-id="82faa-162">Aksi takdirde, uygun bir `Deconstruct` *türü*bir üye olarak mevcutsa, giriş *değerinin türü tür ile* *uyumlu* değilse, derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="82faa-162">Otherwise, if a suitable `Deconstruct` exists as a member of *type*, it is a compile-time error if the type of the input value is not *pattern-compatible* with *type*.</span></span> <span data-ttu-id="82faa-163">Çalışma zamanında, giriş değeri *türe*göre test edilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-163">At runtime the input value is tested against *type*.</span></span> <span data-ttu-id="82faa-164">Bu başarısız olursa konumsal model eşleşmesi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="82faa-164">If this fails then the positional pattern match fails.</span></span> <span data-ttu-id="82faa-165">Başarılı olursa, giriş değeri bu türe dönüştürülür ve `Deconstruct` `out` parametrelerini almak için derleyici tarafından oluşturulan yeni değişkenlerle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="82faa-165">If it succeeds,  the input value is converted to this type and `Deconstruct` is invoked with fresh compiler-generated variables to receive the `out` parameters.</span></span> <span data-ttu-id="82faa-166">Alınan her bir değer, karşılık gelen *alt düzen*ile eşleştirilir ve bu başarılı olursa eşleşme başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="82faa-166">Each value that was received is matched against the corresponding *subpattern*, and the match succeeds if all of these succeed.</span></span> <span data-ttu-id="82faa-167">Herhangi bir *alt düzenin* bir *tanımlayıcısı*varsa, bu, `Deconstruct`karşılık gelen konumunda bir parametre adı vermelidir.</span><span class="sxs-lookup"><span data-stu-id="82faa-167">If any *subpattern* has an *identifier*, then that must name a parameter at the corresponding position of `Deconstruct`.</span></span>
- <span data-ttu-id="82faa-168">Aksi takdirde, *tür* atlanmışsa ve giriş değeri `object` veya `ITuple` ya da örtük bir başvuru dönüştürmesi tarafından `ITuple` dönüştürülebildiğiniz bir tür veya alt desenler arasında hiçbir *tanımlayıcı* olmazsa, `ITuple`kullanarak eşleşeceğiz.</span><span class="sxs-lookup"><span data-stu-id="82faa-168">Otherwise if *type* was omitted, and the input value is of type `object` or `ITuple` or some type that can be converted to `ITuple` by an implicit reference conversion, and no *identifier* appears among the subpatterns, then we match using `ITuple`.</span></span>
- <span data-ttu-id="82faa-169">Aksi halde, model derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="82faa-169">Otherwise the pattern is a compile-time error.</span></span>

<span data-ttu-id="82faa-170">Çalışma zamanında alt desenleri eşleştirildiği sıra belirtilmemiş ve başarısız eşleşme tüm alt desenlerle eşleştirmeye çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-170">The order in which subpatterns are matched at runtime is unspecified, and a failed match may not attempt to match all subpatterns.</span></span>

##### <a name="example"></a><span data-ttu-id="82faa-171">Örnek</span><span class="sxs-lookup"><span data-stu-id="82faa-171">Example</span></span>

<span data-ttu-id="82faa-172">Bu örnek, bu belirtimde açıklanan birçok özelliği kullanır</span><span class="sxs-lookup"><span data-stu-id="82faa-172">This example uses many of the features described in this specification</span></span>

``` c#
    var newState = (GetState(), action, hasKey) switch {
        (DoorState.Closed, Action.Open, _) => DoorState.Opened,
        (DoorState.Opened, Action.Close, _) => DoorState.Closed,
        (DoorState.Closed, Action.Lock, true) => DoorState.Locked,
        (DoorState.Locked, Action.Unlock, true) => DoorState.Closed,
        (var state, _, _) => state };
```

#### <a name="property-pattern"></a><span data-ttu-id="82faa-173">Özellik kalıbı</span><span class="sxs-lookup"><span data-stu-id="82faa-173">Property Pattern</span></span>

<span data-ttu-id="82faa-174">Bir özellik deseninin giriş değerinin `null` olmadığını ve yinelemeli olarak erişilebilir özellikler veya alanlar kullanılarak ayıklanan değerlerle eşleştiğini kontrol eder.</span><span class="sxs-lookup"><span data-stu-id="82faa-174">A property pattern checks that the input value is not `null` and recursively matches values extracted by the use of accessible properties or fields.</span></span>

```antlr
property_pattern
    : type? property_subpattern simple_designation?
    ;
property_subpattern
    : '{' '}'
    | '{' subpatterns ','? '}'
    ;
```

<span data-ttu-id="82faa-175">Bir _property_pattern_ _alt deseninin_ bir _tanımlayıcı_ içermemesi (bir _tanımlayıcıya_sahip ikinci biçimde olması gerekir) hatadır.</span><span class="sxs-lookup"><span data-stu-id="82faa-175">It is an error if any _subpattern_ of a _property_pattern_ does not contain an _identifier_ (it must be of the second form, which has an _identifier_).</span></span>  <span data-ttu-id="82faa-176">Son alt örüntüden sonra sondaki virgül isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="82faa-176">A trailing comma after the last subpattern is optional.</span></span>

<span data-ttu-id="82faa-177">Null denetim deseninin bir önemsiz Özellik deseninin dışına düştüğünü unutmayın.</span><span class="sxs-lookup"><span data-stu-id="82faa-177">Note that a null-checking pattern falls out of a trivial property pattern.</span></span> <span data-ttu-id="82faa-178">`s` dizenin null olmadığını denetlemek için aşağıdaki formlardan birini yazabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="82faa-178">To check if the string `s` is non-null, you can write any of the following forms</span></span>

```csharp
if (s is object o) ... // o is of type object
if (s is string x) ... // x is of type string
if (s is {} x) ... // x is of type string
if (s is {}) ...
```

<span data-ttu-id="82faa-179">`{` *property_pattern_list* `}`düzeniyle *bir ifade ile* eşleşme verildiğinde, *type* *e* ifadesi *tür*tarafından belirlenen *t* türü ile *kalıp uyumlu* değilse, derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="82faa-179">Given a match of an expression *e* to the pattern *type* `{` *property_pattern_list* `}`, it is a compile-time error if the expression *e* is not *pattern-compatible* with the type *T* designated by *type*.</span></span> <span data-ttu-id="82faa-180">Tür yoksa, bu, *e*'nin statik türü olarak ele alır.</span><span class="sxs-lookup"><span data-stu-id="82faa-180">If the type is absent, we take it to be the static type of *e*.</span></span> <span data-ttu-id="82faa-181">*Tanımlayıcı* varsa *, tür türünde*bir model değişkeni bildirir.</span><span class="sxs-lookup"><span data-stu-id="82faa-181">If the *identifier* is present, it declares a pattern variable of type *type*.</span></span> <span data-ttu-id="82faa-182">*Property_pattern_list* sol tarafında görünen tanımlayıcıların her biri, erişilebilir okunabilir bir özellik veya *alan olarak belirtmelidir.* *Property_pattern* *simple_designation* varsa *T*türünde bir model değişkeni tanımlar.</span><span class="sxs-lookup"><span data-stu-id="82faa-182">Each of the identifiers appearing on the left-hand-side of its *property_pattern_list* must designate an accessible readable property or field of *T*. If the *simple_designation* of the *property_pattern* is present, it defines a pattern variable of type *T*.</span></span>

<span data-ttu-id="82faa-183">Çalışma zamanında, ifade *T*'ye göre test edilir. Bu başarısız olursa, özellik deseninin eşleşmesi başarısız olur ve sonuç `false`.</span><span class="sxs-lookup"><span data-stu-id="82faa-183">At runtime, the expression is tested against *T*. If this fails then the property pattern match fails and the result is `false`.</span></span> <span data-ttu-id="82faa-184">Başarılı olursa, her bir *property_subpattern* alanı veya özelliği okunurdur ve değeri karşılık gelen düzeniyle eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-184">If it succeeds, then each *property_subpattern* field or property is read and its value matched against its corresponding pattern.</span></span> <span data-ttu-id="82faa-185">Tam eşleşmenin sonucu yalnızca, bunlardan herhangi birinin sonucu `false``false`.</span><span class="sxs-lookup"><span data-stu-id="82faa-185">The result of the whole match is `false` only if the result of any of these is `false`.</span></span> <span data-ttu-id="82faa-186">Alt desenlerin eşleştirildiği sıra belirtilmemiş ve başarısız eşleşme çalışma zamanında tüm alt desenlerle eşleşmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-186">The order in which subpatterns are matched is not specified, and a failed match may not match all subpatterns at runtime.</span></span> <span data-ttu-id="82faa-187">Eşleşme başarılı olursa ve *property_pattern* *simple_designation* bir *single_variable_designation*ise, eşleşen değere atanmış *T* türünde bir değişkeni tanımlar.</span><span class="sxs-lookup"><span data-stu-id="82faa-187">If the match succeeds and the *simple_designation* of the *property_pattern* is a *single_variable_designation*, it defines a variable of type *T* that is assigned the matched value.</span></span>

> <span data-ttu-id="82faa-188">Note: Özellik deseninin, anonim türlerle eşleşmesi için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-188">Note: The property pattern can be used to pattern-match with anonymous types.</span></span>

##### <a name="example"></a><span data-ttu-id="82faa-189">Örnek</span><span class="sxs-lookup"><span data-stu-id="82faa-189">Example</span></span>

```csharp
if (o is string { Length: 5 } s)
```

### <a name="switch-expression"></a><span data-ttu-id="82faa-190">Anahtar Ifadesi</span><span class="sxs-lookup"><span data-stu-id="82faa-190">Switch Expression</span></span>

<span data-ttu-id="82faa-191">Bir ifade bağlamı için `switch`benzeri semantiğini desteklemek üzere bir *switch_expression* eklenir.</span><span class="sxs-lookup"><span data-stu-id="82faa-191">A *switch_expression* is added to support `switch`-like semantics for an expression context.</span></span>

<span data-ttu-id="82faa-192">C# Dil sözdizimi, aşağıdaki sözdizimsel üretimlerle birlikte artırmadır:</span><span class="sxs-lookup"><span data-stu-id="82faa-192">The C# language syntax is augmented with the following syntactic productions:</span></span>

```antlr
multiplicative_expression
    : switch_expression
    | multiplicative_expression '*' switch_expression
    | multiplicative_expression '/' switch_expression
    | multiplicative_expression '%' switch_expression
    ;
switch_expression
    : range_expression 'switch' '{' '}'
    | range_expression 'switch' '{' switch_expression_arms ','? '}'
    ;
switch_expression_arms
    : switch_expression_arm
    | switch_expression_arms ',' switch_expression_arm
    ;
switch_expression_arm
    : pattern case_guard? '=>' expression
    ;
case_guard
    : 'when' null_coalescing_expression
    ;
```

<span data-ttu-id="82faa-193">*Switch_expression* *expression_statement*olarak izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="82faa-193">The *switch_expression* is not permitted as an *expression_statement*.</span></span>

> <span data-ttu-id="82faa-194">Bu, gelecekteki bir düzeltmeye yönelik olarak size bakıyor.</span><span class="sxs-lookup"><span data-stu-id="82faa-194">We are looking at relaxing this in a future revision.</span></span>

<span data-ttu-id="82faa-195">*Switch_expression* türü, böyle bir tür varsa ve anahtar ifadesinin her ARM içindeki ifade örtülü olarak bu türe dönüştürülebileceğinden, *switch_expression_arm*s `=>` belirteçlerinin sağında görünen ifadelerin [*en iyi ortak türüdür*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) .</span><span class="sxs-lookup"><span data-stu-id="82faa-195">The type of the *switch_expression* is the [*best common type*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) of the expressions appearing to the right of the `=>` tokens of the *switch_expression_arm*s if such a type exists and the expression in every arm of the switch expression can be implicitly converted to that type.</span></span>  <span data-ttu-id="82faa-196">Ayrıca, her bir ARM ifadesinin `T`için örtük bir dönüştürme var `T` her türe bir anahtar ifadesinden önceden tanımlanmış bir örtük dönüştürme olan yeni bir *anahtar ifadesi dönüştürmesi*ekledik.</span><span class="sxs-lookup"><span data-stu-id="82faa-196">In addition, we add a new *switch expression conversion*, which is a predefined implicit conversion from a switch expression to every type `T` for which there exists an implicit conversion from each arm's expression to `T`.</span></span>

<span data-ttu-id="82faa-197">Bazı önceki desenler ve korumada her zaman eşleşeceğinden, bazı *switch_expression_arm*deseninin sonucu etkilemediği bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="82faa-197">It is an error if some *switch_expression_arm*'s pattern cannot affect the result because some previous pattern and guard will always match.</span></span>

<span data-ttu-id="82faa-198">Anahtar ifadesinin bir kolu, girişinin her değerini *işlediğinde, anahtar* ifadesi tam olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-198">A switch expression is said to be *exhaustive* if some arm of the switch expression handles every value of its input.</span></span>  <span data-ttu-id="82faa-199">Anahtar ifadesi *ayrıntılı*değilse, derleyici bir uyarı üretir.</span><span class="sxs-lookup"><span data-stu-id="82faa-199">The compiler shall produce a warning if a switch expression is not *exhaustive*.</span></span>

<span data-ttu-id="82faa-200">Çalışma zamanında, *switch_expression* sonucu, *switch_expression* sol tarafındaki ifadenin *switch_expression_arm*düzeniyle eşleştiği ve *case_guard, varsa* switch_expression_arm sonucunu veren ilk *switch_expression_arm* *ifadesinin* değeridir. Bu, `true`olarak değerlendiriliyorsa *.*</span><span class="sxs-lookup"><span data-stu-id="82faa-200">At runtime, the result of the *switch_expression* is the value of the *expression* of the first *switch_expression_arm* for which the expression on the left-hand-side of the *switch_expression* matches the *switch_expression_arm*'s pattern, and for which the *case_guard* of the *switch_expression_arm*, if present, evaluates to `true`.</span></span> <span data-ttu-id="82faa-201">Böyle bir *switch_expression_arm*yoksa, *switch_expression* özel durum `System.Runtime.CompilerServices.SwitchExpressionException`örneği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="82faa-201">If there is no such *switch_expression_arm*, the *switch_expression* throws an instance of the exception `System.Runtime.CompilerServices.SwitchExpressionException`.</span></span>

### <a name="optional-parens-when-switching-on-a-tuple-literal"></a><span data-ttu-id="82faa-202">Bir demet sabit değerinde geçiş yaparken isteğe bağlı paranlar</span><span class="sxs-lookup"><span data-stu-id="82faa-202">Optional parens when switching on a tuple literal</span></span>

<span data-ttu-id="82faa-203">*Switch_statement*kullanarak bir demet hazır bilgisine geçiş yapmak için, gereksiz parmalar gibi görünen öğeleri yazmanız gerekir</span><span class="sxs-lookup"><span data-stu-id="82faa-203">In order to switch on a tuple literal using the *switch_statement*, you have to write what appear to be redundant parens</span></span>

```csharp
switch ((a, b))
{
```

<span data-ttu-id="82faa-204">İzin vermek için</span><span class="sxs-lookup"><span data-stu-id="82faa-204">To permit</span></span>

```csharp
switch (a, b)
{
```

<span data-ttu-id="82faa-205">switch ifadesinin parantezleri, geçiş yapılan ifade bir demet sabit değeri olduğunda isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="82faa-205">the parentheses of the switch statement are optional when the expression being switched on is a tuple literal.</span></span>

### <a name="order-of-evaluation-in-pattern-matching"></a><span data-ttu-id="82faa-206">Düzen eşleme içinde değerlendirme sırası</span><span class="sxs-lookup"><span data-stu-id="82faa-206">Order of evaluation in pattern-matching</span></span>

<span data-ttu-id="82faa-207">Desenler eşleme sırasında yürütülen işlemlerin yeniden sıralanması için derleme esnekliği sağlamak, desenli eşleme verimliliğini artırmak için kullanılabilecek esnekliğe izin verebilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-207">Giving the compiler flexibility in reordering the operations executed during pattern-matching can permit flexibility that can be used to improve the efficiency of pattern-matching.</span></span> <span data-ttu-id="82faa-208">(Zorlanmaz) gereksinimi, bir düzende erişilen özelliklerin ve Deyapý yöntemlerinin "saf" olması gerekir (yan etki, ıdempotent, vb.).</span><span class="sxs-lookup"><span data-stu-id="82faa-208">The (unenforced) requirement would be that properties accessed in a pattern, and the Deconstruct methods, are required to be "pure" (side-effect free, idempotent, etc).</span></span> <span data-ttu-id="82faa-209">Bu, yalnızca yeniden düzenleme işlemlerinde derleyici esnekliğine izin verdiğimiz bir dil kavramı olarak önemli bir değer ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="82faa-209">That doesn't mean that we would add purity as a language concept, only that we would allow the compiler flexibility in reordering operations.</span></span>

<span data-ttu-id="82faa-210">**Çözüm 2018-04-04 LDM**: onaylandı: derleyicinin, `ITuple``Deconstruct`, özellik erişimlerine ve yöntemlerin çağrılarına yeniden düzenleme yapmasına izin verilir ve döndürülen değerlerin birden çok çağrının aynı olduğunu varsayabilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-210">**Resolution 2018-04-04 LDM**: confirmed: the compiler is permitted to reorder calls to `Deconstruct`, property accesses, and invocations of methods in `ITuple`, and may assume that returned values are the same from multiple calls.</span></span> <span data-ttu-id="82faa-211">Derleyici, sonucu etkileyebilecek işlevleri çağırmamalıdır ve gelecekte derleyicinin ürettiği değerlendirme sırasında değişiklik yapmadan önce çok dikkatli olacak.</span><span class="sxs-lookup"><span data-stu-id="82faa-211">The compiler should not invoke functions that cannot affect the result, and we will be very careful before making any changes to the compiler-generated order of evaluation in the future.</span></span>

### <a name="some-possible-optimizations"></a><span data-ttu-id="82faa-212">Bazı olası Iyileştirmeler</span><span class="sxs-lookup"><span data-stu-id="82faa-212">Some Possible Optimizations</span></span>

<span data-ttu-id="82faa-213">Desen eşleştirme derlemesi, desenlerin yaygın parçalarından faydalanabilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-213">The compilation of pattern matching can take advantage of common parts of patterns.</span></span> <span data-ttu-id="82faa-214">Örneğin, bir *switch_statement* art arda iki desenin en üst düzey tür testi aynı türde ise, oluşturulan kod ikinci desen için tür testini atlayabilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-214">For example, if the top-level type test of two successive patterns in a *switch_statement* is the same type, the generated code can skip the type test for the second pattern.</span></span>

<span data-ttu-id="82faa-215">Desenlerin bazıları tamsayı veya dizeler olduğunda, derleyici, dilin önceki sürümlerindeki bir switch ifadesinin oluşturduğu aynı kod türünü oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="82faa-215">When some of the patterns are integers or strings, the compiler can generate the same kind of code it generates for a switch-statement in earlier versions of the language.</span></span>

<span data-ttu-id="82faa-216">Bu tür iyileştirmeler hakkında daha fazla bilgi için bkz. [[Scott ve Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "Ne zaman eşle, derleme buluşsal yöntemlerini ne zaman").</span><span class="sxs-lookup"><span data-stu-id="82faa-216">For more on these kinds of optimizations, see [[Scott and Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "When Do Match-Compilation Heuristics Matter?").</span></span>
