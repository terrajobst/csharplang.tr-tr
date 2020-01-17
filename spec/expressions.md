---
ms.openlocfilehash: f61039abd6bd557ac0ea625e6aac1c8bafa57b02
ms.sourcegitcommit: e134bb7058e9848120b93b345f96d6ac0cb8c815
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "71704082"
---
# <a name="expressions"></a><span data-ttu-id="2886a-101">{1&gt;İfadeler&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-101">Expressions</span></span>

<span data-ttu-id="2886a-102">İfade, işleç ve işlenen dizisidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-102">An expression is a sequence of operators and operands.</span></span> <span data-ttu-id="2886a-103">Bu bölümde sözdizimi, işlenenler ve işleçler değerlendirmesi sırası ve ifadelerin anlamı tanımlanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="2886a-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

## <a name="expression-classifications"></a><span data-ttu-id="2886a-104">İfade sınıflandırmaları</span><span class="sxs-lookup"><span data-stu-id="2886a-104">Expression classifications</span></span>

<span data-ttu-id="2886a-105">Bir ifade, aşağıdakilerden biri olarak sınıflandırıldı:</span><span class="sxs-lookup"><span data-stu-id="2886a-105">An expression is classified as one of the following:</span></span>

*  <span data-ttu-id="2886a-106">Bir değer.</span><span class="sxs-lookup"><span data-stu-id="2886a-106">A value.</span></span> <span data-ttu-id="2886a-107">Her değerin ilişkili bir türü vardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-107">Every value has an associated type.</span></span>
*  <span data-ttu-id="2886a-108">Bir değişken.</span><span class="sxs-lookup"><span data-stu-id="2886a-108">A variable.</span></span> <span data-ttu-id="2886a-109">Her değişkenin bir ilişkili türü vardır, yani bu değişkenin belirtilen türü.</span><span class="sxs-lookup"><span data-stu-id="2886a-109">Every variable has an associated type, namely the declared type of the variable.</span></span>
*  <span data-ttu-id="2886a-110">Ad alanı.</span><span class="sxs-lookup"><span data-stu-id="2886a-110">A namespace.</span></span> <span data-ttu-id="2886a-111">Bu sınıflandırmayla bir ifade, yalnızca bir *member_access* ([üye erişimi](expressions.md#member-access)) sol tarafında görünür.</span><span class="sxs-lookup"><span data-stu-id="2886a-111">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="2886a-112">Diğer bir bağlamda, ad alanı olarak sınıflandırılan bir ifade, derleme zamanı hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>
*  <span data-ttu-id="2886a-113">Bir tür.</span><span class="sxs-lookup"><span data-stu-id="2886a-113">A type.</span></span> <span data-ttu-id="2886a-114">Bu sınıflandırmayla bir ifade, yalnızca bir *member_access* ([üye erişimi](expressions.md#member-access)) veya `as` işleci ([as işleci](expressions.md#the-as-operator)), `is` işleci ([IS işleci](expressions.md#the-is-operator)) veya `typeof` işleci ([typeof işleci](expressions.md#the-typeof-operator)) için bir işlenen olarak yer alabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-114">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)), or as an operand for the `as` operator ([The as operator](expressions.md#the-as-operator)), the `is` operator ([The is operator](expressions.md#the-is-operator)), or the `typeof` operator ([The typeof operator](expressions.md#the-typeof-operator)).</span></span> <span data-ttu-id="2886a-115">Başka bir bağlamda, tür olarak sınıflandırılan bir ifade, derleme zamanı hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>
*  <span data-ttu-id="2886a-116">Bir üye aramasının ([üye arama](expressions.md#member-lookup)) sonucu olan aşırı yüklenmiş yöntemler kümesi olan bir yöntem grubu.</span><span class="sxs-lookup"><span data-stu-id="2886a-116">A method group, which is a set of overloaded methods resulting from a member lookup ([Member lookup](expressions.md#member-lookup)).</span></span> <span data-ttu-id="2886a-117">Bir yöntem grubunun ilişkili bir örnek ifadesi ve ilişkili bir tür bağımsız değişkeni listesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-117">A method group may have an associated instance expression and an associated type argument list.</span></span> <span data-ttu-id="2886a-118">Bir örnek yöntemi çağrıldığında, örnek ifadesinin değerlendirilme sonucu `this` ([Bu erişim](expressions.md#this-access)) tarafından temsil edilen örnek haline gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-118">When an instance method is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="2886a-119">Bir yöntem grubuna bir *invocation_expression* ([çağırma ifadelerinde](expressions.md#invocation-expressions)), bir *delegate_creation_expression* ([temsilci oluşturma ifadeleri](expressions.md#delegate-creation-expressions)) ve bir bir bir işleç işlecinin sol tarafı olarak izin verilir ve örtülü olarak uyumlu bir temsilci türüne dönüştürülebilir ([Yöntem grubu dönüştürmeleri](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="2886a-119">A method group is permitted in an *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)) , a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) and as the left hand side of an is operator, and can be implicitly converted to a compatible delegate type ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="2886a-120">Diğer bir bağlamda, Yöntem grubu olarak sınıflandırılan bir ifade derleme zamanı hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-120">In any other context, an expression classified as a method group causes a compile-time error.</span></span>
*  <span data-ttu-id="2886a-121">Null bir sabit değer.</span><span class="sxs-lookup"><span data-stu-id="2886a-121">A null literal.</span></span> <span data-ttu-id="2886a-122">Bu sınıflandırmayla bir ifade, örtük olarak bir başvuru türüne veya null yapılabilir türe dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-122">An expression with this classification can be implicitly converted to a reference type or nullable type.</span></span>
*  <span data-ttu-id="2886a-123">Anonim bir işlev.</span><span class="sxs-lookup"><span data-stu-id="2886a-123">An anonymous function.</span></span> <span data-ttu-id="2886a-124">Bu sınıflandırmayla bir ifade, örtülü olarak uyumlu bir temsilci türüne veya ifade ağacı türüne dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-124">An expression with this classification can be implicitly converted to a compatible delegate type or expression tree type.</span></span>
*  <span data-ttu-id="2886a-125">Özellik erişimi.</span><span class="sxs-lookup"><span data-stu-id="2886a-125">A property access.</span></span> <span data-ttu-id="2886a-126">Her özellik erişiminin, özelliğin türü olarak ilişkili bir türü vardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-126">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="2886a-127">Ayrıca, özellik erişiminin ilişkili bir örnek ifadesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-127">Furthermore, a property access may have an associated instance expression.</span></span> <span data-ttu-id="2886a-128">Örnek özellik erişiminin bir erişimcisi (`get` veya `set` bloğu) çağrıldığında, örnek ifadesinin değerlendirilme sonucu `this` ([Bu erişim](expressions.md#this-access)) tarafından temsil edilen örnek haline gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-128">When an accessor (the `get` or `set` block) of an instance property access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="2886a-129">Bir olay erişimi.</span><span class="sxs-lookup"><span data-stu-id="2886a-129">An event access.</span></span> <span data-ttu-id="2886a-130">Her olay erişiminde ilişkili bir tür bulunur, bu olay türü olayın türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="2886a-131">Ayrıca, bir olay erişiminin ilişkili bir örnek ifadesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-131">Furthermore, an event access may have an associated instance expression.</span></span> <span data-ttu-id="2886a-132">`+=` ve `-=` işleçlerinin ([olay atama](expressions.md#event-assignment)) sol tarafında bir olay erişimi görünebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-132">An event access may appear as the left hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="2886a-133">Diğer bir bağlamda, olay erişimi olarak sınıflandırılan bir ifade, derleme zamanı hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>
*  <span data-ttu-id="2886a-134">Dizin Oluşturucu erişimi.</span><span class="sxs-lookup"><span data-stu-id="2886a-134">An indexer access.</span></span> <span data-ttu-id="2886a-135">Her Dizin Oluşturucu erişiminin ilişkili bir türü vardır, yani dizin oluşturucunun öğe türü.</span><span class="sxs-lookup"><span data-stu-id="2886a-135">Every indexer access has an associated type, namely the element type of the indexer.</span></span> <span data-ttu-id="2886a-136">Ayrıca, Dizin Oluşturucu erişiminin ilişkili bir örnek ifadesi ve ilişkili bağımsız değişken listesi vardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-136">Furthermore, an indexer access has an associated instance expression and an associated argument list.</span></span> <span data-ttu-id="2886a-137">Bir Dizin Oluşturucu erişiminin bir erişimcisi (`get` veya `set` bloğu) çağrıldığında, örnek ifadesinin değerlendirilme sonucu `this` ([Bu erişim](expressions.md#this-access)) tarafından temsil edilen örnek olur ve bağımsız değişken listesinin değerlendirilme sonucu, çağrının parametre listesi olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-137">When an accessor (the `get` or `set` block) of an indexer access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)), and the result of evaluating the argument list becomes the parameter list of the invocation.</span></span>
*  <span data-ttu-id="2886a-138">Hiçbir şey.</span><span class="sxs-lookup"><span data-stu-id="2886a-138">Nothing.</span></span> <span data-ttu-id="2886a-139">Bu, ifade `void`dönüş türü olan bir yöntemin çağrılışında oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-139">This occurs when the expression is an invocation of a method with a return type of `void`.</span></span> <span data-ttu-id="2886a-140">Nothing olarak sınıflandırılan bir ifade yalnızca bir *statement_expression* ([ifade deyimleri](statements.md#expression-statements)) bağlamında geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-140">An expression classified as nothing is only valid in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

<span data-ttu-id="2886a-141">Bir ifadenin nihai sonucu hiçbir şekilde bir ad alanı, tür, Yöntem grubu veya olay erişimdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-141">The final result of an expression is never a namespace, type, method group, or event access.</span></span> <span data-ttu-id="2886a-142">Bunun yerine, yukarıda belirtildiği gibi, bu ifade kategorileri yalnızca belirli bağlamlarda izin verilen ara yapılardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-142">Rather, as noted above, these categories of expressions are intermediate constructs that are only permitted in certain contexts.</span></span>

<span data-ttu-id="2886a-143">Bir özellik erişimi veya Dizin Oluşturucu erişimi, *Get erişimcisinin* veya *set erişimcisinin*çağrılması gerçekleştirerek her zaman bir değer olarak yeniden sınıflanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-143">A property access or indexer access is always reclassified as a value by performing an invocation of the *get accessor* or the *set accessor*.</span></span> <span data-ttu-id="2886a-144">Belirli erişimci, özelliğin veya dizin oluşturucunun erişim bağlamı tarafından belirlenir: erişim bir atamanın hedefi ise, *küme erişimcisi* yeni bir değer ([basit atama](expressions.md#simple-assignment)) atamak için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-144">The particular accessor is determined by the context of the property or indexer access: If the access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="2886a-145">Aksi takdirde, *get erişimcisi* geçerli değeri ([ifadelerin değerleri](expressions.md#values-of-expressions)) almak için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-145">Otherwise, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="values-of-expressions"></a><span data-ttu-id="2886a-146">İfadelerin değerleri</span><span class="sxs-lookup"><span data-stu-id="2886a-146">Values of expressions</span></span>

<span data-ttu-id="2886a-147">Bir ifadeyi içeren yapıların çoğu sonunda ifadenin bir ***değeri***belirtmek için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-147">Most of the constructs that involve an expression ultimately require the expression to denote a ***value***.</span></span> <span data-ttu-id="2886a-148">Bu gibi durumlarda, gerçek ifade bir ad alanı, bir tür, bir yöntem grubu veya hiçbir şey ifade ediyorsa, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-148">In such cases, if the actual expression denotes a namespace, a type, a method group, or nothing, a compile-time error occurs.</span></span> <span data-ttu-id="2886a-149">Ancak, ifade bir özellik erişimi, Dizin Oluşturucu erişimi veya bir değişken ise, özelliğin, dizin oluşturucunun veya değişkenin değeri örtük olarak yerine kullanılır:</span><span class="sxs-lookup"><span data-stu-id="2886a-149">However, if the expression denotes a property access, an indexer access, or a variable, the value of the property, indexer, or variable is implicitly substituted:</span></span>

*  <span data-ttu-id="2886a-150">Bir değişkenin değeri, değişken tarafından tanımlanan depolama konumunda şu anda depolanan değer olacaktır.</span><span class="sxs-lookup"><span data-stu-id="2886a-150">The value of a variable is simply the value currently stored in the storage location identified by the variable.</span></span> <span data-ttu-id="2886a-151">Değerin alınabilmesi için bir değişken kesin olarak atanmış ([kesin atama](variables.md#definite-assignment)) olarak kabul edilmelidir, aksi takdirde bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-151">A variable must be considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained, or otherwise a compile-time error occurs.</span></span>
*  <span data-ttu-id="2886a-152">Özellik erişim ifadesinin değeri, özelliğin *get erişimcisi* çağrılarak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-152">The value of a property access expression is obtained by invoking the *get accessor* of the property.</span></span> <span data-ttu-id="2886a-153">Özelliğin *get erişimcisi*yoksa, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-153">If the property has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="2886a-154">Aksi takdirde, bir işlev üye çağrısı ([dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) gerçekleştirilir ve çağrının sonucu özellik erişim ifadesinin değeri olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-154">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed, and the result of the invocation becomes the value of the property access expression.</span></span>
*  <span data-ttu-id="2886a-155">Dizin Oluşturucu erişim ifadesinin değeri, dizin oluşturucunun *get erişimcisi* çağrılarak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-155">The value of an indexer access expression is obtained by invoking the *get accessor* of the indexer.</span></span> <span data-ttu-id="2886a-156">Dizin oluşturucunun *get erişimcisi*yoksa, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-156">If the indexer has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="2886a-157">Aksi takdirde, Dizin Oluşturucu erişim ifadesiyle ilişkili bağımsız değişken listesiyle bir işlev üye çağrısı ([dinamik aşırı yükleme çözümlemesi Için derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) gerçekleştirilir ve çağrının sonucu Dizin Oluşturucu erişim ifadesinin değeri olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-157">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed with the argument list associated with the indexer access expression, and the result of the invocation becomes the value of the indexer access expression.</span></span>

## <a name="static-and-dynamic-binding"></a><span data-ttu-id="2886a-158">Statik ve dinamik bağlama</span><span class="sxs-lookup"><span data-stu-id="2886a-158">Static and Dynamic Binding</span></span>

<span data-ttu-id="2886a-159">Bileşen ifadelerinin türü veya değerine (bağımsız değişkenler, işlenenler, alıcılar) göre bir işlemin anlamını belirleme işlemi genellikle ***bağlama***olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-159">The process of determining the meaning of an operation based on the type or value of constituent expressions (arguments, operands, receivers) is often referred to as ***binding***.</span></span> <span data-ttu-id="2886a-160">Örneğin, bir yöntem çağrısının anlamı alıcı ve bağımsız değişkenlerin türüne göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-160">For instance the meaning of a method call is determined based on the type of the receiver and arguments.</span></span> <span data-ttu-id="2886a-161">Bir işlecin anlamı, işlenenlerinin türüne göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-161">The meaning of an operator is determined based on the type of its operands.</span></span>

<span data-ttu-id="2886a-162">C# Bir işlemin anlamı genellikle derleme zamanında, bileşen ifadelerinin derleme zamanı türüne göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-162">In C# the meaning of an operation is usually determined at compile-time, based on the compile-time type of its constituent expressions.</span></span> <span data-ttu-id="2886a-163">Benzer şekilde, bir ifade hata içeriyorsa, hata algılanır ve derleyici tarafından raporlanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-163">Likewise, if an expression contains an error, the error is detected and reported by the compiler.</span></span> <span data-ttu-id="2886a-164">Bu yaklaşım ***statik bağlama***olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="2886a-164">This approach is known as ***static binding***.</span></span>

<span data-ttu-id="2886a-165">Ancak, bir ifade dinamik bir ifadesiyse (yani türü `dynamic`) Bu, içinde yer aldığı herhangi bir bağlamanın, derleme zamanında sahip olduğu tür yerine çalışma zamanı türüne (çalışma zamanında gösterdiği nesnenin gerçek türü) bağlı olması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="2886a-165">However, if an expression is a dynamic expression (i.e. has the type `dynamic`) this indicates that any binding that it participates in should be based on its run-time type (i.e. the actual type of the object it denotes at run-time) rather than the type it has at compile-time.</span></span> <span data-ttu-id="2886a-166">Bu nedenle, bu tür bir işlemin bağlanması, programın çalışması sırasında işlemin gerçekleştirileceği zamana kadar ertelenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-166">The binding of such an operation is therefore deferred until the time where the operation is to be executed during the running of the program.</span></span> <span data-ttu-id="2886a-167">Bu, ***dinamik bağlama***olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-167">This is referred to as ***dynamic binding***.</span></span>

<span data-ttu-id="2886a-168">Bir işlem dinamik olarak bağlandığında, derleyici tarafından çok az veya hiçbir denetim yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-168">When an operation is dynamically bound, little or no checking is performed by the compiler.</span></span> <span data-ttu-id="2886a-169">Çalışma zamanı bağlama başarısız olursa, hatalar çalışma zamanında özel durum olarak bildirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-169">Instead if the run-time binding fails, errors are reported as exceptions at run-time.</span></span>

<span data-ttu-id="2886a-170">' De C# aşağıdaki işlemler bağlamaya tabidir:</span><span class="sxs-lookup"><span data-stu-id="2886a-170">The following operations in C# are subject to binding:</span></span>

*  <span data-ttu-id="2886a-171">Üye erişimi: `e.M`</span><span class="sxs-lookup"><span data-stu-id="2886a-171">Member access: `e.M`</span></span>
*  <span data-ttu-id="2886a-172">Yöntem çağırma: `e.M(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="2886a-172">Method invocation: `e.M(e1, ..., eN)`</span></span>
*  <span data-ttu-id="2886a-173">Temsilci çağrısı:`e(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="2886a-173">Delegate invocation:`e(e1, ..., eN)`</span></span>
*  <span data-ttu-id="2886a-174">Öğe erişimi: `e[e1, ..., eN]`</span><span class="sxs-lookup"><span data-stu-id="2886a-174">Element access: `e[e1, ..., eN]`</span></span>
*  <span data-ttu-id="2886a-175">Nesne oluşturma: `new C(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="2886a-175">Object creation: `new C(e1, ..., eN)`</span></span>
*  <span data-ttu-id="2886a-176">Aşırı yüklenmiş Birli İşleçler: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span><span class="sxs-lookup"><span data-stu-id="2886a-176">Overloaded unary operators: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span></span>
*  <span data-ttu-id="2886a-177">Aşırı yüklenmiş ikili işleçler: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span><span class="sxs-lookup"><span data-stu-id="2886a-177">Overloaded binary operators: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span></span>
*  <span data-ttu-id="2886a-178">Atama işleçleri: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span><span class="sxs-lookup"><span data-stu-id="2886a-178">Assignment operators: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span></span>
*  <span data-ttu-id="2886a-179">Örtük ve açık dönüştürmeler</span><span class="sxs-lookup"><span data-stu-id="2886a-179">Implicit and explicit conversions</span></span>

<span data-ttu-id="2886a-180">Hiçbir dinamik ifade dahil edildiğinde, C# varsayılan statik bağlama olur, bu, bileşen ifadelerinin derleme zamanı türlerinin seçim sürecinde kullanıldığı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-180">When no dynamic expressions are involved, C# defaults to static binding, which means that the compile-time types of constituent expressions are used in the selection process.</span></span> <span data-ttu-id="2886a-181">Ancak, yukarıda listelenen işlemler içindeki yapısal ifadelerden biri dinamik bir ifade olduğunda, işlem bunun yerine dinamik olarak bağlanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-181">However, when one of the constituent expressions in the operations listed above is a dynamic expression, the operation is instead dynamically bound.</span></span>

### <a name="binding-time"></a><span data-ttu-id="2886a-182">Bağlama Zamanı</span><span class="sxs-lookup"><span data-stu-id="2886a-182">Binding-time</span></span>

<span data-ttu-id="2886a-183">Statik bağlama, derleme zamanında gerçekleşir, ancak dinamik bağlama çalışma zamanında gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="2886a-183">Static binding takes place at compile-time, whereas dynamic binding takes place at run-time.</span></span> <span data-ttu-id="2886a-184">Aşağıdaki bölümlerde, bağlama ***zamanı*** terimi, bağlamanın ne zaman gerçekleşdiğine bağlı olarak, derleme zamanı veya çalışma zamanı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-184">In the following sections, the term ***binding-time*** refers to either compile-time or run-time, depending on when the binding takes place.</span></span>

<span data-ttu-id="2886a-185">Aşağıdaki örnek, statik ve dinamik bağlamanın ve bağlama zamanının önemli sayısını göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="2886a-185">The following example illustrates the notions of static and dynamic binding and of binding-time:</span></span>
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

<span data-ttu-id="2886a-186">İlk iki çağrı statik olarak bağlanmıştır: `Console.WriteLine` aşırı yüklemesi, bağımsız değişkeninin derleme zamanı türüne göre çekilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-186">The first two calls are statically bound: the overload of `Console.WriteLine` is picked based on the compile-time type of their argument.</span></span> <span data-ttu-id="2886a-187">Bu nedenle, bağlama zamanı derleme zamanı olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-187">Thus, the binding-time is compile-time.</span></span>

<span data-ttu-id="2886a-188">Üçüncü çağrı dinamik olarak bağlı: `Console.WriteLine` aşırı yüklemesi, bağımsız değişkeninin çalışma zamanı türüne göre çekilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-188">The third call is dynamically bound: the overload of `Console.WriteLine` is picked based on the run-time type of its argument.</span></span> <span data-ttu-id="2886a-189">Bağımsız değişken dinamik bir ifade olduğu için bu durum, derleme zamanı türü `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="2886a-189">This happens because the argument is a dynamic expression -- its compile-time type is `dynamic`.</span></span> <span data-ttu-id="2886a-190">Bu nedenle, üçüncü çağrının bağlama zamanı çalışma zamanı olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-190">Thus, the binding-time for the third call is run-time.</span></span>

### <a name="dynamic-binding"></a><span data-ttu-id="2886a-191">Dinamik bağlama</span><span class="sxs-lookup"><span data-stu-id="2886a-191">Dynamic binding</span></span>

<span data-ttu-id="2886a-192">Dinamik bağlamanın amacı, C# programların ***dinamik nesnelerle***etkileşime geçmesini sağlamaktır, yani C# tür sisteminin normal kurallarını takip etmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-192">The purpose of dynamic binding is to allow C# programs to interact with ***dynamic objects***, i.e. objects that do not follow the normal rules of the C# type system.</span></span> <span data-ttu-id="2886a-193">Dinamik nesneler, farklı türlerde sistemlerle diğer programlama dillerinden nesneler olabilir veya farklı işlemler için kendi bağlama semantiğini uygulamak üzere programlı bir şekilde kurulum olan nesneler olabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-193">Dynamic objects may be objects from other programming languages with different types systems, or they may be objects that are programmatically setup to implement their own binding semantics for different operations.</span></span>

<span data-ttu-id="2886a-194">Dinamik bir nesnenin kendi semantiğini uyguladığı mekanizma uygulama tanımlı ' dır.</span><span class="sxs-lookup"><span data-stu-id="2886a-194">The mechanism by which a dynamic object implements its own semantics is implementation defined.</span></span> <span data-ttu-id="2886a-195">Verilen bir arabirim (yeniden tanımlanmış bir uygulama), dinamik nesneler tarafından özel semantiklere sahip oldukları C# çalışma zamanına işaret etmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-195">A given interface -- again implementation defined -- is implemented by dynamic objects to signal to the C# run-time that they have special semantics.</span></span> <span data-ttu-id="2886a-196">Bu nedenle, dinamik bir nesne üzerindeki her bir işlem dinamik olarak bağlandığında, bu belgede belirtiler C# yerine kendi bağlama semantiğinin yerine geçer.</span><span class="sxs-lookup"><span data-stu-id="2886a-196">Thus, whenever operations on a dynamic object are dynamically bound, their own binding semantics, rather than those of C# as specified in this document, take over.</span></span>

<span data-ttu-id="2886a-197">Dinamik bağlamanın amacı dinamik nesnelerle birlikte çalıştırılmasına izin verirken, C# dinamik olmasına bakılmaksızın tüm nesnelerde dinamik bağlamaya izin verir.</span><span class="sxs-lookup"><span data-stu-id="2886a-197">While the purpose of dynamic binding is to allow interoperation with dynamic objects, C# allows dynamic binding on all objects, whether they are dynamic or not.</span></span> <span data-ttu-id="2886a-198">Bu, dinamik nesnelerin daha yumuşak bir şekilde tümleştirilmesini sağlar, çünkü bunlar üzerindeki işlemlerin sonuçları dinamik nesneler olmayabilir, ancak hala derleme zamanında programcıya bilinmeyen bir tür olabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-198">This allows for a smoother integration of dynamic objects, as the results of operations on them may not themselves be dynamic objects, but are still of a type unknown to the programmer at compile-time.</span></span> <span data-ttu-id="2886a-199">Ayrıca dinamik bağlama, hiçbir nesne dahil, dinamik nesneler olmasa bile hataya açık olan yansıma tabanlı kodu ortadan kaldırmaya yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-199">Also dynamic binding can help eliminate error-prone reflection-based code even when no objects involved are dynamic objects.</span></span>

<span data-ttu-id="2886a-200">Aşağıdaki bölümlerde, dinamik bağlama uygulandığında, her bir yapı için, dinamik bağlama uygulandığında, derleme zaman denetimi (varsa), varsa derleme zamanı ve ifade sınıflandırmasının ne olduğu ile ilgili olarak açıklanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-200">The following sections describe for each construct in the language exactly when dynamic binding is applied, what compile time checking -- if any -- is applied, and what the compile-time result and expression classification is.</span></span>

### <a name="types-of-constituent-expressions"></a><span data-ttu-id="2886a-201">Anayent ifadelerin türleri</span><span class="sxs-lookup"><span data-stu-id="2886a-201">Types of constituent expressions</span></span>

<span data-ttu-id="2886a-202">Bir işlem statik olarak bağlandığında, bir bileşen ifadesinin türü (örn. bir alıcı, bir bağımsız değişken, bir dizin veya işlenen), her zaman bu ifadenin derleme zamanı türü olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-202">When an operation is statically bound, the type of a constituent expression (e.g. a receiver, an argument, an index or an operand) is always considered to be the compile-time type of that expression.</span></span>

<span data-ttu-id="2886a-203">Bir işlem dinamik olarak bağlandığında, bileşen ifadesinin türü, bileşen ifadesinin derleme zamanı türüne bağlı olarak farklı yollarla belirlenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-203">When an operation is dynamically bound, the type of a constituent expression is determined in different ways depending on the compile-time type of the constituent expression:</span></span>

*  <span data-ttu-id="2886a-204">Derleme zamanı türü `dynamic` yapısal ifadesi, ifadenin çalışma zamanında değerlendirilen gerçek değerin türüne sahip olarak kabul edilir</span><span class="sxs-lookup"><span data-stu-id="2886a-204">A constituent expression of compile-time type `dynamic` is considered to have the type of the actual value that the expression evaluates to at runtime</span></span>
*  <span data-ttu-id="2886a-205">Derleme zamanı türü bir tür parametresi olan bir anayent ifadesi tür parametresinin çalışma zamanında bağlı olduğu türe sahip olarak kabul edilir</span><span class="sxs-lookup"><span data-stu-id="2886a-205">A constituent expression whose compile-time type is a type parameter is considered to have the type which the type parameter is bound to at runtime</span></span>
*  <span data-ttu-id="2886a-206">Aksi halde, oluşturan ifade derleme zamanı türüne sahip olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-206">Otherwise the constituent expression is considered to have its compile-time type.</span></span>

## <a name="operators"></a><span data-ttu-id="2886a-207">İşleçler</span><span class="sxs-lookup"><span data-stu-id="2886a-207">Operators</span></span>

<span data-ttu-id="2886a-208">İfadeler, ***işlenenler*** ve ***işleçlerden***oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-208">Expressions are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="2886a-209">Bir ifadenin işleçleri, işlenenlerin hangi işlemleri uygulanacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="2886a-209">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="2886a-210">Operatör örnekleri arasında `+`, `-`, `*`, `/`ve `new`sayılabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-210">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="2886a-211">İşlenenlerin örnekleri, sabit değerleri, alanları, yerel değişkenleri ve ifadeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="2886a-211">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="2886a-212">Üç tür işleç vardır:</span><span class="sxs-lookup"><span data-stu-id="2886a-212">There are three kinds of operators:</span></span>

*  <span data-ttu-id="2886a-213">Birli İşleçler.</span><span class="sxs-lookup"><span data-stu-id="2886a-213">Unary operators.</span></span> <span data-ttu-id="2886a-214">Birli İşleçler bir işlenen alır ve ön ek gösterimini (örneğin, `--x`) veya sonek gösterimini (`x++`gibi) kullanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-214">The unary operators take one operand and use either prefix notation (such as `--x`) or postfix notation (such as `x++`).</span></span>
*  <span data-ttu-id="2886a-215">İkili işleçler.</span><span class="sxs-lookup"><span data-stu-id="2886a-215">Binary operators.</span></span> <span data-ttu-id="2886a-216">İkili işleçler iki işlenen alır ve tüm yanlışın kullanımı (örneğin, `x + y`).</span><span class="sxs-lookup"><span data-stu-id="2886a-216">The binary operators take two operands and all use infix notation (such as `x + y`).</span></span>
*  <span data-ttu-id="2886a-217">Üçlü işleç.</span><span class="sxs-lookup"><span data-stu-id="2886a-217">Ternary operator.</span></span> <span data-ttu-id="2886a-218">Yalnızca bir üçlü operatör, `?:`var; üç işlenen alır ve geçersiz kılma gösterimi (`c ? x : y`) kullanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-218">Only one ternary operator, `?:`, exists; it takes three operands and uses infix notation (`c ? x : y`).</span></span>

<span data-ttu-id="2886a-219">Bir ifadedeki işleçlerin değerlendirilme sırası, işleçlerin ***Öncelik*** ve ***ilişkilendirilebilirliği*** ([işleç önceliği ve ilişkilendirilebilirlik](expressions.md#operator-precedence-and-associativity)) tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-219">The order of evaluation of operators in an expression is determined by the ***precedence*** and ***associativity*** of the operators ([Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)).</span></span>

<span data-ttu-id="2886a-220">Bir ifadedeki işlenenler soldan sağa değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-220">Operands in an expression are evaluated from left to right.</span></span> <span data-ttu-id="2886a-221">Örneğin, `F(i) + G(i++) * H(i)``F` yöntemi, eski `i`değeri kullanılarak çağrılır, yöntem `G` eski `i`değeriyle çağrılır ve son olarak, yöntem `H` yeni `i`değeri ile çağırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-221">For example, in `F(i) + G(i++) * H(i)`, method `F` is called using the old value of `i`, then method `G` is called with the old value of `i`, and, finally, method `H` is called with the new value of `i`.</span></span> <span data-ttu-id="2886a-222">Bu, ' den farklıdır ve işleç önceliğine ilgisiz değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-222">This is separate from and unrelated to operator precedence.</span></span>

<span data-ttu-id="2886a-223">Bazı işleçler ***aşırı***yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-223">Certain operators can be ***overloaded***.</span></span> <span data-ttu-id="2886a-224">İşleç aşırı yüklemesi, Kullanıcı tanımlı operatör uygulamalarının bir veya her ikisinin de Kullanıcı tanımlı sınıf veya yapı türünde ([operatör aşırı yüklemesi](expressions.md#operator-overloading)) olduğu işlemler için belirtilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="2886a-224">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type ([Operator overloading](expressions.md#operator-overloading)).</span></span>

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="2886a-225">İşleç önceliği ve ilişkilendirilebilirlik</span><span class="sxs-lookup"><span data-stu-id="2886a-225">Operator precedence and associativity</span></span>

<span data-ttu-id="2886a-226">Bir ifade birden çok işleç içerdiğinde, işleçlerin ***önceliği*** ayrı işleçlerin değerlendirilme sırasını denetler.</span><span class="sxs-lookup"><span data-stu-id="2886a-226">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="2886a-227">Örneğin, `*` işleci ikili `+` işlecinden daha yüksek önceliğe sahip olduğundan ifade `x + y * z` `x + (y * z)` olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-227">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the binary `+` operator.</span></span> <span data-ttu-id="2886a-228">Bir işlecin önceliği, ilişkili dilbilgisi üretiminin tanımıyla belirlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-228">The precedence of an operator is established by the definition of its associated grammar production.</span></span> <span data-ttu-id="2886a-229">Örneğin, bir *additive_expression* , `+` veya `-` işleçleriyle ayrılan *multiplicative_expression*s dizisinden oluşur. böylece `+` ve `-` işleçleri `*`, `/`ve `%` işleçlerinden daha düşük önceliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="2886a-229">For example, an *additive_expression* consists of a sequence of *multiplicative_expression*s separated by `+` or `-` operators, thus giving the `+` and `-` operators lower precedence than the `*`, `/`, and `%` operators.</span></span>

<span data-ttu-id="2886a-230">Aşağıdaki tablo, en yüksekten en düşüğe öncelik sırasına göre tüm işleçleri özetler:</span><span class="sxs-lookup"><span data-stu-id="2886a-230">The following table summarizes all operators in order of precedence from highest to lowest:</span></span>

| <span data-ttu-id="2886a-231">__Bölüm__</span><span class="sxs-lookup"><span data-stu-id="2886a-231">__Section__</span></span>                                                                                   | <span data-ttu-id="2886a-232">__Kategori__</span><span class="sxs-lookup"><span data-stu-id="2886a-232">__Category__</span></span>                | <span data-ttu-id="2886a-233">__İşleçler__</span><span class="sxs-lookup"><span data-stu-id="2886a-233">__Operators__</span></span> | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [<span data-ttu-id="2886a-234">Birincil ifadeler</span><span class="sxs-lookup"><span data-stu-id="2886a-234">Primary expressions</span></span>](expressions.md#primary-expressions)                                     | <span data-ttu-id="2886a-235">Birincil</span><span class="sxs-lookup"><span data-stu-id="2886a-235">Primary</span></span>                     | <span data-ttu-id="2886a-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span><span class="sxs-lookup"><span data-stu-id="2886a-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span></span> | 
| [<span data-ttu-id="2886a-237">Birli İşleçler</span><span class="sxs-lookup"><span data-stu-id="2886a-237">Unary operators</span></span>](expressions.md#unary-operators)                                             | <span data-ttu-id="2886a-238">Li</span><span class="sxs-lookup"><span data-stu-id="2886a-238">Unary</span></span>                       | <span data-ttu-id="2886a-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span><span class="sxs-lookup"><span data-stu-id="2886a-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span></span> | 
| [<span data-ttu-id="2886a-240">Aritmetik işleçler</span><span class="sxs-lookup"><span data-stu-id="2886a-240">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="2886a-241">Çarpma</span><span class="sxs-lookup"><span data-stu-id="2886a-241">Multiplicative</span></span>              | <span data-ttu-id="2886a-242">`*`  `/`  `%`</span><span class="sxs-lookup"><span data-stu-id="2886a-242">`*`  `/`  `%`</span></span> | 
| [<span data-ttu-id="2886a-243">Aritmetik işleçler</span><span class="sxs-lookup"><span data-stu-id="2886a-243">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="2886a-244">Msal</span><span class="sxs-lookup"><span data-stu-id="2886a-244">Additive</span></span>                    | <span data-ttu-id="2886a-245">`+`  `-`</span><span class="sxs-lookup"><span data-stu-id="2886a-245">`+`  `-`</span></span>      | 
| [<span data-ttu-id="2886a-246">Kaydırma işleçleri</span><span class="sxs-lookup"><span data-stu-id="2886a-246">Shift operators</span></span>](expressions.md#shift-operators)                                             | <span data-ttu-id="2886a-247">Shift</span><span class="sxs-lookup"><span data-stu-id="2886a-247">Shift</span></span>                       | <span data-ttu-id="2886a-248">`<<`  `>>`</span><span class="sxs-lookup"><span data-stu-id="2886a-248">`<<`  `>>`</span></span>    | 
| [<span data-ttu-id="2886a-249">İlişkisel ve tür testi işleçleri</span><span class="sxs-lookup"><span data-stu-id="2886a-249">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="2886a-250">İlişkisel ve tür testi</span><span class="sxs-lookup"><span data-stu-id="2886a-250">Relational and type testing</span></span> | <span data-ttu-id="2886a-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span><span class="sxs-lookup"><span data-stu-id="2886a-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span></span> | 
| [<span data-ttu-id="2886a-252">İlişkisel ve tür testi işleçleri</span><span class="sxs-lookup"><span data-stu-id="2886a-252">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="2886a-253">Eşitlik</span><span class="sxs-lookup"><span data-stu-id="2886a-253">Equality</span></span>                    | <span data-ttu-id="2886a-254">`==`  `!=`</span><span class="sxs-lookup"><span data-stu-id="2886a-254">`==`  `!=`</span></span>    | 
| [<span data-ttu-id="2886a-255">Mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="2886a-255">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="2886a-256">Mantıksal VE</span><span class="sxs-lookup"><span data-stu-id="2886a-256">Logical AND</span></span>                 | `&`           | 
| [<span data-ttu-id="2886a-257">Mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="2886a-257">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="2886a-258">Mantıksal XOR</span><span class="sxs-lookup"><span data-stu-id="2886a-258">Logical XOR</span></span>                 | `^`           | 
| [<span data-ttu-id="2886a-259">Mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="2886a-259">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="2886a-260">Mantıksal VEYA</span><span class="sxs-lookup"><span data-stu-id="2886a-260">Logical OR</span></span>                  | <code>&#124;</code>           |
| [<span data-ttu-id="2886a-261">Koşullu mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="2886a-261">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="2886a-262">Koşullu VE</span><span class="sxs-lookup"><span data-stu-id="2886a-262">Conditional AND</span></span>             | `&&`          | 
| [<span data-ttu-id="2886a-263">Koşullu mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="2886a-263">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="2886a-264">Koşullu VEYA</span><span class="sxs-lookup"><span data-stu-id="2886a-264">Conditional OR</span></span>              | <code>&#124;&#124;</code>          | 
| [<span data-ttu-id="2886a-265">Null birleştirme işleci</span><span class="sxs-lookup"><span data-stu-id="2886a-265">The null coalescing operator</span></span>](expressions.md#the-null-coalescing-operator)                   | <span data-ttu-id="2886a-266">Null birleşim</span><span class="sxs-lookup"><span data-stu-id="2886a-266">Null coalescing</span></span>             | `??`          | 
| [<span data-ttu-id="2886a-267">Koşullu işleç</span><span class="sxs-lookup"><span data-stu-id="2886a-267">Conditional operator</span></span>](expressions.md#conditional-operator)                                   | <span data-ttu-id="2886a-268">Koşullu</span><span class="sxs-lookup"><span data-stu-id="2886a-268">Conditional</span></span>                 | `?:`          | 
| <span data-ttu-id="2886a-269">[Atama işleçleri](expressions.md#assignment-operators), [anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)</span><span class="sxs-lookup"><span data-stu-id="2886a-269">[Assignment operators](expressions.md#assignment-operators), [Anonymous function expressions](expressions.md#anonymous-function-expressions)</span></span>  | <span data-ttu-id="2886a-270">Atama ve lambda ifadesi</span><span class="sxs-lookup"><span data-stu-id="2886a-270">Assignment and lambda expression</span></span> | <span data-ttu-id="2886a-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span><span class="sxs-lookup"><span data-stu-id="2886a-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span></span> | 

<span data-ttu-id="2886a-272">Aynı önceliğe sahip iki işleç arasında bir işlenen gerçekleştiğinde, işleçlerin ilişkilendirilebilirliği, işlemlerin gerçekleştirileceği sırayı denetler:</span><span class="sxs-lookup"><span data-stu-id="2886a-272">When an operand occurs between two operators with the same precedence, the associativity of the operators controls the order in which the operations are performed:</span></span>

*  <span data-ttu-id="2886a-273">Atama işleçleri ve null birleşim işleci dışında, tüm ikili işleçler ***sola ilişkilendirilebilir***, yani işlemler soldan sağa yapılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-273">Except for the assignment operators and the null coalescing operator, all binary operators are ***left-associative***, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="2886a-274">Örneğin, `x + y + z` `(x + y) + z`olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-274">For example, `x + y + z` is evaluated as `(x + y) + z`.</span></span>
*  <span data-ttu-id="2886a-275">Atama işleçleri, null birleşim işleci ve koşullu işleç (`?:`), ***doğru ilişkilendirilebilir***, yani işlemler sağdan sola yapılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-275">The assignment operators, the null coalescing operator and the conditional operator (`?:`) are ***right-associative***, meaning that operations are performed from right to left.</span></span> <span data-ttu-id="2886a-276">Örneğin, `x = y = z` `x = (y = z)`olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-276">For example, `x = y = z` is evaluated as `x = (y = z)`.</span></span>

<span data-ttu-id="2886a-277">Öncelik ve ilişkilendirilebilirlik, parantezler kullanılarak denetlenebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-277">Precedence and associativity can be controlled using parentheses.</span></span> <span data-ttu-id="2886a-278">Örneğin, `x + y * z` önce `y` `z` ile çarpar ve sonucu `x`ekler, ancak `(x + y) * z` önce `x` ve `y` ekler ve sonra sonucu `z`ile çarpar.</span><span class="sxs-lookup"><span data-stu-id="2886a-278">For example, `x + y * z` first multiplies `y` by `z` and then adds the result to `x`, but `(x + y) * z` first adds `x` and `y` and then multiplies the result by `z`.</span></span>

### <a name="operator-overloading"></a><span data-ttu-id="2886a-279">İşleç aşırı yüklemesi</span><span class="sxs-lookup"><span data-stu-id="2886a-279">Operator overloading</span></span>

<span data-ttu-id="2886a-280">Tüm birli ve ikili işleçlerin, herhangi bir ifadede otomatik olarak kullanılabilen önceden tanımlanmış uygulamaları vardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-280">All unary and binary operators have predefined implementations that are automatically available in any expression.</span></span> <span data-ttu-id="2886a-281">Önceden tanımlanmış uygulamalara ek olarak, Kullanıcı tanımlı uygulamalar sınıflar ve yapılar ([işleçler](classes.md#operators)) `operator` bildirimleri dahil ederek tanıtılamaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-281">In addition to the predefined implementations, user-defined implementations can be introduced by including `operator` declarations in classes and structs ([Operators](classes.md#operators)).</span></span> <span data-ttu-id="2886a-282">Kullanıcı tanımlı operatör uygulamaları, her zaman önceden tanımlanmış operatör uygulamalarından önceliklidir: yalnızca geçerli bir Kullanıcı tanımlı operatör uygulaması yoksa, [birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution) ve [ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)bölümünde açıklandığı gibi önceden tanımlanmış operatör uygulamaları göz önünde bulundurulacaktır.</span><span class="sxs-lookup"><span data-stu-id="2886a-282">User-defined operator implementations always take precedence over predefined operator implementations: Only when no applicable user-defined operator implementations exist will the predefined operator implementations be considered, as described in [Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution).</span></span>

<span data-ttu-id="2886a-283">***Fazla yüklenebilir Birli İşleçler*** şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2886a-283">The ***overloadable unary operators*** are:</span></span>
```csharp
+   -   !   ~   ++   --   true   false
```

<span data-ttu-id="2886a-284">`true` ve `false` açıkça ifadelerde kullanılmasa da (ve bu nedenle, [işleç önceliği ve ilişkilendirilebilirliği](expressions.md#operator-precedence-and-associativity)içindeki öncelik tablosuna dahil edilmez), birkaç ifade bağlamlarında çağrıldıklarından işleçler kabul edilir: Boolean Ifadeleri ([Boolean ifadeleri](expressions.md#boolean-expressions)) ve koşullu ([koşullu işleç](expressions.md#conditional-operator)) ve koşullu mantıksal işleçler (koşullu[mantıksal işleçler](expressions.md#conditional-logical-operators)) içeren ifadeler.</span><span class="sxs-lookup"><span data-stu-id="2886a-284">Although `true` and `false` are not used explicitly in expressions (and therefore are not included in the precedence table in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)), they are considered operators because they are invoked in several expression contexts: boolean expressions ([Boolean expressions](expressions.md#boolean-expressions)) and expressions involving the conditional ([Conditional operator](expressions.md#conditional-operator)), and conditional logical operators ([Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span>

<span data-ttu-id="2886a-285">***Fazla yüklenebilir ikili işleçler*** şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2886a-285">The ***overloadable binary operators*** are:</span></span>
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

<span data-ttu-id="2886a-286">Yalnızca yukarıda listelenen operatörler aşırı yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-286">Only the operators listed above can be overloaded.</span></span> <span data-ttu-id="2886a-287">Özellikle, üye erişimi, yöntem çağırma veya `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`ve `is` işleçlerini aşırı yüklemek mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-287">In particular, it is not possible to overload member access, method invocation, or the `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, and `is` operators.</span></span>

<span data-ttu-id="2886a-288">İkili işleç aşırı yüklendiğinde, karşılık gelen atama işleci de dolaylı olarak aşırı yüklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="2886a-288">When a binary operator is overloaded, the corresponding assignment operator, if any, is also implicitly overloaded.</span></span> <span data-ttu-id="2886a-289">Örneğin, işleç `*` aşırı yüklemesi aynı zamanda `*=`işlecinin aşırı yüküne sahiptir.</span><span class="sxs-lookup"><span data-stu-id="2886a-289">For example, an overload of operator `*` is also an overload of operator `*=`.</span></span> <span data-ttu-id="2886a-290">Bu, [bileşik atamada](expressions.md#compound-assignment)daha ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2886a-290">This is described further in [Compound assignment](expressions.md#compound-assignment).</span></span> <span data-ttu-id="2886a-291">Atama işlecinin kendisinin (`=`) aşırı yüklenemez olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2886a-291">Note that the assignment operator itself (`=`) cannot be overloaded.</span></span> <span data-ttu-id="2886a-292">Atama her zaman bir değeri bir değişkene basit bit temelinde bir kopyasını gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="2886a-292">An assignment always performs a simple bit-wise copy of a value into a variable.</span></span>

<span data-ttu-id="2886a-293">`(T)x`gibi dönüştürme işlemleri, Kullanıcı tanımlı dönüştürmeler ([Kullanıcı tanımlı dönüştürmeler](conversions.md#user-defined-conversions)) sağlanarak aşırı yüklenmiştir.</span><span class="sxs-lookup"><span data-stu-id="2886a-293">Cast operations, such as `(T)x`, are overloaded by providing user-defined conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

<span data-ttu-id="2886a-294">`a[x]`gibi öğe erişimi, aşırı yüklenebilir bir operatör olarak kabul edilmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-294">Element access, such as `a[x]`, is not considered an overloadable operator.</span></span> <span data-ttu-id="2886a-295">Bunun yerine, Kullanıcı tanımlı dizin oluşturucular ([Dizin oluşturucular](classes.md#indexers)) aracılığıyla desteklenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-295">Instead, user-defined indexing is supported through indexers ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="2886a-296">İfadelerde, işleçlere işleç gösterimi kullanılarak başvurulur ve bildirimlerinde, işlev gösterimi kullanılarak işleçlere başvurulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-296">In expressions, operators are referenced using operator notation, and in declarations, operators are referenced using functional notation.</span></span> <span data-ttu-id="2886a-297">Aşağıdaki tabloda, birli ve ikili işleçler için işleç ve işlevsel gösterimler arasındaki ilişki gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="2886a-297">The following table shows the relationship between operator and functional notations for unary and binary operators.</span></span> <span data-ttu-id="2886a-298">İlk girişte, *op* fazla yüklenebilir birli ön ek işlecini gösterir.</span><span class="sxs-lookup"><span data-stu-id="2886a-298">In the first entry, *op* denotes any overloadable unary prefix operator.</span></span> <span data-ttu-id="2886a-299">İkinci girişte, *op* birli sonek `++` ve `--` işleçlerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="2886a-299">In the second entry, *op* denotes the unary postfix `++` and `--` operators.</span></span> <span data-ttu-id="2886a-300">Üçüncü girişte, *op* , aşırı yüklenebilir ikili işleç olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="2886a-300">In the third entry, *op* denotes any overloadable binary operator.</span></span>


| <span data-ttu-id="2886a-301">__İşleç gösterimi__</span><span class="sxs-lookup"><span data-stu-id="2886a-301">__Operator notation__</span></span> | <span data-ttu-id="2886a-302">__İşlevsel Gösterim__</span><span class="sxs-lookup"><span data-stu-id="2886a-302">__Functional notation__</span></span> |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

<span data-ttu-id="2886a-303">Kullanıcı tanımlı işleç bildirimleri her zaman, işleç bildirimini içeren sınıf veya yapı türünde parametrelerden en az birini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2886a-303">User-defined operator declarations always require at least one of the parameters to be of the class or struct type that contains the operator declaration.</span></span> <span data-ttu-id="2886a-304">Bu nedenle, Kullanıcı tanımlı bir işlecin önceden tanımlanmış bir işleçle aynı imzaya sahip olması mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-304">Thus, it is not possible for a user-defined operator to have the same signature as a predefined operator.</span></span>

<span data-ttu-id="2886a-305">Kullanıcı tanımlı işleç bildirimleri bir işlecin sözdizimini, önceliğini veya ilişkilendirilebilirliğini değiştiremez.</span><span class="sxs-lookup"><span data-stu-id="2886a-305">User-defined operator declarations cannot modify the syntax, precedence, or associativity of an operator.</span></span> <span data-ttu-id="2886a-306">Örneğin, `/` işleci her zaman bir ikili işleçtir, her zaman [işleç önceliği ve ilişkilendirilebilirliği](expressions.md#operator-precedence-and-associativity)içinde belirtilen öncelik düzeyine sahiptir ve her zaman sola ilişkilendirilebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-306">For example, the `/` operator is always a binary operator, always has the precedence level specified in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity), and is always left-associative.</span></span>

<span data-ttu-id="2886a-307">Kullanıcı tanımlı bir işlecin, kiralamaları yaptığı herhangi bir hesaplamayı gerçekleştirmesi mümkün olsa da, büyük bir süre içinde olması beklenen, farklı sonuçlar üreten uygulamalar kesinlikle önerilmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-307">While it is possible for a user-defined operator to perform any computation it pleases, implementations that produce results other than those that are intuitively expected are strongly discouraged.</span></span> <span data-ttu-id="2886a-308">Örneğin, `operator ==` bir uygulama, eşitlik için iki işleneni karşılaştırmalı ve uygun bir `bool` sonucu döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-308">For example, an implementation of `operator ==` should compare the two operands for equality and return an appropriate `bool` result.</span></span>

<span data-ttu-id="2886a-309">[Koşullu mantıksal işleçler](expressions.md#conditional-logical-operators) aracılığıyla [birincil ifadelerde](expressions.md#primary-expressions) bağımsız işleçlerin açıklamaları, işleçlerin önceden tanımlanmış uygulamalarını ve her bir operatör için uygulanan ek kuralları belirtir.</span><span class="sxs-lookup"><span data-stu-id="2886a-309">The descriptions of individual operators in [Primary expressions](expressions.md#primary-expressions) through [Conditional logical operators](expressions.md#conditional-logical-operators) specify the predefined implementations of the operators and any additional rules that apply to each operator.</span></span> <span data-ttu-id="2886a-310">Açıklamalar ***birli operatör aşırı yükleme çözünürlüğü***, ***ikili işleç aşırı yükleme çözümlemesi***ve ***sayısal yükseltme***, tanımları aşağıdaki bölümlerde bulunan tanımlardan birini kullanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-310">The descriptions make use of the terms ***unary operator overload resolution***, ***binary operator overload resolution***, and ***numeric promotion***, definitions of which are found in the following sections.</span></span>

### <a name="unary-operator-overload-resolution"></a><span data-ttu-id="2886a-311">Birli operatör aşırı yükleme çözünürlüğü</span><span class="sxs-lookup"><span data-stu-id="2886a-311">Unary operator overload resolution</span></span>

<span data-ttu-id="2886a-312">`op x` veya `x op`bir işlem, `op` aşırı yüklenebilir birli bir işleçtir ve `x` `X`türünde bir ifadedir, aşağıdaki şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-312">An operation of the form `op x` or `x op`, where `op` is an overloadable unary operator, and `x` is an expression of type `X`, is processed as follows:</span></span>

*  <span data-ttu-id="2886a-313">İşlem `operator op(x)` için `X` tarafından sunulan aday Kullanıcı tanımlı operatörler kümesi, [aday Kullanıcı tanımlı işleçlerin](expressions.md#candidate-user-defined-operators)kuralları kullanılarak belirlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-313">The set of candidate user-defined operators provided by `X` for the operation `operator op(x)` is determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span>
*  <span data-ttu-id="2886a-314">Aday Kullanıcı tanımlı işleçler kümesi boş değilse, bu işlem için aday işleçler kümesi olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-314">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="2886a-315">Aksi halde, yükseltilmemiş formları dahil olmak üzere önceden tanımlanmış birli `operator op` uygulamalar, işlem için aday işleçler kümesi olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-315">Otherwise, the predefined unary `operator op` implementations, including their lifted forms, become the set of candidate operators for the operation.</span></span> <span data-ttu-id="2886a-316">Belirli bir işlecin önceden tanımlanmış uygulamaları, işlecinin açıklamasında belirtilir ([birincil ifadeler](expressions.md#primary-expressions) ve [Birli İşleçler](expressions.md#unary-operators)).</span><span class="sxs-lookup"><span data-stu-id="2886a-316">The predefined implementations of a given operator are specified in the description of the operator ([Primary expressions](expressions.md#primary-expressions) and [Unary operators](expressions.md#unary-operators)).</span></span>
*  <span data-ttu-id="2886a-317">Aşırı [yükleme çözümlemesi](expressions.md#overload-resolution) çözüm kuralları, `(x)`bağımsız değişken listesine göre en iyi işleci seçmek üzere aday işleçler kümesine uygulanır ve bu işleç aşırı yükleme çözümleme işleminin sonucu olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-317">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="2886a-318">Aşırı yükleme çözümlemesi tek bir en iyi operatör seçmezse bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-318">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="binary-operator-overload-resolution"></a><span data-ttu-id="2886a-319">İkili işleç aşırı yükleme çözümü</span><span class="sxs-lookup"><span data-stu-id="2886a-319">Binary operator overload resolution</span></span>

<span data-ttu-id="2886a-320">`x op y`bir işlem, `op` aşırı yüklenebilir bir ikili işleçtir, `x` `X`türünde bir ifadedir ve `y` `Y`türünde bir ifadedir, aşağıdaki şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-320">An operation of the form `x op y`, where `op` is an overloadable binary operator, `x` is an expression of type `X`, and `y` is an expression of type `Y`, is processed as follows:</span></span>

*  <span data-ttu-id="2886a-321">İşlem `operator op(x,y)` için `X` ve `Y` tarafından sunulan aday Kullanıcı tanımlı operatörler kümesi belirlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-321">The set of candidate user-defined operators provided by `X` and `Y` for the operation `operator op(x,y)` is determined.</span></span> <span data-ttu-id="2886a-322">Küme, `X` tarafından sunulan aday işleçleri birleşiminden ve `Y`tarafından sunulan aday işleçlerden (her biri [aday Kullanıcı tanımlı işleçlerin](expressions.md#candidate-user-defined-operators)kuralları kullanılarak belirlenir) oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-322">The set consists of the union of the candidate operators provided by `X` and the candidate operators provided by `Y`, each determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span> <span data-ttu-id="2886a-323">`X` ve `Y` aynı türde veya `X` ve `Y` ortak bir temel türden türetildiyse, paylaşılan aday işleçler yalnızca Birleşik küme içinde bir kez gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="2886a-323">If `X` and `Y` are the same type, or if `X` and `Y` are derived from a common base type, then shared candidate operators only occur in the combined set once.</span></span>
*  <span data-ttu-id="2886a-324">Aday Kullanıcı tanımlı işleçler kümesi boş değilse, bu işlem için aday işleçler kümesi olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-324">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="2886a-325">Aksi halde, yükseltilmemiş formları dahil olmak üzere önceden tanımlanmış ikili `operator op` uygulamalar, işlem için aday işleçler kümesi olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-325">Otherwise, the predefined binary `operator op` implementations, including their lifted forms,  become the set of candidate operators for the operation.</span></span> <span data-ttu-id="2886a-326">Belirli bir işlecin önceden tanımlanmış uygulamaları, işlecinin açıklamasında belirtilir ( [Koşullu mantıksal işleçler](expressions.md#conditional-logical-operators)aracılığıyla[Aritmetik işleçler](expressions.md#arithmetic-operators) ).</span><span class="sxs-lookup"><span data-stu-id="2886a-326">The predefined implementations of a given operator are specified in the description of the operator ([Arithmetic operators](expressions.md#arithmetic-operators) through [Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span> <span data-ttu-id="2886a-327">Önceden tanımlanmış Enum ve temsilci işleçleri için kabul edilen tek işleçler, işlenenlerden birinin bağlama zamanı türü olan bir Enum veya temsilci türü tarafından tanımlananlar.</span><span class="sxs-lookup"><span data-stu-id="2886a-327">For predefined enum and delegate operators, the only operators considered are those defined by an enum or delegate type that is the binding-time type of one of the operands.</span></span>
*  <span data-ttu-id="2886a-328">Aşırı [yükleme çözümlemesi](expressions.md#overload-resolution) çözüm kuralları, `(x,y)`bağımsız değişken listesine göre en iyi işleci seçmek üzere aday işleçler kümesine uygulanır ve bu işleç aşırı yükleme çözümleme işleminin sonucu olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-328">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x,y)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="2886a-329">Aşırı yükleme çözümlemesi tek bir en iyi operatör seçmezse bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-329">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="candidate-user-defined-operators"></a><span data-ttu-id="2886a-330">Aday Kullanıcı tanımlı işleçler</span><span class="sxs-lookup"><span data-stu-id="2886a-330">Candidate user-defined operators</span></span>

<span data-ttu-id="2886a-331">`op` fazla yüklenebilir bir operatör olduğu ve `A` bir bağımsız değişken listesi olduğu bir tür `T` ve bir işlem `operator op(A)`veriliyorsa, `T` için `operator op(A)` tarafından sağlanan aday Kullanıcı tanımlı işleçler kümesi aşağıdaki şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-331">Given a type `T` and an operation `operator op(A)`, where `op` is an overloadable operator and `A` is an argument list, the set of candidate user-defined operators provided by `T` for `operator op(A)` is determined as follows:</span></span>

*  <span data-ttu-id="2886a-332">`T0`türünü saptayın.</span><span class="sxs-lookup"><span data-stu-id="2886a-332">Determine the type `T0`.</span></span> <span data-ttu-id="2886a-333">`T` null yapılabilir bir tür ise, `T0` temel türüdür, aksi takdirde `T0` `T`eşittir.</span><span class="sxs-lookup"><span data-stu-id="2886a-333">If `T` is a nullable type, `T0` is its underlying type, otherwise `T0` is equal to `T`.</span></span>
*  <span data-ttu-id="2886a-334">`T0` ve bu tür operatörlerin tüm yükseltilmemiş formlarında, en az bir operatör bağımsız değişken listesi `A`göre uygulanabilir ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)), aday işleçler kümesi, `T0`söz konusu tüm ilgili işleçlerden oluşur. `operator op`</span><span class="sxs-lookup"><span data-stu-id="2886a-334">For all `operator op` declarations in `T0` and all lifted forms of such operators, if at least one operator is applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the argument list `A`, then the set of candidate operators consists of all such applicable operators in `T0`.</span></span>
*  <span data-ttu-id="2886a-335">Aksi takdirde, `T0` `object`, aday işleçleri kümesi boştur.</span><span class="sxs-lookup"><span data-stu-id="2886a-335">Otherwise, if `T0` is `object`, the set of candidate operators is empty.</span></span>
*  <span data-ttu-id="2886a-336">Aksi takdirde, `T0` tarafından sunulan aday işleçleri kümesi, `T0`doğrudan temel sınıfı tarafından sunulan aday işleçler kümesidir veya `T0` bir tür parametresi ise `T0` etkin taban sınıfı olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-336">Otherwise, the set of candidate operators provided by `T0` is the set of candidate operators provided by the direct base class of `T0`, or the effective base class of `T0` if `T0` is a type parameter.</span></span>

### <a name="numeric-promotions"></a><span data-ttu-id="2886a-337">Sayısal yükseltmeler</span><span class="sxs-lookup"><span data-stu-id="2886a-337">Numeric promotions</span></span>

<span data-ttu-id="2886a-338">Sayısal yükseltme, önceden tanımlanmış birli ve ikili sayısal işleçlerin işlenenlerinin belirli örtük dönüştürmelerini otomatik olarak gerçekleştirmekten oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-338">Numeric promotion consists of automatically performing certain implicit conversions of the operands of the predefined unary and binary numeric operators.</span></span> <span data-ttu-id="2886a-339">Sayısal yükseltme, farklı bir mekanizma değildir, ancak önceden tanımlanmış işleçlere aşırı yükleme çözümlemesi uygulamanın bir etkisi vardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-339">Numeric promotion is not a distinct mechanism, but rather an effect of applying overload resolution to the predefined operators.</span></span> <span data-ttu-id="2886a-340">Sayısal yükseltme, Kullanıcı tanımlı işleçlerin benzer etkileri sergilemek üzere uygulanabilmesine rağmen özellikle Kullanıcı tanımlı işleçlerin değerlendirilmesini etkilemez.</span><span class="sxs-lookup"><span data-stu-id="2886a-340">Numeric promotion specifically does not affect evaluation of user-defined operators, although user-defined operators can be implemented to exhibit similar effects.</span></span>

<span data-ttu-id="2886a-341">Sayısal yükseltmede örnek olarak, ikili `*` işlecinin önceden tanımlanmış uygulamalarını göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="2886a-341">As an example of numeric promotion, consider the predefined implementations of the binary `*` operator:</span></span>

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

<span data-ttu-id="2886a-342">Bu işleç kümesine aşırı yükleme çözümleme kuralları ([aşırı yükleme çözümlemesi](expressions.md#overload-resolution)) uygulandığında, efekt, örtük dönüştürmelerin işlenen türlerinden ilk birini seçmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-342">When overload resolution rules ([Overload resolution](expressions.md#overload-resolution)) are applied to this set of operators, the effect is to select the first of the operators for which implicit conversions exist from the operand types.</span></span> <span data-ttu-id="2886a-343">Örneğin, `b` bir `byte` olduğu ve `s` `short`olduğu `b * s`işlem için, aşırı yükleme çözümü en iyi operatör olarak `operator *(int,int)` seçer.</span><span class="sxs-lookup"><span data-stu-id="2886a-343">For example, for the operation `b * s`, where `b` is a `byte` and `s` is a `short`, overload resolution selects `operator *(int,int)` as the best operator.</span></span> <span data-ttu-id="2886a-344">Bu nedenle, `b` ve `s` `int`'e dönüştürüleceğinden ve sonucun türü `int`.</span><span class="sxs-lookup"><span data-stu-id="2886a-344">Thus, the effect is that `b` and `s` are converted to `int`, and the type of the result is `int`.</span></span> <span data-ttu-id="2886a-345">Benzer şekilde, `i` bir `int` olduğu ve `d` `double`olduğu `i * d`işlem için, aşırı yükleme çözümü en iyi operatör olarak `operator *(double,double)` seçer.</span><span class="sxs-lookup"><span data-stu-id="2886a-345">Likewise, for the operation `i * d`, where `i` is an `int` and `d` is a `double`, overload resolution selects `operator *(double,double)` as the best operator.</span></span>

#### <a name="unary-numeric-promotions"></a><span data-ttu-id="2886a-346">Tekli sayı yükseltmeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-346">Unary numeric promotions</span></span>

<span data-ttu-id="2886a-347">Önceden tanımlanmış `+`, `-`ve `~` birli operatörlerin işlenenleri için birli sayısal yükseltme oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-347">Unary numeric promotion occurs for the operands of the predefined `+`, `-`, and `~` unary operators.</span></span> <span data-ttu-id="2886a-348">Birli sayısal yükseltme, `char` türüne `sbyte`, `byte`, `short`, `ushort`veya `int`türündeki işlenenleri dönüştürmeyi içerir.</span><span class="sxs-lookup"><span data-stu-id="2886a-348">Unary numeric promotion simply consists of converting operands of type `sbyte`, `byte`, `short`, `ushort`, or `char` to type `int`.</span></span> <span data-ttu-id="2886a-349">Ayrıca, birli `-` işleci için, birli sayısal promosyon `uint` türündeki işlenenleri `long`türüne dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="2886a-349">Additionally, for the unary `-` operator, unary numeric promotion converts operands of type `uint` to type `long`.</span></span>

#### <a name="binary-numeric-promotions"></a><span data-ttu-id="2886a-350">İkili sayısal yükseltmeler</span><span class="sxs-lookup"><span data-stu-id="2886a-350">Binary numeric promotions</span></span>

<span data-ttu-id="2886a-351">Önceden tanımlanmış `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`ve `<=` ikili işleçlerinin işlenenleri için ikili sayısal yükseltme oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-351">Binary numeric promotion occurs for the operands of the predefined `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, and `<=` binary operators.</span></span> <span data-ttu-id="2886a-352">İkili sayısal yükseltme, ilişkisel olmayan işleçler söz konusu olduğunda her iki işleneni dolaylı olarak ortak bir türe dönüştürür. bu da işlemin sonuç türü olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-352">Binary numeric promotion implicitly converts both operands to a common type which, in case of the non-relational operators, also becomes the result type of the operation.</span></span> <span data-ttu-id="2886a-353">İkili sayısal yükseltme aşağıdaki kuralları, burada göründükleri düzende uygulamaktan oluşur:</span><span class="sxs-lookup"><span data-stu-id="2886a-353">Binary numeric promotion consists of applying the following rules, in the order they appear here:</span></span>

*  <span data-ttu-id="2886a-354">Her iki işlenen `decimal`türünde ise, diğer işlenen `decimal`türüne dönüştürülür veya diğer işlenen `float` veya `double`türünde olduğunda bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-354">If either operand is of type `decimal`, the other operand is converted to type `decimal`, or a binding-time error occurs if the other operand is of type `float` or `double`.</span></span>
*  <span data-ttu-id="2886a-355">Aksi halde, her iki işlenen `double`türünde ise, diğer işlenen `double`türüne dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="2886a-355">Otherwise, if either operand is of type `double`, the other operand is converted to type `double`.</span></span>
*  <span data-ttu-id="2886a-356">Aksi halde, her iki işlenen `float`türünde ise, diğer işlenen `float`türüne dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="2886a-356">Otherwise, if either operand is of type `float`, the other operand is converted to type `float`.</span></span>
*  <span data-ttu-id="2886a-357">Aksi halde, her iki işlenen `ulong`türünde ise, diğer işlenen `ulong`türüne dönüştürülür veya diğer işlenen `sbyte`, `short`, `int`veya `long`türünde olduğunda bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-357">Otherwise, if either operand is of type `ulong`, the other operand is converted to type `ulong`, or a binding-time error occurs if the other operand is of type `sbyte`, `short`, `int`, or `long`.</span></span>
*  <span data-ttu-id="2886a-358">Aksi halde, her iki işlenen `long`türünde ise, diğer işlenen `long`türüne dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="2886a-358">Otherwise, if either operand is of type `long`, the other operand is converted to type `long`.</span></span>
*  <span data-ttu-id="2886a-359">Aksi halde, her iki işlenen de `uint` ve diğer işlenen `sbyte`, `short`veya `int`türündedir, her iki işlenen de `long`türüne dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="2886a-359">Otherwise, if either operand is of type `uint` and the other operand is of type `sbyte`, `short`, or `int`, both operands are converted to type `long`.</span></span>
*  <span data-ttu-id="2886a-360">Aksi halde, her iki işlenen `uint`türünde ise, diğer işlenen `uint`türüne dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="2886a-360">Otherwise, if either operand is of type `uint`, the other operand is converted to type `uint`.</span></span>
*  <span data-ttu-id="2886a-361">Aksi halde, her iki işlenen `int`türüne dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="2886a-361">Otherwise, both operands are converted to type `int`.</span></span>

<span data-ttu-id="2886a-362">İlk kuralın `decimal` türünü karıştıraş tüm işlemlere `double` ve `float` türleriyle izin vermez.</span><span class="sxs-lookup"><span data-stu-id="2886a-362">Note that the first rule disallows any operations that mix the `decimal` type with the `double` and `float` types.</span></span> <span data-ttu-id="2886a-363">Kural, `decimal` türü ile `double` ve `float` türleri arasında örtük dönüştürme olmaması durumunda yer alan bir olay izler.</span><span class="sxs-lookup"><span data-stu-id="2886a-363">The rule follows from the fact that there are no implicit conversions between the `decimal` type and the `double` and `float` types.</span></span>

<span data-ttu-id="2886a-364">Ayrıca, diğer işlenen işaretli bir integral türü olduğunda bir işlenenin `ulong` türünden olması mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-364">Also note that it is not possible for an operand to be of type `ulong` when the other operand is of a signed integral type.</span></span> <span data-ttu-id="2886a-365">Bunun nedeni, `ulong` tam aralığını ve imzalı integral türlerini temsil eden hiçbir integral türü yok.</span><span class="sxs-lookup"><span data-stu-id="2886a-365">The reason is that no integral type exists that can represent the full range of `ulong` as well as the signed integral types.</span></span>

<span data-ttu-id="2886a-366">Yukarıdaki her iki durumda da, bir işleneni açıkça diğer işlenenle uyumlu bir türe dönüştürmek için bir atama ifadesi kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-366">In both of the above cases, a cast expression can be used to explicitly convert one operand to a type that is compatible with the other operand.</span></span>

<span data-ttu-id="2886a-367">Örnekte</span><span class="sxs-lookup"><span data-stu-id="2886a-367">In the example</span></span>
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
<span data-ttu-id="2886a-368">bir `decimal` `double`çarpamadığı için bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-368">a binding-time error occurs because a `decimal` cannot be multiplied by a `double`.</span></span> <span data-ttu-id="2886a-369">Hata, ikinci işleneni aşağıdaki gibi `decimal`olarak açıkça dönüştürerek çözümlenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-369">The error is resolved by explicitly converting the second operand to `decimal`, as follows:</span></span>

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a><span data-ttu-id="2886a-370">Yükseltilmemiş işleçleri</span><span class="sxs-lookup"><span data-stu-id="2886a-370">Lifted operators</span></span>

<span data-ttu-id="2886a-371">***Yükseltilmemiş işleçleri*** , null olamayan değer türlerinde çalışan, önceden tanımlanmış ve Kullanıcı tanımlı işleçlere izin vererek bu türlerin null yapılabilir formlarla de kullanılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="2886a-371">***Lifted operators*** permit predefined and user-defined operators that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="2886a-372">Yükseltilmemiş işleçleri, aşağıda açıklandığı gibi, belirli gereksinimleri karşılayan önceden tanımlı ve Kullanıcı tanımlı işleçlerden oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="2886a-372">Lifted operators are constructed from predefined and user-defined operators that meet certain requirements, as described in the following:</span></span>

*   <span data-ttu-id="2886a-373">Birli İşleçler için</span><span class="sxs-lookup"><span data-stu-id="2886a-373">For the unary operators</span></span>

    ```csharp
    +  ++  -  --  !  ~
    ```

    <span data-ttu-id="2886a-374">bir işlecin yükseltilmemiş formu, işlenen ve sonuç türleri null yapılamayan değer türleri ise vardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-374">a lifted form of an operator exists if the operand and result types are both non-nullable value types.</span></span> <span data-ttu-id="2886a-375">Yükseltilmemiş formu, işlenene ve sonuç türlerine tek bir `?` değiştiricisi eklenerek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-375">The lifted form is constructed by adding a single `?` modifier to the operand and result types.</span></span> <span data-ttu-id="2886a-376">Yükseltilmemiş işleci, işlenen null ise null değeri üretir.</span><span class="sxs-lookup"><span data-stu-id="2886a-376">The lifted operator produces a null value if the operand is null.</span></span> <span data-ttu-id="2886a-377">Aksi takdirde, yükseltilmemiş işleci işleneni geri sarar, temeldeki işleci uygular ve sonucu sarmalar.</span><span class="sxs-lookup"><span data-stu-id="2886a-377">Otherwise, the lifted operator unwraps the operand, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="2886a-378">İkili işleçler için</span><span class="sxs-lookup"><span data-stu-id="2886a-378">For the binary operators</span></span>

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    <span data-ttu-id="2886a-379">bir işlecin yükseltilmemiş formu, işlenen ve sonuç türleri null yapılamayan tüm değer türleri ise vardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-379">a lifted form of an operator exists if the operand and result types are all non-nullable value types.</span></span> <span data-ttu-id="2886a-380">Yükseltilmemiş formu, her işlenenin ve sonuç türünün tek bir `?` değiştiricisi eklenerek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-380">The lifted form is constructed by adding a single `?` modifier to each operand and result type.</span></span> <span data-ttu-id="2886a-381">Yükseltilmemiş işleci, bir veya her iki işlenen de null ise null değeri üretir ( [Boolean mantıksal işleçler](expressions.md#boolean-logical-operators)bölümünde açıklandığı gibi `bool?` türünün `&` ve `|` işleçleri).</span><span class="sxs-lookup"><span data-stu-id="2886a-381">The lifted operator produces a null value if one or both operands are null (an exception being the `&` and `|` operators of the `bool?` type, as described in [Boolean logical operators](expressions.md#boolean-logical-operators)).</span></span> <span data-ttu-id="2886a-382">Aksi halde, yükseltilmemiş işleci işlenenleri sarmalanmış, temeldeki işleci uygular ve sonucu sarmalar.</span><span class="sxs-lookup"><span data-stu-id="2886a-382">Otherwise, the lifted operator unwraps the operands, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="2886a-383">Eşitlik işleçleri için</span><span class="sxs-lookup"><span data-stu-id="2886a-383">For the equality operators</span></span>

    ```csharp
    ==  !=
    ```

    <span data-ttu-id="2886a-384">bir işlecin yükseltilmemiş formu, işlenen türleri hem null yapılamayan değer türleri hem de sonuç türü `bool`ise vardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-384">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="2886a-385">Yükseltilmemiş formu, her bir işlenen türüne tek bir `?` değiştiricisi eklenerek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-385">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="2886a-386">Yükseltilmemiş işleci iki null değeri eşit kabul eder ve null olmayan bir değer null değeri boş değer olarak eşit değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-386">The lifted operator considers two null values equal, and a null value unequal to any non-null value.</span></span> <span data-ttu-id="2886a-387">Her iki işlenen de null değilse, yükseltilmemiş işleci işlenenleri geri sarar ve `bool` sonucunu üretmek için temeldeki işleci uygular.</span><span class="sxs-lookup"><span data-stu-id="2886a-387">If both operands are non-null, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

*   <span data-ttu-id="2886a-388">İlişkisel işleçler için</span><span class="sxs-lookup"><span data-stu-id="2886a-388">For the relational operators</span></span>

    ```csharp
    <  >  <=  >=
    ```

    <span data-ttu-id="2886a-389">bir işlecin yükseltilmemiş formu, işlenen türleri hem null yapılamayan değer türleri hem de sonuç türü `bool`ise vardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-389">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="2886a-390">Yükseltilmemiş formu, her bir işlenen türüne tek bir `?` değiştiricisi eklenerek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-390">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="2886a-391">Bir veya her iki işlenen de null ise, yükseltilmemiş işleci `false` değeri üretir.</span><span class="sxs-lookup"><span data-stu-id="2886a-391">The lifted operator produces the value `false` if one or both operands are null.</span></span> <span data-ttu-id="2886a-392">Aksi halde, yükseltilmemiş işleci işlenenleri geri sarar ve `bool` sonucunu üretmek için temeldeki işleci uygular.</span><span class="sxs-lookup"><span data-stu-id="2886a-392">Otherwise, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

## <a name="member-lookup"></a><span data-ttu-id="2886a-393">Üye arama</span><span class="sxs-lookup"><span data-stu-id="2886a-393">Member lookup</span></span>

<span data-ttu-id="2886a-394">Üye arama, bir tür bağlamındaki bir adın anlamını tespit eden işlemdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-394">A member lookup is the process whereby the meaning of a name in the context of a type is determined.</span></span> <span data-ttu-id="2886a-395">Bir ifadede *simple_name* ([basit adlar](expressions.md#simple-names)) veya bir *member_access* ([üye erişimi](expressions.md#member-access)) değerlendirme işleminin parçası olarak bir üye arama gerçekleşebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-395">A member lookup can occur as part of evaluating a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)) in an expression.</span></span> <span data-ttu-id="2886a-396">*Simple_name* veya *member_access* bir *invocation_expression* ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)) *primary_expression* olarak gerçekleşirse, üyenin çağrılması söylenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-396">If the *simple_name* or *member_access* occurs as the *primary_expression* of an *invocation_expression* ([Method invocations](expressions.md#method-invocations)), the member is said to be invoked.</span></span>

<span data-ttu-id="2886a-397">Bir üye bir yöntem veya olaydır veya bir temsilci türünün ([Temsilciler](delegates.md)) ya da tür `dynamic` ([dinamik tür](types.md#the-dynamic-type)) bir sabit, alan veya özellik ise, üyenin *ıncable*olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-397">If a member is a method or event, or if it is a constant, field or property of either a delegate type ([Delegates](delegates.md)) or the type `dynamic` ([The dynamic type](types.md#the-dynamic-type)), then the member is said to be *invocable*.</span></span>

<span data-ttu-id="2886a-398">Üye arama yalnızca bir üyenin adını değil, üyenin sahip olduğu tür parametrelerinin sayısını ve üyenin erişilebilir olup olmadığını dikkate alır.</span><span class="sxs-lookup"><span data-stu-id="2886a-398">Member lookup considers not only the name of a member but also the number of type parameters the member has and whether the member is accessible.</span></span> <span data-ttu-id="2886a-399">Üye arama amaçları için, genel metotlar ve iç içe geçmiş genel türler ilgili bildirimlerinde belirtilen tür parametrelerinin sayısına ve diğer tüm üyelerin sıfır tür parametrelerine sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2886a-399">For the purposes of member lookup, generic methods and nested generic types have the number of type parameters indicated in their respective declarations and all other members have zero type parameters.</span></span>

<span data-ttu-id="2886a-400">Bir ada `N` bir ad `K` tür `T` parametreleri bir üye araması, aşağıdaki gibi işlenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-400">A member lookup of a name `N` with `K` type parameters in a type `T` is processed as follows:</span></span>

*  <span data-ttu-id="2886a-401">İlk olarak, `N` adlı bir erişilebilir üye kümesi belirlenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-401">First, a set of accessible members named `N` is determined:</span></span>
    * <span data-ttu-id="2886a-402">`T` bir tür parametresidir, küme, `T`için birincil kısıtlama veya ikincil kısıtlama ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) olarak belirtilen her türde `N` adlı erişilebilir üye kümelerinin birleşimidir. bu, `object` `N` adlı erişilebilir Üyeler kümesiyle birlikte.</span><span class="sxs-lookup"><span data-stu-id="2886a-402">If `T` is a type parameter, then the set is the union of the sets of accessible members named `N` in each of the types specified as a primary constraint or secondary constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) for `T`, along with the set of accessible members named `N` in `object`.</span></span>
    * <span data-ttu-id="2886a-403">Aksi takdirde, küme, devralınan Üyeler ve `object` `N` adlı erişilebilir Üyeler dahil olmak üzere `T` `N` adlı tüm erişilebilir ([üye erişim](basic-concepts.md#member-access)) üyelerinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-403">Otherwise, the set consists of all accessible ([Member access](basic-concepts.md#member-access)) members named `N` in `T`, including inherited members and the accessible members named `N` in `object`.</span></span> <span data-ttu-id="2886a-404">`T` oluşturulmuş bir türse, Üyeler kümesi, [oluşturulan türlerin üyelerinde](classes.md#members-of-constructed-types)açıklanan tür bağımsız değişkenlerini değiştirerek alınır.</span><span class="sxs-lookup"><span data-stu-id="2886a-404">If `T` is a constructed type, the set of members is obtained by substituting type arguments as described in [Members of constructed types](classes.md#members-of-constructed-types).</span></span> <span data-ttu-id="2886a-405">`override` değiştiricisi içeren Üyeler kümeden çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-405">Members that include an `override` modifier are excluded from the set.</span></span>
*  <span data-ttu-id="2886a-406">Sonra, `K` sıfır ise, bildirimleri tür parametreleri içeren tüm iç içe türler kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-406">Next, if `K` is zero, all nested types whose declarations include type parameters are removed.</span></span> <span data-ttu-id="2886a-407">`K` sıfır değilse, farklı sayıda tür parametrelerine sahip tüm Üyeler kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-407">If `K` is not zero, all members with a different number of type parameters are removed.</span></span> <span data-ttu-id="2886a-408">`K` sıfırdan olduğunda, tür çıkarımı işlemi ([tür çıkarımı](expressions.md#type-inference)) tür bağımsız değişkenlerini çıkarsanbileceği için tür parametrelerine sahip yöntemlerin kaldırılmadığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2886a-408">Note that when `K` is zero, methods having type parameters are not removed, since the type inference process ([Type inference](expressions.md#type-inference)) might be able to infer the type arguments.</span></span>
*  <span data-ttu-id="2886a-409">Sonra, üye *çağrılırsa*,*çağrılamayan* tüm Üyeler kümeden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-409">Next, if the member is *invoked*, all non-*invocable* members are removed from the set.</span></span>
*  <span data-ttu-id="2886a-410">Daha sonra, diğer Üyeler tarafından gizlenen Üyeler kümeden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-410">Next, members that are hidden by other members are removed from the set.</span></span> <span data-ttu-id="2886a-411">Küme içindeki her üye `S.M` için, `S` üye `M` bildirildiği türdür, aşağıdaki kurallar uygulanır:</span><span class="sxs-lookup"><span data-stu-id="2886a-411">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied:</span></span>
    * <span data-ttu-id="2886a-412">`M` bir sabit, alan, özellik, olay veya numaralandırma üyesiyse, `S` temel türünde belirtilen tüm Üyeler kümeden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-412">If `M` is a constant, field, property, event, or enumeration member, then all members declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="2886a-413">`M` bir tür bildirimidir, temel `S` türünde bildirilmemiş tüm türler kümeden kaldırılır ve `S` temel türünde bildirildiği `M` aynı sayıda tür parametrelerine sahip tüm tür bildirimleri kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-413">If `M` is a type declaration, then all non-types declared in a base type of `S` are removed from the set, and all type declarations with the same number of type parameters as `M` declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="2886a-414">`M` bir yöntem ise, temel türünde `S` belirtilen tüm yöntem olmayan Üyeler kümeden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-414">If `M` is a method, then all non-method members declared in a base type of `S` are removed from the set.</span></span>
*  <span data-ttu-id="2886a-415">Daha sonra, sınıf üyeleri tarafından gizlenen arabirim üyeleri kümeden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-415">Next, interface members that are hidden by class members are removed from the set.</span></span> <span data-ttu-id="2886a-416">Bu adım yalnızca `T` bir tür parametresidir ve `T` hem `object` dışındaki etkin bir temel sınıfa hem de boş olmayan bir etkin arabirim kümesine sahip ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) sahip olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-416">This step only has an effect if `T` is a type parameter and `T` has both an effective base class other than `object` and a non-empty effective interface set ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="2886a-417">Küme içindeki her üye `S.M` için `S` üye `M` bildirildiği türdür, `S` `object`dışında bir sınıf bildirimidir, aşağıdaki kurallar uygulanır:</span><span class="sxs-lookup"><span data-stu-id="2886a-417">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied if `S` is a class declaration other than `object`:</span></span>
    * <span data-ttu-id="2886a-418">`M` bir sabit, alan, özellik, olay, numaralandırma üyesi veya tür bildirimi ise, bir arabirim bildiriminde belirtilen tüm Üyeler kümeden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-418">If `M` is a constant, field, property, event, enumeration member, or type declaration, then all members declared in an interface declaration are removed from the set.</span></span>
    * <span data-ttu-id="2886a-419">`M` bir yöntem ise, bir arabirim bildiriminde belirtilen tüm yöntem olmayan Üyeler kümeden kaldırılır ve bir arabirim bildiriminde bildirildiği `M` aynı imzaya sahip tüm yöntemler kümeden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-419">If `M` is a method, then all non-method members declared in an interface declaration are removed from the set, and all methods with the same signature as `M` declared in an interface declaration are removed from the set.</span></span>
*  <span data-ttu-id="2886a-420">Son olarak, gizli üyeleri kaldırdıkça, aramanın sonucu belirlenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-420">Finally, having removed hidden members, the result of the lookup is determined:</span></span>
    * <span data-ttu-id="2886a-421">Küme, yöntem olmayan tek bir üyenin içeriyorsa, bu üye aramanın sonucudur.</span><span class="sxs-lookup"><span data-stu-id="2886a-421">If the set consists of a single member that is not a method, then this member is the result of the lookup.</span></span>
    * <span data-ttu-id="2886a-422">Aksi takdirde, küme yalnızca yöntemleri içeriyorsa, bu yöntem grubu aramanın sonucudur.</span><span class="sxs-lookup"><span data-stu-id="2886a-422">Otherwise, if the set contains only methods, then this group of methods is the result of the lookup.</span></span>
    * <span data-ttu-id="2886a-423">Aksi takdirde, arama belirsizdir ve bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-423">Otherwise, the lookup is ambiguous, and a binding-time error occurs.</span></span>

<span data-ttu-id="2886a-424">Tür parametreleri ve arabirimleri dışındaki türlerde üye aramaları ve kesinlikle tek devralma olan arabirimlerde üye aramaları için (devralma zincirindeki her arabirim tam olarak sıfır veya bir doğrudan temel arabirimine sahiptir), arama kurallarının etkisi yalnızca bu türetilmiş Üyeler aynı ad veya imzaya sahip temel üyeleri gizler.</span><span class="sxs-lookup"><span data-stu-id="2886a-424">For member lookups in types other than type parameters and interfaces, and member lookups in interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effect of the lookup rules is simply that derived members hide base members with the same name or signature.</span></span> <span data-ttu-id="2886a-425">Bu tür tek devralma aramaları hiçbir şekilde belirsiz değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-425">Such single-inheritance lookups are never ambiguous.</span></span> <span data-ttu-id="2886a-426">Birden çok devralma arabirimindeki üye aramalarından muhtemelen olabilecek belirsizlikleri, [arabirim üye erişimi](interfaces.md#interface-member-access)bölümünde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2886a-426">The ambiguities that can possibly arise from member lookups in multiple-inheritance interfaces are described in [Interface member access](interfaces.md#interface-member-access).</span></span>

### <a name="base-types"></a><span data-ttu-id="2886a-427">Temel türler</span><span class="sxs-lookup"><span data-stu-id="2886a-427">Base types</span></span>

<span data-ttu-id="2886a-428">Üye arama amacıyla bir tür `T` aşağıdaki temel türlere sahip olacak şekilde değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-428">For purposes of member lookup, a type `T` is considered to have the following base types:</span></span>

*  <span data-ttu-id="2886a-429">`T` `object`, `T` temel tür içermez.</span><span class="sxs-lookup"><span data-stu-id="2886a-429">If `T` is `object`, then `T` has no base type.</span></span>
*  <span data-ttu-id="2886a-430">`T` bir *enum_type*, `T` taban türleri `System.Enum`, `System.ValueType`ve `object`sınıf türleridir.</span><span class="sxs-lookup"><span data-stu-id="2886a-430">If `T` is an *enum_type*, the base types of `T` are the class types `System.Enum`, `System.ValueType`, and `object`.</span></span>
*  <span data-ttu-id="2886a-431">`T` bir *struct_type*, `T` taban türleri `System.ValueType` ve `object`sınıflardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-431">If `T` is a *struct_type*, the base types of `T` are the class types `System.ValueType` and `object`.</span></span>
*  <span data-ttu-id="2886a-432">`T` *class_type*`T` temel türleri, sınıf türü `object`dahil `T`temel sınıflarıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-432">If `T` is a *class_type*, the base types of `T` are the base classes of `T`, including the class type `object`.</span></span>
*  <span data-ttu-id="2886a-433">`T` bir *interface_type*, `T` taban türleri `T` temel arayüzlerdir ve `object`sınıf türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-433">If `T` is an *interface_type*, the base types of `T` are the base interfaces of `T` and the class type `object`.</span></span>
*  <span data-ttu-id="2886a-434">`T` bir *array_type*, `T` taban türleri `System.Array` ve `object`sınıflardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-434">If `T` is an *array_type*, the base types of `T` are the class types `System.Array` and `object`.</span></span>
*  <span data-ttu-id="2886a-435">`T` bir *delegate_type*, `T` taban türleri `System.Delegate` ve `object`sınıflardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-435">If `T` is a *delegate_type*, the base types of `T` are the class types `System.Delegate` and `object`.</span></span>

## <a name="function-members"></a><span data-ttu-id="2886a-436">İşlev üyeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-436">Function members</span></span>

<span data-ttu-id="2886a-437">İşlev üyeleri, çalıştırılabilir deyimler içeren üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="2886a-437">Function members are members that contain executable statements.</span></span> <span data-ttu-id="2886a-438">İşlev üyeleri her zaman türlerin üyeleridir ve ad alanlarının üyeleri olamaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-438">Function members are always members of types and cannot be members of namespaces.</span></span> <span data-ttu-id="2886a-439">C#Aşağıdaki işlev üyesi kategorilerini tanımlar:</span><span class="sxs-lookup"><span data-stu-id="2886a-439">C# defines the following categories of function members:</span></span>

*  <span data-ttu-id="2886a-440">Yöntemler</span><span class="sxs-lookup"><span data-stu-id="2886a-440">Methods</span></span>
*  <span data-ttu-id="2886a-441">Özellikler</span><span class="sxs-lookup"><span data-stu-id="2886a-441">Properties</span></span>
*  <span data-ttu-id="2886a-442">Olaylar</span><span class="sxs-lookup"><span data-stu-id="2886a-442">Events</span></span>
*  <span data-ttu-id="2886a-443">Dizin Oluşturucular</span><span class="sxs-lookup"><span data-stu-id="2886a-443">Indexers</span></span>
*  <span data-ttu-id="2886a-444">Kullanıcı tanımlı işleçler</span><span class="sxs-lookup"><span data-stu-id="2886a-444">User-defined operators</span></span>
*  <span data-ttu-id="2886a-445">Örnek oluşturucular</span><span class="sxs-lookup"><span data-stu-id="2886a-445">Instance constructors</span></span>
*  <span data-ttu-id="2886a-446">Statik oluşturucular</span><span class="sxs-lookup"><span data-stu-id="2886a-446">Static constructors</span></span>
*  <span data-ttu-id="2886a-447">Yıkıcılar</span><span class="sxs-lookup"><span data-stu-id="2886a-447">Destructors</span></span>

<span data-ttu-id="2886a-448">Yok ediciler ve statik oluşturucular dışında (açıkça çağrılamaz), işlev üyelerinde bulunan deyimler işlev üyesi etkinleştirmeleri aracılığıyla yürütülür.</span><span class="sxs-lookup"><span data-stu-id="2886a-448">Except for destructors and static constructors (which cannot be invoked explicitly), the statements contained in function members are executed through function member invocations.</span></span> <span data-ttu-id="2886a-449">İşlev üye çağrısı yazmak için gerçek sözdizimi, belirli bir işlev üyesi kategorisine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-449">The actual syntax for writing a function member invocation depends on the particular function member category.</span></span>

<span data-ttu-id="2886a-450">İşlev üyesi çağırma işlevinin bağımsız değişken listesi ([bağımsız değişken listeleri](expressions.md#argument-lists)), işlev üyesinin parametreleri için gerçek değerler veya değişken başvuruları sağlar.</span><span class="sxs-lookup"><span data-stu-id="2886a-450">The argument list ([Argument lists](expressions.md#argument-lists)) of a function member invocation provides actual values or variable references for the parameters of the function member.</span></span>

<span data-ttu-id="2886a-451">Genel yöntemlerin etkinleştirmeleri, yönteme geçirilecek tür bağımsız değişkenleri kümesini belirlemede tür çıkarımı kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-451">Invocations of generic methods may employ type inference to determine the set of type arguments to pass to the method.</span></span> <span data-ttu-id="2886a-452">Bu işlem [tür çıkarımı](expressions.md#type-inference)bölümünde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="2886a-452">This process is described in [Type inference](expressions.md#type-inference).</span></span>

<span data-ttu-id="2886a-453">Yöntemler, Dizin oluşturucular, işleçler ve örnek oluşturucuların çağırmaları, hangi bir tür üye kümesinin çalıştırılacağını belirleyen aşırı yükleme çözümlemesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="2886a-453">Invocations of methods, indexers, operators and instance constructors employ overload resolution to determine which of a candidate set of function members to invoke.</span></span> <span data-ttu-id="2886a-454">Bu işlem [aşırı yükleme çözümlemesi](expressions.md#overload-resolution)bölümünde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="2886a-454">This process is described in [Overload resolution](expressions.md#overload-resolution).</span></span>

<span data-ttu-id="2886a-455">Bağlama zamanında belirli bir işlev üyesi tanımlandıktan sonra, muhtemelen aşırı yükleme çözünürlüğü aracılığıyla işlev üyesini çağırma işleminin gerçek çalışma zamanı [denetimi, dinamik aşırı yükleme çözümünün derleme zamanı denetiminde](expressions.md#compile-time-checking-of-dynamic-overload-resolution)açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="2886a-455">Once a particular function member has been identified at binding-time, possibly through overload resolution, the actual run-time process of invoking the function member is described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="2886a-456">Aşağıdaki tabloda, açıkça çağrılabilen işlev üyelerinin altı kategorisini içeren yapılar içinde gerçekleşen işlem özetlenmektedir.</span><span class="sxs-lookup"><span data-stu-id="2886a-456">The following table summarizes the processing that takes place in constructs involving the six categories of function members that can be explicitly invoked.</span></span> <span data-ttu-id="2886a-457">`e`, `x`, `y`ve `value` değişkenler veya değerler olarak sınıflandırılan ifadeleri gösterir, `T` tür olarak sınıflandırılan bir ifadeyi belirtir, `F` yöntemin basit adıdır ve `P` bir özelliğin basit adıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-457">In the table, `e`, `x`, `y`, and `value` indicate expressions classified as variables or values, `T` indicates an expression classified as a type, `F` is the simple name of a method, and `P` is the simple name of a property.</span></span>


| <span data-ttu-id="2886a-458">__Oluşturma__</span><span class="sxs-lookup"><span data-stu-id="2886a-458">__Construct__</span></span>     | <span data-ttu-id="2886a-459">__Örnek__</span><span class="sxs-lookup"><span data-stu-id="2886a-459">__Example__</span></span>    | <span data-ttu-id="2886a-460">__Açıklama__</span><span class="sxs-lookup"><span data-stu-id="2886a-460">__Description__</span></span> |
|-------------------|----------------|-----------------|
| <span data-ttu-id="2886a-461">Yöntem çağırma</span><span class="sxs-lookup"><span data-stu-id="2886a-461">Method invocation</span></span> | `F(x,y)`       | <span data-ttu-id="2886a-462">Aşırı yükleme çözümlemesi, kapsayan sınıf veya yapıda en iyi yöntem `F` seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-462">Overload resolution is applied to select the best method `F` in the containing class or struct.</span></span> <span data-ttu-id="2886a-463">Yöntemi, `(x,y)`bağımsız değişken listesiyle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-463">The method is invoked with the argument list `(x,y)`.</span></span> <span data-ttu-id="2886a-464">Yöntem `static`değilse, örnek ifadesi `this`.</span><span class="sxs-lookup"><span data-stu-id="2886a-464">If the method is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.F(x,y)`     | <span data-ttu-id="2886a-465">Sınıf veya yapı `T`en iyi yöntem `F` seçmek için aşırı yükleme çözümlemesi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-465">Overload resolution is applied to select the best method `F` in the class or struct `T`.</span></span> <span data-ttu-id="2886a-466">Yöntem `static`değilse bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-466">A binding-time error occurs if the method is not `static`.</span></span> <span data-ttu-id="2886a-467">Yöntemi, `(x,y)`bağımsız değişken listesiyle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-467">The method is invoked with the argument list `(x,y)`.</span></span> | 
|                   | `e.F(x,y)`     | <span data-ttu-id="2886a-468">Sınıf, yapı veya `e`türü tarafından verilen arabirimdeki en iyi yöntem F 'yi seçmek için aşırı yükleme çözümlemesi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-468">Overload resolution is applied to select the best method F in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="2886a-469">Yöntem `static`bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-469">A binding-time error occurs if the method is `static`.</span></span> <span data-ttu-id="2886a-470">Yöntemi, `e` örnek ifadesiyle çağrılır ve bağımsız değişken listesi `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="2886a-470">The method is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="2886a-471">Özellik erişimi</span><span class="sxs-lookup"><span data-stu-id="2886a-471">Property access</span></span>   | `P`            | <span data-ttu-id="2886a-472">Özelliğin `get` erişimcisi, kapsayan sınıf veya yapıda `P` çağrıldı.</span><span class="sxs-lookup"><span data-stu-id="2886a-472">The `get` accessor of the property `P` in the containing class or struct is invoked.</span></span> <span data-ttu-id="2886a-473">`P` salt yazılır olduğunda bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-473">A compile-time error occurs if `P` is write-only.</span></span> <span data-ttu-id="2886a-474">`P` `static`değilse, örnek ifadesi `this`.</span><span class="sxs-lookup"><span data-stu-id="2886a-474">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `P = value`    | <span data-ttu-id="2886a-475">`set` erişimcisi, kapsayan sınıf veya yapıda `P`, `(value)`bağımsız değişken listesi ile çağırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-475">The `set` accessor of the property `P` in the containing class or struct is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="2886a-476">`P` salt okunurdur derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-476">A compile-time error occurs if `P` is read-only.</span></span> <span data-ttu-id="2886a-477">`P` `static`değilse, örnek ifadesi `this`.</span><span class="sxs-lookup"><span data-stu-id="2886a-477">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.P`          | <span data-ttu-id="2886a-478">Sınıf veya yapı `T` `P` özelliğin `get` erişimcisi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-478">The `get` accessor of the property `P` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="2886a-479">`P` `static` değilse veya `P` salt yazılır ise, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-479">A compile-time error occurs if `P` is not `static` or if `P` is write-only.</span></span> | 
|                   | `T.P = value`  | <span data-ttu-id="2886a-480">Sınıf veya yapı `T` `P` özelliğin `set` erişimcisi, `(value)`bağımsız değişken listesi ile çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-480">The `set` accessor of the property `P` in the class or struct `T` is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="2886a-481">`P` `static` değilse veya `P` salt okunurdur bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-481">A compile-time error occurs if `P` is not `static` or if `P` is read-only.</span></span> | 
|                   | `e.P`          | <span data-ttu-id="2886a-482">Sınıf, yapı veya `e` türü tarafından verilen arabirimdeki `P` özelliğin `get` erişimcisi, `e`örneği ifadesiyle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-482">The `get` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="2886a-483">`P` `static` veya `P` salt yazılır ise bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-483">A binding-time error occurs if `P` is `static` or if `P` is write-only.</span></span> | 
|                   | `e.P = value`  | <span data-ttu-id="2886a-484">Sınıf, yapı veya `e` türü tarafından verilen arabirimdeki `P` özelliğin `set` erişimcisi, örnek ifadesi `e` ve bağımsız değişken listesi `(value)`ile çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-484">The `set` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e` and the argument list `(value)`.</span></span> <span data-ttu-id="2886a-485">`P` `static` veya `P` salt okunurdur bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-485">A binding-time error occurs if `P` is `static` or if `P` is read-only.</span></span> | 
| <span data-ttu-id="2886a-486">Olay erişimi</span><span class="sxs-lookup"><span data-stu-id="2886a-486">Event access</span></span>      | `E += value`   | <span data-ttu-id="2886a-487">Kapsayan sınıf veya yapı içindeki olay `E` `add` erişimcisi çağrıldı.</span><span class="sxs-lookup"><span data-stu-id="2886a-487">The `add` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="2886a-488">`E` statik değilse, örnek ifadesi `this`.</span><span class="sxs-lookup"><span data-stu-id="2886a-488">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `E -= value`   | <span data-ttu-id="2886a-489">Kapsayan sınıf veya yapı içindeki olay `E` `remove` erişimcisi çağrıldı.</span><span class="sxs-lookup"><span data-stu-id="2886a-489">The `remove` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="2886a-490">`E` statik değilse, örnek ifadesi `this`.</span><span class="sxs-lookup"><span data-stu-id="2886a-490">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `T.E += value` | <span data-ttu-id="2886a-491">Sınıf veya yapı `T` içindeki olay `E` `add` erişimcisi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-491">The `add` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="2886a-492">`E` statik değilse bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-492">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `T.E -= value` | <span data-ttu-id="2886a-493">Sınıf veya yapı `T` içindeki olay `E` `remove` erişimcisi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-493">The `remove` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="2886a-494">`E` statik değilse bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-494">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `e.E += value` | <span data-ttu-id="2886a-495">Sınıf, yapı veya `e` türü tarafından verilen arabirimdeki olay `E` `add` erişimcisi, `e`örnek ifadesiyle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-495">The `add` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="2886a-496">`E` statik olduğunda bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-496">A binding-time error occurs if `E` is static.</span></span> | 
|                   | `e.E -= value` | <span data-ttu-id="2886a-497">Sınıf, yapı veya `e` türü tarafından verilen arabirimdeki olay `E` `remove` erişimcisi, `e`örnek ifadesiyle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-497">The `remove` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="2886a-498">`E` statik olduğunda bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-498">A binding-time error occurs if `E` is static.</span></span> | 
| <span data-ttu-id="2886a-499">Dizin Oluşturucu erişimi</span><span class="sxs-lookup"><span data-stu-id="2886a-499">Indexer access</span></span>    | `e[x,y]`       | <span data-ttu-id="2886a-500">Aşırı yükleme çözümlemesi, sınıf, yapı veya e-postayla verilen arabirimdeki en iyi Dizin oluşturucuyu seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-500">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of e.</span></span> <span data-ttu-id="2886a-501">Dizin oluşturucunun `get` erişimcisi, `e` örnek ifadesiyle ve bağımsız değişken listesi `(x,y)`çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-501">The `get` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> <span data-ttu-id="2886a-502">Dizin Oluşturucu salt yazılır ise bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-502">A binding-time error occurs if the indexer is write-only.</span></span> | 
|                   | `e[x,y] = value` | <span data-ttu-id="2886a-503">Sınıf, yapı veya `e`türü tarafından verilen arabirimdeki en iyi Dizin oluşturucuyu seçmek için aşırı yükleme çözümlemesi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-503">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="2886a-504">Dizin oluşturucunun `set` erişimcisi, `e` örnek ifadesiyle ve bağımsız değişken listesi `(x,y,value)`çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-504">The `set` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y,value)`.</span></span> <span data-ttu-id="2886a-505">Dizin Oluşturucu salt okunurdur bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-505">A binding-time error occurs if the indexer is read-only.</span></span> | 
| <span data-ttu-id="2886a-506">İşleç çağırma</span><span class="sxs-lookup"><span data-stu-id="2886a-506">Operator invocation</span></span> | `-x`         | <span data-ttu-id="2886a-507">`x`türü tarafından verilen sınıfta veya yapıda en iyi birli işleci seçmek için aşırı yükleme çözümlemesi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-507">Overload resolution is applied to select the best unary operator in the class or struct given by the type of `x`.</span></span> <span data-ttu-id="2886a-508">Seçili operatör `(x)`bağımsız değişken listesiyle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-508">The selected operator is invoked with the argument list `(x)`.</span></span> | 
|                     | `x + y`      | <span data-ttu-id="2886a-509">`x` ve `y`türleri tarafından verilen sınıflarda veya yapılarda en iyi ikili işleci seçmek için aşırı yükleme çözümlemesi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-509">Overload resolution is applied to select the best binary operator in the classes or structs given by the types of `x` and `y`.</span></span> <span data-ttu-id="2886a-510">Seçili operatör `(x,y)`bağımsız değişken listesiyle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-510">The selected operator is invoked with the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="2886a-511">Örnek Oluşturucu çağrısı</span><span class="sxs-lookup"><span data-stu-id="2886a-511">Instance constructor invocation</span></span> | `new T(x,y)` | <span data-ttu-id="2886a-512">Sınıf veya yapı `T`en iyi örnek oluşturucusunu seçmek için aşırı yükleme çözümlemesi uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-512">Overload resolution is applied to select the best instance constructor in the class or struct `T`.</span></span> <span data-ttu-id="2886a-513">Örnek Oluşturucu `(x,y)`bağımsız değişken listesiyle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-513">The instance constructor is invoked with the argument list `(x,y)`.</span></span> | 

### <a name="argument-lists"></a><span data-ttu-id="2886a-514">Bağımsız değişken listeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-514">Argument lists</span></span>

<span data-ttu-id="2886a-515">Her işlev üyesi ve temsilci çağrısı, işlev üyesinin parametreleri için gerçek değerler veya değişken başvuruları sağlayan bir bağımsız değişken listesi içerir.</span><span class="sxs-lookup"><span data-stu-id="2886a-515">Every function member and delegate invocation includes an argument list which provides actual values or variable references for the parameters of the function member.</span></span> <span data-ttu-id="2886a-516">İşlev üyesi çağrısının bağımsız değişken listesini belirtmek için sözdizimi, işlev üyesi kategorisine bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="2886a-516">The syntax for specifying the argument list of a function member invocation depends on the function member category:</span></span>

*  <span data-ttu-id="2886a-517">Örnek oluşturucular, Yöntemler, Dizin oluşturucular ve temsilciler için bağımsız değişkenler, aşağıda açıklandığı gibi *argument_list*olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-517">For instance constructors, methods, indexers and delegates, the arguments are specified as an *argument_list*, as described below.</span></span> <span data-ttu-id="2886a-518">Dizin oluşturucular için, `set` erişimcisini çağırırken bağımsız değişken listesi, atama işlecinin sağ işleneni olarak belirtilen ifadeyi de içerir.</span><span class="sxs-lookup"><span data-stu-id="2886a-518">For indexers, when invoking the `set` accessor, the argument list additionally includes the expression specified as the right operand of the assignment operator.</span></span>
*  <span data-ttu-id="2886a-519">Özellikler için, `get` erişimcisi çağrılırken bağımsız değişken listesi boştur ve `set` erişimcisi çağrılırken atama işlecinin sağ işleneni olarak belirtilen ifadeden oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-519">For properties, the argument list is empty when invoking the `get` accessor, and consists of the expression specified as the right operand of the assignment operator when invoking the `set` accessor.</span></span>
*  <span data-ttu-id="2886a-520">Olaylar için bağımsız değişken listesi, `+=` veya `-=` işlecinin sağ işleneni olarak belirtilen ifadeden oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-520">For events, the argument list consists of the expression specified as the right operand of the `+=` or `-=` operator.</span></span>
*  <span data-ttu-id="2886a-521">Kullanıcı tanımlı işleçler için bağımsız değişken listesi birli işlecin tek işleneninden veya ikili işlecinin iki işleneninden oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-521">For user-defined operators, the argument list consists of the single operand of the unary operator or the two operands of the binary operator.</span></span>

<span data-ttu-id="2886a-522">Özelliklerinin bağımsız değişkenleri ([Özellikler](classes.md#properties)), Olaylar ([Olaylar](classes.md#events)) ve Kullanıcı tanımlı işleçler ([işleçler](classes.md#operators)) her zaman değer parametreleri olarak geçirilir ([değer parametreleri](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="2886a-522">The arguments of properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), and user-defined operators ([Operators](classes.md#operators)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)).</span></span> <span data-ttu-id="2886a-523">Dizin oluşturucularının bağımsız değişkenleri ([Dizin oluşturucular](classes.md#indexers)) her zaman değer parametreleri ([değer parametreleri](classes.md#value-parameters)) veya parametre dizileri ([parametre dizileri](classes.md#parameter-arrays)) olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-523">The arguments of indexers ([Indexers](classes.md#indexers)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)) or parameter arrays ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="2886a-524">Başvuru ve çıkış parametreleri bu işlev üyesi kategorileri için desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-524">Reference and output parameters are not supported for these categories of function members.</span></span>

<span data-ttu-id="2886a-525">Örnek oluşturucusunun, yöntemin, dizin oluşturucunun veya temsilci çağrısının bağımsız değişkenleri *argument_list*olarak belirtilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-525">The arguments of an instance constructor, method, indexer or delegate invocation are specified as an *argument_list*:</span></span>

```antlr
argument_list
    : argument (',' argument)*
    ;

argument
    : argument_name? argument_value
    ;

argument_name
    : identifier ':'
    ;

argument_value
    : expression
    | 'ref' variable_reference
    | 'out' variable_reference
    ;
```

<span data-ttu-id="2886a-526">*Argument_list* , virgülle ayrılmış bir veya daha fazla *bağımsız değişkenden*oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-526">An *argument_list* consists of one or more *argument*s, separated by commas.</span></span> <span data-ttu-id="2886a-527">Her bağımsız değişken, bir *argument_value*ve ardından bir isteğe bağlı *argument_name* oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-527">Each argument consists of an optional  *argument_name* followed by an *argument_value*.</span></span> <span data-ttu-id="2886a-528">*Argument_name* bir *bağımsız* değişken ***adlandırılmış bağımsız değişken***olarak adlandırılır, ancak *argument_name* olmayan bir *bağımsız* değişken ***konumsal bir bağımsız***değişkendir.</span><span class="sxs-lookup"><span data-stu-id="2886a-528">An *argument* with an *argument_name* is referred to as a ***named argument***, whereas an *argument* without an *argument_name* is a ***positional argument***.</span></span> <span data-ttu-id="2886a-529">Konumsal bir bağımsız değişkenin bir *argument_list*adlandırılmış bir bağımsız değişkenden sonra görünmesi hatadır.</span><span class="sxs-lookup"><span data-stu-id="2886a-529">It is an error for a positional argument to appear after a named argument in an *argument_list*.</span></span>

<span data-ttu-id="2886a-530">*Argument_value* aşağıdaki formlardan birini alabilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-530">The *argument_value* can take one of the following forms:</span></span>

*  <span data-ttu-id="2886a-531">Bağımsız değişkenin değer parametresi olarak geçtiğini belirten bir *ifade*([değer parametreleri](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="2886a-531">An *expression*, indicating that the argument is passed as a value parameter ([Value parameters](classes.md#value-parameters)).</span></span>
*  <span data-ttu-id="2886a-532">Anahtar sözcüğü, bağımsız değişkenin bir başvuru parametresi ([başvuru parametreleri](classes.md#reference-parameters)) olarak geçtiğini belirten bir *variable_reference* ([değişken başvuruları](variables.md#variable-references)) `ref`.</span><span class="sxs-lookup"><span data-stu-id="2886a-532">The keyword `ref` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as a reference parameter ([Reference parameters](classes.md#reference-parameters)).</span></span> <span data-ttu-id="2886a-533">Bir değişken başvuru parametresi olarak geçirilebilmesi için kesinlikle atanmalı ([kesin atama](variables.md#definite-assignment)).</span><span class="sxs-lookup"><span data-stu-id="2886a-533">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter.</span></span> <span data-ttu-id="2886a-534">Anahtar sözcüğü, bağımsız değişkenin çıkış parametresi olarak geçtiğini belirten bir *variable_reference* ([değişken başvuruları](variables.md#variable-references)[) `out`](classes.md#output-parameters).</span><span class="sxs-lookup"><span data-stu-id="2886a-534">The keyword `out` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as an output parameter ([Output parameters](classes.md#output-parameters)).</span></span> <span data-ttu-id="2886a-535">Değişken, değişkenin çıkış parametresi olarak geçirildiği bir işlev üyesi çağrısı sonrasında kesin olarak atanmış ([kesin atama](variables.md#definite-assignment)) olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-535">A variable is considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) following a function member invocation in which the variable is passed as an output parameter.</span></span>

#### <a name="corresponding-parameters"></a><span data-ttu-id="2886a-536">Karşılık gelen parametreler</span><span class="sxs-lookup"><span data-stu-id="2886a-536">Corresponding parameters</span></span>

<span data-ttu-id="2886a-537">Bir bağımsız değişken listesindeki her bağımsız değişken için, çağrılan işlev üyesinde veya temsilde karşılık gelen bir parametre olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-537">For each argument in an argument list there has to be a corresponding parameter in the function member or delegate being invoked.</span></span>

<span data-ttu-id="2886a-538">Aşağıda kullanılan parametre listesi aşağıdaki şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-538">The parameter list used in the following is determined as follows:</span></span>

*  <span data-ttu-id="2886a-539">Sınıflarda tanımlı sanal yöntemler ve Dizin oluşturucular için, parametre listesi, alıcı türü ve temel sınıfları arasında arama yaparak işlev üyesinin en belirgin bildiriminden veya geçersiz kılınmadan çekilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-539">For virtual methods and indexers defined in classes, the parameter list is picked from the most specific declaration or override of the function member, starting with the static type of the receiver, and searching through its base classes.</span></span>
*  <span data-ttu-id="2886a-540">Arabirim yöntemleri ve Dizin oluşturucular için, parametre listesi, arabirim türü ile başlayıp temel arabirimler üzerinden arama yaparak üyenin en belirli tanımını oluşturacak şekilde çekilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-540">For interface methods and indexers, the parameter list is picked form the most specific definition of the member, starting with the interface type and searching through the base interfaces.</span></span> <span data-ttu-id="2886a-541">Benzersiz bir parametre listesi bulunamazsa, erişilemeyen adlara sahip bir parametre listesi ve isteğe bağlı parametre oluşturulur; bu nedenle, etkinleştirmeleri adlandırılmış parametreleri kullanamaz veya isteğe bağlı bağımsız değişkenleri alamaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-541">If no unique parameter list is found, a parameter list with inaccessible names and no optional parameters is constructed, so that invocations cannot use named parameters or omit optional arguments.</span></span>
*  <span data-ttu-id="2886a-542">Kısmi yöntemler için, tanımlayan kısmi Yöntem bildiriminin parametre listesi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-542">For partial methods, the parameter list of the defining partial method declaration is used.</span></span>
*  <span data-ttu-id="2886a-543">Diğer tüm işlev üyeleri ve temsilciler için yalnızca tek bir parametre listesi bulunur ve bu, kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-543">For all other function members and delegates there is only a single parameter list, which is the one used.</span></span>

<span data-ttu-id="2886a-544">Bir bağımsız değişkenin veya parametrenin konumu, bağımsız değişken listesi veya parametre listesinde kendisinden önceki bağımsız değişken veya parametre sayısı olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-544">The position of an argument or parameter is defined as the number of arguments or parameters preceding it in the argument list or parameter list.</span></span>

<span data-ttu-id="2886a-545">İşlev üyesi bağımsız değişkenleri için karşılık gelen parametreler şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="2886a-545">The corresponding parameters for function member arguments are established as follows:</span></span>

*  <span data-ttu-id="2886a-546">Örnek oluşturucular, Yöntemler, Dizin oluşturucular ve temsilciler *argument_list* bağımsız değişkenler:</span><span class="sxs-lookup"><span data-stu-id="2886a-546">Arguments in the *argument_list* of instance constructors, methods, indexers and delegates:</span></span>
    * <span data-ttu-id="2886a-547">Parametre listesindeki aynı konumda sabit bir parametrenin gerçekleştiği konumsal bir bağımsız değişken bu parametreye karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-547">A positional argument where a fixed parameter occurs at the same position in the parameter list corresponds to that parameter.</span></span>
    * <span data-ttu-id="2886a-548">Normal biçiminde çağrılan bir parametre dizisi olan bir işlev üyesinin Konumsal bağımsız değişkeni, parametre listesinde aynı konumda olması gereken parametre dizisine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-548">A positional argument of a function member with a parameter array invoked in its normal form corresponds to the parameter  array, which must occur at the same position in the parameter list.</span></span>
    * <span data-ttu-id="2886a-549">Bir parametre dizisi olan bir işlev üyesinin, parametre listesinde aynı konumda hiçbir sabit parametre gerçekleşmediği, parametre dizisindeki bir öğeye karşılık gelen bir konum bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="2886a-549">A positional argument of a function member with a parameter array invoked in its expanded form, where no fixed parameter occurs at the same position in the parameter list, corresponds to an element in the parameter array.</span></span>
    * <span data-ttu-id="2886a-550">Adlandırılmış bir bağımsız değişken, parametre listesinde aynı ada sahip parametreye karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-550">A named argument corresponds to the parameter of the same name in the parameter list.</span></span>
    * <span data-ttu-id="2886a-551">Dizin oluşturucular için, `set` erişimcisi çağrılırken, atama işlecinin sağ işleneni olarak belirtilen ifade, `set` erişimci bildiriminin örtük `value` parametresine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-551">For indexers, when invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="2886a-552">Özellikler için `get` erişimcisi çağrılırken bağımsız değişken yoktur.</span><span class="sxs-lookup"><span data-stu-id="2886a-552">For properties, when invoking the `get` accessor there are no arguments.</span></span> <span data-ttu-id="2886a-553">`set` erişimcisi çağrılırken, atama işlecinin sağ işleneni olarak belirtilen ifade, `set` erişimci bildiriminin örtük `value` parametresine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-553">When invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="2886a-554">Kullanıcı tanımlı Birli İşleçler (dönüşümler dahil) için, tek işlenen işleç bildiriminin tek parametresine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-554">For user-defined unary operators (including conversions), the single operand corresponds to the single parameter of the operator declaration.</span></span>
*  <span data-ttu-id="2886a-555">Kullanıcı tanımlı ikili işleçler için, sol işlenen ilk parametreye karşılık gelir ve sağ işlenen işleç bildiriminin ikinci parametresine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-555">For user-defined binary operators, the left operand corresponds to the first parameter, and the right operand corresponds to the second parameter of the operator declaration.</span></span>

#### <a name="run-time-evaluation-of-argument-lists"></a><span data-ttu-id="2886a-556">Bağımsız değişken listelerinin çalışma süresi değerlendirmesi</span><span class="sxs-lookup"><span data-stu-id="2886a-556">Run-time evaluation of argument lists</span></span>

<span data-ttu-id="2886a-557">Bir işlev üye çağrısının çalışma zamanı işlemesi sırasında ([dinamik aşırı yükleme çözümlemesi Için derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), bir bağımsız değişken listesinin ifadeleri ya da değişken başvuruları, soldan sağa aşağıdaki gibi sırayla değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-557">During the run-time processing of a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), the expressions or variable references of an argument list are evaluated in order, from left to right, as follows:</span></span>

*  <span data-ttu-id="2886a-558">Bir değer parametresi için bağımsız değişken ifadesi değerlendirilir ve ilgili parametre türüne örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-558">For a value parameter, the argument expression is evaluated and an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to the corresponding parameter type is performed.</span></span> <span data-ttu-id="2886a-559">Sonuç değeri, işlev üye çağrısında değer parametresinin başlangıç değeri olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-559">The resulting value becomes the initial value of the value parameter in the function member invocation.</span></span>
*  <span data-ttu-id="2886a-560">Bir başvuru veya çıkış parametresi için, değişken başvurusu değerlendirilir ve sonuçta elde edilen depolama konumu, işlev üyesi çağrısında parametresi tarafından temsil edilen depolama konumu olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-560">For a reference or output parameter, the variable reference is evaluated and the resulting storage location becomes the storage location represented by the parameter in the function member invocation.</span></span> <span data-ttu-id="2886a-561">Başvuru veya çıkış parametresi olarak verilen değişken başvurusu bir *reference_type*dizi öğesi ise, dizinin öğe türünün parametrenin türüyle aynı olduğundan emin olmak için bir çalışma zamanı denetimi yapılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-561">If the variable reference given as a reference or output parameter is an array element of a *reference_type*, a run-time check is performed to ensure that the element type of the array is identical to the type of the parameter.</span></span> <span data-ttu-id="2886a-562">Bu denetim başarısız olursa, bir `System.ArrayTypeMismatchException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-562">If this check fails, a `System.ArrayTypeMismatchException` is thrown.</span></span>

<span data-ttu-id="2886a-563">Yöntemler, Dizin oluşturucular ve örnek oluşturucular, en sağdaki parametreleri parametre dizisi ([parametre dizileri](classes.md#parameter-arrays)) olarak bildirebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-563">Methods, indexers, and instance constructors may declare their right-most parameter to be a parameter array ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="2886a-564">Bu tür işlev üyeleri, uygulanabilir olduğu ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)) bağlı olarak kendi normal biçiminde veya genişletilmiş biçiminde çağırılır:</span><span class="sxs-lookup"><span data-stu-id="2886a-564">Such function members are invoked either in their normal form or in their expanded form depending on which is applicable ([Applicable function member](expressions.md#applicable-function-member)):</span></span>

*  <span data-ttu-id="2886a-565">Parametre dizisi olan bir işlev üyesi normal biçimde çağrıldığında, parametre dizisi için verilen bağımsız değişken parametre dizi türüne örtük olarak dönüştürülebilir ([örtük dönüştürmeler](conversions.md#implicit-conversions)) tek bir ifade olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-565">When a function member with a parameter array is invoked in its normal form, the argument given for the parameter array must be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="2886a-566">Bu durumda, parametre dizisi tam olarak bir değer parametresi gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="2886a-566">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="2886a-567">Bir parametre dizisi olan bir işlev üyesi genişletilmiş biçimde çağrıldığında, çağırma parametre dizisi için sıfır veya daha fazla Konumsal bağımsız değişkeni, her bağımsız değişkenin parametre dizisinin öğe türüne örtük olarak dönüştürülebilir bir ifade ([örtük dönüştürmeler](conversions.md#implicit-conversions)) belirtmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2886a-567">When a function member with a parameter array is invoked in its expanded form, the invocation must specify zero or more positional arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="2886a-568">Bu durumda, çağırma parametre dizisi türünde bağımsız değişken sayısına karşılık gelen bir bir örnek oluşturur, dizi örneğinin öğelerini verilen bağımsız değişken değerleriyle başlatır ve gerçek olarak yeni oluşturulan dizi örneğini kullanır değişkendir.</span><span class="sxs-lookup"><span data-stu-id="2886a-568">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="2886a-569">Bir bağımsız değişken listesinin ifadeleri her zaman yazıldığı sırada değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-569">The expressions of an argument list are always evaluated in the order they are written.</span></span> <span data-ttu-id="2886a-570">Bu nedenle örnek</span><span class="sxs-lookup"><span data-stu-id="2886a-570">Thus, the example</span></span>
```csharp
class Test
{
    static void F(int x, int y = -1, int z = -2) {
        System.Console.WriteLine("x = {0}, y = {1}, z = {2}", x, y, z);
    }

    static void Main() {
        int i = 0;
        F(i++, i++, i++);
        F(z: i++, x: i++);
    }
}
```
<span data-ttu-id="2886a-571">çıktıyı üretir</span><span class="sxs-lookup"><span data-stu-id="2886a-571">produces the output</span></span>
```console
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

<span data-ttu-id="2886a-572">Dizi birlikte değişim kuralları ([dizi Kovaryans](arrays.md#array-covariance)), dizi türü bir değere `A[]` `B[]`bir dizi türü örneğine izin verir, `B` ' den `A`örtük bir başvuru dönüştürmesi sağlanmış olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-572">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="2886a-573">Bu kurallar nedeniyle, bir *reference_type* dizi öğesi başvuru veya çıkış parametresi olarak geçirildiğinde, dizinin gerçek öğe türünün parametreyle aynı olduğundan emin olmak için bir çalışma zamanı denetimi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-573">Because of these rules, when an array element of a *reference_type* is passed as a reference or output parameter, a run-time check is required to ensure that the actual element type of the array is identical to that of the parameter.</span></span> <span data-ttu-id="2886a-574">Örnekte</span><span class="sxs-lookup"><span data-stu-id="2886a-574">In the example</span></span>
```csharp
class Test
{
    static void F(ref object x) {...}

    static void Main() {
        object[] a = new object[10];
        object[] b = new string[10];
        F(ref a[0]);        // Ok
        F(ref b[1]);        // ArrayTypeMismatchException
    }
}
```
<span data-ttu-id="2886a-575">`F` ikinci çağrılması `System.ArrayTypeMismatchException` oluşturulmasına neden olur çünkü `b` gerçek öğe türü `string` ve `object`değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-575">the second invocation of `F` causes a `System.ArrayTypeMismatchException` to be thrown because the actual element type of `b` is `string` and not `object`.</span></span>

<span data-ttu-id="2886a-576">Bir parametre dizisi olan bir işlev üyesi genişletilmiş biçimde çağrıldığında, genişletilmiş parametrelere bir dizi Başlatıcısı ([dizi oluşturma ifadeleri](expressions.md#array-creation-expressions)) içeren bir dizi oluşturma ifadesi eklenirse, çağırma tam olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-576">When a function member with a parameter array is invoked in its expanded form, the invocation is processed exactly as if an array creation expression with an array initializer ([Array creation expressions](expressions.md#array-creation-expressions)) was inserted around the expanded parameters.</span></span> <span data-ttu-id="2886a-577">Örneğin, bildirim verildiğinde</span><span class="sxs-lookup"><span data-stu-id="2886a-577">For example, given the declaration</span></span>
```csharp
void F(int x, int y, params object[] args);
```
<span data-ttu-id="2886a-578">yöntemin genişletilmiş biçimini aşağıdaki şekilde çağırma</span><span class="sxs-lookup"><span data-stu-id="2886a-578">the following invocations of the expanded form of the method</span></span>
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
<span data-ttu-id="2886a-579">tam olarak karşılık gelir</span><span class="sxs-lookup"><span data-stu-id="2886a-579">correspond exactly to</span></span>
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

<span data-ttu-id="2886a-580">Özellikle, parametre dizisi için belirtilen 0 bağımsız değişken olduğunda boş bir dizi oluşturulduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2886a-580">In particular, note that an empty array is created when there are zero arguments given for the parameter array.</span></span>

<span data-ttu-id="2886a-581">Bağımsız değişkenler, karşılık gelen isteğe bağlı parametrelere sahip bir işlev üyesinden atlandığında, işlev üyesi bildiriminin varsayılan bağımsız değişkenleri dolaylı olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-581">When arguments are omitted from a function member with corresponding optional parameters, the default arguments of the function member declaration are implicitly passed.</span></span> <span data-ttu-id="2886a-582">Bunlar her zaman sabit olduğundan, değerlendirmesi kalan bağımsız değişkenlerin değerlendirme sırasını etkilemez.</span><span class="sxs-lookup"><span data-stu-id="2886a-582">Because these are always constant, their evaluation will not impact the evaluation order of the remaining arguments.</span></span>

### <a name="type-inference"></a><span data-ttu-id="2886a-583">Tür çıkarımı</span><span class="sxs-lookup"><span data-stu-id="2886a-583">Type inference</span></span>

<span data-ttu-id="2886a-584">Genel bir yöntem tür bağımsız değişkenleri belirtilmeden çağrıldığında, bir ***tür çıkarım*** işlemi çağrının tür bağımsız değişkenlerini çıkarmayı dener.</span><span class="sxs-lookup"><span data-stu-id="2886a-584">When a generic method is called without specifying type arguments, a ***type inference*** process attempts to infer type arguments for the call.</span></span> <span data-ttu-id="2886a-585">Tür çıkarımı varlığı, genel bir yöntemi çağırmak için daha uygun bir sözdiziminin kullanılmasına izin verir ve programcı 'nın gereksiz tür bilgilerini belirtmelerini önler.</span><span class="sxs-lookup"><span data-stu-id="2886a-585">The presence of type inference allows a more convenient syntax to be used for calling a generic method, and allows the programmer to avoid specifying redundant type information.</span></span> <span data-ttu-id="2886a-586">Örneğin, yöntem bildirimi verildiğinde:</span><span class="sxs-lookup"><span data-stu-id="2886a-586">For example, given the method declaration:</span></span>
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
<span data-ttu-id="2886a-587">açıkça bir tür bağımsız değişkeni belirtmeden `Choose` yöntemi çağırmak mümkündür:</span><span class="sxs-lookup"><span data-stu-id="2886a-587">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

<span data-ttu-id="2886a-588">Tür çıkarımı aracılığıyla `int` ve `string` tür bağımsız değişkenleri, bağımsız değişkenlerden yöntemine belirlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-588">Through type inference, the type arguments `int` and `string` are determined from the arguments to the method.</span></span>

<span data-ttu-id="2886a-589">Tür çıkarımı, bir yöntem çağırma işleminin ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)) bağlama zamanı işlemenin bir parçası olarak oluşur ve çağrının aşırı yükleme çözümleme adımından önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="2886a-589">Type inference occurs as part of the binding-time processing of a method invocation ([Method invocations](expressions.md#method-invocations)) and takes place before the overload resolution step of the invocation.</span></span> <span data-ttu-id="2886a-590">Bir yöntem çağrısında belirli bir yöntem grubu belirtildiğinde ve yöntem çağrısının bir parçası olarak hiçbir tür bağımsız değişkeni belirtilmediğinde, yöntem grubundaki her genel yönteme tür çıkarımı uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-590">When a particular method group is specified in a method invocation, and no type arguments are specified as part of the method invocation, type inference is applied to each generic method in the method group.</span></span> <span data-ttu-id="2886a-591">Tür çıkarımı başarılı olursa, sonraki aşırı yükleme çözümlemesi için bağımsız değişken türlerini belirlemede çıkartılan tür bağımsız değişkenleri kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-591">If type inference succeeds, then the inferred type arguments are used to determine the types of arguments for subsequent overload resolution.</span></span> <span data-ttu-id="2886a-592">Aşırı yükleme çözümlemesi, çağırabileceğiniz bir genel yöntem seçerse, çağırma için gerçek tür bağımsız değişkenleri olarak, çıkartılan tür bağımsız değişkenleri kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-592">If overload resolution chooses a generic method as the one to invoke, then the inferred type arguments are used as the actual type arguments for the invocation.</span></span> <span data-ttu-id="2886a-593">Belirli bir yöntemin tür çıkarımı başarısız olursa, bu yöntem aşırı yükleme çözümüne katılmaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-593">If type inference for a particular method fails, that method does not participate in overload resolution.</span></span> <span data-ttu-id="2886a-594">, Ve içindeki çıkarım hatası, bağlama zamanı hatasına neden olmaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-594">The failure of type inference, in and of itself, does not cause a binding-time error.</span></span> <span data-ttu-id="2886a-595">Ancak, aşırı yükleme çözümlemesi sırasında bir bağlama zamanı hatasına neden olur ve ilgili herhangi bir yöntemi bulamaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-595">However, it often leads to a binding-time error when overload resolution then fails to find any applicable methods.</span></span>

<span data-ttu-id="2886a-596">Sağlanan bağımsız değişken sayısı yöntemdeki parametre sayısından farklıysa, çıkarımı hemen başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-596">If the supplied number of arguments is different than the number of parameters in the method, then inference immediately fails.</span></span> <span data-ttu-id="2886a-597">Aksi takdirde, genel yöntemin aşağıdaki imzaya sahip olduğunu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="2886a-597">Otherwise, assume that the generic method has the following signature:</span></span>
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

<span data-ttu-id="2886a-598">`M(E1...Em)` bir yöntem çağrısı ile, tür çıkarımı görevi `X1...Xn` tür parametrelerinin her biri için `S1...Sn` benzersiz tür bağımsız değişkenlerini bulmalıdır, böylece çağrının `M<S1...Sn>(E1...Em)` geçerli hale gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-598">With a method call of the form `M(E1...Em)` the task of type inference is to find unique type arguments `S1...Sn` for each of the type parameters `X1...Xn` so that the call `M<S1...Sn>(E1...Em)` becomes valid.</span></span>

<span data-ttu-id="2886a-599">Çıkarımı işlemi sırasında her tür parametre `Xi`, belirli bir tür `Si` *sabitlenir* veya ilişkili bir *sınır*kümesiyle *sabitlenemez* .</span><span class="sxs-lookup"><span data-stu-id="2886a-599">During the process of inference each type parameter `Xi` is either *fixed* to a particular type `Si` or *unfixed* with an associated set of *bounds*.</span></span> <span data-ttu-id="2886a-600">Her bir sınır `T`türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-600">Each of the bounds is some type `T`.</span></span> <span data-ttu-id="2886a-601">Başlangıçta her tür değişkeni `Xi` boş bir sınır kümesiyle düzeltilmedi.</span><span class="sxs-lookup"><span data-stu-id="2886a-601">Initially each type variable `Xi` is unfixed with an empty set of bounds.</span></span>

<span data-ttu-id="2886a-602">Tür çıkarımı aşamalar halinde gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="2886a-602">Type inference takes place in phases.</span></span> <span data-ttu-id="2886a-603">Her aşama, önceki aşamanın bulgularını temel alarak daha fazla tür değişkeni için tür bağımsız değişkenlerini çıkarması denenecek.</span><span class="sxs-lookup"><span data-stu-id="2886a-603">Each phase will try to infer type arguments for more type variables based on the findings of the previous phase.</span></span> <span data-ttu-id="2886a-604">İlk aşama bazı ilk dereceden sınırları yapar, ancak ikinci aşama belirli türlere tür değişkenlerini ve daha fazla sınırları düzeltir.</span><span class="sxs-lookup"><span data-stu-id="2886a-604">The first phase makes some initial inferences of bounds, whereas the second phase fixes type variables to specific types and infers further bounds.</span></span> <span data-ttu-id="2886a-605">İkinci aşamanın birkaç kez tekrarlanmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-605">The second phase may have to be repeated a number of times.</span></span>

<span data-ttu-id="2886a-606">*Note:* Tür çıkarımı yalnızca genel bir yöntem çağrıldığında değil.</span><span class="sxs-lookup"><span data-stu-id="2886a-606">*Note:* Type inference takes place not only when a generic method is called.</span></span> <span data-ttu-id="2886a-607">Yöntem gruplarının dönüştürülmesine ilişkin tür çıkarımı, [Yöntem gruplarının dönüştürülmesi için tür çıkarımı](expressions.md#type-inference-for-conversion-of-method-groups) ve bir ifade kümesinin en iyi ortak [türünü bulma bölümünde açıklanmıştır.](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)</span><span class="sxs-lookup"><span data-stu-id="2886a-607">Type inference for conversion of method groups is described in [Type inference for conversion of method groups](expressions.md#type-inference-for-conversion-of-method-groups) and finding the best common type of a set of expressions is described in [Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span></span>

#### <a name="the-first-phase"></a><span data-ttu-id="2886a-608">İlk aşama</span><span class="sxs-lookup"><span data-stu-id="2886a-608">The first phase</span></span>

<span data-ttu-id="2886a-609">Yöntem bağımsız değişkenlerinin her biri için `Ei`:</span><span class="sxs-lookup"><span data-stu-id="2886a-609">For each of the method arguments `Ei`:</span></span>

*   <span data-ttu-id="2886a-610">`Ei` anonim bir işlevse, açık bir *parametre türü çıkarımı* ([Açık parametre türü](expressions.md#explicit-parameter-type-inferences)) `Ei` ' dan `Ti` yapılır</span><span class="sxs-lookup"><span data-stu-id="2886a-610">If `Ei` is an anonymous function, an *explicit parameter type inference* ([Explicit parameter type inferences](expressions.md#explicit-parameter-type-inferences)) is made from `Ei` to `Ti`</span></span>
*   <span data-ttu-id="2886a-611">Aksi takdirde, `Ei` `U` bir tür `xi` ve bir değer parametresi ise, `U` *'den* `Ti` *'e* *daha düşük bir çıkarım* yapılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-611">Otherwise, if `Ei` has a type `U` and `xi` is a value parameter then a *lower-bound inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="2886a-612">Aksi takdirde, `Ei` `U` bir tür varsa ve `xi` bir `ref` veya `out` parametresi ise `U` ' *den* `Ti`' *e* *doğru bir çıkarım* yapılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-612">Otherwise, if `Ei` has a type `U` and `xi` is a `ref` or `out` parameter then an *exact inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="2886a-613">Aksi takdirde, bu bağımsız değişken için çıkarımı yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-613">Otherwise, no inference is made for this argument.</span></span>


#### <a name="the-second-phase"></a><span data-ttu-id="2886a-614">İkinci aşama</span><span class="sxs-lookup"><span data-stu-id="2886a-614">The second phase</span></span>

<span data-ttu-id="2886a-615">İkinci aşama aşağıdaki gibi ilerler:</span><span class="sxs-lookup"><span data-stu-id="2886a-615">The second phase proceeds as follows:</span></span>

*   <span data-ttu-id="2886a-616">Hiçbir `Xj` ([bağımlılıklara CE](expressions.md#dependence)) *bağımlı* olmayan `Xi` tüm *sabit* olmayan tür değişkenleri düzeltildi ([düzeltiliyor](expressions.md#fixing)).</span><span class="sxs-lookup"><span data-stu-id="2886a-616">All *unfixed* type variables `Xi` which do not *depend on* ([Dependence](expressions.md#dependence)) any `Xj` are fixed ([Fixing](expressions.md#fixing)).</span></span>
*   <span data-ttu-id="2886a-617">Böyle bir tür değişkeni yoksa, tüm *sabit* olmayan tür değişkenleri aşağıdaki ayrı tutmadaki `Xi` *sabittir* :</span><span class="sxs-lookup"><span data-stu-id="2886a-617">If no such type variables exist, all *unfixed* type variables `Xi` are *fixed* for which all of the following hold:</span></span>
    *   <span data-ttu-id="2886a-618">`Xi` bağlı olan en az bir tür değişken `Xj` vardır</span><span class="sxs-lookup"><span data-stu-id="2886a-618">There is at least one type variable `Xj` that depends on `Xi`</span></span>
    *   <span data-ttu-id="2886a-619">`Xi` boş olmayan bir sınır kümesine sahip</span><span class="sxs-lookup"><span data-stu-id="2886a-619">`Xi` has a non-empty set of bounds</span></span>
*   <span data-ttu-id="2886a-620">Böyle bir tür değişkeni yoksa ve hala *sabit* olmayan tür değişkenleri varsa, tür çıkarımı başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-620">If no such type variables exist and there are still *unfixed* type variables, type inference fails.</span></span>
*   <span data-ttu-id="2886a-621">Aksi halde, daha fazla *sabit olmayan* tür değişkeni yoksa, tür çıkarımı başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-621">Otherwise, if no further *unfixed* type variables exist, type inference succeeds.</span></span>
*   <span data-ttu-id="2886a-622">Aksi takdirde, karşılık gelen parametre türü `Ei`, *Çıkış türlerinin* ([çıktı türleri](expressions.md#output-types)) *sabit* olmayan tür `Xj` değişkenleri içeren `Ti`, ancak *giriş türleri* ([giriş türleri](expressions.md#input-types)) olmayan tüm *bağımsız değişkenler için `Ei` '* *dan* `Ti`' a[bir](expressions.md#output-type-inferences) *Çıkış türü çıkarımı* yapılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-622">Otherwise, for all arguments `Ei` with corresponding parameter type `Ti` where the *output types* ([Output types](expressions.md#output-types)) contain *unfixed* type variables `Xj` but the *input types* ([Input types](expressions.md#input-types)) do not, an *output type inference* ([Output type inferences](expressions.md#output-type-inferences)) is made *from* `Ei` *to* `Ti`.</span></span> <span data-ttu-id="2886a-623">İkinci aşama yinelenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-623">Then the second phase is repeated.</span></span>

#### <a name="input-types"></a><span data-ttu-id="2886a-624">Giriş türleri</span><span class="sxs-lookup"><span data-stu-id="2886a-624">Input types</span></span>

<span data-ttu-id="2886a-625">`E` bir yöntem grubu veya örtük olarak yazılmış anonim işlevse ve `T` bir temsilci türü ya da ifade ağacı türsiyse, tüm `T` parametre türleri, *türü `T`olan* `E` *giriş türleridir* .</span><span class="sxs-lookup"><span data-stu-id="2886a-625">If `E` is a method group or implicitly typed anonymous function and `T` is a delegate type or expression tree type then all the parameter types of `T` are *input types* of `E` *with type* `T`.</span></span>

####  <a name="output-types"></a><span data-ttu-id="2886a-626">Çıkış türleri</span><span class="sxs-lookup"><span data-stu-id="2886a-626">Output types</span></span>

<span data-ttu-id="2886a-627">`E` bir yöntem grubu veya anonim işlevse ve `T` bir temsilci türü ya da ifade ağacı türü ise, `T` dönüş türü `T`*türünde* bir `E` *Çıkış türüdür* .</span><span class="sxs-lookup"><span data-stu-id="2886a-627">If `E` is a method group or an anonymous function and `T` is a delegate type or expression tree type then the return type of `T` is an *output type of* `E` *with type* `T`.</span></span>

#### <a name="dependence"></a><span data-ttu-id="2886a-628">Düzeyde bağımlı</span><span class="sxs-lookup"><span data-stu-id="2886a-628">Dependence</span></span>

<span data-ttu-id="2886a-629">*Sabit olmayan* *bir tür değişkeni `Xi`,* `Xj` türünde bir `Ek` *giriş türünde* meydana gelir ve `Tk` türünde `Xi` bir *çıkış türünde* meydana gelen bir `Ek` `Xj` `Tk`</span><span class="sxs-lookup"><span data-stu-id="2886a-629">An *unfixed* type variable `Xi` *depends directly on* an unfixed type variable `Xj` if for some argument `Ek` with type `Tk` `Xj` occurs in an *input type* of `Ek` with type `Tk` and `Xi` occurs in an *output type* of `Ek` with type `Tk`.</span></span>

<span data-ttu-id="2886a-630">`Xj`, `Xj` *doğrudan* `Xi` bağımlıysa veya `Xi` *doğrudan `Xk` 'ye* bağımlıysa ve `Xk` *`Xj`bağımlı ise* `Xi` *bağlıdır* .</span><span class="sxs-lookup"><span data-stu-id="2886a-630">`Xj` *depends on* `Xi` if `Xj` *depends directly on* `Xi` or if `Xi` *depends directly on* `Xk` and `Xk` *depends on* `Xj`.</span></span> <span data-ttu-id="2886a-631">Bu nedenle "bağlı olan", geçişli ancak yansımalı olmayan "açık" kapanışı değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-631">Thus "depends on" is the transitive but not reflexive closure of "depends directly on".</span></span>

#### <a name="output-type-inferences"></a><span data-ttu-id="2886a-632">Çıkış türü ında</span><span class="sxs-lookup"><span data-stu-id="2886a-632">Output type inferences</span></span>

<span data-ttu-id="2886a-633">Aşağıdaki şekilde *`E` bir ifadeden* *bir tür* `T` bir *Çıkış türü çıkarımı* yapılır:</span><span class="sxs-lookup"><span data-stu-id="2886a-633">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="2886a-634">`E`, `U` çıkartılan dönüş türü olan bir anonim işlevse[(çıkartılan dönüş türü](expressions.md#inferred-return-type)) ve `T`, dönüş türü `Tb`olan bir temsilci türü veya ifade ağacı türü ise *, `U` ile `Tb`* *arasında* daha *düşük bağlantılı bir çıkarım* ([alt sınır](expressions.md#lower-bound-inferences)) yapılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-634">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="2886a-635">Aksi takdirde, `E` bir yöntem grubu ise ve `T`, parametre türleri `T1...Tk` ve dönüş türü `Tb`olan bir temsilci türü ya da ifade ağacı türü `E` ve `T1...Tk` bir `U`dönüş türü ile tek bir yöntem ortaya çıkardı `U` *, `Tb`ile* arasında *daha düşük* bir çıkarma yapılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-635">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="2886a-636">Aksi takdirde, `E` `U`türünde bir ifade ise, `U` ' *den* `T` *'ye* *daha düşük bir çıkarım* yapılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-636">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
*  <span data-ttu-id="2886a-637">Aksi takdirde, hiçbir Inse yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-637">Otherwise, no inferences are made.</span></span>

#### <a name="explicit-parameter-type-inferences"></a><span data-ttu-id="2886a-638">Açık parametre türü</span><span class="sxs-lookup"><span data-stu-id="2886a-638">Explicit parameter type inferences</span></span>

<span data-ttu-id="2886a-639">Bir ifadeden `E` bir *Açık parametre türü çıkarımı* *aşağıdaki şekilde `T`* bir *türe yapılır:*</span><span class="sxs-lookup"><span data-stu-id="2886a-639">An *explicit parameter type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="2886a-640">`E` parametre türleri `U1...Uk` açık *şekilde* belirlenmiş bir anonim işlevse ve `T` parametre türlerine sahip bir temsilci türü veya ifade ağacı türü `V1...Vk`, her *`Ui` `Ui` ilgili `Vi`* bir *çıkarım* ([tam](expressions.md#exact-inferences)).</span><span class="sxs-lookup"><span data-stu-id="2886a-640">If `E` is an explicitly typed anonymous function with parameter types `U1...Uk` and `T` is a delegate type or expression tree type with parameter types `V1...Vk` then for each `Ui` an *exact inference* ([Exact inferences](expressions.md#exact-inferences)) is made *from* `Ui` *to* the corresponding `Vi`.</span></span>

#### <a name="exact-inferences"></a><span data-ttu-id="2886a-641">Tam ında</span><span class="sxs-lookup"><span data-stu-id="2886a-641">Exact inferences</span></span>

<span data-ttu-id="2886a-642">*Bir tür `V` bir `U`* *tam çıkarımı* aşağıdaki gibi yapılır:</span><span class="sxs-lookup"><span data-stu-id="2886a-642">An *exact inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="2886a-643">`V` *düzeltilmeyen* `Xi` biri ise, `U` `Xi`için tam sınırlara eklenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-643">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of exact bounds for `Xi`.</span></span>

*  <span data-ttu-id="2886a-644">Aksi takdirde, `V1...Vk` ayarlar ve `U1...Uk` aşağıdaki durumlardan herhangi birinin uygulayıp uygulamadığı denetlenerek belirlenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-644">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>

   *  <span data-ttu-id="2886a-645">`V`, `V1[...]` bir dizi türüdür ve `U` aynı derecenin `U1[...]` bir dizi türüdür</span><span class="sxs-lookup"><span data-stu-id="2886a-645">`V` is an array type `V1[...]` and `U` is an array type `U1[...]`  of the same rank</span></span>
   *  <span data-ttu-id="2886a-646">`V` tür `V1?` ve `U` türdür `U1?`</span><span class="sxs-lookup"><span data-stu-id="2886a-646">`V` is the type `V1?` and `U` is the type `U1?`</span></span>
   *  <span data-ttu-id="2886a-647">`V`, oluşturulmuş bir tür `C<V1...Vk>`ve `U` oluşturulmuş bir tür `C<U1...Uk>`</span><span class="sxs-lookup"><span data-stu-id="2886a-647">`V` is a constructed type `C<V1...Vk>`and `U` is a constructed type `C<U1...Uk>`</span></span>

   <span data-ttu-id="2886a-648">Bu durumlardan herhangi biri uygunsa *, her `Ui` karşılık gelen `Vi`* *tam çıkarımı* yapılır .</span><span class="sxs-lookup"><span data-stu-id="2886a-648">If any of these cases apply then an *exact inference* is made *from* each `Ui` *to* the corresponding `Vi`.</span></span>

*  <span data-ttu-id="2886a-649">Aksi takdirde hiçbir Inse yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-649">Otherwise no inferences are made.</span></span>

#### <a name="lower-bound-inferences"></a><span data-ttu-id="2886a-650">Alt sınır</span><span class="sxs-lookup"><span data-stu-id="2886a-650">Lower-bound inferences</span></span>

<span data-ttu-id="2886a-651">*Bir tür* `V` bir `U` *alt sınır çıkarımı* *Şu şekilde yapılır* :</span><span class="sxs-lookup"><span data-stu-id="2886a-651">A *lower-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="2886a-652">`V` *düzeltilmeyen* `Xi` biri ise, `U` `Xi`alt sınırlar kümesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-652">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of lower bounds for `Xi`.</span></span>
*  <span data-ttu-id="2886a-653">Aksi takdirde, `V` tür `V1?`ve `U` `U1?` tür ise, `U1` `V1`'e daha düşük bir sınır çıkarımı yapılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-653">Otherwise, if `V` is the type `V1?`and `U` is the type `U1?` then a lower bound inference is made from `U1` to `V1`.</span></span>
*  <span data-ttu-id="2886a-654">Aksi takdirde, `U1...Uk` ayarlar ve `V1...Vk` aşağıdaki durumlardan herhangi birinin uygulayıp uygulamadığı denetlenerek belirlenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-654">Otherwise, sets `U1...Uk` and `V1...Vk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="2886a-655">`V`, `V1[...]` bir dizi türüdür `U` ve aynı dereceye sahip bir dizi türü `U1[...]` (veya geçerli temel türü `U1[...]`olan bir tür parametresidir)</span><span class="sxs-lookup"><span data-stu-id="2886a-655">`V` is an array type `V1[...]` and `U` is an array type `U1[...]` (or a type parameter whose effective base type is `U1[...]`) of the same rank</span></span>
   *  <span data-ttu-id="2886a-656">`V` `IEnumerable<V1>`, `ICollection<V1>` veya `IList<V1>` ve `U` tek boyutlu dizi türü `U1[]`(veya geçerli temel türü `U1[]`olan bir tür parametresi)</span><span class="sxs-lookup"><span data-stu-id="2886a-656">`V` is one of `IEnumerable<V1>`, `ICollection<V1>` or `IList<V1>` and `U` is a one-dimensional array type `U1[]`(or a type parameter whose effective base type is `U1[]`)</span></span>
   *  <span data-ttu-id="2886a-657">`V`, oluşturulmuş bir sınıf, yapı, arabirim veya temsilci türü `C<V1...Vk>` `C<U1...Uk>` ve `U` (ya da `U` bir tür parametresi ise, etkin taban sınıfı veya etkin arabirim kümesinin herhangi bir üyesi) ile aynıdır, öğesinden devralır (doğrudan veya dolaylı) ya da uygular (doğrudan veya dolaylı) `C<U1...Uk>`.</span><span class="sxs-lookup"><span data-stu-id="2886a-657">`V` is a constructed class, struct, interface or delegate type `C<V1...Vk>` and there is a unique type `C<U1...Uk>` such that `U` (or, if `U` is a type parameter, its effective base class or any member of its effective interface set) is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) `C<U1...Uk>`.</span></span>

      <span data-ttu-id="2886a-658">("Benzersizlik" kısıtlaması, büyük/küçük harf `U`, `X` veya `Y`olabileceği `U1` `C<T>` için, bu durum arabirimindeki `C<T> {} class U: C<X>, C<Y> {}`bir çıkarım yapılmayacağı anlamına gelir.)</span><span class="sxs-lookup"><span data-stu-id="2886a-658">(The "uniqueness" restriction means that in the case interface `C<T> {} class U: C<X>, C<Y> {}`, then no inference is made when inferring from `U` to `C<T>` because `U1` could be `X` or `Y`.)</span></span>

   <span data-ttu-id="2886a-659">Bu durumlardan herhangi biri uygunsa her bir *`Ui` aşağıdaki şekilde* karşılık *gelen `Vi`* bir çıkarımı yapılır:</span><span class="sxs-lookup"><span data-stu-id="2886a-659">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>

   *  <span data-ttu-id="2886a-660">`Ui` bir başvuru türü olarak bilinmiyorsa, *kesin bir çıkarım* yapılır</span><span class="sxs-lookup"><span data-stu-id="2886a-660">If `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="2886a-661">Aksi takdirde, `U` bir dizi türü ise, *alt sınır çıkarımı* yapılır</span><span class="sxs-lookup"><span data-stu-id="2886a-661">Otherwise, if `U` is an array type then a *lower-bound inference* is made</span></span>
   *  <span data-ttu-id="2886a-662">Aksi takdirde, `V` `C<V1...Vk>`, çıkarımı, `C`i-TH türü parametresine bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="2886a-662">Otherwise, if `V` is `C<V1...Vk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="2886a-663">Daha fazla değişken ise, *alt sınır çıkarımı* yapılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-663">If it is covariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="2886a-664">Değişken karşıtı ise, *üst sınır çıkarımı* yapılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-664">If it is contravariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="2886a-665">Sabit ise, *kesin bir çıkarım* yapılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-665">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="2886a-666">Aksi takdirde, hiçbir Inse yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-666">Otherwise, no inferences are made.</span></span>

#### <a name="upper-bound-inferences"></a><span data-ttu-id="2886a-667">Üst sınır</span><span class="sxs-lookup"><span data-stu-id="2886a-667">Upper-bound inferences</span></span>

<span data-ttu-id="2886a-668">`U` *bir tür* *`V` bir* *üst sınır çıkarımı* aşağıdaki gibi yapılır:</span><span class="sxs-lookup"><span data-stu-id="2886a-668">An *upper-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="2886a-669">`V` *düzeltilmeyen* `Xi` biri ise, `Xi`için üst sınırlar kümesine `U` eklenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-669">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of upper bounds for `Xi`.</span></span>
*  <span data-ttu-id="2886a-670">Aksi takdirde, `V1...Vk` ayarlar ve `U1...Uk` aşağıdaki durumlardan herhangi birinin uygulayıp uygulamadığı denetlenerek belirlenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-670">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="2886a-671">`U`, `U1[...]` bir dizi türüdür ve `V` aynı derecenin `V1[...]` bir dizi türüdür</span><span class="sxs-lookup"><span data-stu-id="2886a-671">`U` is an array type `U1[...]` and `V` is an array type `V1[...]` of the same rank</span></span>
   *  <span data-ttu-id="2886a-672">`U` bir `IEnumerable<Ue>`, `ICollection<Ue>` veya `IList<Ue>` ve `V` tek boyutlu dizi türüdür `Ve[]`</span><span class="sxs-lookup"><span data-stu-id="2886a-672">`U` is one of `IEnumerable<Ue>`, `ICollection<Ue>` or `IList<Ue>` and `V` is a one-dimensional array type `Ve[]`</span></span>
   *  <span data-ttu-id="2886a-673">`U` tür `U1?` ve `V` türdür `V1?`</span><span class="sxs-lookup"><span data-stu-id="2886a-673">`U` is the type `U1?` and `V` is the type `V1?`</span></span>
   *  <span data-ttu-id="2886a-674">`U` oluşturulan sınıf, yapı, arabirim veya temsilci türü `C<U1...Uk>` ve `V`, ile özdeş olan (doğrudan veya dolaylı) ya da (doğrudan veya dolaylı olarak) benzersiz bir tür uygulayan bir sınıf, yapı, arabirim veya temsilci türüdür `C<V1...Vk>`</span><span class="sxs-lookup"><span data-stu-id="2886a-674">`U` is constructed class, struct, interface or delegate type `C<U1...Uk>` and `V` is a class, struct, interface or delegate type which is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) a unique type `C<V1...Vk>`</span></span>

      <span data-ttu-id="2886a-675">("Benzersizlik" kısıtlaması, `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`varsa `C<U1>` `V<Q>`'e kadar bir çıkarımı yapılmadığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="2886a-675">(The "uniqueness" restriction means that if we have `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, then no inference is made when inferring from `C<U1>` to `V<Q>`.</span></span> <span data-ttu-id="2886a-676">Inor, `U1` `X<Q>` veya `Y<Q>`olarak yapılmaz.)</span><span class="sxs-lookup"><span data-stu-id="2886a-676">Inferences are not made from `U1` to either `X<Q>` or `Y<Q>`.)</span></span>

   <span data-ttu-id="2886a-677">Bu durumlardan herhangi biri uygunsa her bir *`Ui` aşağıdaki şekilde* karşılık *gelen `Vi`* bir çıkarımı yapılır:</span><span class="sxs-lookup"><span data-stu-id="2886a-677">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>
   *  <span data-ttu-id="2886a-678">`Ui` bir başvuru türü olarak bilinmiyorsa, *kesin bir çıkarım* yapılır</span><span class="sxs-lookup"><span data-stu-id="2886a-678">If  `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="2886a-679">Aksi takdirde, `V` bir dizi türü ise, *üst sınır çıkarımı* yapılır</span><span class="sxs-lookup"><span data-stu-id="2886a-679">Otherwise, if `V` is an array type then an *upper-bound inference* is made</span></span>
   *  <span data-ttu-id="2886a-680">Aksi takdirde, `U` `C<U1...Uk>`, çıkarımı, `C`i-TH türü parametresine bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="2886a-680">Otherwise, if `U` is `C<U1...Uk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="2886a-681">Covaryant ise, *üst sınır çıkarımı* yapılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-681">If it is covariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="2886a-682">Değişken karşıtı ise, *alt sınır çıkarımı* yapılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-682">If it is contravariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="2886a-683">Sabit ise, *kesin bir çıkarım* yapılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-683">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="2886a-684">Aksi takdirde, hiçbir Inse yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-684">Otherwise, no inferences are made.</span></span>   

#### <a name="fixing"></a><span data-ttu-id="2886a-685">Düzelttikten</span><span class="sxs-lookup"><span data-stu-id="2886a-685">Fixing</span></span>

<span data-ttu-id="2886a-686">Bir dizi sınırı olan `Xi` *sabit olmayan* bir tür değişkeni aşağıdaki gibi *düzeltilir* :</span><span class="sxs-lookup"><span data-stu-id="2886a-686">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>

*  <span data-ttu-id="2886a-687">`Uj` *aday türleri* kümesi, `Xi`sınırları kümesindeki tüm türlerin kümesi olarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-687">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
*  <span data-ttu-id="2886a-688">Sonra her bir `Xi` için her birini sırayla inceliyoruz `U`: `U` aynı olmayan tüm türler `Uj` `Xi` aday kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-688">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="2886a-689">Tüm `Xi` `U` her bir alt sınır için `U` örtük bir dönüştürme *olmadığı* `Uj` tüm türler için aday kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-689">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="2886a-690">`U` örtük bir dönüştürme *olmadığı* `Uj` tüm türlerin `Xi` `U` her üst sınır için aday kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-690">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
*  <span data-ttu-id="2886a-691">Kalan aday türleri arasında `Uj`, tüm diğer aday türlerine örtük bir dönüştürme olan `V` benzersiz bir tür vardır ve `Xi` `V`için düzeltilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-691">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then `Xi` is fixed to `V`.</span></span>
*  <span data-ttu-id="2886a-692">Aksi takdirde tür çıkarımı başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-692">Otherwise, type inference fails.</span></span>

#### <a name="inferred-return-type"></a><span data-ttu-id="2886a-693">Çıkarılan dönüş türü</span><span class="sxs-lookup"><span data-stu-id="2886a-693">Inferred return type</span></span>

<span data-ttu-id="2886a-694">Anonim bir işlevin `F` çıkarılan dönüş türü, tür çıkarımı ve aşırı yükleme çözümlemesi sırasında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-694">The inferred return type of an anonymous function `F` is used during type inference and overload resolution.</span></span> <span data-ttu-id="2886a-695">Çıkarılan dönüş türü yalnızca, açıkça verildiği veya bir kapsayan genel üzerinde tür çıkarımı sırasında çıkarsandığı için, tüm parametre türlerinin bilindikleri anonim bir işlev için belirlenebilir Yöntem çağırma.</span><span class="sxs-lookup"><span data-stu-id="2886a-695">The inferred return type can only be determined for an anonymous function where all parameter types are known, either because they are explicitly given, provided through an anonymous function conversion or inferred during type inference on an enclosing generic method invocation.</span></span>

<span data-ttu-id="2886a-696">***Çıkarılan sonuç türü*** aşağıdaki şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-696">The ***inferred result type*** is determined as follows:</span></span>

*  <span data-ttu-id="2886a-697">`F` gövdesi türü olan bir *ifadesiyse* , `F` çıkarılan sonuç türü bu ifadenin türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-697">If the body of `F` is an *expression* that has a type, then the inferred result type of `F` is the type of that expression.</span></span>
*  <span data-ttu-id="2886a-698">`F` gövdesi bir *blok* ise ve bloğun `return` deyimlerindeki ifadelerin kümesi en iyi ortak tür `T` ([bir ifade kümesinin En Iyi ortak türünü bulmayla](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), bu durumda `F` çıkarılan sonuç türü `T`olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-698">If the body of `F` is a *block* and the set of expressions in the block's `return` statements has a best common type `T` ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), then the inferred result type of `F` is `T`.</span></span>
*  <span data-ttu-id="2886a-699">Aksi takdirde, `F`için bir sonuç türü çıkarsanamıyor.</span><span class="sxs-lookup"><span data-stu-id="2886a-699">Otherwise, a result type cannot be inferred for `F`.</span></span>

<span data-ttu-id="2886a-700">***Çıkarılan dönüş türü*** aşağıdaki şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-700">The ***inferred return type*** is determined as follows:</span></span>

*  <span data-ttu-id="2886a-701">`F` zaman uyumsuz ise ve `F` gövdesi Nothing ([ifade sınıflandırmaları](expressions.md#expression-classifications)) olarak sınıflandırılmış bir ifade veya hiçbir dönüş deyiminin ifadesi olmadığı bir deyim bloğu ise, çıkarılan dönüş türü `System.Threading.Tasks.Task`</span><span class="sxs-lookup"><span data-stu-id="2886a-701">If `F` is async and the body of `F` is either an expression classified as nothing ([Expression classifications](expressions.md#expression-classifications)), or a statement block where no return statements have expressions, the inferred return type is `System.Threading.Tasks.Task`</span></span>
*  <span data-ttu-id="2886a-702">`F` zaman uyumsuz ise ve ortaya çıkarılan sonuç türü `T`içeriyorsa, çıkarılan dönüş türü `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="2886a-702">If `F` is async and has an inferred result type `T`, the inferred return type is `System.Threading.Tasks.Task<T>`.</span></span>
*  <span data-ttu-id="2886a-703">`F` zaman uyumsuz ise ve ortaya çıkarılan sonuç türü `T`içeriyorsa, çıkarılan dönüş türü `T`.</span><span class="sxs-lookup"><span data-stu-id="2886a-703">If `F` is non-async and has an inferred result type `T`, the inferred return type is `T`.</span></span>
*  <span data-ttu-id="2886a-704">Aksi takdirde `F`için bir dönüş türü çıkarsanamıyor.</span><span class="sxs-lookup"><span data-stu-id="2886a-704">Otherwise a return type cannot be inferred for `F`.</span></span>

<span data-ttu-id="2886a-705">Anonim işlevler içeren tür çıkarımı örneği olarak, `System.Linq.Enumerable` sınıfında belirtilen `Select` uzantı yöntemini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="2886a-705">As an example of type inference involving anonymous functions, consider the `Select` extension method declared in the `System.Linq.Enumerable` class:</span></span>
```csharp
namespace System.Linq
{
    public static class Enumerable
    {
        public static IEnumerable<TResult> Select<TSource,TResult>(
            this IEnumerable<TSource> source,
            Func<TSource,TResult> selector)
        {
            foreach (TSource element in source) yield return selector(element);
        }
    }
}
```

<span data-ttu-id="2886a-706">`System.Linq` ad alanının bir `using` yan tümcesiyle içeri aktarıldığını ve `string`türünde bir `Name` özelliği olan bir sınıf `Customer` verildiğini varsayarsak, `Select` yöntemi bir müşterilerin listesinin adlarını seçmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-706">Assuming the `System.Linq` namespace was imported with a `using` clause, and given a class `Customer` with a `Name` property of type `string`, the `Select` method can be used to select the names of a list of customers:</span></span>
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

<span data-ttu-id="2886a-707">`Select` uzantı yöntemi çağırma ([genişletme yöntemi etkinleştirmeleri](expressions.md#extension-method-invocations)), statik bir yöntem çağrısına yapılan çağrıyı yeniden yazarak işlenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-707">The extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)) of `Select` is processed by rewriting the invocation to a static method invocation:</span></span>
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

<span data-ttu-id="2886a-708">Tür bağımsız değişkenleri açıkça belirtilmediği için tür çıkarımı tür bağımsız değişkenlerini çıkarsanacak şekilde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-708">Since type arguments were not explicitly specified, type inference is used to infer the type arguments.</span></span> <span data-ttu-id="2886a-709">İlk olarak, `customers` bağımsız değişkeni `source` parametresiyle ilgilidir ve `Customer``T`.</span><span class="sxs-lookup"><span data-stu-id="2886a-709">First, the `customers` argument is related to the `source` parameter, inferring `T` to be `Customer`.</span></span> <span data-ttu-id="2886a-710">Daha sonra, yukarıda açıklanan anonim işlev türü çıkarımı işlemini kullanarak, `c` `Customer`türü verilir ve `c.Name` ifadesi `selector` parametresinin dönüş türüyle ilgilidir ve `S` `string`.</span><span class="sxs-lookup"><span data-stu-id="2886a-710">Then, using the anonymous function type inference process described above, `c` is given type `Customer`, and the expression `c.Name` is related to the return type of the `selector` parameter, inferring `S` to be `string`.</span></span> <span data-ttu-id="2886a-711">Bu nedenle, çağırma şu şekilde eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="2886a-711">Thus, the invocation is equivalent to</span></span>
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
<span data-ttu-id="2886a-712">ve sonuç `IEnumerable<string>`türündedir.</span><span class="sxs-lookup"><span data-stu-id="2886a-712">and the result is of type `IEnumerable<string>`.</span></span>

<span data-ttu-id="2886a-713">Aşağıdaki örnek, anonim işlev türü çıkarımını genel yöntem çağrısında bağımsız değişkenler arasında "Flow" öğesine nasıl izin verdiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="2886a-713">The following example demonstrates how anonymous function type inference allows type information to "flow" between arguments in a generic method invocation.</span></span> <span data-ttu-id="2886a-714">Yöntem verildi:</span><span class="sxs-lookup"><span data-stu-id="2886a-714">Given the method:</span></span>
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

<span data-ttu-id="2886a-715">Çağırma için tür çıkarımı:</span><span class="sxs-lookup"><span data-stu-id="2886a-715">Type inference for the invocation:</span></span>
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
<span data-ttu-id="2886a-716">Şu şekilde devam eder: Ilk olarak, `"1:15:30"` bağımsız değişken `value` parametresiyle ilgilidir ve `string``X`.</span><span class="sxs-lookup"><span data-stu-id="2886a-716">proceeds as follows: First, the argument `"1:15:30"` is related to the `value` parameter, inferring `X` to be `string`.</span></span> <span data-ttu-id="2886a-717">Ardından, `s`ilk adsız işlevin parametresi, `string`Çıkarsanan tür verilir ve `TimeSpan.Parse(s)` ifadesi `f1`dönüş türüyle ilgilidir ve `Y` `System.TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="2886a-717">Then, the parameter of the first anonymous function, `s`, is given the inferred type `string`, and the expression `TimeSpan.Parse(s)` is related to the return type of `f1`, inferring `Y` to be `System.TimeSpan`.</span></span> <span data-ttu-id="2886a-718">Son olarak, `t`ikinci adsız işlevin parametresine `System.TimeSpan`Çıkarsanan tür verilir ve `t.TotalSeconds` ifadesi `f2`dönüş türüyle ilgilidir ve `Z` `double`.</span><span class="sxs-lookup"><span data-stu-id="2886a-718">Finally, the parameter of the second anonymous function, `t`, is given the inferred type `System.TimeSpan`, and the expression `t.TotalSeconds` is related to the return type of `f2`, inferring `Z` to be `double`.</span></span> <span data-ttu-id="2886a-719">Bu nedenle, çağrının sonucu `double`türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-719">Thus, the result of the invocation is of type `double`.</span></span>

#### <a name="type-inference-for-conversion-of-method-groups"></a><span data-ttu-id="2886a-720">Yöntem gruplarının dönüştürülmesi için tür çıkarımı</span><span class="sxs-lookup"><span data-stu-id="2886a-720">Type inference for conversion of method groups</span></span>

<span data-ttu-id="2886a-721">Genel yöntemlerin çağrılarına benzer şekilde, genel bir yöntem içeren `M` bir yöntem grubu, belirli bir temsilci türüne `D` ([Yöntem grubu dönüştürmeleri](conversions.md#method-group-conversions)) dönüştürüldüğünde tür çıkarımı da uygulanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-721">Similar to calls of generic methods, type inference must also be applied when a method group `M` containing a generic method is converted to a given delegate type `D` ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="2886a-722">Verilen Yöntem</span><span class="sxs-lookup"><span data-stu-id="2886a-722">Given a method</span></span>
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
<span data-ttu-id="2886a-723">`M` temsilci türüne atanmakta olan Yöntem grubu, `D` tür çıkarımı `S1...Sn`, ifadenin tür bağımsız değişkenlerini bulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="2886a-723">and the method group `M` being assigned to the delegate type `D` the task of type inference is to find type arguments `S1...Sn` so that the expression:</span></span>
```csharp
M<S1...Sn>
```
<span data-ttu-id="2886a-724">`D`ile uyumlu ([temsilci bildirimleri](delegates.md#delegate-declarations)) olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-724">becomes compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`.</span></span>

<span data-ttu-id="2886a-725">Genel yöntem çağrılarının tür çıkarımı algoritmasından farklı olarak, bu durumda yalnızca bağımsız değişken *türleri*vardır, hiçbir bağımsız değişken *ifadesi*yoktur.</span><span class="sxs-lookup"><span data-stu-id="2886a-725">Unlike the type inference algorithm for generic method calls, in this case there are only argument *types*, no argument *expressions*.</span></span> <span data-ttu-id="2886a-726">Özellikle, anonim işlev yoktur ve bu nedenle birden çok çıkarım aşaması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-726">In particular, there are no anonymous functions and hence no need for multiple phases of inference.</span></span>

<span data-ttu-id="2886a-727">Bunun yerine, tüm `Xi` *düzeltilmez*ve `D` *`Uj` her bağımsız değişken türünden `M`* karşılık gelen parametre türü `Tj` *için* *alt sınır çıkarımı* yapılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-727">Instead, all `Xi` are considered *unfixed*, and a *lower-bound inference* is made *from* each argument type `Uj` of `D` *to* the corresponding parameter type `Tj` of `M`.</span></span> <span data-ttu-id="2886a-728">`Xi` herhangi bir sınır bulunmazsa, tür çıkarımı başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-728">If for any of the `Xi` no bounds were found, type inference fails.</span></span> <span data-ttu-id="2886a-729">Aksi takdirde, tüm `Xi`, tür çıkarımı sonucu olan karşılık gelen `Si`*sabitlenmiştir* .</span><span class="sxs-lookup"><span data-stu-id="2886a-729">Otherwise, all `Xi` are *fixed* to corresponding `Si`, which are the result of type inference.</span></span>

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a><span data-ttu-id="2886a-730">Bir ifade kümesinin en iyi ortak türünü bulma</span><span class="sxs-lookup"><span data-stu-id="2886a-730">Finding the best common type of a set of expressions</span></span>

<span data-ttu-id="2886a-731">Bazı durumlarda, bir dizi ifade için ortak bir türün çıkarsanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2886a-731">In some cases, a common type needs to be inferred for a set of expressions.</span></span> <span data-ttu-id="2886a-732">Özellikle, örtük olarak yazılan dizilerin öğe türleri ve *blok* gövdeleri olan anonim işlevlerin dönüş türleri bu şekilde bulunur.</span><span class="sxs-lookup"><span data-stu-id="2886a-732">In particular, the element types of implicitly typed arrays and the return types of anonymous functions with *block* bodies are found in this way.</span></span>

<span data-ttu-id="2886a-733">Bu çıkarımı `E1...Em` bir ifade kümesi verildiğinde ıntuicanlı, bir yöntemi çağırmak için eşdeğer olmalıdır</span><span class="sxs-lookup"><span data-stu-id="2886a-733">Intuitively, given a set of expressions `E1...Em` this inference should be equivalent to calling a method</span></span>
```csharp
Tr M<X>(X x1 ... X xm)
```
<span data-ttu-id="2886a-734">bağımsız değişken olarak `Ei`.</span><span class="sxs-lookup"><span data-stu-id="2886a-734">with the `Ei` as arguments.</span></span>

<span data-ttu-id="2886a-735">Daha kesin olarak, çıkarım, *sabit olmayan* bir tür değişkeniyle başlar `X`.</span><span class="sxs-lookup"><span data-stu-id="2886a-735">More precisely, the inference starts out with an *unfixed* type variable `X`.</span></span> <span data-ttu-id="2886a-736">Daha sonra *Çıkış türü* *, her `Ei`* `X`*olarak* yapılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-736">*Output type inferences* are then made *from* each `Ei` *to* `X`.</span></span> <span data-ttu-id="2886a-737">Son olarak, `X` *sabittir* ve başarılı olursa, sonuçta elde edilen tür `S` ifadeler için en iyi ortak türdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-737">Finally, `X` is *fixed* and, if successful, the resulting type `S` is the resulting best common type for the expressions.</span></span> <span data-ttu-id="2886a-738">Böyle `S` yoksa, ifadelerde en iyi ortak tür yoktur.</span><span class="sxs-lookup"><span data-stu-id="2886a-738">If no such `S` exists, the expressions have no best common type.</span></span>

### <a name="overload-resolution"></a><span data-ttu-id="2886a-739">Aşırı yükleme çözümü</span><span class="sxs-lookup"><span data-stu-id="2886a-739">Overload resolution</span></span>

<span data-ttu-id="2886a-740">Aşırı yükleme çözümlemesi, verilen bağımsız değişken listesini ve aday işlev üyelerini çağırmak için en iyi işlev üyesini seçmeye yönelik bir bağlama zamanı mekanizmasıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-740">Overload resolution is a binding-time mechanism for selecting the best function member to invoke given an argument list and a set of candidate function members.</span></span> <span data-ttu-id="2886a-741">Aşırı yükleme çözümlemesi, içindeki C#aşağıdaki farklı bağlamlarda çağrılacak işlev üyesini seçer:</span><span class="sxs-lookup"><span data-stu-id="2886a-741">Overload resolution selects the function member to invoke in the following distinct contexts within C#:</span></span>

*  <span data-ttu-id="2886a-742">*İnvocation_expression* adında bir yöntemin çağrılması ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="2886a-742">Invocation of a method named in an *invocation_expression* ([Method invocations](expressions.md#method-invocations)).</span></span>
*  <span data-ttu-id="2886a-743">*Object_creation_expression* ([nesne oluşturma ifadelerinde](expressions.md#object-creation-expressions)) adlı örnek oluşturucunun çağrılması.</span><span class="sxs-lookup"><span data-stu-id="2886a-743">Invocation of an instance constructor named in an *object_creation_expression* ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>
*  <span data-ttu-id="2886a-744">Bir *element_access* Dizin Oluşturucu erişimcisinin çağrılması ([öğe erişimi](expressions.md#element-access)).</span><span class="sxs-lookup"><span data-stu-id="2886a-744">Invocation of an indexer accessor through an *element_access* ([Element access](expressions.md#element-access)).</span></span>
*  <span data-ttu-id="2886a-745">İfadede önceden tanımlanmış veya Kullanıcı tanımlı bir işlecin çağrılması ([birli operatör aşırı yükleme çözümü](expressions.md#unary-operator-overload-resolution) ve [ikili işleç aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="2886a-745">Invocation of a predefined or user-defined operator referenced in an expression ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)).</span></span>

<span data-ttu-id="2886a-746">Bu bağlamların her biri, yukarıda listelenen bölümlerde ayrıntılı olarak açıklandığı gibi aday işlev üyeleri kümesini ve bağımsız değişkenlerin listesini kendi benzersiz şeklinde tanımlar.</span><span class="sxs-lookup"><span data-stu-id="2886a-746">Each of these contexts defines the set of candidate function members and the list of arguments in its own unique way, as described in detail in the sections listed above.</span></span> <span data-ttu-id="2886a-747">Örneğin, bir yöntem çağrısı için aday kümesi `override` ([üye arama](expressions.md#member-lookup)) olarak işaretlenmiş yöntemleri içermez ve türetilmiş bir sınıftaki herhangi bir yöntem geçerliyse ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)) temel sınıftaki Yöntemler aday değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-747">For example, the set of candidates for a method invocation does not include methods marked `override` ([Member lookup](expressions.md#member-lookup)), and methods in a base class are not candidates if any method in a derived class is applicable ([Method invocations](expressions.md#method-invocations)).</span></span>

<span data-ttu-id="2886a-748">Aday işlev üyeleri ve bağımsız değişken listesi tanımlandıktan sonra, en iyi işlev üyesinin seçimi her durumda aynıdır:</span><span class="sxs-lookup"><span data-stu-id="2886a-748">Once the candidate function members and the argument list have been identified, the selection of the best function member is the same in all cases:</span></span>

*  <span data-ttu-id="2886a-749">Uygulanabilir aday işlev üyeleri kümesi verildiğinde, bu küme içindeki en iyi işlev üyesi bulunur.</span><span class="sxs-lookup"><span data-stu-id="2886a-749">Given the set of applicable candidate function members, the best function member in that set is located.</span></span> <span data-ttu-id="2886a-750">Küme yalnızca bir işlev üyesi içeriyorsa, bu işlev üyesi en iyi işlev üyesidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-750">If the set contains only one function member, then that function member is the best function member.</span></span> <span data-ttu-id="2886a-751">Aksi halde, en iyi işlev üyesi, her bir işlev üyesinin [daha iyi işlev üyesinde](expressions.md#better-function-member)olan kuralları kullanarak diğer tüm işlev üyeleriyle karşılaştırıldığı belirtilen bağımsız değişken listesine göre daha iyi olan bir işlev üyesidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-751">Otherwise, the best function member is the one function member that is better than all other function members with respect to the given argument list, provided that each function member is compared to all other function members using the rules in [Better function member](expressions.md#better-function-member).</span></span> <span data-ttu-id="2886a-752">Tüm diğer işlev üyelerinden daha iyi olan tam olarak bir işlev üyesi yoksa, işlev üye çağrısı belirsizdir ve bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-752">If there is not exactly one function member that is better than all other function members, then the function member invocation is ambiguous and a binding-time error occurs.</span></span>

<span data-ttu-id="2886a-753">Aşağıdaki bölümlerde, koşullara ***uygun işlev üyesinin*** ve ***daha iyi işlev üyesinin***tam anlamları tanımlanacaktır.</span><span class="sxs-lookup"><span data-stu-id="2886a-753">The following sections define the exact meanings of the terms ***applicable function member*** and ***better function member***.</span></span>

#### <a name="applicable-function-member"></a><span data-ttu-id="2886a-754">Geçerli işlev üyesi</span><span class="sxs-lookup"><span data-stu-id="2886a-754">Applicable function member</span></span>

<span data-ttu-id="2886a-755">Bir işlev üyesi, aşağıdakilerin tümü doğru olduğunda `A` bağımsız değişken listesine göre ***geçerli bir işlev üyesi*** olarak kabul edilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-755">A function member is said to be an ***applicable function member*** with respect to an argument list `A` when all of the following are true:</span></span>

*  <span data-ttu-id="2886a-756">`A` her bir bağımsız değişken, [karşılık gelen parametrelerde](expressions.md#corresponding-parameters)açıklandığı gibi işlev üye bildirimindeki bir parametreye karşılık gelir ve hiçbir bağımsız değişkenin karşılık geldiği parametre isteğe bağlı bir parametredir.</span><span class="sxs-lookup"><span data-stu-id="2886a-756">Each argument in `A` corresponds to a parameter in the function member declaration as described in [Corresponding parameters](expressions.md#corresponding-parameters), and any parameter to which no argument corresponds is an optional parameter.</span></span>
*  <span data-ttu-id="2886a-757">`A`her bağımsız değişken için, bağımsız değişkenin (örn. değer, `ref`veya `out`) parametre geçirme modu, karşılık gelen parametrenin parametre geçirme moduyla aynıdır ve</span><span class="sxs-lookup"><span data-stu-id="2886a-757">For each argument in `A`, the parameter passing mode of the argument (i.e., value, `ref`, or `out`) is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="2886a-758">bir değer parametresi veya bir parametre dizisi için, bağımsız değişkenden karşılık gelen parametrenin türüne örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) var veya</span><span class="sxs-lookup"><span data-stu-id="2886a-758">for a value parameter or a parameter array, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="2886a-759">bir `ref` veya `out` parametresi için bağımsız değişkenin türü karşılık gelen parametrenin türüyle aynıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-759">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span> <span data-ttu-id="2886a-760">All, bir `ref` veya `out` parametresi geçilen bağımsız değişken için bir diğer addır.</span><span class="sxs-lookup"><span data-stu-id="2886a-760">After all, a `ref` or `out` parameter is an alias for the argument passed.</span></span>

<span data-ttu-id="2886a-761">Bir parametre dizisi içeren bir işlev üyesi için, işlev üyesi Yukarıdaki kurallar tarafından geçerliyse, bunun ***normal biçiminde***geçerli olduğu söylenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-761">For a function member that includes a parameter array, if the function member is applicable by the above rules, it is said to be applicable in its ***normal form***.</span></span> <span data-ttu-id="2886a-762">Bir parametre dizisi içeren bir işlev üyesi normal biçiminde geçerli değilse, işlev üyesi ***genişletilmiş biçimde***uygulanabilir olabilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-762">If a function member that includes a parameter array is not applicable in its normal form, the function member may instead be applicable in its ***expanded form***:</span></span>

*  <span data-ttu-id="2886a-763">Genişletilmiş form, işlev üyesi bildirimindeki parametre dizisinin parametre dizisinin öğe türünün sıfır veya daha fazla değer parametresiyle değiştirilerek oluşturulur ve bağımsız değişken listesindeki bağımsız değişkenlerin sayısı `A`, toplam parametre sayısıyla eşleşir.</span><span class="sxs-lookup"><span data-stu-id="2886a-763">The expanded form is constructed by replacing the parameter array in the function member declaration with zero or more value parameters of the element type of the parameter array such that the number of arguments in the argument list `A` matches the total number of parameters.</span></span> <span data-ttu-id="2886a-764">`A`, işlev üyesi bildirimindeki sabit parametrelerin sayısından daha az bağımsız değişken içeriyorsa, işlev üyesinin genişletilmiş biçimi oluşturulamaz ve bu nedenle geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-764">If `A` has fewer arguments than the number of fixed parameters in the function member declaration, the expanded form of the function member cannot be constructed and is thus not applicable.</span></span>
*  <span data-ttu-id="2886a-765">Aksi takdirde genişletilmiş form, bağımsız değişkeninin parametre geçirme modu, karşılık gelen parametrenin parametre geçirme moduyla aynı olduğunda, `A` her bir bağımsız değişken için geçerlidir ve</span><span class="sxs-lookup"><span data-stu-id="2886a-765">Otherwise, the expanded form is applicable if for each argument in `A` the parameter passing mode of the argument is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="2886a-766">sabit bir değer parametresi veya genişletme tarafından oluşturulan bir değer parametresi için, bağımsız değişkenin türünden, karşılık gelen parametrenin türüne örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) vardır veya</span><span class="sxs-lookup"><span data-stu-id="2886a-766">for a fixed value parameter or a value parameter created by the expansion, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the type of the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="2886a-767">bir `ref` veya `out` parametresi için bağımsız değişkenin türü karşılık gelen parametrenin türüyle aynıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-767">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span>

#### <a name="better-function-member"></a><span data-ttu-id="2886a-768">Daha iyi işlev üyesi</span><span class="sxs-lookup"><span data-stu-id="2886a-768">Better function member</span></span>

<span data-ttu-id="2886a-769">Daha iyi işlev üyesini belirleme amaçları doğrultusunda, yalnızca bağımsız değişken ifadelerinin orijinal bağımsız değişken listesinde göründükleri sırada kendilerini içeren bir aşağı açılan bağımsız değişken listesi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-769">For the purposes of determining the better function member, a stripped-down argument list A is constructed containing just the argument expressions themselves in the order they appear in the original argument list.</span></span>

<span data-ttu-id="2886a-770">Aday işlev üyelerinin her biri için parametre listeleri aşağıdaki şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="2886a-770">Parameter lists for each of the candidate function members are constructed in the following way:</span></span>

*  <span data-ttu-id="2886a-771">İşlev üyesi yalnızca genişletilmiş biçimde uygulanabiliyorsa genişletilmiş form kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-771">The expanded form is used if the function member was applicable only in the expanded form.</span></span>
*  <span data-ttu-id="2886a-772">Karşılık gelen bağımsız değişken içermeyen isteğe bağlı parametreler parametre listesinden kaldırılır</span><span class="sxs-lookup"><span data-stu-id="2886a-772">Optional parameters with no corresponding arguments are removed from the parameter list</span></span>
*  <span data-ttu-id="2886a-773">Parametreler, bağımsız değişken listesindeki ilgili bağımsız değişkenle aynı konumda gerçekleşmeleri için yeniden sıralanmaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-773">The parameters are reordered so that they occur at the same position as the corresponding argument in the argument list.</span></span>

<span data-ttu-id="2886a-774">Bağımsız değişken listesi `A` verildiğinde `{E1, E2, ..., En}` ve iki uygulanabilir ***işlev üyesi `Mp`*** ve `Mq` `{P1, P2, ..., Pn}` ve `{Q1, Q2, ..., Qn}`parametre türleri ile `Mp`</span><span class="sxs-lookup"><span data-stu-id="2886a-774">Given an argument list `A` with a set of argument expressions `{E1, E2, ..., En}` and two applicable function members `Mp` and `Mq` with parameter types `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}`, `Mp` is defined to be a ***better function member*** than `Mq` if</span></span>

*  <span data-ttu-id="2886a-775">Her bağımsız değişken için, `Ex` 'den `Qx` örtük dönüştürme `Ex` ' den `Px`' a örtük dönüşümden daha iyi değildir ve</span><span class="sxs-lookup"><span data-stu-id="2886a-775">for each argument, the implicit conversion from `Ex` to `Qx` is not better than the implicit conversion from `Ex` to `Px`, and</span></span>
*  <span data-ttu-id="2886a-776">en az bir bağımsız değişken için `Ex` 'den `Px` dönüştürme `Ex` 'dan `Qx`dönüştürenden daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-776">for at least one argument, the conversion from `Ex` to `Px` is better than the conversion from `Ex` to `Qx`.</span></span>

<span data-ttu-id="2886a-777">Bu değerlendirmeyi gerçekleştirirken, `Mp` veya `Mq` genişletilmiş biçimde geçerliyse, `Px` veya `Qx` parametre listesinin genişletilmiş biçimindeki bir parametreye başvurur.</span><span class="sxs-lookup"><span data-stu-id="2886a-777">When performing this evaluation, if `Mp` or `Mq` is applicable in its expanded form, then `Px` or `Qx` refers to a parameter in the expanded form of the parameter list.</span></span>

<span data-ttu-id="2886a-778">Parametre türü dizileri `{P1, P2, ..., Pn}` ve `{Q1, Q2, ..., Qn}` eşdeğerdir (yani, her bir `Pi` karşılık gelen `Qi`bir kimlik dönüştürmesi vardır), daha iyi işlev üyesini belirleyebilmek için aşağıdaki bağlama kuralları sırasıyla uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-778">In case the parameter type sequences `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}` are equivalent (i.e. each `Pi` has an identity conversion to the corresponding `Qi`), the following tie-breaking rules are applied, in order, to determine the better function member.</span></span>

*  <span data-ttu-id="2886a-779">`Mp` genel olmayan bir yöntem ise ve `Mq` genel bir yöntem ise, `Mp` `Mq`daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-779">If `Mp` is a non-generic method and `Mq` is a generic method, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="2886a-780">Aksi takdirde, `Mp` normal biçiminde uygulanabiliyorsa ve `Mq` bir `params` dizisine sahipse ve yalnızca genişletilmiş biçimde geçerliyse, `Mp` `Mq`daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-780">Otherwise, if `Mp` is applicable in its normal form and `Mq` has a `params` array and is applicable only in its expanded form, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="2886a-781">Aksi takdirde, `Mp` `Mq`daha fazla sayıda parametre içeriyorsa `Mp` `Mq`daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-781">Otherwise, if `Mp` has more declared parameters than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="2886a-782">Her iki yöntem de `params` dizileri içeriyorsa ve yalnızca genişletilmiş formlarında uygulanabildiğinde bu durum oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-782">This can occur if both methods have `params` arrays and are applicable only in their expanded forms.</span></span>
*  <span data-ttu-id="2886a-783">Aksi takdirde, `Mp` tüm parametrelerinin karşılık gelen bir bağımsız değişkeni varsa, varsayılan bağımsız değişkenlerin `Mq` ' de en az bir isteğe bağlı parametre yerine kullanılması gerekiyorsa `Mp` `Mq`daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-783">Otherwise if all parameters of `Mp` have a corresponding argument whereas default arguments need to be substituted for at least one optional parameter in `Mq` then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="2886a-784">Aksi takdirde, `Mp` `Mq`daha fazla parametre türü varsa `Mp` `Mq`daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-784">Otherwise, if `Mp` has more specific parameter types than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="2886a-785">`{R1, R2, ..., Rn}` ve `{S1, S2, ..., Sn}` ' nin örneklenmiş ve genişletilmemiş parametre türlerini `Mp` ve `Mq`temsil etmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="2886a-785">Let `{R1, R2, ..., Rn}` and `{S1, S2, ..., Sn}` represent the uninstantiated and unexpanded parameter types of `Mp` and `Mq`.</span></span> <span data-ttu-id="2886a-786">`Mp`parametre türleri, her bir parametre için `Mq`' den daha belirgin `Sx``Rx` ve en az bir parametre için `Rx` `Sx`daha büyüktür:</span><span class="sxs-lookup"><span data-stu-id="2886a-786">`Mp`'s parameter types are more specific than `Mq`'s if, for each parameter, `Rx` is not less specific than `Sx`, and, for at least one parameter, `Rx` is more specific than `Sx`:</span></span>
   *  <span data-ttu-id="2886a-787">Tür parametresi, tür olmayan bir parametreden daha az özgüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-787">A type parameter is less specific than a non-type parameter.</span></span>
   *  <span data-ttu-id="2886a-788">Yinelemeli olarak, oluşturulmuş bir tür, en az bir tür bağımsız değişkeni daha büyükse ve hiçbir tür bağımsız değişkeni diğer bir tür bağımsız değişkeniyle daha az özel değilse, oluşturulmuş bir tür farklı oluşturulmuş türden (aynı sayıda tür bağımsız değişkenle) daha özgüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-788">Recursively, a constructed type is more specific than another constructed type (with the same number of type arguments) if at least one type argument is more specific and no type argument is less specific than the corresponding type argument in the other.</span></span>
   *  <span data-ttu-id="2886a-789">Bir dizi türü, birincinin öğe türü ikincisinin öğe türünden daha belirgin ise, başka bir dizi türünden (aynı sayıda boyut ile) daha özgüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-789">An array type is more specific than another array type (with the same number of dimensions) if the element type of the first is more specific than the element type of the second.</span></span>
*  <span data-ttu-id="2886a-790">Aksi takdirde, bir üye yükseltilmemiş olmayan bir işleçtir ve diğeri bir yükseltilmemiş işleçse, yükseltilmemiş olmayan bir tane daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-790">Otherwise if one member is a non-lifted operator and  the other is a lifted operator, the non-lifted one is better.</span></span>
*  <span data-ttu-id="2886a-791">Aksi halde, hiçbir işlev üyesi daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-791">Otherwise, neither function member is better.</span></span>

#### <a name="better-conversion-from-expression"></a><span data-ttu-id="2886a-792">Deyimden daha iyi dönüştürme</span><span class="sxs-lookup"><span data-stu-id="2886a-792">Better conversion from expression</span></span>

<span data-ttu-id="2886a-793">Bir ifadeden `E` bir tür `T1`ve bir ifadeden `E` bir tür `T2`dönüştüren `C2` örtük bir dönüştürme olan örtük bir dönüştürme `C1` verildiğinde, `C1` `C2` tam olarak eşleşmiyorsa ve aşağıdakilerden en az biri olan `E` `T2` ***daha iyi bir dönüştürme*** olur:</span><span class="sxs-lookup"><span data-stu-id="2886a-793">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>

* <span data-ttu-id="2886a-794">`E` `T1` tam olarak eşleşiyor ([tam eşleştirme ifadesi](expressions.md#exactly-matching-expression))</span><span class="sxs-lookup"><span data-stu-id="2886a-794">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
* <span data-ttu-id="2886a-795">`T1`, `T2` daha iyi bir dönüştürme hedefi ([daha iyi dönüştürme hedefi](expressions.md#better-conversion-target))</span><span class="sxs-lookup"><span data-stu-id="2886a-795">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target))</span></span>

#### <a name="exactly-matching-expression"></a><span data-ttu-id="2886a-796">Tam eşleştirme Ifadesi</span><span class="sxs-lookup"><span data-stu-id="2886a-796">Exactly matching Expression</span></span>

<span data-ttu-id="2886a-797">Bir ifade `E` ve bir tür `T`verildiğinde, aşağıdakilerden biri varsa `T` `E` tam olarak eşleşir:</span><span class="sxs-lookup"><span data-stu-id="2886a-797">Given an expression `E` and a type `T`, `E` exactly matches `T` if one of the following holds:</span></span>

*  <span data-ttu-id="2886a-798">`E` `S`bir tür ve `S` bir kimlik dönüştürmesi vardır `T`</span><span class="sxs-lookup"><span data-stu-id="2886a-798">`E` has a type `S`, and an identity conversion exists from `S` to `T`</span></span>
*  <span data-ttu-id="2886a-799">`E` anonim bir işlevdir, `T` bir temsilci türü `D` ya da bir ifade ağacı türü `Expression<D>` ve aşağıdakilerden birini içerir:</span><span class="sxs-lookup"><span data-stu-id="2886a-799">`E` is an anonymous function, `T` is either a delegate type `D` or an expression tree type `Expression<D>` and one of the following holds:</span></span>
   *  <span data-ttu-id="2886a-800">Çıkarılan bir dönüş türü, `D` ([çıkartılan dönüş türü](expressions.md#inferred-return-type)) parametre listesi bağlamında `E` için `X` vardır ve `X` bir kimlik dönüştürmesi `D` dönüş türüne mevcuttur</span><span class="sxs-lookup"><span data-stu-id="2886a-800">An inferred return type `X` exists for `E` in the context of the parameter list of `D` ([Inferred return type](expressions.md#inferred-return-type)), and an identity conversion exists from `X` to the return type of `D`</span></span>
   *  <span data-ttu-id="2886a-801">`E` zaman uyumsuz değil ve `D` bir dönüş türüne sahip `Y` ya da `E` zaman uyumsuz ve `D` bir dönüş türü `Task<Y>`ve aşağıdakilerden biri şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="2886a-801">Either `E` is non-async and `D` has a return type `Y` or `E` is async and `D` has a return type `Task<Y>`, and one of the following holds:</span></span>
      * <span data-ttu-id="2886a-802">`E` gövdesi tam olarak eşleşen bir ifadedir `Y`</span><span class="sxs-lookup"><span data-stu-id="2886a-802">The body of `E` is an expression that exactly matches `Y`</span></span>
      * <span data-ttu-id="2886a-803">`E` gövdesi, her dönüş deyiminin tam olarak eşleşen bir ifade döndürdüğü bir deyim bloğudur `Y`</span><span class="sxs-lookup"><span data-stu-id="2886a-803">The body of `E` is a statement block where every return statement returns an expression that exactly matches `Y`</span></span>

#### <a name="better-conversion-target"></a><span data-ttu-id="2886a-804">Daha iyi dönüştürme hedefi</span><span class="sxs-lookup"><span data-stu-id="2886a-804">Better conversion target</span></span>

<span data-ttu-id="2886a-805">`T1` ve `T2`iki farklı tür verildiğinde `T1`, `T2` ve `T1` arasında örtük dönüştürme yoksa aşağıdakilerden en az biri varsa, `T2` daha iyi bir dönüştürme hedefidir:</span><span class="sxs-lookup"><span data-stu-id="2886a-805">Given two different types `T1` and `T2`, `T1` is a better conversion target than `T2` if no implicit conversion from `T2` to `T1` exists, and at least one of the following holds:</span></span>

*  <span data-ttu-id="2886a-806">`T1` 'den `T2` örtük bir dönüştürme var</span><span class="sxs-lookup"><span data-stu-id="2886a-806">An implicit conversion from `T1` to `T2` exists</span></span>
*  <span data-ttu-id="2886a-807">`T1`, bir temsilci türü `D1` ya da bir ifade ağacı türü `Expression<D1>``T2` bir temsilci türü `D2` veya bir ifade ağacı türü `Expression<D2>`, `D1` bir dönüş türü `S1` ve aşağıdakilerden biri tutalardan biridir:</span><span class="sxs-lookup"><span data-stu-id="2886a-807">`T1` is either a delegate type `D1` or an expression tree type `Expression<D1>`, `T2` is either a delegate type `D2` or an expression tree type `Expression<D2>`, `D1` has a return type `S1` and one of the following holds:</span></span>
   * <span data-ttu-id="2886a-808">`D2` void döndürüyor</span><span class="sxs-lookup"><span data-stu-id="2886a-808">`D2` is void returning</span></span>
   * <span data-ttu-id="2886a-809">`D2`, `S2`bir dönüş türüne sahiptir ve `S1` daha iyi bir dönüştürme hedefidir `S2`</span><span class="sxs-lookup"><span data-stu-id="2886a-809">`D2` has a return type `S2`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="2886a-810">`T1` `Task<S1>`, `T2` `Task<S2>`ve `S1` daha iyi bir dönüştürme hedefi `S2`</span><span class="sxs-lookup"><span data-stu-id="2886a-810">`T1` is `Task<S1>`, `T2` is `Task<S2>`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="2886a-811">`T1` `S1` veya `S1?` `S1` işaretli bir integral türüdür ve `T2`, `S2` işaretsiz bir integral türü olduğu `S2?` veya `S2`.</span><span class="sxs-lookup"><span data-stu-id="2886a-811">`T1` is `S1` or `S1?` where `S1` is a signed integral type, and `T2` is `S2` or `S2?` where `S2` is an unsigned integral type.</span></span> <span data-ttu-id="2886a-812">Özellikle:</span><span class="sxs-lookup"><span data-stu-id="2886a-812">Specifically:</span></span>
   * <span data-ttu-id="2886a-813">`S1` `sbyte` ve `S2` `byte`, `ushort`, `uint`veya `ulong`</span><span class="sxs-lookup"><span data-stu-id="2886a-813">`S1` is `sbyte` and `S2` is `byte`, `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="2886a-814">`S1` `short` ve `S2` `ushort`, `uint`veya `ulong`</span><span class="sxs-lookup"><span data-stu-id="2886a-814">`S1` is `short` and `S2` is `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="2886a-815">`S1` `int` ve `S2` `uint`veya `ulong`</span><span class="sxs-lookup"><span data-stu-id="2886a-815">`S1` is `int` and `S2` is `uint`, or `ulong`</span></span>
   * <span data-ttu-id="2886a-816">`S1` `long` ve `S2` `ulong`</span><span class="sxs-lookup"><span data-stu-id="2886a-816">`S1` is `long` and `S2` is `ulong`</span></span>

#### <a name="overloading-in-generic-classes"></a><span data-ttu-id="2886a-817">Genel sınıflarda aşırı yükleme</span><span class="sxs-lookup"><span data-stu-id="2886a-817">Overloading in generic classes</span></span>

<span data-ttu-id="2886a-818">Belirtilen imzaların benzersiz olması gerekir, ancak tür bağımsız değişkenlerinin değişimi de özdeş imzalara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-818">While signatures as declared must be unique, it is possible that substitution of type arguments results in identical signatures.</span></span> <span data-ttu-id="2886a-819">Bu gibi durumlarda, yukarıdaki aşırı yükleme çözümünün bağlama kuralları en belirli üyeyi seçer.</span><span class="sxs-lookup"><span data-stu-id="2886a-819">In such cases, the tie-breaking rules of overload resolution above will pick the most specific member.</span></span>

<span data-ttu-id="2886a-820">Aşağıdaki örneklerde, bu kurala göre geçerli ve geçersiz olan aşırı yüklemeler gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="2886a-820">The following examples show overloads that are valid and invalid according to this rule:</span></span>

```csharp
interface I1<T> {...}

interface I2<T> {...}

class G1<U>
{
    int F1(U u);                  // Overload resolution for G<int>.F1
    int F1(int i);                // will pick non-generic

    void F2(I1<U> a);             // Valid overload
    void F2(I2<U> a);
}

class G2<U,V>
{
    void F3(U u, V v);            // Valid, but overload resolution for
    void F3(V v, U u);            // G2<int,int>.F3 will fail

    void F4(U u, I1<V> v);        // Valid, but overload resolution for    
    void F4(I1<V> v, U u);        // G2<I1<int>,int>.F4 will fail

    void F5(U u1, I1<V> v2);      // Valid overload
    void F5(V v1, U u2);

    void F6(ref U u);             // valid overload
    void F6(out V v);
}
```

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a><span data-ttu-id="2886a-821">Dinamik aşırı yükleme çözümlemesi için derleme zamanı denetimi</span><span class="sxs-lookup"><span data-stu-id="2886a-821">Compile-time checking of dynamic overload resolution</span></span>

<span data-ttu-id="2886a-822">Dinamik olarak bağlı işlemler için, derleme zamanında çözümleme için olası aday adayları bilinmiyor.</span><span class="sxs-lookup"><span data-stu-id="2886a-822">For most dynamically bound operations the set of possible candidates for resolution is unknown at compile-time.</span></span> <span data-ttu-id="2886a-823">Ancak, bazı durumlarda aday kümesi derleme zamanında bilinir:</span><span class="sxs-lookup"><span data-stu-id="2886a-823">In certain cases, however the candidate set is known at compile-time:</span></span>

*  <span data-ttu-id="2886a-824">Dinamik bağımsız değişkenlerle statik yöntem çağrıları</span><span class="sxs-lookup"><span data-stu-id="2886a-824">Static method calls with dynamic arguments</span></span>
*  <span data-ttu-id="2886a-825">Örnek yöntemi, alıcının dinamik bir ifade olmadığı yerleri çağırır</span><span class="sxs-lookup"><span data-stu-id="2886a-825">Instance method calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="2886a-826">Alıcının dinamik bir ifade olmadığı Dizin Oluşturucu çağrıları</span><span class="sxs-lookup"><span data-stu-id="2886a-826">Indexer calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="2886a-827">Dinamik bağımsız değişkenlerle Oluşturucu çağrıları</span><span class="sxs-lookup"><span data-stu-id="2886a-827">Constructor calls with dynamic arguments</span></span>

<span data-ttu-id="2886a-828">Bu durumlarda, her bir aday için sınırlı bir derleme zamanı denetimi gerçekleştirilerek, bunlardan herhangi birinin çalışma zamanında uygulayıp uygulamamadığını görebilirler. Bu denetim aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="2886a-828">In these cases a limited compile-time check is performed for each candidate to see if any of them could possibly apply at run-time.This check consists of the following steps:</span></span>

*  <span data-ttu-id="2886a-829">Kısmi tür çıkarımı: `dynamic` türünde bir bağımsız değişkene doğrudan veya dolaylı olarak bağımlı olmayan herhangi bir tür bağımsız değişkeni, [tür çıkarımı](expressions.md#type-inference)kuralları kullanılarak çıkarsanacaktır.</span><span class="sxs-lookup"><span data-stu-id="2886a-829">Partial type inference: Any type argument that does not depend directly or indirectly on an argument of type `dynamic` is inferred using the rules of [Type inference](expressions.md#type-inference).</span></span> <span data-ttu-id="2886a-830">Kalan tür bağımsız değişkenleri bilinmiyor.</span><span class="sxs-lookup"><span data-stu-id="2886a-830">The remaining type arguments are unknown.</span></span>
*  <span data-ttu-id="2886a-831">Kısmi uygulanabilirlik denetimi: uygulanabilirliği, [uygulanabilir işlev üyesine](expressions.md#applicable-function-member)göre denetlenir, ancak türleri bilinmiyor olan parametreleri yoksayar.</span><span class="sxs-lookup"><span data-stu-id="2886a-831">Partial applicability check: Applicability is checked according to [Applicable function member](expressions.md#applicable-function-member), but ignoring parameters whose types are unknown.</span></span>
*  <span data-ttu-id="2886a-832">Bu testi hiçbir aday geçirmediğinde, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-832">If no candidate passes this test, a compile-time error occurs.</span></span>

### <a name="function-member-invocation"></a><span data-ttu-id="2886a-833">İşlev üyesi çağırma</span><span class="sxs-lookup"><span data-stu-id="2886a-833">Function member invocation</span></span>

<span data-ttu-id="2886a-834">Bu bölümde, belirli bir işlev üyesini çağırmak için çalışma zamanında gerçekleşen işlem açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="2886a-834">This section describes the process that takes place at run-time to invoke a particular function member.</span></span> <span data-ttu-id="2886a-835">Bir bağlama zamanı işleminin, büyük olasılıkla bir aday işlev üyesi kümesine aşırı yükleme çözümlemesi uygulayarak, çağırmak için belirli bir üyeyi daha önce belirlediğini kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-835">It is assumed that a binding-time process has already determined the particular member to invoke, possibly by applying overload resolution to a set of candidate function members.</span></span>

<span data-ttu-id="2886a-836">Çağırma işlemini açıklama amaçlarıyla, işlev üyeleri iki kategoriye ayrılmıştır:</span><span class="sxs-lookup"><span data-stu-id="2886a-836">For purposes of describing the invocation process, function members are divided into two categories:</span></span>

*  <span data-ttu-id="2886a-837">Statik işlev üyeleri.</span><span class="sxs-lookup"><span data-stu-id="2886a-837">Static function members.</span></span> <span data-ttu-id="2886a-838">Bunlar örnek oluşturucular, statik yöntemler, statik özellik erişimcileri ve Kullanıcı tanımlı işleçlerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-838">These are instance constructors, static methods, static property accessors, and user-defined operators.</span></span> <span data-ttu-id="2886a-839">Statik işlev üyeleri her zaman sanal değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-839">Static function members are always non-virtual.</span></span>
*  <span data-ttu-id="2886a-840">Örnek işlevi üyeleri.</span><span class="sxs-lookup"><span data-stu-id="2886a-840">Instance function members.</span></span> <span data-ttu-id="2886a-841">Bunlar örnek yöntemleridir, örnek özellik erişimcileri ve Dizin Oluşturucu erişimcileri.</span><span class="sxs-lookup"><span data-stu-id="2886a-841">These are instance methods, instance property accessors, and indexer accessors.</span></span> <span data-ttu-id="2886a-842">Örnek işlevi üyeleri sanal olmayan veya sanal değil, her zaman belirli bir örnek üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-842">Instance function members are either non-virtual or virtual, and are always invoked on a particular instance.</span></span> <span data-ttu-id="2886a-843">Örnek bir örnek ifadesi tarafından hesaplanır ve işlev üyesi içinde `this` ([Bu erişim](expressions.md#this-access)) olarak erişilebilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-843">The instance is computed by an instance expression, and it becomes accessible within the function member as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="2886a-844">Bir işlev üyesi çağrısının çalışma zamanı işleme aşağıdaki adımlardan oluşur; burada `M` İşlev üyesidir ve `M` bir örnek üyesiyse `E` örnek ifadedir:</span><span class="sxs-lookup"><span data-stu-id="2886a-844">The run-time processing of a function member invocation consists of the following steps, where `M` is the function member and, if `M` is an instance member, `E` is the instance expression:</span></span>

*  <span data-ttu-id="2886a-845">`M` statik bir işlev üyesiyse:</span><span class="sxs-lookup"><span data-stu-id="2886a-845">If `M` is a static function member:</span></span>
   * <span data-ttu-id="2886a-846">Bağımsız değişken listesi, [bağımsız değişken listelerinde](expressions.md#argument-lists)açıklandığı şekilde değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-846">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="2886a-847">`M` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-847">`M` is invoked.</span></span>

*  <span data-ttu-id="2886a-848">`M` bir *value_type*belirtilen örnek işlev üyesiyse:</span><span class="sxs-lookup"><span data-stu-id="2886a-848">If `M` is an instance function member declared in a *value_type*:</span></span>
   * <span data-ttu-id="2886a-849">`E` değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-849">`E` is evaluated.</span></span> <span data-ttu-id="2886a-850">Bu değerlendirme bir özel duruma neden olursa başka bir adım yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-850">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="2886a-851">`E` bir değişken olarak sınıflandırılmadıysa, `E`türünün geçici bir yerel değişkeni oluşturulur ve bu değişkene `E` değeri atanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-851">If `E` is not classified as a variable, then a temporary local variable of `E`'s type is created and the value of `E` is assigned to that variable.</span></span> <span data-ttu-id="2886a-852">`E` daha sonra bu geçici yerel değişkene bir başvuru olarak yeniden sınıflanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-852">`E` is then reclassified as a reference to that temporary local variable.</span></span> <span data-ttu-id="2886a-853">Geçici değişken `M`içinde `this` olarak erişilebilir, ancak başka hiçbir şekilde erişilemez.</span><span class="sxs-lookup"><span data-stu-id="2886a-853">The temporary variable is accessible as `this` within `M`, but not in any other way.</span></span> <span data-ttu-id="2886a-854">Bu nedenle, yalnızca `E` gerçek bir değişken olduğunda, çağıranın `M` `this`yaptığı değişiklikleri gözlemlemek mümkün olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-854">Thus, only when `E` is a true variable is it possible for the caller to observe the changes that `M` makes to `this`.</span></span>
   * <span data-ttu-id="2886a-855">Bağımsız değişken listesi, [bağımsız değişken listelerinde](expressions.md#argument-lists)açıklandığı şekilde değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-855">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="2886a-856">`M` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-856">`M` is invoked.</span></span> <span data-ttu-id="2886a-857">`E` başvurduğu değişken `this`tarafından başvurulan değişken olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-857">The variable referenced by `E` becomes the variable referenced by `this`.</span></span>

*  <span data-ttu-id="2886a-858">`M` bir *reference_type*belirtilen örnek işlev üyesiyse:</span><span class="sxs-lookup"><span data-stu-id="2886a-858">If `M` is an instance function member declared in a *reference_type*:</span></span>
   * <span data-ttu-id="2886a-859">`E` değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-859">`E` is evaluated.</span></span> <span data-ttu-id="2886a-860">Bu değerlendirme bir özel duruma neden olursa başka bir adım yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-860">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="2886a-861">Bağımsız değişken listesi, [bağımsız değişken listelerinde](expressions.md#argument-lists)açıklandığı şekilde değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-861">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="2886a-862">`E` türü bir *value_type*ise, `E` `object`türüne dönüştürmek için bir paketleme dönüştürmesi ([kutulama dönüştürmeleri](types.md#boxing-conversions)) gerçekleştirilir ve `E` aşağıdaki adımlarda `object` türünde olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-862">If the type of `E` is a *value_type*, a boxing conversion ([Boxing conversions](types.md#boxing-conversions)) is performed to convert `E` to type `object`, and `E` is considered to be of type `object` in the following steps.</span></span> <span data-ttu-id="2886a-863">Bu durumda, `M` yalnızca bir `System.Object`üyesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-863">In this case, `M` could only be a member of `System.Object`.</span></span>
   * <span data-ttu-id="2886a-864">`E` değeri geçerli olacak şekilde işaretlendi.</span><span class="sxs-lookup"><span data-stu-id="2886a-864">The value of `E` is checked to be valid.</span></span> <span data-ttu-id="2886a-865">`E` değeri `null`, bir `System.NullReferenceException` oluşturulur ve başka bir adım yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-865">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
   * <span data-ttu-id="2886a-866">Çağrılacak işlev üyesi uygulama belirlenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-866">The function member implementation to invoke is determined:</span></span>
     * <span data-ttu-id="2886a-867">`E` bağlama zamanı türü bir arabirimdir, çağrılacak işlev üyesi, `E`tarafından başvurulan örneğin çalışma zamanı türü tarafından verilen `M` uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-867">If the binding-time type of `E` is an interface, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="2886a-868">Bu işlev üyesi, `E`tarafından başvurulan örneğin çalışma zamanı türü tarafından verilen `M` uygulamasını belirlemekte kullanılacak arabirim eşleme kuralları ([arabirim eşleme](interfaces.md#interface-mapping)) uygulanarak belirlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-868">This function member is determined by applying the interface mapping rules ([Interface mapping](interfaces.md#interface-mapping)) to determine the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="2886a-869">Aksi takdirde, `M` bir sanal işlev üyesiyse, çağrılacak işlev üyesi, `E`tarafından başvurulan örneğin çalışma zamanı türü tarafından verilen `M` uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-869">Otherwise, if `M` is a virtual function member, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="2886a-870">Bu işlev üyesi, `E`tarafından başvurulan örneğin çalışma zamanı türüne göre `M` en fazla türetilmiş uygulamayı ([sanal yöntemleri](classes.md#virtual-methods)) belirlemek için kurallar uygulanarak belirlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-870">This function member is determined by applying the rules for determining the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of `M` with respect to the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="2886a-871">Aksi takdirde, `M` sanal olmayan bir işlev üyesi ve çağrılacak işlev üyesi `M` kendisidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-871">Otherwise, `M` is a non-virtual function member, and the function member to invoke is `M` itself.</span></span>
   * <span data-ttu-id="2886a-872">Yukarıdaki adımda belirlenen işlev üyesi uygulama çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-872">The function member implementation determined in the step above is invoked.</span></span> <span data-ttu-id="2886a-873">`E` başvurduğu nesne `this`tarafından başvurulan nesne haline gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-873">The object referenced by `E` becomes the object referenced by `this`.</span></span>

#### <a name="invocations-on-boxed-instances"></a><span data-ttu-id="2886a-874">Paketlenmiş örneklerde etkinleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-874">Invocations on boxed instances</span></span>

<span data-ttu-id="2886a-875">Bir *value_type* uygulanan bir işlev üyesi, aşağıdaki durumlarda bu *value_type* paketlenmiş bir örneği aracılığıyla çağrılabilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-875">A function member implemented in a *value_type* can be invoked through a boxed instance of that *value_type* in the following situations:</span></span>

*  <span data-ttu-id="2886a-876">İşlev üyesi, türden devralınan bir yöntemin `override` `object` ve `object`türünde bir örnek ifadesi aracılığıyla çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-876">When the function member is an `override` of a method inherited from type `object` and is invoked through an instance expression of type `object`.</span></span>
*  <span data-ttu-id="2886a-877">İşlev üyesi bir arabirim işlevi üyesinin bir uygulamasıdır ve bir *interface_type*örnek ifadesi aracılığıyla çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-877">When the function member is an implementation of an interface function member and is invoked through an instance expression of an *interface_type*.</span></span>
*  <span data-ttu-id="2886a-878">İşlev üyesi bir temsilci aracılığıyla çağrıldığında.</span><span class="sxs-lookup"><span data-stu-id="2886a-878">When the function member is invoked through a delegate.</span></span>

<span data-ttu-id="2886a-879">Bu durumlarda kutulanmış örnek, *value_type*bir değişken içeren olarak değerlendirilir ve bu değişken, işlev üyesi çağrısı içinde `this` tarafından başvurulan değişken olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-879">In these situations, the boxed instance is considered to contain a variable of the *value_type*, and this variable becomes the variable referenced by `this` within the function member invocation.</span></span> <span data-ttu-id="2886a-880">Özellikle bu, bir işlev üyesinin paketlenmiş bir örnek üzerinde çağrıldığı anlamına gelir, işlev üyesinin kutulanmış örnekte bulunan değeri değiştirmesi mümkündür.</span><span class="sxs-lookup"><span data-stu-id="2886a-880">In particular, this means that when a function member is invoked on a boxed instance, it is possible for the function member to modify the value contained in the boxed instance.</span></span>

## <a name="primary-expressions"></a><span data-ttu-id="2886a-881">Birincil ifadeler</span><span class="sxs-lookup"><span data-stu-id="2886a-881">Primary expressions</span></span>

<span data-ttu-id="2886a-882">Birincil ifadeler, en basit ifade biçimlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="2886a-882">Primary expressions include the simplest forms of expressions.</span></span>

```antlr
primary_expression
    : primary_no_array_creation_expression
    | array_creation_expression
    ;

primary_no_array_creation_expression
    : literal
    | interpolated_string_expression
    | simple_name
    | parenthesized_expression
    | member_access
    | invocation_expression
    | element_access
    | this_access
    | base_access
    | post_increment_expression
    | post_decrement_expression
    | object_creation_expression
    | delegate_creation_expression
    | anonymous_object_creation_expression
    | typeof_expression
    | checked_expression
    | unchecked_expression
    | default_value_expression
    | nameof_expression
    | anonymous_method_expression
    | primary_no_array_creation_expression_unsafe
    ;
```

<span data-ttu-id="2886a-883">Birincil ifadeler *array_creation_expression*s ve *primary_no_array_creation_expression*s arasında bölünür.</span><span class="sxs-lookup"><span data-stu-id="2886a-883">Primary expressions are divided between *array_creation_expression*s and *primary_no_array_creation_expression*s.</span></span> <span data-ttu-id="2886a-884">Dizi oluşturma-ifadesini diğer basit ifade formlarıyla birlikte listelemek yerine bu şekilde düşünerek, dilbilgisinde,</span><span class="sxs-lookup"><span data-stu-id="2886a-884">Treating array-creation-expression in this way, rather than listing it along with the other simple expression forms, enables the grammar to disallow potentially confusing code such as</span></span>
```csharp
object o = new int[3][1];
```
<span data-ttu-id="2886a-885">Aksi takdirde</span><span class="sxs-lookup"><span data-stu-id="2886a-885">which would otherwise be interpreted as</span></span>
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a><span data-ttu-id="2886a-886">Sabit değerler</span><span class="sxs-lookup"><span data-stu-id="2886a-886">Literals</span></span>

<span data-ttu-id="2886a-887">*Değişmez* değer ([sabit değerler](lexical-structure.md#literals)) içeren bir *primary_expression* değer olarak sınıflandırıldı.</span><span class="sxs-lookup"><span data-stu-id="2886a-887">A *primary_expression* that consists of a *literal* ([Literals](lexical-structure.md#literals)) is classified as a value.</span></span>


### <a name="interpolated-strings"></a><span data-ttu-id="2886a-888">Ara değerli dizeler</span><span class="sxs-lookup"><span data-stu-id="2886a-888">Interpolated strings</span></span>

<span data-ttu-id="2886a-889">*İnterpolated_string_expression* , bir `$` işaretinden sonra, `{` ve `}`ile ayrılmış, ifadeler ve biçimlendirme belirtimlerini içeren boşluklar ve düz bir dize değişmez değeri oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-889">An *interpolated_string_expression* consists of a `$` sign followed by a regular or verbatim string literal, wherein holes, delimited by `{` and `}`, enclose expressions and formatting specifications.</span></span> <span data-ttu-id="2886a-890">Enterpolasyonlu dize ifadesi, [enterpolasyonlu dize sabit değerleri](lexical-structure.md#interpolated-string-literals)içinde açıklandığı gibi ayrı belirteçlere bölünmüş bir *interpolated_string_literal* sonucudur.</span><span class="sxs-lookup"><span data-stu-id="2886a-890">An interpolated string expression is the result of an *interpolated_string_literal* that has been broken up into individual tokens, as described in [Interpolated string literals](lexical-structure.md#interpolated-string-literals).</span></span>

```antlr
interpolated_string_expression
    : '$' interpolated_regular_string
    | '$' interpolated_verbatim_string
    ;

interpolated_regular_string
    : interpolated_regular_string_whole
    | interpolated_regular_string_start interpolated_regular_string_body interpolated_regular_string_end
    ;

interpolated_regular_string_body
    : interpolation (interpolated_regular_string_mid interpolation)*
    ;

interpolation
    : expression
    | expression ',' constant_expression
    ;

interpolated_verbatim_string
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_body
    : interpolation (interpolated_verbatim_string_mid interpolation)+
    ;
```

<span data-ttu-id="2886a-891">Bir ilişkilendirmedeki *constant_expression* `int`örtülü dönüştürmeye sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-891">The *constant_expression* in an interpolation must have an implicit conversion to `int`.</span></span>

<span data-ttu-id="2886a-892">Bir *interpolated_string_expression* değer olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-892">An *interpolated_string_expression* is classified as a value.</span></span> <span data-ttu-id="2886a-893">Örtülü olarak enterpolasyonlu bir dize dönüştürmesi ([örtük olarak enterpolasyonlu dize dönüştürmeleri](conversions.md#implicit-interpolated-string-conversions)) ile `System.IFormattable` veya `System.FormattableString` olarak dönüştürülürse, enterpolasyonlu dize ifadesinde bu tür</span><span class="sxs-lookup"><span data-stu-id="2886a-893">If it is immediately converted to `System.IFormattable` or `System.FormattableString` with an implicit interpolated string conversion ([Implicit interpolated string conversions](conversions.md#implicit-interpolated-string-conversions)), the interpolated string expression has that type.</span></span> <span data-ttu-id="2886a-894">Aksi takdirde, türü `string`vardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-894">Otherwise, it has the type `string`.</span></span>

<span data-ttu-id="2886a-895">Enterpolasyonlu bir dizenin türü `System.IFormattable` veya `System.FormattableString`, anlamı bir `System.Runtime.CompilerServices.FormattableStringFactory.Create`çağrıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-895">If the type of an interpolated string is `System.IFormattable` or `System.FormattableString`, the meaning is a call to `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span></span> <span data-ttu-id="2886a-896">Tür `string`ise, ifadenin anlamı `string.Format`bir çağrıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-896">If the type is `string`, the meaning of the expression is a call to `string.Format`.</span></span> <span data-ttu-id="2886a-897">Her iki durumda da, çağrının bağımsız değişken listesi, her ilişkilendirme için yer tutucular içeren bir biçim dizesi değişmez değerinden ve yer sahiplerine karşılık gelen her bir ifade için bir bağımsız değişkenle oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-897">In both cases, the argument list of the call consists of a format string literal with placeholders for each interpolation, and an argument for each expression corresponding to the place holders.</span></span>

<span data-ttu-id="2886a-898">Biçim dizesi değişmez değeri aşağıdaki gibi oluşturulur; burada `N`, *interpolated_string_expression*içindeki enterpolasyonun sayısıdır:</span><span class="sxs-lookup"><span data-stu-id="2886a-898">The format string literal is constructed as follows, where `N` is the number of interpolations in the *interpolated_string_expression*:</span></span>

*  <span data-ttu-id="2886a-899">Bir *interpolated_regular_string_whole* veya *interpolated_verbatim_string_whole* `$` işaretini izliyorsa, biçim dizesi değişmez değeri bu belirteç olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-899">If an *interpolated_regular_string_whole* or an *interpolated_verbatim_string_whole* follows the `$` sign, then the format string literal is that token.</span></span>
*  <span data-ttu-id="2886a-900">Aksi takdirde, biçim dizesi değişmez değeri şunlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="2886a-900">Otherwise, the format string literal consists of:</span></span> 
   *  <span data-ttu-id="2886a-901">İlk *interpolated_regular_string_start* veya *interpolated_verbatim_string_start*</span><span class="sxs-lookup"><span data-stu-id="2886a-901">First the *interpolated_regular_string_start* or *interpolated_verbatim_string_start*</span></span>
   *  <span data-ttu-id="2886a-902">Sonra `0` `I` her numara için `N-1`:</span><span class="sxs-lookup"><span data-stu-id="2886a-902">Then for each number `I` from `0` to `N-1`:</span></span> 
      * <span data-ttu-id="2886a-903">`I` ondalık temsili</span><span class="sxs-lookup"><span data-stu-id="2886a-903">The decimal representation of `I`</span></span>
      * <span data-ttu-id="2886a-904">Ardından, karşılık gelen *enterpolasyon* bir *constant_expression*içeriyorsa, bir `,` (virgül) ve ardından *constant_expression* değerinin ondalık temsili gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-904">Then, if the corresponding *interpolation* has a *constant_expression*, a `,` (comma) followed by the decimal representation of the value of the *constant_expression*</span></span>
      * <span data-ttu-id="2886a-905">*İnterpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* veya *interpolated_verbatim_string_end* karşılık gelen ilişkilendirmeyi hemen takip edin.</span><span class="sxs-lookup"><span data-stu-id="2886a-905">Then the *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* or *interpolated_verbatim_string_end* immediately following the corresponding interpolation.</span></span>

<span data-ttu-id="2886a-906">Sonraki bağımsız değişkenler, sırasıyla *enterpolasyonların* (varsa) *ifadeleridir* .</span><span class="sxs-lookup"><span data-stu-id="2886a-906">The subsequent arguments are simply the *expressions* from the *interpolations* (if any), in order.</span></span>

<span data-ttu-id="2886a-907">TODO: örnekler.</span><span class="sxs-lookup"><span data-stu-id="2886a-907">TODO: examples.</span></span>


### <a name="simple-names"></a><span data-ttu-id="2886a-908">Basit adlar</span><span class="sxs-lookup"><span data-stu-id="2886a-908">Simple names</span></span>

<span data-ttu-id="2886a-909">*Simple_name* bir tanımlayıcıdan, isteğe bağlı olarak bir tür bağımsız değişken listesi tarafından oluşur:</span><span class="sxs-lookup"><span data-stu-id="2886a-909">A *simple_name* consists of an identifier, optionally followed by a type argument list:</span></span>

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

<span data-ttu-id="2886a-910">*Simple_name* , `I` tek bir tanımlayıcı olduğu ve `<A1,...,Ak>` isteğe bağlı bir *type_argument_list*olduğu `I<A1,...,Ak>`form `I` veya form olabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-910">A *simple_name* is either of the form `I` or of the form `I<A1,...,Ak>`, where `I` is a single identifier and `<A1,...,Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="2886a-911">*Type_argument_list* belirtilmediğinde, sıfır `K` göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="2886a-911">When no *type_argument_list* is specified, consider `K` to be zero.</span></span> <span data-ttu-id="2886a-912">*Simple_name* değerlendirilir ve aşağıdaki gibi sınıflandırılacaktır:</span><span class="sxs-lookup"><span data-stu-id="2886a-912">The *simple_name* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="2886a-913">`K` sıfırsa ve *simple_name* bir *blok* içinde görünürse ve *blok*(veya kapsayan *bloğun*) yerel değişken bildirim alanı ([Bildirimler](basic-concepts.md#declarations)), ad `I`olan bir yerel değişken, parametre veya sabit içeriyorsa, *simple_name* bu yerel değişkene, parametreye veya sabitine başvurur ve bir değişken veya değer olarak sınıflandırılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="2886a-913">If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>
*  <span data-ttu-id="2886a-914">`K` sıfırsa ve *simple_name* bir genel yöntem bildiriminin gövdesinde yer alıyorsa ve bu bildirimde `I`adında bir tür parametresi varsa, *simple_name* bu tür parametresine başvurur.</span><span class="sxs-lookup"><span data-stu-id="2886a-914">If `K` is zero and the *simple_name* appears within the body of a generic method declaration and if that declaration includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
*  <span data-ttu-id="2886a-915">Aksi halde, her bir örnek türü için `T` ([örnek türü](classes.md#the-instance-type)), hemen kapsayan tür bildiriminin örnek türüyle başlar ve her bir kapsayan sınıfın ya da yapı bildiriminin örnek türüne (varsa) devam edin:</span><span class="sxs-lookup"><span data-stu-id="2886a-915">Otherwise, for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of the immediately enclosing type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
   *  <span data-ttu-id="2886a-916">`K` sıfırsa ve `T` bildirimi, ad `I`olan bir tür parametresi içeriyorsa, *simple_name* bu tür parametresine başvurur.</span><span class="sxs-lookup"><span data-stu-id="2886a-916">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
   *  <span data-ttu-id="2886a-917">Aksi halde, `K` tür bağımsız değişkenleriyle `T` `I` üye araması ([üye araması](expressions.md#member-lookup)) bir eşleşme üretir:</span><span class="sxs-lookup"><span data-stu-id="2886a-917">Otherwise, if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match:</span></span>
      * <span data-ttu-id="2886a-918">`T`, hemen kapsayan sınıfın veya yapı türünün örnek türü ise ve arama bir veya daha fazla yöntemi tanımlarsa, sonuç, ilişkili bir örnek ifadesi `this`olan bir yöntem grubudur.</span><span class="sxs-lookup"><span data-stu-id="2886a-918">If `T` is the instance type of the immediately enclosing class or struct type and the lookup identifies one or more methods, the result is a method group with an associated instance expression of `this`.</span></span> <span data-ttu-id="2886a-919">Bir tür bağımsız değişken listesi belirtilmişse, genel bir yöntemi ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)) çağırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-919">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
      * <span data-ttu-id="2886a-920">Aksi takdirde `T`, arama bir örnek üye tanımlarsa ve başvuru bir örnek oluşturucusunun, örnek yönteminin veya örnek erişimcisinin gövdesinde gerçekleşirse, sonuç, form `this.I`bir üye erişimi ([üye erişimi](expressions.md#member-access)) ile aynıdır ve bu, hemen kapsayan sınıf veya yapı türünün örnek türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-920">Otherwise, if `T` is the instance type of the immediately enclosing class or struct type, if the lookup identifies an instance member, and if the reference occurs within the body of an instance constructor, an instance method, or an instance accessor, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `this.I`.</span></span> <span data-ttu-id="2886a-921">Bu yalnızca `K` sıfır olduğunda meydana gelebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-921">This can only happen when `K` is zero.</span></span>
      * <span data-ttu-id="2886a-922">Aksi halde, sonuç, `T.I` veya `T.I<A1,...,Ak>`bir üye erişimi ([üye erişimi](expressions.md#member-access)) ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-922">Otherwise, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `T.I` or `T.I<A1,...,Ak>`.</span></span> <span data-ttu-id="2886a-923">Bu durumda, *simple_name* bir örnek üyesine başvurması için bağlama zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-923">In this case, it is a binding-time error for the *simple_name* to refer to an instance member.</span></span>

*  <span data-ttu-id="2886a-924">Aksi takdirde `N`her bir ad alanı için *simple_name* gerçekleştiği ad alanından başlayarak, kapsayan her ad alanıyla (varsa) devam eder ve genel ad alanıyla sona eriyor, aşağıdaki adımlar bir varlık bulunana kadar değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-924">Otherwise, for each namespace `N`, starting with the namespace in which the *simple_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
   *  <span data-ttu-id="2886a-925">`K` sıfırsa ve `I` `N`bir ad alanının adı ise:</span><span class="sxs-lookup"><span data-stu-id="2886a-925">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
      * <span data-ttu-id="2886a-926">*Simple_name* gerçekleştiği konum `N` için bir ad alanı bildirimiyle Çevrese ve ad alanı bildirimi, adı `I` ad alanı veya türle ilişkilendiren bir *extern_alias_directive* veya *using_alias_directive* içeriyorsa, *simple_name* belirsizdir ve derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-926">If the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="2886a-927">Aksi takdirde *simple_name* , `N``I` adlı ad alanı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-927">Otherwise, the *simple_name* refers to the namespace named `I` in `N`.</span></span>
   *  <span data-ttu-id="2886a-928">Aksi takdirde, `N` ad `I` ve `K` tür parametrelerine sahip erişilebilir bir tür içeriyorsa:</span><span class="sxs-lookup"><span data-stu-id="2886a-928">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
      * <span data-ttu-id="2886a-929">`K` sıfırsa ve *simple_name* gerçekleştiği konum `N` için bir ad alanı bildirimiyle Çevrese ve ad alanı bildirimi ad using_alias_directive adını bir ad alanı veya türle ilişkilendiren bir *extern_alias_directive* veya * `I`* içeriyorsa, *simple_name* belirsizdir ve bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-929">If `K` is zero and the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="2886a-930">Aksi takdirde *namespace_or_type_name* , verilen tür bağımsız değişkenleriyle oluşturulan türe başvurur.</span><span class="sxs-lookup"><span data-stu-id="2886a-930">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="2886a-931">Aksi takdirde, *simple_name* gerçekleştiği konum `N`için bir ad alanı bildirimiyle çevrelenmiş ise:</span><span class="sxs-lookup"><span data-stu-id="2886a-931">Otherwise, if the location where the *simple_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
      * <span data-ttu-id="2886a-932">`K` sıfırsa ve ad alanı bildirimi, adı `I` içeri aktarılan bir ad alanı veya türle ilişkilendirir *extern_alias_directive* veya *using_alias_directive* içeriyorsa, *simple_name* bu ad alanına veya türe başvurur.</span><span class="sxs-lookup"><span data-stu-id="2886a-932">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *simple_name* refers to that namespace or type.</span></span>
      * <span data-ttu-id="2886a-933">Aksi takdirde, *using_namespace_directive*s ve ad alanı bildiriminin *using_static_directive*tarafından içeri aktarılan ad alanları ve tür bildirimleri, ad `I` ve `K` tür parametrelerine sahip tek bir erişilebilir tür veya uzantı olmayan statik üye içeriyorsa, *simple_name* bu türü veya verilen tür bağımsız değişkenleriyle oluşturulan üyeyi ifade eder.</span><span class="sxs-lookup"><span data-stu-id="2886a-933">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_static_directive*s of the namespace declaration contain exactly one accessible type or non-extension static member having name `I` and `K` type parameters, then the *simple_name* refers to that type or member constructed with the given type arguments.</span></span>
      * <span data-ttu-id="2886a-934">Aksi takdirde, ad alanı bildiriminin *using_namespace_directive*tarafından içeri aktarılan ad alanları ve türler, ad `I` ve `K` tür parametrelerine sahip uzantı dışı bir statik üye içeriyorsa, *simple_name* belirsizdir ve bir hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-934">Otherwise, if the namespaces and types imported by the *using_namespace_directive*s of the namespace declaration contain more than one accessible type or non-extension-method static member having name `I` and `K` type parameters, then the *simple_name* is ambiguous and an error occurs.</span></span>

   <span data-ttu-id="2886a-935">Bu adımın tamamı bir *namespace_or_type_name* ([ad alanı ve tür adları](basic-concepts.md#namespace-and-type-names)) işlenirken ilgili adıma tam olarak paralel olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2886a-935">Note that this entire step is exactly parallel to the corresponding step in the processing of a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span>

*  <span data-ttu-id="2886a-936">Aksi takdirde, *simple_name* tanımsız olur ve derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-936">Otherwise, the *simple_name* is undefined and a compile-time error occurs.</span></span>


### <a name="parenthesized-expressions"></a><span data-ttu-id="2886a-937">Parantez içinde ifadeler</span><span class="sxs-lookup"><span data-stu-id="2886a-937">Parenthesized expressions</span></span>

<span data-ttu-id="2886a-938">*Parenthesized_expression* parantez içine alınmış bir *ifadeden* oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-938">A *parenthesized_expression* consists of an *expression* enclosed in parentheses.</span></span>

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

<span data-ttu-id="2886a-939">Parantez içindeki *ifade* hesaplanarak bir *parenthesized_expression* değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-939">A *parenthesized_expression* is evaluated by evaluating the *expression* within the parentheses.</span></span> <span data-ttu-id="2886a-940">Parantez içindeki *ifade* bir ad alanı veya tür alıyorsa, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-940">If the *expression* within the parentheses denotes a namespace or type, a compile-time error occurs.</span></span> <span data-ttu-id="2886a-941">Aksi takdirde, *parenthesized_expression* sonucu içerilen *ifadenin*değerlendirmesinin sonucudur.</span><span class="sxs-lookup"><span data-stu-id="2886a-941">Otherwise, the result of the *parenthesized_expression* is the result of the evaluation of the contained *expression*.</span></span>

### <a name="member-access"></a><span data-ttu-id="2886a-942">Üye erişimi</span><span class="sxs-lookup"><span data-stu-id="2886a-942">Member access</span></span>

<span data-ttu-id="2886a-943">Bir *member_access* *primary_expression*, *predefined_type*ya da *qualified_alias_member*, ardından bir "`.`" belirteci ve ardından bir *tanımlayıcı*tarafından ve isteğe bağlı olarak, bir *type_argument_list*tarafından oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-943">A *member_access* consists of a *primary_expression*, a *predefined_type*, or a *qualified_alias_member*, followed by a "`.`" token, followed by an *identifier*, optionally followed by a *type_argument_list*.</span></span>

```antlr
member_access
    : primary_expression '.' identifier type_argument_list?
    | predefined_type '.' identifier type_argument_list?
    | qualified_alias_member '.' identifier
    ;

predefined_type
    : 'bool'   | 'byte'  | 'char'  | 'decimal' | 'double' | 'float' | 'int' | 'long'
    | 'object' | 'sbyte' | 'short' | 'string'  | 'uint'   | 'ulong' | 'ushort'
    ;
```

<span data-ttu-id="2886a-944">*Qualified_alias_member* üretim, [ad alanı diğer adı niteleyicilerine](namespaces.md#namespace-alias-qualifiers)göre tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-944">The *qualified_alias_member* production is defined in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span>

<span data-ttu-id="2886a-945">*Member_access* , `E` bir birincil ifade olduğu, `I` tek bir tanımlayıcı ve `<A1, ..., Ak>` isteğe bağlı bir *type_argument_list*olduğu `E.I<A1, ..., Ak>`form `E.I` veya form olabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-945">A *member_access* is either of the form `E.I` or of the form `E.I<A1, ..., Ak>`, where `E` is a primary-expression, `I` is a single identifier and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="2886a-946">*Type_argument_list* belirtilmediğinde, sıfır `K` göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="2886a-946">When no *type_argument_list* is specified, consider `K` to be zero.</span></span>

<span data-ttu-id="2886a-947">`dynamic` türünde bir *primary_expression* olan *member_access* dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="2886a-947">A *member_access* with a *primary_expression* of type `dynamic` is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="2886a-948">Bu durumda, derleyici üye erişimini `dynamic`türünde bir özellik erişimi olarak sınıflandırır.</span><span class="sxs-lookup"><span data-stu-id="2886a-948">In this case the compiler classifies the member access as a property access of type `dynamic`.</span></span> <span data-ttu-id="2886a-949">*Member_access* anlamını belirlemede aşağıdaki kurallar, *primary_expression*derleme zamanı türü yerine çalışma zamanı türü kullanılarak çalışma zamanında uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-949">The rules below to determine the meaning of the *member_access* are then applied at run-time, using the run-time type instead of the compile-time type of the *primary_expression*.</span></span> <span data-ttu-id="2886a-950">Bu çalışma zamanı sınıflandırması bir yöntem grubuna yol açar, üye erişimi bir *invocation_expression* *primary_expression* olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-950">If this run-time classification leads to a method group, then the member access must be the *primary_expression* of an *invocation_expression*.</span></span>

<span data-ttu-id="2886a-951">*Member_access* değerlendirilir ve aşağıdaki gibi sınıflandırılacaktır:</span><span class="sxs-lookup"><span data-stu-id="2886a-951">The *member_access* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="2886a-952">`K` sıfırsa ve `E` bir ad alanı ve `E` `I`ada sahip bir iç içe ad alanı içeriyorsa, sonuç bu ad alanıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-952">If `K` is zero and `E` is a namespace and `E` contains a nested namespace with name `I`, then the result is that namespace.</span></span>
*  <span data-ttu-id="2886a-953">Aksi takdirde, `E` bir ad alanı ise ve `E` ad `I` ve `K` tür parametrelerine sahip erişilebilir bir tür içeriyorsa, sonuç verilen tür bağımsız değişkenleriyle oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-953">Otherwise, if `E` is a namespace and `E` contains an accessible type having name `I` and `K` type parameters, then the result is that type constructed with the given type arguments.</span></span>
*  <span data-ttu-id="2886a-954">`E` bir *predefined_type* ya da tür olarak sınıflandırılan bir *primary_expression* ise, `E` bir tür parametresi değilse ve `I` `E` tür parametrelerine sahip `K`bir üye araması ([üye arama](expressions.md#member-lookup)) bir eşleşme üretirse,  değerlendirilir ve aşağıdaki gibi sınıflandırılacaktır:</span><span class="sxs-lookup"><span data-stu-id="2886a-954">If `E` is a *predefined_type* or a *primary_expression* classified as a type, if `E` is not a type parameter, and if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `E` with `K` type parameters produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="2886a-955">`I`, bir türü tanımlarsa, bu tür belirtilen tür bağımsız değişkenleriyle oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-955">If `I` identifies a type, then the result is that type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="2886a-956">`I` bir veya daha fazla yöntemi tanımlarsa, sonuç ilişkili örnek ifadesi olmayan bir yöntem grubudur.</span><span class="sxs-lookup"><span data-stu-id="2886a-956">If `I` identifies one or more methods, then the result is a method group with no associated instance expression.</span></span> <span data-ttu-id="2886a-957">Bir tür bağımsız değişken listesi belirtilmişse, genel bir yöntemi ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)) çağırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-957">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="2886a-958">`I` bir `static` özelliğini tanımlarsa, sonuç, ilişkili örnek ifadesi olmayan bir özellik erişimiydi.</span><span class="sxs-lookup"><span data-stu-id="2886a-958">If `I` identifies a `static` property, then the result is a property access with no associated instance expression.</span></span>
   *  <span data-ttu-id="2886a-959">`I` bir `static` alanı tanımlarsa:</span><span class="sxs-lookup"><span data-stu-id="2886a-959">If `I` identifies a `static` field:</span></span>
      * <span data-ttu-id="2886a-960">Alan `readonly` ve başvuru, alanın bildirildiği sınıf veya yapının statik oluşturucusunun dışında oluşursa, sonuç bir değerdir, yani statik alanın değeri `E` `I`.</span><span class="sxs-lookup"><span data-stu-id="2886a-960">If the field is `readonly` and the reference occurs outside the static constructor of the class or struct in which the field is declared, then the result is a value, namely the value of the static field `I` in `E`.</span></span>
      * <span data-ttu-id="2886a-961">Aksi takdirde, sonuç bir değişkendir, yani statik alan `E` `I`.</span><span class="sxs-lookup"><span data-stu-id="2886a-961">Otherwise, the result is a variable, namely the static field `I` in `E`.</span></span>
   *  <span data-ttu-id="2886a-962">`I` bir `static` olayı tanımlarsa:</span><span class="sxs-lookup"><span data-stu-id="2886a-962">If `I` identifies a `static` event:</span></span>
      * <span data-ttu-id="2886a-963">Başvuru, olayın bildirildiği sınıf veya yapı içinde oluşursa ve olay *event_accessor_declarations* ([Olaylar](classes.md#events)) olmadan bildirilirse, `E.I` tam olarak `I` statik bir alan gibi işlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-963">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), then `E.I` is processed exactly as if `I` were a static field.</span></span>
      * <span data-ttu-id="2886a-964">Aksi halde, sonuç, ilişkili örnek ifadesi olmayan bir olay erişimiydi.</span><span class="sxs-lookup"><span data-stu-id="2886a-964">Otherwise, the result is an event access with no associated instance expression.</span></span>
   *  <span data-ttu-id="2886a-965">`I` bir sabiti tanımlarsa, sonuç bir değerdir, yani bu sabit değeridir.</span><span class="sxs-lookup"><span data-stu-id="2886a-965">If `I` identifies a constant, then the result is a value, namely the value of that constant.</span></span>
    * <span data-ttu-id="2886a-966">`I` bir numaralandırma üyesini tanımlarsa, sonuç bir değerdir, yani bu numaralandırma üyesinin değeri.</span><span class="sxs-lookup"><span data-stu-id="2886a-966">If `I` identifies an enumeration member, then the result is a value, namely the value of that enumeration member.</span></span>
    * <span data-ttu-id="2886a-967">Aksi takdirde, `E.I` geçersiz bir üye başvurusu ve derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-967">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="2886a-968">`E` bir özellik erişimi, Dizin Oluşturucu erişimi, değişken veya değer, `T`türü ve `K` türü bağımsız değişkenlerle `T` `I` üye araması ([üye arama](expressions.md#member-lookup)) bir eşleşme üretir, `E.I` değerlendirilir ve aşağıdaki gibi sınıflandırılacaktır:</span><span class="sxs-lookup"><span data-stu-id="2886a-968">If `E` is a property access, indexer access, variable, or value, the type of which is `T`, and a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="2886a-969">İlk olarak, `E` bir özellik veya Dizin Oluşturucu erişimi ise, özellik veya Dizin Oluşturucu erişimi değeri elde edilir ([Ifadelerin değerleri](expressions.md#values-of-expressions)) ve `E` bir değer olarak yeniden sınıflanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-969">First, if `E` is a property or indexer access, then the value of the property or indexer access is obtained ([Values of expressions](expressions.md#values-of-expressions)) and `E` is reclassified as a value.</span></span>
   *  <span data-ttu-id="2886a-970">`I` bir veya daha fazla yöntemi tanımlarsa, sonuç `E`ilişkili bir örnek ifadesi olan bir yöntem grubudur.</span><span class="sxs-lookup"><span data-stu-id="2886a-970">If `I` identifies one or more methods, then the result is a method group with an associated instance expression of `E`.</span></span> <span data-ttu-id="2886a-971">Bir tür bağımsız değişken listesi belirtilmişse, genel bir yöntemi ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)) çağırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-971">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="2886a-972">`I` bir örnek özelliğini tanımlarsa,</span><span class="sxs-lookup"><span data-stu-id="2886a-972">If `I` identifies an instance property,</span></span>
      * <span data-ttu-id="2886a-973">`E` `this`, `I` otomatik olarak uygulanan bir özelliği ([otomatik olarak uygulanan özellikler](classes.md#automatically-implemented-properties)) bir ayarlayıcı olmadan tanımlar ve başvuru, bir sınıf veya yapı `T`türü için örnek oluşturucu içinde meydana gelir. Bu durumda, sonuç bir değişkendir, yani `I` tarafından verilen `T` örneğindeki `this`tarafından verilen otomatik özellik için gizli yedekleme alanı.</span><span class="sxs-lookup"><span data-stu-id="2886a-973">If `E` is `this`, `I` identifies an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)) without a setter, and the reference occurs within an instance constructor for a class or struct type `T`, then the result is a variable, namely the hidden backing field for the auto-property given by `I` in the instance of `T` given by `this`.</span></span>
      * <span data-ttu-id="2886a-974">Aksi takdirde sonuç, `E`ilişkili bir örnek ifadesiyle bir özellik erişimiydi.</span><span class="sxs-lookup"><span data-stu-id="2886a-974">Otherwise, the result is a property access with an associated instance expression of `E`.</span></span>
   *  <span data-ttu-id="2886a-975">`T` bir *class_type* ve bu *class_type*bir örnek alanı `I` tanımlar:</span><span class="sxs-lookup"><span data-stu-id="2886a-975">If `T` is a *class_type* and `I` identifies an instance field of that *class_type*:</span></span>
      * <span data-ttu-id="2886a-976">`E` değeri `null`, bir `System.NullReferenceException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-976">If the value of `E` is `null`, then a `System.NullReferenceException` is thrown.</span></span>
      * <span data-ttu-id="2886a-977">Aksi halde, alan `readonly` ve başvuru, alanın bildirildiği sınıfın örnek oluşturucusunun dışında oluşursa, sonuç bir değerdir, yani alanın değeri `E`tarafından başvurulan nesnede `I`.</span><span class="sxs-lookup"><span data-stu-id="2886a-977">Otherwise, if the field is `readonly` and the reference occurs outside an instance constructor of the class in which the field is declared, then the result is a value, namely the value of the field `I` in the object referenced by `E`.</span></span>
      * <span data-ttu-id="2886a-978">Aksi takdirde, sonuç bir değişkendir, ancak alan `E`tarafından başvurulan nesnedeki `I`.</span><span class="sxs-lookup"><span data-stu-id="2886a-978">Otherwise, the result is a variable, namely the field `I` in the object referenced by `E`.</span></span>
   *  <span data-ttu-id="2886a-979">`T` bir *struct_type* ve bu *struct_type*bir örnek alanı `I` tanımlar:</span><span class="sxs-lookup"><span data-stu-id="2886a-979">If `T` is a *struct_type* and `I` identifies an instance field of that *struct_type*:</span></span>
      * <span data-ttu-id="2886a-980">`E` bir değer ise veya alan `readonly` ve başvuru, alanın bildirildiği yapının örnek oluşturucusunun dışında oluşursa, sonuç bir değerdir, yani alanın değeri, `E`tarafından verilen yapı örneğinde `I`.</span><span class="sxs-lookup"><span data-stu-id="2886a-980">If `E` is a value, or if the field is `readonly` and the reference occurs outside an instance constructor of the struct in which the field is declared, then the result is a value, namely the value of the field `I` in the struct instance given by `E`.</span></span>
      * <span data-ttu-id="2886a-981">Aksi takdirde, sonuç bir değişkendir, yani alan `E`tarafından verilen yapı örneğinde `I`.</span><span class="sxs-lookup"><span data-stu-id="2886a-981">Otherwise, the result is a variable, namely the field `I` in the struct instance given by `E`.</span></span>
   *  <span data-ttu-id="2886a-982">`I` bir örnek olayı tanımlarsa:</span><span class="sxs-lookup"><span data-stu-id="2886a-982">If `I` identifies an instance event:</span></span>
      * <span data-ttu-id="2886a-983">Başvuru, olayın bildirildiği sınıf veya yapı içinde oluşursa ve olay *event_accessor_declarations* ([Olaylar](classes.md#events)) olmadan bildirildiği ve başvuru bir `+=` veya `-=` işlecinin sol tarafında gerçekleşmemişse, `E.I` tam `I` olarak bir örnek alanı olduğu gibi işlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-983">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), and the reference does not occur as the left-hand side of a `+=` or `-=` operator, then `E.I` is processed exactly as if `I` was an instance field.</span></span>
      * <span data-ttu-id="2886a-984">Aksi takdirde, sonuç `E`ilişkili bir örnek ifadesiyle bir olay erişimiydi.</span><span class="sxs-lookup"><span data-stu-id="2886a-984">Otherwise, the result is an event access with an associated instance expression of `E`.</span></span>
*  <span data-ttu-id="2886a-985">Aksi takdirde, bir uzantı yöntemi çağırma ([uzantı yöntemi etkinleştirmeleri](expressions.md#extension-method-invocations)) olarak `E.I` işlemek için bir girişimde bulunuldu.</span><span class="sxs-lookup"><span data-stu-id="2886a-985">Otherwise, an attempt is made to process `E.I` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="2886a-986">Bu başarısız olursa, `E.I` geçersiz bir üye başvurusu ve bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-986">If this fails, `E.I` is an invalid member reference, and a binding-time error occurs.</span></span>

#### <a name="identical-simple-names-and-type-names"></a><span data-ttu-id="2886a-987">Aynı basit adlar ve tür adları</span><span class="sxs-lookup"><span data-stu-id="2886a-987">Identical simple names and type names</span></span>

<span data-ttu-id="2886a-988">Formun üye erişiminin `E.I`, `E` tek bir tanımlayıcıdır ve *simple_name* ([basit adlar](expressions.md#simple-names)) olarak `E` anlamı bir sabit, alan, özellik, yerel değişken veya bir *`E`* ([ad alanı ve tür adları](basic-concepts.md#namespace-and-type-names)) olarak type_name anlamı ile aynı türde bir parametre ise, her ikisi de `E` olası anlamlarıyla izin verilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-988">In a member access of the form `E.I`, if `E` is a single identifier, and if the meaning of `E` as a *simple_name* ([Simple names](expressions.md#simple-names)) is a constant, field, property, local variable, or parameter with the same type as the meaning of `E` as a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)), then both possible meanings of `E` are permitted.</span></span> <span data-ttu-id="2886a-989">`I` her iki durumda da `E` türünde bir üye olması gerektiğinden `E.I` iki olası anlamı hiçbir zaman belirsiz değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-989">The two possible meanings of `E.I` are never ambiguous, since `I` must necessarily be a member of the type `E` in both cases.</span></span> <span data-ttu-id="2886a-990">Diğer bir deyişle, kural yalnızca statik üyelere ve derleme zamanı hatası oluşması durumunda `E` iç içe geçmiş türlerine erişime izin verir.</span><span class="sxs-lookup"><span data-stu-id="2886a-990">In other words, the rule simply permits access to the static members and nested types of `E` where a compile-time error would otherwise have occurred.</span></span> <span data-ttu-id="2886a-991">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2886a-991">For example:</span></span>
```csharp
struct Color
{
    public static readonly Color White = new Color(...);
    public static readonly Color Black = new Color(...);

    public Color Complement() {...}
}

class A
{
    public Color Color;                // Field Color of type Color

    void F() {
        Color = Color.Black;           // References Color.Black static member
        Color = Color.Complement();    // Invokes Complement() on Color field
    }

    static void G() {
        Color c = Color.White;         // References Color.White static member
    }
}
```

#### <a name="grammar-ambiguities"></a><span data-ttu-id="2886a-992">Dilbilgisi belirsizlikleri</span><span class="sxs-lookup"><span data-stu-id="2886a-992">Grammar ambiguities</span></span>

<span data-ttu-id="2886a-993">*Simple_name* ([basit adlar](expressions.md#simple-names)) ve *member_access* ([üye erişimi](expressions.md#member-access)) üretimleri, ifadelerde dilbilgisi için belirsizlikleri verebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-993">The productions for *simple_name* ([Simple names](expressions.md#simple-names)) and *member_access* ([Member access](expressions.md#member-access)) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="2886a-994">Örneğin, ifade:</span><span class="sxs-lookup"><span data-stu-id="2886a-994">For example, the statement:</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="2886a-995">, `G < A` ve `B > (7)`iki bağımsız değişkenle `F` çağrısı olarak yorumlanamaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-995">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="2886a-996">Alternatif olarak, iki tür bağımsız değişkeni ve bir normal bağımsız değişkenle `G` genel bir yöntem çağrısı olan bir bağımsız değişkenle `F` çağrısı olarak yorumlanabilecek.</span><span class="sxs-lookup"><span data-stu-id="2886a-996">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

<span data-ttu-id="2886a-997">Bir belirteç dizisi bir *simple_name* ([basit adlar](expressions.md#simple-names)), *member_access* ([üye erişimi](expressions.md#member-access)) veya *pointer_member_access* ([işaretçi üyesi erişimi](unsafe-code.md#pointer-member-access)) ile biten bir *type_argument_list* ([tür bağımsız değişkenleri](types.md#type-arguments)) olarak ayrıştırılamıyorsa, kapatma `>` belirtecini izleyen belirteç incelenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-997">If a sequence of tokens can be parsed (in context) as a *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), or *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) ending with a *type_argument_list* ([Type arguments](types.md#type-arguments)), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="2886a-998">Bunlardan biri ise</span><span class="sxs-lookup"><span data-stu-id="2886a-998">If it is one of</span></span>
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
<span data-ttu-id="2886a-999">ardından *type_argument_list* *simple_name*bir parçası olarak tutulur, *member_access* veya *pointer_member_access* ve belirteçleri dizisinin diğer olası ayrıştırması atılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-999">then the *type_argument_list* is retained as part of the *simple_name*, *member_access* or *pointer_member_access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="2886a-1000">Aksi takdirde, belirteç dizisinin başka bir olası ayrıştırması olmasa bile type_argument_list *simple_name*, *member_access* veya *pointer_member_access*bir parçası olarak kabul edilmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1000">Otherwise, the *type_argument_list* is not considered to be part of the *simple_name*, *member_access* or *pointer_member_access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="2886a-1001">Bu kuralların bir *namespace_or_type_name* *type_argument_list* ayrıştırılırken ([ad alanı ve tür adları](basic-concepts.md#namespace-and-type-names)) uygulanmadığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2886a-1001">Note that these rules are not applied when parsing a *type_argument_list* in a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span> <span data-ttu-id="2886a-1002">İfade</span><span class="sxs-lookup"><span data-stu-id="2886a-1002">The statement</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="2886a-1003">Bu kurala göre, iki tür bağımsız değişkeni ve bir normal bağımsız değişkenle `G` genel bir yöntem çağrısı olan bir bağımsız değişkenle `F` çağrısı olarak yorumlanacaktır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1003">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="2886a-1004">Deyimler</span><span class="sxs-lookup"><span data-stu-id="2886a-1004">The statements</span></span>
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
<span data-ttu-id="2886a-1005">her biri, iki bağımsız değişkenle `F` çağrısı olarak yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1005">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="2886a-1006">İfade</span><span class="sxs-lookup"><span data-stu-id="2886a-1006">The statement</span></span>
```csharp
x = F < A > +y;
```
<span data-ttu-id="2886a-1007">, bir *type_argument_list* ve ardından ikili artı işleci gelen bir *simple_name* yerine, ' den daha az işleç, büyüktür operatörü ve birli artı `x = (F < A) > (+y)`işleci olarak yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1007">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple_name* with a *type_argument_list* followed by a binary plus operator.</span></span> <span data-ttu-id="2886a-1008">İfadesinde</span><span class="sxs-lookup"><span data-stu-id="2886a-1008">In the statement</span></span>
```csharp
x = y is C<T> + z;
```
<span data-ttu-id="2886a-1009">belirteçler `C<T>`, *type_argument_list*bir *namespace_or_type_name* olarak yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1009">the tokens `C<T>` are interpreted as a *namespace_or_type_name* with a *type_argument_list*.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="2886a-1010">Çağırma ifadeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-1010">Invocation expressions</span></span>

<span data-ttu-id="2886a-1011">Bir yöntemi çağırmak için bir *invocation_expression* kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1011">An *invocation_expression* is used to invoke a method.</span></span>

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

<span data-ttu-id="2886a-1012">Aşağıdakilerden en az biri tutuyorsa, bir *invocation_expression* dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)):</span><span class="sxs-lookup"><span data-stu-id="2886a-1012">An *invocation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="2886a-1013">*Primary_expression* `dynamic`derleme zamanı türü vardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1013">The *primary_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="2886a-1014">İsteğe bağlı *argument_list* en az bir bağımsız değişkeni derleme zamanı türü `dynamic` ve *primary_expression* bir temsilci türüne sahip değil.</span><span class="sxs-lookup"><span data-stu-id="2886a-1014">At least one argument of the optional *argument_list* has compile-time type `dynamic` and the *primary_expression* does not have a delegate type.</span></span>

<span data-ttu-id="2886a-1015">Bu durumda, derleyici *invocation_expression* `dynamic`türünde bir değer olarak sınıflandırır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1015">In this case the compiler classifies the *invocation_expression* as a value of type `dynamic`.</span></span> <span data-ttu-id="2886a-1016">*İnvocation_expression* anlamını öğrenmek için aşağıdaki kurallar, *primary_expression* ve derleme zamanı türü `dynamic`olan bağımsız değişkenlerin derleme zamanı türü yerine çalışma zamanı türü kullanılarak çalışma zamanında uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1016">The rules below to determine the meaning of the *invocation_expression* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_expression* and arguments which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="2886a-1017">*Primary_expression* , derleme zamanı türü `dynamic`yoksa, bu durumda çağrı, [dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)konusunda açıklandığı gibi sınırlı bir derleme süresi denetimi yapar.</span><span class="sxs-lookup"><span data-stu-id="2886a-1017">If the *primary_expression* does not have compile-time type `dynamic`, then the method invocation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="2886a-1018">Bir *invocation_expression* *primary_expression* bir yöntem grubu ya da bir *delegate_type*değeri olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1018">The *primary_expression* of an *invocation_expression* must be a method group or a value of a *delegate_type*.</span></span> <span data-ttu-id="2886a-1019">*Primary_expression* bir yöntem grubu ise, *invocation_expression* bir yöntem çağrıdır ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="2886a-1019">If the *primary_expression* is a method group, the *invocation_expression* is a method invocation ([Method invocations](expressions.md#method-invocations)).</span></span> <span data-ttu-id="2886a-1020">*Primary_expression* bir *delegate_type*değeri ise, *invocation_expression* bir temsilci çağrıdır ([temsilci etkinleştirmeleri](expressions.md#delegate-invocations)).</span><span class="sxs-lookup"><span data-stu-id="2886a-1020">If the *primary_expression* is a value of a *delegate_type*, the *invocation_expression* is a delegate invocation ([Delegate invocations](expressions.md#delegate-invocations)).</span></span> <span data-ttu-id="2886a-1021">*Primary_expression* bir yöntem grubu veya bir *delegate_type*değeri değilse, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1021">If the *primary_expression* is neither a method group nor a value of a *delegate_type*, a binding-time error occurs.</span></span>

<span data-ttu-id="2886a-1022">İsteğe bağlı *argument_list* ([bağımsız değişken listeleri](expressions.md#argument-lists)), yöntemin parametreleri için değerler veya değişken başvuruları sağlar.</span><span class="sxs-lookup"><span data-stu-id="2886a-1022">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) provides values or variable references for the parameters of the method.</span></span>

<span data-ttu-id="2886a-1023">Bir *invocation_expression* değerlendirilme sonucu aşağıdaki gibi sınıflandırıldı:</span><span class="sxs-lookup"><span data-stu-id="2886a-1023">The result of evaluating an *invocation_expression* is classified as follows:</span></span>

*  <span data-ttu-id="2886a-1024">*İnvocation_expression* , `void`döndüren bir yöntemi veya temsilciyi çağıralıyorsa, sonuç Nothing olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1024">If the *invocation_expression* invokes a method or delegate that returns `void`, the result is nothing.</span></span> <span data-ttu-id="2886a-1025">Nothing olarak sınıflandırılan bir ifadeye, yalnızca bir *statement_expression* ([ifade deyimleri](statements.md#expression-statements)) bağlamında veya bir *lambda_expression* ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) gövdesinde izin verilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1025">An expression that is classified as nothing is permitted only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)) or as the body of a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="2886a-1026">Aksi halde bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1026">Otherwise a binding-time error occurs.</span></span>
*  <span data-ttu-id="2886a-1027">Aksi takdirde, sonuç Yöntem veya temsilci tarafından döndürülen türün bir değeridir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1027">Otherwise, the result is a value of the type returned by the method or delegate.</span></span>

#### <a name="method-invocations"></a><span data-ttu-id="2886a-1028">Yöntem etkinleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-1028">Method invocations</span></span>

<span data-ttu-id="2886a-1029">Bir yöntem çağrısı için, invocation_expression *primary_expression* bir yöntem grubu olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1029">For a method invocation, the *primary_expression* of the *invocation_expression* must be a method group.</span></span> <span data-ttu-id="2886a-1030">Yöntem grubu, çağrılacak bir yöntemi veya çağırmak için belirli bir yöntemi seçebileceğiniz aşırı yüklenmiş yöntemleri belirler.</span><span class="sxs-lookup"><span data-stu-id="2886a-1030">The method group identifies the one method to invoke or the set of overloaded methods from which to choose a specific method to invoke.</span></span> <span data-ttu-id="2886a-1031">İkinci durumda, çağrılacak belirli yöntemin belirlenmesi, *argument_list*bağımsız değişkenlerin türleri tarafından belirtilen bağlamı temel alır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1031">In the latter case, determination of the specific method to invoke is based on the context provided by the types of the arguments in the *argument_list*.</span></span>

<span data-ttu-id="2886a-1032">`M` `M(A)`bir yöntem grubu (muhtemelen bir *type_argument_list*dahil) ve `A` isteğe bağlı bir *argument_list*olduğunda, bu form için bir yöntem çağrısı olan bağlama zamanı işleme, aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="2886a-1032">The binding-time processing of a method invocation of the form `M(A)`, where `M` is a method group (possibly including a *type_argument_list*), and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="2886a-1033">Yöntem çağrısı için aday yöntemler kümesi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1033">The set of candidate methods for the method invocation is constructed.</span></span> <span data-ttu-id="2886a-1034">Her yöntem için `F` Yöntem grubuyla ilişkili `M`:</span><span class="sxs-lookup"><span data-stu-id="2886a-1034">For each method `F` associated with the method group `M`:</span></span>
   *  <span data-ttu-id="2886a-1035">`F` genel değilse, şu durumlarda `F` bir adaydır:</span><span class="sxs-lookup"><span data-stu-id="2886a-1035">If `F` is non-generic, `F` is a candidate when:</span></span>
      * <span data-ttu-id="2886a-1036">`M`, tür bağımsız değişken listesine sahip değil ve</span><span class="sxs-lookup"><span data-stu-id="2886a-1036">`M` has no type argument list, and</span></span>
      * <span data-ttu-id="2886a-1037">`F`, `A` ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)) göre uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1037">`F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="2886a-1038">`F` geneldir ve `M` tür bağımsız değişken listesine sahip değilse, şu durumlarda `F` bir adaydır:</span><span class="sxs-lookup"><span data-stu-id="2886a-1038">If `F` is generic and `M` has no type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="2886a-1039">Tür çıkarımı ([tür çıkarımı](expressions.md#type-inference)) başarılı, çağrının tür bağımsız değişkenlerinin bir listesini, ve</span><span class="sxs-lookup"><span data-stu-id="2886a-1039">Type inference ([Type inference](expressions.md#type-inference)) succeeds, inferring a list of type arguments for the call, and</span></span>
      * <span data-ttu-id="2886a-1040">Çıkarsanan tür bağımsız değişkenleri karşılık gelen yöntem türü parametreleri yerine eklendikten sonra, F parametre listesindeki tüm oluşturulmuş türler kısıtlamalarını karşılar ([kısıtlamalar karşılamaları](types.md#satisfying-constraints)) ve `F` parametre listesi `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)) açısından geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1040">Once the inferred type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="2886a-1041">`F` geneldir ve `M` bir tür bağımsız değişken listesi içeriyorsa, şu durumlarda bir aday `F`:</span><span class="sxs-lookup"><span data-stu-id="2886a-1041">If `F` is generic and `M` includes a type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="2886a-1042">`F`, tür bağımsız değişkeni listesinde sağlanan sayıda yöntem türü parametreye sahiptir ve</span><span class="sxs-lookup"><span data-stu-id="2886a-1042">`F` has the same number of method type parameters as were supplied in the type argument list, and</span></span>
      * <span data-ttu-id="2886a-1043">Tür bağımsız değişkenleri karşılık gelen yöntem türü parametreleri yerine eklendikten sonra, F parametre listesindeki tüm oluşturulmuş türler kısıtlamalarını karşılar ([kısıtlamalar karşılamaları](types.md#satisfying-constraints)) ve `F` parametre listesi `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)) açısından geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1043">Once the type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
*  <span data-ttu-id="2886a-1044">Aday yöntemler kümesi yalnızca en çok türetilen türlerden Yöntemler içerecek şekilde azaltılır: `C.F` her bir yöntem Için, `C` `F` bildirildiği tür, `C` temel türünde bildirildiği tüm yöntemler kümeden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1044">The set of candidate methods is reduced to contain only methods from the most derived types: For each method `C.F` in the set, where `C` is the type in which the method `F` is declared, all methods declared in a base type of `C` are removed from the set.</span></span> <span data-ttu-id="2886a-1045">Ayrıca, `C` `object`dışında bir sınıf türü ise, bir arabirim türünde belirtilen tüm yöntemler kümeden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1045">Furthermore, if `C` is a class type other than `object`, all methods declared in an interface type are removed from the set.</span></span> <span data-ttu-id="2886a-1046">(Bu ikinci kural yalnızca Yöntem grubu, nesne dışındaki etkin bir temel sınıfa sahip olan bir tür parametresindeki üye aramasının ve boş olmayan etkin bir arabirim kümesinin sonucu olduğunda etkiler.)</span><span class="sxs-lookup"><span data-stu-id="2886a-1046">(This latter rule only has affect when the method group was the result of a member lookup on a type parameter having an effective base class other than object and a non-empty effective interface set.)</span></span>
*  <span data-ttu-id="2886a-1047">Elde edilen aday yöntemler kümesi boşsa, aşağıdaki adımlar üzerinde daha fazla işleme terk edilir ve bunun yerine, çağırma yöntemi çağırma ([uzantı yöntemi etkinleştirmeleri](expressions.md#extension-method-invocations)) olarak çağrıyı işlemek için bir girişimde bulunuldu.</span><span class="sxs-lookup"><span data-stu-id="2886a-1047">If the resulting set of candidate methods is empty, then further processing along the following steps are abandoned, and instead an attempt is made to process the invocation as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="2886a-1048">Bu başarısız olursa, geçerli hiçbir yöntem yoktur ve bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1048">If this fails, then no applicable methods exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="2886a-1049">Aday Yöntemler kümesinin en iyi yöntemi, [aşırı yükleme çözümünün](expressions.md#overload-resolution)aşırı yükleme çözümleme kuralları kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1049">The best method of the set of candidate methods is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="2886a-1050">Tek bir en iyi yöntem tanımlanamıyorsa, yöntem çağırma belirsizdir ve bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1050">If a single best method cannot be identified, the method invocation is ambiguous, and a binding-time error occurs.</span></span> <span data-ttu-id="2886a-1051">Aşırı yükleme çözümlemesi gerçekleştirirken, genel yöntemin parametreleri karşılık gelen yöntem türü parametreleri için tür bağımsız değişkenleri (sağlanan veya çıkartılan) değiştirildikten sonra değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1051">When performing overload resolution, the parameters of a generic method are considered after substituting the type arguments (supplied or inferred) for the corresponding method type parameters.</span></span>
*  <span data-ttu-id="2886a-1052">Seçilen en iyi yöntemin son doğrulaması gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1052">Final validation of the chosen best method is performed:</span></span>
   * <span data-ttu-id="2886a-1053">Yöntem, Yöntem grubu bağlamında onaylanır: en iyi yöntem bir statik yöntem ise, Yöntem grubu bir *simple_name* veya bir *member_access* bir tür aracılığıyla sonuçlanmış olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1053">The method is validated in the context of the method group: If the best method is a static method, the method group must have resulted from a *simple_name* or a *member_access* through a type.</span></span> <span data-ttu-id="2886a-1054">En iyi yöntem bir örnek yöntemi ise, Yöntem grubu bir *simple_name*, bir değişken veya değer aracılığıyla bir *member_access* veya bir *base_access*sonuçlanmış olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1054">If the best method is an instance method, the method group must have resulted from a *simple_name*, a *member_access* through a variable or value, or a *base_access*.</span></span> <span data-ttu-id="2886a-1055">Bu gereksinimlerin hiçbiri doğru değilse, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1055">If neither of these requirements is true, a binding-time error occurs.</span></span>
   * <span data-ttu-id="2886a-1056">En iyi yöntem genel bir yöntem ise, tür bağımsız değişkenleri (sağlanan veya çıkartılan) genel yöntemde belirtilen kısıtlamalara (karşılayan[kısıtlamalar](types.md#satisfying-constraints)) göre denetlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1056">If the best method is a generic method, the type arguments (supplied or inferred) are checked against the constraints ([Satisfying constraints](types.md#satisfying-constraints)) declared on the generic method.</span></span> <span data-ttu-id="2886a-1057">Herhangi bir tür bağımsız değişkeni tür parametresindeki karşılık gelen kısıtlamayı karşılamaz bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1057">If any type argument does not satisfy the corresponding constraint(s) on the type parameter, a binding-time error occurs.</span></span>

<span data-ttu-id="2886a-1058">Yukarıdaki adımlarla bağlama zamanında bir Yöntem seçildikten ve doğrulandıktan sonra, gerçek çalışma zamanı çağrısı, [dinamik aşırı yükleme çözümünün derleme zamanı denetiminde](expressions.md#compile-time-checking-of-dynamic-overload-resolution)açıklanan işlev üye çağırma kurallarına göre işlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1058">Once a method has been selected and validated at binding-time by the above steps, the actual run-time invocation is processed according to the rules of function member invocation described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="2886a-1059">Yukarıda açıklanan çözüm kurallarının sezgisel etkisi aşağıdaki gibidir: bir yöntem çağrısı tarafından çağrılan belirli bir yöntemi bulmak Için, yöntem çağırma tarafından belirtilen türle başlayın ve en az bir tane geçerli olana kadar devralma zincirini devam edin. erişilebilir, geçersiz kılma olmayan yöntem bildirimi bulundu.</span><span class="sxs-lookup"><span data-stu-id="2886a-1059">The intuitive effect of the resolution rules described above is as follows: To locate the particular method invoked by a method invocation, start with the type indicated by the method invocation and proceed up the inheritance chain until at least one applicable, accessible, non-override method declaration is found.</span></span> <span data-ttu-id="2886a-1060">Daha sonra tür çıkarımı ve aşırı yükleme çözümlemesini, bu tür içinde belirtilen geçerli, erişilebilir, geçersiz kılma olmayan yöntemler kümesi ve bu nedenle seçilen yöntemi çağırmak için gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="2886a-1060">Then perform type inference and overload resolution on the set of applicable, accessible, non-override methods declared in that type and invoke the method thus selected.</span></span> <span data-ttu-id="2886a-1061">Hiçbir yöntem bulunmazsa, çağırma yöntemini bir genişletme yöntemi çağrısı olarak işlemeyi deneyin.</span><span class="sxs-lookup"><span data-stu-id="2886a-1061">If no method was found, try instead to process the invocation as an extension method invocation.</span></span>

#### <a name="extension-method-invocations"></a><span data-ttu-id="2886a-1062">Genişletme yöntemi etkinleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-1062">Extension method invocations</span></span>

<span data-ttu-id="2886a-1063">Bir yöntem çağrısında ([paketlenmiş örneklerde çağırma](expressions.md#invocations-on-boxed-instances)), formlardan birinin</span><span class="sxs-lookup"><span data-stu-id="2886a-1063">In a method invocation ([Invocations on boxed instances](expressions.md#invocations-on-boxed-instances)) of one of the forms</span></span>
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
<span data-ttu-id="2886a-1064">çağrının normal işlemesi uygulanabilir bir yöntem bulamıyorsa, yapıyı uzantı metodu çağrısı olarak işlemek için bir girişimde bulunuldu.</span><span class="sxs-lookup"><span data-stu-id="2886a-1064">if the normal processing of the invocation finds no applicable methods, an attempt is made to process the construct as an extension method invocation.</span></span> <span data-ttu-id="2886a-1065">Eğer *Expr* veya *bağımsız* değişkenlerin herhangi birinde derleme zamanı türü `dynamic`varsa, genişletme yöntemleri uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-1065">If *expr* or any of the *args* has compile-time type `dynamic`, extension methods will not apply.</span></span>

<span data-ttu-id="2886a-1066">Amaç, karşılık gelen statik yöntem çağrısının gerçekleşmesi için en iyi *type_name* `C`bulma yöntemidir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1066">The objective is to find the best *type_name* `C`, so that the corresponding static method invocation can take place:</span></span>
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

<span data-ttu-id="2886a-1067">Bir genişletme yöntemi, şu durumlarda ***uygun*** `Ci.Mj`:</span><span class="sxs-lookup"><span data-stu-id="2886a-1067">An extension method `Ci.Mj` is ***eligible*** if:</span></span>

*  <span data-ttu-id="2886a-1068">`Ci` genel olmayan, iç içe olmayan bir sınıftır</span><span class="sxs-lookup"><span data-stu-id="2886a-1068">`Ci` is a non-generic, non-nested class</span></span>
*  <span data-ttu-id="2886a-1069">`Mj` adı *tanımlayıcı*</span><span class="sxs-lookup"><span data-stu-id="2886a-1069">The name of `Mj` is *identifier*</span></span>
*  <span data-ttu-id="2886a-1070">`Mj`, yukarıda gösterildiği gibi bağımsız değişkenlere statik bir yöntem olarak uygulandığında erişilebilir ve uygulanabilir</span><span class="sxs-lookup"><span data-stu-id="2886a-1070">`Mj` is accessible and applicable when applied to the arguments as a static method as shown above</span></span>
*  <span data-ttu-id="2886a-1071">Bir örtük kimlik, başvuru veya paketleme dönüştürmesi, *ifadenin* `Mj`ilk parametresinin türüne.</span><span class="sxs-lookup"><span data-stu-id="2886a-1071">An implicit identity, reference or boxing conversion exists from *expr* to the type of the first parameter of `Mj`.</span></span>

<span data-ttu-id="2886a-1072">`C` araması şu şekilde devam eder:</span><span class="sxs-lookup"><span data-stu-id="2886a-1072">The search for `C` proceeds as follows:</span></span>

*  <span data-ttu-id="2886a-1073">En yakın kapsayan ad alanı bildirimiyle başlayarak, kapsayan her bir ad alanı bildirimine devam edin ve kapsayan derleme birimi ile sona ermek üzere, bir dizi genişletme yöntemini bulmak için art arda denemeler yapılır:</span><span class="sxs-lookup"><span data-stu-id="2886a-1073">Starting with the closest enclosing namespace declaration, continuing with each enclosing namespace declaration, and ending with the containing compilation unit, successive attempts are made to find a candidate set of extension methods:</span></span>
   * <span data-ttu-id="2886a-1074">Verilen ad alanı veya derleme birimi, uygun genişletme yöntemleriyle `Mj``Ci` genel olmayan tür bildirimlerini doğrudan içeriyorsa, bu uzantı yöntemlerinin kümesi aday kümesidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1074">If the given namespace or compilation unit directly contains non-generic type declarations `Ci` with eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
   * <span data-ttu-id="2886a-1075">*Using_static_declarations* tarafından içeri aktarılan ve doğrudan verilen ad alanındaki veya derleme `Mj`birimindeki *using_namespace_directive*s tarafından içeri aktarılan ad alanları için `Ci`, bu genişletme yöntemlerinin kümesi aday kümesidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1075">If types `Ci` imported by *using_static_declarations* and directly declared in namespaces imported by *using_namespace_directive*s in the given namespace or compilation unit directly contain eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
*  <span data-ttu-id="2886a-1076">Herhangi bir kapsayan ad alanı bildiriminde veya derleme biriminde hiçbir aday kümesi bulunamazsa, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1076">If no candidate set is found in any enclosing namespace declaration or compilation unit, a compile-time error occurs.</span></span>
*  <span data-ttu-id="2886a-1077">Aksi halde, aşırı yükleme çözümlemesi aday kümesine ([aşırı yükleme çözümlemesi](expressions.md#overload-resolution)) açıklandığı şekilde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1077">Otherwise, overload resolution is applied to the candidate set as described in ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="2886a-1078">Tek bir en iyi yöntem bulunamazsa, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1078">If no single best method is found, a compile-time error occurs.</span></span>
*  <span data-ttu-id="2886a-1079">`C`, en iyi yöntemin bir genişletme yöntemi olarak bildirildiği türdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1079">`C` is the type within which the best method is declared as an extension method.</span></span>

<span data-ttu-id="2886a-1080">`C` bir hedef olarak kullanıldığında, yöntem çağrısı daha sonra statik bir yöntem çağırma ([dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1080">Using `C` as a target, the method call is then processed as a static method invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="2886a-1081">Önceki kurallar, örnek yöntemlerinin genişletme yöntemlerine göre öncelikli olduğunu, iç ad alanı bildirimlerinde kullanılabilen genişletme yöntemlerinin, dış ad alanı bildirimlerinde bulunan genişletme yöntemlerine ve bu uzantıya göre öncelikli olduğunu ifade ediyor. bir ad alanında doğrudan tanımlanan Yöntemler, using ad alanı yönergesi ile aynı ad alanına aktarılan genişletme yöntemlerine göre önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1081">The preceding rules mean that instance methods take precedence over extension methods, that extension methods available in inner namespace declarations take precedence over extension methods available in outer namespace declarations, and that extension methods declared directly in a namespace take precedence over extension methods imported into that same namespace with a using namespace directive.</span></span> <span data-ttu-id="2886a-1082">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2886a-1082">For example:</span></span>
```csharp
public static class E
{
    public static void F(this object obj, int i) { }

    public static void F(this object obj, string s) { }
}

class A { }

class B
{
    public void F(int i) { }
}

class C
{
    public void F(object obj) { }
}

class X
{
    static void Test(A a, B b, C c) {
        a.F(1);              // E.F(object, int)
        a.F("hello");        // E.F(object, string)

        b.F(1);              // B.F(int)
        b.F("hello");        // E.F(object, string)

        c.F(1);              // C.F(object)
        c.F("hello");        // C.F(object)
    }
}
```

<span data-ttu-id="2886a-1083">Örnekte, `B`yöntemi ilk uzantı yöntemine göre önceliklidir ve `C`yöntemi her iki uzantı yönteminden önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1083">In the example, `B`'s method takes precedence over the first extension method, and `C`'s method takes precedence over both extension methods.</span></span>

```csharp
public static class C
{
    public static void F(this int i) { Console.WriteLine("C.F({0})", i); }
    public static void G(this int i) { Console.WriteLine("C.G({0})", i); }
    public static void H(this int i) { Console.WriteLine("C.H({0})", i); }
}

namespace N1
{
    public static class D
    {
        public static void F(this int i) { Console.WriteLine("D.F({0})", i); }
        public static void G(this int i) { Console.WriteLine("D.G({0})", i); }
    }
}

namespace N2
{
    using N1;

    public static class E
    {
        public static void F(this int i) { Console.WriteLine("E.F({0})", i); }
    }

    class Test
    {
        static void Main(string[] args)
        {
            1.F();
            2.G();
            3.H();
        }
    }
}
```

<span data-ttu-id="2886a-1084">Bu örneğin çıktısı şu şekilde olur:</span><span class="sxs-lookup"><span data-stu-id="2886a-1084">The output of this example is:</span></span>
```console
E.F(1)
D.G(2)
C.H(3)
```
<span data-ttu-id="2886a-1085">`D.G` `C.G`üzerinden önceliklidir ve `E.F` `D.F` ve `C.F`göre önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1085">`D.G` takes precedence over `C.G`, and `E.F` takes precedence over both `D.F` and `C.F`.</span></span>

#### <a name="delegate-invocations"></a><span data-ttu-id="2886a-1086">Temsilci etkinleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-1086">Delegate invocations</span></span>

<span data-ttu-id="2886a-1087">Bir temsilci çağrısı için, *invocation_expression* *primary_expression* bir *delegate_type*değeri olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1087">For a delegate invocation, the *primary_expression* of the *invocation_expression* must be a value of a *delegate_type*.</span></span> <span data-ttu-id="2886a-1088">Ayrıca, *delegate_type* *delegate_type*aynı parametre listesi olan bir işlev üyesi olması halinde *delegate_type* , *invocation_expression* *argument_list* göre uygulanabilir ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1088">Furthermore, considering the *delegate_type* to be a function member with the same parameter list as the *delegate_type*, the *delegate_type* must be applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the *argument_list* of the *invocation_expression*.</span></span>

<span data-ttu-id="2886a-1089">`D(A)`, bir *delegate_type* *primary_expression* `D` ve `A` isteğe bağlı bir *argument_list*olduğunda, bu form için bir temsilci çağrısı çalıştırma zamanı işleme, aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="2886a-1089">The run-time processing of a delegate invocation of the form `D(A)`, where `D` is a *primary_expression* of a *delegate_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="2886a-1090">`D` değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1090">`D` is evaluated.</span></span> <span data-ttu-id="2886a-1091">Bu değerlendirme bir özel duruma neden olursa başka bir adım yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1091">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="2886a-1092">`D` değeri geçerli olacak şekilde işaretlendi.</span><span class="sxs-lookup"><span data-stu-id="2886a-1092">The value of `D` is checked to be valid.</span></span> <span data-ttu-id="2886a-1093">`D` değeri `null`, bir `System.NullReferenceException` oluşturulur ve başka bir adım yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1093">If the value of `D` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="2886a-1094">Aksi takdirde, `D` bir temsilci örneğine başvurudur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1094">Otherwise, `D` is a reference to a delegate instance.</span></span> <span data-ttu-id="2886a-1095">İşlev üyesi etkinleştirmeleri ([dinamik aşırı yükleme çözümlemesi Için derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), temsilcinin çağırma listesindeki çağrılabilir varlıkların her birinde gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1095">Function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) are performed on each of the callable entities in the invocation list of the delegate.</span></span> <span data-ttu-id="2886a-1096">Örnek ve örnek yönteminden oluşan çağrılabilir varlıklar için, çağırma örneği çağrılabilir varlıkta bulunan örneğidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1096">For callable entities consisting of an instance and instance method, the instance for the invocation is the instance contained in the callable entity.</span></span>

### <a name="element-access"></a><span data-ttu-id="2886a-1097">Öğe erişimi</span><span class="sxs-lookup"><span data-stu-id="2886a-1097">Element access</span></span>

<span data-ttu-id="2886a-1098">Bir *element_access* *primary_no_array_creation_expression*, ardından bir "`[`" belirteci ve ardından bir *argument_list*ve ardından "`]`" belirteci ile oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1098">An *element_access* consists of a *primary_no_array_creation_expression*, followed by a "`[`" token, followed by an *argument_list*, followed by a "`]`" token.</span></span> <span data-ttu-id="2886a-1099">*Argument_list* , virgülle ayrılmış bir veya daha fazla *bağımsız değişkenden*oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1099">The *argument_list* consists of one or more *argument*s, separated by commas.</span></span>

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

<span data-ttu-id="2886a-1100">Bir *element_access* *argument_list* `ref` veya `out` bağımsız değişken içermesine izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1100">The *argument_list* of an *element_access* is not allowed to contain `ref` or `out` arguments.</span></span>

<span data-ttu-id="2886a-1101">Aşağıdakilerden en az biri tutuyorsa, bir *element_access* dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)):</span><span class="sxs-lookup"><span data-stu-id="2886a-1101">An *element_access* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="2886a-1102">*Primary_no_array_creation_expression* `dynamic`derleme zamanı türü vardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1102">The *primary_no_array_creation_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="2886a-1103">*Argument_list* en az bir ifadesinin derleme zamanı türü `dynamic` ve *primary_no_array_creation_expression* bir dizi türüne sahip değil.</span><span class="sxs-lookup"><span data-stu-id="2886a-1103">At least one expression of the *argument_list* has compile-time type `dynamic` and the *primary_no_array_creation_expression* does not have an array type.</span></span>

<span data-ttu-id="2886a-1104">Bu durumda, derleyici *element_access* `dynamic`türünde bir değer olarak sınıflandırır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1104">In this case the compiler classifies the *element_access* as a value of type `dynamic`.</span></span> <span data-ttu-id="2886a-1105">*Element_access* anlamını öğrenmek için aşağıdaki kurallar, *primary_no_array_creation_expression* ve derleme zamanı türü `dynamic`olan *argument_list* ifadelerin derleme zamanı türü yerine çalışma zamanı türü kullanılarak çalışma zamanında uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1105">The rules below to determine the meaning of the *element_access* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_no_array_creation_expression* and *argument_list* expressions which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="2886a-1106">*Primary_no_array_creation_expression* derleme zamanı türünde `dynamic`yoksa, öğe erişimi, [dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)konusunda açıklandığı gibi sınırlı bir derleme süresi denetimi yapar.</span><span class="sxs-lookup"><span data-stu-id="2886a-1106">If the *primary_no_array_creation_expression* does not have compile-time type `dynamic`, then the element access undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="2886a-1107">Bir *element_access* *primary_no_array_creation_expression* bir *array_type*değeri ise, *element_access* bir dizi erişimdir ([dizi erişimi](expressions.md#array-access)).</span><span class="sxs-lookup"><span data-stu-id="2886a-1107">If the *primary_no_array_creation_expression* of an *element_access* is a value of an *array_type*, the *element_access* is an array access ([Array access](expressions.md#array-access)).</span></span> <span data-ttu-id="2886a-1108">Aksi takdirde *primary_no_array_creation_expression* , bir veya daha fazla Indexer üyesine sahip bir sınıf, yapı veya arabirim türünün bir değişkeni ya da değeri olmalıdır ve bu durumda *element_access* bir Dizin Oluşturucu erişimi ([Dizin Oluşturucu erişimi](expressions.md#indexer-access)) olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1108">Otherwise, the *primary_no_array_creation_expression* must be a variable or value of a class, struct, or interface type that has one or more indexer members, in which case the *element_access* is an indexer access ([Indexer access](expressions.md#indexer-access)).</span></span>

#### <a name="array-access"></a><span data-ttu-id="2886a-1109">Dizi erişimi</span><span class="sxs-lookup"><span data-stu-id="2886a-1109">Array access</span></span>

<span data-ttu-id="2886a-1110">Dizi erişimi için *element_access* *primary_no_array_creation_expression* bir *array_type*değeri olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1110">For an array access, the *primary_no_array_creation_expression* of the *element_access* must be a value of an *array_type*.</span></span> <span data-ttu-id="2886a-1111">Ayrıca, dizi erişiminin *argument_list* adlandırılmış bağımsız değişkenler içermesine izin verilmez. *Argument_list* ifadelerin sayısı, *array_type*derecesine göre aynı olmalıdır ve her ifadenin `int`, `uint`, `long`, `ulong`veya bu türlerden bir veya daha fazla örtülü olarak dönüştürülebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1111">Furthermore, the *argument_list* of an array access is not allowed to contain named arguments.The number of expressions in the *argument_list* must be the same as the rank of the *array_type*, and each expression must be of type `int`, `uint`, `long`, `ulong`, or must be implicitly convertible to one or more of these types.</span></span>

<span data-ttu-id="2886a-1112">Dizi erişiminin hesaplanmasının sonucu, dizinin öğe türünün bir değişkenidir; Yani, *argument_list*ifade (ler) in değeri tarafından seçilen dizi öğesi.</span><span class="sxs-lookup"><span data-stu-id="2886a-1112">The result of evaluating an array access is a variable of the element type of the array, namely the array element selected by the value(s) of the expression(s) in the *argument_list*.</span></span>

<span data-ttu-id="2886a-1113">Form `P[A]`, `P` bir *array_type* *primary_no_array_creation_expression* olduğu ve `A` bir *argument_list*olan bir dizi erişiminin çalışma zamanı işleme, aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="2886a-1113">The run-time processing of an array access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of an *array_type* and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="2886a-1114">`P` değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1114">`P` is evaluated.</span></span> <span data-ttu-id="2886a-1115">Bu değerlendirme bir özel duruma neden olursa başka bir adım yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1115">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="2886a-1116">*Argument_list* Dizin ifadeleri soldan sağa sırayla değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1116">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="2886a-1117">Her bir dizin ifadesinin değerlendirmesi aşağıdaki türlerden birine örtük dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) gerçekleştirilir: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="2886a-1117">Following evaluation of each index expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="2886a-1118">Bu listede örtük bir dönüştürmenin bulunduğu ilk tür seçilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1118">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="2886a-1119">Örneğin, Dizin ifadesi türü `short`, `short` 'den `int` ve `short` 'den `long` 'e örtük dönüştürmeler yapıladıklarından `int` örtük bir dönüştürme işlemi gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1119">For instance, if the index expression is of type `short` then an implicit conversion to `int` is performed, since implicit conversions from `short` to `int` and from `short` to `long` are possible.</span></span> <span data-ttu-id="2886a-1120">Bir dizin ifadesi veya sonraki örtük dönüştürme değerlendirmesi bir özel duruma neden oluyorsa, daha fazla dizin ifadesi değerlendirilmez ve başka bir adım yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1120">If evaluation of an index expression or the subsequent implicit conversion causes an exception, then no further index expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="2886a-1121">`P` değeri geçerli olacak şekilde işaretlendi.</span><span class="sxs-lookup"><span data-stu-id="2886a-1121">The value of `P` is checked to be valid.</span></span> <span data-ttu-id="2886a-1122">`P` değeri `null`, bir `System.NullReferenceException` oluşturulur ve başka bir adım yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1122">If the value of `P` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="2886a-1123">*Argument_list* her bir ifadenin değeri, `P`tarafından başvurulan dizi örneğinin her boyutunun gerçek sınırlarına göre denetlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1123">The value of each expression in the *argument_list* is checked against the actual bounds of each dimension of the array instance referenced by `P`.</span></span> <span data-ttu-id="2886a-1124">Bir veya daha fazla değer Aralık dışında ise, bir `System.IndexOutOfRangeException` oluşturulur ve başka bir adım yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1124">If one or more values are out of range, a `System.IndexOutOfRangeException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="2886a-1125">Dizin ifadeleri tarafından verilen dizi öğesinin konumu hesaplanır ve bu konum dizi erişiminin sonucu olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1125">The location of the array element given by the index expression(s) is computed, and this location becomes the result of the array access.</span></span>

#### <a name="indexer-access"></a><span data-ttu-id="2886a-1126">Dizin Oluşturucu erişimi</span><span class="sxs-lookup"><span data-stu-id="2886a-1126">Indexer access</span></span>

<span data-ttu-id="2886a-1127">Dizin Oluşturucu erişimi için, element_access *primary_no_array_creation_expression* bir sınıf , yapı veya arabirim türünde bir değişken ya da değer olmalıdır ve bu tür *element_access* *argument_list* açısından geçerli olan bir veya daha fazla Dizin Oluşturucu uygulamalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1127">For an indexer access, the *primary_no_array_creation_expression* of the *element_access* must be a variable or value of a class, struct, or interface type, and this type must implement one or more indexers that are applicable with respect to the *argument_list* of the *element_access*.</span></span>

<span data-ttu-id="2886a-1128">Form `P[A]`Dizin Oluşturucu erişiminin bağlama zamanı işleme, `P` bir sınıf, yapı veya arabirim türü `T`*primary_no_array_creation_expression* ve `A` bir *argument_list*ise aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="2886a-1128">The binding-time processing of an indexer access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of a class, struct, or interface type `T`, and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="2886a-1129">`T` tarafından belirtilen dizin oluşturucular kümesi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1129">The set of indexers provided by `T` is constructed.</span></span> <span data-ttu-id="2886a-1130">Küme, `T` belirtilen tüm dizin oluşturuculardan veya `override` bildirimleri olmayan ve geçerli bağlamda erişilebilen bir `T` taban türünde ([üye erişim](basic-concepts.md#member-access)) oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1130">The set consists of all indexers declared in `T` or a base type of `T` that are not `override` declarations and are accessible in the current context ([Member access](basic-concepts.md#member-access)).</span></span>
*  <span data-ttu-id="2886a-1131">Küme, uygulanabilir ve diğer Dizin oluşturucular tarafından gizlenmediği dizin oluşturucularının düşürüldü.</span><span class="sxs-lookup"><span data-stu-id="2886a-1131">The set is reduced to those indexers that are applicable and not hidden by other indexers.</span></span> <span data-ttu-id="2886a-1132">Aşağıdaki kurallar, kümesindeki `S.I` her bir dizin oluşturucuya uygulanır; burada `S`, dizin oluşturucunun `I` bildirildiği türdür:</span><span class="sxs-lookup"><span data-stu-id="2886a-1132">The following rules are applied to each indexer `S.I` in the set, where `S` is the type in which the indexer `I` is declared:</span></span>
   * <span data-ttu-id="2886a-1133">`I`, `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)) açısından geçerli değilse `I` kümeden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1133">If `I` is not applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then `I` is removed from the set.</span></span>
   * <span data-ttu-id="2886a-1134">`I`, `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)) açısından geçerliyse, `S` temel türünde belirtilen tüm dizin oluşturucular kümeden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1134">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then all indexers declared in a base type of `S` are removed from the set.</span></span>
   * <span data-ttu-id="2886a-1135">`I`, `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)) ve `S` `object`dışında bir sınıf türü ise, bir arabirimde bildirildiği tüm dizin oluşturucular kümeden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1135">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)) and `S` is a class type other than `object`, all indexers declared in an interface are removed from the set.</span></span>
*  <span data-ttu-id="2886a-1136">Elde edilen aday Dizin oluşturucular kümesi boşsa, uygulanabilir Dizin oluşturucular yok ve bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1136">If the resulting set of candidate indexers is empty, then no applicable indexers exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="2886a-1137">Aday Dizin oluşturucular kümesinin en iyi Dizin Oluşturucusu, [aşırı yükleme çözümünün](expressions.md#overload-resolution)aşırı yükleme çözümleme kuralları kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1137">The best indexer of the set of candidate indexers is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="2886a-1138">Tek bir en iyi Dizin Oluşturucu tanımlanamıyorsa, Dizin Oluşturucu erişimi belirsizdir ve bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1138">If a single best indexer cannot be identified, the indexer access is ambiguous, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="2886a-1139">*Argument_list* Dizin ifadeleri soldan sağa sırayla değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1139">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="2886a-1140">Dizin Oluşturucu erişimini işlemenin sonucu, Dizin Oluşturucu erişimi olarak sınıflandırılan bir ifadedir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1140">The result of processing the indexer access is an expression classified as an indexer access.</span></span> <span data-ttu-id="2886a-1141">Dizin Oluşturucu erişim ifadesi, yukarıdaki adımda belirlenen dizin oluşturucuya başvurur ve `P` ilişkili bir örnek ifadesi ve `A`ilişkili bağımsız değişken listesi içerir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1141">The indexer access expression references the indexer determined in the step above, and has an associated instance expression of `P` and an associated argument list of `A`.</span></span>

<span data-ttu-id="2886a-1142">Bir Dizin Oluşturucu erişimi, kullanıldığı içeriğe bağlı olarak, dizin oluşturucunun *Get erişimcisinin* veya *set erişimcisinin* çağrısına neden olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1142">Depending on the context in which it is used, an indexer access causes invocation of either the *get accessor* or the *set accessor* of the indexer.</span></span> <span data-ttu-id="2886a-1143">Dizin Oluşturucu erişimi bir atamanın hedefi ise, *küme erişimcisi* yeni bir değer atamak için çağrılır ([basit atama](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="2886a-1143">If the indexer access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="2886a-1144">Diğer tüm durumlarda, *get erişimcisi* geçerli değeri ([ifadelerin değerleri](expressions.md#values-of-expressions)) almak için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1144">In all other cases, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="this-access"></a><span data-ttu-id="2886a-1145">Bu erişim</span><span class="sxs-lookup"><span data-stu-id="2886a-1145">This access</span></span>

<span data-ttu-id="2886a-1146">*This_Access* , ayrılmış sözcükten `this`oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1146">A *this_access* consists of the reserved word `this`.</span></span>

```antlr
this_access
    : 'this'
    ;
```

<span data-ttu-id="2886a-1147">Bir *This_Access* yalnızca bir örnek oluşturucusunun, örnek yönteminin veya örnek erişimcinin *bloğunda* izin verilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1147">A *this_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="2886a-1148">Aşağıdaki anlamlara sahiptir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1148">It has one of the following meanings:</span></span>

*  <span data-ttu-id="2886a-1149">Bir sınıfın örnek Oluşturucusu içindeki bir *primary_expression* `this` kullanıldığında, değer olarak sınıflandırıldı.</span><span class="sxs-lookup"><span data-stu-id="2886a-1149">When `this` is used in a *primary_expression* within an instance constructor of a class, it is classified as a value.</span></span> <span data-ttu-id="2886a-1150">Değerin türü, kullanımın gerçekleştiği sınıfın örnek türü ([örnek türü](classes.md#the-instance-type)) ve değer, oluşturulmakta olan nesneye bir başvurudur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1150">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object being constructed.</span></span>
*  <span data-ttu-id="2886a-1151">`this`, bir sınıfın örnek metodu veya örnek erişimcisi içindeki bir *primary_expression* kullanıldığında, değer olarak sınıflandırılacaktır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1151">When `this` is used in a *primary_expression* within an instance method or instance accessor of a class, it is classified as a value.</span></span> <span data-ttu-id="2886a-1152">Değerin türü, kullanımının gerçekleştiği sınıfın örnek türü ([örnek türü](classes.md#the-instance-type)) ve değer, yöntemin veya erişimcinin çağrıldığı nesneye bir başvurudur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1152">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object for which the method or accessor was invoked.</span></span>
*  <span data-ttu-id="2886a-1153">Bir yapının örnek Oluşturucusu içindeki bir *primary_expression* `this` kullanıldığında, değişken olarak sınıflandırılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1153">When `this` is used in a *primary_expression* within an instance constructor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="2886a-1154">Değişkenin türü, kullanımın gerçekleştiği yapının örnek türüdür ([örnek türü](classes.md#the-instance-type)) ve değişken oluşturulan yapıyı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="2886a-1154">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs, and the variable represents the struct being constructed.</span></span> <span data-ttu-id="2886a-1155">Bir yapının örnek oluşturucusunun `this` değişkeni, yapı türünün bir `out` parametresiyle tamamen aynı şekilde davranır — özellikle bu, değişkenin örnek oluşturucusunun her yürütme yolunda kesinlikle atanması gerektiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1155">The `this` variable of an instance constructor of a struct behaves exactly the same as an `out` parameter of the struct type—in particular, this means that the variable must be definitely assigned in every execution path of the instance constructor.</span></span>
*  <span data-ttu-id="2886a-1156">`this`, bir yapının örnek yöntemi veya örnek erişimcisi içindeki bir *primary_expression* kullanıldığında, bir değişken olarak sınıflandırılacaktır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1156">When `this` is used in a *primary_expression* within an instance method or instance accessor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="2886a-1157">Değişkenin türü, kullanımının gerçekleştiği yapının örnek türüdür ([örnek türü](classes.md#the-instance-type)).</span><span class="sxs-lookup"><span data-stu-id="2886a-1157">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs.</span></span>
   * <span data-ttu-id="2886a-1158">Yöntem veya erişimci bir yineleyici ([yineleyiciler](classes.md#iterators)) değilse, `this` değişkeni Yöntem veya erişimcinin çağrıldığı yapıyı temsil eder ve yapı türünün bir `ref` parametresiyle tam olarak aynı şekilde davranır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1158">If the method or accessor is not an iterator ([Iterators](classes.md#iterators)), the `this` variable represents the struct for which the method or accessor was invoked, and behaves exactly the same as a `ref` parameter of the struct type.</span></span>
   * <span data-ttu-id="2886a-1159">Yöntem veya erişimci bir yineleyici ise, `this` değişkeni yöntemi veya erişimcinin çağrıldığı yapının bir kopyasını temsil eder ve yapı türünün bir değer parametresiyle tam olarak aynı şekilde davranır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1159">If the method or accessor is an iterator, the `this` variable represents a copy of the struct for which the method or accessor was invoked, and behaves exactly the same as a value parameter of the struct type.</span></span>

<span data-ttu-id="2886a-1160">Yukarıda listelenenlerin dışında bir bağlamda *primary_expression* `this` kullanımı, derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1160">Use of `this` in a *primary_expression* in a context other than the ones listed above is a compile-time error.</span></span> <span data-ttu-id="2886a-1161">Özellikle, bir statik yöntemde `this`, statik bir özellik erişimcisinde veya bir alan bildiriminin bir *variable_initializer* başvurmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1161">In particular, it is not possible to refer to `this` in a static method, a static property accessor, or in a *variable_initializer* of a field declaration.</span></span>

### <a name="base-access"></a><span data-ttu-id="2886a-1162">Temel erişim</span><span class="sxs-lookup"><span data-stu-id="2886a-1162">Base access</span></span>

<span data-ttu-id="2886a-1163">Bir *base_access* , "`.`" belirteci ve bir tanımlayıcı ya da köşeli ayraç içine alınmış bir *argument_list* gelen ayrılmış sözcükten `base` oluşur:</span><span class="sxs-lookup"><span data-stu-id="2886a-1163">A *base_access* consists of the reserved word `base` followed by either a "`.`" token and an identifier or an *argument_list* enclosed in square brackets:</span></span>

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

<span data-ttu-id="2886a-1164">Geçerli sınıf veya yapıda benzer adlandırılmış Üyeler tarafından gizlenen temel sınıf üyelerine erişmek için bir *base_access* kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1164">A *base_access* is used to access base class members that are hidden by similarly named members in the current class or struct.</span></span> <span data-ttu-id="2886a-1165">Bir *base_access* yalnızca bir örnek oluşturucusunun, örnek yönteminin veya örnek erişimcinin *bloğunda* izin verilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1165">A *base_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="2886a-1166">`base.I` bir sınıfta veya yapıda olduğunda, `I` bu sınıfın veya yapının temel sınıfının bir üyesini belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1166">When `base.I` occurs in a class or struct, `I` must denote a member of the base class of that class or struct.</span></span> <span data-ttu-id="2886a-1167">Benzer şekilde, bir sınıfta `base[E]` oluştuğunda, temel sınıfta ilgili bir Dizin Oluşturucu bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1167">Likewise, when `base[E]` occurs in a class, an applicable indexer must exist in the base class.</span></span>

<span data-ttu-id="2886a-1168">Bağlama zamanında, `base.I` ve `base[E]` `((B)this).I` *base_access* ifadeleri tam olarak yazılmış gibi değerlendirilir; burada `((B)this)[E]`, yapının gerçekleştiği sınıfın veya yapının temel sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1168">At binding-time, *base_access* expressions of the form `base.I` and `base[E]` are evaluated exactly as if they were written `((B)this).I` and `((B)this)[E]`, where `B` is the base class of the class or struct in which the construct occurs.</span></span> <span data-ttu-id="2886a-1169">Bu nedenle, `base.I` ve `base[E]` `this.I` ve `this[E]`karşılık gelir, bunun dışında `this` temel sınıfın bir örneği olarak görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1169">Thus, `base.I` and `base[E]` correspond to `this.I` and `this[E]`, except `this` is viewed as an instance of the base class.</span></span>

<span data-ttu-id="2886a-1170">Bir *base_access* bir sanal işlev üyesine (bir yöntem, özellik veya Dizin Oluşturucu) başvurduğunda, çalışma zamanında hangi işlev üyesinin çalıştırılacağını belirleme ([dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1170">When a *base_access* references a virtual function member (a method, property, or indexer), the determination of which function member to invoke at run-time ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is changed.</span></span> <span data-ttu-id="2886a-1171">Çağrılan işlev üyesi, işlev üyesinin, `B` (`this`çalışma zamanı türüne göre değil, temel olmayan bir erişimde olduğu gibi) ilgili en çok türetilmiş uygulama ([sanal yöntemler](classes.md#virtual-methods)) ile belirlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1171">The function member that is invoked is determined by finding the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of the function member with respect to `B` (instead of with respect to the run-time type of `this`, as would be usual in a non-base access).</span></span> <span data-ttu-id="2886a-1172">Bu nedenle, bir `virtual` işlevi üyesinin `override` içinde, işlev üyesinin devralınmış uygulamasını çağırmak için bir *base_access* kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1172">Thus, within an `override` of a `virtual` function member, a *base_access* can be used to invoke the inherited implementation of the function member.</span></span> <span data-ttu-id="2886a-1173">Bir *base_access* tarafından başvurulan işlev üyesi Özet ise, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1173">If the function member referenced by a *base_access* is abstract, a binding-time error occurs.</span></span>

### <a name="postfix-increment-and-decrement-operators"></a><span data-ttu-id="2886a-1174">Sonek artırma ve azaltma işleçleri</span><span class="sxs-lookup"><span data-stu-id="2886a-1174">Postfix increment and decrement operators</span></span>

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

<span data-ttu-id="2886a-1175">Bir sonek artışı veya azaltma işleminin işleneni, değişken olarak sınıflandırılmış bir ifade, özellik erişimi veya Dizin Oluşturucu erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1175">The operand of a postfix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="2886a-1176">İşlemin sonucu, işlenenden aynı türde bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1176">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="2886a-1177">*Primary_expression* , derleme zamanı türüne `dynamic`, işleç dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)), *post_increment_expression* veya *post_decrement_expression* derleme zamanı türü `dynamic` vardır ve aşağıdaki kurallar çalışma zamanında *primary_expression*çalışma zamanı türü kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1177">If the *primary_expression* has the compile-time type `dynamic` then the operator is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), the *post_increment_expression* or *post_decrement_expression* has the compile-time type `dynamic` and the following rules are applied at run-time using the run-time type of the *primary_expression*.</span></span>

<span data-ttu-id="2886a-1178">Bir sonek artışı veya azaltma işleminin işleneni bir özellik veya Dizin Oluşturucu erişimi ise, özelliğin veya dizin oluşturucunun hem `get` hem de bir `set` erişimcisi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1178">If the operand of a postfix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="2886a-1179">Bu durumda, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1179">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="2886a-1180">Tekil operatör aşırı yükleme çözümü ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1180">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="2886a-1181">Önceden tanımlı `++` ve `--` işleçleri şu türler için mevcuttur: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`ve herhangi bir numaralandırma türü.</span><span class="sxs-lookup"><span data-stu-id="2886a-1181">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="2886a-1182">Önceden tanımlanmış `++` işleçleri, işlenene 1 eklenerek üretilen değeri döndürür ve önceden tanımlanmış `--` işleçleri işleneni 1 çıkararak üretilen değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1182">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="2886a-1183">`checked` bağlamında, bu ekleme veya çıkarma sonucu sonuç türü aralığının dışındaysa ve sonuç türü bir tam sayı türü veya Enum türü ise, bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1183">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="2886a-1184">`x++` veya `x--` form Sonek artışı veya azaltma işleminin çalışma zamanı işleme aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="2886a-1184">The run-time processing of a postfix increment or decrement operation of the form `x++` or `x--` consists of the following steps:</span></span>

*   <span data-ttu-id="2886a-1185">`x` bir değişken olarak sınıflandırıldıysanız:</span><span class="sxs-lookup"><span data-stu-id="2886a-1185">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="2886a-1186">`x`, değişkeni üretmek için değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1186">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="2886a-1187">`x` değeri kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1187">The value of `x` is saved.</span></span>
    * <span data-ttu-id="2886a-1188">Seçilen işleç, bağımsız değişkeni olarak kaydedilen `x` değeriyle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1188">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="2886a-1189">İşleci tarafından döndürülen değer, `x`değerlendirmesi tarafından verilen konumda depolanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1189">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="2886a-1190">`x` kaydedilen değeri işlemin sonucu olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1190">The saved value of `x` becomes the result of the operation.</span></span>
*   <span data-ttu-id="2886a-1191">`x`, özellik veya Dizin Oluşturucu erişimi olarak sınıflandırıldıysanız:</span><span class="sxs-lookup"><span data-stu-id="2886a-1191">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="2886a-1192">Örnek ifadesi (`x` `static`değilse) ve bağımsız değişken listesi (`x` bir Dizin Oluşturucu erişimsiyse) `x` ile ilişkili olarak değerlendirilir ve sonuçlar sonraki `get` ve `set` erişimci etkinleştirmeleri içinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1192">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="2886a-1193">`x` `get` erişimcisi çağrılır ve döndürülen değer kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1193">The `get` accessor of `x` is invoked and the returned value is saved.</span></span>
    * <span data-ttu-id="2886a-1194">Seçilen işleç, bağımsız değişkeni olarak kaydedilen `x` değeriyle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1194">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="2886a-1195">`x` `set` erişimcisi, işleç tarafından `value` bağımsız değişkeni olarak döndürülen değerle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1195">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="2886a-1196">`x` kaydedilen değeri işlemin sonucu olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1196">The saved value of `x` becomes the result of the operation.</span></span>

<span data-ttu-id="2886a-1197">`++` ve `--` işleçleri önek gösterimini de destekler ([önek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="2886a-1197">The `++` and `--` operators also support prefix notation ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="2886a-1198">Genellikle, `x++` veya `x--` sonucu işlemden önceki `x` değeridir, ancak `++x` veya `--x` sonucu işlemden sonraki `x` değeridir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1198">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="2886a-1199">Her iki durumda da `x`, işlemden sonra aynı değere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1199">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="2886a-1200">Bir `operator ++` veya `operator --` uygulama, sonek veya ön ek gösterimi kullanılarak çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1200">An `operator ++` or `operator --` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="2886a-1201">İki gösterimler için ayrı işleç uygulamalarına sahip olmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1201">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="the-new-operator"></a><span data-ttu-id="2886a-1202">New işleci</span><span class="sxs-lookup"><span data-stu-id="2886a-1202">The new operator</span></span>

<span data-ttu-id="2886a-1203">`new` işleci, türlerin yeni örneklerini oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1203">The `new` operator is used to create new instances of types.</span></span>

<span data-ttu-id="2886a-1204">Üç `new` ifade biçimi vardır:</span><span class="sxs-lookup"><span data-stu-id="2886a-1204">There are three forms of `new` expressions:</span></span>

*  <span data-ttu-id="2886a-1205">Nesne oluşturma ifadeleri, sınıf türlerinin ve değer türlerinin yeni örneklerini oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1205">Object creation expressions are used to create new instances of class types and value types.</span></span>
*  <span data-ttu-id="2886a-1206">Dizi oluşturma ifadeleri dizi türlerinin yeni örneklerini oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1206">Array creation expressions are used to create new instances of array types.</span></span>
*  <span data-ttu-id="2886a-1207">Temsilci oluşturma ifadeleri temsilci türlerinin yeni örneklerini oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1207">Delegate creation expressions are used to create new instances of delegate types.</span></span>

<span data-ttu-id="2886a-1208">`new` işleci bir tür örneğinin oluşturulmasını, ancak dinamik bellek ayırmayı göstermez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1208">The `new` operator implies creation of an instance of a type, but does not necessarily imply dynamic allocation of memory.</span></span> <span data-ttu-id="2886a-1209">Özellikle, değer türlerinin örnekleri, bulundukları değişkenlerin ötesinde ek bellek gerektirmez ve değer türlerinin örneklerini oluşturmak için `new` kullanıldığında dinamik ayırmalar gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1209">In particular, instances of value types require no additional memory beyond the variables in which they reside, and no dynamic allocations occur when `new` is used to create instances of value types.</span></span>

#### <a name="object-creation-expressions"></a><span data-ttu-id="2886a-1210">Nesne oluşturma ifadeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-1210">Object creation expressions</span></span>

<span data-ttu-id="2886a-1211">Bir *object_creation_expression* *class_type* veya *value_type*yeni bir örneğini oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1211">An *object_creation_expression* is used to create a new instance of a *class_type* or a *value_type*.</span></span>

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;

object_or_collection_initializer
    : object_initializer
    | collection_initializer
    ;
```

<span data-ttu-id="2886a-1212">Object_creation_expression *türü* *class_type*, *value_type* veya *type_parameter*olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1212">The *type* of an *object_creation_expression* must be a *class_type*, a *value_type* or a *type_parameter*.</span></span> <span data-ttu-id="2886a-1213">*Tür* bir *class_type*`abstract` olamaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-1213">The *type* cannot be an `abstract` *class_type*.</span></span>

<span data-ttu-id="2886a-1214">İsteğe bağlı *argument_list* ([bağımsız değişken listeleri](expressions.md#argument-lists)) yalnızca *tür* bir *class_type* veya *struct_type*olduğunda izin verilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1214">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) is permitted only if the *type* is a *class_type* or a *struct_type*.</span></span>

<span data-ttu-id="2886a-1215">Nesne oluşturma ifadesi, Oluşturucu bağımsız değişken listesini atlayabilir ve kapsayan parantez, bir nesne Başlatıcısı veya koleksiyon başlatıcısı içerir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1215">An object creation expression can omit the constructor argument list and enclosing parentheses provided it includes an object initializer or collection initializer.</span></span> <span data-ttu-id="2886a-1216">Oluşturucu bağımsız değişken listesini atlama ve kapsayan parantezler boş bir bağımsız değişken listesi belirtmeye eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1216">Omitting the constructor argument list and enclosing parentheses is equivalent to specifying an empty argument list.</span></span>

<span data-ttu-id="2886a-1217">Bir nesne Başlatıcısı veya koleksiyon başlatıcısı içeren bir nesne oluşturma ifadesinin işlenmesi, ilk örnek oluşturucuyu işlemeyi ve ardından nesne Başlatıcısı ([nesne başlatıcıları](expressions.md#object-initializers)) veya koleksiyon başlatıcısı ([koleksiyon başlatıcıları](expressions.md#collection-initializers)) tarafından belirtilen üye veya öğe başlatmaları işlemeyi içerir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1217">Processing of an object creation expression that includes an object initializer or collection initializer consists of first processing the instance constructor and then processing the member or element initializations specified by the object initializer ([Object initializers](expressions.md#object-initializers)) or collection initializer ([Collection initializers](expressions.md#collection-initializers)).</span></span>

<span data-ttu-id="2886a-1218">İsteğe bağlı *argument_list* bağımsız değişkenlerden herhangi birinin derleme zamanı türü `dynamic` varsa *object_creation_expression* dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)) ve aşağıdaki kurallar, derleme zamanı türü `dynamic`olan *argument_list* bağımsız değişkenlerinin çalışma zamanı türü kullanılarak çalışma zamanında uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1218">If any of the arguments in the optional *argument_list* has the compile-time type `dynamic` then the *object_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) and the following rules are applied at run-time using the run-time type of those arguments of the *argument_list* that have the compile time type `dynamic`.</span></span> <span data-ttu-id="2886a-1219">Ancak, nesne oluşturma, [dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)konusunda açıklandığı gibi sınırlı bir derleme süresi denetimi yapar.</span><span class="sxs-lookup"><span data-stu-id="2886a-1219">However, the object creation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="2886a-1220">Form `new T(A)`*object_creation_expression* bağlama zamanı işleme; burada `T` *class_type* veya *value_type* , `A` ise isteğe bağlı bir *argument_list*, aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="2886a-1220">The binding-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is a *class_type* or a *value_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="2886a-1221">`T` bir *value_type* ve `A` yoksa:</span><span class="sxs-lookup"><span data-stu-id="2886a-1221">If `T` is a *value_type* and `A` is not present:</span></span>
    * <span data-ttu-id="2886a-1222">*Object_creation_expression* varsayılan Oluşturucu çağrıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1222">The *object_creation_expression* is a default constructor invocation.</span></span> <span data-ttu-id="2886a-1223">*Object_creation_expression* sonucu, `T`türünde bir değerdir, yani [System. ValueType türünde](types.md#the-systemvaluetype-type)tanımlanan `T` için varsayılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1223">The result of the *object_creation_expression* is a value of type `T`, namely the default value for `T` as defined in [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>
*   <span data-ttu-id="2886a-1224">Aksi takdirde, `T` bir *type_parameter* ve `A` yoksa:</span><span class="sxs-lookup"><span data-stu-id="2886a-1224">Otherwise, if `T` is a *type_parameter* and `A` is not present:</span></span>
    * <span data-ttu-id="2886a-1225">`T`için hiçbir değer türü kısıtlaması veya Oluşturucu kısıtlaması ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) belirtilmemişse, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1225">If no value type constraint or constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) has been specified for `T`, a binding-time error occurs.</span></span>
    * <span data-ttu-id="2886a-1226">*Object_creation_expression* sonucu, tür parametresinin bağlandığı çalışma zamanı türünün bir değeridir, yani bu türün varsayılan yapıcısını çağırma sonucu.</span><span class="sxs-lookup"><span data-stu-id="2886a-1226">The result of the *object_creation_expression* is a value of the run-time type that the type parameter has been bound to, namely the result of invoking the default constructor of that type.</span></span> <span data-ttu-id="2886a-1227">Çalışma zamanı türü bir başvuru türü veya değer türü olabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1227">The run-time type may be a reference type or a value type.</span></span>
*   <span data-ttu-id="2886a-1228">Aksi takdirde, `T` bir *class_type* veya *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="2886a-1228">Otherwise, if `T` is a *class_type* or a *struct_type*:</span></span>
    * <span data-ttu-id="2886a-1229">`T` bir `abstract` *class_type*ise, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1229">If `T` is an `abstract` *class_type*, a compile-time error occurs.</span></span>
    * <span data-ttu-id="2886a-1230">Çağrılacak örnek Oluşturucu [aşırı yükleme çözümünün](expressions.md#overload-resolution)aşırı yükleme çözümleme kuralları kullanılarak belirlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1230">The instance constructor to invoke is determined using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="2886a-1231">Aday örnek oluşturucular kümesi, `A` ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)) açısından geçerli olan `T` tarafından belirtilen tüm erişilebilir örnek oluşturuculardan oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1231">The set of candidate instance constructors consists of all accessible instance constructors declared in `T` which are applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="2886a-1232">Aday örnek oluşturucular kümesi boşsa veya tek bir en iyi örnek Oluşturucu tanımlanamıyorsa, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1232">If the set of candidate instance constructors is empty, or if a single best instance constructor cannot be identified, a binding-time error occurs.</span></span>
    * <span data-ttu-id="2886a-1233">*Object_creation_expression* sonucu, yukarıdaki adımda belirlenen örnek oluşturucuyu çağırarak üretilen değer olan `T`türünde bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1233">The result of the *object_creation_expression* is a value of type `T`, namely the value produced by invoking the instance constructor determined in the step above.</span></span>
*  <span data-ttu-id="2886a-1234">Aksi takdirde, *object_creation_expression* geçersizdir ve bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1234">Otherwise, the *object_creation_expression* is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="2886a-1235">*Object_creation_expression* dinamik olarak bağlı olsa da, derleme zamanı türü hala `T`.</span><span class="sxs-lookup"><span data-stu-id="2886a-1235">Even if the *object_creation_expression* is dynamically bound, the compile-time type is still `T`.</span></span>

<span data-ttu-id="2886a-1236">Form `new T(A)`*object_creation_expression* çalışma zamanı işleme; burada `T` *class_type* veya *struct_type* ve `A` isteğe bağlı bir *argument_list*, aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="2886a-1236">The run-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is *class_type* or a *struct_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="2886a-1237">`T` bir *class_type*:</span><span class="sxs-lookup"><span data-stu-id="2886a-1237">If `T` is a *class_type*:</span></span>
    * <span data-ttu-id="2886a-1238">`T` sınıfının yeni bir örneği ayrıldı.</span><span class="sxs-lookup"><span data-stu-id="2886a-1238">A new instance of class `T` is allocated.</span></span> <span data-ttu-id="2886a-1239">Yeni örneği ayırmak için yeterli kullanılabilir bellek yoksa bir `System.OutOfMemoryException` oluşturulur ve başka bir adım yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1239">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="2886a-1240">Yeni örneğin tüm alanları varsayılan değerlerine ([varsayılan değerler](variables.md#default-values)) başlatılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1240">All fields of the new instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
    * <span data-ttu-id="2886a-1241">Örnek Oluşturucu, işlev üye çağırma kurallarına göre çağrılır ([dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="2886a-1241">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="2886a-1242">Yeni ayrılmış örneğe bir başvuru, örnek oluşturucusuna otomatik olarak geçirilir ve örneğe `this`olarak bu oluşturucunun içinden erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1242">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>
*   <span data-ttu-id="2886a-1243">`T` bir *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="2886a-1243">If `T` is a *struct_type*:</span></span>
    * <span data-ttu-id="2886a-1244">`T` türünde bir örnek, geçici bir yerel değişken ayırarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1244">An instance of type `T` is created by allocating a temporary local variable.</span></span> <span data-ttu-id="2886a-1245">Bir *struct_type* örnek Oluşturucusu oluşturulan örneğin her bir alanına kesinlikle değer atamak gerektiğinden, geçici değişkenin başlatılması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1245">Since an instance constructor of a *struct_type* is required to definitely assign a value to each field of the instance being created, no initialization of the temporary variable is necessary.</span></span>
    * <span data-ttu-id="2886a-1246">Örnek Oluşturucu, işlev üye çağırma kurallarına göre çağrılır ([dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="2886a-1246">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="2886a-1247">Yeni ayrılmış örneğe bir başvuru, örnek oluşturucusuna otomatik olarak geçirilir ve örneğe `this`olarak bu oluşturucunun içinden erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1247">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>

#### <a name="object-initializers"></a><span data-ttu-id="2886a-1248">Nesne başlatıcıları</span><span class="sxs-lookup"><span data-stu-id="2886a-1248">Object initializers</span></span>

<span data-ttu-id="2886a-1249">Bir ***nesne Başlatıcısı*** , bir nesnenin sıfır veya daha fazla alanı, özelliği veya dizinli öğeleri için değerler belirtir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1249">An ***object initializer*** specifies values for zero or more fields, properties or indexed elements of an object.</span></span>

```antlr
object_initializer
    : '{' member_initializer_list? '}'
    | '{' member_initializer_list ',' '}'
    ;

member_initializer_list
    : member_initializer (',' member_initializer)*
    ;

member_initializer
    : initializer_target '=' initializer_value
    ;

initializer_target
    : identifier
    | '[' argument_list ']'
    ;

initializer_value
    : expression
    | object_or_collection_initializer
    ;
```

<span data-ttu-id="2886a-1250">Bir nesne Başlatıcısı, `{` ve `}` belirteçleri içine alınmış ve virgülle ayrılmış bir dizi üye başlatıcıdan oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1250">An object initializer consists of a sequence of member initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="2886a-1251">Her bir *member_initializer* başlatma için bir hedef belirler.</span><span class="sxs-lookup"><span data-stu-id="2886a-1251">Each *member_initializer* designates a target for the initialization.</span></span> <span data-ttu-id="2886a-1252">Bir *tanımlayıcı* , başlatılan nesnenin erişilebilir bir alanı veya özelliğini sağlamalıdır, ancak köşeli ayraç içine alınmış bir *argument_list* , başlatılan nesnede erişilebilir bir Dizin Oluşturucu için bağımsız değişkenler belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1252">An *identifier* must name an accessible field or property of the object being initialized, whereas an *argument_list* enclosed in square brackets must specify arguments for an accessible indexer on the object being initialized.</span></span> <span data-ttu-id="2886a-1253">Bir nesne başlatıcısının aynı alan veya özellik için birden fazla üye başlatıcısı içermesi hatadır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1253">It is an error for an object initializer to include more than one member initializer for the same field or property.</span></span>

<span data-ttu-id="2886a-1254">Her *initializer_target* ardından bir eşittir işareti ve bir ifade, bir nesne Başlatıcısı veya bir koleksiyon başlatıcısı gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1254">Each *initializer_target* is followed by an equals sign and either an expression, an object initializer or a collection initializer.</span></span> <span data-ttu-id="2886a-1255">Nesne Başlatıcısı içindeki ifadeler, başlatılan yeni oluşturulan nesneye başvuracaktır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1255">It is not possible for expressions within the object initializer to refer to the newly created object it is initializing.</span></span>

<span data-ttu-id="2886a-1256">Eşittir işaretinden sonra bir ifade belirten bir üye başlatıcısı, hedefle ([basit atama](expressions.md#simple-assignment)) aynı şekilde işlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1256">A member initializer that specifies an expression after the equals sign is processed in the same way as an assignment ([Simple assignment](expressions.md#simple-assignment)) to the target.</span></span>

<span data-ttu-id="2886a-1257">Eşittir işaretinden sonra bir nesne Başlatıcısı belirten bir üye başlatıcısı, ***iç içe geçmiş nesne Başlatıcısı***, yani gömülü bir nesnenin bir başlatması.</span><span class="sxs-lookup"><span data-stu-id="2886a-1257">A member initializer that specifies an object initializer after the equals sign is a ***nested object initializer***, i.e. an initialization of an embedded object.</span></span> <span data-ttu-id="2886a-1258">Alan veya özelliğe yeni bir değer atamak yerine, iç içe nesne başlatıcısındaki atamalar, alan veya özellik üyelerine atamalar olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1258">Instead of assigning a new value to the field or property, the assignments in the nested object initializer are treated as assignments to members of the field or property.</span></span> <span data-ttu-id="2886a-1259">İç içe nesne başlatıcıları bir değer türü olan özelliklere veya bir değer türüne sahip salt okuma alanlarına uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-1259">Nested object initializers cannot be applied to properties with a value type, or to read-only fields with a value type.</span></span>

<span data-ttu-id="2886a-1260">Eşittir işaretinden sonra bir koleksiyon başlatıcısı belirten üye başlatıcısı, gömülü bir koleksiyonun başlatılmasından.</span><span class="sxs-lookup"><span data-stu-id="2886a-1260">A member initializer that specifies a collection initializer after the equals sign is an initialization of an embedded collection.</span></span> <span data-ttu-id="2886a-1261">Hedef alana, özelliğe veya dizin oluşturucuya yeni bir koleksiyon atamak yerine, başlatıcıda verilen öğeler hedef tarafından başvurulan koleksiyona eklenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1261">Instead of assigning a new collection to the target field, property or indexer, the elements given in the initializer are added to the collection referenced by the target.</span></span> <span data-ttu-id="2886a-1262">Hedef, [koleksiyon başlatıcıları](expressions.md#collection-initializers)'nda belirtilen gereksinimleri karşılayan bir koleksiyon türünde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1262">The target must be of a collection type that satisfies the requirements specified in [Collection initializers](expressions.md#collection-initializers).</span></span>

<span data-ttu-id="2886a-1263">Bir dizin başlatıcısının bağımsız değişkenleri her zaman tam olarak bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1263">The arguments to an index initializer will always be evaluated exactly once.</span></span> <span data-ttu-id="2886a-1264">Bu nedenle, bağımsız değişkenler (örneğin, boş bir iç içe bir başlatıcı nedeniyle) hiç kullanılmasa bile, bunların yan etkileri için değerlendirilirler.</span><span class="sxs-lookup"><span data-stu-id="2886a-1264">Thus, even if the arguments end up never getting used (e.g. because of an empty nested initializer), they will be evaluated for their side effects.</span></span>

<span data-ttu-id="2886a-1265">Aşağıdaki sınıf iki koordinat içeren bir noktayı temsil eder:</span><span class="sxs-lookup"><span data-stu-id="2886a-1265">The following class represents a point with two coordinates:</span></span>
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

<span data-ttu-id="2886a-1266">`Point` bir örneği aşağıdaki şekilde oluşturulup başlatılabilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1266">An instance of `Point` can be created and initialized as follows:</span></span>
```csharp
Point a = new Point { X = 0, Y = 1 };
```
<span data-ttu-id="2886a-1267">aynı etkiye sahip</span><span class="sxs-lookup"><span data-stu-id="2886a-1267">which has the same effect as</span></span>
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
<span data-ttu-id="2886a-1268">`__a`, aksi durumda görünmeyen ve erişilemeyen geçici değişken.</span><span class="sxs-lookup"><span data-stu-id="2886a-1268">where `__a` is an otherwise invisible and inaccessible temporary variable.</span></span> <span data-ttu-id="2886a-1269">Aşağıdaki sınıf, iki noktadan oluşturulan bir dikdörtgeni temsil eder:</span><span class="sxs-lookup"><span data-stu-id="2886a-1269">The following class represents a rectangle created from two points:</span></span>
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

<span data-ttu-id="2886a-1270">`Rectangle` bir örneği aşağıdaki şekilde oluşturulup başlatılabilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1270">An instance of `Rectangle` can be created and initialized as follows:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
<span data-ttu-id="2886a-1271">aynı etkiye sahip</span><span class="sxs-lookup"><span data-stu-id="2886a-1271">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
Point __p1 = new Point();
__p1.X = 0;
__p1.Y = 1;
__r.P1 = __p1;
Point __p2 = new Point();
__p2.X = 2;
__p2.Y = 3;
__r.P2 = __p2; 
Rectangle r = __r;
```
<span data-ttu-id="2886a-1272">`__r`, `__p1` ve `__p2`, aksi durumda görünmez ve erişilemeyen geçici değişkenlerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1272">where `__r`, `__p1` and `__p2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="2886a-1273">`Rectangle`Oluşturucusu iki katıştırılmış `Point` örneği ayırırsa</span><span class="sxs-lookup"><span data-stu-id="2886a-1273">If `Rectangle`'s constructor allocates the two embedded `Point` instances</span></span>
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
<span data-ttu-id="2886a-1274">Aşağıdaki yapı, yeni örnekleri atamak yerine katıştırılmış `Point` örneklerini başlatmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1274">the following construct can be used to initialize the embedded `Point` instances instead of assigning new instances:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
<span data-ttu-id="2886a-1275">aynı etkiye sahip</span><span class="sxs-lookup"><span data-stu-id="2886a-1275">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

<span data-ttu-id="2886a-1276">C 'nin uygun bir tanımı verildiğinde aşağıdaki örnek:</span><span class="sxs-lookup"><span data-stu-id="2886a-1276">Given an appropriate definition of C, the following example:</span></span>
```csharp
var c = new C {
    x = true,
    y = { a = "Hello" },
    z = { 1, 2, 3 },
    ["x"] = 5,
    [0,0] = { "a", "b" },
    [1,2] = {}
};
```
<span data-ttu-id="2886a-1277">Bu atama dizisine eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1277">is equivalent to this series of assignments:</span></span>
```csharp
C __c = new C();
__c.x = true;
__c.y.a = "Hello";
__c.z.Add(1); 
__c.z.Add(2);
__c.z.Add(3);
string __i1 = "x";
__c[__i1] = 5;
int __i2 = 0, __i3 = 0;
__c[__i2,__i3].Add("a");
__c[__i2,__i3].Add("b");
int __i4 = 1, __i5 = 2;
var c = __c;
```
<span data-ttu-id="2886a-1278">Burada `__c`, vb., kaynak kodla görünmeyen ve erişilemeyen değişkenler oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1278">where `__c`, etc., are generated variables that are invisible and inaccessible to the source code.</span></span> <span data-ttu-id="2886a-1279">`[0,0]` bağımsız değişkenlerinin yalnızca bir kez değerlendirileceğini ve `[1,2]` bağımsız değişkenlerinin hiç kullanılmasa bile bir kez değerlendirildiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2886a-1279">Note that the arguments for `[0,0]` are evaluated only once, and the arguments for `[1,2]` are evaluated once even though they are never used.</span></span>

#### <a name="collection-initializers"></a><span data-ttu-id="2886a-1280">Koleksiyon başlatıcıları</span><span class="sxs-lookup"><span data-stu-id="2886a-1280">Collection initializers</span></span>

<span data-ttu-id="2886a-1281">Koleksiyon Başlatıcısı, bir koleksiyonun öğelerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1281">A collection initializer specifies the elements of a collection.</span></span>

```antlr
collection_initializer
    : '{' element_initializer_list '}'
    | '{' element_initializer_list ',' '}'
    ;

element_initializer_list
    : element_initializer (',' element_initializer)*
    ;

element_initializer
    : non_assignment_expression
    | '{' expression_list '}'
    ;

expression_list
    : expression (',' expression)*
    ;
```

<span data-ttu-id="2886a-1282">Bir koleksiyon başlatıcısı, `{` ve `}` belirteçleri içine alınmış ve virgülle ayrılmış bir dizi öğe başlatıcısından oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1282">A collection initializer consists of a sequence of element initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="2886a-1283">Her öğe başlatıcısı, başlatılmakta olan koleksiyon nesnesine eklenecek bir öğe belirtir ve `{` ve `}` belirteçleri ve virgülle ayrılmış bir ifade listesinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1283">Each element initializer specifies an element to be added to the collection object being initialized, and consists of a list of expressions enclosed by `{` and `}` tokens and separated by commas.</span></span>  <span data-ttu-id="2886a-1284">Tek ifadeyle bir öğe başlatıcısı küme ayracı olmadan yazılabilir, ancak üye başlatıcılarla belirsizliğe engel olmak için atama ifadesi olamaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-1284">A single-expression element initializer can be written without braces, but cannot then be an assignment expression, to avoid ambiguity with member initializers.</span></span> <span data-ttu-id="2886a-1285">*Non_assignment_expression* üretimi [ifadede](expressions.md#expression)tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1285">The *non_assignment_expression* production is defined in [Expression](expressions.md#expression).</span></span>

<span data-ttu-id="2886a-1286">Aşağıda, bir koleksiyon başlatıcısı içeren bir nesne oluşturma ifadesine örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1286">The following is an example of an object creation expression that includes a collection initializer:</span></span>
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

<span data-ttu-id="2886a-1287">Koleksiyon başlatıcısının uygulandığı koleksiyon nesnesi `System.Collections.IEnumerable` uygulayan bir türde veya bir derleme zamanı hatası meydana gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1287">The collection object to which a collection initializer is applied must be of a type that implements `System.Collections.IEnumerable` or a compile-time error occurs.</span></span> <span data-ttu-id="2886a-1288">Koleksiyon Başlatıcısı, belirtilen her öğe için, her bir çağrı için normal üye arama ve aşırı yükleme çözümlemesi uygulayarak, hedef nesnede, öğe başlatıcısı için bağımsız değişken listesi olarak bir `Add` yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1288">For each specified element in order, the collection initializer invokes an `Add` method on the target object with the expression list of the element initializer as argument list, applying normal member lookup and overload resolution for each invocation.</span></span> <span data-ttu-id="2886a-1289">Bu nedenle, koleksiyon nesnesinin her öğe başlatıcısı için `Add` ad ile ilgili bir örnek veya genişletme yöntemi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1289">Thus, the collection object must have an applicable instance or extension method with the name `Add` for each element initializer.</span></span>

<span data-ttu-id="2886a-1290">Aşağıdaki sınıf, bir kişinin adını ve telefon numaralarının listesini temsil eder:</span><span class="sxs-lookup"><span data-stu-id="2886a-1290">The following class represents a contact with a name and a list of phone numbers:</span></span>
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

<span data-ttu-id="2886a-1291">`List<Contact>` aşağıdaki şekilde oluşturulup başlatılabilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1291">A `List<Contact>` can be created and initialized as follows:</span></span>
```csharp
var contacts = new List<Contact> {
    new Contact {
        Name = "Chris Smith",
        PhoneNumbers = { "206-555-0101", "425-882-8080" }
    },
    new Contact {
        Name = "Bob Harris",
        PhoneNumbers = { "650-555-0199" }
    }
};
```
<span data-ttu-id="2886a-1292">aynı etkiye sahip</span><span class="sxs-lookup"><span data-stu-id="2886a-1292">which has the same effect as</span></span>
```csharp
var __clist = new List<Contact>();
Contact __c1 = new Contact();
__c1.Name = "Chris Smith";
__c1.PhoneNumbers.Add("206-555-0101");
__c1.PhoneNumbers.Add("425-882-8080");
__clist.Add(__c1);
Contact __c2 = new Contact();
__c2.Name = "Bob Harris";
__c2.PhoneNumbers.Add("650-555-0199");
__clist.Add(__c2);
var contacts = __clist;
```
<span data-ttu-id="2886a-1293">`__clist`, `__c1` ve `__c2`, aksi durumda görünmez ve erişilemeyen geçici değişkenlerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1293">where `__clist`, `__c1` and `__c2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="2886a-1294">Dizi oluşturma ifadeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-1294">Array creation expressions</span></span>

<span data-ttu-id="2886a-1295">Bir *array_creation_expression* *array_type*yeni bir örneğini oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1295">An *array_creation_expression* is used to create a new instance of an *array_type*.</span></span>

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

<span data-ttu-id="2886a-1296">İlk formun dizi oluşturma ifadesi, bağımsız ifadelerin her birini ifade listesinden silmenin sonucu olan türün bir dizi örneğini ayırır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1296">An array creation expression of the first form allocates an array instance of the type that results from deleting each of the individual expressions from the expression list.</span></span> <span data-ttu-id="2886a-1297">Örneğin, dizi oluşturma ifadesi `new int[10,20]` `int[,]`türünde bir dizi örneği üretir ve dizi oluşturma ifadesi `new int[10][,]` `int[][,]`türünde bir dizi üretir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1297">For example, the array creation expression `new int[10,20]` produces an array instance of type `int[,]`, and the array creation expression `new int[10][,]` produces an array of type `int[][,]`.</span></span> <span data-ttu-id="2886a-1298">İfade listesindeki her bir ifade `int`, `uint`, `long`veya `ulong`türünde ya da örtülü olarak bu türlerden bir veya daha fazlasına dönüştürülebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1298">Each expression in the expression list must be of type `int`, `uint`, `long`, or `ulong`, or implicitly convertible to one or more of these types.</span></span> <span data-ttu-id="2886a-1299">Her ifadenin değeri, yeni ayrılan dizi örneğindeki karşılık gelen boyutun uzunluğunu belirler.</span><span class="sxs-lookup"><span data-stu-id="2886a-1299">The value of each expression determines the length of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="2886a-1300">Bir dizi boyutunun uzunluğunun negatif olması gerektiğinden, ifade listesinde negatif bir değer olan *constant_expression* bir derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1300">Since the length of an array dimension must be nonnegative, it is a compile-time error to have a *constant_expression* with a negative value in the expression list.</span></span>

<span data-ttu-id="2886a-1301">Güvenli olmayan bir bağlam ([güvenli olmayan bağlamların](unsafe-code.md#unsafe-contexts)) dışında, dizilerin düzeni belirtilmemiş olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1301">Except in an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)), the layout of arrays is unspecified.</span></span>

<span data-ttu-id="2886a-1302">İlk formun dizi oluşturma ifadesi bir dizi başlatıcısı içeriyorsa, ifade listesindeki her bir ifade bir sabit olmalıdır ve ifade listesi tarafından belirtilen sıralama ve boyut uzunlukları dizi başlatıcısının ile aynı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1302">If an array creation expression of the first form includes an array initializer, each expression in the expression list must be a constant and the rank and dimension lengths specified by the expression list must match those of the array initializer.</span></span>

<span data-ttu-id="2886a-1303">İkinci veya üçüncü formun dizi oluşturma ifadesinde, belirtilen dizi türü veya sıralama belirticisinin sıralaması dizi başlatıcısının ile aynı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1303">In an array creation expression of the second or third form, the rank of the specified array type or rank specifier must match that of the array initializer.</span></span> <span data-ttu-id="2886a-1304">Tek tek boyut uzunlukları, dizi başlatıcısının karşılık gelen iç içe geçme düzeylerindeki öğelerin sayısından algılanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1304">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the array initializer.</span></span> <span data-ttu-id="2886a-1305">Bu nedenle, ifadesi</span><span class="sxs-lookup"><span data-stu-id="2886a-1305">Thus, the expression</span></span>
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
<span data-ttu-id="2886a-1306">tam olarak karşılık gelen</span><span class="sxs-lookup"><span data-stu-id="2886a-1306">exactly corresponds to</span></span>
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

<span data-ttu-id="2886a-1307">Üçüncü formun dizi oluşturma ifadesi, ***örtük olarak yazılmış dizi oluşturma ifadesi***olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1307">An array creation expression of the third form is referred to as an ***implicitly typed array creation expression***.</span></span> <span data-ttu-id="2886a-1308">Dizinin öğe türü açıkça verilmemiştir, ancak dizi başlatıcısındaki ifadelerin kümesinin en iyi ortak türü ([bir ifade kümesinin en iyi ortak türünü bulma](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) olarak belirlenir, ikinci forma benzerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1308">It is similar to the second form, except that the element type of the array is not explicitly given, but determined as the best common type ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) of the set of expressions in the array initializer.</span></span> <span data-ttu-id="2886a-1309">Çok boyutlu bir dizi için, yani *rank_specifier* en az bir virgül içerdiğinde, bu küme iç içe *array_initializer*s ' de bulunan tüm *ifade*öğeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1309">For a multidimensional array, i.e., one where the *rank_specifier* contains at least one comma, this set comprises all *expression*s found in nested *array_initializer*s.</span></span>

<span data-ttu-id="2886a-1310">Dizi başlatıcıları, [dizi başlatıcılarda](arrays.md#array-initializers)daha ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1310">Array initializers are described further in [Array initializers](arrays.md#array-initializers).</span></span>

<span data-ttu-id="2886a-1311">Dizi oluşturma ifadesinin değerlendirilme sonucu bir değer olarak sınıflandırıldığından, yani yeni ayrılmış dizi örneğine bir başvurudur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1311">The result of evaluating an array creation expression is classified as a value, namely a reference to the newly allocated array instance.</span></span> <span data-ttu-id="2886a-1312">Bir dizi oluşturma ifadesinin çalışma zamanı işleme aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="2886a-1312">The run-time processing of an array creation expression consists of the following steps:</span></span>

*  <span data-ttu-id="2886a-1313">*Expression_list* boyut uzunluğu ifadeleri, soldan sağa doğru sırayla değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1313">The dimension length expressions of the *expression_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="2886a-1314">Her bir ifadenin aşağıdaki türlerden birine yönelik olarak örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) gerçekleştirilir: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="2886a-1314">Following evaluation of each expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="2886a-1315">Bu listede örtük bir dönüştürmenin bulunduğu ilk tür seçilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1315">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="2886a-1316">Bir ifade veya sonraki örtük dönüştürme değerlendirmesi bir özel duruma neden oluyorsa, başka ifadeler değerlendirilmez ve başka bir adım yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1316">If evaluation of an expression or the subsequent implicit conversion causes an exception, then no further expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="2886a-1317">Boyut uzunlukları için hesaplanan değerler aşağıdaki şekilde onaylanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1317">The computed values for the dimension lengths are validated as follows.</span></span> <span data-ttu-id="2886a-1318">Bir veya daha fazla değer sıfırdan küçükse bir `System.OverflowException` oluşturulur ve başka adımlar yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1318">If one or more of the values are less than zero, a `System.OverflowException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="2886a-1319">Verilen boyut uzunluklarına sahip bir dizi örneği ayrıldı.</span><span class="sxs-lookup"><span data-stu-id="2886a-1319">An array instance with the given dimension lengths is allocated.</span></span> <span data-ttu-id="2886a-1320">Yeni örneği ayırmak için yeterli kullanılabilir bellek yoksa bir `System.OutOfMemoryException` oluşturulur ve başka bir adım yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1320">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="2886a-1321">Yeni dizi örneğinin tüm öğeleri varsayılan değerlerine ([varsayılan değerler](variables.md#default-values)) başlatılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1321">All elements of the new array instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
*  <span data-ttu-id="2886a-1322">Dizi oluşturma ifadesi bir dizi başlatıcısı içeriyorsa, dizi başlatıcısındaki her bir ifade değerlendirilir ve karşılık gelen dizi öğesine atanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1322">If the array creation expression contains an array initializer, then each expression in the array initializer is evaluated and assigned to its corresponding array element.</span></span> <span data-ttu-id="2886a-1323">Değerlendirmeler ve Atamalar, ifadelerin dizi başlatıcıda yazıldığı sırada gerçekleştirilir — diğer bir deyişle, öğeler, en sağdaki boyut ilk artarak Dizin sırasında başlatılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1323">The evaluations and assignments are performed in the order the expressions are written in the array initializer—in other words, elements are initialized in increasing index order, with the rightmost dimension increasing first.</span></span> <span data-ttu-id="2886a-1324">Belirli bir ifadenin değerlendirilmesi veya karşılık gelen dizi öğesine sonraki atama bir özel duruma neden oluyorsa, daha fazla öğe başlatılmaz (ve kalan öğelerin varsayılan değerleri olur).</span><span class="sxs-lookup"><span data-stu-id="2886a-1324">If evaluation of a given expression or the subsequent assignment to the corresponding array element causes an exception, then no further elements are initialized (and the remaining elements will thus have their default values).</span></span>

<span data-ttu-id="2886a-1325">Dizi oluşturma ifadesi bir dizi türünün öğeleriyle bir dizinin örneklemesine izin verir, ancak bu tür bir dizinin öğeleri el ile başlatılmış olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1325">An array creation expression permits instantiation of an array with elements of an array type, but the elements of such an array must be manually initialized.</span></span> <span data-ttu-id="2886a-1326">Örneğin, ekstresi</span><span class="sxs-lookup"><span data-stu-id="2886a-1326">For example, the statement</span></span>
```csharp
int[][] a = new int[100][];
```
<span data-ttu-id="2886a-1327">`int[]`türünde 100 öğe içeren tek boyutlu bir dizi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1327">creates a single-dimensional array with 100 elements of type `int[]`.</span></span> <span data-ttu-id="2886a-1328">Her öğenin başlangıç değeri `null`.</span><span class="sxs-lookup"><span data-stu-id="2886a-1328">The initial value of each element is `null`.</span></span> <span data-ttu-id="2886a-1329">Aynı dizi oluşturma ifadesinin alt dizileri ve deyimi de örneği oluşturması mümkün değildir</span><span class="sxs-lookup"><span data-stu-id="2886a-1329">It is not possible for the same array creation expression to also instantiate the sub-arrays, and the statement</span></span>
```csharp
int[][] a = new int[100][5];        // Error
```
<span data-ttu-id="2886a-1330">derleme zamanı hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1330">results in a compile-time error.</span></span> <span data-ttu-id="2886a-1331">Alt dizilerin örneklenmesi bunun yerine el ile gerçekleştirilmelidir, örneğin</span><span class="sxs-lookup"><span data-stu-id="2886a-1331">Instantiation of the sub-arrays must instead be performed manually, as in</span></span>
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

<span data-ttu-id="2886a-1332">Dizi dizisinde "dikdörtgen" bir şekil varsa, bu, alt diziler aynı uzunluktadır ve çok boyutlu bir dizi kullanmak daha verimlidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1332">When an array of arrays has a "rectangular" shape, that is when the sub-arrays are all of the same length, it is more efficient to use a multi-dimensional array.</span></span> <span data-ttu-id="2886a-1333">Yukarıdaki örnekte, dizi dizisinin örneklenmesi 101 nesne oluşturuyor — bir dış dizi ve 100 alt dizileri.</span><span class="sxs-lookup"><span data-stu-id="2886a-1333">In the example above, instantiation of the array of arrays creates 101 objects—one outer array and 100 sub-arrays.</span></span> <span data-ttu-id="2886a-1334">Buna karşılık,</span><span class="sxs-lookup"><span data-stu-id="2886a-1334">In contrast,</span></span>
```csharp
int[,] = new int[100, 5];
```
<span data-ttu-id="2886a-1335">yalnızca tek bir nesne, iki boyutlu bir dizi oluşturur ve ayırmayı tek bir ifadede gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1335">creates only a single object, a two-dimensional array, and accomplishes the allocation in a single statement.</span></span>

<span data-ttu-id="2886a-1336">Aşağıda örtük olarak yazılmış dizi oluşturma ifadelerinin örnekleri verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1336">The following are examples of implicitly typed array creation expressions:</span></span>
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

<span data-ttu-id="2886a-1337">Son ifade, hiçbir `int` ne de `string` örtülü olarak dönüştürülebilir olduğundan ve en iyi ortak tür olmadığından, derleme zamanı hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1337">The last expression causes a compile-time error because neither `int` nor `string` is implicitly convertible to the other, and so there is no best common type.</span></span> <span data-ttu-id="2886a-1338">Açıkça yazılmış bir dizi oluşturma ifadesi bu durumda kullanılmalıdır, örneğin `object[]`tür olarak belirtiliyor.</span><span class="sxs-lookup"><span data-stu-id="2886a-1338">An explicitly typed array creation expression must be used in this case, for example specifying the type to be `object[]`.</span></span> <span data-ttu-id="2886a-1339">Alternatif olarak, öğelerinden biri ortak bir temel türe alınabilir, bu da çıkarılan öğe türü olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1339">Alternatively, one of the elements can be cast to a common base type, which would then become the inferred element type.</span></span>

<span data-ttu-id="2886a-1340">Anonim olarak belirlenmiş veri yapıları oluşturmak için örtük olarak yazılmış dizi oluşturma ifadeleri anonim nesne başlatıcıları ([anonim nesne oluşturma ifadeleri](expressions.md#anonymous-object-creation-expressions)) ile birleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1340">Implicitly typed array creation expressions can be combined with anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) to create anonymously typed data structures.</span></span> <span data-ttu-id="2886a-1341">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2886a-1341">For example:</span></span>
```csharp
var contacts = new[] {
    new {
        Name = "Chris Smith",
        PhoneNumbers = new[] { "206-555-0101", "425-882-8080" }
    },
    new {
        Name = "Bob Harris",
        PhoneNumbers = new[] { "650-555-0199" }
    }
};
```

#### <a name="delegate-creation-expressions"></a><span data-ttu-id="2886a-1342">Temsilci oluşturma ifadeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-1342">Delegate creation expressions</span></span>

<span data-ttu-id="2886a-1343">Bir *delegate_creation_expression* *delegate_type*yeni bir örneğini oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1343">A *delegate_creation_expression* is used to create a new instance of a *delegate_type*.</span></span>

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

<span data-ttu-id="2886a-1344">Bir temsilci oluşturma ifadesinin bağımsız değişkeni bir yöntem grubu, anonim bir işlev veya `dynamic` ya da *delegate_type*derleme zamanı türünde bir değer olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1344">The argument of a delegate creation expression must be a method group, an anonymous function or a value of either the compile time type `dynamic` or a *delegate_type*.</span></span> <span data-ttu-id="2886a-1345">Bağımsız değişken bir yöntem grubu ise, yöntemini ve bir örnek yöntemi için, bir temsilci oluşturulacak nesneyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="2886a-1345">If the argument is a method group, it identifies the method and, for an instance method, the object for which to create a delegate.</span></span> <span data-ttu-id="2886a-1346">Bağımsız değişken anonim bir işlevse, temsilci hedefinin parametrelerini ve Yöntem gövdesini doğrudan tanımlar.</span><span class="sxs-lookup"><span data-stu-id="2886a-1346">If the argument is an anonymous function it directly defines the parameters and method body of the delegate target.</span></span> <span data-ttu-id="2886a-1347">Bağımsız değişken bir değer ise, bir kopyasını oluşturmak için bir temsilci örneğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="2886a-1347">If the argument is a value it identifies a delegate instance of which to create a copy.</span></span>

<span data-ttu-id="2886a-1348">*İfadede* derleme zamanı türü `dynamic`varsa, *delegate_creation_expression* dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)) ve aşağıdaki kurallar, *ifadenin*çalışma zamanı türü kullanılarak çalışma zamanında uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1348">If the *expression* has the compile-time type `dynamic`, the *delegate_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), and the rules below are applied at run-time using the run-time type of the *expression*.</span></span> <span data-ttu-id="2886a-1349">Aksi takdirde kurallar derleme zamanında uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1349">Otherwise the rules are applied at compile-time.</span></span>

<span data-ttu-id="2886a-1350">Form `new D(E)`*delegate_creation_expression* , `D` bir *delegate_type* ve `E` bir *ifade*olan bağlama zamanı işleme, aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="2886a-1350">The binding-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*  <span data-ttu-id="2886a-1351">`E` bir yöntem grubu ise, temsilci oluşturma ifadesi, bir yöntem grubu dönüştürmesi ([Yöntem grubu dönüştürmeleri](conversions.md#method-group-conversions)) ile `E` `D`aynı şekilde işlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1351">If `E` is a method group, the delegate creation expression is processed in the same way as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="2886a-1352">`E` anonim bir işlevse, temsilci oluşturma ifadesi, anonim işlev dönüştürme ([anonim işlev dönüştürmeleri](conversions.md#anonymous-function-conversions)) ile `E` `D`arasında aynı şekilde işlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1352">If `E` is an anonymous function, the delegate creation expression is processed in the same way as an anonymous function conversion ([Anonymous function conversions](conversions.md#anonymous-function-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="2886a-1353">`E` bir değer ise, `E` uyumlu olmalıdır ([temsilci bildirimleri](delegates.md#delegate-declarations)) `D`ve sonuç, `E`olarak aynı çağırma listesine başvuran `D` yeni oluşturulan bir temsilciye başvurudur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1353">If `E` is a value, `E` must be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`, and the result is a reference to a newly created delegate of type `D` that refers to the same invocation list as `E`.</span></span> <span data-ttu-id="2886a-1354">`E` `D`uyumlu değilse, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1354">If `E` is not compatible with `D`, a compile-time error occurs.</span></span>

<span data-ttu-id="2886a-1355">Form `new D(E)`*delegate_creation_expression* çalışma zamanı işleme; burada `D` *delegate_type* ve `E` bir *ifadedir*, aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="2886a-1355">The run-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*   <span data-ttu-id="2886a-1356">`E` bir yöntem grubu ise, temsilci oluşturma ifadesi bir yöntem grubu dönüştürmesi ([Yöntem grubu dönüştürmeleri](conversions.md#method-group-conversions)) olarak `E` `D`olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1356">If `E` is a method group, the delegate creation expression is evaluated as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*   <span data-ttu-id="2886a-1357">`E` anonim bir işlevse, temsilci oluşturma, `E` bir anonim işlev dönüştürmesi olarak değerlendirilir `D` ([anonim işlev dönüştürmeleri](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="2886a-1357">If `E` is an anonymous function, the delegate creation is evaluated as an anonymous function conversion from `E` to `D` ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>
*   <span data-ttu-id="2886a-1358">`E` bir *delegate_type*değeridir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1358">If `E` is a value of a *delegate_type*:</span></span>
    * <span data-ttu-id="2886a-1359">`E` değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1359">`E` is evaluated.</span></span> <span data-ttu-id="2886a-1360">Bu değerlendirme bir özel duruma neden olursa başka bir adım yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1360">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="2886a-1361">`E` değeri `null`, bir `System.NullReferenceException` oluşturulur ve başka bir adım yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1361">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="2886a-1362">`D` temsilci türünün yeni bir örneği ayrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1362">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="2886a-1363">Yeni örneği ayırmak için yeterli kullanılabilir bellek yoksa bir `System.OutOfMemoryException` oluşturulur ve başka bir adım yürütülmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1363">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="2886a-1364">Yeni temsilci örneği, `E`tarafından verilen temsilci örneğiyle aynı çağırma listesiyle başlatılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1364">The new delegate instance is initialized with the same invocation list as the delegate instance given by `E`.</span></span>

<span data-ttu-id="2886a-1365">Temsilcinin çağırma listesi, temsilcinin örneklendiği zaman belirlenir ve sonra temsilcinin tüm ömrü boyunca sabit kalır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1365">The invocation list of a delegate is determined when the delegate is instantiated and then remains constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="2886a-1366">Diğer bir deyişle, oluşturulduktan sonra bir temsilcinin hedef çağrılabilir varlıklarını değiştirmek mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1366">In other words, it is not possible to change the target callable entities of a delegate once it has been created.</span></span> <span data-ttu-id="2886a-1367">İki temsilci birleştirildiğinde veya bir diğeri ([temsilci bildirimlerinden](delegates.md#delegate-declarations)) kaldırıldığında, yeni bir temsilci oluşur; Mevcut temsilcinin içeriği değiştirilmedi.</span><span class="sxs-lookup"><span data-stu-id="2886a-1367">When two delegates are combined or one is removed from another ([Delegate declarations](delegates.md#delegate-declarations)), a new delegate results; no existing delegate has its contents changed.</span></span>

<span data-ttu-id="2886a-1368">Bir özellik, Dizin Oluşturucu, Kullanıcı tanımlı işleç, örnek Oluşturucu, yıkıcı veya statik oluşturucuya başvuran bir temsilci oluşturmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1368">It is not possible to create a delegate that refers to a property, indexer, user-defined operator, instance constructor, destructor, or static constructor.</span></span>

<span data-ttu-id="2886a-1369">Yukarıda açıklandığı gibi, bir yöntem grubundan bir temsilci oluşturulduğunda, temsilci 'nin biçimsel parametre listesi ve dönüş türü, ne tür aşırı yüklenmiş yöntemlerin ekleneceğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="2886a-1369">As described above, when a delegate is created from a method group, the formal parameter list and return type of the delegate determine which of the overloaded methods to select.</span></span> <span data-ttu-id="2886a-1370">Örnekte</span><span class="sxs-lookup"><span data-stu-id="2886a-1370">In the example</span></span>
```csharp
delegate double DoubleFunc(double x);

class A
{
    DoubleFunc f = new DoubleFunc(Square);

    static float Square(float x) {
        return x * x;
    }

    static double Square(double x) {
        return x * x;
    }
}
```
<span data-ttu-id="2886a-1371">Bu yöntem, biçimsel parametre listesi ve `DoubleFunc`dönüş türü ile tam olarak eşleştiğinden, `A.f` alanı ikinci `Square` yöntemine başvuran bir temsilciyle başlatılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1371">the `A.f` field is initialized with a delegate that refers to the second `Square` method because that method exactly matches the formal parameter list and return type of `DoubleFunc`.</span></span> <span data-ttu-id="2886a-1372">İkinci `Square` yöntemi yoktu, derleme zamanı hatası oluşmuş olabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1372">Had the second `Square` method not been present, a compile-time error would have occurred.</span></span>

#### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="2886a-1373">Anonim nesne oluşturma ifadeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-1373">Anonymous object creation expressions</span></span>

<span data-ttu-id="2886a-1374">Anonim türdeki bir nesne oluşturmak için bir *anonymous_object_creation_expression* kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1374">An *anonymous_object_creation_expression* is used to create an object of an anonymous type.</span></span>

```antlr
anonymous_object_creation_expression
    : 'new' anonymous_object_initializer
    ;

anonymous_object_initializer
    : '{' member_declarator_list? '}'
    | '{' member_declarator_list ',' '}'
    ;

member_declarator_list
    : member_declarator (',' member_declarator)*
    ;

member_declarator
    : simple_name
    | member_access
    | base_access
    | null_conditional_member_access
    | identifier '=' expression
    ;
```

<span data-ttu-id="2886a-1375">Anonim bir nesne Başlatıcısı, anonim bir tür bildirir ve bu türün bir örneğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1375">An anonymous object initializer declares an anonymous type and returns an instance of that type.</span></span> <span data-ttu-id="2886a-1376">Anonim tür, doğrudan `object`devralan isimsiz bir sınıf türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1376">An anonymous type is a nameless class type that inherits directly from `object`.</span></span> <span data-ttu-id="2886a-1377">Anonim bir türün üyeleri, türünün bir örneğini oluşturmak için kullanılan anonim nesne başlatıcıdan çıkarılan salt okunurdur salt yazılır özelliklerden oluşan bir dizidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1377">The members of an anonymous type are a sequence of read-only properties inferred from the anonymous object initializer used to create an instance of the type.</span></span> <span data-ttu-id="2886a-1378">Özellikle, formun anonim nesne Başlatıcısı</span><span class="sxs-lookup"><span data-stu-id="2886a-1378">Specifically, an anonymous object initializer of the form</span></span>
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
<span data-ttu-id="2886a-1379">formun anonim türünü bildirir</span><span class="sxs-lookup"><span data-stu-id="2886a-1379">declares an anonymous type of the form</span></span>
```csharp
class __Anonymous1
{
    private readonly T1 f1;
    private readonly T2 f2;
    ...
    private readonly Tn fn;

    public __Anonymous1(T1 a1, T2 a2, ..., Tn an) {
        f1 = a1;
        f2 = a2;
        ...
        fn = an;
    }

    public T1 p1 { get { return f1; } }
    public T2 p2 { get { return f2; } }
    ...
    public Tn pn { get { return fn; } }

    public override bool Equals(object __o) { ... }
    public override int GetHashCode() { ... }
}
```
<span data-ttu-id="2886a-1380">Her `Tx` karşılık gelen ifade `ex`türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1380">where each `Tx` is the type of the corresponding expression `ex`.</span></span> <span data-ttu-id="2886a-1381">Bir *member_declarator* kullanılan ifadenin türü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1381">The expression used in a *member_declarator* must have a type.</span></span> <span data-ttu-id="2886a-1382">Bu nedenle, *member_declarator* bir ifadenin null veya anonim bir işlev olması için derleme zamanı hatası vardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1382">Thus, it is a compile-time error for an expression in a *member_declarator* to be null or an anonymous function.</span></span> <span data-ttu-id="2886a-1383">Ayrıca, ifadenin güvenli olmayan bir türü olması için derleme zamanı hatası da vardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1383">It is also a compile-time error for the expression to have an unsafe type.</span></span>

<span data-ttu-id="2886a-1384">Anonim bir türün adı ve `Equals` yöntemi parametresi, derleyici tarafından otomatik olarak oluşturulur ve program metninde başvurulamaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-1384">The names of an anonymous type and of the parameter to its `Equals` method are automatically generated by the compiler and cannot be referenced in program text.</span></span>

<span data-ttu-id="2886a-1385">Aynı programda aynı ada ve derleme zamanı türlerine sahip bir özellikler dizisini belirten iki anonim nesne başlatıcıları aynı şekilde aynı anonim türde örnekler oluşturacaktır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1385">Within the same program, two anonymous object initializers that specify a sequence of properties of the same names and compile-time types in the same order will produce instances of the same anonymous type.</span></span>

<span data-ttu-id="2886a-1386">Örnekte</span><span class="sxs-lookup"><span data-stu-id="2886a-1386">In the example</span></span>
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
<span data-ttu-id="2886a-1387">`p1` ve `p2` aynı anonim türde olduğundan, son satırdaki atamaya izin verilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1387">the assignment on the last line is permitted because `p1` and `p2` are of the same anonymous type.</span></span>

<span data-ttu-id="2886a-1388">Anonim türlerdeki `Equals` ve `GetHashcode` yöntemleri `object`devralınan yöntemleri geçersiz kılar ve özelliklerin `Equals` ve `GetHashcode` bakımından tanımlanmıştır. böylece, aynı anonim türdeki iki örnek yalnızca tüm özellikleri eşitse eşit olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1388">The `Equals` and `GetHashcode` methods on anonymous types override the methods inherited from `object`, and are defined in terms of the `Equals` and `GetHashcode` of the properties, so that two instances of the same anonymous type are equal if and only if all their properties are equal.</span></span>

<span data-ttu-id="2886a-1389">Bir üye bildirimci basit bir ad ([tür çıkarımı](expressions.md#type-inference)), bir üye erişimi ([dinamik aşırı yükleme çözümlemesi için derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), temel erişim ([temel erişim](expressions.md#base-access)) veya null koşullu üye erişimi ([projeksiyon başlatıcıları olarak null Koşullu ifadeler](expressions.md#null-conditional-expressions-as-projection-initializers)) için kısaltılabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1389">A member declarator can be abbreviated to a simple name ([Type inference](expressions.md#type-inference)), a member access ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), a base access ([Base access](expressions.md#base-access)) or a null-conditional member access ([Null-conditional expressions as projection initializers](expressions.md#null-conditional-expressions-as-projection-initializers)).</span></span> <span data-ttu-id="2886a-1390">Bu, ***projeksiyon başlatıcısı*** olarak adlandırılır ve aynı ada sahip bir özelliğe bir bildirim ve atama için toplu bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1390">This is called a ***projection initializer*** and is shorthand for a declaration of and assignment to a property with the same name.</span></span> <span data-ttu-id="2886a-1391">Özellikle, formların üye bildirimcileri</span><span class="sxs-lookup"><span data-stu-id="2886a-1391">Specifically, member declarators of the forms</span></span>
```csharp
identifier
expr.identifier
```
<span data-ttu-id="2886a-1392">sırasıyla aşağıdaki gibi tam olarak eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1392">are precisely equivalent to the following, respectively:</span></span>
```csharp
identifier = identifier
identifier = expr.identifier
```

<span data-ttu-id="2886a-1393">Bu nedenle, bir projeksiyon başlatıcısında *tanımlayıcı* hem değeri hem de değerin atandığı alanı veya özelliği seçer.</span><span class="sxs-lookup"><span data-stu-id="2886a-1393">Thus, in a projection initializer the *identifier* selects both the value and the field or property to which the value is assigned.</span></span> <span data-ttu-id="2886a-1394">Bir projeksiyon başlatıcısı projesi yalnızca bir değer değil, aynı zamanda değerin adı değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1394">Intuitively, a projection initializer projects not just a value, but also the name of the value.</span></span>

### <a name="the-typeof-operator"></a><span data-ttu-id="2886a-1395">Typeof işleci</span><span class="sxs-lookup"><span data-stu-id="2886a-1395">The typeof operator</span></span>

<span data-ttu-id="2886a-1396">`typeof` işleci, bir türün `System.Type` nesnesini almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1396">The `typeof` operator is used to obtain the `System.Type` object for a type.</span></span>

```antlr
typeof_expression
    : 'typeof' '(' type ')'
    | 'typeof' '(' unbound_type_name ')'
    | 'typeof' '(' 'void' ')'
    ;

unbound_type_name
    : identifier generic_dimension_specifier?
    | identifier '::' identifier generic_dimension_specifier?
    | unbound_type_name '.' identifier generic_dimension_specifier?
    ;

generic_dimension_specifier
    : '<' comma* '>'
    ;

comma
    : ','
    ;
```

<span data-ttu-id="2886a-1397">*Typeof_expression* ilk formu, bir `typeof` anahtar sözcüğünden ve sonra parantez içine alınmış bir *tür*ile oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1397">The first form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *type*.</span></span> <span data-ttu-id="2886a-1398">Bu formun bir ifadesinin sonucu, belirtilen tür için `System.Type` nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1398">The result of an expression of this form is the `System.Type` object for the indicated type.</span></span> <span data-ttu-id="2886a-1399">Verilen herhangi bir tür için yalnızca bir `System.Type` nesnesi vardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1399">There is only one `System.Type` object for any given type.</span></span> <span data-ttu-id="2886a-1400">Bu, bir tür `T`için `typeof(T) == typeof(T)` her zaman doğru olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1400">This means that for a type `T`, `typeof(T) == typeof(T)` is always true.</span></span> <span data-ttu-id="2886a-1401">*Tür* `dynamic`olamaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-1401">The *type* cannot be `dynamic`.</span></span>

<span data-ttu-id="2886a-1402">*Typeof_expression* ikinci biçimi `typeof` bir anahtar sözcükten ve sonra parantez içine alınmış *unbound_type_name*oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1402">The second form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *unbound_type_name*.</span></span> <span data-ttu-id="2886a-1403">Bir *unbound_type_name* bir *type_name* ([ad alanı ve tür adları](basic-concepts.md#namespace-and-type-names)) çok benzerdir, çünkü bir *unbound_type_name* *type_name bir* *type_argument_list içeren* *generic_dimension_specifier*s 'yi içerir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1403">An *unbound_type_name* is very similar to a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) except that an *unbound_type_name* contains *generic_dimension_specifier*s where a *type_name* contains *type_argument_list*s.</span></span> <span data-ttu-id="2886a-1404">Bir *typeof_expression* işleneni hem *unbound_type_name* hem de *type_name*her ikisi de bir *generic_dimension_specifier* veya *type_argument_list*içerdiğinde, belirteçlerin sırası bir *type_name*olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1404">When the operand of a *typeof_expression* is a sequence of tokens that satisfies the grammars of both *unbound_type_name* and *type_name*, namely when it contains neither a *generic_dimension_specifier* nor a *type_argument_list*, the sequence of tokens is considered to be a *type_name*.</span></span> <span data-ttu-id="2886a-1405">Bir *unbound_type_name* anlamı aşağıdaki şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1405">The meaning of an *unbound_type_name* is determined as follows:</span></span>

*  <span data-ttu-id="2886a-1406">Her bir *generic_dimension_specifier* aynı sayı sayısına sahip bir *type_argument_list* değiştirip her bir *type_argument*gibi anahtar sözcüğü `object`, belirteç sırasını *type_name* dönüştürün.</span><span class="sxs-lookup"><span data-stu-id="2886a-1406">Convert the sequence of tokens to a *type_name* by replacing each *generic_dimension_specifier* with a *type_argument_list* having the same number of commas and the keyword `object` as each *type_argument*.</span></span>
*  <span data-ttu-id="2886a-1407">Tüm tür parametresi kısıtlamalarını yoksayarak elde edilen *type_name*değerlendirin.</span><span class="sxs-lookup"><span data-stu-id="2886a-1407">Evaluate the resulting *type_name*, while ignoring all type parameter constraints.</span></span>
*  <span data-ttu-id="2886a-1408">*Unbound_type_name* , ortaya çıkan oluşturulan türle ilişkili ilişkisiz genel tür ([bağlı ve ilişkisiz türler](types.md#bound-and-unbound-types)) ile çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1408">The *unbound_type_name* resolves to the unbound generic type associated with the resulting constructed type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span>

<span data-ttu-id="2886a-1409">*Typeof_expression* sonucu, sonuçta elde edilen ilişkisiz genel tür için `System.Type` nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1409">The result of the *typeof_expression* is the `System.Type` object for the resulting unbound generic type.</span></span>

<span data-ttu-id="2886a-1410">Üçüncü *typeof_expression* biçimi, bir `typeof` anahtar sözcüğünden ve sonra parantez içine alınmış `void` anahtar kelimesiyle oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1410">The third form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized `void` keyword.</span></span> <span data-ttu-id="2886a-1411">Bu formun bir ifadesinin sonucu, bir türün yokluğunu temsil eden `System.Type` nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1411">The result of an expression of this form is the `System.Type` object that represents the absence of a type.</span></span> <span data-ttu-id="2886a-1412">`typeof(void)` tarafından döndürülen tür nesnesi herhangi bir tür için döndürülen tür nesnesinden farklıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1412">The type object returned by `typeof(void)` is distinct from the type object returned for any type.</span></span> <span data-ttu-id="2886a-1413">Bu özel tür nesnesi, dildeki yöntemlere izin veren sınıf kitaplıklarında yararlıdır; burada, bu yöntemlerin bir `System.Type`örneğiyle void yöntemler de dahil olmak üzere herhangi bir yöntemin dönüş türünü temsil etmek için bir yol olmasını ister.</span><span class="sxs-lookup"><span data-stu-id="2886a-1413">This special type object is useful in class libraries that allow reflection onto methods in the language, where those methods wish to have a way to represent the return type of any method, including void methods, with an instance of `System.Type`.</span></span>

<span data-ttu-id="2886a-1414">`typeof` işleci bir tür parametresinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1414">The `typeof` operator can be used on a type parameter.</span></span> <span data-ttu-id="2886a-1415">Sonuç, tür parametresine bağlanan çalışma zamanı türünün `System.Type` nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1415">The result is the `System.Type` object for the run-time type that was bound to the type parameter.</span></span> <span data-ttu-id="2886a-1416">`typeof` işleci, oluşturulmuş bir tür veya ilişkisiz genel tür ([bağlı ve ilişkisiz türler](types.md#bound-and-unbound-types)) üzerinde de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1416">The `typeof` operator can also be used on a constructed type or an unbound generic type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span> <span data-ttu-id="2886a-1417">İlişkisiz genel tür için `System.Type` nesnesi, örnek türünün `System.Type` nesnesiyle aynı değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1417">The `System.Type` object for an unbound generic type is not the same as the `System.Type` object of the instance type.</span></span> <span data-ttu-id="2886a-1418">Örnek türü her zaman çalışma zamanında bir kapalı oluşturulmuş türdür, `System.Type` nesnesi kullanımda olan çalışma zamanı türü bağımsız değişkenlerine bağlıdır, ancak ilişkisiz genel türün tür bağımsız değişkenleri yoktur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1418">The instance type is always a closed constructed type at run-time so its `System.Type` object depends on the run-time type arguments in use, while the unbound generic type has no type arguments.</span></span>

<span data-ttu-id="2886a-1419">Örnek</span><span class="sxs-lookup"><span data-stu-id="2886a-1419">The example</span></span>
```csharp
using System;

class X<T>
{
    public static void PrintTypes() {
        Type[] t = {
            typeof(int),
            typeof(System.Int32),
            typeof(string),
            typeof(double[]),
            typeof(void),
            typeof(T),
            typeof(X<T>),
            typeof(X<X<T>>),
            typeof(X<>)
        };
        for (int i = 0; i < t.Length; i++) {
            Console.WriteLine(t[i]);
        }
    }
}

class Test
{
    static void Main() {
        X<int>.PrintTypes();
    }
}
```
<span data-ttu-id="2886a-1420">Aşağıdaki çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1420">produces the following output:</span></span>
```console
System.Int32
System.Int32
System.String
System.Double[]
System.Void
System.Int32
X`1[System.Int32]
X`1[X`1[System.Int32]]
X`1[T]
```

<span data-ttu-id="2886a-1421">`int` ve `System.Int32` aynı türde olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2886a-1421">Note that `int` and `System.Int32` are the same type.</span></span>

<span data-ttu-id="2886a-1422">Ayrıca `typeof(X<>)` sonucunun tür bağımsız değişkenine dayanmadığını, ancak `typeof(X<T>)` sonucunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2886a-1422">Also note that the result of `typeof(X<>)` does not depend on the type argument but the result of `typeof(X<T>)` does.</span></span>

### <a name="the-checked-and-unchecked-operators"></a><span data-ttu-id="2886a-1423">Checked ve unchecked işleçleri</span><span class="sxs-lookup"><span data-stu-id="2886a-1423">The checked and unchecked operators</span></span>

<span data-ttu-id="2886a-1424">`checked` ve `unchecked` işleçleri, tam sayı türü aritmetik işlemler ve dönüştürmeler için ***taşma denetimi bağlamını*** denetlemek üzere kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1424">The `checked` and `unchecked` operators are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

<span data-ttu-id="2886a-1425">`checked` işleci içerilen ifadeyi işaretlenmiş bir bağlamda değerlendirir ve `unchecked` işleci içerilen ifadeyi işaretlenmemiş bir bağlamda değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1425">The `checked` operator evaluates the contained expression in a checked context, and the `unchecked` operator evaluates the contained expression in an unchecked context.</span></span> <span data-ttu-id="2886a-1426">Bir *checked_expression* veya *unchecked_expression* tam olarak bir *parenthesized_expression* ([parantez içine alınmış ifadelerde](expressions.md#parenthesized-expressions)) karşılık gelir, ancak içerilen ifade verilen taşma Denetim bağlamında değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1426">A *checked_expression* or *unchecked_expression* corresponds exactly to a *parenthesized_expression* ([Parenthesized expressions](expressions.md#parenthesized-expressions)), except that the contained expression is evaluated in the given overflow checking context.</span></span>

<span data-ttu-id="2886a-1427">Taşma Denetim bağlamı, `checked` ve `unchecked` deyimleri ([Checked ve unchecked deyimleri](statements.md#the-checked-and-unchecked-statements)) aracılığıyla da denetlenebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1427">The overflow checking context can also be controlled through the `checked` and `unchecked` statements ([The checked and unchecked statements](statements.md#the-checked-and-unchecked-statements)).</span></span>

<span data-ttu-id="2886a-1428">Aşağıdaki işlemler, `checked` ve `unchecked` işleçleri ve deyimleri tarafından oluşturulan taşma denetimi bağlamından etkilenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1428">The following operations are affected by the overflow checking context established by the `checked` and `unchecked` operators and statements:</span></span>

*  <span data-ttu-id="2886a-1429">Ön tanımlı `++` ve `--` Birli İşleçler ([sonek artırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators) ve [ön ek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)), işlenen bir integral türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1429">The predefined `++` and `--` unary operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="2886a-1430">Ön tanımlı `-` birli işleci ([birli eksi işleci](expressions.md#unary-minus-operator)), işlenen bir integral türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1430">The predefined `-` unary operator ([Unary minus operator](expressions.md#unary-minus-operator)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="2886a-1431">Ön tanımlı `+`, `-`, `*`ve `/` ikili işleçler ([Aritmetik işleçler](expressions.md#arithmetic-operators)), her iki işlenen de İntegral türlerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1431">The predefined `+`, `-`, `*`, and `/` binary operators ([Arithmetic operators](expressions.md#arithmetic-operators)), when both operands are of integral types.</span></span>
*  <span data-ttu-id="2886a-1432">Tek bir integral türünden başka bir integral türüne veya `float` ya da `double` bir integral türüne açık sayısal dönüştürmeler ([Açık sayısal dönüştürmeler](conversions.md#explicit-numeric-conversions)).</span><span class="sxs-lookup"><span data-stu-id="2886a-1432">Explicit numeric conversions ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from one integral type to another integral type, or from `float` or `double` to an integral type.</span></span>

<span data-ttu-id="2886a-1433">Yukarıdaki işlemlerden biri hedef türünde temsil etmek için çok büyük bir sonuç üretdüğünde, işlemin gerçekleştirildiği bağlam, sonuçta elde edilen davranışı denetler:</span><span class="sxs-lookup"><span data-stu-id="2886a-1433">When one of the above operations produce a result that is too large to represent in the destination type, the context in which the operation is performed controls the resulting behavior:</span></span>

*  <span data-ttu-id="2886a-1434">`checked` bağlamında, işlem sabit bir ifadesiyse ([sabit ifadeler](expressions.md#constant-expressions)), bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1434">In a `checked` context, if the operation is a constant expression ([Constant expressions](expressions.md#constant-expressions)), a compile-time error occurs.</span></span> <span data-ttu-id="2886a-1435">Aksi takdirde, işlem çalışma zamanında gerçekleştirildiğinde bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1435">Otherwise, when the operation is performed at run-time, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="2886a-1436">`unchecked` bağlamında, sonuç, hedef türüne uymayan yüksek sıralı bitleri atarak atılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1436">In an `unchecked` context, the result is truncated by discarding any high-order bits that do not fit in the destination type.</span></span>

<span data-ttu-id="2886a-1437">Herhangi bir `checked` veya `unchecked` işleci ya da deyimler tarafından alınmamış sabit olmayan ifadeler (çalışma zamanında değerlendirilen ifadeler) için, `checked` değerlendirmesi için dış faktörler (derleyici anahtarları ve yürütme ortamı yapılandırması gibi) çağrısı olmadığı sürece varsayılan taşma denetimi bağlamı `unchecked`.</span><span class="sxs-lookup"><span data-stu-id="2886a-1437">For non-constant expressions (expressions that are evaluated at run-time) that are not enclosed by any `checked` or `unchecked` operators or statements, the default overflow checking context is `unchecked` unless external factors (such as compiler switches and execution environment configuration) call for `checked` evaluation.</span></span>

<span data-ttu-id="2886a-1438">Sabit ifadeler (derleme zamanında tam olarak değerlendirilebilecek ifadeler) için, varsayılan taşma denetimi bağlamı her zaman `checked`.</span><span class="sxs-lookup"><span data-stu-id="2886a-1438">For constant expressions (expressions that can be fully evaluated at compile-time), the default overflow checking context is always `checked`.</span></span> <span data-ttu-id="2886a-1439">Sabit bir ifade bir `unchecked` bağlamına açıkça yerleştirilmediği için, ifadenin derleme zamanı değerlendirmesi sırasında oluşan taşmalar her zaman derleme zamanı hatalarına neden olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1439">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur during the compile-time evaluation of the expression always cause compile-time errors.</span></span>

<span data-ttu-id="2886a-1440">Anonim işlevin gövdesi, anonim işlevin gerçekleştiği `checked` veya `unchecked` bağlamlarından etkilenmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1440">The body of an anonymous function is not affected by `checked` or `unchecked` contexts in which the anonymous function occurs.</span></span>

<span data-ttu-id="2886a-1441">Örnekte</span><span class="sxs-lookup"><span data-stu-id="2886a-1441">In the example</span></span>
```csharp
class Test
{
    static readonly int x = 1000000;
    static readonly int y = 1000000;

    static int F() {
        return checked(x * y);      // Throws OverflowException
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Depends on default
    }
}
```
<span data-ttu-id="2886a-1442">hiçbir deyimin hiçbiri derleme zamanında değerlendirilemediğinden, derleme zamanı hataları bildirilmemiştir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1442">no compile-time errors are reported since neither of the expressions can be evaluated at compile-time.</span></span> <span data-ttu-id="2886a-1443">Çalışma zamanında `F` yöntemi bir `System.OverflowException`oluşturur ve `G` yöntemi-727379968 döndürür (Aralık dışı sonucun alt 32 bitleridir).</span><span class="sxs-lookup"><span data-stu-id="2886a-1443">At run-time, the `F` method throws a `System.OverflowException`, and the `G` method returns -727379968 (the lower 32 bits of the out-of-range result).</span></span> <span data-ttu-id="2886a-1444">`H` yönteminin davranışı, derleme için varsayılan taşma denetimi bağlamına bağlıdır, ancak `F` veya `G`ile aynı olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1444">The behavior of the `H` method depends on the default overflow checking context for the compilation, but it is either the same as `F` or the same as `G`.</span></span>

<span data-ttu-id="2886a-1445">Örnekte</span><span class="sxs-lookup"><span data-stu-id="2886a-1445">In the example</span></span>
```csharp
class Test
{
    const int x = 1000000;
    const int y = 1000000;

    static int F() {
        return checked(x * y);      // Compile error, overflow
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Compile error, overflow
    }
}
```
<span data-ttu-id="2886a-1446">`F` sabit ifadeler değerlendirilirken meydana gelen taşmalar ve `H` ifadeler bir `checked` bağlamında değerlendirildiğinden derleme zamanı hatalarının raporlanmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1446">the overflows that occur when evaluating the constant expressions in `F` and `H` cause compile-time errors to be reported because the expressions are evaluated in a `checked` context.</span></span> <span data-ttu-id="2886a-1447">`G`sabit ifade değerlendirilirken bir taşma da oluşur, ancak değerlendirme bir `unchecked` bağlamında gerçekleşirken taşma bildirilmedi.</span><span class="sxs-lookup"><span data-stu-id="2886a-1447">An overflow also occurs when evaluating the constant expression in `G`, but since the evaluation takes place in an `unchecked` context, the overflow is not reported.</span></span>

<span data-ttu-id="2886a-1448">`checked` ve `unchecked` işleçleri yalnızca "`(`" ve "`)`" belirteçlerinde bulunan metin içeriğini eklemek bu işlemler için taşma denetimi bağlamını etkiler.</span><span class="sxs-lookup"><span data-stu-id="2886a-1448">The `checked` and `unchecked` operators only affect the overflow checking context for those operations that are textually contained within the "`(`" and "`)`" tokens.</span></span> <span data-ttu-id="2886a-1449">İşleçler, içerilen ifadenin hesaplanmasının sonucu olarak çağrılan işlev üyelerini etkilemez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1449">The operators have no effect on function members that are invoked as a result of evaluating the contained expression.</span></span> <span data-ttu-id="2886a-1450">Örnekte</span><span class="sxs-lookup"><span data-stu-id="2886a-1450">In the example</span></span>
```csharp
class Test
{
    static int Multiply(int x, int y) {
        return x * y;
    }

    static int F() {
        return checked(Multiply(1000000, 1000000));
    }
}
```
<span data-ttu-id="2886a-1451">`F` `checked` kullanımı `Multiply``x * y` değerlendirmesini etkilemez, bu nedenle `x * y` varsayılan taşma denetimi bağlamında değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1451">the use of `checked` in `F` does not affect the evaluation of `x * y` in `Multiply`, so `x * y` is evaluated in the default overflow checking context.</span></span>

<span data-ttu-id="2886a-1452">`unchecked` işleci, işaretli integral türlerinin sabitlerini onaltılık gösterimde yazarken kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1452">The `unchecked` operator is convenient when writing constants of the signed integral types in hexadecimal notation.</span></span> <span data-ttu-id="2886a-1453">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2886a-1453">For example:</span></span>
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

<span data-ttu-id="2886a-1454">Yukarıdaki onaltılık sabitlerin her ikisi de `uint`türündedir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1454">Both of the hexadecimal constants above are of type `uint`.</span></span> <span data-ttu-id="2886a-1455">Sabitler `int` aralığın dışında olduğundan, `unchecked` işleci olmadan `int` yapılan yayınlar derleme zamanı hataları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1455">Because the constants are outside the `int` range, without the `unchecked` operator, the casts to `int` would produce compile-time errors.</span></span>

<span data-ttu-id="2886a-1456">`checked` ve `unchecked` işleçleri ve deyimleri, programcıların bazı sayısal hesaplamaların belirli yönlerini denetlemesine olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1456">The `checked` and `unchecked` operators and statements allow programmers to control certain aspects of some numeric calculations.</span></span> <span data-ttu-id="2886a-1457">Ancak, bazı sayısal işleçlerin davranışı işlenenlerinin veri türlerine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1457">However, the behavior of some numeric operators depends on their operands' data types.</span></span> <span data-ttu-id="2886a-1458">Örneğin, iki ondalıkın çarpılması her zaman açık bir `unchecked` yapısı içinde bile taşma üzerinde bir özel durumla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1458">For example, multiplying two decimals always results in an exception on overflow even within an explicitly `unchecked` construct.</span></span> <span data-ttu-id="2886a-1459">Benzer şekilde, iki kayan noktalar, açıkça bir `checked` yapısı içinde bile taşma durumunda hiçbir şekilde neden olmaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-1459">Similarly, multiplying two floats never results in an exception on overflow even within an explicitly `checked` construct.</span></span> <span data-ttu-id="2886a-1460">Ayrıca, diğer işleçler, varsayılan veya açık olsun denetim modundan hiçbir şekilde etkilenmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1460">In addition, other operators are never affected by the mode of checking, whether default or explicit.</span></span>

### <a name="default-value-expressions"></a><span data-ttu-id="2886a-1461">Varsayılan değer ifadeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-1461">Default value expressions</span></span>

<span data-ttu-id="2886a-1462">Varsayılan değer ifadesi bir türün varsayılan değerini ([varsayılan değerler](variables.md#default-values)) almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1462">A default value expression is used to obtain the default value ([Default values](variables.md#default-values)) of a type.</span></span> <span data-ttu-id="2886a-1463">Tür parametresi bir değer türü veya bir başvuru türü ise, genellikle varsayılan değer ifadesi tür parametreleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1463">Typically a default value expression is used for type parameters, since it may not be known if the type parameter is a value type or a reference type.</span></span> <span data-ttu-id="2886a-1464">(Tür parametresi bir başvuru türü olarak bilinmediği takdirde, `null` sabit değerinden bir tür parametresine dönüştürme yok.)</span><span class="sxs-lookup"><span data-stu-id="2886a-1464">(No conversion exists from the `null` literal to a type parameter unless the type parameter is known to be a reference type.)</span></span>

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

<span data-ttu-id="2886a-1465">Bir default_value_expression *tür* , çalışma zamanında bir başvuru türü olarak değerlendirilirse, sonuç o türe dönüştürülür `null`.</span><span class="sxs-lookup"><span data-stu-id="2886a-1465">If the *type* in a *default_value_expression* evaluates at run-time to a reference type, the result is `null` converted to that type.</span></span> <span data-ttu-id="2886a-1466">Bir default_value_expression *türü* çalışma zamanında bir değer türüne değerlendirilirse, sonuç, *value_type*varsayılan değeridir ([Varsayılan oluşturucular](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="2886a-1466">If the *type* in a *default_value_expression* evaluates at run-time to a value type, the result is the *value_type*'s default value ([Default constructors](types.md#default-constructors)).</span></span>

<span data-ttu-id="2886a-1467">*Default_value_expression* bir başvuru türü veya bir başvuru türü olarak bilinen bir tür parametresi ise, bir sabit[ifadedir (](classes.md#type-parameter-constraints)[sabit ifadeler](expressions.md#constant-expressions)).</span><span class="sxs-lookup"><span data-stu-id="2886a-1467">A *default_value_expression* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) if the type is a reference type or a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="2886a-1468">Ayrıca, tür şu değer türlerinden biri ise *default_value_expression* sabit bir ifadedir: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`veya herhangi bir numaralandırma türü.</span><span class="sxs-lookup"><span data-stu-id="2886a-1468">In addition, a *default_value_expression* is a constant expression if the type is one of the following value types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, or any enumeration type.</span></span>


### <a name="nameof-expressions"></a><span data-ttu-id="2886a-1469">NameOf ifadeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-1469">Nameof expressions</span></span>

<span data-ttu-id="2886a-1470">Bir *nameof_expression* , bir program varlığının adını sabit bir dize olarak almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1470">A *nameof_expression* is used to obtain the name of a program entity as a constant string.</span></span>

```antlr
nameof_expression
    : 'nameof' '(' named_entity ')'
    ;

named_entity
    : simple_name
    | named_entity_target '.' identifier type_argument_list?
    ;

named_entity_target
    : 'this'
    | 'base'
    | named_entity 
    | predefined_type 
    | qualified_alias_member
    ;
```

<span data-ttu-id="2886a-1471">Dilbilgisi, *named_entity* işleneni her zaman bir ifadedir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1471">Grammatically speaking, the *named_entity* operand is always an expression.</span></span> <span data-ttu-id="2886a-1472">`nameof` ayrılmış bir anahtar sözcük olmadığından, bir NameOf ifadesi, basit ad `nameof`çağrısı ile her zaman sözdizimsel olarak belirsizdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1472">Because `nameof` is not a reserved keyword, a nameof expression is always syntactically ambiguous with an invocation of the simple name `nameof`.</span></span> <span data-ttu-id="2886a-1473">Uyumluluk nedenleriyle, adın bir ad araması ([basit adlar](expressions.md#simple-names)) `nameof` başarılı olursa, çağrının yasal olup olmamasına bakılmaksızın ifade *invocation_expression* olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1473">For compatibility reasons, if a name lookup ([Simple names](expressions.md#simple-names)) of the name `nameof` succeeds, the expression is treated as an *invocation_expression* -- regardless of whether the invocation is legal.</span></span> <span data-ttu-id="2886a-1474">Aksi takdirde, bir *nameof_expression*.</span><span class="sxs-lookup"><span data-stu-id="2886a-1474">Otherwise it is a *nameof_expression*.</span></span>

<span data-ttu-id="2886a-1475">Bir *nameof_expression* *named_entity* anlamı, ifadenin anlamı olarak ifade edilir; Yani *simple_name*, bir *base_access* veya *member_access*olarak.</span><span class="sxs-lookup"><span data-stu-id="2886a-1475">The meaning of the *named_entity* of a *nameof_expression* is the meaning of it as an expression; that is, either as a *simple_name*, a *base_access* or a *member_access*.</span></span> <span data-ttu-id="2886a-1476">Ancak, [basit adlarda](expressions.md#simple-names) ve [üye erişimlerinde](expressions.md#member-access) açıklanan aramanın bir hata ile sonuçlandığı durumlarda, bir örnek üyesi statik bir bağlamda bulunduğu için, *nameof_expression* böyle bir hata üretir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1476">However, where the lookup described in [Simple names](expressions.md#simple-names) and [Member access](expressions.md#member-access) results in an error because an instance member was found in a static context, a *nameof_expression* produces no such error.</span></span>

<span data-ttu-id="2886a-1477">Bir yöntem grubunu bir *type_argument_list*sahip olacak şekilde belirlemek *named_entity* bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1477">It is a compile-time error for a *named_entity* designating a method group to have a *type_argument_list*.</span></span> <span data-ttu-id="2886a-1478">Bir *named_entity_target* `dynamic`türüne sahip olması için derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1478">It is a compile time error for a *named_entity_target* to have the type `dynamic`.</span></span>

<span data-ttu-id="2886a-1479">*Nameof_expression* , `string`türünde sabit bir ifadedir ve çalışma zamanında hiçbir etkiye sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1479">A *nameof_expression* is a constant expression of type `string`, and has no effect at runtime.</span></span> <span data-ttu-id="2886a-1480">Özellikle, *named_entity* değerlendirilmez ve kesin atama analizinin amaçları doğrultusunda yok sayılır ([basit ifadeler için genel kurallar](variables.md#general-rules-for-simple-expressions)).</span><span class="sxs-lookup"><span data-stu-id="2886a-1480">Specifically, its *named_entity* is not evaluated, and is ignored for the purposes of definite assignment analysis ([General rules for simple expressions](variables.md#general-rules-for-simple-expressions)).</span></span> <span data-ttu-id="2886a-1481">Değeri, isteğe bağlı son *type_argument_list*önce *named_entity* , aşağıdaki şekilde dönüştürülecek olan en son tanıtıcıdır:</span><span class="sxs-lookup"><span data-stu-id="2886a-1481">Its value is the last identifier of the *named_entity* before the optional final *type_argument_list*, transformed in the following way:</span></span>

* <span data-ttu-id="2886a-1482">Kullanıldıysa, "`@`" ön eki kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1482">The prefix "`@`", if used, is removed.</span></span>
* <span data-ttu-id="2886a-1483">Her *unicode_escape_sequence* karşılık gelen Unicode karakteriyle dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1483">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
* <span data-ttu-id="2886a-1484">*Formatting_characters* kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1484">Any *formatting_characters* are removed.</span></span>

<span data-ttu-id="2886a-1485">Bunlar, tanımlayıcılar arasında eşitlik test edilirken [tanımlayıcılarla](lexical-structure.md#identifiers) uygulanan dönüşümlerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1485">These are the same transformations applied in [Identifiers](lexical-structure.md#identifiers) when testing equality between identifiers.</span></span>

<span data-ttu-id="2886a-1486">TODO: örnekler</span><span class="sxs-lookup"><span data-stu-id="2886a-1486">TODO: examples</span></span>

### <a name="anonymous-method-expressions"></a><span data-ttu-id="2886a-1487">Anonim yöntem ifadeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-1487">Anonymous method expressions</span></span>

<span data-ttu-id="2886a-1488">Bir *anonymous_method_expression* anonim bir işlevi tanımlamanın iki yöntemlerinden biridir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1488">An *anonymous_method_expression* is one of two ways of defining an anonymous function.</span></span> <span data-ttu-id="2886a-1489">Bunlar, [anonim işlev ifadelerinde](expressions.md#anonymous-function-expressions)daha ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1489">These are further described in [Anonymous function expressions](expressions.md#anonymous-function-expressions).</span></span>

## <a name="unary-operators"></a><span data-ttu-id="2886a-1490">Birli İşleçler</span><span class="sxs-lookup"><span data-stu-id="2886a-1490">Unary operators</span></span>

<span data-ttu-id="2886a-1491">`?`, `+`, `-`, `!`, `~`, `++`, `--`, cast ve `await` işleçleri Birli İşleçler olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1491">The `?`, `+`, `-`, `!`, `~`, `++`, `--`, cast, and `await` operators are called the unary operators.</span></span>

```antlr
unary_expression
    : primary_expression
    | null_conditional_expression
    | '+' unary_expression
    | '-' unary_expression
    | '!' unary_expression
    | '~' unary_expression
    | pre_increment_expression
    | pre_decrement_expression
    | cast_expression
    | await_expression
    | unary_expression_unsafe
    ;
```

<span data-ttu-id="2886a-1492">Bir *unary_expression* işleneni derleme zamanı türü `dynamic`sahipse, dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="2886a-1492">If the operand of a *unary_expression* has the compile-time type `dynamic`, it is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="2886a-1493">Bu durumda *unary_expression* derleme zamanı türü `dynamic`ve aşağıda açıklanan çözüm, işlenenin çalışma zamanı türü kullanılarak çalışma zamanında gerçekleşmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1493">In this case the compile-time type of the *unary_expression* is `dynamic`, and the resolution described below will take place at run-time using the run-time type of the operand.</span></span>

### <a name="null-conditional-operator"></a><span data-ttu-id="2886a-1494">Null-koşullu işleç</span><span class="sxs-lookup"><span data-stu-id="2886a-1494">Null-conditional operator</span></span>

<span data-ttu-id="2886a-1495">Null koşullu işleç, yalnızca bu işlenen null değilse, işlenene bir işlem listesi uygular.</span><span class="sxs-lookup"><span data-stu-id="2886a-1495">The null-conditional operator applies a list of operations to its operand only if that operand is non-null.</span></span> <span data-ttu-id="2886a-1496">Aksi takdirde, işleci uygulamanın sonucu `null`.</span><span class="sxs-lookup"><span data-stu-id="2886a-1496">Otherwise the result of applying the operator is `null`.</span></span>

```antlr
null_conditional_expression
    : primary_expression null_conditional_operations
    ;

null_conditional_operations
    : null_conditional_operations? '?' '.' identifier type_argument_list?
    | null_conditional_operations? '?' '[' argument_list ']'
    | null_conditional_operations '.' identifier type_argument_list?
    | null_conditional_operations '[' argument_list ']'
    | null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="2886a-1497">İşlem listesi, üye erişimi ve öğe erişim işlemleri (kendileri de null koşullu olabilir) ve çağırma içerebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1497">The list of operations can include member access and element access operations (which may themselves be null-conditional), as well as invocation.</span></span>

<span data-ttu-id="2886a-1498">Örneğin, `a.b?[0]?.c()` ifadesi bir *primary_expression* `a.b` ve *null_conditional_operations* `?[0]` (null-koşullu öğe erişimi), `?.c` (null-koşullu üye erişimi) ve `()` (çağırma) içeren bir *null_conditional_expression* .</span><span class="sxs-lookup"><span data-stu-id="2886a-1498">For example, the expression `a.b?[0]?.c()` is a *null_conditional_expression* with a *primary_expression* `a.b` and *null_conditional_operations* `?[0]` (null-conditional element access), `?.c` (null-conditional member access) and `()` (invocation).</span></span>

<span data-ttu-id="2886a-1499">*Primary_expression* `P`sahip bir *null_conditional_expression* `E` için, metin içeriğini eklemek tarafından alınan ve bir *`E0` `?` her* birinden önde gelen null_conditional_operations kaldıran ifade olmasına izin ver.</span><span class="sxs-lookup"><span data-stu-id="2886a-1499">For a *null_conditional_expression* `E` with a *primary_expression* `P`, let `E0` be the expression obtained by textually removing the leading `?` from each of the *null_conditional_operations* of `E` that have one.</span></span> <span data-ttu-id="2886a-1500">Kavramsal olarak, `E0` `?`s tarafından temsil edilen null denetimlerin hiçbiri bir `null`bulmadığı takdirde değerlendirilecek ifadedir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1500">Conceptually, `E0` is the expression that will be evaluated if none of the null checks represented by the `?`s do find a `null`.</span></span>

<span data-ttu-id="2886a-1501">Ayrıca, metin içeriğini eklemek tarafından elde edilen `E1`, önde gelen `?` `E`' deki *null_conditional_operations* yalnızca ilkini kaldırarak elde edin.</span><span class="sxs-lookup"><span data-stu-id="2886a-1501">Also, let `E1` be the expression obtained by textually removing the leading `?` from just the first of the *null_conditional_operations* in `E`.</span></span> <span data-ttu-id="2886a-1502">Bu, *birincil ifadeye* (yalnızca bir `?`) veya başka bir *null_conditional_expression*yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1502">This may lead to a *primary-expression* (if there was just one `?`) or to another *null_conditional_expression*.</span></span>

<span data-ttu-id="2886a-1503">Örneğin, `E` ifade `a.b?[0]?.c()`, `E0` ifade `a.b[0].c()` ve `E1` ifade `a.b[0]?.c()`.</span><span class="sxs-lookup"><span data-stu-id="2886a-1503">For example, if `E` is the expression `a.b?[0]?.c()`, then `E0` is the expression `a.b[0].c()` and `E1` is the expression `a.b[0]?.c()`.</span></span>

<span data-ttu-id="2886a-1504">`E0` hiçbir şey olarak sınıflandırıldıysanız `E` hiçbir şey olarak sınıflandırılacaktır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1504">If `E0` is classified as nothing, then `E` is classified as nothing.</span></span> <span data-ttu-id="2886a-1505">Aksi halde E bir değer olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1505">Otherwise E is classified as a value.</span></span>

<span data-ttu-id="2886a-1506">`E0` ve `E1`, `E`anlamını belirlemede kullanılır:</span><span class="sxs-lookup"><span data-stu-id="2886a-1506">`E0` and `E1` are used to determine the meaning of `E`:</span></span>

*  <span data-ttu-id="2886a-1507">`E` *statement_expression* olarak gerçekleşirse `E` anlamı deyimde aynı olur</span><span class="sxs-lookup"><span data-stu-id="2886a-1507">If `E` occurs as a *statement_expression* the meaning of `E` is the same as the statement</span></span>

   ```csharp
   if ((object)P != null) E1;
   ```

   <span data-ttu-id="2886a-1508">Bu P yalnızca bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1508">except that P is evaluated only once.</span></span>

*  <span data-ttu-id="2886a-1509">Aksi takdirde, `E0` hiçbir şey Nothing olarak sınıflandırıldığında, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1509">Otherwise, if `E0` is classified as nothing a compile-time error occurs.</span></span>

*  <span data-ttu-id="2886a-1510">Aksi takdirde, `E0`türü `T0`.</span><span class="sxs-lookup"><span data-stu-id="2886a-1510">Otherwise, let `T0` be the type of `E0`.</span></span>

   *  <span data-ttu-id="2886a-1511">`T0`, başvuru türü veya null yapılamayan bir değer türü olarak bilinen bir tür parametresi ise, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1511">If `T0` is a type parameter that is not known to be a reference type or a non-nullable value type, a compile-time error occurs.</span></span>

   *  <span data-ttu-id="2886a-1512">`T0` null yapılamayan bir değer türü ise, `E` türü `T0?`ve `E` anlamı</span><span class="sxs-lookup"><span data-stu-id="2886a-1512">If `T0` is a non-nullable value type, then the type of `E` is `T0?`, and the meaning of `E` is the same as</span></span>

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      <span data-ttu-id="2886a-1513">`P` hariç, yalnızca bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1513">except that `P` is evaluated only once.</span></span>

   *  <span data-ttu-id="2886a-1514">Aksi takdirde, E türü T0 ve E 'nin anlamı</span><span class="sxs-lookup"><span data-stu-id="2886a-1514">Otherwise the type of E is T0, and the meaning of E is the same as</span></span>

      ```csharp
      ((object)P == null) ? null : E1
      ```

      <span data-ttu-id="2886a-1515">`P` hariç, yalnızca bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1515">except that `P` is evaluated only once.</span></span>

<span data-ttu-id="2886a-1516">`E1` bir *null_conditional_expression*kendi kendine `null`, daha fazla `?`olmadığı sürece bu kurallar yeniden uygulanır ve ifade, birincil ifade `E0`kadar tüm şekilde azaltılmıştır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1516">If `E1` is itself a *null_conditional_expression*, then these rules are applied again, nesting the tests for `null` until there are no further `?`'s, and the expression has been reduced all the way down to the primary-expression `E0`.</span></span>

<span data-ttu-id="2886a-1517">Örneğin, ifade `a.b?[0]?.c()` deyiminde olduğu gibi deyim ifadesi olarak gerçekleşirse:</span><span class="sxs-lookup"><span data-stu-id="2886a-1517">For example, if the expression `a.b?[0]?.c()` occurs as a statement-expression, as in the statement:</span></span>
```csharp
a.b?[0]?.c();
```
<span data-ttu-id="2886a-1518">anlamı şu şekilde eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1518">its meaning is equivalent to:</span></span>
```csharp
if (a.b != null) a.b[0]?.c();
```
<span data-ttu-id="2886a-1519">yeniden eşittir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1519">which again is equivalent to:</span></span>
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
<span data-ttu-id="2886a-1520">`a.b` ve `a.b[0]` yalnızca bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1520">Except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

<span data-ttu-id="2886a-1521">Değeri kullanıldığı bir bağlamda gerçekleşirse, şöyle olduğu gibi:</span><span class="sxs-lookup"><span data-stu-id="2886a-1521">If it occurs in a context where its value is used, as in:</span></span>
```csharp
var x = a.b?[0]?.c();
```
<span data-ttu-id="2886a-1522">son çağrının türünün null yapılamayan bir değer türü olmadığı varsayılarak, anlamı şu değere eşittir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1522">and assuming that the type of the final invocation is not a non-nullable value type, its meaning is equivalent to:</span></span>
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
<span data-ttu-id="2886a-1523">`a.b` ve `a.b[0]` yalnızca bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1523">except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

#### <a name="null-conditional-expressions-as-projection-initializers"></a><span data-ttu-id="2886a-1524">Projeksiyon başlatıcıları olarak null Koşullu ifadeler</span><span class="sxs-lookup"><span data-stu-id="2886a-1524">Null-conditional expressions as projection initializers</span></span>

<span data-ttu-id="2886a-1525">Null koşullu bir ifadeye, yalnızca bir (isteğe bağlı null koşullu) üye erişimiyle biterse bir *anonymous_object_creation_expression* ([anonim nesne oluşturma ifadelerinde](expressions.md#anonymous-object-creation-expressions)) *member_declarator* olarak izin verilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1525">A null-conditional expression is only allowed as a *member_declarator* in an *anonymous_object_creation_expression* ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) if it ends with an (optionally null-conditional) member access.</span></span> <span data-ttu-id="2886a-1526">Dilbilgisi, bu gereksinim şöyle ifade edilebilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1526">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

<span data-ttu-id="2886a-1527">Bu, yukarıdaki *null_conditional_expression* dilbilgisinde özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1527">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="2886a-1528">[Anonim nesne oluşturma ifadelerinde](expressions.md#anonymous-object-creation-expressions) *member_declarator* için üretim, yalnızca *null_conditional_member_access*içerir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1528">The production for *member_declarator* in [Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions) then includes only *null_conditional_member_access*.</span></span>

#### <a name="null-conditional-expressions-as-statement-expressions"></a><span data-ttu-id="2886a-1529">Deyim ifadeleri olarak null Koşullu ifadeler</span><span class="sxs-lookup"><span data-stu-id="2886a-1529">Null-conditional expressions as statement expressions</span></span>

<span data-ttu-id="2886a-1530">Null koşullu ifadeye yalnızca bir çağrı ile bitiyorsa *statement_expression* ([ifade deyimleri](statements.md#expression-statements)) olarak izin verilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1530">A null-conditional expression is only allowed as a *statement_expression* ([Expression statements](statements.md#expression-statements)) if it ends with an invocation.</span></span> <span data-ttu-id="2886a-1531">Dilbilgisi, bu gereksinim şöyle ifade edilebilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1531">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="2886a-1532">Bu, yukarıdaki *null_conditional_expression* dilbilgisinde özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1532">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="2886a-1533">[Expression deyimlerdeki](statements.md#expression-statements) *statement_expression* için üretim, yalnızca *null_conditional_invocation_expression*içerir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1533">The production for *statement_expression* in [Expression statements](statements.md#expression-statements) then includes only *null_conditional_invocation_expression*.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="2886a-1534">Tekli artı işleci</span><span class="sxs-lookup"><span data-stu-id="2886a-1534">Unary plus operator</span></span>

<span data-ttu-id="2886a-1535">`+x`bir işlem için, tekil işleç aşırı yükleme çözümlemesi ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1535">For an operation of the form `+x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="2886a-1536">İşlenen, seçili işlecin parametre türüne dönüştürülür ve sonucun türü işlecin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1536">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="2886a-1537">Önceden tanımlanmış birli artı işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2886a-1537">The predefined unary plus operators are:</span></span>

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

<span data-ttu-id="2886a-1538">Bu işleçlerin her biri için sonuç yalnızca işlenenin değeridir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1538">For each of these operators, the result is simply the value of the operand.</span></span>

### <a name="unary-minus-operator"></a><span data-ttu-id="2886a-1539">Birli eksi işleci</span><span class="sxs-lookup"><span data-stu-id="2886a-1539">Unary minus operator</span></span>

<span data-ttu-id="2886a-1540">`-x`bir işlem için, tekil işleç aşırı yükleme çözümlemesi ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1540">For an operation of the form `-x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="2886a-1541">İşlenen, seçili işlecin parametre türüne dönüştürülür ve sonucun türü işlecin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1541">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="2886a-1542">Önceden tanımlanmış olumsuzlama işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2886a-1542">The predefined negation operators are:</span></span>

*  <span data-ttu-id="2886a-1543">Tamsayı değilleme:</span><span class="sxs-lookup"><span data-stu-id="2886a-1543">Integer negation:</span></span>

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   <span data-ttu-id="2886a-1544">Sonuç, sıfırdan `x` çıkararak hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1544">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="2886a-1545">`x` değeri, işlenen türünün en küçük gösterilebilir tablo değeridir (`int` için-2 ^ 31 veya `long`için-2 ^ 63), `x` matematik Olumsuzlaştırma, işlenen türü içinde gösterilemeyen bir tablo değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1545">If the value of `x` is the smallest representable value of the operand type (-2^31 for `int` or -2^63 for `long`), then the mathematical negation of `x` is not representable within the operand type.</span></span> <span data-ttu-id="2886a-1546">Bu bir `checked` bağlamı içinde oluşursa, bir `System.OverflowException` oluşturulur; `unchecked` bağlamı içinde oluşursa, sonuç işlenenin değeridir ve taşma raporlanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1546">If this occurs within a `checked` context, a `System.OverflowException` is thrown; if it occurs within an `unchecked` context, the result is the value of the operand and the overflow is not reported.</span></span>

   <span data-ttu-id="2886a-1547">Olumsuzlama işlecinin işleneni `uint`türünde ise, `long`türüne dönüştürülür ve sonucun türü `long`olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1547">If the operand of the negation operator is of type `uint`, it is converted to type `long`, and the type of the result is `long`.</span></span> <span data-ttu-id="2886a-1548">Bir özel durum, 2147483648 (-2 ^ 31) değerinin ondalık tamsayı sabit değeri ([tamsayı değişmez](lexical-structure.md#integer-literals)değer) olarak yazılmasına `int` izin veren bir kuraldır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1548">An exception is the rule that permits the `int` value -2147483648 (-2^31) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

   <span data-ttu-id="2886a-1549">Olumsuzlama işlecinin işleneni `ulong`türünde ise, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1549">If the operand of the negation operator is of type `ulong`, a compile-time error occurs.</span></span> <span data-ttu-id="2886a-1550">Özel durum,-9223372036854775808 (-2 ^ 63) `long` değerinin ondalık tamsayı sabit değeri ([tamsayı değişmez](lexical-structure.md#integer-literals)değer) olarak yazılmasına izin veren kuraldır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1550">An exception is the rule that permits the `long` value -9223372036854775808 (-2^63) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

*  <span data-ttu-id="2886a-1551">Kayan nokta olumsuzlama:</span><span class="sxs-lookup"><span data-stu-id="2886a-1551">Floating-point negation:</span></span>

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   <span data-ttu-id="2886a-1552">Sonuç, ters çevrilme ile `x` değeridir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1552">The result is the value of `x` with its sign inverted.</span></span> <span data-ttu-id="2886a-1553">`x` NaN ise, sonuç de NaN olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1553">If `x` is NaN, the result is also NaN.</span></span>

*  <span data-ttu-id="2886a-1554">Ondalık olumsuzlama:</span><span class="sxs-lookup"><span data-stu-id="2886a-1554">Decimal negation:</span></span>

   ```csharp
   decimal operator -(decimal x);
   ```

   <span data-ttu-id="2886a-1555">Sonuç, sıfırdan `x` çıkararak hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1555">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="2886a-1556">Decimal olumsuzlama, `System.Decimal`türünde birli eksi işlecinin kullanılmasıyla eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1556">Decimal negation is equivalent to using the unary minus operator of type `System.Decimal`.</span></span>

### <a name="logical-negation-operator"></a><span data-ttu-id="2886a-1557">Mantıksal Değilleme İşleci</span><span class="sxs-lookup"><span data-stu-id="2886a-1557">Logical negation operator</span></span>

<span data-ttu-id="2886a-1558">`!x`bir işlem için, tekil işleç aşırı yükleme çözümlemesi ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1558">For an operation of the form `!x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="2886a-1559">İşlenen, seçili işlecin parametre türüne dönüştürülür ve sonucun türü işlecin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1559">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="2886a-1560">Yalnızca bir tane önceden tanımlanmış mantıksal olumsuzlama işleci var:</span><span class="sxs-lookup"><span data-stu-id="2886a-1560">Only one predefined logical negation operator exists:</span></span>
```csharp
bool operator !(bool x);
```

<span data-ttu-id="2886a-1561">Bu işleç, işlenenin mantıksal olumsuzunu hesaplar: işlenen `true`, sonuç `false`.</span><span class="sxs-lookup"><span data-stu-id="2886a-1561">This operator computes the logical negation of the operand: If the operand is `true`, the result is `false`.</span></span> <span data-ttu-id="2886a-1562">İşlenen `false`, sonuç `true`olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1562">If the operand is `false`, the result is `true`.</span></span>

### <a name="bitwise-complement-operator"></a><span data-ttu-id="2886a-1563">Bit düzeyinde tamamlama işleci</span><span class="sxs-lookup"><span data-stu-id="2886a-1563">Bitwise complement operator</span></span>

<span data-ttu-id="2886a-1564">`~x`bir işlem için, tekil işleç aşırı yükleme çözümlemesi ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1564">For an operation of the form `~x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="2886a-1565">İşlenen, seçili işlecin parametre türüne dönüştürülür ve sonucun türü işlecin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1565">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="2886a-1566">Önceden tanımlanmış bit düzeyinde tamamlama işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2886a-1566">The predefined bitwise complement operators are:</span></span>
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

<span data-ttu-id="2886a-1567">Bu işleçlerin her biri için, işlemin sonucu `x`bit düzeyinde tamamlayıcı bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1567">For each of these operators, the result of the operation is the bitwise complement of `x`.</span></span>

<span data-ttu-id="2886a-1568">Her numaralandırma türü `E` örtük olarak aşağıdaki bit düzeyinde tamamlayıcı işleci sağlar:</span><span class="sxs-lookup"><span data-stu-id="2886a-1568">Every enumeration type `E` implicitly provides the following bitwise complement operator:</span></span>

```csharp
E operator ~(E x);
```

<span data-ttu-id="2886a-1569">`~x`değerlendirilmesinin sonucu, `x` bir numaralandırma türünün temel alınan `U`türü `E` bir ifadesiyse, `(E)(~(U)x)`' a dönüştürme her zaman `E` bağlamında ([denetlenen ve işaretlenmemiş operatörler](expressions.md#the-checked-and-unchecked-operators)) gibi gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1569">The result of evaluating `~x`, where `x` is an expression of an enumeration type `E` with an underlying type `U`, is exactly the same as evaluating `(E)(~(U)x)`, except that the conversion to `E` is always performed as if in an `unchecked` context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span>

### <a name="prefix-increment-and-decrement-operators"></a><span data-ttu-id="2886a-1570">Önek arttırma ve azaltma işleçleri</span><span class="sxs-lookup"><span data-stu-id="2886a-1570">Prefix increment and decrement operators</span></span>

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

<span data-ttu-id="2886a-1571">Önek artırma veya azaltma işleminin işleneni, değişken olarak sınıflandırılmış bir ifade, özellik erişimi veya Dizin Oluşturucu erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1571">The operand of a prefix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="2886a-1572">İşlemin sonucu, işlenenden aynı türde bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1572">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="2886a-1573">Bir önek artışı veya azaltma işleminin işleneni bir özellik veya Dizin Oluşturucu erişimi ise, özelliğin veya dizin oluşturucunun hem `get` hem de bir `set` erişimcisi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1573">If the operand of a prefix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="2886a-1574">Bu durumda, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1574">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="2886a-1575">Tekil operatör aşırı yükleme çözümü ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1575">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="2886a-1576">Önceden tanımlı `++` ve `--` işleçleri şu türler için mevcuttur: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`ve herhangi bir numaralandırma türü.</span><span class="sxs-lookup"><span data-stu-id="2886a-1576">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="2886a-1577">Önceden tanımlanmış `++` işleçleri, işlenene 1 eklenerek üretilen değeri döndürür ve önceden tanımlanmış `--` işleçleri işleneni 1 çıkararak üretilen değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1577">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="2886a-1578">`checked` bağlamında, bu ekleme veya çıkarma sonucu sonuç türü aralığının dışındaysa ve sonuç türü bir tam sayı türü veya Enum türü ise, bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1578">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="2886a-1579">`++x` veya `--x` bir ön ekin çalışma zamanı işleme işlemi, aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="2886a-1579">The run-time processing of a prefix increment or decrement operation of the form `++x` or `--x` consists of the following steps:</span></span>

*   <span data-ttu-id="2886a-1580">`x` bir değişken olarak sınıflandırıldıysanız:</span><span class="sxs-lookup"><span data-stu-id="2886a-1580">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="2886a-1581">`x`, değişkeni üretmek için değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1581">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="2886a-1582">Seçili operatör, bağımsız değişkeni olarak `x` değeriyle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1582">The selected operator is invoked with the value of `x` as its argument.</span></span>
    * <span data-ttu-id="2886a-1583">İşleci tarafından döndürülen değer, `x`değerlendirmesi tarafından verilen konumda depolanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1583">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="2886a-1584">İşleci tarafından döndürülen değer işlemin sonucu olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1584">The value returned by the operator becomes the result of the operation.</span></span>
*   <span data-ttu-id="2886a-1585">`x`, özellik veya Dizin Oluşturucu erişimi olarak sınıflandırıldıysanız:</span><span class="sxs-lookup"><span data-stu-id="2886a-1585">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="2886a-1586">Örnek ifadesi (`x` `static`değilse) ve bağımsız değişken listesi (`x` bir Dizin Oluşturucu erişimsiyse) `x` ile ilişkili olarak değerlendirilir ve sonuçlar sonraki `get` ve `set` erişimci etkinleştirmeleri içinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1586">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="2886a-1587">`x` `get` erişimcisi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1587">The `get` accessor of `x` is invoked.</span></span>
    * <span data-ttu-id="2886a-1588">Seçili operatör, bağımsız değişkeni olarak `get` erişimcisinin döndürdüğü değerle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1588">The selected operator is invoked with the value returned by the `get` accessor as its argument.</span></span>
    * <span data-ttu-id="2886a-1589">`x` `set` erişimcisi, işleç tarafından `value` bağımsız değişkeni olarak döndürülen değerle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1589">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="2886a-1590">İşleci tarafından döndürülen değer işlemin sonucu olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1590">The value returned by the operator becomes the result of the operation.</span></span>

<span data-ttu-id="2886a-1591">`++` ve `--` işleçleri de sonek gösterimini ([Sonek artışı ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators)) destekler.</span><span class="sxs-lookup"><span data-stu-id="2886a-1591">The `++` and `--` operators also support postfix notation ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="2886a-1592">Genellikle, `x++` veya `x--` sonucu işlemden önceki `x` değeridir, ancak `++x` veya `--x` sonucu işlemden sonraki `x` değeridir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1592">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="2886a-1593">Her iki durumda da `x`, işlemden sonra aynı değere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1593">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="2886a-1594">Bir `operator++` veya `operator--` uygulama, sonek veya ön ek gösterimi kullanılarak çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1594">An `operator++` or `operator--` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="2886a-1595">İki gösterimler için ayrı işleç uygulamalarına sahip olmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1595">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="2886a-1596">Atama ifadeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-1596">Cast expressions</span></span>

<span data-ttu-id="2886a-1597">Bir ifadeyi açıkça verilen bir türe dönüştürmek için *cast_expression* kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1597">A *cast_expression* is used to explicitly convert an expression to a given type.</span></span>

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

<span data-ttu-id="2886a-1598">*Cast_expression* , `T` bir *tür* olduğu ve `E` *unary_expression*, `E` türüne `T`değerine sahip bir açık dönüştürme ([Açık dönüştürmeler](conversions.md#explicit-conversions)) gerçekleştirir `(T)E`form.</span><span class="sxs-lookup"><span data-stu-id="2886a-1598">A *cast_expression* of the form `(T)E`, where `T` is a *type* and `E` is a *unary_expression*, performs an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) of the value of `E` to type `T`.</span></span> <span data-ttu-id="2886a-1599">`E` `T`' den hiçbir açık dönüştürme yoksa, bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1599">If no explicit conversion exists from `E` to `T`, a binding-time error occurs.</span></span> <span data-ttu-id="2886a-1600">Aksi takdirde, sonuç açık dönüştürme tarafından üretilen değerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1600">Otherwise, the result is the value produced by the explicit conversion.</span></span> <span data-ttu-id="2886a-1601">Sonuç, `E` bir değişken olduğunu belirtse bile her zaman bir değer olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1601">The result is always classified as a value, even if `E` denotes a variable.</span></span>

<span data-ttu-id="2886a-1602">Bir *cast_expression* için dilbilgisi, belirli sözdizimsel belirsizlikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="2886a-1602">The grammar for a *cast_expression* leads to certain syntactic ambiguities.</span></span> <span data-ttu-id="2886a-1603">Örneğin, `(x)-y` ifade, bir *cast_expression* (`x`türüne `-y` dönüştürme) veya additive_expression *ile birleştirilmiş bir parenthesized_expression olarak* *(`x - y)`* değerini hesaplayan) olarak yorumlanacaktır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1603">For example, the expression `(x)-y` could either be interpreted as a *cast_expression* (a cast of `-y` to type `x`) or as an *additive_expression* combined with a *parenthesized_expression* (which computes the value `x - y)`.</span></span>

<span data-ttu-id="2886a-1604">*Cast_expression* belirsizlikleri çözümlemek için aşağıdaki kural bulunur: parantez içine alınmış bir veya daha fazla *belirteç*[dizisi (boşluk](lexical-structure.md#white-space)), yalnızca aşağıdakilerden en az biri doğru ise, bir *cast_expression* başlangıcını kabul ediyor:</span><span class="sxs-lookup"><span data-stu-id="2886a-1604">To resolve *cast_expression* ambiguities, the following rule exists: A sequence of one or more *token*s ([White space](lexical-structure.md#white-space)) enclosed in parentheses is considered the start of a *cast_expression* only if at least one of the following are true:</span></span>

*  <span data-ttu-id="2886a-1605">Belirteçlerin sırası, bir *tür*için doğru dilbilgisinde bulunur, ancak bir *ifade*için kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-1605">The sequence of tokens is correct grammar for a *type*, but not for an *expression*.</span></span>
*  <span data-ttu-id="2886a-1606">Belirteçlerin sırası, bir *tür*için doğru dilbilgisi ve kapatma parantezinden hemen sonraki belirteç, "`~`", belirteç "`!`", belirteç "`(`", *tanımlayıcı* ([Unicode karakter kaçış dizileri](lexical-structure.md#unicode-character-escape-sequences)), *değişmez değer* ([sabit değerler](lexical-structure.md#literals)) veya `as` ve `is`dışında herhangi bir *anahtar sözcüğü* ([anahtar](lexical-structure.md#keywords)sözcük).</span><span class="sxs-lookup"><span data-stu-id="2886a-1606">The sequence of tokens is correct grammar for a *type*, and the token immediately following the closing parentheses is the token "`~`", the token "`!`", the token "`(`", an *identifier* ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)), a *literal* ([Literals](lexical-structure.md#literals)), or any *keyword* ([Keywords](lexical-structure.md#keywords)) except `as` and `is`.</span></span>

<span data-ttu-id="2886a-1607">Yukarıdaki "doğru dilbilgisi" terimi, yalnızca belirteçlerin sırasının belirli dilbilgisi üretimine uyması gerektiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1607">The term "correct grammar" above means only that the sequence of tokens must conform to the particular grammatical production.</span></span> <span data-ttu-id="2886a-1608">Bu, özellikle herhangi bir anayayrılan tanımlayıcıların gerçek anlamını düşünmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1608">It specifically does not consider the actual meaning of any constituent identifiers.</span></span> <span data-ttu-id="2886a-1609">Örneğin, `x` ve `y` tanımlayıcılar ise, `x.y` gerçekten bir tür belirtmese bile `x.y`, bir tür için doğru dilbilgisi olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1609">For example, if `x` and `y` are identifiers, then `x.y` is correct grammar for a type, even if `x.y` doesn't actually denote a type.</span></span>

<span data-ttu-id="2886a-1610">Kesinleştirme kuralından, `x` ve `y` tanımlayıcılar ise, `(x)y`, `(x)(y)`ve `(x)(-y)` *, cast_expression bir*tür tanımlasa bile, `(x)-y` değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1610">From the disambiguation rule it follows that, if `x` and `y` are identifiers, `(x)y`, `(x)(y)`, and `(x)(-y)` are *cast_expression*s, but `(x)-y` is not, even if `x` identifies a type.</span></span> <span data-ttu-id="2886a-1611">Ancak, `x` önceden tanımlanmış bir türü tanımlayan bir anahtar sözcüktür (örneğin, `int`), dört *cast_expression*formun tamamı, bu tür bir anahtar sözcük muhtemelen bir ifade olamaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-1611">However, if `x` is a keyword that identifies a predefined type (such as `int`), then all four forms are *cast_expression*s (because such a keyword could not possibly be an expression by itself).</span></span>

### <a name="await-expressions"></a><span data-ttu-id="2886a-1612">Await ifadeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-1612">Await expressions</span></span>

<span data-ttu-id="2886a-1613">Await işleci, işlenen tarafından temsil edilen zaman uyumsuz işlem tamamlanana kadar kapsayan zaman uyumsuz işlevin değerlendirmesini askıya almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1613">The await operator is used to suspend evaluation of the enclosing async function until the asynchronous operation represented by the operand has completed.</span></span>

```antlr
await_expression
    : 'await' unary_expression
    ;
```

<span data-ttu-id="2886a-1614">Bir *await_expression* yalnızca zaman uyumsuz bir işlevin gövdesinde ([yineleyiciler](classes.md#iterators)) izin verilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1614">An *await_expression* is only allowed in the body of an async function ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="2886a-1615">En yakın kapsayan zaman uyumsuz işlevi içinde, bu yerlerde bir *await_expression* gerçekleşmeyebilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1615">Within the nearest enclosing async function, an *await_expression* may not occur in these places:</span></span>

*  <span data-ttu-id="2886a-1616">İç içe geçmiş (zaman uyumsuz) anonim bir işlev içinde</span><span class="sxs-lookup"><span data-stu-id="2886a-1616">Inside a nested (non-async) anonymous function</span></span>
*  <span data-ttu-id="2886a-1617">*Lock_statement* bloğunun içinde</span><span class="sxs-lookup"><span data-stu-id="2886a-1617">Inside the block of a *lock_statement*</span></span>
*  <span data-ttu-id="2886a-1618">Güvenli olmayan bir bağlamda</span><span class="sxs-lookup"><span data-stu-id="2886a-1618">In an unsafe context</span></span>

<span data-ttu-id="2886a-1619">Bir *await_expression* , bir *query_expression*içinde çoğu yerde gerçekleşmediğini unutmayın, çünkü bunlar sözdizimsel olarak zaman uyumsuz lambda ifadeleri kullanmak üzere dönüştürüldü.</span><span class="sxs-lookup"><span data-stu-id="2886a-1619">Note that an *await_expression* cannot occur in most places within a *query_expression*, because those are syntactically transformed to use non-async lambda expressions.</span></span>

<span data-ttu-id="2886a-1620">Zaman uyumsuz bir işlevin içinde `await` tanımlayıcı olarak kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-1620">Inside of an async function, `await` cannot be used as an identifier.</span></span> <span data-ttu-id="2886a-1621">Bu nedenle, await ifadeleri ve tanımlayıcılar içeren çeşitli ifadeler arasında sözdizimsel belirsizlik yoktur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1621">There is therefore no syntactic ambiguity between await-expressions and various expressions involving identifiers.</span></span> <span data-ttu-id="2886a-1622">Zaman uyumsuz işlevlerin dışında `await` normal tanımlayıcı işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1622">Outside of async functions, `await` acts as a normal identifier.</span></span>

<span data-ttu-id="2886a-1623">Bir *await_expression* işleneni ***görev***olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1623">The operand of an *await_expression* is called the ***task***.</span></span> <span data-ttu-id="2886a-1624">*Await_expression* değerlendirilen zaman uyumsuz bir işlemi temsil eder veya tamamlanmayabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1624">It represents an asynchronous operation that may or may not be complete at the time the *await_expression* is evaluated.</span></span> <span data-ttu-id="2886a-1625">Await işlecinin amacı, beklenen zaman uyumsuz işlevin yürütülmesini, bekleme görevi tamamlanana kadar askıya almak ve sonra sonucunu elde etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1625">The purpose of the await operator is to suspend execution of the enclosing async function until the awaited task is complete, and then obtain its outcome.</span></span>

#### <a name="awaitable-expressions"></a><span data-ttu-id="2886a-1626">Awasever ifadeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-1626">Awaitable expressions</span></span>

<span data-ttu-id="2886a-1627">Await ifadesinin görevi için ***beklenen***bir ifade olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1627">The task of an await expression is required to be ***awaitable***.</span></span> <span data-ttu-id="2886a-1628">Aşağıdakilerden biri varsa, bir ifade `t` beklenebilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1628">An expression `t` is awaitable if one of the following holds:</span></span>

*  <span data-ttu-id="2886a-1629">`t` derleme zamanı türüdür `dynamic`</span><span class="sxs-lookup"><span data-stu-id="2886a-1629">`t` is of compile time type `dynamic`</span></span>
*  <span data-ttu-id="2886a-1630">`t`, hiçbir parametre olmadan ve hiçbir tür parametresi olmadan `GetAwaiter` adlı erişilebilir bir örnek veya genişletme yöntemine sahiptir ve bir dönüş türü, aşağıdaki her bir bekletme için `A`.</span><span class="sxs-lookup"><span data-stu-id="2886a-1630">`t` has an accessible instance or extension method called `GetAwaiter` with no parameters and no type parameters, and a return type `A` for which all of the following hold:</span></span>
   * <span data-ttu-id="2886a-1631">`A` arabirim `System.Runtime.CompilerServices.INotifyCompletion` uygular (bundan sonra breçekimi için `INotifyCompletion` olarak bilinirdi)</span><span class="sxs-lookup"><span data-stu-id="2886a-1631">`A` implements the interface `System.Runtime.CompilerServices.INotifyCompletion` (hereafter known as `INotifyCompletion` for brevity)</span></span>
   * <span data-ttu-id="2886a-1632">`A`, `IsCompleted` erişilebilir, okunabilir bir örnek özelliğine sahiptir `bool`</span><span class="sxs-lookup"><span data-stu-id="2886a-1632">`A` has an accessible, readable instance property `IsCompleted` of type `bool`</span></span>
   * <span data-ttu-id="2886a-1633">`A`, parametresiz ve hiçbir tür parametresi olmayan `GetResult` erişilebilir bir örnek metoduna sahiptir</span><span class="sxs-lookup"><span data-stu-id="2886a-1633">`A` has an accessible instance method `GetResult` with no parameters and no type parameters</span></span>

<span data-ttu-id="2886a-1634">`GetAwaiter` yönteminin amacı, görev için bir ***awaiter*** elde sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1634">The purpose of the `GetAwaiter` method is to obtain an ***awaiter*** for the task.</span></span> <span data-ttu-id="2886a-1635">`A` türü await ifadesi için ***awaiter türü*** olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1635">The type `A` is called the ***awaiter type*** for the await expression.</span></span>

<span data-ttu-id="2886a-1636">`IsCompleted` özelliğinin amacı görevin zaten tamamlanıp tamamlanmadığını belirlemektir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1636">The purpose of the `IsCompleted` property is to determine if the task is already complete.</span></span> <span data-ttu-id="2886a-1637">Bu durumda, değerlendirmeyi askıya almanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-1637">If so, there is no need to suspend evaluation.</span></span>

<span data-ttu-id="2886a-1638">`INotifyCompletion.OnCompleted` yönteminin amacı, göreve bir "devamlılık" kaydı sağlamaktır; Yani, görev tamamlandıktan sonra çağrılacak bir temsilci (`System.Action`türü).</span><span class="sxs-lookup"><span data-stu-id="2886a-1638">The purpose of the `INotifyCompletion.OnCompleted` method is to sign up a "continuation" to the task; i.e. a delegate (of type `System.Action`) that will be invoked once the task is complete.</span></span>

<span data-ttu-id="2886a-1639">`GetResult` yönteminin amacı, tamamlandıktan sonra görevin sonucunu elde etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1639">The purpose of the `GetResult` method is to obtain the outcome of the task once it is complete.</span></span> <span data-ttu-id="2886a-1640">Bu sonuç, muhtemelen bir sonuç değeriyle birlikte başarılı bir tamamlama veya `GetResult` yöntemi tarafından oluşturulan bir özel durum olabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1640">This outcome may be successful completion, possibly with a result value, or it may be an exception which is thrown by the `GetResult` method.</span></span>

#### <a name="classification-of-await-expressions"></a><span data-ttu-id="2886a-1641">Await ifadelerinin sınıflandırması</span><span class="sxs-lookup"><span data-stu-id="2886a-1641">Classification of await expressions</span></span>

<span data-ttu-id="2886a-1642">`await t` ifade, ifade `(t).GetAwaiter().GetResult()`aynı şekilde sınıflandırıldı.</span><span class="sxs-lookup"><span data-stu-id="2886a-1642">The expression `await t` is classified the same way as the expression `(t).GetAwaiter().GetResult()`.</span></span> <span data-ttu-id="2886a-1643">Bu nedenle, `GetResult` dönüş türü `void`, *await_expression* hiçbir şey olarak sınıflandırılacaktır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1643">Thus, if the return type of `GetResult` is `void`, the *await_expression* is classified as nothing.</span></span> <span data-ttu-id="2886a-1644">`T`void olmayan bir dönüş türüne sahipse *await_expression* , `T`türünde bir değer olarak sınıflandırılacaktır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1644">If it has a non-void return type `T`, the *await_expression* is classified as a value of type `T`.</span></span>

#### <a name="runtime-evaluation-of-await-expressions"></a><span data-ttu-id="2886a-1645">Await ifadelerinin çalışma zamanı değerlendirmesi</span><span class="sxs-lookup"><span data-stu-id="2886a-1645">Runtime evaluation of await expressions</span></span>

<span data-ttu-id="2886a-1646">Çalışma zamanında ifade `await t` aşağıdaki gibi değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-1646">At runtime, the expression `await t` is evaluated as follows:</span></span>

*  <span data-ttu-id="2886a-1647">Bir awaiter `a`, ifade `(t).GetAwaiter()`değerlendirilirken elde edilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1647">An awaiter `a` is obtained by evaluating the expression `(t).GetAwaiter()`.</span></span>
*  <span data-ttu-id="2886a-1648">Bir `bool` `b`, ifade `(a).IsCompleted`değerlendirilirken elde edilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1648">A `bool` `b` is obtained by evaluating the expression `(a).IsCompleted`.</span></span>
*  <span data-ttu-id="2886a-1649">`b` `false`, değerlendirme, `a` arabirimi `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (bundan sonra breçekimi için `ICriticalNotifyCompletion` olarak bilinirdi) uygulayıp uygulamadığına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1649">If `b` is `false` then evaluation depends on whether `a` implements the interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (hereafter known as `ICriticalNotifyCompletion` for brevity).</span></span> <span data-ttu-id="2886a-1650">Bu denetim bağlama zamanında yapılır; Yani, `a` derleme zamanı türü `dynamic`varsa ve derleme sırasında çalışma zamanında.</span><span class="sxs-lookup"><span data-stu-id="2886a-1650">This check is done at binding time; i.e. at runtime if `a` has the compile time type `dynamic`, and at compile time otherwise.</span></span> <span data-ttu-id="2886a-1651">`r` sürdürme temsilcisini ([yineleyiciler](classes.md#iterators)) göstermek için:</span><span class="sxs-lookup"><span data-stu-id="2886a-1651">Let `r` denote the resumption delegate ([Iterators](classes.md#iterators)):</span></span>
    * <span data-ttu-id="2886a-1652">`a` `ICriticalNotifyCompletion`uygulamaz `(a as (INotifyCompletion)).OnCompleted(r)` ifade değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1652">If `a` does not implement `ICriticalNotifyCompletion`, then the expression `(a as (INotifyCompletion)).OnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="2886a-1653">`a` `ICriticalNotifyCompletion`uygulıyorsa `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` ifade değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1653">If `a` does implement `ICriticalNotifyCompletion`, then the expression `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="2886a-1654">Değerlendirme daha sonra askıya alınır ve denetim zaman uyumsuz işlevin geçerli çağıranına döndürülür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1654">Evaluation is then suspended, and control is returned to the current caller of the async function.</span></span>
*  <span data-ttu-id="2886a-1655">Hemen sonra (`b` `true`) veya daha sonra sürdürme temsilcisinin çağrılması durumunda (`b` `false`ise), ifade `(a).GetResult()` değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1655">Either immediately after (if `b` was `true`), or upon later invocation of the resumption delegate (if `b` was `false`), the expression `(a).GetResult()` is evaluated.</span></span> <span data-ttu-id="2886a-1656">Değer döndürürse, bu değer *await_expression*sonucudur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1656">If it returns a value, that value is the result of the *await_expression*.</span></span> <span data-ttu-id="2886a-1657">Aksi takdirde sonuç Nothing olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1657">Otherwise the result is nothing.</span></span>

<span data-ttu-id="2886a-1658">Bir awaiter 'ın `INotifyCompletion.OnCompleted` ve `ICriticalNotifyCompletion.UnsafeOnCompleted` Arabirim yöntemlerinin uygulanması, temsilci `r` en çok bir kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1658">An awaiter's implementation of the interface methods `INotifyCompletion.OnCompleted` and `ICriticalNotifyCompletion.UnsafeOnCompleted` should cause the delegate `r` to be invoked at most once.</span></span> <span data-ttu-id="2886a-1659">Aksi takdirde, kapsayan zaman uyumsuz işlevinin davranışı tanımsız olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1659">Otherwise, the behavior of the enclosing async function is undefined.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="2886a-1660">Aritmetik İşleçler</span><span class="sxs-lookup"><span data-stu-id="2886a-1660">Arithmetic operators</span></span>

<span data-ttu-id="2886a-1661">`*`, `/`, `%`, `+`ve `-` işleçleri aritmetik işleçler olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1661">The `*`, `/`, `%`, `+`, and `-` operators are called the arithmetic operators.</span></span>

```antlr
multiplicative_expression
    : unary_expression
    | multiplicative_expression '*' unary_expression
    | multiplicative_expression '/' unary_expression
    | multiplicative_expression '%' unary_expression
    ;

additive_expression
    : multiplicative_expression
    | additive_expression '+' multiplicative_expression
    | additive_expression '-' multiplicative_expression
    ;
```

<span data-ttu-id="2886a-1662">Aritmetik işlecin bir işleneni derleme zamanı türü `dynamic`, ifade dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="2886a-1662">If an operand of an arithmetic operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="2886a-1663">Bu durumda, ifadenin derleme zamanı türü `dynamic`ve aşağıda açıklanan çözüm, derleme zamanı türü `dynamic`sahip olan işlenenleri çalışma zamanı türü kullanılarak çalışma zamanında gerçekleşmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1663">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

### <a name="multiplication-operator"></a><span data-ttu-id="2886a-1664">Çarpma işleci</span><span class="sxs-lookup"><span data-stu-id="2886a-1664">Multiplication operator</span></span>

<span data-ttu-id="2886a-1665">`x * y`bir işlem için, belirli bir operatör uygulamasını seçmek üzere ikili işleç aşırı yükleme çözümü ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1665">For an operation of the form `x * y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="2886a-1666">İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1666">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="2886a-1667">Önceden tanımlanmış çarpma işleçleri aşağıda listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1667">The predefined multiplication operators are listed below.</span></span> <span data-ttu-id="2886a-1668">İşleçler `x` ve `y`çarpımını hesaplar.</span><span class="sxs-lookup"><span data-stu-id="2886a-1668">The operators all compute the product of `x` and `y`.</span></span>

*  <span data-ttu-id="2886a-1669">Tamsayı çarpma:</span><span class="sxs-lookup"><span data-stu-id="2886a-1669">Integer multiplication:</span></span>

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   <span data-ttu-id="2886a-1670">`checked` bağlamında, ürün sonuç türü aralığının dışındaysa, bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1670">In a `checked` context, if the product is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="2886a-1671">`unchecked` bağlamında, taşmalar raporlanmaz ve sonuç türü aralığı dışındaki önemli yüksek sıralı bitler atılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1671">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>


*  <span data-ttu-id="2886a-1672">Kayan nokta çarpma:</span><span class="sxs-lookup"><span data-stu-id="2886a-1672">Floating-point multiplication:</span></span>

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   <span data-ttu-id="2886a-1673">Ürün, IEEE 754 aritmetik kurallarına göre hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1673">The product is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="2886a-1674">Aşağıdaki tabloda, sıfır dışında sınırlı değer, sıfır, sonsuz ve NaN 'nin tüm olası birleşimlerinin sonuçları listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1674">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="2886a-1675">Tabloda, `x` ve `y` pozitif sonlu değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1675">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="2886a-1676">`z`, `x * y`sonucudur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1676">`z` is the result of `x * y`.</span></span> <span data-ttu-id="2886a-1677">Sonuç hedef türü için çok büyükse, `z` sonsuzluk olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1677">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="2886a-1678">Sonuç, hedef türü için çok küçükse, `z` sıfırdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1678">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | <span data-ttu-id="2886a-1679">\+ y</span><span class="sxs-lookup"><span data-stu-id="2886a-1679">+y</span></span>   | <span data-ttu-id="2886a-1680">-y</span><span class="sxs-lookup"><span data-stu-id="2886a-1680">-y</span></span>   | <span data-ttu-id="2886a-1681">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1681">+0</span></span>  | <span data-ttu-id="2886a-1682">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1682">-0</span></span>  | <span data-ttu-id="2886a-1683">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1683">+inf</span></span> | <span data-ttu-id="2886a-1684">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1684">-inf</span></span> | <span data-ttu-id="2886a-1685">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1685">NaN</span></span> | 
   | <span data-ttu-id="2886a-1686">{1&gt;+&lt;1}x</span><span class="sxs-lookup"><span data-stu-id="2886a-1686">+x</span></span>   | <span data-ttu-id="2886a-1687">\+ z</span><span class="sxs-lookup"><span data-stu-id="2886a-1687">+z</span></span>   | <span data-ttu-id="2886a-1688">-z</span><span class="sxs-lookup"><span data-stu-id="2886a-1688">-z</span></span>   | <span data-ttu-id="2886a-1689">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1689">+0</span></span>  | <span data-ttu-id="2886a-1690">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1690">-0</span></span>  | <span data-ttu-id="2886a-1691">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1691">+inf</span></span> | <span data-ttu-id="2886a-1692">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1692">-inf</span></span> | <span data-ttu-id="2886a-1693">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1693">NaN</span></span> | 
   | <span data-ttu-id="2886a-1694">{1&gt;-&lt;1}x</span><span class="sxs-lookup"><span data-stu-id="2886a-1694">-x</span></span>   | <span data-ttu-id="2886a-1695">-z</span><span class="sxs-lookup"><span data-stu-id="2886a-1695">-z</span></span>   | <span data-ttu-id="2886a-1696">\+ z</span><span class="sxs-lookup"><span data-stu-id="2886a-1696">+z</span></span>   | <span data-ttu-id="2886a-1697">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1697">-0</span></span>  | <span data-ttu-id="2886a-1698">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1698">+0</span></span>  | <span data-ttu-id="2886a-1699">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1699">-inf</span></span> | <span data-ttu-id="2886a-1700">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1700">+inf</span></span> | <span data-ttu-id="2886a-1701">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1701">NaN</span></span> | 
   | <span data-ttu-id="2886a-1702">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1702">+0</span></span>   | <span data-ttu-id="2886a-1703">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1703">+0</span></span>   | <span data-ttu-id="2886a-1704">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1704">-0</span></span>   | <span data-ttu-id="2886a-1705">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1705">+0</span></span>  | <span data-ttu-id="2886a-1706">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1706">-0</span></span>  | <span data-ttu-id="2886a-1707">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1707">NaN</span></span>  | <span data-ttu-id="2886a-1708">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1708">NaN</span></span>  | <span data-ttu-id="2886a-1709">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1709">NaN</span></span> | 
   | <span data-ttu-id="2886a-1710">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1710">-0</span></span>   | <span data-ttu-id="2886a-1711">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1711">-0</span></span>   | <span data-ttu-id="2886a-1712">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1712">+0</span></span>   | <span data-ttu-id="2886a-1713">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1713">-0</span></span>  | <span data-ttu-id="2886a-1714">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1714">+0</span></span>  | <span data-ttu-id="2886a-1715">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1715">NaN</span></span>  | <span data-ttu-id="2886a-1716">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1716">NaN</span></span>  | <span data-ttu-id="2886a-1717">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1717">NaN</span></span> | 
   | <span data-ttu-id="2886a-1718">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1718">+inf</span></span> | <span data-ttu-id="2886a-1719">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1719">+inf</span></span> | <span data-ttu-id="2886a-1720">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1720">-inf</span></span> | <span data-ttu-id="2886a-1721">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1721">NaN</span></span> | <span data-ttu-id="2886a-1722">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1722">NaN</span></span> | <span data-ttu-id="2886a-1723">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1723">+inf</span></span> | <span data-ttu-id="2886a-1724">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1724">-inf</span></span> | <span data-ttu-id="2886a-1725">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1725">NaN</span></span> | 
   | <span data-ttu-id="2886a-1726">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1726">-inf</span></span> | <span data-ttu-id="2886a-1727">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1727">-inf</span></span> | <span data-ttu-id="2886a-1728">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1728">+inf</span></span> | <span data-ttu-id="2886a-1729">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1729">NaN</span></span> | <span data-ttu-id="2886a-1730">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1730">NaN</span></span> | <span data-ttu-id="2886a-1731">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1731">-inf</span></span> | <span data-ttu-id="2886a-1732">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1732">+inf</span></span> | <span data-ttu-id="2886a-1733">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1733">NaN</span></span> | 
   | <span data-ttu-id="2886a-1734">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1734">NaN</span></span>  | <span data-ttu-id="2886a-1735">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1735">NaN</span></span>  | <span data-ttu-id="2886a-1736">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1736">NaN</span></span>  | <span data-ttu-id="2886a-1737">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1737">NaN</span></span> | <span data-ttu-id="2886a-1738">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1738">NaN</span></span> | <span data-ttu-id="2886a-1739">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1739">NaN</span></span>  | <span data-ttu-id="2886a-1740">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1740">NaN</span></span>  | <span data-ttu-id="2886a-1741">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1741">NaN</span></span> | 

*  <span data-ttu-id="2886a-1742">Ondalık çarpma:</span><span class="sxs-lookup"><span data-stu-id="2886a-1742">Decimal multiplication:</span></span>

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   <span data-ttu-id="2886a-1743">Elde edilen değer `decimal` biçimde temsil etmek için çok büyükse, bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1743">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="2886a-1744">Sonuç değeri `decimal` biçimde temsil etmek için çok küçük ise, sonuç sıfırdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1744">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="2886a-1745">Sonucun ölçeği, herhangi bir yuvarlamadan önce, iki işlenenin ölçeklerinin toplamıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1745">The scale of the result, before any rounding, is the sum of the scales of the two operands.</span></span>

   <span data-ttu-id="2886a-1746">Ondalık çarpma, `System.Decimal`türünde çarpma işlecinin kullanılmasıyla eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1746">Decimal multiplication is equivalent to using the multiplication operator of type `System.Decimal`.</span></span>


### <a name="division-operator"></a><span data-ttu-id="2886a-1747">Bölme işleci</span><span class="sxs-lookup"><span data-stu-id="2886a-1747">Division operator</span></span>

<span data-ttu-id="2886a-1748">`x / y`bir işlem için, belirli bir operatör uygulamasını seçmek üzere ikili işleç aşırı yükleme çözümü ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1748">For an operation of the form `x / y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="2886a-1749">İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1749">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="2886a-1750">Önceden tanımlanmış bölüm işleçleri aşağıda listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1750">The predefined division operators are listed below.</span></span> <span data-ttu-id="2886a-1751">İşleçler `x` ve `y`bölümünü hesaplar.</span><span class="sxs-lookup"><span data-stu-id="2886a-1751">The operators all compute the quotient of `x` and `y`.</span></span>

*  <span data-ttu-id="2886a-1752">Tamsayı bölme:</span><span class="sxs-lookup"><span data-stu-id="2886a-1752">Integer division:</span></span>

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   <span data-ttu-id="2886a-1753">Sağ işlenenin değeri sıfırsa, bir `System.DivideByZeroException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1753">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="2886a-1754">Bölme sonucu sıfıra doğru yuvarlar.</span><span class="sxs-lookup"><span data-stu-id="2886a-1754">The division rounds the result towards zero.</span></span> <span data-ttu-id="2886a-1755">Bu nedenle, sonucun mutlak değeri, iki işlenenin alanının mutlak değerinden küçük veya ona eşit olan en büyük olası tamsayıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1755">Thus the absolute value of the result is the largest possible integer that is less than or equal to the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="2886a-1756">İki işlenen iki işlenen de ters işaret sahibi olduğunda, sonuç sıfır veya pozitif, sıfır veya negatif olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1756">The result is zero or positive when the two operands have the same sign and zero or negative when the two operands have opposite signs.</span></span>

   <span data-ttu-id="2886a-1757">Sol işlenen en küçük gösterilemeyen `int` veya `long` değeri ve sağ işlenen `-1`ise, bir taşma oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1757">If the left operand is the smallest representable `int` or `long` value and the right operand is `-1`, an overflow occurs.</span></span> <span data-ttu-id="2886a-1758">`checked` bağlamında, bu, bir `System.ArithmeticException` (veya alt sınıfın) oluşturulmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1758">In a `checked` context, this causes a `System.ArithmeticException` (or a subclass thereof) to be thrown.</span></span> <span data-ttu-id="2886a-1759">`unchecked` bağlamında, uygulama tanımlı bir `System.ArithmeticException` (veya alt sınıfın) oluşturulup oluşturulmayacağını ya da taşma değeri sol işlenenin elde edilen değerle bildirilmeyen bir şekilde geçer.</span><span class="sxs-lookup"><span data-stu-id="2886a-1759">In an `unchecked` context, it is implementation-defined as to whether a `System.ArithmeticException` (or a subclass thereof) is thrown or the overflow goes unreported with the resulting value being that of the left operand.</span></span>

*  <span data-ttu-id="2886a-1760">Kayan nokta bölmesi:</span><span class="sxs-lookup"><span data-stu-id="2886a-1760">Floating-point division:</span></span>

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   <span data-ttu-id="2886a-1761">Bu bölüm, IEEE 754 aritmetiğinin kurallarına göre hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1761">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="2886a-1762">Aşağıdaki tabloda, sıfır dışında sınırlı değer, sıfır, sonsuz ve NaN 'nin tüm olası birleşimlerinin sonuçları listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1762">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="2886a-1763">Tabloda, `x` ve `y` pozitif sonlu değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1763">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="2886a-1764">`z`, `x / y`sonucudur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1764">`z` is the result of `x / y`.</span></span> <span data-ttu-id="2886a-1765">Sonuç hedef türü için çok büyükse, `z` sonsuzluk olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1765">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="2886a-1766">Sonuç, hedef türü için çok küçükse, `z` sıfırdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1766">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="2886a-1767">\+ y</span><span class="sxs-lookup"><span data-stu-id="2886a-1767">+y</span></span>   | <span data-ttu-id="2886a-1768">-y</span><span class="sxs-lookup"><span data-stu-id="2886a-1768">-y</span></span>   | <span data-ttu-id="2886a-1769">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1769">+0</span></span>   | <span data-ttu-id="2886a-1770">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1770">-0</span></span>   | <span data-ttu-id="2886a-1771">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1771">+inf</span></span> | <span data-ttu-id="2886a-1772">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1772">-inf</span></span> | <span data-ttu-id="2886a-1773">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1773">NaN</span></span>  | 
   | <span data-ttu-id="2886a-1774">{1&gt;+&lt;1}x</span><span class="sxs-lookup"><span data-stu-id="2886a-1774">+x</span></span>   | <span data-ttu-id="2886a-1775">\+ z</span><span class="sxs-lookup"><span data-stu-id="2886a-1775">+z</span></span>   | <span data-ttu-id="2886a-1776">-z</span><span class="sxs-lookup"><span data-stu-id="2886a-1776">-z</span></span>   | <span data-ttu-id="2886a-1777">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1777">+inf</span></span> | <span data-ttu-id="2886a-1778">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1778">-inf</span></span> | <span data-ttu-id="2886a-1779">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1779">+0</span></span>   | <span data-ttu-id="2886a-1780">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1780">-0</span></span>   | <span data-ttu-id="2886a-1781">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1781">NaN</span></span>  | 
   | <span data-ttu-id="2886a-1782">{1&gt;-&lt;1}x</span><span class="sxs-lookup"><span data-stu-id="2886a-1782">-x</span></span>   | <span data-ttu-id="2886a-1783">-z</span><span class="sxs-lookup"><span data-stu-id="2886a-1783">-z</span></span>   | <span data-ttu-id="2886a-1784">\+ z</span><span class="sxs-lookup"><span data-stu-id="2886a-1784">+z</span></span>   | <span data-ttu-id="2886a-1785">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1785">-inf</span></span> | <span data-ttu-id="2886a-1786">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1786">+inf</span></span> | <span data-ttu-id="2886a-1787">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1787">-0</span></span>   | <span data-ttu-id="2886a-1788">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1788">+0</span></span>   | <span data-ttu-id="2886a-1789">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1789">NaN</span></span>  | 
   | <span data-ttu-id="2886a-1790">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1790">+0</span></span>   | <span data-ttu-id="2886a-1791">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1791">+0</span></span>   | <span data-ttu-id="2886a-1792">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1792">-0</span></span>   | <span data-ttu-id="2886a-1793">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1793">NaN</span></span>  | <span data-ttu-id="2886a-1794">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1794">NaN</span></span>  | <span data-ttu-id="2886a-1795">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1795">+0</span></span>   | <span data-ttu-id="2886a-1796">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1796">-0</span></span>   | <span data-ttu-id="2886a-1797">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1797">NaN</span></span>  | 
   | <span data-ttu-id="2886a-1798">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1798">-0</span></span>   | <span data-ttu-id="2886a-1799">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1799">-0</span></span>   | <span data-ttu-id="2886a-1800">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1800">+0</span></span>   | <span data-ttu-id="2886a-1801">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1801">NaN</span></span>  | <span data-ttu-id="2886a-1802">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1802">NaN</span></span>  | <span data-ttu-id="2886a-1803">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1803">-0</span></span>   | <span data-ttu-id="2886a-1804">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1804">+0</span></span>   | <span data-ttu-id="2886a-1805">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1805">NaN</span></span>  | 
   | <span data-ttu-id="2886a-1806">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1806">+inf</span></span> | <span data-ttu-id="2886a-1807">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1807">+inf</span></span> | <span data-ttu-id="2886a-1808">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1808">-inf</span></span> | <span data-ttu-id="2886a-1809">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1809">+inf</span></span> | <span data-ttu-id="2886a-1810">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1810">-inf</span></span> | <span data-ttu-id="2886a-1811">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1811">NaN</span></span>  | <span data-ttu-id="2886a-1812">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1812">NaN</span></span>  | <span data-ttu-id="2886a-1813">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1813">NaN</span></span>  | 
   | <span data-ttu-id="2886a-1814">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1814">-inf</span></span> | <span data-ttu-id="2886a-1815">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1815">-inf</span></span> | <span data-ttu-id="2886a-1816">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1816">+inf</span></span> | <span data-ttu-id="2886a-1817">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1817">-inf</span></span> | <span data-ttu-id="2886a-1818">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1818">+inf</span></span> | <span data-ttu-id="2886a-1819">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1819">NaN</span></span>  | <span data-ttu-id="2886a-1820">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1820">NaN</span></span>  | <span data-ttu-id="2886a-1821">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1821">NaN</span></span>  | 
   | <span data-ttu-id="2886a-1822">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1822">NaN</span></span>  | <span data-ttu-id="2886a-1823">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1823">NaN</span></span>  | <span data-ttu-id="2886a-1824">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1824">NaN</span></span>  | <span data-ttu-id="2886a-1825">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1825">NaN</span></span>  | <span data-ttu-id="2886a-1826">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1826">NaN</span></span>  | <span data-ttu-id="2886a-1827">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1827">NaN</span></span>  | <span data-ttu-id="2886a-1828">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1828">NaN</span></span>  | <span data-ttu-id="2886a-1829">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1829">NaN</span></span>  | 

*  <span data-ttu-id="2886a-1830">Ondalık bölme:</span><span class="sxs-lookup"><span data-stu-id="2886a-1830">Decimal division:</span></span>

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   <span data-ttu-id="2886a-1831">Sağ işlenenin değeri sıfırsa, bir `System.DivideByZeroException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1831">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="2886a-1832">Elde edilen değer `decimal` biçimde temsil etmek için çok büyükse, bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1832">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="2886a-1833">Sonuç değeri `decimal` biçimde temsil etmek için çok küçük ise, sonuç sıfırdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1833">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="2886a-1834">Sonucun ölçeği, en yakın gösterilebilir tablo ondalık değerine eşit olan bir sonucu doğru matematik sonucuyla koruyacak en küçük ölçeğe neden olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1834">The scale of the result is the smallest scale that will preserve a result equal to the nearest representable decimal value to the true mathematical result.</span></span>

   <span data-ttu-id="2886a-1835">Ondalık bölme `System.Decimal`türünde Bölme işlecinin kullanılmasıyla eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1835">Decimal division is equivalent to using the division operator of type `System.Decimal`.</span></span>


### <a name="remainder-operator"></a><span data-ttu-id="2886a-1836">Kalan işleç</span><span class="sxs-lookup"><span data-stu-id="2886a-1836">Remainder operator</span></span>

<span data-ttu-id="2886a-1837">`x % y`bir işlem için, belirli bir operatör uygulamasını seçmek üzere ikili işleç aşırı yükleme çözümü ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1837">For an operation of the form `x % y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="2886a-1838">İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1838">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="2886a-1839">Önceden tanımlanmış geri kalan işleçler aşağıda listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1839">The predefined remainder operators are listed below.</span></span> <span data-ttu-id="2886a-1840">İşleçler, `x` ve `y`arasında bölmenin geri kalanını hesaplar.</span><span class="sxs-lookup"><span data-stu-id="2886a-1840">The operators all compute the remainder of the division between `x` and `y`.</span></span>

*  <span data-ttu-id="2886a-1841">Tamsayı geri kalanı:</span><span class="sxs-lookup"><span data-stu-id="2886a-1841">Integer remainder:</span></span>

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   <span data-ttu-id="2886a-1842">`x % y` sonucu, `x - (x / y) * y`tarafından üretilen değerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1842">The result of `x % y` is the value produced by `x - (x / y) * y`.</span></span> <span data-ttu-id="2886a-1843">`y` sıfırsa, bir `System.DivideByZeroException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1843">If `y` is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="2886a-1844">Sol işlenen en küçük `int` veya `long` değeri ise ve sağ işlenen `-1`, bir `System.OverflowException` atılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1844">If the left operand is the smallest `int` or `long` value and the right operand is `-1`, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="2886a-1845">Hiçbir durumda `x % y` bir özel durum oluşturmayan bir özel `x / y` durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1845">In no case does `x % y` throw an exception where `x / y` would not throw an exception.</span></span>

*  <span data-ttu-id="2886a-1846">Kayan nokta kalanı:</span><span class="sxs-lookup"><span data-stu-id="2886a-1846">Floating-point remainder:</span></span>

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   <span data-ttu-id="2886a-1847">Aşağıdaki tabloda, sıfır dışında sınırlı değer, sıfır, sonsuz ve NaN 'nin tüm olası birleşimlerinin sonuçları listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1847">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="2886a-1848">Tabloda, `x` ve `y` pozitif sonlu değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1848">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="2886a-1849">`z`, `x % y` sonucudur ve `x - n * y`olarak hesaplanır; burada `n`, `x / y`küçük veya eşit olan en büyük tamsayıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1849">`z` is the result of `x % y` and is computed as `x - n * y`, where `n` is the largest possible integer that is less than or equal to `x / y`.</span></span> <span data-ttu-id="2886a-1850">Bu geri kalanı hesaplama yöntemi, tamsayı işlenenleri için kullanılan ile benzerdir, ancak IEEE 754 tanımından (`n` `x / y`en yakın tamsayıdır) farklıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1850">This method of computing the remainder is analogous to that used for integer operands, but differs from the IEEE 754 definition (in which `n` is the integer closest to `x / y`).</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="2886a-1851">\+ y</span><span class="sxs-lookup"><span data-stu-id="2886a-1851">+y</span></span>   | <span data-ttu-id="2886a-1852">-y</span><span class="sxs-lookup"><span data-stu-id="2886a-1852">-y</span></span>   | <span data-ttu-id="2886a-1853">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1853">+0</span></span>   | <span data-ttu-id="2886a-1854">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1854">-0</span></span>   | <span data-ttu-id="2886a-1855">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1855">+inf</span></span> | <span data-ttu-id="2886a-1856">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1856">-inf</span></span> | <span data-ttu-id="2886a-1857">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1857">NaN</span></span>  | 
   | <span data-ttu-id="2886a-1858">{1&gt;+&lt;1}x</span><span class="sxs-lookup"><span data-stu-id="2886a-1858">+x</span></span>   | <span data-ttu-id="2886a-1859">\+ z</span><span class="sxs-lookup"><span data-stu-id="2886a-1859">+z</span></span>   | <span data-ttu-id="2886a-1860">\+ z</span><span class="sxs-lookup"><span data-stu-id="2886a-1860">+z</span></span>   | <span data-ttu-id="2886a-1861">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1861">NaN</span></span>  | <span data-ttu-id="2886a-1862">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1862">NaN</span></span>  | <span data-ttu-id="2886a-1863">x</span><span class="sxs-lookup"><span data-stu-id="2886a-1863">x</span></span>    | <span data-ttu-id="2886a-1864">x</span><span class="sxs-lookup"><span data-stu-id="2886a-1864">x</span></span>    | <span data-ttu-id="2886a-1865">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1865">NaN</span></span>  | 
   | <span data-ttu-id="2886a-1866">{1&gt;-&lt;1}x</span><span class="sxs-lookup"><span data-stu-id="2886a-1866">-x</span></span>   | <span data-ttu-id="2886a-1867">-z</span><span class="sxs-lookup"><span data-stu-id="2886a-1867">-z</span></span>   | <span data-ttu-id="2886a-1868">-z</span><span class="sxs-lookup"><span data-stu-id="2886a-1868">-z</span></span>   | <span data-ttu-id="2886a-1869">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1869">NaN</span></span>  | <span data-ttu-id="2886a-1870">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1870">NaN</span></span>  | <span data-ttu-id="2886a-1871">{1&gt;-&lt;1}x</span><span class="sxs-lookup"><span data-stu-id="2886a-1871">-x</span></span>   | <span data-ttu-id="2886a-1872">{1&gt;-&lt;1}x</span><span class="sxs-lookup"><span data-stu-id="2886a-1872">-x</span></span>   | <span data-ttu-id="2886a-1873">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1873">NaN</span></span>  | 
   | <span data-ttu-id="2886a-1874">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1874">+0</span></span>   | <span data-ttu-id="2886a-1875">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1875">+0</span></span>   | <span data-ttu-id="2886a-1876">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1876">+0</span></span>   | <span data-ttu-id="2886a-1877">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1877">NaN</span></span>  | <span data-ttu-id="2886a-1878">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1878">NaN</span></span>  | <span data-ttu-id="2886a-1879">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1879">+0</span></span>   | <span data-ttu-id="2886a-1880">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1880">+0</span></span>   | <span data-ttu-id="2886a-1881">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1881">NaN</span></span>  | 
   | <span data-ttu-id="2886a-1882">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1882">-0</span></span>   | <span data-ttu-id="2886a-1883">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1883">-0</span></span>   | <span data-ttu-id="2886a-1884">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1884">-0</span></span>   | <span data-ttu-id="2886a-1885">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1885">NaN</span></span>  | <span data-ttu-id="2886a-1886">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1886">NaN</span></span>  | <span data-ttu-id="2886a-1887">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1887">-0</span></span>   | <span data-ttu-id="2886a-1888">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1888">-0</span></span>   | <span data-ttu-id="2886a-1889">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1889">NaN</span></span>  | 
   | <span data-ttu-id="2886a-1890">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1890">+inf</span></span> | <span data-ttu-id="2886a-1891">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1891">NaN</span></span>  | <span data-ttu-id="2886a-1892">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1892">NaN</span></span>  | <span data-ttu-id="2886a-1893">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1893">NaN</span></span>  | <span data-ttu-id="2886a-1894">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1894">NaN</span></span>  | <span data-ttu-id="2886a-1895">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1895">NaN</span></span>  | <span data-ttu-id="2886a-1896">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1896">NaN</span></span>  | <span data-ttu-id="2886a-1897">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1897">NaN</span></span>  | 
   | <span data-ttu-id="2886a-1898">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1898">-inf</span></span> | <span data-ttu-id="2886a-1899">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1899">NaN</span></span>  | <span data-ttu-id="2886a-1900">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1900">NaN</span></span>  | <span data-ttu-id="2886a-1901">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1901">NaN</span></span>  | <span data-ttu-id="2886a-1902">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1902">NaN</span></span>  | <span data-ttu-id="2886a-1903">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1903">NaN</span></span>  | <span data-ttu-id="2886a-1904">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1904">NaN</span></span>  | <span data-ttu-id="2886a-1905">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1905">NaN</span></span>  | 
   | <span data-ttu-id="2886a-1906">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1906">NaN</span></span>  | <span data-ttu-id="2886a-1907">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1907">NaN</span></span>  | <span data-ttu-id="2886a-1908">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1908">NaN</span></span>  | <span data-ttu-id="2886a-1909">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1909">NaN</span></span>  | <span data-ttu-id="2886a-1910">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1910">NaN</span></span>  | <span data-ttu-id="2886a-1911">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1911">NaN</span></span>  | <span data-ttu-id="2886a-1912">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1912">NaN</span></span>  | <span data-ttu-id="2886a-1913">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1913">NaN</span></span>  | 

*  <span data-ttu-id="2886a-1914">Ondalık kalan:</span><span class="sxs-lookup"><span data-stu-id="2886a-1914">Decimal remainder:</span></span>

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   <span data-ttu-id="2886a-1915">Sağ işlenenin değeri sıfırsa, bir `System.DivideByZeroException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1915">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="2886a-1916">Sonucun ölçeği, herhangi bir yuvarlamadan önce, iki işlenenin ölçeklerinin daha büyük olması ve sıfır olmayan bir değer varsa, `x`işaretiyle aynıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1916">The scale of the result, before any rounding, is the larger of the scales of the two operands, and the sign of the result, if non-zero, is the same as that of `x`.</span></span>

   <span data-ttu-id="2886a-1917">Ondalık geri kalanı `System.Decimal`türündeki kalan işlecin kullanılmasıyla eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1917">Decimal remainder is equivalent to using the remainder operator of type `System.Decimal`.</span></span>


### <a name="addition-operator"></a><span data-ttu-id="2886a-1918">Toplama işleci</span><span class="sxs-lookup"><span data-stu-id="2886a-1918">Addition operator</span></span>

<span data-ttu-id="2886a-1919">`x + y`bir işlem için, belirli bir operatör uygulamasını seçmek üzere ikili işleç aşırı yükleme çözümü ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1919">For an operation of the form `x + y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="2886a-1920">İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1920">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="2886a-1921">Önceden tanımlanmış ekleme işleçleri aşağıda listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1921">The predefined addition operators are listed below.</span></span> <span data-ttu-id="2886a-1922">Sayısal ve sabit listesi türlerinde, önceden tanımlanmış ekleme işleçleri iki işlenenin toplamını hesaplar.</span><span class="sxs-lookup"><span data-stu-id="2886a-1922">For numeric and enumeration types, the predefined addition operators compute the sum of the two operands.</span></span> <span data-ttu-id="2886a-1923">Bir veya her iki işlenen de dize türünde olduğunda, önceden tanımlanmış toplama işleçleri işlenen dize gösterimini birleştirir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1923">When one or both operands are of type string, the predefined addition operators concatenate the string representation of the operands.</span></span>

*  <span data-ttu-id="2886a-1924">Tamsayı ekleme:</span><span class="sxs-lookup"><span data-stu-id="2886a-1924">Integer addition:</span></span>

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   <span data-ttu-id="2886a-1925">`checked` bağlamında, Toplam Sonuç türü aralığının dışındaysa, bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1925">In a `checked` context, if the sum is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="2886a-1926">`unchecked` bağlamında, taşmalar raporlanmaz ve sonuç türü aralığı dışındaki önemli yüksek sıralı bitler atılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1926">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="2886a-1927">Kayan nokta ekleme:</span><span class="sxs-lookup"><span data-stu-id="2886a-1927">Floating-point addition:</span></span>

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   <span data-ttu-id="2886a-1928">Toplam, IEEE 754 aritmetiğinin kurallarına göre hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1928">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="2886a-1929">Aşağıdaki tabloda, sıfır dışında sınırlı değer, sıfır, sonsuz ve NaN 'nin tüm olası birleşimlerinin sonuçları listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1929">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="2886a-1930">Tabloda, `x` ve `y` sıfır dışında sonlu değerlerdir ve `z` `x + y`sonucudur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1930">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x + y`.</span></span> <span data-ttu-id="2886a-1931">`x` ve `y` aynı büyüklük, ancak ters işaretlere sahipse, `z` pozitif sıfırdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-1931">If `x` and `y` have the same magnitude but opposite signs, `z` is positive zero.</span></span> <span data-ttu-id="2886a-1932">`x + y` hedef türünde temsil etmek için çok büyükse, `z`, `x + y`aynı işareti içeren bir sonsuzluk olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1932">If `x + y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x + y`.</span></span>

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="2886a-1933">{1&gt;y&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1933">y</span></span>    | <span data-ttu-id="2886a-1934">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1934">+0</span></span>   | <span data-ttu-id="2886a-1935">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1935">-0</span></span>   | <span data-ttu-id="2886a-1936">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1936">+inf</span></span> | <span data-ttu-id="2886a-1937">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1937">-inf</span></span> | <span data-ttu-id="2886a-1938">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1938">NaN</span></span>  | 
   | <span data-ttu-id="2886a-1939">x</span><span class="sxs-lookup"><span data-stu-id="2886a-1939">x</span></span>    | <span data-ttu-id="2886a-1940">z</span><span class="sxs-lookup"><span data-stu-id="2886a-1940">z</span></span>    | <span data-ttu-id="2886a-1941">x</span><span class="sxs-lookup"><span data-stu-id="2886a-1941">x</span></span>    | <span data-ttu-id="2886a-1942">x</span><span class="sxs-lookup"><span data-stu-id="2886a-1942">x</span></span>    | <span data-ttu-id="2886a-1943">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1943">+inf</span></span> | <span data-ttu-id="2886a-1944">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1944">-inf</span></span> | <span data-ttu-id="2886a-1945">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1945">NaN</span></span>  | 
   | <span data-ttu-id="2886a-1946">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1946">+0</span></span>   | <span data-ttu-id="2886a-1947">{1&gt;y&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1947">y</span></span>    | <span data-ttu-id="2886a-1948">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1948">+0</span></span>   | <span data-ttu-id="2886a-1949">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1949">+0</span></span>   | <span data-ttu-id="2886a-1950">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1950">+inf</span></span> | <span data-ttu-id="2886a-1951">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1951">-inf</span></span> | <span data-ttu-id="2886a-1952">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1952">NaN</span></span>  | 
   | <span data-ttu-id="2886a-1953">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1953">-0</span></span>   | <span data-ttu-id="2886a-1954">{1&gt;y&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1954">y</span></span>    | <span data-ttu-id="2886a-1955">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-1955">+0</span></span>   | <span data-ttu-id="2886a-1956">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-1956">-0</span></span>   | <span data-ttu-id="2886a-1957">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1957">+inf</span></span> | <span data-ttu-id="2886a-1958">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1958">-inf</span></span> | <span data-ttu-id="2886a-1959">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1959">NaN</span></span>  | 
   | <span data-ttu-id="2886a-1960">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1960">+inf</span></span> | <span data-ttu-id="2886a-1961">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1961">+inf</span></span> | <span data-ttu-id="2886a-1962">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1962">+inf</span></span> | <span data-ttu-id="2886a-1963">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1963">+inf</span></span> | <span data-ttu-id="2886a-1964">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1964">+inf</span></span> | <span data-ttu-id="2886a-1965">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1965">NaN</span></span>  | <span data-ttu-id="2886a-1966">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1966">NaN</span></span>  | 
   | <span data-ttu-id="2886a-1967">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1967">-inf</span></span> | <span data-ttu-id="2886a-1968">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1968">-inf</span></span> | <span data-ttu-id="2886a-1969">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1969">-inf</span></span> | <span data-ttu-id="2886a-1970">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1970">-inf</span></span> | <span data-ttu-id="2886a-1971">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1971">NaN</span></span>  | <span data-ttu-id="2886a-1972">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-1972">-inf</span></span> | <span data-ttu-id="2886a-1973">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1973">NaN</span></span>  | 
   | <span data-ttu-id="2886a-1974">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1974">NaN</span></span>  | <span data-ttu-id="2886a-1975">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1975">NaN</span></span>  | <span data-ttu-id="2886a-1976">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1976">NaN</span></span>  | <span data-ttu-id="2886a-1977">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1977">NaN</span></span>  | <span data-ttu-id="2886a-1978">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1978">NaN</span></span>  | <span data-ttu-id="2886a-1979">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1979">NaN</span></span>  | <span data-ttu-id="2886a-1980">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-1980">NaN</span></span>  | 

*  <span data-ttu-id="2886a-1981">Ondalık ekleme:</span><span class="sxs-lookup"><span data-stu-id="2886a-1981">Decimal addition:</span></span>

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   <span data-ttu-id="2886a-1982">Elde edilen değer `decimal` biçimde temsil etmek için çok büyükse, bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-1982">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="2886a-1983">Sonucun ölçeği, herhangi bir yuvarlamadan önce, iki işlenenin ölçeklendirilmesine göre daha büyüktür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1983">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="2886a-1984">Ondalık ekleme, `System.Decimal`türünde toplama işlecinin kullanılmasıyla eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1984">Decimal addition is equivalent to using the addition operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="2886a-1985">Numaralandırma ekleme.</span><span class="sxs-lookup"><span data-stu-id="2886a-1985">Enumeration addition.</span></span> <span data-ttu-id="2886a-1986">Her numaralandırma türü örtük olarak aşağıdaki önceden tanımlı işleçleri sağlar; burada `E` sabit listesi türüdür ve `U` `E`temel türüdür:</span><span class="sxs-lookup"><span data-stu-id="2886a-1986">Every enumeration type implicitly provides the following predefined operators, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   <span data-ttu-id="2886a-1987">Çalışma zamanında bu işleçler tam olarak `(E)((U)x + (U)y)`olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1987">At run-time these operators are evaluated exactly as `(E)((U)x + (U)y)`.</span></span>

*  <span data-ttu-id="2886a-1988">Dize birleştirme:</span><span class="sxs-lookup"><span data-stu-id="2886a-1988">String concatenation:</span></span>

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   <span data-ttu-id="2886a-1989">İkili `+` işlecinin bu aşırı yüklemeleri dize birleştirmesi gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1989">These overloads of the binary `+` operator perform string concatenation.</span></span> <span data-ttu-id="2886a-1990">Dize birleştirme işleneni `null`ise boş bir dize değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1990">If an operand of string concatenation is `null`, an empty string is substituted.</span></span> <span data-ttu-id="2886a-1991">Aksi takdirde, dize olmayan herhangi bir bağımsız değişken `object`türünden devralınan sanal `ToString` yöntemi çağrılarak dize gösterimine dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="2886a-1991">Otherwise, any non-string argument is converted to its string representation by invoking the virtual `ToString` method inherited from type `object`.</span></span> <span data-ttu-id="2886a-1992">`ToString` `null`döndürürse boş bir dize değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1992">If `ToString` returns `null`, an empty string is substituted.</span></span>

   ```csharp
   using System;
   
   class Test
   {
       static void Main() {
           string s = null;
           Console.WriteLine("s = >" + s + "<");        // displays s = ><
           int i = 1;
           Console.WriteLine("i = " + i);               // displays i = 1
           float f = 1.2300E+15F;
           Console.WriteLine("f = " + f);               // displays f = 1.23E+15
           decimal d = 2.900m;
           Console.WriteLine("d = " + d);               // displays d = 2.900
       }
   }
   ```

   Dize birleştirme işlecinin sonucu, sol işlenenin karakterlerinden ve ardından sağ işlenenin karakterlerinden oluşan bir dizedir. Dize birleştirme işleci hiçbir şekilde `null` değer döndürmez. <span data-ttu-id="2886a-1995">Elde edilen dizeyi ayırmak için yeterli kullanılabilir bellek yoksa bir `System.OutOfMemoryException` oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1995">A `System.OutOfMemoryException` may be thrown if there is not enough memory available to allocate the resulting string.</span></span>

*  <span data-ttu-id="2886a-1996">Temsilci birleşimi.</span><span class="sxs-lookup"><span data-stu-id="2886a-1996">Delegate combination.</span></span> <span data-ttu-id="2886a-1997">Her temsilci türü örtük olarak aşağıdaki önceden tanımlı işleci sağlar; burada `D` temsilci türüdür:</span><span class="sxs-lookup"><span data-stu-id="2886a-1997">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator +(D x, D y);
   ```

   <span data-ttu-id="2886a-1998">İkili `+` işleci, her iki işlenen de `D`bazı temsilci türleri olduğunda temsilci bileşimini gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="2886a-1998">The binary `+` operator performs delegate combination when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="2886a-1999">(İşlenenler farklı temsilci türlerine sahip ise, bir bağlama zamanı hatası oluşur.) İlk işlenen `null`, işlemin sonucu ikinci işlenenin değeridir (aynı zamanda `null`olsa bile).</span><span class="sxs-lookup"><span data-stu-id="2886a-1999">(If the operands have different delegate types, a binding-time error occurs.) If the first operand is `null`, the result of the operation is the value of the second operand (even if that is also `null`).</span></span> <span data-ttu-id="2886a-2000">Aksi takdirde, ikinci işlenen `null`, işlemin sonucu ilk işlenenin değeridir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2000">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="2886a-2001">Aksi takdirde, işlem sonucu çağrıldığında, ilk işleneni çağırır ve sonra ikinci işleneni çağıran yeni bir temsilci örneğidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2001">Otherwise, the result of the operation is a new delegate instance that, when invoked, invokes the first operand and then invokes the second operand.</span></span> <span data-ttu-id="2886a-2002">Temsilci birleşimi örnekleri için bkz. [çıkarma işleci](expressions.md#subtraction-operator) ve [temsilci çağrısı](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="2886a-2002">For examples of delegate combination, see [Subtraction operator](expressions.md#subtraction-operator) and [Delegate invocation](delegates.md#delegate-invocation).</span></span> <span data-ttu-id="2886a-2003">`System.Delegate` bir temsilci türü olmadığından, `operator` `+` tanımlı değil.</span><span class="sxs-lookup"><span data-stu-id="2886a-2003">Since `System.Delegate` is not a delegate type, `operator` `+` is not defined for it.</span></span>

### <a name="subtraction-operator"></a><span data-ttu-id="2886a-2004">Çıkarma işleci</span><span class="sxs-lookup"><span data-stu-id="2886a-2004">Subtraction operator</span></span>

<span data-ttu-id="2886a-2005">`x - y`bir işlem için, belirli bir operatör uygulamasını seçmek üzere ikili işleç aşırı yükleme çözümü ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2005">For an operation of the form `x - y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="2886a-2006">İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2006">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="2886a-2007">Önceden tanımlanmış çıkarma işleçleri aşağıda listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2007">The predefined subtraction operators are listed below.</span></span> <span data-ttu-id="2886a-2008">İşleçler `x``y` tüm çıkar.</span><span class="sxs-lookup"><span data-stu-id="2886a-2008">The operators all subtract `y` from `x`.</span></span>

*  <span data-ttu-id="2886a-2009">Tamsayı çıkarma:</span><span class="sxs-lookup"><span data-stu-id="2886a-2009">Integer subtraction:</span></span>

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   <span data-ttu-id="2886a-2010">`checked` bağlamında, fark sonuç türü aralığının dışındaysa, bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2010">In a `checked` context, if the difference is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="2886a-2011">`unchecked` bağlamında, taşmalar raporlanmaz ve sonuç türü aralığı dışındaki önemli yüksek sıralı bitler atılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2011">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="2886a-2012">Kayan nokta çıkarma:</span><span class="sxs-lookup"><span data-stu-id="2886a-2012">Floating-point subtraction:</span></span>

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   <span data-ttu-id="2886a-2013">Aradaki fark, IEEE 754 aritmetiğinin kurallarına göre hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2013">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="2886a-2014">Aşağıdaki tabloda, sıfır dışında sınırlı değer, sıfır, sonsuz ve NaNs 'ın tüm olası birleşimlerinin sonuçları listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2014">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaNs.</span></span> <span data-ttu-id="2886a-2015">Tabloda, `x` ve `y` sıfır dışında sonlu değerlerdir ve `z` `x - y`sonucudur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2015">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x - y`.</span></span> <span data-ttu-id="2886a-2016">`x` ve `y` eşitse, `z` pozitif sıfırdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2016">If `x` and `y` are equal, `z` is positive zero.</span></span> <span data-ttu-id="2886a-2017">`x - y` hedef türünde temsil etmek için çok büyükse, `z`, `x - y`aynı işareti içeren bir sonsuzluk olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2017">If `x - y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x - y`.</span></span>

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | <span data-ttu-id="2886a-2018">{1&gt;y&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-2018">y</span></span>    | <span data-ttu-id="2886a-2019">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-2019">+0</span></span>   | <span data-ttu-id="2886a-2020">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-2020">-0</span></span>   | <span data-ttu-id="2886a-2021">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-2021">+inf</span></span> | <span data-ttu-id="2886a-2022">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-2022">-inf</span></span> | <span data-ttu-id="2886a-2023">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-2023">NaN</span></span> | 
   | <span data-ttu-id="2886a-2024">x</span><span class="sxs-lookup"><span data-stu-id="2886a-2024">x</span></span>    | <span data-ttu-id="2886a-2025">z</span><span class="sxs-lookup"><span data-stu-id="2886a-2025">z</span></span>    | <span data-ttu-id="2886a-2026">x</span><span class="sxs-lookup"><span data-stu-id="2886a-2026">x</span></span>    | <span data-ttu-id="2886a-2027">x</span><span class="sxs-lookup"><span data-stu-id="2886a-2027">x</span></span>    | <span data-ttu-id="2886a-2028">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-2028">-inf</span></span> | <span data-ttu-id="2886a-2029">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-2029">+inf</span></span> | <span data-ttu-id="2886a-2030">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-2030">NaN</span></span> | 
   | <span data-ttu-id="2886a-2031">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-2031">+0</span></span>   | <span data-ttu-id="2886a-2032">-y</span><span class="sxs-lookup"><span data-stu-id="2886a-2032">-y</span></span>   | <span data-ttu-id="2886a-2033">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-2033">+0</span></span>   | <span data-ttu-id="2886a-2034">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-2034">+0</span></span>   | <span data-ttu-id="2886a-2035">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-2035">-inf</span></span> | <span data-ttu-id="2886a-2036">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-2036">+inf</span></span> | <span data-ttu-id="2886a-2037">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-2037">NaN</span></span> | 
   | <span data-ttu-id="2886a-2038">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-2038">-0</span></span>   | <span data-ttu-id="2886a-2039">-y</span><span class="sxs-lookup"><span data-stu-id="2886a-2039">-y</span></span>   | <span data-ttu-id="2886a-2040">-0</span><span class="sxs-lookup"><span data-stu-id="2886a-2040">-0</span></span>   | <span data-ttu-id="2886a-2041">+0</span><span class="sxs-lookup"><span data-stu-id="2886a-2041">+0</span></span>   | <span data-ttu-id="2886a-2042">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-2042">-inf</span></span> | <span data-ttu-id="2886a-2043">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-2043">+inf</span></span> | <span data-ttu-id="2886a-2044">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-2044">NaN</span></span> | 
   | <span data-ttu-id="2886a-2045">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-2045">+inf</span></span> | <span data-ttu-id="2886a-2046">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-2046">+inf</span></span> | <span data-ttu-id="2886a-2047">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-2047">+inf</span></span> | <span data-ttu-id="2886a-2048">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-2048">+inf</span></span> | <span data-ttu-id="2886a-2049">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-2049">NaN</span></span>  | <span data-ttu-id="2886a-2050">\+ INF</span><span class="sxs-lookup"><span data-stu-id="2886a-2050">+inf</span></span> | <span data-ttu-id="2886a-2051">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-2051">NaN</span></span> | 
   | <span data-ttu-id="2886a-2052">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-2052">-inf</span></span> | <span data-ttu-id="2886a-2053">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-2053">-inf</span></span> | <span data-ttu-id="2886a-2054">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-2054">-inf</span></span> | <span data-ttu-id="2886a-2055">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-2055">-inf</span></span> | <span data-ttu-id="2886a-2056">-INF</span><span class="sxs-lookup"><span data-stu-id="2886a-2056">-inf</span></span> | <span data-ttu-id="2886a-2057">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-2057">NaN</span></span>  | <span data-ttu-id="2886a-2058">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-2058">NaN</span></span> | 
   | <span data-ttu-id="2886a-2059">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-2059">NaN</span></span>  | <span data-ttu-id="2886a-2060">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-2060">NaN</span></span>  | <span data-ttu-id="2886a-2061">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-2061">NaN</span></span>  | <span data-ttu-id="2886a-2062">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-2062">NaN</span></span>  | <span data-ttu-id="2886a-2063">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-2063">NaN</span></span>  | <span data-ttu-id="2886a-2064">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-2064">NaN</span></span>  | <span data-ttu-id="2886a-2065">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2886a-2065">NaN</span></span> | 

*  <span data-ttu-id="2886a-2066">Ondalık çıkarma:</span><span class="sxs-lookup"><span data-stu-id="2886a-2066">Decimal subtraction:</span></span>

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   <span data-ttu-id="2886a-2067">Elde edilen değer `decimal` biçimde temsil etmek için çok büyükse, bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2067">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="2886a-2068">Sonucun ölçeği, herhangi bir yuvarlamadan önce, iki işlenenin ölçeklendirilmesine göre daha büyüktür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2068">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="2886a-2069">Decimal çıkarma, `System.Decimal`türünde çıkarma işlecinin kullanılmasıyla eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2069">Decimal subtraction is equivalent to using the subtraction operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="2886a-2070">Sabit Listesi çıkarma.</span><span class="sxs-lookup"><span data-stu-id="2886a-2070">Enumeration subtraction.</span></span> <span data-ttu-id="2886a-2071">Her numaralandırma türü örtük olarak aşağıdaki önceden tanımlı işleci sağlar; burada `E` sabit listesi türüdür ve `U` `E`temel türüdür:</span><span class="sxs-lookup"><span data-stu-id="2886a-2071">Every enumeration type implicitly provides the following predefined operator, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   U operator -(E x, E y);
   ```

   <span data-ttu-id="2886a-2072">Bu işleç tam olarak `(U)((U)x - (U)y)`olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2072">This operator is evaluated exactly as `(U)((U)x - (U)y)`.</span></span> <span data-ttu-id="2886a-2073">Diğer bir deyişle işleci, `x` ve `y`Ordinal değerleri arasındaki farkı hesaplar ve sonucun türü, numaralandırmanın temel alınan türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2073">In other words, the operator computes the difference between the ordinal values of `x` and `y`, and the type of the result is the underlying type of the enumeration.</span></span>

   ```csharp
   E operator -(E x, U y);
   ```

   <span data-ttu-id="2886a-2074">Bu işleç tam olarak `(E)((U)x - y)`olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2074">This operator is evaluated exactly as `(E)((U)x - y)`.</span></span> <span data-ttu-id="2886a-2075">Diğer bir deyişle işleci, numaralandırmanın temel alınan türünden bir değeri çıkartır ve sabit listesinin bir değerini çıkarır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2075">In other words, the operator subtracts a value from the underlying type of the enumeration, yielding a value of the enumeration.</span></span>

*  <span data-ttu-id="2886a-2076">Temsilci kaldırma.</span><span class="sxs-lookup"><span data-stu-id="2886a-2076">Delegate removal.</span></span> <span data-ttu-id="2886a-2077">Her temsilci türü örtük olarak aşağıdaki önceden tanımlı işleci sağlar; burada `D` temsilci türüdür:</span><span class="sxs-lookup"><span data-stu-id="2886a-2077">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator -(D x, D y);
   ```

   <span data-ttu-id="2886a-2078">İkili `-` işleci her iki işlenen de `D`bazı temsilci türleri olduğunda temsilci kaldırma işlemini gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2078">The binary `-` operator performs delegate removal when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="2886a-2079">İşlenenler farklı temsilci türlerine sahip ise, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2079">If the operands have different delegate types, a binding-time error occurs.</span></span> <span data-ttu-id="2886a-2080">İlk işlenen `null`, işlemin sonucu `null`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2080">If the first operand is `null`, the result of the operation is `null`.</span></span> <span data-ttu-id="2886a-2081">Aksi takdirde, ikinci işlenen `null`, işlemin sonucu ilk işlenenin değeridir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2081">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="2886a-2082">Aksi halde, her iki işlenen de bir veya daha fazla girişe sahip çağrı listelerini ([temsilci bildirimleri](delegates.md#delegate-declarations)) temsil eder ve sonuç, ikinci işlenenin listesinin ilk olarak doğru bir bitişik alt listesi olması şartıyla, ikinci işlenenin listesini içeren ilk işlenenin listesinden oluşan yeni bir çağırma listesidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2082">Otherwise, both operands represent invocation lists ([Delegate declarations](delegates.md#delegate-declarations)) having one or more entries, and the result is a new invocation list consisting of the first operand's list with the second operand's entries removed from it, provided the second operand's list is a proper contiguous sublist of the first's.</span></span>     <span data-ttu-id="2886a-2083">(Alt liste eşitliğini belirleyebilmek için, karşılık gelen girişler, temsilci eşitlik işleci ([temsilci eşitlik işleçleri](expressions.md#delegate-equality-operators)) ile karşılaştırılır.) Aksi halde, sonuç sol işlenenin değeridir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2083">(To determine sublist equality, corresponding entries are compared as for the delegate equality operator ([Delegate equality operators](expressions.md#delegate-equality-operators)).) Otherwise, the result is the value of the left operand.</span></span> <span data-ttu-id="2886a-2084">İşlemde işlenen listelerden hiçbiri değiştirilmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-2084">Neither of the operands' lists is changed in the process.</span></span> <span data-ttu-id="2886a-2085">İkinci işlenenin listesi, ilk işlenen listesindeki bitişik girdilerin birden çok alt listesiyle eşleşiyorsa, bitişik girdilerin en sağ eşleşen alt listesi kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2085">If the second operand's list matches multiple sublists of contiguous entries in the first operand's list, the right-most matching sublist of contiguous entries is removed.</span></span> <span data-ttu-id="2886a-2086">Kaldırma işlemi boş bir liste ile sonuçlanırsa sonuç `null`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2086">If removal results in an empty list, the result is `null`.</span></span> <span data-ttu-id="2886a-2087">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2886a-2087">For example:</span></span>

   ```csharp
   delegate void D(int x);
   
   class C
   {
       public static void M1(int i) { /* ... */ }
       public static void M2(int i) { /* ... */ }
   }

   class Test
   {
       static void Main() { 
           D cd1 = new D(C.M1);
           D cd2 = new D(C.M2);
           D cd3 = cd1 + cd2 + cd2 + cd1;   // M1 + M2 + M2 + M1
           cd3 -= cd1;                      // => M1 + M2 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd2;                // => M2 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd2;                // => M1 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd1;                // => M1 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd1;                // => M1 + M2 + M2 + M1
       }
   }
   ```

## <a name="shift-operators"></a><span data-ttu-id="2886a-2088">Kaydırma işleçleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2088">Shift operators</span></span>

<span data-ttu-id="2886a-2089">`<<` ve `>>` işleçleri bit kaydırma işlemlerini gerçekleştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2089">The `<<` and `>>` operators are used to perform bit shifting operations.</span></span>

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

<span data-ttu-id="2886a-2090">Bir *shift_expression* işleneni derleme zamanı türü `dynamic`içeriyorsa, ifade dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="2886a-2090">If an operand of a *shift_expression* has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="2886a-2091">Bu durumda, ifadenin derleme zamanı türü `dynamic`ve aşağıda açıklanan çözüm, derleme zamanı türü `dynamic`sahip olan işlenenleri çalışma zamanı türü kullanılarak çalışma zamanında gerçekleşmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2091">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="2886a-2092">`x << count` veya `x >> count`form işlemi için, belirli bir operatör uygulamasını seçmek üzere ikili işleç aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2092">For an operation of the form `x << count` or `x >> count`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="2886a-2093">İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2093">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="2886a-2094">Aşırı yüklenmiş bir kaydırma işleci bildirirken, İlk işlenenin türü her zaman operatör bildirimini içeren sınıf veya yapı olmalıdır ve ikinci işlenenin türü her zaman `int`olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2094">When declaring an overloaded shift operator, the type of the first operand must always be the class or struct containing the operator declaration, and the type of the second operand must always be `int`.</span></span>

<span data-ttu-id="2886a-2095">Önceden tanımlanmış kaydırma işleçleri aşağıda listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2095">The predefined shift operators are listed below.</span></span>

*  <span data-ttu-id="2886a-2096">Sola kaydır:</span><span class="sxs-lookup"><span data-stu-id="2886a-2096">Shift left:</span></span>

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   <span data-ttu-id="2886a-2097">`<<` işleci, aşağıda açıklandığı gibi hesaplanan sayıda bit `x` sola kayar.</span><span class="sxs-lookup"><span data-stu-id="2886a-2097">The `<<` operator shifts `x` left by a number of bits computed as described below.</span></span>

   <span data-ttu-id="2886a-2098">`x` sonuç türü aralığının dışındaki yüksek sıralı bitler atılır, kalan bitler sola kaydıralınır ve düşük sıralı boş bit konumları sıfır olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2098">The high-order bits outside the range of the result type of `x` are discarded, the remaining bits are shifted left, and the low-order empty bit positions are set to zero.</span></span>

*  <span data-ttu-id="2886a-2099">Sağa kaydır:</span><span class="sxs-lookup"><span data-stu-id="2886a-2099">Shift right:</span></span>

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   <span data-ttu-id="2886a-2100">`>>` işleci, aşağıda açıklandığı gibi hesaplanmış bir bit sayısına göre `x` kayar.</span><span class="sxs-lookup"><span data-stu-id="2886a-2100">The `>>` operator shifts `x` right by a number of bits computed as described below.</span></span>

   <span data-ttu-id="2886a-2101">`x` `int` veya `long`türünde olduğunda, `x` düşük sıralı bitleri atılır, kalan bitler sağa kaydıralınır ve `x` negatifse yüksek sıralı boş bit konumları sıfır olarak ayarlanır ve `x` negatifse, bir olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2101">When `x` is of type `int` or `long`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero if `x` is non-negative and set to one if `x` is negative.</span></span>

   <span data-ttu-id="2886a-2102">`x` `uint` veya `ulong`türünde olduğunda, `x` düşük sıralı bitleri atılır, kalan bitler sağa kaydıralınır ve yüksek sıralı boş bit konumları sıfır olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2102">When `x` is of type `uint` or `ulong`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero.</span></span>

<span data-ttu-id="2886a-2103">Önceden tanımlanmış işleçler için, kaydırma yapılacak bit sayısı şu şekilde hesaplanır:</span><span class="sxs-lookup"><span data-stu-id="2886a-2103">For the predefined operators, the number of bits to shift is computed as follows:</span></span>

*  <span data-ttu-id="2886a-2104">`x` türü `int` veya `uint`olduğunda, kaydırma sayısı düşük sıralı beş bit `count`tarafından verilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2104">When the type of `x` is `int` or `uint`, the shift count is given by the low-order five bits of `count`.</span></span> <span data-ttu-id="2886a-2105">Diğer bir deyişle, kaydırma sayısı `count & 0x1F`hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2105">In other words, the shift count is computed from `count & 0x1F`.</span></span>
*  <span data-ttu-id="2886a-2106">`x` türü `long` veya `ulong`olduğunda, kaydırma sayısı en düşük altı bit `count`tarafından verilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2106">When the type of `x` is `long` or `ulong`, the shift count is given by the low-order six bits of `count`.</span></span> <span data-ttu-id="2886a-2107">Diğer bir deyişle, kaydırma sayısı `count & 0x3F`hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2107">In other words, the shift count is computed from `count & 0x3F`.</span></span>

<span data-ttu-id="2886a-2108">Sonuç kaydırma sayısı sıfırsa, kaydırma işleçleri yalnızca `x`değerini döndürür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2108">If the resulting shift count is zero, the shift operators simply return the value of `x`.</span></span>

<span data-ttu-id="2886a-2109">SHIFT işlemleri hiçbir şekilde taşmaya neden olmaz ve `checked` ve `unchecked` bağlamlarında aynı sonuçları üretir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2109">Shift operations never cause overflows and produce the same results in `checked` and `unchecked` contexts.</span></span>

<span data-ttu-id="2886a-2110">`>>` işlecinin sol işleneni işaretli bir integral türünde olduğunda, işleç, işlenenin en önemli bit (işaret biti) değerinde bir aritmetik kaydırma gerçekleştirir ve yüksek sıralı boş bit konumlarına yayılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2110">When the left operand of the `>>` operator is of a signed integral type, the operator performs an arithmetic shift right wherein the value of the most significant bit (the sign bit) of the operand is propagated to the high-order empty bit positions.</span></span> <span data-ttu-id="2886a-2111">`>>` işlecinin sol işleneni işaretsiz bir integral türünde olduğunda, işleç yüksek sıralı boş bit konumlarda her zaman sıfır olarak ayarlanmış bir mantıksal kaydırma gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2111">When the left operand of the `>>` operator is of an unsigned integral type, the operator performs a logical shift right wherein high-order empty bit positions are always set to zero.</span></span> <span data-ttu-id="2886a-2112">İşlenen türünden çıkarılan ' nin ters işlemini gerçekleştirmek için, açık atamalar kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2112">To perform the opposite operation of that inferred from the operand type, explicit casts can be used.</span></span> <span data-ttu-id="2886a-2113">Örneğin, `x` `int`türünde bir değişkense, işlem `unchecked((int)((uint)x >> y))` `x`sağ bir mantıksal kaydırma gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2113">For example, if `x` is a variable of type `int`, the operation `unchecked((int)((uint)x >> y))` performs a logical shift right of `x`.</span></span>

## <a name="relational-and-type-testing-operators"></a><span data-ttu-id="2886a-2114">İlişkisel ve tür testi işleçleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2114">Relational and type-testing operators</span></span>

<span data-ttu-id="2886a-2115">`==`, `!=`, `<`, `>`, `<=`, `>=`, `is` ve `as` işleçleri ilişkisel ve tür testi işleçleri olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2115">The `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` and `as` operators are called the relational and type-testing operators.</span></span>

```antlr
relational_expression
    : shift_expression
    | relational_expression '<' shift_expression
    | relational_expression '>' shift_expression
    | relational_expression '<=' shift_expression
    | relational_expression '>=' shift_expression
    | relational_expression 'is' type
    | relational_expression 'as' type
    ;

equality_expression
    : relational_expression
    | equality_expression '==' relational_expression
    | equality_expression '!=' relational_expression
    ;
```

<span data-ttu-id="2886a-2116">`is` işleci [,,,](expressions.md#the-is-operator) ve `as` işleci [as işleci](expressions.md#the-as-operator)içinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2116">The `is` operator is described in [The is operator](expressions.md#the-is-operator) and the `as` operator is described in [The as operator](expressions.md#the-as-operator).</span></span>

<span data-ttu-id="2886a-2117">`==`, `!=`, `<`, `>`, `<=` ve `>=` işleçleri ***karşılaştırma işleçleridir***.</span><span class="sxs-lookup"><span data-stu-id="2886a-2117">The `==`, `!=`, `<`, `>`, `<=` and `>=` operators are ***comparison operators***.</span></span>

<span data-ttu-id="2886a-2118">Karşılaştırma işlecinin bir işleneni derleme zamanı türü `dynamic`, ifade dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="2886a-2118">If an operand of a comparison operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="2886a-2119">Bu durumda, ifadenin derleme zamanı türü `dynamic`ve aşağıda açıklanan çözüm, derleme zamanı türü `dynamic`sahip olan işlenenleri çalışma zamanı türü kullanılarak çalışma zamanında gerçekleşmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2119">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="2886a-2120">`x` *op* `y`biçiminde bir işlem için, *op* bir karşılaştırma operatörü olduğunda, belirli bir operatör uygulamasını seçmek için aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2120">For an operation of the form `x` *op* `y`, where *op* is a comparison operator, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="2886a-2121">İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2121">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="2886a-2122">Önceden tanımlanmış karşılaştırma işleçleri aşağıdaki bölümlerde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2122">The predefined comparison operators are described in the following sections.</span></span> <span data-ttu-id="2886a-2123">Önceden tanımlanmış tüm karşılaştırma işleçleri, aşağıdaki tabloda açıklandığı gibi `bool`türünde bir sonuç döndürür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2123">All predefined comparison operators return a result of type `bool`, as described in the following table.</span></span>


| <span data-ttu-id="2886a-2124">__İşlem__</span><span class="sxs-lookup"><span data-stu-id="2886a-2124">__Operation__</span></span> | <span data-ttu-id="2886a-2125">__Sonuç__</span><span class="sxs-lookup"><span data-stu-id="2886a-2125">__Result__</span></span>                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | <span data-ttu-id="2886a-2126">`x` `y`eşitse `true` `false` Aksi takdirde</span><span class="sxs-lookup"><span data-stu-id="2886a-2126">`true` if `x` is equal to `y`, `false` otherwise</span></span>                 | 
| `x != y`      | <span data-ttu-id="2886a-2127">`x` `y`eşitse `true` `false` Aksi takdirde</span><span class="sxs-lookup"><span data-stu-id="2886a-2127">`true` if `x` is not equal to `y`, `false` otherwise</span></span>             | 
| `x < y`       | <span data-ttu-id="2886a-2128">`x` `y`'den küçükse `true` `false` Aksi takdirde</span><span class="sxs-lookup"><span data-stu-id="2886a-2128">`true` if `x` is less than `y`, `false` otherwise</span></span>                | 
| `x > y`       | <span data-ttu-id="2886a-2129">`x` `y`daha büyükse `true` `false` Aksi takdirde</span><span class="sxs-lookup"><span data-stu-id="2886a-2129">`true` if `x` is greater than `y`, `false` otherwise</span></span>             | 
| `x <= y`      | <span data-ttu-id="2886a-2130">`x` `y`daha az veya eşit ise `true` `false` Aksi halde</span><span class="sxs-lookup"><span data-stu-id="2886a-2130">`true` if `x` is less than or equal to `y`, `false` otherwise</span></span>    | 
| `x >= y`      | <span data-ttu-id="2886a-2131">`x` `y`büyük veya buna eşitse `true` `false` Aksi takdirde</span><span class="sxs-lookup"><span data-stu-id="2886a-2131">`true` if `x` is greater than or equal to `y`, `false` otherwise</span></span> | 

### <a name="integer-comparison-operators"></a><span data-ttu-id="2886a-2132">Tamsayı karşılaştırma işleçleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2132">Integer comparison operators</span></span>

<span data-ttu-id="2886a-2133">Önceden tanımlanmış tamsayı karşılaştırma işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2886a-2133">The predefined integer comparison operators are:</span></span>
```csharp
bool operator ==(int x, int y);
bool operator ==(uint x, uint y);
bool operator ==(long x, long y);
bool operator ==(ulong x, ulong y);

bool operator !=(int x, int y);
bool operator !=(uint x, uint y);
bool operator !=(long x, long y);
bool operator !=(ulong x, ulong y);

bool operator <(int x, int y);
bool operator <(uint x, uint y);
bool operator <(long x, long y);
bool operator <(ulong x, ulong y);

bool operator >(int x, int y);
bool operator >(uint x, uint y);
bool operator >(long x, long y);
bool operator >(ulong x, ulong y);

bool operator <=(int x, int y);
bool operator <=(uint x, uint y);
bool operator <=(long x, long y);
bool operator <=(ulong x, ulong y);

bool operator >=(int x, int y);
bool operator >=(uint x, uint y);
bool operator >=(long x, long y);
bool operator >=(ulong x, ulong y);
```

<span data-ttu-id="2886a-2134">Bu işleçlerin her biri, iki tamsayı işleneninin sayısal değerlerini karşılaştırır ve belirli bir ilişkinin `true` veya `false`olduğunu belirten `bool` bir değer döndürür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2134">Each of these operators compares the numeric values of the two integer operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span>

### <a name="floating-point-comparison-operators"></a><span data-ttu-id="2886a-2135">Kayan nokta karşılaştırma işleçleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2135">Floating-point comparison operators</span></span>

<span data-ttu-id="2886a-2136">Önceden tanımlanmış kayan nokta karşılaştırma işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2886a-2136">The predefined floating-point comparison operators are:</span></span>
```csharp
bool operator ==(float x, float y);
bool operator ==(double x, double y);

bool operator !=(float x, float y);
bool operator !=(double x, double y);

bool operator <(float x, float y);
bool operator <(double x, double y);

bool operator >(float x, float y);
bool operator >(double x, double y);

bool operator <=(float x, float y);
bool operator <=(double x, double y);

bool operator >=(float x, float y);
bool operator >=(double x, double y);
```

<span data-ttu-id="2886a-2137">İşleçler, işlenenleri IEEE 754 standardının kurallarına göre karşılaştırır:</span><span class="sxs-lookup"><span data-stu-id="2886a-2137">The operators compare the operands according to the rules of the IEEE 754 standard:</span></span>

*  <span data-ttu-id="2886a-2138">Her iki işlenen de NaN ise sonuç, `!=`hariç tüm işleçler için `false`, sonuç olarak `true`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2138">If either operand is NaN, the result is `false` for all operators except `!=`, for which the result is `true`.</span></span> <span data-ttu-id="2886a-2139">Her iki işlenen için `x != y` her zaman `!(x == y)`ile aynı sonucu üretir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2139">For any two operands, `x != y` always produces the same result as `!(x == y)`.</span></span> <span data-ttu-id="2886a-2140">Ancak, bir veya her iki işlenen de NaN olduğunda, `<`, `>`, `<=`ve `>=` işleçleri ters işlecin mantıksal değille aynı sonuçları oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-2140">However, when one or both operands are NaN, the `<`, `>`, `<=`, and `>=` operators do not produce the same results as the logical negation of the opposite operator.</span></span> <span data-ttu-id="2886a-2141">Örneğin, `x` ve `y` ikisi NaN ise, `x < y` `false`, ancak `!(x >= y)` `true`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2141">For example, if either of `x` and `y` is NaN, then `x < y` is `false`, but `!(x >= y)` is `true`.</span></span>
*  <span data-ttu-id="2886a-2142">Hiçbir işlenen NaN olmadığında, işleçler iki kayan nokta işleneninin değerlerini sıralamaya göre karşılaştırır</span><span class="sxs-lookup"><span data-stu-id="2886a-2142">When neither operand is NaN, the operators compare the values of the two floating-point operands with respect to the ordering</span></span>

   ```csharp
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   <span data-ttu-id="2886a-2143">Burada `min` ve `max`, verilen kayan nokta biçiminde temsil edilebilecek en küçük ve en büyük pozitif sınırlı değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2143">where `min` and `max` are the smallest and largest positive finite values that can be represented in the given floating-point format.</span></span> <span data-ttu-id="2886a-2144">Bu sıralamanın önemli etkileri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2886a-2144">Notable effects of this ordering are:</span></span>
   * <span data-ttu-id="2886a-2145">Negatif ve pozitif sıfırlar eşit kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2145">Negative and positive zeros are considered equal.</span></span>
   * <span data-ttu-id="2886a-2146">Negatif sonsuzluk, diğer tüm değerlerden daha az kabul edilir, ancak başka bir negatif sonsuzya eşittir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2146">A negative infinity is considered less than all other values, but equal to another negative infinity.</span></span>
   * <span data-ttu-id="2886a-2147">Pozitif sonsuzluk, diğer tüm değerlerden daha büyük sayılır, ancak başka bir pozitif sonsuzya eşittir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2147">A positive infinity is considered greater than all other values, but equal to another positive infinity.</span></span>

### <a name="decimal-comparison-operators"></a><span data-ttu-id="2886a-2148">Ondalık karşılaştırma işleçleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2148">Decimal comparison operators</span></span>

<span data-ttu-id="2886a-2149">Önceden tanımlanmış ondalık karşılaştırma işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2886a-2149">The predefined decimal comparison operators are:</span></span>
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

<span data-ttu-id="2886a-2150">Bu işleçlerin her biri, iki ondalık işlenenin sayısal değerlerini karşılaştırır ve belirli bir ilişkinin `true` veya `false`olduğunu belirten `bool` bir değer döndürür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2150">Each of these operators compares the numeric values of the two decimal operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span> <span data-ttu-id="2886a-2151">Her ondalık karşılaştırma, `System.Decimal`türünde karşılık gelen ilişkisel veya eşitlik işlecinin kullanılmasıyla eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2151">Each decimal comparison is equivalent to using the corresponding relational or equality operator of type `System.Decimal`.</span></span>

### <a name="boolean-equality-operators"></a><span data-ttu-id="2886a-2152">Boole eşitlik işleçleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2152">Boolean equality operators</span></span>

<span data-ttu-id="2886a-2153">Önceden tanımlanmış Boole eşitlik işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2886a-2153">The predefined boolean equality operators are:</span></span>
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

<span data-ttu-id="2886a-2154">`==` sonucu, hem `x` hem de `y` `true` ve `x` ve `y` `false`ise `true`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2154">The result of `==` is `true` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="2886a-2155">Aksi takdirde, sonuç `false`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2155">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="2886a-2156">`!=` sonucu, hem `x` hem de `y` `true` ve `x` ve `y` `false`ise `false`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2156">The result of `!=` is `false` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="2886a-2157">Aksi takdirde, sonuç `true`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2157">Otherwise, the result is `true`.</span></span> <span data-ttu-id="2886a-2158">İşlenenler `bool`türünde olduğunda `!=` işleci `^` işleci ile aynı sonucu üretir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2158">When the operands are of type `bool`, the `!=` operator produces the same result as the `^` operator.</span></span>

### <a name="enumeration-comparison-operators"></a><span data-ttu-id="2886a-2159">Sabit Listesi karşılaştırma işleçleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2159">Enumeration comparison operators</span></span>

<span data-ttu-id="2886a-2160">Her numaralandırma türü örtük olarak aşağıdaki önceden tanımlı karşılaştırma işleçlerini sağlar:</span><span class="sxs-lookup"><span data-stu-id="2886a-2160">Every enumeration type implicitly provides the following predefined comparison operators:</span></span>
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

<span data-ttu-id="2886a-2161">`x` ve `y`, temel alınan bir tür `U`olan `E` bir numaralandırma türünün ifadeleridir ve `op` karşılaştırma işleçlerinden biri olan `x op y`değerlendirilme sonucu, `((U)x) op ((U)y)`değerlendirmesiyle tamamen aynıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2161">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the comparison operators, is exactly the same as evaluating `((U)x) op ((U)y)`.</span></span> <span data-ttu-id="2886a-2162">Diğer bir deyişle, numaralandırma türü karşılaştırma işleçleri yalnızca iki işlenenin temel alınan integral değerlerini karşılaştırmaktır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2162">In other words, the enumeration type comparison operators simply compare the underlying integral values of the two operands.</span></span>

### <a name="reference-type-equality-operators"></a><span data-ttu-id="2886a-2163">Başvuru türü eşitlik işleçleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2163">Reference type equality operators</span></span>

<span data-ttu-id="2886a-2164">Önceden tanımlanmış başvuru türü eşitlik işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2886a-2164">The predefined reference type equality operators are:</span></span>
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

<span data-ttu-id="2886a-2165">İşleçler, eşitlik veya eşitlik için iki başvuruyu karşılaştırmanın sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2165">The operators return the result of comparing the two references for equality or non-equality.</span></span>

<span data-ttu-id="2886a-2166">Önceden tanımlanmış başvuru türü eşitlik işleçleri `object`türündeki işlenenleri kabul ettiğinde, uygulanabilir `operator ==` ve `operator !=` üyelerini bildirmez tüm türlere uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2166">Since the predefined reference type equality operators accept operands of type `object`, they apply to all types that do not declare applicable `operator ==` and `operator !=` members.</span></span> <span data-ttu-id="2886a-2167">Buna karşılık, geçerli kullanıcı tanımlı eşitlik işleçleri, önceden tanımlanmış başvuru türü eşitlik işleçlerini etkin bir şekilde gizler.</span><span class="sxs-lookup"><span data-stu-id="2886a-2167">Conversely, any applicable user-defined equality operators effectively hide the predefined reference type equality operators.</span></span>

<span data-ttu-id="2886a-2168">Önceden tanımlanmış başvuru türü eşitlik işleçleri aşağıdakilerden birini gerektirir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2168">The predefined reference type equality operators require one of the following:</span></span>

*  <span data-ttu-id="2886a-2169">Her iki işlenen de *reference_type* veya değişmez değer `null`bilinen bir türün değeridir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2169">Both operands are a value of a type known to be a *reference_type* or the literal `null`.</span></span> <span data-ttu-id="2886a-2170">Ayrıca, bir açık başvuru dönüştürmesi ([açık başvuru dönüştürmeleri](conversions.md#explicit-reference-conversions)), herhangi bir işlenenin türünden diğer işlenenin türüne de sahiptir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2170">Furthermore, an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from the type of either operand to the type of the other operand.</span></span>
*  <span data-ttu-id="2886a-2171">Bir işlenen, `T` bir *type_parameter* olan ve diğer işlenen ise `null`olan `T` türünde bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2171">One operand is a value of type `T` where `T` is a *type_parameter* and the other operand is the literal `null`.</span></span> <span data-ttu-id="2886a-2172">Ayrıca `T` değer türü kısıtlamasına sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2172">Furthermore `T` does not have the value type constraint.</span></span>

<span data-ttu-id="2886a-2173">Bu koşullardan biri doğru değilse, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2173">Unless one of these conditions are true, a binding-time error occurs.</span></span> <span data-ttu-id="2886a-2174">Bu kuralların önemli etkileri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2886a-2174">Notable implications of these rules are:</span></span>

*  <span data-ttu-id="2886a-2175">Bu, bağlama zamanında farklı oldukları bilinen iki başvuruyu karşılaştırmak için önceden tanımlanmış başvuru türü eşitlik işleçlerini kullanmanın bir bağlama zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2175">It is a binding-time error to use the predefined reference type equality operators to compare two references that are known to be different at binding-time.</span></span> <span data-ttu-id="2886a-2176">Örneğin, işlenenlerin bağlama zamanı türleri `A` ve `B`iki sınıf türütürlerdi ve hiçbiri ne `A` ne de `B` türetilmediği takdirde, iki işlenen de aynı nesneye başvuruda bulunmak olanaksızdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2176">For example, if the binding-time types of the operands are two class types `A` and `B`, and if neither `A` nor `B` derives from the other, then it would be impossible for the two operands to reference the same object.</span></span> <span data-ttu-id="2886a-2177">Bu nedenle, işlem bağlama zamanı hatası olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2177">Thus, the operation is considered a binding-time error.</span></span>
*  <span data-ttu-id="2886a-2178">Önceden tanımlanmış başvuru türü eşitlik işleçleri değer türü işlenenlerinin karşılaştırılabilmesi için izin vermez.</span><span class="sxs-lookup"><span data-stu-id="2886a-2178">The predefined reference type equality operators do not permit value type operands to be compared.</span></span> <span data-ttu-id="2886a-2179">Bu nedenle, bir struct türü kendi eşitlik işleçlerini bildirmediği için, bu yapı türünün değerlerini karşılaştırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2179">Therefore, unless a struct type declares its own equality operators, it is not possible to compare values of that struct type.</span></span>
*  <span data-ttu-id="2886a-2180">Önceden tanımlanmış başvuru türü eşitlik işleçleri, işlenenleri için paketleme işlemlerine neden olmaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-2180">The predefined reference type equality operators never cause boxing operations to occur for their operands.</span></span> <span data-ttu-id="2886a-2181">Yeni ayrılmış paketlenmiş örneklere yapılan başvuruların diğer tüm başvurulardan farklı olması gerektiğinden, bu tür kutulanabilir işlemleri gerçekleştirmek anlamlı değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2181">It would be meaningless to perform such boxing operations, since references to the newly allocated boxed instances would necessarily differ from all other references.</span></span>
*  <span data-ttu-id="2886a-2182">Tür parametre `T` türü işleneni `null`ile karşılaştırıldıysanız ve `T` çalışma zamanı türü bir değer türündeyse, karşılaştırma sonucu `false`olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2182">If an operand of a type parameter type `T` is compared to `null`, and the run-time type of `T` is a value type, the result of the comparison is `false`.</span></span>

<span data-ttu-id="2886a-2183">Aşağıdaki örnek, kısıtlanmamış bir tür parametre türünün bağımsız değişkeninin `null`olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="2886a-2183">The following example checks whether an argument of an unconstrained type parameter type is `null`.</span></span>
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

<span data-ttu-id="2886a-2184">`T` bir değer türünü temsil etse bile `x == null` yapısına izin verilir ve sonuç yalnızca `T` bir değer türü olduğunda `false` olarak tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2184">The `x == null` construct is permitted even though `T` could represent a value type, and the result is simply defined to be `false` when `T` is a value type.</span></span>

<span data-ttu-id="2886a-2185">`x == y` veya `x != y`form işlemi için, uygulanabilir `operator ==` veya `operator !=` varsa, işleç aşırı yükleme çözümü ([ikili işleç aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) kuralları önceden tanımlanmış başvuru türü eşitlik işleci yerine bu işleci seçer.</span><span class="sxs-lookup"><span data-stu-id="2886a-2185">For an operation of the form `x == y` or `x != y`, if any applicable `operator ==` or `operator !=` exists, the operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) rules will select that operator instead of the predefined reference type equality operator.</span></span> <span data-ttu-id="2886a-2186">Ancak, bir veya her ikisini de `object`türüne açıkça atayarak önceden tanımlanmış başvuru türü eşitlik işlecini seçmek her zaman mümkündür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2186">However, it is always possible to select the predefined reference type equality operator by explicitly casting one or both of the operands to type `object`.</span></span> <span data-ttu-id="2886a-2187">Örnek</span><span class="sxs-lookup"><span data-stu-id="2886a-2187">The example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        string s = "Test";
        string t = string.Copy(s);
        Console.WriteLine(s == t);
        Console.WriteLine((object)s == t);
        Console.WriteLine(s == (object)t);
        Console.WriteLine((object)s == (object)t);
    }
}
```
<span data-ttu-id="2886a-2188">çıktıyı üretir</span><span class="sxs-lookup"><span data-stu-id="2886a-2188">produces the output</span></span>
```console
True
False
False
False
```

<span data-ttu-id="2886a-2189">`s` ve `t` değişkenleri aynı karakterleri içeren iki ayrı `string` örneğine başvurur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2189">The `s` and `t` variables refer to two distinct `string` instances containing the same characters.</span></span> <span data-ttu-id="2886a-2190">Her iki işlenen de `string`tür olduğunda önceden tanımlanmış dize eşitlik işleci ([dize eşitlik işleçleri](expressions.md#string-equality-operators)) seçildiğinden ilk karşılaştırma çıkışı `True`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2190">The first comparison outputs `True` because the predefined string equality operator ([String equality operators](expressions.md#string-equality-operators)) is selected when both operands are of type `string`.</span></span> <span data-ttu-id="2886a-2191">Kalan karşılaştırmalar, bir veya her ikisi de `object`tür olduğunda önceden tanımlanmış başvuru türü eşitlik işleci seçildiğinden, tüm çıkış `False`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2191">The remaining comparisons all output `False` because the predefined reference type equality operator is selected when one or both of the operands are of type `object`.</span></span>

<span data-ttu-id="2886a-2192">Yukarıdaki tekniğinin değer türleri için anlamlı olmadığına unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2886a-2192">Note that the above technique is not meaningful for value types.</span></span> <span data-ttu-id="2886a-2193">Örnek</span><span class="sxs-lookup"><span data-stu-id="2886a-2193">The example</span></span>
```csharp
class Test
{
    static void Main() {
        int i = 123;
        int j = 123;
        System.Console.WriteLine((object)i == (object)j);
    }
}
```
<span data-ttu-id="2886a-2194">yayınlar, kutulanmış `int` değerlerinin iki ayrı örneğine başvuru oluşturduğundan `False` çıktılar.</span><span class="sxs-lookup"><span data-stu-id="2886a-2194">outputs `False` because the casts create references to two separate instances of boxed `int` values.</span></span>

### <a name="string-equality-operators"></a><span data-ttu-id="2886a-2195">Dize eşitlik işleçleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2195">String equality operators</span></span>

<span data-ttu-id="2886a-2196">Önceden tanımlanmış dize eşitlik işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2886a-2196">The predefined string equality operators are:</span></span>
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

<span data-ttu-id="2886a-2197">Aşağıdakilerden biri doğru olduğunda iki `string` değeri eşit kabul edilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2197">Two `string` values are considered equal when one of the following is true:</span></span>

*  <span data-ttu-id="2886a-2198">Her iki değer de `null`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2198">Both values are `null`.</span></span>
*  <span data-ttu-id="2886a-2199">Her iki değer de aynı uzunlukta ve her bir karakter konumunda özdeş karakterlere sahip olan dize örneklerine yönelik null olmayan başvurulardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2199">Both values are non-null references to string instances that have identical lengths and identical characters in each character position.</span></span>

<span data-ttu-id="2886a-2200">Dize eşitlik işleçleri dize başvuruları yerine dize değerlerini karşılaştırır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2200">The string equality operators compare string values rather than string references.</span></span> <span data-ttu-id="2886a-2201">İki ayrı dize örneği tam olarak aynı karakter dizisini içerdiğinde, dizelerin değerleri eşittir, ancak başvurular farklıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2201">When two separate string instances contain the exact same sequence of characters, the values of the strings are equal, but the references are different.</span></span> <span data-ttu-id="2886a-2202">[Başvuru türü eşitlik işleçleri](expressions.md#reference-type-equality-operators)bölümünde açıklandığı gibi, dize değerleri yerine dize başvurularını karşılaştırmak için başvuru türü eşitlik işleçleri kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2202">As described in [Reference type equality operators](expressions.md#reference-type-equality-operators), the reference type equality operators can be used to compare string references instead of string values.</span></span>

### <a name="delegate-equality-operators"></a><span data-ttu-id="2886a-2203">Eşitlik işleçlerini devretmek</span><span class="sxs-lookup"><span data-stu-id="2886a-2203">Delegate equality operators</span></span>

<span data-ttu-id="2886a-2204">Her temsilci türü örtük olarak aşağıdaki önceden tanımlı karşılaştırma işleçlerini sağlar:</span><span class="sxs-lookup"><span data-stu-id="2886a-2204">Every delegate type implicitly provides the following predefined comparison operators:</span></span>

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

<span data-ttu-id="2886a-2205">İki temsilci örneği aşağıdaki gibi kabul edilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2205">Two delegate instances are considered equal as follows:</span></span>

*  <span data-ttu-id="2886a-2206">Temsilci örneklerinden biri `null`ise, yalnızca ikisi de `null`varsa eşittir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2206">If either of the delegate instances is `null`, they are equal if and only if both are `null`.</span></span>
*  <span data-ttu-id="2886a-2207">Temsilcilerin farklı çalışma zamanı türleri varsa, bunlar hiçbir zaman eşit değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2207">If the delegates have different run-time type they are never equal.</span></span>
*  <span data-ttu-id="2886a-2208">Her iki temsilci örneği de bir çağırma listesine ([temsilci bildirimleri](delegates.md#delegate-declarations)) sahip ise, bu örnekler eşit olur ve yalnızca çağırma listeleri aynı uzunluktadır ve tek bir çağrı listesindeki her giriş, karşılık gelen giriş listesindeki diğer çağrı listesinde sırasıyla eşittir (aşağıda tanımlandığı gibi).</span><span class="sxs-lookup"><span data-stu-id="2886a-2208">If both of the delegate instances have an invocation list ([Delegate declarations](delegates.md#delegate-declarations)), those instances are equal if and only if their invocation lists are the same length, and each entry in one's invocation list is equal (as defined below) to the corresponding entry, in order, in the other's invocation list.</span></span>

<span data-ttu-id="2886a-2209">Aşağıdaki kurallar, çağırma listesi girişlerinin eşitlik düzeyini yönetir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2209">The following rules govern the equality of invocation list entries:</span></span>

*  <span data-ttu-id="2886a-2210">İki çağırma listesi girişi her ikisi de aynı statik yönteme başvurursanız, girdiler eşittir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2210">If two invocation list entries both refer to the same static method then the entries are equal.</span></span>
*  <span data-ttu-id="2886a-2211">İki çağırma listesi girişi her ikisi de aynı hedef nesnede statik olmayan bir yönteme (başvuru eşitlik işleçleri tarafından tanımlandığı gibi) başvurmazsa, girdiler eşittir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2211">If two invocation list entries both refer to the same non-static method on the same target object (as defined by the reference equality operators) then the entries are equal.</span></span>
*  <span data-ttu-id="2886a-2212">Aynı (muhtemelen boş) yakalanan dış değişken örnekleri kümesiyle aynı (büyük olasılıkla boş) bir değere sahip anlamsal özdeş *anonymous_method_expression*s veya *lambda_expression*s değerlendirmesinden üretilen çağırma listesi girişleri, eşit olması için izin verilir (ancak gerekli değildir).</span><span class="sxs-lookup"><span data-stu-id="2886a-2212">Invocation list entries produced from evaluation of semantically identical *anonymous_method_expression*s or *lambda_expression*s with the same (possibly empty) set of captured outer variable instances are permitted (but not required) to be equal.</span></span>

### <a name="equality-operators-and-null"></a><span data-ttu-id="2886a-2213">Eşitlik işleçleri ve null</span><span class="sxs-lookup"><span data-stu-id="2886a-2213">Equality operators and null</span></span>

<span data-ttu-id="2886a-2214">`==` ve `!=` işleçleri, işlem için önceden tanımlanmış veya Kullanıcı tanımlı bir operatör (unlifted veya yükseltilmemiş biçiminde) mevcut olmasa bile, tek bir işlenenin null yapılabilir türde ve diğeri de `null` değişmez değeri olarak izin verir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2214">The `==` and `!=` operators permit one operand to be a value of a nullable type and the other to be the `null` literal, even if no predefined or user-defined operator (in unlifted or lifted form) exists for the operation.</span></span>

<span data-ttu-id="2886a-2215">Formlardan birine bir işlem için</span><span class="sxs-lookup"><span data-stu-id="2886a-2215">For an operation of one of the forms</span></span>
```csharp
x == null
null == x
x != null
null != x
```
<span data-ttu-id="2886a-2216">`x`, null yapılabilir bir türdeki bir ifadedir, işleç aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) ilgili bir işleci bulamazsa, bunun yerine `x``HasValue` özelliğinden hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2216">where `x` is an expression of a nullable type, if operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) fails to find an applicable operator, the result is instead computed from the `HasValue` property of `x`.</span></span> <span data-ttu-id="2886a-2217">Özellikle, ilk iki form `!x.HasValue`çevrilir ve son iki form `x.HasValue`dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2217">Specifically, the first two forms are translated into `!x.HasValue`, and last two forms are translated into `x.HasValue`.</span></span>

### <a name="the-is-operator"></a><span data-ttu-id="2886a-2218">SIS işleci</span><span class="sxs-lookup"><span data-stu-id="2886a-2218">The is operator</span></span>

<span data-ttu-id="2886a-2219">`is` işleci, bir nesnenin çalışma zamanı türünün belirli bir tür ile uyumlu olup olmadığını dinamik olarak denetlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2219">The `is` operator is used to dynamically check if the run-time type of an object is compatible with a given type.</span></span> <span data-ttu-id="2886a-2220">İşlemin sonucu `E is T`, `E` bir ifadedir ve `T` bir tür ise, `E` bir başvuru dönüştürmesi, paketleme dönüştürmesi veya bir kutudan çıkarma dönüştürmesi tarafından `T` türüne başarıyla dönüştürülüp dönüştürülmeyeceğini belirten bir Boole değeridir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2220">The result of the operation `E is T`, where `E` is an expression and `T` is a type, is a boolean value indicating whether `E` can successfully be converted to type `T` by a reference conversion, a boxing conversion, or an unboxing conversion.</span></span> <span data-ttu-id="2886a-2221">Tür bağımsız değişkenleri tüm tür parametreleri yerine eklendikten sonra işlem aşağıdaki gibi değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2221">The operation is evaluated as follows, after type arguments have been substituted for all type parameters:</span></span>

*  <span data-ttu-id="2886a-2222">`E` anonim bir işlevse, derleme zamanı hatası oluşur</span><span class="sxs-lookup"><span data-stu-id="2886a-2222">If `E` is an anonymous function, a compile-time error occurs</span></span>
*  <span data-ttu-id="2886a-2223">`E` bir yöntem grubu veya `null` değişmez değeri ise, `E` türü bir başvuru türü veya null yapılabilir bir tür ise ve `E` değeri null ise sonuç false olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2223">If `E` is a method group or the `null` literal, of if the type of `E` is a reference type or a nullable type and the value of `E` is null, the result is false.</span></span>
*  <span data-ttu-id="2886a-2224">Aksi takdirde, `D` `E` dinamik türünü aşağıdaki gibi temsil edelim:</span><span class="sxs-lookup"><span data-stu-id="2886a-2224">Otherwise, let `D` represent the dynamic type of `E` as follows:</span></span>
   * <span data-ttu-id="2886a-2225">`E` türü bir başvuru türü ise, `D`, `E`tarafından örnek başvurusunun çalışma zamanı türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2225">If the type of `E` is a reference type, `D` is the run-time type of the instance reference by `E`.</span></span>
   * <span data-ttu-id="2886a-2226">`E` türü null yapılabilir bir tür ise, `D` bu null yapılabilir türün temel türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2226">If the type of `E` is a nullable type, `D` is the underlying type of that nullable type.</span></span>
   * <span data-ttu-id="2886a-2227">`E` türü null yapılamayan bir değer türü ise, `D` `E`türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2227">If the type of `E` is a non-nullable value type, `D` is the type of `E`.</span></span>
*  <span data-ttu-id="2886a-2228">İşlemin sonucu `D` ve `T` aşağıdaki gibi değişir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2228">The result of the operation depends on `D` and `T` as follows:</span></span>
   * <span data-ttu-id="2886a-2229">`T` bir başvuru türü ise, `D` ve `T` aynı türde ise sonuç true olur. `D` bir başvuru türüdür ve `D` bir değer türü ise ve `T` ' dan `D` ' a bir paketleme dönüştürmesi varsa.</span><span class="sxs-lookup"><span data-stu-id="2886a-2229">If `T` is a reference type, the result is true if `D` and `T` are the same type, if `D` is a reference type and an implicit reference conversion from `D` to `T` exists, or if `D` is a value type and a boxing conversion from `D` to `T` exists.</span></span>
   * <span data-ttu-id="2886a-2230">`T` null yapılabilir bir tür ise, `D` temeldeki `T`türü ise sonuç true olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2230">If `T` is a nullable type, the result is true if `D` is the underlying type of `T`.</span></span>
   * <span data-ttu-id="2886a-2231">`T` null yapılamayan bir değer türü ise, `D` ve `T` aynı türde ise sonuç true olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2231">If `T` is a non-nullable value type, the result is true if `D` and `T` are the same type.</span></span>
   * <span data-ttu-id="2886a-2232">Aksi takdirde, sonuç false 'tur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2232">Otherwise, the result is false.</span></span>

<span data-ttu-id="2886a-2233">Kullanıcı tanımlı dönüştürmelerin `is` işleci tarafından değerlendirilmediğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2886a-2233">Note that user defined conversions, are not considered by the `is` operator.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="2886a-2234">As işleci</span><span class="sxs-lookup"><span data-stu-id="2886a-2234">The as operator</span></span>

<span data-ttu-id="2886a-2235">`as` işleci, açıkça bir değeri belirli bir başvuru türüne veya null yapılabilir türe dönüştürmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2235">The `as` operator is used to explicitly convert a value to a given reference type or nullable type.</span></span> <span data-ttu-id="2886a-2236">Atama ifadesinin ([cast ifadeleri](expressions.md#cast-expressions)) aksine, `as` işleci hiçbir şekilde özel durum oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-2236">Unlike a cast expression ([Cast expressions](expressions.md#cast-expressions)), the `as` operator never throws an exception.</span></span> <span data-ttu-id="2886a-2237">Bunun yerine, belirtilen dönüştürme mümkün değilse, sonuçta elde edilen değer `null`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2237">Instead, if the indicated conversion is not possible, the resulting value is `null`.</span></span>

<span data-ttu-id="2886a-2238">Form `E as T`işleminde, `E` bir ifade olmalıdır ve `T` bir başvuru türü, bir başvuru türü olarak bilinen bir tür parametresi veya null yapılabilir bir tür olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2238">In an operation of the form `E as T`, `E` must be an expression and `T` must be a reference type, a type parameter known to be a reference type, or a nullable type.</span></span> <span data-ttu-id="2886a-2239">Ayrıca, aşağıdakilerden en az birinin doğru olması gerekir, aksi takdirde bir derleme zamanı hatası oluşur:</span><span class="sxs-lookup"><span data-stu-id="2886a-2239">Furthermore, at least one of the following must be true, or otherwise a compile-time error occurs:</span></span>

*  <span data-ttu-id="2886a-2240">Bir kimlik ([kimlik dönüştürme](conversions.md#identity-conversion)), örtük null yapılabilir[(örtük Nullable dönüşümler](conversions.md#implicit-nullable-conversions)), örtük başvuru ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions)), kutulama ([kutulama dönüştürmeleri](conversions.md#boxing-conversions)), açık boş değer atanabilir ([Açık Nullable dönüşümler](conversions.md#explicit-nullable-conversions)), açık başvuru ([açık başvuru dönüştürmeleri](conversions.md#explicit-reference-conversions)) veya kutudan çıkarma (kutudan çıkarma[dönüştürmeleri](conversions.md#unboxing-conversions)) dönüştürme `E` `T`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2240">An identity ([Identity conversion](conversions.md#identity-conversion)), implicit nullable ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)), implicit reference ([Implicit reference conversions](conversions.md#implicit-reference-conversions)), boxing ([Boxing conversions](conversions.md#boxing-conversions)), explicit nullable ([Explicit nullable conversions](conversions.md#explicit-nullable-conversions)), explicit reference ([Explicit reference conversions](conversions.md#explicit-reference-conversions)), or unboxing ([Unboxing conversions](conversions.md#unboxing-conversions)) conversion exists from `E` to `T`.</span></span>
*  <span data-ttu-id="2886a-2241">`E` veya `T` türü açık bir tür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2241">The type of `E` or `T` is an open type.</span></span>
*  <span data-ttu-id="2886a-2242">`E`, `null` değişmez değerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2242">`E` is the `null` literal.</span></span>

<span data-ttu-id="2886a-2243">`E` derleme zamanı türü `dynamic`değilse, işlem `E as T` aynı sonucu üretir</span><span class="sxs-lookup"><span data-stu-id="2886a-2243">If the compile-time type of `E` is not `dynamic`, the operation `E as T` produces the same result as</span></span>
```csharp
E is T ? (T)(E) : (T)null
```
<span data-ttu-id="2886a-2244">`E` hariç, yalnızca bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2244">except that `E` is only evaluated once.</span></span> <span data-ttu-id="2886a-2245">Derleyicinin, yukarıdaki genişleme tarafından kapsanan iki dinamik tür denetimi yerine en çok bir dinamik tür denetimi gerçekleştirmesi için `E as T` iyileştirmek beklenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2245">The compiler can be expected to optimize `E as T` to perform at most one dynamic type check as opposed to the two dynamic type checks implied by the expansion above.</span></span>

<span data-ttu-id="2886a-2246">Derleme zamanı türü `E` `dynamic`ise, atama işlecinin aksine `as` işleci dinamik olarak bağlı değildir ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="2886a-2246">If the compile-time type of `E` is `dynamic`, unlike the cast operator the `as` operator is not dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="2886a-2247">Bu nedenle, bu durumda genişletme şu şekilde olur:</span><span class="sxs-lookup"><span data-stu-id="2886a-2247">Therefore the expansion in this case is:</span></span>
```csharp
E is T ? (T)(object)(E) : (T)null
```

<span data-ttu-id="2886a-2248">Kullanıcı tanımlı dönüştürmeler gibi bazı dönüştürmelerin `as` işleçleriyle mümkün olmadığına ve bunun yerine atama ifadeleri kullanılarak gerçekleştirilmesi gerektiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2886a-2248">Note that some conversions, such as user defined conversions, are not possible with the `as` operator and should instead be performed using cast expressions.</span></span>

<span data-ttu-id="2886a-2249">Örnekte</span><span class="sxs-lookup"><span data-stu-id="2886a-2249">In the example</span></span>
```csharp
class X
{

    public string F(object o) {
        return o as string;        // OK, string is a reference type
    }

    public T G<T>(object o) where T: Attribute {
        return o as T;             // Ok, T has a class constraint
    }

    public U H<U>(object o) {
        return o as U;             // Error, U is unconstrained 
    }
}
```
<span data-ttu-id="2886a-2250">sınıf kısıtlamasına sahip olduğundan, `G` tür parametresi `T` bir başvuru türü olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2250">the type parameter `T` of `G` is known to be a reference type, because it has the class constraint.</span></span> <span data-ttu-id="2886a-2251">`H` tür parametresi `U` değil, ancak Bu nedenle `H` `as` işlecinin kullanımına izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-2251">The type parameter `U` of `H` is not however; hence the use of the `as` operator in `H` is disallowed.</span></span>

## <a name="logical-operators"></a><span data-ttu-id="2886a-2252">Mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="2886a-2252">Logical operators</span></span>

<span data-ttu-id="2886a-2253">`&`, `^`ve `|` işleçleri mantıksal işleçler olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2253">The `&`, `^`, and `|` operators are called the logical operators.</span></span>

```antlr
and_expression
    : equality_expression
    | and_expression '&' equality_expression
    ;

exclusive_or_expression
    : and_expression
    | exclusive_or_expression '^' and_expression
    ;

inclusive_or_expression
    : exclusive_or_expression
    | inclusive_or_expression '|' exclusive_or_expression
    ;
```

<span data-ttu-id="2886a-2254">Bir mantıksal işlecin işleneni derleme zamanı türü `dynamic`, ifade dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="2886a-2254">If an operand of a logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="2886a-2255">Bu durumda, ifadenin derleme zamanı türü `dynamic`ve aşağıda açıklanan çözüm, derleme zamanı türü `dynamic`sahip olan işlenenleri çalışma zamanı türü kullanılarak çalışma zamanında gerçekleşmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2255">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="2886a-2256">`op` mantıksal işleçlerden biri olduğu `x op y`form işlemi için, belirli bir operatör uygulamasını seçmek üzere aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2256">For an operation of the form `x op y`, where `op` is one of the logical operators, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="2886a-2257">İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2257">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="2886a-2258">Önceden tanımlanmış mantıksal işleçler aşağıdaki bölümlerde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2258">The predefined logical operators are described in the following sections.</span></span>

### <a name="integer-logical-operators"></a><span data-ttu-id="2886a-2259">Tamsayı mantıksal işleçleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2259">Integer logical operators</span></span>

<span data-ttu-id="2886a-2260">Önceden tanımlı tamsayı mantıksal işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2886a-2260">The predefined integer logical operators are:</span></span>
```csharp
int operator &(int x, int y);
uint operator &(uint x, uint y);
long operator &(long x, long y);
ulong operator &(ulong x, ulong y);

int operator |(int x, int y);
uint operator |(uint x, uint y);
long operator |(long x, long y);
ulong operator |(ulong x, ulong y);

int operator ^(int x, int y);
uint operator ^(uint x, uint y);
long operator ^(long x, long y);
ulong operator ^(ulong x, ulong y);
```

<span data-ttu-id="2886a-2261">`&` işleci iki işlenenin bit düzeyinde mantıksal `AND` hesaplar, `|` işleci iki işlenenin bit düzeyinde mantıksal `OR` hesaplar ve `^` işleci iki işlenenin bit düzeyinde mantıksal dışlamalı `OR` hesaplar.</span><span class="sxs-lookup"><span data-stu-id="2886a-2261">The `&` operator computes the bitwise logical `AND` of the two operands, the `|` operator computes the bitwise logical `OR` of the two operands, and the `^` operator computes the bitwise logical exclusive `OR` of the two operands.</span></span> <span data-ttu-id="2886a-2262">Bu işlemlerden taşmalar mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2262">No overflows are possible from these operations.</span></span>

### <a name="enumeration-logical-operators"></a><span data-ttu-id="2886a-2263">Sabit listesi mantıksal işleçleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2263">Enumeration logical operators</span></span>

<span data-ttu-id="2886a-2264">Her numaralandırma türü `E` örtük olarak aşağıdaki önceden tanımlanmış mantıksal işleçleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="2886a-2264">Every enumeration type `E` implicitly provides the following predefined logical operators:</span></span>

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

<span data-ttu-id="2886a-2265">`x` ve `y`, temel alınan bir tür `U``E` bir numaralandırma türünün ifadeleridir ve `op`, mantıksal işleçlerden biridir, `(E)((U)x op (U)y)`değerlendirmesiyle tamamen aynıdır. `x op y`</span><span class="sxs-lookup"><span data-stu-id="2886a-2265">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the logical operators, is exactly the same as evaluating `(E)((U)x op (U)y)`.</span></span> <span data-ttu-id="2886a-2266">Diğer bir deyişle, mantıksal işleçler numaralandırma türü mantıksal işlemleri yalnızca iki işlenenin temel alınan türünde gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2266">In other words, the enumeration type logical operators simply perform the logical operation on the underlying type of the two operands.</span></span>

### <a name="boolean-logical-operators"></a><span data-ttu-id="2886a-2267">Boole mantıksal işleçleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2267">Boolean logical operators</span></span>

<span data-ttu-id="2886a-2268">Önceden tanımlanmış Boole mantıksal işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2886a-2268">The predefined boolean logical operators are:</span></span>
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

<span data-ttu-id="2886a-2269">`x & y` sonucu, hem `x` hem de `y` `true`ise `true`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2269">The result of `x & y` is `true` if both `x` and `y` are `true`.</span></span> <span data-ttu-id="2886a-2270">Aksi takdirde, sonuç `false`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2270">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="2886a-2271">`x | y` sonucu, `x` ya da `y` `true`ise `true`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2271">The result of `x | y` is `true` if either `x` or `y` is `true`.</span></span> <span data-ttu-id="2886a-2272">Aksi takdirde, sonuç `false`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2272">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="2886a-2273">`x ^ y` sonucu `x` `true` ve `y` `false`ve `x` `false` ve `y` `true`ise `true`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2273">The result of `x ^ y` is `true` if `x` is `true` and `y` is `false`, or `x` is `false` and `y` is `true`.</span></span> <span data-ttu-id="2886a-2274">Aksi takdirde, sonuç `false`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2274">Otherwise, the result is `false`.</span></span> <span data-ttu-id="2886a-2275">İşlenenler `bool`türünde olduğunda `^` işleci `!=` işleci ile aynı sonucu hesaplar.</span><span class="sxs-lookup"><span data-stu-id="2886a-2275">When the operands are of type `bool`, the `^` operator computes the same result as the `!=` operator.</span></span>

### <a name="nullable-boolean-logical-operators"></a><span data-ttu-id="2886a-2276">Null yapılabilir Boolean mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="2886a-2276">Nullable boolean logical operators</span></span>

<span data-ttu-id="2886a-2277">Null yapılabilir Boolean türü `bool?` üç değeri temsil edebilir, `true`, `false`ve `null`ve kavramsal olarak SQL 'de Boolean ifadeler için kullanılan üç değerli tür ile benzerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2277">The nullable boolean type `bool?` can represent three values, `true`, `false`, and `null`, and is conceptually similar to the three-valued type used for boolean expressions in SQL.</span></span> <span data-ttu-id="2886a-2278">`bool?` işlenenleri için `&` ve `|` işleçleri tarafından üretilen sonuçların SQL 'in üç değerli mantığıyla tutarlı olduğundan emin olmak için, aşağıdaki önceden tanımlanmış işleçler verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2278">To ensure that the results produced by the `&` and `|` operators for `bool?` operands are consistent with SQL's three-valued logic, the following predefined operators are provided:</span></span>

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

<span data-ttu-id="2886a-2279">Aşağıdaki tabloda, `true`, `false`ve `null`değerlerinin tüm birleşimleri için bu işleçler tarafından oluşturulan sonuçlar listelenmektedir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2279">The following table lists the results produced by these operators for all combinations of the values `true`, `false`, and `null`.</span></span>

| `x`     | `y`     | `x & y` | <code>x &#124; y</code> |
|:-------:|:-------:|:-------:|:-------:|
| `true`  | `true`  | `true`  | `true`  | 
| `true`  | `false` | `false` | `true`  | 
| `true`  | `null`  | `null`  | `true`  | 
| `false` | `true`  | `false` | `true`  | 
| `false` | `false` | `false` | `false` | 
| `false` | `null`  | `false` | `null`  | 
| `null`  | `true`  | `null`  | `true`  | 
| `null`  | `false` | `false` | `null`  | 
| `null`  | `null`  | `null`  | `null`  | 

## <a name="conditional-logical-operators"></a><span data-ttu-id="2886a-2280">Koşullu mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="2886a-2280">Conditional logical operators</span></span>

<span data-ttu-id="2886a-2281">`&&` ve `||` işleçleri Koşullu mantıksal işleçler olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2281">The `&&` and `||` operators are called the conditional logical operators.</span></span> <span data-ttu-id="2886a-2282">Bunlar ayrıca "kısa devre dışı" mantıksal işleçler olarak da adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2282">They are also called the "short-circuiting" logical operators.</span></span>

```antlr
conditional_and_expression
    : inclusive_or_expression
    | conditional_and_expression '&&' inclusive_or_expression
    ;

conditional_or_expression
    : conditional_and_expression
    | conditional_or_expression '||' conditional_and_expression
    ;
```

<span data-ttu-id="2886a-2283">`&&` ve `||` işleçleri, `&` ve `|` işleçlerinin koşullu sürümleridir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2283">The `&&` and `||` operators are conditional versions of the `&` and `|` operators:</span></span>

*  <span data-ttu-id="2886a-2284">İşlem `x && y` işlem `x & y`karşılık gelir, ancak `y` yalnızca `x` `false`olmadığında değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2284">The operation `x && y` corresponds to the operation `x & y`, except that `y` is evaluated only if `x` is not `false`.</span></span>
*  <span data-ttu-id="2886a-2285">İşlem `x || y` işlem `x | y`karşılık gelir, ancak `y` yalnızca `x` `true`olmadığında değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2285">The operation `x || y` corresponds to the operation `x | y`, except that `y` is evaluated only if `x` is not `true`.</span></span>

<span data-ttu-id="2886a-2286">Koşullu bir mantıksal işlecin işleneni derleme zamanı türü `dynamic`, ifade dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="2886a-2286">If an operand of a conditional logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="2886a-2287">Bu durumda, ifadenin derleme zamanı türü `dynamic`ve aşağıda açıklanan çözüm, derleme zamanı türü `dynamic`sahip olan işlenenleri çalışma zamanı türü kullanılarak çalışma zamanında gerçekleşmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2287">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="2886a-2288">`x && y` veya `x || y` form işlemi, işlem `x & y` veya `x | y`yazılmış gibi aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2288">An operation of the form `x && y` or `x || y` is processed by applying overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x & y` or `x | y`.</span></span> <span data-ttu-id="2886a-2289">Ni</span><span class="sxs-lookup"><span data-stu-id="2886a-2289">Then,</span></span>

*  <span data-ttu-id="2886a-2290">Aşırı yükleme çözümlemesi tek bir en iyi operatör bulamazsa veya aşırı yükleme çözümlemesi önceden tanımlı tamsayı mantıksal işleçlerinden birini seçerse bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2290">If overload resolution fails to find a single best operator, or if overload resolution selects one of the predefined integer logical operators, a binding-time error occurs.</span></span>
*  <span data-ttu-id="2886a-2291">Aksi takdirde, seçili operatör önceden tanımlanmış Boole mantıksal işleçlerinden ([Boolean mantıksal işleçler](expressions.md#boolean-logical-operators)) veya null yapılabilir Boolean mantıksal Işleçlerden ([null yapılabilir Boolean mantıksal](expressions.md#nullable-boolean-logical-operators)işleçler) biri ise, işlem [Boole Koşullu mantıksal işleçler](expressions.md#boolean-conditional-logical-operators)bölümünde açıklandığı şekilde işlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2291">Otherwise, if the selected operator is one of the predefined boolean logical operators ([Boolean logical operators](expressions.md#boolean-logical-operators)) or nullable boolean logical operators ([Nullable boolean logical operators](expressions.md#nullable-boolean-logical-operators)), the operation is processed as described in [Boolean conditional logical operators](expressions.md#boolean-conditional-logical-operators).</span></span>
*  <span data-ttu-id="2886a-2292">Aksi takdirde, seçilen işleç Kullanıcı tanımlı bir işleçtir ve işlem [Kullanıcı tanımlı Koşullu mantıksal işleçler](expressions.md#user-defined-conditional-logical-operators)bölümünde açıklandığı şekilde işlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2292">Otherwise, the selected operator is a user-defined operator, and the operation is processed as described in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

<span data-ttu-id="2886a-2293">Koşullu mantıksal işleçleri doğrudan aşırı yüklemek mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2293">It is not possible to directly overload the conditional logical operators.</span></span> <span data-ttu-id="2886a-2294">Ancak, Koşullu mantıksal işleçler normal mantıksal işleçler açısından değerlendirildiğinden, normal mantıksal operatörlerin aşırı yüklemeleri, Koşullu mantıksal işleçlerin aşırı yüklerini de kabul edilen belirli kısıtlamalarla birlikte bulunur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2294">However, because the conditional logical operators are evaluated in terms of the regular logical operators, overloads of the regular logical operators are, with certain restrictions, also considered overloads of the conditional logical operators.</span></span> <span data-ttu-id="2886a-2295">Bu, [Kullanıcı tanımlı Koşullu mantıksal işleçlerde](expressions.md#user-defined-conditional-logical-operators)daha ayrıntılı olarak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2295">This is described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

### <a name="boolean-conditional-logical-operators"></a><span data-ttu-id="2886a-2296">Boole Koşullu mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="2886a-2296">Boolean conditional logical operators</span></span>

<span data-ttu-id="2886a-2297">`&&` veya `||` işlenenleri `bool`, ya da işlenenleri geçerli bir `operator &` veya `operator |`tanımlamayan, ancak `bool`örtük dönüştürmeler tanımlamadığında, işlem şu şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2297">When the operands of `&&` or `||` are of type `bool`, or when the operands are of types that do not define an applicable `operator &` or `operator |`, but do define implicit conversions to `bool`, the operation is processed as follows:</span></span>

*  <span data-ttu-id="2886a-2298">İşlem `x && y` `x ? y : false`olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2298">The operation `x && y` is evaluated as `x ? y : false`.</span></span> <span data-ttu-id="2886a-2299">Diğer bir deyişle, `x` ilk olarak değerlendirilir ve `bool`türüne dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2299">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="2886a-2300">`x` `true`, `y` değerlendirilir ve `bool`türüne dönüştürülür ve bu işlemin sonucu olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2300">Then, if `x` is `true`, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span> <span data-ttu-id="2886a-2301">Aksi takdirde, işlemin sonucu `false`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2301">Otherwise, the result of the operation is `false`.</span></span>
*  <span data-ttu-id="2886a-2302">İşlem `x || y` `x ? true : y`olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2302">The operation `x || y` is evaluated as `x ? true : y`.</span></span> <span data-ttu-id="2886a-2303">Diğer bir deyişle, `x` ilk olarak değerlendirilir ve `bool`türüne dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2303">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="2886a-2304">`x` `true`, işlemin sonucu `true`olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2304">Then, if `x` is `true`, the result of the operation is `true`.</span></span> <span data-ttu-id="2886a-2305">Aksi takdirde, `y` değerlendirilir ve `bool`türüne dönüştürülür ve bu işlemin sonucu olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2305">Otherwise, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span>

### <a name="user-defined-conditional-logical-operators"></a><span data-ttu-id="2886a-2306">Kullanıcı tanımlı Koşullu mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="2886a-2306">User-defined conditional logical operators</span></span>

<span data-ttu-id="2886a-2307">`&&` veya `||` işlenenleri, Kullanıcı tanımlı geçerli bir `operator &` veya `operator |`bildiren türlerse, aşağıdakilerin her ikisi de doğru olmalıdır; burada `T`, seçili işlecin bildirildiği türdür:</span><span class="sxs-lookup"><span data-stu-id="2886a-2307">When the operands of `&&` or `||` are of types that declare an applicable user-defined `operator &` or `operator |`, both of the following must be true, where `T` is the type in which the selected operator is declared:</span></span>

*  <span data-ttu-id="2886a-2308">Seçili işlecin her bir parametresinin dönüş türü ve türü `T`olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2308">The return type and the type of each parameter of the selected operator must be `T`.</span></span> <span data-ttu-id="2886a-2309">Diğer bir deyişle, işleç, mantıksal `AND` veya `T`türünde iki işlenenin mantıksal `OR` hesaplamalıdır ve `T`türünde bir sonuç döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2309">In other words, the operator must compute the logical `AND` or the logical `OR` of two operands of type `T`, and must return a result of type `T`.</span></span>
*  <span data-ttu-id="2886a-2310">`T`, `operator true` ve `operator false`bildirimleri içermelidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2310">`T` must contain declarations of `operator true` and `operator false`.</span></span>

<span data-ttu-id="2886a-2311">Bu gereksinimlerin herhangi biri karşılanmıyorsa bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2311">A binding-time error occurs if either of these requirements is not satisfied.</span></span> <span data-ttu-id="2886a-2312">Aksi takdirde, `&&` veya `||` işlemi, Kullanıcı tanımlı `operator true` veya `operator false` seçili kullanıcı tanımlı işleçle birleştirilerek değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2312">Otherwise, the `&&` or `||` operation is evaluated by combining the user-defined `operator true` or `operator false` with the selected user-defined operator:</span></span>

*  <span data-ttu-id="2886a-2313">İşlem `x && y` `T.false(x) ? x : T.&(x, y)`olarak değerlendirilir; burada `T.false(x)` `T`olarak belirtilen `operator false` bir çağrıdır ve `T.&(x, y)` seçilen `operator &`çağrıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2313">The operation `x && y` is evaluated as `T.false(x) ? x : T.&(x, y)`, where `T.false(x)` is an invocation of the `operator false` declared in `T`, and `T.&(x, y)` is an invocation of the selected `operator &`.</span></span> <span data-ttu-id="2886a-2314">Diğer bir deyişle `x` ilk değerlendirilir ve `x` kesinlikle yanlış olduğunu anlamak için `operator false` sonuç üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2314">In other words, `x` is first evaluated and `operator false` is invoked on the result to determine if `x` is definitely false.</span></span> <span data-ttu-id="2886a-2315">`x` kesinlikle false ise, işlemin sonucu daha önce `x`için hesaplanan değerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2315">Then, if `x` is definitely false, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="2886a-2316">Aksi takdirde, `y` değerlendirilir ve seçilen `operator &`, daha önce `x` için hesaplanan değerde ve işlemin sonucunu üretmek için `y` için hesaplanan değer üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2316">Otherwise, `y` is evaluated, and the selected `operator &` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>
*  <span data-ttu-id="2886a-2317">İşlem `x || y` `T.true(x) ? x : T.|(x, y)`olarak değerlendirilir; burada `T.true(x)` `T`olarak belirtilen `operator true` bir çağrıdır ve `T.|(x,y)` seçilen `operator|`çağrıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2317">The operation `x || y` is evaluated as `T.true(x) ? x : T.|(x, y)`, where `T.true(x)` is an invocation of the `operator true` declared in `T`, and `T.|(x,y)` is an invocation of the selected `operator|`.</span></span> <span data-ttu-id="2886a-2318">Diğer bir deyişle, `x` ilk değerlendirilir ve `x` kesinlikle doğru doğru olup olmadığını anlamak için `operator true` sonuç üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2318">In other words, `x` is first evaluated and `operator true` is invoked on the result to determine if `x` is definitely true.</span></span> <span data-ttu-id="2886a-2319">Daha sonra, `x` kesinlikle doğru ise, işlemin sonucu daha önce `x`için hesaplanan değerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2319">Then, if `x` is definitely true, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="2886a-2320">Aksi takdirde, `y` değerlendirilir ve seçilen `operator |`, daha önce `x` için hesaplanan değerde ve işlemin sonucunu üretmek için `y` için hesaplanan değer üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2320">Otherwise, `y` is evaluated, and the selected `operator |` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>

<span data-ttu-id="2886a-2321">Bu işlemlerden birinde, `x` tarafından verilen ifade yalnızca bir kez değerlendirilir ve `y` tarafından verilen ifade değerlendirilmez ya da tam olarak bir kez değerlendirilmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-2321">In either of these operations, the expression given by `x` is only evaluated once, and the expression given by `y` is either not evaluated or evaluated exactly once.</span></span>

<span data-ttu-id="2886a-2322">`operator true` ve `operator false`uygulayan bir tür örneği için bkz. [veritabanı Boole türü](structs.md#database-boolean-type).</span><span class="sxs-lookup"><span data-stu-id="2886a-2322">For an example of a type that implements `operator true` and `operator false`, see [Database boolean type](structs.md#database-boolean-type).</span></span>

## <a name="the-null-coalescing-operator"></a><span data-ttu-id="2886a-2323">Null birleştirme işleci</span><span class="sxs-lookup"><span data-stu-id="2886a-2323">The null coalescing operator</span></span>

<span data-ttu-id="2886a-2324">`??` işlecine null birleşim işleci denir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2324">The `??` operator is called the null coalescing operator.</span></span>

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

<span data-ttu-id="2886a-2325">Form `a ?? b` bir boş birleştirme ifadesi, `a` null yapılabilir bir tür veya başvuru türü olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2325">A null coalescing expression of the form `a ?? b` requires `a` to be of a nullable type or reference type.</span></span> <span data-ttu-id="2886a-2326">Null olmayan `a`, `a ?? b` sonucu `a`; Aksi takdirde, sonuç `b`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2326">If `a` is non-null, the result of `a ?? b` is `a`; otherwise, the result is `b`.</span></span> <span data-ttu-id="2886a-2327">İşlem yalnızca `a` null ise `b` değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2327">The operation evaluates `b` only if `a` is null.</span></span>

<span data-ttu-id="2886a-2328">Null birleştirme işleci, işlemlerin sağdan sola gruplanarak doğru ilişkilendirilebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2328">The null coalescing operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="2886a-2329">Örneğin, `a ?? b ?? c` form ifadesi `a ?? (b ?? c)`olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2329">For example, an expression of the form `a ?? b ?? c` is evaluated as `a ?? (b ?? c)`.</span></span> <span data-ttu-id="2886a-2330">Genel koşullarda, form `E1 ?? E2 ?? ... ?? En` bir ifade, null olmayan veya tüm işlenenler null ise null olan işlenenlerin Birincini döndürür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2330">In general terms, an expression of the form `E1 ?? E2 ?? ... ?? En` returns the first of the operands that is non-null, or null if all operands are null.</span></span>

<span data-ttu-id="2886a-2331">`a ?? b` ifadenin türü, işlenenlerde hangi örtük dönüştürmelerin kullanılabilir olduğuna bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2331">The type of the expression `a ?? b` depends on which implicit conversions are available on the operands.</span></span> <span data-ttu-id="2886a-2332">Tercih sırasına göre `a ?? b` türü `A0`, `A`veya `B`, burada `A` tür olan `a` (`a` bir tür olduğunda), `B` türü `b` olur (`b` bir tür olduğunda) ve `A0`, null yapılabilir bir tür ise `A` temel türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2332">In order of preference, the type of `a ?? b` is `A0`, `A`, or `B`, where `A` is the type of `a` (provided that `a` has a type), `B` is the type of `b` (provided that `b` has a type), and `A0` is the underlying type of `A` if `A` is a nullable type, or `A` otherwise.</span></span> <span data-ttu-id="2886a-2333">Özellikle, `a ?? b` aşağıdaki şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2333">Specifically, `a ?? b` is processed as follows:</span></span>

*  <span data-ttu-id="2886a-2334">`A` varsa ve null yapılabilir bir tür ya da bir başvuru türü değilse, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2334">If `A` exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>
*  <span data-ttu-id="2886a-2335">`b` dinamik bir ifadesiyse, sonuç türü `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2335">If `b` is a dynamic expression, the result type is `dynamic`.</span></span> <span data-ttu-id="2886a-2336">Çalışma zamanında `a` ilk olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2336">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="2886a-2337">`a` null değilse, `a` dinamik olarak dönüştürülür ve bu sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2337">If `a` is not null, `a` is converted to dynamic, and this becomes the result.</span></span> <span data-ttu-id="2886a-2338">Aksi takdirde, `b` değerlendirilir ve bu sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2338">Otherwise, `b` is evaluated, and this becomes the result.</span></span>
*  <span data-ttu-id="2886a-2339">Aksi takdirde, `A` varsa ve null yapılabilir bir tür ise ve `b` `A0`'e örtük bir dönüştürme varsa, sonuç türü `A0`olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2339">Otherwise, if `A` exists and is a nullable type and an implicit conversion exists from `b` to `A0`, the result type is `A0`.</span></span> <span data-ttu-id="2886a-2340">Çalışma zamanında `a` ilk olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2340">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="2886a-2341">`a` null değilse, `a` `A0`tipine geri sarılır ve bu sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2341">If `a` is not null, `a` is unwrapped to type `A0`, and this becomes the result.</span></span> <span data-ttu-id="2886a-2342">Aksi takdirde, `b` değerlendirilir ve `A0`türüne dönüştürülür ve bu sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2342">Otherwise, `b` is evaluated and converted to type `A0`, and this becomes the result.</span></span>
*  <span data-ttu-id="2886a-2343">Aksi takdirde, `A` varsa ve `b` `A`'e örtük bir dönüştürme varsa, sonuç türü `A`olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2343">Otherwise, if `A` exists and an implicit conversion exists from `b` to `A`, the result type is `A`.</span></span> <span data-ttu-id="2886a-2344">Çalışma zamanında `a` ilk olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2344">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="2886a-2345">`a` null değilse, `a` sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2345">If `a` is not null, `a` becomes the result.</span></span> <span data-ttu-id="2886a-2346">Aksi takdirde, `b` değerlendirilir ve `A`türüne dönüştürülür ve bu sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2346">Otherwise, `b` is evaluated and converted to type `A`, and this becomes the result.</span></span>
*  <span data-ttu-id="2886a-2347">Aksi takdirde, `b` `B` bir tür varsa ve `B``a` bir örtük dönüştürme varsa, sonuç türü `B`olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2347">Otherwise, if `b` has a type `B` and an implicit conversion exists from `a` to `B`, the result type is `B`.</span></span> <span data-ttu-id="2886a-2348">Çalışma zamanında `a` ilk olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2348">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="2886a-2349">`a` null değilse, `A0` `a` (`A` varsa ve null yapılabilir) ve `B`türüne dönüştürülürse, bu, bu sonuç haline gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2349">If `a` is not null, `a` is unwrapped to type `A0` (if `A` exists and is nullable) and converted to type `B`, and this becomes the result.</span></span> <span data-ttu-id="2886a-2350">Aksi takdirde, `b` değerlendirilir ve sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2350">Otherwise, `b` is evaluated and becomes the result.</span></span>
*  <span data-ttu-id="2886a-2351">Aksi takdirde, `a` ve `b` uyumsuzdur ve derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2351">Otherwise, `a` and `b` are incompatible, and a compile-time error occurs.</span></span>

## <a name="conditional-operator"></a><span data-ttu-id="2886a-2352">Koşullu işleç</span><span class="sxs-lookup"><span data-stu-id="2886a-2352">Conditional operator</span></span>

<span data-ttu-id="2886a-2353">`?:` işlecine koşullu işleç denir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2353">The `?:` operator is called the conditional operator.</span></span> <span data-ttu-id="2886a-2354">Üçlü işleç olarak da adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2354">It is at times also called the ternary operator.</span></span>

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

<span data-ttu-id="2886a-2355">Formun koşullu bir ifadesi öncelikle koşul `b`değerlendirir `b ? x : y`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2355">A conditional expression of the form `b ? x : y` first evaluates the condition `b`.</span></span> <span data-ttu-id="2886a-2356">`b` `true`, `x` değerlendirilir ve işlemin sonucu olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2356">Then, if `b` is `true`, `x` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="2886a-2357">Aksi takdirde, `y` değerlendirilir ve işlemin sonucu olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2357">Otherwise, `y` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="2886a-2358">Koşullu bir ifade `x` ve `y`hiçbir şekilde değerlendirmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-2358">A conditional expression never evaluates both `x` and `y`.</span></span>

<span data-ttu-id="2886a-2359">Koşullu operatör doğru ilişkilendirilebilir, yani işlemler sağdan sola gruplandırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2359">The conditional operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="2886a-2360">Örneğin, `a ? b : c ? d : e` form ifadesi `a ? b : (c ? d : e)`olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2360">For example, an expression of the form `a ? b : c ? d : e` is evaluated as `a ? b : (c ? d : e)`.</span></span>

<span data-ttu-id="2886a-2361">`?:` işlecinin ilk işleneni, örtük olarak `bool`veya `operator true`uygulayan bir türün bir ifadesi olarak dönüştürülebileceği bir ifade olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2361">The first operand of the `?:` operator must be an expression that can be implicitly converted to `bool`, or an expression of a type that implements `operator true`.</span></span> <span data-ttu-id="2886a-2362">Bu gereksinimlerin hiçbiri karşılanmazsa, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2362">If neither of these requirements is satisfied, a compile-time error occurs.</span></span>

<span data-ttu-id="2886a-2363">`?:` işlecinin ikinci ve üçüncü işlenenleri, `x` ve `y`, koşullu ifadenin türünü denetler.</span><span class="sxs-lookup"><span data-stu-id="2886a-2363">The second and third operands, `x` and `y`, of the `?:` operator control the type of the conditional expression.</span></span>

*  <span data-ttu-id="2886a-2364">`x` türü `X` varsa ve `y` tür `Y` sahipse</span><span class="sxs-lookup"><span data-stu-id="2886a-2364">If `x` has type `X` and `y` has type `Y` then</span></span>
   * <span data-ttu-id="2886a-2365">Örtük dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)), `X` `Y`, ancak `Y` ' den `X`' a varsa, `Y` koşullu ifadenin türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2365">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `X` to `Y`, but not from `Y` to `X`, then `Y` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="2886a-2366">Örtük dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)), `Y` `X`, ancak `X` ' den `Y`' a varsa, `X` koşullu ifadenin türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2366">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `Y` to `X`, but not from `X` to `Y`, then `X` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="2886a-2367">Aksi takdirde, hiçbir ifade türü belirlenemez ve derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2367">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="2886a-2368">Yalnızca bir `x` ve `y` bir tür varsa ve hem `x` hem de `y`, bu türe örtülü olarak dönüştürülebilir ve sonra koşullu ifadenin türüdür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2368">If only one of `x` and `y` has a type, and both `x` and `y`, of are implicitly convertible to that type, then that is the type of the conditional expression.</span></span>
*  <span data-ttu-id="2886a-2369">Aksi takdirde, hiçbir ifade türü belirlenemez ve derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2369">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>

<span data-ttu-id="2886a-2370">Formun koşullu ifadesinin çalışma zamanı işleme `b ? x : y` aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="2886a-2370">The run-time processing of a conditional expression of the form `b ? x : y` consists of the following steps:</span></span>

*  <span data-ttu-id="2886a-2371">İlk olarak, `b` değerlendirilir ve `b` `bool` değeri belirlenir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2371">First, `b` is evaluated, and the `bool` value of `b` is determined:</span></span>
   * <span data-ttu-id="2886a-2372">`b` türünden `bool` örtük bir dönüştürme varsa, bu örtük dönüştürme bir `bool` değeri üretmek için gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2372">If an implicit conversion from the type of `b` to `bool` exists, then this implicit conversion is performed to produce a `bool` value.</span></span>
   * <span data-ttu-id="2886a-2373">Aksi takdirde, bir `bool` değeri üretmek için `b` türü tarafından tanımlanan `operator true` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2373">Otherwise, the `operator true` defined by the type of `b` is invoked to produce a `bool` value.</span></span>
*  <span data-ttu-id="2886a-2374">Yukarıdaki adım tarafından üretilen `bool` değeri `true`, `x` değerlendirilir ve koşullu ifadenin türüne dönüştürülür ve bu, koşullu ifadenin sonucu olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2374">If the `bool` value produced by the step above is `true`, then `x` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>
*  <span data-ttu-id="2886a-2375">Aksi takdirde, `y` değerlendirilir ve koşullu ifadenin türüne dönüştürülür ve bu, koşullu ifadenin sonucu olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2375">Otherwise, `y` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>

## <a name="anonymous-function-expressions"></a><span data-ttu-id="2886a-2376">Anonim işlev ifadeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2376">Anonymous function expressions</span></span>

<span data-ttu-id="2886a-2377">***Anonim işlev*** , "satır içi" yöntem tanımını temsil eden bir ifadedir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2377">An ***anonymous function*** is an expression that represents an "in-line" method definition.</span></span> <span data-ttu-id="2886a-2378">Anonim bir işlev, ve için bir değer veya tür içermez, ancak uyumlu bir temsilciye veya ifade ağacı türüne dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2378">An anonymous function does not have a value or type in and of itself, but is convertible to a compatible delegate or expression tree type.</span></span> <span data-ttu-id="2886a-2379">Anonim işlev dönüştürmenin değerlendirmesi, dönüştürmenin hedef türüne bağlıdır: bir temsilci türü ise, dönüştürme, anonim işlevin tanımladığı yönteme başvuran bir temsilci değeri olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2379">The evaluation of an anonymous function conversion depends on the target type of the conversion: If it is a delegate type, the conversion evaluates to a delegate value referencing the method which the anonymous function defines.</span></span> <span data-ttu-id="2886a-2380">Bir ifade ağacı türü ise dönüştürme, yöntemin yapısını bir nesne yapısı olarak temsil eden bir ifade ağacı olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2380">If it is an expression tree type, the conversion evaluates to an expression tree which represents the structure of the method as an object structure.</span></span>

<span data-ttu-id="2886a-2381">Geçmiş nedenlerle anonim işlevlerin iki sözdizimsel özelliği vardır, örneğin *lambda_expression*s ve *anonymous_method_expression*s.</span><span class="sxs-lookup"><span data-stu-id="2886a-2381">For historical reasons there are two syntactic flavors of anonymous functions, namely *lambda_expression*s and *anonymous_method_expression*s.</span></span> <span data-ttu-id="2886a-2382">Neredeyse tüm amaçlar için *lambda_expression*s, geriye doğru uyumluluk için dilde kalan *anonymous_method_expression*s ' den daha kısa ve açıklayıcı bir şekilde bulunur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2382">For almost all purposes, *lambda_expression*s are more concise and expressive than *anonymous_method_expression*s, which remain in the language for backwards compatibility.</span></span>

```antlr
lambda_expression
    : anonymous_function_signature '=>' anonymous_function_body
    ;

anonymous_method_expression
    : 'delegate' explicit_anonymous_function_signature? block
    ;

anonymous_function_signature
    : explicit_anonymous_function_signature
    | implicit_anonymous_function_signature
    ;

explicit_anonymous_function_signature
    : '(' explicit_anonymous_function_parameter_list? ')'
    ;

explicit_anonymous_function_parameter_list
    : explicit_anonymous_function_parameter (',' explicit_anonymous_function_parameter)*
    ;

explicit_anonymous_function_parameter
    : anonymous_function_parameter_modifier? type identifier
    ;

anonymous_function_parameter_modifier
    : 'ref'
    | 'out'
    ;

implicit_anonymous_function_signature
    : '(' implicit_anonymous_function_parameter_list? ')'
    | implicit_anonymous_function_parameter
    ;

implicit_anonymous_function_parameter_list
    : implicit_anonymous_function_parameter (',' implicit_anonymous_function_parameter)*
    ;

implicit_anonymous_function_parameter
    : identifier
    ;

anonymous_function_body
    : expression
    | block
    ;
```

<span data-ttu-id="2886a-2383">`=>` işleci atama (`=`) ile aynı önceliğe sahiptir ve doğru ilişkilendirilebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2383">The `=>` operator has the same precedence as assignment (`=`) and is right-associative.</span></span>

<span data-ttu-id="2886a-2384">`async` değiştiricisine sahip anonim bir işlev zaman uyumsuz bir işlevdir ve [yineleyiciler](classes.md#iterators)bölümünde açıklanan kuralları izler.</span><span class="sxs-lookup"><span data-stu-id="2886a-2384">An anonymous function with the `async` modifier is an async function and follows the rules described in [Iterators](classes.md#iterators).</span></span>

<span data-ttu-id="2886a-2385">Bir *lambda_expression* biçimindeki anonim işlevin parametreleri açık veya örtük olarak yazılabilir olabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2385">The parameters of an anonymous function in the form of a *lambda_expression* can be explicitly or implicitly typed.</span></span> <span data-ttu-id="2886a-2386">Açıkça yazılmış bir parametre listesinde, her parametrenin türü açıkça belirtilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2386">In an explicitly typed parameter list, the type of each parameter is explicitly stated.</span></span> <span data-ttu-id="2886a-2387">Örtük olarak yazılmış bir parametre listesinde, parametrelerin türleri, anonim işlevin gerçekleştiği bağlamdan çıkarslanmıştır — özellikle, anonim işlev uyumlu bir temsilci türüne veya ifade ağacı türüne dönüştürüldüğünde, bu tür parametre türlerini ([anonim işlev dönüştürmeleri](conversions.md#anonymous-function-conversions)) sağlar.</span><span class="sxs-lookup"><span data-stu-id="2886a-2387">In an implicitly typed parameter list, the types of the parameters are inferred from the context in which the anonymous function occurs—specifically, when the anonymous function is converted to a compatible delegate type or expression tree type, that type provides the parameter types ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="2886a-2388">Tek ve örtük olarak yazılmış bir parametreye sahip anonim bir işlevde, parantez parametre listesinden atlanabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2388">In an anonymous function with a single, implicitly typed parameter, the parentheses may be omitted from the parameter list.</span></span> <span data-ttu-id="2886a-2389">Diğer bir deyişle, formun anonim bir işlevi</span><span class="sxs-lookup"><span data-stu-id="2886a-2389">In other words, an anonymous function of the form</span></span>
```csharp
( param ) => expr
```
<span data-ttu-id="2886a-2390">kısaltılabilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2390">can be abbreviated to</span></span>
```csharp
param => expr
```

<span data-ttu-id="2886a-2391">Bir *anonymous_method_expression* biçimindeki adsız işlevin parametre listesi isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2391">The parameter list of an anonymous function in the form of an *anonymous_method_expression* is optional.</span></span> <span data-ttu-id="2886a-2392">Belirtilmişse, parametrelerin açıkça yazılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2392">If given, the parameters must be explicitly typed.</span></span> <span data-ttu-id="2886a-2393">Aksi takdirde, anonim işlev `out` parametreleri içermeyen herhangi bir parametre listesi olan bir temsilciye dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2393">If not, the anonymous function is convertible to a delegate with any parameter list not containing `out` parameters.</span></span>

<span data-ttu-id="2886a-2394">Adsız bir işlevin *blok* gövdesine erişilebilir ([bitiş noktaları ve erişilebilirlik](statements.md#end-points-and-reachability)). Bu, anonim işlev ulaşılamaz bir bildirimde gerçekleşmezse.</span><span class="sxs-lookup"><span data-stu-id="2886a-2394">A *block* body of an anonymous function is reachable ([End points and reachability](statements.md#end-points-and-reachability)) unless the anonymous function occurs inside an unreachable statement.</span></span>

<span data-ttu-id="2886a-2395">Anonim işlevlere bazı örnekler aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2395">Some examples of anonymous functions follow below:</span></span>

```csharp
x => x + 1                              // Implicitly typed, expression body
x => { return x + 1; }                  // Implicitly typed, statement body
(int x) => x + 1                        // Explicitly typed, expression body
(int x) => { return x + 1; }            // Explicitly typed, statement body
(x, y) => x * y                         // Multiple parameters
() => Console.WriteLine()               // No parameters
async (t1,t2) => await t1 + await t2    // Async
delegate (int x) { return x + 1; }      // Anonymous method expression
delegate { return 1 + 1; }              // Parameter list omitted
```

<span data-ttu-id="2886a-2396">*Lambda_expression*s ve *anonymous_method_expression*s davranışı aşağıdaki noktaları hariç aynıdır:</span><span class="sxs-lookup"><span data-stu-id="2886a-2396">The behavior of *lambda_expression*s and *anonymous_method_expression*s is the same except for the following points:</span></span>

*  <span data-ttu-id="2886a-2397">*anonymous_method_expression*s, parametre listesinin tamamen atlanmasına izin verir ve herhangi bir değer parametreleri listesinin temsilci türlerine göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2397">*anonymous_method_expression*s permit the parameter list to be omitted entirely, yielding convertibility to delegate types of any list of value parameters.</span></span>
*  <span data-ttu-id="2886a-2398">*lambda_expression*s parametre türlerinin atlanmasına ve çıkarlanmasına izin verirken, *anonymous_method_expression*s parametre türlerinin açıkça belirtilmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2398">*lambda_expression*s permit parameter types to be omitted and inferred whereas *anonymous_method_expression*s require parameter types to be explicitly stated.</span></span>
*  <span data-ttu-id="2886a-2399">Bir *lambda_expression* gövdesi bir ifade veya deyim bloğu olabilir, ancak bir *anonymous_method_expression* gövdesi bir deyim bloğu olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2399">The body of a *lambda_expression* can be an expression or a statement block whereas the body of an *anonymous_method_expression* must be a statement block.</span></span>
*  <span data-ttu-id="2886a-2400">Yalnızca *lambda_expression*s, uyumlu ifade ağacı türlerine Dönüştürmelere sahiptir ([ifade ağacı türleri](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="2886a-2400">Only *lambda_expression*s have conversions to compatible expression tree types ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-signatures"></a><span data-ttu-id="2886a-2401">Anonim işlev imzaları</span><span class="sxs-lookup"><span data-stu-id="2886a-2401">Anonymous function signatures</span></span>

<span data-ttu-id="2886a-2402">Anonim işlev için isteğe bağlı *anonymous_function_signature* adları ve isteğe bağlı olarak anonim işlev için biçimsel parametrelerin türlerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="2886a-2402">The optional *anonymous_function_signature* of an anonymous function defines the names and optionally the types of the formal parameters for the anonymous function.</span></span> <span data-ttu-id="2886a-2403">Anonim işlevin parametrelerinin kapsamı *anonymous_function_body*.</span><span class="sxs-lookup"><span data-stu-id="2886a-2403">The scope of the parameters of the anonymous function is the *anonymous_function_body*.</span></span> <span data-ttu-id="2886a-2404">([Kapsamlar](basic-concepts.md#scopes)) Parametre listesi ile birlikte (verildiyse) anonim-yöntem-gövde bir bildirim alanı ([Bildirimler](basic-concepts.md#declarations)) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2404">([Scopes](basic-concepts.md#scopes)) Together with the parameter list (if given) the anonymous-method-body constitutes a declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="2886a-2405">Bu nedenle, anonim işlev parametresinin adı için bir yerel değişkenin adı, yerel sabit veya kapsamı *anonymous_method_expression* ya da *lambda_expression*içeren bir parametre için derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2405">It is thus a compile-time error for the name of a parameter of the anonymous function to match the name of a local variable, local constant or parameter whose scope includes the *anonymous_method_expression* or *lambda_expression*.</span></span>

<span data-ttu-id="2886a-2406">Anonim bir işlevde *explicit_anonymous_function_signature*varsa, uyumlu temsilci türleri ve ifade ağacı türleri kümesi aynı sırayla aynı parametre türleri ve değiştiricilerle kısıtlıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2406">If an anonymous function has an *explicit_anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have the same parameter types and modifiers in the same order.</span></span> <span data-ttu-id="2886a-2407">Yöntem grubu dönüştürmelerinde ([Yöntem grubu dönüştürmeleri](conversions.md#method-group-conversions)), anonim işlev parametre türlerinin Contra-varyansı desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-2407">In contrast to method group conversions ([Method group conversions](conversions.md#method-group-conversions)), contra-variance of anonymous function parameter types is not supported.</span></span> <span data-ttu-id="2886a-2408">Anonim bir işlevde *anonymous_function_signature*yoksa, uyumlu temsilci türleri ve ifade ağacı türleri kümesi, hiçbir `out` parametresi olmayan ile kısıtlıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2408">If an anonymous function does not have an *anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have no `out` parameters.</span></span>

<span data-ttu-id="2886a-2409">*Anonymous_function_signature* öznitelikler veya parametre dizisi içeremediğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2886a-2409">Note that an *anonymous_function_signature* cannot include attributes or a parameter array.</span></span> <span data-ttu-id="2886a-2410">Bununla birlikte, bir *anonymous_function_signature* , parametre listesi parametre dizisi içeren bir temsilci türüyle uyumlu olabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2410">Nevertheless, an *anonymous_function_signature* may be compatible with a delegate type whose parameter list contains a parameter array.</span></span>

<span data-ttu-id="2886a-2411">Ayrıca, aynı zamanda derleme zamanında ([ifade ağacı türleri](types.md#expression-tree-types)) hala başarısız olabilecek bir ifade ağacı türüne dönüştürme</span><span class="sxs-lookup"><span data-stu-id="2886a-2411">Note also that conversion to an expression tree type, even if compatible, may still fail at compile-time ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-bodies"></a><span data-ttu-id="2886a-2412">Anonim işlev gövdeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2412">Anonymous function bodies</span></span>

<span data-ttu-id="2886a-2413">Anonim bir işlevin gövdesi (*ifadesi* veya *bloğu*) aşağıdaki kurallara tabidir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2413">The body (*expression* or *block*) of an anonymous function is subject to the following rules:</span></span>

*  <span data-ttu-id="2886a-2414">Anonim işlev bir imza içeriyorsa, İmzada belirtilen parametreler gövdede kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2414">If the anonymous function includes a signature, the parameters specified in the signature are available in the body.</span></span> <span data-ttu-id="2886a-2415">Anonim işlevde imza yoksa, parametrelere sahip bir temsilci türüne veya ifade türüne dönüştürülebilir ([anonim işlev dönüştürmeleri](conversions.md#anonymous-function-conversions)), ancak parametrelere gövdede erişilemez.</span><span class="sxs-lookup"><span data-stu-id="2886a-2415">If the anonymous function has no signature it can be converted to a delegate type or expression type having parameters ([Anonymous function conversions](conversions.md#anonymous-function-conversions)), but the parameters cannot be accessed in the body.</span></span>
*  <span data-ttu-id="2886a-2416">En yakın kapsayan anonim işlevin imzasında (varsa) belirtilen `ref` veya `out` parametreleri dışında, gövdenin bir `ref` veya `out` parametresine erişmesi için derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2416">Except for `ref` or `out` parameters specified in the signature (if any) of the nearest enclosing anonymous function, it is a compile-time error for the body to access a `ref` or `out` parameter.</span></span>
*  <span data-ttu-id="2886a-2417">`this` türü bir yapı türünde olduğunda, gövdenin `this`erişmesi için derleme zamanı hatası olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2417">When the type of `this` is a struct type, it is a compile-time error for the body to access `this`.</span></span> <span data-ttu-id="2886a-2418">Bu, erişimin açık (`this.x`gibi) veya örtük (`x`, yapının örnek üyesi olduğu `x` gibi) için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2418">This is true whether the access is explicit (as in `this.x`) or implicit (as in `x` where `x` is an instance member of the struct).</span></span> <span data-ttu-id="2886a-2419">Bu kural yalnızca bu erişimi yasaklar ve üye aramanın yapının bir üyesi olup olmadığını etkilemez.</span><span class="sxs-lookup"><span data-stu-id="2886a-2419">This rule simply prohibits such access and does not affect whether member lookup results in a member of the struct.</span></span>
*  <span data-ttu-id="2886a-2420">Gövde, anonim işlevin dış değişkenlerine ([dış değişkenler](expressions.md#outer-variables)) erişebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2420">The body has access to the outer variables ([Outer variables](expressions.md#outer-variables)) of the anonymous function.</span></span> <span data-ttu-id="2886a-2421">Bir dış değişkene erişim, *lambda_expression* veya *anonymous_method_expression* değerlendirildiği sırada etkin olan değişkenin örneğine başvuracaktır ([anonim işlev ifadelerinin değerlendirmesi](expressions.md#evaluation-of-anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="2886a-2421">Access of an outer variable will reference the instance of the variable that is active at the time the *lambda_expression* or *anonymous_method_expression* is evaluated ([Evaluation of anonymous function expressions](expressions.md#evaluation-of-anonymous-function-expressions)).</span></span>
*  <span data-ttu-id="2886a-2422">Gövdesi için bir derleme zamanı hatası, örneğin, hedefi gövdenin dışında veya kapsanan bir anonim işlevin gövdesinde olan bir `goto` bir ifade, `break` ifade veya `continue` ifadesini içermelidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2422">It is a compile-time error for the body to contain a `goto` statement, `break` statement, or `continue` statement whose target is outside the body or within the body of a contained anonymous function.</span></span>
*  <span data-ttu-id="2886a-2423">Gövdedeki bir `return` ifade, kapsayan işlev üyesinden değil, en yakın kapsayan anonim işlev çağrısından denetim döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="2886a-2423">A `return` statement in the body returns control from an invocation of the nearest enclosing anonymous function, not from the enclosing function member.</span></span> <span data-ttu-id="2886a-2424">`return` deyiminde belirtilen bir ifade, en yakın kapsayan *lambda_expression* veya *Anonymous_method_expression* dönüştürüldüğü ([anonim işlev dönüştürmeleri](conversions.md#anonymous-function-conversions)), temsilci türünün veya ifade ağacı türünün dönüş türüne örtülü olarak dönüştürülebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2424">An expression specified in a `return` statement must be implicitly convertible to the return type of the delegate type or expression tree type to which the nearest enclosing *lambda_expression* or *anonymous_method_expression* is converted ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="2886a-2425">*Lambda_expression* veya *anonymous_method_expression*için değerlendirme ve çağırma aracılığıyla değil, anonim bir işlevin bloğunu yürütmek için bir yol olup olmadığı açıkça belirtilmemiş.</span><span class="sxs-lookup"><span data-stu-id="2886a-2425">It is explicitly unspecified whether there is any way to execute the block of an anonymous function other than through evaluation and invocation of the *lambda_expression* or *anonymous_method_expression*.</span></span> <span data-ttu-id="2886a-2426">Özellikle, derleyici bir veya daha fazla adlandırılmış yöntemi ya da türü birleştirerek anonim bir işlev uygulamayı tercih edebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2426">In particular, the compiler may choose to implement an anonymous function by synthesizing one or more named methods or types.</span></span> <span data-ttu-id="2886a-2427">Bu tür birleştirilmiş öğelerin adları, derleyici kullanımı için ayrılmış bir biçimde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2427">The names of any such synthesized elements must be of a form reserved for compiler use.</span></span>

### <a name="overload-resolution-and-anonymous-functions"></a><span data-ttu-id="2886a-2428">Aşırı yükleme çözümlemesi ve anonim işlevler</span><span class="sxs-lookup"><span data-stu-id="2886a-2428">Overload resolution and anonymous functions</span></span>

<span data-ttu-id="2886a-2429">Bir bağımsız değişken listesindeki anonim işlevler tür çıkarımı ve aşırı yükleme çözümüne katılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2429">Anonymous functions in an argument list participate in type inference and overload resolution.</span></span> <span data-ttu-id="2886a-2430">Tam kurallar için lütfen [tür çıkarımı](expressions.md#type-inference) ve [aşırı yükleme çözümlemesi](expressions.md#overload-resolution) ' ne bakın.</span><span class="sxs-lookup"><span data-stu-id="2886a-2430">Please refer to [Type inference](expressions.md#type-inference) and [Overload resolution](expressions.md#overload-resolution) for the exact rules.</span></span>

<span data-ttu-id="2886a-2431">Aşağıdaki örnekte, anonim işlevlerin aşırı yükleme çözünürlüğünde etkileri gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2431">The following example illustrates the effect of anonymous functions on overload resolution.</span></span>

```csharp
class ItemList<T>: List<T>
{
    public int Sum(Func<T,int> selector) {
        int sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }

    public double Sum(Func<T,double> selector) {
        double sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }
}
```

<span data-ttu-id="2886a-2432">`ItemList<T>` sınıfında iki `Sum` yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2432">The `ItemList<T>` class has two `Sum` methods.</span></span> <span data-ttu-id="2886a-2433">Her biri, bir liste öğesinden toplanacak değeri çıkaran bir `selector` bağımsız değişkeni alır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2433">Each takes a `selector` argument, which extracts the value to sum over from a list item.</span></span> <span data-ttu-id="2886a-2434">Ayıklanan değer bir `int` veya `double` olabilir ve sonuçta elde edilen Sum aynı şekilde bir `int` veya `double`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2434">The extracted value can be either an `int` or a `double` and the resulting sum is likewise either an `int` or a `double`.</span></span>

<span data-ttu-id="2886a-2435">`Sum` yöntemleri, bir siparişte ayrıntı satırları listesinden toplamları hesaplamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2435">The `Sum` methods could for example be used to compute sums from a list of detail lines in an order.</span></span>

```csharp
class Detail
{
    public int UnitCount;
    public double UnitPrice;
    ...
}

void ComputeSums() {
    ItemList<Detail> orderDetails = GetOrderDetails(...);
    int totalUnits = orderDetails.Sum(d => d.UnitCount);
    double orderTotal = orderDetails.Sum(d => d.UnitPrice * d.UnitCount);
    ...
}
```

<span data-ttu-id="2886a-2436">`orderDetails.Sum`ilk çağrısında, anonim işlev `d => d. UnitCount` hem `Func<Detail,int>` hem de `Func<Detail,double>`uyumlu olduğundan, her iki `Sum` yöntemi de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2436">In the first invocation of `orderDetails.Sum`, both `Sum` methods are applicable because the anonymous function `d => d. UnitCount` is compatible with both `Func<Detail,int>` and `Func<Detail,double>`.</span></span> <span data-ttu-id="2886a-2437">Ancak, `Func<Detail,int>` dönüştürme işlemi `Func<Detail,double>`dönüşümünden daha iyi olduğundan, aşırı yükleme çözümlemesi ilk `Sum` yöntemini seçer.</span><span class="sxs-lookup"><span data-stu-id="2886a-2437">However, overload resolution picks the first `Sum` method because the conversion to `Func<Detail,int>` is better than the conversion to `Func<Detail,double>`.</span></span>

<span data-ttu-id="2886a-2438">`orderDetails.Sum`ikinci çağrılışında, anonim işlev `d => d.UnitPrice * d.UnitCount` `double`türünde bir değer ürettiğinden yalnızca ikinci `Sum` yöntemi geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2438">In the second invocation of `orderDetails.Sum`, only the second `Sum` method is applicable because the anonymous function `d => d.UnitPrice * d.UnitCount` produces a value of type `double`.</span></span> <span data-ttu-id="2886a-2439">Bu nedenle, aşırı yükleme çözümlemesi bu çağrı için ikinci `Sum` yöntemini seçer.</span><span class="sxs-lookup"><span data-stu-id="2886a-2439">Thus, overload resolution picks the second `Sum` method for that invocation.</span></span>

### <a name="anonymous-functions-and-dynamic-binding"></a><span data-ttu-id="2886a-2440">Anonim işlevler ve dinamik bağlama</span><span class="sxs-lookup"><span data-stu-id="2886a-2440">Anonymous functions and dynamic binding</span></span>

<span data-ttu-id="2886a-2441">Anonim bir işlev, dinamik olarak bağlı bir işlemin alıcısı, bağımsız değişkeni veya işleneni olamaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-2441">An anonymous function cannot be a receiver, argument or operand of a dynamically bound operation.</span></span>

### <a name="outer-variables"></a><span data-ttu-id="2886a-2442">Dış değişkenler</span><span class="sxs-lookup"><span data-stu-id="2886a-2442">Outer variables</span></span>

<span data-ttu-id="2886a-2443">Kapsamı *lambda_expression* veya *anonymous_method_expression* içeren herhangi bir yerel değişken, değer parametresi veya parametre dizisine anonim işlevin ***dış değişkeni*** denir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2443">Any local variable, value parameter, or parameter array whose scope includes the *lambda_expression* or *anonymous_method_expression* is called an ***outer variable*** of the anonymous function.</span></span> <span data-ttu-id="2886a-2444">Bir sınıfın örnek işlev üyesinde, `this` değeri bir değer parametresi olarak değerlendirilir ve işlev üyesi içinde bulunan herhangi bir anonim işlevin dış değişkenidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2444">In an instance function member of a class, the `this` value is considered a value parameter and is an outer variable of any anonymous function contained within the function member.</span></span>

#### <a name="captured-outer-variables"></a><span data-ttu-id="2886a-2445">Yakalanan dış değişkenler</span><span class="sxs-lookup"><span data-stu-id="2886a-2445">Captured outer variables</span></span>

<span data-ttu-id="2886a-2446">Bir dış değişkene adsız bir işlev tarafından başvuruluyorsa, dış değişken anonim işlev tarafından ***yakalanarak*** bildirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2446">When an outer variable is referenced by an anonymous function, the outer variable is said to have been ***captured*** by the anonymous function.</span></span> <span data-ttu-id="2886a-2447">Genellikle, yerel bir değişkenin ömrü, ilişkili olduğu blok veya deyimin yürütmesi ile sınırlıdır ([yerel değişkenler](variables.md#local-variables)).</span><span class="sxs-lookup"><span data-stu-id="2886a-2447">Ordinarily, the lifetime of a local variable is limited to execution of the block or statement with which it is associated ([Local variables](variables.md#local-variables)).</span></span> <span data-ttu-id="2886a-2448">Ancak, yakalanan bir dış değişkenin ömrü, anonim işlevden oluşturulan temsilci veya ifade ağacı çöp toplama için uygun hale gelene kadar en az genişletilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2448">However, the lifetime of a captured outer variable is extended at least until the delegate or expression tree created from the anonymous function becomes eligible for garbage collection.</span></span>

<span data-ttu-id="2886a-2449">Örnekte</span><span class="sxs-lookup"><span data-stu-id="2886a-2449">In the example</span></span>
```csharp
using System;

delegate int D();

class Test
{
    static D F() {
        int x = 0;
        D result = () => ++x;
        return result;
    }

    static void Main() {
        D d = F();
        Console.WriteLine(d());
        Console.WriteLine(d());
        Console.WriteLine(d());
    }
}
```
<span data-ttu-id="2886a-2450">`x` yerel değişkeni anonim işlev tarafından yakalanır ve `x` yaşam süresi en az, `F` ' den döndürülen temsilci atık toplama için uygun hale gelene kadar (programın çok sona erene kadar gerçekleşmez) genişletilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2450">the local variable `x` is captured by the anonymous function, and the lifetime of `x` is extended at least until the delegate returned from `F` becomes eligible for garbage collection (which doesn't happen until the very end of the program).</span></span> <span data-ttu-id="2886a-2451">Anonymous işlevinin her çağrısı aynı `x`örneği üzerinde çalışır, örneğin çıktısı şu şekilde olur:</span><span class="sxs-lookup"><span data-stu-id="2886a-2451">Since each invocation of the anonymous function operates on the same instance of `x`, the output of the example is:</span></span>
```console
1
2
3
```

<span data-ttu-id="2886a-2452">Yerel bir değişken veya bir değer parametresi anonim bir işlev tarafından yakalandıktan sonra, yerel değişken veya parametre artık sabit bir değişken ([sabit ve taşınabilir değişkenler](unsafe-code.md#fixed-and-moveable-variables)) olarak kabul edilmez, ancak bunun yerine taşınabilir bir değişken olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2452">When a local variable or a value parameter is captured by an anonymous function, the local variable or parameter is no longer considered to be a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), but is instead considered to be a moveable variable.</span></span> <span data-ttu-id="2886a-2453">Bu nedenle, yakalanan bir dış değişkenin adresini alan `unsafe` kodun, önce değişkeni onarmak için `fixed` ifadesini kullanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2453">Thus any `unsafe` code that takes the address of a captured outer variable must first use the `fixed` statement to fix the variable.</span></span>

<span data-ttu-id="2886a-2454">Yakalanamayan bir değişkenin aksine, yakalanan bir yerel değişkenin aynı anda birden fazla yürütme iş parçacığına sunulabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2886a-2454">Note that unlike an uncaptured variable, a captured local variable can be simultaneously exposed to multiple threads of execution.</span></span>

#### <a name="instantiation-of-local-variables"></a><span data-ttu-id="2886a-2455">Yerel değişkenlerin örneklenmesi</span><span class="sxs-lookup"><span data-stu-id="2886a-2455">Instantiation of local variables</span></span>

<span data-ttu-id="2886a-2456">Bir yerel değişken, yürütme değişkenin kapsamına girdiğinde ***örnek*** olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2456">A local variable is considered to be ***instantiated*** when execution enters the scope of the variable.</span></span> <span data-ttu-id="2886a-2457">Örneğin, aşağıdaki yöntem çağrıldığında yerel değişken `x`, döngünün her yinelemesi için bir kez oluşturulur ve üç kez başlatılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2457">For example, when the following method is invoked, the local variable `x` is instantiated and initialized three times—once for each iteration of the loop.</span></span>

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="2886a-2458">Ancak, `x` bildirimini döngü dışına taşımak `x`tek bir örneklemede oluşur:</span><span class="sxs-lookup"><span data-stu-id="2886a-2458">However, moving the declaration of `x` outside the loop results in a single instantiation of `x`:</span></span>
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="2886a-2459">Yakalanmadığı zaman, bir yerel değişkenin hangi sıklıkta örneklendirildiğini tam olarak gözlemleyebilmenin bir yolu yoktur. örneklemelerin yaşam süreleri kopuk olduğundan, her örnekleme için aynı depolama konumunu kullanmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2459">When not captured, there is no way to observe exactly how often a local variable is instantiated—because the lifetimes of the instantiations are disjoint, it is possible for each instantiation to simply use the same storage location.</span></span> <span data-ttu-id="2886a-2460">Ancak, anonim bir işlev yerel bir değişken yakaladığında, örnekleme etkileri görünür hale gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2460">However, when an anonymous function captures a local variable, the effects of instantiation become apparent.</span></span>

<span data-ttu-id="2886a-2461">Örnek</span><span class="sxs-lookup"><span data-stu-id="2886a-2461">The example</span></span>
```csharp
using System;

delegate void D();

class Test
{
    static D[] F() {
        D[] result = new D[3];
        for (int i = 0; i < 3; i++) {
            int x = i * 2 + 1;
            result[i] = () => { Console.WriteLine(x); };
        }
        return result;
    }

    static void Main() {
        foreach (D d in F()) d();
    }
}
```
<span data-ttu-id="2886a-2462">çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2462">produces the output:</span></span>
```console
1
3
5
```

<span data-ttu-id="2886a-2463">Ancak, `x` bildirimi döngü dışına taşındığında:</span><span class="sxs-lookup"><span data-stu-id="2886a-2463">However, when the declaration of `x` is moved outside the loop:</span></span>
```csharp
static D[] F() {
    D[] result = new D[3];
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        result[i] = () => { Console.WriteLine(x); };
    }
    return result;
}
```
<span data-ttu-id="2886a-2464">çıktı:</span><span class="sxs-lookup"><span data-stu-id="2886a-2464">the output is:</span></span>
```console
5
5
5
```

<span data-ttu-id="2886a-2465">Bir for döngüsü bir yineleme değişkeni bildirirse, bu değişkenin kendisi döngünün dışında bildirildikleri kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2465">If a for-loop declares an iteration variable, that variable itself is considered to be declared outside of the loop.</span></span> <span data-ttu-id="2886a-2466">Bu nedenle, örnek yineleme değişkeninin kendisini yakalamak üzere değiştirilirse:</span><span class="sxs-lookup"><span data-stu-id="2886a-2466">Thus, if the example is changed to capture the iteration variable itself:</span></span>

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
<span data-ttu-id="2886a-2467">yalnızca bir yineleme değişkeni örneği yakalanır, bu da çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2467">only one instance of the iteration variable is captured, which produces the output:</span></span>
```console
3
3
3
```

<span data-ttu-id="2886a-2468">Bazı yakalanan değişkenlerin paylaşılması için anonim işlev temsilcilerinin, hala diğerlerinin farklı örneklerine sahip olması mümkündür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2468">It is possible for anonymous function delegates to share some captured variables yet have separate instances of others.</span></span> <span data-ttu-id="2886a-2469">Örneğin, `F` olarak değiştirilirse</span><span class="sxs-lookup"><span data-stu-id="2886a-2469">For example, if `F` is changed to</span></span>
```csharp
static D[] F() {
    D[] result = new D[3];
    int x = 0;
    for (int i = 0; i < 3; i++) {
        int y = 0;
        result[i] = () => { Console.WriteLine("{0} {1}", ++x, ++y); };
    }
    return result;
}
```
<span data-ttu-id="2886a-2470">üç temsilci aynı `x` örneğini yakalar, ancak `y`örneklerini ayırır ve çıkış şu şekilde olur:</span><span class="sxs-lookup"><span data-stu-id="2886a-2470">the three delegates capture the same instance of `x` but separate instances of `y`, and the output is:</span></span>
```console
1 1
2 1
3 1
```

<span data-ttu-id="2886a-2471">Ayrı anonim işlevler, bir dış değişkenin aynı örneğini yakalayabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2471">Separate anonymous functions can capture the same instance of an outer variable.</span></span> <span data-ttu-id="2886a-2472">Örnekte:</span><span class="sxs-lookup"><span data-stu-id="2886a-2472">In the example:</span></span>
```csharp
using System;

delegate void Setter(int value);

delegate int Getter();

class Test
{
    static void Main() {
        int x = 0;
        Setter s = (int value) => { x = value; };
        Getter g = () => { return x; };
        s(5);
        Console.WriteLine(g());
        s(10);
        Console.WriteLine(g());
    }
}
```
<span data-ttu-id="2886a-2473">iki anonim işlev, `x`yerel değişkenin aynı örneğini yakalar ve bu nedenle bu değişken aracılığıyla "iletişim kurabilir".</span><span class="sxs-lookup"><span data-stu-id="2886a-2473">the two anonymous functions capture the same instance of the local variable `x`, and they can thus "communicate" through that variable.</span></span> <span data-ttu-id="2886a-2474">Örneğin çıktısı şu şekilde olur:</span><span class="sxs-lookup"><span data-stu-id="2886a-2474">The output of the example is:</span></span>
```console
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a><span data-ttu-id="2886a-2475">Anonim işlev ifadelerinin değerlendirmesi</span><span class="sxs-lookup"><span data-stu-id="2886a-2475">Evaluation of anonymous function expressions</span></span>

<span data-ttu-id="2886a-2476">Anonim bir işlev `F` her zaman doğrudan veya bir temsilci oluşturma `new D(F)`ifadesinin yürütülmesi aracılığıyla bir temsilci türüne `D` ya da bir ifade ağacı türüne dönüştürülmelidir `E`olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2476">An anonymous function `F` must always be converted to a delegate type `D` or an expression tree type `E`, either directly or through the execution of a delegate creation expression `new D(F)`.</span></span> <span data-ttu-id="2886a-2477">Bu dönüştürme, anonim işlev [dönüştürmeleri](conversions.md#anonymous-function-conversions)bölümünde açıklandığı gibi anonim işlevin sonucunu belirler.</span><span class="sxs-lookup"><span data-stu-id="2886a-2477">This conversion determines the result of the anonymous function, as described in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>

## <a name="query-expressions"></a><span data-ttu-id="2886a-2478">Sorgu ifadeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2478">Query expressions</span></span>

<span data-ttu-id="2886a-2479">***Sorgu ifadeleri*** , SQL ve XQuery gibi ilişkisel ve hiyerarşik sorgu dillerine benzer sorgular için dil ile tümleşik bir sözdizimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="2886a-2479">***Query expressions*** provide a language integrated syntax for queries that is similar to relational and hierarchical query languages such as SQL and XQuery.</span></span>

```antlr
query_expression
    : from_clause query_body
    ;

from_clause
    : 'from' type? identifier 'in' expression
    ;

query_body
    : query_body_clauses? select_or_group_clause query_continuation?
    ;

query_body_clauses
    : query_body_clause
    | query_body_clauses query_body_clause
    ;

query_body_clause
    : from_clause
    | let_clause
    | where_clause
    | join_clause
    | join_into_clause
    | orderby_clause
    ;

let_clause
    : 'let' identifier '=' expression
    ;

where_clause
    : 'where' boolean_expression
    ;

join_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression
    ;

join_into_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression 'into' identifier
    ;

orderby_clause
    : 'orderby' orderings
    ;

orderings
    : ordering (',' ordering)*
    ;

ordering
    : expression ordering_direction?
    ;

ordering_direction
    : 'ascending'
    | 'descending'
    ;

select_or_group_clause
    : select_clause
    | group_clause
    ;

select_clause
    : 'select' expression
    ;

group_clause
    : 'group' expression 'by' expression
    ;

query_continuation
    : 'into' identifier query_body
    ;
```

<span data-ttu-id="2886a-2480">Sorgu ifadesi bir `from` yan tümcesiyle başlar ve `select` ya da `group` yan tümcesiyle biter.</span><span class="sxs-lookup"><span data-stu-id="2886a-2480">A query expression begins with a `from` clause and ends with either a `select` or `group` clause.</span></span> <span data-ttu-id="2886a-2481">İlk `from` yan tümcesinin arkasından sıfır veya daha fazla `from`, `let`, `where`, `join` veya `orderby` yan tümceleri gelebilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2481">The initial `from` clause can be followed by zero or more `from`, `let`, `where`, `join` or `orderby` clauses.</span></span> <span data-ttu-id="2886a-2482">Her `from` yan tümcesi, bir ***dizi***öğeleri üzerinde değişen bir ***Aralık değişkeni*** sunan bir üreticidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2482">Each `from` clause is a generator introducing a ***range variable*** which ranges over the elements of a ***sequence***.</span></span> <span data-ttu-id="2886a-2483">Her `let` yan tümcesi, önceki Aralık değişkenlerine göre hesaplanan bir değeri temsil eden bir Aralık değişkeni tanıtır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2483">Each `let` clause introduces a range variable representing a value computed by means of previous range variables.</span></span> <span data-ttu-id="2886a-2484">Her `where` yan tümcesi, nesneleri sonuçtan dışlayan bir filtredir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2484">Each `where` clause is a filter that excludes items from the result.</span></span> <span data-ttu-id="2886a-2485">Her `join` yan tümcesi, eşleşen çiftleri oluşturan kaynak dizinin belirtilen anahtarlarını başka bir dizinin anahtarlarıyla karşılaştırır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2485">Each `join` clause compares specified keys of the source sequence with keys of another sequence, yielding matching pairs.</span></span> <span data-ttu-id="2886a-2486">Her `orderby` yan tümcesi öğeleri belirtilen ölçütlere göre yeniden sıralar. Son `select` veya `group` yan tümcesi, Aralık değişkenleri açısından sonucun şeklini belirtir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2486">Each `orderby` clause reorders items according to specified criteria.The final `select` or `group` clause specifies the shape of the result in terms of the range variables.</span></span> <span data-ttu-id="2886a-2487">Son olarak, bir `into` yan tümcesi, bir sorgunun sonuçlarını sonraki bir sorguda Oluşturucu olarak düşünerek, "splice" sorguları için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2487">Finally, an `into` clause can be used to "splice" queries by treating the results of one query as a generator in a subsequent query.</span></span>

### <a name="ambiguities-in-query-expressions"></a><span data-ttu-id="2886a-2488">Sorgu ifadelerinde belirsizlikleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2488">Ambiguities in query expressions</span></span>

<span data-ttu-id="2886a-2489">Sorgu ifadeleri, belirli bir bağlamda özel anlamı olan tanımlayıcılar içeren bir dizi "bağlamsal anahtar sözcük", yani.</span><span class="sxs-lookup"><span data-stu-id="2886a-2489">Query expressions contain a number of "contextual keywords", i.e., identifiers that have special meaning in a given context.</span></span> <span data-ttu-id="2886a-2490">Bunlar özellikle `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` ve `by`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2490">Specifically these are `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` and `by`.</span></span> <span data-ttu-id="2886a-2491">Bu tanımlayıcıların, anahtar sözcük veya basit adlar olarak karışık olarak kullanılması nedeniyle sorgu ifadelerinde belirsizlikleri kaçınmak için, bu tanımlayıcılar bir sorgu ifadesinin içinde herhangi bir yerde olduğunda anahtar sözcük olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2491">In order to avoid ambiguities in query expressions caused by mixed use of these identifiers as keywords or simple names, these identifiers are considered keywords when occurring anywhere within a query expression.</span></span>

<span data-ttu-id="2886a-2492">Bu amaçla, bir sorgu ifadesi "`from identifier`" ile başlayan ve "`;`", "`=`" veya "`,`" dışında herhangi bir belirteç tarafından başlayan ifadedir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2492">For this purpose, a query expression is any expression that starts with "`from identifier`" followed by any token except "`;`", "`=`" or "`,`".</span></span>

<span data-ttu-id="2886a-2493">Bu sözcüklerin bir sorgu ifadesinde tanımlayıcı olarak kullanılabilmesi için, "`@`" ([tanımlayıcılar](lexical-structure.md#identifiers)) ön eki uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2493">In order to use these words as identifiers within a query expression, they can be prefixed with "`@`" ([Identifiers](lexical-structure.md#identifiers)).</span></span>

### <a name="query-expression-translation"></a><span data-ttu-id="2886a-2494">Sorgu ifadesi çevirisi</span><span class="sxs-lookup"><span data-stu-id="2886a-2494">Query expression translation</span></span>

<span data-ttu-id="2886a-2495">Dil C# , sorgu ifadelerinin yürütme semantiğini belirtmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-2495">The C# language does not specify the execution semantics of query expressions.</span></span> <span data-ttu-id="2886a-2496">Bunun yerine sorgu ifadeleri *sorgu ifade düzenine* ([sorgu ifade deseninin](expressions.md#the-query-expression-pattern)) bağlı olan yöntemlerin etkinleştirilmesinde çevrilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2496">Rather, query expressions are translated into invocations of methods that adhere to the *query expression pattern* ([The query expression pattern](expressions.md#the-query-expression-pattern)).</span></span> <span data-ttu-id="2886a-2497">Özellikle, sorgu ifadeleri `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`ve `Cast`adlı yöntemlerin etkinleştirilmesine çevrilir. [Sorgu ifadesi](expressions.md#the-query-expression-pattern)düzeninde açıklandığı gibi, bu yöntemlerin belirli imzaları ve sonuç türlerini olması beklenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2497">Specifically, query expressions are translated into invocations of methods named `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, and `Cast`.These methods are expected to have particular signatures and result types, as described in [The query expression pattern](expressions.md#the-query-expression-pattern).</span></span> <span data-ttu-id="2886a-2498">Bu yöntemler, sorgulanan nesnenin örnek yöntemleri veya nesne dışındaki genişletme yöntemleri olabilir ve sorgunun gerçek yürütmesini uygular.</span><span class="sxs-lookup"><span data-stu-id="2886a-2498">These methods can be instance methods of the object being queried or extension methods that are external to the object, and they implement the actual execution of the query.</span></span>

<span data-ttu-id="2886a-2499">Sorgu ifadelerinden Yöntem etkinleştirmeleri 'e yapılan çeviri, herhangi bir tür bağlama veya aşırı yükleme çözümlemesi yapılmadan önce oluşan bir sözdizimsel eşleme olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2499">The translation from query expressions to method invocations is a syntactic mapping that occurs before any type binding or overload resolution has been performed.</span></span> <span data-ttu-id="2886a-2500">Çeviri, sözdizimsel olarak doğru bir şekilde garanti edilir, ancak anlamsal olarak doğru C# kodun üretilmesi garanti edilmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-2500">The translation is guaranteed to be syntactically correct, but it is not guaranteed to produce semantically correct C# code.</span></span> <span data-ttu-id="2886a-2501">Sorgu ifadelerinin çevirisinde, sonuçta elde edilen yöntem etkinleştirmeleri normal Yöntem etkinleştirmeleri olarak işlenir ve bu, örneğin Yöntemler yoksa, bağımsız değişkenlerin yanlış türleri varsa veya yöntemler geneldir ve Tür çıkarımı başarısız oluyor.</span><span class="sxs-lookup"><span data-stu-id="2886a-2501">Following translation of query expressions, the resulting method invocations are processed as regular method invocations, and this may in turn uncover errors, for example if the methods do not exist, if arguments have wrong types, or if the methods are generic and type inference fails.</span></span>

<span data-ttu-id="2886a-2502">Bir sorgu ifadesi, daha fazla azaltmayı mümkün olana kadar, aşağıdaki Çeviriler tekrar tekrar uygulanarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2502">A query expression is processed by repeatedly applying the following translations until no further reductions are possible.</span></span> <span data-ttu-id="2886a-2503">Çeviriler uygulama sırasıyla listelenir: her bölümde, yukarıdaki bölümlerdeki çevirilerin tüketilildiği ve tükendiğinde, bir bölümün aynı sorgu ifadesinin işlenmesinde daha sonra yeniden ziyaret edilmesinin varsayıldığı varsayılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2503">The translations are listed in order of application: each section assumes that the translations in the preceding sections have been performed exhaustively, and once exhausted, a section will not later be revisited in the processing of the same query expression.</span></span>

<span data-ttu-id="2886a-2504">Sorgu ifadelerinde Aralık değişkenlerine atamaya izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="2886a-2504">Assignment to range variables is not allowed in query expressions.</span></span> <span data-ttu-id="2886a-2505">Ancak bir C# uygulamaya bu kısıtlamayı her zaman zorlamamasına izin verilir, çünkü bu durum bazen burada sunulan sözdizimsel çeviri düzeninde mümkün olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2505">However a C# implementation is permitted to not always enforce this restriction, since this may sometimes not be possible with the syntactic translation scheme presented here.</span></span>

<span data-ttu-id="2886a-2506">Belirli Çeviriler, `*`tarafından belirtilen saydam tanımlayıcılarla Aralık değişkenleri ekler.</span><span class="sxs-lookup"><span data-stu-id="2886a-2506">Certain translations inject range variables with transparent identifiers denoted by `*`.</span></span> <span data-ttu-id="2886a-2507">Saydam tanımlayıcıların özel özellikleri, [saydam tanımlayıcılarla](expressions.md#transparent-identifiers)daha ayrıntılı bir şekilde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2507">The special properties of transparent identifiers are discussed further in [Transparent identifiers](expressions.md#transparent-identifiers).</span></span>

#### <a name="select-and-groupby-clauses-with-continuations"></a><span data-ttu-id="2886a-2508">Devamlılıklarla seçim ve GroupBy yan tümceleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2508">Select and groupby clauses with continuations</span></span>

<span data-ttu-id="2886a-2509">Devam eden bir sorgu ifadesi</span><span class="sxs-lookup"><span data-stu-id="2886a-2509">A query expression with a continuation</span></span>
```csharp
from ... into x ...
```
<span data-ttu-id="2886a-2510">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2510">is translated into</span></span>
```csharp
from x in ( from ... ) ...
```

<span data-ttu-id="2886a-2511">Aşağıdaki bölümlerde yer alan Çeviriler, sorguların `into` devamlılıklar olmadığını varsayar.</span><span class="sxs-lookup"><span data-stu-id="2886a-2511">The translations in the following sections assume that queries have no `into` continuations.</span></span>

<span data-ttu-id="2886a-2512">Örnek</span><span class="sxs-lookup"><span data-stu-id="2886a-2512">The example</span></span>
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="2886a-2513">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2513">is translated into</span></span>
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="2886a-2514">son çeviri</span><span class="sxs-lookup"><span data-stu-id="2886a-2514">the final translation of which is</span></span>
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a><span data-ttu-id="2886a-2515">Açık Aralık değişken türleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2515">Explicit range variable types</span></span>

<span data-ttu-id="2886a-2516">Açıkça bir Aralık değişkeni türünü belirten `from` yan tümcesi</span><span class="sxs-lookup"><span data-stu-id="2886a-2516">A `from` clause that explicitly specifies a range variable type</span></span>
```csharp
from T x in e
```
<span data-ttu-id="2886a-2517">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2517">is translated into</span></span>
```csharp
from x in ( e ) . Cast < T > ( )
```

<span data-ttu-id="2886a-2518">Açıkça bir Aralık değişkeni türünü belirten `join` yan tümcesi</span><span class="sxs-lookup"><span data-stu-id="2886a-2518">A `join` clause that explicitly specifies a range variable type</span></span>
```csharp
join T x in e on k1 equals k2
```
<span data-ttu-id="2886a-2519">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2519">is translated into</span></span>
```csharp
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

<span data-ttu-id="2886a-2520">Aşağıdaki bölümlerde yer alan Çeviriler, sorguların hiçbir açık Aralık değişken türüne sahip olmadığını varsayar.</span><span class="sxs-lookup"><span data-stu-id="2886a-2520">The translations in the following sections assume that queries have no explicit range variable types.</span></span>

<span data-ttu-id="2886a-2521">Örnek</span><span class="sxs-lookup"><span data-stu-id="2886a-2521">The example</span></span>
```csharp
from Customer c in customers
where c.City == "London"
select c
```
<span data-ttu-id="2886a-2522">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2522">is translated into</span></span>
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
<span data-ttu-id="2886a-2523">son çeviri</span><span class="sxs-lookup"><span data-stu-id="2886a-2523">the final translation of which is</span></span>
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

<span data-ttu-id="2886a-2524">Açık Aralık değişkeni türleri genel olmayan `IEnumerable` arabirimini uygulayan, ancak genel `IEnumerable<T>` arabirimini uygulayan koleksiyonları sorgulamak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2524">Explicit range variable types are useful for querying collections that implement the non-generic `IEnumerable` interface, but not the generic `IEnumerable<T>` interface.</span></span> <span data-ttu-id="2886a-2525">Yukarıdaki örnekte, `customers` `ArrayList`türünde olması durumunda bu durum olacaktır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2525">In the example above, this would be the case if `customers` were of type `ArrayList`.</span></span>

#### <a name="degenerate-query-expressions"></a><span data-ttu-id="2886a-2526">Sorgu ifadeleri oluşturmayı kaldırma</span><span class="sxs-lookup"><span data-stu-id="2886a-2526">Degenerate query expressions</span></span>

<span data-ttu-id="2886a-2527">Formun sorgu ifadesi</span><span class="sxs-lookup"><span data-stu-id="2886a-2527">A query expression of the form</span></span>
```csharp
from x in e select x
```
<span data-ttu-id="2886a-2528">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2528">is translated into</span></span>
```csharp
( e ) . Select ( x => x )
```

<span data-ttu-id="2886a-2529">Örnek</span><span class="sxs-lookup"><span data-stu-id="2886a-2529">The example</span></span>
```csharp
from c in customers
select c
```
<span data-ttu-id="2886a-2530">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2530">is translated into</span></span>
```csharp
customers.Select(c => c)
```

<span data-ttu-id="2886a-2531">Bir bozuk sorgu ifadesi, kaynağın öğelerini önemli bir şekilde seçen bir ifadedir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2531">A degenerate query expression is one that trivially selects the elements of the source.</span></span> <span data-ttu-id="2886a-2532">Çevirinin daha sonraki bir aşaması, diğer çeviri adımları tarafından tanıtılan ve kaynakları kendi kaynağıyla değiştirerek çıkartılmış olan sorgu kaldırma işlemleri kaldırır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2532">A later phase of the translation removes degenerate queries introduced by other translation steps by replacing them with their source.</span></span> <span data-ttu-id="2886a-2533">Sorgu ifadesinin sonucunun, sorgunun istemcisinin türünü ve kimliğini açığa çıkardığından, hiçbir şekilde kaynak nesnenin kendisi olduğundan emin olmak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2533">It is important however to ensure that the result of a query expression is never the source object itself, as that would reveal the type and identity of the source to the client of the query.</span></span> <span data-ttu-id="2886a-2534">Bu nedenle bu adım, kaynak kodunda doğrudan yazılan sorguları, kaynak üzerinde açıkça `Select` çağırarak kaldırır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2534">Therefore this step protects degenerate queries written directly in source code by explicitly calling `Select` on the source.</span></span> <span data-ttu-id="2886a-2535">Daha sonra bu yöntemlerin kaynak nesnenin kendisini döndürmemesini sağlamak için `Select` ve diğer sorgu işleçleri uygulayıcılarına kadar.</span><span class="sxs-lookup"><span data-stu-id="2886a-2535">It is then up to the implementers of `Select` and other query operators to ensure that these methods never return the source object itself.</span></span>

#### <a name="from-let-where-join-and-orderby-clauses"></a><span data-ttu-id="2886a-2536">Kimden, Let, WHERE, JOIN ve OrderBy yan tümceleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2536">From, let, where, join and orderby clauses</span></span>

<span data-ttu-id="2886a-2537">İkinci bir `from` yan tümcesi ve sonra `select` yan tümcesi içeren bir sorgu ifadesi</span><span class="sxs-lookup"><span data-stu-id="2886a-2537">A query expression with a second `from` clause followed by a `select` clause</span></span>
```csharp
from x1 in e1
from x2 in e2
select v
```
<span data-ttu-id="2886a-2538">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2538">is translated into</span></span>
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="2886a-2539">İkinci bir `from` yan tümcesine sahip bir sorgu ifadesi ve ardından `select` yan tümcesi dışında bir öğe.</span><span class="sxs-lookup"><span data-stu-id="2886a-2539">A query expression with a second `from` clause followed by something other than a `select` clause:</span></span>

```csharp
from x1 in e1
from x2 in e2
...
```
<span data-ttu-id="2886a-2540">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2540">is translated into</span></span>
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

<span data-ttu-id="2886a-2541">`let` yan tümcesiyle bir sorgu ifadesi</span><span class="sxs-lookup"><span data-stu-id="2886a-2541">A query expression with a `let` clause</span></span>
```csharp
from x in e
let y = f
...
```
<span data-ttu-id="2886a-2542">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2542">is translated into</span></span>
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

<span data-ttu-id="2886a-2543">`where` yan tümcesiyle bir sorgu ifadesi</span><span class="sxs-lookup"><span data-stu-id="2886a-2543">A query expression with a `where` clause</span></span>
```csharp
from x in e
where f
...
```
<span data-ttu-id="2886a-2544">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2544">is translated into</span></span>
```csharp
from x in ( e ) . Where ( x => f )
...
```

<span data-ttu-id="2886a-2545">Bir `into` ve ardından bir `select` yan tümcesi olmadan `join` yan tümcesiyle bir sorgu ifadesi</span><span class="sxs-lookup"><span data-stu-id="2886a-2545">A query expression with a `join` clause without an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
<span data-ttu-id="2886a-2546">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2546">is translated into</span></span>
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="2886a-2547">Bir `into` ve ardından bir `select` yan tümcesi dışında bir `join` yan tümcesi içeren bir sorgu ifadesi</span><span class="sxs-lookup"><span data-stu-id="2886a-2547">A query expression with a `join` clause without an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
<span data-ttu-id="2886a-2548">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2548">is translated into</span></span>
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

<span data-ttu-id="2886a-2549">Bir `into` ve ardından bir `select` yan tümcesi içeren `join` yan tümcesiyle bir sorgu ifadesi</span><span class="sxs-lookup"><span data-stu-id="2886a-2549">A query expression with a `join` clause with an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
<span data-ttu-id="2886a-2550">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2550">is translated into</span></span>
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

<span data-ttu-id="2886a-2551">Bir `into` ve ardından bir `select` yan tümcesi dışında bir `join` yan tümcesi içeren bir sorgu ifadesi</span><span class="sxs-lookup"><span data-stu-id="2886a-2551">A query expression with a `join` clause with an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
<span data-ttu-id="2886a-2552">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2552">is translated into</span></span>
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

<span data-ttu-id="2886a-2553">`orderby` yan tümcesiyle bir sorgu ifadesi</span><span class="sxs-lookup"><span data-stu-id="2886a-2553">A query expression with an `orderby` clause</span></span>
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
<span data-ttu-id="2886a-2554">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2554">is translated into</span></span>
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

<span data-ttu-id="2886a-2555">Bir sıralama yan tümcesi `descending` yön göstergesi belirtiyorsa, bunun yerine `OrderByDescending` veya `ThenByDescending` bir çağırma oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2555">If an ordering clause specifies a `descending` direction indicator, an invocation of `OrderByDescending` or `ThenByDescending` is produced instead.</span></span>

<span data-ttu-id="2886a-2556">Aşağıdaki Çeviriler `let`, `where`, `join` veya `orderby` yan tümcelerinin olmadığını ve her sorgu ifadesinde bir ilk `from` yan tümcesinin daha fazlasını belirtdüğünü varsayar.</span><span class="sxs-lookup"><span data-stu-id="2886a-2556">The following translations assume that there are no `let`, `where`, `join` or `orderby` clauses, and no more than the one initial `from` clause in each query expression.</span></span>

<span data-ttu-id="2886a-2557">Örnek</span><span class="sxs-lookup"><span data-stu-id="2886a-2557">The example</span></span>
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="2886a-2558">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2558">is translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

<span data-ttu-id="2886a-2559">Örnek</span><span class="sxs-lookup"><span data-stu-id="2886a-2559">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="2886a-2560">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2560">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="2886a-2561">son çeviri</span><span class="sxs-lookup"><span data-stu-id="2886a-2561">the final translation of which is</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
<span data-ttu-id="2886a-2562">Burada `x`, aksi durumda görünmeyen ve erişilemeyen bir derleyicinin ürettiği tanıtıcıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2562">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="2886a-2563">Örnek</span><span class="sxs-lookup"><span data-stu-id="2886a-2563">The example</span></span>
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
<span data-ttu-id="2886a-2564">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2564">is translated into</span></span>
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
<span data-ttu-id="2886a-2565">son çeviri</span><span class="sxs-lookup"><span data-stu-id="2886a-2565">the final translation of which is</span></span>
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
<span data-ttu-id="2886a-2566">Burada `x`, aksi durumda görünmeyen ve erişilemeyen bir derleyicinin ürettiği tanıtıcıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2566">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="2886a-2567">Örnek</span><span class="sxs-lookup"><span data-stu-id="2886a-2567">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
<span data-ttu-id="2886a-2568">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2568">is translated into</span></span>
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

<span data-ttu-id="2886a-2569">Örnek</span><span class="sxs-lookup"><span data-stu-id="2886a-2569">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="2886a-2570">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2570">is translated into</span></span>
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="2886a-2571">son çeviri</span><span class="sxs-lookup"><span data-stu-id="2886a-2571">the final translation of which is</span></span>
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
<span data-ttu-id="2886a-2572">`x` ve `y` derleyici tarafından oluşturulan ve aksi durumda görünmeyen ve erişilemeyen tanımlayıcılardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2572">where `x` and `y` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="2886a-2573">Örnek</span><span class="sxs-lookup"><span data-stu-id="2886a-2573">The example</span></span>
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
<span data-ttu-id="2886a-2574">Son çeviriyi içerir</span><span class="sxs-lookup"><span data-stu-id="2886a-2574">has the final translation</span></span>
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a><span data-ttu-id="2886a-2575">Yan tümceleri Seç</span><span class="sxs-lookup"><span data-stu-id="2886a-2575">Select clauses</span></span>

<span data-ttu-id="2886a-2576">Formun sorgu ifadesi</span><span class="sxs-lookup"><span data-stu-id="2886a-2576">A query expression of the form</span></span>
```csharp
from x in e select v
```
<span data-ttu-id="2886a-2577">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2577">is translated into</span></span>
```csharp
( e ) . Select ( x => v )
```
<span data-ttu-id="2886a-2578">v 'nin x tanımlayıcısı olduğu durumlar dışında, çeviri yalnızca</span><span class="sxs-lookup"><span data-stu-id="2886a-2578">except when v is the identifier x, the translation is simply</span></span>
```csharp
( e )
```

<span data-ttu-id="2886a-2579">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2886a-2579">For example</span></span>
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
<span data-ttu-id="2886a-2580">yalnızca şuna çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2580">is simply translated into</span></span>
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a><span data-ttu-id="2886a-2581">GroupBy yan tümceleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2581">Groupby clauses</span></span>

<span data-ttu-id="2886a-2582">Formun sorgu ifadesi</span><span class="sxs-lookup"><span data-stu-id="2886a-2582">A query expression of the form</span></span>
```csharp
from x in e group v by k
```
<span data-ttu-id="2886a-2583">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2583">is translated into</span></span>
```csharp
( e ) . GroupBy ( x => k , x => v )
```
<span data-ttu-id="2886a-2584">v 'nin x tanımlayıcısı olduğu durumlar dışında, çeviri</span><span class="sxs-lookup"><span data-stu-id="2886a-2584">except when v is the identifier x, the translation is</span></span>
```csharp
( e ) . GroupBy ( x => k )
```

<span data-ttu-id="2886a-2585">Örnek</span><span class="sxs-lookup"><span data-stu-id="2886a-2585">The example</span></span>
```csharp
from c in customers
group c.Name by c.Country
```
<span data-ttu-id="2886a-2586">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2586">is translated into</span></span>
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a><span data-ttu-id="2886a-2587">Saydam tanımlayıcılar</span><span class="sxs-lookup"><span data-stu-id="2886a-2587">Transparent identifiers</span></span>

<span data-ttu-id="2886a-2588">Belirli Çeviriler, `*`tarafından belirtilen ***saydam tanımlayıcılarla*** Aralık değişkenleri ekler.</span><span class="sxs-lookup"><span data-stu-id="2886a-2588">Certain translations inject range variables with ***transparent identifiers*** denoted by `*`.</span></span> <span data-ttu-id="2886a-2589">Saydam tanımlayıcılar uygun bir dil özelliği değildir; Bunlar yalnızca sorgu ifadesi çevirisi işleminde bir ara adım olarak mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2589">Transparent identifiers are not a proper language feature; they exist only as an intermediate step in the query expression translation process.</span></span>

<span data-ttu-id="2886a-2590">Bir sorgu çevirisi bir saydam tanımlayıcıyı geçersiz kılar, ek çeviri adımları saydam tanımlayıcıyı anonim işlevlere ve anonim nesne başlatıcılarına yayar.</span><span class="sxs-lookup"><span data-stu-id="2886a-2590">When a query translation injects a transparent identifier, further translation steps propagate the transparent identifier into anonymous functions and anonymous object initializers.</span></span> <span data-ttu-id="2886a-2591">Bu bağlamlarda saydam tanımlayıcılar aşağıdaki davranışa sahiptir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2591">In those contexts, transparent identifiers have the following behavior:</span></span>

*  <span data-ttu-id="2886a-2592">Bir saydam tanımlayıcı, anonim bir işlevde parametre olarak gerçekleştiğinde, ilişkili anonim türün üyeleri anonim işlevin gövdesinde otomatik olarak kapsam içinde olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2592">When a transparent identifier occurs as a parameter in an anonymous function, the members of the associated anonymous type are automatically in scope in the body of the anonymous function.</span></span>
*  <span data-ttu-id="2886a-2593">Saydam tanımlayıcısı olan bir üye kapsam içinde olduğunda, o üyenin üyeleri de kapsam içinde olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2593">When a member with a transparent identifier is in scope, the members of that member are in scope as well.</span></span>
*  <span data-ttu-id="2886a-2594">Bir saydam tanımlayıcı, anonim nesne başlatıcısında bir üye bildirimci olarak oluştuğunda, saydam tanımlayıcı içeren bir üye tanıtır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2594">When a transparent identifier occurs as a member declarator in an anonymous object initializer, it introduces a member with a transparent identifier.</span></span>
*  <span data-ttu-id="2886a-2595">Yukarıda açıklanan çeviri adımlarında, saydam tanımlayıcılar her zaman anonim türlerle birlikte tanıtılmıştır ve birden çok Aralık değişkenini tek bir nesnenin üyeleri olarak yakalama amacını taşıyan bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="2886a-2595">In the translation steps described above, transparent identifiers are always introduced together with anonymous types, with the intent of capturing multiple range variables as members of a single object.</span></span> <span data-ttu-id="2886a-2596">C# Uygulamasının birden çok Aralık değişkenini gruplamak için anonim türlerden farklı bir mekanizma kullanmasına izin verilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2596">An implementation of C# is permitted to use a different mechanism than anonymous types to group together multiple range variables.</span></span> <span data-ttu-id="2886a-2597">Aşağıdaki çeviri örnekleri, anonim türlerin kullanıldığını varsayar ve saydam tanımlayıcıların nasıl çevrilebilmesi gerektiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2597">The following translation examples assume that anonymous types are used, and show how transparent identifiers can be translated away.</span></span>

<span data-ttu-id="2886a-2598">Örnek</span><span class="sxs-lookup"><span data-stu-id="2886a-2598">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
<span data-ttu-id="2886a-2599">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2599">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

<span data-ttu-id="2886a-2600">daha fazla çevrilmiş</span><span class="sxs-lookup"><span data-stu-id="2886a-2600">which is further translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
<span data-ttu-id="2886a-2601">saydam tanımlayıcılar silindiklerinde, eşittir</span><span class="sxs-lookup"><span data-stu-id="2886a-2601">which, when transparent identifiers are erased, is equivalent to</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
<span data-ttu-id="2886a-2602">Burada `x`, aksi durumda görünmeyen ve erişilemeyen bir derleyicinin ürettiği tanıtıcıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2602">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="2886a-2603">Örnek</span><span class="sxs-lookup"><span data-stu-id="2886a-2603">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="2886a-2604">çevrilir</span><span class="sxs-lookup"><span data-stu-id="2886a-2604">is translated into</span></span>
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="2886a-2605">daha az</span><span class="sxs-lookup"><span data-stu-id="2886a-2605">which is further reduced to</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
<span data-ttu-id="2886a-2606">son çeviri</span><span class="sxs-lookup"><span data-stu-id="2886a-2606">the final translation of which is</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c, o }).
Join(details, x => x.o.OrderID, d => d.OrderID,
    (x, d) => new { x, d }).
Join(products, y => y.d.ProductID, p => p.ProductID,
    (y, p) => new { y, p }).
Select(z => new { z.y.x.c.Name, z.y.x.o.OrderDate, z.p.ProductName })
```
<span data-ttu-id="2886a-2607">`x`, `y`ve `z` derleyici tarafından oluşturulan ve aksi durumda görünmeyen ve erişilemeyen tanımlayıcılardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2607">where `x`, `y`, and `z` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

### <a name="the-query-expression-pattern"></a><span data-ttu-id="2886a-2608">Sorgu ifadesi deseninin</span><span class="sxs-lookup"><span data-stu-id="2886a-2608">The query expression pattern</span></span>

<span data-ttu-id="2886a-2609">***Sorgu ifadesi stili*** , sorgu ifadelerini desteklemek için uygulayabileceğiniz yöntemlerin bir modelini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2609">The ***Query expression pattern*** establishes a pattern of methods that types can implement to support query expressions.</span></span> <span data-ttu-id="2886a-2610">Sorgu ifadeleri, sözdizimsel eşleme yoluyla Yöntem etkinleştirmeleri 'e çevrildiği için, türlerin sorgu ifadesi deseninin nasıl uygulandığı konusunda önemli ölçüde esneklik vardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2610">Because query expressions are translated to method invocations by means of a syntactic mapping, types have considerable flexibility in how they implement the query expression pattern.</span></span> <span data-ttu-id="2886a-2611">Örneğin, iki aynı çağırma söz dizimini içerdiğinden ve Yöntemler, anonim işlevler her ikisine de dönüştürülebildiğinden temsilci veya ifade ağaçları isteyebildiğinden, düzenin yöntemleri örnek yöntemleri veya genişletme yöntemleri olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2611">For example, the methods of the pattern can be implemented as instance methods or as extension methods because the two have the same invocation syntax, and the methods can request delegates or expression trees because anonymous functions are convertible to both.</span></span>

<span data-ttu-id="2886a-2612">Sorgu ifadesi modelini destekleyen `C<T>` genel bir tür için önerilen şekil aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2612">The recommended shape of a generic type `C<T>` that supports the query expression pattern is shown below.</span></span> <span data-ttu-id="2886a-2613">Parametre ve sonuç türleri arasındaki uygun ilişkileri göstermek için genel bir tür kullanılır, ancak genel olmayan türler için de düzeni uygulamak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2613">A generic type is used in order to illustrate the proper relationships between parameter and result types, but it is possible to implement the pattern for non-generic types as well.</span></span>

```csharp
delegate R Func<T1,R>(T1 arg1);

delegate R Func<T1,T2,R>(T1 arg1, T2 arg2);

class C
{
    public C<T> Cast<T>();
}

class C<T> : C
{
    public C<T> Where(Func<T,bool> predicate);

    public C<U> Select<U>(Func<T,U> selector);

    public C<V> SelectMany<U,V>(Func<T,C<U>> selector,
        Func<T,U,V> resultSelector);

    public C<V> Join<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,U,V> resultSelector);

    public C<V> GroupJoin<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,C<U>,V> resultSelector);

    public O<T> OrderBy<K>(Func<T,K> keySelector);

    public O<T> OrderByDescending<K>(Func<T,K> keySelector);

    public C<G<K,T>> GroupBy<K>(Func<T,K> keySelector);

    public C<G<K,E>> GroupBy<K,E>(Func<T,K> keySelector,
        Func<T,E> elementSelector);
}

class O<T> : C<T>
{
    public O<T> ThenBy<K>(Func<T,K> keySelector);

    public O<T> ThenByDescending<K>(Func<T,K> keySelector);
}

class G<K,T> : C<T>
{
    public K Key { get; }
}
```

<span data-ttu-id="2886a-2614">Yukarıdaki yöntemler `Func<T1,R>` ve `Func<T1,T2,R>`genel temsilci türlerini kullanır, ancak parametre ve sonuç türlerinde aynı ilişkilerle diğer temsilci veya ifade ağacı türleri de aynı şekilde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2614">The methods above use the generic delegate types `Func<T1,R>` and `Func<T1,T2,R>`, but they could equally well have used other delegate or expression tree types with the same relationships in parameter and result types.</span></span>

<span data-ttu-id="2886a-2615">`ThenBy` ve `ThenByDescending` yöntemlerinin yalnızca bir `OrderBy` veya `OrderByDescending`sonucu üzerinde kullanılabilir olmasını sağlayan `C<T>` ve `O<T>` arasında önerilen ilişkiye dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="2886a-2615">Notice the recommended relationship between `C<T>` and `O<T>` which ensures that the `ThenBy` and `ThenByDescending` methods are available only on the result of an `OrderBy` or `OrderByDescending`.</span></span> <span data-ttu-id="2886a-2616">Ayrıca, her bir iç sıranın ek bir `Key` özelliğine sahip olduğu `GroupBy`, bir dizi sıra için önerilen şekle de dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="2886a-2616">Also notice the recommended shape of the result of `GroupBy` -- a sequence of sequences, where each inner sequence has an additional `Key` property.</span></span>

<span data-ttu-id="2886a-2617">`System.Linq` ad alanı, `System.Collections.Generic.IEnumerable<T>` arabirimini uygulayan herhangi bir tür için sorgu işleci deseninin bir uygulamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="2886a-2617">The `System.Linq` namespace provides an implementation of the query operator pattern for any type that implements the `System.Collections.Generic.IEnumerable<T>` interface.</span></span>

## <a name="assignment-operators"></a><span data-ttu-id="2886a-2618">Atama işleçleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2618">Assignment operators</span></span>

<span data-ttu-id="2886a-2619">Atama işleçleri bir değişkene, özelliğe, olaya veya Dizin Oluşturucu öğesine yeni bir değer atar.</span><span class="sxs-lookup"><span data-stu-id="2886a-2619">The assignment operators assign a new value to a variable, a property, an event, or an indexer element.</span></span>

```antlr
assignment
    : unary_expression assignment_operator expression
    ;

assignment_operator
    : '='
    | '+='
    | '-='
    | '*='
    | '/='
    | '%='
    | '&='
    | '|='
    | '^='
    | '<<='
    | right_shift_assignment
    ;
```

<span data-ttu-id="2886a-2620">Atamanın sol işleneni, değişken olarak sınıflandırılmış bir ifade, özellik erişimi, Dizin Oluşturucu erişimi veya olay erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2620">The left operand of an assignment must be an expression classified as a variable, a property access, an indexer access, or an event access.</span></span>

<span data-ttu-id="2886a-2621">`=` işlecine ***basit atama işleci***denir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2621">The `=` operator is called the ***simple assignment operator***.</span></span> <span data-ttu-id="2886a-2622">Sağ işlenenin değerini, sol işlenen tarafından verilen değişken, özellik veya Dizin Oluşturucu öğesine atar.</span><span class="sxs-lookup"><span data-stu-id="2886a-2622">It assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="2886a-2623">Basit atama işlecinin sol işleneni bir olay erişimi olmayabilir ( [alan benzeri olaylar](classes.md#field-like-events)bölümünde açıklananlar dışında).</span><span class="sxs-lookup"><span data-stu-id="2886a-2623">The left operand of the simple assignment operator may not be an event access (except as described in [Field-like events](classes.md#field-like-events)).</span></span> <span data-ttu-id="2886a-2624">Basit atama işleci [basit atamada](expressions.md#simple-assignment)açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2624">The simple assignment operator is described in [Simple assignment](expressions.md#simple-assignment).</span></span>

<span data-ttu-id="2886a-2625">`=` işlecinden farklı atama işleçleri ***bileşik atama işleçleri***olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2625">The assignment operators other than the `=` operator are called the ***compound assignment operators***.</span></span> <span data-ttu-id="2886a-2626">Bu işleçler iki işlenen üzerinde belirtilen işlemi gerçekleştirir ve ardından sonuçtaki değeri, sol işlenen tarafından verilen değişken, özellik veya Dizin Oluşturucu öğesine atar.</span><span class="sxs-lookup"><span data-stu-id="2886a-2626">These operators perform the indicated operation on the two operands, and then assign the resulting value to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="2886a-2627">Bileşik atama işleçleri [Birleşik atama](expressions.md#compound-assignment)bölümünde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2627">The compound assignment operators are described in [Compound assignment](expressions.md#compound-assignment).</span></span>

<span data-ttu-id="2886a-2628">Sol işlenen olarak bir olay erişim ifadesiyle `+=` ve `-=` işleçlerine *olay atama işleçleri*denir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2628">The `+=` and `-=` operators with an event access expression as the left operand are called the *event assignment operators*.</span></span> <span data-ttu-id="2886a-2629">Sol işlenen olarak bir olay erişimiyle hiçbir başka atama işleci geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2629">No other assignment operator is valid with an event access as the left operand.</span></span> <span data-ttu-id="2886a-2630">Olay atama işleçleri [olay atamasında](expressions.md#event-assignment)açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2630">The event assignment operators are described in [Event assignment](expressions.md#event-assignment).</span></span>

<span data-ttu-id="2886a-2631">Atama işleçleri doğru ilişkilendirilebilir, yani işlemler sağdan sola gruplandırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2631">The assignment operators are right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="2886a-2632">Örneğin, `a = b = c` form ifadesi `a = (b = c)`olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2632">For example, an expression of the form `a = b = c` is evaluated as `a = (b = c)`.</span></span>

### <a name="simple-assignment"></a><span data-ttu-id="2886a-2633">Basit atama</span><span class="sxs-lookup"><span data-stu-id="2886a-2633">Simple assignment</span></span>

<span data-ttu-id="2886a-2634">`=` işlecine basit atama işleci denir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2634">The `=` operator is called the simple assignment operator.</span></span>

<span data-ttu-id="2886a-2635">Basit bir atamanın sol işleneni form `E.P` veya `E` derleme zamanı türü `dynamic`sahip olduğu `E[Ei]`, atama dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="2886a-2635">If the left operand of a simple assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="2886a-2636">Bu durumda, atama ifadesinin derleme zamanı türü `dynamic`ve aşağıda açıklanan çözüm, `E`çalışma zamanı türüne göre çalışma zamanında gerçekleşmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2636">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="2886a-2637">Basit bir atamada, sağ işlenen, sol işlenenin türüne örtük olarak dönüştürülebilir bir ifade olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2637">In a simple assignment, the right operand must be an expression that is implicitly convertible to the type of the left operand.</span></span> <span data-ttu-id="2886a-2638">İşlem, sol işlenen tarafından verilen değişken, özellik veya Dizin Oluşturucu öğesine sağ işlenenin değerini atar.</span><span class="sxs-lookup"><span data-stu-id="2886a-2638">The operation assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span>

<span data-ttu-id="2886a-2639">Basit atama ifadesinin sonucu, sol işlenene atanan değerdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2639">The result of a simple assignment expression is the value assigned to the left operand.</span></span> <span data-ttu-id="2886a-2640">Sonuç, sol işleneniyle aynı türe sahiptir ve her zaman bir değer olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2640">The result has the same type as the left operand and is always classified as a value.</span></span>

<span data-ttu-id="2886a-2641">Sol işlenen bir özellik veya Dizin Oluşturucu erişimi ise, özelliğin veya dizin oluşturucunun bir `set` erişimcisi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2641">If the left operand is a property or indexer access, the property or indexer must have a `set` accessor.</span></span> <span data-ttu-id="2886a-2642">Bu durumda, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2642">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="2886a-2643">Form `x = y` basit atamanın çalışma zamanı işleme aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="2886a-2643">The run-time processing of a simple assignment of the form `x = y` consists of the following steps:</span></span>

*  <span data-ttu-id="2886a-2644">`x` bir değişken olarak sınıflandırıldıysanız:</span><span class="sxs-lookup"><span data-stu-id="2886a-2644">If `x` is classified as a variable:</span></span>
   * <span data-ttu-id="2886a-2645">`x`, değişkeni üretmek için değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2645">`x` is evaluated to produce the variable.</span></span>
   * <span data-ttu-id="2886a-2646">`y` değerlendirilir ve gerekirse örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) aracılığıyla `x` türüne dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2646">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="2886a-2647">`x` tarafından verilen değişken bir *reference_type*dizi öğesidir `y` için hesaplanan değerin, `x` bir öğe olan dizi örneğiyle uyumlu olduğundan emin olmak için bir çalışma zamanı denetimi yapılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2647">If the variable given by `x` is an array element of a *reference_type*, a run-time check is performed to ensure that the value computed for `y` is compatible with the array instance of which `x` is an element.</span></span> <span data-ttu-id="2886a-2648">`y` `null`veya `y` tarafından başvurulan örneğin gerçek türünden, `x`içeren dizi örneğinin gerçek öğe türüne dolaylı bir başvuru dönüştürmesi ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions)) varsa denetim başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2648">The check succeeds if `y` is `null`, or if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the actual type of the instance referenced by `y` to the actual element type of the array instance containing `x`.</span></span> <span data-ttu-id="2886a-2649">Aksi takdirde, bir `System.ArrayTypeMismatchException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2649">Otherwise, a `System.ArrayTypeMismatchException` is thrown.</span></span>
   * <span data-ttu-id="2886a-2650">`y` değerlendirmesinden ve dönüşümden elde edilen değer, `x`değerlendirmesi tarafından verilen konuma depolanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2650">The value resulting from the evaluation and conversion of `y` is stored into the location given by the evaluation of `x`.</span></span>
*  <span data-ttu-id="2886a-2651">`x`, özellik veya Dizin Oluşturucu erişimi olarak sınıflandırıldıysanız:</span><span class="sxs-lookup"><span data-stu-id="2886a-2651">If `x` is classified as a property or indexer access:</span></span>
   * <span data-ttu-id="2886a-2652">Örnek ifadesi (`x` `static`değilse) ve bağımsız değişken listesi (`x` bir Dizin Oluşturucu erişimsiyse) `x` ile ilişkili olarak değerlendirilir ve sonuçlar sonraki `set` erişimci çağrısında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2652">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `set` accessor invocation.</span></span>
   * <span data-ttu-id="2886a-2653">`y` değerlendirilir ve gerekirse örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) aracılığıyla `x` türüne dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2653">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="2886a-2654">`x` `set` erişimcisi `value` bağımsız değişkeni olarak `y` için hesaplanan değerle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2654">The `set` accessor of `x` is invoked with the value computed for `y` as its `value` argument.</span></span>

<span data-ttu-id="2886a-2655">Dizi birlikte değişim kuralları ([dizi Kovaryans](arrays.md#array-covariance)), dizi türü bir değere `A[]` `B[]`bir dizi türü örneğine izin verir, `B` ' den `A`örtük bir başvuru dönüştürmesi sağlanmış olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2655">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="2886a-2656">Bu kurallar nedeniyle, bir *reference_type* dizi öğesine atama, atanmakta olan değerin dizi örneğiyle uyumlu olduğundan emin olmak için bir çalışma zamanı denetimi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2656">Because of these rules, assignment to an array element of a *reference_type* requires a run-time check to ensure that the value being assigned is compatible with the array instance.</span></span> <span data-ttu-id="2886a-2657">Örnekte</span><span class="sxs-lookup"><span data-stu-id="2886a-2657">In the example</span></span>
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
<span data-ttu-id="2886a-2658">Son atama, bir `ArrayList` örneği bir `string[]`öğesinde depolanamadığı için `System.ArrayTypeMismatchException` oluşturulmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2658">the last assignment causes a `System.ArrayTypeMismatchException` to be thrown because an instance of `ArrayList` cannot be stored in an element of a `string[]`.</span></span>

<span data-ttu-id="2886a-2659">Bir *struct_type* içinde bildirildiği bir özellik veya Dizin Oluşturucu bir atamanın hedefi olduğunda, özellik veya Dizin Oluşturucu erişimi ile ilişkili örnek ifadesi bir değişken olarak sınıflandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2659">When a property or indexer declared in a *struct_type* is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="2886a-2660">Örnek ifadesi bir değer olarak sınıflandırılasiyse, bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2660">If the instance expression is classified as a value, a binding-time error occurs.</span></span> <span data-ttu-id="2886a-2661">[Üye erişimi](expressions.md#member-access)nedeniyle aynı kural alanlar için de geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2661">Because of [Member access](expressions.md#member-access), the same rule also applies to fields.</span></span>

<span data-ttu-id="2886a-2662">Bildirimler verildi:</span><span class="sxs-lookup"><span data-stu-id="2886a-2662">Given the declarations:</span></span>
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int X {
        get { return x; }
        set { x = value; }
    }

    public int Y {
        get { return y; }
        set { y = value; }
    }
}

struct Rectangle
{
    Point a, b;

    public Rectangle(Point a, Point b) {
        this.a = a;
        this.b = b;
    }

    public Point A {
        get { return a; }
        set { a = value; }
    }

    public Point B {
        get { return b; }
        set { b = value; }
    }
}
```
<span data-ttu-id="2886a-2663">Örnekte</span><span class="sxs-lookup"><span data-stu-id="2886a-2663">in the example</span></span>
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
<span data-ttu-id="2886a-2664">`p` ve `r` değişkenler olduğundan `p.X`, `p.Y`, `r.A`ve `r.B` atamalara izin verilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2664">the assignments to `p.X`, `p.Y`, `r.A`, and `r.B` are permitted because `p` and `r` are variables.</span></span> <span data-ttu-id="2886a-2665">Ancak, örnekte</span><span class="sxs-lookup"><span data-stu-id="2886a-2665">However, in the example</span></span>
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
<span data-ttu-id="2886a-2666">`r.A` ve `r.B` değişken olmadığından atamalar geçersizdir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2666">the assignments are all invalid, since `r.A` and `r.B` are not variables.</span></span>

### <a name="compound-assignment"></a><span data-ttu-id="2886a-2667">Bileşik atama</span><span class="sxs-lookup"><span data-stu-id="2886a-2667">Compound assignment</span></span>

<span data-ttu-id="2886a-2668">Bileşik atamanın sol işleneni `E.P` veya `E` derleme zamanı türü `dynamic`sahip olduğu `E[Ei]`, atama dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="2886a-2668">If the left operand of a compound assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="2886a-2669">Bu durumda, atama ifadesinin derleme zamanı türü `dynamic`ve aşağıda açıklanan çözüm, `E`çalışma zamanı türüne göre çalışma zamanında gerçekleşmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2669">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="2886a-2670">`x op= y` bir işlem, işlem `x op y`yazılmış gibi ikili işleç aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2670">An operation of the form `x op= y` is processed by applying binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x op y`.</span></span> <span data-ttu-id="2886a-2671">Ni</span><span class="sxs-lookup"><span data-stu-id="2886a-2671">Then,</span></span>

*  <span data-ttu-id="2886a-2672">Seçili işlecin dönüş türü örtük olarak `x`türüne dönüştürülebilir ise, işlem `x = x op y`olarak değerlendirilir, ancak `x` yalnızca bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2672">If the return type of the selected operator is implicitly convertible to the type of `x`, the operation is evaluated as `x = x op y`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="2886a-2673">Aksi takdirde, seçilen işleç önceden tanımlanmış bir işleçse, seçili işlecin dönüş türü `x`türüne açıkça dönüştürülebilir ve `y` `x` türüne örtük olarak dönüştürülebilir veya işleç bir kaydırma işleçse, işlem `x = (T)(x op y)`olarak değerlendirilir; burada `T`, `x`yalnızca bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2673">Otherwise, if the selected operator is a predefined operator, if the return type of the selected operator is explicitly convertible to the type of `x`, and if `y` is implicitly convertible to the type of `x` or the operator is a shift operator, then the operation is evaluated as `x = (T)(x op y)`, where `T` is the type of `x`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="2886a-2674">Aksi takdirde, bileşik atama geçersizdir ve bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2674">Otherwise, the compound assignment is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="2886a-2675">"Yalnızca bir kez değerlendirilir" terimi, `x op y`değerlendirmesinde, `x` yapısal ifadelerin sonuçlarının geçici olarak kaydedildiği ve `x`atamasını gerçekleştirirken yeniden kullanıldığı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2675">The term "evaluated only once" means that in the evaluation of `x op y`, the results of any constituent expressions of `x` are temporarily saved and then reused when performing the assignment to `x`.</span></span> <span data-ttu-id="2886a-2676">Örneğin, atama `A()[B()] += C()`, `A` `int[]`döndüren bir yöntem olduğu ve `B` ve `C` `int`döndüren metotlardır, yöntemler yalnızca bir kez çağrılır; sırasıyla `A`, `B``C`.</span><span class="sxs-lookup"><span data-stu-id="2886a-2676">For example, in the assignment `A()[B()] += C()`, where `A` is a method returning `int[]`, and `B` and `C` are methods returning `int`, the methods are invoked only once, in the order `A`, `B`, `C`.</span></span>

<span data-ttu-id="2886a-2677">Bileşik atamanın sol işleneni bir özellik erişimi veya Dizin Oluşturucu erişimi olduğunda, özelliğin veya dizin oluşturucunun hem `get` erişimcisi hem de bir `set` erişimcisi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2677">When the left operand of a compound assignment is a property access or indexer access, the property or indexer must have both a `get` accessor and a `set` accessor.</span></span> <span data-ttu-id="2886a-2678">Bu durumda, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2678">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="2886a-2679">Yukarıdaki ikinci kural, `x op= y` belirli bağlamlarda `x = (T)(x op y)` olarak değerlendirilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2679">The second rule above permits `x op= y` to be evaluated as `x = (T)(x op y)` in certain contexts.</span></span> <span data-ttu-id="2886a-2680">Kural, sol işlenen `sbyte`, `byte`, `short`, `ushort`veya `char`türünde olduğunda, önceden tanımlanmış işleçlerin Bileşik işleçler olarak kullanılabilmesi için vardır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2680">The rule exists such that the predefined operators can be used as compound operators when the left operand is of type `sbyte`, `byte`, `short`, `ushort`, or `char`.</span></span> <span data-ttu-id="2886a-2681">Her iki bağımsız değişken de bu türlerden birinde olsa da, önceden tanımlanmış işleçler, [ikili sayısal yükseltmeler](expressions.md#binary-numeric-promotions)bölümünde açıklandığı gibi `int`türünde bir sonuç üretir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2681">Even when both arguments are of one of those types, the predefined operators produce a result of type `int`, as described in [Binary numeric promotions](expressions.md#binary-numeric-promotions).</span></span> <span data-ttu-id="2886a-2682">Bu nedenle, bir dönüştürme olmadan sonucu sol işlenene atamak mümkün olmaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-2682">Thus, without a cast it would not be possible to assign the result to the left operand.</span></span>

<span data-ttu-id="2886a-2683">Önceden tanımlanmış işleçler için kuralın sezgisel etkisi, hem `x op y` hem de `x = y` izin verildiğinde `x op= y` izin verilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2683">The intuitive effect of the rule for predefined operators is simply that `x op= y` is permitted if both of `x op y` and `x = y` are permitted.</span></span> <span data-ttu-id="2886a-2684">Örnekte</span><span class="sxs-lookup"><span data-stu-id="2886a-2684">In the example</span></span>
```csharp
byte b = 0;
char ch = '\0';
int i = 0;

b += 1;             // Ok
b += 1000;          // Error, b = 1000 not permitted
b += i;             // Error, b = i not permitted
b += (byte)i;       // Ok

ch += 1;            // Error, ch = 1 not permitted
ch += (char)1;      // Ok
```
<span data-ttu-id="2886a-2685">her hatanın sezgisel olmasının nedeni, karşılık gelen basit atamanın de hata olması olabilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2685">the intuitive reason for each error is that a corresponding simple assignment would also have been an error.</span></span>

<span data-ttu-id="2886a-2686">Bu ayrıca bileşik atama işlemlerinin yükseltilmemiş işlemlerini desteklediği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2686">This also means that compound assignment operations support lifted operations.</span></span> <span data-ttu-id="2886a-2687">Örnekte</span><span class="sxs-lookup"><span data-stu-id="2886a-2687">In the example</span></span>
```csharp
int? i = 0;
i += 1;             // Ok
```
<span data-ttu-id="2886a-2688">yükseltilmemiş işleci `+(int?,int?)` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2688">the lifted operator `+(int?,int?)` is used.</span></span>

### <a name="event-assignment"></a><span data-ttu-id="2886a-2689">Olay ataması</span><span class="sxs-lookup"><span data-stu-id="2886a-2689">Event assignment</span></span>

<span data-ttu-id="2886a-2690">Bir `+=` veya `-=` işlecinin sol işleneni bir olay erişimi olarak sınıflandırıldığında, ifade aşağıdaki gibi değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2690">If the left operand of a `+=` or `-=` operator is classified as an event access, then the expression is evaluated as follows:</span></span>

*  <span data-ttu-id="2886a-2691">Olay erişiminin, varsa örnek ifadesi değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2691">The instance expression, if any, of the event access is evaluated.</span></span>
*  <span data-ttu-id="2886a-2692">`+=` veya `-=` işlecinin sağ işleneni değerlendirilir ve gerekirse örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) aracılığıyla sol işlenenin türüne dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2692">The right operand of the `+=` or `-=` operator is evaluated, and, if required, converted to the type of the left operand through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
*  <span data-ttu-id="2886a-2693">Etkinliğin olay erişimcisi, değerlendirmeden sonra ve gerekirse dönüştürme işlemi tamamlandıktan sonra bağımsız değişken listesiyle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2693">An event accessor of the event is invoked, with argument list consisting of the right operand, after evaluation and, if necessary, conversion.</span></span> <span data-ttu-id="2886a-2694">İşleç `+=`, `add` erişimci çağrılır; işleç `-=`, `remove` erişimcisi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2694">If the operator was `+=`, the `add` accessor is invoked; if the operator was `-=`, the `remove` accessor is invoked.</span></span>

<span data-ttu-id="2886a-2695">Olay atama ifadesi bir değer vermez.</span><span class="sxs-lookup"><span data-stu-id="2886a-2695">An event assignment expression does not yield a value.</span></span> <span data-ttu-id="2886a-2696">Bu nedenle, bir olay atama ifadesi yalnızca bir *statement_expression* ([ifade deyimleri](statements.md#expression-statements)) bağlamında geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2696">Thus, an event assignment expression is valid only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

## <a name="expression"></a><span data-ttu-id="2886a-2697">İfade</span><span class="sxs-lookup"><span data-stu-id="2886a-2697">Expression</span></span>

<span data-ttu-id="2886a-2698">Bir *ifade* *non_assignment_expression* veya *atamadır*.</span><span class="sxs-lookup"><span data-stu-id="2886a-2698">An *expression* is either a *non_assignment_expression* or an *assignment*.</span></span>

```antlr
expression
    : non_assignment_expression
    | assignment
    ;

non_assignment_expression
    : conditional_expression
    | lambda_expression
    | query_expression
    ;
```

## <a name="constant-expressions"></a><span data-ttu-id="2886a-2699">Sabit ifadeler</span><span class="sxs-lookup"><span data-stu-id="2886a-2699">Constant expressions</span></span>

<span data-ttu-id="2886a-2700">*Constant_expression* , derleme zamanında tam olarak değerlendirilebilen bir ifadedir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2700">A *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span>

```antlr
constant_expression
    : expression
    ;
```

<span data-ttu-id="2886a-2701">Sabit bir ifade, `null` sabit değeri veya şu türlerden birine sahip bir değer olmalıdır: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`veya herhangi bir numaralandırma türü.</span><span class="sxs-lookup"><span data-stu-id="2886a-2701">A constant expression must be the `null` literal or a value with one of  the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`, or any enumeration type.</span></span> <span data-ttu-id="2886a-2702">Sabit ifadelerde yalnızca aşağıdaki yapılara izin verilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2702">Only the following constructs are permitted in constant expressions:</span></span>

*  <span data-ttu-id="2886a-2703">Değişmez değerler (`null` değişmez değeri dahil).</span><span class="sxs-lookup"><span data-stu-id="2886a-2703">Literals (including the `null` literal).</span></span>
*  <span data-ttu-id="2886a-2704">Sınıf ve yapı türlerinin `const` üyelerine başvurular.</span><span class="sxs-lookup"><span data-stu-id="2886a-2704">References to `const` members of class and struct types.</span></span>
*  <span data-ttu-id="2886a-2705">Numaralandırma türlerinin üyelerine başvurular.</span><span class="sxs-lookup"><span data-stu-id="2886a-2705">References to members of enumeration types.</span></span>
*  <span data-ttu-id="2886a-2706">`const` parametrelere veya yerel değişkenlere başvurular</span><span class="sxs-lookup"><span data-stu-id="2886a-2706">References to `const` parameters or local variables</span></span>
*  <span data-ttu-id="2886a-2707">Kendi sabit ifadeleri olan parantezli alt ifadeler.</span><span class="sxs-lookup"><span data-stu-id="2886a-2707">Parenthesized sub-expressions, which are themselves constant expressions.</span></span>
*  <span data-ttu-id="2886a-2708">Hedef türü yukarıda listelenen türlerden biri olarak verilen atama ifadeleri.</span><span class="sxs-lookup"><span data-stu-id="2886a-2708">Cast expressions, provided the target type is one of the types listed above.</span></span>
*  <span data-ttu-id="2886a-2709">`checked` ve `unchecked` ifadeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2709">`checked` and `unchecked` expressions</span></span>
*  <span data-ttu-id="2886a-2710">Varsayılan değer ifadeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2710">Default value expressions</span></span>
*  <span data-ttu-id="2886a-2711">NameOf ifadeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2711">Nameof expressions</span></span>
*  <span data-ttu-id="2886a-2712">Önceden tanımlanmış `+`, `-`, `!`ve `~` Birli İşleçler.</span><span class="sxs-lookup"><span data-stu-id="2886a-2712">The predefined `+`, `-`, `!`, and `~` unary operators.</span></span>
*  <span data-ttu-id="2886a-2713">Önceden tanımlanmış `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`ve `>=` ikili işleçleri, her işlenen yukarıda listelenen bir tür.</span><span class="sxs-lookup"><span data-stu-id="2886a-2713">The predefined `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, and `>=` binary operators, provided each operand is of a type listed above.</span></span>
*  <span data-ttu-id="2886a-2714">`?:` koşullu işleç.</span><span class="sxs-lookup"><span data-stu-id="2886a-2714">The `?:` conditional operator.</span></span>

<span data-ttu-id="2886a-2715">Sabit ifadelerde aşağıdaki Dönüştürmelere izin verilir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2715">The following conversions are permitted in constant expressions:</span></span>

*  <span data-ttu-id="2886a-2716">Kimlik dönüştürmeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2716">Identity conversions</span></span>
*  <span data-ttu-id="2886a-2717">Sayısal Dönüşümler</span><span class="sxs-lookup"><span data-stu-id="2886a-2717">Numeric conversions</span></span>
*  <span data-ttu-id="2886a-2718">Sabit Listesi dönüştürmeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2718">Enumeration conversions</span></span>
*  <span data-ttu-id="2886a-2719">Sabit ifade dönüştürmeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2719">Constant expression conversions</span></span>
*  <span data-ttu-id="2886a-2720">Örtük ve açık başvuru dönüştürmeleri, dönüştürmelerin kaynağı null değeri değerlendiren sabit bir ifadedir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2720">Implicit and explicit reference conversions, provided that the source of the conversions is a constant expression that evaluates to the null value.</span></span>

<span data-ttu-id="2886a-2721">Null olmayan değerlerin kutulamayı, kutudan çıkarma ve örtük başvuru dönüştürmelerine dahil diğer dönüştürmeler sabit ifadelerde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="2886a-2721">Other conversions including boxing, unboxing and implicit reference conversions of non-null values are not permitted in constant expressions.</span></span> <span data-ttu-id="2886a-2722">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2886a-2722">For example:</span></span>
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
<span data-ttu-id="2886a-2723">bir paketleme dönüştürmesi gerektiğinden, ı başlatması bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2723">the initialization of i is an error because a boxing conversion is required.</span></span> <span data-ttu-id="2886a-2724">Null olmayan bir değerden örtük bir başvuru dönüştürmesi gerektiğinden Str başlatma bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2724">The initialization of str is an error because an implicit reference conversion from a non-null value is required.</span></span>

<span data-ttu-id="2886a-2725">Bir ifade yukarıda listelenen gereksinimleri karşılaışında, ifade derleme zamanında değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2725">Whenever an expression fulfills the requirements listed above, the expression is evaluated at compile-time.</span></span> <span data-ttu-id="2886a-2726">Bu, ifade sabit olmayan yapılar içeren daha büyük bir ifadenin alt ifadesi olsa da geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2726">This is true even if the expression is a sub-expression of a larger expression that contains non-constant constructs.</span></span>

<span data-ttu-id="2886a-2727">Sabit ifadelerin derleme zamanı değerlendirmesi, sabit olmayan ifadelerin çalışma zamanı değerlendirmesiyle aynı kuralları kullanır, ancak çalışma zamanı değerlendirmesinin bir özel durum oluşturması, derleme zamanı değerlendirmesinin de bir derleme zamanı hatası oluşmasına neden olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2886a-2727">The compile-time evaluation of constant expressions uses the same rules as run-time evaluation of non-constant expressions, except that where run-time evaluation would have thrown an exception, compile-time evaluation causes a compile-time error to occur.</span></span>

<span data-ttu-id="2886a-2728">Sabit bir ifade bir `unchecked` bağlamına açıkça yerleştirilmediği için, ifadenin derleme zamanı değerlendirmesi sırasında integral türü aritmetik işlemlerde ve dönüştürmelerde oluşan taşmalar her zaman derleme zamanı hatalarına neden olur ([sabit ifadeler](expressions.md#constant-expressions)).</span><span class="sxs-lookup"><span data-stu-id="2886a-2728">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur in integral-type arithmetic operations and conversions during the compile-time evaluation of the expression always cause compile-time errors ([Constant expressions](expressions.md#constant-expressions)).</span></span>

<span data-ttu-id="2886a-2729">Sabit ifadeler aşağıda listelenen bağlamlarda oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2729">Constant expressions occur in the contexts listed below.</span></span> <span data-ttu-id="2886a-2730">Bu bağlamlarda, bir ifade derleme zamanında tam olarak değerlendirilemez bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2730">In these contexts, a compile-time error occurs if an expression cannot be fully evaluated at compile-time.</span></span>

*  <span data-ttu-id="2886a-2731">Sabit bildirimler ([sabitler](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="2886a-2731">Constant declarations ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="2886a-2732">Numaralandırma üyesi bildirimleri ([enum üyeleri](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="2886a-2732">Enumeration member declarations ([Enum members](enums.md#enum-members)).</span></span>
*  <span data-ttu-id="2886a-2733">Biçimsel parametre listelerinin varsayılan bağımsız değişkenleri ([Yöntem parametreleri](classes.md#method-parameters))</span><span class="sxs-lookup"><span data-stu-id="2886a-2733">Default arguments of formal parameter lists ([Method parameters](classes.md#method-parameters))</span></span>
*  <span data-ttu-id="2886a-2734">`switch` deyimin etiketleri `case` ([Switch bildirisi](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="2886a-2734">`case` labels of a `switch` statement ([The switch statement](statements.md#the-switch-statement)).</span></span>
*  <span data-ttu-id="2886a-2735">`goto case` deyimleri ([goto deyimi](statements.md#the-goto-statement)).</span><span class="sxs-lookup"><span data-stu-id="2886a-2735">`goto case` statements ([The goto statement](statements.md#the-goto-statement)).</span></span>
*  <span data-ttu-id="2886a-2736">Bir Başlatıcı içeren bir dizi oluşturma ifadesinde ([dizi oluşturma ifadelerinde](expressions.md#array-creation-expressions)) boyut uzunlukları.</span><span class="sxs-lookup"><span data-stu-id="2886a-2736">Dimension lengths in an array creation expression ([Array creation expressions](expressions.md#array-creation-expressions)) that includes an initializer.</span></span>
*  <span data-ttu-id="2886a-2737">Öznitelikler ([öznitelikler](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="2886a-2737">Attributes ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="2886a-2738">Örtük bir sabit ifade dönüştürmesi ([örtük sabit ifade dönüştürmeleri](conversions.md#implicit-constant-expression-conversions)), `int` türünde sabit bir ifadenin `sbyte`, `byte`, `short`, `ushort`, `uint`veya `ulong`olarak dönüştürülmesini sağlar, ancak sabit ifadenin değeri hedef türü aralığı içinde olur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2738">An implicit constant expression conversion ([Implicit constant expression conversions](conversions.md#implicit-constant-expression-conversions)) permits a constant expression of type `int` to be converted to `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the constant expression is within the range of the destination type.</span></span>

## <a name="boolean-expressions"></a><span data-ttu-id="2886a-2739">Boole ifadeleri</span><span class="sxs-lookup"><span data-stu-id="2886a-2739">Boolean expressions</span></span>

<span data-ttu-id="2886a-2740">*Boolean_expression* , `bool`türünde bir sonuç veren bir ifadedir; Aşağıda belirtildiği gibi, belirli bağlamlarda doğrudan veya `operator true` uygulaması aracılığıyla.</span><span class="sxs-lookup"><span data-stu-id="2886a-2740">A *boolean_expression* is an expression that yields a result of type `bool`; either directly or through application of `operator true` in certain contexts as specified in the following.</span></span>

```antlr
boolean_expression
    : expression
    ;
```

<span data-ttu-id="2886a-2741">Bir *if_statement* ([IF deyimi](statements.md#the-if-statement)), *while_statement* ([while](statements.md#the-while-statement)deyimi), *do_statement* ([Do deyimi](statements.md#the-do-statement)) veya *for_statement* ([for deyimi](statements.md#the-for-statement)) denetim koşullu ifadesi bir *Boolean_expression*.</span><span class="sxs-lookup"><span data-stu-id="2886a-2741">The controlling conditional expression of an *if_statement* ([The if statement](statements.md#the-if-statement)), *while_statement* ([The while statement](statements.md#the-while-statement)), *do_statement* ([The do statement](statements.md#the-do-statement)), or *for_statement* ([The for statement](statements.md#the-for-statement)) is a *boolean_expression*.</span></span> <span data-ttu-id="2886a-2742">`?:` işlecinin ([koşullu operatör](expressions.md#conditional-operator)) denetim koşullu ifadesi, bir *Boolean_expression*aynı kurallara uyar, ancak işleç önceliğin nedenleri bir *conditional_or_expression*olarak sınıflandırıldı.</span><span class="sxs-lookup"><span data-stu-id="2886a-2742">The controlling conditional expression of the `?:` operator ([Conditional operator](expressions.md#conditional-operator)) follows the same rules as a *boolean_expression*, but for reasons of operator precedence is classified as a *conditional_or_expression*.</span></span>

<span data-ttu-id="2886a-2743">Bir *boolean_expression* `E`, aşağıdaki gibi `bool`türünde bir değer üretebilmek için gereklidir:</span><span class="sxs-lookup"><span data-stu-id="2886a-2743">A *boolean_expression* `E` is required to be able to produce a value of type `bool`, as follows:</span></span>

*  <span data-ttu-id="2886a-2744">`E` örtük olarak dönüştürülebilir bir `bool`, örtük dönüştürme uygulanmış çalışma zamanında.</span><span class="sxs-lookup"><span data-stu-id="2886a-2744">If `E` is implicitly convertible to `bool` then at runtime that implicit conversion is applied.</span></span>
*  <span data-ttu-id="2886a-2745">Aksi halde, tek işleç aşırı yükleme çözümlemesi ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)), `E``true` işlecinin benzersiz bir en iyi uygulamasını bulmak için kullanılır ve bu uygulama çalışma zamanında uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2886a-2745">Otherwise, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is used to find a unique best implementation of operator `true` on `E`, and that implementation is applied at runtime.</span></span>
*  <span data-ttu-id="2886a-2746">Böyle bir işleç bulunmazsa, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="2886a-2746">If no such operator is found, a binding-time error occurs.</span></span>

<span data-ttu-id="2886a-2747">[Veritabanı Boole türünde](structs.md#database-boolean-type) `DBBool` struct türü, `operator true` ve `operator false`uygulayan bir türe örnek sağlar.</span><span class="sxs-lookup"><span data-stu-id="2886a-2747">The `DBBool` struct type in [Database boolean type](structs.md#database-boolean-type) provides an example of a type that implements `operator true` and `operator false`.</span></span>
