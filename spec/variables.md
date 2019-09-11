---
ms.openlocfilehash: a01cf9387b8dc47de036bf0bd1496c19a441d81c
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876803"
---
# <a name="variables"></a><span data-ttu-id="22df5-101">Değişkenler</span><span class="sxs-lookup"><span data-stu-id="22df5-101">Variables</span></span>

<span data-ttu-id="22df5-102">Değişkenler, depolama konumlarını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="22df5-102">Variables represent storage locations.</span></span> <span data-ttu-id="22df5-103">Her değişken, değişkende hangi değerlerin depolanabileceğini belirleyen bir tür içerir.</span><span class="sxs-lookup"><span data-stu-id="22df5-103">Every variable has a type that determines what values can be stored in the variable.</span></span> <span data-ttu-id="22df5-104">C#tür açısından güvenli bir dildir ve C# derleyici, değişkenler içinde depolanan değerlerin her zaman uygun türde olmasını garanti eder.</span><span class="sxs-lookup"><span data-stu-id="22df5-104">C# is a type-safe language, and the C# compiler guarantees that values stored in variables are always of the appropriate type.</span></span> <span data-ttu-id="22df5-105">Bir değişkenin değeri atama yoluyla veya `++` ve `--` işleçleri kullanılarak değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="22df5-105">The value of a variable can be changed through assignment or through use of the `++` and `--` operators.</span></span>

<span data-ttu-id="22df5-106">Değerin alınabilmesi için önce bir değişken kesin olarak ***atanmalıdır*** ([kesin atama](variables.md#definite-assignment)).</span><span class="sxs-lookup"><span data-stu-id="22df5-106">A variable must be ***definitely assigned*** ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained.</span></span>

<span data-ttu-id="22df5-107">Aşağıdaki bölümlerde açıklandığı gibi, değişkenler ***Başlangıçta atanır*** veya ***Başlangıçta atanmamıştır***.</span><span class="sxs-lookup"><span data-stu-id="22df5-107">As described in the following sections, variables are either ***initially assigned*** or ***initially unassigned***.</span></span> <span data-ttu-id="22df5-108">Başlangıçta atanan değişkenin iyi tanımlanmış bir başlangıç değeri vardır ve her zaman kesinlikle atanan olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="22df5-108">An initially assigned variable has a well-defined initial value and is always considered definitely assigned.</span></span> <span data-ttu-id="22df5-109">Başlangıçta atanmamış bir değişkenin ilk değeri yok.</span><span class="sxs-lookup"><span data-stu-id="22df5-109">An initially unassigned variable has no initial value.</span></span> <span data-ttu-id="22df5-110">Başlangıçta atanmamış bir değişkenin belirli bir konumda kesinlikle atanabileceği kabul edilmesi için, değişkene bir atama, bu konuma yönelik her olası yürütme yolunda gerçekleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="22df5-110">For an initially unassigned variable to be considered definitely assigned at a certain location, an assignment to the variable must occur in every possible execution path leading to that location.</span></span>

## <a name="variable-categories"></a><span data-ttu-id="22df5-111">Değişken kategorileri</span><span class="sxs-lookup"><span data-stu-id="22df5-111">Variable categories</span></span>

<span data-ttu-id="22df5-112">C#değişkenlerin yedi kategorisini tanımlar: statik değişkenler, örnek değişkenleri, dizi öğeleri, değer parametreleri, başvuru parametreleri, çıkış parametreleri ve yerel değişkenler.</span><span class="sxs-lookup"><span data-stu-id="22df5-112">C# defines seven categories of variables: static variables, instance variables, array elements, value parameters, reference parameters, output parameters, and local variables.</span></span> <span data-ttu-id="22df5-113">Aşağıdaki bölümler bu kategorilerin her birini anlatmaktadır.</span><span class="sxs-lookup"><span data-stu-id="22df5-113">The sections that follow describe each of these categories.</span></span>

<span data-ttu-id="22df5-114">Örnekte</span><span class="sxs-lookup"><span data-stu-id="22df5-114">In the example</span></span>
```csharp
class A
{
    public static int x;
    int y;

    void F(int[] v, int a, ref int b, out int c) {
        int i = 1;
        c = a + b++;
    }
}
```
<span data-ttu-id="22df5-115">`x``y` statik bir değişkendir, bir örnek `v[0]` değişkenidir, bir dizi öğesidir `a` , bir `c` değer parametresidir `b` , bir başvuru parametresidir, bir çıkış parametresidir ve `i` bir yerel değişkendir .</span><span class="sxs-lookup"><span data-stu-id="22df5-115">`x` is a static variable, `y` is an instance variable, `v[0]` is an array element, `a` is a value parameter, `b` is a reference parameter, `c` is an output parameter, and `i` is a local variable.</span></span>

### <a name="static-variables"></a><span data-ttu-id="22df5-116">Statik değişkenler</span><span class="sxs-lookup"><span data-stu-id="22df5-116">Static variables</span></span>

<span data-ttu-id="22df5-117">`static` Değiştiriciyle belirtilen bir alana ***statik değişken***denir.</span><span class="sxs-lookup"><span data-stu-id="22df5-117">A field declared with the `static` modifier is called a ***static variable***.</span></span> <span data-ttu-id="22df5-118">Statik bir değişken, kendi kapsayıcı türü için statik oluşturucunun ([statik oluşturucular](classes.md#static-constructors)) yürütülmesinden önce varlığına gelir ve ilişkili uygulama etki alanı mevcut olduğunda var olmaya erer.</span><span class="sxs-lookup"><span data-stu-id="22df5-118">A static variable comes into existence before execution of the static constructor ([Static constructors](classes.md#static-constructors)) for its containing type, and ceases to exist when the associated application domain ceases to exist.</span></span>

<span data-ttu-id="22df5-119">Statik bir değişkenin ilk değeri, değişkenin türünün varsayılan değeridir ([varsayılan değerlerdir](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="22df5-119">The initial value of a static variable is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="22df5-120">Kesin atama denetimi amacıyla, statik bir değişken başlangıçta atanan olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="22df5-120">For purposes of definite assignment checking, a static variable is considered initially assigned.</span></span>

### <a name="instance-variables"></a><span data-ttu-id="22df5-121">Örnek değişkenleri</span><span class="sxs-lookup"><span data-stu-id="22df5-121">Instance variables</span></span>

<span data-ttu-id="22df5-122">`static` Değiştirici olmadan tanımlanan bir alana ***örnek değişkeni***denir.</span><span class="sxs-lookup"><span data-stu-id="22df5-122">A field declared without the `static` modifier is called an ***instance variable***.</span></span>

#### <a name="instance-variables-in-classes"></a><span data-ttu-id="22df5-123">Sınıflarda örnek değişkenleri</span><span class="sxs-lookup"><span data-stu-id="22df5-123">Instance variables in classes</span></span>

<span data-ttu-id="22df5-124">Bir sınıfın örnek değişkeni, bu sınıfın yeni bir örneği oluşturulduğunda var olur ve bu örneğe bir başvuru olmadığında ve örneğin yok edicinin (varsa) yürütüldüğünden, var olmaya erer.</span><span class="sxs-lookup"><span data-stu-id="22df5-124">An instance variable of a class comes into existence when a new instance of that class is created, and ceases to exist when there are no references to that instance and the instance's destructor (if any) has executed.</span></span>

<span data-ttu-id="22df5-125">Bir sınıfın örnek değişkeninin ilk değeri, değişkenin türünün varsayılan değeridir ([varsayılan değerlerdir](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="22df5-125">The initial value of an instance variable of a class is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="22df5-126">Kesin atama denetimi amacıyla, bir sınıfın örnek değişkeni başlangıçta atanan olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="22df5-126">For the purpose of definite assignment checking, an instance variable of a class is considered initially assigned.</span></span>

#### <a name="instance-variables-in-structs"></a><span data-ttu-id="22df5-127">Yapılarda örnek değişkenleri</span><span class="sxs-lookup"><span data-stu-id="22df5-127">Instance variables in structs</span></span>

<span data-ttu-id="22df5-128">Bir yapının örnek değişkeni, ait olduğu yapı değişkeniyle tam olarak aynı yaşam süresine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="22df5-128">An instance variable of a struct has exactly the same lifetime as the struct variable to which it belongs.</span></span> <span data-ttu-id="22df5-129">Diğer bir deyişle, bir yapı türü değişkeni var olduğunda veya varsa, yapının örnek değişkenlerini çok da yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="22df5-129">In other words, when a variable of a struct type comes into existence or ceases to exist, so too do the instance variables of the struct.</span></span>

<span data-ttu-id="22df5-130">Bir yapının örnek değişkeninin ilk atama durumu, kapsayan yapı değişkeni ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-130">The initial assignment state of an instance variable of a struct is the same as that of the containing struct variable.</span></span> <span data-ttu-id="22df5-131">Diğer bir deyişle, bir struct değişkeni başlangıçta atanan olarak kabul edildiğinde, bu yüzden örnek değişkenleri çok fazla olur ve bir struct değişkeni başlangıçta atanmamış olarak kabul edildiğinde, örnek değişkenleri de aynı şekilde atanmamış olur.</span><span class="sxs-lookup"><span data-stu-id="22df5-131">In other words, when a struct variable is considered initially assigned, so too are its instance variables, and when a struct variable is considered initially unassigned, its instance variables are likewise unassigned.</span></span>

### <a name="array-elements"></a><span data-ttu-id="22df5-132">Dizi öğeleri</span><span class="sxs-lookup"><span data-stu-id="22df5-132">Array elements</span></span>

<span data-ttu-id="22df5-133">Bir dizinin öğeleri, bir dizi örneği oluşturulduğunda varlığına gelir ve bu dizi örneğine bir başvuru olmadığında var olmaya sona bırakılır.</span><span class="sxs-lookup"><span data-stu-id="22df5-133">The elements of an array come into existence when an array instance is created, and cease to exist when there are no references to that array instance.</span></span>

<span data-ttu-id="22df5-134">Bir dizinin öğelerinin ilk değeri, dizi öğelerinin türünün varsayılan değeridir ([varsayılan değerlerdir](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="22df5-134">The initial value of each of the elements of an array is the default value ([Default values](variables.md#default-values)) of the type of the array elements.</span></span>

<span data-ttu-id="22df5-135">Kesin atama denetimi amacıyla, bir dizi öğesi başlangıçta atanan olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="22df5-135">For the purpose of definite assignment checking, an array element is considered initially assigned.</span></span>

### <a name="value-parameters"></a><span data-ttu-id="22df5-136">Değer parametreleri</span><span class="sxs-lookup"><span data-stu-id="22df5-136">Value parameters</span></span>

<span data-ttu-id="22df5-137">`ref` Or`out` değiştiricisi olmadan belirtilen bir parametre bir ***değer parametresidir***.</span><span class="sxs-lookup"><span data-stu-id="22df5-137">A parameter declared without a `ref` or `out` modifier is a ***value parameter***.</span></span>

<span data-ttu-id="22df5-138">Bir değer parametresi, işlev üyesi (yöntem, örnek Oluşturucu, erişimci veya işleç) veya parametrenin ait olduğu anonim işlev çağrılandan sonra var olur ve bu, çağrısında verilen bağımsız değişkenin değeri ile başlatılır.</span><span class="sxs-lookup"><span data-stu-id="22df5-138">A value parameter comes into existence upon invocation of the function member (method, instance constructor, accessor, or operator) or anonymous function to which the parameter belongs, and is initialized with the value of the argument given in the invocation.</span></span> <span data-ttu-id="22df5-139">İşlev üyesi veya anonim işlev geri alındıktan sonra normalde bir değer parametresi yok olarak sona erer.</span><span class="sxs-lookup"><span data-stu-id="22df5-139">A value parameter normally ceases to exist upon return of the function member or anonymous function.</span></span> <span data-ttu-id="22df5-140">Ancak, değer parametresi anonim bir işlev ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) tarafından yakalandıysa, bu anonim işlevden oluşturulan temsilci veya ifade ağacı çöp toplama için uygun olana kadar en az bir değeri uzatır.</span><span class="sxs-lookup"><span data-stu-id="22df5-140">However, if the value parameter is captured by an anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), its life time extends at least until the delegate or expression tree created from that anonymous function is eligible for garbage collection.</span></span>

<span data-ttu-id="22df5-141">Kesin atama denetimi amacıyla bir değer parametresi başlangıçta atanan olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="22df5-141">For the purpose of definite assignment checking, a value parameter is considered initially assigned.</span></span>

### <a name="reference-parameters"></a><span data-ttu-id="22df5-142">Başvuru parametreleri</span><span class="sxs-lookup"><span data-stu-id="22df5-142">Reference parameters</span></span>

<span data-ttu-id="22df5-143">`ref` Değiştirici ile belirtilen bir parametre bir ***başvuru parametresidir***.</span><span class="sxs-lookup"><span data-stu-id="22df5-143">A parameter declared with a `ref` modifier is a ***reference parameter***.</span></span>

<span data-ttu-id="22df5-144">Başvuru parametresi yeni bir depolama konumu oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="22df5-144">A reference parameter does not create a new storage location.</span></span> <span data-ttu-id="22df5-145">Bunun yerine, bir başvuru parametresi, işlev üyesinde bağımsız değişken olarak verilen değişken ile aynı depolama konumunu temsil eder veya anonim işlev çağırma.</span><span class="sxs-lookup"><span data-stu-id="22df5-145">Instead, a reference parameter represents the same storage location as the variable given as the argument in the function member or anonymous function invocation.</span></span> <span data-ttu-id="22df5-146">Bu nedenle, başvuru parametresinin değeri her zaman temeldeki değişkenle aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-146">Thus, the value of a reference parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="22df5-147">Aşağıdaki kesin atama kuralları başvuru parametreleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="22df5-147">The following definite assignment rules apply to reference parameters.</span></span> <span data-ttu-id="22df5-148">Çıkış parametrelerinde açıklanan çıkış parametrelerine yönelik farklı kurallara göz önünde [edin.](variables.md#output-parameters)</span><span class="sxs-lookup"><span data-stu-id="22df5-148">Note the different rules for output parameters described in [Output parameters](variables.md#output-parameters).</span></span>

*  <span data-ttu-id="22df5-149">Bir değişken, bir işlev üyesine veya temsilci çağrısına başvuru parametresi olarak geçirilebilmesi için kesinlikle atanmalı ([kesin atama](variables.md#definite-assignment)).</span><span class="sxs-lookup"><span data-stu-id="22df5-149">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="22df5-150">Bir işlev üyesi veya anonim işlev içinde, başlangıçta atanan bir başvuru parametresi olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="22df5-150">Within a function member or anonymous function, a reference parameter is considered initially assigned.</span></span>

<span data-ttu-id="22df5-151">Bir yapı türünün örnek metodu veya örnek erişimcisi içinde, `this` anahtar sözcüğü tam olarak yapı türünün başvuru parametresi olarak davranır ([Bu erişim](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="22df5-151">Within an instance method or instance accessor of a struct type, the `this` keyword behaves exactly as a reference parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="output-parameters"></a><span data-ttu-id="22df5-152">Çıktı parametreleri</span><span class="sxs-lookup"><span data-stu-id="22df5-152">Output parameters</span></span>

<span data-ttu-id="22df5-153">`out` Değiştirici ile belirtilen bir parametre bir ***çıkış parametresidir***.</span><span class="sxs-lookup"><span data-stu-id="22df5-153">A parameter declared with an `out` modifier is an ***output parameter***.</span></span>

<span data-ttu-id="22df5-154">Çıkış parametresi yeni bir depolama konumu oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="22df5-154">An output parameter does not create a new storage location.</span></span> <span data-ttu-id="22df5-155">Bunun yerine, bir çıkış parametresi işlev üyesinde veya temsilci çağrısında bağımsız değişken olarak verilen değişkenle aynı depolama konumunu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="22df5-155">Instead, an output parameter represents the same storage location as the variable given as the argument in the function member or delegate invocation.</span></span> <span data-ttu-id="22df5-156">Bu nedenle, bir çıktı parametresinin değeri her zaman temeldeki değişkenle aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-156">Thus, the value of an output parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="22df5-157">Aşağıdaki kesin atama kuralları çıkış parametreleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="22df5-157">The following definite assignment rules apply to output parameters.</span></span> <span data-ttu-id="22df5-158">[Başvuru](variables.md#reference-parameters)parametrelerinde açıklanan başvuru parametrelerine yönelik farklı kurallara göz önünde edin.</span><span class="sxs-lookup"><span data-stu-id="22df5-158">Note the different rules for reference parameters described in [Reference parameters](variables.md#reference-parameters).</span></span>

*  <span data-ttu-id="22df5-159">Bir değişken, bir işlev üyesinde veya temsilci çağrısında çıkış parametresi olarak geçirilebilmesi için kesinlikle atanmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-159">A variable need not be definitely assigned before it can be passed as an output parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="22df5-160">Bir işlev üyesinin veya temsilci çağrısının normal tamamlanmasını takip eden bir çıktı parametresi olarak geçirilen her değişken, o yürütme yolunda atanmış olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="22df5-160">Following the normal completion of a function member or delegate invocation, each variable that was passed as an output parameter is considered assigned in that execution path.</span></span>
*  <span data-ttu-id="22df5-161">Bir işlev üyesi veya anonim işlev içinde, bir çıkış parametresi başlangıçta atanmamış olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="22df5-161">Within a function member or anonymous function, an output parameter is considered initially unassigned.</span></span>
*  <span data-ttu-id="22df5-162">İşlev üyesi veya anonim işlev normal bir şekilde döndürüldüğünden, bir işlev üyesinin veya anonim işlevin her çıkış parametresi kesinlikle atanmalıdır ([kesin atama](variables.md#definite-assignment)).</span><span class="sxs-lookup"><span data-stu-id="22df5-162">Every output parameter of a function member or anonymous function must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before the function member or anonymous function returns normally.</span></span>

<span data-ttu-id="22df5-163">Yapı türünün `this` örnek Oluşturucusu içinde anahtar sözcüğü, yapı türünün ([Bu erişim](expressions.md#this-access)) bir çıkış parametresi olarak tam olarak davranır.</span><span class="sxs-lookup"><span data-stu-id="22df5-163">Within an instance constructor of a struct type, the `this` keyword behaves exactly as an output parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="local-variables"></a><span data-ttu-id="22df5-164">Yerel değişkenler</span><span class="sxs-lookup"><span data-stu-id="22df5-164">Local variables</span></span>

<span data-ttu-id="22df5-165">***Yerel bir değişken*** bir *blok*, *for_statement*, *switch_statement* veya *using_statement*ile oluşabilen bir *local_variable_declaration*tarafından bildirilmiştir; ya da bir *foreach_statement* ya da bir *try_statement*için *specific_catch_clause* .</span><span class="sxs-lookup"><span data-stu-id="22df5-165">A ***local variable*** is declared by a *local_variable_declaration*, which may occur in a *block*, a *for_statement*, a *switch_statement* or a *using_statement*; or by a *foreach_statement* or a *specific_catch_clause* for a *try_statement*.</span></span>

<span data-ttu-id="22df5-166">Yerel bir değişkenin ömrü, program yürütmenin, bu için ayrılan depolama garantisi olan bölümüdür.</span><span class="sxs-lookup"><span data-stu-id="22df5-166">The lifetime of a local variable is the portion of program execution during which storage is guaranteed to be reserved for it.</span></span> <span data-ttu-id="22df5-167">Bu yaşam süresi en az *girişi,* *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*veya *specific_catch_clause* ile ilişkili olduğu, Bu *bloğun*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*veya *specific_catch_clause* herhangi bir şekilde yürütülmesi sona erer.</span><span class="sxs-lookup"><span data-stu-id="22df5-167">This lifetime extends at least from entry into the *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* with which it is associated, until execution of that *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* ends in any way.</span></span> <span data-ttu-id="22df5-168">(Bir kapalı *blok* girilmesi veya bir yöntemi çağırmak askıya alır, ancak bitmez, geçerli *blok*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*veya specific_ yürütmez  *catch_clause*.) Yerel değişken anonim bir işlev ([yakalanan dış değişkenler](expressions.md#captured-outer-variables)) tarafından yakalandıysa, yaşam süresi en az, anonim işlevden oluşturulan temsilci veya ifade ağacı, buna başvurmak üzere gelen diğer nesnelerle birlikte yakalanan değişken çöp toplama için uygun.</span><span class="sxs-lookup"><span data-stu-id="22df5-168">(Entering an enclosed *block* or calling a method suspends, but does not end, execution of the current *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause*.) If the local variable is captured by an anonymous function ([Captured outer variables](expressions.md#captured-outer-variables)), its lifetime extends at least until the delegate or expression tree created from the anonymous function, along with any other objects that come to reference the captured variable, are eligible for garbage collection.</span></span>

<span data-ttu-id="22df5-169">Üst *blok*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*veya *specific_catch_clause* yinelemeli olarak girilirse, her biri yerel değişkenin yeni bir örneği oluşturulur. zaman ve varsa, *local_variable_initializer*her seferinde değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="22df5-169">If the parent *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* is entered recursively, a new instance of the local variable is created each time, and its *local_variable_initializer*, if any, is evaluated each time.</span></span>

<span data-ttu-id="22df5-170">Bir *local_variable_declaration* tarafından tanıtılan yerel bir değişken otomatik olarak başlatılmaz ve bu nedenle varsayılan değere sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="22df5-170">A local variable introduced by a *local_variable_declaration* is not automatically initialized and thus has no default value.</span></span> <span data-ttu-id="22df5-171">Kesin atama denetimi amacıyla, bir *local_variable_declaration* tarafından tanıtılan yerel bir değişken başlangıçta atanmamış olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="22df5-171">For the purpose of definite assignment checking, a local variable introduced by a *local_variable_declaration* is considered initially unassigned.</span></span> <span data-ttu-id="22df5-172">Bir *local_variable_declaration* , bir *local_variable_initializer*içerebilir; bu durumda değişken yalnızca başlatma ifadesinden ([bildirim deyimleri](variables.md#declaration-statements)) sonra kesin olarak atanır.</span><span class="sxs-lookup"><span data-stu-id="22df5-172">A *local_variable_declaration* may include a *local_variable_initializer*, in which case the variable is considered definitely assigned only after the initializing expression ([Declaration statements](variables.md#declaration-statements)).</span></span>

<span data-ttu-id="22df5-173">Bir *local_variable_declaration*tarafından tanıtılan yerel bir değişken kapsamında, bu yerel değişkene *local_variable_declarator*' den önceki bir metinsel konumda başvurabileceğiniz derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-173">Within the scope of a local variable introduced by a *local_variable_declaration*, it is a compile-time error to refer to that local variable in a textual position that precedes its *local_variable_declarator*.</span></span> <span data-ttu-id="22df5-174">Yerel değişken bildirimi örtük ([yerel değişken bildirimleri](statements.md#local-variable-declarations)) ise, *local_variable_declarator*içinde değişkene başvurmak için de bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="22df5-174">If the local variable declaration is implicit ([Local variable declarations](statements.md#local-variable-declarations)), it is also an error to refer to the variable within its *local_variable_declarator*.</span></span>

<span data-ttu-id="22df5-175">Bir *foreach_statement* veya *specific_catch_clause* tarafından tanıtılan yerel bir değişken, tüm kapsamda kesin olarak atanmış olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="22df5-175">A local variable introduced by a *foreach_statement* or a *specific_catch_clause* is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="22df5-176">Yerel bir değişkenin gerçek yaşam süresi uygulamaya bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-176">The actual lifetime of a local variable is implementation-dependent.</span></span> <span data-ttu-id="22df5-177">Örneğin, bir derleyici bir bloktaki yerel değişkenin yalnızca o bloktaki küçük bir bölüm için kullanıldığını statik olarak belirleyebilir.</span><span class="sxs-lookup"><span data-stu-id="22df5-177">For example, a compiler might statically determine that a local variable in a block is only used for a small portion of that block.</span></span> <span data-ttu-id="22df5-178">Derleyici bu analizi kullanarak, değişkenin depolamanın, kapsayan bloğundan daha kısa bir yaşam süresine sahip olan bir kod üretebilir.</span><span class="sxs-lookup"><span data-stu-id="22df5-178">Using this analysis, the compiler could generate code that results in the variable's storage having a shorter lifetime than its containing block.</span></span>

<span data-ttu-id="22df5-179">Yerel başvuru değişkeni tarafından başvurulan depolama alanı, yerel başvuru değişkeninin ([Otomatik bellek yönetimi](basic-concepts.md#automatic-memory-management)) yaşam süresinden bağımsız olarak geri kazanılır.</span><span class="sxs-lookup"><span data-stu-id="22df5-179">The storage referred to by a local reference variable is reclaimed independently of the lifetime of that local reference variable ([Automatic memory management](basic-concepts.md#automatic-memory-management)).</span></span>

## <a name="default-values"></a><span data-ttu-id="22df5-180">Varsayılan değerler</span><span class="sxs-lookup"><span data-stu-id="22df5-180">Default values</span></span>

<span data-ttu-id="22df5-181">Aşağıdaki değişken kategorileri varsayılan değerlerine otomatik olarak başlatılır:</span><span class="sxs-lookup"><span data-stu-id="22df5-181">The following categories of variables are automatically initialized to their default values:</span></span>

*  <span data-ttu-id="22df5-182">Statik değişkenler.</span><span class="sxs-lookup"><span data-stu-id="22df5-182">Static variables.</span></span>
*  <span data-ttu-id="22df5-183">Sınıf örneklerinin örnek değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="22df5-183">Instance variables of class instances.</span></span>
*  <span data-ttu-id="22df5-184">Dizi öğeleri.</span><span class="sxs-lookup"><span data-stu-id="22df5-184">Array elements.</span></span>

<span data-ttu-id="22df5-185">Bir değişkenin varsayılan değeri, değişkenin türüne bağlıdır ve aşağıdaki şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="22df5-185">The default value of a variable depends on the type of the variable and is determined as follows:</span></span>

*  <span data-ttu-id="22df5-186">Bir *value_type*değişkeni için varsayılan değer, *value_type*'nin varsayılan oluşturucusu tarafından hesaplanan değerle aynıdır ([Varsayılan oluşturucular](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="22df5-186">For a variable of a *value_type*, the default value is the same as the value computed by the *value_type*'s default constructor ([Default constructors](types.md#default-constructors)).</span></span>
*  <span data-ttu-id="22df5-187">Bir *reference_type*değişkeni için varsayılan değer `null`.</span><span class="sxs-lookup"><span data-stu-id="22df5-187">For a variable of a *reference_type*, the default value is `null`.</span></span>

<span data-ttu-id="22df5-188">Varsayılan değerlere başlatma işlemi, kullanım için ayrılmadan önce bellek Yöneticisi veya çöp toplayıcı başlatma belleğini tümüyle bit sıfır olarak başlatmaya gerek kalmadan yapılır.</span><span class="sxs-lookup"><span data-stu-id="22df5-188">Initialization to default values is typically done by having the memory manager or garbage collector initialize memory to all-bits-zero before it is allocated for use.</span></span> <span data-ttu-id="22df5-189">Bu nedenle, null başvuruyu göstermek için tümü-bit-sıfır kullanmak kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-189">For this reason, it is convenient to use all-bits-zero to represent the null reference.</span></span>

## <a name="definite-assignment"></a><span data-ttu-id="22df5-190">Kesin atama</span><span class="sxs-lookup"><span data-stu-id="22df5-190">Definite assignment</span></span>

<span data-ttu-id="22df5-191">Bir işlev üyesinin çalıştırılabilir kodundaki belirli bir konumda, derleyici, belirli bir statik Akış Analizi ([kesin atamayı belirlemek Için kesin kurallar](variables.md#precise-rules-for-determining-definite-assignment)) tarafından kanıtlabiliyorsa ***,*** değişken otomatik olarak başlatıldı veya en az bir atamanın hedefi oldu.</span><span class="sxs-lookup"><span data-stu-id="22df5-191">At a given location in the executable code of a function member, a variable is said to be ***definitely assigned*** if the compiler can prove, by a particular static flow analysis ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)), that the variable has been automatically initialized or has been the target of at least one assignment.</span></span> <span data-ttu-id="22df5-192">Belirli bir deyişle, kesin atama kuralları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="22df5-192">Informally stated, the rules of definite assignment are:</span></span>

*  <span data-ttu-id="22df5-193">Başlangıçta atanan değişken ([Başlangıçta atanan değişkenler](variables.md#initially-assigned-variables)) her zaman kesinlikle atanmış olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="22df5-193">An initially assigned variable ([Initially assigned variables](variables.md#initially-assigned-variables)) is always considered definitely assigned.</span></span>
*  <span data-ttu-id="22df5-194">Başlangıçta atanmamış değişken ([Başlangıçta atanmamış değişkenler](variables.md#initially-unassigned-variables)), söz konusu konuma yönelik tüm olası yürütme yolları aşağıdakilerden en az birini içeriyorsa, belirli bir konumda kesinlikle atanmış olarak kabul edilir:</span><span class="sxs-lookup"><span data-stu-id="22df5-194">An initially unassigned variable ([Initially unassigned variables](variables.md#initially-unassigned-variables)) is considered definitely assigned at a given location if all possible execution paths leading to that location contain at least one of the following:</span></span>
    * <span data-ttu-id="22df5-195">Değişkenin sol işlenen olduğu basit atama ([basit atama](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="22df5-195">A simple assignment ([Simple assignment](expressions.md#simple-assignment)) in which the variable is the left operand.</span></span>
    * <span data-ttu-id="22df5-196">Değişkeni çıkış parametresi olarak geçiren bir çağırma ifadesi ([çağırma ifadeleri](expressions.md#invocation-expressions)) veya nesne oluşturma Ifadesi ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="22df5-196">An invocation expression ([Invocation expressions](expressions.md#invocation-expressions)) or object creation expression ([Object creation expressions](expressions.md#object-creation-expressions)) that passes the variable as an output parameter.</span></span>
    * <span data-ttu-id="22df5-197">Yerel bir değişken için, değişken başlatıcısı içeren bir yerel değişken bildirimi ([yerel değişken bildirimleri](statements.md#local-variable-declarations)).</span><span class="sxs-lookup"><span data-stu-id="22df5-197">For a local variable, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) that includes a variable initializer.</span></span>

<span data-ttu-id="22df5-198">Yukarıdaki resmi olmayan kuralları temel alan biçimsel belirtim, [Başlangıçta atanan değişkenler](variables.md#initially-assigned-variables), [Başlangıçta atanmamış değişkenler](variables.md#initially-unassigned-variables)ve kesin [atamayı belirlemek için tam kurallar](variables.md#precise-rules-for-determining-definite-assignment)bölümünde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="22df5-198">The formal specification underlying the above informal rules is described in [Initially assigned variables](variables.md#initially-assigned-variables), [Initially unassigned variables](variables.md#initially-unassigned-variables), and [Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment).</span></span>

<span data-ttu-id="22df5-199">Bir *struct_type* değişkeninin örnek değişkenlerinin kesin atama durumları, tek tek ve toplu olarak izlenir.</span><span class="sxs-lookup"><span data-stu-id="22df5-199">The definite assignment states of instance variables of a *struct_type* variable are tracked individually as well as collectively.</span></span> <span data-ttu-id="22df5-200">Yukarıdaki kurallara ek olarak, aşağıdaki kurallar *struct_type* değişkenleri ve onların örnek değişkenleri için geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="22df5-200">In addition to the rules above, the following rules apply to *struct_type* variables and their instance variables:</span></span>

*  <span data-ttu-id="22df5-201">İçeren *struct_type* değişkeni kesin olarak atanmış olarak kabul edildiğinde, örnek değişken kesin olarak atanır olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="22df5-201">An instance variable is considered definitely assigned if its containing *struct_type* variable is considered definitely assigned.</span></span>
*  <span data-ttu-id="22df5-202">Örnek değişkenlerinin her biri kesinlikle atanmış olarak kabul edildiğinde, bir *struct_type* değişkeni kesin olarak atanır.</span><span class="sxs-lookup"><span data-stu-id="22df5-202">A *struct_type* variable is considered definitely assigned if each of its instance variables is considered definitely assigned.</span></span>

<span data-ttu-id="22df5-203">Kesin atama, aşağıdaki bağlamlarda gereksinimdir:</span><span class="sxs-lookup"><span data-stu-id="22df5-203">Definite assignment is a requirement in the following contexts:</span></span>

*  <span data-ttu-id="22df5-204">Bir değişken, değerinin alındığı her konumda kesinlikle atanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-204">A variable must be definitely assigned at each location where its value is obtained.</span></span> <span data-ttu-id="22df5-205">Bu, tanımsız değerlerin hiçbir şekilde gerçekleşmemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="22df5-205">This ensures that undefined values never occur.</span></span> <span data-ttu-id="22df5-206">Bir ifadede bir değişkenin oluşma nedeni, değişkenin değerini elde etmek için kabul edilir, ancak</span><span class="sxs-lookup"><span data-stu-id="22df5-206">The occurrence of a variable in an expression is considered to obtain the value of the variable, except when</span></span>
    * <span data-ttu-id="22df5-207">değişken, basit bir atamanın sol işleneni,</span><span class="sxs-lookup"><span data-stu-id="22df5-207">the variable is the left operand of a simple assignment,</span></span>
    * <span data-ttu-id="22df5-208">değişken, çıkış parametresi olarak geçirilir veya</span><span class="sxs-lookup"><span data-stu-id="22df5-208">the variable is passed as an output parameter, or</span></span>
    * <span data-ttu-id="22df5-209">değişken bir *struct_type* değişkenidir ve bir üye erişiminin sol işleneni olarak gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="22df5-209">the variable is a *struct_type* variable and occurs as the left operand of a member access.</span></span>
*  <span data-ttu-id="22df5-210">Bir değişken, başvuru parametresi olarak geçirildiği her bir konumda kesinlikle atanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-210">A variable must be definitely assigned at each location where it is passed as a reference parameter.</span></span> <span data-ttu-id="22df5-211">Bu, çağrılan işlev üyesinin başlangıçta atanan başvuru parametresini düşünebilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="22df5-211">This ensures that the function member being invoked can consider the reference parameter initially assigned.</span></span>
*  <span data-ttu-id="22df5-212">İşlev üyesinin tüm çıkış parametreleri, işlev üyesinin döndürdüğü her konumda kesinlikle atanmalıdır (bir `return` ifadesiyle veya işlev üyesi gövdesinin sonuna ulaşan yürütme yoluyla).</span><span class="sxs-lookup"><span data-stu-id="22df5-212">All output parameters of a function member must be definitely assigned at each location where the function member returns (through a `return` statement or through execution reaching the end of the function member body).</span></span> <span data-ttu-id="22df5-213">Bu, işlev üyelerinin çıkış parametrelerinde tanımsız değerler döndürmemesini sağlar, bu sayede derleyicinin değişken atama ile eşdeğer bir çıkış parametresi olarak bir değişken alan bir işlev üye çağrısını düşünmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="22df5-213">This ensures that function members do not return undefined values in output parameters, thus enabling the compiler to consider a function member invocation that takes a variable as an output parameter equivalent to an assignment to the variable.</span></span>
*  <span data-ttu-id="22df5-214">Bir `this` *struct_type* Instance oluşturucusunun değişkeni, örnek oluşturucunun döndürdüğü her bir konumda kesinlikle atanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-214">The `this` variable of a *struct_type* instance constructor must be definitely assigned at each location where that instance constructor returns.</span></span>

### <a name="initially-assigned-variables"></a><span data-ttu-id="22df5-215">Başlangıçta atanan değişkenler</span><span class="sxs-lookup"><span data-stu-id="22df5-215">Initially assigned variables</span></span>

<span data-ttu-id="22df5-216">Aşağıdaki değişken kategorileri başlangıçta atanan olarak sınıflandırılır:</span><span class="sxs-lookup"><span data-stu-id="22df5-216">The following categories of variables are classified as initially assigned:</span></span>

*  <span data-ttu-id="22df5-217">Statik değişkenler.</span><span class="sxs-lookup"><span data-stu-id="22df5-217">Static variables.</span></span>
*  <span data-ttu-id="22df5-218">Sınıf örneklerinin örnek değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="22df5-218">Instance variables of class instances.</span></span>
*  <span data-ttu-id="22df5-219">Başlangıçta atanan yapı değişkenlerinin örnek değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="22df5-219">Instance variables of initially assigned struct variables.</span></span>
*  <span data-ttu-id="22df5-220">Dizi öğeleri.</span><span class="sxs-lookup"><span data-stu-id="22df5-220">Array elements.</span></span>
*  <span data-ttu-id="22df5-221">Değer parametreleri.</span><span class="sxs-lookup"><span data-stu-id="22df5-221">Value parameters.</span></span>
*  <span data-ttu-id="22df5-222">Başvuru parametreleri.</span><span class="sxs-lookup"><span data-stu-id="22df5-222">Reference parameters.</span></span>
*  <span data-ttu-id="22df5-223">Bir `catch` yan tümce `foreach` veya deyime göre belirtilen değişkenler.</span><span class="sxs-lookup"><span data-stu-id="22df5-223">Variables declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="initially-unassigned-variables"></a><span data-ttu-id="22df5-224">Başlangıçta atanmamış değişkenler</span><span class="sxs-lookup"><span data-stu-id="22df5-224">Initially unassigned variables</span></span>

<span data-ttu-id="22df5-225">Aşağıdaki değişken kategorileri başlangıçta atanmamış olarak sınıflandırılır:</span><span class="sxs-lookup"><span data-stu-id="22df5-225">The following categories of variables are classified as initially unassigned:</span></span>

*  <span data-ttu-id="22df5-226">Başlangıçta atanmamış yapı değişkenlerinin örnek değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="22df5-226">Instance variables of initially unassigned struct variables.</span></span>
*  <span data-ttu-id="22df5-227">Yapı örneği oluşturucularının `this` değişkeni de dahil olmak üzere çıkış parametreleri.</span><span class="sxs-lookup"><span data-stu-id="22df5-227">Output parameters, including the `this` variable of struct instance constructors.</span></span>
*  <span data-ttu-id="22df5-228">Bir `catch` yan tümce veya bir `foreach` bildirimde tanımlananlar hariç yerel değişkenler.</span><span class="sxs-lookup"><span data-stu-id="22df5-228">Local variables, except those declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="precise-rules-for-determining-definite-assignment"></a><span data-ttu-id="22df5-229">Kesin atamayı belirlemek için kesin kurallar</span><span class="sxs-lookup"><span data-stu-id="22df5-229">Precise rules for determining definite assignment</span></span>

<span data-ttu-id="22df5-230">Kullanılan her değişkenin kesinlikle atandığını tespit etmek için derleyicinin bu bölümde açıklanacak bir işlem kullanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="22df5-230">In order to determine that each used variable is definitely assigned, the compiler must use a process that is equivalent to the one described in this section.</span></span>

<span data-ttu-id="22df5-231">Derleyici, bir veya daha fazla başlangıçta atanmamış değişkenine sahip her bir işlev üyesinin gövdesini işler.</span><span class="sxs-lookup"><span data-stu-id="22df5-231">The compiler processes the body of each function member that has one or more initially unassigned variables.</span></span> <span data-ttu-id="22df5-232">Her başlangıçta atanmamış *her değişken için*, derleyici işlev üyesinde aşağıdaki noktaların her birinde bulunan *v* için kesin bir ***atama durumu*** belirler:</span><span class="sxs-lookup"><span data-stu-id="22df5-232">For each initially unassigned variable *v*, the compiler determines a ***definite assignment state*** for *v* at each of the following points in the function member:</span></span>

*  <span data-ttu-id="22df5-233">Her deyimin başlangıcında</span><span class="sxs-lookup"><span data-stu-id="22df5-233">At the beginning of each statement</span></span>
*  <span data-ttu-id="22df5-234">Her deyimin bitiş noktasında ([bitiş noktaları ve ulaşılabilirlik](statements.md#end-points-and-reachability))</span><span class="sxs-lookup"><span data-stu-id="22df5-234">At the end point ([End points and reachability](statements.md#end-points-and-reachability)) of each statement</span></span>
*  <span data-ttu-id="22df5-235">Her bir yay üzerinde, denetimi başka bir bildirime veya bir deyimin bitiş noktasına aktarır</span><span class="sxs-lookup"><span data-stu-id="22df5-235">On each arc which transfers control to another statement or to the end point of a statement</span></span>
*  <span data-ttu-id="22df5-236">Her ifadenin başlangıcında</span><span class="sxs-lookup"><span data-stu-id="22df5-236">At the beginning of each expression</span></span>
*  <span data-ttu-id="22df5-237">Her ifadenin sonunda</span><span class="sxs-lookup"><span data-stu-id="22df5-237">At the end of each expression</span></span>

<span data-ttu-id="22df5-238">*V* 'nin kesin atama durumu aşağıdakilerden biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="22df5-238">The definite assignment state of *v* can be either:</span></span>

*  <span data-ttu-id="22df5-239">Kesinlikle atandı.</span><span class="sxs-lookup"><span data-stu-id="22df5-239">Definitely assigned.</span></span> <span data-ttu-id="22df5-240">Bu, tüm olası denetimde bu noktaya akar, *v* 'nin bir değer atandığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="22df5-240">This indicates that on all possible control flows to this point, *v* has been assigned a value.</span></span>
*  <span data-ttu-id="22df5-241">Kesin olarak atanmadı.</span><span class="sxs-lookup"><span data-stu-id="22df5-241">Not definitely assigned.</span></span> <span data-ttu-id="22df5-242">Türünde `bool`bir ifadenin sonundaki bir değişkenin durumu için, kesinlikle atanmayan bir değişkenin durumu (ancak gerekli değildir) aşağıdaki alt durumlardan birine denk gelebilir:</span><span class="sxs-lookup"><span data-stu-id="22df5-242">For the state of a variable at the end of an expression of type `bool`, the state of a variable that isn't definitely assigned may (but doesn't necessarily) fall into one of the following sub-states:</span></span>
    * <span data-ttu-id="22df5-243">Doğru ifadeden sonra kesinlikle atandı.</span><span class="sxs-lookup"><span data-stu-id="22df5-243">Definitely assigned after true expression.</span></span> <span data-ttu-id="22df5-244">Bu durum, Boole ifadesi doğru olarak değerlendiriliyorsa, ancak Boolean ifadesi false olarak değerlendiriliyorsa atanmadığında, *v* 'nin kesinlikle atandığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="22df5-244">This state indicates that *v* is definitely assigned if the boolean expression evaluated as true, but is not necessarily assigned if the boolean expression evaluated as false.</span></span>
    * <span data-ttu-id="22df5-245">Yanlış ifadeden sonra kesinlikle atandı.</span><span class="sxs-lookup"><span data-stu-id="22df5-245">Definitely assigned after false expression.</span></span> <span data-ttu-id="22df5-246">Bu durum, Boole ifadesi false olarak değerlendirilirse, ancak Boolean ifadesi doğru olarak değerlendiriliyorsa atanmadığında, *v* 'nin kesinlikle atandığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="22df5-246">This state indicates that *v* is definitely assigned if the boolean expression evaluated as false, but is not necessarily assigned if the boolean expression evaluated as true.</span></span>

<span data-ttu-id="22df5-247">Aşağıdaki kurallar, her konumda bir *v* değişkeninin durumunun nasıl belirlendiğini yönetir.</span><span class="sxs-lookup"><span data-stu-id="22df5-247">The following rules govern how the state of a variable *v* is determined at each location.</span></span>

#### <a name="general-rules-for-statements"></a><span data-ttu-id="22df5-248">Deyimler için genel kurallar</span><span class="sxs-lookup"><span data-stu-id="22df5-248">General rules for statements</span></span>

*  <span data-ttu-id="22df5-249">bir işlev üye gövdesinin başlangıcında, *v* kesinlikle atanmaz.</span><span class="sxs-lookup"><span data-stu-id="22df5-249">*v* is not definitely assigned at the beginning of a function member body.</span></span>
*  <span data-ttu-id="22df5-250">*v* , herhangi bir ulaşılamaz deyimin başlangıcında kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="22df5-250">*v* is definitely assigned at the beginning of any unreachable statement.</span></span>
*  <span data-ttu-id="22df5-251">Diğer herhangi bir deyimin başlangıcında bulunan *v* 'nin kesin atama durumu, o deyimin başlangıcını hedefleyen tüm denetim akış aktarımları üzerinde *v* 'nin kesin atama durumu denetlenerek belirlenir.</span><span class="sxs-lookup"><span data-stu-id="22df5-251">The definite assignment state of *v* at the beginning of any other statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the beginning of that statement.</span></span> <span data-ttu-id="22df5-252">(Ve yalnızca *Bu) tüm* denetim akışı aktarımlarına açık bir şekilde atanmışsa, *v* , deyimin başlangıcında kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="22df5-252">If (and only if) *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the beginning of the statement.</span></span> <span data-ttu-id="22df5-253">Olası denetim akış aktarımları kümesi, bildirime ulaşılabilirlik ([bitiş noktaları ve erişilebilirlik](statements.md#end-points-and-reachability)) gibi şekilde belirlenir.</span><span class="sxs-lookup"><span data-stu-id="22df5-253">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>
*  <span data-ttu-id="22df5-254">Bir bloğun, `checked` ,`do` ,,`foreach`,, ,`using`,,,,,,,,,,, veya bitiş noktasındaki kesin atama durumu `lock` `unchecked` `if` `while` `for` `switch`bildirim, o deyimin bitiş noktasını hedefleyen tüm denetim akışı aktarımları üzerinde, *v* 'nin kesin atama durumunun denetlenmesi tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="22df5-254">The definite assignment state of *v* at the end point of a block, `checked`, `unchecked`, `if`, `while`, `do`, `for`, `foreach`, `lock`, `using`, or `switch` statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the end point of that statement.</span></span> <span data-ttu-id="22df5-255">Bu *, her* türlü denetim akışı aktarımına açık bir şekilde atanırsa, *v* , deyimin bitiş noktasında kesin olarak atanır.</span><span class="sxs-lookup"><span data-stu-id="22df5-255">If *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="22df5-256">Güvenmiyorsanız *v* , deyimin bitiş noktasında kesin olarak atanmaz.</span><span class="sxs-lookup"><span data-stu-id="22df5-256">Otherwise; *v* is not definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="22df5-257">Olası denetim akış aktarımları kümesi, bildirime ulaşılabilirlik ([bitiş noktaları ve erişilebilirlik](statements.md#end-points-and-reachability)) gibi şekilde belirlenir.</span><span class="sxs-lookup"><span data-stu-id="22df5-257">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>

#### <a name="block-statements-checked-and-unchecked-statements"></a><span data-ttu-id="22df5-258">Block deyimleri, Checked ve unchecked deyimleri</span><span class="sxs-lookup"><span data-stu-id="22df5-258">Block statements, checked, and unchecked statements</span></span>

<span data-ttu-id="22df5-259">Denetimdeki, blok içindeki (veya bloğun bitiş noktasındaki), blok içindeki bildirim listesinin ilk ifadesine aktarım yapılan *belirli bir atama* durumu, blok öncesi *v* 'nin kesin atama bildirimiyle aynıdır , `checked`, veya `unchecked` deyimidir.</span><span class="sxs-lookup"><span data-stu-id="22df5-259">The definite assignment state of *v* on the control transfer to the first statement of the statement list in the block (or to the end point of the block, if the statement list is empty) is the same as the definite assignment statement of *v* before the block, `checked`, or `unchecked` statement.</span></span>

#### <a name="expression-statements"></a><span data-ttu-id="22df5-260">İfade deyimleri</span><span class="sxs-lookup"><span data-stu-id="22df5-260">Expression statements</span></span>

<span data-ttu-id="22df5-261">*Expr* *ifadesi içeren bir ifade deyimi* için:</span><span class="sxs-lookup"><span data-stu-id="22df5-261">For an expression statement *stmt* that consists of the expression *expr*:</span></span>

*  <span data-ttu-id="22df5-262">*v* , *ifadenin* başlangıcında, *stmt*'ın başlangıcında aynı kesin atama durumuna sahiptir.</span><span class="sxs-lookup"><span data-stu-id="22df5-262">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="22df5-263">Eğer, *ifadenin*sonunda kesin olarak *atanmışsa, bu* , *stmt*öğesinin son noktasında kesinlikle atanır; güvenmiyorsanız Bu, *stmt*öğesinin son noktasında kesinlikle atanmaz.</span><span class="sxs-lookup"><span data-stu-id="22df5-263">If *v* if definitely assigned at the end of *expr*, it is definitely assigned at the end point of *stmt*; otherwise; it is not definitely assigned at the end point of *stmt*.</span></span>

#### <a name="declaration-statements"></a><span data-ttu-id="22df5-264">Bildirim deyimleri</span><span class="sxs-lookup"><span data-stu-id="22df5-264">Declaration statements</span></span>

*  <span data-ttu-id="22df5-265">*Stmt* , başlatıcılar olmadan bir bildirim bildirimidir, bu durumda,, *stmt*'ın sonunda, *stmt* öğesinin son noktasında aynı kesin atama *durumuna sahip olur* .</span><span class="sxs-lookup"><span data-stu-id="22df5-265">If *stmt* is a declaration statement without initializers, then *v* has the same definite assignment state at the end point of *stmt* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="22df5-266">*Stmt* , başlatıcıları olan bir bildirim bildirimidir, bu durumda *,, bir* Başlatıcı içeren her bildirim için bir atama bildirimiyle birlikte, *v* için kesin atama durumu, bildirim).</span><span class="sxs-lookup"><span data-stu-id="22df5-266">If *stmt* is a declaration statement with initializers, then the definite assignment state for *v* is determined as if *stmt* were a statement list, with one assignment statement for each declaration with an initializer (in the order of declaration).</span></span>

#### <a name="if-statements"></a><span data-ttu-id="22df5-267">If deyimleri</span><span class="sxs-lookup"><span data-stu-id="22df5-267">If statements</span></span>

<span data-ttu-id="22df5-268">Şu biçimdeki `if` bir *ifade için* :</span><span class="sxs-lookup"><span data-stu-id="22df5-268">For an `if` statement *stmt* of the form:</span></span>
```csharp
if ( expr ) then_stmt else else_stmt
```

*  <span data-ttu-id="22df5-269">*v* , *ifadenin* başlangıcında, *stmt*'ın başlangıcında aynı kesin atama durumuna sahiptir.</span><span class="sxs-lookup"><span data-stu-id="22df5-269">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="22df5-270">*V* , *Expr*'nin sonuna kesinlikle atanmışsa, *then_stmt* 'e denetim akışı aktarımına ve herhangi bir else tümcesi yoksa, *else_stmt* veya *stmt* öğesinin bitiş noktasına kesin bir şekilde atanır.</span><span class="sxs-lookup"><span data-stu-id="22df5-270">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt* and to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="22df5-271">Eğer *v* , *ifadenin*sonunda "kesin ifadeden sonra kesin olarak atandı" durumuna sahipse, kesin olarak *then_stmt*'e denetim akışı aktarımına atanır ve yalnızca else_ 'e yönelik denetim akışı aktarımına kesin olarak atanmaz.else yan tümcesi yoksa, stmt veya *stmt* öğesinin bitiş noktasına.</span><span class="sxs-lookup"><span data-stu-id="22df5-271">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt*, and not definitely assigned on the control flow transfer to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="22df5-272">Eğer *v* , *ifadenin*sonundaki "yanlış ifadeden sonra kesin olarak atandı" durumuna sahipse, bu durumda kesin olarak *else_stmt*'e denetim akışı aktarımına atanır ve then_stmt 'e yönelik denetim akışı aktarımına kesin olarak atanmaz *then_stmt*.</span><span class="sxs-lookup"><span data-stu-id="22df5-272">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *else_stmt*, and not definitely assigned on the control flow transfer to *then_stmt*.</span></span> <span data-ttu-id="22df5-273">Kesin olarak, yalnızca *then_stmt*bitiş noktasında kesin olarak atanmışsa, bu, *stmt* öğesinin son noktasında kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="22df5-273">It is definitely assigned at the end-point of *stmt* if and only if it is definitely assigned at the end-point of *then_stmt*.</span></span>
*  <span data-ttu-id="22df5-274">Aksi halde, *then_stmt* veya *else_stmt*için denetim akışı aktarımına veya else yan tümcesi yoksa, *stmt* öğesinin bitiş noktasına *kesin olarak* atanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="22df5-274">Otherwise, *v* is considered not definitely assigned on the control flow transfer to either the *then_stmt* or *else_stmt*, or to the end-point of *stmt* if there is no else clause.</span></span>

#### <a name="switch-statements"></a><span data-ttu-id="22df5-275">Switch deyimleri</span><span class="sxs-lookup"><span data-stu-id="22df5-275">Switch statements</span></span>

<span data-ttu-id="22df5-276">Denetim ifadesi `switch` içeren *bir deyim deyiminde:*</span><span class="sxs-lookup"><span data-stu-id="22df5-276">In a `switch` statement *stmt* with a controlling expression *expr*:</span></span>

*  <span data-ttu-id="22df5-277">*İfadenin* başındaki *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* durumu ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-277">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="22df5-278">Denetim *akışı 'nın erişilebilir* anahtar bloğu bildirim listesine aktarımı üzerindeki kesin atama durumu, *ifadenin*sonundaki *v* 'nin kesin atama durumuyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-278">The definite assignment state of *v* on the control flow transfer to a reachable switch block statement list is the same as the definite assignment state of *v* at the end of *expr*.</span></span>

#### <a name="while-statements"></a><span data-ttu-id="22df5-279">While deyimleri</span><span class="sxs-lookup"><span data-stu-id="22df5-279">While statements</span></span>

<span data-ttu-id="22df5-280">Şu biçimdeki `while` bir *ifade için* :</span><span class="sxs-lookup"><span data-stu-id="22df5-280">For a `while` statement *stmt* of the form:</span></span>
```csharp
while ( expr ) while_body
```

*  <span data-ttu-id="22df5-281">*v* , *ifadenin* başlangıcında, *stmt*'ın başlangıcında aynı kesin atama durumuna sahiptir.</span><span class="sxs-lookup"><span data-stu-id="22df5-281">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="22df5-282">*V* , *ifadenin*sonuna kesinlikle atanmışsa, *while_body* öğesine ve *stmt*bitiş noktasına denetim akışı aktarımına kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="22df5-282">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body* and to the end point of *stmt*.</span></span>
*  <span data-ttu-id="22df5-283">Eğer *v* , *ifadenin*sonunda "kesin ifadeden sonra kesin olarak atandı" durumuna sahipse, kesin olarak *while_body*'e denetim akışı aktarımına atanır, ancak bu, kesin olarak, *stmt*öğesinin son noktasında atanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="22df5-283">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body*, but not definitely assigned at the end-point of *stmt*.</span></span>
*  <span data-ttu-id="22df5-284">Eğer *v* , *ifadenin*sonundaki "yanlış ifadeden sonra kesin olarak atandı" durumuna sahipse, bu durumda kesin olarak, denetim akışı aktarımına denetim akışı aktarımı üzerinde *kesin olarak*atanmamıştır.  *_gövde*.</span><span class="sxs-lookup"><span data-stu-id="22df5-284">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*, but not definitely assigned on the control flow transfer to *while_body*.</span></span>

#### <a name="do-statements"></a><span data-ttu-id="22df5-285">Do deyimleri</span><span class="sxs-lookup"><span data-stu-id="22df5-285">Do statements</span></span>

<span data-ttu-id="22df5-286">Şu biçimdeki `do` bir *ifade için* :</span><span class="sxs-lookup"><span data-stu-id="22df5-286">For a `do` statement *stmt* of the form:</span></span>
```csharp
do do_body while ( expr ) ;
```

*  <span data-ttu-id="22df5-287">*v* *, stmt 'ın* başından itibaren, *do_body* ' den başlayarak, denetim akışı aktarımında aynı kesin atama durumuna *sahiptir.*</span><span class="sxs-lookup"><span data-stu-id="22df5-287">*v* has the same definite assignment state on the control flow transfer from the beginning of *stmt* to *do_body* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="22df5-288">*v* , *do_body*bitiş noktasında *ifadenin* başlangıcında aynı kesin atama durumuna sahiptir.</span><span class="sxs-lookup"><span data-stu-id="22df5-288">*v* has the same definite assignment state at the beginning of *expr* as at the end point of *do_body*.</span></span>
*  <span data-ttu-id="22df5-289">*V* , *Expr*'nin sonuna kesinlikle atanmışsa, bu, bir dizi sonunda denetim akışı aktarımına kesin olarak *atanır.*</span><span class="sxs-lookup"><span data-stu-id="22df5-289">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>
*  <span data-ttu-id="22df5-290">Eğer *v* , *ifadenin*sonunda "false ifadeden sonra kesin olarak atandı" durumuna sahipse, bu durumda kesin bir şekilde, *stmt*öğesinin bitiş noktasına bir denetim akışı aktarımına atanır.</span><span class="sxs-lookup"><span data-stu-id="22df5-290">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>

#### <a name="for-statements"></a><span data-ttu-id="22df5-291">For deyimleri</span><span class="sxs-lookup"><span data-stu-id="22df5-291">For statements</span></span>

<span data-ttu-id="22df5-292">Formun bir `for` ifadesine yönelik kesin atama denetimi:</span><span class="sxs-lookup"><span data-stu-id="22df5-292">Definite assignment checking for a `for` statement of the form:</span></span>
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
<span data-ttu-id="22df5-293">deyimin yazıldığı gibi yapılır:</span><span class="sxs-lookup"><span data-stu-id="22df5-293">is done as if the statement were written:</span></span>
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

<span data-ttu-id="22df5-294">`for` Deyimden *for_condition* atlandığında, kesin atama değerlendirmesi, *for_condition* , yukarıdaki genişlemede değiştirilmiş `true` gibi devam eder.</span><span class="sxs-lookup"><span data-stu-id="22df5-294">If the *for_condition* is omitted from the `for` statement, then evaluation of definite assignment proceeds as if *for_condition* were replaced with `true` in the above expansion.</span></span>

#### <a name="break-continue-and-goto-statements"></a><span data-ttu-id="22df5-295">Break, Continue ve goto deyimleri</span><span class="sxs-lookup"><span data-stu-id="22df5-295">Break, continue, and goto statements</span></span>

<span data-ttu-id="22df5-296">`break`, ,Veya`goto` ifadesinin neden olduğu denetim akışı aktarımında bulunan *v* 'nin kesin atama durumu, bildiriminin başındaki v 'nin kesin atama durumuyla aynıdır. `continue`</span><span class="sxs-lookup"><span data-stu-id="22df5-296">The definite assignment state of *v* on the control flow transfer caused by a `break`, `continue`, or `goto` statement is the same as the definite assignment state of *v* at the beginning of the statement.</span></span>

#### <a name="throw-statements"></a><span data-ttu-id="22df5-297">Throw deyimleri</span><span class="sxs-lookup"><span data-stu-id="22df5-297">Throw statements</span></span>

<span data-ttu-id="22df5-298">Formun *bir ifade alanı* için</span><span class="sxs-lookup"><span data-stu-id="22df5-298">For a statement *stmt* of the form</span></span>
```csharp
throw expr ;
```

<span data-ttu-id="22df5-299">*İfadenin* başındaki *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* 'nin kesin atama durumuyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-299">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>

#### <a name="return-statements"></a><span data-ttu-id="22df5-300">Return deyimleri</span><span class="sxs-lookup"><span data-stu-id="22df5-300">Return statements</span></span>

<span data-ttu-id="22df5-301">Formun *bir ifade alanı* için</span><span class="sxs-lookup"><span data-stu-id="22df5-301">For a statement *stmt* of the form</span></span>
```csharp
return expr ;
```

*  <span data-ttu-id="22df5-302">*İfadenin* başındaki *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* 'nin kesin atama durumuyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-302">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="22df5-303">*V* bir çıkış parametresi ise, bu durumda kesinlikle atanmalıdır:</span><span class="sxs-lookup"><span data-stu-id="22df5-303">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="22df5-304">*ifadenin* ardından</span><span class="sxs-lookup"><span data-stu-id="22df5-304">after *expr*</span></span>
    * <span data-ttu-id="22df5-305">ya `finally` da ifadesini`return` kapsayan - bir `try` `finally` veya bloğununsonunda`finally` . `try` - `catch` -</span><span class="sxs-lookup"><span data-stu-id="22df5-305">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

<span data-ttu-id="22df5-306">Şu biçimdeki bir ifade için:</span><span class="sxs-lookup"><span data-stu-id="22df5-306">For a statement stmt of the form:</span></span>
```csharp
return ;
```

*  <span data-ttu-id="22df5-307">*V* bir çıkış parametresi ise, bu durumda kesinlikle atanmalıdır:</span><span class="sxs-lookup"><span data-stu-id="22df5-307">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="22df5-308">*stmt* Before</span><span class="sxs-lookup"><span data-stu-id="22df5-308">before *stmt*</span></span>
    * <span data-ttu-id="22df5-309">ya `finally` da ifadesini`return` kapsayan - bir `try` `finally` veya bloğununsonunda`finally` . `try` - `catch` -</span><span class="sxs-lookup"><span data-stu-id="22df5-309">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

#### <a name="try-catch-statements"></a><span data-ttu-id="22df5-310">Try-catch deyimleri</span><span class="sxs-lookup"><span data-stu-id="22df5-310">Try-catch statements</span></span>

<span data-ttu-id="22df5-311">Şu biçimdeki bir *ifade için* :</span><span class="sxs-lookup"><span data-stu-id="22df5-311">For a statement *stmt* of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  <span data-ttu-id="22df5-312">*Try_block* başlangıcında bulunan *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* 'nin kesin atama durumuyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-312">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="22df5-313">*Catch_block_i* (herhangi bir *ı*için) başlangıcında bulunan *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* 'nin kesin atama durumuyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-313">The definite assignment state of *v* at the beginning of *catch_block_i* (for any *i*) is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="22df5-314">Try_block 'in son noktasında tam olarak atanan *v 'nin kesin* atama durumu *, (* ve yalnızca bu durum) *v* , ve her *catch_block_i* için (1 ile n *arasında)* ).</span><span class="sxs-lookup"><span data-stu-id="22df5-314">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) *v* is definitely assigned at the end-point of *try_block* and every *catch_block_i* (for every *i* from 1 to *n*).</span></span>

#### <a name="try-finally-statements"></a><span data-ttu-id="22df5-315">Try-finally deyimleri</span><span class="sxs-lookup"><span data-stu-id="22df5-315">Try-finally statements</span></span>

<span data-ttu-id="22df5-316">Şu biçimdeki `try` bir *ifade için* :</span><span class="sxs-lookup"><span data-stu-id="22df5-316">For a `try` statement *stmt* of the form:</span></span>
```csharp
try try_block finally finally_block
```

*  <span data-ttu-id="22df5-317">*Try_block* başlangıcında bulunan *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* 'nin kesin atama durumuyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-317">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="22df5-318">*Finally_block* başlangıcında bulunan *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* 'nin kesin atama durumuyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-318">The definite assignment state of *v* at the beginning of *finally_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="22df5-319">*Stmt* *öğesinin son* noktasında kesin atama durumu, (ve yalnızca) aşağıdakilerden en az biri doğru ise, kesin olarak atanır:</span><span class="sxs-lookup"><span data-stu-id="22df5-319">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) at least one of the following is true:</span></span>
    * <span data-ttu-id="22df5-320">*v* , *try_block* 'in son noktasında kesinlikle atanır</span><span class="sxs-lookup"><span data-stu-id="22df5-320">*v* is definitely assigned at the end-point of *try_block*</span></span>
    * <span data-ttu-id="22df5-321">*v* , *finally_block* 'in son noktasında kesinlikle atanır</span><span class="sxs-lookup"><span data-stu-id="22df5-321">*v* is definitely assigned at the end-point of *finally_block*</span></span>

<span data-ttu-id="22df5-322">*Try_block*içinde başlayan ve *try_block*dışında biten bir denetim akışı `goto` aktarımı (örneğin, bir ifade) yapılırsa *, v ise* bu denetim akışı aktarımı üzerinde de kesin olarak kabul edilir. *finally_block*bitiş noktasında kesin olarak atanır.</span><span class="sxs-lookup"><span data-stu-id="22df5-322">If a control flow transfer (for example, a `goto` statement) is made that begins within *try_block*, and ends outside of *try_block*, then *v* is also considered definitely assigned on that control flow transfer if *v* is definitely assigned at the end-point of *finally_block*.</span></span> <span data-ttu-id="22df5-323">(Bu yalnızca bir ise, bu denetim akışı aktarımındaki başka bir nedenden dolayı *v* 'nin kesin olarak atanması durumunda kesinlikle atanmamıştır.)</span><span class="sxs-lookup"><span data-stu-id="22df5-323">(This is not an only if—if *v* is definitely assigned for another reason on this control flow transfer, then it is still considered definitely assigned.)</span></span>

#### <a name="try-catch-finally-statements"></a><span data-ttu-id="22df5-324">Try-catch-finally deyimleri</span><span class="sxs-lookup"><span data-stu-id="22df5-324">Try-catch-finally statements</span></span>

<span data-ttu-id="22df5-325">`try` Formun bir - ifadesiyleilgili- kesin atama Analizi: `catch` `finally`</span><span class="sxs-lookup"><span data-stu-id="22df5-325">Definite assignment analysis for a `try`-`catch`-`finally` statement of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
<span data-ttu-id="22df5-326">deyimin `try` bir `finally` ifadeyikapsayan- bir ifade olması halinde yapılır: `try` - `catch`</span><span class="sxs-lookup"><span data-stu-id="22df5-326">is done as if the statement were a `try`-`finally` statement enclosing a `try`-`catch` statement:</span></span>
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

<span data-ttu-id="22df5-327">Aşağıdaki örnek, bir `try` deyimin farklı bloklarının ([TRY ifadesinin](statements.md#the-try-statement)) kesin atamayı nasıl etkilediğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="22df5-327">The following example demonstrates how the different blocks of a `try` statement ([The try statement](statements.md#the-try-statement)) affect definite assignment.</span></span>
```csharp
class A
{
    static void F() {
        int i, j;
        try {
            goto LABEL;
            // neither i nor j definitely assigned
            i = 1;
            // i definitely assigned
        }

        catch {
            // neither i nor j definitely assigned
            i = 3;
            // i definitely assigned
        }

        finally {
            // neither i nor j definitely assigned
            j = 5;
            // j definitely assigned
            }
        // i and j definitely assigned
        LABEL:;
        // j definitely assigned

    }
}
```

#### <a name="foreach-statements"></a><span data-ttu-id="22df5-328">Foreach deyimleri</span><span class="sxs-lookup"><span data-stu-id="22df5-328">Foreach statements</span></span>

<span data-ttu-id="22df5-329">Şu biçimdeki `foreach` bir *ifade için* :</span><span class="sxs-lookup"><span data-stu-id="22df5-329">For a `foreach` statement *stmt* of the form:</span></span>
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  <span data-ttu-id="22df5-330">*İfadenin* başındaki *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* durumu ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-330">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="22df5-331">*Embedded_statement* 'e veya denetimin bitiş noktasına aktarım sırasında bulunan *v* 'nin kesin atama *durumu,* *ifadenin*sonundaki *v* durumu ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-331">The definite assignment state of *v* on the control flow transfer to *embedded_statement* or to the end point of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="using-statements"></a><span data-ttu-id="22df5-332">Using deyimleri</span><span class="sxs-lookup"><span data-stu-id="22df5-332">Using statements</span></span>

<span data-ttu-id="22df5-333">Şu biçimdeki `using` bir *ifade için* :</span><span class="sxs-lookup"><span data-stu-id="22df5-333">For a `using` statement *stmt* of the form:</span></span>
```csharp
using ( resource_acquisition ) embedded_statement
```

*  <span data-ttu-id="22df5-334">*Resource_acquisition* başlangıcında bulunan *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* durumu ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-334">The definite assignment state of *v* at the beginning of *resource_acquisition* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="22df5-335">*Embedded_statement* 'e yönelik denetim akışı aktarımında bulunan *v* 'nin kesin atama durumu, *resource_acquisition*sonundaki *v* durumuyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-335">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *resource_acquisition*.</span></span>

#### <a name="lock-statements"></a><span data-ttu-id="22df5-336">Lock deyimleri</span><span class="sxs-lookup"><span data-stu-id="22df5-336">Lock statements</span></span>

<span data-ttu-id="22df5-337">Şu biçimdeki `lock` bir *ifade için* :</span><span class="sxs-lookup"><span data-stu-id="22df5-337">For a `lock` statement *stmt* of the form:</span></span>
```csharp
lock ( expr ) embedded_statement
```

*  <span data-ttu-id="22df5-338">*İfadenin* başındaki *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* durumu ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-338">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="22df5-339">*Embedded_statement* 'e yönelik denetim akışı aktarımında bulunan *v* 'nin kesin atama durumu, *Expr*'nin sonundaki *v* durumuyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-339">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="yield-statements"></a><span data-ttu-id="22df5-340">Yield deyimleri</span><span class="sxs-lookup"><span data-stu-id="22df5-340">Yield statements</span></span>

<span data-ttu-id="22df5-341">Şu biçimdeki `yield return` bir *ifade için* :</span><span class="sxs-lookup"><span data-stu-id="22df5-341">For a `yield return` statement *stmt* of the form:</span></span>
```csharp
yield return expr ;
```

*  <span data-ttu-id="22df5-342">*İfadenin* başındaki *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* durumu ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-342">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="22df5-343">*Stmt* 'in sonundaki *v* 'nin kesin atama durumu, *ifadenin*sonundaki *v* durumu ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-343">The definite assignment state of *v* at the end of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>
*  <span data-ttu-id="22df5-344">Bir `yield break` deyimin, kesin atama durumu üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="22df5-344">A `yield break` statement has no effect on the definite assignment state.</span></span>

#### <a name="general-rules-for-simple-expressions"></a><span data-ttu-id="22df5-345">Basit ifadeler için genel kurallar</span><span class="sxs-lookup"><span data-stu-id="22df5-345">General rules for simple expressions</span></span>

<span data-ttu-id="22df5-346">Aşağıdaki kural bu tür ifadeler için geçerlidir: değişmez değerler ([değişmez değerler](expressions.md#literals)), basit adlar ([basit adlar](expressions.md#simple-names)), üye erişim ifadeleri ([üye erişimi](expressions.md#member-access)), dizinlenmemiş taban erişim ifadeleri ([temel erişim](expressions.md#base-access)), `typeof`ifadeler ([typeof işleci](expressions.md#the-typeof-operator)), varsayılan değer ifadeleri ([varsayılan değer ifadeleri](expressions.md#default-value-expressions)) ve `nameof` ifadeler ([NameOf ifadeleri](expressions.md#nameof-expressions)).</span><span class="sxs-lookup"><span data-stu-id="22df5-346">The following rule applies to these kinds of expressions: literals ([Literals](expressions.md#literals)), simple names ([Simple names](expressions.md#simple-names)), member access expressions ([Member access](expressions.md#member-access)), non-indexed base access expressions ([Base access](expressions.md#base-access)), `typeof` expressions ([The typeof operator](expressions.md#the-typeof-operator)), default value expressions ([Default value expressions](expressions.md#default-value-expressions)) and `nameof` expressions ([Nameof expressions](expressions.md#nameof-expressions)).</span></span>

*  <span data-ttu-id="22df5-347">Böyle bir ifadenin sonundaki *v* 'nin kesin atama durumu, ifadenin başındaki *v* 'nin kesin atama durumuyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-347">The definite assignment state of *v* at the end of such an expression is the same as the definite assignment state of *v* at the beginning of the expression.</span></span>

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a><span data-ttu-id="22df5-348">Katıştırılmış ifadeleri olan ifadeler için genel kurallar</span><span class="sxs-lookup"><span data-stu-id="22df5-348">General rules for expressions with embedded expressions</span></span>

<span data-ttu-id="22df5-349">Aşağıdaki kurallar bu tür ifadeler için geçerlidir: parantez içine alınmış ifadeler ([parantez Içine alınmış ifadeler](expressions.md#parenthesized-expressions)), öğe erişim Ifadeleri ([öğe erişimi](expressions.md#element-access)), dizin oluşturma ([taban erişimi](expressions.md#base-access)), artış ve azaltma ifadeleri ([sonek artırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators), [önek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)), atama ifadeleri ([atama ifadeleri](expressions.md#cast-expressions)), birli `+`, `-`, `~`, `*`ifadeler, ikili `+` `-` ,`%` ,,`>=`,, ,`>>`,,, ,`>`, `<<` `<` `<=` `*` `/` `==`, ,`is`,, ,`&`[](expressions.md#arithmetic-operators) [](expressions.md#shift-operators), ifadeler(aritmetikişleçler,`^` kaydırma işleçleri, ilişkisel ve tür-test `as` `!=` `|` [ işleçler](expressions.md#relational-and-type-testing-operators), [mantıksal işleçler](expressions.md#logical-operators)), bileşik atama ifadeleri ([bileşik](expressions.md#compound-assignment) `checked` atama) ve `unchecked` ifadeler ([denetlenen ve işaretlenmemiş işleçler](expressions.md#the-checked-and-unchecked-operators)), ayrıca dizi ve temsilci oluşturma ifadeleri ([New işleci](expressions.md#the-new-operator)).</span><span class="sxs-lookup"><span data-stu-id="22df5-349">The following rules apply to these kinds of expressions: parenthesized expressions ([Parenthesized expressions](expressions.md#parenthesized-expressions)), element access expressions ([Element access](expressions.md#element-access)), base access expressions with indexing ([Base access](expressions.md#base-access)), increment and decrement expressions ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), cast expressions ([Cast expressions](expressions.md#cast-expressions)), unary `+`, `-`, `~`, `*` expressions, binary `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` expressions ([Arithmetic operators](expressions.md#arithmetic-operators), [Shift operators](expressions.md#shift-operators), [Relational and type-testing operators](expressions.md#relational-and-type-testing-operators), [Logical operators](expressions.md#logical-operators)), compound assignment expressions ([Compound assignment](expressions.md#compound-assignment)), `checked` and `unchecked` expressions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), plus array and delegate creation expressions ([The new operator](expressions.md#the-new-operator)).</span></span>

<span data-ttu-id="22df5-350">Bu ifadelerin her birinde, sabit bir düzende koşulsuz olarak değerlendirilen bir veya daha fazla alt ifade vardır.</span><span class="sxs-lookup"><span data-stu-id="22df5-350">Each of these expressions has one or more sub-expressions that are unconditionally evaluated in a fixed order.</span></span> <span data-ttu-id="22df5-351">Örneğin, ikili `%` işleci işlecin sol tarafını, sonra sağ tarafını değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="22df5-351">For example, the binary `%` operator evaluates the left hand side of the operator, then the right hand side.</span></span> <span data-ttu-id="22df5-352">Dizin oluşturma işlemi, dizinlenmiş ifadeyi değerlendirir ve sonra her bir dizin ifadesini soldan sağa sırayla değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="22df5-352">An indexing operation evaluates the indexed expression, and then evaluates each of the index expressions, in order from left to right.</span></span> <span data-ttu-id="22df5-353">Bir *ifade ifadesi için*, *E2,..., en*, bu sırayla değerlendirilen alt ifadeler:</span><span class="sxs-lookup"><span data-stu-id="22df5-353">For an expression *expr*, which has sub-expressions *e1, e2, ..., eN*, evaluated in that order:</span></span>

*  <span data-ttu-id="22df5-354">*E1* 'ın başında bulunan *v* 'nin kesin atama durumu, *ifadenin*başındaki kesin atama durumuyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-354">The definite assignment state of *v* at the beginning of *e1* is the same as the definite assignment state at the beginning of *expr*.</span></span>
*  <span data-ttu-id="22df5-355">*Ei* 'nin*başındaki (bir* kereden fazla) *v* 'nin kesin atama durumu, önceki alt ifadenin sonundaki kesin atama durumuyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-355">The definite assignment state of *v* at the beginning of *ei* (*i* greater than one) is the same as the definite assignment state at the end of the previous sub-expression.</span></span>
*  <span data-ttu-id="22df5-356">*İfadenin* sonundaki *v* 'nin kesin atama durumu, *en* sonunda kesin atama durumuyla aynıdır</span><span class="sxs-lookup"><span data-stu-id="22df5-356">The definite assignment state of *v* at the end of *expr* is the same as the definite assignment state at the end of *eN*</span></span>

#### <a name="invocation-expressions-and-object-creation-expressions"></a><span data-ttu-id="22df5-357">Çağırma ifadeleri ve nesne oluşturma ifadeleri</span><span class="sxs-lookup"><span data-stu-id="22df5-357">Invocation expressions and object creation expressions</span></span>

<span data-ttu-id="22df5-358">Formun *çağırma ifadesi için* :</span><span class="sxs-lookup"><span data-stu-id="22df5-358">For an invocation expression *expr* of the form:</span></span>
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
<span data-ttu-id="22df5-359">ya da formun nesne oluşturma ifadesi:</span><span class="sxs-lookup"><span data-stu-id="22df5-359">or an object creation expression of the form:</span></span>
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  <span data-ttu-id="22df5-360">Bir çağırma ifadesi için, *primary_expression* Before the *v* 'nin kesin atama durumu, *ifadenin* *Before ifadesi* ile aynı olur.</span><span class="sxs-lookup"><span data-stu-id="22df5-360">For an invocation expression, the definite assignment state of *v* before *primary_expression* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="22df5-361">Bir çağırma ifadesinde, *arg1* Before the *v* 'nin kesin atama durumu *primary_expression*sonrasında *v* 'nin durumuyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-361">For an invocation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* after *primary_expression*.</span></span>
*  <span data-ttu-id="22df5-362">Bir nesne oluşturma ifadesinde, *arg1* Before the *v* 'nin kesin atama durumu, *ifadenin* *Before ifadesi* ile aynı olur.</span><span class="sxs-lookup"><span data-stu-id="22df5-362">For an object creation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="22df5-363">Her bir bağımsız değişken için, *Argi* *'nin kesin* atama durumu normal ifade kuralları tarafından *belirlenir ve herhangi*bir `ref` veya `out` değiştiricilerin yok sayılıyor.</span><span class="sxs-lookup"><span data-stu-id="22df5-363">For each argument *argi*, the definite assignment state of *v* after *argi* is determined by the normal expression rules, ignoring any `ref` or `out` modifiers.</span></span>
*  <span data-ttu-id="22df5-364">Birden çok *daha büyük olan her bir* bağımsız değişken için, *Argi* 'nin, önceki *bağımsız*değişkenden sonraki *v* durumuyla aynı *olması için,* *o kadar kesin* bir atama durumu.</span><span class="sxs-lookup"><span data-stu-id="22df5-364">For each argument *argi* for any *i* greater than one, the definite assignment state of *v* before *argi* is the same as the state of *v* after the previous *arg*.</span></span>
*  <span data-ttu-id="22df5-365">Sanal değişken, bağımsız değişkenlerin herhangi bir `out` bağımsız değişkeni olarak (yani, formun `out v`bir bağımsız değişkeni) olarak geçirilse, *ifadenin* sonunda *v* durumu kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="22df5-365">If the variable *v* is passed as an `out` argument (i.e., an argument of the form `out v`) in any of the arguments, then the state of *v* after *expr* is definitely assigned.</span></span> <span data-ttu-id="22df5-366">Güvenmiyorsanız *ifade* sonrasında *v* 'Nin durumu, *argN*sonrasında *v* durumu ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-366">Otherwise; the state of *v* after *expr* is the same as the state of *v* after *argN*.</span></span>
*  <span data-ttu-id="22df5-367">Dizi başlatıcıları ([dizi oluşturma ifadeleri](expressions.md#array-creation-expressions)), nesne başlatıcıları ([nesne başlatıcıları](expressions.md#object-initializers)), koleksiyon başlatıcıları ([koleksiyon başlatıcıları](expressions.md#collection-initializers)) ve anonim nesne başlatıcıları ([anonim nesne oluşturma) için ifadeler](expressions.md#anonymous-object-creation-expressions)), kesin atama durumu bu yapıların bakımından tanımlandığı genişlemeyle belirlenir.</span><span class="sxs-lookup"><span data-stu-id="22df5-367">For array initializers ([Array creation expressions](expressions.md#array-creation-expressions)), object initializers ([Object initializers](expressions.md#object-initializers)), collection initializers ([Collection initializers](expressions.md#collection-initializers)) and anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)), the definite assignment state is determined by the expansion that these constructs are defined in terms of.</span></span>

#### <a name="simple-assignment-expressions"></a><span data-ttu-id="22df5-368">Basit atama ifadeleri</span><span class="sxs-lookup"><span data-stu-id="22df5-368">Simple assignment expressions</span></span>

<span data-ttu-id="22df5-369">Formun`w = expr_rhs` *bir ifadesi için* :</span><span class="sxs-lookup"><span data-stu-id="22df5-369">For an expression *expr* of the form `w = expr_rhs`:</span></span>

*  <span data-ttu-id="22df5-370">*Expr_rhs* Before the *v* 'nin kesin atama durumu, *ifadenin*önüne *v* 'nin kesin atama durumuyla aynı olur.</span><span class="sxs-lookup"><span data-stu-id="22df5-370">The definite assignment state of *v* before *expr_rhs* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="22df5-371">*İfadenin* sonunda, belirli bir *v* atama durumu, tarafından belirlenir:</span><span class="sxs-lookup"><span data-stu-id="22df5-371">The definite assignment state of *v* after *expr* is determined by:</span></span>
   * <span data-ttu-id="22df5-372">*W* , *v*ile aynı değişkense, *ifadenin* *sonunda o 'nun kesin* atama durumu kesin olarak atanır.</span><span class="sxs-lookup"><span data-stu-id="22df5-372">If *w* is the same variable as *v*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="22df5-373">Aksi takdirde, atama bir yapı türünün örnek Oluşturucusu içinde oluşursa, *w* bir özellik erişimidir, yapılandırılmış örnek için bir otomatik olarak *uygulanan özellik* *P*, *daha sonra,* *ifadenin* sonunda tam atama durumu kesin olarak atanır.</span><span class="sxs-lookup"><span data-stu-id="22df5-373">Otherwise, if the assignment occurs within the instance constructor of a struct type, if *w* is a property access designating an automatically implemented property *P* on the instance being constructed and *v* is the hidden backing field of *P*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="22df5-374">Aksi halde, *ifadenin* sonunda *v* 'nin kesin atama durumu, *expr_rhs*sonrasında *v* 'nin kesin atama durumuyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-374">Otherwise, the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_rhs*.</span></span>

#### <a name="-conditional-and-expressions"></a><span data-ttu-id="22df5-375">& & (koşullu ve) ifadeler</span><span class="sxs-lookup"><span data-stu-id="22df5-375">&& (conditional AND) expressions</span></span>

<span data-ttu-id="22df5-376">Formun`expr_first && expr_second` *bir ifadesi için* :</span><span class="sxs-lookup"><span data-stu-id="22df5-376">For an expression *expr* of the form `expr_first && expr_second`:</span></span>

*  <span data-ttu-id="22df5-377">*Expr_first* Before the *v* 'nin kesin atama durumu, *ifadenin*önüne *v* 'nin kesin atama durumuyla aynı olur.</span><span class="sxs-lookup"><span data-stu-id="22df5-377">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="22df5-378">*Expr_first* sonrasında *v* 'nin durumu kesin olarak atanır veya "doğru bir ifadeden sonra kesin olarak atanır" ise, *expr_second* *'in kesin* atama durumu kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="22df5-378">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after true expression".</span></span> <span data-ttu-id="22df5-379">Aksi takdirde, kesinlikle atanmaz.</span><span class="sxs-lookup"><span data-stu-id="22df5-379">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="22df5-380">*İfadenin* sonunda, belirli bir *v* atama durumu, tarafından belirlenir:</span><span class="sxs-lookup"><span data-stu-id="22df5-380">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="22df5-381">*Expr_first* `false`değeri olan bir sabit ifadesiyse, *ifadenin* sonunda *v* 'nin kesin atama durumu, *expr_first*sonrasında *v* 'nin kesin atama durumuyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-381">If *expr_first* is a constant expression with the value `false`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="22df5-382">Aksi halde, *expr_first* sonrasında *v* 'nin durumu kesin olarak atandıktan sonra, *ifadenin* sonunda *v* durumu kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="22df5-382">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="22df5-383">Aksi halde, *expr_second* sonrasında *v* 'nin durumu kesin olarak atandıktan ve *expr_first* sonrasında *v* 'nin durumu "kesinlikle yanlış ifadeden sonra atanır" ise, *ifadenin* sonunda *v* durumu kesinlikle olur atanan.</span><span class="sxs-lookup"><span data-stu-id="22df5-383">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after false expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="22df5-384">Aksi halde, *expr_second* sonrasında *v* 'nin durumu kesinlikle atanırsa veya "doğru ifadeden sonra kesin olarak atanır" ise, *ifadenin* sonunda *v* durumu "kesinlikle doğru ifadeden sonra atanır" olur.</span><span class="sxs-lookup"><span data-stu-id="22df5-384">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="22df5-385">Aksi halde, *expr_first* sonrasında *v* 'nin durumu "kesinlikle yanlış ifadeden sonra atanır" ise ve *expr_second* sonrasında *v* 'nin durumu "kesinlikle yanlış ifadeden sonra atanır" ise, sonrasında *v durumu Expr* "kesinlikle false ifadeden sonra atanır".</span><span class="sxs-lookup"><span data-stu-id="22df5-385">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after false expression", and the state of *v* after *expr_second* is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="22df5-386">Aksi halde, *ifadenin* sonunda *v* durumu kesinlikle atanmaz.</span><span class="sxs-lookup"><span data-stu-id="22df5-386">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="22df5-387">Örnekte</span><span class="sxs-lookup"><span data-stu-id="22df5-387">In the example</span></span>
```csharp
class A
{
    static void F(int x, int y) {
        int i;
        if (x >= 0 && (i = y) >= 0) {
            // i definitely assigned
        }
        else {
            // i not definitely assigned
        }
        // i not definitely assigned
    }
}
```
<span data-ttu-id="22df5-388">değişken `i` , bir `if` deyimin gömülü deyimlerinden birinde kesin olarak kabul edilir ancak diğeri de değildir.</span><span class="sxs-lookup"><span data-stu-id="22df5-388">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="22df5-389">`if` Yöntemi içindeki`F`deyiminde, ifadenin `i` yürütülmesiherzamanbueklideyiminyürütülmesindenönce,değişkenilkeklenmişifadeyekesinlikleatanır.`(i = y)`</span><span class="sxs-lookup"><span data-stu-id="22df5-389">In the `if` statement in method `F`, the variable `i` is definitely assigned in the first embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="22df5-390">Buna karşılık, değişken `i` , yanlış bir şekilde test edilmiş olabilir ve değişken atanmakta olan değişkenin `i` atanmasından `x >= 0` kaynaklanabilir.</span><span class="sxs-lookup"><span data-stu-id="22df5-390">In contrast, the variable `i` is not definitely assigned in the second embedded statement, since `x >= 0` might have tested false, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-conditional-or-expressions"></a><span data-ttu-id="22df5-391">|| (koşullu OR) ifadeler</span><span class="sxs-lookup"><span data-stu-id="22df5-391">|| (conditional OR) expressions</span></span>

<span data-ttu-id="22df5-392">Formun`expr_first || expr_second` *bir ifadesi için* :</span><span class="sxs-lookup"><span data-stu-id="22df5-392">For an expression *expr* of the form `expr_first || expr_second`:</span></span>

*  <span data-ttu-id="22df5-393">*Expr_first* Before the *v* 'nin kesin atama durumu, *ifadenin*önüne *v* 'nin kesin atama durumuyla aynı olur.</span><span class="sxs-lookup"><span data-stu-id="22df5-393">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="22df5-394">*Expr_first* sonrasında *v* 'nin durumu kesinlikle atanır veya "yanlış ifadeden sonra kesinlikle atanır" olduğunda, *expr_second* *'in kesin* atama durumu kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="22df5-394">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after false expression".</span></span> <span data-ttu-id="22df5-395">Aksi takdirde, kesinlikle atanmaz.</span><span class="sxs-lookup"><span data-stu-id="22df5-395">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="22df5-396">*İfadenin* sonunda, kesin atama *bildiriminin sonuna göre* belirlenir:</span><span class="sxs-lookup"><span data-stu-id="22df5-396">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="22df5-397">*Expr_first* `true`değeri olan bir sabit ifadesiyse, *ifadenin* sonunda *v* 'nin kesin atama durumu, *expr_first*sonrasında *v* 'nin kesin atama durumuyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-397">If *expr_first* is a constant expression with the value `true`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="22df5-398">Aksi halde, *expr_first* sonrasında *v* 'nin durumu kesin olarak atandıktan sonra, *ifadenin* sonunda *v* durumu kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="22df5-398">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="22df5-399">Aksi halde, *expr_second* sonrasında *v* 'nin durumu kesin olarak atandıktan ve *expr_first* sonrasında *v* 'nin durumu "kesin ifadeden sonra açık olarak atanır" ise, *ifadenin* sonunda *v* durumu kesinlikle olur atanan.</span><span class="sxs-lookup"><span data-stu-id="22df5-399">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after true expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="22df5-400">Aksi halde, *expr_second* sonrasında *v* 'nin durumu kesinlikle atanmışsa veya "yanlış ifadeden sonra kesinlikle atanır" ise, *ifadenin* sonunda *v* durumu "kesinlikle yanlış ifadeden sonra atanır" olur.</span><span class="sxs-lookup"><span data-stu-id="22df5-400">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="22df5-401">Aksi halde, *expr_first* sonrasında *v* 'nin durumu "kesin ifadeden sonra" kesinlikle atanır "ise ve *expr_second* sonrasında *v* 'nin durumu" kesinlikle doğru *ifadeden sonra*"kesin ifadeden sonra kesinlikle atanır".</span><span class="sxs-lookup"><span data-stu-id="22df5-401">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after true expression", and the state of *v* after *expr_second* is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="22df5-402">Aksi halde, *ifadenin* sonunda *v* durumu kesinlikle atanmaz.</span><span class="sxs-lookup"><span data-stu-id="22df5-402">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="22df5-403">Örnekte</span><span class="sxs-lookup"><span data-stu-id="22df5-403">In the example</span></span>
```csharp
class A
{
    static void G(int x, int y) {
        int i;
        if (x >= 0 || (i = y) >= 0) {
            // i not definitely assigned
        }
        else {
            // i definitely assigned
        }
        // i not definitely assigned
    }
}
```
<span data-ttu-id="22df5-404">değişken `i` , bir `if` deyimin gömülü deyimlerinden birinde kesin olarak kabul edilir ancak diğeri de değildir.</span><span class="sxs-lookup"><span data-stu-id="22df5-404">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="22df5-405">`if` Yöntemi içindeki`G`deyiminde, ifadenin `i` yürütülmesiherzamanbueklideyiminyürütülmesindenönce,değişkenikincigömülüifadeyekesinlikleatanır.`(i = y)`</span><span class="sxs-lookup"><span data-stu-id="22df5-405">In the `if` statement in method `G`, the variable `i` is definitely assigned in the second embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="22df5-406">Buna karşılık, değişken `i` , doğru bir şekilde test edilmiş olabilir, bu `x >= 0` da değişken atanmakta olan değişkenin `i` atanmamış olması gibi, ilk ekli ifadede kesin olarak atanmaz.</span><span class="sxs-lookup"><span data-stu-id="22df5-406">In contrast, the variable `i` is not definitely assigned in the first embedded statement, since `x >= 0` might have tested true, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-logical-negation-expressions"></a><span data-ttu-id="22df5-407">!</span><span class="sxs-lookup"><span data-stu-id="22df5-407">!</span></span> <span data-ttu-id="22df5-408">(mantıksal değilleme) ifadeleri</span><span class="sxs-lookup"><span data-stu-id="22df5-408">(logical negation) expressions</span></span>

<span data-ttu-id="22df5-409">Formun`! expr_operand` *bir ifadesi için* :</span><span class="sxs-lookup"><span data-stu-id="22df5-409">For an expression *expr* of the form `! expr_operand`:</span></span>

*  <span data-ttu-id="22df5-410">*Expr_operand* Before the *v* 'nin kesin atama durumu, *ifadenin*önüne *v* 'nin kesin atama durumuyla aynı olur.</span><span class="sxs-lookup"><span data-stu-id="22df5-410">The definite assignment state of *v* before *expr_operand* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="22df5-411">*İfadenin* sonunda, belirli bir *v* atama durumu, tarafından belirlenir:</span><span class="sxs-lookup"><span data-stu-id="22df5-411">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="22df5-412">\* Expr_operand \* sonrasında *v* 'nin durumu kesin olarak atandıktan sonra, *Expr* 'den sonra *v* durumu kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="22df5-412">If the state of *v* after \*expr_operand \*is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="22df5-413">\* Expr_operand \* sonrasında *v* 'nin durumu kesin olarak atanmamışsa, *ifadenin* sonunda *v* durumu kesinlikle atanmaz.</span><span class="sxs-lookup"><span data-stu-id="22df5-413">If the state of *v* after \*expr_operand \*is not definitely assigned, then the state of *v* after *expr* is not definitely assigned.</span></span>
    * <span data-ttu-id="22df5-414">\* Expr_operand \* sonrasında *v* 'nin durumu "kesinlikle" yanlış ifadeden sonra atanır "ise, *ifadenin* sonunda *v* durumu" kesinlikle doğru ifadeden sonra atanır "olur.</span><span class="sxs-lookup"><span data-stu-id="22df5-414">If the state of *v* after \*expr_operand \*is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="22df5-415">\* Expr_operand \* sonrasında *v* 'nin durumu "kesinlikle doğru ifadeden sonra atanır" ise, *ifadenin* sonunda *v* durumu "kesinlikle yanlış ifadeden sonra atanır" olur.</span><span class="sxs-lookup"><span data-stu-id="22df5-415">If the state of *v* after \*expr_operand \*is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>

#### <a name="-null-coalescing-expressions"></a><span data-ttu-id="22df5-416">??</span><span class="sxs-lookup"><span data-stu-id="22df5-416">??</span></span> <span data-ttu-id="22df5-417">(boş birleştirme) ifadeleri</span><span class="sxs-lookup"><span data-stu-id="22df5-417">(null coalescing) expressions</span></span>

<span data-ttu-id="22df5-418">Formun`expr_first ?? expr_second` *bir ifadesi için* :</span><span class="sxs-lookup"><span data-stu-id="22df5-418">For an expression *expr* of the form `expr_first ?? expr_second`:</span></span>

*  <span data-ttu-id="22df5-419">*Expr_first* Before the *v* 'nin kesin atama durumu, *ifadenin*önüne *v* 'nin kesin atama durumuyla aynı olur.</span><span class="sxs-lookup"><span data-stu-id="22df5-419">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="22df5-420">*Expr_second* Before the *v* 'nin kesin atama durumu, *expr_first*sonrasında *v* 'nin kesin atama durumuyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-420">The definite assignment state of *v* before *expr_second* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
*  <span data-ttu-id="22df5-421">*İfadenin* sonunda, kesin atama *bildiriminin sonuna göre* belirlenir:</span><span class="sxs-lookup"><span data-stu-id="22df5-421">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="22df5-422">*Expr_first* , null değeri olan bir sabit Ifadedir ([sabit ifadeler](expressions.md#constant-expressions)), *expr_second* *sonrasında v durumu* ile *aynı olur.*</span><span class="sxs-lookup"><span data-stu-id="22df5-422">If *expr_first* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value null, then the state of *v* after *expr* is the same as the state of *v* after *expr_second*.</span></span>
*  <span data-ttu-id="22df5-423">Aksi takdirde, *expr_first*sonrasında *v* 'nin durumu, *ifadenin* *sonunda kesin* atama durumuyla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-423">Otherwise, the state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>

#### <a name="-conditional-expressions"></a><span data-ttu-id="22df5-424">?: (koşullu) ifadeler</span><span class="sxs-lookup"><span data-stu-id="22df5-424">?: (conditional) expressions</span></span>

<span data-ttu-id="22df5-425">Formun`expr_cond ? expr_true : expr_false` *bir ifadesi için* :</span><span class="sxs-lookup"><span data-stu-id="22df5-425">For an expression *expr* of the form `expr_cond ? expr_true : expr_false`:</span></span>

*  <span data-ttu-id="22df5-426">*Expr_cond* Before the *v* 'nin kesin atama durumu, *Expr*'den önceki *v* durumu ile aynı olur.</span><span class="sxs-lookup"><span data-stu-id="22df5-426">The definite assignment state of *v* before *expr_cond* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="22df5-427">*Expr_true* önce, ve yalnızca aşağıdakilerden biri varsa, kesin olarak atanan *v* 'nin kesin atama durumu:</span><span class="sxs-lookup"><span data-stu-id="22df5-427">The definite assignment state of *v* before *expr_true* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="22df5-428">*expr_cond* değeri olan sabit bir ifadedir`false`</span><span class="sxs-lookup"><span data-stu-id="22df5-428">*expr_cond* is a constant expression with the value `false`</span></span>
    * <span data-ttu-id="22df5-429">*expr_cond* sonrasında *v* 'nin durumu kesin olarak atanır veya "doğru ifadeden sonra kesinlikle atanır".</span><span class="sxs-lookup"><span data-stu-id="22df5-429">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after true expression".</span></span>
*  <span data-ttu-id="22df5-430">*Expr_false* önce, ve yalnızca aşağıdakilerden biri varsa, kesin olarak atanan *v* 'nin kesin atama durumu:</span><span class="sxs-lookup"><span data-stu-id="22df5-430">The definite assignment state of *v* before *expr_false* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="22df5-431">*expr_cond* değeri olan sabit bir ifadedir`true`</span><span class="sxs-lookup"><span data-stu-id="22df5-431">*expr_cond* is a constant expression with the value `true`</span></span>
*  <span data-ttu-id="22df5-432">*expr_cond* sonrasında *v* 'nin durumu kesinlikle atanır veya "kesinlikle" yanlış ifadeden sonra atanır "olur.</span><span class="sxs-lookup"><span data-stu-id="22df5-432">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after false expression".</span></span>
*  <span data-ttu-id="22df5-433">*İfadenin* sonunda, belirli bir *v* atama durumu, tarafından belirlenir:</span><span class="sxs-lookup"><span data-stu-id="22df5-433">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="22df5-434">*Expr_cond* , değeri `true` olan bir sabit ifadesiyse ([sabit deyimler](expressions.md#constant-expressions)), daha sonra, *expr_true* *sonrasında v durumu* ile *aynı olur.*</span><span class="sxs-lookup"><span data-stu-id="22df5-434">If *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `true` then the state of *v* after *expr* is the same as the state of *v* after *expr_true*.</span></span>
    * <span data-ttu-id="22df5-435">Aksi takdirde, *expr_cond* sabit bir ifadedir ([sabit deyimler](expressions.md#constant-expressions) `false` ) ve ardından, *ifadenin* *ardından,* *expr_false*sonrasında *v* durumu ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-435">Otherwise, if *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `false` then the state of *v* after *expr* is the same as the state of *v* after *expr_false*.</span></span>
    * <span data-ttu-id="22df5-436">Aksi halde, *expr_true* sonrasında *v* 'nin durumu kesin olarak atandıktan ve *expr_false* sonrasında *v* 'nin durumu kesin olarak atandıktan sonra, *Expr* *'nin durumu* kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="22df5-436">Otherwise, if the state of *v* after *expr_true* is definitely assigned and the state of *v* after *expr_false* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="22df5-437">Aksi halde, *ifadenin* sonunda *v* durumu kesinlikle atanmaz.</span><span class="sxs-lookup"><span data-stu-id="22df5-437">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

#### <a name="anonymous-functions"></a><span data-ttu-id="22df5-438">Anonim işlevler</span><span class="sxs-lookup"><span data-stu-id="22df5-438">Anonymous functions</span></span>

<span data-ttu-id="22df5-439">Bir *lambda_expression* veya *anonymous_method_expression* ifadesinde gövde ( *blok* ya da *ifade*) *gövdesi*için:</span><span class="sxs-lookup"><span data-stu-id="22df5-439">For a *lambda_expression* or *anonymous_method_expression* *expr* with a body (either *block* or *expression*) *body*:</span></span>

*  <span data-ttu-id="22df5-440">*Gövdeden* önceki bir dış değişken *v* 'nin kesin atama durumu, *ifadenin*önceki *d* durumuyla aynı olur.</span><span class="sxs-lookup"><span data-stu-id="22df5-440">The definite assignment state of an outer variable *v* before *body* is the same as the state of *v* before *expr*.</span></span> <span data-ttu-id="22df5-441">Diğer bir deyişle, dış değişkenlerin kesin atama durumu anonim işlev bağlamından devralınır.</span><span class="sxs-lookup"><span data-stu-id="22df5-441">That is, definite assignment state of outer variables is inherited from the context of the anonymous function.</span></span>
*  <span data-ttu-id="22df5-442">*İfadenin* *sonrasında bir* dış değişkenin kesin atama durumu, *ifadenin*önünde *v* durumu ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="22df5-442">The definite assignment state of an outer variable *v* after *expr* is the same as the state of *v* before *expr*.</span></span>

<span data-ttu-id="22df5-443">Örnek</span><span class="sxs-lookup"><span data-stu-id="22df5-443">The example</span></span>
```csharp
delegate bool Filter(int i);

void F() {
    int max;

    // Error, max is not definitely assigned
    Filter f = (int n) => n < max;

    max = 5;
    DoWork(f);
}
```
<span data-ttu-id="22df5-444">anonim işlevin bildirildiği yere kesin `max` olarak atanmadığından, derleme zamanı hatası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="22df5-444">generates a compile-time error since `max` is not definitely assigned where the anonymous function is declared.</span></span> <span data-ttu-id="22df5-445">Örnek</span><span class="sxs-lookup"><span data-stu-id="22df5-445">The example</span></span>
```csharp
delegate void D();

void F() {
    int n;
    D d = () => { n = 1; };

    d();

    // Error, n is not definitely assigned
    Console.WriteLine(n);
}
```
<span data-ttu-id="22df5-446">anonim işlevde olan atamanın `n` , anonim işlevin dışına yönelik `n` kesin atama durumu üzerinde hiçbir etkisi olmadığından, derleme zamanı hatası da oluşturur.</span><span class="sxs-lookup"><span data-stu-id="22df5-446">also generates a compile-time error since the assignment to `n` in the anonymous function has no affect on the definite assignment state of `n` outside the anonymous function.</span></span>

## <a name="variable-references"></a><span data-ttu-id="22df5-447">Değişken başvuruları</span><span class="sxs-lookup"><span data-stu-id="22df5-447">Variable references</span></span>

<span data-ttu-id="22df5-448">Bir *variable_reference* , değişken olarak sınıflandırılan bir *ifadedir* .</span><span class="sxs-lookup"><span data-stu-id="22df5-448">A *variable_reference* is an *expression* that is classified as a variable.</span></span> <span data-ttu-id="22df5-449">Bir *variable_reference* , geçerli değeri getirmek ve yeni bir değeri depolamak için her ikisine de erişilebilen bir depolama konumunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="22df5-449">A *variable_reference* denotes a storage location that can be accessed both to fetch the current value and to store a new value.</span></span>

```antlr
variable_reference
    : expression
    ;
```

<span data-ttu-id="22df5-450">C ve C++içinde bir *variable_reference* , *lvalue*olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="22df5-450">In C and C++, a *variable_reference* is known as an *lvalue*.</span></span>

## <a name="atomicity-of-variable-references"></a><span data-ttu-id="22df5-451">Değişken başvurularının Atomicity değeri</span><span class="sxs-lookup"><span data-stu-id="22df5-451">Atomicity of variable references</span></span>

<span data-ttu-id="22df5-452">Aşağıdaki veri türlerini `bool`okuma ve yazma işlemleri atomik:, `short` `byte` `uint`, `sbyte` `char` `ushort` ,,,,,,`int`vebaşvurutürleridir. `float`</span><span class="sxs-lookup"><span data-stu-id="22df5-452">Reads and writes of the following data types are atomic: `bool`, `char`, `byte`, `sbyte`, `short`, `ushort`, `uint`, `int`, `float`, and reference types.</span></span> <span data-ttu-id="22df5-453">Bunlara ek olarak, önceki listede bulunan temel bir türle birlikte sabit listesi türlerinin okuma ve yazma işlemleri de atomik bir yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="22df5-453">In addition, reads and writes of enum types with an underlying type in the previous list are also atomic.</span></span> <span data-ttu-id="22df5-454">`long` ,`ulong`,, Ve`decimal`gibi diğer türlerin okuma ve yazma işlemlerinin yanı sıra Kullanıcı tanımlı türlerin atomik olması garanti edilmez. `double`</span><span class="sxs-lookup"><span data-stu-id="22df5-454">Reads and writes of other types, including `long`, `ulong`, `double`, and `decimal`, as well as user-defined types, are not guaranteed to be atomic.</span></span> <span data-ttu-id="22df5-455">Bu amaçla tasarlanan kitaplık işlevleri dışında, artış veya azaltma durumunda olduğu gibi atomik okuma-değiştirme-yazma garantisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="22df5-455">Aside from the library functions designed for that purpose, there is no guarantee of atomic read-modify-write, such as in the case of increment or decrement.</span></span>

