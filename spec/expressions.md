# <a name="expressions"></a><span data-ttu-id="958cd-101">İfadeler</span><span class="sxs-lookup"><span data-stu-id="958cd-101">Expressions</span></span>

<span data-ttu-id="958cd-102">Bir ifade, işleçler ve işlenenler dizisidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-102">An expression is a sequence of operators and operands.</span></span> <span data-ttu-id="958cd-103">Bu bölümde sözdizimi, değerlendirme işleçler ve işlenenler ve ifadeler anlamını sırasını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="958cd-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

## <a name="expression-classifications"></a><span data-ttu-id="958cd-104">İfade sınıflandırmaları</span><span class="sxs-lookup"><span data-stu-id="958cd-104">Expression classifications</span></span>

<span data-ttu-id="958cd-105">Bir ifade aşağıdakilerden biri sınıflandırıldığını:</span><span class="sxs-lookup"><span data-stu-id="958cd-105">An expression is classified as one of the following:</span></span>

*  <span data-ttu-id="958cd-106">Bir değer.</span><span class="sxs-lookup"><span data-stu-id="958cd-106">A value.</span></span> <span data-ttu-id="958cd-107">Her değer, ilişkili bir türe sahip.</span><span class="sxs-lookup"><span data-stu-id="958cd-107">Every value has an associated type.</span></span>
*  <span data-ttu-id="958cd-108">Bir değişken.</span><span class="sxs-lookup"><span data-stu-id="958cd-108">A variable.</span></span> <span data-ttu-id="958cd-109">Her değişken, ilişkili bir türü, yani değişkeninin bildirilen türü vardır.</span><span class="sxs-lookup"><span data-stu-id="958cd-109">Every variable has an associated type, namely the declared type of the variable.</span></span>
*  <span data-ttu-id="958cd-110">Bir ad alanı.</span><span class="sxs-lookup"><span data-stu-id="958cd-110">A namespace.</span></span> <span data-ttu-id="958cd-111">Bu sınıflandırmada bir ifade yalnızca sol tarafında görünebilir bir *member_access* ([üye erişimi](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="958cd-111">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="958cd-112">Diğer herhangi bir bağlam içinde bir ad alanı olarak sınıflandırılmış bir ifade, bir derleme zamanı hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>
*  <span data-ttu-id="958cd-113">Bir tür.</span><span class="sxs-lookup"><span data-stu-id="958cd-113">A type.</span></span> <span data-ttu-id="958cd-114">Bu sınıflandırmada bir ifade yalnızca sol tarafında görünebilir bir *member_access* ([üye erişimi](expressions.md#member-access)), veya bir işleneni olarak `as` işleci ([işleci ](expressions.md#the-as-operator)), `is` işleci ([işleci](expressions.md#the-is-operator)), veya `typeof` işleci ([typeof işleci](expressions.md#the-typeof-operator)).</span><span class="sxs-lookup"><span data-stu-id="958cd-114">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)), or as an operand for the `as` operator ([The as operator](expressions.md#the-as-operator)), the `is` operator ([The is operator](expressions.md#the-is-operator)), or the `typeof` operator ([The typeof operator](expressions.md#the-typeof-operator)).</span></span> <span data-ttu-id="958cd-115">Diğer herhangi bir bağlam içinde bir tür olarak sınıflandırılmış bir ifade, bir derleme zamanı hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>
*  <span data-ttu-id="958cd-116">Bir üye aramasından kaynaklanan aşırı yüklenmiş yöntemler kümesi bir yöntem grubu ([üye araması](expressions.md#member-lookup)).</span><span class="sxs-lookup"><span data-stu-id="958cd-116">A method group, which is a set of overloaded methods resulting from a member lookup ([Member lookup](expressions.md#member-lookup)).</span></span> <span data-ttu-id="958cd-117">Bir yöntem grubu, ilişkilendirilmiş ifadenin ve ilişkili tür bağımsız değişken listesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-117">A method group may have an associated instance expression and an associated type argument list.</span></span> <span data-ttu-id="958cd-118">Bir örnek yöntemi çağrıldığında, örnek ifade değerlendirme sonucu örneği tarafından temsil edilen olur `this` ([bu erişim](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="958cd-118">When an instance method is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="958cd-119">Bir yöntem grubu içinde izin verilen bir *invocation_expression* ([çağrı ifadeleri](expressions.md#invocation-expressions)), *delegate_creation_expression* ([temsilci oluşturma ifadeleri](expressions.md#delegate-creation-expressions)) ve sol tarafında bir işleç ve bir uyumlu temsilci türüne örtük olarak dönüştürülebilir ([yöntem grubu dönüştürmeler](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="958cd-119">A method group is permitted in an *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)) , a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) and as the left hand side of an is operator, and can be implicitly converted to a compatible delegate type ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="958cd-120">Diğer herhangi bir bağlam içinde bir yöntem grubu sınıflandırılmış bir ifade, bir derleme zamanı hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-120">In any other context, an expression classified as a method group causes a compile-time error.</span></span>
*  <span data-ttu-id="958cd-121">Bir null sabit değer.</span><span class="sxs-lookup"><span data-stu-id="958cd-121">A null literal.</span></span> <span data-ttu-id="958cd-122">Bu sınıflandırmada bir ifade bir başvuru türüyle veya Null olabilen türü örtük olarak dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-122">An expression with this classification can be implicitly converted to a reference type or nullable type.</span></span>
*  <span data-ttu-id="958cd-123">Anonim bir işlevdir.</span><span class="sxs-lookup"><span data-stu-id="958cd-123">An anonymous function.</span></span> <span data-ttu-id="958cd-124">Bu sınıflandırmada bir ifade bir uyumlu temsilci türü ya da ifade ağaç türüne örtük olarak dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-124">An expression with this classification can be implicitly converted to a compatible delegate type or expression tree type.</span></span>
*  <span data-ttu-id="958cd-125">Özellik erişimi.</span><span class="sxs-lookup"><span data-stu-id="958cd-125">A property access.</span></span> <span data-ttu-id="958cd-126">Yani özellik türünün ilişkili bir türü her özellik erişebilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-126">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="958cd-127">Ayrıca, bir özellik erişimi ilişkili örnek bir ifade olabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-127">Furthermore, a property access may have an associated instance expression.</span></span> <span data-ttu-id="958cd-128">Erişimci olduğunda ( `get` veya `set` blok) örneğinin özellik erişimi çağrılır, örnek ifade değerlendirme sonucu örneği tarafından temsil edilen olur `this` ([bu erişim](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="958cd-128">When an accessor (the `get` or `set` block) of an instance property access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="958cd-129">Bir olay erişim.</span><span class="sxs-lookup"><span data-stu-id="958cd-129">An event access.</span></span> <span data-ttu-id="958cd-130">İlişkili bir türü, yani olay türünü her olay erişebilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="958cd-131">Ayrıca, bir olay erişimini ilişkili örnek bir ifade olabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-131">Furthermore, an event access may have an associated instance expression.</span></span> <span data-ttu-id="958cd-132">Olay erişimini'nın sol işlenen olarak görünebilir `+=` ve `-=` işleçleri ([olay ataması](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="958cd-132">An event access may appear as the left hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="958cd-133">Diğer herhangi bir bağlam içinde bir olay erişimini sınıflandırılmış bir ifade, bir derleme zamanı hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>
*  <span data-ttu-id="958cd-134">Bir dizin oluşturucu erişim.</span><span class="sxs-lookup"><span data-stu-id="958cd-134">An indexer access.</span></span> <span data-ttu-id="958cd-135">İlişkili bir türü, yani öğe türü Indexer'ın her dizin oluşturucu erişebilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-135">Every indexer access has an associated type, namely the element type of the indexer.</span></span> <span data-ttu-id="958cd-136">Ayrıca, bir dizin oluşturucu erişim ilişkilendirilmiş ifade ve ilişkili bağımsız değişken listesi vardır.</span><span class="sxs-lookup"><span data-stu-id="958cd-136">Furthermore, an indexer access has an associated instance expression and an associated argument list.</span></span> <span data-ttu-id="958cd-137">Erişimci olduğunda ( `get` veya `set` blok) bir dizin oluşturucu erişim çağrılır, örnek ifade değerlendirme sonucu örneği tarafından temsil edilen olur `this` ([bu erişim](expressions.md#this-access)), sonucu bağımsız değişken listesi değerlendirme çağırmanın parametre listesi olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-137">When an accessor (the `get` or `set` block) of an indexer access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)), and the result of evaluating the argument list becomes the parameter list of the invocation.</span></span>
*  <span data-ttu-id="958cd-138">Bir şey yok.</span><span class="sxs-lookup"><span data-stu-id="958cd-138">Nothing.</span></span> <span data-ttu-id="958cd-139">İfade bir yöntem çağrısından dönüş türüne sahip olduğunda bu gerçekleşir `void`.</span><span class="sxs-lookup"><span data-stu-id="958cd-139">This occurs when the expression is an invocation of a method with a return type of `void`.</span></span> <span data-ttu-id="958cd-140">Hiçbir şey yalnızca bağlamında geçerli olarak sınıflandırılmış bir ifade bir *statement_expression* ([ifade deyimleri](statements.md#expression-statements)).</span><span class="sxs-lookup"><span data-stu-id="958cd-140">An expression classified as nothing is only valid in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

<span data-ttu-id="958cd-141">Bir ifade sonucunu hiçbir zaman ad alanı, tür, yöntem grubuna veya olay erişimini ' dir.</span><span class="sxs-lookup"><span data-stu-id="958cd-141">The final result of an expression is never a namespace, type, method group, or event access.</span></span> <span data-ttu-id="958cd-142">Bunun yerine, yukarıda belirtildiği gibi bu kategorileri ifadelerin yalnızca belirli bağlamlarda izin verilen ara yapılarıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-142">Rather, as noted above, these categories of expressions are intermediate constructs that are only permitted in certain contexts.</span></span>

<span data-ttu-id="958cd-143">Bir özellik erişimi veya dizin oluşturucu erişim her zaman bir değer olarak, bir çağrı gerçekleştirerek yeniden sınıflandırılması *get erişimcisine* veya *set erişimcisine*.</span><span class="sxs-lookup"><span data-stu-id="958cd-143">A property access or indexer access is always reclassified as a value by performing an invocation of the *get accessor* or the *set accessor*.</span></span> <span data-ttu-id="958cd-144">Belirli bir erişimci özelliğin veya dizin oluşturucu erişim bağlamında tarafından belirlenir: erişim atama, hedef ise *set erişimcisine* yeni bir değer atamak için çağrılır ([basit atama](expressions.md#simple-assignment)) .</span><span class="sxs-lookup"><span data-stu-id="958cd-144">The particular accessor is determined by the context of the property or indexer access: If the access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="958cd-145">Aksi takdirde, *get erişimcisine* geçerli değerini almak için çağrılır ([ifadelerin değerleri](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="958cd-145">Otherwise, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="values-of-expressions"></a><span data-ttu-id="958cd-146">İfadelerin değerleri</span><span class="sxs-lookup"><span data-stu-id="958cd-146">Values of expressions</span></span>

<span data-ttu-id="958cd-147">Bir ifade içeren yapıları çoğunu nihai olarak belirtmek için ifadeyi gerektiren bir ***değer***.</span><span class="sxs-lookup"><span data-stu-id="958cd-147">Most of the constructs that involve an expression ultimately require the expression to denote a ***value***.</span></span> <span data-ttu-id="958cd-148">Gerçek ifade bir ad alanı, bir tür, bir yöntem grubu veya hiçbir şey gösteriyorsa bu gibi durumlarda, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-148">In such cases, if the actual expression denotes a namespace, a type, a method group, or nothing, a compile-time error occurs.</span></span> <span data-ttu-id="958cd-149">Ancak, ifade, özellik erişimi, bir dizin oluşturucu erişim veya bir değişken gösteriyorsa, özelliği, dizin oluşturucu veya değişken değeri örtük olarak değiştirilir:</span><span class="sxs-lookup"><span data-stu-id="958cd-149">However, if the expression denotes a property access, an indexer access, or a variable, the value of the property, indexer, or variable is implicitly substituted:</span></span>

*  <span data-ttu-id="958cd-150">Yalnızca şu anda değişkeni tarafından tanımlanan depolama konumunda depolanan değeri bir değişkene değeridir.</span><span class="sxs-lookup"><span data-stu-id="958cd-150">The value of a variable is simply the value currently stored in the storage location identified by the variable.</span></span> <span data-ttu-id="958cd-151">Bir değişken kesinlikle atanan dikkate alınmalıdır ([belirli atama onayına](variables.md#definite-assignment)) önce değeri alınabilir veya aksi halde bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-151">A variable must be considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained, or otherwise a compile-time error occurs.</span></span>
*  <span data-ttu-id="958cd-152">Bir özellik erişim ifadesinin değerine çağırarak elde edilen *get erişimcisine* özelliği.</span><span class="sxs-lookup"><span data-stu-id="958cd-152">The value of a property access expression is obtained by invoking the *get accessor* of the property.</span></span> <span data-ttu-id="958cd-153">Özellik Hayır varsa *get erişimcisine*, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-153">If the property has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="958cd-154">Aksi takdirde, işlev üyesi çağırma ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) gerçekleştirilir, ve çağrısının sonucunu özellik erişim ifadesinin değeri olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-154">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed, and the result of the invocation becomes the value of the property access expression.</span></span>
*  <span data-ttu-id="958cd-155">Bir dizin oluşturucu erişim ifadesinin değerini çağırarak elde *get erişimcisine* dizin oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="958cd-155">The value of an indexer access expression is obtained by invoking the *get accessor* of the indexer.</span></span> <span data-ttu-id="958cd-156">Dizin Oluşturucu Hayır varsa *get erişimcisine*, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-156">If the indexer has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="958cd-157">Aksi takdirde, bir işlev üye çağrısı ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) bağımsız değişkeniyle gerçekleştirilir listesi výraz indexeru erişim ile ilişkili ve bu değeri çağrısının sonucunu olur Dizin Oluşturucu erişim ifadesi.</span><span class="sxs-lookup"><span data-stu-id="958cd-157">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed with the argument list associated with the indexer access expression, and the result of the invocation becomes the value of the indexer access expression.</span></span>

## <a name="static-and-dynamic-binding"></a><span data-ttu-id="958cd-158">Statik ve dinamik bağlama</span><span class="sxs-lookup"><span data-stu-id="958cd-158">Static and Dynamic Binding</span></span>

<span data-ttu-id="958cd-159">Tür veya bağlı ifadeleri (bağımsız değişkenler, işlenen, Alıcılar) değerini temel alan bir işlem anlamını belirleme işlemi, genellikle olarak adlandırılır ***bağlama***.</span><span class="sxs-lookup"><span data-stu-id="958cd-159">The process of determining the meaning of an operation based on the type or value of constituent expressions (arguments, operands, receivers) is often referred to as ***binding***.</span></span> <span data-ttu-id="958cd-160">Örneğin bir yöntem çağrısının anlamı alıcı ve bağımsız değişkenler türüne göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-160">For instance the meaning of a method call is determined based on the type of the receiver and arguments.</span></span> <span data-ttu-id="958cd-161">Bir işleç anlamını işlenenleri türüne göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-161">The meaning of an operator is determined based on the type of its operands.</span></span>

<span data-ttu-id="958cd-162">C# anlamı bir işlem, genellikle belirlenir derleme zamanında, bağlı alt ifadeler, derleme zamanı türüne dayalı.</span><span class="sxs-lookup"><span data-stu-id="958cd-162">In C# the meaning of an operation is usually determined at compile-time, based on the compile-time type of its constituent expressions.</span></span> <span data-ttu-id="958cd-163">Benzer şekilde, bir ifade bir hata varsa hata algılandı ve derleyici tarafından bildirilen.</span><span class="sxs-lookup"><span data-stu-id="958cd-163">Likewise, if an expression contains an error, the error is detected and reported by the compiler.</span></span> <span data-ttu-id="958cd-164">Bu yaklaşım olarak da bilinen ***statik bağlama***.</span><span class="sxs-lookup"><span data-stu-id="958cd-164">This approach is known as ***static binding***.</span></span>

<span data-ttu-id="958cd-165">Ancak, bir ifade dinamik bir ifadeyi ise (yani türüne sahip `dynamic`) bu yer aldığı herhangi bir bağlamayı çalışma zamanı türünü (yani gerçek nesnenin türü çalışma zamanında gösterir) üzerinde bulunan, türü yerine dayalı olmaması gerektiğini gösterir. derleme zamanı.</span><span class="sxs-lookup"><span data-stu-id="958cd-165">However, if an expression is a dynamic expression (i.e. has the type `dynamic`) this indicates that any binding that it participates in should be based on its run-time type (i.e. the actual type of the object it denotes at run-time) rather than the type it has at compile-time.</span></span> <span data-ttu-id="958cd-166">Bu tür bir işlem bağlaması, bu nedenle işlem programın çalıştırılması sırasında yürütülmek üzere olduğu zamana kadar ertelenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-166">The binding of such an operation is therefore deferred until the time where the operation is to be executed during the running of the program.</span></span> <span data-ttu-id="958cd-167">Bu olarak adlandırılır ***dinamik bağlama***.</span><span class="sxs-lookup"><span data-stu-id="958cd-167">This is referred to as ***dynamic binding***.</span></span>

<span data-ttu-id="958cd-168">Bir işlemi dinamik olarak bağlı olduğunda, çok az kayıpla veya hiç denetimi derleyici tarafından gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-168">When an operation is dynamically bound, little or no checking is performed by the compiler.</span></span> <span data-ttu-id="958cd-169">Bunun yerine çalışma zamanı bağlama başarısız olursa, hataları çalışma zamanında özel durumlar olarak raporlanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-169">Instead if the run-time binding fails, errors are reported as exceptions at run-time.</span></span>

<span data-ttu-id="958cd-170">Aşağıdaki C# işlemlerinde tabi bağlanır:</span><span class="sxs-lookup"><span data-stu-id="958cd-170">The following operations in C# are subject to binding:</span></span>

*  <span data-ttu-id="958cd-171">Üye erişimi: `e.M`</span><span class="sxs-lookup"><span data-stu-id="958cd-171">Member access: `e.M`</span></span>
*  <span data-ttu-id="958cd-172">Yöntem çağırma: `e.M(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="958cd-172">Method invocation: `e.M(e1, ..., eN)`</span></span>
*  <span data-ttu-id="958cd-173">Temsilci çağrısı:`e(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="958cd-173">Delegate invocation:`e(e1, ..., eN)`</span></span>
*  <span data-ttu-id="958cd-174">Öğe erişimi: `e[e1, ..., eN]`</span><span class="sxs-lookup"><span data-stu-id="958cd-174">Element access: `e[e1, ..., eN]`</span></span>
*  <span data-ttu-id="958cd-175">Nesne oluşturma: `new C(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="958cd-175">Object creation: `new C(e1, ..., eN)`</span></span>
*  <span data-ttu-id="958cd-176">Birli işleçler aşırı: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span><span class="sxs-lookup"><span data-stu-id="958cd-176">Overloaded unary operators: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span></span>
*  <span data-ttu-id="958cd-177">İkili işleçler aşırı: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<` , `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span><span class="sxs-lookup"><span data-stu-id="958cd-177">Overloaded binary operators: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span></span>
*  <span data-ttu-id="958cd-178">Atama İşleçleri: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span><span class="sxs-lookup"><span data-stu-id="958cd-178">Assignment operators: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span></span>
*  <span data-ttu-id="958cd-179">Örtük ve açık dönüştürmeler</span><span class="sxs-lookup"><span data-stu-id="958cd-179">Implicit and explicit conversions</span></span>

<span data-ttu-id="958cd-180">Hiçbir dinamik ifadeler söz konusu olduğunda, C# derleme zamanı türlerini bağlı ifadeleri seçim işleminde kullanıldığı anlamına gelir statik bağlama varsayılan kullanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-180">When no dynamic expressions are involved, C# defaults to static binding, which means that the compile-time types of constituent expressions are used in the selection process.</span></span> <span data-ttu-id="958cd-181">Yukarıda listelenen işlemlerinde bağlı ifadelerden biri dinamik bir ifadeyi olduğunda, ancak işlemi yerine dinamik olarak bağlı.</span><span class="sxs-lookup"><span data-stu-id="958cd-181">However, when one of the constituent expressions in the operations listed above is a dynamic expression, the operation is instead dynamically bound.</span></span>

### <a name="binding-time"></a><span data-ttu-id="958cd-182">Bağlama zamanı</span><span class="sxs-lookup"><span data-stu-id="958cd-182">Binding-time</span></span>

<span data-ttu-id="958cd-183">Çalışma zamanında dinamik bağlama gerçekleşmeden ise statik bağlama derleme zamanında, gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="958cd-183">Static binding takes place at compile-time, whereas dynamic binding takes place at run-time.</span></span> <span data-ttu-id="958cd-184">Aşağıdaki bölümlerde terimi ***bağlama zamanı*** derleme zamanı veya çalışma bağlama ne zaman yer aldığı bağlı olarak süresi, ifade eder.</span><span class="sxs-lookup"><span data-stu-id="958cd-184">In the following sections, the term ***binding-time*** refers to either compile-time or run-time, depending on when the binding takes place.</span></span>

<span data-ttu-id="958cd-185">Aşağıdaki örnek, statik ve dinamik bağlama ve bağlama zamanı kavramları göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="958cd-185">The following example illustrates the notions of static and dynamic binding and of binding-time:</span></span>
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

<span data-ttu-id="958cd-186">İlk iki statik olarak bağlı: aşırı yüklemesini `Console.WriteLine` kendi bağımsız değişkenin derleme zamanı türüne göre seçilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-186">The first two calls are statically bound: the overload of `Console.WriteLine` is picked based on the compile-time type of their argument.</span></span> <span data-ttu-id="958cd-187">Bu nedenle, derleme zamanı bağlama zamanı.</span><span class="sxs-lookup"><span data-stu-id="958cd-187">Thus, the binding-time is compile-time.</span></span>

<span data-ttu-id="958cd-188">Üçüncü çağrı dinamik olarak bağlı: aşırı yüklemesini `Console.WriteLine` bağımsız değişkeninin çalışma zamanı türüne göre seçilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-188">The third call is dynamically bound: the overload of `Console.WriteLine` is picked based on the run-time type of its argument.</span></span> <span data-ttu-id="958cd-189">Derleme zamanı türü bildiğinizdir dinamik bir ifadeyi bağımsız değişken olduğu için böyle `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="958cd-189">This happens because the argument is a dynamic expression -- its compile-time type is `dynamic`.</span></span> <span data-ttu-id="958cd-190">Bu nedenle, üçüncü çağrı için bağlama süresi, çalışma zamanı.</span><span class="sxs-lookup"><span data-stu-id="958cd-190">Thus, the binding-time for the third call is run-time.</span></span>

### <a name="dynamic-binding"></a><span data-ttu-id="958cd-191">Dinamik bağlama</span><span class="sxs-lookup"><span data-stu-id="958cd-191">Dynamic binding</span></span>

<span data-ttu-id="958cd-192">Dinamik bağlama amacı ile etkileşim kurmak C# programları izin vermektir ***dinamik nesneler***, yani normal C# kurallarına izlemeyin nesneleri tür sistemi.</span><span class="sxs-lookup"><span data-stu-id="958cd-192">The purpose of dynamic binding is to allow C# programs to interact with ***dynamic objects***, i.e. objects that do not follow the normal rules of the C# type system.</span></span> <span data-ttu-id="958cd-193">Dinamik nesneler nesnelerden farklı sistemleri ile başka bir programlama dili olabilir veya program aracılığıyla farklı işlemler için kendi bağlama semantiği uygulamak için Kurulum olan nesneler olması olabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-193">Dynamic objects may be objects from other programming languages with different types systems, or they may be objects that are programmatically setup to implement their own binding semantics for different operations.</span></span>

<span data-ttu-id="958cd-194">Dinamik Nesne kendi semantiği uygulayan mekanizması tanımlanan uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-194">The mechanism by which a dynamic object implements its own semantics is implementation defined.</span></span> <span data-ttu-id="958cd-195">Belirli bir arabirim--yeniden tanımlanan uygulama--C# çalışma zamanı için özel semantiğine sahip olduğunu göstermek için dinamik nesneler tarafından uygulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-195">A given interface -- again implementation defined -- is implemented by dynamic objects to signal to the C# run-time that they have special semantics.</span></span> <span data-ttu-id="958cd-196">Dinamik bir nesne üzerinde işlemler dinamik olarak bağlı olduğunda, bu nedenle, kendi bağlama semantiği yerine C# bu belgede belirtildiği gibi devralır.</span><span class="sxs-lookup"><span data-stu-id="958cd-196">Thus, whenever operations on a dynamic object are dynamically bound, their own binding semantics, rather than those of C# as specified in this document, take over.</span></span>

<span data-ttu-id="958cd-197">Dinamik nesnelerle birlikte çalışabilirlik izin vermek için dinamik bağlama amacı olmakla birlikte, dinamik olsalar da olmadığını C# dinamik bağlamanın tüm nesneler üzerinde sağlar.</span><span class="sxs-lookup"><span data-stu-id="958cd-197">While the purpose of dynamic binding is to allow interoperation with dynamic objects, C# allows dynamic binding on all objects, whether they are dynamic or not.</span></span> <span data-ttu-id="958cd-198">Bu, üzerinde işlemler sonuçlarını değil kendilerini dinamik nesneler olabilir, ancak yine de programcıları derleme zamanında bilinmeyen bir tür ilgilidir dinamik Nesne için daha sorunsuz bir tümleştirme sağlar.</span><span class="sxs-lookup"><span data-stu-id="958cd-198">This allows for a smoother integration of dynamic objects, as the results of operations on them may not themselves be dynamic objects, but are still of a type unknown to the programmer at compile-time.</span></span> <span data-ttu-id="958cd-199">Ayrıca Dinamik bağlama ilgili hiçbir nesne dinamik nesneler olsa bile hata yapmaya açık yansıma tabanlı kodun ortadan kaldırılmasına yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-199">Also dynamic binding can help eliminate error-prone reflection-based code even when no objects involved are dynamic objects.</span></span>

<span data-ttu-id="958cd-200">Tam olarak tüm--uygulanır, dinamik bağlama, ne zaman denetleme--derleme uygulandığında ve hangi derleme zamanı sonucu ve ifade sınıflandırma olan dilde her yapı için aşağıdaki bölümlerde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="958cd-200">The following sections describe for each construct in the language exactly when dynamic binding is applied, what compile time checking -- if any -- is applied, and what the compile-time result and expression classification is.</span></span>

### <a name="types-of-constituent-expressions"></a><span data-ttu-id="958cd-201">Bağlı ifade türleri</span><span class="sxs-lookup"><span data-stu-id="958cd-201">Types of constituent expressions</span></span>

<span data-ttu-id="958cd-202">Bir işlem statik olarak bağlı olduğunda (örneğin bir alıcı ve bağımsız değişken, bir dizin veya bir işlenen) bağlı bir ifadenin türü her zaman bu ifade, derleme zamanı türünde kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-202">When an operation is statically bound, the type of a constituent expression (e.g. a receiver, and argument, an index or an operand) is always considered to be the compile-time type of that expression.</span></span>

<span data-ttu-id="958cd-203">Bir işlemi dinamik olarak bağlı olduğunda, bağlı bir ifadenin türü oluşturan ifadenin derleme zamanı türüne bağlı olarak farklı şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="958cd-203">When an operation is dynamically bound, the type of a constituent expression is determined in different ways depending on the compile-time type of the constituent expression:</span></span>

*  <span data-ttu-id="958cd-204">Derleme zamanı türündeki bağlı bir ifade `dynamic` ifade için çalışma zamanında değerlendirilir. gerçek değer türü olduğu kabul edilir</span><span class="sxs-lookup"><span data-stu-id="958cd-204">A constituent expression of compile-time type `dynamic` is considered to have the type of the actual value that the expression evaluates to at runtime</span></span>
*  <span data-ttu-id="958cd-205">Derleme zamanı türü olan bir tür parametresine bağlı bir ifade çalışma zamanında tür parametresi bağlı türe sahip olduğu kabul edildiği</span><span class="sxs-lookup"><span data-stu-id="958cd-205">A constituent expression whose compile-time type is a type parameter is considered to have the type which the type parameter is bound to at runtime</span></span>
*  <span data-ttu-id="958cd-206">Aksi takdirde bağlı ifadesi, derleme zamanı türü olduğu kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-206">Otherwise the constituent expression is considered to have its compile-time type.</span></span>

## <a name="operators"></a><span data-ttu-id="958cd-207">İşleçler</span><span class="sxs-lookup"><span data-stu-id="958cd-207">Operators</span></span>

<span data-ttu-id="958cd-208">İfadeleri oluşturulmuş ***işlenenler*** ve ***işleçleri***.</span><span class="sxs-lookup"><span data-stu-id="958cd-208">Expressions are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="958cd-209">İfade işleçleri işlenenlere uygulamak için hangi işlemleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="958cd-209">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="958cd-210">İşleçler örnekler `+`, `-`, `*`, `/`, ve `new`.</span><span class="sxs-lookup"><span data-stu-id="958cd-210">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="958cd-211">Değişmez değerler, alanlar, yerel değişkenleri ve ifadeleri işlenenler örnekleridir.</span><span class="sxs-lookup"><span data-stu-id="958cd-211">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="958cd-212">Üç tür işleç vardır:</span><span class="sxs-lookup"><span data-stu-id="958cd-212">There are three kinds of operators:</span></span>

*  <span data-ttu-id="958cd-213">Birli işleçler.</span><span class="sxs-lookup"><span data-stu-id="958cd-213">Unary operators.</span></span> <span data-ttu-id="958cd-214">Birli işleçleri, tek bir işlenen alır ve iki önek gösterimini kullanın (gibi `--x`) veya sonek gösteriminde (gibi `x++`).</span><span class="sxs-lookup"><span data-stu-id="958cd-214">The unary operators take one operand and use either prefix notation (such as `--x`) or postfix notation (such as `x++`).</span></span>
*  <span data-ttu-id="958cd-215">İkili işleçler.</span><span class="sxs-lookup"><span data-stu-id="958cd-215">Binary operators.</span></span> <span data-ttu-id="958cd-216">İkili işleçler iki işlenen alır ve tüm içtakı gösterimi kullanın (gibi `x + y`).</span><span class="sxs-lookup"><span data-stu-id="958cd-216">The binary operators take two operands and all use infix notation (such as `x + y`).</span></span>
*  <span data-ttu-id="958cd-217">Üçlü işleci.</span><span class="sxs-lookup"><span data-stu-id="958cd-217">Ternary operator.</span></span> <span data-ttu-id="958cd-218">Yalnızca bir Üçlü işleç `?:`, var; üç işlenen alır ve infix gösterim kullanır (`c ? x : y`).</span><span class="sxs-lookup"><span data-stu-id="958cd-218">Only one ternary operator, `?:`, exists; it takes three operands and uses infix notation (`c ? x : y`).</span></span>

<span data-ttu-id="958cd-219">Bir ifadede, işleçlerin değerlendirilme sırasını tarafından belirlenen ***öncelik*** ve ***ilişkilendirilebilirliği*** işleçlerinin ([İşleç önceliği ve ilişkiselliği](expressions.md#operator-precedence-and-associativity)) .</span><span class="sxs-lookup"><span data-stu-id="958cd-219">The order of evaluation of operators in an expression is determined by the ***precedence*** and ***associativity*** of the operators ([Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)).</span></span>

<span data-ttu-id="958cd-220">İfadelerde işlenenlerin soldan sağa doğru değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-220">Operands in an expression are evaluated from left to right.</span></span> <span data-ttu-id="958cd-221">Örneğin, `F(i) + G(i++) * H(i)`, yöntemi `F` eski değeri kullanılarak çağrılacak `i`, ardından yöntemi `G` eski değeriyle adlı `i`ve son olarak, yöntemi `H` Yenideğerleçağrılır`i`.</span><span class="sxs-lookup"><span data-stu-id="958cd-221">For example, in `F(i) + G(i++) * H(i)`, method `F` is called using the old value of `i`, then method `G` is called with the old value of `i`, and, finally, method `H` is called with the new value of `i`.</span></span> <span data-ttu-id="958cd-222">İşleç önceliği ayrıdır ve ilgisiz budur.</span><span class="sxs-lookup"><span data-stu-id="958cd-222">This is separate from and unrelated to operator precedence.</span></span>

<span data-ttu-id="958cd-223">Belirli işleçleri olabilir ***aşırı***.</span><span class="sxs-lookup"><span data-stu-id="958cd-223">Certain operators can be ***overloaded***.</span></span> <span data-ttu-id="958cd-224">İşleç aşırı yüklemesi bir yerde işlemleri için belirtilen kullanıcı tanımlı işleç uygulamalarına izin verir ya da her iki işlenen kullanıcı tanımlı sınıf veya yapı türü ([işleci aşırı yüklemesi](expressions.md#operator-overloading)).</span><span class="sxs-lookup"><span data-stu-id="958cd-224">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type ([Operator overloading](expressions.md#operator-overloading)).</span></span>

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="958cd-225">İşleç önceliği ve ilişkiselliği</span><span class="sxs-lookup"><span data-stu-id="958cd-225">Operator precedence and associativity</span></span>

<span data-ttu-id="958cd-226">Bir ifade birden çok işleç içeren ***öncelik*** işleçleri tek tek işleçler değerlendirilme sırası denetler.</span><span class="sxs-lookup"><span data-stu-id="958cd-226">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="958cd-227">Örneğin, ifade `x + y * z` değerlendirmesinde `x + (y * z)` çünkü `*` işleci, ikili daha yüksek bir önceliğe sahip `+` işleci.</span><span class="sxs-lookup"><span data-stu-id="958cd-227">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the binary `+` operator.</span></span> <span data-ttu-id="958cd-228">Bir işleç önceliği, ilişkili dilbilgisi üretim tanımına göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-228">The precedence of an operator is established by the definition of its associated grammar production.</span></span> <span data-ttu-id="958cd-229">Örneğin, bir *additive_expression* oluşan bir dizi *multiplicative_expression*s ayırarak `+` veya `-` işleçleri, bu nedenle vererek `+` ve `-` işleçleri daha düşük önceliğe `*`, `/`, ve `%` işleçleri.</span><span class="sxs-lookup"><span data-stu-id="958cd-229">For example, an *additive_expression* consists of a sequence of *multiplicative_expression*s separated by `+` or `-` operators, thus giving the `+` and `-` operators lower precedence than the `*`, `/`, and `%` operators.</span></span>

<span data-ttu-id="958cd-230">Tüm işleçler sırayla önceliği en yüksekten en düşüğe aşağıdaki tabloda özetlenmiştir:</span><span class="sxs-lookup"><span data-stu-id="958cd-230">The following table summarizes all operators in order of precedence from highest to lowest:</span></span>

| <span data-ttu-id="958cd-231">__Bölüm__</span><span class="sxs-lookup"><span data-stu-id="958cd-231">__Section__</span></span>                                                                                   | <span data-ttu-id="958cd-232">__Kategori__</span><span class="sxs-lookup"><span data-stu-id="958cd-232">__Category__</span></span>                | <span data-ttu-id="958cd-233">__İşleçler__</span><span class="sxs-lookup"><span data-stu-id="958cd-233">__Operators__</span></span> | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [<span data-ttu-id="958cd-234">Birincil ifadeler</span><span class="sxs-lookup"><span data-stu-id="958cd-234">Primary expressions</span></span>](expressions.md#primary-expressions)                                     | <span data-ttu-id="958cd-235">Birincil</span><span class="sxs-lookup"><span data-stu-id="958cd-235">Primary</span></span>                     | <span data-ttu-id="958cd-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span><span class="sxs-lookup"><span data-stu-id="958cd-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span></span> | 
| [<span data-ttu-id="958cd-237">Birli işleçler</span><span class="sxs-lookup"><span data-stu-id="958cd-237">Unary operators</span></span>](expressions.md#unary-operators)                                             | <span data-ttu-id="958cd-238">Birli</span><span class="sxs-lookup"><span data-stu-id="958cd-238">Unary</span></span>                       | <span data-ttu-id="958cd-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span><span class="sxs-lookup"><span data-stu-id="958cd-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span></span> | 
| [<span data-ttu-id="958cd-240">Aritmetik işleçler</span><span class="sxs-lookup"><span data-stu-id="958cd-240">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="958cd-241">Çarpma</span><span class="sxs-lookup"><span data-stu-id="958cd-241">Multiplicative</span></span>              | <span data-ttu-id="958cd-242">`*`  `/`  `%`</span><span class="sxs-lookup"><span data-stu-id="958cd-242">`*`  `/`  `%`</span></span> | 
| [<span data-ttu-id="958cd-243">Aritmetik işleçler</span><span class="sxs-lookup"><span data-stu-id="958cd-243">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="958cd-244">Eklenebilir</span><span class="sxs-lookup"><span data-stu-id="958cd-244">Additive</span></span>                    | <span data-ttu-id="958cd-245">`+`  `-`</span><span class="sxs-lookup"><span data-stu-id="958cd-245">`+`  `-`</span></span>      | 
| [<span data-ttu-id="958cd-246">Kaydırma işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-246">Shift operators</span></span>](expressions.md#shift-operators)                                             | <span data-ttu-id="958cd-247">Kaydırma</span><span class="sxs-lookup"><span data-stu-id="958cd-247">Shift</span></span>                       | <span data-ttu-id="958cd-248">`<<`  `>>`</span><span class="sxs-lookup"><span data-stu-id="958cd-248">`<<`  `>>`</span></span>    | 
| [<span data-ttu-id="958cd-249">İlişkisel ve tür testi işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-249">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="958cd-250">İlişkisel ve tür testi</span><span class="sxs-lookup"><span data-stu-id="958cd-250">Relational and type testing</span></span> | <span data-ttu-id="958cd-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span><span class="sxs-lookup"><span data-stu-id="958cd-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span></span> | 
| [<span data-ttu-id="958cd-252">İlişkisel ve tür testi işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-252">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="958cd-253">Eşitlik</span><span class="sxs-lookup"><span data-stu-id="958cd-253">Equality</span></span>                    | <span data-ttu-id="958cd-254">`==`  `!=`</span><span class="sxs-lookup"><span data-stu-id="958cd-254">`==`  `!=`</span></span>    | 
| [<span data-ttu-id="958cd-255">Mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="958cd-255">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="958cd-256">Mantıksal VE</span><span class="sxs-lookup"><span data-stu-id="958cd-256">Logical AND</span></span>                 | `&`           | 
| [<span data-ttu-id="958cd-257">Mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="958cd-257">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="958cd-258">Mantıksal XOR</span><span class="sxs-lookup"><span data-stu-id="958cd-258">Logical XOR</span></span>                 | `^`           | 
| [<span data-ttu-id="958cd-259">Mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="958cd-259">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="958cd-260">Mantıksal VEYA</span><span class="sxs-lookup"><span data-stu-id="958cd-260">Logical OR</span></span>                  | <code>&#124;</code>           |
| [<span data-ttu-id="958cd-261">Koşullu mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="958cd-261">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="958cd-262">Koşullu VE</span><span class="sxs-lookup"><span data-stu-id="958cd-262">Conditional AND</span></span>             | `&&`          | 
| [<span data-ttu-id="958cd-263">Koşullu mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="958cd-263">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="958cd-264">Koşullu VEYA</span><span class="sxs-lookup"><span data-stu-id="958cd-264">Conditional OR</span></span>              | <code>&#124;&#124;</code>          | 
| [<span data-ttu-id="958cd-265">Null birleşim işleci</span><span class="sxs-lookup"><span data-stu-id="958cd-265">The null coalescing operator</span></span>](expressions.md#the-null-coalescing-operator)                   | <span data-ttu-id="958cd-266">Null birleşim</span><span class="sxs-lookup"><span data-stu-id="958cd-266">Null coalescing</span></span>             | `??`          | 
| [<span data-ttu-id="958cd-267">Koşullu işleç</span><span class="sxs-lookup"><span data-stu-id="958cd-267">Conditional operator</span></span>](expressions.md#conditional-operator)                                   | <span data-ttu-id="958cd-268">Koşullu</span><span class="sxs-lookup"><span data-stu-id="958cd-268">Conditional</span></span>                 | `?:`          | 
| <span data-ttu-id="958cd-269">[Atama işleçleri](expressions.md#assignment-operators), [anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)</span><span class="sxs-lookup"><span data-stu-id="958cd-269">[Assignment operators](expressions.md#assignment-operators), [Anonymous function expressions](expressions.md#anonymous-function-expressions)</span></span>  | <span data-ttu-id="958cd-270">Atama ve lambda ifadesi</span><span class="sxs-lookup"><span data-stu-id="958cd-270">Assignment and lambda expression</span></span> | <span data-ttu-id="958cd-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span><span class="sxs-lookup"><span data-stu-id="958cd-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span></span> | 

<span data-ttu-id="958cd-272">Aynı önceliğe sahip iki işleç arasında bir işlenen ortaya çıktığında, işleçlerin ilişkiselliği işlemlerinin gerçekleştirildiği sırayı denetler:</span><span class="sxs-lookup"><span data-stu-id="958cd-272">When an operand occurs between two operators with the same precedence, the associativity of the operators controls the order in which the operations are performed:</span></span>

*  <span data-ttu-id="958cd-273">Atama işleçleri ve null birleşim işleci hariç tüm ikili işleçler şunlardır: ***ilişkilendirilebilir***, yani işlemler soldan sağa doğru gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-273">Except for the assignment operators and the null coalescing operator, all binary operators are ***left-associative***, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="958cd-274">Örneğin, `x + y + z` değerlendirmesinde `(x + y) + z`.</span><span class="sxs-lookup"><span data-stu-id="958cd-274">For example, `x + y + z` is evaluated as `(x + y) + z`.</span></span>
*  <span data-ttu-id="958cd-275">Atama İşleçleri, null birleşim işleci ve koşullu işleç (`?:`) olan ***sağla ilişkilendirilebilir***, sağdan sola işlemler gerçekleştirilir anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-275">The assignment operators, the null coalescing operator and the conditional operator (`?:`) are ***right-associative***, meaning that operations are performed from right to left.</span></span> <span data-ttu-id="958cd-276">Örneğin, `x = y = z` değerlendirmesinde `x = (y = z)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-276">For example, `x = y = z` is evaluated as `x = (y = z)`.</span></span>

<span data-ttu-id="958cd-277">Öncelik ve ilişkisellik parantez kullanılarak denetlenebilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-277">Precedence and associativity can be controlled using parentheses.</span></span> <span data-ttu-id="958cd-278">Örneğin, `x + y * z` ilk çarpar `y` tarafından `z` ve ardından sonuca ekler `x`, ancak `(x + y) * z` ilk ekler `x` ve `y` ve sonucu çarpan `z`.</span><span class="sxs-lookup"><span data-stu-id="958cd-278">For example, `x + y * z` first multiplies `y` by `z` and then adds the result to `x`, but `(x + y) * z` first adds `x` and `y` and then multiplies the result by `z`.</span></span>

### <a name="operator-overloading"></a><span data-ttu-id="958cd-279">İşleç aşırı yüklemesi</span><span class="sxs-lookup"><span data-stu-id="958cd-279">Operator overloading</span></span>

<span data-ttu-id="958cd-280">Tüm birli ve ikili işleçler, herhangi bir ifade içinde otomatik olarak kullanılabilir uygulamalar önceden tanımlanmış.</span><span class="sxs-lookup"><span data-stu-id="958cd-280">All unary and binary operators have predefined implementations that are automatically available in any expression.</span></span> <span data-ttu-id="958cd-281">Önceden tanımlanmış uygulamalarının yanı sıra, kullanıcı tanımlı uygulamaları dahil ederek tanıtılabilir `operator` bildirimlerinde sınıflar ve yapılar ([işleçleri](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="958cd-281">In addition to the predefined implementations, user-defined implementations can be introduced by including `operator` declarations in classes and structs ([Operators](classes.md#operators)).</span></span> <span data-ttu-id="958cd-282">Kullanıcı tanımlı işleç uygulamalarına her zaman önceliklidir önceden tanımlanmış işleç uygulamalarına: hiçbir geçerli kullanıcı tanımlı işleç uygulamalarına varken önceden tanımlanmış işleç uygulamalarına, içindeaçıklananşekildedeğerlendirilmesiyalnızca[ Birli işleç aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution) ve [ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="958cd-282">User-defined operator implementations always take precedence over predefined operator implementations: Only when no applicable user-defined operator implementations exist will the predefined operator implementations be considered, as described in [Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution).</span></span>

<span data-ttu-id="958cd-283">***Aşırı yüklenebilir birli işleçler*** şunlardır:</span><span class="sxs-lookup"><span data-stu-id="958cd-283">The ***overloadable unary operators*** are:</span></span>
```csharp
+   -   !   ~   ++   --   true   false
```

<span data-ttu-id="958cd-284">Ancak `true` ve `false` ifadelerinde açıkça kullanılmaz (ve bu nedenle öncelik tablosunda bulunmayan [İşleç önceliği ve ilişkiselliği](expressions.md#operator-precedence-and-associativity)), çünkü bunlar işleçler değerlendirilir birden fazla ifade bağlamlarda çağrılır: boolean ifadeleri ([Boolean ifadeler](expressions.md#boolean-expressions)) ve koşul içeren ifadeler ([koşullu işleç](expressions.md#conditional-operator)) ve koşullu mantıksal işleçler ([koşullu mantıksal işleçler](expressions.md#conditional-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="958cd-284">Although `true` and `false` are not used explicitly in expressions (and therefore are not included in the precedence table in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)), they are considered operators because they are invoked in several expression contexts: boolean expressions ([Boolean expressions](expressions.md#boolean-expressions)) and expressions involving the conditional ([Conditional operator](expressions.md#conditional-operator)), and conditional logical operators ([Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span>

<span data-ttu-id="958cd-285">***Aşırı yüklenebilir ikili işleçler*** şunlardır:</span><span class="sxs-lookup"><span data-stu-id="958cd-285">The ***overloadable binary operators*** are:</span></span>
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

<span data-ttu-id="958cd-286">Yalnızca yukarıda listelenen işleçleri aşırı yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-286">Only the operators listed above can be overloaded.</span></span> <span data-ttu-id="958cd-287">Özellikle, üye erişimi, yöntem çağırma aşırı mümkün değildir veya `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, ve `is` işleçleri.</span><span class="sxs-lookup"><span data-stu-id="958cd-287">In particular, it is not possible to overload member access, method invocation, or the `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, and `is` operators.</span></span>

<span data-ttu-id="958cd-288">İkili İşleç aşırı karşılık gelen atama işleci, varsa, aynı zamanda örtük olarak aşırı yüklenmiş olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-288">When a binary operator is overloaded, the corresponding assignment operator, if any, is also implicitly overloaded.</span></span> <span data-ttu-id="958cd-289">Örneğin, bir işleç aşırı `*` aynı zamanda bir işleç aşırı yüklemesi olan `*=`.</span><span class="sxs-lookup"><span data-stu-id="958cd-289">For example, an overload of operator `*` is also an overload of operator `*=`.</span></span> <span data-ttu-id="958cd-290">Daha ayrıntılı açıklanır budur [bileşik atama](expressions.md#compound-assignment).</span><span class="sxs-lookup"><span data-stu-id="958cd-290">This is described further in [Compound assignment](expressions.md#compound-assignment).</span></span> <span data-ttu-id="958cd-291">Unutmayın atama işleci (`=`) aşırı yüklenemez.</span><span class="sxs-lookup"><span data-stu-id="958cd-291">Note that the assignment operator itself (`=`) cannot be overloaded.</span></span> <span data-ttu-id="958cd-292">Atama her zaman bir değişkene bir değer Bitsel basit kopyasını gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="958cd-292">An assignment always performs a simple bit-wise copy of a value into a variable.</span></span>

<span data-ttu-id="958cd-293">İşlemleri gibi cast `(T)x`, kullanıcı tanımlı dönüşümler sağlayarak aşırı ([kullanıcı tanımlı dönüşümler](conversions.md#user-defined-conversions)).</span><span class="sxs-lookup"><span data-stu-id="958cd-293">Cast operations, such as `(T)x`, are overloaded by providing user-defined conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

<span data-ttu-id="958cd-294">Öğe erişimi, gibi `a[x]`, bir aşırı yüklenebilir işleç olarak kabul edilmez.</span><span class="sxs-lookup"><span data-stu-id="958cd-294">Element access, such as `a[x]`, is not considered an overloadable operator.</span></span> <span data-ttu-id="958cd-295">Kullanıcı tanımlı dizin dizin oluşturucular bunun yerine, desteklenir ([dizin oluşturucular](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="958cd-295">Instead, user-defined indexing is supported through indexers ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="958cd-296">İfadeler, işleçleri, işleç gösterimi kullanılarak başvurulur ve bildirimlerinde, işleçler işlevsel bir gösterim kullanılarak başvurulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-296">In expressions, operators are referenced using operator notation, and in declarations, operators are referenced using functional notation.</span></span> <span data-ttu-id="958cd-297">Aşağıdaki tabloda, işleci ve tekli veya ikili işleçler için işlevsel gösterimler arasındaki ilişkiyi gösterir.</span><span class="sxs-lookup"><span data-stu-id="958cd-297">The following table shows the relationship between operator and functional notations for unary and binary operators.</span></span> <span data-ttu-id="958cd-298">İlk giriş *op* hiçbir aşırı yüklenebilir birli önek işleci gösterir.</span><span class="sxs-lookup"><span data-stu-id="958cd-298">In the first entry, *op* denotes any overloadable unary prefix operator.</span></span> <span data-ttu-id="958cd-299">İkinci giriş *op* birli sonek gösterir `++` ve `--` işleçleri.</span><span class="sxs-lookup"><span data-stu-id="958cd-299">In the second entry, *op* denotes the unary postfix `++` and `--` operators.</span></span> <span data-ttu-id="958cd-300">Üçüncü giriş *op* hiçbir aşırı yüklenebilir ikili işleç gösterir.</span><span class="sxs-lookup"><span data-stu-id="958cd-300">In the third entry, *op* denotes any overloadable binary operator.</span></span>


| <span data-ttu-id="958cd-301">__İşleç gösterimi__</span><span class="sxs-lookup"><span data-stu-id="958cd-301">__Operator notation__</span></span> | <span data-ttu-id="958cd-302">__İşlevsel bir gösterim__</span><span class="sxs-lookup"><span data-stu-id="958cd-302">__Functional notation__</span></span> |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

<span data-ttu-id="958cd-303">Kullanıcı tanımlı işleç bildirimleri her zaman en az bir parametre işleç bildirimi içeren sınıf veya yapı türü olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="958cd-303">User-defined operator declarations always require at least one of the parameters to be of the class or struct type that contains the operator declaration.</span></span> <span data-ttu-id="958cd-304">Bu nedenle, önceden tanımlanmış işleç aynı imzaya sahip bir kullanıcı tanımlı işleç için mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="958cd-304">Thus, it is not possible for a user-defined operator to have the same signature as a predefined operator.</span></span>

<span data-ttu-id="958cd-305">Kullanıcı tanımlı işleç bildirimi sözdizimi, öncelik veya bir işleç ilişkilendirilebilirliği değiştiremezsiniz.</span><span class="sxs-lookup"><span data-stu-id="958cd-305">User-defined operator declarations cannot modify the syntax, precedence, or associativity of an operator.</span></span> <span data-ttu-id="958cd-306">Örneğin, `/` ikili işleç, her zaman sahip öncelik düzeyi belirtilen işleci her zaman, [İşleç önceliği ve ilişkiselliği](expressions.md#operator-precedence-and-associativity)ve her zaman ilişkilendirilebilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-306">For example, the `/` operator is always a binary operator, always has the precedence level specified in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity), and is always left-associative.</span></span>

<span data-ttu-id="958cd-307">Bunu pleases herhangi bir hesaplama gerçekleştirmek bir kullanıcı tanımlı işleç için mümkün olsa da, sonuçlar sezgisel beklenen dışındaki uygulamalar kesinlikle önerilmez.</span><span class="sxs-lookup"><span data-stu-id="958cd-307">While it is possible for a user-defined operator to perform any computation it pleases, implementations that produce results other than those that are intuitively expected are strongly discouraged.</span></span> <span data-ttu-id="958cd-308">Örneğin, uygulaması `operator ==` ve eşitlik için iki işlenenden karşılaştırma uygun bir dönüş `bool` sonucu.</span><span class="sxs-lookup"><span data-stu-id="958cd-308">For example, an implementation of `operator ==` should compare the two operands for equality and return an appropriate `bool` result.</span></span>

<span data-ttu-id="958cd-309">Tek tek işleçler açıklamalarını [birincil ifadeler](expressions.md#primary-expressions) aracılığıyla [koşullu mantıksal işleçler](expressions.md#conditional-logical-operators) önceden tanımlanmış uygulamalar işleçler ve geçerli tüm ek kuralları belirtin her işleç için.</span><span class="sxs-lookup"><span data-stu-id="958cd-309">The descriptions of individual operators in [Primary expressions](expressions.md#primary-expressions) through [Conditional logical operators](expressions.md#conditional-logical-operators) specify the predefined implementations of the operators and any additional rules that apply to each operator.</span></span> <span data-ttu-id="958cd-310">Açıklamaları olun kullanım koşulları ***birli işleç aşırı yükleme çözünürlüğü***, ***ikili işleci aşırı yükleme çözünürlüğü***, ve ***sayısal yükseltme***, bir işlem olan tanımları Aşağıdaki bölümlerde bulundu.</span><span class="sxs-lookup"><span data-stu-id="958cd-310">The descriptions make use of the terms ***unary operator overload resolution***, ***binary operator overload resolution***, and ***numeric promotion***, definitions of which are found in the following sections.</span></span>

### <a name="unary-operator-overload-resolution"></a><span data-ttu-id="958cd-311">Birli işleç aşırı yükleme çözünürlüğü</span><span class="sxs-lookup"><span data-stu-id="958cd-311">Unary operator overload resolution</span></span>

<span data-ttu-id="958cd-312">Bir işlem formun `op x` veya `x op`burada `op` bir aşırı yüklenebilir birli işleç ve `x` türündeki bir ifade `X`, şu şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="958cd-312">An operation of the form `op x` or `x op`, where `op` is an overloadable unary operator, and `x` is an expression of type `X`, is processed as follows:</span></span>

*  <span data-ttu-id="958cd-313">Aday kullanıcı tanımlı işleçler tarafından sağlanan dizi `X` işlemi için `operator op(x)` kurallarını kullanarak belirlenir [aday kullanıcı tanımlı işleçler](expressions.md#candidate-user-defined-operators).</span><span class="sxs-lookup"><span data-stu-id="958cd-313">The set of candidate user-defined operators provided by `X` for the operation `operator op(x)` is determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span>
*  <span data-ttu-id="958cd-314">Aday kullanıcı tanımlı işleçler kümesi boş değilse, bu işlem için aday işleçleri kümesini olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-314">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="958cd-315">Aksi takdirde, önceden tanımlanmış birli `operator op` lifted formlarına dahil olmak üzere uygulamalar, işlem için aday işleçleri kümesini haline gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-315">Otherwise, the predefined unary `operator op` implementations, including their lifted forms, become the set of candidate operators for the operation.</span></span> <span data-ttu-id="958cd-316">Önceden tanımlanmış uygulamalar belirli bir işlecin işleci açıklamasında belirtilen ([birincil ifadeler](expressions.md#primary-expressions) ve [birli işleçler](expressions.md#unary-operators)).</span><span class="sxs-lookup"><span data-stu-id="958cd-316">The predefined implementations of a given operator are specified in the description of the operator ([Primary expressions](expressions.md#primary-expressions) and [Unary operators](expressions.md#unary-operators)).</span></span>
*  <span data-ttu-id="958cd-317">Aşırı yükleme çözünürlüğü kurallarına [aşırı yükleme çözünürlüğü](expressions.md#overload-resolution) bağımsız değişken listesi ile ilgili en iyi operatör seçmek için aday işleçleri kümesini uygulanan `(x)`, ve bu işleci aşırı yükleme sonucu haline gelir. Çözümleme işlemi.</span><span class="sxs-lookup"><span data-stu-id="958cd-317">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="958cd-318">Tek bir en iyi operatör seçmek aşırı yükleme çözümlemesi başarısız olursa bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-318">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="binary-operator-overload-resolution"></a><span data-ttu-id="958cd-319">İkili İşleç aşırı yükleme çözünürlüğü</span><span class="sxs-lookup"><span data-stu-id="958cd-319">Binary operator overload resolution</span></span>

<span data-ttu-id="958cd-320">Bir işlem formun `x op y`burada `op` bir aşırı yüklenebilir ikili işleç `x` türündeki bir ifade `X`, ve `y` türündeki bir ifade `Y`, şu şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="958cd-320">An operation of the form `x op y`, where `op` is an overloadable binary operator, `x` is an expression of type `X`, and `y` is an expression of type `Y`, is processed as follows:</span></span>

*  <span data-ttu-id="958cd-321">Aday kullanıcı tanımlı işleçler tarafından sağlanan dizi `X` ve `Y` işlemi için `operator op(x,y)` belirlenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-321">The set of candidate user-defined operators provided by `X` and `Y` for the operation `operator op(x,y)` is determined.</span></span> <span data-ttu-id="958cd-322">Tarafından sağlanan aday işleçleri birleşimi kümesi oluşan `X` ve tarafından sağlanan aday işleçleri `Y`, her belirlenen kurallarını kullanarak [aday kullanıcı tanımlı işleçler](expressions.md#candidate-user-defined-operators).</span><span class="sxs-lookup"><span data-stu-id="958cd-322">The set consists of the union of the candidate operators provided by `X` and the candidate operators provided by `Y`, each determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span> <span data-ttu-id="958cd-323">Varsa `X` ve `Y` aynı türde olan veya `X` ve `Y` paylaşılan aday işleçler yalnızca Birleşik kümesine defa geçebildiği sonra ortak bir taban türünden türetilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-323">If `X` and `Y` are the same type, or if `X` and `Y` are derived from a common base type, then shared candidate operators only occur in the combined set once.</span></span>
*  <span data-ttu-id="958cd-324">Aday kullanıcı tanımlı işleçler kümesi boş değilse, bu işlem için aday işleçleri kümesini olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-324">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="958cd-325">Aksi takdirde, önceden tanımlanmış ikili `operator op` lifted formlarına dahil olmak üzere uygulamalar, işlem için aday işleçleri kümesini haline gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-325">Otherwise, the predefined binary `operator op` implementations, including their lifted forms,  become the set of candidate operators for the operation.</span></span> <span data-ttu-id="958cd-326">Önceden tanımlanmış uygulamalar belirli bir işlecin işleci açıklamasında belirtilen ([aritmetik işleçler](expressions.md#arithmetic-operators) aracılığıyla [koşullu mantıksal işleçler](expressions.md#conditional-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="958cd-326">The predefined implementations of a given operator are specified in the description of the operator ([Arithmetic operators](expressions.md#arithmetic-operators) through [Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span> <span data-ttu-id="958cd-327">Önceden tanımlanmış sabit listesi ve temsilci işleçleri için kabul yalnızca biri bağlama zamanı türü olan bir enum veya temsilci türüne göre tanımlanan işleçleridir.</span><span class="sxs-lookup"><span data-stu-id="958cd-327">For predefined enum and delegate operators, the only operators considered are those defined by an enum or delegate type that is the binding-time type of one of the operands.</span></span>
*  <span data-ttu-id="958cd-328">Aşırı yükleme çözünürlüğü kurallarına [aşırı yükleme çözünürlüğü](expressions.md#overload-resolution) bağımsız değişken listesi ile ilgili en iyi operatör seçmek için aday işleçleri kümesini uygulanan `(x,y)`, ve bu işleci aşırı yükleme sonucu haline gelir. Çözümleme işlemi.</span><span class="sxs-lookup"><span data-stu-id="958cd-328">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x,y)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="958cd-329">Tek bir en iyi operatör seçmek aşırı yükleme çözümlemesi başarısız olursa bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-329">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="candidate-user-defined-operators"></a><span data-ttu-id="958cd-330">Aday kullanıcı tanımlı işleçler</span><span class="sxs-lookup"><span data-stu-id="958cd-330">Candidate user-defined operators</span></span>

<span data-ttu-id="958cd-331">Belirli bir türe `T` ve bir işlem `operator op(A)`burada `op` aşırı yüklenebilir bir işleç ve `A` aday kümesini bir bağımsız değişken listesi tarafından sağlanan kullanıcı tanımlı işleçler olan `T` için `operator op(A)` belirlenir şu şekilde:</span><span class="sxs-lookup"><span data-stu-id="958cd-331">Given a type `T` and an operation `operator op(A)`, where `op` is an overloadable operator and `A` is an argument list, the set of candidate user-defined operators provided by `T` for `operator op(A)` is determined as follows:</span></span>

*  <span data-ttu-id="958cd-332">Tür belirleme `T0`.</span><span class="sxs-lookup"><span data-stu-id="958cd-332">Determine the type `T0`.</span></span> <span data-ttu-id="958cd-333">Varsa `T` boş değer atanabilir bir tür `T0` Aksi takdirde, temelindeki türe olan `T0` eşittir `T`.</span><span class="sxs-lookup"><span data-stu-id="958cd-333">If `T` is a nullable type, `T0` is its underlying type, otherwise `T0` is equal to `T`.</span></span>
*  <span data-ttu-id="958cd-334">Tüm `operator op` bildirimlerinde `T0` ve tüm yükseltilmiş böyle işleçlerinin forms en az bir işleç uygulanabilir olduğunda ([geçerli işlev üyesi](expressions.md#applicable-function-member)) bağımsız değişken listesine göre `A`, ardından kümesi Aday işleçler gibi ilgili tüm işleçler oluşur `T0`.</span><span class="sxs-lookup"><span data-stu-id="958cd-334">For all `operator op` declarations in `T0` and all lifted forms of such operators, if at least one operator is applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the argument list `A`, then the set of candidate operators consists of all such applicable operators in `T0`.</span></span>
*  <span data-ttu-id="958cd-335">Aksi takdirde `T0` olduğu `object`, aday işleçleri kümesini boştur.</span><span class="sxs-lookup"><span data-stu-id="958cd-335">Otherwise, if `T0` is `object`, the set of candidate operators is empty.</span></span>
*  <span data-ttu-id="958cd-336">Aksi takdirde, aday işleçleri kümesini sağlayan `T0` doğrudan taban sınıfı tarafından sağlanan aday işleçleri kümesi `T0`, veya etkili temel sınıfını `T0` varsa `T0` parametre türüdür.</span><span class="sxs-lookup"><span data-stu-id="958cd-336">Otherwise, the set of candidate operators provided by `T0` is the set of candidate operators provided by the direct base class of `T0`, or the effective base class of `T0` if `T0` is a type parameter.</span></span>

### <a name="numeric-promotions"></a><span data-ttu-id="958cd-337">Sayısal promosyonlar</span><span class="sxs-lookup"><span data-stu-id="958cd-337">Numeric promotions</span></span>

<span data-ttu-id="958cd-338">Sayısal promosyon, belirli örtük dönüştürmeleri önceden tanımlı tekli veya ikili sayısal işleçler işlenenlerin gerçekleştirme otomatik olarak oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-338">Numeric promotion consists of automatically performing certain implicit conversions of the operands of the predefined unary and binary numeric operators.</span></span> <span data-ttu-id="958cd-339">Sayısal yükseltme ayrı bir mekanizma, ancak önceden tanımlı operatörler için aşırı yükleme çözünürlüğü uygulanması yerine bir etkin değil.</span><span class="sxs-lookup"><span data-stu-id="958cd-339">Numeric promotion is not a distinct mechanism, but rather an effect of applying overload resolution to the predefined operators.</span></span> <span data-ttu-id="958cd-340">Kullanıcı tanımlı işleçler benzer etkileri göstermesi için uygulanabilir ancak sayısal yükseltme değerlendirmesi kullanıcı tanımlı işleçler, özellikle etkilemez.</span><span class="sxs-lookup"><span data-stu-id="958cd-340">Numeric promotion specifically does not affect evaluation of user-defined operators, although user-defined operators can be implemented to exhibit similar effects.</span></span>

<span data-ttu-id="958cd-341">Sayısal yükseltme örnek olarak, önceden tanımlanmış uygulamalar ikili göz önünde bulundurun `*` işleci:</span><span class="sxs-lookup"><span data-stu-id="958cd-341">As an example of numeric promotion, consider the predefined implementations of the binary `*` operator:</span></span>

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

<span data-ttu-id="958cd-342">Ne zaman çözümleme kurallarını aşırı yükleme ([aşırı yükleme çözünürlüğü](expressions.md#overload-resolution)) bu kümesine uygulanan operatörleri, işlenen türleri için örtük dönüştürmelerin mevcut işleçlerinin ilk seçmek için etkisidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-342">When overload resolution rules ([Overload resolution](expressions.md#overload-resolution)) are applied to this set of operators, the effect is to select the first of the operators for which implicit conversions exist from the operand types.</span></span> <span data-ttu-id="958cd-343">Örneğin, işlemi için `b * s`burada `b` olduğu bir `byte` ve `s` olduğu bir `short`, aşırı yükleme çözümlemesi seçer `operator *(int,int)` en iyi işleci olarak.</span><span class="sxs-lookup"><span data-stu-id="958cd-343">For example, for the operation `b * s`, where `b` is a `byte` and `s` is a `short`, overload resolution selects `operator *(int,int)` as the best operator.</span></span> <span data-ttu-id="958cd-344">Bu nedenle, etkin olan `b` ve `s` dönüştürülür `int`, ve sonuç türü `int`.</span><span class="sxs-lookup"><span data-stu-id="958cd-344">Thus, the effect is that `b` and `s` are converted to `int`, and the type of the result is `int`.</span></span> <span data-ttu-id="958cd-345">Benzer şekilde, işlem için `i * d`burada `i` olduğu bir `int` ve `d` olduğu bir `double`, aşırı yükleme çözümlemesi seçer `operator *(double,double)` en iyi işleci olarak.</span><span class="sxs-lookup"><span data-stu-id="958cd-345">Likewise, for the operation `i * d`, where `i` is an `int` and `d` is a `double`, overload resolution selects `operator *(double,double)` as the best operator.</span></span>

#### <a name="unary-numeric-promotions"></a><span data-ttu-id="958cd-346">Birli sayısal promosyonlar</span><span class="sxs-lookup"><span data-stu-id="958cd-346">Unary numeric promotions</span></span>

<span data-ttu-id="958cd-347">Önceden tanımlanmış işlenenler için sayısal yükseltmesi Unary gerçekleşir `+`, `-`, ve `~` birli işleçler.</span><span class="sxs-lookup"><span data-stu-id="958cd-347">Unary numeric promotion occurs for the operands of the predefined `+`, `-`, and `~` unary operators.</span></span> <span data-ttu-id="958cd-348">Yalnızca sayısal yükseltmesi Unary oluşur türündeki işlenenler dönüştürme `sbyte`, `byte`, `short`, `ushort`, veya `char` türüne `int`.</span><span class="sxs-lookup"><span data-stu-id="958cd-348">Unary numeric promotion simply consists of converting operands of type `sbyte`, `byte`, `short`, `ushort`, or `char` to type `int`.</span></span> <span data-ttu-id="958cd-349">Ayrıca, birli için `-` işleci, sayısal yükseltmesi unary dönüştürür türündeki işlenenler `uint` türüne `long`.</span><span class="sxs-lookup"><span data-stu-id="958cd-349">Additionally, for the unary `-` operator, unary numeric promotion converts operands of type `uint` to type `long`.</span></span>

#### <a name="binary-numeric-promotions"></a><span data-ttu-id="958cd-350">İkili sayısal promosyonlar</span><span class="sxs-lookup"><span data-stu-id="958cd-350">Binary numeric promotions</span></span>

<span data-ttu-id="958cd-351">Önceden tanımlanmış işlenenler için ikili sayısal yükseltme gerçekleşir `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, ve `<=` ikili işleçler.</span><span class="sxs-lookup"><span data-stu-id="958cd-351">Binary numeric promotion occurs for the operands of the predefined `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, and `<=` binary operators.</span></span> <span data-ttu-id="958cd-352">İkili sayısal yükseltme, her iki işlenen de ilişkisel olmayan işleçleri olması durumunda, ayrıca işlemin sonuç türü olur, ortak bir türe örtük olarak dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="958cd-352">Binary numeric promotion implicitly converts both operands to a common type which, in case of the non-relational operators, also becomes the result type of the operation.</span></span> <span data-ttu-id="958cd-353">Burada göründükleri sırayla aşağıdaki kuralları, uygulama ikili sayısal yükseltme oluşur:</span><span class="sxs-lookup"><span data-stu-id="958cd-353">Binary numeric promotion consists of applying the following rules, in the order they appear here:</span></span>

*  <span data-ttu-id="958cd-354">İki işlenenden türü ise `decimal`, diğer işlenen türüne dönüştürülür `decimal`, veya bir bağlama hatası diğer işlenen türü ise oluşur `float` veya `double`.</span><span class="sxs-lookup"><span data-stu-id="958cd-354">If either operand is of type `decimal`, the other operand is converted to type `decimal`, or a binding-time error occurs if the other operand is of type `float` or `double`.</span></span>
*  <span data-ttu-id="958cd-355">Aksi takdirde iki işlenenden türü ise `double`, diğer işlenen türüne dönüştürülür `double`.</span><span class="sxs-lookup"><span data-stu-id="958cd-355">Otherwise, if either operand is of type `double`, the other operand is converted to type `double`.</span></span>
*  <span data-ttu-id="958cd-356">Aksi takdirde iki işlenenden türü ise `float`, diğer işlenen türüne dönüştürülür `float`.</span><span class="sxs-lookup"><span data-stu-id="958cd-356">Otherwise, if either operand is of type `float`, the other operand is converted to type `float`.</span></span>
*  <span data-ttu-id="958cd-357">Aksi takdirde iki işlenenden türü ise `ulong`, diğer işlenen türüne dönüştürülür `ulong`, veya bir bağlama hatası diğer işlenen türü ise oluşur `sbyte`, `short`, `int`, veya `long`.</span><span class="sxs-lookup"><span data-stu-id="958cd-357">Otherwise, if either operand is of type `ulong`, the other operand is converted to type `ulong`, or a binding-time error occurs if the other operand is of type `sbyte`, `short`, `int`, or `long`.</span></span>
*  <span data-ttu-id="958cd-358">Aksi takdirde iki işlenenden türü ise `long`, diğer işlenen türüne dönüştürülür `long`.</span><span class="sxs-lookup"><span data-stu-id="958cd-358">Otherwise, if either operand is of type `long`, the other operand is converted to type `long`.</span></span>
*  <span data-ttu-id="958cd-359">Aksi takdirde iki işlenenden türü ise `uint` ve diğer işlenen türünde `sbyte`, `short`, veya `int`, her iki işlenen de türüne dönüştürülür `long`.</span><span class="sxs-lookup"><span data-stu-id="958cd-359">Otherwise, if either operand is of type `uint` and the other operand is of type `sbyte`, `short`, or `int`, both operands are converted to type `long`.</span></span>
*  <span data-ttu-id="958cd-360">Aksi takdirde iki işlenenden türü ise `uint`, diğer işlenen türüne dönüştürülür `uint`.</span><span class="sxs-lookup"><span data-stu-id="958cd-360">Otherwise, if either operand is of type `uint`, the other operand is converted to type `uint`.</span></span>
*  <span data-ttu-id="958cd-361">Aksi takdirde, her iki işlenen de türüne dönüştürülür `int`.</span><span class="sxs-lookup"><span data-stu-id="958cd-361">Otherwise, both operands are converted to type `int`.</span></span>

<span data-ttu-id="958cd-362">İlk kural karıştırmak herhangi bir işlem izin vermiyor Not `decimal` tür `double` ve `float` türleri.</span><span class="sxs-lookup"><span data-stu-id="958cd-362">Note that the first rule disallows any operations that mix the `decimal` type with the `double` and `float` types.</span></span> <span data-ttu-id="958cd-363">Arasında örtük dönüştürme işlemi yok olgu gelen kuralını izler `decimal` türü ve `double` ve `float` türleri.</span><span class="sxs-lookup"><span data-stu-id="958cd-363">The rule follows from the fact that there are no implicit conversions between the `decimal` type and the `double` and `float` types.</span></span>

<span data-ttu-id="958cd-364">Ayrıca bir işlenen türünde olması mümkün olmadığını göz önünde bulundurun `ulong` imzalı bir tamsayı türünde olduğunda diğer işlenen.</span><span class="sxs-lookup"><span data-stu-id="958cd-364">Also note that it is not possible for an operand to be of type `ulong` when the other operand is of a signed integral type.</span></span> <span data-ttu-id="958cd-365">Neden hiçbir integral türü bulunmaktadır: tam aralığını temsil edebilir `ulong` işaretli integral türlerindeki yanı sıra.</span><span class="sxs-lookup"><span data-stu-id="958cd-365">The reason is that no integral type exists that can represent the full range of `ulong` as well as the signed integral types.</span></span>

<span data-ttu-id="958cd-366">Yukarıdaki durumların her ikisinde atama ifadesi açıkça bir işlenen diğer işlenen ile uyumlu bir türe dönüştürmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-366">In both of the above cases, a cast expression can be used to explicitly convert one operand to a type that is compatible with the other operand.</span></span>

<span data-ttu-id="958cd-367">Örnekte</span><span class="sxs-lookup"><span data-stu-id="958cd-367">In the example</span></span>
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
<span data-ttu-id="958cd-368">bağlama zamanı hatası oluşur bir `decimal` çarpılmasıyla edilemez bir `double`.</span><span class="sxs-lookup"><span data-stu-id="958cd-368">a binding-time error occurs because a `decimal` cannot be multiplied by a `double`.</span></span> <span data-ttu-id="958cd-369">İkinci işleneni açıkça dönüştürerek hata çözümlenene `decimal`gibi:</span><span class="sxs-lookup"><span data-stu-id="958cd-369">The error is resolved by explicitly converting the second operand to `decimal`, as follows:</span></span>

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a><span data-ttu-id="958cd-370">Yükseltilmiş işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-370">Lifted operators</span></span>

<span data-ttu-id="958cd-371">***İşleçler yükseltilmiş*** da bu türlerin boş değer atanabilir forms ile kullanılmak üzere atanamayan değer türleri üzerinde çalışması önceden tanımlanmış ve kullanıcı tanımlı işleçler izin verir.</span><span class="sxs-lookup"><span data-stu-id="958cd-371">***Lifted operators*** permit predefined and user-defined operators that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="958cd-372">Yükseltilmiş işleçleri, aşağıda açıklandığı gibi belirli gereksinimleri karşılaması önceden tanımlanmış ve kullanıcı tanımlı işleçler oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="958cd-372">Lifted operators are constructed from predefined and user-defined operators that meet certain requirements, as described in the following:</span></span>

*   <span data-ttu-id="958cd-373">Birli işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-373">For the unary operators</span></span>

    ```csharp
    +  ++  -  --  !  ~
    ```

    <span data-ttu-id="958cd-374">Her iki değer atanamayan değer türleri işlenen ve sonuç türleri olması durumunda lifted form bir işleç var.</span><span class="sxs-lookup"><span data-stu-id="958cd-374">a lifted form of an operator exists if the operand and result types are both non-nullable value types.</span></span> <span data-ttu-id="958cd-375">Tek bir ekleyerek lifted formun oluşturulan `?` değiştiricisi işlenen ve sonuç türleri için.</span><span class="sxs-lookup"><span data-stu-id="958cd-375">The lifted form is constructed by adding a single `?` modifier to the operand and result types.</span></span> <span data-ttu-id="958cd-376">İşlenen null ise lifted işlecini bir null değer oluşturur.</span><span class="sxs-lookup"><span data-stu-id="958cd-376">The lifted operator produces a null value if the operand is null.</span></span> <span data-ttu-id="958cd-377">Aksi takdirde lifted işleci, işlenen kaydırmayacağını, temel alınan işlecini uygular ve sonucu sarmalar.</span><span class="sxs-lookup"><span data-stu-id="958cd-377">Otherwise, the lifted operator unwraps the operand, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="958cd-378">İkili işleçler için</span><span class="sxs-lookup"><span data-stu-id="958cd-378">For the binary operators</span></span>

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    <span data-ttu-id="958cd-379">Tüm NULL olmayan değer türleri işlenen ve sonuç türleri olması durumunda lifted form bir işleç var.</span><span class="sxs-lookup"><span data-stu-id="958cd-379">a lifted form of an operator exists if the operand and result types are all non-nullable value types.</span></span> <span data-ttu-id="958cd-380">Tek bir ekleyerek lifted formun oluşturulan `?` değiştiricisi her işlenen ve sonuç türü.</span><span class="sxs-lookup"><span data-stu-id="958cd-380">The lifted form is constructed by adding a single `?` modifier to each operand and result type.</span></span> <span data-ttu-id="958cd-381">Varsa lifted işleci bir null değer üreten veya her iki işlenen de null (bir özel durum olan `&` ve `|` operatörleri `bool?` açıklandığı yazın [Boolean mantıksal işleçler](expressions.md#boolean-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="958cd-381">The lifted operator produces a null value if one or both operands are null (an exception being the `&` and `|` operators of the `bool?` type, as described in [Boolean logical operators](expressions.md#boolean-logical-operators)).</span></span> <span data-ttu-id="958cd-382">Aksi takdirde, lifted işleci işlenenler kaydırmayacağını, temel alınan işleci uygulanır ve sonuç sarmalar.</span><span class="sxs-lookup"><span data-stu-id="958cd-382">Otherwise, the lifted operator unwraps the operands, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="958cd-383">Eşitlik işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-383">For the equality operators</span></span>

    ```csharp
    ==  !=
    ```

    <span data-ttu-id="958cd-384">İşlenen türleri atanamayan değer türleri ve sonuç türü olup olmadığını ise lifted form bir işleç var. `bool`.</span><span class="sxs-lookup"><span data-stu-id="958cd-384">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="958cd-385">Tek bir ekleyerek lifted formun oluşturulan `?` değiştiricisi her işlenen türü.</span><span class="sxs-lookup"><span data-stu-id="958cd-385">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="958cd-386">Lifted işleci iki null değeri eşit ve null değeri eşit olmayan herhangi bir null olmayan değer için değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="958cd-386">The lifted operator considers two null values equal, and a null value unequal to any non-null value.</span></span> <span data-ttu-id="958cd-387">Her iki işlenen de null olmayan, lifted işleci işlenenler sarmalanmış olmaktan çıkaran ve üretmek için temel alınan işleci geçerlidir `bool` sonucu.</span><span class="sxs-lookup"><span data-stu-id="958cd-387">If both operands are non-null, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

*   <span data-ttu-id="958cd-388">İlişkisel işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-388">For the relational operators</span></span>

    ```csharp
    <  >  <=  >=
    ```

    <span data-ttu-id="958cd-389">İşlenen türleri atanamayan değer türleri ve sonuç türü olup olmadığını ise lifted form bir işleç var. `bool`.</span><span class="sxs-lookup"><span data-stu-id="958cd-389">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="958cd-390">Tek bir ekleyerek lifted formun oluşturulan `?` değiştiricisi her işlenen türü.</span><span class="sxs-lookup"><span data-stu-id="958cd-390">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="958cd-391">Değer lifted işleci üretir `false` bir veya iki işlenenin null olduğunda.</span><span class="sxs-lookup"><span data-stu-id="958cd-391">The lifted operator produces the value `false` if one or both operands are null.</span></span> <span data-ttu-id="958cd-392">Aksi takdirde lifted işleci işlenenler sarmalanmış olmaktan çıkaran ve oluşturmak için temel alınan işleci geçerlidir `bool` sonucu.</span><span class="sxs-lookup"><span data-stu-id="958cd-392">Otherwise, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

## <a name="member-lookup"></a><span data-ttu-id="958cd-393">Üye araması</span><span class="sxs-lookup"><span data-stu-id="958cd-393">Member lookup</span></span>

<span data-ttu-id="958cd-394">Üye araması anlamı bir türün bağlamı içindeki bir adın yapabildiği belirlenir işlemidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-394">A member lookup is the process whereby the meaning of a name in the context of a type is determined.</span></span> <span data-ttu-id="958cd-395">Değerlendirme bir parçası olarak bir üye araması oluşabilir bir *simple_name* ([basit adları](expressions.md#simple-names)) veya bir *member_access* ([üye erişimi](expressions.md#member-access)) içinde bir ifade.</span><span class="sxs-lookup"><span data-stu-id="958cd-395">A member lookup can occur as part of evaluating a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)) in an expression.</span></span> <span data-ttu-id="958cd-396">Varsa *simple_name* veya *member_access* olarak gerçekleşir *primary_expression* , bir *invocation_expression* ([ Yöntem çağrıları](expressions.md#method-invocations)), üye çağrılmasına kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-396">If the *simple_name* or *member_access* occurs as the *primary_expression* of an *invocation_expression* ([Method invocations](expressions.md#method-invocations)), the member is said to be invoked.</span></span>

<span data-ttu-id="958cd-397">Üye bir yöntem veya olay ise ya da bir sabit, alan veya özellik bir temsilci türü ise ([Temsilciler](delegates.md)) veya türü `dynamic` ([dinamik tür](types.md#the-dynamic-type)), üye olduğusöylenirsonra*çağrılmayan*.</span><span class="sxs-lookup"><span data-stu-id="958cd-397">If a member is a method or event, or if it is a constant, field or property of either a delegate type ([Delegates](delegates.md)) or the type `dynamic` ([The dynamic type](types.md#the-dynamic-type)), then the member is said to be *invocable*.</span></span>

<span data-ttu-id="958cd-398">Üye araması üyesi aynı zamanda üyesinin tür parametreleri ve üye erişilebilir olup yalnızca adını göz önünde bulundurur.</span><span class="sxs-lookup"><span data-stu-id="958cd-398">Member lookup considers not only the name of a member but also the number of type parameters the member has and whether the member is accessible.</span></span> <span data-ttu-id="958cd-399">Üye araması amacıyla, genel yöntemleri ve iç içe geçmiş genel türler dizi ilgili bildirimleri belirtilen tür parametreleri ve diğer tüm üyeleri sıfır Tür parametrelerine sahip.</span><span class="sxs-lookup"><span data-stu-id="958cd-399">For the purposes of member lookup, generic methods and nested generic types have the number of type parameters indicated in their respective declarations and all other members have zero type parameters.</span></span>

<span data-ttu-id="958cd-400">Adı bir üye araması `N` ile `K` tür parametrelerindeki tür `T` şu şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="958cd-400">A member lookup of a name `N` with `K` type parameters in a type `T` is processed as follows:</span></span>

*  <span data-ttu-id="958cd-401">İlk olarak bir dizi adlı erişilebilir üyeler `N` belirlenir:</span><span class="sxs-lookup"><span data-stu-id="958cd-401">First, a set of accessible members named `N` is determined:</span></span>
    * <span data-ttu-id="958cd-402">Varsa `T` adlı erişilebilir üyeler kümesi birleşimi kümesi ise bir tür parametresi olduğundan `N` her bir birincil kısıtlaması veya ikincil bir kısıtlama olarak belirtilen türlerle ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) için `T`, adlı erişilebilir üyeler kümesini birlikte `N` içinde `object`.</span><span class="sxs-lookup"><span data-stu-id="958cd-402">If `T` is a type parameter, then the set is the union of the sets of accessible members named `N` in each of the types specified as a primary constraint or secondary constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) for `T`, along with the set of accessible members named `N` in `object`.</span></span>
    * <span data-ttu-id="958cd-403">Aksi takdirde, küme, tüm erişilebilir oluşur ([üye erişimi](basic-concepts.md#member-access)) adlı üye `N` içinde `T`devralınan üyeleri ve adlı erişilebilir Üyeler dahil olmak üzere `N` içinde `object`.</span><span class="sxs-lookup"><span data-stu-id="958cd-403">Otherwise, the set consists of all accessible ([Member access](basic-concepts.md#member-access)) members named `N` in `T`, including inherited members and the accessible members named `N` in `object`.</span></span> <span data-ttu-id="958cd-404">Varsa `T` bir oluşturulmuş tür olsa üyelerin kümesini tür bağımsız değişkenleri açıklanan şekilde değiştirerek elde edilen [oluşturulan türler üyeleri](classes.md#members-of-constructed-types).</span><span class="sxs-lookup"><span data-stu-id="958cd-404">If `T` is a constructed type, the set of members is obtained by substituting type arguments as described in [Members of constructed types](classes.md#members-of-constructed-types).</span></span> <span data-ttu-id="958cd-405">İçeren üye bir `override` değiştiricisi kümeden hariç tutulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-405">Members that include an `override` modifier are excluded from the set.</span></span>
*  <span data-ttu-id="958cd-406">Ardından, eğer `K` sıfırsa, türleri, bildirimleri içeren tür parametreleri kaldırıldığında tüm iç içe.</span><span class="sxs-lookup"><span data-stu-id="958cd-406">Next, if `K` is zero, all nested types whose declarations include type parameters are removed.</span></span> <span data-ttu-id="958cd-407">Varsa `K` sıfır olmayan tür parametreleri kaldırılır, farklı bir değere sahip tüm üyeleri olan.</span><span class="sxs-lookup"><span data-stu-id="958cd-407">If `K` is not zero, all members with a different number of type parameters are removed.</span></span> <span data-ttu-id="958cd-408">Dikkat edin `K` sıfır, yöntemleri sahip olmak parametreleri kaldırılmaz, bu yana tür çıkarımı işlem türü ([anlam çıkarma](expressions.md#type-inference)) tür bağımsız değişkenlerini çıkarsanacak mümkün olabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-408">Note that when `K` is zero, methods having type parameters are not removed, since the type inference process ([Type inference](expressions.md#type-inference)) might be able to infer the type arguments.</span></span>
*  <span data-ttu-id="958cd-409">Üye ise sonraki *çağrılan*, tüm olmayan-*çağrılmayan* üyeleri kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-409">Next, if the member is *invoked*, all non-*invocable* members are removed from the set.</span></span>
*  <span data-ttu-id="958cd-410">Ardından, diğer üyeleri tarafından gizlenen üyeleri kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-410">Next, members that are hidden by other members are removed from the set.</span></span> <span data-ttu-id="958cd-411">Her üye için `S.M` kümesindeki burada `S` tür, üye `M` bildirildiğinde, aşağıdaki kurallar uygulanır:</span><span class="sxs-lookup"><span data-stu-id="958cd-411">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied:</span></span>
    * <span data-ttu-id="958cd-412">Varsa `M` tüm üyelerini bir temel türde bildirilen sonra sabit, alan, özelliği, olay veya numaralandırma üyesi olduğu `S` kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-412">If `M` is a constant, field, property, event, or enumeration member, then all members declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="958cd-413">Varsa `M` bir tür bildiriminde kullanıldığında, ardından tüm olmayan bir temel türde bildirilen türleri olan `S` kümesinden kaldırılır ve tür parametreleri aynı sayıda bildirimlerle type all `M` bir temel türde bildirilen `S` kaldırılır kümesinden.</span><span class="sxs-lookup"><span data-stu-id="958cd-413">If `M` is a type declaration, then all non-types declared in a base type of `S` are removed from the set, and all type declarations with the same number of type parameters as `M` declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="958cd-414">Varsa `M` tüm yöntemi olmayan üyeleri bir temel türde bildirilen sonra bir yöntem olan `S` kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-414">If `M` is a method, then all non-method members declared in a base type of `S` are removed from the set.</span></span>
*  <span data-ttu-id="958cd-415">Ardından, sınıf üyeleri tarafından gizlenen bir arabirim üyeleri kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-415">Next, interface members that are hidden by class members are removed from the set.</span></span> <span data-ttu-id="958cd-416">Bu adım yalnızca, bir etkisi olmaz `T` parametre türüdür ve `T` hem etkin temel bir sınıf dışında olan `object` ve boş olmayan etkin arabirimi ayarlayın ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="958cd-416">This step only has an effect if `T` is a type parameter and `T` has both an effective base class other than `object` and a non-empty effective interface set ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="958cd-417">Her üye için `S.M` kümesindeki burada `S` hangi tür üyesi `M` bildirildiğinde, aşağıdaki kurallar uygulanır, `S` sınıf bildirimi dışında olan `object`:</span><span class="sxs-lookup"><span data-stu-id="958cd-417">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied if `S` is a class declaration other than `object`:</span></span>
    * <span data-ttu-id="958cd-418">Varsa `M` bir sabit, alan, özelliği, olay, sabit listesi üyesi veya türü bildirimi bir arabirim bildirimi içinde bildirilen tüm üyelerle kümesinden kaldırılır sonra olduğu.</span><span class="sxs-lookup"><span data-stu-id="958cd-418">If `M` is a constant, field, property, event, enumeration member, or type declaration, then all members declared in an interface declaration are removed from the set.</span></span>
    * <span data-ttu-id="958cd-419">Varsa `M` bir yöntem bir arabirim bildirimi içinde bildirilen tüm yöntemi olmayan üyeleri kümesi ve aynı imzaya sahip tüm yöntemleri kaldırılır sonra olduğu `M` bildirilen bir arabirimde bildirimi kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-419">If `M` is a method, then all non-method members declared in an interface declaration are removed from the set, and all methods with the same signature as `M` declared in an interface declaration are removed from the set.</span></span>
*  <span data-ttu-id="958cd-420">Son olarak, gizli üyeleri kaldırılmış arama sonucu belirlenir:</span><span class="sxs-lookup"><span data-stu-id="958cd-420">Finally, having removed hidden members, the result of the lookup is determined:</span></span>
    * <span data-ttu-id="958cd-421">Belirlenen bir metot değil tek bir üyesi oluşuyorsa, ardından bu arama sonucunu üyesidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-421">If the set consists of a single member that is not a method, then this member is the result of the lookup.</span></span>
    * <span data-ttu-id="958cd-422">Küme yalnızca yöntemleri içeriyorsa, aksi takdirde, ardından bu yöntemleri arama sonucu grubudur.</span><span class="sxs-lookup"><span data-stu-id="958cd-422">Otherwise, if the set contains only methods, then this group of methods is the result of the lookup.</span></span>
    * <span data-ttu-id="958cd-423">Aksi takdirde, belirsiz arama ve bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-423">Otherwise, the lookup is ambiguous, and a binding-time error occurs.</span></span>

<span data-ttu-id="958cd-424">Tür parametreleri ve arabirimler dışında türlerindeki üye aramalar ve kesinlikle tek devralımlı arabirimler üye arama için (devralma zincirini her arabirim tam olarak sıfır veya bir doğrudan temel arabirim varsa), arama kurallarını etkisi yalnızca, aynı ad veya imzaya sahip üyeler Gizle temel üyeler türetilmiş.</span><span class="sxs-lookup"><span data-stu-id="958cd-424">For member lookups in types other than type parameters and interfaces, and member lookups in interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effect of the lookup rules is simply that derived members hide base members with the same name or signature.</span></span> <span data-ttu-id="958cd-425">Bu tür tek devralımlı aramaları hiçbir zaman belirsiz olabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-425">Such single-inheritance lookups are never ambiguous.</span></span> <span data-ttu-id="958cd-426">Büyük olasılıkla birden çok devralma, arabirimler üye arama gelen ortaya çıkabilecek belirsizlikleri açıklanan [arabirim üye erişimi](interfaces.md#interface-member-access).</span><span class="sxs-lookup"><span data-stu-id="958cd-426">The ambiguities that can possibly arise from member lookups in multiple-inheritance interfaces are described in [Interface member access](interfaces.md#interface-member-access).</span></span>

### <a name="base-types"></a><span data-ttu-id="958cd-427">Taban türleri</span><span class="sxs-lookup"><span data-stu-id="958cd-427">Base types</span></span>

<span data-ttu-id="958cd-428">Bir tür üyesi arama amacıyla `T` aşağıdaki temel türleri olduğu kabul edilir:</span><span class="sxs-lookup"><span data-stu-id="958cd-428">For purposes of member lookup, a type `T` is considered to have the following base types:</span></span>

*  <span data-ttu-id="958cd-429">Varsa `T` olduğu `object`, ardından `T` temel tür yok.</span><span class="sxs-lookup"><span data-stu-id="958cd-429">If `T` is `object`, then `T` has no base type.</span></span>
*  <span data-ttu-id="958cd-430">Varsa `T` olduğu bir *enum_type*, temel türde `T` sınıf türleri `System.Enum`, `System.ValueType`, ve `object`.</span><span class="sxs-lookup"><span data-stu-id="958cd-430">If `T` is an *enum_type*, the base types of `T` are the class types `System.Enum`, `System.ValueType`, and `object`.</span></span>
*  <span data-ttu-id="958cd-431">Varsa `T` olduğu bir *struct_type*, temel türde `T` sınıf türleri `System.ValueType` ve `object`.</span><span class="sxs-lookup"><span data-stu-id="958cd-431">If `T` is a *struct_type*, the base types of `T` are the class types `System.ValueType` and `object`.</span></span>
*  <span data-ttu-id="958cd-432">Varsa `T` olduğu bir *class_type*, temel türde `T` temel sınıfları, `T`, sınıf türü de dahil olmak üzere `object`.</span><span class="sxs-lookup"><span data-stu-id="958cd-432">If `T` is a *class_type*, the base types of `T` are the base classes of `T`, including the class type `object`.</span></span>
*  <span data-ttu-id="958cd-433">Varsa `T` olduğu bir *INTERFACE_TYPE*, temel türde `T` temel arabirimleri olan `T` ve sınıf türüne `object`.</span><span class="sxs-lookup"><span data-stu-id="958cd-433">If `T` is an *interface_type*, the base types of `T` are the base interfaces of `T` and the class type `object`.</span></span>
*  <span data-ttu-id="958cd-434">Varsa `T` olduğu bir *array_type*, temel türde `T` sınıf türleri `System.Array` ve `object`.</span><span class="sxs-lookup"><span data-stu-id="958cd-434">If `T` is an *array_type*, the base types of `T` are the class types `System.Array` and `object`.</span></span>
*  <span data-ttu-id="958cd-435">Varsa `T` olduğu bir *delegate_type*, temel türde `T` sınıf türleri `System.Delegate` ve `object`.</span><span class="sxs-lookup"><span data-stu-id="958cd-435">If `T` is a *delegate_type*, the base types of `T` are the class types `System.Delegate` and `object`.</span></span>

## <a name="function-members"></a><span data-ttu-id="958cd-436">İşlev üyeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-436">Function members</span></span>

<span data-ttu-id="958cd-437">İşlev üyeleri yürütülebilir deyimleri içermesine üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="958cd-437">Function members are members that contain executable statements.</span></span> <span data-ttu-id="958cd-438">İşlev üyeleri her zaman türlerin üyelerini ve ad alanları üyeleri olamaz.</span><span class="sxs-lookup"><span data-stu-id="958cd-438">Function members are always members of types and cannot be members of namespaces.</span></span> <span data-ttu-id="958cd-439">C# işlev üyeleri aşağıdaki kategorileri tanımlar:</span><span class="sxs-lookup"><span data-stu-id="958cd-439">C# defines the following categories of function members:</span></span>

*  <span data-ttu-id="958cd-440">Yöntemler</span><span class="sxs-lookup"><span data-stu-id="958cd-440">Methods</span></span>
*  <span data-ttu-id="958cd-441">Özellikler</span><span class="sxs-lookup"><span data-stu-id="958cd-441">Properties</span></span>
*  <span data-ttu-id="958cd-442">Olaylar</span><span class="sxs-lookup"><span data-stu-id="958cd-442">Events</span></span>
*  <span data-ttu-id="958cd-443">Dizin Oluşturucular</span><span class="sxs-lookup"><span data-stu-id="958cd-443">Indexers</span></span>
*  <span data-ttu-id="958cd-444">Kullanıcı tanımlı işleçler</span><span class="sxs-lookup"><span data-stu-id="958cd-444">User-defined operators</span></span>
*  <span data-ttu-id="958cd-445">Örnek oluşturucuları</span><span class="sxs-lookup"><span data-stu-id="958cd-445">Instance constructors</span></span>
*  <span data-ttu-id="958cd-446">Statik oluşturucular</span><span class="sxs-lookup"><span data-stu-id="958cd-446">Static constructors</span></span>
*  <span data-ttu-id="958cd-447">Yıkıcılar</span><span class="sxs-lookup"><span data-stu-id="958cd-447">Destructors</span></span>

<span data-ttu-id="958cd-448">Yok ediciler ve (hangi açıkça çağrılamaz) statik oluşturucular dışında işlev üyeleri içinde yer alan ifadeleri işlev üyesi çağrılarını yürütülür.</span><span class="sxs-lookup"><span data-stu-id="958cd-448">Except for destructors and static constructors (which cannot be invoked explicitly), the statements contained in function members are executed through function member invocations.</span></span> <span data-ttu-id="958cd-449">Bir işlev üye çağrısı yazma olarak gerçek sözdizimini üzerinde belirli işlev üye kategorisine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-449">The actual syntax for writing a function member invocation depends on the particular function member category.</span></span>

<span data-ttu-id="958cd-450">Bağımsız değişken listesi ([bağımsız değişken listeleri](expressions.md#argument-lists)) işlevi üyesi çağırma gerçek değerleri veya değişken başvuruları işlevi üyesinin parametrelerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="958cd-450">The argument list ([Argument lists](expressions.md#argument-lists)) of a function member invocation provides actual values or variable references for the parameters of the function member.</span></span>

<span data-ttu-id="958cd-451">Genel yöntemler çağrıları tür bağımsız değişkenleri, yönteme kümesini belirlemek için tür çıkarımı uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-451">Invocations of generic methods may employ type inference to determine the set of type arguments to pass to the method.</span></span> <span data-ttu-id="958cd-452">Bu işlem açıklanan [anlam çıkarma](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="958cd-452">This process is described in [Type inference](expressions.md#type-inference).</span></span>

<span data-ttu-id="958cd-453">Yöntemleri, Dizinleyicileri, işleçleri ve örnek oluşturucuları çağrılarını aşırı yükleme çözünürlüğü, aday birtakım çağrılacak işlev üyeleri belirlemek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="958cd-453">Invocations of methods, indexers, operators and instance constructors employ overload resolution to determine which of a candidate set of function members to invoke.</span></span> <span data-ttu-id="958cd-454">Bu işlem açıklanan [aşırı yükleme çözünürlüğü](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="958cd-454">This process is described in [Overload resolution](expressions.md#overload-resolution).</span></span>

<span data-ttu-id="958cd-455">Belirli işlev üyesi bağlama zamanında belirlendikten sonra büyük olasılıkla aşırı yükleme çözünürlüğü işlevi üye çağrılırken gerçek çalışma zamanı açıklanan işlemidir [derleme zamanı dinamik aşırı yükleme çözünürlüğüdenetleniyor](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="958cd-455">Once a particular function member has been identified at binding-time, possibly through overload resolution, the actual run-time process of invoking the function member is described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="958cd-456">Açıkça çağrılan işlev üyeleri altı kategorileri içeren yapıları yerinde alan işleme aşağıdaki tabloda özetlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="958cd-456">The following table summarizes the processing that takes place in constructs involving the six categories of function members that can be explicitly invoked.</span></span> <span data-ttu-id="958cd-457">Tabloda `e`, `x`, `y`, ve `value` değişkenler veya değerler olarak sınıflandırılan ifadeleri belirtmek `T` bir tür olarak sınıflandırılmış bir ifadeyi gösterir `F` basit bir yöntem ve adıdır`P` basit bir özelliğin adıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-457">In the table, `e`, `x`, `y`, and `value` indicate expressions classified as variables or values, `T` indicates an expression classified as a type, `F` is the simple name of a method, and `P` is the simple name of a property.</span></span>


| <span data-ttu-id="958cd-458">__Yapısı__</span><span class="sxs-lookup"><span data-stu-id="958cd-458">__Construct__</span></span>     | <span data-ttu-id="958cd-459">__Örnek__</span><span class="sxs-lookup"><span data-stu-id="958cd-459">__Example__</span></span>    | <span data-ttu-id="958cd-460">__Açıklama__</span><span class="sxs-lookup"><span data-stu-id="958cd-460">__Description__</span></span> |
|-------------------|----------------|-----------------|
| <span data-ttu-id="958cd-461">Yöntem çağırma</span><span class="sxs-lookup"><span data-stu-id="958cd-461">Method invocation</span></span> | `F(x,y)`       | <span data-ttu-id="958cd-462">Aşırı yükleme çözünürlüğü uygulanan en iyi yöntem seçmek için `F` içeren sınıf veya yapı içinde.</span><span class="sxs-lookup"><span data-stu-id="958cd-462">Overload resolution is applied to select the best method `F` in the containing class or struct.</span></span> <span data-ttu-id="958cd-463">Yöntem bağımsız değişken listesi ile çağrılan `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-463">The method is invoked with the argument list `(x,y)`.</span></span> <span data-ttu-id="958cd-464">Yöntem değilse `static`, örnek ifade `this`.</span><span class="sxs-lookup"><span data-stu-id="958cd-464">If the method is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.F(x,y)`     | <span data-ttu-id="958cd-465">Aşırı yükleme çözünürlüğü uygulanan en iyi yöntem seçmek için `F` sınıf veya yapı içinde `T`.</span><span class="sxs-lookup"><span data-stu-id="958cd-465">Overload resolution is applied to select the best method `F` in the class or struct `T`.</span></span> <span data-ttu-id="958cd-466">Yöntem değilse bir bağlama zamanı hatası oluşur `static`.</span><span class="sxs-lookup"><span data-stu-id="958cd-466">A binding-time error occurs if the method is not `static`.</span></span> <span data-ttu-id="958cd-467">Yöntem bağımsız değişken listesi ile çağrılan `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-467">The method is invoked with the argument list `(x,y)`.</span></span> | 
|                   | `e.F(x,y)`     | <span data-ttu-id="958cd-468">Aşırı yükleme çözünürlüğü uygulanan en iyi yöntem F sınıf, yapı veya arabirim türü tarafından verilen seçmek için `e`.</span><span class="sxs-lookup"><span data-stu-id="958cd-468">Overload resolution is applied to select the best method F in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="958cd-469">Yöntem ise bir bağlama zamanı hatası oluşur `static`.</span><span class="sxs-lookup"><span data-stu-id="958cd-469">A binding-time error occurs if the method is `static`.</span></span> <span data-ttu-id="958cd-470">Örnek ifade ile yöntemi çağrılır `e` ve bağımsız değişken listesi `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-470">The method is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="958cd-471">Özellik erişimi</span><span class="sxs-lookup"><span data-stu-id="958cd-471">Property access</span></span>   | `P`            | <span data-ttu-id="958cd-472">`get` Erişimci özelliğin `P` kapsayan sınıfın veya yapının çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-472">The `get` accessor of the property `P` in the containing class or struct is invoked.</span></span> <span data-ttu-id="958cd-473">Bir derleme zamanı hatası oluşur `P` salt yazılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-473">A compile-time error occurs if `P` is write-only.</span></span> <span data-ttu-id="958cd-474">Varsa `P` değil `static`, örnek ifade `this`.</span><span class="sxs-lookup"><span data-stu-id="958cd-474">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `P = value`    | <span data-ttu-id="958cd-475">`set` Erişimci özelliğin `P` içeren sınıf veya yapı bağımsız değişken listesiyle çağrılır `(value)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-475">The `set` accessor of the property `P` in the containing class or struct is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="958cd-476">Bir derleme zamanı hatası oluşur `P` salt okunur.</span><span class="sxs-lookup"><span data-stu-id="958cd-476">A compile-time error occurs if `P` is read-only.</span></span> <span data-ttu-id="958cd-477">Varsa `P` değil `static`, örnek ifade `this`.</span><span class="sxs-lookup"><span data-stu-id="958cd-477">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.P`          | <span data-ttu-id="958cd-478">`get` Erişimci özelliğin `P` sınıf veya yapı içinde `T` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-478">The `get` accessor of the property `P` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="958cd-479">Bir derleme zamanı hatası oluşur `P` değil `static` veya `P` salt yazılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-479">A compile-time error occurs if `P` is not `static` or if `P` is write-only.</span></span> | 
|                   | `T.P = value`  | <span data-ttu-id="958cd-480">`set` Erişimci özelliğin `P` sınıf veya yapı içinde `T` bağımsız değişken listesi ile çağrılan `(value)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-480">The `set` accessor of the property `P` in the class or struct `T` is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="958cd-481">Bir derleme zamanı hatası oluşur `P` değil `static` veya `P` salt okunur.</span><span class="sxs-lookup"><span data-stu-id="958cd-481">A compile-time error occurs if `P` is not `static` or if `P` is read-only.</span></span> | 
|                   | `e.P`          | <span data-ttu-id="958cd-482">`get` Erişimci özelliğin `P` içinde sınıf, yapı veya arabirim türü tarafından verilen `e` örnek ifade ile çağrılan `e`.</span><span class="sxs-lookup"><span data-stu-id="958cd-482">The `get` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="958cd-483">Bir bağlama zamanı hatası oluşur `P` olduğu `static` veya `P` salt yazılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-483">A binding-time error occurs if `P` is `static` or if `P` is write-only.</span></span> | 
|                   | `e.P = value`  | <span data-ttu-id="958cd-484">`set` Erişimci özelliğin `P` içinde sınıf, yapı veya arabirim türü tarafından verilen `e` örnek ifade ile çağrılan `e` ve bağımsız değişken listesi `(value)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-484">The `set` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e` and the argument list `(value)`.</span></span> <span data-ttu-id="958cd-485">Bir bağlama zamanı hatası oluşur `P` olduğu `static` veya `P` salt okunur.</span><span class="sxs-lookup"><span data-stu-id="958cd-485">A binding-time error occurs if `P` is `static` or if `P` is read-only.</span></span> | 
| <span data-ttu-id="958cd-486">Olay erişimini</span><span class="sxs-lookup"><span data-stu-id="958cd-486">Event access</span></span>      | `E += value`   | <span data-ttu-id="958cd-487">`add` Olay erişimcisi `E` kapsayan sınıfın veya yapının çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-487">The `add` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="958cd-488">Varsa `E` olan statik, örnek ifadesidir `this`.</span><span class="sxs-lookup"><span data-stu-id="958cd-488">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `E -= value`   | <span data-ttu-id="958cd-489">`remove` Olay erişimcisi `E` kapsayan sınıfın veya yapının çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-489">The `remove` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="958cd-490">Varsa `E` olan statik, örnek ifadesidir `this`.</span><span class="sxs-lookup"><span data-stu-id="958cd-490">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `T.E += value` | <span data-ttu-id="958cd-491">`add` Olay erişimcisi `E` sınıf veya yapı içinde `T` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-491">The `add` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="958cd-492">Bir bağlama zamanı hatası oluşur `E` statik değil.</span><span class="sxs-lookup"><span data-stu-id="958cd-492">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `T.E -= value` | <span data-ttu-id="958cd-493">`remove` Olay erişimcisi `E` sınıf veya yapı içinde `T` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-493">The `remove` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="958cd-494">Bir bağlama zamanı hatası oluşur `E` statik değil.</span><span class="sxs-lookup"><span data-stu-id="958cd-494">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `e.E += value` | <span data-ttu-id="958cd-495">`add` Olay erişimcisi `E` içinde sınıf, yapı veya arabirim türü tarafından verilen `e` örnek ifade ile çağrılan `e`.</span><span class="sxs-lookup"><span data-stu-id="958cd-495">The `add` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="958cd-496">Bir bağlama zamanı hatası oluşur `E` statiktir.</span><span class="sxs-lookup"><span data-stu-id="958cd-496">A binding-time error occurs if `E` is static.</span></span> | 
|                   | `e.E -= value` | <span data-ttu-id="958cd-497">`remove` Olay erişimcisi `E` içinde sınıf, yapı veya arabirim türü tarafından verilen `e` örnek ifade ile çağrılan `e`.</span><span class="sxs-lookup"><span data-stu-id="958cd-497">The `remove` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="958cd-498">Bir bağlama zamanı hatası oluşur `E` statiktir.</span><span class="sxs-lookup"><span data-stu-id="958cd-498">A binding-time error occurs if `E` is static.</span></span> | 
| <span data-ttu-id="958cd-499">Dizinleyici erişimi</span><span class="sxs-lookup"><span data-stu-id="958cd-499">Indexer access</span></span>    | `e[x,y]`       | <span data-ttu-id="958cd-500">Aşırı yükleme çözünürlüğü, dizin oluşturucunun en iyi sınıf, yapı veya arabirim e türü tarafından verilen seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-500">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of e.</span></span> <span data-ttu-id="958cd-501">`get` Erişimci Indexer'ın örnek ifade ile çağrılan `e` ve bağımsız değişken listesi `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-501">The `get` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> <span data-ttu-id="958cd-502">Dizin Oluşturucu salt yazılır ise bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-502">A binding-time error occurs if the indexer is write-only.</span></span> | 
|                   | `e[x,y] = value` | <span data-ttu-id="958cd-503">Aşırı yükleme çözünürlüğü uygulanan en iyi dizin oluşturucu sınıf, yapı veya arabirim türü tarafından verilen seçmek için `e`.</span><span class="sxs-lookup"><span data-stu-id="958cd-503">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="958cd-504">`set` Erişimci Indexer'ın örnek ifade ile çağrılan `e` ve bağımsız değişken listesi `(x,y,value)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-504">The `set` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y,value)`.</span></span> <span data-ttu-id="958cd-505">Dizin Oluşturucu salt okunur ise bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-505">A binding-time error occurs if the indexer is read-only.</span></span> | 
| <span data-ttu-id="958cd-506">İşleç çağırma</span><span class="sxs-lookup"><span data-stu-id="958cd-506">Operator invocation</span></span> | `-x`         | <span data-ttu-id="958cd-507">Aşırı yükleme çözünürlüğü uygulanan en iyi birli işleç sınıf veya yapı türü tarafından verilen seçmek için `x`.</span><span class="sxs-lookup"><span data-stu-id="958cd-507">Overload resolution is applied to select the best unary operator in the class or struct given by the type of `x`.</span></span> <span data-ttu-id="958cd-508">Seçili işleç bağımsız değişken listesi ile çağrılan `(x)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-508">The selected operator is invoked with the argument list `(x)`.</span></span> | 
|                     | `x + y`      | <span data-ttu-id="958cd-509">Aşırı yükleme çözünürlüğü sınıflar veya yapılar türleri tarafından verilen iyi ikili operatör seçmek için uygulanan `x` ve `y`.</span><span class="sxs-lookup"><span data-stu-id="958cd-509">Overload resolution is applied to select the best binary operator in the classes or structs given by the types of `x` and `y`.</span></span> <span data-ttu-id="958cd-510">Seçili işleç bağımsız değişken listesi ile çağrılan `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-510">The selected operator is invoked with the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="958cd-511">Örnek oluşturucusu çağırma</span><span class="sxs-lookup"><span data-stu-id="958cd-511">Instance constructor invocation</span></span> | `new T(x,y)` | <span data-ttu-id="958cd-512">Sınıf veya yapı en iyi örnek oluşturucusu seçmek için aşırı yükleme çözünürlüğü uygulanan `T`.</span><span class="sxs-lookup"><span data-stu-id="958cd-512">Overload resolution is applied to select the best instance constructor in the class or struct `T`.</span></span> <span data-ttu-id="958cd-513">Örnek oluşturucusu bağımsız değişken listesi ile çağrılan `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-513">The instance constructor is invoked with the argument list `(x,y)`.</span></span> | 

### <a name="argument-lists"></a><span data-ttu-id="958cd-514">Bağımsız değişken listeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-514">Argument lists</span></span>

<span data-ttu-id="958cd-515">Her işlev üyesi ve temsilci çağırma, gerçek değerleri veya değişken başvuruları işlevi üyesinin parametrelerini sağlayan bir bağımsız değişken listesi içerir.</span><span class="sxs-lookup"><span data-stu-id="958cd-515">Every function member and delegate invocation includes an argument list which provides actual values or variable references for the parameters of the function member.</span></span> <span data-ttu-id="958cd-516">Bir işlev üye çağrısının bağımsız değişken listesi belirtmek için sözdizimi işlevi üye kategorisine göre bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="958cd-516">The syntax for specifying the argument list of a function member invocation depends on the function member category:</span></span>

*  <span data-ttu-id="958cd-517">Örneğin Oluşturucular, yöntemler, dizin oluşturucular ve Temsilciler, bağımsız değişkenler olarak belirtilen bir *argument_list*, aşağıda açıklandığı gibi.</span><span class="sxs-lookup"><span data-stu-id="958cd-517">For instance constructors, methods, indexers and delegates, the arguments are specified as an *argument_list*, as described below.</span></span> <span data-ttu-id="958cd-518">Çağrılırken, dizin oluşturucular için `set` erişimci bağımsız değişken listesi ayrıca atama işlecinin sağ işleneni belirtilen ifade içerir.</span><span class="sxs-lookup"><span data-stu-id="958cd-518">For indexers, when invoking the `set` accessor, the argument list additionally includes the expression specified as the right operand of the assignment operator.</span></span>
*  <span data-ttu-id="958cd-519">Özellikler için bağımsız değişken listesi çağrılırken boştur `get` erişimci ve çağrılırken atama işlecinin sağ işleneni belirtilen ifade oluşur `set` erişimcisi.</span><span class="sxs-lookup"><span data-stu-id="958cd-519">For properties, the argument list is empty when invoking the `get` accessor, and consists of the expression specified as the right operand of the assignment operator when invoking the `set` accessor.</span></span>
*  <span data-ttu-id="958cd-520">Olaylar için sağ işleneni belirtilen ifade bağımsız değişken listesi oluşan `+=` veya `-=` işleci.</span><span class="sxs-lookup"><span data-stu-id="958cd-520">For events, the argument list consists of the expression specified as the right operand of the `+=` or `-=` operator.</span></span>
*  <span data-ttu-id="958cd-521">Kullanıcı tanımlı işleçler için bağımsız değişken listesi tek işlecinin işleneni, tekli veya ikili işleci iki işlenenleri oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-521">For user-defined operators, the argument list consists of the single operand of the unary operator or the two operands of the binary operator.</span></span>

<span data-ttu-id="958cd-522">Bağımsız değişken özelliklerinin ([özellikleri](classes.md#properties)), olaylar ([olayları](classes.md#events)) ve kullanıcı tanımlı işleçler ([işleçleri](classes.md#operators)) her zaman değer parametre olarak geçirilen ([ Parametre değeri](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="958cd-522">The arguments of properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), and user-defined operators ([Operators](classes.md#operators)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)).</span></span> <span data-ttu-id="958cd-523">Dizin oluşturucular bağımsız değişkenleri ([dizin oluşturucular](classes.md#indexers)) her zaman değer parametre olarak geçirilen ([değer parametreleri](classes.md#value-parameters)) ya da parametre dizileri ([parametre dizileri](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="958cd-523">The arguments of indexers ([Indexers](classes.md#indexers)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)) or parameter arrays ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="958cd-524">Başvuru ve çıkış parametreleri, bu işlev üyeleri kategoriler için desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="958cd-524">Reference and output parameters are not supported for these categories of function members.</span></span>

<span data-ttu-id="958cd-525">Bir örnek oluşturucusu, yöntem, dizin oluşturucu veya temsilci çağırma bağımsız değişkenler olarak belirtilen bir *argument_list*:</span><span class="sxs-lookup"><span data-stu-id="958cd-525">The arguments of an instance constructor, method, indexer or delegate invocation are specified as an *argument_list*:</span></span>

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

<span data-ttu-id="958cd-526">Bir *argument_list* bir veya daha fazla oluşur *bağımsız değişken*virgülle ayırarak, s.</span><span class="sxs-lookup"><span data-stu-id="958cd-526">An *argument_list* consists of one or more *argument*s, separated by commas.</span></span> <span data-ttu-id="958cd-527">Her bağımsız değişken isteğe oluşur *argument_name* arkasından bir *argument_value*.</span><span class="sxs-lookup"><span data-stu-id="958cd-527">Each argument consists of an optional  *argument_name* followed by an *argument_value*.</span></span> <span data-ttu-id="958cd-528">Bir *bağımsız değişken* ile bir *argument_name* şeklinde adlandırılan bir ***adlandırılmış bağımsız değişkeni***bilgileriyse bir *bağımsız değişken* olmadan bir  *argument_name* olduğu bir ***konumsal bağımsız değişkeni***.</span><span class="sxs-lookup"><span data-stu-id="958cd-528">An *argument* with an *argument_name* is referred to as a ***named argument***, whereas an *argument* without an *argument_name* is a ***positional argument***.</span></span> <span data-ttu-id="958cd-529">Adlandırılmış bir bağımsız değişken görüntülenmesini konumsal bağımsız değişken için bir hata olduğu bir *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="958cd-529">It is an error for a positional argument to appear after a named argument in an *argument_list*.</span></span>

<span data-ttu-id="958cd-530">*Argument_value* aşağıdaki biçimlerden birini alabilir:</span><span class="sxs-lookup"><span data-stu-id="958cd-530">The *argument_value* can take one of the following forms:</span></span>

*  <span data-ttu-id="958cd-531">Bir *ifade*, gösteren bir değer parametre olarak geçirilen bağımsız değişken ([değer parametreleri](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="958cd-531">An *expression*, indicating that the argument is passed as a value parameter ([Value parameters](classes.md#value-parameters)).</span></span>
*  <span data-ttu-id="958cd-532">Anahtar sözcüğü `ref` arkasından bir *variable_reference* ([değişken başvuruları](variables.md#variable-references)), bir başvuru parametresi geçirilen bağımsız değişken belirten ([başvuru parametreleri ](classes.md#reference-parameters)).</span><span class="sxs-lookup"><span data-stu-id="958cd-532">The keyword `ref` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as a reference parameter ([Reference parameters](classes.md#reference-parameters)).</span></span> <span data-ttu-id="958cd-533">Bir değişken kesinlikle atanmalıdır ([belirli atama onayına](variables.md#definite-assignment)) önce bir başvuru parametresi geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-533">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter.</span></span> <span data-ttu-id="958cd-534">Anahtar sözcüğü `out` arkasından bir *variable_reference* ([değişken başvuruları](variables.md#variable-references)), bir output parametresi olarak geçirilen bağımsız değişken belirten ([çıkış parametresi](classes.md#output-parameters)).</span><span class="sxs-lookup"><span data-stu-id="958cd-534">The keyword `out` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as an output parameter ([Output parameters](classes.md#output-parameters)).</span></span> <span data-ttu-id="958cd-535">Bir değişken kesinlikle atanmış olarak kabul edilir ([belirli atama onayına](variables.md#definite-assignment)) izleyen bir işlev üye çağrısı değişkeni bir output parametresi olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-535">A variable is considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) following a function member invocation in which the variable is passed as an output parameter.</span></span>

#### <a name="corresponding-parameters"></a><span data-ttu-id="958cd-536">Karşılık gelen parametreleri</span><span class="sxs-lookup"><span data-stu-id="958cd-536">Corresponding parameters</span></span>

<span data-ttu-id="958cd-537">Bir bağımsız değişken listesindeki her bağımsız değişkeni var. sahip işlev üyesi veya çağrılan temsilci karşılık gelen bir parametre olarak.</span><span class="sxs-lookup"><span data-stu-id="958cd-537">For each argument in an argument list there has to be a corresponding parameter in the function member or delegate being invoked.</span></span>

<span data-ttu-id="958cd-538">Aşağıda kullanılan parametre listesi aşağıdaki gibi belirlenir:</span><span class="sxs-lookup"><span data-stu-id="958cd-538">The parameter list used in the following is determined as follows:</span></span>

*  <span data-ttu-id="958cd-539">Sanal yöntemler ve sınıflar tanımlanmış dizin oluşturuculara, parametre listesi en belirgin bildirimden seçilir ya işlevi üyesinin alıcısı statik türü ile başlayan ve temel sınıflarının aramayı geçersiz kılın.</span><span class="sxs-lookup"><span data-stu-id="958cd-539">For virtual methods and indexers defined in classes, the parameter list is picked from the most specific declaration or override of the function member, starting with the static type of the receiver, and searching through its base classes.</span></span>
*  <span data-ttu-id="958cd-540">Arabirim yöntemleri ve dizin oluşturucular için parametre listesi çekilir üyesinin, arabirim türü ile başlayan ve temel arabirimleri aramayı en belirgin tanımı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="958cd-540">For interface methods and indexers, the parameter list is picked form the most specific definition of the member, starting with the interface type and searching through the base interfaces.</span></span> <span data-ttu-id="958cd-541">Benzersiz bir parametre listesi yok bulunduğunda, erişilemez adlarını ve isteğe bağlı parametre içeren bir parametre listesi oluşturulur, çağrıları adlandırılmış parametreler kullanan veya isteğe bağlı bağımsız değişkenlere atlayın.</span><span class="sxs-lookup"><span data-stu-id="958cd-541">If no unique parameter list is found, a parameter list with inaccessible names and no optional parameters is constructed, so that invocations cannot use named parameters or omit optional arguments.</span></span>
*  <span data-ttu-id="958cd-542">Kısmi yöntemler için tanımlayan kısmi yöntem bildiriminde parametre listesi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-542">For partial methods, the parameter list of the defining partial method declaration is used.</span></span>
*  <span data-ttu-id="958cd-543">Diğer tüm işlev üyeleri ve temsilciler için kullanılanla olan yalnızca bir tek bir parametre listesi yoktur.</span><span class="sxs-lookup"><span data-stu-id="958cd-543">For all other function members and delegates there is only a single parameter list, which is the one used.</span></span>

<span data-ttu-id="958cd-544">Bir değişken veya parametre konumunu sayıda bağımsız değişken veya bağımsız değişken listesi veya parametre listesi önceki parametreleri olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-544">The position of an argument or parameter is defined as the number of arguments or parameters preceding it in the argument list or parameter list.</span></span>

<span data-ttu-id="958cd-545">İşlev üye bağımsız değişkenleri için karşılık gelen parametreleri şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="958cd-545">The corresponding parameters for function member arguments are established as follows:</span></span>

*  <span data-ttu-id="958cd-546">Bağımsız değişken *argument_list* örnek oluşturucuları, yöntemleri, Dizinleyicileri ve temsilciler:</span><span class="sxs-lookup"><span data-stu-id="958cd-546">Arguments in the *argument_list* of instance constructors, methods, indexers and delegates:</span></span>
    * <span data-ttu-id="958cd-547">Konumsal bağımsız değişken bir sabit parametre parametre listesindeki aynı konumda oluştuğu bu parametreye karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-547">A positional argument where a fixed parameter occurs at the same position in the parameter list corresponds to that parameter.</span></span>
    * <span data-ttu-id="958cd-548">Konumsal bağımsız değişken işlev üyesi çağrılır, normal bir biçimde bir parametre dizisi ile aynı konumda parametre listesinde gerçekleşmelidir parametre dizisine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-548">A positional argument of a function member with a parameter array invoked in its normal form corresponds to the parameter  array, which must occur at the same position in the parameter list.</span></span>
    * <span data-ttu-id="958cd-549">Konumsal bağımsız değişken bir sabit parametre parametre listesindeki aynı konumda gerçekleştiği genişletilmiş hâli içinde çağrılan bir parametre dizisi ile işlevi üyesinin parametre dizisi içindeki bir öğeye karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-549">A positional argument of a function member with a parameter array invoked in its expanded form, where no fixed parameter occurs at the same position in the parameter list, corresponds to an element in the parameter array.</span></span>
    * <span data-ttu-id="958cd-550">Adlandırılmış bir bağımsız değişken, parametre listesinde aynı ada sahip parametre karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-550">A named argument corresponds to the parameter of the same name in the parameter list.</span></span>
    * <span data-ttu-id="958cd-551">Çağrılırken, dizin oluşturucular için `set` erişimci, atama işlecinin sağ işleneninin karşılık gelen örtük olarak belirtilen ifade `value` parametresinin `set` erişimci bildirimi.</span><span class="sxs-lookup"><span data-stu-id="958cd-551">For indexers, when invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="958cd-552">Çağrılırken özellikleri `get` erişimci var olan herhangi bir bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="958cd-552">For properties, when invoking the `get` accessor there are no arguments.</span></span> <span data-ttu-id="958cd-553">Çağrılırken `set` erişimci, atama işlecinin sağ işleneninin karşılık gelen örtük olarak belirtilen ifade `value` parametresinin `set` erişimci bildirimi.</span><span class="sxs-lookup"><span data-stu-id="958cd-553">When invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="958cd-554">Kullanıcı tanımlı birli işleçler (dönüştürmeleri dahil) için tek bir işlenen işleç bildirimi, tek bir parametre karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-554">For user-defined unary operators (including conversions), the single operand corresponds to the single parameter of the operator declaration.</span></span>
*  <span data-ttu-id="958cd-555">Kullanıcı tanımlı ikili işleçler için ilk parametre olarak sol işlenen karşılık gelir ve sağ işlenen işleç bildirimi ikinci parametresi için karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-555">For user-defined binary operators, the left operand corresponds to the first parameter, and the right operand corresponds to the second parameter of the operator declaration.</span></span>

#### <a name="run-time-evaluation-of-argument-lists"></a><span data-ttu-id="958cd-556">Çalışma zamanı değerlendirmesini bağımsız değişken listeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-556">Run-time evaluation of argument lists</span></span>

<span data-ttu-id="958cd-557">Bir işlev üye çağrısı çalışma zamanı işlenmesi sırasında ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), ifadeler veya bağımsız değişken listesi değişken başvuruları sırayla soldan sağa doğru olarak değerlendirilir aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="958cd-557">During the run-time processing of a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), the expressions or variable references of an argument list are evaluated in order, from left to right, as follows:</span></span>

*  <span data-ttu-id="958cd-558">Bir değer parametresi için bağımsız değişken ifadesi değerlendirilir ve örtük bir dönüştürme ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) karşılık gelen parametre türü gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-558">For a value parameter, the argument expression is evaluated and an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to the corresponding parameter type is performed.</span></span> <span data-ttu-id="958cd-559">Sonuç değerini işlevi üye çağrısı değer parametresi ilk değeri olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-559">The resulting value becomes the initial value of the value parameter in the function member invocation.</span></span>
*  <span data-ttu-id="958cd-560">Bir başvuru veya çıkış parametresi için değişken başvurusu değerlendirilir ve elde edilen depolama konumu işlev üyesi çağırma parametresi tarafından temsil edilen depolama konumu olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-560">For a reference or output parameter, the variable reference is evaluated and the resulting storage location becomes the storage location represented by the parameter in the function member invocation.</span></span> <span data-ttu-id="958cd-561">Bir dizi öğesinin bir başvuru veya çıktı parametresi olarak verilen bir değişken başvurusu ise bir *reference_type*, dizinin öğe türü parametresinin türü için aynı olduğundan emin olmak için bir çalışma zamanı denetimi gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-561">If the variable reference given as a reference or output parameter is an array element of a *reference_type*, a run-time check is performed to ensure that the element type of the array is identical to the type of the parameter.</span></span> <span data-ttu-id="958cd-562">Bu denetim başarısız olursa bir `System.ArrayTypeMismatchException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-562">If this check fails, a `System.ArrayTypeMismatchException` is thrown.</span></span>

<span data-ttu-id="958cd-563">Yöntemler, dizin oluşturucular ve örnek oluşturucuları bir parametre dizisi, en sağdaki parametre bildirmek ([parametre dizileri](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="958cd-563">Methods, indexers, and instance constructors may declare their right-most parameter to be a parameter array ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="958cd-564">Bu işlev üyeleri normal formlarına veya hangi uygun olduğuna bağlı olarak, genişletilmiş biçimde çağrılır ([geçerli işlev üyesi](expressions.md#applicable-function-member)):</span><span class="sxs-lookup"><span data-stu-id="958cd-564">Such function members are invoked either in their normal form or in their expanded form depending on which is applicable ([Applicable function member](expressions.md#applicable-function-member)):</span></span>

*  <span data-ttu-id="958cd-565">Bir parametre dizisi olan bir işlevi üyesinin, normal bir biçimde çağrıldığında parametre dizisi için belirtilen bağımsız değişken örtük olarak dönüştürülebilir tek bir ifade olmalıdır ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) parametre dizi türü.</span><span class="sxs-lookup"><span data-stu-id="958cd-565">When a function member with a parameter array is invoked in its normal form, the argument given for the parameter array must be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="958cd-566">Bu durumda, parametre dizisi, tam olarak bir değer parametresini gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="958cd-566">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="958cd-567">Bir parametre dizisi olan bir işlevi üyesinin, genişletilmiş biçimde çağrıldığında, sıfır veya daha fazla konumsal bağımsız değişkenler örtük olarak dönüştürülebilir bir ifade bulunduğu her bağımsız değişken, bir parametre dizisi için çağırma belirtmeniz gerekir ([örtük dönüştürmeleri](conversions.md#implicit-conversions)) parametre dizinin öğe türü.</span><span class="sxs-lookup"><span data-stu-id="958cd-567">When a function member with a parameter array is invoked in its expanded form, the invocation must specify zero or more positional arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="958cd-568">Bu durumda, çağırma bağımsız değişken sayısı için karşılık gelen uzunluğu parametre dizi türü örneği oluşturur, belirtilen bağımsız değişken değerleri dizi örneği başlatır ve yeni oluşturulan bir dizi örneği fiili kullanır bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="958cd-568">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="958cd-569">İfade bir bağımsız değişken listesinin, yazıldıkları sırayla her zaman değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-569">The expressions of an argument list are always evaluated in the order they are written.</span></span> <span data-ttu-id="958cd-570">Bu nedenle, örneğin</span><span class="sxs-lookup"><span data-stu-id="958cd-570">Thus, the example</span></span>
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
<span data-ttu-id="958cd-571">çıktıyı üretir</span><span class="sxs-lookup"><span data-stu-id="958cd-571">produces the output</span></span>
```
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

<span data-ttu-id="958cd-572">Dizi değişimini kuralları ([dizi kovaryansı](arrays.md#array-covariance)) bir dizi türünde bir değer izin `A[]` bir dizi türünde bir örneğe bir başvuru olarak `B[]`, gelen bir örtük bir başvuru dönüştürmesi var sağlanan `B` için `A`.</span><span class="sxs-lookup"><span data-stu-id="958cd-572">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="958cd-573">Bu kurallar, bir dizi öğesi olduğunda nedeniyle bir *reference_type* geçirilen bir başvurusu veya çıktı parametresi olarak, dizinin gerçek öğe türü, parametre için aynı olduğundan emin olmak için bir çalışma zamanı denetimi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-573">Because of these rules, when an array element of a *reference_type* is passed as a reference or output parameter, a run-time check is required to ensure that the actual element type of the array is identical to that of the parameter.</span></span> <span data-ttu-id="958cd-574">Örnekte</span><span class="sxs-lookup"><span data-stu-id="958cd-574">In the example</span></span>
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
<span data-ttu-id="958cd-575">İkinci çağırmayı `F` neden olan bir `System.ArrayTypeMismatchException` gerçek öğe türü için durum `b` olduğu `string` değil `object`.</span><span class="sxs-lookup"><span data-stu-id="958cd-575">the second invocation of `F` causes a `System.ArrayTypeMismatchException` to be thrown because the actual element type of `b` is `string` and not `object`.</span></span>

<span data-ttu-id="958cd-576">Bir parametre dizisi olan bir işlevi üyesinin, genişletilmiş biçimde çağrıldığında, çağırma tam olarak bir dizi oluşturma ifadesi olarak bir dizi Başlatıcısı ile işlenir ([dizi oluşturma ifadeleri](expressions.md#array-creation-expressions)) geçici olarak eklendi Genişletilmiş parametreleri.</span><span class="sxs-lookup"><span data-stu-id="958cd-576">When a function member with a parameter array is invoked in its expanded form, the invocation is processed exactly as if an array creation expression with an array initializer ([Array creation expressions](expressions.md#array-creation-expressions)) was inserted around the expanded parameters.</span></span> <span data-ttu-id="958cd-577">Örneğin, bildirimi verilen</span><span class="sxs-lookup"><span data-stu-id="958cd-577">For example, given the declaration</span></span>
```csharp
void F(int x, int y, params object[] args);
```
<span data-ttu-id="958cd-578">yöntem genişletilmiş biçiminin aşağıdaki çağrıları</span><span class="sxs-lookup"><span data-stu-id="958cd-578">the following invocations of the expanded form of the method</span></span>
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
<span data-ttu-id="958cd-579">tam olarak karşılık gelir</span><span class="sxs-lookup"><span data-stu-id="958cd-579">correspond exactly to</span></span>
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

<span data-ttu-id="958cd-580">Özellikle, boş bir dizi parametre dizisi için verilen sıfır bağımsız değişken olduğunda oluşturulduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="958cd-580">In particular, note that an empty array is created when there are zero arguments given for the parameter array.</span></span>

<span data-ttu-id="958cd-581">Karşılık gelen bir isteğe bağlı parametreler içeren bir işlevi üyesinin bağımsız değişken atlanırsa, varsayılan bağımsız değişkenleri işlev üyesi bildirimi örtük olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-581">When arguments are omitted from a function member with corresponding optional parameters, the default arguments of the function member declaration are implicitly passed.</span></span> <span data-ttu-id="958cd-582">Bunlar her zaman sabit olduğu için bunların değerlendirme kalan bağımsız değişkenleri Değerlendirme sırasını etkilemez.</span><span class="sxs-lookup"><span data-stu-id="958cd-582">Because these are always constant, their evaluation will not impact the evaluation order of the remaining arguments.</span></span>

### <a name="type-inference"></a><span data-ttu-id="958cd-583">Tür çıkarımı</span><span class="sxs-lookup"><span data-stu-id="958cd-583">Type inference</span></span>

<span data-ttu-id="958cd-584">Genel yöntem çağrıldığında tür bağımsız değişkenleri, belirtmeden bir ***anlam çıkarma*** işlemi çağrısı için tür bağımsız değişkenleri Infer dener.</span><span class="sxs-lookup"><span data-stu-id="958cd-584">When a generic method is called without specifying type arguments, a ***type inference*** process attempts to infer type arguments for the call.</span></span> <span data-ttu-id="958cd-585">Tür çıkarımı varlığını genel bir yöntem çağırmak için kullanılacak daha kullanışlı bir söz dizimi ve Programcı gereksiz tür bilgilerini belirtmekten kaçınmak olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="958cd-585">The presence of type inference allows a more convenient syntax to be used for calling a generic method, and allows the programmer to avoid specifying redundant type information.</span></span> <span data-ttu-id="958cd-586">Örneğin, yöntem bildiriminde verilen:</span><span class="sxs-lookup"><span data-stu-id="958cd-586">For example, given the method declaration:</span></span>
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
<span data-ttu-id="958cd-587">çağrılacak mümkündür `Choose` tür bağımsız değişkeni açıkça belirtmeden yöntemi:</span><span class="sxs-lookup"><span data-stu-id="958cd-587">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

<span data-ttu-id="958cd-588">Tür çıkarımı, tür bağımsız değişkeni aracılığıyla `int` ve `string` bağımsız değişkenlerden yönteme belirlenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-588">Through type inference, the type arguments `int` and `string` are determined from the arguments to the method.</span></span>

<span data-ttu-id="958cd-589">Tür çıkarımı bağlama zamanı işleme yöntemi çağrısının bir parçası olarak gerçekleşir ([yöntem çağrıları](expressions.md#method-invocations)) ve çağırma aşırı yükleme çözünürlüğü adımdan önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="958cd-589">Type inference occurs as part of the binding-time processing of a method invocation ([Method invocations](expressions.md#method-invocations)) and takes place before the overload resolution step of the invocation.</span></span> <span data-ttu-id="958cd-590">Belirli yöntem grubu içinde bir yöntem çağırmayla belirtilir ve hiçbir tür bağımsız değişkenleri yöntemi çağrısının bir parçası olarak belirtilen tür çıkarımı her genel yöntem yöntem grubu uygulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-590">When a particular method group is specified in a method invocation, and no type arguments are specified as part of the method invocation, type inference is applied to each generic method in the method group.</span></span> <span data-ttu-id="958cd-591">Tür çıkarımı başarılı olursa çıkarsanan tür bağımsız değişkenlerini sonraki aşırı yükleme çözümlemesi için bağımsız değişken türlerini belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-591">If type inference succeeds, then the inferred type arguments are used to determine the types of arguments for subsequent overload resolution.</span></span> <span data-ttu-id="958cd-592">Aşırı yükleme çözünürlüğü çağırmak için bir genel yöntem seçerse, gösterilen türde bağımsız değişkenleri çağrı gerçek tür bağımsız değişkenleri olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-592">If overload resolution chooses a generic method as the one to invoke, then the inferred type arguments are used as the actual type arguments for the invocation.</span></span> <span data-ttu-id="958cd-593">Belirli bir yöntem için tür çıkarımı başarısız olursa, bu yöntem aşırı yükleme çözünürlüğü içinde yer almaz.</span><span class="sxs-lookup"><span data-stu-id="958cd-593">If type inference for a particular method fails, that method does not participate in overload resolution.</span></span> <span data-ttu-id="958cd-594">Tür çıkarımı, içinde ve kendisinin, başarısız bir bağlama zamanı hatası neden olmaz.</span><span class="sxs-lookup"><span data-stu-id="958cd-594">The failure of type inference, in and of itself, does not cause a binding-time error.</span></span> <span data-ttu-id="958cd-595">Ancak, uygun yöntemleri bulmak ardından aşırı yükleme çözümlemesi başarısız olduğunda genellikle bir bağlama zamanı hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-595">However, it often leads to a binding-time error when overload resolution then fails to find any applicable methods.</span></span>

<span data-ttu-id="958cd-596">Sağlanan bağımsız değişken sayısı yöntemindeki parametre sayısından farklı ise, çıkarım hemen başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-596">If the supplied number of arguments is different than the number of parameters in the method, then inference immediately fails.</span></span> <span data-ttu-id="958cd-597">Aksi takdirde, genel yöntem aşağıdaki imzası sahip olduğunu varsayın:</span><span class="sxs-lookup"><span data-stu-id="958cd-597">Otherwise, assume that the generic method has the following signature:</span></span>
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

<span data-ttu-id="958cd-598">Formun yöntemi çağrısıyla `M(E1...Em)` tür çıkarımı görevin benzersiz tür bağımsız değişkenleri bulmaktır `S1...Sn` her tür parametrelerinin `X1...Xn` böylece çağrı `M<S1...Sn>(E1...Em)` geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-598">With a method call of the form `M(E1...Em)` the task of type inference is to find unique type arguments `S1...Sn` for each of the type parameters `X1...Xn` so that the call `M<S1...Sn>(E1...Em)` becomes valid.</span></span>

<span data-ttu-id="958cd-599">Çıkarma işlemi sırasında her tür parametresi `Xi` ya da *sabit* belirli bir türe `Si` veya *sabitlenmemiş* kümesi ile *sınırları*.</span><span class="sxs-lookup"><span data-stu-id="958cd-599">During the process of inference each type parameter `Xi` is either *fixed* to a particular type `Si` or *unfixed* with an associated set of *bounds*.</span></span> <span data-ttu-id="958cd-600">Her sınır bazı türüdür `T`.</span><span class="sxs-lookup"><span data-stu-id="958cd-600">Each of the bounds is some type `T`.</span></span> <span data-ttu-id="958cd-601">Başlangıçta her tür değişkeni `Xi` boş kümesi ile sınırlarının sabitlenmemiş olduğu.</span><span class="sxs-lookup"><span data-stu-id="958cd-601">Initially each type variable `Xi` is unfixed with an empty set of bounds.</span></span>

<span data-ttu-id="958cd-602">Tür çıkarımı aşamada gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="958cd-602">Type inference takes place in phases.</span></span> <span data-ttu-id="958cd-603">Her aşama önceki aşamanın bulguları üzerinde göre daha fazla türü değişkenler için tür bağımsız değişkenleri Infer dener.</span><span class="sxs-lookup"><span data-stu-id="958cd-603">Each phase will try to infer type arguments for more type variables based on the findings of the previous phase.</span></span> <span data-ttu-id="958cd-604">İkinci aşama türü değişkenler belirli türlere giderir ve daha fazla sınır algılar ancak ilk aşaması sınırlarının, ilk bazı çıkarımı yapar.</span><span class="sxs-lookup"><span data-stu-id="958cd-604">The first phase makes some initial inferences of bounds, whereas the second phase fixes type variables to specific types and infers further bounds.</span></span> <span data-ttu-id="958cd-605">İkinci aşama olması gerekebilir birkaç kez yinelenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-605">The second phase may have to be repeated a number of times.</span></span>

<span data-ttu-id="958cd-606">*Not:* tür çıkarımı gerçekleşir yalnızca bir genel yöntem olduğunda çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-606">*Note:* Type inference takes place not only when a generic method is called.</span></span> <span data-ttu-id="958cd-607">Tür çıkarımı yöntemi gruplarına dönüştürülmesi için açıklanan [anlam çıkarma dönüştürme yöntemi gruplarının](expressions.md#type-inference-for-conversion-of-method-groups) ve en iyi ortak türü bir ifade kümesi bulma bölümünde açıklanmıştır [birtakım en iyi ortak türü bulma ifadelerin](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span><span class="sxs-lookup"><span data-stu-id="958cd-607">Type inference for conversion of method groups is described in [Type inference for conversion of method groups](expressions.md#type-inference-for-conversion-of-method-groups) and finding the best common type of a set of expressions is described in [Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span></span>

#### <a name="the-first-phase"></a><span data-ttu-id="958cd-608">Birinci aşama</span><span class="sxs-lookup"><span data-stu-id="958cd-608">The first phase</span></span>

<span data-ttu-id="958cd-609">Her yöntem bağımsız `Ei`:</span><span class="sxs-lookup"><span data-stu-id="958cd-609">For each of the method arguments `Ei`:</span></span>

*   <span data-ttu-id="958cd-610">Varsa `Ei` anonim bir işlev, bir *açık parametre tür çıkarımı* ([açık parametre türü çıkarımı](expressions.md#explicit-parameter-type-inferences)) öğesinden yapılan `Ei` için `Ti`</span><span class="sxs-lookup"><span data-stu-id="958cd-610">If `Ei` is an anonymous function, an *explicit parameter type inference* ([Explicit parameter type inferences](expressions.md#explicit-parameter-type-inferences)) is made from `Ei` to `Ti`</span></span>
*   <span data-ttu-id="958cd-611">Aksi takdirde `Ei` bir türe sahip `U` ve `xi` değer parametresi bir *alt sınır çıkarımı* yapılan *gelen* `U` *için* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="958cd-611">Otherwise, if `Ei` has a type `U` and `xi` is a value parameter then a *lower-bound inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="958cd-612">Aksi takdirde `Ei` bir türe sahip `U` ve `xi` olduğu bir `ref` veya `out` parametresi bir *tam çıkarımı* yapılan *gelen* `U` *için* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="958cd-612">Otherwise, if `Ei` has a type `U` and `xi` is a `ref` or `out` parameter then an *exact inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="958cd-613">Aksi takdirde, hiçbir çıkarımı bu bağımsız değişken için yapılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-613">Otherwise, no inference is made for this argument.</span></span>


#### <a name="the-second-phase"></a><span data-ttu-id="958cd-614">İkinci aşaması</span><span class="sxs-lookup"><span data-stu-id="958cd-614">The second phase</span></span>

<span data-ttu-id="958cd-615">İkinci aşama aşağıdaki gibi çalışır:</span><span class="sxs-lookup"><span data-stu-id="958cd-615">The second phase proceeds as follows:</span></span>

*   <span data-ttu-id="958cd-616">Tüm *sabitlenmemiş* türü değişkenler `Xi` gerektirmeyen *bağlıdır* ([bağımlılığı](expressions.md#dependence)) tüm `Xj` sabit ([düzeltme](expressions.md#fixing)).</span><span class="sxs-lookup"><span data-stu-id="958cd-616">All *unfixed* type variables `Xi` which do not *depend on* ([Dependence](expressions.md#dependence)) any `Xj` are fixed ([Fixing](expressions.md#fixing)).</span></span>
*   <span data-ttu-id="958cd-617">Böyle bir türü değişkenler varsa, tüm *sabitlenmemiş* türü değişkenler `Xi` olan *sabit* kendisi için aşağıdakilerin tümü tutun:</span><span class="sxs-lookup"><span data-stu-id="958cd-617">If no such type variables exist, all *unfixed* type variables `Xi` are *fixed* for which all of the following hold:</span></span>
    *   <span data-ttu-id="958cd-618">En az bir tür değişkeni `Xj` , bağlıdır `Xi`</span><span class="sxs-lookup"><span data-stu-id="958cd-618">There is at least one type variable `Xj` that depends on `Xi`</span></span>
    *   <span data-ttu-id="958cd-619">`Xi` bir olmayan-boş dizi sınırları vardır</span><span class="sxs-lookup"><span data-stu-id="958cd-619">`Xi` has a non-empty set of bounds</span></span>
*   <span data-ttu-id="958cd-620">Böyle bir türü değişkenler mevcutsa ve hala *sabitlenmemiş* değişkenleri, anlam çıkarma başarısız yazın.</span><span class="sxs-lookup"><span data-stu-id="958cd-620">If no such type variables exist and there are still *unfixed* type variables, type inference fails.</span></span>
*   <span data-ttu-id="958cd-621">Aksi takdirde, başka *sabitlenmemiş* türü değişkenler var, tür çıkarımı başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-621">Otherwise, if no further *unfixed* type variables exist, type inference succeeds.</span></span>
*   <span data-ttu-id="958cd-622">Aksi takdirde tüm bağımsız değişkenler için `Ei` karşılık gelen parametre türüyle `Ti` burada *çıktı türleri* ([çıktı türleri](expressions.md#output-types)) içeren *sabitlenmemiş* türü değişkenler `Xj` ancak *giriş türleri* ([giriş türleri](expressions.md#input-types)) bulunmayan bir *çıkış tür çıkarımı* ([çıkış tür çıkarımı ](expressions.md#output-type-inferences)) yapılan *gelen* `Ei` *için* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="958cd-622">Otherwise, for all arguments `Ei` with corresponding parameter type `Ti` where the *output types* ([Output types](expressions.md#output-types)) contain *unfixed* type variables `Xj` but the *input types* ([Input types](expressions.md#input-types)) do not, an *output type inference* ([Output type inferences](expressions.md#output-type-inferences)) is made *from* `Ei` *to* `Ti`.</span></span> <span data-ttu-id="958cd-623">İkinci aşama daha sonra yinelenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-623">Then the second phase is repeated.</span></span>

#### <a name="input-types"></a><span data-ttu-id="958cd-624">Giriş türleri</span><span class="sxs-lookup"><span data-stu-id="958cd-624">Input types</span></span>

<span data-ttu-id="958cd-625">Varsa `E` bir yöntem grubu ya da anonim işlev örtük olarak belirlenmiş ve `T` bir temsilci türü veya ifade ağacı türü sonra tüm parametre türlerini mi `T` olan *giriş türleri* , `E` *türüyle* `T`.</span><span class="sxs-lookup"><span data-stu-id="958cd-625">If `E` is a method group or implicitly typed anonymous function and `T` is a delegate type or expression tree type then all the parameter types of `T` are *input types* of `E` *with type* `T`.</span></span>

####  <a name="output-types"></a><span data-ttu-id="958cd-626">Çıktı türleri</span><span class="sxs-lookup"><span data-stu-id="958cd-626">Output types</span></span>

<span data-ttu-id="958cd-627">Varsa `E` bir yöntem grubu veya anonim bir işlevdir ve `T` bir temsilci türü veya ifade ağacı türü sonra dönüş türü olan `T` olduğu bir *çıktı türünü* `E` *türüne sahip*  `T`.</span><span class="sxs-lookup"><span data-stu-id="958cd-627">If `E` is a method group or an anonymous function and `T` is a delegate type or expression tree type then the return type of `T` is an *output type of* `E` *with type* `T`.</span></span>

#### <a name="dependence"></a><span data-ttu-id="958cd-628">Bağımlılık</span><span class="sxs-lookup"><span data-stu-id="958cd-628">Dependence</span></span>

<span data-ttu-id="958cd-629">Bir *sabitlenmemiş* tür değişkeni `Xi` *doğrudan bağlı olduğu* sabitlenmemiş türü değişkeni `Xj` bazı bağımsız değişkeni ise `Ek` türüyle `Tk` `Xj` oluşan bir *giriş türü* , `Ek` türüyle `Tk` ve `Xi` oluşan bir *çıkış türü* , `Ek` türüyle `Tk`.</span><span class="sxs-lookup"><span data-stu-id="958cd-629">An *unfixed* type variable `Xi` *depends directly on* an unfixed type variable `Xj` if for some argument `Ek` with type `Tk` `Xj` occurs in an *input type* of `Ek` with type `Tk` and `Xi` occurs in an *output type* of `Ek` with type `Tk`.</span></span>

<span data-ttu-id="958cd-630">`Xj` *bağımlı* `Xi` varsa `Xj` *doğrudan bağlı olduğu* `Xi` veya `Xi` *doğrudan bağlı olduğu* `Xk` ve `Xk` *bağlıdır* `Xj`.</span><span class="sxs-lookup"><span data-stu-id="958cd-630">`Xj` *depends on* `Xi` if `Xj` *depends directly on* `Xi` or if `Xi` *depends directly on* `Xk` and `Xk` *depends on* `Xj`.</span></span> <span data-ttu-id="958cd-631">Bu nedenle "bağlıdır" olan "doğrudan bağlı olduğu" geçişli ancak değil Yansımalı kapatma.</span><span class="sxs-lookup"><span data-stu-id="958cd-631">Thus "depends on" is the transitive but not reflexive closure of "depends directly on".</span></span>

#### <a name="output-type-inferences"></a><span data-ttu-id="958cd-632">Çıkış türü çıkarımı</span><span class="sxs-lookup"><span data-stu-id="958cd-632">Output type inferences</span></span>

<span data-ttu-id="958cd-633">Bir *çıkış tür çıkarımı* yapılan *gelen* bir ifade `E` *için* türü `T` şu şekilde:</span><span class="sxs-lookup"><span data-stu-id="958cd-633">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="958cd-634">Varsa `E` çıkarılan dönüş türü anonim bir işlevdir `U` ([Inferred dönüş türü](expressions.md#inferred-return-type)) ve `T` bir temsilci türü veya dönüş türüne sahip ifade ağacı türü `Tb`, ardından *alt sınır çıkarımı* ([alt sınır çıkarımı](expressions.md#lower-bound-inferences)) yapılan *gelen* `U` *için* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="958cd-634">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="958cd-635">Aksi takdirde `E` bir yöntem grubu olduğundan ve `T` bir temsilci türü veya parametre türleri ile ifade ağacı türü `T1...Tk` ve dönüş türü `Tb`ve aşırı yükleme çözünürlüğü `E` türleriyle `T1...Tk` verir bir tek bir yöntemin dönüş türü içeren `U`, ardından bir *alt sınır çıkarımı* yapılan *gelen* `U` *için* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="958cd-635">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="958cd-636">Aksi takdirde `E` türüne sahip ifade `U`, bir *alt sınır çıkarımı* yapılan *gelen* `U` *için* `T`.</span><span class="sxs-lookup"><span data-stu-id="958cd-636">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
*  <span data-ttu-id="958cd-637">Aksi takdirde, hiçbir çıkarımı yapılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-637">Otherwise, no inferences are made.</span></span>

#### <a name="explicit-parameter-type-inferences"></a><span data-ttu-id="958cd-638">Açık parametre tür çıkarımı</span><span class="sxs-lookup"><span data-stu-id="958cd-638">Explicit parameter type inferences</span></span>

<span data-ttu-id="958cd-639">Bir *açık parametre tür çıkarımı* yapılan *gelen* bir ifade `E` *için* türü `T` şu şekilde:</span><span class="sxs-lookup"><span data-stu-id="958cd-639">An *explicit parameter type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="958cd-640">Varsa `E` parametre türleri ile açıkça yazılmış bir anonim işlev `U1...Uk` ve `T` bir temsilci türü veya parametre türleri ile ifade ağacı türü `V1...Vk` sonra her `Ui` bir *tam çıkarım* ([tam çıkarımı](expressions.md#exact-inferences)) yapılan *gelen* `Ui` *için* karşılık gelen `Vi`.</span><span class="sxs-lookup"><span data-stu-id="958cd-640">If `E` is an explicitly typed anonymous function with parameter types `U1...Uk` and `T` is a delegate type or expression tree type with parameter types `V1...Vk` then for each `Ui` an *exact inference* ([Exact inferences](expressions.md#exact-inferences)) is made *from* `Ui` *to* the corresponding `Vi`.</span></span>

#### <a name="exact-inferences"></a><span data-ttu-id="958cd-641">Tam çıkarımı</span><span class="sxs-lookup"><span data-stu-id="958cd-641">Exact inferences</span></span>

<span data-ttu-id="958cd-642">Bir *tam çıkarımı* *gelen* türü `U` *için* türü `V` şu şekilde yapılır:</span><span class="sxs-lookup"><span data-stu-id="958cd-642">An *exact inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="958cd-643">Varsa `V` biri *sabitlenmemiş* `Xi` ardından `U` tam sınırlarının kümesine eklenen `Xi`.</span><span class="sxs-lookup"><span data-stu-id="958cd-643">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of exact bounds for `Xi`.</span></span>

*  <span data-ttu-id="958cd-644">Aksi takdirde ayarlar `V1...Vk` ve `U1...Uk` aşağıdaki durumlarda atanamadığı olmadığının kontrol edilmesiyle belirlenir:</span><span class="sxs-lookup"><span data-stu-id="958cd-644">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>

   *  <span data-ttu-id="958cd-645">`V` bir dizi türü `V1[...]` ve `U` bir dizi türü `U1[...]` aynı boyut,</span><span class="sxs-lookup"><span data-stu-id="958cd-645">`V` is an array type `V1[...]` and `U` is an array type `U1[...]`  of the same rank</span></span>
   *  <span data-ttu-id="958cd-646">`V` türü `V1?` ve `U` türü `U1?`</span><span class="sxs-lookup"><span data-stu-id="958cd-646">`V` is the type `V1?` and `U` is the type `U1?`</span></span>
   *  <span data-ttu-id="958cd-647">`V` bir oluşturulmuş tür olsa `C<V1...Vk>`ve `U` bir oluşturulmuş tür olsa `C<U1...Uk>`</span><span class="sxs-lookup"><span data-stu-id="958cd-647">`V` is a constructed type `C<V1...Vk>`and `U` is a constructed type `C<U1...Uk>`</span></span>

   <span data-ttu-id="958cd-648">Bu durumların herhangi birinde ardından uygularsanız bir *tam çıkarımı* yapılan *gelen* her `Ui` *için* karşılık gelen `Vi`.</span><span class="sxs-lookup"><span data-stu-id="958cd-648">If any of these cases apply then an *exact inference* is made *from* each `Ui` *to* the corresponding `Vi`.</span></span>

*  <span data-ttu-id="958cd-649">Aksi takdirde, hiçbir çıkarımı yapılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-649">Otherwise no inferences are made.</span></span>

#### <a name="lower-bound-inferences"></a><span data-ttu-id="958cd-650">Alt sınır çıkarımı</span><span class="sxs-lookup"><span data-stu-id="958cd-650">Lower-bound inferences</span></span>

<span data-ttu-id="958cd-651">A *alt sınır çıkarımı* *gelen* türü `U` *için* türü `V` şu şekilde yapılır:</span><span class="sxs-lookup"><span data-stu-id="958cd-651">A *lower-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="958cd-652">Varsa `V` biri *sabitlenmemiş* `Xi` ardından `U` alt sınırı kümesine eklenen `Xi`.</span><span class="sxs-lookup"><span data-stu-id="958cd-652">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of lower bounds for `Xi`.</span></span>
*  <span data-ttu-id="958cd-653">Aksi takdirde `V` türü `V1?`ve `U` türü `U1?` alt sınır çıkarımı yapılan sonra `U1` için `V1`.</span><span class="sxs-lookup"><span data-stu-id="958cd-653">Otherwise, if `V` is the type `V1?`and `U` is the type `U1?` then a lower bound inference is made from `U1` to `V1`.</span></span>
*  <span data-ttu-id="958cd-654">Aksi takdirde ayarlar `U1...Uk` ve `V1...Vk` aşağıdaki durumlarda atanamadığı olmadığının kontrol edilmesiyle belirlenir:</span><span class="sxs-lookup"><span data-stu-id="958cd-654">Otherwise, sets `U1...Uk` and `V1...Vk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="958cd-655">`V` bir dizi türü `V1[...]` ve `U` bir dizi türü `U1[...]` (veya tür parametresi, geçerli bir temel türü olan `U1[...]`) aynı boyut,</span><span class="sxs-lookup"><span data-stu-id="958cd-655">`V` is an array type `V1[...]` and `U` is an array type `U1[...]` (or a type parameter whose effective base type is `U1[...]`) of the same rank</span></span>
   *  <span data-ttu-id="958cd-656">`V` biri `IEnumerable<V1>`, `ICollection<V1>` veya `IList<V1>` ve `U` tek boyutlu dizi türü `U1[]`(veya tür parametresi, geçerli bir temel türü olan `U1[]`)</span><span class="sxs-lookup"><span data-stu-id="958cd-656">`V` is one of `IEnumerable<V1>`, `ICollection<V1>` or `IList<V1>` and `U` is a one-dimensional array type `U1[]`(or a type parameter whose effective base type is `U1[]`)</span></span>
   *  <span data-ttu-id="958cd-657">`V` oluşturulan sınıf, yapı, arabirim veya temsilci türü `C<V1...Vk>` ve benzersiz bir tür `C<U1...Uk>` şekilde `U` (veya `U` bir tür parametresi, etkili temel sınıfı veya etkin arabirim kendine herhangi bir üyesi) olan aynıdır, devralınan (doğrudan veya dolaylı olarak) veya (doğrudan veya dolaylı olarak) uygulayan `C<U1...Uk>`.</span><span class="sxs-lookup"><span data-stu-id="958cd-657">`V` is a constructed class, struct, interface or delegate type `C<V1...Vk>` and there is a unique type `C<U1...Uk>` such that `U` (or, if `U` is a type parameter, its effective base class or any member of its effective interface set) is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) `C<U1...Uk>`.</span></span>

      <span data-ttu-id="958cd-658">(Büyük/küçük harf arabiriminde anlamına "benzersizlik" kısıtlama `C<T> {} class U: C<X>, C<Y> {}`, ardından gelen çıkarımını yapma, hiçbir çıkarımı yapılan `U` için `C<T>` çünkü `U1` olabilir `X` veya `Y`.)</span><span class="sxs-lookup"><span data-stu-id="958cd-658">(The "uniqueness" restriction means that in the case interface `C<T> {} class U: C<X>, C<Y> {}`, then no inference is made when inferring from `U` to `C<T>` because `U1` could be `X` or `Y`.)</span></span>

   <span data-ttu-id="958cd-659">Ardından bir çıkarımı yapılan tüm durumlarda uygularsanız *gelen* her `Ui` *için* karşılık gelen `Vi` şu şekilde:</span><span class="sxs-lookup"><span data-stu-id="958cd-659">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>

   *  <span data-ttu-id="958cd-660">Varsa `Ui` bir başvuru türü bilinmiyor bir *tam çıkarımı* yapılır</span><span class="sxs-lookup"><span data-stu-id="958cd-660">If `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="958cd-661">Aksi takdirde `U` bir dizi türü bir *alt sınır çıkarımı* yapılır</span><span class="sxs-lookup"><span data-stu-id="958cd-661">Otherwise, if `U` is an array type then a *lower-bound inference* is made</span></span>
   *  <span data-ttu-id="958cd-662">Aksi takdirde `V` olduğu `C<V1...Vk>` çıkarımı i. tür parametresi üzerinde bağlıdır sonra `C`:</span><span class="sxs-lookup"><span data-stu-id="958cd-662">Otherwise, if `V` is `C<V1...Vk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="958cd-663">Birlikte değişken ise bir *alt sınır çıkarımı* yapılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-663">If it is covariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="958cd-664">Değişken karşıtı ise bir *üst sınır çıkarımı* yapılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-664">If it is contravariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="958cd-665">Sabit olması durumunda bir *tam çıkarımı* yapılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-665">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="958cd-666">Aksi takdirde, hiçbir çıkarımı yapılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-666">Otherwise, no inferences are made.</span></span>

#### <a name="upper-bound-inferences"></a><span data-ttu-id="958cd-667">Üst sınır çıkarımı</span><span class="sxs-lookup"><span data-stu-id="958cd-667">Upper-bound inferences</span></span>

<span data-ttu-id="958cd-668">Bir *üst sınır çıkarımı* *gelen* türü `U` *için* türü `V` şu şekilde yapılır:</span><span class="sxs-lookup"><span data-stu-id="958cd-668">An *upper-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="958cd-669">Varsa `V` biri *sabitlenmemiş* `Xi` ardından `U` için üst sınır kümesine eklenen `Xi`.</span><span class="sxs-lookup"><span data-stu-id="958cd-669">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of upper bounds for `Xi`.</span></span>
*  <span data-ttu-id="958cd-670">Aksi takdirde ayarlar `V1...Vk` ve `U1...Uk` aşağıdaki durumlarda atanamadığı olmadığının kontrol edilmesiyle belirlenir:</span><span class="sxs-lookup"><span data-stu-id="958cd-670">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="958cd-671">`U` bir dizi türü `U1[...]` ve `V` bir dizi türü `V1[...]` aynı boyut,</span><span class="sxs-lookup"><span data-stu-id="958cd-671">`U` is an array type `U1[...]` and `V` is an array type `V1[...]` of the same rank</span></span>
   *  <span data-ttu-id="958cd-672">`U` biri `IEnumerable<Ue>`, `ICollection<Ue>` veya `IList<Ue>` ve `V` tek boyutlu dizi türü `Ve[]`</span><span class="sxs-lookup"><span data-stu-id="958cd-672">`U` is one of `IEnumerable<Ue>`, `ICollection<Ue>` or `IList<Ue>` and `V` is a one-dimensional array type `Ve[]`</span></span>
   *  <span data-ttu-id="958cd-673">`U` türü `U1?` ve `V` türü `V1?`</span><span class="sxs-lookup"><span data-stu-id="958cd-673">`U` is the type `U1?` and `V` is the type `V1?`</span></span>
   *  <span data-ttu-id="958cd-674">`U` oluşturulan sınıf, yapı, arabirim veya temsilci türü olduğundan `C<U1...Uk>` ve `V` bir sınıf, yapı, devralır, (doğrudan veya dolaylı olarak) eşdeğer olan arabirim veya temsilci türünün veya uygular (doğrudan veya dolaylı olarak) olan bir benzersiz tür `C<V1...Vk>`</span><span class="sxs-lookup"><span data-stu-id="958cd-674">`U` is constructed class, struct, interface or delegate type `C<U1...Uk>` and `V` is a class, struct, interface or delegate type which is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) a unique type `C<V1...Vk>`</span></span>

      <span data-ttu-id="958cd-675">(Biz varsa anlamına "benzersizlik" kısıtlama `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, ardından gelen çıkarımını yapma, hiçbir çıkarımı yapılan `C<U1>` için `V<Q>`.</span><span class="sxs-lookup"><span data-stu-id="958cd-675">(The "uniqueness" restriction means that if we have `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, then no inference is made when inferring from `C<U1>` to `V<Q>`.</span></span> <span data-ttu-id="958cd-676">Çıkarımı değil yapılır `U1` ya da `X<Q>` veya `Y<Q>`.)</span><span class="sxs-lookup"><span data-stu-id="958cd-676">Inferences are not made from `U1` to either `X<Q>` or `Y<Q>`.)</span></span>

   <span data-ttu-id="958cd-677">Ardından bir çıkarımı yapılan tüm durumlarda uygularsanız *gelen* her `Ui` *için* karşılık gelen `Vi` şu şekilde:</span><span class="sxs-lookup"><span data-stu-id="958cd-677">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>
   *  <span data-ttu-id="958cd-678">Varsa `Ui` bir başvuru türü bilinmiyor bir *tam çıkarımı* yapılır</span><span class="sxs-lookup"><span data-stu-id="958cd-678">If  `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="958cd-679">Aksi takdirde `V` bir dizi türü bir *üst sınır çıkarımı* yapılır</span><span class="sxs-lookup"><span data-stu-id="958cd-679">Otherwise, if `V` is an array type then an *upper-bound inference* is made</span></span>
   *  <span data-ttu-id="958cd-680">Aksi takdirde `U` olduğu `C<U1...Uk>` çıkarımı i. tür parametresi üzerinde bağlıdır sonra `C`:</span><span class="sxs-lookup"><span data-stu-id="958cd-680">Otherwise, if `U` is `C<U1...Uk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="958cd-681">Birlikte değişken ise bir *üst sınır çıkarımı* yapılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-681">If it is covariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="958cd-682">Değişken karşıtı ise bir *alt sınır çıkarımı* yapılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-682">If it is contravariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="958cd-683">Sabit olması durumunda bir *tam çıkarımı* yapılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-683">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="958cd-684">Aksi takdirde, hiçbir çıkarımı yapılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-684">Otherwise, no inferences are made.</span></span>   

#### <a name="fixing"></a><span data-ttu-id="958cd-685">Düzeltme</span><span class="sxs-lookup"><span data-stu-id="958cd-685">Fixing</span></span>

<span data-ttu-id="958cd-686">Bir *sabitlenmemiş* tür değişkeni `Xi` sınırları bir dizi *sabit* şu şekilde:</span><span class="sxs-lookup"><span data-stu-id="958cd-686">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>

*  <span data-ttu-id="958cd-687">Dizi *aday türleri* `Uj` sınırlarının kümesindeki tüm türleri olarak başlar `Xi`.</span><span class="sxs-lookup"><span data-stu-id="958cd-687">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
*  <span data-ttu-id="958cd-688">Ardından her sınırını inceleyeceğiz `Xi` sırayla: tam her bağlama için `U` , `Xi` tüm türleri `Uj` olmayan aynı `U` aday kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-688">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="958cd-689">Her alt sınırı için `U` , `Xi` tüm türleri `Uj` için hangi orada olduğu *değil* örtük bir dönüştürme `U` aday kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-689">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="958cd-690">Her üst sınırı için `U` , `Xi` tüm türleri `Uj` hangi buradan olan *değil* örtük dönüştürmeleri `U` aday kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-690">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
*  <span data-ttu-id="958cd-691">Eğer kalan aday türleri arasında `Uj` benzersiz bir tür `V` olduğu örtük dönüştürmeleri diğer tüm aday türleri, ardından gelen `Xi` için sabit `V`.</span><span class="sxs-lookup"><span data-stu-id="958cd-691">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then `Xi` is fixed to `V`.</span></span>
*  <span data-ttu-id="958cd-692">Aksi takdirde, tür çıkarımı başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-692">Otherwise, type inference fails.</span></span>

#### <a name="inferred-return-type"></a><span data-ttu-id="958cd-693">Çıkarılan dönüş türü</span><span class="sxs-lookup"><span data-stu-id="958cd-693">Inferred return type</span></span>

<span data-ttu-id="958cd-694">Çıkarılan dönüş türü anonim bir işlevin `F` tür çıkarımı ve aşırı yükleme çözümlemesi sırasında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-694">The inferred return type of an anonymous function `F` is used during type inference and overload resolution.</span></span> <span data-ttu-id="958cd-695">Çıkarılan dönüş türü, burada verilen açık olduğundan, türleri, ya da bilinen tüm parametre bir anonim işlev dönüştürme sağlanan veya üzerinde bir kapsayan tür çıkarımı sırasında genel çıkarılan anonim bir işlev için yalnızca belirlenebilir yöntem çağırma.</span><span class="sxs-lookup"><span data-stu-id="958cd-695">The inferred return type can only be determined for an anonymous function where all parameter types are known, either because they are explicitly given, provided through an anonymous function conversion or inferred during type inference on an enclosing generic method invocation.</span></span>

<span data-ttu-id="958cd-696">***Sonuç türü çıkarımı yapılan*** şu şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="958cd-696">The ***inferred result type*** is determined as follows:</span></span>

*  <span data-ttu-id="958cd-697">Varsa gövdesinin `F` olduğu bir *ifade* türü ve çıkarılan bir sonuç türü olan `F` ifade türü.</span><span class="sxs-lookup"><span data-stu-id="958cd-697">If the body of `F` is an *expression* that has a type, then the inferred result type of `F` is the type of that expression.</span></span>
*  <span data-ttu-id="958cd-698">Varsa gövdesinin `F` olduğu bir *blok* ve bloğun ifadelerde dizi `return` deyimleri en iyi ortak bir türe sahip `T` ([birifadekümesienyaygıntürübulma](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), ardından çıkarılan bir sonuç türü `F` olduğu `T`.</span><span class="sxs-lookup"><span data-stu-id="958cd-698">If the body of `F` is a *block* and the set of expressions in the block's `return` statements has a best common type `T` ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), then the inferred result type of `F` is `T`.</span></span>
*  <span data-ttu-id="958cd-699">Aksi halde, sonuç türü için çıkarsanamıyor `F`.</span><span class="sxs-lookup"><span data-stu-id="958cd-699">Otherwise, a result type cannot be inferred for `F`.</span></span>

<span data-ttu-id="958cd-700">***Dönüş türü çıkarımı yapılan*** şu şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="958cd-700">The ***inferred return type*** is determined as follows:</span></span>

*  <span data-ttu-id="958cd-701">Varsa `F` zaman uyumsuz ve gövdesini `F` nothing sınıflandırılmış bir ya da bir ifade ([ifade sınıflandırmaları](expressions.md#expression-classifications)), ya da dönüş deyim ifadeleri, sahip olduğu bir deyim bloğunu çıkarılan dönüş türü `System.Threading.Tasks.Task`</span><span class="sxs-lookup"><span data-stu-id="958cd-701">If `F` is async and the body of `F` is either an expression classified as nothing ([Expression classifications](expressions.md#expression-classifications)), or a statement block where no return statements have expressions, the inferred return type is `System.Threading.Tasks.Task`</span></span>
*  <span data-ttu-id="958cd-702">Varsa `F` zaman uyumsuz olduğunu ve çıkarılan bir sonuç türü `T`, çıkarılan dönüş türü `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="958cd-702">If `F` is async and has an inferred result type `T`, the inferred return type is `System.Threading.Tasks.Task<T>`.</span></span>
*  <span data-ttu-id="958cd-703">Varsa `F` olmayan zaman uyumsuz olduğunu ve çıkarılan bir sonuç türü `T`, çıkarılan dönüş türü `T`.</span><span class="sxs-lookup"><span data-stu-id="958cd-703">If `F` is non-async and has an inferred result type `T`, the inferred return type is `T`.</span></span>
*  <span data-ttu-id="958cd-704">Aksi takdirde bir dönüş türü için çıkarsanamıyor `F`.</span><span class="sxs-lookup"><span data-stu-id="958cd-704">Otherwise a return type cannot be inferred for `F`.</span></span>

<span data-ttu-id="958cd-705">Tür çıkarımı anonim işlevler içeren bir örnek olarak, göz önünde bulundurun `Select` genişletme yöntemi içinde bildirilen `System.Linq.Enumerable` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="958cd-705">As an example of type inference involving anonymous functions, consider the `Select` extension method declared in the `System.Linq.Enumerable` class:</span></span>
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

<span data-ttu-id="958cd-706">Varsayılarak `System.Linq` ad alanı ile alınmış bir `using` yan tümcesi ve bir sınıf belirtilen `Customer` ile bir `Name` türünün özelliği `string`, `Select` yöntemi, müşterilerin listesini adlarını seçmek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="958cd-706">Assuming the `System.Linq` namespace was imported with a `using` clause, and given a class `Customer` with a `Name` property of type `string`, the `Select` method can be used to select the names of a list of customers:</span></span>
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

<span data-ttu-id="958cd-707">Uzantı metodu çağırma ([uzantısı yöntem çağrıları](expressions.md#extension-method-invocations)), `Select` çağrıya bir statik yöntem çağırma yazarak işlenir:</span><span class="sxs-lookup"><span data-stu-id="958cd-707">The extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)) of `Select` is processed by rewriting the invocation to a static method invocation:</span></span>
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

<span data-ttu-id="958cd-708">Tür bağımsız değişkenleri açıkça belirtilmedi olduğundan, tür çıkarımı tür bağımsız değişkenlerini çıkarsamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-708">Since type arguments were not explicitly specified, type inference is used to infer the type arguments.</span></span> <span data-ttu-id="958cd-709">İlk olarak, `customers` bağımsız değişken ilgili `source` parametresi çıkarımını yapma `T` olmasını `Customer`.</span><span class="sxs-lookup"><span data-stu-id="958cd-709">First, the `customers` argument is related to the `source` parameter, inferring `T` to be `Customer`.</span></span> <span data-ttu-id="958cd-710">Anonim işlev kullanarak yazın, yukarıda açıklanan çıkarımı işleminin `c` türü verilen `Customer`ve ifade `c.Name` dönüş türü için ilgili `selector` parametresi çıkarımını yapma `S` olması`string`.</span><span class="sxs-lookup"><span data-stu-id="958cd-710">Then, using the anonymous function type inference process described above, `c` is given type `Customer`, and the expression `c.Name` is related to the return type of the `selector` parameter, inferring `S` to be `string`.</span></span> <span data-ttu-id="958cd-711">Bu nedenle, çağırma eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="958cd-711">Thus, the invocation is equivalent to</span></span>
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
<span data-ttu-id="958cd-712">ve sonuç türünü `IEnumerable<string>`.</span><span class="sxs-lookup"><span data-stu-id="958cd-712">and the result is of type `IEnumerable<string>`.</span></span>

<span data-ttu-id="958cd-713">Aşağıdaki örnek nasıl anonim işlev türü gösterir çıkarımı "bağımsız değişken bir genel yöntem çağrısı arasında akış" tür bilgilerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="958cd-713">The following example demonstrates how anonymous function type inference allows type information to "flow" between arguments in a generic method invocation.</span></span> <span data-ttu-id="958cd-714">Yöntem verilen:</span><span class="sxs-lookup"><span data-stu-id="958cd-714">Given the method:</span></span>
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

<span data-ttu-id="958cd-715">Tür çıkarımı çağrısı için:</span><span class="sxs-lookup"><span data-stu-id="958cd-715">Type inference for the invocation:</span></span>
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
<span data-ttu-id="958cd-716">aşağıdaki gibi çalışır: ilk olarak, bağımsız değişken `"1:15:30"` ilgili `value` parametresi çıkarımını yapma `X` olmasını `string`.</span><span class="sxs-lookup"><span data-stu-id="958cd-716">proceeds as follows: First, the argument `"1:15:30"` is related to the `value` parameter, inferring `X` to be `string`.</span></span> <span data-ttu-id="958cd-717">Ardından, ilk anonim işlev parametresinin `s`, çıkarsanan tür verilen `string`ve ifade `TimeSpan.Parse(s)` dönüş türü için ilgili `f1`, çıkarımını yapma `Y` olmasını `System.TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="958cd-717">Then, the parameter of the first anonymous function, `s`, is given the inferred type `string`, and the expression `TimeSpan.Parse(s)` is related to the return type of `f1`, inferring `Y` to be `System.TimeSpan`.</span></span> <span data-ttu-id="958cd-718">Son olarak, ikinci anonim işlev parametresi `t`, çıkarsanan tür verilen `System.TimeSpan`ve ifade `t.TotalSeconds` dönüş türü için ilgili `f2`, çıkarımını yapma `Z` olmasını `double`.</span><span class="sxs-lookup"><span data-stu-id="958cd-718">Finally, the parameter of the second anonymous function, `t`, is given the inferred type `System.TimeSpan`, and the expression `t.TotalSeconds` is related to the return type of `f2`, inferring `Z` to be `double`.</span></span> <span data-ttu-id="958cd-719">Bu nedenle, çağrısının sonucunu türünde `double`.</span><span class="sxs-lookup"><span data-stu-id="958cd-719">Thus, the result of the invocation is of type `double`.</span></span>

#### <a name="type-inference-for-conversion-of-method-groups"></a><span data-ttu-id="958cd-720">Dönüştürme yöntemi grupları için tür çıkarımı</span><span class="sxs-lookup"><span data-stu-id="958cd-720">Type inference for conversion of method groups</span></span>

<span data-ttu-id="958cd-721">Benzer şekilde, genel yöntemlerin kullanımını çağrıları, tür çıkarımı ayrıca bir yöntem grubu olduğunda uygulanmalıdır `M` genel yöntem içeren bir temsilci türüne dönüştürülür `D` ([yöntem grubu dönüştürmeler](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="958cd-721">Similar to calls of generic methods, type inference must also be applied when a method group `M` containing a generic method is converted to a given delegate type `D` ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="958cd-722">Verilen bir yöntemi</span><span class="sxs-lookup"><span data-stu-id="958cd-722">Given a method</span></span>
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
<span data-ttu-id="958cd-723">ve yöntem grubu `M` temsilci türüne atanan `D` görevi tür çıkarımı, tür bağımsız değişkenleri bulmaktır `S1...Sn` böylece ifade:</span><span class="sxs-lookup"><span data-stu-id="958cd-723">and the method group `M` being assigned to the delegate type `D` the task of type inference is to find type arguments `S1...Sn` so that the expression:</span></span>
```csharp
M<S1...Sn>
```
<span data-ttu-id="958cd-724">uyumlu hale gelir ([temsilci bildirimi](delegates.md#delegate-declarations)) ile `D`.</span><span class="sxs-lookup"><span data-stu-id="958cd-724">becomes compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`.</span></span>

<span data-ttu-id="958cd-725">Genel yöntem çağrıları için tür çıkarımı algoritması bu durumda olup yalnızca bağımsız değişken *türleri*, hiçbir bağımsız değişken *ifadeleri*.</span><span class="sxs-lookup"><span data-stu-id="958cd-725">Unlike the type inference algorithm for generic method calls, in this case there are only argument *types*, no argument *expressions*.</span></span> <span data-ttu-id="958cd-726">Özellikle, hiçbir anonim işlev vardır ve bu nedenle birden çok aşamaları çıkarımı gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="958cd-726">In particular, there are no anonymous functions and hence no need for multiple phases of inference.</span></span>

<span data-ttu-id="958cd-727">Bunun yerine, tüm `Xi` değerlendirilir *sabitlenmemiş*ve *alt sınır çıkarımı* yapılan *gelen* her bağımsız değişken türü `Uj` , `D` *için* karşılık gelen parametre türü `Tj` , `M`.</span><span class="sxs-lookup"><span data-stu-id="958cd-727">Instead, all `Xi` are considered *unfixed*, and a *lower-bound inference* is made *from* each argument type `Uj` of `D` *to* the corresponding parameter type `Tj` of `M`.</span></span> <span data-ttu-id="958cd-728">Eğer herhangi birinin `Xi` sınır yoktur bulunamadı, tür çıkarımı başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-728">If for any of the `Xi` no bounds were found, type inference fails.</span></span> <span data-ttu-id="958cd-729">Aksi takdirde, tüm `Xi` olan *sabit* karşılık gelen `Si`, tür çıkarımı sonucu olan.</span><span class="sxs-lookup"><span data-stu-id="958cd-729">Otherwise, all `Xi` are *fixed* to corresponding `Si`, which are the result of type inference.</span></span>

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a><span data-ttu-id="958cd-730">Bir ifade kümesi en iyi ortak türü bulma</span><span class="sxs-lookup"><span data-stu-id="958cd-730">Finding the best common type of a set of expressions</span></span>

<span data-ttu-id="958cd-731">Bazı durumlarda, ortak bir türe ifade kümesi için çıkarım gerekir.</span><span class="sxs-lookup"><span data-stu-id="958cd-731">In some cases, a common type needs to be inferred for a set of expressions.</span></span> <span data-ttu-id="958cd-732">Belirli, örtük olarak yazılan diziler öğesi türleri ve dönüş türleri anonim işlevlerle *blok* gövdeleri bu şekilde bulundu.</span><span class="sxs-lookup"><span data-stu-id="958cd-732">In particular, the element types of implicitly typed arrays and the return types of anonymous functions with *block* bodies are found in this way.</span></span>

<span data-ttu-id="958cd-733">Sezgisel, bir ifade kümesi verilen `E1...Em` bu çıkarım bir yöntem çağrısına eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="958cd-733">Intuitively, given a set of expressions `E1...Em` this inference should be equivalent to calling a method</span></span>
```csharp
Tr M<X>(X x1 ... X xm)
```
<span data-ttu-id="958cd-734">ile `Ei` bağımsız değişkenler olarak.</span><span class="sxs-lookup"><span data-stu-id="958cd-734">with the `Ei` as arguments.</span></span>

<span data-ttu-id="958cd-735">Daha kesin çıkarımı başlar bir *sabitlenmemiş* tür değişkeni `X`.</span><span class="sxs-lookup"><span data-stu-id="958cd-735">More precisely, the inference starts out with an *unfixed* type variable `X`.</span></span> <span data-ttu-id="958cd-736">*Çıkış türü çıkarımı* ardından yapılan *gelen* her `Ei` *için* `X`.</span><span class="sxs-lookup"><span data-stu-id="958cd-736">*Output type inferences* are then made *from* each `Ei` *to* `X`.</span></span> <span data-ttu-id="958cd-737">Son olarak, `X` olduğu *sabit* ve başarılı olursa, sonuç türü `S` elde edilen en yaygın tür ifadeler için.</span><span class="sxs-lookup"><span data-stu-id="958cd-737">Finally, `X` is *fixed* and, if successful, the resulting type `S` is the resulting best common type for the expressions.</span></span> <span data-ttu-id="958cd-738">Eğer böyle `S` yoksa, en iyi ortak tür ifadeleri sahip.</span><span class="sxs-lookup"><span data-stu-id="958cd-738">If no such `S` exists, the expressions have no best common type.</span></span>

### <a name="overload-resolution"></a><span data-ttu-id="958cd-739">Aşırı yükleme çözümlemesi</span><span class="sxs-lookup"><span data-stu-id="958cd-739">Overload resolution</span></span>

<span data-ttu-id="958cd-740">Aşırı yükleme çözünürlüğü, bir bağımsız değişken listesi ve aday işlevi üyelerin kümesini çağırmak için en iyi işlevi üye seçme bir bağlama zamanı mekanizmadır.</span><span class="sxs-lookup"><span data-stu-id="958cd-740">Overload resolution is a binding-time mechanism for selecting the best function member to invoke given an argument list and a set of candidate function members.</span></span> <span data-ttu-id="958cd-741">Aşırı yükleme çözünürlüğü, C# içinde aşağıdaki ayrı bağlamlarda çağrılacak işlev üyesi seçer:</span><span class="sxs-lookup"><span data-stu-id="958cd-741">Overload resolution selects the function member to invoke in the following distinct contexts within C#:</span></span>

*  <span data-ttu-id="958cd-742">Adlı bir yöntemin çağrı bir *invocation_expression* ([yöntem çağrıları](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="958cd-742">Invocation of a method named in an *invocation_expression* ([Method invocations](expressions.md#method-invocations)).</span></span>
*  <span data-ttu-id="958cd-743">Adlı bir örnek oluşturucusunda çağrılmasını bir *object_creation_expression* ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="958cd-743">Invocation of an instance constructor named in an *object_creation_expression* ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>
*  <span data-ttu-id="958cd-744">Çağırma bir dizin oluşturucu erişimcisinin bir *element_access* ([öğe erişimi](expressions.md#element-access)).</span><span class="sxs-lookup"><span data-stu-id="958cd-744">Invocation of an indexer accessor through an *element_access* ([Element access](expressions.md#element-access)).</span></span>
*  <span data-ttu-id="958cd-745">Bir deyimde başvurulan önceden tanımlı veya kullanıcı tanımlı işleç çağırmayı ([birli işleç aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution) ve [ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="958cd-745">Invocation of a predefined or user-defined operator referenced in an expression ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)).</span></span>

<span data-ttu-id="958cd-746">Her biri bu içerikler aday işlevi üyelerin kümesini ve bağımsız değişkenler listesi kendi benzersiz bir yolla, yukarıda listelenen bölümlerde ayrıntılı olarak açıklandığı gibi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="958cd-746">Each of these contexts defines the set of candidate function members and the list of arguments in its own unique way, as described in detail in the sections listed above.</span></span> <span data-ttu-id="958cd-747">Örneğin, bir yöntem çağırma için aday kümesini işaretlenmiş yöntemler içermez `override` ([üye araması](expressions.md#member-lookup)), ve bir taban sınıf yöntemlerini olmayan adaylar türetilmiş bir sınıf içinde herhangi bir yöntemi geçerli ise ([ Yöntem çağrıları](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="958cd-747">For example, the set of candidates for a method invocation does not include methods marked `override` ([Member lookup](expressions.md#member-lookup)), and methods in a base class are not candidates if any method in a derived class is applicable ([Method invocations](expressions.md#method-invocations)).</span></span>

<span data-ttu-id="958cd-748">Aday işlev üyeleri ve bağımsız değişken listesi uyguladığınızı belirledikten sonra en iyi işlevi üye seçimi tüm durumlarda aynıdır:</span><span class="sxs-lookup"><span data-stu-id="958cd-748">Once the candidate function members and the argument list have been identified, the selection of the best function member is the same in all cases:</span></span>

*  <span data-ttu-id="958cd-749">Kümesi bulunur, uygun aday işlevi üyelerin kümesini göz önünde bulundurulduğunda, en iyi üye işlevi.</span><span class="sxs-lookup"><span data-stu-id="958cd-749">Given the set of applicable candidate function members, the best function member in that set is located.</span></span> <span data-ttu-id="958cd-750">Set işlevi yalnızca bir üye içeriyorsa, ardından işlevi üyenin en iyi işlevi üyesidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-750">If the set contains only one function member, then that function member is the best function member.</span></span> <span data-ttu-id="958cd-751">Aksi takdirde, en iyi işlevi şartıyla her işlev üyesi kuralları kullanarak diğer işlev üyeleri karşılaştırılır, belirtilen bağımsız değişken listesi ile ilgili diğer tüm işlev üyeleri daha iyi bir işlevin üye üyesidir [ Daha iyi işlevi üye](expressions.md#better-function-member).</span><span class="sxs-lookup"><span data-stu-id="958cd-751">Otherwise, the best function member is the one function member that is better than all other function members with respect to the given argument list, provided that each function member is compared to all other function members using the rules in [Better function member](expressions.md#better-function-member).</span></span> <span data-ttu-id="958cd-752">Tam olarak diğer tüm işlev üyeleri iyi bir işlev üyesi ise, işlev üye çağrısı belirsiz olduğu ve bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-752">If there is not exactly one function member that is better than all other function members, then the function member invocation is ambiguous and a binding-time error occurs.</span></span>

<span data-ttu-id="958cd-753">Aşağıdaki bölümlerde tam anlamları koşullarını tanımlamak ***geçerli işlev üyesi*** ve ***daha iyi işlevi üye***.</span><span class="sxs-lookup"><span data-stu-id="958cd-753">The following sections define the exact meanings of the terms ***applicable function member*** and ***better function member***.</span></span>

#### <a name="applicable-function-member"></a><span data-ttu-id="958cd-754">Geçerli işlev üyesi</span><span class="sxs-lookup"><span data-stu-id="958cd-754">Applicable function member</span></span>

<span data-ttu-id="958cd-755">Bir işlevin üyesi olduğu söylenir bir ***geçerli işlev üyesi*** bağımsız değişken listesine göre `A` şunların hepsini olduğunda true:</span><span class="sxs-lookup"><span data-stu-id="958cd-755">A function member is said to be an ***applicable function member*** with respect to an argument list `A` when all of the following are true:</span></span>

*  <span data-ttu-id="958cd-756">Her bağımsız değişkende `A` bir parametreye işlev üyesi bildiriminde açıklandığı karşılık gelen [karşılık gelen parametreleri](expressions.md#corresponding-parameters), ve herhangi bir parametre hiçbir bağımsız değişken karşılık gelen bir isteğe bağlı bir parametredir.</span><span class="sxs-lookup"><span data-stu-id="958cd-756">Each argument in `A` corresponds to a parameter in the function member declaration as described in [Corresponding parameters](expressions.md#corresponding-parameters), and any parameter to which no argument corresponds is an optional parameter.</span></span>
*  <span data-ttu-id="958cd-757">Her bağımsız değişkeni için `A`, parametre modunu bağımsız değişken geçirme (yani, değer `ref`, veya `out`) karşılık gelen parametre, parametre geçirme moduna aynıdır ve</span><span class="sxs-lookup"><span data-stu-id="958cd-757">For each argument in `A`, the parameter passing mode of the argument (i.e., value, `ref`, or `out`) is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="958cd-758">bir değer parametresini veya bir parametre dizisi, örtük bir dönüştürme ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) karşılık gelen parametre türüne bağımsız değişkende var veya</span><span class="sxs-lookup"><span data-stu-id="958cd-758">for a value parameter or a parameter array, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="958cd-759">için bir `ref` veya `out` parametresi, bağımsız değişken türünü karşılık gelen parametre türü için aynı.</span><span class="sxs-lookup"><span data-stu-id="958cd-759">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span> <span data-ttu-id="958cd-760">Sonuçta bir `ref` veya `out` parametredir geçirilen bağımsız değişken için bir diğer ad.</span><span class="sxs-lookup"><span data-stu-id="958cd-760">After all, a `ref` or `out` parameter is an alias for the argument passed.</span></span>

<span data-ttu-id="958cd-761">Bir parametre dizisi içeren işlev üyesi için yukarıdaki kurallar tarafından işlevi üyesi olup olmadığını, uygulanabilir olduğu söylenir kendi ***normal form***.</span><span class="sxs-lookup"><span data-stu-id="958cd-761">For a function member that includes a parameter array, if the function member is applicable by the above rules, it is said to be applicable in its ***normal form***.</span></span> <span data-ttu-id="958cd-762">Bir parametre dizisi içeren işlev üyesi, normal bir biçimde geçerli değilse, işlev üye bunun yerine uygulanabilir olabilir, ***genişletilmiş form***:</span><span class="sxs-lookup"><span data-stu-id="958cd-762">If a function member that includes a parameter array is not applicable in its normal form, the function member may instead be applicable in its ***expanded form***:</span></span>

*  <span data-ttu-id="958cd-763">Genişletilmiş formun işlev üyesi bildiriminde parametre dizisi sıfır ile değiştirilerek oluşturulur veya daha fazla değer parametre parametresinin öğe türünün gibi dizi bu bağımsız değişken listesindeki bağımsız değişken sayısı `A` toplam eşleşir parametre sayısı.</span><span class="sxs-lookup"><span data-stu-id="958cd-763">The expanded form is constructed by replacing the parameter array in the function member declaration with zero or more value parameters of the element type of the parameter array such that the number of arguments in the argument list `A` matches the total number of parameters.</span></span> <span data-ttu-id="958cd-764">Varsa `A` işlevi üyesi bildiriminde sabit parametre sayısından daha az sayıda bağımsız değişken varsa, genişletilmiş formun işlevi üyesinin oluşturulamaz ve bu nedenle geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="958cd-764">If `A` has fewer arguments than the number of fixed parameters in the function member declaration, the expanded form of the function member cannot be constructed and is thus not applicable.</span></span>
*  <span data-ttu-id="958cd-765">Aksi takdirde, genişletilmiş form, her değişken için geçerli `A` bağımsız değişken geçirme mode parametresi için karşılık gelen parametre, parametre geçirme modu aynıdır ve</span><span class="sxs-lookup"><span data-stu-id="958cd-765">Otherwise, the expanded form is applicable if for each argument in `A` the parameter passing mode of the argument is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="958cd-766">bir sabit değer parametresi veya örtük bir dönüştürme genişletmesi tarafından oluşturulan bir değer parametresini ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) karşılık gelen parametresinin türü için bağımsız değişkenin türünden var veya</span><span class="sxs-lookup"><span data-stu-id="958cd-766">for a fixed value parameter or a value parameter created by the expansion, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the type of the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="958cd-767">için bir `ref` veya `out` parametresi, bağımsız değişken türünü karşılık gelen parametre türü için aynı.</span><span class="sxs-lookup"><span data-stu-id="958cd-767">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span>

#### <a name="better-function-member"></a><span data-ttu-id="958cd-768">Daha iyi işlev üyesi</span><span class="sxs-lookup"><span data-stu-id="958cd-768">Better function member</span></span>

<span data-ttu-id="958cd-769">Daha iyi işlevi üye belirleme amacıyla, yalnızca bağımsız değişken ifadeleri kendilerini özgün bağımsız değişken listesinde göründükleri sırayla içeren stripped-down bağımsız değişken listesini bir oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-769">For the purposes of determining the better function member, a stripped-down argument list A is constructed containing just the argument expressions themselves in the order they appear in the original argument list.</span></span>

<span data-ttu-id="958cd-770">Her üye şu şekilde oluşturulur aday işlevi için parametre listeler:</span><span class="sxs-lookup"><span data-stu-id="958cd-770">Parameter lists for each of the candidate function members are constructed in the following way:</span></span>

*  <span data-ttu-id="958cd-771">Üye işlevi yalnızca genişletilmiş biçiminde geçerli genişletilmiş formun kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-771">The expanded form is used if the function member was applicable only in the expanded form.</span></span>
*  <span data-ttu-id="958cd-772">Karşılık gelen bağımsız değişken içermeyen isteğe bağlı parametreler parametre listesinden kaldırılır</span><span class="sxs-lookup"><span data-stu-id="958cd-772">Optional parameters with no corresponding arguments are removed from the parameter list</span></span>
*  <span data-ttu-id="958cd-773">Bunlar bağımsız değişken listesindeki aynı konuma karşılık gelen bağımsız değişken olarak gerçekleşmesi parametreleri yeniden sıralanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-773">The parameters are reordered so that they occur at the same position as the corresponding argument in the argument list.</span></span>

<span data-ttu-id="958cd-774">Bir bağımsız değişken listesi verilen `A` bağımsız değişken ifadeleri kümesiyle `{E1, E2, ..., En}` ve iki geçerli işlev üyeleri `Mp` ve `Mq` parametre türleri ile `{P1, P2, ..., Pn}` ve `{Q1, Q2, ..., Qn}`, `Mp` olarak tanımlanır bir ***daha iyi işlevi üye*** daha `Mq` varsa</span><span class="sxs-lookup"><span data-stu-id="958cd-774">Given an argument list `A` with a set of argument expressions `{E1, E2, ..., En}` and two applicable function members `Mp` and `Mq` with parameter types `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}`, `Mp` is defined to be a ***better function member*** than `Mq` if</span></span>

*  <span data-ttu-id="958cd-775">Her bir bağımsız değişkeni, arasında örtük dönüşüm `Ex` için `Qx` arasında örtük dönüşüm daha iyi değil `Ex` için `Px`, ve</span><span class="sxs-lookup"><span data-stu-id="958cd-775">for each argument, the implicit conversion from `Ex` to `Qx` is not better than the implicit conversion from `Ex` to `Px`, and</span></span>
*  <span data-ttu-id="958cd-776">en az bir bağımsız değişkeni, dönüştürme `Ex` için `Px` dönüştürme işlemi daha iyidir `Ex` için `Qx`.</span><span class="sxs-lookup"><span data-stu-id="958cd-776">for at least one argument, the conversion from `Ex` to `Px` is better than the conversion from `Ex` to `Qx`.</span></span>

<span data-ttu-id="958cd-777">Bu değerlendirme, gerçekleştirirken `Mp` veya `Mq` genişletilmiş hâli içinde geçerli ise `Px` veya `Qx` parametre listesi genişletilmiş biçiminde bir parametreye başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="958cd-777">When performing this evaluation, if `Mp` or `Mq` is applicable in its expanded form, then `Px` or `Qx` refers to a parameter in the expanded form of the parameter list.</span></span>

<span data-ttu-id="958cd-778">Tür parametresi dizileri durumunda `{P1, P2, ..., Pn}` ve `{Q1, Q2, ..., Qn}` eşdeğerdir (yani her `Pi` karşılık gelen bir kimlik dönüştürme sahip `Qi`), aşağıdaki KRAVAT sonu kuralları uygulanır, daha iyi belirlemek için sırayla üye işlevi.</span><span class="sxs-lookup"><span data-stu-id="958cd-778">In case the parameter type sequences `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}` are equivalent (i.e. each `Pi` has an identity conversion to the corresponding `Qi`), the following tie-breaking rules are applied, in order, to determine the better function member.</span></span>

*  <span data-ttu-id="958cd-779">Varsa `Mp` genel olmayan bir yöntem ve `Mq` genel bir yöntem ise `Mp` göre daha iyidir `Mq`.</span><span class="sxs-lookup"><span data-stu-id="958cd-779">If `Mp` is a non-generic method and `Mq` is a generic method, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="958cd-780">Aksi takdirde `Mp` , normal bir biçimde uygulanabilir ve `Mq` sahip bir `params` dizisi ve ardından genişletilmiş biçimde yalnızca kendi geçerlidir `Mp` göre daha iyidir `Mq`.</span><span class="sxs-lookup"><span data-stu-id="958cd-780">Otherwise, if `Mp` is applicable in its normal form and `Mq` has a `params` array and is applicable only in its expanded form, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="958cd-781">Aksi takdirde `Mp` daha fazla parametre gözükeceğini `Mq`, ardından `Mp` göre daha iyidir `Mq`.</span><span class="sxs-lookup"><span data-stu-id="958cd-781">Otherwise, if `Mp` has more declared parameters than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="958cd-782">Her iki yöntem de sahip olmadığında ortaya çıkabilir `params` diziler ve bu yalnızca genişletilmiş formlarında uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-782">This can occur if both methods have `params` arrays and are applicable only in their expanded forms.</span></span>
*  <span data-ttu-id="958cd-783">Aksi halde, tüm parametreleri `Mp` varsayılan bağımsız değişkenler için en az bir isteğe bağlı bir parametre atanması gerekiyor ancak karşılık gelen bir bağımsız değişken olan `Mq` ardından `Mp` göre daha iyidir `Mq`.</span><span class="sxs-lookup"><span data-stu-id="958cd-783">Otherwise if all parameters of `Mp` have a corresponding argument whereas default arguments need to be substituted for at least one optional parameter in `Mq` then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="958cd-784">Aksi takdirde `Mp` daha fazla belirli parametre türleri vardır `Mq`, ardından `Mp` göre daha iyidir `Mq`.</span><span class="sxs-lookup"><span data-stu-id="958cd-784">Otherwise, if `Mp` has more specific parameter types than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="958cd-785">İzin `{R1, R2, ..., Rn}` ve `{S1, S2, ..., Sn}` örneklenmemiş ve genişletilmemiş parametre türleri temsil eden `Mp` ve `Mq`.</span><span class="sxs-lookup"><span data-stu-id="958cd-785">Let `{R1, R2, ..., Rn}` and `{S1, S2, ..., Sn}` represent the uninstantiated and unexpanded parameter types of `Mp` and `Mq`.</span></span> <span data-ttu-id="958cd-786">`Mp`ın parametre türleri daha belirli `Mq`ait ise, her bir parametreye `Rx` less yazımına özgü değil `Sx`ve en az bir parametre `Rx` daha belirli olduğundan `Sx`:</span><span class="sxs-lookup"><span data-stu-id="958cd-786">`Mp`'s parameter types are more specific than `Mq`'s if, for each parameter, `Rx` is not less specific than `Sx`, and, for at least one parameter, `Rx` is more specific than `Sx`:</span></span>
   *  <span data-ttu-id="958cd-787">Bir tür olmayan parametresi daha az belirli bir tür parametresidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-787">A type parameter is less specific than a non-type parameter.</span></span>
   *  <span data-ttu-id="958cd-788">Yinelemeli olarak oluşturulmuş bir türü en az bir bağımsız değişken türü (tür bağımsız değişkenleri aynı sayıda ile) oluşturulan başka bir tür daha özeldir ve hiçbir tür bağımsız değişkeni karşılık gelen tür bağımsız değişkeni diğer less yazımına özgü daha ayrıntılı olarak.</span><span class="sxs-lookup"><span data-stu-id="958cd-788">Recursively, a constructed type is more specific than another constructed type (with the same number of type arguments) if at least one type argument is more specific and no type argument is less specific than the corresponding type argument in the other.</span></span>
   *  <span data-ttu-id="958cd-789">İlk öğe türü ikinci öğesi türünden daha belirli ise bir dizi türü (aynı boyut sayısı ile) başka bir dizi türü daha belirgin olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-789">An array type is more specific than another array type (with the same number of dimensions) if the element type of the first is more specific than the element type of the second.</span></span>
*  <span data-ttu-id="958cd-790">Aksi takdirde yükseltilmiş bir işleci bir üyesidir ve diğer lifted işleci ise, yükseltilmiş bir iyidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-790">Otherwise if one member is a non-lifted operator and  the other is a lifted operator, the non-lifted one is better.</span></span>
*  <span data-ttu-id="958cd-791">Aksi takdirde, hiçbir işlev üyesi daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-791">Otherwise, neither function member is better.</span></span>

#### <a name="better-conversion-from-expression"></a><span data-ttu-id="958cd-792">İfadeden daha iyi dönüştürme</span><span class="sxs-lookup"><span data-stu-id="958cd-792">Better conversion from expression</span></span>

<span data-ttu-id="958cd-793">Örtük bir dönüştürme verilen `C1` ifadeden dönüştüren `E` türüne `T1`ve örtük bir dönüştürme `C2` ifadeden dönüştüren `E` türüne `T2`, `C1` olan bir ***daha iyi bir dönüştürme*** daha `C2` varsa `E` tam olarak eşleşmiyor `T2` ve aşağıdakilerden birini barındırır:</span><span class="sxs-lookup"><span data-stu-id="958cd-793">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>

* <span data-ttu-id="958cd-794">`E` tam olarak eşleşen `T1` ([ifadesi tam olarak eşleşen](expressions.md#exactly-matching-expression))</span><span class="sxs-lookup"><span data-stu-id="958cd-794">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
* <span data-ttu-id="958cd-795">`T1` daha iyi bir dönüştürme hedefi `T2` ([daha iyi dönüştürme hedef](expressions.md#better-conversion-target))</span><span class="sxs-lookup"><span data-stu-id="958cd-795">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target))</span></span>

#### <a name="exactly-matching-expression"></a><span data-ttu-id="958cd-796">Tam olarak eşleşen bir ifade</span><span class="sxs-lookup"><span data-stu-id="958cd-796">Exactly matching Expression</span></span>

<span data-ttu-id="958cd-797">Bir ifade verilen `E` ve türü `T`, `E` tam olarak eşleşen `T` tutuyorsa aşağıdakilerden biri:</span><span class="sxs-lookup"><span data-stu-id="958cd-797">Given an expression `E` and a type `T`, `E` exactly matches `T` if one of the following holds:</span></span>

*  <span data-ttu-id="958cd-798">`E` bir türe sahip `S`, ve gelen bir kimlik dönüştürme var `S` için `T`</span><span class="sxs-lookup"><span data-stu-id="958cd-798">`E` has a type `S`, and an identity conversion exists from `S` to `T`</span></span>
*  <span data-ttu-id="958cd-799">`E` Anonim bir işlev ise `T` bir temsilci türü `D` veya bir ifade ağacı türü `Expression<D>` ve aşağıdakilerden birini barındırır:</span><span class="sxs-lookup"><span data-stu-id="958cd-799">`E` is an anonymous function, `T` is either a delegate type `D` or an expression tree type `Expression<D>` and one of the following holds:</span></span>
   *  <span data-ttu-id="958cd-800">Çıkarılan bir dönüş türü `X` yok `E` parametre listesini bağlamında `D` ([Inferred dönüş türü](expressions.md#inferred-return-type)), ve gelen bir kimlik dönüştürme var `X` için dönüş türü `D`</span><span class="sxs-lookup"><span data-stu-id="958cd-800">An inferred return type `X` exists for `E` in the context of the parameter list of `D` ([Inferred return type](expressions.md#inferred-return-type)), and an identity conversion exists from `X` to the return type of `D`</span></span>
   *  <span data-ttu-id="958cd-801">Her iki `E` olmayan zaman uyumsuz olduğunu ve `D` dönüş türü olan `Y` veya `E` zaman uyumsuz olduğunu ve `D` dönüş türü olan `Task<Y>`, ve aşağıdakilerden birini barındırır:</span><span class="sxs-lookup"><span data-stu-id="958cd-801">Either `E` is non-async and `D` has a return type `Y` or `E` is async and `D` has a return type `Task<Y>`, and one of the following holds:</span></span>
      * <span data-ttu-id="958cd-802">Gövdesi `E` bir ifade, tam olarak eşleşme. `Y`</span><span class="sxs-lookup"><span data-stu-id="958cd-802">The body of `E` is an expression that exactly matches `Y`</span></span>
      * <span data-ttu-id="958cd-803">Gövdesi `E` her return deyimi bu tam olarak eşleşen bir ifade döndüğü bir deyim bloğunu olduğu `Y`</span><span class="sxs-lookup"><span data-stu-id="958cd-803">The body of `E` is a statement block where every return statement returns an expression that exactly matches `Y`</span></span>

#### <a name="better-conversion-target"></a><span data-ttu-id="958cd-804">Daha iyi dönüştürme hedef</span><span class="sxs-lookup"><span data-stu-id="958cd-804">Better conversion target</span></span>

<span data-ttu-id="958cd-805">Verilen iki `T1` ve `T2`, `T1` daha iyi bir dönüştürme hedefi `T2` örtülü dönüştürme olmaz, `T2` için `T1` var ve aşağıdakilerden birini barındırır:</span><span class="sxs-lookup"><span data-stu-id="958cd-805">Given two different types `T1` and `T2`, `T1` is a better conversion target than `T2` if no implicit conversion from `T2` to `T1` exists, and at least one of the following holds:</span></span>

*  <span data-ttu-id="958cd-806">Örtük bir dönüştürme `T1` için `T2` var.</span><span class="sxs-lookup"><span data-stu-id="958cd-806">An implicit conversion from `T1` to `T2` exists</span></span>
*  <span data-ttu-id="958cd-807">`T1` bir temsilci türü `D1` veya bir ifade ağacı türü `Expression<D1>`, `T2` bir temsilci türü `D2` veya bir ifade ağacı türü `Expression<D2>`, `D1` dönüş türü olan `S1` ve biri Aşağıdaki tutar:</span><span class="sxs-lookup"><span data-stu-id="958cd-807">`T1` is either a delegate type `D1` or an expression tree type `Expression<D1>`, `T2` is either a delegate type `D2` or an expression tree type `Expression<D2>`, `D1` has a return type `S1` and one of the following holds:</span></span>
   * <span data-ttu-id="958cd-808">`D2` void döndüren</span><span class="sxs-lookup"><span data-stu-id="958cd-808">`D2` is void returning</span></span>
   * <span data-ttu-id="958cd-809">`D2` dönüş türü olan `S2`, ve `S1` daha iyi bir dönüştürme hedefi `S2`</span><span class="sxs-lookup"><span data-stu-id="958cd-809">`D2` has a return type `S2`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="958cd-810">`T1` olan `Task<S1>`, `T2` olduğu `Task<S2>`, ve `S1` daha iyi bir dönüştürme hedefi `S2`</span><span class="sxs-lookup"><span data-stu-id="958cd-810">`T1` is `Task<S1>`, `T2` is `Task<S2>`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="958cd-811">`T1` olan `S1` veya `S1?` burada `S1` işaretli bir integral türü ve `T2` olduğu `S2` veya `S2?` burada `S2` işaretsiz bir tamsayı türüdür.</span><span class="sxs-lookup"><span data-stu-id="958cd-811">`T1` is `S1` or `S1?` where `S1` is a signed integral type, and `T2` is `S2` or `S2?` where `S2` is an unsigned integral type.</span></span> <span data-ttu-id="958cd-812">Özellikle:</span><span class="sxs-lookup"><span data-stu-id="958cd-812">Specifically:</span></span>
   * <span data-ttu-id="958cd-813">`S1` olan `sbyte` ve `S2` olduğu `byte`, `ushort`, `uint`, veya `ulong`</span><span class="sxs-lookup"><span data-stu-id="958cd-813">`S1` is `sbyte` and `S2` is `byte`, `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="958cd-814">`S1` olan `short` ve `S2` olduğu `ushort`, `uint`, veya `ulong`</span><span class="sxs-lookup"><span data-stu-id="958cd-814">`S1` is `short` and `S2` is `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="958cd-815">`S1` olan `int` ve `S2` olduğu `uint`, veya `ulong`</span><span class="sxs-lookup"><span data-stu-id="958cd-815">`S1` is `int` and `S2` is `uint`, or `ulong`</span></span>
   * <span data-ttu-id="958cd-816">`S1` olan `long` ve `S2` olduğu `ulong`</span><span class="sxs-lookup"><span data-stu-id="958cd-816">`S1` is `long` and `S2` is `ulong`</span></span>

#### <a name="overloading-in-generic-classes"></a><span data-ttu-id="958cd-817">Genel sınıflar aşırı yükleme</span><span class="sxs-lookup"><span data-stu-id="958cd-817">Overloading in generic classes</span></span>

<span data-ttu-id="958cd-818">Bildirilen imzaları benzersiz olmasını sağlarken tür bağımsız değişkenlerini değiştirme içinde aynı imzaya sonuçları mümkündür.</span><span class="sxs-lookup"><span data-stu-id="958cd-818">While signatures as declared must be unique, it is possible that substitution of type arguments results in identical signatures.</span></span> <span data-ttu-id="958cd-819">Bu gibi durumlarda, yukarıdaki aşırı yükleme çözünürlüğü KRAVAT sonu kurallarını en belirgin üye seçer.</span><span class="sxs-lookup"><span data-stu-id="958cd-819">In such cases, the tie-breaking rules of overload resolution above will pick the most specific member.</span></span>

<span data-ttu-id="958cd-820">Aşağıdaki örnekler, geçerli ve bu kuralına göre geçersiz aşırı yüklemeleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="958cd-820">The following examples show overloads that are valid and invalid according to this rule:</span></span>

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

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a><span data-ttu-id="958cd-821">Derleme zamanı dinamik aşırı yükleme çözünürlüğü denetleniyor</span><span class="sxs-lookup"><span data-stu-id="958cd-821">Compile-time checking of dynamic overload resolution</span></span>

<span data-ttu-id="958cd-822">En dinamik olarak bağlı işlemler için olası adaylar çözümlemesi için derleme zamanında bilinmeyen kümesidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-822">For most dynamically bound operations the set of possible candidates for resolution is unknown at compile-time.</span></span> <span data-ttu-id="958cd-823">Bazı durumlarda, ancak aday kümesi derleme zamanında bilinen:</span><span class="sxs-lookup"><span data-stu-id="958cd-823">In certain cases, however the candidate set is known at compile-time:</span></span>

*  <span data-ttu-id="958cd-824">Dinamik bağımsız değişkenlere sahip statik yöntem çağrıları</span><span class="sxs-lookup"><span data-stu-id="958cd-824">Static method calls with dynamic arguments</span></span>
*  <span data-ttu-id="958cd-825">Örnek yöntemi çağrılarında nerede alıcı dinamik bir ifade değil</span><span class="sxs-lookup"><span data-stu-id="958cd-825">Instance method calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="958cd-826">Dizin Oluşturucu çağrıları nerede alıcı dinamik bir ifade değil</span><span class="sxs-lookup"><span data-stu-id="958cd-826">Indexer calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="958cd-827">Dinamik bağımsız değişkenlere sahip Oluşturucu çağrı</span><span class="sxs-lookup"><span data-stu-id="958cd-827">Constructor calls with dynamic arguments</span></span>

<span data-ttu-id="958cd-828">Bu gibi durumlarda, bunlardan herhangi birinin büyük olasılıkla çalışma zamanında uygulanabilir değilse görmek her adayı için sınırlı bir derleme zamanı denetimi gerçekleştirilir. Bu onay, aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="958cd-828">In these cases a limited compile-time check is performed for each candidate to see if any of them could possibly apply at run-time.This check consists of the following steps:</span></span>

*  <span data-ttu-id="958cd-829">Kısmi tür çıkarımı: herhangi bir tür doğrudan veya dolaylı olarak türünde bir bağımsız değişken üzerinde bağlı olmayan bağımsız değişkeni `dynamic` kurallarını kullanarak algılanır [anlam çıkarma](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="958cd-829">Partial type inference: Any type argument that does not depend directly or indirectly on an argument of type `dynamic` is inferred using the rules of [Type inference](expressions.md#type-inference).</span></span> <span data-ttu-id="958cd-830">Kalan tür bağımsız değişkeni bilinmeyen.</span><span class="sxs-lookup"><span data-stu-id="958cd-830">The remaining type arguments are unknown.</span></span>
*  <span data-ttu-id="958cd-831">Kısmi Uygulanabilirlik denetimi: Uygulanabilirlik göre denetlenir [geçerli işlev üyesi](expressions.md#applicable-function-member), ancak eşleşen türleri bilinmeyen parametreler yoksayılıyor.</span><span class="sxs-lookup"><span data-stu-id="958cd-831">Partial applicability check: Applicability is checked according to [Applicable function member](expressions.md#applicable-function-member), but ignoring parameters whose types are unknown.</span></span>
*  <span data-ttu-id="958cd-832">Hiçbir aday bu testi başarılı olursa, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-832">If no candidate passes this test, a compile-time error occurs.</span></span>

### <a name="function-member-invocation"></a><span data-ttu-id="958cd-833">İşlev üye çağrısı</span><span class="sxs-lookup"><span data-stu-id="958cd-833">Function member invocation</span></span>

<span data-ttu-id="958cd-834">Bu bölümde belirli işlev üye çağırmak için çalışma zamanında gerçekleşen işlemi açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="958cd-834">This section describes the process that takes place at run-time to invoke a particular function member.</span></span> <span data-ttu-id="958cd-835">Bağlama zamanı işlemini çağırmak için çözüm candidate işlev üyeleri bir dizi uygulayarak büyük olasılıkla aşırı belirli üye zaten belirledi varsayılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-835">It is assumed that a binding-time process has already determined the particular member to invoke, possibly by applying overload resolution to a set of candidate function members.</span></span>

<span data-ttu-id="958cd-836">Çağırma işlemi açıklamak amacıyla işlev üyeleri iki kategoriye ayrılır:</span><span class="sxs-lookup"><span data-stu-id="958cd-836">For purposes of describing the invocation process, function members are divided into two categories:</span></span>

*  <span data-ttu-id="958cd-837">Statik işlev üyeleri.</span><span class="sxs-lookup"><span data-stu-id="958cd-837">Static function members.</span></span> <span data-ttu-id="958cd-838">Örnek oluşturucuları, statik yöntemler, statik özellik erişimcileri ve kullanıcı tanımlı işleçler şunlardır.</span><span class="sxs-lookup"><span data-stu-id="958cd-838">These are instance constructors, static methods, static property accessors, and user-defined operators.</span></span> <span data-ttu-id="958cd-839">Statik işlev her zaman sanal olmayan üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="958cd-839">Static function members are always non-virtual.</span></span>
*  <span data-ttu-id="958cd-840">Örnek işlev üyeleri.</span><span class="sxs-lookup"><span data-stu-id="958cd-840">Instance function members.</span></span> <span data-ttu-id="958cd-841">Bu örnek yöntemleri, örnek özellik erişimcileri ve dizin oluşturucu erişimcileri ücretlerdir.</span><span class="sxs-lookup"><span data-stu-id="958cd-841">These are instance methods, instance property accessors, and indexer accessors.</span></span> <span data-ttu-id="958cd-842">Örnek işlev üyeleri, sanal olmayan ya da sanal ve her zaman belirli bir örneği üzerinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-842">Instance function members are either non-virtual or virtual, and are always invoked on a particular instance.</span></span> <span data-ttu-id="958cd-843">Örnek bir örnek ifade Hesaplandı ve işlev üyesi olarak içinde erişilebilir duruma gelir `this` ([bu erişim](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="958cd-843">The instance is computed by an instance expression, and it becomes accessible within the function member as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="958cd-844">Çalışma zamanı işleme işlevi üye çağırmanın, aşağıdaki adımlardan oluşur burada `M` işlevi üyesidir ve `M` bir örnek üyesi `E` örnek ifade:</span><span class="sxs-lookup"><span data-stu-id="958cd-844">The run-time processing of a function member invocation consists of the following steps, where `M` is the function member and, if `M` is an instance member, `E` is the instance expression:</span></span>

*  <span data-ttu-id="958cd-845">Varsa `M` statik işlev üye:</span><span class="sxs-lookup"><span data-stu-id="958cd-845">If `M` is a static function member:</span></span>
   * <span data-ttu-id="958cd-846">Bağımsız değişken listesi açıklandığı gibi değerlendirilir [bağımsız değişken listeleri](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="958cd-846">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="958cd-847">`M` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-847">`M` is invoked.</span></span>

*  <span data-ttu-id="958cd-848">Varsa `M` örnek işlevi üyesine içinde bildirilen bir *value_type*:</span><span class="sxs-lookup"><span data-stu-id="958cd-848">If `M` is an instance function member declared in a *value_type*:</span></span>
   * <span data-ttu-id="958cd-849">`E` değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-849">`E` is evaluated.</span></span> <span data-ttu-id="958cd-850">Bu değerlendirme bir özel durum neden olursa, başka bir adım daha sonra yürütülür.</span><span class="sxs-lookup"><span data-stu-id="958cd-850">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="958cd-851">Varsa `E` bir değişken, ardından geçici bir yerel değişken sınıflandırılmaz `E`ın türü oluşturulur ve değerini `E` bu değişkenine atanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-851">If `E` is not classified as a variable, then a temporary local variable of `E`'s type is created and the value of `E` is assigned to that variable.</span></span> <span data-ttu-id="958cd-852">`E` Ardından, geçici yerel değişken başvuru olarak yeniden sınıflandırılması.</span><span class="sxs-lookup"><span data-stu-id="958cd-852">`E` is then reclassified as a reference to that temporary local variable.</span></span> <span data-ttu-id="958cd-853">Geçici değişken olarak erişilebilir `this` içinde `M`, ancak başka hiçbir şekilde.</span><span class="sxs-lookup"><span data-stu-id="958cd-853">The temporary variable is accessible as `this` within `M`, but not in any other way.</span></span> <span data-ttu-id="958cd-854">Bu nedenle, yalnızca zaman `E` true değişkeni çağırıcı değişiklikleri gözlemlemek için mümkün olduğu, `M` yapar `this`.</span><span class="sxs-lookup"><span data-stu-id="958cd-854">Thus, only when `E` is a true variable is it possible for the caller to observe the changes that `M` makes to `this`.</span></span>
   * <span data-ttu-id="958cd-855">Bağımsız değişken listesi açıklandığı gibi değerlendirilir [bağımsız değişken listeleri](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="958cd-855">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="958cd-856">`M` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-856">`M` is invoked.</span></span> <span data-ttu-id="958cd-857">Tarafından başvurulan değişken `E` tarafından başvurulan değişken olur `this`.</span><span class="sxs-lookup"><span data-stu-id="958cd-857">The variable referenced by `E` becomes the variable referenced by `this`.</span></span>

*  <span data-ttu-id="958cd-858">Varsa `M` örnek işlevi üyesine içinde bildirilen bir *reference_type*:</span><span class="sxs-lookup"><span data-stu-id="958cd-858">If `M` is an instance function member declared in a *reference_type*:</span></span>
   * <span data-ttu-id="958cd-859">`E` değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-859">`E` is evaluated.</span></span> <span data-ttu-id="958cd-860">Bu değerlendirme bir özel durum neden olursa, başka bir adım daha sonra yürütülür.</span><span class="sxs-lookup"><span data-stu-id="958cd-860">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="958cd-861">Bağımsız değişken listesi açıklandığı gibi değerlendirilir [bağımsız değişken listeleri](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="958cd-861">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="958cd-862">Varsa türünü `E` olduğu bir *value_type*, paketleme dönüştürmesi ([kutulama dönüştürmeler](types.md#boxing-conversions)) dönüştürmek için gerçekleştirilen `E` türüne `object`, ve `E` olarak kabul edilir türünde olmasını `object` aşağıdaki adımlarda.</span><span class="sxs-lookup"><span data-stu-id="958cd-862">If the type of `E` is a *value_type*, a boxing conversion ([Boxing conversions](types.md#boxing-conversions)) is performed to convert `E` to type `object`, and `E` is considered to be of type `object` in the following steps.</span></span> <span data-ttu-id="958cd-863">Bu durumda, `M` yalnızca üyesi olabilir `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="958cd-863">In this case, `M` could only be a member of `System.Object`.</span></span>
   * <span data-ttu-id="958cd-864">Değerini `E` geçerli olması için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-864">The value of `E` is checked to be valid.</span></span> <span data-ttu-id="958cd-865">Varsa değerini `E` olduğu `null`, `System.NullReferenceException` oluşturulur ve başka bir adım yürütülür.</span><span class="sxs-lookup"><span data-stu-id="958cd-865">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
   * <span data-ttu-id="958cd-866">Çağrılacak işlev üyesi uygulama belirlenir:</span><span class="sxs-lookup"><span data-stu-id="958cd-866">The function member implementation to invoke is determined:</span></span>
     * <span data-ttu-id="958cd-867">Bağlama zamanı türü `E` bir arabirim, çağrılacak işlev üye uygulamasıdır `M` tarafından başvurulan örnek çalışma zamanı türü tarafından sağlanan `E`.</span><span class="sxs-lookup"><span data-stu-id="958cd-867">If the binding-time type of `E` is an interface, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="958cd-868">Bu işlev üye arabirim eşleme kurallarını uygulayarak belirlenir ([arabirim eşleme](interfaces.md#interface-mapping)) uygulamasını belirlemek için `M` tarafından başvurulan örnek çalışma zamanı türü tarafından sağlanan `E`.</span><span class="sxs-lookup"><span data-stu-id="958cd-868">This function member is determined by applying the interface mapping rules ([Interface mapping](interfaces.md#interface-mapping)) to determine the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="958cd-869">Aksi takdirde `M` sanal işlev üyesi olduğu çağrılacak işlev üye uygulamasıdır `M` tarafından başvurulan örnek çalışma zamanı türü tarafından sağlanan `E`.</span><span class="sxs-lookup"><span data-stu-id="958cd-869">Otherwise, if `M` is a virtual function member, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="958cd-870">Bu işlev üye en türetilen uygulamaya belirlemek için kuralları uygulayarak belirlenir ([sanal yöntemleri](classes.md#virtual-methods)), `M` tarafından başvurulan örnek çalışma zamanı türüne göre `E`.</span><span class="sxs-lookup"><span data-stu-id="958cd-870">This function member is determined by applying the rules for determining the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of `M` with respect to the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="958cd-871">Aksi takdirde, `M` sanal olmayan işlev üyesi olduğu ve çağrılacak işlev üyesidir `M` kendisi.</span><span class="sxs-lookup"><span data-stu-id="958cd-871">Otherwise, `M` is a non-virtual function member, and the function member to invoke is `M` itself.</span></span>
   * <span data-ttu-id="958cd-872">Yukarıdaki adımda belirlenen işlev üyesi uygulama çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-872">The function member implementation determined in the step above is invoked.</span></span> <span data-ttu-id="958cd-873">Tarafından başvurulan nesne `E` tarafından başvurulan nesne haline gelir `this`.</span><span class="sxs-lookup"><span data-stu-id="958cd-873">The object referenced by `E` becomes the object referenced by `this`.</span></span>

#### <a name="invocations-on-boxed-instances"></a><span data-ttu-id="958cd-874">Paketlenmiş örneklerinde etkinleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-874">Invocations on boxed instances</span></span>

<span data-ttu-id="958cd-875">İşlev üyesi uygulanan bir *value_type* , söz konusu kutulanmış örneği çağrılabilir *value_type* aşağıdaki durumlarda:</span><span class="sxs-lookup"><span data-stu-id="958cd-875">A function member implemented in a *value_type* can be invoked through a boxed instance of that *value_type* in the following situations:</span></span>

*  <span data-ttu-id="958cd-876">İşlev üyesi olduğunda bir `override` türden devralınan bir yöntemin `object` ve örnek türündeki bir ifade, çağrılan `object`.</span><span class="sxs-lookup"><span data-stu-id="958cd-876">When the function member is an `override` of a method inherited from type `object` and is invoked through an instance expression of type `object`.</span></span>
*  <span data-ttu-id="958cd-877">Ne zaman işlevi üye işlevi arabirim üyesini uygulamasıdır ve bir örnek ifade çağrılan bir *INTERFACE_TYPE*.</span><span class="sxs-lookup"><span data-stu-id="958cd-877">When the function member is an implementation of an interface function member and is invoked through an instance expression of an *interface_type*.</span></span>
*  <span data-ttu-id="958cd-878">Ne zaman işlev üyesi bir temsilci çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-878">When the function member is invoked through a delegate.</span></span>

<span data-ttu-id="958cd-879">Bu gibi durumlarda paketlenmiş örneği içeren bir değişken değerlendirilir *value_type*, ve bu değişken tarafından başvurulan değişken `this` işlevi üye çağrısı içinde.</span><span class="sxs-lookup"><span data-stu-id="958cd-879">In these situations, the boxed instance is considered to contain a variable of the *value_type*, and this variable becomes the variable referenced by `this` within the function member invocation.</span></span> <span data-ttu-id="958cd-880">Özellikle, bu işlev üyesi paketlenmiş bir örneğinde çağrıldığında, kutulanmış örneğinde yer alan değeri değiştirmek işlev üyesi mümkündür, anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-880">In particular, this means that when a function member is invoked on a boxed instance, it is possible for the function member to modify the value contained in the boxed instance.</span></span>

## <a name="primary-expressions"></a><span data-ttu-id="958cd-881">Birincil ifadeler</span><span class="sxs-lookup"><span data-stu-id="958cd-881">Primary expressions</span></span>

<span data-ttu-id="958cd-882">Birincil ifadeler basit formlar ifadeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="958cd-882">Primary expressions include the simplest forms of expressions.</span></span>

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

<span data-ttu-id="958cd-883">Birincil ifadeler arasında bölünen *array_creation_expression*s ve *primary_no_array_creation_expression*s.</span><span class="sxs-lookup"><span data-stu-id="958cd-883">Primary expressions are divided between *array_creation_expression*s and *primary_no_array_creation_expression*s.</span></span> <span data-ttu-id="958cd-884">Dizi oluşturma ifadesi bu şekilde kullanarak, diğer basit ifade forms birlikte listelemek yerine olası karmaşık kod gibi izin vermeyecek şekilde dilbilgisi sağlar</span><span class="sxs-lookup"><span data-stu-id="958cd-884">Treating array-creation-expression in this way, rather than listing it along with the other simple expression forms, enables the grammar to disallow potentially confusing code such as</span></span>
```csharp
object o = new int[3][1];
```
<span data-ttu-id="958cd-885">hangi olarak yoksa yorumlanacağını</span><span class="sxs-lookup"><span data-stu-id="958cd-885">which would otherwise be interpreted as</span></span>
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a><span data-ttu-id="958cd-886">Sabit değerler</span><span class="sxs-lookup"><span data-stu-id="958cd-886">Literals</span></span>

<span data-ttu-id="958cd-887">A *primary_expression* , oluşur bir *değişmez değer* ([değişmez değerleri](lexical-structure.md#literals)) bir değer olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-887">A *primary_expression* that consists of a *literal* ([Literals](lexical-structure.md#literals)) is classified as a value.</span></span>


### <a name="interpolated-strings"></a><span data-ttu-id="958cd-888">Ara değerli dizeler</span><span class="sxs-lookup"><span data-stu-id="958cd-888">Interpolated strings</span></span>

<span data-ttu-id="958cd-889">Bir *interpolated_string_expression* oluşan bir `$` burada görüntülerle boşluklarını, virgülle ayrılmış bir normal veya verbatim değişmez dize değeri tarafından izlenen oturum `{` ve `}`ifadeleri alın ve biçimlendirme belirtimleri.</span><span class="sxs-lookup"><span data-stu-id="958cd-889">An *interpolated_string_expression* consists of a `$` sign followed by a regular or verbatim string literal, wherein holes, delimited by `{` and `}`, enclose expressions and formatting specifications.</span></span> <span data-ttu-id="958cd-890">İlişkilendirilmiş dize ifadesi sonucu olan bir *interpolated_string_literal* , parçalanmış bireysel belirteçlere açıklandığı [ilişkilendirilmiş dize değişmez değerleri](lexical-structure.md#interpolated-string-literals).</span><span class="sxs-lookup"><span data-stu-id="958cd-890">An interpolated string expression is the result of an *interpolated_string_literal* that has been broken up into individual tokens, as described in [Interpolated string literals](lexical-structure.md#interpolated-string-literals).</span></span>

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

<span data-ttu-id="958cd-891">*Constant_expression* içinde bir ilişkilendirme için örtük bir dönüştürme olmalıdır `int`.</span><span class="sxs-lookup"><span data-stu-id="958cd-891">The *constant_expression* in an interpolation must have an implicit conversion to `int`.</span></span>

<span data-ttu-id="958cd-892">Bir *interpolated_string_expression* bir değer olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-892">An *interpolated_string_expression* is classified as a value.</span></span> <span data-ttu-id="958cd-893">İçin hemen dönüştürülürse `System.IFormattable` veya `System.FormattableString` örtük ilişkilendirilmiş dize dönüştürme ([örtük ilişkilendirilmiş dize dönüştürme](conversions.md#implicit-interpolated-string-conversions)), ilişkilendirilmiş dize ifadesi türü vardır.</span><span class="sxs-lookup"><span data-stu-id="958cd-893">If it is immediately converted to `System.IFormattable` or `System.FormattableString` with an implicit interpolated string conversion ([Implicit interpolated string conversions](conversions.md#implicit-interpolated-string-conversions)), the interpolated string expression has that type.</span></span> <span data-ttu-id="958cd-894">Aksi takdirde, bu türün sahip `string`.</span><span class="sxs-lookup"><span data-stu-id="958cd-894">Otherwise, it has the type `string`.</span></span>

<span data-ttu-id="958cd-895">İlişkilendirilmiş dize türünde ise `System.IFormattable` veya `System.FormattableString`, bir çağrıdır anlamı `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span><span class="sxs-lookup"><span data-stu-id="958cd-895">If the type of an interpolated string is `System.IFormattable` or `System.FormattableString`, the meaning is a call to `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span></span> <span data-ttu-id="958cd-896">Tür ise `string`, bir çağrıdır ifadesinin anlamını `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="958cd-896">If the type is `string`, the meaning of the expression is a call to `string.Format`.</span></span> <span data-ttu-id="958cd-897">Her iki durumda da, her bir ilişkilendirme için yer tutucular ve bağımsız değişken için yer tutucu karşılık gelen her ifade için hazır bir biçim dizesi çağrısının bağımsız değişken listesi oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-897">In both cases, the argument list of the call consists of a format string literal with placeholders for each interpolation, and an argument for each expression corresponding to the place holders.</span></span>

<span data-ttu-id="958cd-898">Biçim dize sabit değeri şu şekilde oluşturulur burada `N` içinde ilişkilendirme sayısı *interpolated_string_expression*:</span><span class="sxs-lookup"><span data-stu-id="958cd-898">The format string literal is constructed as follows, where `N` is the number of interpolations in the *interpolated_string_expression*:</span></span>

*  <span data-ttu-id="958cd-899">Bir *interpolated_regular_string_whole* veya bir *interpolated_verbatim_string_whole* izleyen `$` Bu belirteci biçimi dize sabit değeri ise, oturum açın.</span><span class="sxs-lookup"><span data-stu-id="958cd-899">If an *interpolated_regular_string_whole* or an *interpolated_verbatim_string_whole* follows the `$` sign, then the format string literal is that token.</span></span>
*  <span data-ttu-id="958cd-900">Aksi takdirde, biçim dize sabit değeri şunlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="958cd-900">Otherwise, the format string literal consists of:</span></span> 
   *  <span data-ttu-id="958cd-901">İlk *interpolated_regular_string_start* veya *interpolated_verbatim_string_start*</span><span class="sxs-lookup"><span data-stu-id="958cd-901">First the *interpolated_regular_string_start* or *interpolated_verbatim_string_start*</span></span>
   *  <span data-ttu-id="958cd-902">Sonra her numarası için `I` gelen `0` için `N-1`:</span><span class="sxs-lookup"><span data-stu-id="958cd-902">Then for each number `I` from `0` to `N-1`:</span></span> 
      * <span data-ttu-id="958cd-903">Ondalık temsili `I`</span><span class="sxs-lookup"><span data-stu-id="958cd-903">The decimal representation of `I`</span></span>
      * <span data-ttu-id="958cd-904">Ardından, eğer karşılık gelen *ilişkilendirme* sahip bir *constant_expression*, `,` (ayrılmış) değerini ondalık gösterimini tarafından izlenen *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="958cd-904">Then, if the corresponding *interpolation* has a *constant_expression*, a `,` (comma) followed by the decimal representation of the value of the *constant_expression*</span></span>
      * <span data-ttu-id="958cd-905">Ardından *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* veya *interpolated_ verbatim_string_end* karşılık gelen ilişkilendirme hemen ardından.</span><span class="sxs-lookup"><span data-stu-id="958cd-905">Then the *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* or *interpolated_verbatim_string_end* immediately following the corresponding interpolation.</span></span>

<span data-ttu-id="958cd-906">Yalnızca sonraki bağımsız değişkenler *ifadeleri* gelen *ilişkilendirme* (varsa), sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="958cd-906">The subsequent arguments are simply the *expressions* from the *interpolations* (if any), in order.</span></span>

<span data-ttu-id="958cd-907">TODO: örnekler.</span><span class="sxs-lookup"><span data-stu-id="958cd-907">TODO: examples.</span></span>


### <a name="simple-names"></a><span data-ttu-id="958cd-908">Basit adları</span><span class="sxs-lookup"><span data-stu-id="958cd-908">Simple names</span></span>

<span data-ttu-id="958cd-909">A *simple_name* isteğe bağlı olarak bir tür bağımsız değişken listesi tarafından izlenen bir tanımlayıcı oluşur:</span><span class="sxs-lookup"><span data-stu-id="958cd-909">A *simple_name* consists of an identifier, optionally followed by a type argument list:</span></span>

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

<span data-ttu-id="958cd-910">A *simple_name* herhangi formunun `I` veya formun `I<A1,...,Ak>`burada `I` tek bir tanımlayıcıdır ve `<A1,...,Ak>` isteğe bağlıdır *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="958cd-910">A *simple_name* is either of the form `I` or of the form `I<A1,...,Ak>`, where `I` is a single identifier and `<A1,...,Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="958cd-911">Hiçbir *type_argument_list* olduğundan belirtilen göz önünde bulundurun `K` sıfır olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-911">When no *type_argument_list* is specified, consider `K` to be zero.</span></span> <span data-ttu-id="958cd-912">*Simple_name* değerlendirilir ve şu şekilde sınıflandırılan:</span><span class="sxs-lookup"><span data-stu-id="958cd-912">The *simple_name* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="958cd-913">Varsa `K` sıfırdır ve *simple_name* içinde görünür bir *blok* ve *blok*'s (veya bir kapsayan *blok*ait) yerel değişken bildirimi alanı ([bildirimleri](basic-concepts.md#declarations)) bir yerel değişken, parametre veya adla sabiti içeren `I`, ardından *simple_name* , yerel bir değişkene başvuruyor parametresi veya sabit ve değişken veya değer sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-913">If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>
*  <span data-ttu-id="958cd-914">Varsa `K` sıfırdır ve *simple_name* bir genel yöntem bildirimi gövdesi içinde görüntülenir ve bu bildirimi bir tür parametresi adı ile içeriyorsa `I`, ardından *simple_name*bu tür parametresine başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="958cd-914">If `K` is zero and the *simple_name* appears within the body of a generic method declaration and if that declaration includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
*  <span data-ttu-id="958cd-915">Aksi durumda, her örnek türü için `T` ([örnek türü](classes.md#the-instance-type)), örnek türü kapsayan tür bildirimi ile başlayan ve her kapsayan sınıfın veya yapının örneği türüyle devam bildiriminin (varsa):</span><span class="sxs-lookup"><span data-stu-id="958cd-915">Otherwise, for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of the immediately enclosing type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
   *  <span data-ttu-id="958cd-916">Varsa `K` sıfır ve bildirimi `T` ada sahip bir tür parametresi içeren `I`, ardından *simple_name* bu tür parametresine başvuran.</span><span class="sxs-lookup"><span data-stu-id="958cd-916">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
   *  <span data-ttu-id="958cd-917">Aksi takdirde, bir üye araması ([üye araması](expressions.md#member-lookup)), `I` içinde `T` ile `K` tür bağımsız değişkeni bir eşleşme üretir:</span><span class="sxs-lookup"><span data-stu-id="958cd-917">Otherwise, if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match:</span></span>
      * <span data-ttu-id="958cd-918">Varsa `T` kapsayan sınıf veya yapı türü örneği türüdür ve Ara bir tanımlayan veya hakkında daha fazla yöntem, sonucu bir ilişkili örnek ifade olan bir yöntem grubu olduğundan `this`.</span><span class="sxs-lookup"><span data-stu-id="958cd-918">If `T` is the instance type of the immediately enclosing class or struct type and the lookup identifies one or more methods, the result is a method group with an associated instance expression of `this`.</span></span> <span data-ttu-id="958cd-919">Tür bağımsız değişken listesi belirtilmişse genel yöntem arayan kullanılır ([yöntem çağrıları](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="958cd-919">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
      * <span data-ttu-id="958cd-920">Aksi takdirde `T` arama bir örnek üyesi tanımlıyorsa ve başvuru örnek oluşturucusu, bir örnek yöntemi veya örneği erişimci gövdesi içinde ortaya çıkarsa kapsayan sınıf veya yapı türü örneği türü değil Sonuç üye erişimi ile aynı olur ([üye erişimi](expressions.md#member-access)) biçiminde `this.I`.</span><span class="sxs-lookup"><span data-stu-id="958cd-920">Otherwise, if `T` is the instance type of the immediately enclosing class or struct type, if the lookup identifies an instance member, and if the reference occurs within the body of an instance constructor, an instance method, or an instance accessor, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `this.I`.</span></span> <span data-ttu-id="958cd-921">Bu durum yalnızca oluşabilir olduğunda `K` sıfırdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-921">This can only happen when `K` is zero.</span></span>
      * <span data-ttu-id="958cd-922">Aksi halde, sonuç bir üye erişimi ile aynı olur ([üye erişimi](expressions.md#member-access)) biçiminde `T.I` veya `T.I<A1,...,Ak>`.</span><span class="sxs-lookup"><span data-stu-id="958cd-922">Otherwise, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `T.I` or `T.I<A1,...,Ak>`.</span></span> <span data-ttu-id="958cd-923">Bu durumda, bunun için bir bağlama zamanı hatasına neden olur *simple_name* bir örnek üyesine başvurma.</span><span class="sxs-lookup"><span data-stu-id="958cd-923">In this case, it is a binding-time error for the *simple_name* to refer to an instance member.</span></span>

*  <span data-ttu-id="958cd-924">Aksi durumda, her ad alanı için `N`, ad alanı ile başlayan *simple_name* devam her ad alanı (varsa) kapsayan ve bitiş genel ad alanı ile birlikte aşağıdaki adımlardan oluşur bir varlık bulunana kadar değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="958cd-924">Otherwise, for each namespace `N`, starting with the namespace in which the *simple_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
   *  <span data-ttu-id="958cd-925">Varsa `K` sıfırdır ve `I` bir ad alanındaki adı `N`, ardından:</span><span class="sxs-lookup"><span data-stu-id="958cd-925">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
      * <span data-ttu-id="958cd-926">Varsa konumu burada *simple_name* gerçekleşir için bir ad alanı bildirimi kapsadığı `N` ve ad alanı bildirimi içeren bir *extern_alias_directive* veya  *using_alias_directive* , ad ilişkilendirir `I` ad alanı veya tür, ardından *simple_name* belirsiz ve bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-926">If the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="958cd-927">Aksi takdirde, *simple_name* adlı isim uzayına başvuruyor `I` içinde `N`.</span><span class="sxs-lookup"><span data-stu-id="958cd-927">Otherwise, the *simple_name* refers to the namespace named `I` in `N`.</span></span>
   *  <span data-ttu-id="958cd-928">Aksi takdirde `N` adına sahip bir erişilebilir türü içeren `I` ve `K` tür parametreleri:</span><span class="sxs-lookup"><span data-stu-id="958cd-928">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
      * <span data-ttu-id="958cd-929">Varsa `K` sıfır ve konumun burada *simple_name* gerçekleşir için bir ad alanı bildirimi kapsadığı `N` ve ad alanı bildirimi içeren bir *extern_alias_directive*veya *using_alias_directive* , ad ilişkilendirir `I` ad alanı veya tür, ardından *simple_name* belirsiz ve bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-929">If `K` is zero and the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="958cd-930">Aksi takdirde, *namespace_or_type_name* belirli tür bağımsız değişkenleri ile oluşturulan türü ifade eder.</span><span class="sxs-lookup"><span data-stu-id="958cd-930">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="958cd-931">Aksi halde, konum burada *simple_name* gerçekleşir için bir ad alanı bildirimi kapsadığı `N`:</span><span class="sxs-lookup"><span data-stu-id="958cd-931">Otherwise, if the location where the *simple_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
      * <span data-ttu-id="958cd-932">Varsa `K` sıfırdır ve ad alanı bildirimi içeren bir *extern_alias_directive* veya *using_alias_directive* , ad ilişkilendirir `I` bir içeri aktarılan ad alanıyla veya tür, ardından *simple_name* bu ad alanı veya tür ifade eder.</span><span class="sxs-lookup"><span data-stu-id="958cd-932">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *simple_name* refers to that namespace or type.</span></span>
      * <span data-ttu-id="958cd-933">Aksi takdirde, ad alanları ve tür bildirimleri tarafından aldıysanız *using_namespace_directive*s ve *using_static_directive*ad alanı bildiriminin bir s içeren tam olarak bir erişilebilir türü veya Uzantı olmayan statik üye adına sahip `I` ve `K` tür parametrelerindeki, ardından *simple_name* bu türe veya üyeye belirli tür bağımsız değişkenleri ile oluşturulmuş ifade eder.</span><span class="sxs-lookup"><span data-stu-id="958cd-933">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_static_directive*s of the namespace declaration contain exactly one accessible type or non-extension static member having name `I` and `K` type parameters, then the *simple_name* refers to that type or member constructed with the given type arguments.</span></span>
      * <span data-ttu-id="958cd-934">Aksi takdirde ad alanları ve türler tarafından alınan *using_namespace_directive*ad alanı bildiriminin bir s içeren birden fazla erişilebilir türü veya genişletme yöntemi statik üye adına sahip `I` ve `K` tür parametrelerindeki, ardından *simple_name* belirsiz ve hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-934">Otherwise, if the namespaces and types imported by the *using_namespace_directive*s of the namespace declaration contain more than one accessible type or non-extension-method static member having name `I` and `K` type parameters, then the *simple_name* is ambiguous and an error occurs.</span></span>

   <span data-ttu-id="958cd-935">Tüm bu adımı tam olarak paralel işlenmesini karşılık gelen adımına olduğuna dikkat edin bir *namespace_or_type_name* ([Namespace ve tür adları](basic-concepts.md#namespace-and-type-names)).</span><span class="sxs-lookup"><span data-stu-id="958cd-935">Note that this entire step is exactly parallel to the corresponding step in the processing of a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span>

*  <span data-ttu-id="958cd-936">Aksi takdirde, *simple_name* olup tanımsız ve bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-936">Otherwise, the *simple_name* is undefined and a compile-time error occurs.</span></span>


### <a name="parenthesized-expressions"></a><span data-ttu-id="958cd-937">Parantezli İfade</span><span class="sxs-lookup"><span data-stu-id="958cd-937">Parenthesized expressions</span></span>

<span data-ttu-id="958cd-938">A *parenthesized_expression* oluşan bir *ifade* parantez içine alınmış.</span><span class="sxs-lookup"><span data-stu-id="958cd-938">A *parenthesized_expression* consists of an *expression* enclosed in parentheses.</span></span>

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

<span data-ttu-id="958cd-939">A *parenthesized_expression* değerlendirilerek hesaplanan *ifade* parantez içinde.</span><span class="sxs-lookup"><span data-stu-id="958cd-939">A *parenthesized_expression* is evaluated by evaluating the *expression* within the parentheses.</span></span> <span data-ttu-id="958cd-940">Varsa *ifade* parantez içinde bir ad alanı veya tür, bir derleme zamanı hatası oluşur gösterir.</span><span class="sxs-lookup"><span data-stu-id="958cd-940">If the *expression* within the parentheses denotes a namespace or type, a compile-time error occurs.</span></span> <span data-ttu-id="958cd-941">Aksi takdirde, sonucunu *parenthesized_expression* içerdiği için değerlendirme sonucu *ifade*.</span><span class="sxs-lookup"><span data-stu-id="958cd-941">Otherwise, the result of the *parenthesized_expression* is the result of the evaluation of the contained *expression*.</span></span>

### <a name="member-access"></a><span data-ttu-id="958cd-942">Üye erişimi</span><span class="sxs-lookup"><span data-stu-id="958cd-942">Member access</span></span>

<span data-ttu-id="958cd-943">A *member_access* oluşan bir *primary_expression*, *predefined_type*, veya bir *qualified_alias_member*"çizgidir`.`"belirteci, izleyen bir *tanımlayıcı*ve ardından isteğe bağlı olarak bir *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="958cd-943">A *member_access* consists of a *primary_expression*, a *predefined_type*, or a *qualified_alias_member*, followed by a "`.`" token, followed by an *identifier*, optionally followed by a *type_argument_list*.</span></span>

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

<span data-ttu-id="958cd-944">*Qualified_alias_member* üretim tanımlanmış [Namespace diğer adı niteleyicileri](namespaces.md#namespace-alias-qualifiers).</span><span class="sxs-lookup"><span data-stu-id="958cd-944">The *qualified_alias_member* production is defined in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span>

<span data-ttu-id="958cd-945">A *member_access* herhangi formunun `E.I` veya formun `E.I<A1, ..., Ak>`burada `E` olan bir birincil ifade, `I` tek bir tanımlayıcıdır ve `<A1, ..., Ak>` isteğe bağlıdır  *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="958cd-945">A *member_access* is either of the form `E.I` or of the form `E.I<A1, ..., Ak>`, where `E` is a primary-expression, `I` is a single identifier and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="958cd-946">Hiçbir *type_argument_list* olduğundan belirtilen göz önünde bulundurun `K` sıfır olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-946">When no *type_argument_list* is specified, consider `K` to be zero.</span></span>

<span data-ttu-id="958cd-947">A *member_access* ile bir *primary_expression* türü `dynamic` dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="958cd-947">A *member_access* with a *primary_expression* of type `dynamic` is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="958cd-948">Bu durumda üye erişimi derleyici özellik erişimi türü sınıflandırır `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="958cd-948">In this case the compiler classifies the member access as a property access of type `dynamic`.</span></span> <span data-ttu-id="958cd-949">Anlamını belirlemek için aşağıdaki kuralları *member_access* çalışma zamanında çalışma zamanı tür yerine derleme zamanı türü kullanarak, daha sonra uygulanır *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="958cd-949">The rules below to determine the meaning of the *member_access* are then applied at run-time, using the run-time type instead of the compile-time type of the *primary_expression*.</span></span> <span data-ttu-id="958cd-950">Bu çalışma zamanı sınıflandırma için bir yöntem grubu müşteri adayları sonra üye erişimi olmalıdır *primary_expression* , bir *invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="958cd-950">If this run-time classification leads to a method group, then the member access must be the *primary_expression* of an *invocation_expression*.</span></span>

<span data-ttu-id="958cd-951">*Member_access* değerlendirilir ve şu şekilde sınıflandırılan:</span><span class="sxs-lookup"><span data-stu-id="958cd-951">The *member_access* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="958cd-952">Varsa `K` sıfırdır ve `E` bir ad alanı ve `E` ada sahip iç içe geçmiş bir ad alanı içerir `I`, sonra bu ad alanı oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-952">If `K` is zero and `E` is a namespace and `E` contains a nested namespace with name `I`, then the result is that namespace.</span></span>
*  <span data-ttu-id="958cd-953">Aksi takdirde `E` bir ad alanı ve `E` adına sahip bir erişilebilir türü içeren `I` ve `K` sonucu belirli tür bağımsız değişkenleri ile oluşturulan türü ise parametreleri yazın.</span><span class="sxs-lookup"><span data-stu-id="958cd-953">Otherwise, if `E` is a namespace and `E` contains an accessible type having name `I` and `K` type parameters, then the result is that type constructed with the given type arguments.</span></span>
*  <span data-ttu-id="958cd-954">Varsa `E` olduğu bir *predefined_type* veya *primary_expression* , bir tür olarak sınıflandırılan `E` bir tür parametresi değil ve üye araması ([üyearaması](expressions.md#member-lookup)) ' ın `I` içinde `E` ile `K` tür parametreleri üreten bir eşleşme ardından `E.I` değerlendirilir ve şu şekilde sınıflandırılan:</span><span class="sxs-lookup"><span data-stu-id="958cd-954">If `E` is a *predefined_type* or a *primary_expression* classified as a type, if `E` is not a type parameter, and if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `E` with `K` type parameters produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="958cd-955">Varsa `I` sonucu belirli tür bağımsız değişkenleri ile oluşturulan türü ise bir tür tanımlar.</span><span class="sxs-lookup"><span data-stu-id="958cd-955">If `I` identifies a type, then the result is that type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="958cd-956">Varsa `I` sonucu bir yöntem grubu ile ilişkili örnek ifade yok ise bir veya daha fazla yöntemleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="958cd-956">If `I` identifies one or more methods, then the result is a method group with no associated instance expression.</span></span> <span data-ttu-id="958cd-957">Tür bağımsız değişken listesi belirtilmişse genel yöntem arayan kullanılır ([yöntem çağrıları](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="958cd-957">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="958cd-958">Varsa `I` tanımlayan bir `static` özelliği ve ardından sonucu olan bir özellik erişimi ile ilişkilendirilmiş ifade yok.</span><span class="sxs-lookup"><span data-stu-id="958cd-958">If `I` identifies a `static` property, then the result is a property access with no associated instance expression.</span></span>
   *  <span data-ttu-id="958cd-959">Varsa `I` tanımlayan bir `static` alan:</span><span class="sxs-lookup"><span data-stu-id="958cd-959">If `I` identifies a `static` field:</span></span>
      * <span data-ttu-id="958cd-960">Alan ise `readonly` ve başvuru sınıfın veya yapının alan bildirilmiş statik Oluşturucu dışında gerçekleşir ve ardından bir değeri, yani statik alanının sonucudur `I` içinde `E`.</span><span class="sxs-lookup"><span data-stu-id="958cd-960">If the field is `readonly` and the reference occurs outside the static constructor of the class or struct in which the field is declared, then the result is a value, namely the value of the static field `I` in `E`.</span></span>
      * <span data-ttu-id="958cd-961">Aksi takdirde, bir değişken, yani statik alan sonucudur `I` içinde `E`.</span><span class="sxs-lookup"><span data-stu-id="958cd-961">Otherwise, the result is a variable, namely the static field `I` in `E`.</span></span>
   *  <span data-ttu-id="958cd-962">Varsa `I` tanımlayan bir `static` olay:</span><span class="sxs-lookup"><span data-stu-id="958cd-962">If `I` identifies a `static` event:</span></span>
      * <span data-ttu-id="958cd-963">Başvuru sınıfı veya olay bildirilir ve olayı olmadan bildirildi yapı içinde oluşursa *event_accessor_declarations* ([olayları](classes.md#events)), ardından `E.I` tam olarak işlenir gibi `I` statik alan bırakıldı.</span><span class="sxs-lookup"><span data-stu-id="958cd-963">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), then `E.I` is processed exactly as if `I` were a static field.</span></span>
      * <span data-ttu-id="958cd-964">Aksi takdirde, hiçbir ilişkilendirilmiş ifade ile bir olay erişimini sonucudur.</span><span class="sxs-lookup"><span data-stu-id="958cd-964">Otherwise, the result is an event access with no associated instance expression.</span></span>
   *  <span data-ttu-id="958cd-965">Varsa `I` bir sabiti sonuç değeri, yani bu sabitin değeri ise tanımlar.</span><span class="sxs-lookup"><span data-stu-id="958cd-965">If `I` identifies a constant, then the result is a value, namely the value of that constant.</span></span>
    * <span data-ttu-id="958cd-966">Varsa `I` sonucu bir değeri, yani bu sabit listesi üyesi ise, bir numaralandırma üyesine tanımlar.</span><span class="sxs-lookup"><span data-stu-id="958cd-966">If `I` identifies an enumeration member, then the result is a value, namely the value of that enumeration member.</span></span>
    * <span data-ttu-id="958cd-967">Aksi takdirde, `E.I` geçersiz üye başvuru ve bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-967">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="958cd-968">Varsa `E` özellik erişimi, dizin oluşturucu erişim, değişken veya değer türü, `T`ve bir üye araması ([üye araması](expressions.md#member-lookup)), `I` içinde `T` ile `K` tür bağımsız değişkenleri ardından eşleştirme oluşturur `E.I` değerlendirilir ve şu şekilde sınıflandırılan:</span><span class="sxs-lookup"><span data-stu-id="958cd-968">If `E` is a property access, indexer access, variable, or value, the type of which is `T`, and a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="958cd-969">İlk olarak, eğer `E` bir özellik veya dizin oluşturucu erişim sonra özelliğin değerini ya da dizin oluşturucu erişim elde edilir ([ifadelerin değerleri](expressions.md#values-of-expressions)) ve `E` bir değer olarak yeniden sınıflandırılması.</span><span class="sxs-lookup"><span data-stu-id="958cd-969">First, if `E` is a property or indexer access, then the value of the property or indexer access is obtained ([Values of expressions](expressions.md#values-of-expressions)) and `E` is reclassified as a value.</span></span>
   *  <span data-ttu-id="958cd-970">Varsa `I` sonucu bir ilişkili örnek ifade olan bir yöntem grubu ise bir veya daha fazla yöntemleri tanımlayan `E`.</span><span class="sxs-lookup"><span data-stu-id="958cd-970">If `I` identifies one or more methods, then the result is a method group with an associated instance expression of `E`.</span></span> <span data-ttu-id="958cd-971">Tür bağımsız değişken listesi belirtilmişse genel yöntem arayan kullanılır ([yöntem çağrıları](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="958cd-971">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="958cd-972">Varsa `I` bir Instance özelliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="958cd-972">If `I` identifies an instance property,</span></span>
      * <span data-ttu-id="958cd-973">Varsa `E` olduğu `this`, `I` otomatik olarak uygulanan bir özellik tanımlar ([Özellikleri'otomatik olarak uygulanan](classes.md#automatically-implemented-properties)) için bir örnek oluşturucusu içinde bir ayarlayıcı ve başvuru olmadan bir sınıf veya yapı türü `T`, sonucu bir değişken, yani otomatik-özellik tarafından verilen gizli destek alanı ise `I` örneğinde `T` tarafından verilen `this`.</span><span class="sxs-lookup"><span data-stu-id="958cd-973">If `E` is `this`, `I` identifies an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)) without a setter, and the reference occurs within an instance constructor for a class or struct type `T`, then the result is a variable, namely the hidden backing field for the auto-property given by `I` in the instance of `T` given by `this`.</span></span>
      * <span data-ttu-id="958cd-974">Aksi halde, sonuç ilişkili örnek bir ifade olan bir özellik erişimi, `E`.</span><span class="sxs-lookup"><span data-stu-id="958cd-974">Otherwise, the result is a property access with an associated instance expression of `E`.</span></span>
   *  <span data-ttu-id="958cd-975">Varsa `T` olduğu bir *class_type* ve `I` tanımlar, bir örnek alan *class_type*:</span><span class="sxs-lookup"><span data-stu-id="958cd-975">If `T` is a *class_type* and `I` identifies an instance field of that *class_type*:</span></span>
      * <span data-ttu-id="958cd-976">Varsa değerini `E` olduğu `null`, ardından bir `System.NullReferenceException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-976">If the value of `E` is `null`, then a `System.NullReferenceException` is thrown.</span></span>
      * <span data-ttu-id="958cd-977">Aksi durumda, alan ise `readonly` ve başvuru alan bildirilmiş sınıfının örnek oluşturucusu dışında gerçekleşir ve ardından bir değer, yani bir alanın değerini sonucudur `I` başvurduğu nesnede `E`.</span><span class="sxs-lookup"><span data-stu-id="958cd-977">Otherwise, if the field is `readonly` and the reference occurs outside an instance constructor of the class in which the field is declared, then the result is a value, namely the value of the field `I` in the object referenced by `E`.</span></span>
      * <span data-ttu-id="958cd-978">Aksi takdirde, bir değişken, yani alanın sonucudur `I` başvurduğu nesnede `E`.</span><span class="sxs-lookup"><span data-stu-id="958cd-978">Otherwise, the result is a variable, namely the field `I` in the object referenced by `E`.</span></span>
   *  <span data-ttu-id="958cd-979">Varsa `T` olduğu bir *struct_type* ve `I` tanımlar, bir örnek alan *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="958cd-979">If `T` is a *struct_type* and `I` identifies an instance field of that *struct_type*:</span></span>
      * <span data-ttu-id="958cd-980">Varsa `E` bir değer veya alan ise `readonly` ve başvuru alan bildirilmiş struct'ın örnek oluşturucusu dışında gerçekleşir ve ardından bir değer, yani bir alanın değerini sonucudur `I` tarafından verilen yapı örneğinde `E`.</span><span class="sxs-lookup"><span data-stu-id="958cd-980">If `E` is a value, or if the field is `readonly` and the reference occurs outside an instance constructor of the struct in which the field is declared, then the result is a value, namely the value of the field `I` in the struct instance given by `E`.</span></span>
      * <span data-ttu-id="958cd-981">Aksi takdirde, bir değişken, yani alanın sonucudur `I` tarafından verilen yapı örneğinde `E`.</span><span class="sxs-lookup"><span data-stu-id="958cd-981">Otherwise, the result is a variable, namely the field `I` in the struct instance given by `E`.</span></span>
   *  <span data-ttu-id="958cd-982">Varsa `I` tanımlayan bir örnek olay:</span><span class="sxs-lookup"><span data-stu-id="958cd-982">If `I` identifies an instance event:</span></span>
      * <span data-ttu-id="958cd-983">Başvuru sınıfı veya olay bildirilir ve olayı olmadan bildirildi yapı içinde oluşursa *event_accessor_declarations* ([olayları](classes.md#events)), ve başvuru olarak gerçekleşmez sol tarafında bir `+=` veya `-=` işleci, ardından `E.I` tam olarak işlenen gibi `I` örnek alanı.</span><span class="sxs-lookup"><span data-stu-id="958cd-983">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), and the reference does not occur as the left-hand side of a `+=` or `-=` operator, then `E.I` is processed exactly as if `I` was an instance field.</span></span>
      * <span data-ttu-id="958cd-984">Aksi halde, sonuç ilişkili örnek bir ifade olan bir olay erişimini, `E`.</span><span class="sxs-lookup"><span data-stu-id="958cd-984">Otherwise, the result is an event access with an associated instance expression of `E`.</span></span>
*  <span data-ttu-id="958cd-985">Aksi takdirde, bir işlem için girişimde `E.I` olarak bir uzantı metodu çağırma ([uzantısı yöntem çağrıları](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="958cd-985">Otherwise, an attempt is made to process `E.I` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="958cd-986">Bu başarısız olursa, `E.I` geçersiz üye başvuru ve bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-986">If this fails, `E.I` is an invalid member reference, and a binding-time error occurs.</span></span>

#### <a name="identical-simple-names-and-type-names"></a><span data-ttu-id="958cd-987">Basit aynı adlar ve tür adları</span><span class="sxs-lookup"><span data-stu-id="958cd-987">Identical simple names and type names</span></span>

<span data-ttu-id="958cd-988">Üye erişimi formun içinde `E.I`, `E` tek bir tanımlayıcı ve anlamını `E` olarak bir *simple_name* ([basit adları](expressions.md#simple-names)) olan bir sabit, alan, özelliği yerel değişken veya parametre olarak anlamı, aynı türde `E` olarak bir *type_name* ([Namespace ve tür adları](basic-concepts.md#namespace-and-type-names)), ardından her iki olası anlamı `E` olan izin verilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-988">In a member access of the form `E.I`, if `E` is a single identifier, and if the meaning of `E` as a *simple_name* ([Simple names](expressions.md#simple-names)) is a constant, field, property, local variable, or parameter with the same type as the meaning of `E` as a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)), then both possible meanings of `E` are permitted.</span></span> <span data-ttu-id="958cd-989">İki olası anlamı `E.I` hiçbir zaman bu yana belirsiz olan `I` mutlaka türünün bir üyesi olmalıdır `E` her iki durumda.</span><span class="sxs-lookup"><span data-stu-id="958cd-989">The two possible meanings of `E.I` are never ambiguous, since `I` must necessarily be a member of the type `E` in both cases.</span></span> <span data-ttu-id="958cd-990">Diğer bir deyişle, kural yalnızca statik üyeleri ve iç içe geçmiş türleri erişimine `E` burada bir derleme zamanı hatası Aksi halde oluşmuş.</span><span class="sxs-lookup"><span data-stu-id="958cd-990">In other words, the rule simply permits access to the static members and nested types of `E` where a compile-time error would otherwise have occurred.</span></span> <span data-ttu-id="958cd-991">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="958cd-991">For example:</span></span>
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

#### <a name="grammar-ambiguities"></a><span data-ttu-id="958cd-992">Dilbilgisi belirsizlikleri</span><span class="sxs-lookup"><span data-stu-id="958cd-992">Grammar ambiguities</span></span>

<span data-ttu-id="958cd-993">Üretime *simple_name* ([basit adları](expressions.md#simple-names)) ve *member_access* ([üye erişimi](expressions.md#member-access)) ortaya çıkmasına neden belirsizliklerden verebilirsiniz Dilbilgisi ifadeler için.</span><span class="sxs-lookup"><span data-stu-id="958cd-993">The productions for *simple_name* ([Simple names](expressions.md#simple-names)) and *member_access* ([Member access](expressions.md#member-access)) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="958cd-994">Örneğin, ifade:</span><span class="sxs-lookup"><span data-stu-id="958cd-994">For example, the statement:</span></span>
```
F(G<A,B>(7));
```
<span data-ttu-id="958cd-995">bir çağrı olarak yorumlanabilecek `F` iki bağımsız değişkeni ile `G < A` ve `B > (7)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-995">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="958cd-996">Alternatif olarak, bunu bir çağrı olarak yorumlanabilecek `F` bir bağımsız değişken ile genel yöntem çağrısına olduğu `G` iki tür bağımsız değişkenleri ve normal bir bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="958cd-996">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

<span data-ttu-id="958cd-997">Bir dizi belirteçleri (bağlamda) olarak ayrıştırılabilir varsa bir *simple_name* ([basit adları](expressions.md#simple-names)), *member_access* ([üye erişimi](expressions.md#member-access)), veya *pointer_member_access* ([işaretçi üye erişimi](unsafe-code.md#pointer-member-access)) ile biten bir *type_argument_list* ([tür bağımsız değişkenleri](types.md#type-arguments)), Kapanış hemen ardından belirteci `>` belirteci incelenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-997">If a sequence of tokens can be parsed (in context) as a *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), or *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) ending with a *type_argument_list* ([Type arguments](types.md#type-arguments)), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="958cd-998">Biri ise</span><span class="sxs-lookup"><span data-stu-id="958cd-998">If it is one of</span></span>
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
<span data-ttu-id="958cd-999">ardından *type_argument_list* parçası olarak korunur *simple_name*, *member_access* veya *pointer_member_access* ve tüm diğer olası ayrıştırma belirteçleri dizisini göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-999">then the *type_argument_list* is retained as part of the *simple_name*, *member_access* or *pointer_member_access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="958cd-1000">Aksi takdirde, *type_argument_list* parçası olarak kabul edilmemektedir *simple_name*, *member_access* veya *pointer_member_access*, hiçbir diğer olası ayrıştırma belirteçleri dizisini olsa bile.</span><span class="sxs-lookup"><span data-stu-id="958cd-1000">Otherwise, the *type_argument_list* is not considered to be part of the *simple_name*, *member_access* or *pointer_member_access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="958cd-1001">Bu kurallar Not uygulanan ayrıştırılırken bir *type_argument_list* içinde bir *namespace_or_type_name* ([Namespace ve tür adları](basic-concepts.md#namespace-and-type-names)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1001">Note that these rules are not applied when parsing a *type_argument_list* in a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span> <span data-ttu-id="958cd-1002">Deyimi</span><span class="sxs-lookup"><span data-stu-id="958cd-1002">The statement</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="958cd-1003">Bu kural göre bir çağrı olarak yorumlanır `F` bir bağımsız değişken ile genel yöntem çağrısına olduğu `G` iki tür bağımsız değişkenleri ve normal bir bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="958cd-1003">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="958cd-1004">Deyimleri</span><span class="sxs-lookup"><span data-stu-id="958cd-1004">The statements</span></span>
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
<span data-ttu-id="958cd-1005">Her bir çağrı olarak yorumlanacaktır `F` iki bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="958cd-1005">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="958cd-1006">Deyimi</span><span class="sxs-lookup"><span data-stu-id="958cd-1006">The statement</span></span>
```csharp
x = F < A > +y;
```
<span data-ttu-id="958cd-1007">deyim yazılmış gibi işleç, işleç ve tek işlenenli artı işleci, büyük bir küçüktür olarak yorumlanacaktır `x = (F < A) > (+y)`, yerine olarak bir *simple_name* ile bir *type_argument_list* bir ikili artı işleci tarafından izlenen.</span><span class="sxs-lookup"><span data-stu-id="958cd-1007">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple_name* with a *type_argument_list* followed by a binary plus operator.</span></span> <span data-ttu-id="958cd-1008">Deyimi</span><span class="sxs-lookup"><span data-stu-id="958cd-1008">In the statement</span></span>
```csharp
x = y is C<T> + z;
```
<span data-ttu-id="958cd-1009">belirteçleri `C<T>` olarak yorumlanan bir *namespace_or_type_name* ile bir *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1009">the tokens `C<T>` are interpreted as a *namespace_or_type_name* with a *type_argument_list*.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="958cd-1010">Çağrı ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-1010">Invocation expressions</span></span>

<span data-ttu-id="958cd-1011">Bir *invocation_expression* bir yöntem çağırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1011">An *invocation_expression* is used to invoke a method.</span></span>

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

<span data-ttu-id="958cd-1012">Bir *invocation_expression* dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)) aşağıdakilerden en az birini tutuyorsa:</span><span class="sxs-lookup"><span data-stu-id="958cd-1012">An *invocation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="958cd-1013">*Primary_expression* derleme zamanı türü `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1013">The *primary_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="958cd-1014">En az bir bağımsız değişken isteğe bağlı *argument_list* derleme zamanı türü `dynamic` ve *primary_expression* bir temsilci türü yok.</span><span class="sxs-lookup"><span data-stu-id="958cd-1014">At least one argument of the optional *argument_list* has compile-time type `dynamic` and the *primary_expression* does not have a delegate type.</span></span>

<span data-ttu-id="958cd-1015">Bu durumda, derleyici sınıflandırır *invocation_expression* türünde bir değer olarak `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1015">In this case the compiler classifies the *invocation_expression* as a value of type `dynamic`.</span></span> <span data-ttu-id="958cd-1016">Anlamını belirlemek için aşağıdaki kuralları *invocation_expression* çalışma zamanında derleme zamanı türü ihtiyaçlarını yerine çalışma zamanı türü kullanarak, daha sonra uygulanır *primary_expression* ve derleme zamanı türü olan bağımsız değişkenler `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1016">The rules below to determine the meaning of the *invocation_expression* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_expression* and arguments which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="958cd-1017">Varsa *primary_expression* derleme zamanı tür yok `dynamic`, yöntem çağırma açıklandığı gibi sınırlı bir derleme zamanı denetimi uğradığında sonra [derleme zamanı dinamik aşırı yükleme çözünürlüğü denetleniyor ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="958cd-1017">If the *primary_expression* does not have compile-time type `dynamic`, then the method invocation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="958cd-1018">*Primary_expression* , bir *invocation_expression* bir yöntem grubu ya da bir değeri olmalıdır bir *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1018">The *primary_expression* of an *invocation_expression* must be a method group or a value of a *delegate_type*.</span></span> <span data-ttu-id="958cd-1019">Varsa *primary_expression* bir yöntem grubu olduğundan *invocation_expression* yöntemi bir çağrıdır ([yöntem çağrıları](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1019">If the *primary_expression* is a method group, the *invocation_expression* is a method invocation ([Method invocations](expressions.md#method-invocations)).</span></span> <span data-ttu-id="958cd-1020">Varsa *primary_expression* bir değeri bir *delegate_type*, *invocation_expression* temsilci bir çağrıdır ([temsilci çağrılarını](expressions.md#delegate-invocations)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1020">If the *primary_expression* is a value of a *delegate_type*, the *invocation_expression* is a delegate invocation ([Delegate invocations](expressions.md#delegate-invocations)).</span></span> <span data-ttu-id="958cd-1021">Varsa *primary_expression* ne bir yöntem grubu ne de değeri bir *delegate_type*, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1021">If the *primary_expression* is neither a method group nor a value of a *delegate_type*, a binding-time error occurs.</span></span>

<span data-ttu-id="958cd-1022">İsteğe bağlı *argument_list* ([bağımsız değişken listeleri](expressions.md#argument-lists)) yönteminin parametreleri için değerleri veya değişken başvuruları sağlar.</span><span class="sxs-lookup"><span data-stu-id="958cd-1022">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) provides values or variable references for the parameters of the method.</span></span>

<span data-ttu-id="958cd-1023">Değerlendirme sonucu bir *invocation_expression* şu şekilde sınıflandırılır:</span><span class="sxs-lookup"><span data-stu-id="958cd-1023">The result of evaluating an *invocation_expression* is classified as follows:</span></span>

*  <span data-ttu-id="958cd-1024">Varsa *invocation_expression* bir yöntem veya döndüren bir temsilci çağıran `void`, hiçbir şey sonucudur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1024">If the *invocation_expression* invokes a method or delegate that returns `void`, the result is nothing.</span></span> <span data-ttu-id="958cd-1025">Hiçbir şey yalnızca bağlamında izin verilir olarak sınıflandırılmış bir ifade bir *statement_expression* ([ifade deyimleri](statements.md#expression-statements)) veya gövdesi bir *lambda_expression*([Anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1025">An expression that is classified as nothing is permitted only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)) or as the body of a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="958cd-1026">Aksi halde bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1026">Otherwise a binding-time error occurs.</span></span>
*  <span data-ttu-id="958cd-1027">Aksi halde, sonuç bir yöntem veya temsilci tarafından döndürülen tür değeri.</span><span class="sxs-lookup"><span data-stu-id="958cd-1027">Otherwise, the result is a value of the type returned by the method or delegate.</span></span>

#### <a name="method-invocations"></a><span data-ttu-id="958cd-1028">Yöntem çağrıları</span><span class="sxs-lookup"><span data-stu-id="958cd-1028">Method invocations</span></span>

<span data-ttu-id="958cd-1029">Bir yöntem çağırma için *primary_expression* , *invocation_expression* bir yöntem grubu olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1029">For a method invocation, the *primary_expression* of the *invocation_expression* must be a method group.</span></span> <span data-ttu-id="958cd-1030">Yöntem grubu çağrılacak bir yöntem veya belirli bir yöntemi çağırmak için seçilecek aşırı yüklenmiş yöntemler kümesini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="958cd-1030">The method group identifies the one method to invoke or the set of overloaded methods from which to choose a specific method to invoke.</span></span> <span data-ttu-id="958cd-1031">İkinci durumda, belirli bir yöntemi çağırmak için belirlenmesi bağımsız değişken türleri tarafından sağlanan bağlam bağlıdır *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1031">In the latter case, determination of the specific method to invoke is based on the context provided by the types of the arguments in the *argument_list*.</span></span>

<span data-ttu-id="958cd-1032">Bağlama zamanı işleme biçiminde bir yöntem çağrısının `M(A)`burada `M` bir yöntem grubu olduğundan (belki de dahil olmak üzere bir *type_argument_list*), ve `A` isteğe bağlıdır *argument_ Liste*, aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="958cd-1032">The binding-time processing of a method invocation of the form `M(A)`, where `M` is a method group (possibly including a *type_argument_list*), and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="958cd-1033">Yöntem çağırma için aday yöntemleri kümesini oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1033">The set of candidate methods for the method invocation is constructed.</span></span> <span data-ttu-id="958cd-1034">Her bir yöntemin `F` yöntem grubu ile ilişkili `M`:</span><span class="sxs-lookup"><span data-stu-id="958cd-1034">For each method `F` associated with the method group `M`:</span></span>
   *  <span data-ttu-id="958cd-1035">Varsa `F` genel olmayan, olan `F` bir adaydır olduğunda:</span><span class="sxs-lookup"><span data-stu-id="958cd-1035">If `F` is non-generic, `F` is a candidate when:</span></span>
      * <span data-ttu-id="958cd-1036">`M` hiçbir tür bağımsız değişken listesi vardır ve</span><span class="sxs-lookup"><span data-stu-id="958cd-1036">`M` has no type argument list, and</span></span>
      * <span data-ttu-id="958cd-1037">`F` ilişkilendirilebilmesi için uygulanabilir `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1037">`F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="958cd-1038">Varsa `F` geneldir ve `M` hiçbir tür bağımsız değişken listesi vardır `F` bir adaydır olduğunda:</span><span class="sxs-lookup"><span data-stu-id="958cd-1038">If `F` is generic and `M` has no type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="958cd-1039">Anlam çıkarma ([anlam çıkarma](expressions.md#type-inference)) başarılı, tür bağımsız değişkenleri için arama listesini çıkarımını yapma ve</span><span class="sxs-lookup"><span data-stu-id="958cd-1039">Type inference ([Type inference](expressions.md#type-inference)) succeeds, inferring a list of type arguments for the call, and</span></span>
      * <span data-ttu-id="958cd-1040">İlgili yöntem tür parametreleri için çıkarsanan tür bağımsız değişkenlerini başvurulduğunda sonra parametre listesindeki tüm oluşturulan türler f kendi kısıtlamaları karşılamak ([kısıtlamaları karşılamadığınızı](types.md#satisfying-constraints)) ve parametrelistesi`F` ilişkilendirilebilmesi için uygulanabilir `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1040">Once the inferred type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="958cd-1041">Varsa `F` geneldir ve `M` türü bağımsız değişken listesi içeren `F` bir adaydır olduğunda:</span><span class="sxs-lookup"><span data-stu-id="958cd-1041">If `F` is generic and `M` includes a type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="958cd-1042">`F` tür bağımsız değişken listesinde sağlanan gibi aynı sayıda yöntem tür parametreleri vardır ve</span><span class="sxs-lookup"><span data-stu-id="958cd-1042">`F` has the same number of method type parameters as were supplied in the type argument list, and</span></span>
      * <span data-ttu-id="958cd-1043">Tür bağımsız değişkenleri karşılık gelen yöntemi tür parametreleri için başvurulduğunda sonra parametre listesindeki tüm oluşturulan türler f kendi kısıtlamaları karşılamak ([kısıtlamaları karşılamadığınızı](types.md#satisfying-constraints)) ve parametre listesini `F` ilişkilendirilebilmesi için uygulanabilir `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1043">Once the type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
*  <span data-ttu-id="958cd-1044">Aday yöntemleri kümesini yalnızca en çok türetilen türler yöntemlerinden içerecek şekilde azalır: her bir yöntemin `C.F` kümesindeki burada `C` hangi tür yöntemi `F` bildirilen, tüm yöntemleri temelbirtürdebildirilen`C`kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1044">The set of candidate methods is reduced to contain only methods from the most derived types: For each method `C.F` in the set, where `C` is the type in which the method `F` is declared, all methods declared in a base type of `C` are removed from the set.</span></span> <span data-ttu-id="958cd-1045">Ayrıca, varsa `C` bir sınıf türü dışındaki: `object`, bir arabirim türü içinde bildirilen tüm yöntemler kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1045">Furthermore, if `C` is a class type other than `object`, all methods declared in an interface type are removed from the set.</span></span> <span data-ttu-id="958cd-1046">(Yöntem grubu dışındaki nesne etkin bir temel sınıf ve ayarlanmış bir boş olmayan etkin arabirimi sahip bir tür parametresi bir üye araması sonucu olduğunda bu ikinci kuralı yalnızca bir etkisi yoktur.)</span><span class="sxs-lookup"><span data-stu-id="958cd-1046">(This latter rule only has affect when the method group was the result of a member lookup on a type parameter having an effective base class other than object and a non-empty effective interface set.)</span></span>
*  <span data-ttu-id="958cd-1047">Aday yöntemleri sonuç kümesi boşsa, aşağıdaki adımları başka işlem terk ve bunun yerine bir çağrı bir uzantı metodu çağırma olarak işlemek için girişimde ([uzantısı yöntem çağrıları](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1047">If the resulting set of candidate methods is empty, then further processing along the following steps are abandoned, and instead an attempt is made to process the invocation as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="958cd-1048">Bu başarısız olursa, ardından hiçbir uygun yöntemleri kayıtlı ve bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1048">If this fails, then no applicable methods exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="958cd-1049">Aday yöntemleri kümesinin en iyi yöntem aşırı yükleme çözünürlüğü kurallarını kullanarak tanımlanır [aşırı yükleme çözünürlüğü](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="958cd-1049">The best method of the set of candidate methods is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="958cd-1050">Tek bir en iyi yöntem tanımlanamaz, yöntem çağırma belirsiz ve bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1050">If a single best method cannot be identified, the method invocation is ambiguous, and a binding-time error occurs.</span></span> <span data-ttu-id="958cd-1051">Aşırı yükleme çözümlemesi yaparken bir genel yöntem parametreleri (sağlanan veya çıkarsanan) tür bağımsız değişkenlerini değiştirerek sonra için karşılık gelen yöntem türü parametreleri olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1051">When performing overload resolution, the parameters of a generic method are considered after substituting the type arguments (supplied or inferred) for the corresponding method type parameters.</span></span>
*  <span data-ttu-id="958cd-1052">Seçilen en iyi yöntem, nihai doğrulama gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="958cd-1052">Final validation of the chosen best method is performed:</span></span>
   * <span data-ttu-id="958cd-1053">Yöntemin yöntem grubu kapsamında doğrulanır: en iyi yöntem statik bir yöntemi ise, yöntem grubuna kullanmasından olmalıdır bir *simple_name* veya *member_access* bir tür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1053">The method is validated in the context of the method group: If the best method is a static method, the method group must have resulted from a *simple_name* or a *member_access* through a type.</span></span> <span data-ttu-id="958cd-1054">En iyi yöntem bir örnek yöntem ise, yöntem grubu kullanmasından gereken bir *simple_name*, *member_access* bir değişken veya değer aracılığıyla veya *base_access*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1054">If the best method is an instance method, the method group must have resulted from a *simple_name*, a *member_access* through a variable or value, or a *base_access*.</span></span> <span data-ttu-id="958cd-1055">Bu gereksinimleri hiçbiri true ise, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1055">If neither of these requirements is true, a binding-time error occurs.</span></span>
   * <span data-ttu-id="958cd-1056">En iyi yöntem genel bir yöntem ise (sağlanan veya çıkarsanan) tür bağımsız değişkenlerini yönelik kısıtlamalar denetlenir ([kısıtlamaları karşılamadığınızı](types.md#satisfying-constraints)) genel metodunda bildirilen.</span><span class="sxs-lookup"><span data-stu-id="958cd-1056">If the best method is a generic method, the type arguments (supplied or inferred) are checked against the constraints ([Satisfying constraints](types.md#satisfying-constraints)) declared on the generic method.</span></span> <span data-ttu-id="958cd-1057">Herhangi bir tür bağımsız değişkeni tür parametresi üzerinde karşılık gelen Kısıt karşılamaz, bir bağlama zamanı hatası meydana gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1057">If any type argument does not satisfy the corresponding constraint(s) on the type parameter, a binding-time error occurs.</span></span>

<span data-ttu-id="958cd-1058">Bir yöntem seçili ve Yukarıdaki adımlarla bağlama zamanında doğrulanmış sonra gerçek çalışma zamanı çağırma işlevi üye çağrısının açıklanan kurallara göre işlenir [derleme zamanı dinamik aşırı yükleme çözünürlüğü denetleniyor ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="958cd-1058">Once a method has been selected and validated at binding-time by the above steps, the actual run-time invocation is processed according to the rules of function member invocation described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="958cd-1059">Yukarıda açıklanan çözümleme kurallarını sezgisel etkisini aşağıdaki gibidir: bir yöntem çağrısı tarafından çağrılan yöntemin belirli bulmak için yöntem çağırma tarafından gösterilen türü başlayın ve en az bir kadar geçerli, devralma zincirinde devam erişilebilir, geçersiz kılma olmayan bir yöntem bildiriminde bulunur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1059">The intuitive effect of the resolution rules described above is as follows: To locate the particular method invoked by a method invocation, start with the type indicated by the method invocation and proceed up the inheritance chain until at least one applicable, accessible, non-override method declaration is found.</span></span> <span data-ttu-id="958cd-1060">Ardından tür çıkarımı gerçekleştirin ve aşırı yükleme çözümlemesi, türde bildirilen geçerli, erişilebilir, geçersiz kılma olmayan yöntemler kümesi üzerinde ve bu nedenle seçili yöntemi çağır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1060">Then perform type inference and overload resolution on the set of applicable, accessible, non-override methods declared in that type and invoke the method thus selected.</span></span> <span data-ttu-id="958cd-1061">Hiçbir yöntemi bulunamadı, bunun yerine çağrı bir uzantı metodu çağırma olarak işlenecek deneyin.</span><span class="sxs-lookup"><span data-stu-id="958cd-1061">If no method was found, try instead to process the invocation as an extension method invocation.</span></span>

#### <a name="extension-method-invocations"></a><span data-ttu-id="958cd-1062">Uzantı yöntem çağrıları</span><span class="sxs-lookup"><span data-stu-id="958cd-1062">Extension method invocations</span></span>

<span data-ttu-id="958cd-1063">Bir yöntem çağrısı ([çağrılarını paketlenmiş örneklerinde](expressions.md#invocations-on-boxed-instances)) biçimlerden biri</span><span class="sxs-lookup"><span data-stu-id="958cd-1063">In a method invocation ([Invocations on boxed instances](expressions.md#invocations-on-boxed-instances)) of one of the forms</span></span>
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
<span data-ttu-id="958cd-1064">normal işleme çağrısının hiçbir uygun yöntemleri bulursa, bir yapı bir uzantı metodu çağırma olarak işlemek için girişimde.</span><span class="sxs-lookup"><span data-stu-id="958cd-1064">if the normal processing of the invocation finds no applicable methods, an attempt is made to process the construct as an extension method invocation.</span></span> <span data-ttu-id="958cd-1065">Varsa *expr* veya herhangi bir *args* derleme zamanı türü `dynamic`, genişletme yöntemleri geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="958cd-1065">If *expr* or any of the *args* has compile-time type `dynamic`, extension methods will not apply.</span></span>

<span data-ttu-id="958cd-1066">Amaç en iyi bulmaktır *type_name* `C`, böylece karşılık gelen statik yöntem çağırma gerçekleşir:</span><span class="sxs-lookup"><span data-stu-id="958cd-1066">The objective is to find the best *type_name* `C`, so that the corresponding static method invocation can take place:</span></span>
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

<span data-ttu-id="958cd-1067">Bir genişletme yöntemi `Ci.Mj` olduğu ***uygun*** varsa:</span><span class="sxs-lookup"><span data-stu-id="958cd-1067">An extension method `Ci.Mj` is ***eligible*** if:</span></span>

*  <span data-ttu-id="958cd-1068">`Ci` Genel olmayan, iç içe bir sınıftır</span><span class="sxs-lookup"><span data-stu-id="958cd-1068">`Ci` is a non-generic, non-nested class</span></span>
*  <span data-ttu-id="958cd-1069">Adını `Mj` olduğu *tanımlayıcısı*</span><span class="sxs-lookup"><span data-stu-id="958cd-1069">The name of `Mj` is *identifier*</span></span>
*  <span data-ttu-id="958cd-1070">`Mj` erişilebilir ve bağımsız değişkenler için yukarıda gösterildiği gibi bir statik yöntem olarak uygulandığında uygulanabilir</span><span class="sxs-lookup"><span data-stu-id="958cd-1070">`Mj` is accessible and applicable when applied to the arguments as a static method as shown above</span></span>
*  <span data-ttu-id="958cd-1071">Gelen bir örtük kimlik, başvuru veya paketleme dönüştürmesi var *expr* ilk parametresinin türü için `Mj`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1071">An implicit identity, reference or boxing conversion exists from *expr* to the type of the first parameter of `Mj`.</span></span>

<span data-ttu-id="958cd-1072">Arama `C` aşağıdaki gibi çalışır:</span><span class="sxs-lookup"><span data-stu-id="958cd-1072">The search for `C` proceeds as follows:</span></span>

*  <span data-ttu-id="958cd-1073">Her kapsayan ad alanı bildirimi ile devam etmesini ve içeren derleme birimi ile biten en yakın kapsayan ad alanı bildirimi, başlayarak art arda denemeler genişletme yöntemleri aday kümesini bulmak için oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="958cd-1073">Starting with the closest enclosing namespace declaration, continuing with each enclosing namespace declaration, and ending with the containing compilation unit, successive attempts are made to find a candidate set of extension methods:</span></span>
   * <span data-ttu-id="958cd-1074">Belirtilen ad alanı veya derleme birimi genel olmayan tür bildirimleri doğrudan içerip içermediğini `Ci` uygun genişletme yöntemleri ile `Mj`, sonra da bu genişletme yöntemleri kümesini aday kümesidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1074">If the given namespace or compilation unit directly contains non-generic type declarations `Ci` with eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
   * <span data-ttu-id="958cd-1075">Varsa türleri `Ci` tarafından alınan *using_static_declarations* ve doğrudan tarafından içeri aktarılan ad alanlarını bildirilen *using_namespace_directive*belirtilen ad alanı veya derleme biriminde doğrudan s uygun bir uzantı yöntemlerini içeren `Mj`, sonra da bu genişletme yöntemleri kümesini aday kümesidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1075">If types `Ci` imported by *using_static_declarations* and directly declared in namespaces imported by *using_namespace_directive*s in the given namespace or compilation unit directly contain eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
*  <span data-ttu-id="958cd-1076">Aday ayarlanmadı kapsayan tüm ad alanı bildirimi veya derleme biriminde bulunursa, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1076">If no candidate set is found in any enclosing namespace declaration or compilation unit, a compile-time error occurs.</span></span>
*  <span data-ttu-id="958cd-1077">Aksi takdirde, aşırı yükleme çözünürlüğü açıklanan aday uygulanır ([aşırı yükleme çözünürlüğü](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1077">Otherwise, overload resolution is applied to the candidate set as described in ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="958cd-1078">Tek bir en iyi yöntem bulunursa, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1078">If no single best method is found, a compile-time error occurs.</span></span>
*  <span data-ttu-id="958cd-1079">`C` bir genişletme yöntemi bildirilen en iyi yöntem içinde türüdür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1079">`C` is the type within which the best method is declared as an extension method.</span></span>

<span data-ttu-id="958cd-1080">Kullanarak `C` bir hedef olarak yöntem çağrısından sonra bir statik yöntem çağırma işlenir ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1080">Using `C` as a target, the method call is then processed as a static method invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="958cd-1081">Önceki kuralların ortalama örnek yöntemleri genişletme yöntemleri üzerinde önceliklidir, iç ad alanı bildirimi uzantı yöntemleri genişletme yöntemleri dış ad alanı bildirimi ve bu uzantı üzerinde öncelik kazanır doğrudan bir ad alanında bildirilen yöntemler önceliklidir genişletme yöntemleri kullanarak, aynı ad alanı içeri aktarılan ad alanı yönergesi.</span><span class="sxs-lookup"><span data-stu-id="958cd-1081">The preceding rules mean that instance methods take precedence over extension methods, that extension methods available in inner namespace declarations take precedence over extension methods available in outer namespace declarations, and that extension methods declared directly in a namespace take precedence over extension methods imported into that same namespace with a using namespace directive.</span></span> <span data-ttu-id="958cd-1082">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="958cd-1082">For example:</span></span>
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

<span data-ttu-id="958cd-1083">Örnekte, `B`'s yöntemi ilk genişletme yöntemi göre önceliklidir ve `C`'s yöntemi her iki uzantı yöntemleri göre önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1083">In the example, `B`'s method takes precedence over the first extension method, and `C`'s method takes precedence over both extension methods.</span></span>

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

<span data-ttu-id="958cd-1084">Bu örneğin çıktısı şöyledir:</span><span class="sxs-lookup"><span data-stu-id="958cd-1084">The output of this example is:</span></span>
```
E.F(1)
D.G(2)
C.H(3)
```
<span data-ttu-id="958cd-1085">`D.G` öncelikli `C.G`, ve `E.F` hem de önceliklidir `D.F` ve `C.F`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1085">`D.G` takes precedence over `C.G`, and `E.F` takes precedence over both `D.F` and `C.F`.</span></span>

#### <a name="delegate-invocations"></a><span data-ttu-id="958cd-1086">Temsilci çağrılarını</span><span class="sxs-lookup"><span data-stu-id="958cd-1086">Delegate invocations</span></span>

<span data-ttu-id="958cd-1087">Bir temsilci çağrısı için *primary_expression* , *invocation_expression* değeri olmalıdır bir *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1087">For a delegate invocation, the *primary_expression* of the *invocation_expression* must be a value of a *delegate_type*.</span></span> <span data-ttu-id="958cd-1088">Ayrıca, dikkate *delegate_type* işlevi üye aynı parametre listesiyle *delegate_type*, *delegate_type* uygulanabilir (olması gerekir [Geçerli işlev üyesi](expressions.md#applicable-function-member)) ile ilişkilendirilebilmesi için *argument_list* , *invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1088">Furthermore, considering the *delegate_type* to be a function member with the same parameter list as the *delegate_type*, the *delegate_type* must be applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the *argument_list* of the *invocation_expression*.</span></span>

<span data-ttu-id="958cd-1089">Bir temsilci çağrısı formun çalışma zamanı işlenmesini `D(A)`burada `D` olduğu bir *primary_expression* , bir *delegate_type* ve `A` isteğe bağlı bir olan*argument_list*, aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="958cd-1089">The run-time processing of a delegate invocation of the form `D(A)`, where `D` is a *primary_expression* of a *delegate_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="958cd-1090">`D` değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1090">`D` is evaluated.</span></span> <span data-ttu-id="958cd-1091">Bu değerlendirme bir özel durum neden olursa, başka bir adım yürütülür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1091">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="958cd-1092">Değerini `D` geçerli olması için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1092">The value of `D` is checked to be valid.</span></span> <span data-ttu-id="958cd-1093">Varsa değerini `D` olduğu `null`, `System.NullReferenceException` oluşturulur ve başka bir adım yürütülür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1093">If the value of `D` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="958cd-1094">Aksi takdirde, `D` temsilci örneği bir başvurudur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1094">Otherwise, `D` is a reference to a delegate instance.</span></span> <span data-ttu-id="958cd-1095">İşlev çağrılarını üyesi ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) temsilcinin çağırma listesine çağrılabilir varlıkların her biri üzerinde gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1095">Function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) are performed on each of the callable entities in the invocation list of the delegate.</span></span> <span data-ttu-id="958cd-1096">Bir örnek ve örnek yöntemi oluşan çağrılabilir varlıklar için örnek çağrısına çağrılabilir varlıkta içerilen örneğidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1096">For callable entities consisting of an instance and instance method, the instance for the invocation is the instance contained in the callable entity.</span></span>

### <a name="element-access"></a><span data-ttu-id="958cd-1097">Öğe erişimi</span><span class="sxs-lookup"><span data-stu-id="958cd-1097">Element access</span></span>

<span data-ttu-id="958cd-1098">Bir *element_access* oluşan bir *primary_no_array_creation_expression*ve ardından bir "`[`" belirteci, izleyen bir *argument_list*ve ardından bir " `]`"belirteci.</span><span class="sxs-lookup"><span data-stu-id="958cd-1098">An *element_access* consists of a *primary_no_array_creation_expression*, followed by a "`[`" token, followed by an *argument_list*, followed by a "`]`" token.</span></span> <span data-ttu-id="958cd-1099">*Argument_list* bir veya daha fazla oluşur *bağımsız değişken*virgülle ayırarak, s.</span><span class="sxs-lookup"><span data-stu-id="958cd-1099">The *argument_list* consists of one or more *argument*s, separated by commas.</span></span>

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

<span data-ttu-id="958cd-1100">*Argument_list* , bir *element_access* içermesine izin verilmiyor `ref` veya `out` bağımsız değişkenler.</span><span class="sxs-lookup"><span data-stu-id="958cd-1100">The *argument_list* of an *element_access* is not allowed to contain `ref` or `out` arguments.</span></span>

<span data-ttu-id="958cd-1101">Bir *element_access* dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)) aşağıdakilerden en az birini tutuyorsa:</span><span class="sxs-lookup"><span data-stu-id="958cd-1101">An *element_access* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="958cd-1102">*Primary_no_array_creation_expression* derleme zamanı türü `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1102">The *primary_no_array_creation_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="958cd-1103">En az bir ifade *argument_list* derleme zamanı türü `dynamic` ve *primary_no_array_creation_expression* bir dizi türü yok.</span><span class="sxs-lookup"><span data-stu-id="958cd-1103">At least one expression of the *argument_list* has compile-time type `dynamic` and the *primary_no_array_creation_expression* does not have an array type.</span></span>

<span data-ttu-id="958cd-1104">Bu durumda, derleyici sınıflandırır *element_access* türünde bir değer olarak `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1104">In this case the compiler classifies the *element_access* as a value of type `dynamic`.</span></span> <span data-ttu-id="958cd-1105">Anlamını belirlemek için aşağıdaki kuralları *element_access* çalışma zamanında derleme zamanı türü ihtiyaçlarını yerine çalışma zamanı türü kullanarak, daha sonra uygulanır *primary_no_array_creation_expression*ve *argument_list* derleme zamanı türüne sahip ifade `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1105">The rules below to determine the meaning of the *element_access* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_no_array_creation_expression* and *argument_list* expressions which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="958cd-1106">Varsa *primary_no_array_creation_expression* derleme zamanı tür yok `dynamic`, öğe erişimi açıklandığı gibi sınırlı bir derleme zamanı denetimi uğradığında sonra [derleme zamanı dinamik denetleniyor aşırı yükleme çözümlemesi](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="958cd-1106">If the *primary_no_array_creation_expression* does not have compile-time type `dynamic`, then the element access undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="958cd-1107">Varsa *primary_no_array_creation_expression* , bir *element_access* bir değeri bir *array_type*, *element_access* olduğu bir dizi erişim ([dizi erişim](expressions.md#array-access)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1107">If the *primary_no_array_creation_expression* of an *element_access* is a value of an *array_type*, the *element_access* is an array access ([Array access](expressions.md#array-access)).</span></span> <span data-ttu-id="958cd-1108">Aksi takdirde, *primary_no_array_creation_expression* bir değişken veya değeri bir sınıf, yapı veya bir veya daha fazla dizin oluşturucu üye, bu durumda olan bir arabirim türü olmalıdır *element_access* olduğu bir Dizin Oluşturucu erişim ([dizinleyici erişimi](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1108">Otherwise, the *primary_no_array_creation_expression* must be a variable or value of a class, struct, or interface type that has one or more indexer members, in which case the *element_access* is an indexer access ([Indexer access](expressions.md#indexer-access)).</span></span>

#### <a name="array-access"></a><span data-ttu-id="958cd-1109">Dizi erişimi</span><span class="sxs-lookup"><span data-stu-id="958cd-1109">Array access</span></span>

<span data-ttu-id="958cd-1110">Bir dizi erişim *primary_no_array_creation_expression* , *element_access* değeri olmalıdır bir *array_type*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1110">For an array access, the *primary_no_array_creation_expression* of the *element_access* must be a value of an *array_type*.</span></span> <span data-ttu-id="958cd-1111">Ayrıca, *argument_list* bir dizi erişim adlandırılmış bağımsız değişkenler içeren izni yok. İfadelerin sayısı *argument_list* boyut sayısı aynı olmalıdır *array_type*, ve her deyim türü olmalıdır `int`, `uint`, `long`, `ulong`, ya da bir veya daha fazla bu türleri için örtük olarak dönüştürülebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1111">Furthermore, the *argument_list* of an array access is not allowed to contain named arguments.The number of expressions in the *argument_list* must be the same as the rank of the *array_type*, and each expression must be of type `int`, `uint`, `long`, `ulong`, or must be implicitly convertible to one or more of these types.</span></span>

<span data-ttu-id="958cd-1112">Yani ifadelerinin içinde değerleri tarafından seçilen dizi öğesi dizinin öğe türü bir değişken bir dizi erişim değerlendirmesinin sonucu olan *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1112">The result of evaluating an array access is a variable of the element type of the array, namely the array element selected by the value(s) of the expression(s) in the *argument_list*.</span></span>

<span data-ttu-id="958cd-1113">Dizi erişiminin formun çalışma zamanı işlenmesini `P[A]`burada `P` olduğu bir *primary_no_array_creation_expression* , bir *array_type* ve `A` bir olduğu*argument_list*, aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="958cd-1113">The run-time processing of an array access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of an *array_type* and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="958cd-1114">`P` değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1114">`P` is evaluated.</span></span> <span data-ttu-id="958cd-1115">Bu değerlendirme bir özel durum neden olursa, başka bir adım yürütülür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1115">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="958cd-1116">Dizin ifadelerin *argument_list* sırayla soldan sağa doğru değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1116">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="958cd-1117">Her dizin ifadesi, örtük bir dönüştürme değerlendirmesinin ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) şu türlerden biri olarak gerçekleştirilir: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1117">Following evaluation of each index expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="958cd-1118">Örtük bir dönüştürme bulunduğu bu listedeki ilk türü seçilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1118">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="958cd-1119">Örneğin, Dizin ifadesi türü ise `short` ardından örtük dönüştürmeleri `int` örtük dönüştürmelerine bu yana gerçekleştirilen `short` için `int` ve `short` için `long` mümkündür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1119">For instance, if the index expression is of type `short` then an implicit conversion to `int` is performed, since implicit conversions from `short` to `int` and from `short` to `long` are possible.</span></span> <span data-ttu-id="958cd-1120">Bir dizini ifade veya sonraki örtük dönüştürme değerinin bir özel durum neden olursa, daha fazla dizin ifadesi değerlendirilir ve başka adımlar yürütülür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1120">If evaluation of an index expression or the subsequent implicit conversion causes an exception, then no further index expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="958cd-1121">Değerini `P` geçerli olması için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1121">The value of `P` is checked to be valid.</span></span> <span data-ttu-id="958cd-1122">Varsa değerini `P` olduğu `null`, `System.NullReferenceException` oluşturulur ve başka bir adım yürütülür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1122">If the value of `P` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="958cd-1123">Her bir ifadenin değerine *argument_list* gerçek sınırları her boyutunun başvurduğu dizi örneği karşılaştırılarak `P`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1123">The value of each expression in the *argument_list* is checked against the actual bounds of each dimension of the array instance referenced by `P`.</span></span> <span data-ttu-id="958cd-1124">Bir veya daha fazla değer aralık dışında ise bir `System.IndexOutOfRangeException` oluşturulur ve başka bir adım yürütülür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1124">If one or more values are out of range, a `System.IndexOutOfRangeException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="958cd-1125">Dizin ifadelerini tarafından verilen bir dizi öğesinin konumunu hesaplanır ve bu konum dizi erişimi sonucunu olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1125">The location of the array element given by the index expression(s) is computed, and this location becomes the result of the array access.</span></span>

#### <a name="indexer-access"></a><span data-ttu-id="958cd-1126">Dizinleyici erişimi</span><span class="sxs-lookup"><span data-stu-id="958cd-1126">Indexer access</span></span>

<span data-ttu-id="958cd-1127">Bir dizin oluşturucu erişim *primary_no_array_creation_expression* , *element_access* bir değişkeni veya bir sınıf, yapı veya arabirim türü değeri olması gerekir ve bu tür bir veya daha fazla uygulamalıdır ilişkilendirilebilmesi için geçerli olan dizin oluşturucular *argument_list* , *element_access*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1127">For an indexer access, the *primary_no_array_creation_expression* of the *element_access* must be a variable or value of a class, struct, or interface type, and this type must implement one or more indexers that are applicable with respect to the *argument_list* of the *element_access*.</span></span>

<span data-ttu-id="958cd-1128">Bir dizin oluşturucu erişim formun bağlama zamanı işlenmesini `P[A]`burada `P` olduğu bir *primary_no_array_creation_expression* bir sınıf, yapı veya arabirim türü `T`, ve `A` olduğu bir *argument_list*, aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="958cd-1128">The binding-time processing of an indexer access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of a class, struct, or interface type `T`, and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="958cd-1129">Dizin oluşturucular tarafından sağlanan dizi `T` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1129">The set of indexers provided by `T` is constructed.</span></span> <span data-ttu-id="958cd-1130">Küme içinde bildirilen tüm dizin oluşturucuların oluşur `T` veya bir taban türü `T` olmayan `override` bildirimleri ve bu bağlamda erişilebilir ([üye erişimi](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1130">The set consists of all indexers declared in `T` or a base type of `T` that are not `override` declarations and are accessible in the current context ([Member access](basic-concepts.md#member-access)).</span></span>
*  <span data-ttu-id="958cd-1131">Belirlenen, geçerli ve gizlenmesi bu dizin oluşturucular tarafından diğer dizin oluşturucular için azaltılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1131">The set is reduced to those indexers that are applicable and not hidden by other indexers.</span></span> <span data-ttu-id="958cd-1132">Her dizin oluşturucu için aşağıdaki kurallar uygulanır `S.I` kümesindeki burada `S` türüdür, dizin oluşturucu `I` bildirilir:</span><span class="sxs-lookup"><span data-stu-id="958cd-1132">The following rules are applied to each indexer `S.I` in the set, where `S` is the type in which the indexer `I` is declared:</span></span>
   * <span data-ttu-id="958cd-1133">Varsa `I` ilişkilendirilebilmesi için geçerli değildir `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)), ardından `I` kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1133">If `I` is not applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then `I` is removed from the set.</span></span>
   * <span data-ttu-id="958cd-1134">Varsa `I` ilişkilendirilebilmesi için uygulanabilir `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)), tüm dizin oluşturucuların temel bir tür içinde bildirilirse `S` kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1134">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then all indexers declared in a base type of `S` are removed from the set.</span></span>
   * <span data-ttu-id="958cd-1135">Varsa `I` ilişkilendirilebilmesi için uygulanabilir `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)) ve `S` bir sınıf türü dışında: `object`, bir arabirimde bildirilen tüm dizin oluşturucuların kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1135">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)) and `S` is a class type other than `object`, all indexers declared in an interface are removed from the set.</span></span>
*  <span data-ttu-id="958cd-1136">Aday dizin oluşturucular sonuç kümesi boşsa, ardından ilgili hiçbir dizin oluşturucular var ve bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1136">If the resulting set of candidate indexers is empty, then no applicable indexers exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="958cd-1137">En iyi aday dizin oluşturucular kümesini Oluşturucusu aşırı yükleme çözünürlüğü kurallarını kullanarak tanımlanır [aşırı yükleme çözünürlüğü](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="958cd-1137">The best indexer of the set of candidate indexers is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="958cd-1138">Tek bir en iyi dizin oluşturucu tanımlanamıyor dizinleyici erişimi belirsiz ve bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1138">If a single best indexer cannot be identified, the indexer access is ambiguous, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="958cd-1139">Dizin ifadelerin *argument_list* sırayla soldan sağa doğru değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1139">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="958cd-1140">Dizin Oluşturucu erişim işlemenin sonucu bir dizin oluşturucu erişim sınıflandırılmış bir ifadedir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1140">The result of processing the indexer access is an expression classified as an indexer access.</span></span> <span data-ttu-id="958cd-1141">Výraz indexeru erişim Yukarıdaki adımda belirlenen dizin oluşturucu başvuruyor ve ilişkili örneği deyimine sahip `P` ve bir ilişkili bağımsız değişken listesi `A`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1141">The indexer access expression references the indexer determined in the step above, and has an associated instance expression of `P` and an associated argument list of `A`.</span></span>

<span data-ttu-id="958cd-1142">Bu kullanılır bağlama bağlı olarak bir dizin oluşturucu erişim çağrılması ya da neden olur. *get erişimcisine* veya *set erişimcisine* dizin oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="958cd-1142">Depending on the context in which it is used, an indexer access causes invocation of either the *get accessor* or the *set accessor* of the indexer.</span></span> <span data-ttu-id="958cd-1143">Dizin Oluşturucu erişim atama, hedef ise *set erişimcisine* yeni bir değer atamak için çağrılır ([basit atama](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1143">If the indexer access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="958cd-1144">Diğer tüm durumlarda *get erişimcisine* geçerli değerini almak için çağrılır ([ifadelerin değerleri](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1144">In all other cases, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="this-access"></a><span data-ttu-id="958cd-1145">Bu erişim</span><span class="sxs-lookup"><span data-stu-id="958cd-1145">This access</span></span>

<span data-ttu-id="958cd-1146">A *this_access* ayrılmış sözcükten `this`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1146">A *this_access* consists of the reserved word `this`.</span></span>

```antlr
this_access
    : 'this'
    ;
```

<span data-ttu-id="958cd-1147">A *this_access* yalnızca izin verilen *blok* örnek oluşturucusu, bir örnek yöntemi veya bir örneği erişimcisi.</span><span class="sxs-lookup"><span data-stu-id="958cd-1147">A *this_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="958cd-1148">Biri, aşağıdaki anlamlara sahiptir:</span><span class="sxs-lookup"><span data-stu-id="958cd-1148">It has one of the following meanings:</span></span>

*  <span data-ttu-id="958cd-1149">Zaman `this` kullanılan bir *primary_expression* sınıfının örnek oluşturucusu içinde bir değer olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1149">When `this` is used in a *primary_expression* within an instance constructor of a class, it is classified as a value.</span></span> <span data-ttu-id="958cd-1150">Değer türü örneği türüdür ([örnek türü](classes.md#the-instance-type)) sınıfın içine girerek kullanımı gerçekleşir ve yapılandırılmakta olan nesneye bir başvuru değerdir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1150">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object being constructed.</span></span>
*  <span data-ttu-id="958cd-1151">Zaman `this` kullanılan bir *primary_expression* bir örnek yöntemi veya bir sınıfın örneği erişimci içinde bir değer olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1151">When `this` is used in a *primary_expression* within an instance method or instance accessor of a class, it is classified as a value.</span></span> <span data-ttu-id="958cd-1152">Değer türü örneği türüdür ([örnek türü](classes.md#the-instance-type)), içinde kullanımı gerçekleşir ve yöntem veya erişimci çağrılan nesne başvuru değeri olduğu sınıf.</span><span class="sxs-lookup"><span data-stu-id="958cd-1152">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object for which the method or accessor was invoked.</span></span>
*  <span data-ttu-id="958cd-1153">Zaman `this` kullanılan bir *primary_expression* bir yapı içinde bir örnek oluşturucusunda bir değişken olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1153">When `this` is used in a *primary_expression* within an instance constructor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="958cd-1154">Değişkeninin türü örnek türüdür ([örnek türü](classes.md#the-instance-type)) yapı içinde hangi kullanımı gerçekleşir ve yapılandırılmakta struct değişkeni temsil eder.</span><span class="sxs-lookup"><span data-stu-id="958cd-1154">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs, and the variable represents the struct being constructed.</span></span> <span data-ttu-id="958cd-1155">`this` Değişken bir örnek oluşturucusunda bir yapının, aynı şekilde davranır bir `out` yapı türünde parametre — özellikle, bu değişken her örneğinin yürütme yolu kesinlikle atanmalıdır anlamına gelir Oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="958cd-1155">The `this` variable of an instance constructor of a struct behaves exactly the same as an `out` parameter of the struct type—in particular, this means that the variable must be definitely assigned in every execution path of the instance constructor.</span></span>
*  <span data-ttu-id="958cd-1156">Zaman `this` kullanılan bir *primary_expression* bir örnek yöntemi veya bir yapının örneği erişimci içinde bir değişken olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1156">When `this` is used in a *primary_expression* within an instance method or instance accessor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="958cd-1157">Değişkeninin türü örnek türüdür ([örnek türü](classes.md#the-instance-type)) yapı içinde kullanımı gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1157">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs.</span></span>
   * <span data-ttu-id="958cd-1158">Yöntem veya erişimci bir yineleyici değilse ([yineleyiciler](classes.md#iterators)), `this` değişkenini temsil eder, yapı, yöntem veya erişimci çağrıldı ve aynı şekilde davranır bir `ref` yapı türünde parametre.</span><span class="sxs-lookup"><span data-stu-id="958cd-1158">If the method or accessor is not an iterator ([Iterators](classes.md#iterators)), the `this` variable represents the struct for which the method or accessor was invoked, and behaves exactly the same as a `ref` parameter of the struct type.</span></span>
   * <span data-ttu-id="958cd-1159">Bir yineleyici yöntem veya erişimci ise `this` değişken, yöntem veya erişimci çağrıldı ve tam olarak aynı yapı türünün bir değer parametresini davranışını struct'ın bir kopyasını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="958cd-1159">If the method or accessor is an iterator, the `this` variable represents a copy of the struct for which the method or accessor was invoked, and behaves exactly the same as a value parameter of the struct type.</span></span>

<span data-ttu-id="958cd-1160">Kullanım `this` içinde bir *primary_expression* yukarıda listelenen olanlar dışındaki bir bağlamda bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1160">Use of `this` in a *primary_expression* in a context other than the ones listed above is a compile-time error.</span></span> <span data-ttu-id="958cd-1161">Özellikle, başvurmak mümkün değildir `this` statik bir yöntemi, bir statik özellik erişimcisi ya da bir *variable_initializer* alanı bildirimi.</span><span class="sxs-lookup"><span data-stu-id="958cd-1161">In particular, it is not possible to refer to `this` in a static method, a static property accessor, or in a *variable_initializer* of a field declaration.</span></span>

### <a name="base-access"></a><span data-ttu-id="958cd-1162">Temel erişim</span><span class="sxs-lookup"><span data-stu-id="958cd-1162">Base access</span></span>

<span data-ttu-id="958cd-1163">A *base_access* ayrılmış sözcükten `base` tarafından izlenen bir "`.`" belirteci ve tanımlayıcısı veya bir *argument_list* köşeli ayraçlar içinde:</span><span class="sxs-lookup"><span data-stu-id="958cd-1163">A *base_access* consists of the reserved word `base` followed by either a "`.`" token and an identifier or an *argument_list* enclosed in square brackets:</span></span>

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

<span data-ttu-id="958cd-1164">A *base_access* benzer şekilde adlandırılmış üyeleri geçerli sınıf veya yapı tarafından gizlenen bir temel sınıf üyelerinin erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1164">A *base_access* is used to access base class members that are hidden by similarly named members in the current class or struct.</span></span> <span data-ttu-id="958cd-1165">A *base_access* yalnızca izin verilen *blok* örnek oluşturucusu, bir örnek yöntemi veya bir örneği erişimcisi.</span><span class="sxs-lookup"><span data-stu-id="958cd-1165">A *base_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="958cd-1166">Zaman `base.I` bir sınıf ya da yapının gerçekleşir `I` o sınıfın veya yapının temel sınıfının üyesi belirtmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1166">When `base.I` occurs in a class or struct, `I` must denote a member of the base class of that class or struct.</span></span> <span data-ttu-id="958cd-1167">Benzer şekilde, `base[E]` uygulanabilir bir dizin oluşturucu, temel sınıfta bulunmalıdır bir sınıf içinde gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1167">Likewise, when `base[E]` occurs in a class, an applicable indexer must exist in the base class.</span></span>

<span data-ttu-id="958cd-1168">Bağlama zamanında *base_access* ifadeleri biçiminde `base.I` ve `base[E]` tam olarak yazılmış yokmuş gibi değerlendirilir `((B)this).I` ve `((B)this)[E]`burada `B` sınıfın temel sınıf ya da yapı oluştuğu yapı.</span><span class="sxs-lookup"><span data-stu-id="958cd-1168">At binding-time, *base_access* expressions of the form `base.I` and `base[E]` are evaluated exactly as if they were written `((B)this).I` and `((B)this)[E]`, where `B` is the base class of the class or struct in which the construct occurs.</span></span> <span data-ttu-id="958cd-1169">Bu nedenle, `base.I` ve `base[E]` karşılık `this.I` ve `this[E]`, dışında `this` temel sınıfın bir örneği görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1169">Thus, `base.I` and `base[E]` correspond to `this.I` and `this[E]`, except `this` is viewed as an instance of the base class.</span></span>

<span data-ttu-id="958cd-1170">Olduğunda bir *base_access* belirlenmesi, çalışma zamanında çağrılacak üye işlevi bir sanal işlev üyesi (yöntemi, özelliği veya dizin oluşturucu) için başvurur ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetleniyor ](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1170">When a *base_access* references a virtual function member (a method, property, or indexer), the determination of which function member to invoke at run-time ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is changed.</span></span> <span data-ttu-id="958cd-1171">Çağrılan işlev üyesi en türetilen uygulamaya bularak belirlenir ([sanal yöntemleri](classes.md#virtual-methods)) ilişkilendirilebilmesi için işlevi üyesinin `B` (yerine çalışma zamanı türü göre `this`, olarak temel olmayan Access'te normal olacaktır).</span><span class="sxs-lookup"><span data-stu-id="958cd-1171">The function member that is invoked is determined by finding the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of the function member with respect to `B` (instead of with respect to the run-time type of `this`, as would be usual in a non-base access).</span></span> <span data-ttu-id="958cd-1172">Bu nedenle, içinde bir `override` , bir `virtual` üye işlevi, bir *base_access* işlevi üye'nın devralınmış uygulaması çağırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1172">Thus, within an `override` of a `virtual` function member, a *base_access* can be used to invoke the inherited implementation of the function member.</span></span> <span data-ttu-id="958cd-1173">İşlev üyesi tarafından başvurulan bir *base_access* bir bağlama zamanı hatası oluşur, soyuttur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1173">If the function member referenced by a *base_access* is abstract, a binding-time error occurs.</span></span>

### <a name="postfix-increment-and-decrement-operators"></a><span data-ttu-id="958cd-1174">Sonek artırma ve azaltma işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-1174">Postfix increment and decrement operators</span></span>

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

<span data-ttu-id="958cd-1175">İşlenen bir sonek artırma veya azaltma işlemi bir değişken, bir özellik erişimi veya bir dizin oluşturucu erişim sınıflandırılmış bir ifade olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1175">The operand of a postfix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="958cd-1176">İşleminin sonucu, işlenenin aynı türden bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1176">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="958cd-1177">Varsa *primary_expression* derleme zamanı türü `dynamic` işleci dinamik olarak bağlı sonra ([dinamik bağlama](expressions.md#dynamic-binding)), *post_increment_expression*veya *post_decrement_expression* derleme zamanı türü `dynamic` ve çalışma zamanı türünü kullanarak zamanında aşağıdaki kurallar uygulanır *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1177">If the *primary_expression* has the compile-time type `dynamic` then the operator is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), the *post_increment_expression* or *post_decrement_expression* has the compile-time type `dynamic` and the following rules are applied at run-time using the run-time type of the *primary_expression*.</span></span>

<span data-ttu-id="958cd-1178">Bir sonek işleneni artırmak, veya azaltma işlemi bir özellik veya dizin oluşturucu erişim, özelliği veya dizin oluşturucu gerekir sahip hem bir `get` ve `set` erişimcisi.</span><span class="sxs-lookup"><span data-stu-id="958cd-1178">If the operand of a postfix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="958cd-1179">Durum bu değilse, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1179">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="958cd-1180">Birli işleç aşırı yükleme çözümlemesi ([birli işleç aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1180">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="958cd-1181">Önceden tanımlanmış `++` ve `--` işleçleri aşağıdaki türleri için mevcut: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`ve herhangi bir sabit listesi türü.</span><span class="sxs-lookup"><span data-stu-id="958cd-1181">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="958cd-1182">Önceden tanımlanmış `++` işleçleri dönüş işleneni ve önceden tanımlanmış 1 ekleyerek üretilen değeri `--` işleçleri, işlenende 1 çıkarılmasıyla oluşturulan değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1182">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="958cd-1183">İçinde bir `checked` bu ekleme veya çıkarma sonucu sonuç türü aralığının dışında ve sonuç türü bir integral türünü ya da sabit listesi türü ise bağlam bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1183">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="958cd-1184">Çalışma zamanı işlenmesini bir sonek artırma veya azaltma işlemi formun `x++` veya `x--` aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="958cd-1184">The run-time processing of a postfix increment or decrement operation of the form `x++` or `x--` consists of the following steps:</span></span>

*   <span data-ttu-id="958cd-1185">Varsa `x` bir değişken olarak sınıflandırıldığını:</span><span class="sxs-lookup"><span data-stu-id="958cd-1185">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="958cd-1186">`x` değişkeni oluşturmak için değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1186">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="958cd-1187">Değerini `x` kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1187">The value of `x` is saved.</span></span>
    * <span data-ttu-id="958cd-1188">Seçili işleç kaydedilmiş değerle çağrıldığını `x` bağımsız değişken olarak.</span><span class="sxs-lookup"><span data-stu-id="958cd-1188">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="958cd-1189">Operatör tarafından döndürülen değer değerlendirmesi tarafından belirtilen konumda depolanan `x`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1189">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="958cd-1190">Kaydedilen değeri `x` işleminin sonucu olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1190">The saved value of `x` becomes the result of the operation.</span></span>
*   <span data-ttu-id="958cd-1191">Varsa `x` bir özellik veya dizin oluşturucu erişim sınıflandırıldığını:</span><span class="sxs-lookup"><span data-stu-id="958cd-1191">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="958cd-1192">Örnek ifade (varsa `x` değil `static`) ve bağımsız değişken listesi (varsa `x` bir dizin oluşturucu erişim) ile ilişkili `x` değerlendirilir, ve sonuçları sonraki kullanılan `get` ve `set` erişimci çağrıları.</span><span class="sxs-lookup"><span data-stu-id="958cd-1192">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="958cd-1193">`get` Erişimcisine `x` çağrılır ve döndürülen değer kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1193">The `get` accessor of `x` is invoked and the returned value is saved.</span></span>
    * <span data-ttu-id="958cd-1194">Seçili işleç kaydedilmiş değerle çağrıldığını `x` bağımsız değişken olarak.</span><span class="sxs-lookup"><span data-stu-id="958cd-1194">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="958cd-1195">`set` Erişimcisine `x` operatör olarak tarafından döndürülen değerle çağrılır, `value` bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="958cd-1195">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="958cd-1196">Kaydedilen değeri `x` işleminin sonucu olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1196">The saved value of `x` becomes the result of the operation.</span></span>

<span data-ttu-id="958cd-1197">`++` Ve `--` işleçleri de destekler öneki biçimini ([önek arttırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1197">The `++` and `--` operators also support prefix notation ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="958cd-1198">Genellikle, sonucunu `x++` veya `x--` değeri `x` işleminden önce ise sonucu `++x` veya `--x` değeri `x` işleminden sonra.</span><span class="sxs-lookup"><span data-stu-id="958cd-1198">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="958cd-1199">Her iki durumda da `x` kendisini işleminden sonra aynı değere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1199">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="958cd-1200">Bir `operator ++` veya `operator --` uygulama sonek veya önek gösterim kullanılarak çağrılacak.</span><span class="sxs-lookup"><span data-stu-id="958cd-1200">An `operator ++` or `operator --` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="958cd-1201">İki gösterimler için ayrı bir işleç uygulamalarına sahip mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1201">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="the-new-operator"></a><span data-ttu-id="958cd-1202">New işleci</span><span class="sxs-lookup"><span data-stu-id="958cd-1202">The new operator</span></span>

<span data-ttu-id="958cd-1203">`new` İşleci yeni türlerin örneklerini oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1203">The `new` operator is used to create new instances of types.</span></span>

<span data-ttu-id="958cd-1204">Üç tür vardır `new` ifadeleri:</span><span class="sxs-lookup"><span data-stu-id="958cd-1204">There are three forms of `new` expressions:</span></span>

*  <span data-ttu-id="958cd-1205">Nesne oluşturma ifadeler yeni örneklerini sınıf türleri ve değer türleri oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1205">Object creation expressions are used to create new instances of class types and value types.</span></span>
*  <span data-ttu-id="958cd-1206">Dizi oluşturma ifadeleri, dizi türleri yeni örnekleri oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1206">Array creation expressions are used to create new instances of array types.</span></span>
*  <span data-ttu-id="958cd-1207">Temsilci oluşturma ifadeler yeni örnekleri temsilci türleri oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1207">Delegate creation expressions are used to create new instances of delegate types.</span></span>

<span data-ttu-id="958cd-1208">`new` İşleci, bir türün bir örneğinin oluşturulmasını gösterir, ancak dinamik bellek ayrılması gelmez.</span><span class="sxs-lookup"><span data-stu-id="958cd-1208">The `new` operator implies creation of an instance of a type, but does not necessarily imply dynamic allocation of memory.</span></span> <span data-ttu-id="958cd-1209">Özellikle, örnekleri değer türlerinin değişkenleri bulundukları ve dinamik edilmedi meydana ötesinde ek bellek gerektirir, `new` türlerin örneklerini oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1209">In particular, instances of value types require no additional memory beyond the variables in which they reside, and no dynamic allocations occur when `new` is used to create instances of value types.</span></span>

#### <a name="object-creation-expressions"></a><span data-ttu-id="958cd-1210">Nesne oluşturma ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-1210">Object creation expressions</span></span>

<span data-ttu-id="958cd-1211">Bir *object_creation_expression* yeni bir örneğini oluşturmak için kullanılan bir *class_type* veya *value_type*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1211">An *object_creation_expression* is used to create a new instance of a *class_type* or a *value_type*.</span></span>

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

<span data-ttu-id="958cd-1212">*Türü* , bir *object_creation_expression* olmalıdır bir *class_type*, *value_type* veya *type_parameter* .</span><span class="sxs-lookup"><span data-stu-id="958cd-1212">The *type* of an *object_creation_expression* must be a *class_type*, a *value_type* or a *type_parameter*.</span></span> <span data-ttu-id="958cd-1213">*Türü* olamaz bir `abstract` *class_type*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1213">The *type* cannot be an `abstract` *class_type*.</span></span>

<span data-ttu-id="958cd-1214">İsteğe bağlı *argument_list* ([bağımsız değişken listeleri](expressions.md#argument-lists)) yalnızca izin verilen *türü* olduğu bir *class_type* veya *struct_ tür*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1214">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) is permitted only if the *type* is a *class_type* or a *struct_type*.</span></span>

<span data-ttu-id="958cd-1215">Parantez içine bir nesne Başlatıcı veya koleksiyon Başlatıcısı içerir sağlanan ve bir nesne oluşturma ifadesi oluşturucu bağımsız değişken listesini atlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="958cd-1215">An object creation expression can omit the constructor argument list and enclosing parentheses provided it includes an object initializer or collection initializer.</span></span> <span data-ttu-id="958cd-1216">Oluşturucu bağımsız değişken listesi atlama ve parantez kapsayan bir boş bağımsız değişken listesi belirtmeye eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1216">Omitting the constructor argument list and enclosing parentheses is equivalent to specifying an empty argument list.</span></span>

<span data-ttu-id="958cd-1217">İlk örnek oluşturucusu ve ardından nesne Başlatıcı (BelirtilenüyeveyaöğeninbaşlatmalarişlemebirnesneBaşlatıcıveyakoleksiyonBaşlatıcısıiçerenbirnesneoluşturmaifadesiişlenmesinioluşur[ Nesne başlatıcılarda](expressions.md#object-initializers)) veya koleksiyon Başlatıcısı ([koleksiyon başlatıcıları](expressions.md#collection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1217">Processing of an object creation expression that includes an object initializer or collection initializer consists of first processing the instance constructor and then processing the member or element initializations specified by the object initializer ([Object initializers](expressions.md#object-initializers)) or collection initializer ([Collection initializers](expressions.md#collection-initializers)).</span></span>

<span data-ttu-id="958cd-1218">İsteğe bağlı bağımsız değişken varsa *argument_list* derleme zamanı türü `dynamic` sonra *object_creation_expression* dinamik olarak bağlı ([dinamikbağlama](expressions.md#dynamic-binding)) ve bağımsız değişkenleri çalışma zamanı türünü kullanarak zamanında aşağıdaki kurallar uygulanır *argument_list* derleme zamanı türü `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1218">If any of the arguments in the optional *argument_list* has the compile-time type `dynamic` then the *object_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) and the following rules are applied at run-time using the run-time type of those arguments of the *argument_list* that have the compile time type `dynamic`.</span></span> <span data-ttu-id="958cd-1219">Ancak, nesne oluşturma açıklandığı gibi sınırlı bir derleme zamanı denetimi uğradığında [derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="958cd-1219">However, the object creation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="958cd-1220">Bağlama zamanı işlenmesini bir *object_creation_expression* formun `new T(A)`burada `T` olduğu bir *class_type* veya *value_type* ve `A` isteğe bağlıdır *argument_list*, aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="958cd-1220">The binding-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is a *class_type* or a *value_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="958cd-1221">Varsa `T` olduğu bir *value_type* ve `A` mevcut değil:</span><span class="sxs-lookup"><span data-stu-id="958cd-1221">If `T` is a *value_type* and `A` is not present:</span></span>
    * <span data-ttu-id="958cd-1222">*Object_creation_expression* bir varsayılan oluşturucu çağrıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1222">The *object_creation_expression* is a default constructor invocation.</span></span> <span data-ttu-id="958cd-1223">Sonucu *object_creation_expression* türü değeri `T`, yani için varsayılan değer `T` tanımlandığı gibi [System.ValueType türü](types.md#the-systemvaluetype-type).</span><span class="sxs-lookup"><span data-stu-id="958cd-1223">The result of the *object_creation_expression* is a value of type `T`, namely the default value for `T` as defined in [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>
*   <span data-ttu-id="958cd-1224">Aksi takdirde `T` olduğu bir *type_parameter* ve `A` mevcut değil:</span><span class="sxs-lookup"><span data-stu-id="958cd-1224">Otherwise, if `T` is a *type_parameter* and `A` is not present:</span></span>
    * <span data-ttu-id="958cd-1225">Hiçbir değer tür kısıtlaması veya Oluşturucu kısıtlaması ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) için belirtilen `T`, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1225">If no value type constraint or constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) has been specified for `T`, a binding-time error occurs.</span></span>
    * <span data-ttu-id="958cd-1226">Sonucu *object_creation_expression* tür parametresi için ilişkili çalışma zamanı tür değeri adlı varsayılan oluşturucu türü çağırma sonucu.</span><span class="sxs-lookup"><span data-stu-id="958cd-1226">The result of the *object_creation_expression* is a value of the run-time type that the type parameter has been bound to, namely the result of invoking the default constructor of that type.</span></span> <span data-ttu-id="958cd-1227">Çalışma zamanı türü bir başvuru türü veya değer türü olabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1227">The run-time type may be a reference type or a value type.</span></span>
*   <span data-ttu-id="958cd-1228">Aksi takdirde `T` olduğu bir *class_type* veya *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="958cd-1228">Otherwise, if `T` is a *class_type* or a *struct_type*:</span></span>
    * <span data-ttu-id="958cd-1229">Varsa `T` olduğu bir `abstract` *class_type*, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1229">If `T` is an `abstract` *class_type*, a compile-time error occurs.</span></span>
    * <span data-ttu-id="958cd-1230">Çağrılacak örnek oluşturucusu aşırı yükleme çözünürlüğü kurallarını kullanarak belirlenir [aşırı yükleme çözünürlüğü](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="958cd-1230">The instance constructor to invoke is determined using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="958cd-1231">Bildirilen tüm erişilebilir örnek oluşturucular aday örnek oluşturucuları kümesini oluşan `T` ilişkilendirilebilmesi için geçerli olan `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1231">The set of candidate instance constructors consists of all accessible instance constructors declared in `T` which are applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="958cd-1232">Aday örnek oluşturucuları kümesi boşsa veya tek bir en iyi örnek oluşturucusu tanımladıysanız, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1232">If the set of candidate instance constructors is empty, or if a single best instance constructor cannot be identified, a binding-time error occurs.</span></span>
    * <span data-ttu-id="958cd-1233">Sonucu *object_creation_expression* türü değeri `T`, yani değer üretilmediğini Yukarıdaki adımda belirlenen örnek oluşturucusunu çağırarak.</span><span class="sxs-lookup"><span data-stu-id="958cd-1233">The result of the *object_creation_expression* is a value of type `T`, namely the value produced by invoking the instance constructor determined in the step above.</span></span>
*  <span data-ttu-id="958cd-1234">Aksi takdirde, *object_creation_expression* geçersiz ve bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1234">Otherwise, the *object_creation_expression* is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="958cd-1235">Bile *object_creation_expression* dinamik bağlı, derleme zamanı hala türdür `T`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1235">Even if the *object_creation_expression* is dynamically bound, the compile-time type is still `T`.</span></span>

<span data-ttu-id="958cd-1236">Çalışma zamanı işlenmesini bir *object_creation_expression* formun `new T(A)`burada `T` olduğu *class_type* veya *struct_type* ve `A` isteğe bağlıdır *argument_list*, aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="958cd-1236">The run-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is *class_type* or a *struct_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="958cd-1237">Varsa `T` olduğu bir *class_type*:</span><span class="sxs-lookup"><span data-stu-id="958cd-1237">If `T` is a *class_type*:</span></span>
    * <span data-ttu-id="958cd-1238">Sınıfının yeni bir örneğini `T` ayrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1238">A new instance of class `T` is allocated.</span></span> <span data-ttu-id="958cd-1239">Yeni bir örneğini ayırmak yeterli bellek yoksa bir `System.OutOfMemoryException` oluşturulur ve başka bir adım yürütülür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1239">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="958cd-1240">Yeni örneğin tüm alanları varsayılan değerlerine başlatılır ([varsayılan değerler](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1240">All fields of the new instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
    * <span data-ttu-id="958cd-1241">Örnek oluşturucusu işlevi üye çağırma kurallarına göre çağrılır ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1241">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="958cd-1242">Yeni ayrılmış örneğe bir başvuru için örnek oluşturucusu otomatik olarak geçirilir ve örnek bu oluşturucu içinde erişilebilir `this`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1242">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>
*   <span data-ttu-id="958cd-1243">Varsa `T` olduğu bir *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="958cd-1243">If `T` is a *struct_type*:</span></span>
    * <span data-ttu-id="958cd-1244">Türün bir örneğini `T` geçici yerel değişken ayırarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1244">An instance of type `T` is created by allocating a temporary local variable.</span></span> <span data-ttu-id="958cd-1245">Bir örnek oluşturucusunu beri bir *struct_type* kesinlikle oluşturulmasını, hiçbir başlatma geçici değişken gereklidir örneğin her alan için bir değer atamak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1245">Since an instance constructor of a *struct_type* is required to definitely assign a value to each field of the instance being created, no initialization of the temporary variable is necessary.</span></span>
    * <span data-ttu-id="958cd-1246">Örnek oluşturucusu işlevi üye çağırma kurallarına göre çağrılır ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1246">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="958cd-1247">Yeni ayrılmış örneğe bir başvuru için örnek oluşturucusu otomatik olarak geçirilir ve örnek bu oluşturucu içinde erişilebilir `this`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1247">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>

#### <a name="object-initializers"></a><span data-ttu-id="958cd-1248">Nesne başlatıcıları</span><span class="sxs-lookup"><span data-stu-id="958cd-1248">Object initializers</span></span>

<span data-ttu-id="958cd-1249">Bir ***nesne Başlatıcı*** sıfır veya daha fazla alanlar, özellikler veya dizini oluşturulmuş bir nesne öğelerin değerleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1249">An ***object initializer*** specifies values for zero or more fields, properties or indexed elements of an object.</span></span>

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

<span data-ttu-id="958cd-1250">Kapsadığı üyesi başlatıcıları, bir dizi nesne Başlatıcı oluşan `{` ve `}` belirteçler ve virgülle ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="958cd-1250">An object initializer consists of a sequence of member initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="958cd-1251">Her *member_initializer* başlatma hedefi belirler.</span><span class="sxs-lookup"><span data-stu-id="958cd-1251">Each *member_initializer* designates a target for the initialization.</span></span> <span data-ttu-id="958cd-1252">Bir *tanımlayıcı* ise bir erişilebilir alan veya özellik başlatılmakta, nesnenin adı olmalıdır bir *argument_list* içine üzerinde erişilebilir bir dizin oluşturucu için bağımsız değişkenler köşeli ayraç belirtmelisiniz başlatılan nesne.</span><span class="sxs-lookup"><span data-stu-id="958cd-1252">An *identifier* must name an accessible field or property of the object being initialized, whereas an *argument_list* enclosed in square brackets must specify arguments for an accessible indexer on the object being initialized.</span></span> <span data-ttu-id="958cd-1253">Nesne Başlatıcı aynı alan veya özellik için birden fazla üye Başlatıcı eklemek için bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1253">It is an error for an object initializer to include more than one member initializer for the same field or property.</span></span>

<span data-ttu-id="958cd-1254">Her *initializer_target* bir eşittir işareti ve bir ifade, bir nesne Başlatıcı veya koleksiyon başlatıcısı tarafından izlenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1254">Each *initializer_target* is followed by an equals sign and either an expression, an object initializer or a collection initializer.</span></span> <span data-ttu-id="958cd-1255">İçinde başlatma yeni oluşturulan nesneye başvurmak için nesne Başlatıcı ifadeleri için mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1255">It is not possible for expressions within the object initializer to refer to the newly created object it is initializing.</span></span>

<span data-ttu-id="958cd-1256">Eşittir işareti atama aynı şekilde işlendikten sonra bir ifade belirtir bir üye Başlatıcısı ([basit atama](expressions.md#simple-assignment)) hedef.</span><span class="sxs-lookup"><span data-stu-id="958cd-1256">A member initializer that specifies an expression after the equals sign is processed in the same way as an assignment ([Simple assignment](expressions.md#simple-assignment)) to the target.</span></span>

<span data-ttu-id="958cd-1257">Nesne Başlatıcı sonra eşittir işareti belirten bir üye başlatıcısı bir ***iç içe geçmiş nesne Başlatıcı***, yani gömülü bir nesnenin bir başlatma.</span><span class="sxs-lookup"><span data-stu-id="958cd-1257">A member initializer that specifies an object initializer after the equals sign is a ***nested object initializer***, i.e. an initialization of an embedded object.</span></span> <span data-ttu-id="958cd-1258">Alan veya özellik için yeni bir değer atamak yerine, iç içe geçmiş nesne Başlatıcı atamalarını atamaları üyelerine alanı veya özelliği olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1258">Instead of assigning a new value to the field or property, the assignments in the nested object initializer are treated as assignments to members of the field or property.</span></span> <span data-ttu-id="958cd-1259">İç içe geçmiş nesne başlatıcıları özelliklere sahip bir değer türü veya değer türü salt okunur alanlara uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="958cd-1259">Nested object initializers cannot be applied to properties with a value type, or to read-only fields with a value type.</span></span>

<span data-ttu-id="958cd-1260">Eşittir işaretinden sonra koleksiyon Başlatıcısı belirten bir üye başlatıcısı, katıştırılmış bir koleksiyonun bir başlatmadır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1260">A member initializer that specifies a collection initializer after the equals sign is an initialization of an embedded collection.</span></span> <span data-ttu-id="958cd-1261">Hedef alan, özelliği veya dizin oluşturucu için yeni bir koleksiyon atamak yerine başlatıcısında verilen öğeleri hedef tarafından başvurulan koleksiyona eklenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1261">Instead of assigning a new collection to the target field, property or indexer, the elements given in the initializer are added to the collection referenced by the target.</span></span> <span data-ttu-id="958cd-1262">Hedef belirtilen gereksinimleri karşılayan bir koleksiyon türünde olmalıdır [koleksiyon başlatıcıları](expressions.md#collection-initializers).</span><span class="sxs-lookup"><span data-stu-id="958cd-1262">The target must be of a collection type that satisfies the requirements specified in [Collection initializers](expressions.md#collection-initializers).</span></span>

<span data-ttu-id="958cd-1263">Bir dizin Başlatıcı bağımsız değişkenleri her zaman tam olarak bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1263">The arguments to an index initializer will always be evaluated exactly once.</span></span> <span data-ttu-id="958cd-1264">Bu nedenle, bunlar bağımsız değişkenler (örneğin bir boş iç içe geçmiş Başlatıcı nedeniyle) hiç kullanılmadı düştüğünden olsa bile, yan etkileri için değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1264">Thus, even if the arguments end up never getting used (e.g. because of an empty nested initializer), they will be evaluated for their side effects.</span></span>

<span data-ttu-id="958cd-1265">Aşağıdaki sınıf iki koordinat noktasıyla temsil eder:</span><span class="sxs-lookup"><span data-stu-id="958cd-1265">The following class represents a point with two coordinates:</span></span>
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

<span data-ttu-id="958cd-1266">Örneği `Point` oluşturulabilir ve şu şekilde başlatıldı:</span><span class="sxs-lookup"><span data-stu-id="958cd-1266">An instance of `Point` can be created and initialized as follows:</span></span>
```csharp
Point a = new Point { X = 0, Y = 1 };
```
<span data-ttu-id="958cd-1267">aynı etkiye sahip</span><span class="sxs-lookup"><span data-stu-id="958cd-1267">which has the same effect as</span></span>
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
<span data-ttu-id="958cd-1268">Burada `__a` yoksa görünmez ve erişilemez bir geçici değişken.</span><span class="sxs-lookup"><span data-stu-id="958cd-1268">where `__a` is an otherwise invisible and inaccessible temporary variable.</span></span> <span data-ttu-id="958cd-1269">Aşağıdaki sınıf iki noktalarından oluşturulan bir dikdörtgen temsil eder:</span><span class="sxs-lookup"><span data-stu-id="958cd-1269">The following class represents a rectangle created from two points:</span></span>
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

<span data-ttu-id="958cd-1270">Örneği `Rectangle` oluşturulabilir ve şu şekilde başlatıldı:</span><span class="sxs-lookup"><span data-stu-id="958cd-1270">An instance of `Rectangle` can be created and initialized as follows:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
<span data-ttu-id="958cd-1271">aynı etkiye sahip</span><span class="sxs-lookup"><span data-stu-id="958cd-1271">which has the same effect as</span></span>
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
<span data-ttu-id="958cd-1272">Burada `__r`, `__p1` ve `__p2` görünmez ve erişilemez Aksi takdirde geçici değişkenlerdir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1272">where `__r`, `__p1` and `__p2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="958cd-1273">Varsa `Rectangle`ın katıştırılmış iki Oluşturucu ayırır `Point` örnekleri</span><span class="sxs-lookup"><span data-stu-id="958cd-1273">If `Rectangle`'s constructor allocates the two embedded `Point` instances</span></span>
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
<span data-ttu-id="958cd-1274">Şu yapı katıştırılmış başlatmak için kullanılan `Point` yeni örnekleri atamak yerine örnekleri:</span><span class="sxs-lookup"><span data-stu-id="958cd-1274">the following construct can be used to initialize the embedded `Point` instances instead of assigning new instances:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
<span data-ttu-id="958cd-1275">aynı etkiye sahip</span><span class="sxs-lookup"><span data-stu-id="958cd-1275">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

<span data-ttu-id="958cd-1276">C, aşağıdaki örnek, uygun bir tanımını ele alalım:</span><span class="sxs-lookup"><span data-stu-id="958cd-1276">Given an appropriate definition of C, the following example:</span></span>
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
<span data-ttu-id="958cd-1277">Bu bir dizi atamaları eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="958cd-1277">is equivalent to this series of assignments:</span></span>
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
<span data-ttu-id="958cd-1278">Burada `__c`vb. görünmez ve kaynak kodu için erişilemez oluşturulan değişkenlerdir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1278">where `__c`, etc., are generated variables that are invisible and inaccessible to the source code.</span></span> <span data-ttu-id="958cd-1279">Unutmayın bağımsız değişkenleri için `[0,0]` değerlendirilen yalnızca bir kez ve bağımsız değişkenleri `[1,2]` hiçbir zaman kullanılmaz olsa bile bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1279">Note that the arguments for `[0,0]` are evaluated only once, and the arguments for `[1,2]` are evaluated once even though they are never used.</span></span>

#### <a name="collection-initializers"></a><span data-ttu-id="958cd-1280">Koleksiyon başlatıcıları</span><span class="sxs-lookup"><span data-stu-id="958cd-1280">Collection initializers</span></span>

<span data-ttu-id="958cd-1281">Koleksiyon başlatıcısı, bir koleksiyonun öğeleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1281">A collection initializer specifies the elements of a collection.</span></span>

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

<span data-ttu-id="958cd-1282">Koleksiyon başlatıcısı, bir dizi öğesi başlatıcıları, kapsadığı oluşan `{` ve `}` belirteçler ve virgülle ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="958cd-1282">A collection initializer consists of a sequence of element initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="958cd-1283">Her öğe Başlatıcısı başlatılmakta koleksiyon nesnesine eklenecek bir öğeyi belirtir ve ifadeler tarafından alınmış bir listesini oluşur `{` ve `}` belirteçler ve virgülle ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="958cd-1283">Each element initializer specifies an element to be added to the collection object being initialized, and consists of a list of expressions enclosed by `{` and `}` tokens and separated by commas.</span></span>  <span data-ttu-id="958cd-1284">Tek ifadeli öğe Başlatıcısı küme ayraçları olmadan yazılabilir, ancak ardından belirsizlik üyesi başlatıcıları önlemek için bir atama ifadesi olamaz.</span><span class="sxs-lookup"><span data-stu-id="958cd-1284">A single-expression element initializer can be written without braces, but cannot then be an assignment expression, to avoid ambiguity with member initializers.</span></span> <span data-ttu-id="958cd-1285">*Non_assignment_expression* üretim tanımlanmış [ifade](expressions.md#expression).</span><span class="sxs-lookup"><span data-stu-id="958cd-1285">The *non_assignment_expression* production is defined in [Expression](expressions.md#expression).</span></span>

<span data-ttu-id="958cd-1286">Koleksiyon Başlatıcısı içeren bir nesne oluşturma ifadesi örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="958cd-1286">The following is an example of an object creation expression that includes a collection initializer:</span></span>
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

<span data-ttu-id="958cd-1287">Koleksiyon Başlatıcısı uygulandığı koleksiyon nesnesi uygulayan bir türü olmalıdır `System.Collections.IEnumerable` veya bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1287">The collection object to which a collection initializer is applied must be of a type that implements `System.Collections.IEnumerable` or a compile-time error occurs.</span></span> <span data-ttu-id="958cd-1288">Koleksiyon Başlatıcısı sırada belirtilen her öğe için çağıran bir `Add` yöntemi hedef uygulama normal üye araması bağımsız değişken listesi öğe Başlatıcısı ile ifade listesi nesnesi ve aşırı yükleme çözümlemesi her.</span><span class="sxs-lookup"><span data-stu-id="958cd-1288">For each specified element in order, the collection initializer invokes an `Add` method on the target object with the expression list of the element initializer as argument list, applying normal member lookup and overload resolution for each invocation.</span></span> <span data-ttu-id="958cd-1289">Bu nedenle, koleksiyon nesnesi adı ile ilgili bir örneği veya genişletme metodu olmalıdır `Add` her öğe Başlatıcısı için.</span><span class="sxs-lookup"><span data-stu-id="958cd-1289">Thus, the collection object must have an applicable instance or extension method with the name `Add` for each element initializer.</span></span>

<span data-ttu-id="958cd-1290">Aşağıdaki sınıf bir kişiyle bir ad ve telefon numaralarının listesini temsil eder:</span><span class="sxs-lookup"><span data-stu-id="958cd-1290">The following class represents a contact with a name and a list of phone numbers:</span></span>
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

<span data-ttu-id="958cd-1291">A `List<Contact>` oluşturulabilir ve şu şekilde başlatıldı:</span><span class="sxs-lookup"><span data-stu-id="958cd-1291">A `List<Contact>` can be created and initialized as follows:</span></span>
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
<span data-ttu-id="958cd-1292">aynı etkiye sahip</span><span class="sxs-lookup"><span data-stu-id="958cd-1292">which has the same effect as</span></span>
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
<span data-ttu-id="958cd-1293">Burada `__clist`, `__c1` ve `__c2` görünmez ve erişilemez Aksi takdirde geçici değişkenlerdir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1293">where `__clist`, `__c1` and `__c2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="958cd-1294">Dizi oluşturma ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-1294">Array creation expressions</span></span>

<span data-ttu-id="958cd-1295">Bir *array_creation_expression* yeni bir örneğini oluşturmak için kullanılan bir *array_type*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1295">An *array_creation_expression* is used to create a new instance of an *array_type*.</span></span>

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

<span data-ttu-id="958cd-1296">Bir dizi oluşturma ifadesi ilk form, her biri ayrı ayrı ifadeleri ifade listeden silmesi sonuçları türünde bir dizi örnek ayırır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1296">An array creation expression of the first form allocates an array instance of the type that results from deleting each of the individual expressions from the expression list.</span></span> <span data-ttu-id="958cd-1297">Örneğin, dizi oluşturma ifadesi `new int[10,20]` türünün bir dizi örneği üretir `int[,]`ve dizi oluşturma ifadesi `new int[10][,]` türünde bir dizi üretir `int[][,]`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1297">For example, the array creation expression `new int[10,20]` produces an array instance of type `int[,]`, and the array creation expression `new int[10][,]` produces an array of type `int[][,]`.</span></span> <span data-ttu-id="958cd-1298">Her deyim ifade listesi içinde türünde olmalıdır `int`, `uint`, `long`, veya `ulong`, ya da bir veya daha fazla bu türleri için örtük olarak dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1298">Each expression in the expression list must be of type `int`, `uint`, `long`, or `ulong`, or implicitly convertible to one or more of these types.</span></span> <span data-ttu-id="958cd-1299">Her bir ifadenin değerine karşılık gelen yeni ayrılan dizi örneği boyutun uzunluğunu belirler.</span><span class="sxs-lookup"><span data-stu-id="958cd-1299">The value of each expression determines the length of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="958cd-1300">Bir dizi boyutun uzunluğu negatif olmayan olması gerektiğinden, bir derleme zamanı hatası sahip olduğu bir *constant_expression* ifade listesi negatif bir değere sahip.</span><span class="sxs-lookup"><span data-stu-id="958cd-1300">Since the length of an array dimension must be nonnegative, it is a compile-time error to have a *constant_expression* with a negative value in the expression list.</span></span>

<span data-ttu-id="958cd-1301">Güvenli olmayan bir bağlamda dışında ([güvensiz bir bağlamı](unsafe-code.md#unsafe-contexts)), dizileri düzenini belirtilmemiş.</span><span class="sxs-lookup"><span data-stu-id="958cd-1301">Except in an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)), the layout of arrays is unspecified.</span></span>

<span data-ttu-id="958cd-1302">İlk biçiminde bir dizi oluşturma ifadesi dizi Başlatıcı içeriyorsa, ifade listesi içinde her bir ifade bir sabit olmalıdır ve ifade listesi tarafından belirtilen boyut sayısı ve boyutu uzunlukları dizi Başlatıcısı içeriğiyle eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1302">If an array creation expression of the first form includes an array initializer, each expression in the expression list must be a constant and the rank and dimension lengths specified by the expression list must match those of the array initializer.</span></span>

<span data-ttu-id="958cd-1303">İçinde bir dizi oluşturma ifadesi ikinci veya üçüncü formunun boyut sayısı belirtilen dizi türü veya boyut tanımlayıcısı dizi Başlatıcısı eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1303">In an array creation expression of the second or third form, the rank of the specified array type or rank specifier must match that of the array initializer.</span></span> <span data-ttu-id="958cd-1304">Tek tek boyut uzunlukları, öğeleri dizi başlatıcısı, karşılık gelen iç içe geçme düzeylerinin her biri çok sayıda algılanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1304">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the array initializer.</span></span> <span data-ttu-id="958cd-1305">Bu nedenle, ifade</span><span class="sxs-lookup"><span data-stu-id="958cd-1305">Thus, the expression</span></span>
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
<span data-ttu-id="958cd-1306">tam olarak karşılık gelir</span><span class="sxs-lookup"><span data-stu-id="958cd-1306">exactly corresponds to</span></span>
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

<span data-ttu-id="958cd-1307">Üçüncü biçiminde bir dizi oluşturma ifadesi olarak adlandırılır bir ***dizi oluşturma ifadesi örtük***.</span><span class="sxs-lookup"><span data-stu-id="958cd-1307">An array creation expression of the third form is referred to as an ***implicitly typed array creation expression***.</span></span> <span data-ttu-id="958cd-1308">Dizinin öğe türü açıkça, ancak en yaygın türü olarak belirlenen verilmez, ikinci forma benzer ([en iyi ortak türü bir ifade kümesi bulma](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) dizi ifadeleri kümesinin Başlatıcı.</span><span class="sxs-lookup"><span data-stu-id="958cd-1308">It is similar to the second form, except that the element type of the array is not explicitly given, but determined as the best common type ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) of the set of expressions in the array initializer.</span></span> <span data-ttu-id="958cd-1309">Çok boyutlu bir dizi için başka bir deyişle, bir where *rank_specifier* en az bir virgül içeren tüm bu kümesi oluşur *ifade*içinde bulunan iç içe geçmiş s *array_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="958cd-1309">For a multidimensional array, i.e., one where the *rank_specifier* contains at least one comma, this set comprises all *expression*s found in nested *array_initializer*s.</span></span>

<span data-ttu-id="958cd-1310">Dizi başlatıcıları daha ayrıntılı açıklanır [Dizi başlatıcıları](arrays.md#array-initializers).</span><span class="sxs-lookup"><span data-stu-id="958cd-1310">Array initializers are described further in [Array initializers](arrays.md#array-initializers).</span></span>

<span data-ttu-id="958cd-1311">Bir dizi oluşturma ifadesi değerlendirme sonucu bir değer, yani yeni ayrılan dizi örneğe bir başvuru olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1311">The result of evaluating an array creation expression is classified as a value, namely a reference to the newly allocated array instance.</span></span> <span data-ttu-id="958cd-1312">Bir dizi oluşturma ifadesi çalışma zamanı işlenmesini aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="958cd-1312">The run-time processing of an array creation expression consists of the following steps:</span></span>

*  <span data-ttu-id="958cd-1313">Boyut uzunluğu ifadelerin *expression_list* sırayla soldan sağa doğru değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1313">The dimension length expressions of the *expression_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="958cd-1314">Örtük bir dönüştürme her ifade değerlendirmesinin ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) şu türlerden biri olarak gerçekleştirilir: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1314">Following evaluation of each expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="958cd-1315">Örtük bir dönüştürme bulunduğu bu listedeki ilk türü seçilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1315">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="958cd-1316">Bir ifade veya sonraki örtük dönüştürme değerinin bir özel durum neden olursa, hiçbir ek ifadeler değerlendirilir ve başka bir adım yürütülür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1316">If evaluation of an expression or the subsequent implicit conversion causes an exception, then no further expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="958cd-1317">Boyut uzunluğu hesaplanan değerleri şu şekilde doğrulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1317">The computed values for the dimension lengths are validated as follows.</span></span> <span data-ttu-id="958cd-1318">Bir veya daha fazla değer olan küçükse sıfır, bir `System.OverflowException` oluşturulur ve başka bir adım yürütülür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1318">If one or more of the values are less than zero, a `System.OverflowException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="958cd-1319">Belirtilen boyut uzunlukta bir dizi örneğiyle ayrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1319">An array instance with the given dimension lengths is allocated.</span></span> <span data-ttu-id="958cd-1320">Yeni bir örneğini ayırmak yeterli bellek yoksa bir `System.OutOfMemoryException` oluşturulur ve başka bir adım yürütülür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1320">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="958cd-1321">Yeni dizi örneğinin tüm öğeleri varsayılan değerlerine başlatılır ([varsayılan değerler](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1321">All elements of the new array instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
*  <span data-ttu-id="958cd-1322">Dizi oluşturma ifadesi dizi Başlatıcı içeriyorsa, ardından her bir dizi Başlatıcısı ifadesinde değerlendirilir ve karşılık gelen dizi öğesine atanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1322">If the array creation expression contains an array initializer, then each expression in the array initializer is evaluated and assigned to its corresponding array element.</span></span> <span data-ttu-id="958cd-1323">Atamalar ve değerlendirmeleri ifadeleri, dizi başlatıcısında yazılır sırayla gerçekleştirilir — diğer bir deyişle, öğeleri en sağdaki boyutu ilk artan dizin sırasıyla artan düzende başlatılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1323">The evaluations and assignments are performed in the order the expressions are written in the array initializer—in other words, elements are initialized in increasing index order, with the rightmost dimension increasing first.</span></span> <span data-ttu-id="958cd-1324">Belirtilen bir ifade veya sonraki atama karşılık gelen bir dizi öğesinin değerinin bir özel durum neden olursa, daha fazla öğe başlatılır (ve kalan öğeleri, bu nedenle varsayılan değerlerine sahip olur).</span><span class="sxs-lookup"><span data-stu-id="958cd-1324">If evaluation of a given expression or the subsequent assignment to the corresponding array element causes an exception, then no further elements are initialized (and the remaining elements will thus have their default values).</span></span>

<span data-ttu-id="958cd-1325">Bir dizi oluşturma ifadesi dizi oluşturmada bir dizi türünde öğelerle izin verir, ancak böyle bir dizinin öğeleri el ile başlatılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1325">An array creation expression permits instantiation of an array with elements of an array type, but the elements of such an array must be manually initialized.</span></span> <span data-ttu-id="958cd-1326">Örneğin, deyim</span><span class="sxs-lookup"><span data-stu-id="958cd-1326">For example, the statement</span></span>
```csharp
int[][] a = new int[100][];
```
<span data-ttu-id="958cd-1327">tek boyutlu bir dizi türü 100 öğelerle oluşturur `int[]`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1327">creates a single-dimensional array with 100 elements of type `int[]`.</span></span> <span data-ttu-id="958cd-1328">Her öğenin ilk değer `null`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1328">The initial value of each element is `null`.</span></span> <span data-ttu-id="958cd-1329">Ayrıca alt dizileri ve deyim örneği oluşturmak aynı dizi oluşturma ifadesi için mümkün değildir</span><span class="sxs-lookup"><span data-stu-id="958cd-1329">It is not possible for the same array creation expression to also instantiate the sub-arrays, and the statement</span></span>
```csharp
int[][] a = new int[100][5];        // Error
```
<span data-ttu-id="958cd-1330">bir derleme zamanı hatası sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1330">results in a compile-time error.</span></span> <span data-ttu-id="958cd-1331">Alt dizileri örneğinin bunun yerine el ile giriş olarak gerçekleştirilmelidir</span><span class="sxs-lookup"><span data-stu-id="958cd-1331">Instantiation of the sub-arrays must instead be performed manually, as in</span></span>
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

<span data-ttu-id="958cd-1332">Dizi alt dizileri tümü aynı uzunlukta olduğunda olan "dikdörtgen" bir şekil varsa çok boyutlu bir dizi kullanmak daha verimlidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1332">When an array of arrays has a "rectangular" shape, that is when the sub-arrays are all of the same length, it is more efficient to use a multi-dimensional array.</span></span> <span data-ttu-id="958cd-1333">Yukarıdaki örnekte, dizi örneğinin 101 nesneleri oluşturur — bir dış dizi ve 100 alt dizi.</span><span class="sxs-lookup"><span data-stu-id="958cd-1333">In the example above, instantiation of the array of arrays creates 101 objects—one outer array and 100 sub-arrays.</span></span> <span data-ttu-id="958cd-1334">Buna karşılık,</span><span class="sxs-lookup"><span data-stu-id="958cd-1334">In contrast,</span></span>
```csharp
int[,] = new int[100, 5];
```
<span data-ttu-id="958cd-1335">yalnızca tek bir nesne, iki boyutlu bir dizi oluşturur ve tek bir deyimde ayırma gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1335">creates only a single object, a two-dimensional array, and accomplishes the allocation in a single statement.</span></span>

<span data-ttu-id="958cd-1336">Türü örtük olarak belirlenmiş dizi oluşturma örnekleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="958cd-1336">The following are examples of implicitly typed array creation expressions:</span></span>
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

<span data-ttu-id="958cd-1337">Son deyim bir derleme zamanı hatası olduğundan neden ne `int` ya da `string` diğerine örtük olarak dönüştürülebilir ve bu nedenle var olmayan en sık yazın.</span><span class="sxs-lookup"><span data-stu-id="958cd-1337">The last expression causes a compile-time error because neither `int` nor `string` is implicitly convertible to the other, and so there is no best common type.</span></span> <span data-ttu-id="958cd-1338">Açıkça belirlenmiş dizi oluşturma ifadesi bu durumda, örneğin olmasını türü belirtme kullanılmalıdır `object[]`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1338">An explicitly typed array creation expression must be used in this case, for example specifying the type to be `object[]`.</span></span> <span data-ttu-id="958cd-1339">Alternatif olarak, bir öğenin bunlar da sonra gösterilen öğe türü ortak bir taban türe, başvurusuna yayınlanabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1339">Alternatively, one of the elements can be cast to a common base type, which would then become the inferred element type.</span></span>

<span data-ttu-id="958cd-1340">Türü örtük olarak belirlenmiş dizi oluşturma ifadeleri anonim nesne başlatıcıları birleştirilebilir ([anonim nesne oluşturma ifadeleri](expressions.md#anonymous-object-creation-expressions)) anonim olarak oluşturmak için veri yapılarını yazılı.</span><span class="sxs-lookup"><span data-stu-id="958cd-1340">Implicitly typed array creation expressions can be combined with anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) to create anonymously typed data structures.</span></span> <span data-ttu-id="958cd-1341">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="958cd-1341">For example:</span></span>
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

#### <a name="delegate-creation-expressions"></a><span data-ttu-id="958cd-1342">Temsilci oluşturma ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-1342">Delegate creation expressions</span></span>

<span data-ttu-id="958cd-1343">A *delegate_creation_expression* yeni bir örneğini oluşturmak için kullanılan bir *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1343">A *delegate_creation_expression* is used to create a new instance of a *delegate_type*.</span></span>

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

<span data-ttu-id="958cd-1344">Temsilci oluşturma ifadesine bağımsız değişkeni bir yöntem grubu, anonim bir işlev veya derleme zamanı tür değeri olmalıdır `dynamic` veya *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1344">The argument of a delegate creation expression must be a method group, an anonymous function or a value of either the compile time type `dynamic` or a *delegate_type*.</span></span> <span data-ttu-id="958cd-1345">Bağımsız değişken bir yöntem grubu ise, bu yöntem tanımlar ve bir örnek yöntemi temsilci oluşturmak istediğiniz bir nesne.</span><span class="sxs-lookup"><span data-stu-id="958cd-1345">If the argument is a method group, it identifies the method and, for an instance method, the object for which to create a delegate.</span></span> <span data-ttu-id="958cd-1346">Bağımsız değişken anonim bir işlev ise doğrudan temsilci hedef yöntem gövdesini ve parametreleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="958cd-1346">If the argument is an anonymous function it directly defines the parameters and method body of the delegate target.</span></span> <span data-ttu-id="958cd-1347">Bağımsız değişken bir değer ise bir kopyasını oluşturmak için bir temsilci örneğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="958cd-1347">If the argument is a value it identifies a delegate instance of which to create a copy.</span></span>

<span data-ttu-id="958cd-1348">Varsa *ifade* derleme zamanı türü `dynamic`, *delegate_creation_expression* dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)) ve aşağıdaki kuralları çalışma zamanı türünü kullanarak zamanında uygulanan *ifade*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1348">If the *expression* has the compile-time type `dynamic`, the *delegate_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), and the rules below are applied at run-time using the run-time type of the *expression*.</span></span> <span data-ttu-id="958cd-1349">Aksi takdirde kuralları, derleme zamanında uygulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1349">Otherwise the rules are applied at compile-time.</span></span>

<span data-ttu-id="958cd-1350">Bağlama zamanı işlenmesini bir *delegate_creation_expression* formun `new D(E)`burada `D` olduğu bir *delegate_type* ve `E` olduğu bir *ifadesi* , aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="958cd-1350">The binding-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*  <span data-ttu-id="958cd-1351">Varsa `E` bir yöntem grubu olduğundan temsilci oluşturma ifadesi, bir yöntem grubu dönüştürme aynı şekilde işlenir ([yöntem grubu dönüştürmeler](conversions.md#method-group-conversions)) öğesinden `E` için `D`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1351">If `E` is a method group, the delegate creation expression is processed in the same way as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="958cd-1352">Varsa `E` anonim bir işlev, temsilci oluşturma ifadesi, tıpkı bir anonim işlev dönüştürme olarak işlenir ([anonim işlev dönüştürmeler](conversions.md#anonymous-function-conversions)) gelen `E` için `D`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1352">If `E` is an anonymous function, the delegate creation expression is processed in the same way as an anonymous function conversion ([Anonymous function conversions](conversions.md#anonymous-function-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="958cd-1353">Varsa `E` bir değer `E` uyumlu olması gerekir ([temsilci bildirimi](delegates.md#delegate-declarations)) ile `D`, ve sonuç yeni oluşturulan bir temsilci türü başvuru `D` aynı çağrıya başvuruyor liste olarak `E`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1353">If `E` is a value, `E` must be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`, and the result is a reference to a newly created delegate of type `D` that refers to the same invocation list as `E`.</span></span> <span data-ttu-id="958cd-1354">Varsa `E` ile uyumlu olmayan `D`, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1354">If `E` is not compatible with `D`, a compile-time error occurs.</span></span>

<span data-ttu-id="958cd-1355">Çalışma zamanı işlenmesini bir *delegate_creation_expression* formun `new D(E)`burada `D` olduğu bir *delegate_type* ve `E` olduğu bir *ifadesi* , aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="958cd-1355">The run-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*   <span data-ttu-id="958cd-1356">Varsa `E` bir yöntem grubu olduğundan temsilci oluşturma ifadesi bir yöntem grubu dönüştürme olarak değerlendirilir ([yöntem grubu dönüştürmeler](conversions.md#method-group-conversions)) öğesinden `E` için `D`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1356">If `E` is a method group, the delegate creation expression is evaluated as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*   <span data-ttu-id="958cd-1357">Varsa `E` anonim bir işlev, temsilci oluşturma bir anonim işlev dönüştürme değerlendirmesinde `E` için `D` ([anonim işlev dönüştürmeler](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1357">If `E` is an anonymous function, the delegate creation is evaluated as an anonymous function conversion from `E` to `D` ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>
*   <span data-ttu-id="958cd-1358">Varsa `E` bir değeri bir *delegate_type*:</span><span class="sxs-lookup"><span data-stu-id="958cd-1358">If `E` is a value of a *delegate_type*:</span></span>
    * <span data-ttu-id="958cd-1359">`E` değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1359">`E` is evaluated.</span></span> <span data-ttu-id="958cd-1360">Bu değerlendirme bir özel durum neden olursa, başka bir adım yürütülür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1360">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="958cd-1361">Varsa değerini `E` olduğu `null`, `System.NullReferenceException` oluşturulur ve başka bir adım yürütülür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1361">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="958cd-1362">Temsilci türüne yeni bir örneğini `D` ayrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1362">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="958cd-1363">Yeni bir örneğini ayırmak yeterli bellek yoksa bir `System.OutOfMemoryException` oluşturulur ve başka bir adım yürütülür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1363">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="958cd-1364">Yeni temsilci örneğini temsilci örneği tarafından verilen aynı çağırma listesiyle başlatılır `E`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1364">The new delegate instance is initialized with the same invocation list as the delegate instance given by `E`.</span></span>

<span data-ttu-id="958cd-1365">Temsilci örneği ve ardından tüm temsilci ömrü boyunca sabit mi kaldığını bir temsilcinin çağırma listesi belirlenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1365">The invocation list of a delegate is determined when the delegate is instantiated and then remains constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="958cd-1366">Diğer bir deyişle, oluşturulduktan sonra bir temsilcinin hedef çağrılabilir varlıkları değiştirmek mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1366">In other words, it is not possible to change the target callable entities of a delegate once it has been created.</span></span> <span data-ttu-id="958cd-1367">Ne zaman iki temsilci olduğunda birleştirilir veya bir kaldırıldı diğerinden ([temsilci bildirimi](delegates.md#delegate-declarations)), yeni bir temsilci sonuçlanır; varolan bir temsilci değiştirilen içeriğini sahip.</span><span class="sxs-lookup"><span data-stu-id="958cd-1367">When two delegates are combined or one is removed from another ([Delegate declarations](delegates.md#delegate-declarations)), a new delegate results; no existing delegate has its contents changed.</span></span>

<span data-ttu-id="958cd-1368">Bir özellik, dizin oluşturucu, kullanıcı tanımlı işleç, örnek oluşturucusu, yok Edicisi veya statik Oluşturucu başvuran bir temsilci oluşturmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1368">It is not possible to create a delegate that refers to a property, indexer, user-defined operator, instance constructor, destructor, or static constructor.</span></span>

<span data-ttu-id="958cd-1369">Bir temsilci yukarıda açıklandığı gibi bir yöntem grubu, biçimsel parametre listesi oluşturulduğunda ve temsilcinin dönüş türü, seçmek için aşırı yüklenmiş yöntemler belirleyin.</span><span class="sxs-lookup"><span data-stu-id="958cd-1369">As described above, when a delegate is created from a method group, the formal parameter list and return type of the delegate determine which of the overloaded methods to select.</span></span> <span data-ttu-id="958cd-1370">Örnekte</span><span class="sxs-lookup"><span data-stu-id="958cd-1370">In the example</span></span>
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
<span data-ttu-id="958cd-1371">`A.f` alan ikinci başvuran bir temsilci ile başlatılan `Square` yöntemi yöntemin dönüş türünü ve biçimsel parametre listesi tam olarak eşleştiğinden `DoubleFunc`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1371">the `A.f` field is initialized with a delegate that refers to the second `Square` method because that method exactly matches the formal parameter list and return type of `DoubleFunc`.</span></span> <span data-ttu-id="958cd-1372">İkinci vardı `Square` yöntemi yüklenmemiş değil, bir derleme zamanı hatası oluşmuş.</span><span class="sxs-lookup"><span data-stu-id="958cd-1372">Had the second `Square` method not been present, a compile-time error would have occurred.</span></span>

#### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="958cd-1373">Anonim nesne oluşturma ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-1373">Anonymous object creation expressions</span></span>

<span data-ttu-id="958cd-1374">Bir *anonymous_object_creation_expression* anonim bir türün bir nesne oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1374">An *anonymous_object_creation_expression* is used to create an object of an anonymous type.</span></span>

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

<span data-ttu-id="958cd-1375">Anonim nesne Başlatıcı, anonim bir tür bildirir ve o türün örneğini döndürür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1375">An anonymous object initializer declares an anonymous type and returns an instance of that type.</span></span> <span data-ttu-id="958cd-1376">Anonim bir tür doğrudan devralan bir adsız sınıf türüdür `object`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1376">An anonymous type is a nameless class type that inherits directly from `object`.</span></span> <span data-ttu-id="958cd-1377">Anonim bir tür, türün bir örneğini oluşturmak için kullanılan anonim nesne başlatıcısından algılanan salt okunur özellikler bir dizi üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1377">The members of an anonymous type are a sequence of read-only properties inferred from the anonymous object initializer used to create an instance of the type.</span></span> <span data-ttu-id="958cd-1378">Özellikle, anonim nesne Başlatıcı form</span><span class="sxs-lookup"><span data-stu-id="958cd-1378">Specifically, an anonymous object initializer of the form</span></span>
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
<span data-ttu-id="958cd-1379">Formun anonim bir tür bildirir</span><span class="sxs-lookup"><span data-stu-id="958cd-1379">declares an anonymous type of the form</span></span>
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
<span data-ttu-id="958cd-1380">Burada her `Tx` karşılık gelen ifade türü `ex`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1380">where each `Tx` is the type of the corresponding expression `ex`.</span></span> <span data-ttu-id="958cd-1381">İçinde kullanılan ifade bir *member_declarator* bir türü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1381">The expression used in a *member_declarator* must have a type.</span></span> <span data-ttu-id="958cd-1382">Bu nedenle, bir ifade için bir derleme zamanı hatası olduğu bir *member_declarator* null veya anonim bir işlevdir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1382">Thus, it is a compile-time error for an expression in a *member_declarator* to be null or an anonymous function.</span></span> <span data-ttu-id="958cd-1383">Ayrıca bir derleme zamanı hatası güvenli olmayan bir tür için ifadeyi için bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1383">It is also a compile-time error for the expression to have an unsafe type.</span></span>

<span data-ttu-id="958cd-1384">Anonim bir türün ve parametre adlarını kendi `Equals` yöntemi derleyici tarafından otomatik olarak oluşturulur ve program metni başvurulamaz.</span><span class="sxs-lookup"><span data-stu-id="958cd-1384">The names of an anonymous type and of the parameter to its `Equals` method are automatically generated by the compiler and cannot be referenced in program text.</span></span>

<span data-ttu-id="958cd-1385">Aynı programın içinde aynı sırada aynı adları ve derleme zamanı türü özellikleri dizisi belirtin iki anonim nesne başlatıcıları aynı anonim türün örneğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1385">Within the same program, two anonymous object initializers that specify a sequence of properties of the same names and compile-time types in the same order will produce instances of the same anonymous type.</span></span>

<span data-ttu-id="958cd-1386">Örnekte</span><span class="sxs-lookup"><span data-stu-id="958cd-1386">In the example</span></span>
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
<span data-ttu-id="958cd-1387">son satırı atamaya izin `p1` ve `p2` anonim türleri aynı.</span><span class="sxs-lookup"><span data-stu-id="958cd-1387">the assignment on the last line is permitted because `p1` and `p2` are of the same anonymous type.</span></span>

<span data-ttu-id="958cd-1388">`Equals` Ve `GetHashcode` anonim türlerde geçersiz kılma yöntemleri devralınan yöntemleri `object`, açısından tanımlanan `Equals` ve `GetHashcode` özelliklerinin iki örneği aynı anonim tür eşit olacak şekilde, ve yalnızca bunların tüm özelliklerini eşitse.</span><span class="sxs-lookup"><span data-stu-id="958cd-1388">The `Equals` and `GetHashcode` methods on anonymous types override the methods inherited from `object`, and are defined in terms of the `Equals` and `GetHashcode` of the properties, so that two instances of the same anonymous type are equal if and only if all their properties are equal.</span></span>

<span data-ttu-id="958cd-1389">Basit bir ad üye bildirimcisi kısaltılmış ([anlam çıkarma](expressions.md#type-inference)), üye erişimi ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), bir taban erişim ([temel erişim](expressions.md#base-access)) ya da bir üyesi null koşullu erişim ([projeksiyon başlatıcılar olarak Null koşullu ifadeler](expressions.md#null-conditional-expressions-as-projection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1389">A member declarator can be abbreviated to a simple name ([Type inference](expressions.md#type-inference)), a member access ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), a base access ([Base access](expressions.md#base-access)) or a null-conditional member access ([Null-conditional expressions as projection initializers](expressions.md#null-conditional-expressions-as-projection-initializers)).</span></span> <span data-ttu-id="958cd-1390">Bu adlı bir ***projeksiyon Başlatıcı*** ve aynı ada sahip bir özellik ataması ve bildirimi için toplu özelliktir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1390">This is called a ***projection initializer*** and is shorthand for a declaration of and assignment to a property with the same name.</span></span> <span data-ttu-id="958cd-1391">Özellikle, üye bildirimcilerde formları</span><span class="sxs-lookup"><span data-stu-id="958cd-1391">Specifically, member declarators of the forms</span></span>
```csharp
identifier
expr.identifier
```
<span data-ttu-id="958cd-1392">sırasıyla aşağıdaki tam olarak eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="958cd-1392">are precisely equivalent to the following, respectively:</span></span>
```csharp
identifier = identifier
identifier = expr.identifier
```

<span data-ttu-id="958cd-1393">Bu nedenle, projeksiyon başlatıcısında *tanımlayıcı* hem değer ve alan veya özellik değeri atanır seçer.</span><span class="sxs-lookup"><span data-stu-id="958cd-1393">Thus, in a projection initializer the *identifier* selects both the value and the field or property to which the value is assigned.</span></span> <span data-ttu-id="958cd-1394">Sezgisel, yalnızca bir değer, aynı zamanda değerin adını bir projeksiyon Başlatıcısı yansıtıyor.</span><span class="sxs-lookup"><span data-stu-id="958cd-1394">Intuitively, a projection initializer projects not just a value, but also the name of the value.</span></span>

### <a name="the-typeof-operator"></a><span data-ttu-id="958cd-1395">Typeof işleci</span><span class="sxs-lookup"><span data-stu-id="958cd-1395">The typeof operator</span></span>

<span data-ttu-id="958cd-1396">`typeof` Elde etmek için kullanılan işleci `System.Type` nesne türü.</span><span class="sxs-lookup"><span data-stu-id="958cd-1396">The `typeof` operator is used to obtain the `System.Type` object for a type.</span></span>

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

<span data-ttu-id="958cd-1397">İlk formu *typeof_expression* oluşan bir `typeof` tarafından bir parantez içine alınmış anahtar sözcüğünü *türü*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1397">The first form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *type*.</span></span> <span data-ttu-id="958cd-1398">Bu formu, bir ifadenin sonucu `System.Type` belirtilen tür için nesnesi.</span><span class="sxs-lookup"><span data-stu-id="958cd-1398">The result of an expression of this form is the `System.Type` object for the indicated type.</span></span> <span data-ttu-id="958cd-1399">Yalnızca bir tane `System.Type` herhangi bir türü için nesne.</span><span class="sxs-lookup"><span data-stu-id="958cd-1399">There is only one `System.Type` object for any given type.</span></span> <span data-ttu-id="958cd-1400">Bir tür için buna `T`, `typeof(T) == typeof(T)` her zaman doğrudur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1400">This means that for a type `T`, `typeof(T) == typeof(T)` is always true.</span></span> <span data-ttu-id="958cd-1401">*Türü* olamaz `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1401">The *type* cannot be `dynamic`.</span></span>

<span data-ttu-id="958cd-1402">İkinci form, *typeof_expression* oluşan bir `typeof` tarafından bir parantez içine alınmış anahtar sözcüğünü *unbound_type_name*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1402">The second form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *unbound_type_name*.</span></span> <span data-ttu-id="958cd-1403">Bir *unbound_type_name* çok benzer bir *type_name* ([Namespace ve tür adları](basic-concepts.md#namespace-and-type-names)) dışında bir *unbound_type_name* içerir *generic_dimension_specifier*s burada bir *type_name* içeren *type_argument_list*s.</span><span class="sxs-lookup"><span data-stu-id="958cd-1403">An *unbound_type_name* is very similar to a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) except that an *unbound_type_name* contains *generic_dimension_specifier*s where a *type_name* contains *type_argument_list*s.</span></span> <span data-ttu-id="958cd-1404">Zaman işleneni bir *typeof_expression* hem de yapılarının dilbilgisi karşılayan belirteçler dizisidir *unbound_type_name* ve *type_name*, yani zaman içeriyor ne bir *generic_dimension_specifier* ya da bir *type_argument_list*, bir dizi belirteçleri olarak kabul edilir bir *type_name*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1404">When the operand of a *typeof_expression* is a sequence of tokens that satisfies the grammars of both *unbound_type_name* and *type_name*, namely when it contains neither a *generic_dimension_specifier* nor a *type_argument_list*, the sequence of tokens is considered to be a *type_name*.</span></span> <span data-ttu-id="958cd-1405">Anlamı bir *unbound_type_name* şu şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="958cd-1405">The meaning of an *unbound_type_name* is determined as follows:</span></span>

*  <span data-ttu-id="958cd-1406">Belirteçleri için bir dizi dönüştürme bir *type_name* her değiştirerek *generic_dimension_specifier* ile bir *type_argument_list* virgül aynı sayıda olması ve anahtar sözcüğü `object` her *type_argument*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1406">Convert the sequence of tokens to a *type_name* by replacing each *generic_dimension_specifier* with a *type_argument_list* having the same number of commas and the keyword `object` as each *type_argument*.</span></span>
*  <span data-ttu-id="958cd-1407">Ortaya çıkan değerlendirmek *type_name*, çalışırken tüm tür parametresi kısıtlamaları yoksayılıyor.</span><span class="sxs-lookup"><span data-stu-id="958cd-1407">Evaluate the resulting *type_name*, while ignoring all type parameter constraints.</span></span>
*  <span data-ttu-id="958cd-1408">*Unbound_type_name* sonuç olarak oluşturulan tür ile ilişkili ilişkisiz genel tür çözümler ([bağlı ve türleri ilişkisiz](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1408">The *unbound_type_name* resolves to the unbound generic type associated with the resulting constructed type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span>

<span data-ttu-id="958cd-1409">Sonucu *typeof_expression* olduğu `System.Type` genel tür ilişkisiz sonuç nesnesi.</span><span class="sxs-lookup"><span data-stu-id="958cd-1409">The result of the *typeof_expression* is the `System.Type` object for the resulting unbound generic type.</span></span>

<span data-ttu-id="958cd-1410">Üçüncü biçiminde *typeof_expression* oluşan bir `typeof` tarafından bir parantez içine alınmış anahtar sözcüğünü `void` anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="958cd-1410">The third form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized `void` keyword.</span></span> <span data-ttu-id="958cd-1411">Bu formu, bir ifadenin sonucu `System.Type` devamsızlık türü temsil eden nesne.</span><span class="sxs-lookup"><span data-stu-id="958cd-1411">The result of an expression of this form is the `System.Type` object that represents the absence of a type.</span></span> <span data-ttu-id="958cd-1412">Tarafından döndürülen tür nesnesi `typeof(void)` herhangi bir türü için döndürülen tür nesneden farklıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1412">The type object returned by `typeof(void)` is distinct from the type object returned for any type.</span></span> <span data-ttu-id="958cd-1413">Burada bu yöntem istediğiniz örneğiyle birlikte void yöntemleri dahil olmak üzere herhangi bir yöntem dönüş türünü temsil eden bir yöntemden faydalanabilir yansıma yöntemleri üzerine olanak dilde sınıf kitaplıkları bu özel türü nesne yararlıdır `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1413">This special type object is useful in class libraries that allow reflection onto methods in the language, where those methods wish to have a way to represent the return type of any method, including void methods, with an instance of `System.Type`.</span></span>

<span data-ttu-id="958cd-1414">`typeof` İşleci bir tür parametresinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1414">The `typeof` operator can be used on a type parameter.</span></span> <span data-ttu-id="958cd-1415">Sonuç `System.Type` tür parametresine bağlanan çalışma zamanı türü için nesne.</span><span class="sxs-lookup"><span data-stu-id="958cd-1415">The result is the `System.Type` object for the run-time type that was bound to the type parameter.</span></span> <span data-ttu-id="958cd-1416">`typeof` İşleci, aynı zamanda oluşturulmuş bir türü veya ilişkisiz bir genel tür üzerinde kullanılabilir ([bağlı ve türleri ilişkisiz](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1416">The `typeof` operator can also be used on a constructed type or an unbound generic type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span> <span data-ttu-id="958cd-1417">`System.Type` İlişkisiz bir genel tür ile aynı değil nesne `System.Type` örneği türündeki nesne.</span><span class="sxs-lookup"><span data-stu-id="958cd-1417">The `System.Type` object for an unbound generic type is not the same as the `System.Type` object of the instance type.</span></span> <span data-ttu-id="958cd-1418">Bu nedenle, her zaman olduğu kapalı bir oluşturulmuş tür çalışma zamanında örnek türü kendi `System.Type` nesne bağlıdır kullanın, çalışma zamanı tür bağımsız değişkenlerde ilişkisiz genel tür hiçbir tür bağımsız değişkenleri sahipken.</span><span class="sxs-lookup"><span data-stu-id="958cd-1418">The instance type is always a closed constructed type at run-time so its `System.Type` object depends on the run-time type arguments in use, while the unbound generic type has no type arguments.</span></span>

<span data-ttu-id="958cd-1419">Örnek</span><span class="sxs-lookup"><span data-stu-id="958cd-1419">The example</span></span>
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
<span data-ttu-id="958cd-1420">aşağıdaki çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="958cd-1420">produces the following output:</span></span>
```
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

<span data-ttu-id="958cd-1421">Unutmayın `int` ve `System.Int32` aynı türdedir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1421">Note that `int` and `System.Int32` are the same type.</span></span>

<span data-ttu-id="958cd-1422">Ayrıca sonucunu `typeof(X<>)` tür bağımsız değişkeni ancak sonucu benzemez `typeof(X<T>)` yapar.</span><span class="sxs-lookup"><span data-stu-id="958cd-1422">Also note that the result of `typeof(X<>)` does not depend on the type argument but the result of `typeof(X<T>)` does.</span></span>

### <a name="the-checked-and-unchecked-operators"></a><span data-ttu-id="958cd-1423">Checked ve unchecked işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-1423">The checked and unchecked operators</span></span>

<span data-ttu-id="958cd-1424">`checked` Ve `unchecked` işleçleri denetlemek için kullanılan ***taşma denetimi bağlamını*** Tamsayı türünde aritmetik işlemler ve dönüştürmeler için.</span><span class="sxs-lookup"><span data-stu-id="958cd-1424">The `checked` and `unchecked` operators are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

<span data-ttu-id="958cd-1425">`checked` İşleci denetlenen bir bağlamda kapsanan ifadeyi değerlendirir ve `unchecked` işleci işaretlenmemiş bir bağlamda kapsanan ifadeyi değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1425">The `checked` operator evaluates the contained expression in a checked context, and the `unchecked` operator evaluates the contained expression in an unchecked context.</span></span> <span data-ttu-id="958cd-1426">A *checked_expression* veya *unchecked_expression* tam olarak karşılık gelen bir *parenthesized_expression* ([parantezliifade](expressions.md#parenthesized-expressions)) dışında verilen taşma denetimi bağlamını kapsanan ifade değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1426">A *checked_expression* or *unchecked_expression* corresponds exactly to a *parenthesized_expression* ([Parenthesized expressions](expressions.md#parenthesized-expressions)), except that the contained expression is evaluated in the given overflow checking context.</span></span>

<span data-ttu-id="958cd-1427">İçerik denetleme taşma ayrıca aracılığıyla denetlenebilir `checked` ve `unchecked` deyimleri ([checked ve unchecked deyimleri](statements.md#the-checked-and-unchecked-statements)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1427">The overflow checking context can also be controlled through the `checked` and `unchecked` statements ([The checked and unchecked statements](statements.md#the-checked-and-unchecked-statements)).</span></span>

<span data-ttu-id="958cd-1428">Aşağıdaki işlemleri tarafından kurulan bağlam taşma etkilenen `checked` ve `unchecked` işleçler ve deyimler:</span><span class="sxs-lookup"><span data-stu-id="958cd-1428">The following operations are affected by the overflow checking context established by the `checked` and `unchecked` operators and statements:</span></span>

*  <span data-ttu-id="958cd-1429">Önceden tanımlanmış `++` ve `--` birli işleçleri ([sonek arttırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators) ve [önek arttırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)), işlenen integral olduğunda yazın.</span><span class="sxs-lookup"><span data-stu-id="958cd-1429">The predefined `++` and `--` unary operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="958cd-1430">Önceden tanımlanmış `-` birli işleç ([birli eksi işleci](expressions.md#unary-minus-operator)), işlenen bir tamsayı türünde olduğunda.</span><span class="sxs-lookup"><span data-stu-id="958cd-1430">The predefined `-` unary operator ([Unary minus operator](expressions.md#unary-minus-operator)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="958cd-1431">Önceden tanımlanmış `+`, `-`, `*`, ve `/` ikili işleçler ([aritmetik işleçler](expressions.md#arithmetic-operators)), her iki işlenen de integral türleridir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1431">The predefined `+`, `-`, `*`, and `/` binary operators ([Arithmetic operators](expressions.md#arithmetic-operators)), when both operands are of integral types.</span></span>
*  <span data-ttu-id="958cd-1432">Açık sayısal Dönüşümler ([açık sayısal dönüşümler](conversions.md#explicit-numeric-conversions)) bir integral türünden başka bir integral türüne veya gelen `float` veya `double` bir integral türü.</span><span class="sxs-lookup"><span data-stu-id="958cd-1432">Explicit numeric conversions ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from one integral type to another integral type, or from `float` or `double` to an integral type.</span></span>

<span data-ttu-id="958cd-1433">Ne zaman yukarıdaki işlemlerden biri hedef türü, işlemi gerçekleştirilen denetimler olduğu bağlam sonuç davranışına temsil etmek için çok büyük bir sonuç üreten:</span><span class="sxs-lookup"><span data-stu-id="958cd-1433">When one of the above operations produce a result that is too large to represent in the destination type, the context in which the operation is performed controls the resulting behavior:</span></span>

*  <span data-ttu-id="958cd-1434">İçinde bir `checked` bağlamı, işlemi bir sabit ifade ise ([sabit ifadeler](expressions.md#constant-expressions)), bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1434">In a `checked` context, if the operation is a constant expression ([Constant expressions](expressions.md#constant-expressions)), a compile-time error occurs.</span></span> <span data-ttu-id="958cd-1435">Aksi durumda, çalışma zamanında, işlemi gerçekleştirilirken bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1435">Otherwise, when the operation is performed at run-time, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="958cd-1436">İçinde bir `unchecked` bağlamı, sonucu, hedef türüne uygun değil herhangi bir yüksek sıra bitleri atarak kısaltılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1436">In an `unchecked` context, the result is truncated by discarding any high-order bits that do not fit in the destination type.</span></span>

<span data-ttu-id="958cd-1437">Sabit olmayan ifade (çalışma zamanında değerlendirilir ifadeleri) değil içine alınan göre `checked` veya `unchecked` işleçleri deyimleri, bağlam varsayılan taşma mi `unchecked` (derleyici gibi dış etkenler sürece anahtarlar ve yürütme ortamı yapılandırması) arama `checked` değerlendirme.</span><span class="sxs-lookup"><span data-stu-id="958cd-1437">For non-constant expressions (expressions that are evaluated at run-time) that are not enclosed by any `checked` or `unchecked` operators or statements, the default overflow checking context is `unchecked` unless external factors (such as compiler switches and execution environment configuration) call for `checked` evaluation.</span></span>

<span data-ttu-id="958cd-1438">(Tam olarak derleme zamanında değerlendirilebilen ifadeleri) sabit ifadeler için bağlam varsayılan taşma her zaman olduğu `checked`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1438">For constant expressions (expressions that can be fully evaluated at compile-time), the default overflow checking context is always `checked`.</span></span> <span data-ttu-id="958cd-1439">Sabit bir ifade açıkça yerleştirilir sürece bir `unchecked` içerik, ifade her zaman derleme zamanı değerlendirmesi sırasında gerçekleşen taşıyor, derleme zamanı hatalarına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1439">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur during the compile-time evaluation of the expression always cause compile-time errors.</span></span>

<span data-ttu-id="958cd-1440">Anonim bir işlev gövdesini etkilenmez `checked` veya `unchecked` anonim işlev gerçekleştiği bağlamı.</span><span class="sxs-lookup"><span data-stu-id="958cd-1440">The body of an anonymous function is not affected by `checked` or `unchecked` contexts in which the anonymous function occurs.</span></span>

<span data-ttu-id="958cd-1441">Örnekte</span><span class="sxs-lookup"><span data-stu-id="958cd-1441">In the example</span></span>
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
<span data-ttu-id="958cd-1442">Hiçbiri bir ifadenin derleme zamanında değerlendirilebilen bu yana herhangi bir derleme zamanı hata bildirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1442">no compile-time errors are reported since neither of the expressions can be evaluated at compile-time.</span></span> <span data-ttu-id="958cd-1443">Çalışma zamanında, `F` yöntem bir `System.OverflowException`ve `G` yöntemi-727379968 (aralık dışı sonuç alt 32 bit) döndürür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1443">At run-time, the `F` method throws a `System.OverflowException`, and the `G` method returns -727379968 (the lower 32 bits of the out-of-range result).</span></span> <span data-ttu-id="958cd-1444">Davranışını `H` yöntemi, derleme için içerik denetimi varsayılan taşma bağlıdır, ancak aynı olan `F` veya aynı `G`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1444">The behavior of the `H` method depends on the default overflow checking context for the compilation, but it is either the same as `F` or the same as `G`.</span></span>

<span data-ttu-id="958cd-1445">Örnekte</span><span class="sxs-lookup"><span data-stu-id="958cd-1445">In the example</span></span>
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
<span data-ttu-id="958cd-1446">Sabit ifadelerde değerlendirirken oluşan taşıyor `F` ve `H` ifadeler değerlendirilir çünkü bildirilmesini derleme zamanı hatalarına neden bir `checked` bağlamı.</span><span class="sxs-lookup"><span data-stu-id="958cd-1446">the overflows that occur when evaluating the constant expressions in `F` and `H` cause compile-time errors to be reported because the expressions are evaluated in a `checked` context.</span></span> <span data-ttu-id="958cd-1447">Taşma Sabit ifadede değerlendirirken gerçekleşir `G`, ancak değerlendirme gerçekleştikten sonra bir `unchecked` bağlamı taşma bildirilmedi.</span><span class="sxs-lookup"><span data-stu-id="958cd-1447">An overflow also occurs when evaluating the constant expression in `G`, but since the evaluation takes place in an `unchecked` context, the overflow is not reported.</span></span>

<span data-ttu-id="958cd-1448">`checked` Ve `unchecked` işleçler yalnızca metin içeriğini eklemek bulunan bu işlemler için bağlam taşma etkiler "`(`"ve"`)`" belirteçleri.</span><span class="sxs-lookup"><span data-stu-id="958cd-1448">The `checked` and `unchecked` operators only affect the overflow checking context for those operations that are textually contained within the "`(`" and "`)`" tokens.</span></span> <span data-ttu-id="958cd-1449">İşleçler, kapsanan ifade değerlendirme sonucu olarak çağrılan işlev üyeleri üzerinde etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1449">The operators have no effect on function members that are invoked as a result of evaluating the contained expression.</span></span> <span data-ttu-id="958cd-1450">Örnekte</span><span class="sxs-lookup"><span data-stu-id="958cd-1450">In the example</span></span>
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
<span data-ttu-id="958cd-1451">kullanımını `checked` içinde `F` değerlendirmesi etkilemez `x * y` içinde `Multiply`, bu nedenle `x * y` varsayılan taşma denetimi bağlamını değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1451">the use of `checked` in `F` does not affect the evaluation of `x * y` in `Multiply`, so `x * y` is evaluated in the default overflow checking context.</span></span>

<span data-ttu-id="958cd-1452">`unchecked` İşleci, işaretli integral türlerindeki sabitleri onaltılık gösterimde yazarken kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1452">The `unchecked` operator is convenient when writing constants of the signed integral types in hexadecimal notation.</span></span> <span data-ttu-id="958cd-1453">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="958cd-1453">For example:</span></span>
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

<span data-ttu-id="958cd-1454">Yukarıdaki onaltılık sabitler her ikisi de türlerinin `uint`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1454">Both of the hexadecimal constants above are of type `uint`.</span></span> <span data-ttu-id="958cd-1455">Sabitler dışında olduğundan `int` olmadan aralığı `unchecked` işleç, yayınlar için `int` derleme zamanı hataya neden.</span><span class="sxs-lookup"><span data-stu-id="958cd-1455">Because the constants are outside the `int` range, without the `unchecked` operator, the casts to `int` would produce compile-time errors.</span></span>

<span data-ttu-id="958cd-1456">`checked` Ve `unchecked` işleçler ve ifadeler programcılar bazı sayısal hesaplamalar belirli yönlerini denetlemek izin verin.</span><span class="sxs-lookup"><span data-stu-id="958cd-1456">The `checked` and `unchecked` operators and statements allow programmers to control certain aspects of some numeric calculations.</span></span> <span data-ttu-id="958cd-1457">Ancak, bazı sayısal işleçlerin davranışını işlenenlerini veri türlerine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1457">However, the behavior of some numeric operators depends on their operands' data types.</span></span> <span data-ttu-id="958cd-1458">Örneğin, her zaman iki ondalık basamak çarparak sonuçlanıyor bir özel durum taşmada bile içinde bir açıkça `unchecked` oluşturun.</span><span class="sxs-lookup"><span data-stu-id="958cd-1458">For example, multiplying two decimals always results in an exception on overflow even within an explicitly `unchecked` construct.</span></span> <span data-ttu-id="958cd-1459">Benzer şekilde, iki çarparak hiçbir zaman sonuçları bir özel durum taşmada bile içinde gezinen bir açıkça `checked` oluşturun.</span><span class="sxs-lookup"><span data-stu-id="958cd-1459">Similarly, multiplying two floats never results in an exception on overflow even within an explicitly `checked` construct.</span></span> <span data-ttu-id="958cd-1460">Ayrıca, diğer işleçlerin Denetleme modu tarafından hiçbir zaman etkilenir, yoksa varsayılan veya açık.</span><span class="sxs-lookup"><span data-stu-id="958cd-1460">In addition, other operators are never affected by the mode of checking, whether default or explicit.</span></span>

### <a name="default-value-expressions"></a><span data-ttu-id="958cd-1461">Varsayılan değer ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-1461">Default value expressions</span></span>

<span data-ttu-id="958cd-1462">Varsayılan değeri elde etmek için kullanılan bir varsayılan değer ifadesi ([varsayılan değerler](variables.md#default-values)) türü.</span><span class="sxs-lookup"><span data-stu-id="958cd-1462">A default value expression is used to obtain the default value ([Default values](variables.md#default-values)) of a type.</span></span> <span data-ttu-id="958cd-1463">Genellikle bir varsayılan değer ifadesi türü parametre bir değer türü veya bir başvuru türü ise, bilinmeyebilir beri tür parametreleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1463">Typically a default value expression is used for type parameters, since it may not be known if the type parameter is a value type or a reference type.</span></span> <span data-ttu-id="958cd-1464">(Öğesinden dönüştürme var `null` tür parametresi olarak bir başvuru türü bilinen sürece sabit bir tür parametresine.)</span><span class="sxs-lookup"><span data-stu-id="958cd-1464">(No conversion exists from the `null` literal to a type parameter unless the type parameter is known to be a reference type.)</span></span>

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

<span data-ttu-id="958cd-1465">Varsa *türü* içinde bir *default_value_expression* değerlendirir çalışma zamanında bir başvuru türü için sonucudur `null` bu türe dönüştürülüp.</span><span class="sxs-lookup"><span data-stu-id="958cd-1465">If the *type* in a *default_value_expression* evaluates at run-time to a reference type, the result is `null` converted to that type.</span></span> <span data-ttu-id="958cd-1466">Varsa *türü* içinde bir *default_value_expression* değerlendirir çalışma zamanında bir değer türüne, sonucudur *value_type*ait varsayılan değer ([varsayılan Oluşturucular](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1466">If the *type* in a *default_value_expression* evaluates at run-time to a value type, the result is the *value_type*'s default value ([Default constructors](types.md#default-constructors)).</span></span>

<span data-ttu-id="958cd-1467">A *default_value_expression* sabit ifade ([sabit ifadeler](expressions.md#constant-expressions)) türü bir başvuru türü veya bir başvuru türü olarak bilinen bir tür parametresi ise ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1467">A *default_value_expression* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) if the type is a reference type or a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="958cd-1468">Ayrıca, bir *default_value_expression* sabit bir ifade türü aşağıdaki değer türlerinden biri ise: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, ya da herhangi bir numaralandırma türü.</span><span class="sxs-lookup"><span data-stu-id="958cd-1468">In addition, a *default_value_expression* is a constant expression if the type is one of the following value types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, or any enumeration type.</span></span>


### <a name="nameof-expressions"></a><span data-ttu-id="958cd-1469">Nameof ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-1469">Nameof expressions</span></span>

<span data-ttu-id="958cd-1470">A *nameof_expression* bir program varlık olarak bir sabit dize adını almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1470">A *nameof_expression* is used to obtain the name of a program entity as a constant string.</span></span>

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

<span data-ttu-id="958cd-1471">Dilbilgisi bakımından Konuşmayı *named_entity* işlenen, her zaman bir ifade.</span><span class="sxs-lookup"><span data-stu-id="958cd-1471">Grammatically speaking, the *named_entity* operand is always an expression.</span></span> <span data-ttu-id="958cd-1472">Çünkü `nameof` ayrılmış bir anahtar sözcük olmayan bir nameof ifade her zaman basit bir ad ile bir çağrı sözdizimi kurallarına göre belirsiz `nameof`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1472">Because `nameof` is not a reserved keyword, a nameof expression is always syntactically ambiguous with an invocation of the simple name `nameof`.</span></span> <span data-ttu-id="958cd-1473">Ad arama, uyumluluk nedenleriyle ([basit adları](expressions.md#simple-names)) adını `nameof` başarılı, ifade olarak işlenir bir *invocation_expression* --çağırma olmasına bakılmaksızın yasal.</span><span class="sxs-lookup"><span data-stu-id="958cd-1473">For compatibility reasons, if a name lookup ([Simple names](expressions.md#simple-names)) of the name `nameof` succeeds, the expression is treated as an *invocation_expression* -- regardless of whether the invocation is legal.</span></span> <span data-ttu-id="958cd-1474">Aksi durumda olduğu bir *nameof_expression*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1474">Otherwise it is a *nameof_expression*.</span></span>

<span data-ttu-id="958cd-1475">Anlamını *named_entity* , bir *nameof_expression* Bunu anlamı; ifade olarak olarak diğer bir deyişle, ya da bir *simple_name*, *base_access*  veya *member_access*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1475">The meaning of the *named_entity* of a *nameof_expression* is the meaning of it as an expression; that is, either as a *simple_name*, a *base_access* or a *member_access*.</span></span> <span data-ttu-id="958cd-1476">Bununla birlikte, burada arama açıklanan [basit adları](expressions.md#simple-names) ve [üye erişimi](expressions.md#member-access) bir örnek üye statik bir bağlamda bulunamadığı için hatayla sonuçlanır bir *nameof_expression*böyle bir hata oluşturur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1476">However, where the lookup described in [Simple names](expressions.md#simple-names) and [Member access](expressions.md#member-access) results in an error because an instance member was found in a static context, a *nameof_expression* produces no such error.</span></span>

<span data-ttu-id="958cd-1477">Bir derleme zamanı hata için bir *named_entity* atamak için bir yöntem grubu bir *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1477">It is a compile-time error for a *named_entity* designating a method group to have a *type_argument_list*.</span></span> <span data-ttu-id="958cd-1478">Bir derleme zamanı hata için bir *named_entity_target* türünün `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1478">It is a compile time error for a *named_entity_target* to have the type `dynamic`.</span></span>

<span data-ttu-id="958cd-1479">A *nameof_expression* sabit ifade türü `string`, ve çalışma zamanında hiçbir etkisi olmaz.</span><span class="sxs-lookup"><span data-stu-id="958cd-1479">A *nameof_expression* is a constant expression of type `string`, and has no effect at runtime.</span></span> <span data-ttu-id="958cd-1480">Özellikle, kendi *named_entity* değerlendirilmez ve belirli atama onayına analiz amacıyla göz ardı edilir ([basit ifadeler için genel kurallar](variables.md#general-rules-for-simple-expressions)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1480">Specifically, its *named_entity* is not evaluated, and is ignored for the purposes of definite assignment analysis ([General rules for simple expressions](variables.md#general-rules-for-simple-expressions)).</span></span> <span data-ttu-id="958cd-1481">Değerini son tanımlayıcısıdır *named_entity* isteğe bağlı son önce *type_argument_list*, aşağıdaki şekilde dönüştürülmüş:</span><span class="sxs-lookup"><span data-stu-id="958cd-1481">Its value is the last identifier of the *named_entity* before the optional final *type_argument_list*, transformed in the following way:</span></span>

* <span data-ttu-id="958cd-1482">Önek "`@`", kullandıysanız kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1482">The prefix "`@`", if used, is removed.</span></span>
* <span data-ttu-id="958cd-1483">Her *unicode_escape_sequence* , karşılık gelen bir Unicode karakter dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1483">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
* <span data-ttu-id="958cd-1484">Tüm *formatting_characters* kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1484">Any *formatting_characters* are removed.</span></span>

<span data-ttu-id="958cd-1485">Uygulanan aynı dönüştürmeleri bunlar [tanımlayıcıları](lexical-structure.md#identifiers) tanımlayıcıları arasındaki test ederken.</span><span class="sxs-lookup"><span data-stu-id="958cd-1485">These are the same transformations applied in [Identifiers](lexical-structure.md#identifiers) when testing equality between identifiers.</span></span>

<span data-ttu-id="958cd-1486">TODO: örnekler</span><span class="sxs-lookup"><span data-stu-id="958cd-1486">TODO: examples</span></span>

### <a name="anonymous-method-expressions"></a><span data-ttu-id="958cd-1487">Anonim yöntem ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-1487">Anonymous method expressions</span></span>

<span data-ttu-id="958cd-1488">Bir *anonymous_method_expression* anonim bir işlevdir tanımlamanın iki yollarından biridir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1488">An *anonymous_method_expression* is one of two ways of defining an anonymous function.</span></span> <span data-ttu-id="958cd-1489">Bunlar daha ayrıntılı açıklanmıştır [anonim işlev ifadeleri](expressions.md#anonymous-function-expressions).</span><span class="sxs-lookup"><span data-stu-id="958cd-1489">These are further described in [Anonymous function expressions](expressions.md#anonymous-function-expressions).</span></span>

## <a name="unary-operators"></a><span data-ttu-id="958cd-1490">Birli işleçler</span><span class="sxs-lookup"><span data-stu-id="958cd-1490">Unary operators</span></span>

<span data-ttu-id="958cd-1491">`?`, `+`, `-`, `!`, `~`, `++`, `--`, Noktaya yayın, ve `await` işleçleri birli işleçler çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1491">The `?`, `+`, `-`, `!`, `~`, `++`, `--`, cast, and `await` operators are called the unary operators.</span></span>

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

<span data-ttu-id="958cd-1492">Varsa işleneni bir *unary_expression* derleme zamanı türü `dynamic`, dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1492">If the operand of a *unary_expression* has the compile-time type `dynamic`, it is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="958cd-1493">Bu durumda derleme zamanı türü *unary_expression* olduğu `dynamic`, ve çalışma zamanında çalışma zamanı tür işleneninin kullanarak aşağıda açıklanan çözümleme gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1493">In this case the compile-time type of the *unary_expression* is `dynamic`, and the resolution described below will take place at run-time using the run-time type of the operand.</span></span>

### <a name="null-conditional-operator"></a><span data-ttu-id="958cd-1494">Null koşullu işleci</span><span class="sxs-lookup"><span data-stu-id="958cd-1494">Null-conditional operator</span></span>

<span data-ttu-id="958cd-1495">Yalnızca işlenen null değilse null koşullu işleci, işleneni işlemlerin bir listesi uygular.</span><span class="sxs-lookup"><span data-stu-id="958cd-1495">The null-conditional operator applies a list of operations to its operand only if that operand is non-null.</span></span> <span data-ttu-id="958cd-1496">Aksi takdirde sonucudur işlecini `null`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1496">Otherwise the result of applying the operator is `null`.</span></span>

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

<span data-ttu-id="958cd-1497">Üye erişimi ve (hangi kendilerini null koşullu olabilir) öğe erişim işlemleri yanı sıra, çağırma işlemlerin listesini içerebilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1497">The list of operations can include member access and element access operations (which may themselves be null-conditional), as well as invocation.</span></span>

<span data-ttu-id="958cd-1498">Örneğin, ifade `a.b?[0]?.c()` olduğu bir *null_conditional_expression* ile bir *primary_expression* `a.b` ve *null_conditional_operations* `?[0]` (öğesi null koşullu erişimi) `?.c` (null koşullu üye erişimi) ve `()` (çağırma).</span><span class="sxs-lookup"><span data-stu-id="958cd-1498">For example, the expression `a.b?[0]?.c()` is a *null_conditional_expression* with a *primary_expression* `a.b` and *null_conditional_operations* `?[0]` (null-conditional element access), `?.c` (null-conditional member access) and `()` (invocation).</span></span>

<span data-ttu-id="958cd-1499">İçin bir *null_conditional_expression* `E` ile bir *primary_expression* `P`, let `E0` olması öndegelenmetiniçeriğinieklemekkaldırarakeldeedilenifade`?`her birinden *null_conditional_operations* , `E` bir sahip.</span><span class="sxs-lookup"><span data-stu-id="958cd-1499">For a *null_conditional_expression* `E` with a *primary_expression* `P`, let `E0` be the expression obtained by textually removing the leading `?` from each of the *null_conditional_operations* of `E` that have one.</span></span> <span data-ttu-id="958cd-1500">Kavramsal olarak, `E0` null denetimleri hiçbiri tarafından temsil edilen, hesaplanacak olan ifade `?`s bulma bir `null`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1500">Conceptually, `E0` is the expression that will be evaluated if none of the null checks represented by the `?`s do find a `null`.</span></span>

<span data-ttu-id="958cd-1501">Ayrıca, `E1` olması önde gelen metin içeriğini eklemek kaldırarak elde edilen ifade `?` yalnızca gelen, ilk *null_conditional_operations* içinde `E`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1501">Also, let `E1` be the expression obtained by textually removing the leading `?` from just the first of the *null_conditional_operations* in `E`.</span></span> <span data-ttu-id="958cd-1502">Bu neden bir *birincil ifade* (varsa yalnızca `?`) veya başka bir *null_conditional_expression*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1502">This may lead to a *primary-expression* (if there was just one `?`) or to another *null_conditional_expression*.</span></span>

<span data-ttu-id="958cd-1503">Örneğin, varsa `E` ifade `a.b?[0]?.c()`, ardından `E0` ifade `a.b[0].c()` ve `E1` ifade `a.b[0]?.c()`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1503">For example, if `E` is the expression `a.b?[0]?.c()`, then `E0` is the expression `a.b[0].c()` and `E1` is the expression `a.b[0]?.c()`.</span></span>

<span data-ttu-id="958cd-1504">Varsa `E0` nothing, ardından sınıflandırılır `E` nothing sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1504">If `E0` is classified as nothing, then `E` is classified as nothing.</span></span> <span data-ttu-id="958cd-1505">Aksi takdirde E bir değer olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1505">Otherwise E is classified as a value.</span></span>

<span data-ttu-id="958cd-1506">`E0` ve `E1` anlamını belirlemek için kullanılan `E`:</span><span class="sxs-lookup"><span data-stu-id="958cd-1506">`E0` and `E1` are used to determine the meaning of `E`:</span></span>

*  <span data-ttu-id="958cd-1507">Varsa `E` olarak gerçekleşir bir *statement_expression* anlamını `E` deyimi ile aynıdır</span><span class="sxs-lookup"><span data-stu-id="958cd-1507">If `E` occurs as a *statement_expression* the meaning of `E` is the same as the statement</span></span>

   ```csharp
   if ((object)P != null) E1;
   ```

   <span data-ttu-id="958cd-1508">dışında P yalnızca bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1508">except that P is evaluated only once.</span></span>

*  <span data-ttu-id="958cd-1509">Aksi takdirde `E0` bir derleme zamanı hatası oluşur nothing sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1509">Otherwise, if `E0` is classified as nothing a compile-time error occurs.</span></span>

*  <span data-ttu-id="958cd-1510">Aksi takdirde, izin `T0` türünde `E0`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1510">Otherwise, let `T0` be the type of `E0`.</span></span>

   *  <span data-ttu-id="958cd-1511">Varsa `T0` bir derleme zamanı hatası oluşur, bir başvuru türüyle veya NULL olmayan değer türü olması için bilinmeyen bir tür parametresi.</span><span class="sxs-lookup"><span data-stu-id="958cd-1511">If `T0` is a type parameter that is not known to be a reference type or a non-nullable value type, a compile-time error occurs.</span></span>

   *  <span data-ttu-id="958cd-1512">Varsa `T0` NULL olmayan bir değer türü ve türü olan `E` olduğu `T0?`ve anlamını `E` aynıdır</span><span class="sxs-lookup"><span data-stu-id="958cd-1512">If `T0` is a non-nullable value type, then the type of `E` is `T0?`, and the meaning of `E` is the same as</span></span>

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      <span data-ttu-id="958cd-1513">dışında `P` yalnızca bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1513">except that `P` is evaluated only once.</span></span>

   *  <span data-ttu-id="958cd-1514">Aksi takdirde E T0 türüdür ve E anlamını aynıdır</span><span class="sxs-lookup"><span data-stu-id="958cd-1514">Otherwise the type of E is T0, and the meaning of E is the same as</span></span>

      ```csharp
      ((object)P == null) ? null : E1
      ```

      <span data-ttu-id="958cd-1515">dışında `P` yalnızca bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1515">except that `P` is evaluated only once.</span></span>

<span data-ttu-id="958cd-1516">Varsa `E1` kendisi bir *null_conditional_expression*, daha sonra testler için iç içe yeniden, bu kurallar uygulanır `null` kalmayana kadar başka `?`'s, ve ifade tüm aşağı azaltıldı Birincil ifade `E0`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1516">If `E1` is itself a *null_conditional_expression*, then these rules are applied again, nesting the tests for `null` until there are no further `?`'s, and the expression has been reduced all the way down to the primary-expression `E0`.</span></span>

<span data-ttu-id="958cd-1517">Örneğin, ifade `a.b?[0]?.c()` deyim olduğu gibi bir deyim-ifadesi olarak gerçekleşir:</span><span class="sxs-lookup"><span data-stu-id="958cd-1517">For example, if the expression `a.b?[0]?.c()` occurs as a statement-expression, as in the statement:</span></span>
```csharp
a.b?[0]?.c();
```
<span data-ttu-id="958cd-1518">anlamını eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="958cd-1518">its meaning is equivalent to:</span></span>
```csharp
if (a.b != null) a.b[0]?.c();
```
<span data-ttu-id="958cd-1519">hangi yeniden eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="958cd-1519">which again is equivalent to:</span></span>
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
<span data-ttu-id="958cd-1520">Dışında `a.b` ve `a.b[0]` yalnızca bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1520">Except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

<span data-ttu-id="958cd-1521">Değerini, olarak kullanıldığı bir bağlam içinde ortaya çıkarsa:</span><span class="sxs-lookup"><span data-stu-id="958cd-1521">If it occurs in a context where its value is used, as in:</span></span>
```csharp
var x = a.b?[0]?.c();
```
<span data-ttu-id="958cd-1522">ve son çağırma türü, NULL olmayan bir değer türü değil varsayarak anlamını eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="958cd-1522">and assuming that the type of the final invocation is not a non-nullable value type, its meaning is equivalent to:</span></span>
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
<span data-ttu-id="958cd-1523">dışında `a.b` ve `a.b[0]` yalnızca bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1523">except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

#### <a name="null-conditional-expressions-as-projection-initializers"></a><span data-ttu-id="958cd-1524">Null koşullu ifadeler projeksiyon başlatıcılar olarak</span><span class="sxs-lookup"><span data-stu-id="958cd-1524">Null-conditional expressions as projection initializers</span></span>

<span data-ttu-id="958cd-1525">Null koşullu ifade yalnızca olarak kullanılabilir bir *member_declarator* içinde bir *anonymous_object_creation_expression* ([anonim nesne oluşturma ifadeleri](expressions.md#anonymous-object-creation-expressions)), Bu, bir (isteğe bağlı olarak null koşullu) üye erişimi ile sona erer.</span><span class="sxs-lookup"><span data-stu-id="958cd-1525">A null-conditional expression is only allowed as a *member_declarator* in an *anonymous_object_creation_expression* ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) if it ends with an (optionally null-conditional) member access.</span></span> <span data-ttu-id="958cd-1526">Dilbilgisi bakımından, bu gereklilik olarak ifade edilebilir:</span><span class="sxs-lookup"><span data-stu-id="958cd-1526">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

<span data-ttu-id="958cd-1527">Bu bir dil bilgisi için özel durumdur *null_conditional_expression* yukarıda.</span><span class="sxs-lookup"><span data-stu-id="958cd-1527">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="958cd-1528">Üretim için *member_declarator* içinde [anonim nesne oluşturma ifadeleri](expressions.md#anonymous-object-creation-expressions) yalnızca içeren *null_conditional_member_access*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1528">The production for *member_declarator* in [Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions) then includes only *null_conditional_member_access*.</span></span>

#### <a name="null-conditional-expressions-as-statement-expressions"></a><span data-ttu-id="958cd-1529">Null koşullu ifadeleri deyim ifadeleri olarak</span><span class="sxs-lookup"><span data-stu-id="958cd-1529">Null-conditional expressions as statement expressions</span></span>

<span data-ttu-id="958cd-1530">Null koşullu ifade yalnızca olarak kullanılabilir bir *statement_expression* ([ifade deyimleri](statements.md#expression-statements)) ile bir çağrı sona ererse.</span><span class="sxs-lookup"><span data-stu-id="958cd-1530">A null-conditional expression is only allowed as a *statement_expression* ([Expression statements](statements.md#expression-statements)) if it ends with an invocation.</span></span> <span data-ttu-id="958cd-1531">Dilbilgisi bakımından, bu gereklilik olarak ifade edilebilir:</span><span class="sxs-lookup"><span data-stu-id="958cd-1531">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="958cd-1532">Bu bir dil bilgisi için özel durumdur *null_conditional_expression* yukarıda.</span><span class="sxs-lookup"><span data-stu-id="958cd-1532">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="958cd-1533">Üretim için *statement_expression* içinde [ifade deyimleri](statements.md#expression-statements) yalnızca içeren *null_conditional_invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1533">The production for *statement_expression* in [Expression statements](statements.md#expression-statements) then includes only *null_conditional_invocation_expression*.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="958cd-1534">Tekli artı işleci</span><span class="sxs-lookup"><span data-stu-id="958cd-1534">Unary plus operator</span></span>

<span data-ttu-id="958cd-1535">Formun bir işlem için `+x`, birli işleç aşırı yükleme çözümlemesi ([birli işleç aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1535">For an operation of the form `+x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="958cd-1536">İşlenen seçili işleç parametre türüne dönüştürülür ve sonuç türü işlecinin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1536">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="958cd-1537">Önceden tanımlanmış birli artı işleçler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="958cd-1537">The predefined unary plus operators are:</span></span>

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

<span data-ttu-id="958cd-1538">Bu işleçlerden her biri için sonuç basit işlenenin değeridir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1538">For each of these operators, the result is simply the value of the operand.</span></span>

### <a name="unary-minus-operator"></a><span data-ttu-id="958cd-1539">Birli eksi işleci</span><span class="sxs-lookup"><span data-stu-id="958cd-1539">Unary minus operator</span></span>

<span data-ttu-id="958cd-1540">Formun bir işlem için `-x`, birli işleç aşırı yükleme çözümlemesi ([birli işleç aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1540">For an operation of the form `-x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="958cd-1541">İşlenen seçili işleç parametre türüne dönüştürülür ve sonuç türü işlecinin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1541">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="958cd-1542">Önceden tanımlanmış değilleme işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="958cd-1542">The predefined negation operators are:</span></span>

*  <span data-ttu-id="958cd-1543">Tamsayı olumsuzlama:</span><span class="sxs-lookup"><span data-stu-id="958cd-1543">Integer negation:</span></span>

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   <span data-ttu-id="958cd-1544">Sonuç çıkarılmasıyla hesaplanır `x` sıfırdan.</span><span class="sxs-lookup"><span data-stu-id="958cd-1544">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="958cd-1545">Varsa değerini, `x` işlenen türünde gösterilebilir en küçük değer (-2 ^ 31 için `int` veya -2 ^ 63 için `long`), matematik negation ardından `x` işlenen türü içinde gösterilebilir değil.</span><span class="sxs-lookup"><span data-stu-id="958cd-1545">If the value of of `x` is the smallest representable value of the operand type (-2^31 for `int` or -2^63 for `long`), then the mathematical negation of `x` is not representable within the operand type.</span></span> <span data-ttu-id="958cd-1546">İçinde bu meydana gelirse bir `checked` bağlamı bir `System.OverflowException` içinde ortaya çıkarsa; oluşturulur bir `unchecked` bağlamını, işlenenin sonucudur ve taşma bildirilmedi.</span><span class="sxs-lookup"><span data-stu-id="958cd-1546">If this occurs within a `checked` context, a `System.OverflowException` is thrown; if it occurs within an `unchecked` context, the result is the value of the operand and the overflow is not reported.</span></span>

   <span data-ttu-id="958cd-1547">Değilleme işleci, işlenenin türü olup olmadığını `uint`, türüne dönüştürülür `long`, ve sonuç türü `long`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1547">If the operand of the negation operator is of type `uint`, it is converted to type `long`, and the type of the result is `long`.</span></span> <span data-ttu-id="958cd-1548">Bir özel durum verir kuralıdır `int` değeri -2147483648 (-2 ^ 31) ondalık tamsayı sabit değeri yazılacak ([tamsayı sabit değerlerinde](lexical-structure.md#integer-literals)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1548">An exception is the rule that permits the `int` value -2147483648 (-2^31) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

   <span data-ttu-id="958cd-1549">Değilleme işleci, işlenenin türü olup olmadığını `ulong`, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1549">If the operand of the negation operator is of type `ulong`, a compile-time error occurs.</span></span> <span data-ttu-id="958cd-1550">Bir özel durum verir kuralıdır `long` değer -9223372036854775808 (-2 ^ 63) ondalık tamsayı sabit değeri yazılacak ([tamsayı sabit değerlerinde](lexical-structure.md#integer-literals)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1550">An exception is the rule that permits the `long` value -9223372036854775808 (-2^63) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

*  <span data-ttu-id="958cd-1551">Kayan nokta olumsuzlama:</span><span class="sxs-lookup"><span data-stu-id="958cd-1551">Floating-point negation:</span></span>

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   <span data-ttu-id="958cd-1552">Sonuç değeri `x` ters işareti ile.</span><span class="sxs-lookup"><span data-stu-id="958cd-1552">The result is the value of `x` with its sign inverted.</span></span> <span data-ttu-id="958cd-1553">Varsa `x` , NaN olup da NaN sonucudur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1553">If `x` is NaN, the result is also NaN.</span></span>

*  <span data-ttu-id="958cd-1554">Ondalık olumsuzlama:</span><span class="sxs-lookup"><span data-stu-id="958cd-1554">Decimal negation:</span></span>

   ```csharp
   decimal operator -(decimal x);
   ```

   <span data-ttu-id="958cd-1555">Sonuç çıkarılmasıyla hesaplanır `x` sıfırdan.</span><span class="sxs-lookup"><span data-stu-id="958cd-1555">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="958cd-1556">Ondalık olumsuzlama birli eksi işleci türünde kullanmaya eşdeğerdir `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1556">Decimal negation is equivalent to using the unary minus operator of type `System.Decimal`.</span></span>

### <a name="logical-negation-operator"></a><span data-ttu-id="958cd-1557">Mantıksal değilleme işleci</span><span class="sxs-lookup"><span data-stu-id="958cd-1557">Logical negation operator</span></span>

<span data-ttu-id="958cd-1558">Formun bir işlem için `!x`, birli işleç aşırı yükleme çözümlemesi ([birli işleç aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1558">For an operation of the form `!x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="958cd-1559">İşlenen seçili işleç parametre türüne dönüştürülür ve sonuç türü işlecinin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1559">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="958cd-1560">Yalnızca bir önceden tanımlanmış mantıksal değilleme işleci vardır:</span><span class="sxs-lookup"><span data-stu-id="958cd-1560">Only one predefined logical negation operator exists:</span></span>
```csharp
bool operator !(bool x);
```

<span data-ttu-id="958cd-1561">Bu işleç işlenenin mantıksal olumsuzlama hesaplar: işlenen `true`, sonuç `false`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1561">This operator computes the logical negation of the operand: If the operand is `true`, the result is `false`.</span></span> <span data-ttu-id="958cd-1562">İşlenen `false`, sonuç `true`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1562">If the operand is `false`, the result is `true`.</span></span>

### <a name="bitwise-complement-operator"></a><span data-ttu-id="958cd-1563">Bit düzeyinde tamamlama işleci</span><span class="sxs-lookup"><span data-stu-id="958cd-1563">Bitwise complement operator</span></span>

<span data-ttu-id="958cd-1564">Formun bir işlem için `~x`, birli işleç aşırı yükleme çözümlemesi ([birli işleç aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1564">For an operation of the form `~x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="958cd-1565">İşlenen seçili işleç parametre türüne dönüştürülür ve sonuç türü işlecinin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1565">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="958cd-1566">Önceden tanımlanmış bir bit düzeyinde tamamlayıcı işleçler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="958cd-1566">The predefined bitwise complement operators are:</span></span>
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

<span data-ttu-id="958cd-1567">Bu işleçlerden her biri için işlemin sonucunu, bit düzeyinde tamamlayıcı olan `x`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1567">For each of these operators, the result of the operation is the bitwise complement of `x`.</span></span>

<span data-ttu-id="958cd-1568">Her bir numaralandırma türü `E` örtük olarak aşağıdaki bit düzeyinde tamamlama işleci sağlar:</span><span class="sxs-lookup"><span data-stu-id="958cd-1568">Every enumeration type `E` implicitly provides the following bitwise complement operator:</span></span>

```csharp
E operator ~(E x);
```

<span data-ttu-id="958cd-1569">Değerlendirme sonucu `~x`burada `x` ifade bir sabit listesi türü `E` bir temel türü ile `U`, tam olarak değerlendiriliyor aynıdır `(E)(~(U)x)`dışında dönüştürme `E` olduğu her zaman olarak gerçekleştirilen sahipse bir `unchecked` bağlam ([checked ve unchecked işleçleri](expressions.md#the-checked-and-unchecked-operators)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1569">The result of evaluating `~x`, where `x` is an expression of an enumeration type `E` with an underlying type `U`, is exactly the same as evaluating `(E)(~(U)x)`, except that the conversion to `E` is always performed as if in an `unchecked` context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span>

### <a name="prefix-increment-and-decrement-operators"></a><span data-ttu-id="958cd-1570">Önek artırma ve azaltma işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-1570">Prefix increment and decrement operators</span></span>

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

<span data-ttu-id="958cd-1571">İşlenen bir önek artırma veya azaltma işlemi bir değişken, bir özellik erişimi veya bir dizin oluşturucu erişim sınıflandırılmış bir ifade olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1571">The operand of a prefix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="958cd-1572">İşleminin sonucu, işlenenin aynı türden bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1572">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="958cd-1573">Bir önek işleneni artırmak, veya azaltma işlemi bir özellik veya dizin oluşturucu erişim, özelliği veya dizin oluşturucu gerekir sahip hem bir `get` ve `set` erişimcisi.</span><span class="sxs-lookup"><span data-stu-id="958cd-1573">If the operand of a prefix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="958cd-1574">Durum bu değilse, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1574">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="958cd-1575">Birli işleç aşırı yükleme çözümlemesi ([birli işleç aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1575">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="958cd-1576">Önceden tanımlanmış `++` ve `--` işleçleri aşağıdaki türleri için mevcut: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`ve herhangi bir sabit listesi türü.</span><span class="sxs-lookup"><span data-stu-id="958cd-1576">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="958cd-1577">Önceden tanımlanmış `++` işleçleri dönüş işleneni ve önceden tanımlanmış 1 ekleyerek üretilen değeri `--` işleçleri, işlenende 1 çıkarılmasıyla oluşturulan değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1577">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="958cd-1578">İçinde bir `checked` bu ekleme veya çıkarma sonucu sonuç türü aralığının dışında ve sonuç türü bir integral türünü ya da sabit listesi türü ise bağlam bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1578">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="958cd-1579">Çalışma zamanı işlenmesini önek artırma veya azaltma işlemi formun `++x` veya `--x` aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="958cd-1579">The run-time processing of a prefix increment or decrement operation of the form `++x` or `--x` consists of the following steps:</span></span>

*   <span data-ttu-id="958cd-1580">Varsa `x` bir değişken olarak sınıflandırıldığını:</span><span class="sxs-lookup"><span data-stu-id="958cd-1580">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="958cd-1581">`x` değişkeni oluşturmak için değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1581">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="958cd-1582">Seçili işleç değeri ile çağrılan `x` bağımsız değişken olarak.</span><span class="sxs-lookup"><span data-stu-id="958cd-1582">The selected operator is invoked with the value of `x` as its argument.</span></span>
    * <span data-ttu-id="958cd-1583">Operatör tarafından döndürülen değer değerlendirmesi tarafından belirtilen konumda depolanan `x`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1583">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="958cd-1584">Operatör tarafından döndürülen değer, işlemin sonucunu haline gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1584">The value returned by the operator becomes the result of the operation.</span></span>
*   <span data-ttu-id="958cd-1585">Varsa `x` bir özellik veya dizin oluşturucu erişim sınıflandırıldığını:</span><span class="sxs-lookup"><span data-stu-id="958cd-1585">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="958cd-1586">Örnek ifade (varsa `x` değil `static`) ve bağımsız değişken listesi (varsa `x` bir dizin oluşturucu erişim) ile ilişkili `x` değerlendirilir, ve sonuçları sonraki kullanılan `get` ve `set` erişimci çağrıları.</span><span class="sxs-lookup"><span data-stu-id="958cd-1586">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="958cd-1587">`get` Erişimcisine `x` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1587">The `get` accessor of `x` is invoked.</span></span>
    * <span data-ttu-id="958cd-1588">Seçili işleç tarafından döndürülen değerle çağrılır `get` bağımsız değişken olarak erişimcisi.</span><span class="sxs-lookup"><span data-stu-id="958cd-1588">The selected operator is invoked with the value returned by the `get` accessor as its argument.</span></span>
    * <span data-ttu-id="958cd-1589">`set` Erişimcisine `x` operatör olarak tarafından döndürülen değerle çağrılır, `value` bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="958cd-1589">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="958cd-1590">Operatör tarafından döndürülen değer, işlemin sonucunu haline gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1590">The value returned by the operator becomes the result of the operation.</span></span>

<span data-ttu-id="958cd-1591">`++` Ve `--` işleçleri de destekler sonek gösteriminde ([sonek arttırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1591">The `++` and `--` operators also support postfix notation ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="958cd-1592">Genellikle, sonucunu `x++` veya `x--` değeri `x` işleminden önce ise sonucu `++x` veya `--x` değeri `x` işleminden sonra.</span><span class="sxs-lookup"><span data-stu-id="958cd-1592">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="958cd-1593">Her iki durumda da `x` kendisini işleminden sonra aynı değere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1593">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="958cd-1594">Bir `operator++` veya `operator--` uygulama sonek veya önek gösterim kullanılarak çağrılacak.</span><span class="sxs-lookup"><span data-stu-id="958cd-1594">An `operator++` or `operator--` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="958cd-1595">İki gösterimler için ayrı bir işleç uygulamalarına sahip mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1595">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="958cd-1596">Cast ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-1596">Cast expressions</span></span>

<span data-ttu-id="958cd-1597">A *cast_expression* açıkça bir ifadenin belirtilen bir türe dönüştürmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1597">A *cast_expression* is used to explicitly convert an expression to a given type.</span></span>

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

<span data-ttu-id="958cd-1598">A *cast_expression* formun `(T)E`burada `T` olduğu bir *türü* ve `E` olduğu bir *unary_expression*, açık bir gerçekleştirir dönüştürme ([açık dönüştürmeler](conversions.md#explicit-conversions)) değerinin `E` türüne `T`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1598">A *cast_expression* of the form `(T)E`, where `T` is a *type* and `E` is a *unary_expression*, performs an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) of the value of `E` to type `T`.</span></span> <span data-ttu-id="958cd-1599">Açık bir dönüştürme gelen varsa `E` için `T`, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1599">If no explicit conversion exists from `E` to `T`, a binding-time error occurs.</span></span> <span data-ttu-id="958cd-1600">Aksi halde sonuç, açık bir dönüştürme tarafından üretilen değeridir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1600">Otherwise, the result is the value produced by the explicit conversion.</span></span> <span data-ttu-id="958cd-1601">Sonucu her zaman bir değer olarak sınıflandırılır bile `E` bir değişkeni gösterir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1601">The result is always classified as a value, even if `E` denotes a variable.</span></span>

<span data-ttu-id="958cd-1602">Dil bilgisi için bir *cast_expression* için belirli bir söz dizimi belirsizliğe neden olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1602">The grammar for a *cast_expression* leads to certain syntactic ambiguities.</span></span> <span data-ttu-id="958cd-1603">Örneğin, ifade `(x)-y` ya da olarak yorumlanabilecek bir *cast_expression* (atanacağını `-y` türüne `x`) veya farklı bir *additive_expression* birlikte bir *parenthesized_expression* (değeri hesaplar `x - y)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1603">For example, the expression `(x)-y` could either be interpreted as a *cast_expression* (a cast of `-y` to type `x`) or as an *additive_expression* combined with a *parenthesized_expression* (which computes the value `x - y)`.</span></span>

<span data-ttu-id="958cd-1604">Çözümlenecek *cast_expression* belirsizlikler, aşağıdaki kural var: bir dizi bir veya daha fazla *belirteci*s ([boşluk](lexical-structure.md#white-space)) içine parantez içinde başlangıç olarak kabul edilir bir *cast_expression* yalnızca aşağıdakilerden en az biri doğruysa:</span><span class="sxs-lookup"><span data-stu-id="958cd-1604">To resolve *cast_expression* ambiguities, the following rule exists: A sequence of one or more *token*s ([White space](lexical-structure.md#white-space)) enclosed in parentheses is considered the start of a *cast_expression* only if at least one of the following are true:</span></span>

*  <span data-ttu-id="958cd-1605">Bir dizi belirteçleri doğru dilbilgisi için olan bir *türü*, ancak bir *ifade*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1605">The sequence of tokens is correct grammar for a *type*, but not for an *expression*.</span></span>
*  <span data-ttu-id="958cd-1606">Dizi belirteçleri doğru dilbilgisi için olan bir *türü*, ve ayraç takip belirtecin belirteç "`~`", belirteç "`!`", belirteç "`(`", bir  *tanımlayıcı* ([Unicode karakter kaçış dizileri](lexical-structure.md#unicode-character-escape-sequences)), *değişmez değer* ([değişmez değerleri](lexical-structure.md#literals)), veya tüm *anahtar sözcüğü*([Anahtar sözcükleri](lexical-structure.md#keywords)) dışında `as` ve `is`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1606">The sequence of tokens is correct grammar for a *type*, and the token immediately following the closing parentheses is the token "`~`", the token "`!`", the token "`(`", an *identifier* ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)), a *literal* ([Literals](lexical-structure.md#literals)), or any *keyword* ([Keywords](lexical-structure.md#keywords)) except `as` and `is`.</span></span>

<span data-ttu-id="958cd-1607">Terim "doğru dilbilgisi" yukarıda yalnızca, bir dizi belirteçleri belirli dilbilgisi üretim uymalıdır anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1607">The term "correct grammar" above means only that the sequence of tokens must conform to the particular grammatical production.</span></span> <span data-ttu-id="958cd-1608">Özellikle bağlı tüm tanımlayıcılar gerçek anlamını algılamaz.</span><span class="sxs-lookup"><span data-stu-id="958cd-1608">It specifically does not consider the actual meaning of any constituent identifiers.</span></span> <span data-ttu-id="958cd-1609">Örneğin, varsa `x` ve `y` , ardından tanımlayıcılardır `x.y` olan bir tür için doğru dilbilgisi bile `x.y` gerçekte bir tür belirtmek değil.</span><span class="sxs-lookup"><span data-stu-id="958cd-1609">For example, if `x` and `y` are identifiers, then `x.y` is correct grammar for a type, even if `x.y` doesn't actually denote a type.</span></span>

<span data-ttu-id="958cd-1610">Takip eden, varsa Kesinleştirme kuraldan `x` ve `y` tanımlayıcılardır, `(x)y`, `(x)(y)`, ve `(x)(-y)` olan *cast_expression*s, ancak `(x)-y` değil, bile `x` bir türü tanımlar.</span><span class="sxs-lookup"><span data-stu-id="958cd-1610">From the disambiguation rule it follows that, if `x` and `y` are identifiers, `(x)y`, `(x)(y)`, and `(x)(-y)` are *cast_expression*s, but `(x)-y` is not, even if `x` identifies a type.</span></span> <span data-ttu-id="958cd-1611">Ancak, varsa `x` önceden tanımlanmış bir türü tanımlayan bir anahtar sözcüğü (gibi `int`), tüm dört formlar sonra *cast_expression*s (böyle bir anahtar sözcük büyük olasılıkla bir ifade kendisi tarafından çözümlenemediğinden).</span><span class="sxs-lookup"><span data-stu-id="958cd-1611">However, if `x` is a keyword that identifies a predefined type (such as `int`), then all four forms are *cast_expression*s (because such a keyword could not possibly be an expression by itself).</span></span>

### <a name="await-expressions"></a><span data-ttu-id="958cd-1612">Await ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-1612">Await expressions</span></span>

<span data-ttu-id="958cd-1613">Await işleci işlenen tarafından temsil edilen zaman uyumsuz işlem tamamlanıncaya kadar kapsayan zaman uyumsuz İşlev değerlendirmesi askıya almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1613">The await operator is used to suspend evaluation of the enclosing async function until the asynchronous operation represented by the operand has completed.</span></span>

```antlr
await_expression
    : 'await' unary_expression
    ;
```

<span data-ttu-id="958cd-1614">Bir *await_expression* yalnızca bir zaman uyumsuz işlev gövdesinde izin verilir ([yineleyiciler](classes.md#iterators)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1614">An *await_expression* is only allowed in the body of an async function ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="958cd-1615">İçinde en yakın kapsayan zaman uyumsuz işlev bir *await_expression* bu sayfalarda gerçekleşmeyebilir:</span><span class="sxs-lookup"><span data-stu-id="958cd-1615">Within the nearest enclosing async function, an *await_expression* may not occur in these places:</span></span>

*  <span data-ttu-id="958cd-1616">İç içe geçmiş (async olmayan) bir anonim işlev içinde</span><span class="sxs-lookup"><span data-stu-id="958cd-1616">Inside a nested (non-async) anonymous function</span></span>
*  <span data-ttu-id="958cd-1617">Bloğu içinde bir *lock_statement*</span><span class="sxs-lookup"><span data-stu-id="958cd-1617">Inside the block of a *lock_statement*</span></span>
*  <span data-ttu-id="958cd-1618">Güvenli olmayan bir bağlamda</span><span class="sxs-lookup"><span data-stu-id="958cd-1618">In an unsafe context</span></span>

<span data-ttu-id="958cd-1619">Unutmayın bir *await_expression* içinde birçok yerinden olamaz bir *query_expression*olanlar olmayan zaman uyumsuz lambda ifadelerini kullanmak sözdizimsel olarak dönüştürülür çünkü.</span><span class="sxs-lookup"><span data-stu-id="958cd-1619">Note that an *await_expression* cannot occur in most places within a *query_expression*, because those are syntactically transformed to use non-async lambda expressions.</span></span>

<span data-ttu-id="958cd-1620">Bir zaman uyumsuz işlev içinde `await` tanımlayıcı olarak kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="958cd-1620">Inside of an async function, `await` cannot be used as an identifier.</span></span> <span data-ttu-id="958cd-1621">Bu nedenle hiçbir belirsizliğine await ifadeleri ve tanımlayıcıları içeren çeşitli ifadeler arasında yoktur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1621">There is therefore no syntactic ambiguity between await-expressions and various expressions involving identifiers.</span></span> <span data-ttu-id="958cd-1622">Zaman uyumsuz işlevleri dışında `await` normal bir tanımlayıcı olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="958cd-1622">Outside of async functions, `await` acts as a normal identifier.</span></span>

<span data-ttu-id="958cd-1623">İşleneni bir *await_expression* çağrılır ***görev***.</span><span class="sxs-lookup"><span data-stu-id="958cd-1623">The operand of an *await_expression* is called the ***task***.</span></span> <span data-ttu-id="958cd-1624">Zaman tam olmayabilir veya zaman uyumsuz bir işlem temsil ettiği *await_expression* değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1624">It represents an asynchronous operation that may or may not be complete at the time the *await_expression* is evaluated.</span></span> <span data-ttu-id="958cd-1625">Await işleci amacı, beklenen görev tamamlanana kadar kapsayan zaman uyumsuz işlev yürütülmesini askıya alma ve sonra sonucunu elde etmektir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1625">The purpose of the await operator is to suspend execution of the enclosing async function until the awaited task is complete, and then obtain its outcome.</span></span>

#### <a name="awaitable-expressions"></a><span data-ttu-id="958cd-1626">Beklenebilir ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-1626">Awaitable expressions</span></span>

<span data-ttu-id="958cd-1627">Görev bir await ifadesi olması gereken ***beklenebilir***.</span><span class="sxs-lookup"><span data-stu-id="958cd-1627">The task of an await expression is required to be ***awaitable***.</span></span> <span data-ttu-id="958cd-1628">Bir ifade `t` aşağıdakilerden birini tutuyorsa beklenebilir olduğu:</span><span class="sxs-lookup"><span data-stu-id="958cd-1628">An expression `t` is awaitable if one of the following holds:</span></span>

*  <span data-ttu-id="958cd-1629">`t` derleme zamanı türü `dynamic`</span><span class="sxs-lookup"><span data-stu-id="958cd-1629">`t` is of compile time type `dynamic`</span></span>
*  <span data-ttu-id="958cd-1630">`t` sahip olarak adlandırılan bir erişilebilir örneği veya genişletme yöntemi `GetAwaiter` parametresiz ve hiçbir tür parametreleri ve dönüş türü ile `A` kendisi için aşağıdakilerin tümü tutun:</span><span class="sxs-lookup"><span data-stu-id="958cd-1630">`t` has an accessible instance or extension method called `GetAwaiter` with no parameters and no type parameters, and a return type `A` for which all of the following hold:</span></span>
   * <span data-ttu-id="958cd-1631">`A` arabirimi uygulayan `System.Runtime.CompilerServices.INotifyCompletion` (bundan böyle olarak bilinen `INotifyCompletion` kısaltma)</span><span class="sxs-lookup"><span data-stu-id="958cd-1631">`A` implements the interface `System.Runtime.CompilerServices.INotifyCompletion` (hereafter known as `INotifyCompletion` for brevity)</span></span>
   * <span data-ttu-id="958cd-1632">`A` erişilebilir, okunabilir örneği özelliği `IsCompleted` türü `bool`</span><span class="sxs-lookup"><span data-stu-id="958cd-1632">`A` has an accessible, readable instance property `IsCompleted` of type `bool`</span></span>
   * <span data-ttu-id="958cd-1633">`A` erişilebilir bir örnek yöntemi olan `GetResult` parametresiz ve hiçbir tür parametreleri</span><span class="sxs-lookup"><span data-stu-id="958cd-1633">`A` has an accessible instance method `GetResult` with no parameters and no type parameters</span></span>

<span data-ttu-id="958cd-1634">Amacı `GetAwaiter` yöntemi olduğundan edinmek için bir ***awaiter*** görev.</span><span class="sxs-lookup"><span data-stu-id="958cd-1634">The purpose of the `GetAwaiter` method is to obtain an ***awaiter*** for the task.</span></span> <span data-ttu-id="958cd-1635">Türü `A` çağrılır ***awaiter türü*** await ifadesi için.</span><span class="sxs-lookup"><span data-stu-id="958cd-1635">The type `A` is called the ***awaiter type*** for the await expression.</span></span>

<span data-ttu-id="958cd-1636">Amacı `IsCompleted` görev zaten tamamlandıysa, belirlemek için özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1636">The purpose of the `IsCompleted` property is to determine if the task is already complete.</span></span> <span data-ttu-id="958cd-1637">Bu durumda, değerlendirme askıya almak için gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1637">If so, there is no need to suspend evaluation.</span></span>

<span data-ttu-id="958cd-1638">Amacı `INotifyCompletion.OnCompleted` yöntemdir; görev için bir "Devam" kaydolma için başka bir deyişle bir temsilci (tür `System.Action`), çağrılacak görev tamamlandıktan sonra.</span><span class="sxs-lookup"><span data-stu-id="958cd-1638">The purpose of the `INotifyCompletion.OnCompleted` method is to sign up a "continuation" to the task; i.e. a delegate (of type `System.Action`) that will be invoked once the task is complete.</span></span>

<span data-ttu-id="958cd-1639">Amacı `GetResult` yöntemi tamamlandığında, görevin sonucunu elde etmektir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1639">The purpose of the `GetResult` method is to obtain the outcome of the task once it is complete.</span></span> <span data-ttu-id="958cd-1640">Bu sonucu bir sonuç değeri ile büyük olasılıkla Başarılı tamamlama olabilir veya tarafından oluşturulan bir özel durum olabilir `GetResult` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="958cd-1640">This outcome may be successful completion, possibly with a result value, or it may be an exception which is thrown by the `GetResult` method.</span></span>

#### <a name="classification-of-await-expressions"></a><span data-ttu-id="958cd-1641">Sınıflandırılması await ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-1641">Classification of await expressions</span></span>

<span data-ttu-id="958cd-1642">İfade `await t` ifade aynı şekilde sınıflandırılır `(t).GetAwaiter().GetResult()`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1642">The expression `await t` is classified the same way as the expression `(t).GetAwaiter().GetResult()`.</span></span> <span data-ttu-id="958cd-1643">Bu nedenle, dönüş türü `GetResult` olduğu `void`, *await_expression* nothing sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1643">Thus, if the return type of `GetResult` is `void`, the *await_expression* is classified as nothing.</span></span> <span data-ttu-id="958cd-1644">Void olmayan dönüş türü varsa `T`, *await_expression* türünde bir değer sınıflandırılır `T`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1644">If it has a non-void return type `T`, the *await_expression* is classified as a value of type `T`.</span></span>

#### <a name="runtime-evaluation-of-await-expressions"></a><span data-ttu-id="958cd-1645">Çalışma zamanı değerlendirmesini await ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-1645">Runtime evaluation of await expressions</span></span>

<span data-ttu-id="958cd-1646">Çalışma zamanında, ifade `await t` gibi değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="958cd-1646">At runtime, the expression `await t` is evaluated as follows:</span></span>

*  <span data-ttu-id="958cd-1647">Bir awaiter `a` ifadenin değerlendirilmesi yoluyla elde edilen `(t).GetAwaiter()`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1647">An awaiter `a` is obtained by evaluating the expression `(t).GetAwaiter()`.</span></span>
*  <span data-ttu-id="958cd-1648">A `bool` `b` ifadenin değerlendirilmesi yoluyla elde edilen `(a).IsCompleted`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1648">A `bool` `b` is obtained by evaluating the expression `(a).IsCompleted`.</span></span>
*  <span data-ttu-id="958cd-1649">Varsa `b` olduğu `false` değerlendirme bağlıdır sonra `a` arabirimi uygulayan `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (bundan böyle olarak bilinen `ICriticalNotifyCompletion` kısaltma).</span><span class="sxs-lookup"><span data-stu-id="958cd-1649">If `b` is `false` then evaluation depends on whether `a` implements the interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (hereafter known as `ICriticalNotifyCompletion` for brevity).</span></span> <span data-ttu-id="958cd-1650">Bu denetim, zaman bağlama sırasında gerçekleştirilir; yani, çalışma zamanında, `a` derleme zamanı tür sahip `dynamic`ve aksi takdirde derleme zamanında.</span><span class="sxs-lookup"><span data-stu-id="958cd-1650">This check is done at binding time; i.e. at runtime if `a` has the compile time type `dynamic`, and at compile time otherwise.</span></span> <span data-ttu-id="958cd-1651">İzin `r` sürdürme temsilci belirtmek ([yineleyiciler](classes.md#iterators)):</span><span class="sxs-lookup"><span data-stu-id="958cd-1651">Let `r` denote the resumption delegate ([Iterators](classes.md#iterators)):</span></span>
    * <span data-ttu-id="958cd-1652">Varsa `a` uygulamıyor `ICriticalNotifyCompletion`, ifade `(a as (INotifyCompletion)).OnCompleted(r)` değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1652">If `a` does not implement `ICriticalNotifyCompletion`, then the expression `(a as (INotifyCompletion)).OnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="958cd-1653">Varsa `a` uygulamak `ICriticalNotifyCompletion`, ifade `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1653">If `a` does implement `ICriticalNotifyCompletion`, then the expression `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="958cd-1654">Değerlendirme sonra askıya alınır ve denetim, zaman uyumsuz işlev geçerli çağırana döndürülür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1654">Evaluation is then suspended, and control is returned to the current caller of the async function.</span></span>
*  <span data-ttu-id="958cd-1655">Ya da hemen sonra (varsa `b` olduğu `true`), veya sürdürme temsilcinin sonraki çağrı sırasında (varsa `b` olduğu `false`), ifade `(a).GetResult()` değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1655">Either immediately after (if `b` was `true`), or upon later invocation of the resumption delegate (if `b` was `false`), the expression `(a).GetResult()` is evaluated.</span></span> <span data-ttu-id="958cd-1656">Bir değer döndürürse, bu değeri sonucudur *await_expression*.</span><span class="sxs-lookup"><span data-stu-id="958cd-1656">If it returns a value, that value is the result of the *await_expression*.</span></span> <span data-ttu-id="958cd-1657">Aksi halde sonuç hiçbir şey olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1657">Otherwise the result is nothing.</span></span>

<span data-ttu-id="958cd-1658">Arabirim yöntemleri bir awaiter'ın uygulaması `INotifyCompletion.OnCompleted` ve `ICriticalNotifyCompletion.UnsafeOnCompleted` temsilci edilmesini `r` en fazla bir kez çağrılacak.</span><span class="sxs-lookup"><span data-stu-id="958cd-1658">An awaiter's implementation of the interface methods `INotifyCompletion.OnCompleted` and `ICriticalNotifyCompletion.UnsafeOnCompleted` should cause the delegate `r` to be invoked at most once.</span></span> <span data-ttu-id="958cd-1659">Aksi takdirde, kapsayan zaman uyumsuz işlev davranışı tanımsızdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1659">Otherwise, the behavior of the enclosing async function is undefined.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="958cd-1660">Aritmetik işleçler</span><span class="sxs-lookup"><span data-stu-id="958cd-1660">Arithmetic operators</span></span>

<span data-ttu-id="958cd-1661">`*`, `/`, `%`, `+`, Ve `-` işleçleri aritmetik işleçler çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1661">The `*`, `/`, `%`, `+`, and `-` operators are called the arithmetic operators.</span></span>

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

<span data-ttu-id="958cd-1662">Bir aritmetik işlecinin bir işleneni derleme zamanı türü olup olmadığını `dynamic`, ifadeyi dinamik olarak bağlı sonra ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="958cd-1662">If an operand of an arithmetic operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="958cd-1663">Bu durumda derleme zamanı ifadenin türüdür `dynamic`, ve çalışma zamanında derleme zamanı türü bu işlenenler çalışma zamanı türünü kullanarak aşağıda açıklanan çözümleme gerçekleşecek `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1663">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

### <a name="multiplication-operator"></a><span data-ttu-id="958cd-1664">Çarpma işleci</span><span class="sxs-lookup"><span data-stu-id="958cd-1664">Multiplication operator</span></span>

<span data-ttu-id="958cd-1665">Formun bir işlem için `x * y`, ikili İşleç aşırı yükleme çözümlemesi ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1665">For an operation of the form `x * y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="958cd-1666">İşlenenler seçilen operatöre parametre türleri dönüştürülür ve sonuç türü işlecinin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1666">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="958cd-1667">Aşağıdaki önceden tanımlanmış çarpma işleçleri listelenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1667">The predefined multiplication operators are listed below.</span></span> <span data-ttu-id="958cd-1668">Tüm işleçler çarpımını `x` ve `y`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1668">The operators all compute the product of `x` and `y`.</span></span>

*  <span data-ttu-id="958cd-1669">Tamsayı çarpma:</span><span class="sxs-lookup"><span data-stu-id="958cd-1669">Integer multiplication:</span></span>

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   <span data-ttu-id="958cd-1670">İçinde bir `checked` ürün sonuç türü aralığının dışında ise bağlam bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1670">In a `checked` context, if the product is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="958cd-1671">İçinde bir `unchecked` bağlamı, taşan bildirilmez ve tüm önemli sonuç türü yüksek düzeyli bitleri aralığın dışında atılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1671">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>


*  <span data-ttu-id="958cd-1672">Kayan nokta çarpma:</span><span class="sxs-lookup"><span data-stu-id="958cd-1672">Floating-point multiplication:</span></span>

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   <span data-ttu-id="958cd-1673">Ürün aritmetik IEEE 754 kurallarına göre hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1673">The product is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="958cd-1674">Sıfır olmayan sınırlı değerler tüm olası eşleştirme birleşimlerini, sıfır sonsuz ve NaN'ın sonuçları aşağıdaki tabloda listelenmektedir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1674">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="958cd-1675">Tabloda `x` ve `y` sonlu pozitif değerler.</span><span class="sxs-lookup"><span data-stu-id="958cd-1675">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="958cd-1676">`z` sonucu `x * y`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1676">`z` is the result of `x * y`.</span></span> <span data-ttu-id="958cd-1677">Sonucu hedef türü için çok büyük ise `z` sonsuz.</span><span class="sxs-lookup"><span data-stu-id="958cd-1677">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="958cd-1678">Sonucu hedef türü için çok küçükse `z` sıfırdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1678">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | <span data-ttu-id="958cd-1679">+ y</span><span class="sxs-lookup"><span data-stu-id="958cd-1679">+y</span></span>   | <span data-ttu-id="958cd-1680">-y</span><span class="sxs-lookup"><span data-stu-id="958cd-1680">-y</span></span>   | <span data-ttu-id="958cd-1681">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1681">+0</span></span>  | <span data-ttu-id="958cd-1682">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1682">-0</span></span>  | <span data-ttu-id="958cd-1683">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1683">+inf</span></span> | <span data-ttu-id="958cd-1684">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1684">-inf</span></span> | <span data-ttu-id="958cd-1685">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1685">NaN</span></span> | 
   | <span data-ttu-id="958cd-1686">+ x</span><span class="sxs-lookup"><span data-stu-id="958cd-1686">+x</span></span>   | <span data-ttu-id="958cd-1687">+ z</span><span class="sxs-lookup"><span data-stu-id="958cd-1687">+z</span></span>   | <span data-ttu-id="958cd-1688">-z</span><span class="sxs-lookup"><span data-stu-id="958cd-1688">-z</span></span>   | <span data-ttu-id="958cd-1689">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1689">+0</span></span>  | <span data-ttu-id="958cd-1690">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1690">-0</span></span>  | <span data-ttu-id="958cd-1691">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1691">+inf</span></span> | <span data-ttu-id="958cd-1692">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1692">-inf</span></span> | <span data-ttu-id="958cd-1693">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1693">NaN</span></span> | 
   | <span data-ttu-id="958cd-1694">-x</span><span class="sxs-lookup"><span data-stu-id="958cd-1694">-x</span></span>   | <span data-ttu-id="958cd-1695">-z</span><span class="sxs-lookup"><span data-stu-id="958cd-1695">-z</span></span>   | <span data-ttu-id="958cd-1696">+ z</span><span class="sxs-lookup"><span data-stu-id="958cd-1696">+z</span></span>   | <span data-ttu-id="958cd-1697">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1697">-0</span></span>  | <span data-ttu-id="958cd-1698">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1698">+0</span></span>  | <span data-ttu-id="958cd-1699">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1699">-inf</span></span> | <span data-ttu-id="958cd-1700">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1700">+inf</span></span> | <span data-ttu-id="958cd-1701">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1701">NaN</span></span> | 
   | <span data-ttu-id="958cd-1702">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1702">+0</span></span>   | <span data-ttu-id="958cd-1703">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1703">+0</span></span>   | <span data-ttu-id="958cd-1704">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1704">-0</span></span>   | <span data-ttu-id="958cd-1705">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1705">+0</span></span>  | <span data-ttu-id="958cd-1706">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1706">-0</span></span>  | <span data-ttu-id="958cd-1707">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1707">NaN</span></span>  | <span data-ttu-id="958cd-1708">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1708">NaN</span></span>  | <span data-ttu-id="958cd-1709">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1709">NaN</span></span> | 
   | <span data-ttu-id="958cd-1710">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1710">-0</span></span>   | <span data-ttu-id="958cd-1711">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1711">-0</span></span>   | <span data-ttu-id="958cd-1712">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1712">+0</span></span>   | <span data-ttu-id="958cd-1713">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1713">-0</span></span>  | <span data-ttu-id="958cd-1714">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1714">+0</span></span>  | <span data-ttu-id="958cd-1715">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1715">NaN</span></span>  | <span data-ttu-id="958cd-1716">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1716">NaN</span></span>  | <span data-ttu-id="958cd-1717">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1717">NaN</span></span> | 
   | <span data-ttu-id="958cd-1718">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1718">+inf</span></span> | <span data-ttu-id="958cd-1719">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1719">+inf</span></span> | <span data-ttu-id="958cd-1720">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1720">-inf</span></span> | <span data-ttu-id="958cd-1721">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1721">NaN</span></span> | <span data-ttu-id="958cd-1722">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1722">NaN</span></span> | <span data-ttu-id="958cd-1723">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1723">+inf</span></span> | <span data-ttu-id="958cd-1724">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1724">-inf</span></span> | <span data-ttu-id="958cd-1725">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1725">NaN</span></span> | 
   | <span data-ttu-id="958cd-1726">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1726">-inf</span></span> | <span data-ttu-id="958cd-1727">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1727">-inf</span></span> | <span data-ttu-id="958cd-1728">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1728">+inf</span></span> | <span data-ttu-id="958cd-1729">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1729">NaN</span></span> | <span data-ttu-id="958cd-1730">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1730">NaN</span></span> | <span data-ttu-id="958cd-1731">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1731">-inf</span></span> | <span data-ttu-id="958cd-1732">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1732">+inf</span></span> | <span data-ttu-id="958cd-1733">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1733">NaN</span></span> | 
   | <span data-ttu-id="958cd-1734">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1734">NaN</span></span>  | <span data-ttu-id="958cd-1735">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1735">NaN</span></span>  | <span data-ttu-id="958cd-1736">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1736">NaN</span></span>  | <span data-ttu-id="958cd-1737">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1737">NaN</span></span> | <span data-ttu-id="958cd-1738">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1738">NaN</span></span> | <span data-ttu-id="958cd-1739">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1739">NaN</span></span>  | <span data-ttu-id="958cd-1740">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1740">NaN</span></span>  | <span data-ttu-id="958cd-1741">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1741">NaN</span></span> | 

*  <span data-ttu-id="958cd-1742">Ondalık çarpma:</span><span class="sxs-lookup"><span data-stu-id="958cd-1742">Decimal multiplication:</span></span>

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   <span data-ttu-id="958cd-1743">Sonuçta elde edilen değeri içinde temsil etmek için çok büyük ise `decimal` biçiminde bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1743">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="958cd-1744">Sonuç değeri içinde temsil etmek için çok küçük olup olmadığını `decimal` sonuç biçimi de sıfırdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1744">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="958cd-1745">Bir yuvarlama önce bu sonucun ölçek iki işlenenden ölçekler toplamıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1745">The scale of the result, before any rounding, is the sum of the scales of the two operands.</span></span>

   <span data-ttu-id="958cd-1746">Ondalık çarpma çarpma işleci türünde kullanmaya eşdeğerdir `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1746">Decimal multiplication is equivalent to using the multiplication operator of type `System.Decimal`.</span></span>


### <a name="division-operator"></a><span data-ttu-id="958cd-1747">Bölme işleci</span><span class="sxs-lookup"><span data-stu-id="958cd-1747">Division operator</span></span>

<span data-ttu-id="958cd-1748">Formun bir işlem için `x / y`, ikili İşleç aşırı yükleme çözümlemesi ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1748">For an operation of the form `x / y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="958cd-1749">İşlenenler seçilen operatöre parametre türleri dönüştürülür ve sonuç türü işlecinin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1749">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="958cd-1750">Önceden tanımlanmış bölme işleçlerini aşağıda listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1750">The predefined division operators are listed below.</span></span> <span data-ttu-id="958cd-1751">Tüm işleçler kalanını işlem `x` ve `y`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1751">The operators all compute the quotient of `x` and `y`.</span></span>

*  <span data-ttu-id="958cd-1752">Tamsayı bölme:</span><span class="sxs-lookup"><span data-stu-id="958cd-1752">Integer division:</span></span>

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   <span data-ttu-id="958cd-1753">Sağ işlenen değeri sıfır olursa bir `System.DivideByZeroException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1753">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="958cd-1754">Bölme işlemi sonucu sıfıra doğru yuvarlar.</span><span class="sxs-lookup"><span data-stu-id="958cd-1754">The division rounds the result towards zero.</span></span> <span data-ttu-id="958cd-1755">Bu nedenle sonuç mutlak değerini iki işlenenden sayının mutlak değerini eşit veya küçük en büyük olası tamsayı ' dir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1755">Thus the absolute value of the result is the largest possible integer that is less than or equal to the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="958cd-1756">İki işlenen de aynı işarete ve sıfır veya iki işlenenden işaretleri olduğunda negatif sıfır ya da pozitif sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1756">The result is zero or positive when the two operands have the same sign and zero or negative when the two operands have opposite signs.</span></span>

   <span data-ttu-id="958cd-1757">Sol işlenen en küçük gösterilebilir ise `int` veya `long` değeri ve sağ işlenen `-1`, taşma meydana gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1757">If the left operand is the smallest representable `int` or `long` value and the right operand is `-1`, an overflow occurs.</span></span> <span data-ttu-id="958cd-1758">İçinde bir `checked` bağlam, bu neden olur. bir `System.ArithmeticException` (veya bir alt yapanın) oluşturulması için.</span><span class="sxs-lookup"><span data-stu-id="958cd-1758">In a `checked` context, this causes a `System.ArithmeticException` (or a subclass thereof) to be thrown.</span></span> <span data-ttu-id="958cd-1759">İçinde bir `unchecked` bağlamında mı kullanılacağına uygulama tarafından tanımlanan bir `System.ArithmeticException` (veya bir alt yapanın) oluşturulur veya taşma, sol işlenen olan sonuç değerle bildirilmeyen gider.</span><span class="sxs-lookup"><span data-stu-id="958cd-1759">In an `unchecked` context, it is implementation-defined as to whether a `System.ArithmeticException` (or a subclass thereof) is thrown or the overflow goes unreported with the resulting value being that of the left operand.</span></span>

*  <span data-ttu-id="958cd-1760">Kayan nokta bölme:</span><span class="sxs-lookup"><span data-stu-id="958cd-1760">Floating-point division:</span></span>

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   <span data-ttu-id="958cd-1761">Sayının aritmetik IEEE 754 kurallarına göre hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1761">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="958cd-1762">Sıfır olmayan sınırlı değerler tüm olası eşleştirme birleşimlerini, sıfır sonsuz ve NaN'ın sonuçları aşağıdaki tabloda listelenmektedir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1762">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="958cd-1763">Tabloda `x` ve `y` sonlu pozitif değerler.</span><span class="sxs-lookup"><span data-stu-id="958cd-1763">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="958cd-1764">`z` sonucu `x / y`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1764">`z` is the result of `x / y`.</span></span> <span data-ttu-id="958cd-1765">Sonucu hedef türü için çok büyük ise `z` sonsuz.</span><span class="sxs-lookup"><span data-stu-id="958cd-1765">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="958cd-1766">Sonucu hedef türü için çok küçükse `z` sıfırdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1766">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="958cd-1767">+ y</span><span class="sxs-lookup"><span data-stu-id="958cd-1767">+y</span></span>   | <span data-ttu-id="958cd-1768">-y</span><span class="sxs-lookup"><span data-stu-id="958cd-1768">-y</span></span>   | <span data-ttu-id="958cd-1769">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1769">+0</span></span>   | <span data-ttu-id="958cd-1770">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1770">-0</span></span>   | <span data-ttu-id="958cd-1771">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1771">+inf</span></span> | <span data-ttu-id="958cd-1772">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1772">-inf</span></span> | <span data-ttu-id="958cd-1773">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1773">NaN</span></span>  | 
   | <span data-ttu-id="958cd-1774">+ x</span><span class="sxs-lookup"><span data-stu-id="958cd-1774">+x</span></span>   | <span data-ttu-id="958cd-1775">+ z</span><span class="sxs-lookup"><span data-stu-id="958cd-1775">+z</span></span>   | <span data-ttu-id="958cd-1776">-z</span><span class="sxs-lookup"><span data-stu-id="958cd-1776">-z</span></span>   | <span data-ttu-id="958cd-1777">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1777">+inf</span></span> | <span data-ttu-id="958cd-1778">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1778">-inf</span></span> | <span data-ttu-id="958cd-1779">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1779">+0</span></span>   | <span data-ttu-id="958cd-1780">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1780">-0</span></span>   | <span data-ttu-id="958cd-1781">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1781">NaN</span></span>  | 
   | <span data-ttu-id="958cd-1782">-x</span><span class="sxs-lookup"><span data-stu-id="958cd-1782">-x</span></span>   | <span data-ttu-id="958cd-1783">-z</span><span class="sxs-lookup"><span data-stu-id="958cd-1783">-z</span></span>   | <span data-ttu-id="958cd-1784">+ z</span><span class="sxs-lookup"><span data-stu-id="958cd-1784">+z</span></span>   | <span data-ttu-id="958cd-1785">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1785">-inf</span></span> | <span data-ttu-id="958cd-1786">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1786">+inf</span></span> | <span data-ttu-id="958cd-1787">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1787">-0</span></span>   | <span data-ttu-id="958cd-1788">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1788">+0</span></span>   | <span data-ttu-id="958cd-1789">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1789">NaN</span></span>  | 
   | <span data-ttu-id="958cd-1790">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1790">+0</span></span>   | <span data-ttu-id="958cd-1791">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1791">+0</span></span>   | <span data-ttu-id="958cd-1792">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1792">-0</span></span>   | <span data-ttu-id="958cd-1793">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1793">NaN</span></span>  | <span data-ttu-id="958cd-1794">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1794">NaN</span></span>  | <span data-ttu-id="958cd-1795">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1795">+0</span></span>   | <span data-ttu-id="958cd-1796">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1796">-0</span></span>   | <span data-ttu-id="958cd-1797">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1797">NaN</span></span>  | 
   | <span data-ttu-id="958cd-1798">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1798">-0</span></span>   | <span data-ttu-id="958cd-1799">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1799">-0</span></span>   | <span data-ttu-id="958cd-1800">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1800">+0</span></span>   | <span data-ttu-id="958cd-1801">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1801">NaN</span></span>  | <span data-ttu-id="958cd-1802">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1802">NaN</span></span>  | <span data-ttu-id="958cd-1803">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1803">-0</span></span>   | <span data-ttu-id="958cd-1804">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1804">+0</span></span>   | <span data-ttu-id="958cd-1805">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1805">NaN</span></span>  | 
   | <span data-ttu-id="958cd-1806">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1806">+inf</span></span> | <span data-ttu-id="958cd-1807">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1807">+inf</span></span> | <span data-ttu-id="958cd-1808">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1808">-inf</span></span> | <span data-ttu-id="958cd-1809">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1809">+inf</span></span> | <span data-ttu-id="958cd-1810">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1810">-inf</span></span> | <span data-ttu-id="958cd-1811">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1811">NaN</span></span>  | <span data-ttu-id="958cd-1812">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1812">NaN</span></span>  | <span data-ttu-id="958cd-1813">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1813">NaN</span></span>  | 
   | <span data-ttu-id="958cd-1814">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1814">-inf</span></span> | <span data-ttu-id="958cd-1815">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1815">-inf</span></span> | <span data-ttu-id="958cd-1816">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1816">+inf</span></span> | <span data-ttu-id="958cd-1817">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1817">-inf</span></span> | <span data-ttu-id="958cd-1818">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1818">+inf</span></span> | <span data-ttu-id="958cd-1819">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1819">NaN</span></span>  | <span data-ttu-id="958cd-1820">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1820">NaN</span></span>  | <span data-ttu-id="958cd-1821">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1821">NaN</span></span>  | 
   | <span data-ttu-id="958cd-1822">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1822">NaN</span></span>  | <span data-ttu-id="958cd-1823">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1823">NaN</span></span>  | <span data-ttu-id="958cd-1824">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1824">NaN</span></span>  | <span data-ttu-id="958cd-1825">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1825">NaN</span></span>  | <span data-ttu-id="958cd-1826">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1826">NaN</span></span>  | <span data-ttu-id="958cd-1827">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1827">NaN</span></span>  | <span data-ttu-id="958cd-1828">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1828">NaN</span></span>  | <span data-ttu-id="958cd-1829">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1829">NaN</span></span>  | 

*  <span data-ttu-id="958cd-1830">Ondalık bölme:</span><span class="sxs-lookup"><span data-stu-id="958cd-1830">Decimal division:</span></span>

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   <span data-ttu-id="958cd-1831">Sağ işlenen değeri sıfır olursa bir `System.DivideByZeroException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1831">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="958cd-1832">Sonuçta elde edilen değeri içinde temsil etmek için çok büyük ise `decimal` biçiminde bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1832">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="958cd-1833">Sonuç değeri içinde temsil etmek için çok küçük olup olmadığını `decimal` sonuç biçimi de sıfırdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1833">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="958cd-1834">Ölçek sonucu bir sonuç eşit korumak ölçeğin en küçük olan en yakın gerçek matematik sonucun ondalık gösterilebilir değere.</span><span class="sxs-lookup"><span data-stu-id="958cd-1834">The scale of the result is the smallest scale that will preserve a result equal to the nearest representable decimal value to the true mathematical result.</span></span>

   <span data-ttu-id="958cd-1835">Ondalık bölme bölme işleci türünde kullanmaya eşdeğerdir `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1835">Decimal division is equivalent to using the division operator of type `System.Decimal`.</span></span>


### <a name="remainder-operator"></a><span data-ttu-id="958cd-1836">Kalan işleci</span><span class="sxs-lookup"><span data-stu-id="958cd-1836">Remainder operator</span></span>

<span data-ttu-id="958cd-1837">Formun bir işlem için `x % y`, ikili İşleç aşırı yükleme çözümlemesi ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1837">For an operation of the form `x % y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="958cd-1838">İşlenenler seçilen operatöre parametre türleri dönüştürülür ve sonuç türü işlecinin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1838">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="958cd-1839">Aşağıdaki önceden tanımlanmış kalan işleçleri listelenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1839">The predefined remainder operators are listed below.</span></span> <span data-ttu-id="958cd-1840">Tüm işleçleri arasında bölme kalanını işlem `x` ve `y`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1840">The operators all compute the remainder of the division between `x` and `y`.</span></span>

*  <span data-ttu-id="958cd-1841">Tamsayı kalan:</span><span class="sxs-lookup"><span data-stu-id="958cd-1841">Integer remainder:</span></span>

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   <span data-ttu-id="958cd-1842">Sonucu `x % y` değeri tarafından üretilen `x - (x / y) * y`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1842">The result of `x % y` is the value produced by `x - (x / y) * y`.</span></span> <span data-ttu-id="958cd-1843">Varsa `y` sıfır, bir `System.DivideByZeroException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1843">If `y` is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="958cd-1844">Sol işlenen en küçük ise `int` veya `long` değeri ve sağ işlenen `-1`, `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1844">If the left operand is the smallest `int` or `long` value and the right operand is `-1`, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="958cd-1845">Hiçbir durumda mu `x % y` bir özel durum burada `x / y` bir özel durum oluşturması beklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="958cd-1845">In no case does `x % y` throw an exception where `x / y` would not throw an exception.</span></span>

*  <span data-ttu-id="958cd-1846">Kayan nokta kalanını:</span><span class="sxs-lookup"><span data-stu-id="958cd-1846">Floating-point remainder:</span></span>

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   <span data-ttu-id="958cd-1847">Sıfır olmayan sınırlı değerler tüm olası eşleştirme birleşimlerini, sıfır sonsuz ve NaN'ın sonuçları aşağıdaki tabloda listelenmektedir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1847">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="958cd-1848">Tabloda `x` ve `y` sonlu pozitif değerler.</span><span class="sxs-lookup"><span data-stu-id="958cd-1848">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="958cd-1849">`z` sonucu `x % y` ve olarak hesaplanan `x - n * y`burada `n` küçük veya eşit en büyük olası tamsayı `x / y`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1849">`z` is the result of `x % y` and is computed as `x - n * y`, where `n` is the largest possible integer that is less than or equal to `x / y`.</span></span> <span data-ttu-id="958cd-1850">Kalanı hesaplama, bu yöntem, tamsayı işlenenler için kullanılan benzer ancak IEEE 754 tanımından farklılık gösterir (burada `n` en yakın bir tamsayıdır `x / y`).</span><span class="sxs-lookup"><span data-stu-id="958cd-1850">This method of computing the remainder is analogous to that used for integer operands, but differs from the IEEE 754 definition (in which `n` is the integer closest to `x / y`).</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="958cd-1851">+ y</span><span class="sxs-lookup"><span data-stu-id="958cd-1851">+y</span></span>   | <span data-ttu-id="958cd-1852">-y</span><span class="sxs-lookup"><span data-stu-id="958cd-1852">-y</span></span>   | <span data-ttu-id="958cd-1853">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1853">+0</span></span>   | <span data-ttu-id="958cd-1854">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1854">-0</span></span>   | <span data-ttu-id="958cd-1855">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1855">+inf</span></span> | <span data-ttu-id="958cd-1856">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1856">-inf</span></span> | <span data-ttu-id="958cd-1857">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1857">NaN</span></span>  | 
   | <span data-ttu-id="958cd-1858">+ x</span><span class="sxs-lookup"><span data-stu-id="958cd-1858">+x</span></span>   | <span data-ttu-id="958cd-1859">+ z</span><span class="sxs-lookup"><span data-stu-id="958cd-1859">+z</span></span>   | <span data-ttu-id="958cd-1860">+ z</span><span class="sxs-lookup"><span data-stu-id="958cd-1860">+z</span></span>   | <span data-ttu-id="958cd-1861">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1861">NaN</span></span>  | <span data-ttu-id="958cd-1862">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1862">NaN</span></span>  | <span data-ttu-id="958cd-1863">x</span><span class="sxs-lookup"><span data-stu-id="958cd-1863">x</span></span>    | <span data-ttu-id="958cd-1864">x</span><span class="sxs-lookup"><span data-stu-id="958cd-1864">x</span></span>    | <span data-ttu-id="958cd-1865">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1865">NaN</span></span>  | 
   | <span data-ttu-id="958cd-1866">-x</span><span class="sxs-lookup"><span data-stu-id="958cd-1866">-x</span></span>   | <span data-ttu-id="958cd-1867">-z</span><span class="sxs-lookup"><span data-stu-id="958cd-1867">-z</span></span>   | <span data-ttu-id="958cd-1868">-z</span><span class="sxs-lookup"><span data-stu-id="958cd-1868">-z</span></span>   | <span data-ttu-id="958cd-1869">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1869">NaN</span></span>  | <span data-ttu-id="958cd-1870">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1870">NaN</span></span>  | <span data-ttu-id="958cd-1871">-x</span><span class="sxs-lookup"><span data-stu-id="958cd-1871">-x</span></span>   | <span data-ttu-id="958cd-1872">-x</span><span class="sxs-lookup"><span data-stu-id="958cd-1872">-x</span></span>   | <span data-ttu-id="958cd-1873">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1873">NaN</span></span>  | 
   | <span data-ttu-id="958cd-1874">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1874">+0</span></span>   | <span data-ttu-id="958cd-1875">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1875">+0</span></span>   | <span data-ttu-id="958cd-1876">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1876">+0</span></span>   | <span data-ttu-id="958cd-1877">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1877">NaN</span></span>  | <span data-ttu-id="958cd-1878">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1878">NaN</span></span>  | <span data-ttu-id="958cd-1879">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1879">+0</span></span>   | <span data-ttu-id="958cd-1880">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1880">+0</span></span>   | <span data-ttu-id="958cd-1881">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1881">NaN</span></span>  | 
   | <span data-ttu-id="958cd-1882">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1882">-0</span></span>   | <span data-ttu-id="958cd-1883">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1883">-0</span></span>   | <span data-ttu-id="958cd-1884">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1884">-0</span></span>   | <span data-ttu-id="958cd-1885">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1885">NaN</span></span>  | <span data-ttu-id="958cd-1886">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1886">NaN</span></span>  | <span data-ttu-id="958cd-1887">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1887">-0</span></span>   | <span data-ttu-id="958cd-1888">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1888">-0</span></span>   | <span data-ttu-id="958cd-1889">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1889">NaN</span></span>  | 
   | <span data-ttu-id="958cd-1890">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1890">+inf</span></span> | <span data-ttu-id="958cd-1891">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1891">NaN</span></span>  | <span data-ttu-id="958cd-1892">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1892">NaN</span></span>  | <span data-ttu-id="958cd-1893">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1893">NaN</span></span>  | <span data-ttu-id="958cd-1894">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1894">NaN</span></span>  | <span data-ttu-id="958cd-1895">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1895">NaN</span></span>  | <span data-ttu-id="958cd-1896">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1896">NaN</span></span>  | <span data-ttu-id="958cd-1897">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1897">NaN</span></span>  | 
   | <span data-ttu-id="958cd-1898">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1898">-inf</span></span> | <span data-ttu-id="958cd-1899">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1899">NaN</span></span>  | <span data-ttu-id="958cd-1900">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1900">NaN</span></span>  | <span data-ttu-id="958cd-1901">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1901">NaN</span></span>  | <span data-ttu-id="958cd-1902">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1902">NaN</span></span>  | <span data-ttu-id="958cd-1903">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1903">NaN</span></span>  | <span data-ttu-id="958cd-1904">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1904">NaN</span></span>  | <span data-ttu-id="958cd-1905">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1905">NaN</span></span>  | 
   | <span data-ttu-id="958cd-1906">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1906">NaN</span></span>  | <span data-ttu-id="958cd-1907">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1907">NaN</span></span>  | <span data-ttu-id="958cd-1908">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1908">NaN</span></span>  | <span data-ttu-id="958cd-1909">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1909">NaN</span></span>  | <span data-ttu-id="958cd-1910">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1910">NaN</span></span>  | <span data-ttu-id="958cd-1911">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1911">NaN</span></span>  | <span data-ttu-id="958cd-1912">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1912">NaN</span></span>  | <span data-ttu-id="958cd-1913">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1913">NaN</span></span>  | 

*  <span data-ttu-id="958cd-1914">Ondalık kalan:</span><span class="sxs-lookup"><span data-stu-id="958cd-1914">Decimal remainder:</span></span>

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   <span data-ttu-id="958cd-1915">Sağ işlenen değeri sıfır olursa bir `System.DivideByZeroException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1915">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="958cd-1916">Bir yuvarlama önce bu sonucun ölçek büyük ölçekleri iki işlenenden biri ve sonucun oturum sıfır olmayan, aynı `x`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1916">The scale of the result, before any rounding, is the larger of the scales of the two operands, and the sign of the result, if non-zero, is the same as that of `x`.</span></span>

   <span data-ttu-id="958cd-1917">Ondalık kalan kalan işleci türünde kullanmaya eşdeğerdir `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1917">Decimal remainder is equivalent to using the remainder operator of type `System.Decimal`.</span></span>


### <a name="addition-operator"></a><span data-ttu-id="958cd-1918">Toplama işleci</span><span class="sxs-lookup"><span data-stu-id="958cd-1918">Addition operator</span></span>

<span data-ttu-id="958cd-1919">Formun bir işlem için `x + y`, ikili İşleç aşırı yükleme çözümlemesi ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1919">For an operation of the form `x + y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="958cd-1920">İşlenenler seçilen operatöre parametre türleri dönüştürülür ve sonuç türü işlecinin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="958cd-1920">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="958cd-1921">Aşağıdaki önceden tanımlanmış toplama işleçleri listelenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1921">The predefined addition operators are listed below.</span></span> <span data-ttu-id="958cd-1922">Sayısal ve sabit listesi türleri için önceden tanımlanmış toplama işleçleri iki işlenenden toplam hesaplaması.</span><span class="sxs-lookup"><span data-stu-id="958cd-1922">For numeric and enumeration types, the predefined addition operators compute the sum of the two operands.</span></span> <span data-ttu-id="958cd-1923">Bir veya iki işlenenin türü dize olmadığında, önceden tanımlı toplama işleçleri işlenenler dize gösterimini art arda ekler.</span><span class="sxs-lookup"><span data-stu-id="958cd-1923">When one or both operands are of type string, the predefined addition operators concatenate the string representation of the operands.</span></span>

*  <span data-ttu-id="958cd-1924">Tamsayı ayrıca:</span><span class="sxs-lookup"><span data-stu-id="958cd-1924">Integer addition:</span></span>

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   <span data-ttu-id="958cd-1925">İçinde bir `checked` toplamı sonuç türü aralığının dışında ise bağlam bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1925">In a `checked` context, if the sum is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="958cd-1926">İçinde bir `unchecked` bağlamı, taşan bildirilmez ve tüm önemli sonuç türü yüksek düzeyli bitleri aralığın dışında atılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1926">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="958cd-1927">Kayan nokta ekleme:</span><span class="sxs-lookup"><span data-stu-id="958cd-1927">Floating-point addition:</span></span>

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   <span data-ttu-id="958cd-1928">Toplam aritmetik IEEE 754 kurallarına göre hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1928">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="958cd-1929">Sıfır olmayan sınırlı değerler tüm olası eşleştirme birleşimlerini, sıfır sonsuz ve NaN'ın sonuçları aşağıdaki tabloda listelenmektedir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1929">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="958cd-1930">Tabloda `x` ve `y` sınırlı sıfır olmayan değerler ve `z` sonucu `x + y`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1930">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x + y`.</span></span> <span data-ttu-id="958cd-1931">Varsa `x` ve `y` aynı büyüklük sahip ancak işaretlere ters `z` pozitif sıfır.</span><span class="sxs-lookup"><span data-stu-id="958cd-1931">If `x` and `y` have the same magnitude but opposite signs, `z` is positive zero.</span></span> <span data-ttu-id="958cd-1932">Varsa `x + y` hedef türünde temsil etmek için çok büyük `z` aynı işarete sahip bir sonsuzluk olup `x + y`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1932">If `x + y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x + y`.</span></span>

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="958cd-1933">y</span><span class="sxs-lookup"><span data-stu-id="958cd-1933">y</span></span>    | <span data-ttu-id="958cd-1934">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1934">+0</span></span>   | <span data-ttu-id="958cd-1935">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1935">-0</span></span>   | <span data-ttu-id="958cd-1936">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1936">+inf</span></span> | <span data-ttu-id="958cd-1937">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1937">-inf</span></span> | <span data-ttu-id="958cd-1938">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1938">NaN</span></span>  | 
   | <span data-ttu-id="958cd-1939">x</span><span class="sxs-lookup"><span data-stu-id="958cd-1939">x</span></span>    | <span data-ttu-id="958cd-1940">z</span><span class="sxs-lookup"><span data-stu-id="958cd-1940">z</span></span>    | <span data-ttu-id="958cd-1941">x</span><span class="sxs-lookup"><span data-stu-id="958cd-1941">x</span></span>    | <span data-ttu-id="958cd-1942">x</span><span class="sxs-lookup"><span data-stu-id="958cd-1942">x</span></span>    | <span data-ttu-id="958cd-1943">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1943">+inf</span></span> | <span data-ttu-id="958cd-1944">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1944">-inf</span></span> | <span data-ttu-id="958cd-1945">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1945">NaN</span></span>  | 
   | <span data-ttu-id="958cd-1946">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1946">+0</span></span>   | <span data-ttu-id="958cd-1947">y</span><span class="sxs-lookup"><span data-stu-id="958cd-1947">y</span></span>    | <span data-ttu-id="958cd-1948">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1948">+0</span></span>   | <span data-ttu-id="958cd-1949">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1949">+0</span></span>   | <span data-ttu-id="958cd-1950">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1950">+inf</span></span> | <span data-ttu-id="958cd-1951">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1951">-inf</span></span> | <span data-ttu-id="958cd-1952">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1952">NaN</span></span>  | 
   | <span data-ttu-id="958cd-1953">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1953">-0</span></span>   | <span data-ttu-id="958cd-1954">y</span><span class="sxs-lookup"><span data-stu-id="958cd-1954">y</span></span>    | <span data-ttu-id="958cd-1955">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-1955">+0</span></span>   | <span data-ttu-id="958cd-1956">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-1956">-0</span></span>   | <span data-ttu-id="958cd-1957">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1957">+inf</span></span> | <span data-ttu-id="958cd-1958">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1958">-inf</span></span> | <span data-ttu-id="958cd-1959">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1959">NaN</span></span>  | 
   | <span data-ttu-id="958cd-1960">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1960">+inf</span></span> | <span data-ttu-id="958cd-1961">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1961">+inf</span></span> | <span data-ttu-id="958cd-1962">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1962">+inf</span></span> | <span data-ttu-id="958cd-1963">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1963">+inf</span></span> | <span data-ttu-id="958cd-1964">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1964">+inf</span></span> | <span data-ttu-id="958cd-1965">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1965">NaN</span></span>  | <span data-ttu-id="958cd-1966">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1966">NaN</span></span>  | 
   | <span data-ttu-id="958cd-1967">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1967">-inf</span></span> | <span data-ttu-id="958cd-1968">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1968">-inf</span></span> | <span data-ttu-id="958cd-1969">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1969">-inf</span></span> | <span data-ttu-id="958cd-1970">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1970">-inf</span></span> | <span data-ttu-id="958cd-1971">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1971">NaN</span></span>  | <span data-ttu-id="958cd-1972">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-1972">-inf</span></span> | <span data-ttu-id="958cd-1973">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1973">NaN</span></span>  | 
   | <span data-ttu-id="958cd-1974">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1974">NaN</span></span>  | <span data-ttu-id="958cd-1975">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1975">NaN</span></span>  | <span data-ttu-id="958cd-1976">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1976">NaN</span></span>  | <span data-ttu-id="958cd-1977">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1977">NaN</span></span>  | <span data-ttu-id="958cd-1978">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1978">NaN</span></span>  | <span data-ttu-id="958cd-1979">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1979">NaN</span></span>  | <span data-ttu-id="958cd-1980">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-1980">NaN</span></span>  | 

*  <span data-ttu-id="958cd-1981">Ondalık ayrıca:</span><span class="sxs-lookup"><span data-stu-id="958cd-1981">Decimal addition:</span></span>

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   <span data-ttu-id="958cd-1982">Sonuçta elde edilen değeri içinde temsil etmek için çok büyük ise `decimal` biçiminde bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1982">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="958cd-1983">Bir yuvarlama önce bu sonucun ölçek büyük ölçekleri iki işlenenden biri ' dir.</span><span class="sxs-lookup"><span data-stu-id="958cd-1983">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="958cd-1984">Ondalık toplama türü Toplama işleci kullanmaya eşdeğerdir `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1984">Decimal addition is equivalent to using the addition operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="958cd-1985">Sabit listesi ekleme.</span><span class="sxs-lookup"><span data-stu-id="958cd-1985">Enumeration addition.</span></span> <span data-ttu-id="958cd-1986">Aşağıdaki önceden tanımlanmış işleçleri, her bir numaralandırma türü örtük olarak sağlar burada `E` sabit listesi türüdür ve `U` temel alınan türü `E`:</span><span class="sxs-lookup"><span data-stu-id="958cd-1986">Every enumeration type implicitly provides the following predefined operators, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   <span data-ttu-id="958cd-1987">Çalışma zamanında tam olarak bu işleçler değerlendirilir `(E)((U)x + (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1987">At run-time these operators are evaluated exactly as `(E)((U)x + (U)y)`.</span></span>

*  <span data-ttu-id="958cd-1988">Dize birleştirme:</span><span class="sxs-lookup"><span data-stu-id="958cd-1988">String concatenation:</span></span>

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   <span data-ttu-id="958cd-1989">Bu aşırı yüklemeler ikili `+` dize birleştirme işlecini gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="958cd-1989">These overloads of the binary `+` operator perform string concatenation.</span></span> <span data-ttu-id="958cd-1990">Dize birleştirme işleci ise `null`, boş bir dize yerine konur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1990">If an operand of string concatenation is `null`, an empty string is substituted.</span></span> <span data-ttu-id="958cd-1991">Aksi takdirde, herhangi bir dize olmayan bağımsız değişken dize gösterimine sanal çağırarak dönüştürülür `ToString` yöntemi türden devralınan `object`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1991">Otherwise, any non-string argument is converted to its string representation by invoking the virtual `ToString` method inherited from type `object`.</span></span> <span data-ttu-id="958cd-1992">Varsa `ToString` döndürür `null`, boş bir dize yerine konur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1992">If `ToString` returns `null`, an empty string is substituted.</span></span>

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

   Dize birleştirme işlecini sol işlenen sağ işlenenin karakterleridir karakterleri içeren bir dize sonucudur. Dize birleştirme işlecini hiç dönmüyor bir `null` değeri. <span data-ttu-id="958cd-1995">A `System.OutOfMemoryException` sonuç dizesi ayırmak yeterli bellek yoksa oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-1995">A `System.OutOfMemoryException` may be thrown if there is not enough memory available to allocate the resulting string.</span></span>

*  <span data-ttu-id="958cd-1996">Temsilci birleşimi.</span><span class="sxs-lookup"><span data-stu-id="958cd-1996">Delegate combination.</span></span> <span data-ttu-id="958cd-1997">Her temsilci türü örtük olarak aşağıdaki önceden tanımlanmış işleç sağlar burada `D` temsilci türü:</span><span class="sxs-lookup"><span data-stu-id="958cd-1997">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator +(D x, D y);
   ```

   <span data-ttu-id="958cd-1998">İkili `+` işleci, temsilci birleşimi her iki işlenen de bazı temsilci türünde olduğunda gerçekleştirir `D`.</span><span class="sxs-lookup"><span data-stu-id="958cd-1998">The binary `+` operator performs delegate combination when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="958cd-1999">(İşlenenleri farklı temsilci türleri varsa, bir bağlama zamanı hatası oluşur.) Birinci işlenenin ise `null`, işlemin sonucunu ikinci işlenenin değeri (Ayrıca olsa bile `null`).</span><span class="sxs-lookup"><span data-stu-id="958cd-1999">(If the operands have different delegate types, a binding-time error occurs.) If the first operand is `null`, the result of the operation is the value of the second operand (even if that is also `null`).</span></span> <span data-ttu-id="958cd-2000">Aksi durumda, ikinci işleneni ise `null`, işlemin sonucunu birinci işlenenin değeridir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2000">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="958cd-2001">Aksi takdirde, işlemin sonucunu yeni bir temsilci örneği, çağrılır, birinci işlenenin çağırır ve ardından ikinci işlenenin çağırır olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2001">Otherwise, the result of the operation is a new delegate instance that, when invoked, invokes the first operand and then invokes the second operand.</span></span> <span data-ttu-id="958cd-2002">Temsilci birleşimi örnekleri için bkz: [çıkarma işleci](expressions.md#subtraction-operator) ve [temsilci çağırma](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="958cd-2002">For examples of delegate combination, see [Subtraction operator](expressions.md#subtraction-operator) and [Delegate invocation](delegates.md#delegate-invocation).</span></span> <span data-ttu-id="958cd-2003">Bu yana `System.Delegate` bir temsilci türü değil `operator` `+` için tanımlı değil.</span><span class="sxs-lookup"><span data-stu-id="958cd-2003">Since `System.Delegate` is not a delegate type, `operator` `+` is not defined for it.</span></span>

### <a name="subtraction-operator"></a><span data-ttu-id="958cd-2004">Çıkarma işleci</span><span class="sxs-lookup"><span data-stu-id="958cd-2004">Subtraction operator</span></span>

<span data-ttu-id="958cd-2005">Formun bir işlem için `x - y`, ikili İşleç aşırı yükleme çözümlemesi ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2005">For an operation of the form `x - y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="958cd-2006">İşlenenler seçilen operatöre parametre türleri dönüştürülür ve sonuç türü işlecinin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="958cd-2006">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="958cd-2007">Aşağıdaki önceden tanımlanmış çıkarma işleçleri listelenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2007">The predefined subtraction operators are listed below.</span></span> <span data-ttu-id="958cd-2008">Tüm çıkarma işleçleri `y` gelen `x`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2008">The operators all subtract `y` from `x`.</span></span>

*  <span data-ttu-id="958cd-2009">Tamsayı çıkarma:</span><span class="sxs-lookup"><span data-stu-id="958cd-2009">Integer subtraction:</span></span>

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   <span data-ttu-id="958cd-2010">İçinde bir `checked` fark sonuç türü aralığının dışında ise bağlam bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2010">In a `checked` context, if the difference is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="958cd-2011">İçinde bir `unchecked` bağlamı, taşan bildirilmez ve tüm önemli sonuç türü yüksek düzeyli bitleri aralığın dışında atılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2011">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="958cd-2012">Kayan nokta çıkarma:</span><span class="sxs-lookup"><span data-stu-id="958cd-2012">Floating-point subtraction:</span></span>

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   <span data-ttu-id="958cd-2013">Fark aritmetik IEEE 754 kurallarına göre hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2013">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="958cd-2014">Sıfır olmayan sınırlı değerler tüm olası eşleştirme birleşimlerini, sıfır sonsuz ve NaN'ler sonuçları aşağıdaki tabloda listelenmektedir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2014">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaNs.</span></span> <span data-ttu-id="958cd-2015">Tabloda `x` ve `y` sınırlı sıfır olmayan değerler ve `z` sonucu `x - y`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2015">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x - y`.</span></span> <span data-ttu-id="958cd-2016">Varsa `x` ve `y` eşit olup olmadığını `z` pozitif sıfır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2016">If `x` and `y` are equal, `z` is positive zero.</span></span> <span data-ttu-id="958cd-2017">Varsa `x - y` hedef türünde temsil etmek için çok büyük `z` aynı işarete sahip bir sonsuzluk olup `x - y`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2017">If `x - y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x - y`.</span></span>

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   | <span data-ttu-id="958cd-2018">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-2018">NaN</span></span>  | <span data-ttu-id="958cd-2019">y</span><span class="sxs-lookup"><span data-stu-id="958cd-2019">y</span></span>    | <span data-ttu-id="958cd-2020">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-2020">+0</span></span>   | <span data-ttu-id="958cd-2021">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-2021">-0</span></span>   | <span data-ttu-id="958cd-2022">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-2022">+inf</span></span> | <span data-ttu-id="958cd-2023">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-2023">-inf</span></span> | <span data-ttu-id="958cd-2024">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-2024">NaN</span></span> | 
   | <span data-ttu-id="958cd-2025">x</span><span class="sxs-lookup"><span data-stu-id="958cd-2025">x</span></span>    | <span data-ttu-id="958cd-2026">z</span><span class="sxs-lookup"><span data-stu-id="958cd-2026">z</span></span>    | <span data-ttu-id="958cd-2027">x</span><span class="sxs-lookup"><span data-stu-id="958cd-2027">x</span></span>    | <span data-ttu-id="958cd-2028">x</span><span class="sxs-lookup"><span data-stu-id="958cd-2028">x</span></span>    | <span data-ttu-id="958cd-2029">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-2029">-inf</span></span> | <span data-ttu-id="958cd-2030">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-2030">+inf</span></span> | <span data-ttu-id="958cd-2031">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-2031">NaN</span></span> | 
   | <span data-ttu-id="958cd-2032">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-2032">+0</span></span>   | <span data-ttu-id="958cd-2033">-y</span><span class="sxs-lookup"><span data-stu-id="958cd-2033">-y</span></span>   | <span data-ttu-id="958cd-2034">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-2034">+0</span></span>   | <span data-ttu-id="958cd-2035">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-2035">+0</span></span>   | <span data-ttu-id="958cd-2036">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-2036">-inf</span></span> | <span data-ttu-id="958cd-2037">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-2037">+inf</span></span> | <span data-ttu-id="958cd-2038">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-2038">NaN</span></span> | 
   | <span data-ttu-id="958cd-2039">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-2039">-0</span></span>   | <span data-ttu-id="958cd-2040">-y</span><span class="sxs-lookup"><span data-stu-id="958cd-2040">-y</span></span>   | <span data-ttu-id="958cd-2041">-0</span><span class="sxs-lookup"><span data-stu-id="958cd-2041">-0</span></span>   | <span data-ttu-id="958cd-2042">+0</span><span class="sxs-lookup"><span data-stu-id="958cd-2042">+0</span></span>   | <span data-ttu-id="958cd-2043">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-2043">-inf</span></span> | <span data-ttu-id="958cd-2044">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-2044">+inf</span></span> | <span data-ttu-id="958cd-2045">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-2045">NaN</span></span> | 
   | <span data-ttu-id="958cd-2046">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-2046">+inf</span></span> | <span data-ttu-id="958cd-2047">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-2047">+inf</span></span> | <span data-ttu-id="958cd-2048">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-2048">+inf</span></span> | <span data-ttu-id="958cd-2049">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-2049">+inf</span></span> | <span data-ttu-id="958cd-2050">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-2050">NaN</span></span>  | <span data-ttu-id="958cd-2051">+ INF</span><span class="sxs-lookup"><span data-stu-id="958cd-2051">+inf</span></span> | <span data-ttu-id="958cd-2052">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-2052">NaN</span></span> | 
   | <span data-ttu-id="958cd-2053">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-2053">-inf</span></span> | <span data-ttu-id="958cd-2054">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-2054">-inf</span></span> | <span data-ttu-id="958cd-2055">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-2055">-inf</span></span> | <span data-ttu-id="958cd-2056">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-2056">-inf</span></span> | <span data-ttu-id="958cd-2057">-INF</span><span class="sxs-lookup"><span data-stu-id="958cd-2057">-inf</span></span> | <span data-ttu-id="958cd-2058">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-2058">NaN</span></span>  | <span data-ttu-id="958cd-2059">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-2059">NaN</span></span> | 
   | <span data-ttu-id="958cd-2060">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-2060">NaN</span></span>  | <span data-ttu-id="958cd-2061">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-2061">NaN</span></span>  | <span data-ttu-id="958cd-2062">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-2062">NaN</span></span>  | <span data-ttu-id="958cd-2063">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-2063">NaN</span></span>  | <span data-ttu-id="958cd-2064">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-2064">NaN</span></span>  | <span data-ttu-id="958cd-2065">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-2065">NaN</span></span>  | <span data-ttu-id="958cd-2066">NaN</span><span class="sxs-lookup"><span data-stu-id="958cd-2066">NaN</span></span> | 

*  <span data-ttu-id="958cd-2067">Ondalık çıkarma:</span><span class="sxs-lookup"><span data-stu-id="958cd-2067">Decimal subtraction:</span></span>

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   <span data-ttu-id="958cd-2068">Sonuçta elde edilen değeri içinde temsil etmek için çok büyük ise `decimal` biçiminde bir `System.OverflowException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2068">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="958cd-2069">Bir yuvarlama önce bu sonucun ölçek büyük ölçekleri iki işlenenden biri ' dir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2069">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="958cd-2070">Ondalık çıkarma tür çıkarma işleci kullanmaya eşdeğerdir `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2070">Decimal subtraction is equivalent to using the subtraction operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="958cd-2071">Sabit listesi çıkarma.</span><span class="sxs-lookup"><span data-stu-id="958cd-2071">Enumeration subtraction.</span></span> <span data-ttu-id="958cd-2072">Her bir numaralandırma türü örtük olarak aşağıdaki önceden tanımlanmış işleç sağlar burada `E` sabit listesi türüdür ve `U` temel alınan türü `E`:</span><span class="sxs-lookup"><span data-stu-id="958cd-2072">Every enumeration type implicitly provides the following predefined operator, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   U operator -(E x, E y);
   ```

   <span data-ttu-id="958cd-2073">Bu işleç, tam olarak değerlendirilir `(U)((U)x - (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2073">This operator is evaluated exactly as `(U)((U)x - (U)y)`.</span></span> <span data-ttu-id="958cd-2074">Diğer bir deyişle, işlecin sıralı değerlerini arasındaki farkı hesaplar `x` ve `y`, ve sonuç türü temel numaralandırma türüdür.</span><span class="sxs-lookup"><span data-stu-id="958cd-2074">In other words, the operator computes the difference between the ordinal values of `x` and `y`, and the type of the result is the underlying type of the enumeration.</span></span>

   ```csharp
   E operator -(E x, U y);
   ```

   <span data-ttu-id="958cd-2075">Bu işleç, tam olarak değerlendirilir `(E)((U)x - y)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2075">This operator is evaluated exactly as `(E)((U)x - y)`.</span></span> <span data-ttu-id="958cd-2076">Diğer bir deyişle, işleci, bir numaralandırma değeri oluşturan bir sabit listesi türünü temel değeri çıkarır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2076">In other words, the operator subtracts a value from the underlying type of the enumeration, yielding a value of the enumeration.</span></span>

*  <span data-ttu-id="958cd-2077">Temizleme temsilci.</span><span class="sxs-lookup"><span data-stu-id="958cd-2077">Delegate removal.</span></span> <span data-ttu-id="958cd-2078">Her temsilci türü örtük olarak aşağıdaki önceden tanımlanmış işleç sağlar burada `D` temsilci türü:</span><span class="sxs-lookup"><span data-stu-id="958cd-2078">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator -(D x, D y);
   ```

   <span data-ttu-id="958cd-2079">İkili `-` işleç, her iki işlenen de bazı temsilci türünde olduğunda temsilci kaldırma gerçekleştirir `D`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2079">The binary `-` operator performs delegate removal when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="958cd-2080">İşlenenleri farklı temsilci türleri varsa, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2080">If the operands have different delegate types, a binding-time error occurs.</span></span> <span data-ttu-id="958cd-2081">Birinci işlenenin ise `null`, işlem sonucu `null`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2081">If the first operand is `null`, the result of the operation is `null`.</span></span> <span data-ttu-id="958cd-2082">Aksi durumda, ikinci işleneni ise `null`, işlemin sonucunu birinci işlenenin değeridir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2082">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="958cd-2083">Aksi takdirde, her iki işlenen de çağırma listeleri temsil eden ([temsilci bildirimi](delegates.md#delegate-declarations)) bir veya daha fazla giriş ve sonuç sahip olan ilk işlenen 's listesi kaldırıldığında ikinci işlenenin ait girişleri ile oluşan yeni bir çağırma listesi Bu ikinci işlenenin 's liste sağlanmıştır, uygun bir bitişik alt liste ilk ait olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2083">Otherwise, both operands represent invocation lists ([Delegate declarations](delegates.md#delegate-declarations)) having one or more entries, and the result is a new invocation list consisting of the first operand's list with the second operand's entries removed from it, provided the second operand's list is a proper contiguous sublist of the first's.</span></span>     <span data-ttu-id="958cd-2084">(Alt liste eşitlik belirlemek için karşılık gelen girişler için temsilci eşitlik işlecini karşılaştırılır ([temsilci eşitlik işleçleri](expressions.md#delegate-equality-operators)).) Aksi halde sol işlenenin sonucudur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2084">(To determine sublist equality, corresponding entries are compared as for the delegate equality operator ([Delegate equality operators](expressions.md#delegate-equality-operators)).) Otherwise, the result is the value of the left operand.</span></span> <span data-ttu-id="958cd-2085">İşlenenleri listeleri hiçbiri işlemde değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2085">Neither of the operands' lists is changed in the process.</span></span> <span data-ttu-id="958cd-2086">İkinci işlenenin 's listesiyle eşleşen birden çok alt listelerin ilk işlenen 's listesinde bitişik girişlerinin bitişik girişlerinin en sağdaki eşleşen alt liste kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2086">If the second operand's list matches multiple sublists of contiguous entries in the first operand's list, the right-most matching sublist of contiguous entries is removed.</span></span> <span data-ttu-id="958cd-2087">Kaldırma işlemi boş bir liste sonuçlanırsa sonucudur `null`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2087">If removal results in an empty list, the result is `null`.</span></span> <span data-ttu-id="958cd-2088">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="958cd-2088">For example:</span></span>

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

## <a name="shift-operators"></a><span data-ttu-id="958cd-2089">Kaydırma işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2089">Shift operators</span></span>

<span data-ttu-id="958cd-2090">`<<` Ve `>>` işleçler bit kaydırma işlemleri gerçekleştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2090">The `<<` and `>>` operators are used to perform bit shifting operations.</span></span>

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

<span data-ttu-id="958cd-2091">İşleci, bir *öğesinin* derleme zamanı türü `dynamic`, ifadeyi dinamik olarak bağlı sonra ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2091">If an operand of a *shift_expression* has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="958cd-2092">Bu durumda derleme zamanı ifadenin türüdür `dynamic`, ve çalışma zamanında derleme zamanı türü bu işlenenler çalışma zamanı türünü kullanarak aşağıda açıklanan çözümleme gerçekleşecek `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2092">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="958cd-2093">Formun bir işlem için `x << count` veya `x >> count`, ikili İşleç aşırı yükleme çözümlemesi ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2093">For an operation of the form `x << count` or `x >> count`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="958cd-2094">İşlenenler seçilen operatöre parametre türleri dönüştürülür ve sonuç türü işlecinin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="958cd-2094">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="958cd-2095">Aşırı yüklenen kaydırma işlecinin bildirirken, birinci işlenenin türü her zaman sınıfın veya yapının işleç bildirimi içeren olmalıdır ve ikinci işlenenin türü her zaman olmalıdır `int`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2095">When declaring an overloaded shift operator, the type of the first operand must always be the class or struct containing the operator declaration, and the type of the second operand must always be `int`.</span></span>

<span data-ttu-id="958cd-2096">Aşağıdaki önceden tanımlanmış kaydırma işleçleri listelenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2096">The predefined shift operators are listed below.</span></span>

*  <span data-ttu-id="958cd-2097">Sola kaydırma:</span><span class="sxs-lookup"><span data-stu-id="958cd-2097">Shift left:</span></span>

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   <span data-ttu-id="958cd-2098">`<<` İşleci kaydırmalar `x` aşağıda açıklandığı gibi sola bit sayısına göre hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2098">The `<<` operator shifts `x` left by a number of bits computed as described below.</span></span>

   <span data-ttu-id="958cd-2099">Sonuç türü aralık dışında yüksek düzeyli bitlerini `x` olan atılan, kalan bitleri sola kaydırılacak ve düşük düzey boş bit konumları sıfır olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2099">The high-order bits outside the range of the result type of `x` are discarded, the remaining bits are shifted left, and the low-order empty bit positions are set to zero.</span></span>

*  <span data-ttu-id="958cd-2100">Sağa kaydırma:</span><span class="sxs-lookup"><span data-stu-id="958cd-2100">Shift right:</span></span>

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   <span data-ttu-id="958cd-2101">`>>` İşleci kaydırmalar `x` aşağıda açıklandığı sağa bit sayısına göre hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2101">The `>>` operator shifts `x` right by a number of bits computed as described below.</span></span>

   <span data-ttu-id="958cd-2102">Zaman `x` türünde `int` veya `long`, alt sıra bitleri `x` olan atılan, kalan bitleri sağa kaydırılacak ve yüksek düzeyli boş bit konumları sıfır if ayarlanır `x` negatif olmayan ve bir halinde `x` negatiftir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2102">When `x` is of type `int` or `long`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero if `x` is non-negative and set to one if `x` is negative.</span></span>

   <span data-ttu-id="958cd-2103">Zaman `x` türünde `uint` veya `ulong`, alt sıra bitleri `x` olan atılan, kalan bitleri sağa kaydırılacak ve yüksek düzeyli boş bit konumları sıfır olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2103">When `x` is of type `uint` or `ulong`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero.</span></span>

<span data-ttu-id="958cd-2104">Önceden tanımlı operatörler için kaydırılacak bit sayısını gibi hesaplanır:</span><span class="sxs-lookup"><span data-stu-id="958cd-2104">For the predefined operators, the number of bits to shift is computed as follows:</span></span>

*  <span data-ttu-id="958cd-2105">Zaman türü `x` olduğu `int` veya `uint`, kaydırma sayısı düşük düzey beş bitleri tarafından verilen `count`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2105">When the type of `x` is `int` or `uint`, the shift count is given by the low-order five bits of `count`.</span></span> <span data-ttu-id="958cd-2106">Kaydırma sayısı alanından başka bir deyişle, hesaplanan `count & 0x1F`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2106">In other words, the shift count is computed from `count & 0x1F`.</span></span>
*  <span data-ttu-id="958cd-2107">Zaman türü `x` olduğu `long` veya `ulong`, kaydırma sayısı düşük düzey altı bitleri tarafından verilen `count`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2107">When the type of `x` is `long` or `ulong`, the shift count is given by the low-order six bits of `count`.</span></span> <span data-ttu-id="958cd-2108">Kaydırma sayısı alanından başka bir deyişle, hesaplanan `count & 0x3F`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2108">In other words, the shift count is computed from `count & 0x3F`.</span></span>

<span data-ttu-id="958cd-2109">Elde edilen kaydırma sayısı sıfır ise, yalnızca dönüş değeri kaydırma işleçleri `x`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2109">If the resulting shift count is zero, the shift operators simply return the value of `x`.</span></span>

<span data-ttu-id="958cd-2110">Kaydırma işleçlerini hiçbir zaman taşıyor neden ve aynı sonuçlar `checked` ve `unchecked` bağlamı.</span><span class="sxs-lookup"><span data-stu-id="958cd-2110">Shift operations never cause overflows and produce the same results in `checked` and `unchecked` contexts.</span></span>

<span data-ttu-id="958cd-2111">Zaman sol işleneni `>>` işleci imzalı bir tamsayı türünde, burada görüntülerle en önemli bite (imza biti) işlenenin değerini yayılan aritmetik kaydırma şu yüksek düzeyli boş bit konumları için işleç gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2111">When the left operand of the `>>` operator is of a signed integral type, the operator performs an arithmetic shift right wherein the value of the most significant bit (the sign bit) of the operand is propagated to the high-order empty bit positions.</span></span> <span data-ttu-id="958cd-2112">Zaman sol işleneni `>>` işleci bir işaretsiz tamsayı türü, mantıksal kaydırma gibi yüksek düzeyli boş bit konumları her zaman ayarlanır sıfıra bir sağ işlecini uygular.</span><span class="sxs-lookup"><span data-stu-id="958cd-2112">When the left operand of the `>>` operator is of an unsigned integral type, the operator performs a logical shift right wherein high-order empty bit positions are always set to zero.</span></span> <span data-ttu-id="958cd-2113">İşlenen türü ortaya çıkan ters işlemi gerçekleştirmek için açık yayınları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2113">To perform the opposite operation of that inferred from the operand type, explicit casts can be used.</span></span> <span data-ttu-id="958cd-2114">Örneğin, varsa `x` türünde bir değişken `int`, işlemi `unchecked((int)((uint)x >> y))` sağında bir mantıksal kaydırma uygular `x`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2114">For example, if `x` is a variable of type `int`, the operation `unchecked((int)((uint)x >> y))` performs a logical shift right of `x`.</span></span>

## <a name="relational-and-type-testing-operators"></a><span data-ttu-id="958cd-2115">İlişkisel ve tür testi işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2115">Relational and type-testing operators</span></span>

<span data-ttu-id="958cd-2116">`==`, `!=`, `<`, `>`, `<=`, `>=`, `is` Ve `as` işleçleri, ilişkisel ve tür testi işleçleri denir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2116">The `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` and `as` operators are called the relational and type-testing operators.</span></span>

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

<span data-ttu-id="958cd-2117">`is` İşleci açıklanan [işleci](expressions.md#the-is-operator) ve `as` işleci açıklanan [işleci olarak](expressions.md#the-as-operator).</span><span class="sxs-lookup"><span data-stu-id="958cd-2117">The `is` operator is described in [The is operator](expressions.md#the-is-operator) and the `as` operator is described in [The as operator](expressions.md#the-as-operator).</span></span>

<span data-ttu-id="958cd-2118">`==`, `!=`, `<`, `>`, `<=` Ve `>=` işleçleri ***Karşılaştırma işleçleri***.</span><span class="sxs-lookup"><span data-stu-id="958cd-2118">The `==`, `!=`, `<`, `>`, `<=` and `>=` operators are ***comparison operators***.</span></span>

<span data-ttu-id="958cd-2119">Bir karşılaştırma işlecinin bir işleneni derleme zamanı türü olup olmadığını `dynamic`, ifadeyi dinamik olarak bağlı sonra ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2119">If an operand of a comparison operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="958cd-2120">Bu durumda derleme zamanı ifadenin türüdür `dynamic`, ve çalışma zamanında derleme zamanı türü bu işlenenler çalışma zamanı türünü kullanarak aşağıda açıklanan çözümleme gerçekleşecek `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2120">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="958cd-2121">Formun bir işlem için `x` *op* `y`burada *op* bir karşılaştırma işleci, aşırı yükleme çözünürlüğü ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2121">For an operation of the form `x` *op* `y`, where *op* is a comparison operator, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="958cd-2122">İşlenenler seçilen operatöre parametre türleri dönüştürülür ve sonuç türü işlecinin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="958cd-2122">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="958cd-2123">Önceden tanımlanmış Karşılaştırma işleçleri aşağıdaki bölümlerde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2123">The predefined comparison operators are described in the following sections.</span></span> <span data-ttu-id="958cd-2124">Tüm önceden tanımlanmış Karşılaştırma işleçleri bir sonuç türü `bool`aşağıdaki tabloda açıklandığı gibi.</span><span class="sxs-lookup"><span data-stu-id="958cd-2124">All predefined comparison operators return a result of type `bool`, as described in the following table.</span></span>


| <span data-ttu-id="958cd-2125">__İşlemi__</span><span class="sxs-lookup"><span data-stu-id="958cd-2125">__Operation__</span></span> | <span data-ttu-id="958cd-2126">__Sonuç__</span><span class="sxs-lookup"><span data-stu-id="958cd-2126">__Result__</span></span>                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | <span data-ttu-id="958cd-2127">`true` varsa `x` eşittir `y`, `false` Aksi takdirde</span><span class="sxs-lookup"><span data-stu-id="958cd-2127">`true` if `x` is equal to `y`, `false` otherwise</span></span>                 | 
| `x != y`      | <span data-ttu-id="958cd-2128">`true` varsa `x` eşit değildir `y`, `false` Aksi takdirde</span><span class="sxs-lookup"><span data-stu-id="958cd-2128">`true` if `x` is not equal to `y`, `false` otherwise</span></span>             | 
| `x < y`       | <span data-ttu-id="958cd-2129">`true` varsa `x` olduğu küçüktür `y`, `false` Aksi takdirde</span><span class="sxs-lookup"><span data-stu-id="958cd-2129">`true` if `x` is less than `y`, `false` otherwise</span></span>                | 
| `x > y`       | <span data-ttu-id="958cd-2130">`true` varsa `x` büyüktür `y`, `false` Aksi takdirde</span><span class="sxs-lookup"><span data-stu-id="958cd-2130">`true` if `x` is greater than `y`, `false` otherwise</span></span>             | 
| `x <= y`      | <span data-ttu-id="958cd-2131">`true` varsa `x` küçüktür veya eşittir `y`, `false` Aksi takdirde</span><span class="sxs-lookup"><span data-stu-id="958cd-2131">`true` if `x` is less than or equal to `y`, `false` otherwise</span></span>    | 
| `x >= y`      | <span data-ttu-id="958cd-2132">`true` varsa `x` büyüktür veya eşittir `y`, `false` Aksi takdirde</span><span class="sxs-lookup"><span data-stu-id="958cd-2132">`true` if `x` is greater than or equal to `y`, `false` otherwise</span></span> | 

### <a name="integer-comparison-operators"></a><span data-ttu-id="958cd-2133">Tamsayı Karşılaştırma işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2133">Integer comparison operators</span></span>

<span data-ttu-id="958cd-2134">Önceden tanımlanmış tamsayı Karşılaştırma işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="958cd-2134">The predefined integer comparison operators are:</span></span>
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

<span data-ttu-id="958cd-2135">Bu işleçlerden her biri sayısal değerleri döndürür ve iki tamsayı işlenenler karşılaştıran bir `bool` belirli ilişki olup olmadığını belirten bir değer `true` veya `false`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2135">Each of these operators compares the numeric values of the two integer operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span>

### <a name="floating-point-comparison-operators"></a><span data-ttu-id="958cd-2136">Kayan nokta Karşılaştırma işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2136">Floating-point comparison operators</span></span>

<span data-ttu-id="958cd-2137">Önceden tanımlanmış bir kayan nokta Karşılaştırma işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="958cd-2137">The predefined floating-point comparison operators are:</span></span>
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

<span data-ttu-id="958cd-2138">İşleçler işlenenlerin standart IEEE 754 kurallarına göre karşılaştırın:</span><span class="sxs-lookup"><span data-stu-id="958cd-2138">The operators compare the operands according to the rules of the IEEE 754 standard:</span></span>

*  <span data-ttu-id="958cd-2139">İki işlenenden NaN ise, sonucudur `false` tüm işleçleri `!=`, sonucu olan `true`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2139">If either operand is NaN, the result is `false` for all operators except `!=`, for which the result is `true`.</span></span> <span data-ttu-id="958cd-2140">Her iki işlenenleri için `x != y` her zaman aynı sonucu üretir `!(x == y)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2140">For any two operands, `x != y` always produces the same result as `!(x == y)`.</span></span> <span data-ttu-id="958cd-2141">Ancak, bir veya iki işlenenin olduğunda, NaN `<`, `>`, `<=`, ve `>=` işleçleri mantıksal olumsuzlama ters işleci,'dekiyle aynı sonucu üretir değil.</span><span class="sxs-lookup"><span data-stu-id="958cd-2141">However, when one or both operands are NaN, the `<`, `>`, `<=`, and `>=` operators do not produce the same results as the logical negation of the opposite operator.</span></span> <span data-ttu-id="958cd-2142">Örneğin, ya da, `x` ve `y` NaN ise `x < y` olduğu `false`, ancak `!(x >= y)` olduğu `true`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2142">For example, if either of `x` and `y` is NaN, then `x < y` is `false`, but `!(x >= y)` is `true`.</span></span>
*  <span data-ttu-id="958cd-2143">İki işlenenin NaN olduğunda, işleçleri değerleri sıralama göre kayan nokta iki işlenenden karşılaştırın.</span><span class="sxs-lookup"><span data-stu-id="958cd-2143">When neither operand is NaN, the operators compare the values of the two floating-point operands with respect to the ordering</span></span>

   ```
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   <span data-ttu-id="958cd-2144">Burada `min` ve `max` belirli kayan nokta biçiminde temsil edilebilir en küçük ve büyük pozitif sınırlı değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2144">where `min` and `max` are the smallest and largest positive finite values that can be represented in the given floating-point format.</span></span> <span data-ttu-id="958cd-2145">Bu sıralama önemli etkileri verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="958cd-2145">Notable effects of this ordering are:</span></span>
   * <span data-ttu-id="958cd-2146">Negatif ve pozitif sıfır eşit olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2146">Negative and positive zeros are considered equal.</span></span>
   * <span data-ttu-id="958cd-2147">Negatif sonsuz küçük diğer tüm değerler daha ancak başka bir negatif sonsuz eşit olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2147">A negative infinity is considered less than all other values, but equal to another negative infinity.</span></span>
   * <span data-ttu-id="958cd-2148">Diğer tüm değerler büyüktür, ancak başka bir pozitif sonsuz eşit bir pozitif sonsuz olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2148">A positive infinity is considered greater than all other values, but equal to another positive infinity.</span></span>

### <a name="decimal-comparison-operators"></a><span data-ttu-id="958cd-2149">Ondalık Karşılaştırma işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2149">Decimal comparison operators</span></span>

<span data-ttu-id="958cd-2150">Önceden tanımlanmış ondalık Karşılaştırma işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="958cd-2150">The predefined decimal comparison operators are:</span></span>
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

<span data-ttu-id="958cd-2151">Bu işleçlerden her biri iki ondalık işlenenler ve döndürür, sayısal değerler karşılaştıran bir `bool` belirli ilişki olup olmadığını belirten bir değer `true` veya `false`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2151">Each of these operators compares the numeric values of the two decimal operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span> <span data-ttu-id="958cd-2152">Her bir ondalık karşılaştırma karşılık gelen ilişkisel veya eşitlik işleci türünde kullanmakla eşdeğerdir `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2152">Each decimal comparison is equivalent to using the corresponding relational or equality operator of type `System.Decimal`.</span></span>

### <a name="boolean-equality-operators"></a><span data-ttu-id="958cd-2153">Boole eşitlik işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2153">Boolean equality operators</span></span>

<span data-ttu-id="958cd-2154">Önceden tanımlanmış Boole eşitlik işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="958cd-2154">The predefined boolean equality operators are:</span></span>
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

<span data-ttu-id="958cd-2155">Sonucu `==` olduğu `true` hem `x` ve `y` olan `true` veya her iki `x` ve `y` olan `false`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2155">The result of `==` is `true` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="958cd-2156">Aksi halde sonuç, `false`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2156">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="958cd-2157">Sonucu `!=` olduğu `false` hem `x` ve `y` olan `true` veya her iki `x` ve `y` olan `false`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2157">The result of `!=` is `false` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="958cd-2158">Aksi halde sonuç, `true`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2158">Otherwise, the result is `true`.</span></span> <span data-ttu-id="958cd-2159">İşlenen türü olduğunda `bool`, `!=` işleci aynı sonucu üretir `^` işleci.</span><span class="sxs-lookup"><span data-stu-id="958cd-2159">When the operands are of type `bool`, the `!=` operator produces the same result as the `^` operator.</span></span>

### <a name="enumeration-comparison-operators"></a><span data-ttu-id="958cd-2160">Sabit listesi Karşılaştırma işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2160">Enumeration comparison operators</span></span>

<span data-ttu-id="958cd-2161">Her bir numaralandırma türü örtük olarak aşağıdaki önceden tanımlanmış Karşılaştırma işleçleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="958cd-2161">Every enumeration type implicitly provides the following predefined comparison operators:</span></span>
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

<span data-ttu-id="958cd-2162">Değerlendirme sonucu `x op y`burada `x` ve `y` ifadeler bir numaralandırma türünün `E` bir temel türü ile `U`, ve `op` Karşılaştırma işleçleri biridir, tamamen aynı. Değerlendirme `((U)x) op ((U)y)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2162">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the comparison operators, is exactly the same as evaluating `((U)x) op ((U)y)`.</span></span> <span data-ttu-id="958cd-2163">Diğer bir deyişle, sabit listesi türü Karşılaştırma işleçleri, yalnızca arka plandaki tamsayı değerlerini iki işlenenden karşılaştırın.</span><span class="sxs-lookup"><span data-stu-id="958cd-2163">In other words, the enumeration type comparison operators simply compare the underlying integral values of the two operands.</span></span>

### <a name="reference-type-equality-operators"></a><span data-ttu-id="958cd-2164">Başvuru türü eşitlik işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2164">Reference type equality operators</span></span>

<span data-ttu-id="958cd-2165">Önceden tanımlı başvuru türü eşitlik işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="958cd-2165">The predefined reference type equality operators are:</span></span>
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

<span data-ttu-id="958cd-2166">İşleçler, karşılaştırma eşitlik ya da olmayan eşitlik için iki başvuru sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="958cd-2166">The operators return the result of comparing the two references for equality or non-equality.</span></span>

<span data-ttu-id="958cd-2167">Önceden tanımlı başvuru türü eşitlik işleçleri türünde işlenenleri kabul beri `object`, bunlar uygun bildirmeyin tüm türleri için geçerli `operator ==` ve `operator !=` üyeleri.</span><span class="sxs-lookup"><span data-stu-id="958cd-2167">Since the predefined reference type equality operators accept operands of type `object`, they apply to all types that do not declare applicable `operator ==` and `operator !=` members.</span></span> <span data-ttu-id="958cd-2168">Buna karşılık, herhangi bir geçerli kullanıcı tanımlı eşitlik işlecini etkili bir şekilde önceden tanımlı başvuru türü eşitlik işleçleri gizleyin.</span><span class="sxs-lookup"><span data-stu-id="958cd-2168">Conversely, any applicable user-defined equality operators effectively hide the predefined reference type equality operators.</span></span>

<span data-ttu-id="958cd-2169">Önceden tanımlı başvuru türü eşitlik işleçleri aşağıdakilerden birini gerektirir:</span><span class="sxs-lookup"><span data-stu-id="958cd-2169">The predefined reference type equality operators require one of the following:</span></span>

*  <span data-ttu-id="958cd-2170">Her iki işlenen de olduğu bilinen bir türde bir değer olan bir *reference_type* veya sabit değer `null`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2170">Both operands are a value of a type known to be a *reference_type* or the literal `null`.</span></span> <span data-ttu-id="958cd-2171">Ayrıca, bir açık bir başvuru dönüştürmesi ([açık başvuru dönüşümleri](conversions.md#explicit-reference-conversions)) diğer işlenen türü için iki işlenenden türünden bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2171">Furthermore, an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from the type of either operand to the type of the other operand.</span></span>
*  <span data-ttu-id="958cd-2172">Tek bir işlenen türünde bir değer olan `T` burada `T` olduğu bir *type_parameter* ve diğer işlenen değişmez değer `null`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2172">One operand is a value of type `T` where `T` is a *type_parameter* and the other operand is the literal `null`.</span></span> <span data-ttu-id="958cd-2173">Ayrıca `T` değer türü kısıtlaması yok.</span><span class="sxs-lookup"><span data-stu-id="958cd-2173">Furthermore `T` does not have the value type constraint.</span></span>

<span data-ttu-id="958cd-2174">Bu koşullardan biri olmadığı sürece true, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2174">Unless one of these conditions are true, a binding-time error occurs.</span></span> <span data-ttu-id="958cd-2175">Bu kurallar önemli etkileri verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="958cd-2175">Notable implications of these rules are:</span></span>

*  <span data-ttu-id="958cd-2176">Bu önceden tanımlı başvuru türü eşitlik işleçleri bağlama zamanında farklı olduğu bilinen iki başvuru karşılaştırmak için kullanılacak bir bağlama zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2176">It is a binding-time error to use the predefined reference type equality operators to compare two references that are known to be different at binding-time.</span></span> <span data-ttu-id="958cd-2177">Örneğin, iki sınıf türleri bağlama zamanı türleridir işlenenden `A` ve `B`ve ne `A` ya da `B` iki işlenenden aynı nesneye başvurmak imkansız sonra diğerinden, türetilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2177">For example, if the binding-time types of the operands are two class types `A` and `B`, and if neither `A` nor `B` derives from the other, then it would be impossible for the two operands to reference the same object.</span></span> <span data-ttu-id="958cd-2178">Bu nedenle, işlemi bir bağlama zamanı hata olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2178">Thus, the operation is considered a binding-time error.</span></span>
*  <span data-ttu-id="958cd-2179">Önceden tanımlı başvuru türü eşitlik işleçleri değer türü işlenen Karşılaştırılacak izin vermez.</span><span class="sxs-lookup"><span data-stu-id="958cd-2179">The predefined reference type equality operators do not permit value type operands to be compared.</span></span> <span data-ttu-id="958cd-2180">Bir yapı türü kendi eşitlik işleçleri bildirir sürece, bu nedenle, bu yapı türünün değerlerini karşılaştırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2180">Therefore, unless a struct type declares its own equality operators, it is not possible to compare values of that struct type.</span></span>
*  <span data-ttu-id="958cd-2181">Önceden tanımlı başvuru türü eşitlik işleçleri asla kutulama işlemleri, işlenenleri için oluşmasına neden.</span><span class="sxs-lookup"><span data-stu-id="958cd-2181">The predefined reference type equality operators never cause boxing operations to occur for their operands.</span></span> <span data-ttu-id="958cd-2182">Bu yana yeni ayrılan paketlenmiş örneklerine başvuruları mutlaka diğer tüm başvurularından farklı gibi kutulama işlemleri gerçekleştirmek için etmezdi.</span><span class="sxs-lookup"><span data-stu-id="958cd-2182">It would be meaningless to perform such boxing operations, since references to the newly allocated boxed instances would necessarily differ from all other references.</span></span>
*  <span data-ttu-id="958cd-2183">Bir tür parametresi türünde bir işlenen, `T` karşılaştırılır `null`ve çalışma zamanı türü `T` bir değer türüdür karşılaştırmanın sonucudur `false`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2183">If an operand of a type parameter type `T` is compared to `null`, and the run-time type of `T` is a value type, the result of the comparison is `false`.</span></span>

<span data-ttu-id="958cd-2184">Aşağıdaki örnek, sınırlandırılmamış türü parametre türünde bir bağımsız değişken olup olmadığını denetler `null`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2184">The following example checks whether an argument of an unconstrained type parameter type is `null`.</span></span>
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

<span data-ttu-id="958cd-2185">`x == null` Yapısı olsa bile izin `T` bir değer türü temsil edebilir ve sonucu yalnızca olarak tanımlanır `false` olduğunda `T` bir değer türüdür.</span><span class="sxs-lookup"><span data-stu-id="958cd-2185">The `x == null` construct is permitted even though `T` could represent a value type, and the result is simply defined to be `false` when `T` is a value type.</span></span>

<span data-ttu-id="958cd-2186">Formun bir işlem için `x == y` veya `x != y`, her varsa `operator ==` veya `operator !=` yoksa, İşleç aşırı yükleme çözünürlüğü ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) kuralları, seçer önceden tanımlı başvuru türü eşitlik işlecini yerine işleci.</span><span class="sxs-lookup"><span data-stu-id="958cd-2186">For an operation of the form `x == y` or `x != y`, if any applicable `operator ==` or `operator !=` exists, the operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) rules will select that operator instead of the predefined reference type equality operator.</span></span> <span data-ttu-id="958cd-2187">Ancak, her zaman önceden tanımlı başvuru türü eşitlik işlecini birini veya her ikisini işlenenler türüne açıkça atayarak seçmek mümkündür `object`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2187">However, it is always possible to select the predefined reference type equality operator by explicitly casting one or both of the operands to type `object`.</span></span> <span data-ttu-id="958cd-2188">Örnek</span><span class="sxs-lookup"><span data-stu-id="958cd-2188">The example</span></span>
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
<span data-ttu-id="958cd-2189">çıktıyı üretir</span><span class="sxs-lookup"><span data-stu-id="958cd-2189">produces the output</span></span>
```
True
False
False
False
```

<span data-ttu-id="958cd-2190">`s` Ve `t` değişkenleri başvuran iki farklı `string` aynı karakterleri içeren örnekleri.</span><span class="sxs-lookup"><span data-stu-id="958cd-2190">The `s` and `t` variables refer to two distinct `string` instances containing the same characters.</span></span> <span data-ttu-id="958cd-2191">İlk karşılaştırma çıkarır `True` çünkü önceden tanımlı dize eşitlik işlecini ([dize eşitlik işleçleri](expressions.md#string-equality-operators)) her iki işlenen türünde olduğunda seçili `string`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2191">The first comparison outputs `True` because the predefined string equality operator ([String equality operators](expressions.md#string-equality-operators)) is selected when both operands are of type `string`.</span></span> <span data-ttu-id="958cd-2192">Tüm diğer karşılaştırmalar çıkış `False` önceden tanımlı başvuru türü eşitlik işlecini bir veya iki işlenenden türünde olduğunda seçilmediğinden `object`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2192">The remaining comparisons all output `False` because the predefined reference type equality operator is selected when one or both of the operands are of type `object`.</span></span>

<span data-ttu-id="958cd-2193">Yukarıdaki tekniği değer türleri için anlamlı olmadığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="958cd-2193">Note that the above technique is not meaningful for value types.</span></span> <span data-ttu-id="958cd-2194">Örnek</span><span class="sxs-lookup"><span data-stu-id="958cd-2194">The example</span></span>
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
<span data-ttu-id="958cd-2195">çıkaran `False` yayınları iki ayrı örneklerini başvuruları oluşturduğundan Kutulu `int` değerleri.</span><span class="sxs-lookup"><span data-stu-id="958cd-2195">outputs `False` because the casts create references to two separate instances of boxed `int` values.</span></span>

### <a name="string-equality-operators"></a><span data-ttu-id="958cd-2196">Dize eşitlik işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2196">String equality operators</span></span>

<span data-ttu-id="958cd-2197">Önceden tanımlı dize eşitlik işleçleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="958cd-2197">The predefined string equality operators are:</span></span>
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

<span data-ttu-id="958cd-2198">İki `string` aşağıdakilerden biri doğruysa, değerleri eşit değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="958cd-2198">Two `string` values are considered equal when one of the following is true:</span></span>

*  <span data-ttu-id="958cd-2199">Her iki değerler `null`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2199">Both values are `null`.</span></span>
*  <span data-ttu-id="958cd-2200">Her iki aynı uzunluk ve aynı karakterlerin her karakter konumunda sahip dize örnekleri null olmayan başvurular değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2200">Both values are non-null references to string instances that have identical lengths and identical characters in each character position.</span></span>

<span data-ttu-id="958cd-2201">Dize başvurular yerine dize değerleri dize eşitlik işleçleri karşılaştırın.</span><span class="sxs-lookup"><span data-stu-id="958cd-2201">The string equality operators compare string values rather than string references.</span></span> <span data-ttu-id="958cd-2202">İki ayrı bir dize örneği tam olarak aynı karakter dizisi içerdiğinde, dizelerin değerlerin eşit olup, ancak başvuruları farklıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2202">When two separate string instances contain the exact same sequence of characters, the values of the strings are equal, but the references are different.</span></span> <span data-ttu-id="958cd-2203">Bölümünde anlatıldığı gibi [başvuru türü eşitlik işleçleri](expressions.md#reference-type-equality-operators), başvuru türü eşitlik işleçleri dize başvurular yerine dize değerleri karşılaştırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2203">As described in [Reference type equality operators](expressions.md#reference-type-equality-operators), the reference type equality operators can be used to compare string references instead of string values.</span></span>

### <a name="delegate-equality-operators"></a><span data-ttu-id="958cd-2204">Temsilci eşitlik işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2204">Delegate equality operators</span></span>

<span data-ttu-id="958cd-2205">Her temsilci türü örtük olarak aşağıdaki önceden tanımlanmış Karşılaştırma işleçleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="958cd-2205">Every delegate type implicitly provides the following predefined comparison operators:</span></span>

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

<span data-ttu-id="958cd-2206">İki temsilci örneklerini şu şekilde eşit olarak kabul edilir:</span><span class="sxs-lookup"><span data-stu-id="958cd-2206">Two delegate instances are considered equal as follows:</span></span>

*  <span data-ttu-id="958cd-2207">Temsilci örneklerini biri geçerli olduğunda `null`, her ikisi de ve yalnızca, eşit oldukları `null`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2207">If either of the delegate instances is `null`, they are equal if and only if both are `null`.</span></span>
*  <span data-ttu-id="958cd-2208">Hiçbir zaman eşit temsilcileri farklı çalışma zamanı türü varsa.</span><span class="sxs-lookup"><span data-stu-id="958cd-2208">If the delegates have different run-time type they are never equal.</span></span>
*  <span data-ttu-id="958cd-2209">Her iki temsilci örneklerini çağrı listesine sahip olup ([temsilci bildirimi](delegates.md#delegate-declarations)), çağırma listelerini aynı uzunlukta olan ve her birinin çağırma listesindeki girdinin (yukarıda tanımlandığı şekilde) eşittir ve yalnızca, bu örneğinin eşit olup karşılık gelen bir giriş için sırayla diğer tarafın çağırma listesi.</span><span class="sxs-lookup"><span data-stu-id="958cd-2209">If both of the delegate instances have an invocation list ([Delegate declarations](delegates.md#delegate-declarations)), those instances are equal if and only if their invocation lists are the same length, and each entry in one's invocation list is equal (as defined below) to the corresponding entry, in order, in the other's invocation list.</span></span>

<span data-ttu-id="958cd-2210">Aşağıdaki kurallar çağırma Liste girişlerini eşitliğine yöneten:</span><span class="sxs-lookup"><span data-stu-id="958cd-2210">The following rules govern the equality of invocation list entries:</span></span>

*  <span data-ttu-id="958cd-2211">İki çağırma Liste girişlerini hem aynı statik başvuruyorsa yöntemi sonra girişleri eşit olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2211">If two invocation list entries both refer to the same static method then the entries are equal.</span></span>
*  <span data-ttu-id="958cd-2212">İki çağırma Liste girişlerini hem aynı hedef nesne üzerinde aynı statik olmayan yöntemi (referans eşitlik operatörleri tarafından tanımlandığı gibi) başvurursanız girişleri eşit olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2212">If two invocation list entries both refer to the same non-static method on the same target object (as defined by the reference equality operators) then the entries are equal.</span></span>
*  <span data-ttu-id="958cd-2213">Anlamsal olarak özdeş bir değerlendirmesinden gelen çağırma Liste girişlerini üretilen *anonymous_method_expression*s veya *lambda_expression*s yakalanan dış değişken aynı (büyük olasılıkla boş) kümesi örnekleri izin verilir (ancak gerekli değildir) eşit.</span><span class="sxs-lookup"><span data-stu-id="958cd-2213">Invocation list entries produced from evaluation of semantically identical *anonymous_method_expression*s or *lambda_expression*s with the same (possibly empty) set of captured outer variable instances are permitted (but not required) to be equal.</span></span>

### <a name="equality-operators-and-null"></a><span data-ttu-id="958cd-2214">Eşitlik işleçleri ve null</span><span class="sxs-lookup"><span data-stu-id="958cd-2214">Equality operators and null</span></span>

<span data-ttu-id="958cd-2215">`==` Ve `!=` işleçleri, tek bir işlenen boş değer atanabilir bir tür ve diğer olması için bir değer olmasını izin `null` işlemi için (içinde unlifted veya form yükseltilmiş) önceden tanımlı veya kullanıcı tanımlı hiçbir işleç bulunuyor olsa bile değişmez.</span><span class="sxs-lookup"><span data-stu-id="958cd-2215">The `==` and `!=` operators permit one operand to be a value of a nullable type and the other to be the `null` literal, even if no predefined or user-defined operator (in unlifted or lifted form) exists for the operation.</span></span>

<span data-ttu-id="958cd-2216">Bir form için bir işlem</span><span class="sxs-lookup"><span data-stu-id="958cd-2216">For an operation of one of the forms</span></span>
```csharp
x == null
null == x
x != null
null != x
```
<span data-ttu-id="958cd-2217">Burada `x` işleci aşırı yükleme çözümlemesi bir ifade, null yapılabilir bir tür ise ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) başarısız sonucu geçerli bir işleç bulmak için gelen hesaplanan yerine `HasValue` özelliği `x`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2217">where `x` is an expression of a nullable type, if operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) fails to find an applicable operator, the result is instead computed from the `HasValue` property of `x`.</span></span> <span data-ttu-id="958cd-2218">Özellikle, ilk iki forms çevrilir `!x.HasValue`, ve son iki forms çevrilmiş içine `x.HasValue`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2218">Specifically, the first two forms are translated into `!x.HasValue`, and last two forms are translated into `x.HasValue`.</span></span>

### <a name="the-is-operator"></a><span data-ttu-id="958cd-2219">Is işleci</span><span class="sxs-lookup"><span data-stu-id="958cd-2219">The is operator</span></span>

<span data-ttu-id="958cd-2220">`is` İşleci, dinamik olarak bir nesnenin çalışma zamanı türü belirli bir tür ile uyumlu olup olmadığını denetlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2220">The `is` operator is used to dynamically check if the run-time type of an object is compatible with a given type.</span></span> <span data-ttu-id="958cd-2221">İşlemin sonucu `E is T`burada `E` ifade ve `T` bir tür, bir Boole değeri belirten değer olup olmadığını `E` başarıyla türüne dönüştürülebilir `T` bir başvuru dönüştürmesi, bir paketleme tarafından dönüştürme veya bir kutudan çıkarma dönüştürme.</span><span class="sxs-lookup"><span data-stu-id="958cd-2221">The result of the operation `E is T`, where `E` is an expression and `T` is a type, is a boolean value indicating whether `E` can successfully be converted to type `T` by a reference conversion, a boxing conversion, or an unboxing conversion.</span></span> <span data-ttu-id="958cd-2222">Tüm tip parametreleri için tür bağımsız değişkeni yapıştırıyorsanız sonra işlemi aşağıdaki gibi değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="958cd-2222">The operation is evaluated as follows, after type arguments have been substituted for all type parameters:</span></span>

*  <span data-ttu-id="958cd-2223">Varsa `E` bir derleme zamanı hatası oluşur anonim bir işlev ise</span><span class="sxs-lookup"><span data-stu-id="958cd-2223">If `E` is an anonymous function, a compile-time error occurs</span></span>
*  <span data-ttu-id="958cd-2224">Varsa `E` bir yöntem grubu olduğundan veya `null` biri değişmez değer türü `E` bir başvuru türü veya boş değer atanabilir bir tür ve değerini `E` olan null, sonucun false olduğu.</span><span class="sxs-lookup"><span data-stu-id="958cd-2224">If `E` is a method group or the `null` literal, of if the type of `E` is a reference type or a nullable type and the value of `E` is null, the result is false.</span></span>
*  <span data-ttu-id="958cd-2225">Aksi takdirde, izin `D` dinamik türünü temsil eden `E` gibi:</span><span class="sxs-lookup"><span data-stu-id="958cd-2225">Otherwise, let `D` represent the dynamic type of `E` as follows:</span></span>
   * <span data-ttu-id="958cd-2226">Varsa türünü `E` bir başvuru türüdür `D` örneği başvuruya göre çalışma zamanı türü `E`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2226">If the type of `E` is a reference type, `D` is the run-time type of the instance reference by `E`.</span></span>
   * <span data-ttu-id="958cd-2227">Varsa türünü `E` boş değer atanabilir bir tür `D` temel alınan türü, boş değer atanabilir bir tür.</span><span class="sxs-lookup"><span data-stu-id="958cd-2227">If the type of `E` is a nullable type, `D` is the underlying type of that nullable type.</span></span>
   * <span data-ttu-id="958cd-2228">Varsa türünü `E` bir NULL olmayan değer türü `D` türü `E`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2228">If the type of `E` is a non-nullable value type, `D` is the type of `E`.</span></span>
*  <span data-ttu-id="958cd-2229">İşlemin sonucu bağımlı `D` ve `T` şu şekilde:</span><span class="sxs-lookup"><span data-stu-id="958cd-2229">The result of the operation depends on `D` and `T` as follows:</span></span>
   * <span data-ttu-id="958cd-2230">Varsa `T` bir başvuru türü ise sonucun true olduğu, `D` ve `T` aynı türde olan `D` bir başvuru türü ve bir örtük bir başvuru dönüştürme `D` için `T` var, veya `D` bir değer türü ve kutulama dönüştürme `D` için `T` bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2230">If `T` is a reference type, the result is true if `D` and `T` are the same type, if `D` is a reference type and an implicit reference conversion from `D` to `T` exists, or if `D` is a value type and a boxing conversion from `D` to `T` exists.</span></span>
   * <span data-ttu-id="958cd-2231">Varsa `T` boş değer atanabilir bir tür olduğunda sonucun true olduğu, `D` temel alınan türü `T`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2231">If `T` is a nullable type, the result is true if `D` is the underlying type of `T`.</span></span>
   * <span data-ttu-id="958cd-2232">Varsa `T` bir NULL olmayan değer türü sonucun true olduğu, `D` ve `T` aynı türdeyse.</span><span class="sxs-lookup"><span data-stu-id="958cd-2232">If `T` is a non-nullable value type, the result is true if `D` and `T` are the same type.</span></span>
   * <span data-ttu-id="958cd-2233">Aksi takdirde sonuç yanlış olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2233">Otherwise, the result is false.</span></span>

<span data-ttu-id="958cd-2234">Kullanıcı tanımlı dönüştürmeler dikkate alınmaz tarafından olduğunu unutmayın `is` işleci.</span><span class="sxs-lookup"><span data-stu-id="958cd-2234">Note that user defined conversions, are not considered by the `is` operator.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="958cd-2235">AS işleci</span><span class="sxs-lookup"><span data-stu-id="958cd-2235">The as operator</span></span>

<span data-ttu-id="958cd-2236">`as` İşleci bir verilen başvuru türüyle veya null yapılabilir bir tür için bir değer açıkça dönüştürmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2236">The `as` operator is used to explicitly convert a value to a given reference type or nullable type.</span></span> <span data-ttu-id="958cd-2237">Cast ifadesi aksine ([Cast ifadeleri](expressions.md#cast-expressions)), `as` işleci hiçbir zaman bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2237">Unlike a cast expression ([Cast expressions](expressions.md#cast-expressions)), the `as` operator never throws an exception.</span></span> <span data-ttu-id="958cd-2238">Bunun yerine, belirtilen dönüştürme mümkün değilse, sonuç değeri olan `null`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2238">Instead, if the indicated conversion is not possible, the resulting value is `null`.</span></span>

<span data-ttu-id="958cd-2239">Formun işleminde `E as T`, `E` bir ifade olmalı ve `T` bir başvuru türü bir başvuru türüyle veya null yapılabilir bir tür olarak bilinen bir tür parametresi olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2239">In an operation of the form `E as T`, `E` must be an expression and `T` must be a reference type, a type parameter known to be a reference type, or a nullable type.</span></span> <span data-ttu-id="958cd-2240">Ayrıca, aşağıdakilerden en az biri true ya da aksi takdirde bir derleme zamanı hatası oluşur:</span><span class="sxs-lookup"><span data-stu-id="958cd-2240">Furthermore, at least one of the following must be true, or otherwise a compile-time error occurs:</span></span>

*  <span data-ttu-id="958cd-2241">Bir kimlik ([kimlik dönüştürme](conversions.md#identity-conversion)), örtük boş değer atanabilir ([boş değer atanabilir örtük dönüştürmelerin](conversions.md#implicit-nullable-conversions)), örtük bir başvuru ([örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions)), (kutulama[ Kutulama dönüştürmeler](conversions.md#boxing-conversions)), açık boş değer atanabilir ([açık boş değer atanabilir dönüştürmeler](conversions.md#explicit-nullable-conversions)), açık bir başvuru ([açık başvuru dönüşümleri](conversions.md#explicit-reference-conversions)), veya kutudan çıkarma ([Kutudan çıkarma dönüştürme](conversions.md#unboxing-conversions)) dönüştürme var gelen `E` için `T`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2241">An identity ([Identity conversion](conversions.md#identity-conversion)), implicit nullable ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)), implicit reference ([Implicit reference conversions](conversions.md#implicit-reference-conversions)), boxing ([Boxing conversions](conversions.md#boxing-conversions)), explicit nullable ([Explicit nullable conversions](conversions.md#explicit-nullable-conversions)), explicit reference ([Explicit reference conversions](conversions.md#explicit-reference-conversions)), or unboxing ([Unboxing conversions](conversions.md#unboxing-conversions)) conversion exists from `E` to `T`.</span></span>
*  <span data-ttu-id="958cd-2242">Türünü `E` veya `T` bir açık türdür.</span><span class="sxs-lookup"><span data-stu-id="958cd-2242">The type of `E` or `T` is an open type.</span></span>
*  <span data-ttu-id="958cd-2243">`E` olan `null` değişmez.</span><span class="sxs-lookup"><span data-stu-id="958cd-2243">`E` is the `null` literal.</span></span>

<span data-ttu-id="958cd-2244">Derleme zamanı türü `E` değil `dynamic`, işlemi `E as T` aynı sonucu üretir</span><span class="sxs-lookup"><span data-stu-id="958cd-2244">If the compile-time type of `E` is not `dynamic`, the operation `E as T` produces the same result as</span></span>
```csharp
E is T ? (T)(E) : (T)null
```
<span data-ttu-id="958cd-2245">dışında `E` yalnızca bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2245">except that `E` is only evaluated once.</span></span> <span data-ttu-id="958cd-2246">Derleyici, en iyi duruma getirme beklenebilir `E as T` yukarıdaki genişletme tarafından kapsanan iki dinamik tür denetimlerini aksine en fazla bir dinamik tür denetimi gerçekleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="958cd-2246">The compiler can be expected to optimize `E as T` to perform at most one dynamic type check as opposed to the two dynamic type checks implied by the expansion above.</span></span>

<span data-ttu-id="958cd-2247">Derleme zamanı türü `E` olduğu `dynamic`, atama işleci aksine `as` işleci değil dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2247">If the compile-time type of `E` is `dynamic`, unlike the cast operator the `as` operator is not dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="958cd-2248">Bu nedenle bu durumda genişletme şöyledir:</span><span class="sxs-lookup"><span data-stu-id="958cd-2248">Therefore the expansion in this case is:</span></span>
```csharp
E is T ? (T)(object)(E) : (T)null
```

<span data-ttu-id="958cd-2249">Kullanıcı tanımlı dönüştürmeler gibi bazı dönüştürmeler mümkün olmadığını unutmayın `as` işleci ve bunun yerine cast ifadeleri kullanarak gerçekleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2249">Note that some conversions, such as user defined conversions, are not possible with the `as` operator and should instead be performed using cast expressions.</span></span>

<span data-ttu-id="958cd-2250">Örnekte</span><span class="sxs-lookup"><span data-stu-id="958cd-2250">In the example</span></span>
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
<span data-ttu-id="958cd-2251">tür parametresi `T` , `G` bir başvuru türü olması için sınıf kısıtlaması içerdiğinden bilinir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2251">the type parameter `T` of `G` is known to be a reference type, because it has the class constraint.</span></span> <span data-ttu-id="958cd-2252">Tür parametresi `U` , `H` kullanımını değil ancak; bu nedenle `as` işlecinde `H` izin verilmiyor.</span><span class="sxs-lookup"><span data-stu-id="958cd-2252">The type parameter `U` of `H` is not however; hence the use of the `as` operator in `H` is disallowed.</span></span>

## <a name="logical-operators"></a><span data-ttu-id="958cd-2253">Mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="958cd-2253">Logical operators</span></span>

<span data-ttu-id="958cd-2254">`&`, `^`, Ve `|` işleçleri mantıksal işleçler çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2254">The `&`, `^`, and `|` operators are called the logical operators.</span></span>

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

<span data-ttu-id="958cd-2255">Bir mantıksal işlecinin bir işleneni derleme zamanı türü olup olmadığını `dynamic`, ifadeyi dinamik olarak bağlı sonra ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2255">If an operand of a logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="958cd-2256">Bu durumda derleme zamanı ifadenin türüdür `dynamic`, ve çalışma zamanında derleme zamanı türü bu işlenenler çalışma zamanı türünü kullanarak aşağıda açıklanan çözümleme gerçekleşecek `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2256">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="958cd-2257">Formun bir işlem için `x op y`burada `op` mantıksal işleçler, aşırı yükleme çözünürlüğü biridir ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2257">For an operation of the form `x op y`, where `op` is one of the logical operators, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="958cd-2258">İşlenenler seçilen operatöre parametre türleri dönüştürülür ve sonuç türü işlecinin dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="958cd-2258">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="958cd-2259">Önceden tanımlanmış mantıksal işleçleri aşağıdaki bölümlerde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2259">The predefined logical operators are described in the following sections.</span></span>

### <a name="integer-logical-operators"></a><span data-ttu-id="958cd-2260">Tamsayı mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="958cd-2260">Integer logical operators</span></span>

<span data-ttu-id="958cd-2261">Önceden tanımlanmış tamsayı mantıksal işleçler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="958cd-2261">The predefined integer logical operators are:</span></span>
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

<span data-ttu-id="958cd-2262">`&` İşleci hesaplar. bit düzeyinde mantıksal `AND` iki işlenenden `|` işleci hesaplar. bit düzeyinde mantıksal `OR` iki işlenenden ve `^` işleci mantıksal bit düzeyinde özel hesaplar `OR` iki işlenenden.</span><span class="sxs-lookup"><span data-stu-id="958cd-2262">The `&` operator computes the bitwise logical `AND` of the two operands, the `|` operator computes the bitwise logical `OR` of the two operands, and the `^` operator computes the bitwise logical exclusive `OR` of the two operands.</span></span> <span data-ttu-id="958cd-2263">Bu işlemlerini hiçbir taşıyor mümkündür.</span><span class="sxs-lookup"><span data-stu-id="958cd-2263">No overflows are possible from these operations.</span></span>

### <a name="enumeration-logical-operators"></a><span data-ttu-id="958cd-2264">Mantıksal işleçler numaralandırması</span><span class="sxs-lookup"><span data-stu-id="958cd-2264">Enumeration logical operators</span></span>

<span data-ttu-id="958cd-2265">Her bir numaralandırma türü `E` örtük olarak aşağıdaki önceden tanımlanmış mantıksal işleçleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="958cd-2265">Every enumeration type `E` implicitly provides the following predefined logical operators:</span></span>

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

<span data-ttu-id="958cd-2266">Değerlendirme sonucu `x op y`burada `x` ve `y` ifadeler bir numaralandırma türünün `E` bir temel türü ile `U`, ve `op` mantıksal işleçler biridir, tamamen aynı. Değerlendirme `(E)((U)x op (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2266">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the logical operators, is exactly the same as evaluating `(E)((U)x op (U)y)`.</span></span> <span data-ttu-id="958cd-2267">Diğer bir deyişle, sabit listesi türü mantıksal işleçler yalnızca iki işlenenden temel alınan türü üzerinde mantıksal işlem gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="958cd-2267">In other words, the enumeration type logical operators simply perform the logical operation on the underlying type of the two operands.</span></span>

### <a name="boolean-logical-operators"></a><span data-ttu-id="958cd-2268">Boolean mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="958cd-2268">Boolean logical operators</span></span>

<span data-ttu-id="958cd-2269">Önceden tanımlanmış boolean mantıksal işleçler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="958cd-2269">The predefined boolean logical operators are:</span></span>
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

<span data-ttu-id="958cd-2270">Sonucu `x & y` olduğu `true` hem `x` ve `y` olan `true`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2270">The result of `x & y` is `true` if both `x` and `y` are `true`.</span></span> <span data-ttu-id="958cd-2271">Aksi halde sonuç, `false`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2271">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="958cd-2272">Sonucu `x | y` olduğu `true` ya da `x` veya `y` olduğu `true`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2272">The result of `x | y` is `true` if either `x` or `y` is `true`.</span></span> <span data-ttu-id="958cd-2273">Aksi halde sonuç, `false`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2273">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="958cd-2274">Sonucu `x ^ y` olduğu `true` varsa `x` olduğu `true` ve `y` olduğu `false`, veya `x` olan `false` ve `y` olan `true`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2274">The result of `x ^ y` is `true` if `x` is `true` and `y` is `false`, or `x` is `false` and `y` is `true`.</span></span> <span data-ttu-id="958cd-2275">Aksi halde sonuç, `false`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2275">Otherwise, the result is `false`.</span></span> <span data-ttu-id="958cd-2276">İşlenen türü olduğunda `bool`, `^` işleci aynı sonucu hesaplar `!=` işleci.</span><span class="sxs-lookup"><span data-stu-id="958cd-2276">When the operands are of type `bool`, the `^` operator computes the same result as the `!=` operator.</span></span>

### <a name="nullable-boolean-logical-operators"></a><span data-ttu-id="958cd-2277">Boş değer atanabilir boolean mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="958cd-2277">Nullable boolean logical operators</span></span>

<span data-ttu-id="958cd-2278">Boş değer atanabilir bir boolean türü `bool?` üç değerleri temsil edebilen `true`, `false`, ve `null`ve SQL boolean ifadeler için kullanılan üç değerli türü kavramsal olarak benzer.</span><span class="sxs-lookup"><span data-stu-id="958cd-2278">The nullable boolean type `bool?` can represent three values, `true`, `false`, and `null`, and is conceptually similar to the three-valued type used for boolean expressions in SQL.</span></span> <span data-ttu-id="958cd-2279">Tarafından üretilen sonuçlar emin olmak için `&` ve `|` işleçleri `bool?` işlenenler SQL'in üç değerli mantığı ile tutarlı, aşağıdaki önceden tanımlı operatörler sağlanır:</span><span class="sxs-lookup"><span data-stu-id="958cd-2279">To ensure that the results produced by the `&` and `|` operators for `bool?` operands are consistent with SQL's three-valued logic, the following predefined operators are provided:</span></span>

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

<span data-ttu-id="958cd-2280">Bu işleçler için tüm değerleri birleşimlerini tarafından üretilen sonuçları aşağıdaki tabloda listelenmektedir `true`, `false`, ve `null`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2280">The following table lists the results produced by these operators for all combinations of the values `true`, `false`, and `null`.</span></span>

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

## <a name="conditional-logical-operators"></a><span data-ttu-id="958cd-2281">Koşullu mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="958cd-2281">Conditional logical operators</span></span>

<span data-ttu-id="958cd-2282">`&&` Ve `||` işleçleri koşullu mantıksal işleçler çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2282">The `&&` and `||` operators are called the conditional logical operators.</span></span> <span data-ttu-id="958cd-2283">"Short-circuiting" mantıksal işleçler de çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2283">They are also called the "short-circuiting" logical operators.</span></span>

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

<span data-ttu-id="958cd-2284">`&&` Ve `||` işleçler şunlardır: koşullu sürümleri `&` ve `|` işleçleri:</span><span class="sxs-lookup"><span data-stu-id="958cd-2284">The `&&` and `||` operators are conditional versions of the `&` and `|` operators:</span></span>

*  <span data-ttu-id="958cd-2285">İşlemi `x && y` işlemine karşılık gelir `x & y`dışında `y` yalnızca değerlendirilir `x` değil `false`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2285">The operation `x && y` corresponds to the operation `x & y`, except that `y` is evaluated only if `x` is not `false`.</span></span>
*  <span data-ttu-id="958cd-2286">İşlemi `x || y` işlemine karşılık gelir `x | y`dışında `y` yalnızca değerlendirilir `x` değil `true`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2286">The operation `x || y` corresponds to the operation `x | y`, except that `y` is evaluated only if `x` is not `true`.</span></span>

<span data-ttu-id="958cd-2287">Koşullu bir mantıksal işlecinin bir işleneni derleme zamanı türü olup olmadığını `dynamic`, ifadeyi dinamik olarak bağlı sonra ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2287">If an operand of a conditional logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="958cd-2288">Bu durumda derleme zamanı ifadenin türüdür `dynamic`, ve çalışma zamanında derleme zamanı türü bu işlenenler çalışma zamanı türünü kullanarak aşağıda açıklanan çözümleme gerçekleşecek `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2288">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="958cd-2289">Form işlemini `x && y` veya `x || y` aşırı yükleme çözünürlüğü uygulama tarafından işlenen ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) işlemi olarak yazılmışsa `x & y` veya `x | y`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2289">An operation of the form `x && y` or `x || y` is processed by applying overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x & y` or `x | y`.</span></span> <span data-ttu-id="958cd-2290">Ardından,</span><span class="sxs-lookup"><span data-stu-id="958cd-2290">Then,</span></span>

*  <span data-ttu-id="958cd-2291">Tek bir en iyi işleci bulmak aşırı yükleme çözümlemesi başarısız olursa veya aşırı yükleme çözünürlüğü önceden tanımlanmış tamsayı mantıksal işleçler seçerse, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2291">If overload resolution fails to find a single best operator, or if overload resolution selects one of the predefined integer logical operators, a binding-time error occurs.</span></span>
*  <span data-ttu-id="958cd-2292">Aksi takdirde, seçilen operatöre önceden tanımlanmış boolean mantıksal işleçler ise ([Boolean mantıksal işleçler](expressions.md#boolean-logical-operators)) veya boş değer atanabilir boolean mantıksal işleçler ([null, boolean mantıksal işleçler](expressions.md#nullable-boolean-logical-operators)), işlem bölümünde anlatıldığı gibi işlenir [Boole koşullu mantıksal işleçleri](expressions.md#boolean-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="958cd-2292">Otherwise, if the selected operator is one of the predefined boolean logical operators ([Boolean logical operators](expressions.md#boolean-logical-operators)) or nullable boolean logical operators ([Nullable boolean logical operators](expressions.md#nullable-boolean-logical-operators)), the operation is processed as described in [Boolean conditional logical operators](expressions.md#boolean-conditional-logical-operators).</span></span>
*  <span data-ttu-id="958cd-2293">Aksi takdirde, seçilen operatöre bir kullanıcı tanımlı işleç ve işlem bölümünde anlatıldığı gibi işlenir [kullanıcı tanımlı koşullu mantıksal işleçler](expressions.md#user-defined-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="958cd-2293">Otherwise, the selected operator is a user-defined operator, and the operation is processed as described in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

<span data-ttu-id="958cd-2294">Doğrudan koşullu mantıksal işleçler aşırı mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2294">It is not possible to directly overload the conditional logical operators.</span></span> <span data-ttu-id="958cd-2295">Koşullu mantıksal işleçler normal mantıksal işleçler bakımından değerlendirildiği için ancak normal mantıksal işleçler aşırı belirli kısıtlamalar ile de koşullu mantıksal işleçler aşırı yüklemeleri olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2295">However, because the conditional logical operators are evaluated in terms of the regular logical operators, overloads of the regular logical operators are, with certain restrictions, also considered overloads of the conditional logical operators.</span></span> <span data-ttu-id="958cd-2296">Daha ayrıntılı açıklanır budur [kullanıcı tanımlı koşullu mantıksal işleçler](expressions.md#user-defined-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="958cd-2296">This is described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

### <a name="boolean-conditional-logical-operators"></a><span data-ttu-id="958cd-2297">Boole koşullu mantıksal işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2297">Boolean conditional logical operators</span></span>

<span data-ttu-id="958cd-2298">Zaman işleneni `&&` veya `||` türü `bool`, veya uygun bir tanımlamazsanız türde olduğunda işlenenler `operator &` veya `operator |`, ancak örtük dönüştürme `bool`, işlem şu şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="958cd-2298">When the operands of `&&` or `||` are of type `bool`, or when the operands are of types that do not define an applicable `operator &` or `operator |`, but do define implicit conversions to `bool`, the operation is processed as follows:</span></span>

*  <span data-ttu-id="958cd-2299">İşlemi `x && y` değerlendirmesinde `x ? y : false`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2299">The operation `x && y` is evaluated as `x ? y : false`.</span></span> <span data-ttu-id="958cd-2300">Diğer bir deyişle, `x` önce değerlendirilir ve türüne dönüştürülemediğinden `bool`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2300">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="958cd-2301">Ardından, eğer `x` olduğu `true`, `y` değerlendirilir ve türüne dönüştürülemediğinden `bool`, ve bu işlemin sonucunu olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2301">Then, if `x` is `true`, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span> <span data-ttu-id="958cd-2302">Aksi takdirde işlemi sonucudur `false`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2302">Otherwise, the result of the operation is `false`.</span></span>
*  <span data-ttu-id="958cd-2303">İşlemi `x || y` değerlendirmesinde `x ? true : y`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2303">The operation `x || y` is evaluated as `x ? true : y`.</span></span> <span data-ttu-id="958cd-2304">Diğer bir deyişle, `x` önce değerlendirilir ve türüne dönüştürülemediğinden `bool`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2304">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="958cd-2305">Ardından, eğer `x` olduğu `true`, işlem sonucu `true`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2305">Then, if `x` is `true`, the result of the operation is `true`.</span></span> <span data-ttu-id="958cd-2306">Aksi takdirde, `y` değerlendirilir ve türüne dönüştürülemediğinden `bool`, ve bu işlemin sonucunu olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2306">Otherwise, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span>

### <a name="user-defined-conditional-logical-operators"></a><span data-ttu-id="958cd-2307">Kullanıcı tanımlı koşullu mantıksal işleçler</span><span class="sxs-lookup"><span data-stu-id="958cd-2307">User-defined conditional logical operators</span></span>

<span data-ttu-id="958cd-2308">Olduğunda, işlenenler `&&` veya `||` uygun bir bildirme türleri kullanıcı tanımlı `operator &` veya `operator |`, aşağıdaki her iki burada true olmalıdır `T` seçilen operatöre bildirilmiş türü:</span><span class="sxs-lookup"><span data-stu-id="958cd-2308">When the operands of `&&` or `||` are of types that declare an applicable user-defined `operator &` or `operator |`, both of the following must be true, where `T` is the type in which the selected operator is declared:</span></span>

*  <span data-ttu-id="958cd-2309">Dönüş türü ile seçilen işlecinin her parametresinin türü olmalıdır `T`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2309">The return type and the type of each parameter of the selected operator must be `T`.</span></span> <span data-ttu-id="958cd-2310">Diğer bir deyişle, işlecin mantıksal işlem gerekir `AND` veya mantıksal `OR` türünde iki işlenenden `T`ve bir sonuç türü döndürmelidir `T`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2310">In other words, the operator must compute the logical `AND` or the logical `OR` of two operands of type `T`, and must return a result of type `T`.</span></span>
*  <span data-ttu-id="958cd-2311">`T` bildirimleri içermelidir `operator true` ve `operator false`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2311">`T` must contain declarations of `operator true` and `operator false`.</span></span>

<span data-ttu-id="958cd-2312">Bu iki gereksinimi koşullar karşılanırsa bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2312">A binding-time error occurs if either of these requirements is not satisfied.</span></span> <span data-ttu-id="958cd-2313">Aksi takdirde, `&&` veya `||` işlemi, kullanıcı tanımlı birleştirerek değerlendirilir `operator true` veya `operator false` seçili kullanıcı tanımlı işleç ile:</span><span class="sxs-lookup"><span data-stu-id="958cd-2313">Otherwise, the `&&` or `||` operation is evaluated by combining the user-defined `operator true` or `operator false` with the selected user-defined operator:</span></span>

*  <span data-ttu-id="958cd-2314">İşlemi `x && y` değerlendirmesinde `T.false(x) ? x : T.&(x, y)`burada `T.false(x)` bir çağrıdır, `operator false` bildirilen `T`, ve `T.&(x, y)` bir çağrıdır seçili `operator &`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2314">The operation `x && y` is evaluated as `T.false(x) ? x : T.&(x, y)`, where `T.false(x)` is an invocation of the `operator false` declared in `T`, and `T.&(x, y)` is an invocation of the selected `operator &`.</span></span> <span data-ttu-id="958cd-2315">Diğer bir deyişle, `x` ilk olarak değerlendirilir ve `operator false` sonucunu belirlemek için çağrılır `x` kesinlikle false'tur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2315">In other words, `x` is first evaluated and `operator false` is invoked on the result to determine if `x` is definitely false.</span></span> <span data-ttu-id="958cd-2316">Ardından, eğer `x` işleminin sonucu için önceden hesaplanan değerdir kesinlikle yanlış `x`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2316">Then, if `x` is definitely false, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="958cd-2317">Aksi takdirde, `y` değerlendirilir ve seçili `operator &` için önceden hesaplanan değer çağrılır `x` ve için hesaplanan değer `y` işlemin sonucu verecek.</span><span class="sxs-lookup"><span data-stu-id="958cd-2317">Otherwise, `y` is evaluated, and the selected `operator &` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>
*  <span data-ttu-id="958cd-2318">İşlemi `x || y` değerlendirmesinde `T.true(x) ? x : T.|(x, y)`burada `T.true(x)` bir çağrıdır, `operator true` bildirilen `T`, ve `T.|(x,y)` bir çağrıdır seçili `operator|`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2318">The operation `x || y` is evaluated as `T.true(x) ? x : T.|(x, y)`, where `T.true(x)` is an invocation of the `operator true` declared in `T`, and `T.|(x,y)` is an invocation of the selected `operator|`.</span></span> <span data-ttu-id="958cd-2319">Diğer bir deyişle, `x` ilk olarak değerlendirilir ve `operator true` sonucunu belirlemek için çağrılır `x` kesinlikle geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2319">In other words, `x` is first evaluated and `operator true` is invoked on the result to determine if `x` is definitely true.</span></span> <span data-ttu-id="958cd-2320">Ardından, eğer `x` işleminin sonucu için önceden hesaplanan değerdir kesinlikle true `x`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2320">Then, if `x` is definitely true, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="958cd-2321">Aksi takdirde, `y` değerlendirilir ve seçili `operator |` için önceden hesaplanan değer çağrılır `x` ve için hesaplanan değer `y` işlemin sonucu verecek.</span><span class="sxs-lookup"><span data-stu-id="958cd-2321">Otherwise, `y` is evaluated, and the selected `operator |` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>

<span data-ttu-id="958cd-2322">Bu işlemler tarafından sağlanan ifadenin birini `x` yalnızca bir kez değerlendirilir ve tarafından sağlanan ifadenin `y` olmayan hesaplanan veya tam bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2322">In either of these operations, the expression given by `x` is only evaluated once, and the expression given by `y` is either not evaluated or evaluated exactly once.</span></span>

<span data-ttu-id="958cd-2323">Uygulayan bir tür örneği `operator true` ve `operator false`, bkz: [veritabanı Boole türü](structs.md#database-boolean-type).</span><span class="sxs-lookup"><span data-stu-id="958cd-2323">For an example of a type that implements `operator true` and `operator false`, see [Database boolean type](structs.md#database-boolean-type).</span></span>

## <a name="the-null-coalescing-operator"></a><span data-ttu-id="958cd-2324">Null birleşim işleci</span><span class="sxs-lookup"><span data-stu-id="958cd-2324">The null coalescing operator</span></span>

<span data-ttu-id="958cd-2325">`??` İşleci, null birleşim işleci çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2325">The `??` operator is called the null coalescing operator.</span></span>

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

<span data-ttu-id="958cd-2326">Bir null birleşim ifadesi formun `a ?? b` gerektirir `a` boş değer atanabilir bir tür ya da başvuru türünde olması.</span><span class="sxs-lookup"><span data-stu-id="958cd-2326">A null coalescing expression of the form `a ?? b` requires `a` to be of a nullable type or reference type.</span></span> <span data-ttu-id="958cd-2327">Varsa `a` kullanmaktan, sonucunu `a ?? b` olduğu `a`; Aksi takdirde sonuç `b`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2327">If `a` is non-null, the result of `a ?? b` is `a`; otherwise, the result is `b`.</span></span> <span data-ttu-id="958cd-2328">İşlemi değerlendirir `b` yalnızca `a` null.</span><span class="sxs-lookup"><span data-stu-id="958cd-2328">The operation evaluates `b` only if `a` is null.</span></span>

<span data-ttu-id="958cd-2329">Null birleşim işleci sağla ilişkilendirilebilir, işlemler soldan sağa gruplandırılır anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2329">The null coalescing operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="958cd-2330">Örneğin, bir ifade formun `a ?? b ?? c` değerlendirmesinde `a ?? (b ?? c)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2330">For example, an expression of the form `a ?? b ?? c` is evaluated as `a ?? (b ?? c)`.</span></span> <span data-ttu-id="958cd-2331">Genel koşulları, bir ifade formun `E1 ?? E2 ?? ... ?? En` ilk işlenenden null olmayan veya tüm işlenenler null ise null döndürür.</span><span class="sxs-lookup"><span data-stu-id="958cd-2331">In general terms, an expression of the form `E1 ?? E2 ?? ... ?? En` returns the first of the operands that is non-null, or null if all operands are null.</span></span>

<span data-ttu-id="958cd-2332">İfadenin türü `a ?? b` hangi örtük dönüştürmelerin işlenenler üzerinde kullanılabilir olduğuna bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2332">The type of the expression `a ?? b` depends on which implicit conversions are available on the operands.</span></span> <span data-ttu-id="958cd-2333">Türünü tercih sırasına göre `a ?? b` olduğu `A0`, `A`, veya `B`burada `A` türü `a` (koşuluyla `a` türüne sahip), `B` türü `b` ( koşuluyla `b` türüne sahip), ve `A0` temel alınan türü `A` varsa `A` boş değer atanabilir bir tür veya `A` Aksi takdirde.</span><span class="sxs-lookup"><span data-stu-id="958cd-2333">In order of preference, the type of `a ?? b` is `A0`, `A`, or `B`, where `A` is the type of `a` (provided that `a` has a type), `B` is the type of `b` (provided that `b` has a type), and `A0` is the underlying type of `A` if `A` is a nullable type, or `A` otherwise.</span></span> <span data-ttu-id="958cd-2334">Özellikle, `a ?? b` şu şekilde işlenir:</span><span class="sxs-lookup"><span data-stu-id="958cd-2334">Specifically, `a ?? b` is processed as follows:</span></span>

*  <span data-ttu-id="958cd-2335">Varsa `A` var ve boş değer atanabilir bir tür veya bir başvuru türünün bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2335">If `A` exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>
*  <span data-ttu-id="958cd-2336">Varsa `b` dinamik bir ifade sonucu türü `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2336">If `b` is a dynamic expression, the result type is `dynamic`.</span></span> <span data-ttu-id="958cd-2337">Çalışma zamanında, `a` ilk olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2337">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="958cd-2338">Varsa `a` null değil `a` dinamik diske dönüştürülür ve bu sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2338">If `a` is not null, `a` is converted to dynamic, and this becomes the result.</span></span> <span data-ttu-id="958cd-2339">Aksi takdirde, `b` değerlendirilir ve bu sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2339">Otherwise, `b` is evaluated, and this becomes the result.</span></span>
*  <span data-ttu-id="958cd-2340">Aksi takdirde `A` var ve boş değer atanabilir bir tür ve öğesinden örtük bir dönüştürme yok `b` için `A0`, sonuç türü olan `A0`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2340">Otherwise, if `A` exists and is a nullable type and an implicit conversion exists from `b` to `A0`, the result type is `A0`.</span></span> <span data-ttu-id="958cd-2341">Çalışma zamanında, `a` ilk olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2341">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="958cd-2342">Varsa `a` null değil `a` yazmak için sarmalanmamış `A0`, ve bu sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2342">If `a` is not null, `a` is unwrapped to type `A0`, and this becomes the result.</span></span> <span data-ttu-id="958cd-2343">Aksi takdirde, `b` değerlendirilir ve türüne dönüştürülemediğinden `A0`, ve bu sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2343">Otherwise, `b` is evaluated and converted to type `A0`, and this becomes the result.</span></span>
*  <span data-ttu-id="958cd-2344">Aksi takdirde `A` var ve gelen örtük bir dönüştürme var `b` için `A`, sonuç türü olan `A`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2344">Otherwise, if `A` exists and an implicit conversion exists from `b` to `A`, the result type is `A`.</span></span> <span data-ttu-id="958cd-2345">Çalışma zamanında, `a` ilk olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2345">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="958cd-2346">Varsa `a` null değil `a` sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2346">If `a` is not null, `a` becomes the result.</span></span> <span data-ttu-id="958cd-2347">Aksi takdirde, `b` değerlendirilir ve türüne dönüştürülemediğinden `A`, ve bu sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2347">Otherwise, `b` is evaluated and converted to type `A`, and this becomes the result.</span></span>
*  <span data-ttu-id="958cd-2348">Aksi takdirde `b` bir türe sahip `B` ve gelen örtük bir dönüştürme var `a` için `B`, sonuç türü olan `B`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2348">Otherwise, if `b` has a type `B` and an implicit conversion exists from `a` to `B`, the result type is `B`.</span></span> <span data-ttu-id="958cd-2349">Çalışma zamanında, `a` ilk olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2349">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="958cd-2350">Varsa `a` null değil `a` yazmak için sarmalanmamış olur `A0` (varsa `A` yok ve null olabilir) ve türüne dönüştürülmüş `B`, ve bu sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2350">If `a` is not null, `a` is unwrapped to type `A0` (if `A` exists and is nullable) and converted to type `B`, and this becomes the result.</span></span> <span data-ttu-id="958cd-2351">Aksi takdirde, `b` değerlendirilir ve sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2351">Otherwise, `b` is evaluated and becomes the result.</span></span>
*  <span data-ttu-id="958cd-2352">Aksi takdirde, `a` ve `b` uyumlu olmayan ve bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2352">Otherwise, `a` and `b` are incompatible, and a compile-time error occurs.</span></span>

## <a name="conditional-operator"></a><span data-ttu-id="958cd-2353">Koşullu işleç</span><span class="sxs-lookup"><span data-stu-id="958cd-2353">Conditional operator</span></span>

<span data-ttu-id="958cd-2354">`?:` İşleç, koşullu işleç çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2354">The `?:` operator is called the conditional operator.</span></span> <span data-ttu-id="958cd-2355">Bazen de Üçlü işleç adı verilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2355">It is at times also called the ternary operator.</span></span>

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

<span data-ttu-id="958cd-2356">Bir koşullu ifade formun `b ? x : y` önce koşulu değerlendirir `b`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2356">A conditional expression of the form `b ? x : y` first evaluates the condition `b`.</span></span> <span data-ttu-id="958cd-2357">Ardından, eğer `b` olduğu `true`, `x` değerlendirilir ve işlemin sonucu olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2357">Then, if `b` is `true`, `x` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="958cd-2358">Aksi takdirde, `y` değerlendirilir ve işlemin sonucu olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2358">Otherwise, `y` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="958cd-2359">Bir koşullu ifade hiçbir zaman hem de değerlendirir `x` ve `y`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2359">A conditional expression never evaluates both `x` and `y`.</span></span>

<span data-ttu-id="958cd-2360">Koşullu işleç sağla ilişkilendirilebilir, işlemler soldan sağa gruplandırılır anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2360">The conditional operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="958cd-2361">Örneğin, bir ifade formun `a ? b : c ? d : e` değerlendirmesinde `a ? b : (c ? d : e)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2361">For example, an expression of the form `a ? b : c ? d : e` is evaluated as `a ? b : (c ? d : e)`.</span></span>

<span data-ttu-id="958cd-2362">İlk işleneni `?:` işleci, örtük olarak dönüştürülebilir bir ifade olmalıdır `bool`, veya bir ifade uygulayan bir tür `operator true`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2362">The first operand of the `?:` operator must be an expression that can be implicitly converted to `bool`, or an expression of a type that implements `operator true`.</span></span> <span data-ttu-id="958cd-2363">Bu gereksinimleri hiçbiri sağlanıyorsa, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2363">If neither of these requirements is satisfied, a compile-time error occurs.</span></span>

<span data-ttu-id="958cd-2364">İkinci ve üçüncü işlenenleri `x` ve `y`, biri `?:` işleci denetleyen koşullu ifadenin türü.</span><span class="sxs-lookup"><span data-stu-id="958cd-2364">The second and third operands, `x` and `y`, of the `?:` operator control the type of the conditional expression.</span></span>

*  <span data-ttu-id="958cd-2365">Varsa `x` türünde `X` ve `y` türünde `Y` sonra</span><span class="sxs-lookup"><span data-stu-id="958cd-2365">If `x` has type `X` and `y` has type `Y` then</span></span>
   * <span data-ttu-id="958cd-2366">Örtük bir dönüştürme ise ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) öğesinden var. `X` için `Y`, ancak `Y` için `X`, ardından `Y` koşullu ifadenin türü.</span><span class="sxs-lookup"><span data-stu-id="958cd-2366">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `X` to `Y`, but not from `Y` to `X`, then `Y` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="958cd-2367">Örtük bir dönüştürme ise ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) öğesinden var. `Y` için `X`, ancak `X` için `Y`, ardından `X` koşullu ifadenin türü.</span><span class="sxs-lookup"><span data-stu-id="958cd-2367">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `Y` to `X`, but not from `X` to `Y`, then `X` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="958cd-2368">Aksi takdirde, hiçbir ifade türü belirlenebilir ve bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2368">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="958cd-2369">Yalnızca birini `x` ve `y` bir tür ve her ikisi de sahip `x` ve `y`, koşullu ifadenin türü ise, bu türe örtük olarak dönüştürülebilir olduğundan.</span><span class="sxs-lookup"><span data-stu-id="958cd-2369">If only one of `x` and `y` has a type, and both `x` and `y`, of are implicitly convertible to that type, then that is the type of the conditional expression.</span></span>
*  <span data-ttu-id="958cd-2370">Aksi takdirde, hiçbir ifade türü belirlenebilir ve bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2370">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>

<span data-ttu-id="958cd-2371">Bir koşullu ifade formun çalışma zamanı işlenmesini `b ? x : y` aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="958cd-2371">The run-time processing of a conditional expression of the form `b ? x : y` consists of the following steps:</span></span>

*  <span data-ttu-id="958cd-2372">İlk olarak, `b` değerlendirilir ve `bool` değerini `b` belirlenir:</span><span class="sxs-lookup"><span data-stu-id="958cd-2372">First, `b` is evaluated, and the `bool` value of `b` is determined:</span></span>
   * <span data-ttu-id="958cd-2373">Örtük bir dönüştürme, türü `b` için `bool` üretmek için bu örtülü dönüştürme işlemi daha sonra mevcut bir `bool` değeri.</span><span class="sxs-lookup"><span data-stu-id="958cd-2373">If an implicit conversion from the type of `b` to `bool` exists, then this implicit conversion is performed to produce a `bool` value.</span></span>
   * <span data-ttu-id="958cd-2374">Aksi takdirde, `operator true` türüne göre tanımlanan `b` oluşturmak için çağrılan bir `bool` değeri.</span><span class="sxs-lookup"><span data-stu-id="958cd-2374">Otherwise, the `operator true` defined by the type of `b` is invoked to produce a `bool` value.</span></span>
*  <span data-ttu-id="958cd-2375">Varsa `bool` Yukarıdaki adımı tarafından üretilen değer `true`, ardından `x` değerlendirilir ve koşullu ifade türüne dönüştürülür ve bu koşullu ifadenin sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2375">If the `bool` value produced by the step above is `true`, then `x` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>
*  <span data-ttu-id="958cd-2376">Aksi takdirde, `y` değerlendirilir ve koşullu ifade türüne dönüştürülür ve bu koşullu ifadenin sonuç olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2376">Otherwise, `y` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>

## <a name="anonymous-function-expressions"></a><span data-ttu-id="958cd-2377">Anonim işlev ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2377">Anonymous function expressions</span></span>

<span data-ttu-id="958cd-2378">Bir ***anonim işlev*** bir "satır içi" yöntem tanımını temsil eden bir ifadedir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2378">An ***anonymous function*** is an expression that represents an "in-line" method definition.</span></span> <span data-ttu-id="958cd-2379">Anonim bir işlev bir değer veya tür içinde ve kendisinin yok, ancak uyumlu temsilci veya ifade ağacı türüne dönüştürülemez.</span><span class="sxs-lookup"><span data-stu-id="958cd-2379">An anonymous function does not have a value or type in and of itself, but is convertible to a compatible delegate or expression tree type.</span></span> <span data-ttu-id="958cd-2380">Bir anonim işlev dönüştürme değerlendirmesi dönüştürme hedef türüne bağlıdır: bir temsilci türü ise, anonim işlevini tanımlayan yöntemin başvuran bir temsilci değerine dönüştürme değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2380">The evaluation of an anonymous function conversion depends on the target type of the conversion: If it is a delegate type, the conversion evaluates to a delegate value referencing the method which the anonymous function defines.</span></span> <span data-ttu-id="958cd-2381">Bir ifade ağacı türü ise, dönüştürme yöntemi bir nesne yapısını olarak yapısını temsil eden bir ifade ağacı için değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2381">If it is an expression tree type, the conversion evaluates to an expression tree which represents the structure of the method as an object structure.</span></span>

<span data-ttu-id="958cd-2382">Geçmiş nedenleri vardır iki söz dizimsel türü anonim İşlevler, yani *lambda_expression*s ve *anonymous_method_expression*s.</span><span class="sxs-lookup"><span data-stu-id="958cd-2382">For historical reasons there are two syntactic flavors of anonymous functions, namely *lambda_expression*s and *anonymous_method_expression*s.</span></span> <span data-ttu-id="958cd-2383">Neredeyse tüm amacıyla *lambda_expression*s daha kısa ve daha ifadesel *anonymous_method_expression*s, geriye dönük uyumluluk için dilde kalır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2383">For almost all purposes, *lambda_expression*s are more concise and expressive than *anonymous_method_expression*s, which remain in the language for backwards compatibility.</span></span>

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

<span data-ttu-id="958cd-2384">`=>` İşleci atamayla aynı önceliğe sahip (`=`) ve sağa ilişkilendirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2384">The `=>` operator has the same precedence as assignment (`=`) and is right-associative.</span></span>

<span data-ttu-id="958cd-2385">Anonim bir işlevle `async` değiştiricisi bir zaman uyumsuz işlev ve açıklanan kurallara [yineleyiciler](classes.md#iterators).</span><span class="sxs-lookup"><span data-stu-id="958cd-2385">An anonymous function with the `async` modifier is an async function and follows the rules described in [Iterators](classes.md#iterators).</span></span>

<span data-ttu-id="958cd-2386">Biçiminde anonim bir işlevin parametreleri bir *lambda_expression* açıkça veya dolaylı olarak yazılabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2386">The parameters of an anonymous function in the form of a *lambda_expression* can be explicitly or implicitly typed.</span></span> <span data-ttu-id="958cd-2387">Bir açıkça belirtilmiş bir parametre listesinde her parametresinin türünü açıkça belirtilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2387">In an explicitly typed parameter list, the type of each parameter is explicitly stated.</span></span> <span data-ttu-id="958cd-2388">Bir türü örtük olarak belirlenmiş parametre listesinde parametre türleri anonim işlev oluştuğu bağlamdan algılanır — özellikle zaman anonim işlev bir uyumlu temsilci türü veya türü sağlayan ifade ağaç türüne dönüştürülür parametre türleri ([anonim işlev dönüştürmeler](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2388">In an implicitly typed parameter list, the types of the parameters are inferred from the context in which the anonymous function occurs—specifically, when the anonymous function is converted to a compatible delegate type or expression tree type, that type provides the parameter types ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="958cd-2389">Türü örtük olarak belirlenmiş, tek bir parametre ile anonim işlevde parametre listesinden parantezler atlanabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2389">In an anonymous function with a single, implicitly typed parameter, the parentheses may be omitted from the parameter list.</span></span> <span data-ttu-id="958cd-2390">Formun başka bir deyişle, anonim bir işlev</span><span class="sxs-lookup"><span data-stu-id="958cd-2390">In other words, an anonymous function of the form</span></span>
```csharp
( param ) => expr
```
<span data-ttu-id="958cd-2391">olarak kısaltılır</span><span class="sxs-lookup"><span data-stu-id="958cd-2391">can be abbreviated to</span></span>
```csharp
param => expr
```

<span data-ttu-id="958cd-2392">Biçiminde anonim bir işlevin parametre listesine bir *anonymous_method_expression* isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2392">The parameter list of an anonymous function in the form of an *anonymous_method_expression* is optional.</span></span> <span data-ttu-id="958cd-2393">Verilen parametreler açıkça yazılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2393">If given, the parameters must be explicitly typed.</span></span> <span data-ttu-id="958cd-2394">Anonim işlevi herhangi bir parametre ile bir temsilciye dönüştürülebilir değilse değil içeren liste `out` parametreleri.</span><span class="sxs-lookup"><span data-stu-id="958cd-2394">If not, the anonymous function is convertible to a delegate with any parameter list not containing `out` parameters.</span></span>

<span data-ttu-id="958cd-2395">A *blok* anonim bir işlev gövdesini erişilebilir ([uç noktaları ve ulaşılabilirliği](statements.md#end-points-and-reachability)) erişilemeyen bir ifadeye anonim işlev gerçekleşmediği sürece.</span><span class="sxs-lookup"><span data-stu-id="958cd-2395">A *block* body of an anonymous function is reachable ([End points and reachability](statements.md#end-points-and-reachability)) unless the anonymous function occurs inside an unreachable statement.</span></span>

<span data-ttu-id="958cd-2396">Anonim işlevler bazı örnekleri aşağıda izleyin:</span><span class="sxs-lookup"><span data-stu-id="958cd-2396">Some examples of anonymous functions follow below:</span></span>

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

<span data-ttu-id="958cd-2397">Davranışını *lambda_expression*s ve *anonymous_method_expression*s aşağıdaki noktaları dışında aynı olan:</span><span class="sxs-lookup"><span data-stu-id="958cd-2397">The behavior of *lambda_expression*s and *anonymous_method_expression*s is the same except for the following points:</span></span>

*  <span data-ttu-id="958cd-2398">*anonymous_method_expression*s, herhangi bir değer parametreleri listesi türleri temsilci convertibility sonuçlanmıyor tamamen atlanacak parametre listesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2398">*anonymous_method_expression*s permit the parameter list to be omitted entirely, yielding convertibility to delegate types of any list of value parameters.</span></span>
*  <span data-ttu-id="958cd-2399">*lambda_expression*s izin atlanmış ve ancak çıkarımı yapılan parametre türleri *anonymous_method_expression*s açıkça belirtilmediği parametre türleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2399">*lambda_expression*s permit parameter types to be omitted and inferred whereas *anonymous_method_expression*s require parameter types to be explicitly stated.</span></span>
*  <span data-ttu-id="958cd-2400">Gövdesi bir *lambda_expression* ise bir ifade ya da bir deyim bloğunu olabilir gövdesi bir *anonymous_method_expression* bir deyim bloğunu olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2400">The body of a *lambda_expression* can be an expression or a statement block whereas the body of an *anonymous_method_expression* must be a statement block.</span></span>
*  <span data-ttu-id="958cd-2401">Yalnızca *lambda_expression*s sahip uyumlu bir ifade ağacı türlerine dönüştürme işlemi ([ifade ağacı türleri](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2401">Only *lambda_expression*s have conversions to compatible expression tree types ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-signatures"></a><span data-ttu-id="958cd-2402">Anonim işlev imzası</span><span class="sxs-lookup"><span data-stu-id="958cd-2402">Anonymous function signatures</span></span>

<span data-ttu-id="958cd-2403">İsteğe bağlı *anonymous_function_signature* anonim bir işlev adlarını ve isteğe bağlı olarak anonim işlev biçimsel parametre türlerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="958cd-2403">The optional *anonymous_function_signature* of an anonymous function defines the names and optionally the types of the formal parameters for the anonymous function.</span></span> <span data-ttu-id="958cd-2404">Anonim işlev parametrelerinin kapsamı *anonymous_function_body*.</span><span class="sxs-lookup"><span data-stu-id="958cd-2404">The scope of the parameters of the anonymous function is the *anonymous_function_body*.</span></span> <span data-ttu-id="958cd-2405">([Kapsamları](basic-concepts.md#scopes)) (belirtilmişse) parametre listesi ile birlikte anonim yöntem-gövdesi bir bildirim alanı oluşturur ([bildirimleri](basic-concepts.md#declarations)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2405">([Scopes](basic-concepts.md#scopes)) Together with the parameter list (if given) the anonymous-method-body constitutes a declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="958cd-2406">Bu nedenle bir yerel değişken, yerel sabit veya kapsamı içeren parametre adını eşleştirmek için anonim işlev bir parametre adı için bir derleme zamanı hatası olduğundan *anonymous_method_expression* veya *lambda_ ifade*.</span><span class="sxs-lookup"><span data-stu-id="958cd-2406">It is thus a compile-time error for the name of a parameter of the anonymous function to match the name of a local variable, local constant or parameter whose scope includes the *anonymous_method_expression* or *lambda_expression*.</span></span>

<span data-ttu-id="958cd-2407">Anonim bir işlevdir varsa bir *explicit_anonymous_function_signature*, uyumlu temsilci türleri ve ifade ağacı türleri kümesini aynı sırada değiştiriciler ve aynı parametre türlerine sahip olanlar kısıtlanmış kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2407">If an anonymous function has an *explicit_anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have the same parameter types and modifiers in the same order.</span></span> <span data-ttu-id="958cd-2408">Yöntem grubu dönüştürmeler aksine ([yöntem grubu dönüştürmeler](conversions.md#method-group-conversions)), anonim işlevi parametre türleri karşıt varyansını desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="958cd-2408">In contrast to method group conversions ([Method group conversions](conversions.md#method-group-conversions)), contra-variance of anonymous function parameter types is not supported.</span></span> <span data-ttu-id="958cd-2409">Anonim bir işlevdir yoksa bir *anonymous_function_signature*, uyumlu temsilci türleri ve ifade ağacı türleri kümesini sahip olanlar kısıtlanmış ise `out` parametreleri.</span><span class="sxs-lookup"><span data-stu-id="958cd-2409">If an anonymous function does not have an *anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have no `out` parameters.</span></span>

<span data-ttu-id="958cd-2410">Unutmayın bir *anonymous_function_signature* öznitelikleri veya bir parametre dizisi içeremez.</span><span class="sxs-lookup"><span data-stu-id="958cd-2410">Note that an *anonymous_function_signature* cannot include attributes or a parameter array.</span></span> <span data-ttu-id="958cd-2411">Bununla birlikte, bir *anonymous_function_signature* , parametre listesi içeren bir parametre dizisi bir temsilci türüyle uyumlu olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2411">Nevertheless, an *anonymous_function_signature* may be compatible with a delegate type whose parameter list contains a parameter array.</span></span>

<span data-ttu-id="958cd-2412">Derleme zamanında bile uyumlu yine de başarısız olabilir, ayrıca, bir ifade ağaç türüne dönüştürme unutmayın ([ifade ağacı türleri](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2412">Note also that conversion to an expression tree type, even if compatible, may still fail at compile-time ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-bodies"></a><span data-ttu-id="958cd-2413">Anonim işlev gövdeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2413">Anonymous function bodies</span></span>

<span data-ttu-id="958cd-2414">Gövde (*ifade* veya *blok*) anonim bir işlev olan aşağıdaki kurallarına tabidir:</span><span class="sxs-lookup"><span data-stu-id="958cd-2414">The body (*expression* or *block*) of an anonymous function is subject to the following rules:</span></span>

*  <span data-ttu-id="958cd-2415">Anonim işlev bir imza içeriyorsa, imzasında belirtilen parametreler gövdesinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2415">If the anonymous function includes a signature, the parameters specified in the signature are available in the body.</span></span> <span data-ttu-id="958cd-2416">Anonim işlev imzasına varsa, bir temsilci türü ya da ifade türü parametreleri olan dönüştürülebilir ([anonim işlev dönüştürmeler](conversions.md#anonymous-function-conversions)), ancak parametreler gövdesinde erişilemez.</span><span class="sxs-lookup"><span data-stu-id="958cd-2416">If the anonymous function has no signature it can be converted to a delegate type or expression type having parameters ([Anonymous function conversions](conversions.md#anonymous-function-conversions)), but the parameters cannot be accessed in the body.</span></span>
*  <span data-ttu-id="958cd-2417">Dışında `ref` veya `out` en yakın kapsayan imzasında (varsa) belirtilen parametreler anonim işlev gövdesi erişmek için bir derleme zamanı hatası ise bir `ref` veya `out` parametresi.</span><span class="sxs-lookup"><span data-stu-id="958cd-2417">Except for `ref` or `out` parameters specified in the signature (if any) of the nearest enclosing anonymous function, it is a compile-time error for the body to access a `ref` or `out` parameter.</span></span>
*  <span data-ttu-id="958cd-2418">Zaman türü `this` bir yapı türü gövdesi erişmek için bir derleme zamanı hata `this`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2418">When the type of `this` is a struct type, it is a compile-time error for the body to access `this`.</span></span> <span data-ttu-id="958cd-2419">Bu erişim açık olup geçerlidir (olarak `this.x`) ya da örtük (olarak `x` burada `x` struct'ın bir örnek üyesi).</span><span class="sxs-lookup"><span data-stu-id="958cd-2419">This is true whether the access is explicit (as in `this.x`) or implicit (as in `x` where `x` is an instance member of the struct).</span></span> <span data-ttu-id="958cd-2420">Bu kural yalnızca erişimi engeller ve üye araması sonuçları struct üyesi olup olmadığını etkilemez.</span><span class="sxs-lookup"><span data-stu-id="958cd-2420">This rule simply prohibits such access and does not affect whether member lookup results in a member of the struct.</span></span>
*  <span data-ttu-id="958cd-2421">Gövde dış değişkenlere erişebilir ([dış değişkenler](expressions.md#outer-variables)), anonim bir işlevdir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2421">The body has access to the outer variables ([Outer variables](expressions.md#outer-variables)) of the anonymous function.</span></span> <span data-ttu-id="958cd-2422">Dış bir değişkenin erişim sırasında etkin olan değişken örneğini başvuracağı *lambda_expression* veya *anonymous_method_expression* değerlendirilir ([değerlendirmesi Anonim işlev ifadeleri](expressions.md#evaluation-of-anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2422">Access of an outer variable will reference the instance of the variable that is active at the time the *lambda_expression* or *anonymous_method_expression* is evaluated ([Evaluation of anonymous function expressions](expressions.md#evaluation-of-anonymous-function-expressions)).</span></span>
*  <span data-ttu-id="958cd-2423">Bir derleme zamanı hata içerecek şekilde gövdesi için bir `goto` deyimi, `break` deyimi, veya `continue` hedefi, gövdenin dışında veya kapsanan anonim bir işlevin gövdesi içinde olan ifade.</span><span class="sxs-lookup"><span data-stu-id="958cd-2423">It is a compile-time error for the body to contain a `goto` statement, `break` statement, or `continue` statement whose target is outside the body or within the body of a contained anonymous function.</span></span>
*  <span data-ttu-id="958cd-2424">A `return` gövdesinde deyimi en yakın kapsayan bir çağrısından denetim döndürür kapsayan işlev üyesi değil, anonim işlev.</span><span class="sxs-lookup"><span data-stu-id="958cd-2424">A `return` statement in the body returns control from an invocation of the nearest enclosing anonymous function, not from the enclosing function member.</span></span> <span data-ttu-id="958cd-2425">Belirtilen bir ifade bir `return` ifadesi örtük olarak temsilci türüne veya ifade ağacı türü olan dönüş türüne dönüştürülebilir olmalıdır en yakın kapsayan *lambda_expression* veya *anonymous_ method_expression* dönüştürülür ([anonim işlev dönüştürmeler](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2425">An expression specified in a `return` statement must be implicitly convertible to the return type of the delegate type or expression tree type to which the nearest enclosing *lambda_expression* or *anonymous_method_expression* is converted ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="958cd-2426">Dışında bir anonim İşlev değerlendirmesi ve çağırmayı aracılığıyla bloğunu yürütülecek herhangi bir şekilde olup olmadığını açıkça belirtilmeyen *lambda_expression* veya *anonymous_method_expression*.</span><span class="sxs-lookup"><span data-stu-id="958cd-2426">It is explicitly unspecified whether there is any way to execute the block of an anonymous function other than through evaluation and invocation of the *lambda_expression* or *anonymous_method_expression*.</span></span> <span data-ttu-id="958cd-2427">Özellikle, derleyici bir synthesizing anonim bir işlevdir uygulamayı seçebilir veya daha fazla yöntemleri veya türleri adlandırılmış.</span><span class="sxs-lookup"><span data-stu-id="958cd-2427">In particular, the compiler may choose to implement an anonymous function by synthesizing one or more named methods or types.</span></span> <span data-ttu-id="958cd-2428">Derleyici kullanılmak üzere ayrılmış bir formun tür Sentezlenen öğeleri adları olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2428">The names of any such synthesized elements must be of a form reserved for compiler use.</span></span>

### <a name="overload-resolution-and-anonymous-functions"></a><span data-ttu-id="958cd-2429">Aşırı yükleme çözünürlüğü ve anonim İşlevler</span><span class="sxs-lookup"><span data-stu-id="958cd-2429">Overload resolution and anonymous functions</span></span>

<span data-ttu-id="958cd-2430">Anonim işlevler bir bağımsız değişken listesindeki içinde tür çıkarımı katılmak ve aşırı yükleme çözümlemesi.</span><span class="sxs-lookup"><span data-stu-id="958cd-2430">Anonymous functions in an argument list participate in type inference and overload resolution.</span></span> <span data-ttu-id="958cd-2431">Lütfen [anlam çıkarma](expressions.md#type-inference) ve [aşırı yükleme çözünürlüğü](expressions.md#overload-resolution) kesin kurallar için.</span><span class="sxs-lookup"><span data-stu-id="958cd-2431">Please refer to [Type inference](expressions.md#type-inference) and [Overload resolution](expressions.md#overload-resolution) for the exact rules.</span></span>

<span data-ttu-id="958cd-2432">Aşağıdaki örnek, aşırı yükleme çözünürlüğü anonim işlevler etkisini gösterir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2432">The following example illustrates the effect of anonymous functions on overload resolution.</span></span>

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

<span data-ttu-id="958cd-2433">`ItemList<T>` Sınıfına sahip iki `Sum` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="958cd-2433">The `ItemList<T>` class has two `Sum` methods.</span></span> <span data-ttu-id="958cd-2434">Her alan bir `selector` değeri ayıklar bir liste öğesinden toplama üzerinden bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="958cd-2434">Each takes a `selector` argument, which extracts the value to sum over from a list item.</span></span> <span data-ttu-id="958cd-2435">Ayıklanan değerin şöyle bir `int` veya `double` ve altındaysa aynı şekilde ya da bir `int` veya `double`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2435">The extracted value can be either an `int` or a `double` and the resulting sum is likewise either an `int` or a `double`.</span></span>

<span data-ttu-id="958cd-2436">`Sum` Yöntemleri örneğin kullanılabilir ayrıntı satırları bir sırada bir listeden toplamları hesaplamak için.</span><span class="sxs-lookup"><span data-stu-id="958cd-2436">The `Sum` methods could for example be used to compute sums from a list of detail lines in an order.</span></span>

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

<span data-ttu-id="958cd-2437">İlk çağrısıysa, `orderDetails.Sum`hem `Sum` yöntemleri uygulanabilir olduğundan anonim işlev `d => d. UnitCount` ikisiyle uyumlu `Func<Detail,int>` ve `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2437">In the first invocation of `orderDetails.Sum`, both `Sum` methods are applicable because the anonymous function `d => d. UnitCount` is compatible with both `Func<Detail,int>` and `Func<Detail,double>`.</span></span> <span data-ttu-id="958cd-2438">Ancak, ilk aşırı yükleme çözünürlüğü seçer `Sum` yöntemi olduğundan dönüştürme `Func<Detail,int>` dönüştürme işlemini daha iyidir `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2438">However, overload resolution picks the first `Sum` method because the conversion to `Func<Detail,int>` is better than the conversion to `Func<Detail,double>`.</span></span>

<span data-ttu-id="958cd-2439">İkinci çağrılmasını içinde `orderDetails.Sum`yalnızca ikinci `Sum` yöntemi uygulanabilir olduğundan anonim işlev `d => d.UnitPrice * d.UnitCount` türünde bir değer üreten `double`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2439">In the second invocation of `orderDetails.Sum`, only the second `Sum` method is applicable because the anonymous function `d => d.UnitPrice * d.UnitCount` produces a value of type `double`.</span></span> <span data-ttu-id="958cd-2440">Bu nedenle, aşırı yükleme çözümlemesi seçer ikinci `Sum` , çağırma yöntemi.</span><span class="sxs-lookup"><span data-stu-id="958cd-2440">Thus, overload resolution picks the second `Sum` method for that invocation.</span></span>

### <a name="anonymous-functions-and-dynamic-binding"></a><span data-ttu-id="958cd-2441">Anonim işlevler ve dinamik bağlama</span><span class="sxs-lookup"><span data-stu-id="958cd-2441">Anonymous functions and dynamic binding</span></span>

<span data-ttu-id="958cd-2442">Bir anonim işlev bir alıcı, bağımsız değişken veya dinamik olarak bağlı bir işlemin işlenen olamaz.</span><span class="sxs-lookup"><span data-stu-id="958cd-2442">An anonymous function cannot be a receiver, argument or operand of a dynamically bound operation.</span></span>

### <a name="outer-variables"></a><span data-ttu-id="958cd-2443">Dış değişkenleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2443">Outer variables</span></span>

<span data-ttu-id="958cd-2444">Herhangi bir yerel değişken, değer parametresi veya parametre dizisi kapsamı içerir *lambda_expression* veya *anonymous_method_expression* çağrılır bir ***dış değişken*** Anonim işlevi.</span><span class="sxs-lookup"><span data-stu-id="958cd-2444">Any local variable, value parameter, or parameter array whose scope includes the *lambda_expression* or *anonymous_method_expression* is called an ***outer variable*** of the anonymous function.</span></span> <span data-ttu-id="958cd-2445">Bir sınıfın bir örneği işlevi üyesinde `this` değeri bir değer parametresi olarak kabul edilir ve bir dış değişken işlev üyesi içinde yer alan anonim bir işlevdir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2445">In an instance function member of a class, the `this` value is considered a value parameter and is an outer variable of any anonymous function contained within the function member.</span></span>

#### <a name="captured-outer-variables"></a><span data-ttu-id="958cd-2446">Yakalanan dış değişkenler</span><span class="sxs-lookup"><span data-stu-id="958cd-2446">Captured outer variables</span></span>

<span data-ttu-id="958cd-2447">Bir dış değişkenine bir anonim işlev tarafından başvurulduğunda dış değişken olması bildirilir ***yakalanan*** tarafından anonim bir işlevdir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2447">When an outer variable is referenced by an anonymous function, the outer variable is said to have been ***captured*** by the anonymous function.</span></span> <span data-ttu-id="958cd-2448">Normalde, blok veya olduğu ilişkili deyimi yürütme için bir yerel değişken ömrü sınırlıdır ([yerel değişkenler](variables.md#local-variables)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2448">Ordinarily, the lifetime of a local variable is limited to execution of the block or statement with which it is associated ([Local variables](variables.md#local-variables)).</span></span> <span data-ttu-id="958cd-2449">Ancak, yakalanan bir dış değişken ömrü en az bir temsilci kadar genişletilmiş veya ifade ağacı anonim işlevden oluşturulan çöp toplama işlemi için uygun hale gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2449">However, the lifetime of a captured outer variable is extended at least until the delegate or expression tree created from the anonymous function becomes eligible for garbage collection.</span></span>

<span data-ttu-id="958cd-2450">Örnekte</span><span class="sxs-lookup"><span data-stu-id="958cd-2450">In the example</span></span>
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
<span data-ttu-id="958cd-2451">yerel değişken `x` anonim işlev ve ömrünü tarafından yakalanan `x` döndürüldüğü temsilci kadar en az Genişletilmiş `F` (hangi sonuna kadar gerçekleşmez çöp toplama işlemi için uygun hale gelir program).</span><span class="sxs-lookup"><span data-stu-id="958cd-2451">the local variable `x` is captured by the anonymous function, and the lifetime of `x` is extended at least until the delegate returned from `F` becomes eligible for garbage collection (which doesn't happen until the very end of the program).</span></span> <span data-ttu-id="958cd-2452">Anonim işlev her çağrıldığında'nın aynı örneğinde çalışır olduğundan `x`, örnek çıktısı:</span><span class="sxs-lookup"><span data-stu-id="958cd-2452">Since each invocation of the anonymous function operates on the same instance of `x`, the output of the example is:</span></span>
```
1
2
3
```

<span data-ttu-id="958cd-2453">Yerel bir değişken veya değer parametresi bir anonim işlev tarafından yakalandığında, yerel değişken veya parametre artık sabit bir değişken olarak kabul edilir ([sabit ve taşınabilir değişkenleri](unsafe-code.md#fixed-and-moveable-variables)), ancak bunun yerine bir taşınabilir olarak kabul edilir değişken.</span><span class="sxs-lookup"><span data-stu-id="958cd-2453">When a local variable or a value parameter is captured by an anonymous function, the local variable or parameter is no longer considered to be a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), but is instead considered to be a moveable variable.</span></span> <span data-ttu-id="958cd-2454">Bu nedenle herhangi `unsafe` yakalanan bir dış değişkenin adresini alır kod ilk kullanmalıdır `fixed` değişkeni düzeltmek için deyimi.</span><span class="sxs-lookup"><span data-stu-id="958cd-2454">Thus any `unsafe` code that takes the address of a captured outer variable must first use the `fixed` statement to fix the variable.</span></span>

<span data-ttu-id="958cd-2455">Uncaptured bir değişkeni, yakalanan bir yerel değişken aynı anda birden çok iş parçacığı yürütme kullanıma sunulabilecek şekilde unutmayın.</span><span class="sxs-lookup"><span data-stu-id="958cd-2455">Note that unlike an uncaptured variable, a captured local variable can be simultaneously exposed to multiple threads of execution.</span></span>

#### <a name="instantiation-of-local-variables"></a><span data-ttu-id="958cd-2456">Yerel değişkenler örneklemesi</span><span class="sxs-lookup"><span data-stu-id="958cd-2456">Instantiation of local variables</span></span>

<span data-ttu-id="958cd-2457">Yerel bir değişken olarak kabul edilir ***örneği*** yürütme, değişken kapsamını girdiğinde.</span><span class="sxs-lookup"><span data-stu-id="958cd-2457">A local variable is considered to be ***instantiated*** when execution enters the scope of the variable.</span></span> <span data-ttu-id="958cd-2458">Örneğin, aşağıdaki yöntem olduğunda çağrılır, yerel değişken `x` örneği ve üç kez başlatılan — döngünün her yinelemesinden için bir kez.</span><span class="sxs-lookup"><span data-stu-id="958cd-2458">For example, when the following method is invoked, the local variable `x` is instantiated and initialized three times—once for each iteration of the loop.</span></span>

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="958cd-2459">Ancak, bildirimi taşıma `x` döngü sonuçları tek bir örneğinin dışında `x`:</span><span class="sxs-lookup"><span data-stu-id="958cd-2459">However, moving the declaration of `x` outside the loop results in a single instantiation of `x`:</span></span>
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="958cd-2460">Yakalanan değil, yerel bir değişken tam olarak ne sıklıkta örneği gözlemek için bir yolu yoktur; örneklemeleri ömrü ayrık olduğundan, yalnızca aynı depolama konumu kullanmak üzere her örneklemesi için mümkündür.</span><span class="sxs-lookup"><span data-stu-id="958cd-2460">When not captured, there is no way to observe exactly how often a local variable is instantiated—because the lifetimes of the instantiations are disjoint, it is possible for each instantiation to simply use the same storage location.</span></span> <span data-ttu-id="958cd-2461">Ancak, bir anonim işlev bir yerel değişken yakaladığında örneklemesine etkisini belirgin hale gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2461">However, when an anonymous function captures a local variable, the effects of instantiation become apparent.</span></span>

<span data-ttu-id="958cd-2462">Örnek</span><span class="sxs-lookup"><span data-stu-id="958cd-2462">The example</span></span>
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
<span data-ttu-id="958cd-2463">çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="958cd-2463">produces the output:</span></span>
```
1
3
5
```

<span data-ttu-id="958cd-2464">Ancak, bildirimi `x` döngü dışına taşındı:</span><span class="sxs-lookup"><span data-stu-id="958cd-2464">However, when the declaration of `x` is moved outside the loop:</span></span>
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
<span data-ttu-id="958cd-2465">Çıktı.</span><span class="sxs-lookup"><span data-stu-id="958cd-2465">the output is:</span></span>
```
5
5
5
```

<span data-ttu-id="958cd-2466">Bir for döngüsü bir yineleme değişkeni bildirirse, bu değişkeni döngü dışında bildirilmesi kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2466">If a for-loop declares an iteration variable, that variable itself is considered to be declared outside of the loop.</span></span> <span data-ttu-id="958cd-2467">Bu nedenle, yineleme değişkeni yakalamak için örnek değiştirilirse:</span><span class="sxs-lookup"><span data-stu-id="958cd-2467">Thus, if the example is changed to capture the iteration variable itself:</span></span>

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
<span data-ttu-id="958cd-2468">yineleme değişkeni yalnızca bir örneğini yakalanan, hangi çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="958cd-2468">only one instance of the iteration variable is captured, which produces the output:</span></span>
```
3
3
3
```

<span data-ttu-id="958cd-2469">Bazı yakalanan değişkenlere paylaşmak henüz başkalarının ayrı Örneğiniz için anonim işlev temsilciler için mümkündür.</span><span class="sxs-lookup"><span data-stu-id="958cd-2469">It is possible for anonymous function delegates to share some captured variables yet have separate instances of others.</span></span> <span data-ttu-id="958cd-2470">Örneğin, varsa `F` değiştirildi</span><span class="sxs-lookup"><span data-stu-id="958cd-2470">For example, if `F` is changed to</span></span>
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
<span data-ttu-id="958cd-2471">' ın aynı örneğine üç temsilcileri yakalama `x` ancak örnekleri'ni ayrı `y`, çıktı şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="958cd-2471">the three delegates capture the same instance of `x` but separate instances of `y`, and the output is:</span></span>
```
1 1
2 1
3 1
```

<span data-ttu-id="958cd-2472">Ayrı anonim işlevler bir harici değişken'ın aynı örneğine yakalayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="958cd-2472">Separate anonymous functions can capture the same instance of an outer variable.</span></span> <span data-ttu-id="958cd-2473">Örnekte:</span><span class="sxs-lookup"><span data-stu-id="958cd-2473">In the example:</span></span>
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
<span data-ttu-id="958cd-2474">yerel değişken'ın aynı örneğine iki anonim işlevler yakalama `x`, ve bu nedenle "değişken üzerinden kurabilmek".</span><span class="sxs-lookup"><span data-stu-id="958cd-2474">the two anonymous functions capture the same instance of the local variable `x`, and they can thus "communicate" through that variable.</span></span> <span data-ttu-id="958cd-2475">Örneğin çıktısı şöyledir:</span><span class="sxs-lookup"><span data-stu-id="958cd-2475">The output of the example is:</span></span>
```
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a><span data-ttu-id="958cd-2476">Anonim işlev ifadeleri değerlendirme</span><span class="sxs-lookup"><span data-stu-id="958cd-2476">Evaluation of anonymous function expressions</span></span>

<span data-ttu-id="958cd-2477">Anonim bir işlevdir `F` her zaman bir temsilci türüne dönüştürülmesi gerekir `D` veya bir ifade ağacı türü `E`, doğrudan ya da temsilci oluşturma ifadesine yürütülmesini aracılığıyla `new D(F)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2477">An anonymous function `F` must always be converted to a delegate type `D` or an expression tree type `E`, either directly or through the execution of a delegate creation expression `new D(F)`.</span></span> <span data-ttu-id="958cd-2478">Bu dönüştürme sonucu anonim işlev açıklandığı belirler [anonim işlev dönüştürmeler](conversions.md#anonymous-function-conversions).</span><span class="sxs-lookup"><span data-stu-id="958cd-2478">This conversion determines the result of the anonymous function, as described in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>

## <a name="query-expressions"></a><span data-ttu-id="958cd-2479">Sorgu ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2479">Query expressions</span></span>

<span data-ttu-id="958cd-2480">***Sorgu ifadelerinde*** SQL ve XQuery gibi ilişkisel ve hiyerarşik sorgu dillerini benzer sorgular için tümleşik dil söz dizimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="958cd-2480">***Query expressions*** provide a language integrated syntax for queries that is similar to relational and hierarchical query languages such as SQL and XQuery.</span></span>

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

<span data-ttu-id="958cd-2481">Sorgu ifadesi ile başlayan bir `from` yan tümcesi ve biter ya da bir `select` veya `group` yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="958cd-2481">A query expression begins with a `from` clause and ends with either a `select` or `group` clause.</span></span> <span data-ttu-id="958cd-2482">İlk `from` yan tümcesi ve ardından sıfır veya daha fazla `from`, `let`, `where`, `join` veya `orderby` yan tümceleri.</span><span class="sxs-lookup"><span data-stu-id="958cd-2482">The initial `from` clause can be followed by zero or more `from`, `let`, `where`, `join` or `orderby` clauses.</span></span> <span data-ttu-id="958cd-2483">Her `from` yan tümcesi ise bir oluşturucuyu Giriş bir ***aralık değişkeni*** öğelerinin aralıkları bir ***dizisi***.</span><span class="sxs-lookup"><span data-stu-id="958cd-2483">Each `from` clause is a generator introducing a ***range variable*** which ranges over the elements of a ***sequence***.</span></span> <span data-ttu-id="958cd-2484">Her `let` yan tümce ortaya çıkarır, önceki aralık değişkenleri yoluyla hesaplanan bir değeri temsil eden bir aralık değişkeni.</span><span class="sxs-lookup"><span data-stu-id="958cd-2484">Each `let` clause introduces a range variable representing a value computed by means of previous range variables.</span></span> <span data-ttu-id="958cd-2485">Her `where` öğeleri sonuçtan dışlar bir filtre yan tümcesi ise.</span><span class="sxs-lookup"><span data-stu-id="958cd-2485">Each `where` clause is a filter that excludes items from the result.</span></span> <span data-ttu-id="958cd-2486">Her `join` yan tümcesi, belirtilen kaynak sırası anahtarlarla eşleşen çiftleri sağlayan başka bir dizinin anahtarları karşılaştırır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2486">Each `join` clause compares specified keys of the source sequence with keys of another sequence, yielding matching pairs.</span></span> <span data-ttu-id="958cd-2487">Her `orderby` yan tümcesi belirtilen ölçütlere göre öğeleri yeniden sıralar. En son `select` veya `group` yan tümcesinin aralık değişkenleri açısından sonucun şeklini belirtir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2487">Each `orderby` clause reorders items according to specified criteria.The final `select` or `group` clause specifies the shape of the result in terms of the range variables.</span></span> <span data-ttu-id="958cd-2488">Son olarak, bir `into` "sorguları bir sonraki sorguda bir oluşturucu olarak bir sorgunun sonuçlarını düşünerek splice için" yan tümcesi kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2488">Finally, an `into` clause can be used to "splice" queries by treating the results of one query as a generator in a subsequent query.</span></span>

### <a name="ambiguities-in-query-expressions"></a><span data-ttu-id="958cd-2489">Sorgu ifadelerinde belirsizlikleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2489">Ambiguities in query expressions</span></span>

<span data-ttu-id="958cd-2490">Sorgu ifadeleri "bağlamsal anahtar sözcükler", yani, belirli bir bağlamda özel bir anlamı yoktur tanımlayıcıları sayısını içerir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2490">Query expressions contain a number of "contextual keywords", i.e., identifiers that have special meaning in a given context.</span></span> <span data-ttu-id="958cd-2491">Bunlar özellikle `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` ve `by`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2491">Specifically these are `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` and `by`.</span></span> <span data-ttu-id="958cd-2492">Sorgu ifadeleri anahtar sözcükler veya basit adları bu tanımlayıcıları karışık kullanımına neden belirsizliklerden kaçınmak için bu tanımlayıcıları anahtar sözcükleri herhangi bir yerde bir sorgu ifadesi içinde gerçekleşen zaman olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2492">In order to avoid ambiguities in query expressions caused by mixed use of these identifiers as keywords or simple names, these identifiers are considered keywords when occurring anywhere within a query expression.</span></span>

<span data-ttu-id="958cd-2493">Bu amaç için bir sorgu ifadesi ile başlayan herhangi bir ifade olan "`from identifier`"dışında herhangi bir belirteci tarafından izlenen"`;`","`=`"veya"`,`".</span><span class="sxs-lookup"><span data-stu-id="958cd-2493">For this purpose, a query expression is any expression that starts with "`from identifier`" followed by any token except "`;`", "`=`" or "`,`".</span></span>

<span data-ttu-id="958cd-2494">Sorgu ifadesi içinde tanımlayıcı olarak bu sözcükleri kullanmak için bunlar ile belirtilebilir "`@`" ([tanımlayıcıları](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2494">In order to use these words as identifiers within a query expression, they can be prefixed with "`@`" ([Identifiers](lexical-structure.md#identifiers)).</span></span>

### <a name="query-expression-translation"></a><span data-ttu-id="958cd-2495">Sorgu ifade çevirisi</span><span class="sxs-lookup"><span data-stu-id="958cd-2495">Query expression translation</span></span>

<span data-ttu-id="958cd-2496">C# dili sorgu ifadeleri yürütme semantikleri belirtmiyor.</span><span class="sxs-lookup"><span data-stu-id="958cd-2496">The C# language does not specify the execution semantics of query expressions.</span></span> <span data-ttu-id="958cd-2497">Bunun yerine, sorgu ifadeleri izliyor yöntemlerine çağrılarını çevrilir *sorgu ifade deseninin* ([sorgu ifade deseninin](expressions.md#the-query-expression-pattern)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2497">Rather, query expressions are translated into invocations of methods that adhere to the *query expression pattern* ([The query expression pattern](expressions.md#the-query-expression-pattern)).</span></span> <span data-ttu-id="958cd-2498">Sorgu ifadeleri adlı yöntem çağrılarını özellikle çevrilir `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, ve `Cast`. Bu yöntemler belirli imzalar ve sonuç türleri açıklandığı beklenir [sorgu ifade deseninin](expressions.md#the-query-expression-pattern).</span><span class="sxs-lookup"><span data-stu-id="958cd-2498">Specifically, query expressions are translated into invocations of methods named `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, and `Cast`.These methods are expected to have particular signatures and result types, as described in [The query expression pattern](expressions.md#the-query-expression-pattern).</span></span> <span data-ttu-id="958cd-2499">Bu yöntemler, sorgulanan nesne örnek yöntemlerine veya nesnenin dış genişletme yöntemleri olabilir ve sorgunun gerçek yürütmesi uyguladıkları.</span><span class="sxs-lookup"><span data-stu-id="958cd-2499">These methods can be instance methods of the object being queried or extension methods that are external to the object, and they implement the actual execution of the query.</span></span>

<span data-ttu-id="958cd-2500">Sorgu ifadeleri gelen çeviri yöntem çağrılarına herhangi bir türü bağlamayı önce gerçekleşen bir söz dizimi eşlemesi ya da aşırı yükleme çözünürlüğü gerçekleştirildi.</span><span class="sxs-lookup"><span data-stu-id="958cd-2500">The translation from query expressions to method invocations is a syntactic mapping that occurs before any type binding or overload resolution has been performed.</span></span> <span data-ttu-id="958cd-2501">Çeviri sözdizimsel olarak doğru olmasını garanti edilmez, ancak anlamsal olarak doğru C# kod üretmek için garanti edilmez.</span><span class="sxs-lookup"><span data-stu-id="958cd-2501">The translation is guaranteed to be syntactically correct, but it is not guaranteed to produce semantically correct C# code.</span></span> <span data-ttu-id="958cd-2502">Çeviri sorgu ifadeleri, aşağıdaki oluşturulan yöntem çağrıları normal yöntem çağrıları işlenir ve bu sırayla hataları, örneğin yanlış tür bağımsız değişkenleri varsa yöntemi, mevcut değilse ya da genel yöntemleri açığa çıkarmak ve tür çıkarımı başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2502">Following translation of query expressions, the resulting method invocations are processed as regular method invocations, and this may in turn uncover errors, for example if the methods do not exist, if arguments have wrong types, or if the methods are generic and type inference fails.</span></span>

<span data-ttu-id="958cd-2503">Sorgu ifadesi, başka hiçbir indirimleri mümkün olana kadar sürekli olarak aşağıdaki çevirileri uygulayarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2503">A query expression is processed by repeatedly applying the following translations until no further reductions are possible.</span></span> <span data-ttu-id="958cd-2504">Çevirileri uygulama sırasına göre listelenir: her bölüm önceki bölümlerde yer çevirileri ayrıntısına gerçekleştirilmiş ve tükenmiş sonra bir bölüm daha sonra aynı sorgu ifadesinde işleme tekrar ziyaret değil varsayar.</span><span class="sxs-lookup"><span data-stu-id="958cd-2504">The translations are listed in order of application: each section assumes that the translations in the preceding sections have been performed exhaustively, and once exhausted, a section will not later be revisited in the processing of the same query expression.</span></span>

<span data-ttu-id="958cd-2505">Sorgu ifadelerinde aralık değişkenleri atamaya izin verilmiyor.</span><span class="sxs-lookup"><span data-stu-id="958cd-2505">Assignment to range variables is not allowed in query expressions.</span></span> <span data-ttu-id="958cd-2506">Ancak bir C# uygulaması Bu bazen Burada sunulan söz dizimsel çeviri şemasıyla mümkün olmayabilir olduğundan bu kısıtlama, her zaman zorlamak için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2506">However a C# implementation is permitted to not always enforce this restriction, since this may sometimes not be possible with the syntactic translation scheme presented here.</span></span>

<span data-ttu-id="958cd-2507">Belirli çevirileri çağrılarınızla saydam tanımlayıcılarına sahip aralık değişkenleri ekleme `*`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2507">Certain translations inject range variables with transparent identifiers denoted by `*`.</span></span> <span data-ttu-id="958cd-2508">Saydam tanımlayıcı özel özelliklerini ele alınmıştır daha ayrıntılı olarak [saydam tanımlayıcıları](expressions.md#transparent-identifiers).</span><span class="sxs-lookup"><span data-stu-id="958cd-2508">The special properties of transparent identifiers are discussed further in [Transparent identifiers](expressions.md#transparent-identifiers).</span></span>

#### <a name="select-and-groupby-clauses-with-continuations"></a><span data-ttu-id="958cd-2509">Devamlılıklarla seçin ve groupby yan tümceleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2509">Select and groupby clauses with continuations</span></span>

<span data-ttu-id="958cd-2510">Devamlılık sahip bir sorgu ifadesi</span><span class="sxs-lookup"><span data-stu-id="958cd-2510">A query expression with a continuation</span></span>
```csharp
from ... into x ...
```
<span data-ttu-id="958cd-2511">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2511">is translated into</span></span>
```csharp
from x in ( from ... ) ...
```

<span data-ttu-id="958cd-2512">Aşağıdaki bölümlerde çevirileri sorguları Hayır sahip olduğunuzu varsaymaktadır `into` devamlılıkları.</span><span class="sxs-lookup"><span data-stu-id="958cd-2512">The translations in the following sections assume that queries have no `into` continuations.</span></span>

<span data-ttu-id="958cd-2513">Örnek</span><span class="sxs-lookup"><span data-stu-id="958cd-2513">The example</span></span>
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="958cd-2514">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2514">is translated into</span></span>
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="958cd-2515">hangi son çevrilmesidir</span><span class="sxs-lookup"><span data-stu-id="958cd-2515">the final translation of which is</span></span>
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a><span data-ttu-id="958cd-2516">Açık aralığı değişken türleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2516">Explicit range variable types</span></span>

<span data-ttu-id="958cd-2517">A `from` aralığı değişken türünü açıkça belirten yan tümcesi</span><span class="sxs-lookup"><span data-stu-id="958cd-2517">A `from` clause that explicitly specifies a range variable type</span></span>
```csharp
from T x in e
```
<span data-ttu-id="958cd-2518">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2518">is translated into</span></span>
```csharp
from x in ( e ) . Cast < T > ( )
```

<span data-ttu-id="958cd-2519">A `join` aralığı değişken türünü açıkça belirten yan tümcesi</span><span class="sxs-lookup"><span data-stu-id="958cd-2519">A `join` clause that explicitly specifies a range variable type</span></span>
```
join T x in e on k1 equals k2
```
<span data-ttu-id="958cd-2520">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2520">is translated into</span></span>
```
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

<span data-ttu-id="958cd-2521">Aşağıdaki bölümlerde çevirileri sorguları hiçbir açık aralığı değişken türleri olduğunu varsayın.</span><span class="sxs-lookup"><span data-stu-id="958cd-2521">The translations in the following sections assume that queries have no explicit range variable types.</span></span>

<span data-ttu-id="958cd-2522">Örnek</span><span class="sxs-lookup"><span data-stu-id="958cd-2522">The example</span></span>
```csharp
from Customer c in customers
where c.City == "London"
select c
```
<span data-ttu-id="958cd-2523">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2523">is translated into</span></span>
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
<span data-ttu-id="958cd-2524">hangi son çevrilmesidir</span><span class="sxs-lookup"><span data-stu-id="958cd-2524">the final translation of which is</span></span>
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

<span data-ttu-id="958cd-2525">Açık aralığı değişken türleri: genel olmayan uygulama koleksiyonları sorgulamak için kullanışlı `IEnumerable` arabirimi, ancak genel `IEnumerable<T>` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="958cd-2525">Explicit range variable types are useful for querying collections that implement the non-generic `IEnumerable` interface, but not the generic `IEnumerable<T>` interface.</span></span> <span data-ttu-id="958cd-2526">Yukarıdaki örnekte, bu durum geçerlidir olacaktır `customers` türü olan `ArrayList`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2526">In the example above, this would be the case if `customers` were of type `ArrayList`.</span></span>

#### <a name="degenerate-query-expressions"></a><span data-ttu-id="958cd-2527">Bozuk sorgu ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2527">Degenerate query expressions</span></span>

<span data-ttu-id="958cd-2528">Formun bir sorgu ifadesi</span><span class="sxs-lookup"><span data-stu-id="958cd-2528">A query expression of the form</span></span>
```csharp
from x in e select x
```
<span data-ttu-id="958cd-2529">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2529">is translated into</span></span>
```csharp
( e ) . Select ( x => x )
```

<span data-ttu-id="958cd-2530">Örnek</span><span class="sxs-lookup"><span data-stu-id="958cd-2530">The example</span></span>
```csharp
from c in customers
select c
```
<span data-ttu-id="958cd-2531">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2531">is translated into</span></span>
```csharp
customers.Select(c => c)
```

<span data-ttu-id="958cd-2532">Bozuk bir sorgu ifadesinin artık önemsiz olarak kaynak öğelerini seçer biridir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2532">A degenerate query expression is one that trivially selects the elements of the source.</span></span> <span data-ttu-id="958cd-2533">Bir sonraki çeviri aşaması, kaynak ile değiştirerek diğer çeviri adımları tarafından sunulan bozuk sorguları kaldırır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2533">A later phase of the translation removes degenerate queries introduced by other translation steps by replacing them with their source.</span></span> <span data-ttu-id="958cd-2534">Bir sorgunun sonucu emin olmak için ifade hiçbir zaman kaynak nesnenin kendisi, ancak, kaynak kimliğini ve türünü sorgu istemciye açığa gibi önemlidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2534">It is important however to ensure that the result of a query expression is never the source object itself, as that would reveal the type and identity of the source to the client of the query.</span></span> <span data-ttu-id="958cd-2535">Bu nedenle bu adımı açıkça çağırarak doğrudan kaynak kodda yazılmış bozuk sorguları korur `Select` kaynak.</span><span class="sxs-lookup"><span data-stu-id="958cd-2535">Therefore this step protects degenerate queries written directly in source code by explicitly calling `Select` on the source.</span></span> <span data-ttu-id="958cd-2536">Ardından uygulayıcılar kadar olan `Select` ve bu yöntemlerden hiç kaynak nesne döndürdüğünden emin olmak için diğer sorgu işleçleri.</span><span class="sxs-lookup"><span data-stu-id="958cd-2536">It is then up to the implementers of `Select` and other query operators to ensure that these methods never return the source object itself.</span></span>

#### <a name="from-let-where-join-and-orderby-clauses"></a><span data-ttu-id="958cd-2537">From, where, birleştirme ve orderby yan tümceleri sağlar</span><span class="sxs-lookup"><span data-stu-id="958cd-2537">From, let, where, join and orderby clauses</span></span>

<span data-ttu-id="958cd-2538">İkinci sorgu ifadesiyle `from` yan tümcesi tarafından izlenen bir `select` yan tümcesi</span><span class="sxs-lookup"><span data-stu-id="958cd-2538">A query expression with a second `from` clause followed by a `select` clause</span></span>
```csharp
from x1 in e1
from x2 in e2
select v
```
<span data-ttu-id="958cd-2539">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2539">is translated into</span></span>
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="958cd-2540">İkinci sorgu ifadesiyle `from` yan tümcesi izleyen bir şey tarafından dışındaki bir `select` yan tümcesi:</span><span class="sxs-lookup"><span data-stu-id="958cd-2540">A query expression with a second `from` clause followed by something other than a `select` clause:</span></span>

```csharp
from x1 in e1
from x2 in e2
...
```
<span data-ttu-id="958cd-2541">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2541">is translated into</span></span>
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

<span data-ttu-id="958cd-2542">Bir sorgu ifadesini içeren bir `let` yan tümcesi</span><span class="sxs-lookup"><span data-stu-id="958cd-2542">A query expression with a `let` clause</span></span>
```csharp
from x in e
let y = f
...
```
<span data-ttu-id="958cd-2543">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2543">is translated into</span></span>
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

<span data-ttu-id="958cd-2544">Bir sorgu ifadesini içeren bir `where` yan tümcesi</span><span class="sxs-lookup"><span data-stu-id="958cd-2544">A query expression with a `where` clause</span></span>
```csharp
from x in e
where f
...
```
<span data-ttu-id="958cd-2545">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2545">is translated into</span></span>
```csharp
from x in ( e ) . Where ( x => f )
...
```

<span data-ttu-id="958cd-2546">Bir sorgu ifadesini içeren bir `join` yan tümcesi olmadan bir `into` arkasından bir `select` yan tümcesi</span><span class="sxs-lookup"><span data-stu-id="958cd-2546">A query expression with a `join` clause without an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
<span data-ttu-id="958cd-2547">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2547">is translated into</span></span>
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="958cd-2548">Bir sorgu ifadesini içeren bir `join` yan tümcesi olmadan bir `into` dışında bir şey tarafından izlenen bir `select` yan tümcesi</span><span class="sxs-lookup"><span data-stu-id="958cd-2548">A query expression with a `join` clause without an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
<span data-ttu-id="958cd-2549">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2549">is translated into</span></span>
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

<span data-ttu-id="958cd-2550">Bir sorgu ifadesi ile bir `join` yan tümcesiyle birlikte bir `into` arkasından bir `select` yan tümcesi</span><span class="sxs-lookup"><span data-stu-id="958cd-2550">A query expression with a `join` clause with an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
<span data-ttu-id="958cd-2551">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2551">is translated into</span></span>
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

<span data-ttu-id="958cd-2552">Bir sorgu ifadesini içeren bir `join` yan tümcesiyle birlikte bir `into` dışında bir şey tarafından izlenen bir `select` yan tümcesi</span><span class="sxs-lookup"><span data-stu-id="958cd-2552">A query expression with a `join` clause with an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
<span data-ttu-id="958cd-2553">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2553">is translated into</span></span>
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

<span data-ttu-id="958cd-2554">Bir sorgu ifadesini içeren bir `orderby` yan tümcesi</span><span class="sxs-lookup"><span data-stu-id="958cd-2554">A query expression with an `orderby` clause</span></span>
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
<span data-ttu-id="958cd-2555">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2555">is translated into</span></span>
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

<span data-ttu-id="958cd-2556">Sıralama yan tümcesi belirtir bir `descending` yönü gösterge, çağrısından `OrderByDescending` veya `ThenByDescending` yerine üretilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2556">If an ordering clause specifies a `descending` direction indicator, an invocation of `OrderByDescending` or `ThenByDescending` is produced instead.</span></span>

<span data-ttu-id="958cd-2557">Aşağıdaki çevirileri olduğunu varsayın hiçbir `let`, `where`, `join` veya `orderby` yan tümceleri ve en fazla bir ilk `from` yan tümcesinde her sorgu ifadesi.</span><span class="sxs-lookup"><span data-stu-id="958cd-2557">The following translations assume that there are no `let`, `where`, `join` or `orderby` clauses, and no more than the one initial `from` clause in each query expression.</span></span>

<span data-ttu-id="958cd-2558">Örnek</span><span class="sxs-lookup"><span data-stu-id="958cd-2558">The example</span></span>
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="958cd-2559">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2559">is translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

<span data-ttu-id="958cd-2560">Örnek</span><span class="sxs-lookup"><span data-stu-id="958cd-2560">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="958cd-2561">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2561">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="958cd-2562">hangi son çevrilmesidir</span><span class="sxs-lookup"><span data-stu-id="958cd-2562">the final translation of which is</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
<span data-ttu-id="958cd-2563">Burada `x` yoksa görünmez ve erişilemez bir derleyicinin ürettiği tanımlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2563">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="958cd-2564">Örnek</span><span class="sxs-lookup"><span data-stu-id="958cd-2564">The example</span></span>
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
<span data-ttu-id="958cd-2565">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2565">is translated into</span></span>
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
<span data-ttu-id="958cd-2566">hangi son çevrilmesidir</span><span class="sxs-lookup"><span data-stu-id="958cd-2566">the final translation of which is</span></span>
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
<span data-ttu-id="958cd-2567">Burada `x` yoksa görünmez ve erişilemez bir derleyicinin ürettiği tanımlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2567">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="958cd-2568">Örnek</span><span class="sxs-lookup"><span data-stu-id="958cd-2568">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
<span data-ttu-id="958cd-2569">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2569">is translated into</span></span>
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

<span data-ttu-id="958cd-2570">Örnek</span><span class="sxs-lookup"><span data-stu-id="958cd-2570">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="958cd-2571">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2571">is translated into</span></span>
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="958cd-2572">hangi son çevrilmesidir</span><span class="sxs-lookup"><span data-stu-id="958cd-2572">the final translation of which is</span></span>
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
<span data-ttu-id="958cd-2573">Burada `x` ve `y` aksi görünmez ve erişilemez derleyicinin ürettiği tanıtıcılardır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2573">where `x` and `y` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="958cd-2574">Örnek</span><span class="sxs-lookup"><span data-stu-id="958cd-2574">The example</span></span>
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
<span data-ttu-id="958cd-2575">Son çeviri içeriyor</span><span class="sxs-lookup"><span data-stu-id="958cd-2575">has the final translation</span></span>
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a><span data-ttu-id="958cd-2576">SELECT yan tümceleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2576">Select clauses</span></span>

<span data-ttu-id="958cd-2577">Formun bir sorgu ifadesi</span><span class="sxs-lookup"><span data-stu-id="958cd-2577">A query expression of the form</span></span>
```csharp
from x in e select v
```
<span data-ttu-id="958cd-2578">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2578">is translated into</span></span>
```csharp
( e ) . Select ( x => v )
```
<span data-ttu-id="958cd-2579">v tanımlayıcı olduğunda dışında x, çeviri, yalnızca</span><span class="sxs-lookup"><span data-stu-id="958cd-2579">except when v is the identifier x, the translation is simply</span></span>
```csharp
( e )
```

<span data-ttu-id="958cd-2580">Örneğin</span><span class="sxs-lookup"><span data-stu-id="958cd-2580">For example</span></span>
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
<span data-ttu-id="958cd-2581">yalnızca veri dönüştürülür</span><span class="sxs-lookup"><span data-stu-id="958cd-2581">is simply translated into</span></span>
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a><span data-ttu-id="958cd-2582">Groupby yan tümceleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2582">Groupby clauses</span></span>

<span data-ttu-id="958cd-2583">Formun bir sorgu ifadesi</span><span class="sxs-lookup"><span data-stu-id="958cd-2583">A query expression of the form</span></span>
```csharp
from x in e group v by k
```
<span data-ttu-id="958cd-2584">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2584">is translated into</span></span>
```csharp
( e ) . GroupBy ( x => k , x => v )
```
<span data-ttu-id="958cd-2585">v tanımlayıcı olduğunda dışında x, çeviri,</span><span class="sxs-lookup"><span data-stu-id="958cd-2585">except when v is the identifier x, the translation is</span></span>
```csharp
( e ) . GroupBy ( x => k )
```

<span data-ttu-id="958cd-2586">Örnek</span><span class="sxs-lookup"><span data-stu-id="958cd-2586">The example</span></span>
```csharp
from c in customers
group c.Name by c.Country
```
<span data-ttu-id="958cd-2587">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2587">is translated into</span></span>
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a><span data-ttu-id="958cd-2588">Saydam tanımlayıcı</span><span class="sxs-lookup"><span data-stu-id="958cd-2588">Transparent identifiers</span></span>

<span data-ttu-id="958cd-2589">Aralık değişkenleri ile belirli çevirileri ekleme ***saydam tanımlayıcıları*** çağrılarınızla `*`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2589">Certain translations inject range variables with ***transparent identifiers*** denoted by `*`.</span></span> <span data-ttu-id="958cd-2590">Saydam tanımlayıcı uygun dil özelliği değildir; yalnızca sorgu ifade çevirisi sürecinde ara adım olarak mevcut.</span><span class="sxs-lookup"><span data-stu-id="958cd-2590">Transparent identifiers are not a proper language feature; they exist only as an intermediate step in the query expression translation process.</span></span>

<span data-ttu-id="958cd-2591">Daha fazla sorgu çevirisi saydam tanımlayıcı yerleştirir, çeviri adımları saydam tanımlayıcı anonim işlevler ve anonim nesne başlatıcıları yay.</span><span class="sxs-lookup"><span data-stu-id="958cd-2591">When a query translation injects a transparent identifier, further translation steps propagate the transparent identifier into anonymous functions and anonymous object initializers.</span></span> <span data-ttu-id="958cd-2592">Bu bağlamda, aşağıdaki davranış saydam tanımlayıcıları vardır:</span><span class="sxs-lookup"><span data-stu-id="958cd-2592">In those contexts, transparent identifiers have the following behavior:</span></span>

*  <span data-ttu-id="958cd-2593">Saydam tanımlayıcı bir anonim işlev bir parametre olarak ortaya çıktığında, ilişkili anonim tür otomatik olarak anonim işlev gövdesinde kapsamda üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2593">When a transparent identifier occurs as a parameter in an anonymous function, the members of the associated anonymous type are automatically in scope in the body of the anonymous function.</span></span>
*  <span data-ttu-id="958cd-2594">Saydam tanımlayıcı üyesi kapsamında olduğunda, bu üyenin de kapsamda üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2594">When a member with a transparent identifier is in scope, the members of that member are in scope as well.</span></span>
*  <span data-ttu-id="958cd-2595">Saydam tanımlayıcı olarak bir üye bildirimcisi anonim nesne başlatıcı içinde ortaya çıktığında, bir saydam tanımlayıcı üyesi tanıtır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2595">When a transparent identifier occurs as a member declarator in an anonymous object initializer, it introduces a member with a transparent identifier.</span></span>
*  <span data-ttu-id="958cd-2596">Yukarıda açıklanan çeviri adımlarda saydam tanımlayıcılar her zaman tek bir nesnenin bir üyesi olarak birden çok aralık değişkenleri yakalama amacıyla anonim türleri ile birlikte sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2596">In the translation steps described above, transparent identifiers are always introduced together with anonymous types, with the intent of capturing multiple range variables as members of a single object.</span></span> <span data-ttu-id="958cd-2597">C# uygulaması birden çok aralık değişkenleri gruplamak için anonim türler değerinden farklı bir mekanizma kullanmak için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2597">An implementation of C# is permitted to use a different mechanism than anonymous types to group together multiple range variables.</span></span> <span data-ttu-id="958cd-2598">Anonim türleri kullanılır ve saydam tanımlayıcıları Göster aşağıdaki çeviri örnekler varsayılır hemen çevrilebilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2598">The following translation examples assume that anonymous types are used, and show how transparent identifiers can be translated away.</span></span>

<span data-ttu-id="958cd-2599">Örnek</span><span class="sxs-lookup"><span data-stu-id="958cd-2599">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
<span data-ttu-id="958cd-2600">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2600">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

<span data-ttu-id="958cd-2601">Daha fazla olduğu çevrilir</span><span class="sxs-lookup"><span data-stu-id="958cd-2601">which is further translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
<span data-ttu-id="958cd-2602">Saydam tanımlayıcı silinmesi, hangi eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="958cd-2602">which, when transparent identifiers are erased, is equivalent to</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
<span data-ttu-id="958cd-2603">Burada `x` yoksa görünmez ve erişilemez bir derleyicinin ürettiği tanımlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2603">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="958cd-2604">Örnek</span><span class="sxs-lookup"><span data-stu-id="958cd-2604">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="958cd-2605">erişimcisine</span><span class="sxs-lookup"><span data-stu-id="958cd-2605">is translated into</span></span>
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="958cd-2606">Daha fazla olduğu için azaltıldı</span><span class="sxs-lookup"><span data-stu-id="958cd-2606">which is further reduced to</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
<span data-ttu-id="958cd-2607">hangi son çevrilmesidir</span><span class="sxs-lookup"><span data-stu-id="958cd-2607">the final translation of which is</span></span>
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
<span data-ttu-id="958cd-2608">Burada `x`, `y`, ve `z` aksi görünmez ve erişilemez derleyicinin ürettiği tanıtıcılardır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2608">where `x`, `y`, and `z` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

### <a name="the-query-expression-pattern"></a><span data-ttu-id="958cd-2609">Sorgu ifade deseni</span><span class="sxs-lookup"><span data-stu-id="958cd-2609">The query expression pattern</span></span>

<span data-ttu-id="958cd-2610">***Sorgu ifade deseninin*** türleri sorgu ifadeleri desteklemek için uygulayabileceğiniz yöntemler desenini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2610">The ***Query expression pattern*** establishes a pattern of methods that types can implement to support query expressions.</span></span> <span data-ttu-id="958cd-2611">Sorgu ifadeleri yöntem çağrıları için söz dizimi bir eşleme yoluyla çevrilir için sorgu ifade deseninin nasıl uyguladıkları önemli ölçüde esneklik türleri sahiptir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2611">Because query expressions are translated to method invocations by means of a syntactic mapping, types have considerable flexibility in how they implement the query expression pattern.</span></span> <span data-ttu-id="958cd-2612">Örneğin, düzeni yöntemlerini örnek yöntemlerine veya genişletme yöntemleri olarak çağırma sözdiziminde iki sahip, ve anonim işlevler hem de dönüştürülebilir olduğundan yöntemleri temsilci veya ifade ağaçları isteyebilir uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2612">For example, the methods of the pattern can be implemented as instance methods or as extension methods because the two have the same invocation syntax, and the methods can request delegates or expression trees because anonymous functions are convertible to both.</span></span>

<span data-ttu-id="958cd-2613">Önerilen genel bir tür şeklini `C<T>` desteklediği sorgu ifade deseninin aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2613">The recommended shape of a generic type `C<T>` that supports the query expression pattern is shown below.</span></span> <span data-ttu-id="958cd-2614">Genel tür parametre ve sonuç türleri arasındaki ilişkileri doğru şekilde göstermek için kullanılır, ancak genel olmayan türler deseni uygulamak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="958cd-2614">A generic type is used in order to illustrate the proper relationships between parameter and result types, but it is possible to implement the pattern for non-generic types as well.</span></span>

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

<span data-ttu-id="958cd-2615">Yukarıdaki yöntemleri Genel temsilci türleriyle kullanın `Func<T1,R>` ve `Func<T1,T2,R>`, ancak bunlar eşit derecede iyi diğer temsilci veya ifade ağacı türü aynı parametre ve sonuç türleri ilişkileri kullanmış.</span><span class="sxs-lookup"><span data-stu-id="958cd-2615">The methods above use the generic delegate types `Func<T1,R>` and `Func<T1,T2,R>`, but they could equally well have used other delegate or expression tree types with the same relationships in parameter and result types.</span></span>

<span data-ttu-id="958cd-2616">Önerilen ilişki arasındaki fark `C<T>` ve `O<T>` da sağlar `ThenBy` ve `ThenByDescending` yöntemleri sonucu üzerinde yalnızca bir `OrderBy` veya `OrderByDescending`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2616">Notice the recommended relationship between `C<T>` and `O<T>` which ensures that the `ThenBy` and `ThenByDescending` methods are available only on the result of an `OrderBy` or `OrderByDescending`.</span></span> <span data-ttu-id="958cd-2617">Ayrıca sonucunu önerilen şeklini fark `GroupBy` --sıralarının, her iç sıra sahip olduğu ek `Key` özelliği.</span><span class="sxs-lookup"><span data-stu-id="958cd-2617">Also notice the recommended shape of the result of `GroupBy` -- a sequence of sequences, where each inner sequence has an additional `Key` property.</span></span>

<span data-ttu-id="958cd-2618">`System.Linq` Uygulayan herhangi bir türü için ad alanı sağlar sorgu işleci deseninin bir uygulaması `System.Collections.Generic.IEnumerable<T>` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="958cd-2618">The `System.Linq` namespace provides an implementation of the query operator pattern for any type that implements the `System.Collections.Generic.IEnumerable<T>` interface.</span></span>

## <a name="assignment-operators"></a><span data-ttu-id="958cd-2619">Atama işleçleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2619">Assignment operators</span></span>

<span data-ttu-id="958cd-2620">Atama İşleçleri, yeni bir değer bir değişken, bir özellik, bir olay veya dizin oluşturucu öğenin atayın.</span><span class="sxs-lookup"><span data-stu-id="958cd-2620">The assignment operators assign a new value to a variable, a property, an event, or an indexer element.</span></span>

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

<span data-ttu-id="958cd-2621">Atamanın sol işleneni, bir değişken, bir özellik erişimi, bir dizin oluşturucu erişim veya bir olay erişimini sınıflandırılmış bir ifade olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2621">The left operand of an assignment must be an expression classified as a variable, a property access, an indexer access, or an event access.</span></span>

<span data-ttu-id="958cd-2622">`=` İşleci çağrıldığında ***basit atama işleci***.</span><span class="sxs-lookup"><span data-stu-id="958cd-2622">The `=` operator is called the ***simple assignment operator***.</span></span> <span data-ttu-id="958cd-2623">Sağ işleneninin değerini sol işlenen tarafından belirtilen değişken, özellik veya dizin oluşturucu öğenin atar.</span><span class="sxs-lookup"><span data-stu-id="958cd-2623">It assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="958cd-2624">Basit atama işlecinin sol işleneni, bir olay erişimini olmayabilir (açıklanan haller dışında [alan benzeri olaylara](classes.md#field-like-events)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2624">The left operand of the simple assignment operator may not be an event access (except as described in [Field-like events](classes.md#field-like-events)).</span></span> <span data-ttu-id="958cd-2625">Basit atama işleci açıklanan [basit atama](expressions.md#simple-assignment).</span><span class="sxs-lookup"><span data-stu-id="958cd-2625">The simple assignment operator is described in [Simple assignment](expressions.md#simple-assignment).</span></span>

<span data-ttu-id="958cd-2626">Atama işleçleri dışında `=` işleci çağrılır ***bileşik atama işleçleri***.</span><span class="sxs-lookup"><span data-stu-id="958cd-2626">The assignment operators other than the `=` operator are called the ***compound assignment operators***.</span></span> <span data-ttu-id="958cd-2627">Bu işleçler, iki işlenen üzerinde belirtilen işlemi gerçekleştirmek ve ardından sonuç değerini sol işlenen tarafından belirtilen değişken, özellik veya dizin oluşturucu öğenin atayın.</span><span class="sxs-lookup"><span data-stu-id="958cd-2627">These operators perform the indicated operation on the two operands, and then assign the resulting value to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="958cd-2628">Bileşik atama işleçleri açıklanan [bileşik atama](expressions.md#compound-assignment).</span><span class="sxs-lookup"><span data-stu-id="958cd-2628">The compound assignment operators are described in [Compound assignment](expressions.md#compound-assignment).</span></span>

<span data-ttu-id="958cd-2629">`+=` Ve `-=` işleçleri ile bir olay erişim ifadesi sol işlenen olarak adlandırılır *olay atama işleçleri*.</span><span class="sxs-lookup"><span data-stu-id="958cd-2629">The `+=` and `-=` operators with an event access expression as the left operand are called the *event assignment operators*.</span></span> <span data-ttu-id="958cd-2630">Diğer bir atama işleci sol işlenen olarak bir olay erişimini ile geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2630">No other assignment operator is valid with an event access as the left operand.</span></span> <span data-ttu-id="958cd-2631">Olay atama işleçleri açıklanan [olay ataması](expressions.md#event-assignment).</span><span class="sxs-lookup"><span data-stu-id="958cd-2631">The event assignment operators are described in [Event assignment](expressions.md#event-assignment).</span></span>

<span data-ttu-id="958cd-2632">Atama İşleçleri sağla ilişkilendirilebilir, işlemler soldan sağa gruplandırılır anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2632">The assignment operators are right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="958cd-2633">Örneğin, bir ifade formun `a = b = c` değerlendirmesinde `a = (b = c)`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2633">For example, an expression of the form `a = b = c` is evaluated as `a = (b = c)`.</span></span>

### <a name="simple-assignment"></a><span data-ttu-id="958cd-2634">Basit atama</span><span class="sxs-lookup"><span data-stu-id="958cd-2634">Simple assignment</span></span>

<span data-ttu-id="958cd-2635">`=` İşleci basit atama işleci çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2635">The `=` operator is called the simple assignment operator.</span></span>

<span data-ttu-id="958cd-2636">Sol işleneni, bir basit atama biçiminde olup olmadığını `E.P` veya `E[Ei]` burada `E` derleme zamanı türü `dynamic`, atama dinamik olarak bağlı sonra ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2636">If the left operand of a simple assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="958cd-2637">Bu durumda derleme zamanı Atama ifadesinin türüdür `dynamic`, ve çalışma zamanında çalışma zamanı türüne göre aşağıda açıklanan çözümleme gerçekleşecek `E`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2637">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="958cd-2638">Bir basit atama sağ işlenen, sol işlenen türüne örtük olarak dönüştürülebilir bir ifade olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2638">In a simple assignment, the right operand must be an expression that is implicitly convertible to the type of the left operand.</span></span> <span data-ttu-id="958cd-2639">İşlemi sağ işleneninin değerini sol işlenen tarafından belirtilen değişken, özellik veya dizin oluşturucu öğenin atar.</span><span class="sxs-lookup"><span data-stu-id="958cd-2639">The operation assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span>

<span data-ttu-id="958cd-2640">Basit atama ifadesi sol işlenen için atanan değer sonucudur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2640">The result of a simple assignment expression is the value assigned to the left operand.</span></span> <span data-ttu-id="958cd-2641">Sonuç sol işlenenle aynı türde ve her zaman bir değer olarak sınıflandırılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2641">The result has the same type as the left operand and is always classified as a value.</span></span>

<span data-ttu-id="958cd-2642">Sol işlenen bir özellik veya dizin oluşturucu erişimi varsa özelliği veya dizin oluşturucu olmalıdır bir `set` erişimcisi.</span><span class="sxs-lookup"><span data-stu-id="958cd-2642">If the left operand is a property or indexer access, the property or indexer must have a `set` accessor.</span></span> <span data-ttu-id="958cd-2643">Durum bu değilse, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2643">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="958cd-2644">Bir basit atama formun çalışma zamanı işlenmesini `x = y` aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="958cd-2644">The run-time processing of a simple assignment of the form `x = y` consists of the following steps:</span></span>

*  <span data-ttu-id="958cd-2645">Varsa `x` bir değişken olarak sınıflandırıldığını:</span><span class="sxs-lookup"><span data-stu-id="958cd-2645">If `x` is classified as a variable:</span></span>
   * <span data-ttu-id="958cd-2646">`x` değişkeni oluşturmak için değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2646">`x` is evaluated to produce the variable.</span></span>
   * <span data-ttu-id="958cd-2647">`y` değerlendirilir ve, gerekirse türüne dönüştürülmüş `x` aracılığıyla örtük bir dönüştürme ([örtük dönüştürmelerin](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2647">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="958cd-2648">Varsa tarafından verilen değişkeni `x` bir dizi öğesinin bir *reference_type*, için hesaplanan değer sağlamak için bir çalışma zamanı denetimi gerçekleştirilir `y` biri dizi örneği ile uyumlu `x` olduğu bir öğe.</span><span class="sxs-lookup"><span data-stu-id="958cd-2648">If the variable given by `x` is an array element of a *reference_type*, a run-time check is performed to ensure that the value computed for `y` is compatible with the array instance of which `x` is an element.</span></span> <span data-ttu-id="958cd-2649">Varsa denetimi başarılı `y` olan `null`, ya da örtük bir başvuru dönüştürmesi, ([örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions)) gerçek tür tarafından başvurulan örnek var. `y` gerçek öğe türü dizi örneği içeren `x`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2649">The check succeeds if `y` is `null`, or if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the actual type of the instance referenced by `y` to the actual element type of the array instance containing `x`.</span></span> <span data-ttu-id="958cd-2650">Aksi takdirde, bir `System.ArrayTypeMismatchException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2650">Otherwise, a `System.ArrayTypeMismatchException` is thrown.</span></span>
   * <span data-ttu-id="958cd-2651">Dönüşümü ve değerlendirme elde edilen değer `y` değerlendirmesi tarafından verilen konuma depolanır `x`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2651">The value resulting from the evaluation and conversion of `y` is stored into the location given by the evaluation of `x`.</span></span>
*  <span data-ttu-id="958cd-2652">Varsa `x` bir özellik veya dizin oluşturucu erişim sınıflandırıldığını:</span><span class="sxs-lookup"><span data-stu-id="958cd-2652">If `x` is classified as a property or indexer access:</span></span>
   * <span data-ttu-id="958cd-2653">Örnek ifade (varsa `x` değil `static`) ve bağımsız değişken listesi (varsa `x` bir dizin oluşturucu erişim) ile ilişkili `x` değerlendirilir, ve sonuçları sonraki kullanılan `set` erişimci çağırma.</span><span class="sxs-lookup"><span data-stu-id="958cd-2653">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `set` accessor invocation.</span></span>
   * <span data-ttu-id="958cd-2654">`y` değerlendirilir ve, gerekirse türüne dönüştürülmüş `x` aracılığıyla örtük bir dönüştürme ([örtük dönüştürmelerin](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2654">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="958cd-2655">`set` Erişimcisine `x` için hesaplanan değer ile çağrılan `y` olarak kendi `value` bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="958cd-2655">The `set` accessor of `x` is invoked with the value computed for `y` as its `value` argument.</span></span>

<span data-ttu-id="958cd-2656">Dizi değişimini kuralları ([dizi kovaryansı](arrays.md#array-covariance)) bir dizi türünde bir değer izin `A[]` bir dizi türünde bir örneğe bir başvuru olarak `B[]`, gelen bir örtük bir başvuru dönüştürmesi var sağlanan `B` için `A`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2656">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="958cd-2657">Bu kurallar, bir dizi öğesine atama nedeniyle bir *reference_type* atanan değerin dizi örneği ile uyumlu olduğundan emin olmak için bir çalışma zamanı denetimi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2657">Because of these rules, assignment to an array element of a *reference_type* requires a run-time check to ensure that the value being assigned is compatible with the array instance.</span></span> <span data-ttu-id="958cd-2658">Örnekte</span><span class="sxs-lookup"><span data-stu-id="958cd-2658">In the example</span></span>
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
<span data-ttu-id="958cd-2659">Son ataması neden olan bir `System.ArrayTypeMismatchException` çünkü durum örneğini `ArrayList` bir öğesinde depolanan bir `string[]`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2659">the last assignment causes a `System.ArrayTypeMismatchException` to be thrown because an instance of `ArrayList` cannot be stored in an element of a `string[]`.</span></span>

<span data-ttu-id="958cd-2660">Bir özellik veya dizin oluşturucu ne zaman bildirilen bir *struct_type* örnek ifade atama işleminin hedefi, özellikle ilişkili veya dizin oluşturucu erişim bir değişken olarak sınıflandırılan gerekir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2660">When a property or indexer declared in a *struct_type* is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="958cd-2661">Örnek ifade bir değer olarak sınıflandırılmış bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2661">If the instance expression is classified as a value, a binding-time error occurs.</span></span> <span data-ttu-id="958cd-2662">Nedeniyle [üye erişimi](expressions.md#member-access), aynı kural alanları için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2662">Because of [Member access](expressions.md#member-access), the same rule also applies to fields.</span></span>

<span data-ttu-id="958cd-2663">Bildirimleri verildiğinde:</span><span class="sxs-lookup"><span data-stu-id="958cd-2663">Given the declarations:</span></span>
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
<span data-ttu-id="958cd-2664">Örnekte</span><span class="sxs-lookup"><span data-stu-id="958cd-2664">in the example</span></span>
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
<span data-ttu-id="958cd-2665">atamaları `p.X`, `p.Y`, `r.A`, ve `r.B` için izin verilen `p` ve `r` değişkenlerdir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2665">the assignments to `p.X`, `p.Y`, `r.A`, and `r.B` are permitted because `p` and `r` are variables.</span></span> <span data-ttu-id="958cd-2666">Ancak, örnekte</span><span class="sxs-lookup"><span data-stu-id="958cd-2666">However, in the example</span></span>
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
<span data-ttu-id="958cd-2667">atamaları itibaren tüm geçersiz `r.A` ve `r.B` değişkenleri değildir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2667">the assignments are all invalid, since `r.A` and `r.B` are not variables.</span></span>

### <a name="compound-assignment"></a><span data-ttu-id="958cd-2668">Bileşik atama</span><span class="sxs-lookup"><span data-stu-id="958cd-2668">Compound assignment</span></span>

<span data-ttu-id="958cd-2669">Bileşik atama sol işleneni biçiminde olup olmadığını `E.P` veya `E[Ei]` burada `E` derleme zamanı türü `dynamic`, atama dinamik olarak bağlı sonra ([dinamik bağlama](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2669">If the left operand of a compound assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="958cd-2670">Bu durumda derleme zamanı Atama ifadesinin türüdür `dynamic`, ve çalışma zamanında çalışma zamanı türüne göre aşağıda açıklanan çözümleme gerçekleşecek `E`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2670">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="958cd-2671">Formun bir işlem `x op= y` ikili işleci aşırı yükleme çözünürlüğü uygulama tarafından işlenen ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) işlemi olarak yazılmışsa `x op y`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2671">An operation of the form `x op= y` is processed by applying binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x op y`.</span></span> <span data-ttu-id="958cd-2672">Ardından,</span><span class="sxs-lookup"><span data-stu-id="958cd-2672">Then,</span></span>

*  <span data-ttu-id="958cd-2673">Seçili işlecinin dönüş türü türüne örtük olarak dönüştürülebilir ise `x`, işlemi olarak değerlendirilir `x = x op y`dışında `x` yalnızca bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2673">If the return type of the selected operator is implicitly convertible to the type of `x`, the operation is evaluated as `x = x op y`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="958cd-2674">Aksi takdirde seçilen işlecinin dönüş türü türüne açıkça dönüştürülebilir ise seçilen operatöre bir önceden tanımlanmış işleç ise `x`ve eğer `y` türüne açıkça dönüştürülemez `x` veya işlecin bir kaydırma işleci, sonra da işlemi olarak değerlendirilir `x = (T)(x op y)`burada `T` türü `x`dışında `x` yalnızca bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2674">Otherwise, if the selected operator is a predefined operator, if the return type of the selected operator is explicitly convertible to the type of `x`, and if `y` is implicitly convertible to the type of `x` or the operator is a shift operator, then the operation is evaluated as `x = (T)(x op y)`, where `T` is the type of `x`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="958cd-2675">Aksi takdirde, bileşik atama geçersiz ve bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2675">Otherwise, the compound assignment is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="958cd-2676">"Yalnızca bir kez değerlendirilir" terimi içinde değerlendirmesi anlamına `x op y`, bağlı tüm ifadelerin sonuçlarını `x` geçici olarak kaydedilir ve ardından atamaya gerçekleştirirken yeniden `x`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2676">The term "evaluated only once" means that in the evaluation of `x op y`, the results of any constituent expressions of `x` are temporarily saved and then reused when performing the assignment to `x`.</span></span> <span data-ttu-id="958cd-2677">Örneğin, atama, `A()[B()] += C()`burada `A` döndüren bir yöntem `int[]`, ve `B` ve `C` döndüren yöntemi `int`, yöntemleri, yalnızca bir kez sırayla çağrılır `A`, `B`, `C`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2677">For example, in the assignment `A()[B()] += C()`, where `A` is a method returning `int[]`, and `B` and `C` are methods returning `int`, the methods are invoked only once, in the order `A`, `B`, `C`.</span></span>

<span data-ttu-id="958cd-2678">Bileşik atama sol işleneni bir özellik erişimi ya da dizin oluşturucu erişimi olduğunda özellik veya dizin oluşturucu her ikisi de olmalıdır bir `get` erişimci ve `set` erişimcisi.</span><span class="sxs-lookup"><span data-stu-id="958cd-2678">When the left operand of a compound assignment is a property access or indexer access, the property or indexer must have both a `get` accessor and a `set` accessor.</span></span> <span data-ttu-id="958cd-2679">Durum bu değilse, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2679">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="958cd-2680">İkinci kuralı izinleri yukarıda `x op= y` olarak değerlendirilecek `x = (T)(x op y)` belirli bağlamlarda.</span><span class="sxs-lookup"><span data-stu-id="958cd-2680">The second rule above permits `x op= y` to be evaluated as `x = (T)(x op y)` in certain contexts.</span></span> <span data-ttu-id="958cd-2681">Sol işlenen türü olduğunda, önceden tanımlı operatörler bileşik işleçleri kullanılabilir olacağı şekilde kuralı mevcut `sbyte`, `byte`, `short`, `ushort`, veya `char`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2681">The rule exists such that the predefined operators can be used as compound operators when the left operand is of type `sbyte`, `byte`, `short`, `ushort`, or `char`.</span></span> <span data-ttu-id="958cd-2682">Ne zaman her iki değişken bu türlerin birini olsa bile, önceden tanımlı operatörler türünün bir sonucu üretmek `int`anlatılan şekilde [ikili sayısal promosyonlar](expressions.md#binary-numeric-promotions).</span><span class="sxs-lookup"><span data-stu-id="958cd-2682">Even when both arguments are of one of those types, the predefined operators produce a result of type `int`, as described in [Binary numeric promotions](expressions.md#binary-numeric-promotions).</span></span> <span data-ttu-id="958cd-2683">Bu nedenle, bir yayın olmadan sol işleneni sonucu atamak mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2683">Thus, without a cast it would not be possible to assign the result to the left operand.</span></span>

<span data-ttu-id="958cd-2684">Kural önceden tanımlı operatörler için sezgisel etkisini yalnızca olan `x op= y` hem de izin verilir, `x op y` ve `x = y` izin verilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2684">The intuitive effect of the rule for predefined operators is simply that `x op= y` is permitted if both of `x op y` and `x = y` are permitted.</span></span> <span data-ttu-id="958cd-2685">Örnekte</span><span class="sxs-lookup"><span data-stu-id="958cd-2685">In the example</span></span>
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
<span data-ttu-id="958cd-2686">Her hata için sezgisel nedeni, karşılık gelen bir basit atama bir hata de olabilirdi olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2686">the intuitive reason for each error is that a corresponding simple assignment would also have been an error.</span></span>

<span data-ttu-id="958cd-2687">Bu, ayrıca işlemlerini destekleyen bileşik atama işlemlerinin yükseltilmiş anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2687">This also means that compound assignment operations support lifted operations.</span></span> <span data-ttu-id="958cd-2688">Örnekte</span><span class="sxs-lookup"><span data-stu-id="958cd-2688">In the example</span></span>
```csharp
int? i = 0;
i += 1;             // Ok
```
<span data-ttu-id="958cd-2689">lifted işleci `+(int?,int?)` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2689">the lifted operator `+(int?,int?)` is used.</span></span>

### <a name="event-assignment"></a><span data-ttu-id="958cd-2690">Olay atama</span><span class="sxs-lookup"><span data-stu-id="958cd-2690">Event assignment</span></span>

<span data-ttu-id="958cd-2691">Sol işleneni, bir `+=` veya `-=` işleci bir olay erişimini sınıflandırılır ve ardından ifade gibi değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="958cd-2691">If the left operand of a `+=` or `-=` operator is classified as an event access, then the expression is evaluated as follows:</span></span>

*  <span data-ttu-id="958cd-2692">Örnek ifade varsa olay erişimini değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2692">The instance expression, if any, of the event access is evaluated.</span></span>
*  <span data-ttu-id="958cd-2693">Sağ işleneninin `+=` veya `-=` işleci değerlendirilir ve, gerekirse, sol işlenen aracılığıyla örtük bir dönüştürme türü dönüştürülür ([örtük dönüştürmelerin](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2693">The right operand of the `+=` or `-=` operator is evaluated, and, if required, converted to the type of the left operand through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
*  <span data-ttu-id="958cd-2694">Bir olayın olay erişimcisi sağ işleneni değerlendirmesinden sonra oluşan bağımsız değişken listesiyle çağrılır ve gerekirse, dönüştürme.</span><span class="sxs-lookup"><span data-stu-id="958cd-2694">An event accessor of the event is invoked, with argument list consisting of the right operand, after evaluation and, if necessary, conversion.</span></span> <span data-ttu-id="958cd-2695">İşleç olduysa `+=`, `add` erişimci çağrılır; işleci ise `-=`, `remove` erişimci çağrılır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2695">If the operator was `+=`, the `add` accessor is invoked; if the operator was `-=`, the `remove` accessor is invoked.</span></span>

<span data-ttu-id="958cd-2696">Bir olay atama ifadesi bir değer vermez.</span><span class="sxs-lookup"><span data-stu-id="958cd-2696">An event assignment expression does not yield a value.</span></span> <span data-ttu-id="958cd-2697">Bu nedenle, bir olay atama ifadesi yalnızca bağlamında geçerli bir *statement_expression* ([ifade deyimleri](statements.md#expression-statements)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2697">Thus, an event assignment expression is valid only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

## <a name="expression"></a><span data-ttu-id="958cd-2698">İfade</span><span class="sxs-lookup"><span data-stu-id="958cd-2698">Expression</span></span>

<span data-ttu-id="958cd-2699">Bir *ifade* ya da bir *non_assignment_expression* veya *atama*.</span><span class="sxs-lookup"><span data-stu-id="958cd-2699">An *expression* is either a *non_assignment_expression* or an *assignment*.</span></span>

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

## <a name="constant-expressions"></a><span data-ttu-id="958cd-2700">Sabit ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2700">Constant expressions</span></span>

<span data-ttu-id="958cd-2701">A *constant_expression* , derleme zamanında tam olarak değerlendirilebilecek bir ifadedir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2701">A *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span>

```antlr
constant_expression
    : expression
    ;
```

<span data-ttu-id="958cd-2702">Sabit ifade olmalıdır `null` değişmez değeri ya da bir değer şu türlerden biriyle: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`, `bool`, `object`, `string`, ya da herhangi bir numaralandırma türü.</span><span class="sxs-lookup"><span data-stu-id="958cd-2702">A constant expression must be the `null` literal or a value with one of  the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`, or any enumeration type.</span></span> <span data-ttu-id="958cd-2703">Yalnızca aşağıdaki yapılar, sabit ifadelerde izin verilir:</span><span class="sxs-lookup"><span data-stu-id="958cd-2703">Only the following constructs are permitted in constant expressions:</span></span>

*  <span data-ttu-id="958cd-2704">Değişmez değerler (dahil olmak üzere `null` değişmez değer).</span><span class="sxs-lookup"><span data-stu-id="958cd-2704">Literals (including the `null` literal).</span></span>
*  <span data-ttu-id="958cd-2705">Başvurular `const` üyeleri sınıf ile yapı türü.</span><span class="sxs-lookup"><span data-stu-id="958cd-2705">References to `const` members of class and struct types.</span></span>
*  <span data-ttu-id="958cd-2706">Numaralandırma türleri üyeleri için başvurular.</span><span class="sxs-lookup"><span data-stu-id="958cd-2706">References to members of enumeration types.</span></span>
*  <span data-ttu-id="958cd-2707">Başvurular `const` parametrelerin veya yerel değişkenler</span><span class="sxs-lookup"><span data-stu-id="958cd-2707">References to `const` parameters or local variables</span></span>
*  <span data-ttu-id="958cd-2708">Kendileri sabit ifadeler parantezli alt ifadeleri.</span><span class="sxs-lookup"><span data-stu-id="958cd-2708">Parenthesized sub-expressions, which are themselves constant expressions.</span></span>
*  <span data-ttu-id="958cd-2709">Cast ifadeleri, sağlanan hedef türü, yukarıda listelenen türler biridir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2709">Cast expressions, provided the target type is one of the types listed above.</span></span>
*  <span data-ttu-id="958cd-2710">`checked` ve `unchecked` ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2710">`checked` and `unchecked` expressions</span></span>
*  <span data-ttu-id="958cd-2711">Varsayılan değer ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2711">Default value expressions</span></span>
*  <span data-ttu-id="958cd-2712">Nameof ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2712">Nameof expressions</span></span>
*  <span data-ttu-id="958cd-2713">Önceden tanımlanmış `+`, `-`, `!`, ve `~` birli işleçler.</span><span class="sxs-lookup"><span data-stu-id="958cd-2713">The predefined `+`, `-`, `!`, and `~` unary operators.</span></span>
*  <span data-ttu-id="958cd-2714">Önceden tanımlanmış `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, ve `>=` ikili işleçler, sağlanan her işlenen yukarıda listelenen bir tür.</span><span class="sxs-lookup"><span data-stu-id="958cd-2714">The predefined `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, and `>=` binary operators, provided each operand is of a type listed above.</span></span>
*  <span data-ttu-id="958cd-2715">`?:` Koşullu işleç.</span><span class="sxs-lookup"><span data-stu-id="958cd-2715">The `?:` conditional operator.</span></span>

<span data-ttu-id="958cd-2716">Aşağıdaki dönüşümlerden sabit ifadelerde izin verilir:</span><span class="sxs-lookup"><span data-stu-id="958cd-2716">The following conversions are permitted in constant expressions:</span></span>

*  <span data-ttu-id="958cd-2717">Kimlik dönüştürme</span><span class="sxs-lookup"><span data-stu-id="958cd-2717">Identity conversions</span></span>
*  <span data-ttu-id="958cd-2718">Sayısal dönüşümler</span><span class="sxs-lookup"><span data-stu-id="958cd-2718">Numeric conversions</span></span>
*  <span data-ttu-id="958cd-2719">Sabit listesi dönüştürmeler</span><span class="sxs-lookup"><span data-stu-id="958cd-2719">Enumeration conversions</span></span>
*  <span data-ttu-id="958cd-2720">Sabit ifade dönüştürmeler</span><span class="sxs-lookup"><span data-stu-id="958cd-2720">Constant expression conversions</span></span>
*  <span data-ttu-id="958cd-2721">Örtük ve açık başvuru dönüşümleri, dönüştürmeler kaynağı null değer veren bir sabit ifade sağlanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2721">Implicit and explicit reference conversions, provided that the source of the conversions is a constant expression that evaluates to the null value.</span></span>

<span data-ttu-id="958cd-2722">Kutulama dahil olmak üzere diğer dönüştürme, kutudan çıkarma ve dolaylı başvuru dönüşümleri null olmayan değerlerin sabit ifadelerde izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="958cd-2722">Other conversions including boxing, unboxing and implicit reference conversions of non-null values are not permitted in constant expressions.</span></span> <span data-ttu-id="958cd-2723">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="958cd-2723">For example:</span></span>
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
<span data-ttu-id="958cd-2724">başlatma i olduğundan bir hata paketleme dönüştürmesi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2724">the initialization of i is an error because a boxing conversion is required.</span></span> <span data-ttu-id="958cd-2725">Bir null olmayan değer dosyasından bir örtük bir başvuru dönüştürmesi gerektiğinden str başlatma bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2725">The initialization of str is an error because an implicit reference conversion from a non-null value is required.</span></span>

<span data-ttu-id="958cd-2726">Bir ifade yukarıda listelenen gereksinimleri karşılayan her ifade derleme zamanında değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2726">Whenever an expression fulfills the requirements listed above, the expression is evaluated at compile-time.</span></span> <span data-ttu-id="958cd-2727">İfade sabit olmayan yapıları içeren daha büyük bir ifadenin bir alt ifade olsa bile bu geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2727">This is true even if the expression is a sub-expression of a larger expression that contains non-constant constructs.</span></span>

<span data-ttu-id="958cd-2728">Sabit ifadeler derleme zamanı değerlendirmesi bir derleme zamanı hatası oluşmasına neden olduğu çalışma zamanı değerlendirme atılmış bir özel durum, derleme zamanı değerlendirmesi dışında çalışma zamanı değerlendirme sabit olmayan ifade aynı kurallara kullanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2728">The compile-time evaluation of constant expressions uses the same rules as run-time evaluation of non-constant expressions, except that where run-time evaluation would have thrown an exception, compile-time evaluation causes a compile-time error to occur.</span></span>

<span data-ttu-id="958cd-2729">Sabit bir ifade açıkça yerleştirilir sürece bir `unchecked` bağlam, Tamsayı türünde aritmetik işlemler ve dönüştürmeler ifade her zaman derleme zamanı değerlendirmesi sırasında gerçekleşen taşıyor derleme zamanı hatalarına neden olabilir ([Sabit ifadeler](expressions.md#constant-expressions)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2729">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur in integral-type arithmetic operations and conversions during the compile-time evaluation of the expression always cause compile-time errors ([Constant expressions](expressions.md#constant-expressions)).</span></span>

<span data-ttu-id="958cd-2730">Sabit ifadeler, aşağıda listelenen bağlamlarda oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2730">Constant expressions occur in the contexts listed below.</span></span> <span data-ttu-id="958cd-2731">Bir ifadenin derleme zamanında tam olarak değerlendirilemezse şu bağlamlarda Bu, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2731">In these contexts, a compile-time error occurs if an expression cannot be fully evaluated at compile-time.</span></span>

*  <span data-ttu-id="958cd-2732">Sabit bildirimi ([sabitleri](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2732">Constant declarations ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="958cd-2733">Sabit listesi üye bildirimleri ([numaralandırma üyeleri](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2733">Enumeration member declarations ([Enum members](enums.md#enum-members)).</span></span>
*  <span data-ttu-id="958cd-2734">Varsayılan bağımsız değişkenler biçimsel parametre listesi ([yöntem parametreleri](classes.md#method-parameters))</span><span class="sxs-lookup"><span data-stu-id="958cd-2734">Default arguments of formal parameter lists ([Method parameters](classes.md#method-parameters))</span></span>
*  <span data-ttu-id="958cd-2735">`case` Etiketler, bir `switch` deyimi ([switch deyimi](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2735">`case` labels of a `switch` statement ([The switch statement](statements.md#the-switch-statement)).</span></span>
*  <span data-ttu-id="958cd-2736">`goto case` deyimler ([goto deyimi](statements.md#the-goto-statement)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2736">`goto case` statements ([The goto statement](statements.md#the-goto-statement)).</span></span>
*  <span data-ttu-id="958cd-2737">Bir dizi oluşturma ifadesi içinde uzunluklarını boyut ([dizi oluşturma ifadeleri](expressions.md#array-creation-expressions)), bir başlatıcı içerir.</span><span class="sxs-lookup"><span data-stu-id="958cd-2737">Dimension lengths in an array creation expression ([Array creation expressions](expressions.md#array-creation-expressions)) that includes an initializer.</span></span>
*  <span data-ttu-id="958cd-2738">Öznitelikler ([öznitelikleri](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="958cd-2738">Attributes ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="958cd-2739">Örtük sabit ifade dönüştürme ([örtük sabit ifade dönüştürmeler](conversions.md#implicit-constant-expression-conversions)) türünde sabit bir ifade verir `int` dönüştürülecek `sbyte`, `byte`, `short`, `ushort`, `uint`, veya `ulong`, sağlanan sabit ifadenin değeri hedef türün aralığı içinde.</span><span class="sxs-lookup"><span data-stu-id="958cd-2739">An implicit constant expression conversion ([Implicit constant expression conversions](conversions.md#implicit-constant-expression-conversions)) permits a constant expression of type `int` to be converted to `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the constant expression is within the range of the destination type.</span></span>

## <a name="boolean-expressions"></a><span data-ttu-id="958cd-2740">Boole ifadeleri</span><span class="sxs-lookup"><span data-stu-id="958cd-2740">Boolean expressions</span></span>

<span data-ttu-id="958cd-2741">A *boolean_expression* türünün bir sonucu veren ifade `bool`; ya da doğrudan veya uygulaması aracılığıyla `operator true` aşağıda belirtildiği gibi belirli bağlamlarda.</span><span class="sxs-lookup"><span data-stu-id="958cd-2741">A *boolean_expression* is an expression that yields a result of type `bool`; either directly or through application of `operator true` in certain contexts as specified in the following.</span></span>

```antlr
boolean_expression
    : expression
    ;
```

<span data-ttu-id="958cd-2742">Denetleyen koşullu ifadede bir *if_statement* ([if deyimi](statements.md#the-if-statement)), *while_statement* ([while ifadesi](statements.md#the-while-statement)), *do_statement* ([do deyimi](statements.md#the-do-statement)), veya *for_statement* ([deyimi için](statements.md#the-for-statement)) olan bir *boolean_ ifade*.</span><span class="sxs-lookup"><span data-stu-id="958cd-2742">The controlling conditional expression of an *if_statement* ([The if statement](statements.md#the-if-statement)), *while_statement* ([The while statement](statements.md#the-while-statement)), *do_statement* ([The do statement](statements.md#the-do-statement)), or *for_statement* ([The for statement](statements.md#the-for-statement)) is a *boolean_expression*.</span></span> <span data-ttu-id="958cd-2743">Denetleyen koşullu ifadede `?:` işleci ([koşullu işleç](expressions.md#conditional-operator)) olarak aynı kurallara bir *boolean_expression*, ancak işleci nedenlerle öncelik sınıflandırılır olarak bir *conditional_or_expression*.</span><span class="sxs-lookup"><span data-stu-id="958cd-2743">The controlling conditional expression of the `?:` operator ([Conditional operator](expressions.md#conditional-operator)) follows the same rules as a *boolean_expression*, but for reasons of operator precedence is classified as a *conditional_or_expression*.</span></span>

<span data-ttu-id="958cd-2744">A *boolean_expression* `E` türünde bir değer üretmek için gerekli `bool`gibi:</span><span class="sxs-lookup"><span data-stu-id="958cd-2744">A *boolean_expression* `E` is required to be able to produce a value of type `bool`, as follows:</span></span>

*  <span data-ttu-id="958cd-2745">Varsa `E` örtük olarak dönüştürülebilir `bool` çalışma zamanında bu örtülü bir dönüştürme uygulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2745">If `E` is implicitly convertible to `bool` then at runtime that implicit conversion is applied.</span></span>
*  <span data-ttu-id="958cd-2746">Aksi takdirde, birli işleç aşırı çözümleme ([birli işleç aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) işlecinin benzersiz bir en iyi uygulama bulmak için kullanılan `true` üzerinde `E`, ve bu uygulamayı çalışma zamanında uygulanır.</span><span class="sxs-lookup"><span data-stu-id="958cd-2746">Otherwise, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is used to find a unique best implementation of operator `true` on `E`, and that implementation is applied at runtime.</span></span>
*  <span data-ttu-id="958cd-2747">Böyle bir işleç bulunursa, bir bağlama zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="958cd-2747">If no such operator is found, a binding-time error occurs.</span></span>

<span data-ttu-id="958cd-2748">`DBBool` Yapı türünde [veritabanı Boole türü](structs.md#database-boolean-type) uygulayan bir tür bir örnek sağlar `operator true` ve `operator false`.</span><span class="sxs-lookup"><span data-stu-id="958cd-2748">The `DBBool` struct type in [Database boolean type](structs.md#database-boolean-type) provides an example of a type that implements `operator true` and `operator false`.</span></span>
