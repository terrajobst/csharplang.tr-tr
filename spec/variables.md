---
ms.openlocfilehash: ff285fc202d14c2060c5f005c319c7886458a168
ms.sourcegitcommit: 8152182f0a477cb3082e625b607262cc459a17f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66174244"
---
# <a name="variables"></a><span data-ttu-id="96215-101">Değişkenler</span><span class="sxs-lookup"><span data-stu-id="96215-101">Variables</span></span>

<span data-ttu-id="96215-102">Değişkenleri depolama konumlarını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="96215-102">Variables represent storage locations.</span></span> <span data-ttu-id="96215-103">Her değişken değerlerin neler olması belirleyen bir türe sahip bir değişkende depolanır.</span><span class="sxs-lookup"><span data-stu-id="96215-103">Every variable has a type that determines what values can be stored in the variable.</span></span> <span data-ttu-id="96215-104">C# bir tür kullanımı uyumlu bir dildir ve C# derleyicisi değişkenlerinde depolanan değerler her zaman uygun türü olduğunu garanti eder.</span><span class="sxs-lookup"><span data-stu-id="96215-104">C# is a type-safe language, and the C# compiler guarantees that values stored in variables are always of the appropriate type.</span></span> <span data-ttu-id="96215-105">Bir değişkenin değerini kullanımını veya atama üzerinden değiştirilebilir `++` ve `--` işleçleri.</span><span class="sxs-lookup"><span data-stu-id="96215-105">The value of a variable can be changed through assignment or through use of the `++` and `--` operators.</span></span>

<span data-ttu-id="96215-106">Bir değişken olmalıdır ***kesinlikle atanan*** ([belirli atama onayına](variables.md#definite-assignment)) önce değeri elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="96215-106">A variable must be ***definitely assigned*** ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained.</span></span>

<span data-ttu-id="96215-107">Aşağıdaki bölümlerde açıklandığı gibi ya da değişkenlerdir ***başlangıçta atanan*** veya ***başlangıçta atanmamış***.</span><span class="sxs-lookup"><span data-stu-id="96215-107">As described in the following sections, variables are either ***initially assigned*** or ***initially unassigned***.</span></span> <span data-ttu-id="96215-108">Başlangıçta atanan bir değişkeni, iyi tanımlanmış bir başlangıç değeri vardır ve kesinlikle atanan her zaman değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="96215-108">An initially assigned variable has a well-defined initial value and is always considered definitely assigned.</span></span> <span data-ttu-id="96215-109">Başlangıçta atanmayan bir değişkeni, ilk değer yok.</span><span class="sxs-lookup"><span data-stu-id="96215-109">An initially unassigned variable has no initial value.</span></span> <span data-ttu-id="96215-110">Belirli bir konumda kesinlikle atanmış olarak değerlendirilmesi başlangıçta atanmamış değişken için bu konuma önde gelen tüm olası yürütme yolu değişkeni atama bulunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="96215-110">For an initially unassigned variable to be considered definitely assigned at a certain location, an assignment to the variable must occur in every possible execution path leading to that location.</span></span>

## <a name="variable-categories"></a><span data-ttu-id="96215-111">Değişken kategorileri</span><span class="sxs-lookup"><span data-stu-id="96215-111">Variable categories</span></span>

<span data-ttu-id="96215-112">C# tanımlar yedi kategori değişkenlerin: statik değişkenler, örnek değişkenleri, dizi öğeleri, değer parametreleri, başvuru parametreleri, çıktı parametreleri ve yerel değişkenler.</span><span class="sxs-lookup"><span data-stu-id="96215-112">C# defines seven categories of variables: static variables, instance variables, array elements, value parameters, reference parameters, output parameters, and local variables.</span></span> <span data-ttu-id="96215-113">Aşağıdaki bölümlerde bu kategorilerden her biri açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="96215-113">The sections that follow describe each of these categories.</span></span>

<span data-ttu-id="96215-114">Örnekte</span><span class="sxs-lookup"><span data-stu-id="96215-114">In the example</span></span>
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
<span data-ttu-id="96215-115">`x` bir statik değişken `y` bir örnek değişkeni `v[0]` bir dizi öğe `a` bir değer parametresi `b` bir başvuru parametresi `c` bir çıktı parametresidir ve `i` yerel bir değişkendir.</span><span class="sxs-lookup"><span data-stu-id="96215-115">`x` is a static variable, `y` is an instance variable, `v[0]` is an array element, `a` is a value parameter, `b` is a reference parameter, `c` is an output parameter, and `i` is a local variable.</span></span>

### <a name="static-variables"></a><span data-ttu-id="96215-116">Statik değişkenler</span><span class="sxs-lookup"><span data-stu-id="96215-116">Static variables</span></span>

<span data-ttu-id="96215-117">Bir alana bildirilen `static` değiştiricisi çağrıldığında bir ***statik değişken***.</span><span class="sxs-lookup"><span data-stu-id="96215-117">A field declared with the `static` modifier is called a ***static variable***.</span></span> <span data-ttu-id="96215-118">Bir statik değişken varlığı statik oluşturucunun yürütmeden önce salesforce'taki ([statik oluşturucular](classes.md#static-constructors)) kapsayan türü ve ilişkili uygulama etki alanı olmaktan çıkar mevcut olduğunda mevcut olmaktan çıkar.</span><span class="sxs-lookup"><span data-stu-id="96215-118">A static variable comes into existence before execution of the static constructor ([Static constructors](classes.md#static-constructors)) for its containing type, and ceases to exist when the associated application domain ceases to exist.</span></span>

<span data-ttu-id="96215-119">Statik bir değişkenin başlangıç değerini varsayılan değerdir ([varsayılan değerler](variables.md#default-values)) değişken türü.</span><span class="sxs-lookup"><span data-stu-id="96215-119">The initial value of a static variable is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="96215-120">Belirli atama onayına denetleme amaçları başlangıçta atanan bir statik değişken olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="96215-120">For purposes of definite assignment checking, a static variable is considered initially assigned.</span></span>

### <a name="instance-variables"></a><span data-ttu-id="96215-121">Örnek değişkenleri</span><span class="sxs-lookup"><span data-stu-id="96215-121">Instance variables</span></span>

<span data-ttu-id="96215-122">Bir alan olmadan bildirilen `static` değiştiricisi çağrıldığında bir ***örnek değişkeni***.</span><span class="sxs-lookup"><span data-stu-id="96215-122">A field declared without the `static` modifier is called an ***instance variable***.</span></span>

#### <a name="instance-variables-in-classes"></a><span data-ttu-id="96215-123">Sınıflarda örnek değişkenleri</span><span class="sxs-lookup"><span data-stu-id="96215-123">Instance variables in classes</span></span>

<span data-ttu-id="96215-124">Bu sınıfın yeni bir örneği oluşturulduğunda ve söz konusu örneğine başvuru vardır ve örneğin yıkıcısı (varsa) yürütüldüğünde sonlanacaktır mevcut olmaktan çıkar bir örnek değişkeni bir sınıfın varlığı gelir.</span><span class="sxs-lookup"><span data-stu-id="96215-124">An instance variable of a class comes into existence when a new instance of that class is created, and ceases to exist when there are no references to that instance and the instance's destructor (if any) has executed.</span></span>

<span data-ttu-id="96215-125">Bir sınıfın bir örnek değişkeni ilk değerini varsayılan değerdir ([varsayılan değerler](variables.md#default-values)) değişken türü.</span><span class="sxs-lookup"><span data-stu-id="96215-125">The initial value of an instance variable of a class is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="96215-126">Belirli atama onayına denetlemek amacıyla, başlangıçta atanan bir sınıfın bir örnek değişkeni kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="96215-126">For the purpose of definite assignment checking, an instance variable of a class is considered initially assigned.</span></span>

#### <a name="instance-variables-in-structs"></a><span data-ttu-id="96215-127">Yapılar örneği değişkenleri</span><span class="sxs-lookup"><span data-stu-id="96215-127">Instance variables in structs</span></span>

<span data-ttu-id="96215-128">Bir örnek değişkeni bir yapının ait olduğu yapı değişkeni olarak tam olarak aynı ömrü vardır.</span><span class="sxs-lookup"><span data-stu-id="96215-128">An instance variable of a struct has exactly the same lifetime as the struct variable to which it belongs.</span></span> <span data-ttu-id="96215-129">Diğer bir deyişle, ne zaman bir yapı türünün değişkenini varlığı gelir veya var, bunu olmaktan çıkar çok struct örneği değişkenleri yapın.</span><span class="sxs-lookup"><span data-stu-id="96215-129">In other words, when a variable of a struct type comes into existence or ceases to exist, so too do the instance variables of the struct.</span></span>

<span data-ttu-id="96215-130">Bir yapının bir örnek değişkeni ilk atama durumu, içeren yapı değişkeni aynıdır.</span><span class="sxs-lookup"><span data-stu-id="96215-130">The initial assignment state of an instance variable of a struct is the same as that of the containing struct variable.</span></span> <span data-ttu-id="96215-131">Diğer bir deyişle, bir yapı değişkeni başlangıçta ne zaman kabul edilir atanan, bunu çok örneği değişkenlerini ve bir yapı değişkeni başlangıçta atanmamış olarak kabul edilir, örnek değişkenlerini benzer şekilde atanmamış.</span><span class="sxs-lookup"><span data-stu-id="96215-131">In other words, when a struct variable is considered initially assigned, so too are its instance variables, and when a struct variable is considered initially unassigned, its instance variables are likewise unassigned.</span></span>

### <a name="array-elements"></a><span data-ttu-id="96215-132">Dizi öğeleri</span><span class="sxs-lookup"><span data-stu-id="96215-132">Array elements</span></span>

<span data-ttu-id="96215-133">Bir dizinin öğeleri varlığı bir dizi örneği oluşturulduğunda gelen ve başvuru, dizi örneği olduğunda mevcut sona.</span><span class="sxs-lookup"><span data-stu-id="96215-133">The elements of an array come into existence when an array instance is created, and cease to exist when there are no references to that array instance.</span></span>

<span data-ttu-id="96215-134">Bir dizideki öğelerin her biri olan başlangıç değeri varsayılan değerdir ([varsayılan değerler](variables.md#default-values)) dizi öğelerinin türü.</span><span class="sxs-lookup"><span data-stu-id="96215-134">The initial value of each of the elements of an array is the default value ([Default values](variables.md#default-values)) of the type of the array elements.</span></span>

<span data-ttu-id="96215-135">Belirli atama onayına denetlemek amacıyla, başlangıçta atanan bir dizi öğesine kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="96215-135">For the purpose of definite assignment checking, an array element is considered initially assigned.</span></span>

### <a name="value-parameters"></a><span data-ttu-id="96215-136">Değer parametreleri</span><span class="sxs-lookup"><span data-stu-id="96215-136">Value parameters</span></span>

<span data-ttu-id="96215-137">Bir parametre olmadan bildirilen bir `ref` veya `out` değiştiricisi bir ***değer parametresi***.</span><span class="sxs-lookup"><span data-stu-id="96215-137">A parameter declared without a `ref` or `out` modifier is a ***value parameter***.</span></span>

<span data-ttu-id="96215-138">Varlığı işlevi üyesi (yöntem, örnek oluşturucusu, erişimci veya işleci) veya anonim işlev çağrısı sırasında bir değer parametresini salesforce'taki, parametre ait ve verilen bir çağrıda bağımsız değişkenin değeri ile başlatılır.</span><span class="sxs-lookup"><span data-stu-id="96215-138">A value parameter comes into existence upon invocation of the function member (method, instance constructor, accessor, or operator) or anonymous function to which the parameter belongs, and is initialized with the value of the argument given in the invocation.</span></span> <span data-ttu-id="96215-139">Bir değer parametresini işlevi üye veya anonim bir işlevin dönüş sırasında mevcut normalde olmaktan çıkar.</span><span class="sxs-lookup"><span data-stu-id="96215-139">A value parameter normally ceases to exist upon return of the function member or anonymous function.</span></span> <span data-ttu-id="96215-140">Ancak, değer parametresi bir anonim bir işlev tarafından yakalanır ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)), kendi yaşam süresi en az bir temsilci kadar genişletir ya da ifade ağacı anonim bu işlevden oluşturulan için uygun Çöp toplama.</span><span class="sxs-lookup"><span data-stu-id="96215-140">However, if the value parameter is captured by an anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), its life time extends at least until the delegate or expression tree created from that anonymous function is eligible for garbage collection.</span></span>

<span data-ttu-id="96215-141">Belirli atama onayına denetlemek amacıyla, başlangıçta atanan bir değer parametresini kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="96215-141">For the purpose of definite assignment checking, a value parameter is considered initially assigned.</span></span>

### <a name="reference-parameters"></a><span data-ttu-id="96215-142">Başvuru parametreleri</span><span class="sxs-lookup"><span data-stu-id="96215-142">Reference parameters</span></span>

<span data-ttu-id="96215-143">Bir parametre ile bildirilen bir `ref` değiştiricisi bir ***parametre başvurusunu***.</span><span class="sxs-lookup"><span data-stu-id="96215-143">A parameter declared with a `ref` modifier is a ***reference parameter***.</span></span>

<span data-ttu-id="96215-144">Bir başvuru parametresi, yeni bir depolama konumuna oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="96215-144">A reference parameter does not create a new storage location.</span></span> <span data-ttu-id="96215-145">Bunun yerine, aynı depolama konumu işlev üyesi veya anonim işlev çağırma bağımsız değişken olarak verilen değişkeni olarak bir başvuru parametresi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="96215-145">Instead, a reference parameter represents the same storage location as the variable given as the argument in the function member or anonymous function invocation.</span></span> <span data-ttu-id="96215-146">Bu nedenle, bir başvuru parametresi değeri her zaman temel alınan değişkeni aynıdır.</span><span class="sxs-lookup"><span data-stu-id="96215-146">Thus, the value of a reference parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="96215-147">Aşağıdaki belirli atama onayına kurallar başvuru parametreleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="96215-147">The following definite assignment rules apply to reference parameters.</span></span> <span data-ttu-id="96215-148">Çıktı parametreleri bölümünde açıklanan için farklı kuralları unutmayın [çıkış parametresi](variables.md#output-parameters).</span><span class="sxs-lookup"><span data-stu-id="96215-148">Note the different rules for output parameters described in [Output parameters](variables.md#output-parameters).</span></span>

*  <span data-ttu-id="96215-149">Bir değişken kesinlikle atanmalıdır ([belirli atama onayına](variables.md#definite-assignment)) önce bir işlev çağırma üyesi veya temsilci bir başvuru parametresi olarak geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="96215-149">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="96215-150">Bir işlev üyesi veya anonim işlev içinde bir başvuru parametresi başlangıçta atanan olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="96215-150">Within a function member or anonymous function, a reference parameter is considered initially assigned.</span></span>

<span data-ttu-id="96215-151">Bir örnek yöntemi veya bir yapı türünün örneği erişimci `this` anahtar sözcüğü, tam olarak bir başvuru parametresi yapı türü olarak davranır ([bu erişim](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="96215-151">Within an instance method or instance accessor of a struct type, the `this` keyword behaves exactly as a reference parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="output-parameters"></a><span data-ttu-id="96215-152">Çıktı parametreleri</span><span class="sxs-lookup"><span data-stu-id="96215-152">Output parameters</span></span>

<span data-ttu-id="96215-153">Bir parametre ile bildirilen bir `out` değiştiricisi bir ***çıkış parametresi***.</span><span class="sxs-lookup"><span data-stu-id="96215-153">A parameter declared with an `out` modifier is an ***output parameter***.</span></span>

<span data-ttu-id="96215-154">Çıkış parametresi, yeni bir depolama konumuna oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="96215-154">An output parameter does not create a new storage location.</span></span> <span data-ttu-id="96215-155">Bunun yerine, aynı depolama konumu işlev üyesi veya temsilci çağırma bağımsız değişken olarak verilen değişkeni olarak bir çıkış parametresi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="96215-155">Instead, an output parameter represents the same storage location as the variable given as the argument in the function member or delegate invocation.</span></span> <span data-ttu-id="96215-156">Bu nedenle, bir çıkış parametresinin değeri her zaman temel alınan değişkeni aynıdır.</span><span class="sxs-lookup"><span data-stu-id="96215-156">Thus, the value of an output parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="96215-157">Çıktı parametreleri aşağıdaki belirli atama onayına kurallar geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="96215-157">The following definite assignment rules apply to output parameters.</span></span> <span data-ttu-id="96215-158">Açıklanan başvuru parametreleri için farklı kuralları Not [başvuru parametreleri](variables.md#reference-parameters).</span><span class="sxs-lookup"><span data-stu-id="96215-158">Note the different rules for reference parameters described in [Reference parameters](variables.md#reference-parameters).</span></span>

*  <span data-ttu-id="96215-159">Üye işlevi bir çıktı parametresi olarak geçirilebilir veya çağırma temsilci önce bir değişkeni kesinlikle atanmamış.</span><span class="sxs-lookup"><span data-stu-id="96215-159">A variable need not be definitely assigned before it can be passed as an output parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="96215-160">Normal tamamlama üyesi veya temsilci bir işlev çağrısının bir output parametresi olarak kabul edilir olarak geçirilen her bir değişken yürütme yolda atanır.</span><span class="sxs-lookup"><span data-stu-id="96215-160">Following the normal completion of a function member or delegate invocation, each variable that was passed as an output parameter is considered assigned in that execution path.</span></span>
*  <span data-ttu-id="96215-161">Bir işlev üyesi veya anonim işlev içinde bir output parametresi başlangıçta atanmamış olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="96215-161">Within a function member or anonymous function, an output parameter is considered initially unassigned.</span></span>
*  <span data-ttu-id="96215-162">Her çıkış parametresi bir işlev üyesi ya da anonim işlev kesinlikle atanmalıdır ([belirli atama onayına](variables.md#definite-assignment)) üyesi veya anonim işlev önce işlev normal olarak döndürür.</span><span class="sxs-lookup"><span data-stu-id="96215-162">Every output parameter of a function member or anonymous function must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before the function member or anonymous function returns normally.</span></span>

<span data-ttu-id="96215-163">Bir yapı türünün bir örneği oluşturucu içinde `this` anahtar sözcüğü, tam olarak bir çıkış parametresi yapı türü olarak davranır ([bu erişim](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="96215-163">Within an instance constructor of a struct type, the `this` keyword behaves exactly as an output parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="local-variables"></a><span data-ttu-id="96215-164">Yerel değişkenler</span><span class="sxs-lookup"><span data-stu-id="96215-164">Local variables</span></span>

<span data-ttu-id="96215-165">A ***yerel değişken*** tarafından bildirilen bir *local_variable_declaration*, içinde meydana gelebilir bir *blok*, *for_statement*, bir *switch_statement* veya *using_statement*; ya da bir *foreach_statement* veya *specific_catch_clause* bir için*try_statement*.</span><span class="sxs-lookup"><span data-stu-id="96215-165">A ***local variable*** is declared by a *local_variable_declaration*, which may occur in a *block*, a *for_statement*, a *switch_statement* or a *using_statement*; or by a *foreach_statement* or a *specific_catch_clause* for a *try_statement*.</span></span>

<span data-ttu-id="96215-166">Yerel bir değişken ömrü, program yürütme sırasında depolama için ayrılmış olması garanti bölümüdür.</span><span class="sxs-lookup"><span data-stu-id="96215-166">The lifetime of a local variable is the portion of program execution during which storage is guaranteed to be reserved for it.</span></span> <span data-ttu-id="96215-167">Bu yaşam süresi en az girişinde genişletir *blok*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, veya *specific_catch_clause* , yürütmeyi o kadar ilişkilendirildiği ile *blok*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, veya *specific_catch_clause* herhangi bir şekilde sona erer.</span><span class="sxs-lookup"><span data-stu-id="96215-167">This lifetime extends at least from entry into the *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* with which it is associated, until execution of that *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* ends in any way.</span></span> <span data-ttu-id="96215-168">(Bir kapalı girme *blok* veya yöntemi çağırma askıya alır, ancak sonlanmıyor, geçerli yürütme *blok*, *for_statement*, *switch_statement* , *using_statement*, *foreach_statement*, veya *specific_catch_clause*.) Yerel değişken bir anonim bir işlev tarafından yakalanır ([dış değişkenlere yakalanan](expressions.md#captured-outer-variables)), yaşam süresi en az gelir herhangi bir nesne ile birlikte anonim işlev oluşturulan temsilci veya ifade ağacı kadar genişletir Yakalanan bir değişken başvurusu, atık toplama için uygundur.</span><span class="sxs-lookup"><span data-stu-id="96215-168">(Entering an enclosed *block* or calling a method suspends, but does not end, execution of the current *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause*.) If the local variable is captured by an anonymous function ([Captured outer variables](expressions.md#captured-outer-variables)), its lifetime extends at least until the delegate or expression tree created from the anonymous function, along with any other objects that come to reference the captured variable, are eligible for garbage collection.</span></span>

<span data-ttu-id="96215-169">Üst *blok*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, veya *specific_catch_clause* girilen yinelemeli olarak, yerel değişken yeni bir örneğini her zaman oluşturulur ve kendi *local_variable_initializer*, varsa, değerlendirme her zaman.</span><span class="sxs-lookup"><span data-stu-id="96215-169">If the parent *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* is entered recursively, a new instance of the local variable is created each time, and its *local_variable_initializer*, if any, is evaluated each time.</span></span>

<span data-ttu-id="96215-170">Tarafından sunulan bir yerel değişken bir *local_variable_declaration* otomatik olarak başlatılmadı ve bu nedenle varsayılan değeri yok.</span><span class="sxs-lookup"><span data-stu-id="96215-170">A local variable introduced by a *local_variable_declaration* is not automatically initialized and thus has no default value.</span></span> <span data-ttu-id="96215-171">Belirli atama onayına denetlemek amacıyla, yerel bir değişken sunulan tarafından bir *local_variable_declaration* başlangıçta atanmamış olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="96215-171">For the purpose of definite assignment checking, a local variable introduced by a *local_variable_declaration* is considered initially unassigned.</span></span> <span data-ttu-id="96215-172">A *local_variable_declaration* içerebilir bir *local_variable_initializer*, bu durumda değişkeni yalnızca başlatılırken ifadeden sonra kesinlikle atanmış olarak kabul edilir ([ Bildirim deyimleri](variables.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="96215-172">A *local_variable_declaration* may include a *local_variable_initializer*, in which case the variable is considered definitely assigned only after the initializing expression ([Declaration statements](variables.md#declaration-statements)).</span></span>

<span data-ttu-id="96215-173">Tarafından sunulan yerel bir değişken kapsamında bir *local_variable_declaration*, önündeki değerinin metinsel bir konumda yerel söz konusu değişkene başvurmak için bir derleme zamanı hata kendi *local_variable_declarator*.</span><span class="sxs-lookup"><span data-stu-id="96215-173">Within the scope of a local variable introduced by a *local_variable_declaration*, it is a compile-time error to refer to that local variable in a textual position that precedes its *local_variable_declarator*.</span></span> <span data-ttu-id="96215-174">Yerel değişken bildiriminde örtük ise ([yerel değişken bildirimlerini](statements.md#local-variable-declarations)), ayrıca değişkenin içinde başvurmak için bir hata olduğundan, *local_variable_declarator*.</span><span class="sxs-lookup"><span data-stu-id="96215-174">If the local variable declaration is implicit ([Local variable declarations](statements.md#local-variable-declarations)), it is also an error to refer to the variable within its *local_variable_declarator*.</span></span>

<span data-ttu-id="96215-175">Tarafından sunulan bir yerel değişken bir *foreach_statement* veya *specific_catch_clause* tüm kapsamında kesinlikle atanmış olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="96215-175">A local variable introduced by a *foreach_statement* or a *specific_catch_clause* is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="96215-176">Bir yerel değişkenin gerçek yaşam uygulaması bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="96215-176">The actual lifetime of a local variable is implementation-dependent.</span></span> <span data-ttu-id="96215-177">Örneğin, bir derleyici statik bir yerel değişken bir blok içinde yalnızca o blok küçük bir kısmı için kullanılan saptayabilir.</span><span class="sxs-lookup"><span data-stu-id="96215-177">For example, a compiler might statically determine that a local variable in a block is only used for a small portion of that block.</span></span> <span data-ttu-id="96215-178">Bu analizi kullanarak, derleyici değişkenin depolama içeren bloğun'dan daha kısa bir ömre sahip sonuçlanır kod üretebilir.</span><span class="sxs-lookup"><span data-stu-id="96215-178">Using this analysis, the compiler could generate code that results in the variable's storage having a shorter lifetime than its containing block.</span></span>

<span data-ttu-id="96215-179">Yerel başvuru değişkeni tarafından başvurulan depolama konusu yerel başvuru değişkenin kullanım ömrünün bağımsız olarak geri kazanılır ([otomatik bellek yönetimi](basic-concepts.md#automatic-memory-management)).</span><span class="sxs-lookup"><span data-stu-id="96215-179">The storage referred to by a local reference variable is reclaimed independently of the lifetime of that local reference variable ([Automatic memory management](basic-concepts.md#automatic-memory-management)).</span></span>

## <a name="default-values"></a><span data-ttu-id="96215-180">Varsayılan değerler</span><span class="sxs-lookup"><span data-stu-id="96215-180">Default values</span></span>

<span data-ttu-id="96215-181">Değişkenleri aşağıdaki kategorileri otomatik olarak varsayılan değerlerine başlatılır:</span><span class="sxs-lookup"><span data-stu-id="96215-181">The following categories of variables are automatically initialized to their default values:</span></span>

*  <span data-ttu-id="96215-182">Statik değişkenler.</span><span class="sxs-lookup"><span data-stu-id="96215-182">Static variables.</span></span>
*  <span data-ttu-id="96215-183">Örnek değişkenleri sınıf örneği.</span><span class="sxs-lookup"><span data-stu-id="96215-183">Instance variables of class instances.</span></span>
*  <span data-ttu-id="96215-184">Dizi öğeleri.</span><span class="sxs-lookup"><span data-stu-id="96215-184">Array elements.</span></span>

<span data-ttu-id="96215-185">Bir değişkenin varsayılan değeri, değişkenin türüne bağlıdır ve aşağıdaki şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="96215-185">The default value of a variable depends on the type of the variable and is determined as follows:</span></span>

*  <span data-ttu-id="96215-186">Bir değişken için bir *value_type*, varsayılan değer tarafından hesaplanan değer aynıdır *value_type*ait varsayılan oluşturucu ([varsayılan oluşturucular](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="96215-186">For a variable of a *value_type*, the default value is the same as the value computed by the *value_type*'s default constructor ([Default constructors](types.md#default-constructors)).</span></span>
*  <span data-ttu-id="96215-187">Bir değişken için bir *reference_type*, varsayılan değer `null`.</span><span class="sxs-lookup"><span data-stu-id="96215-187">For a variable of a *reference_type*, the default value is `null`.</span></span>

<span data-ttu-id="96215-188">Başlatma için varsayılan değerler, genellikle bellek yöneticisi sağlayarak gerçekleştirilir veya çöp toplayıcı kullanılmak için ayrılan önce tüm BITS sıfıra bellek başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="96215-188">Initialization to default values is typically done by having the memory manager or garbage collector initialize memory to all-bits-zero before it is allocated for use.</span></span> <span data-ttu-id="96215-189">Bu nedenle, null başvuru temsil etmek için tüm bitler sıfır kullanmak uygundur.</span><span class="sxs-lookup"><span data-stu-id="96215-189">For this reason, it is convenient to use all-bits-zero to represent the null reference.</span></span>

## <a name="definite-assignment"></a><span data-ttu-id="96215-190">Kesin atama</span><span class="sxs-lookup"><span data-stu-id="96215-190">Definite assignment</span></span>

<span data-ttu-id="96215-191">Belirli bir konumda işlevi üyesinin yürütülebilir kod, bir değişken olarak kabul edilir ***kesinlikle atanan*** derleyici, belirli statik akış analizi tarafından kanıtlayabilirsiniz varsa ([kesin belirlemek için kesin kurallar atama](variables.md#precise-rules-for-determining-definite-assignment)), değişken otomatik olarak başlatılmış veya en az bir atama işleminin hedefi olmuştur.</span><span class="sxs-lookup"><span data-stu-id="96215-191">At a given location in the executable code of a function member, a variable is said to be ***definitely assigned*** if the compiler can prove, by a particular static flow analysis ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)), that the variable has been automatically initialized or has been the target of at least one assignment.</span></span> <span data-ttu-id="96215-192">Peter'in belirtildiği gibi belirli atama onayına kurallar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="96215-192">Informally stated, the rules of definite assignment are:</span></span>

*  <span data-ttu-id="96215-193">Başlangıçta atanan bir değişken ([değişkenleri başlangıçta atanan](variables.md#initially-assigned-variables)) her zaman kesin atanmış olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="96215-193">An initially assigned variable ([Initially assigned variables](variables.md#initially-assigned-variables)) is always considered definitely assigned.</span></span>
*  <span data-ttu-id="96215-194">Başlangıçta atanmamış bir değişken ([başlangıçta değişkenleri atanmamış](variables.md#initially-unassigned-variables)) bu konuma önde gelen tüm olası yürütme yolları aşağıdakilerden en az birini içeriyorsa, belirli bir konumda kesinlikle atanmış olarak değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="96215-194">An initially unassigned variable ([Initially unassigned variables](variables.md#initially-unassigned-variables)) is considered definitely assigned at a given location if all possible execution paths leading to that location contain at least one of the following:</span></span>
    * <span data-ttu-id="96215-195">Basit atama ([basit atama](expressions.md#simple-assignment)) sol işlenen değişken olduğu.</span><span class="sxs-lookup"><span data-stu-id="96215-195">A simple assignment ([Simple assignment](expressions.md#simple-assignment)) in which the variable is the left operand.</span></span>
    * <span data-ttu-id="96215-196">Çağrı ifadesi ([çağrı ifadeleri](expressions.md#invocation-expressions)) veya nesne oluşturma ifadesi ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)), değişkeni bir output parametresi olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="96215-196">An invocation expression ([Invocation expressions](expressions.md#invocation-expressions)) or object creation expression ([Object creation expressions](expressions.md#object-creation-expressions)) that passes the variable as an output parameter.</span></span>
    * <span data-ttu-id="96215-197">Bir yerel değişken bildiriminde yerel bir değişken için ([yerel değişken bildirimlerini](statements.md#local-variable-declarations)) içeren değişken başlatıcı.</span><span class="sxs-lookup"><span data-stu-id="96215-197">For a local variable, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) that includes a variable initializer.</span></span>

<span data-ttu-id="96215-198">Yukarıdaki resmi olmayan kuralları temel resmi belirtimi açıklanan [değişkenleri başlangıçta atanan](variables.md#initially-assigned-variables), [başlangıçta değişkenleri atanmamış](variables.md#initially-unassigned-variables), ve [kesin kurallar belirlemek için belirli atama onayına](variables.md#precise-rules-for-determining-definite-assignment).</span><span class="sxs-lookup"><span data-stu-id="96215-198">The formal specification underlying the above informal rules is described in [Initially assigned variables](variables.md#initially-assigned-variables), [Initially unassigned variables](variables.md#initially-unassigned-variables), and [Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment).</span></span>

<span data-ttu-id="96215-199">Örnek değişkenleri belirli atama onayına durumlarını bir *struct_type* değişkeni topluca de ayrı ayrı olarak izlenir.</span><span class="sxs-lookup"><span data-stu-id="96215-199">The definite assignment states of instance variables of a *struct_type* variable are tracked individually as well as collectively.</span></span> <span data-ttu-id="96215-200">Yukarıdaki kurallara ek aşağıdaki kurallar geçerli *struct_type* değişkenleri ve onların örneği değişkenleri:</span><span class="sxs-lookup"><span data-stu-id="96215-200">In additional to the rules above, the following rules apply to *struct_type* variables and their instance variables:</span></span>

*  <span data-ttu-id="96215-201">Bir örnek değişkeni kesinlikle atanan kabul edildiği kapsayıcı *struct_type* değişkeni kesinlikle atanmış olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="96215-201">An instance variable is considered definitely assigned if its containing *struct_type* variable is considered definitely assigned.</span></span>
*  <span data-ttu-id="96215-202">A *struct_type* her örneği değişkenlerini kesinlikle atanan kabul ediliyorsa değişkeni kesinlikle atanan değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="96215-202">A *struct_type* variable is considered definitely assigned if each of its instance variables is considered definitely assigned.</span></span>

<span data-ttu-id="96215-203">Belirli atama onayına şu bağlamlarda bir gereksinimdir:</span><span class="sxs-lookup"><span data-stu-id="96215-203">Definite assignment is a requirement in the following contexts:</span></span>

*  <span data-ttu-id="96215-204">Bir değişken değerini burada elde edilen her bir konumda kesinlikle atanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="96215-204">A variable must be definitely assigned at each location where its value is obtained.</span></span> <span data-ttu-id="96215-205">Bu, tanımsız değerler hiçbir zaman gerçekleşmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="96215-205">This ensures that undefined values never occur.</span></span> <span data-ttu-id="96215-206">Aşağıdakiler haricinde değişkeninin değerini edinme oluşumu bir ifade bir değişken olarak kabul edilir</span><span class="sxs-lookup"><span data-stu-id="96215-206">The occurrence of a variable in an expression is considered to obtain the value of the variable, except when</span></span>
    * <span data-ttu-id="96215-207">sol işleneni, bir basit atama değişkenidir,</span><span class="sxs-lookup"><span data-stu-id="96215-207">the variable is the left operand of a simple assignment,</span></span>
    * <span data-ttu-id="96215-208">değişkeni bir output parametresi olarak geçirilir veya</span><span class="sxs-lookup"><span data-stu-id="96215-208">the variable is passed as an output parameter, or</span></span>
    * <span data-ttu-id="96215-209">Bu değişken bir *struct_type* değişkeni ve üye erişimi sol işleneni gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="96215-209">the variable is a *struct_type* variable and occurs as the left operand of a member access.</span></span>
*  <span data-ttu-id="96215-210">Burada başvuru parametre olarak geçirilen her bir konumdaki kesinlikle bir değişkene atanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="96215-210">A variable must be definitely assigned at each location where it is passed as a reference parameter.</span></span> <span data-ttu-id="96215-211">Bu, çağrılan işlev üyesi başlangıçta atanan başvuru parametresi düşünebilirsiniz sağlar.</span><span class="sxs-lookup"><span data-stu-id="96215-211">This ensures that the function member being invoked can consider the reference parameter initially assigned.</span></span>
*  <span data-ttu-id="96215-212">Tüm çıkış parametreleri işlevi üyesinin işlevi üye döndüğü her konumda kesinlikle atanmalıdır (aracılığıyla bir `return` deyimi veya işlevi üyesinin gövdesi sona erdikten yürütme aracılığıyla).</span><span class="sxs-lookup"><span data-stu-id="96215-212">All output parameters of a function member must be definitely assigned at each location where the function member returns (through a `return` statement or through execution reaching the end of the function member body).</span></span> <span data-ttu-id="96215-213">Bu işlev üyeleri tanımsız değerler çıkış parametreleri, bu nedenle bir output parametresi olarak bir değişken eşdeğer değişkenine bir atamayı alan bir işlev üye çağrısı dikkate alınması gereken derleyici etkinleştirme döndürmediğine sağlar.</span><span class="sxs-lookup"><span data-stu-id="96215-213">This ensures that function members do not return undefined values in output parameters, thus enabling the compiler to consider a function member invocation that takes a variable as an output parameter equivalent to an assignment to the variable.</span></span>
*  <span data-ttu-id="96215-214">`this` Değişkenine bir *struct_type* örnek oluşturucusu Bu örnek oluşturucusu döndüğü her konumda kesinlikle da atanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="96215-214">The `this` variable of a *struct_type* instance constructor must be definitely assigned at each location where that instance constructor returns.</span></span>

### <a name="initially-assigned-variables"></a><span data-ttu-id="96215-215">Başlangıçta atanan değişkenleri</span><span class="sxs-lookup"><span data-stu-id="96215-215">Initially assigned variables</span></span>

<span data-ttu-id="96215-216">Değişkenleri aşağıdaki kategorileri olarak başlangıçta atanan sınıflandırılır:</span><span class="sxs-lookup"><span data-stu-id="96215-216">The following categories of variables are classified as initially assigned:</span></span>

*  <span data-ttu-id="96215-217">Statik değişkenler.</span><span class="sxs-lookup"><span data-stu-id="96215-217">Static variables.</span></span>
*  <span data-ttu-id="96215-218">Örnek değişkenleri sınıf örneği.</span><span class="sxs-lookup"><span data-stu-id="96215-218">Instance variables of class instances.</span></span>
*  <span data-ttu-id="96215-219">Başlangıçta atanan yapı değişkenleri değişkenlerinin örneği.</span><span class="sxs-lookup"><span data-stu-id="96215-219">Instance variables of initially assigned struct variables.</span></span>
*  <span data-ttu-id="96215-220">Dizi öğeleri.</span><span class="sxs-lookup"><span data-stu-id="96215-220">Array elements.</span></span>
*  <span data-ttu-id="96215-221">Parametre değeri.</span><span class="sxs-lookup"><span data-stu-id="96215-221">Value parameters.</span></span>
*  <span data-ttu-id="96215-222">Başvuru parametreleri.</span><span class="sxs-lookup"><span data-stu-id="96215-222">Reference parameters.</span></span>
*  <span data-ttu-id="96215-223">İçinde bildirilmiş değişkenlerin bir `catch` yan tümcesi veya `foreach` deyimi.</span><span class="sxs-lookup"><span data-stu-id="96215-223">Variables declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="initially-unassigned-variables"></a><span data-ttu-id="96215-224">Başlangıçta atanmamış değişkenleri</span><span class="sxs-lookup"><span data-stu-id="96215-224">Initially unassigned variables</span></span>

<span data-ttu-id="96215-225">Değişkenleri aşağıdaki kategorileri, ilk olarak atanmamış sınıflandırılır:</span><span class="sxs-lookup"><span data-stu-id="96215-225">The following categories of variables are classified as initially unassigned:</span></span>

*  <span data-ttu-id="96215-226">Başlangıçta atanmamış yapı değişkenleri değişkenlerinin örneği.</span><span class="sxs-lookup"><span data-stu-id="96215-226">Instance variables of initially unassigned struct variables.</span></span>
*  <span data-ttu-id="96215-227">Çıktı parametreleri de dahil olmak üzere, `this` yapısı örneği oluşturucular değişken.</span><span class="sxs-lookup"><span data-stu-id="96215-227">Output parameters, including the `this` variable of struct instance constructors.</span></span>
*  <span data-ttu-id="96215-228">Bildirilen yerel değişkenler hariç bir `catch` yan tümcesi veya `foreach` deyimi.</span><span class="sxs-lookup"><span data-stu-id="96215-228">Local variables, except those declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="precise-rules-for-determining-definite-assignment"></a><span data-ttu-id="96215-229">Belirli atama onayına belirlemek için kesin kurallar</span><span class="sxs-lookup"><span data-stu-id="96215-229">Precise rules for determining definite assignment</span></span>

<span data-ttu-id="96215-230">Kullanılan her bir değişken kesinlikle atandığını belirlemek için derleyici Bu bölümde açıklanan eşdeğer olan bir işlemi kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="96215-230">In order to determine that each used variable is definitely assigned, the compiler must use a process that is equivalent to the one described in this section.</span></span>

<span data-ttu-id="96215-231">Derleyici, bir veya daha fazla başlangıçta atanmamış değişkenleri her işlevi üyesinin gövdesi işler.</span><span class="sxs-lookup"><span data-stu-id="96215-231">The compiler processes the body of each function member that has one or more initially unassigned variables.</span></span> <span data-ttu-id="96215-232">Başlangıçta atanmamış her değişken için *v*, derleyici belirleyen bir ***kesin atama durumu*** için *v* her işlev üyesi aşağıdaki noktaları:</span><span class="sxs-lookup"><span data-stu-id="96215-232">For each initially unassigned variable *v*, the compiler determines a ***definite assignment state*** for *v* at each of the following points in the function member:</span></span>

*  <span data-ttu-id="96215-233">Her deyimin başında</span><span class="sxs-lookup"><span data-stu-id="96215-233">At the beginning of each statement</span></span>
*  <span data-ttu-id="96215-234">Bir uç noktada ([uç noktaları ve ulaşılabilirliği](statements.md#end-points-and-reachability)) her deyimin</span><span class="sxs-lookup"><span data-stu-id="96215-234">At the end point ([End points and reachability](statements.md#end-points-and-reachability)) of each statement</span></span>
*  <span data-ttu-id="96215-235">Her yay üzerinde hangi denetimi başka bir deyimi veya deyim uç noktasına aktarır</span><span class="sxs-lookup"><span data-stu-id="96215-235">On each arc which transfers control to another statement or to the end point of a statement</span></span>
*  <span data-ttu-id="96215-236">Her bir ifadenin başında</span><span class="sxs-lookup"><span data-stu-id="96215-236">At the beginning of each expression</span></span>
*  <span data-ttu-id="96215-237">Her bir ifadenin sonunda</span><span class="sxs-lookup"><span data-stu-id="96215-237">At the end of each expression</span></span>

<span data-ttu-id="96215-238">Belirli atama onayına durumunu *v* şöyle olabilir:</span><span class="sxs-lookup"><span data-stu-id="96215-238">The definite assignment state of *v* can be either:</span></span>

*  <span data-ttu-id="96215-239">Kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="96215-239">Definitely assigned.</span></span> <span data-ttu-id="96215-240">Bu noktaya tüm olası denetim akış üzerinde gösterir *v* değeri atandı.</span><span class="sxs-lookup"><span data-stu-id="96215-240">This indicates that on all possible control flows to this point, *v* has been assigned a value.</span></span>
*  <span data-ttu-id="96215-241">Kesinlikle atanmamış.</span><span class="sxs-lookup"><span data-stu-id="96215-241">Not definitely assigned.</span></span> <span data-ttu-id="96215-242">Bir değişken türünde bir ifadenin sonunda durumu için `bool`, Mayıs kesin olarak atanmamış (ancak zorunlu değildir) bir değişken durumu aşağıdaki alt durumlarından birine ayrılır:</span><span class="sxs-lookup"><span data-stu-id="96215-242">For the state of a variable at the end of an expression of type `bool`, the state of a variable that isn't definitely assigned may (but doesn't necessarily) fall into one of the following sub-states:</span></span>
    * <span data-ttu-id="96215-243">Kesinlikle true ifadeden sonra atanır.</span><span class="sxs-lookup"><span data-stu-id="96215-243">Definitely assigned after true expression.</span></span> <span data-ttu-id="96215-244">Bu durumu bildiren *v* Boole ifade true değerlendirilir, ancak Boole ifade false değerlendirildiğinde mutlaka atanmamış kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="96215-244">This state indicates that *v* is definitely assigned if the boolean expression evaluated as true, but is not necessarily assigned if the boolean expression evaluated as false.</span></span>
    * <span data-ttu-id="96215-245">Yanlış ifade sonra kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="96215-245">Definitely assigned after false expression.</span></span> <span data-ttu-id="96215-246">Bu durumu bildiren *v* Boole ifade false değerlendirildi, ancak Boole ifade true değerlendirildiğinde mutlaka atanmamış kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="96215-246">This state indicates that *v* is definitely assigned if the boolean expression evaluated as false, but is not necessarily assigned if the boolean expression evaluated as true.</span></span>

<span data-ttu-id="96215-247">Aşağıdaki kurallar yöneten nasıl bir değişkenin durum *v* her konumda belirlenir.</span><span class="sxs-lookup"><span data-stu-id="96215-247">The following rules govern how the state of a variable *v* is determined at each location.</span></span>

#### <a name="general-rules-for-statements"></a><span data-ttu-id="96215-248">İfadeler için genel kurallar</span><span class="sxs-lookup"><span data-stu-id="96215-248">General rules for statements</span></span>

*  <span data-ttu-id="96215-249">*v* kesinlikle işlevi üyesinin gövdesi başında atanmadı.</span><span class="sxs-lookup"><span data-stu-id="96215-249">*v* is not definitely assigned at the beginning of a function member body.</span></span>
*  <span data-ttu-id="96215-250">*v* ulaşılamaz herhangi bir deyimle başında kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="96215-250">*v* is definitely assigned at the beginning of any unreachable statement.</span></span>
*  <span data-ttu-id="96215-251">Belirli atama onayına durumunu *v* herhangi bir deyimle başında belirli atama onayına durumunu denetleyerek belirlenir *v* , başına hedefleyen tüm denetim akışı aktarımları hakkında deyimi.</span><span class="sxs-lookup"><span data-stu-id="96215-251">The definite assignment state of *v* at the beginning of any other statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the beginning of that statement.</span></span> <span data-ttu-id="96215-252">Varsa (ve yalnızca) *v* kesinlikle böyle tüm denetim akışı aktarımları sonra atanan *v* deyimin başında kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="96215-252">If (and only if) *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the beginning of the statement.</span></span> <span data-ttu-id="96215-253">Olası denetim akış aktarımları kümesini deyimi ulaşılabilirlik denetimi olduğu gibi aynı şekilde belirlenir ([uç noktaları ve ulaşılabilirliği](statements.md#end-points-and-reachability)).</span><span class="sxs-lookup"><span data-stu-id="96215-253">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>
*  <span data-ttu-id="96215-254">Belirli atama onayına durumunu *v* bloğunun, uç noktada `checked`, `unchecked`, `if`, `while`, `do`, `for`, `foreach`, `lock`, `using`, veya `switch` deyimi belirli atama onayına durumunu denetleyerek belirlenir *v* Sadeliği bitiş noktasını hedefleyen tüm denetim akışı aktarımları üzerinde.</span><span class="sxs-lookup"><span data-stu-id="96215-254">The definite assignment state of *v* at the end point of a block, `checked`, `unchecked`, `if`, `while`, `do`, `for`, `foreach`, `lock`, `using`, or `switch` statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the end point of that statement.</span></span> <span data-ttu-id="96215-255">Varsa *v* kesinlikle böyle tüm denetim akışı aktarımları sonra atanan *v* deyim bitiş noktasında kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="96215-255">If *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="96215-256">Aksi durumda; *v* kesinlikle deyim bitiş noktasında atanmadı.</span><span class="sxs-lookup"><span data-stu-id="96215-256">Otherwise; *v* is not definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="96215-257">Olası denetim akış aktarımları kümesini deyimi ulaşılabilirlik denetimi olduğu gibi aynı şekilde belirlenir ([uç noktaları ve ulaşılabilirliği](statements.md#end-points-and-reachability)).</span><span class="sxs-lookup"><span data-stu-id="96215-257">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>

#### <a name="block-statements-checked-and-unchecked-statements"></a><span data-ttu-id="96215-258">Blok deyimleri, işaretli ve işaretsiz deyimleri</span><span class="sxs-lookup"><span data-stu-id="96215-258">Block statements, checked, and unchecked statements</span></span>

<span data-ttu-id="96215-259">Belirli atama onayına durumunu *v* denetimde deyim listesinin bloğundaki ilk deyimi (veya deyim listesinin boş olup olmadığını bloğunun uç noktası) aktarımı kesinatamadeyimininaynıdır*v* blokta `checked`, veya `unchecked` deyimi.</span><span class="sxs-lookup"><span data-stu-id="96215-259">The definite assignment state of *v* on the control transfer to the first statement of the statement list in the block (or to the end point of the block, if the statement list is empty) is the same as the definite assignment statement of *v* before the block, `checked`, or `unchecked` statement.</span></span>

#### <a name="expression-statements"></a><span data-ttu-id="96215-260">İfade deyimleri</span><span class="sxs-lookup"><span data-stu-id="96215-260">Expression statements</span></span>

<span data-ttu-id="96215-261">Bir ifade deyimi için *stmt* ifade oluşur *expr*:</span><span class="sxs-lookup"><span data-stu-id="96215-261">For an expression statement *stmt* that consists of the expression *expr*:</span></span>

*  <span data-ttu-id="96215-262">*v* başındaki aynı belirli atama onayına durumuna sahip *expr* olarak başındaki *stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-262">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="96215-263">Varsa *v* kesinlikle sonunda atanmışsa *expr*, kesinlikle bitiş noktasında atanan *stmt*; Aksi takdirde; Bitişnoktasındakesinlikleatanmadı*stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-263">If *v* if definitely assigned at the end of *expr*, it is definitely assigned at the end point of *stmt*; otherwise; it is not definitely assigned at the end point of *stmt*.</span></span>

#### <a name="declaration-statements"></a><span data-ttu-id="96215-264">Bildirim deyimleri</span><span class="sxs-lookup"><span data-stu-id="96215-264">Declaration statements</span></span>

*  <span data-ttu-id="96215-265">Varsa *stmt* sonra bildirim deyimi başlatıcılar olmadan *v* uç noktada aynı belirli atama onayına durumuna sahip *stmt* olarak başındaki*stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-265">If *stmt* is a declaration statement without initializers, then *v* has the same definite assignment state at the end point of *stmt* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="96215-266">Varsa *stmt* belirli atama onayına durumudur başlatıcıları, bir bildirim deyiminin ardından *v* belirlenir gibi *stmt* olan bir deyim listesiyle bir atama Her bir başlatıcı (bildirim) sırasına göre bildirimiyle bildirimi.</span><span class="sxs-lookup"><span data-stu-id="96215-266">If *stmt* is a declaration statement with initializers, then the definite assignment state for *v* is determined as if *stmt* were a statement list, with one assignment statement for each declaration with an initializer (in the order of declaration).</span></span>

#### <a name="if-statements"></a><span data-ttu-id="96215-267">Varsa deyimleri</span><span class="sxs-lookup"><span data-stu-id="96215-267">If statements</span></span>

<span data-ttu-id="96215-268">İçin bir `if` deyimi *stmt* formun:</span><span class="sxs-lookup"><span data-stu-id="96215-268">For an `if` statement *stmt* of the form:</span></span>
```csharp
if ( expr ) then_stmt else else_stmt
```

*  <span data-ttu-id="96215-269">*v* başındaki aynı belirli atama onayına durumuna sahip *expr* olarak başındaki *stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-269">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="96215-270">Varsa *v* sonunda kesinlikle atanır *expr*, denetim akışı aktarımı üzerinde kesinlikle atandıktan sonra *then_stmt* ve ya da *else_stmt*  veya uç nokta *stmt* başka hiçbir yan tümcesi ise.</span><span class="sxs-lookup"><span data-stu-id="96215-270">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt* and to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="96215-271">Varsa *v* sonunda "kesinlikle true ifadeden sonra atanan" durumuna sahip *expr*, denetim akışı aktarımı üzerinde kesinlikle atandıktan sonra *then_stmt*değil denetim akışı aktarımı ya da kesinlikle atanan *else_stmt* veya uç nokta *stmt* başka hiçbir yan tümcesi ise.</span><span class="sxs-lookup"><span data-stu-id="96215-271">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt*, and not definitely assigned on the control flow transfer to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="96215-272">Varsa *v* sonunda "kesinlikle false ifadeden sonra atanan" durumuna sahip *expr*, denetim akışı aktarımı üzerinde kesinlikle atandıktan sonra *else_stmt*değil denetim akışı aktarımı üzerinde kesinlikle atanan *then_stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-272">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *else_stmt*, and not definitely assigned on the control flow transfer to *then_stmt*.</span></span> <span data-ttu-id="96215-273">Kesinlikle bitiş noktasını atanan *stmt* kesinlikle bitiş noktasını atanır ve yalnızca, *then_stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-273">It is definitely assigned at the end-point of *stmt* if and only if it is definitely assigned at the end-point of *then_stmt*.</span></span>
*  <span data-ttu-id="96215-274">Aksi takdirde, *v* ya da denetim akışı aktarımı kesinlikle atanan olarak kabul edilmez *then_stmt* veya *else_stmt*, veya uç nokta  *stmt* başka hiçbir yan tümcesi ise.</span><span class="sxs-lookup"><span data-stu-id="96215-274">Otherwise, *v* is considered not definitely assigned on the control flow transfer to either the *then_stmt* or *else_stmt*, or to the end-point of *stmt* if there is no else clause.</span></span>

#### <a name="switch-statements"></a><span data-ttu-id="96215-275">Switch deyimleri</span><span class="sxs-lookup"><span data-stu-id="96215-275">Switch statements</span></span>

<span data-ttu-id="96215-276">İçinde bir `switch` deyimi *stmt* denetleyen bir ifade ile *expr*:</span><span class="sxs-lookup"><span data-stu-id="96215-276">In a `switch` statement *stmt* with a controlling expression *expr*:</span></span>

*  <span data-ttu-id="96215-277">Belirli atama onayına durumunu *v* başındaki *expr* durumunu aynı *v* başındaki *stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-277">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="96215-278">Belirli atama onayına durumunu *v* üzerinde denetim akışı erişilebilir anahtar engelleme deyimi listesine aktarımı kesin atama durumu aynıdır *v* sonunda *expr*.</span><span class="sxs-lookup"><span data-stu-id="96215-278">The definite assignment state of *v* on the control flow transfer to a reachable switch block statement list is the same as the definite assignment state of *v* at the end of *expr*.</span></span>

#### <a name="while-statements"></a><span data-ttu-id="96215-279">While deyimleri</span><span class="sxs-lookup"><span data-stu-id="96215-279">While statements</span></span>

<span data-ttu-id="96215-280">İçin bir `while` deyimi *stmt* formun:</span><span class="sxs-lookup"><span data-stu-id="96215-280">For a `while` statement *stmt* of the form:</span></span>
```csharp
while ( expr ) while_body
```

*  <span data-ttu-id="96215-281">*v* başındaki aynı belirli atama onayına durumuna sahip *expr* olarak başındaki *stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-281">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="96215-282">Varsa *v* sonunda kesinlikle atanır *expr*, denetim akışı aktarımı üzerinde kesinlikle atandıktan sonra *while_body* ve bitiş noktasına  *stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-282">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body* and to the end point of *stmt*.</span></span>
*  <span data-ttu-id="96215-283">Varsa *v* sonunda "kesinlikle true ifadeden sonra atanan" durumuna sahip *expr*, denetim akışı aktarımı üzerinde kesinlikle atandıktan sonra *while_body*, ancak değil kesinlikle bitiş noktasını atanan *stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-283">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body*, but not definitely assigned at the end-point of *stmt*.</span></span>
*  <span data-ttu-id="96215-284">Varsa *v* sonunda "kesinlikle false ifadeden sonra atanan" durumuna sahip *expr*, kesinlikle bitiş noktasını denetim akışı aktarımı üzerinde atandıktan sonra *stmt* , ancak denetim akışı aktarımı üzerinde kesinlikle atanan *while_body*.</span><span class="sxs-lookup"><span data-stu-id="96215-284">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*, but not definitely assigned on the control flow transfer to *while_body*.</span></span>

#### <a name="do-statements"></a><span data-ttu-id="96215-285">Do ifadeleri</span><span class="sxs-lookup"><span data-stu-id="96215-285">Do statements</span></span>

<span data-ttu-id="96215-286">İçin bir `do` deyimi *stmt* formun:</span><span class="sxs-lookup"><span data-stu-id="96215-286">For a `do` statement *stmt* of the form:</span></span>
```csharp
do do_body while ( expr ) ;
```

*  <span data-ttu-id="96215-287">*v* başından sonuna kadar denetim akışı aktarımı aynı belirli atama onayına durumuna sahip *stmt* için *do_body* olarak başındaki *stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-287">*v* has the same definite assignment state on the control flow transfer from the beginning of *stmt* to *do_body* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="96215-288">*v* başındaki aynı belirli atama onayına durumuna sahip *expr* gibi uç noktada *do_body*.</span><span class="sxs-lookup"><span data-stu-id="96215-288">*v* has the same definite assignment state at the beginning of *expr* as at the end point of *do_body*.</span></span>
*  <span data-ttu-id="96215-289">Varsa *v* sonunda kesinlikle atanır *expr*, kesinlikle bitiş noktasını denetim akışı aktarımı üzerinde atandıktan sonra *stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-289">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>
*  <span data-ttu-id="96215-290">Varsa *v* sonunda "kesinlikle false ifadeden sonra atanan" durumuna sahip *expr*, kesinlikle bitiş noktasını denetim akışı aktarımı üzerinde atandıktan sonra *stmt* .</span><span class="sxs-lookup"><span data-stu-id="96215-290">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>

#### <a name="for-statements"></a><span data-ttu-id="96215-291">For deyimleri</span><span class="sxs-lookup"><span data-stu-id="96215-291">For statements</span></span>

<span data-ttu-id="96215-292">Denetleme belirli atama onayına bir `for` formun deyimi:</span><span class="sxs-lookup"><span data-stu-id="96215-292">Definite assignment checking for a `for` statement of the form:</span></span>
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
<span data-ttu-id="96215-293">deyim yazılmışlar gibi gerçekleştirilir:</span><span class="sxs-lookup"><span data-stu-id="96215-293">is done as if the statement were written:</span></span>
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

<span data-ttu-id="96215-294">Varsa *for_condition* gelen atlanırsa `for` deyimi, ardından belirli atama onayına kazançlar değerlendirmesini gibi *for_condition* ile değiştirilen `true` yukarıdaki genişletme içinde .</span><span class="sxs-lookup"><span data-stu-id="96215-294">If the *for_condition* is omitted from the `for` statement, then evaluation of definite assignment proceeds as if *for_condition* were replaced with `true` in the above expansion.</span></span>

#### <a name="break-continue-and-goto-statements"></a><span data-ttu-id="96215-295">Kesme, devam etmek ve goto deyimleri</span><span class="sxs-lookup"><span data-stu-id="96215-295">Break, continue, and goto statements</span></span>

<span data-ttu-id="96215-296">Belirli atama onayına durumunu *v* kaynaklanan denetim akışı aktarımı üzerinde bir `break`, `continue`, veya `goto` deyimi, kesin atama durumu ile aynı *v* adresindeki deyimin başlangıcı.</span><span class="sxs-lookup"><span data-stu-id="96215-296">The definite assignment state of *v* on the control flow transfer caused by a `break`, `continue`, or `goto` statement is the same as the definite assignment state of *v* at the beginning of the statement.</span></span>

#### <a name="throw-statements"></a><span data-ttu-id="96215-297">Throw deyimleri</span><span class="sxs-lookup"><span data-stu-id="96215-297">Throw statements</span></span>

<span data-ttu-id="96215-298">Bir deyim için *stmt* form</span><span class="sxs-lookup"><span data-stu-id="96215-298">For a statement *stmt* of the form</span></span>
```csharp
throw expr ;
```

<span data-ttu-id="96215-299">Belirli atama onayına durumunu *v* başındaki *expr* kesin atama durumu ile aynı *v* başındaki *stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-299">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>

#### <a name="return-statements"></a><span data-ttu-id="96215-300">Return deyimleri</span><span class="sxs-lookup"><span data-stu-id="96215-300">Return statements</span></span>

<span data-ttu-id="96215-301">Bir deyim için *stmt* form</span><span class="sxs-lookup"><span data-stu-id="96215-301">For a statement *stmt* of the form</span></span>
```csharp
return expr ;
```

*  <span data-ttu-id="96215-302">Belirli atama onayına durumunu *v* başındaki *expr* kesin atama durumu ile aynı *v* başındaki *stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-302">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="96215-303">Varsa *v* , kesinlikle ya da atanmalıdır sonra bir output parametresi olan:</span><span class="sxs-lookup"><span data-stu-id="96215-303">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="96215-304">sonra *ifade*</span><span class="sxs-lookup"><span data-stu-id="96215-304">after *expr*</span></span>
    * <span data-ttu-id="96215-305">ya da sonunda `finally` bloğu bir `try` - `finally` veya `try` - `catch` - `finally` , kapsayan `return` deyimi.</span><span class="sxs-lookup"><span data-stu-id="96215-305">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

<span data-ttu-id="96215-306">Form için bir deyim stmt:</span><span class="sxs-lookup"><span data-stu-id="96215-306">For a statement stmt of the form:</span></span>
```csharp
return ;
```

*  <span data-ttu-id="96215-307">Varsa *v* , kesinlikle ya da atanmalıdır sonra bir output parametresi olan:</span><span class="sxs-lookup"><span data-stu-id="96215-307">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="96215-308">önce *stmt*</span><span class="sxs-lookup"><span data-stu-id="96215-308">before *stmt*</span></span>
    * <span data-ttu-id="96215-309">ya da sonunda `finally` bloğu bir `try` - `finally` veya `try` - `catch` - `finally` , kapsayan `return` deyimi.</span><span class="sxs-lookup"><span data-stu-id="96215-309">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

#### <a name="try-catch-statements"></a><span data-ttu-id="96215-310">Try-catch deyimleri</span><span class="sxs-lookup"><span data-stu-id="96215-310">Try-catch statements</span></span>

<span data-ttu-id="96215-311">Bir deyim için *stmt* formun:</span><span class="sxs-lookup"><span data-stu-id="96215-311">For a statement *stmt* of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  <span data-ttu-id="96215-312">Belirli atama onayına durumunu *v* başındaki *try_block* kesin atama durumu ile aynı *v* başındaki *stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-312">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="96215-313">Belirli atama onayına durumunu *v* başındaki *catch_block_i* (herhangi *miyim*) özelliğine belirli atama durumu ile aynı *v*başındaki *stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-313">The definite assignment state of *v* at the beginning of *catch_block_i* (for any *i*) is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="96215-314">Belirli atama onayına durumunu *v* uç nokta adresindeki *stmt* kesinlikle atanmış bir IF (ve yalnızca) *v* kesinlikle bitiş noktasını atanır  *try_block* ve her *catch_block_i* (için her *miyim* 1'den *n*).</span><span class="sxs-lookup"><span data-stu-id="96215-314">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) *v* is definitely assigned at the end-point of *try_block* and every *catch_block_i* (for every *i* from 1 to *n*).</span></span>

#### <a name="try-finally-statements"></a><span data-ttu-id="96215-315">Try-finally deyimleri</span><span class="sxs-lookup"><span data-stu-id="96215-315">Try-finally statements</span></span>

<span data-ttu-id="96215-316">İçin bir `try` deyimi *stmt* formun:</span><span class="sxs-lookup"><span data-stu-id="96215-316">For a `try` statement *stmt* of the form:</span></span>
```csharp
try try_block finally finally_block
```

*  <span data-ttu-id="96215-317">Belirli atama onayına durumunu *v* başındaki *try_block* kesin atama durumu ile aynı *v* başındaki *stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-317">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="96215-318">Belirli atama onayına durumunu *v* başındaki *finally_block* kesin atama durumu ile aynı *v* başındaki *stmt* .</span><span class="sxs-lookup"><span data-stu-id="96215-318">The definite assignment state of *v* at the beginning of *finally_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="96215-319">Belirli atama onayına durumunu *v* uç nokta adresindeki *stmt* kesinlikle atanmış bir IF (ve yalnızca) aşağıdakilerden en az birini doğrudur:</span><span class="sxs-lookup"><span data-stu-id="96215-319">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) at least one of the following is true:</span></span>
    * <span data-ttu-id="96215-320">*v* kesinlikle bitiş noktasını atanır *try_block*</span><span class="sxs-lookup"><span data-stu-id="96215-320">*v* is definitely assigned at the end-point of *try_block*</span></span>
    * <span data-ttu-id="96215-321">*v* kesinlikle bitiş noktasını atanır *finally_block*</span><span class="sxs-lookup"><span data-stu-id="96215-321">*v* is definitely assigned at the end-point of *finally_block*</span></span>

<span data-ttu-id="96215-322">Denetim akışı aktarımı varsa (örneğin, bir `goto` deyimi) yapılması içinde başlar *try_block*ve dışında sona eren *try_block*, ardından *v* de Bu denetim akışı aktarımını kesinlikle atanan kabul *v* kesinlikle bitiş noktasını atanır *finally_block*.</span><span class="sxs-lookup"><span data-stu-id="96215-322">If a control flow transfer (for example, a `goto` statement) is made that begins within *try_block*, and ends outside of *try_block*, then *v* is also considered definitely assigned on that control flow transfer if *v* is definitely assigned at the end-point of *finally_block*.</span></span> <span data-ttu-id="96215-323">(Bu yalnızca Eğer değildir — varsa *v* hala kesinlikle atanan değerlendirilir sonra kesinlikle bu denetim akışı aktarım başka bir nedenle atanır.)</span><span class="sxs-lookup"><span data-stu-id="96215-323">(This is not an only if—if *v* is definitely assigned for another reason on this control flow transfer, then it is still considered definitely assigned.)</span></span>

#### <a name="try-catch-finally-statements"></a><span data-ttu-id="96215-324">Try-catch-finally deyimleri</span><span class="sxs-lookup"><span data-stu-id="96215-324">Try-catch-finally statements</span></span>

<span data-ttu-id="96215-325">Analiz için kesin atama bir `try` - `catch` - `finally` formun deyimi:</span><span class="sxs-lookup"><span data-stu-id="96215-325">Definite assignment analysis for a `try`-`catch`-`finally` statement of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
<span data-ttu-id="96215-326">deyim'ymiş gibi yapılır bir `try` - `finally` deyimi kapsayan bir `try` - `catch` deyimi:</span><span class="sxs-lookup"><span data-stu-id="96215-326">is done as if the statement were a `try`-`finally` statement enclosing a `try`-`catch` statement:</span></span>
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

<span data-ttu-id="96215-327">Aşağıdaki örnek, gösterir nasıl farklı bloklarını bir `try` deyimi ([try deyimi](statements.md#the-try-statement)) belirli atama onayına etkiler.</span><span class="sxs-lookup"><span data-stu-id="96215-327">The following example demonstrates how the different blocks of a `try` statement ([The try statement](statements.md#the-try-statement)) affect definite assignment.</span></span>
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

#### <a name="foreach-statements"></a><span data-ttu-id="96215-328">Foreach deyimleri</span><span class="sxs-lookup"><span data-stu-id="96215-328">Foreach statements</span></span>

<span data-ttu-id="96215-329">İçin bir `foreach` deyimi *stmt* formun:</span><span class="sxs-lookup"><span data-stu-id="96215-329">For a `foreach` statement *stmt* of the form:</span></span>
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  <span data-ttu-id="96215-330">Belirli atama onayına durumunu *v* başındaki *expr* durumunu aynı *v* başındaki *stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-330">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="96215-331">Belirli atama onayına durumunu *v* üzerinde denetim akışı aktarımı *embedded_statement* veya bitiş noktasını *stmt* durumunu aynı *v* sonunda *expr*.</span><span class="sxs-lookup"><span data-stu-id="96215-331">The definite assignment state of *v* on the control flow transfer to *embedded_statement* or to the end point of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="using-statements"></a><span data-ttu-id="96215-332">Using deyimleri</span><span class="sxs-lookup"><span data-stu-id="96215-332">Using statements</span></span>

<span data-ttu-id="96215-333">İçin bir `using` deyimi *stmt* formun:</span><span class="sxs-lookup"><span data-stu-id="96215-333">For a `using` statement *stmt* of the form:</span></span>
```csharp
using ( resource_acquisition ) embedded_statement
```

*  <span data-ttu-id="96215-334">Belirli atama onayına durumunu *v* başındaki *resource_acquisition* durumunu aynı *v* başındaki *stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-334">The definite assignment state of *v* at the beginning of *resource_acquisition* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="96215-335">Belirli atama onayına durumunu *v* üzerinde denetim akışı aktarımı *embedded_statement* durumunu aynı *v* sonunda *resource_ Alım*.</span><span class="sxs-lookup"><span data-stu-id="96215-335">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *resource_acquisition*.</span></span>

#### <a name="lock-statements"></a><span data-ttu-id="96215-336">Kilit deyimleri</span><span class="sxs-lookup"><span data-stu-id="96215-336">Lock statements</span></span>

<span data-ttu-id="96215-337">İçin bir `lock` deyimi *stmt* formun:</span><span class="sxs-lookup"><span data-stu-id="96215-337">For a `lock` statement *stmt* of the form:</span></span>
```csharp
lock ( expr ) embedded_statement
```

*  <span data-ttu-id="96215-338">Belirli atama onayına durumunu *v* başındaki *expr* durumunu aynı *v* başındaki *stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-338">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="96215-339">Belirli atama onayına durumunu *v* üzerinde denetim akışı aktarımı *embedded_statement* durumunu aynı *v* sonunda *expr*.</span><span class="sxs-lookup"><span data-stu-id="96215-339">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="yield-statements"></a><span data-ttu-id="96215-340">Yield deyimleri</span><span class="sxs-lookup"><span data-stu-id="96215-340">Yield statements</span></span>

<span data-ttu-id="96215-341">İçin bir `yield return` deyimi *stmt* formun:</span><span class="sxs-lookup"><span data-stu-id="96215-341">For a `yield return` statement *stmt* of the form:</span></span>
```csharp
yield return expr ;
```

*  <span data-ttu-id="96215-342">Belirli atama onayına durumunu *v* başındaki *expr* durumunu aynı *v* başındaki *stmt*.</span><span class="sxs-lookup"><span data-stu-id="96215-342">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="96215-343">Belirli atama onayına durumunu *v* sonunda *stmt* durumunu aynı *v* sonunda *expr*.</span><span class="sxs-lookup"><span data-stu-id="96215-343">The definite assignment state of *v* at the end of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>
*  <span data-ttu-id="96215-344">A `yield break` deyimi belirli atama onayına durumu üzerinde hiçbir etkisi.</span><span class="sxs-lookup"><span data-stu-id="96215-344">A `yield break` statement has no effect on the definite assignment state.</span></span>

#### <a name="general-rules-for-simple-expressions"></a><span data-ttu-id="96215-345">Basit ifadeler için genel kurallar</span><span class="sxs-lookup"><span data-stu-id="96215-345">General rules for simple expressions</span></span>

<span data-ttu-id="96215-346">Bu tür deyimler için aşağıdaki kural uygulanır: değişmez değerler ([değişmez değerleri](expressions.md#literals)), basit adları ([basit adları](expressions.md#simple-names)), üye erişimi ifadeleri ([üye erişimi](expressions.md#member-access)), temel erişim dizini oluşturulmamış ifadeleri ([temel erişim](expressions.md#base-access)), `typeof` ifadeleri ([typeof işleci](expressions.md#the-typeof-operator)), varsayılan değer ifadeleri ([varsayılan değer ifadeleri ](expressions.md#default-value-expressions)) ve `nameof` ifadeleri ([Nameof ifadeleri](expressions.md#nameof-expressions)).</span><span class="sxs-lookup"><span data-stu-id="96215-346">The following rule applies to these kinds of expressions: literals ([Literals](expressions.md#literals)), simple names ([Simple names](expressions.md#simple-names)), member access expressions ([Member access](expressions.md#member-access)), non-indexed base access expressions ([Base access](expressions.md#base-access)), `typeof` expressions ([The typeof operator](expressions.md#the-typeof-operator)), default value expressions ([Default value expressions](expressions.md#default-value-expressions)) and `nameof` expressions ([Nameof expressions](expressions.md#nameof-expressions)).</span></span>

*  <span data-ttu-id="96215-347">Belirli atama onayına durumunu *v* böyle bir ifade sonunda kesin atama durumu ile aynıdır *v* ifadenin başında.</span><span class="sxs-lookup"><span data-stu-id="96215-347">The definite assignment state of *v* at the end of such an expression is the same as the definite assignment state of *v* at the beginning of the expression.</span></span>

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a><span data-ttu-id="96215-348">Katıştırılmış ifadeler ile ifadeler için genel kurallar</span><span class="sxs-lookup"><span data-stu-id="96215-348">General rules for expressions with embedded expressions</span></span>

<span data-ttu-id="96215-349">Bu tür deyimler için aşağıdaki kurallar geçerlidir: parantezli ifade ([parantezli ifade](expressions.md#parenthesized-expressions)), öğe erişimi ifadeleri ([öğe erişimi](expressions.md#element-access)), temel erişim ile ifadeler Dizin oluşturma ([temel erişim](expressions.md#base-access)), artırmak ve azaltma ifadeleri ([sonek arttırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators), [önek arttırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)), cast ifadeleri ([Cast ifadeleri](expressions.md#cast-expressions)), birli `+`, `-`, `~`, `*` ifadeleri, ikili `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` ifadeleri ([aritmetik işleçler](expressions.md#arithmetic-operators), [kaydırma işleçleri](expressions.md#shift-operators), [ilişkisel ve tür testi işleçleri](expressions.md#relational-and-type-testing-operators) [Mantıksal işleçler](expressions.md#logical-operators)), bileşik atama deyimleri ([bileşik atama](expressions.md#compound-assignment)), `checked` ve `unchecked` ifadeleri ([checked ve unchecked işleçler](expressions.md#the-checked-and-unchecked-operators)), dizi ve temsilci oluşturma ifadeleri artı ([new işleci](expressions.md#the-new-operator)).</span><span class="sxs-lookup"><span data-stu-id="96215-349">The following rules apply to these kinds of expressions: parenthesized expressions ([Parenthesized expressions](expressions.md#parenthesized-expressions)), element access expressions ([Element access](expressions.md#element-access)), base access expressions with indexing ([Base access](expressions.md#base-access)), increment and decrement expressions ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), cast expressions ([Cast expressions](expressions.md#cast-expressions)), unary `+`, `-`, `~`, `*` expressions, binary `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` expressions ([Arithmetic operators](expressions.md#arithmetic-operators), [Shift operators](expressions.md#shift-operators), [Relational and type-testing operators](expressions.md#relational-and-type-testing-operators), [Logical operators](expressions.md#logical-operators)), compound assignment expressions ([Compound assignment](expressions.md#compound-assignment)), `checked` and `unchecked` expressions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), plus array and delegate creation expressions ([The new operator](expressions.md#the-new-operator)).</span></span>

<span data-ttu-id="96215-350">Bu ifadeler sabit bir sırada koşulsuz olarak değerlendirilen bir veya daha fazla alt ifadeler vardır.</span><span class="sxs-lookup"><span data-stu-id="96215-350">Each of these expressions has one or more sub-expressions that are unconditionally evaluated in a fixed order.</span></span> <span data-ttu-id="96215-351">Örneğin, ikili `%` işleci işlecinin sol tarafı, sonra sağ taraftaki değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="96215-351">For example, the binary `%` operator evaluates the left hand side of the operator, then the right hand side.</span></span> <span data-ttu-id="96215-352">Bir dizin oluşturma işlemi, dizinli ifadeyi değerlendirir ve her birini sırayla soldan sağa doğru dizin ifadeleri değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="96215-352">An indexing operation evaluates the indexed expression, and then evaluates each of the index expressions, in order from left to right.</span></span> <span data-ttu-id="96215-353">Bir ifade için *expr*, alt ifadeler olduğu *e1, e2,..., tr*, o sırada değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="96215-353">For an expression *expr*, which has sub-expressions *e1, e2, ..., eN*, evaluated in that order:</span></span>

*  <span data-ttu-id="96215-354">Belirli atama onayına durumunu *v* başındaki *e1* başındaki kesin atama durumu ile aynı *expr*.</span><span class="sxs-lookup"><span data-stu-id="96215-354">The definite assignment state of *v* at the beginning of *e1* is the same as the definite assignment state at the beginning of *expr*.</span></span>
*  <span data-ttu-id="96215-355">Belirli atama onayına durumunu *v* başındaki *ei* (*miyim* birden büyük) önceki alt ifadenin sonunda kesin atama durumu ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="96215-355">The definite assignment state of *v* at the beginning of *ei* (*i* greater than one) is the same as the definite assignment state at the end of the previous sub-expression.</span></span>
*  <span data-ttu-id="96215-356">Kesin atama durumu *v* sonunda *expr* sonunda kesin atama durumu ile aynı *tr*</span><span class="sxs-lookup"><span data-stu-id="96215-356">The definite assignment state of *v* at the end of *expr* is the same as the definite assignment state at the end of *eN*</span></span>

#### <a name="invocation-expressions-and-object-creation-expressions"></a><span data-ttu-id="96215-357">Çağrı ifadeleri ve nesne oluşturma ifadeleri</span><span class="sxs-lookup"><span data-stu-id="96215-357">Invocation expressions and object creation expressions</span></span>

<span data-ttu-id="96215-358">Çağrı ifadesi için *expr* formun:</span><span class="sxs-lookup"><span data-stu-id="96215-358">For an invocation expression *expr* of the form:</span></span>
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
<span data-ttu-id="96215-359">veya bir nesne oluşturma ifadesi formun:</span><span class="sxs-lookup"><span data-stu-id="96215-359">or an object creation expression of the form:</span></span>
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  <span data-ttu-id="96215-360">Belirli atama onayına durumu bir başlatma ifadesi için *v* önce *primary_expression* durumunu aynı *v* önce *expr*.</span><span class="sxs-lookup"><span data-stu-id="96215-360">For an invocation expression, the definite assignment state of *v* before *primary_expression* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="96215-361">Belirli atama onayına durumu bir başlatma ifadesi için *v* önce *arg1* durumunu aynı *v* sonra *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="96215-361">For an invocation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* after *primary_expression*.</span></span>
*  <span data-ttu-id="96215-362">Bir nesne oluşturma ifadesi, kesin atama durumu için *v* önce *arg1* durumunu aynı *v* önce *expr*.</span><span class="sxs-lookup"><span data-stu-id="96215-362">For an object creation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="96215-363">Her bağımsız değişkeni *argi*, kesin atama durumu *v* sonra *argi* herhangi yoksayılıyor normal ifade kuralları tarafından belirlenir `ref` veya `out`değiştiriciler.</span><span class="sxs-lookup"><span data-stu-id="96215-363">For each argument *argi*, the definite assignment state of *v* after *argi* is determined by the normal expression rules, ignoring any `ref` or `out` modifiers.</span></span>
*  <span data-ttu-id="96215-364">Her bağımsız değişkeni *argi* herhangi *miyim* daha büyük bir özelliğine belirli atama durumu *v* önce *argi* durumu ile aynıdır *v* önceki sonra *arg*.</span><span class="sxs-lookup"><span data-stu-id="96215-364">For each argument *argi* for any *i* greater than one, the definite assignment state of *v* before *argi* is the same as the state of *v* after the previous *arg*.</span></span>
*  <span data-ttu-id="96215-365">Varsa değişkeni *v* olarak geçirilen bir `out` bağımsız değişken (yani, formun bağımsız değişken `out v`) herhangi bir bağımsız değişken, daha sonra durumunu *v* sonra *expr* kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="96215-365">If the variable *v* is passed as an `out` argument (i.e., an argument of the form `out v`) in any of the arguments, then the state of *v* after *expr* is definitely assigned.</span></span> <span data-ttu-id="96215-366">Aksi durumda; durumunu *v* sonra *expr* durumunu aynı *v* sonra *argN*.</span><span class="sxs-lookup"><span data-stu-id="96215-366">Otherwise; the state of *v* after *expr* is the same as the state of *v* after *argN*.</span></span>
*  <span data-ttu-id="96215-367">Dizi başlatıcıları için ([dizi oluşturma ifadeleri](expressions.md#array-creation-expressions)), nesne başlatıcıları ([nesne başlatıcılarda](expressions.md#object-initializers)), koleksiyon başlatıcıları ([koleksiyon başlatıcıları](expressions.md#collection-initializers)) ve Anonim nesne başlatıcıları ([anonim nesne oluşturma ifadeleri](expressions.md#anonymous-object-creation-expressions)), kesin atama durumu açısından bu yapıları tanımlanan genişletme tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="96215-367">For array initializers ([Array creation expressions](expressions.md#array-creation-expressions)), object initializers ([Object initializers](expressions.md#object-initializers)), collection initializers ([Collection initializers](expressions.md#collection-initializers)) and anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)), the definite assignment state is determined by the expansion that these constructs are defined in terms of.</span></span>

#### <a name="simple-assignment-expressions"></a><span data-ttu-id="96215-368">Basit atama deyimleri</span><span class="sxs-lookup"><span data-stu-id="96215-368">Simple assignment expressions</span></span>

<span data-ttu-id="96215-369">Bir ifade için *expr* formun `w = expr_rhs`:</span><span class="sxs-lookup"><span data-stu-id="96215-369">For an expression *expr* of the form `w = expr_rhs`:</span></span>

*  <span data-ttu-id="96215-370">Belirli atama onayına durumunu *v* önce *expr_rhs* kesin atama durumu ile aynı *v* önce *expr*.</span><span class="sxs-lookup"><span data-stu-id="96215-370">The definite assignment state of *v* before *expr_rhs* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="96215-371">Belirli atama onayına durumunu *v* sonra *expr* tarafından belirlenir:</span><span class="sxs-lookup"><span data-stu-id="96215-371">The definite assignment state of *v* after *expr* is determined by:</span></span>
   * <span data-ttu-id="96215-372">Varsa *w* olarak aynı değişken *v*, ardından belirli atama onayına durumunu *v* sonra *expr* kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="96215-372">If *w* is the same variable as *v*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="96215-373">Aksi takdirde, atama bir yapı türünün örnek oluşturucusu içinde ortaya çıkarsa *w* otomatik olarak uygulanan bir özelliği bir özellik erişim *P* yapılandırılmakta örneğinde ve *v* gizli yedekleme alanı *P*, ardından belirli atama onayına durumunu *v* sonra *expr* kesinlikle olduğu atanmış.</span><span class="sxs-lookup"><span data-stu-id="96215-373">Otherwise, if the assignment occurs within the instance constructor of a struct type, if *w* is a property access designating an automatically implemented property *P* on the instance being constructed and *v* is the hidden backing field of *P*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="96215-374">Aksi takdirde, kesin atama durumu *v* sonra *expr* kesin atama durumu ile aynı *v* sonra *expr_rhs*.</span><span class="sxs-lookup"><span data-stu-id="96215-374">Otherwise, the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_rhs*.</span></span>

#### <a name="-conditional-and-expressions"></a><span data-ttu-id="96215-375">& & (ve koşullu) ifadeleri</span><span class="sxs-lookup"><span data-stu-id="96215-375">&& (conditional AND) expressions</span></span>

<span data-ttu-id="96215-376">Bir ifade için *expr* formun `expr_first && expr_second`:</span><span class="sxs-lookup"><span data-stu-id="96215-376">For an expression *expr* of the form `expr_first && expr_second`:</span></span>

*  <span data-ttu-id="96215-377">Belirli atama onayına durumunu *v* önce *expr_first* kesin atama durumu ile aynı *v* önce *expr*.</span><span class="sxs-lookup"><span data-stu-id="96215-377">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="96215-378">Belirli atama onayına durumunu *v* önce *expr_second* varsa kesinlikle atanır durumunu *v* sonra *expr_first* ya da kesinlikle atanan veya "gerçek ifade sonra atanan kesinlikle".</span><span class="sxs-lookup"><span data-stu-id="96215-378">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after true expression".</span></span> <span data-ttu-id="96215-379">Aksi takdirde, kesinlikle atanmadı.</span><span class="sxs-lookup"><span data-stu-id="96215-379">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="96215-380">Belirli atama onayına durumunu *v* sonra *expr* tarafından belirlenir:</span><span class="sxs-lookup"><span data-stu-id="96215-380">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="96215-381">Varsa *expr_first* değerine sahip bir sabit ifade ise `false`, ardından belirli atama onayına durumunu *v* sonra *expr* kesin atama ile aynıdır durumu *v* sonra *expr_first*.</span><span class="sxs-lookup"><span data-stu-id="96215-381">If *expr_first* is a constant expression with the value `false`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="96215-382">Aksi halde, durumunu *v* sonra *expr_first* kesinlikle atandığı, ardından durumunu *v* sonra *expr* kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="96215-382">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="96215-383">Aksi halde, durumunu *v* sonra *expr_second* kesinlikle atanır ve durumunu *v* sonra *expr_first* "kesinlikle olduğu yanlış ifade sonra atanan", ardından durumunu *v* sonra *expr* kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="96215-383">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after false expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="96215-384">Aksi halde, durumunu *v* sonra *expr_second* kesinlikle atanan veya "gerçek ifade sonra atanan kesinlikle", ardından durumunu *v* sonra  *Expr* "kesinlikle true ifadeden sonra atanan".</span><span class="sxs-lookup"><span data-stu-id="96215-384">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="96215-385">Aksi halde, durumunu *v* sonra *expr_first* "false ifadesi sonra atanan kesinlikle" ve durumunu *v* sonra *expr_second* "false ifadesi sonra atanan kesinlikle" sonra durumunu *v* sonra *expr* "kesinlikle false ifadeden sonra atanan".</span><span class="sxs-lookup"><span data-stu-id="96215-385">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after false expression", and the state of *v* after *expr_second* is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="96215-386">Aksi takdirde, durumunu *v* sonra *expr* kesinlikle atanmadı.</span><span class="sxs-lookup"><span data-stu-id="96215-386">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="96215-387">Örnekte</span><span class="sxs-lookup"><span data-stu-id="96215-387">In the example</span></span>
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
<span data-ttu-id="96215-388">değişken `i` kesinlikle atanan katıştırılmış deyimleri biri olarak kabul edilir bir `if` deyimi ancak başka birinde olmayan.</span><span class="sxs-lookup"><span data-stu-id="96215-388">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="96215-389">İçinde `if` yöntemi deyiminde `F`, değişkeni `i` katıştırılmış ilk deyimde çünkü kesinlikle atanan ifade yürütülmesini `(i = y)` önce her zaman bu katıştırılmış deyim yürütme.</span><span class="sxs-lookup"><span data-stu-id="96215-389">In the `if` statement in method `F`, the variable `i` is definitely assigned in the first embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="96215-390">Buna karşılık, değişken `i` ikinci katıştırılmış deyim kesinlikle beri atanmadı `x >= 0` değişkeninde kaynaklanan yanlış sınanmıştır `i` atanmamış.</span><span class="sxs-lookup"><span data-stu-id="96215-390">In contrast, the variable `i` is not definitely assigned in the second embedded statement, since `x >= 0` might have tested false, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-conditional-or-expressions"></a><span data-ttu-id="96215-391">|| (koşullu veya) ifadeleri</span><span class="sxs-lookup"><span data-stu-id="96215-391">|| (conditional OR) expressions</span></span>

<span data-ttu-id="96215-392">Bir ifade için *expr* formun `expr_first || expr_second`:</span><span class="sxs-lookup"><span data-stu-id="96215-392">For an expression *expr* of the form `expr_first || expr_second`:</span></span>

*  <span data-ttu-id="96215-393">Belirli atama onayına durumunu *v* önce *expr_first* kesin atama durumu ile aynı *v* önce *expr*.</span><span class="sxs-lookup"><span data-stu-id="96215-393">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="96215-394">Belirli atama onayına durumunu *v* önce *expr_second* varsa kesinlikle atanır durumunu *v* sonra *expr_first* ya da kesinlikle atanan veya "false ifadesi sonra atanan kesinlikle".</span><span class="sxs-lookup"><span data-stu-id="96215-394">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after false expression".</span></span> <span data-ttu-id="96215-395">Aksi takdirde, kesinlikle atanmadı.</span><span class="sxs-lookup"><span data-stu-id="96215-395">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="96215-396">Belirli atama onayına deyiminin *v* sonra *expr* tarafından belirlenir:</span><span class="sxs-lookup"><span data-stu-id="96215-396">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="96215-397">Varsa *expr_first* değerine sahip bir sabit ifade ise `true`, ardından belirli atama onayına durumunu *v* sonra *expr* kesin atama ile aynıdır durumu *v* sonra *expr_first*.</span><span class="sxs-lookup"><span data-stu-id="96215-397">If *expr_first* is a constant expression with the value `true`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="96215-398">Aksi halde, durumunu *v* sonra *expr_first* kesinlikle atandığı, ardından durumunu *v* sonra *expr* kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="96215-398">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="96215-399">Aksi halde, durumunu *v* sonra *expr_second* kesinlikle atanır ve durumunu *v* sonra *expr_first* "kesinlikle olduğu true deyim sonra atanan", ardından durumunu *v* sonra *expr* kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="96215-399">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after true expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="96215-400">Aksi halde, durumunu *v* sonra *expr_second* kesinlikle atanan veya "false ifadesi sonra atanan kesinlikle", ardından durumunu *v* sonra*expr* "kesinlikle false ifadeden sonra atanan".</span><span class="sxs-lookup"><span data-stu-id="96215-400">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="96215-401">Aksi halde, durumunu *v* sonra *expr_first* "true deyim sonra atanan kesinlikle" ve durumunu *v* sonra *expr_second*"true deyim sonra atanan kesinlikle" sonra durumunu *v* sonra *expr* "kesinlikle true ifadeden sonra atanan".</span><span class="sxs-lookup"><span data-stu-id="96215-401">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after true expression", and the state of *v* after *expr_second* is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="96215-402">Aksi takdirde, durumunu *v* sonra *expr* kesinlikle atanmadı.</span><span class="sxs-lookup"><span data-stu-id="96215-402">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="96215-403">Örnekte</span><span class="sxs-lookup"><span data-stu-id="96215-403">In the example</span></span>
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
<span data-ttu-id="96215-404">değişken `i` kesinlikle atanan katıştırılmış deyimleri biri olarak kabul edilir bir `if` deyimi ancak başka birinde olmayan.</span><span class="sxs-lookup"><span data-stu-id="96215-404">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="96215-405">İçinde `if` yöntemi deyiminde `G`, değişkeni `i` ikinci katıştırılmış deyiminde çünkü kesinlikle atanan ifade yürütülmesini `(i = y)` önce her zaman bu katıştırılmış deyim yürütme.</span><span class="sxs-lookup"><span data-stu-id="96215-405">In the `if` statement in method `G`, the variable `i` is definitely assigned in the second embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="96215-406">Buna karşılık, değişken `i` kesinlikle ilk katıştırılmış deyim beri atanmadı `x >= 0` değişkeninde kaynaklanan doğru sınanmıştır `i` atanmamış.</span><span class="sxs-lookup"><span data-stu-id="96215-406">In contrast, the variable `i` is not definitely assigned in the first embedded statement, since `x >= 0` might have tested true, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-logical-negation-expressions"></a><span data-ttu-id="96215-407">!</span><span class="sxs-lookup"><span data-stu-id="96215-407">!</span></span> <span data-ttu-id="96215-408">(mantıksal olumsuzlama) ifadeleri</span><span class="sxs-lookup"><span data-stu-id="96215-408">(logical negation) expressions</span></span>

<span data-ttu-id="96215-409">Bir ifade için *expr* formun `! expr_operand`:</span><span class="sxs-lookup"><span data-stu-id="96215-409">For an expression *expr* of the form `! expr_operand`:</span></span>

*  <span data-ttu-id="96215-410">Belirli atama onayına durumunu *v* önce *expr_operand* kesin atama durumu ile aynı *v* önce *expr*.</span><span class="sxs-lookup"><span data-stu-id="96215-410">The definite assignment state of *v* before *expr_operand* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="96215-411">Belirli atama onayına durumunu *v* sonra *expr* tarafından belirlenir:</span><span class="sxs-lookup"><span data-stu-id="96215-411">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="96215-412">Varsa durumunu *v* sonra \* expr_operand \* kesinlikle atandığı, ardından durumunu *v* sonra *expr* kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="96215-412">If the state of *v* after \*expr_operand \*is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="96215-413">Varsa durumunu *v* sonra \* expr_operand \* kesinlikle atanmadı, ardından durumunu *v* sonra *expr* kesinlikle atanmadı.</span><span class="sxs-lookup"><span data-stu-id="96215-413">If the state of *v* after \*expr_operand \*is not definitely assigned, then the state of *v* after *expr* is not definitely assigned.</span></span>
    * <span data-ttu-id="96215-414">Varsa durumunu *v* sonra \* expr_operand \* "false ifadesi sonra atanan kesinlikle" sonra durumunu *v* sonra *expr* "kesinlikle true sonra atanan "ifadesi".</span><span class="sxs-lookup"><span data-stu-id="96215-414">If the state of *v* after \*expr_operand \*is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="96215-415">Varsa durumunu *v* sonra \* expr_operand \* "true deyim sonra atanan kesinlikle" sonra durumunu *v* sonra *expr* "kesinlikle false sonra atanan "ifadesi".</span><span class="sxs-lookup"><span data-stu-id="96215-415">If the state of *v* after \*expr_operand \*is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>

#### <a name="-null-coalescing-expressions"></a><span data-ttu-id="96215-416">??</span><span class="sxs-lookup"><span data-stu-id="96215-416">??</span></span> <span data-ttu-id="96215-417">ifadeler (null birleşim)</span><span class="sxs-lookup"><span data-stu-id="96215-417">(null coalescing) expressions</span></span>

<span data-ttu-id="96215-418">Bir ifade için *expr* formun `expr_first ?? expr_second`:</span><span class="sxs-lookup"><span data-stu-id="96215-418">For an expression *expr* of the form `expr_first ?? expr_second`:</span></span>

*  <span data-ttu-id="96215-419">Belirli atama onayına durumunu *v* önce *expr_first* kesin atama durumu ile aynı *v* önce *expr*.</span><span class="sxs-lookup"><span data-stu-id="96215-419">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="96215-420">Belirli atama onayına durumunu *v* önce *expr_second* kesin atama durumu ile aynı *v* sonra *expr_first*.</span><span class="sxs-lookup"><span data-stu-id="96215-420">The definite assignment state of *v* before *expr_second* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
*  <span data-ttu-id="96215-421">Belirli atama onayına deyiminin *v* sonra *expr* tarafından belirlenir:</span><span class="sxs-lookup"><span data-stu-id="96215-421">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="96215-422">Varsa *expr_first* sabit ifade ([sabit ifadeler](expressions.md#constant-expressions)) null değerine ve durumu ile *v* sonra *expr* aynıdır durumunu *v* sonra *expr_second*.</span><span class="sxs-lookup"><span data-stu-id="96215-422">If *expr_first* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value null, then the state of *v* after *expr* is the same as the state of *v* after *expr_second*.</span></span>
*  <span data-ttu-id="96215-423">Aksi takdirde, durumunu *v* sonra *expr* kesin atama durumu ile aynı *v* sonra *expr_first*.</span><span class="sxs-lookup"><span data-stu-id="96215-423">Otherwise, the state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>

#### <a name="-conditional-expressions"></a><span data-ttu-id="96215-424">?: (koşullu) ifadeler</span><span class="sxs-lookup"><span data-stu-id="96215-424">?: (conditional) expressions</span></span>

<span data-ttu-id="96215-425">Bir ifade için *expr* formun `expr_cond ? expr_true : expr_false`:</span><span class="sxs-lookup"><span data-stu-id="96215-425">For an expression *expr* of the form `expr_cond ? expr_true : expr_false`:</span></span>

*  <span data-ttu-id="96215-426">Belirli atama onayına durumunu *v* önce *expr_cond* durumunu aynı *v* önce *expr*.</span><span class="sxs-lookup"><span data-stu-id="96215-426">The definite assignment state of *v* before *expr_cond* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="96215-427">Belirli atama onayına durumunu *v* önce *expr_true* aşağıdakilerden birini tutar ve yalnızca, kesinlikle atanır:</span><span class="sxs-lookup"><span data-stu-id="96215-427">The definite assignment state of *v* before *expr_true* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="96215-428">*expr_cond* değerine sahip bir sabit ifade ise `false`</span><span class="sxs-lookup"><span data-stu-id="96215-428">*expr_cond* is a constant expression with the value `false`</span></span>
    * <span data-ttu-id="96215-429">durumunu *v* sonra *expr_cond* kesinlikle atanmış veya "kesinlikle true ifadeden sonra atanan".</span><span class="sxs-lookup"><span data-stu-id="96215-429">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after true expression".</span></span>
*  <span data-ttu-id="96215-430">Belirli atama onayına durumunu *v* önce *expr_false* aşağıdakilerden birini tutar ve yalnızca, kesinlikle atanır:</span><span class="sxs-lookup"><span data-stu-id="96215-430">The definite assignment state of *v* before *expr_false* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="96215-431">*expr_cond* değerine sahip bir sabit ifade ise `true`</span><span class="sxs-lookup"><span data-stu-id="96215-431">*expr_cond* is a constant expression with the value `true`</span></span>
*  <span data-ttu-id="96215-432">durumunu *v* sonra *expr_cond* kesinlikle atanmış veya "false ifadeden sonra kesinlikle atanan".</span><span class="sxs-lookup"><span data-stu-id="96215-432">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after false expression".</span></span>
*  <span data-ttu-id="96215-433">Belirli atama onayına durumunu *v* sonra *expr* tarafından belirlenir:</span><span class="sxs-lookup"><span data-stu-id="96215-433">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="96215-434">Varsa *expr_cond* sabit ifade ([sabit ifadeler](expressions.md#constant-expressions)) değerine sahip `true` sonra durumunu *v* sonra *expr* durumu aynı *v* sonra *expr_true*.</span><span class="sxs-lookup"><span data-stu-id="96215-434">If *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `true` then the state of *v* after *expr* is the same as the state of *v* after *expr_true*.</span></span>
    * <span data-ttu-id="96215-435">Aksi takdirde *expr_cond* sabit ifade ([sabit ifadeler](expressions.md#constant-expressions)) değerine sahip `false` sonra durumunu *v* sonra *expr* durumunu aynı *v* sonra *expr_false*.</span><span class="sxs-lookup"><span data-stu-id="96215-435">Otherwise, if *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `false` then the state of *v* after *expr* is the same as the state of *v* after *expr_false*.</span></span>
    * <span data-ttu-id="96215-436">Aksi halde, durumunu *v* sonra *expr_true* kesinlikle atanır ve durumunu *v* sonra *expr_false* kesinlikle olduğu atanan, ardından durumunu *v* sonra *expr* kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="96215-436">Otherwise, if the state of *v* after *expr_true* is definitely assigned and the state of *v* after *expr_false* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="96215-437">Aksi takdirde, durumunu *v* sonra *expr* kesinlikle atanmadı.</span><span class="sxs-lookup"><span data-stu-id="96215-437">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

#### <a name="anonymous-functions"></a><span data-ttu-id="96215-438">Anonim İşlevler</span><span class="sxs-lookup"><span data-stu-id="96215-438">Anonymous functions</span></span>

<span data-ttu-id="96215-439">İçin bir *lambda_expression* veya *anonymous_method_expression* *expr* gövde ile (her iki *blok* veya *ifadesi* ) *gövdesi*:</span><span class="sxs-lookup"><span data-stu-id="96215-439">For a *lambda_expression* or *anonymous_method_expression* *expr* with a body (either *block* or *expression*) *body*:</span></span>

*  <span data-ttu-id="96215-440">Bir harici değişken kesin atama durumu *v* önce *gövdesi* durumunu aynı *v* önce *expr*.</span><span class="sxs-lookup"><span data-stu-id="96215-440">The definite assignment state of an outer variable *v* before *body* is the same as the state of *v* before *expr*.</span></span> <span data-ttu-id="96215-441">Diğer bir deyişle, dış değişkenlerin kesin atama durumu anonim işlev bağlamından devralınır.</span><span class="sxs-lookup"><span data-stu-id="96215-441">That is, definite assignment state of outer variables is inherited from the context of the anonymous function.</span></span>
*  <span data-ttu-id="96215-442">Bir harici değişken kesin atama durumu *v* sonra *expr* durumunu aynı *v* önce *expr*.</span><span class="sxs-lookup"><span data-stu-id="96215-442">The definite assignment state of an outer variable *v* after *expr* is the same as the state of *v* before *expr*.</span></span>

<span data-ttu-id="96215-443">Örnek</span><span class="sxs-lookup"><span data-stu-id="96215-443">The example</span></span>
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
<span data-ttu-id="96215-444">bu yana bir derleme zamanı hatası oluşturur `max` burada anonim işlev bildirildiği kesinlikle atanmadı.</span><span class="sxs-lookup"><span data-stu-id="96215-444">generates a compile-time error since `max` is not definitely assigned where the anonymous function is declared.</span></span> <span data-ttu-id="96215-445">Örnek</span><span class="sxs-lookup"><span data-stu-id="96215-445">The example</span></span>
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
<span data-ttu-id="96215-446">Ayrıca atamaya beri bir derleme zamanı hatası oluşturur `n` anonim işlev kesin atama durumu üzerinde herhangi bir etkisi yoktur `n` dışında anonim bir işlevdir.</span><span class="sxs-lookup"><span data-stu-id="96215-446">also generates a compile-time error since the assignment to `n` in the anonymous function has no affect on the definite assignment state of `n` outside the anonymous function.</span></span>

## <a name="variable-references"></a><span data-ttu-id="96215-447">Değişken başvuruları</span><span class="sxs-lookup"><span data-stu-id="96215-447">Variable references</span></span>

<span data-ttu-id="96215-448">A *variable_reference* olduğu bir *ifade* bir değişken olarak sınıflandırılmış.</span><span class="sxs-lookup"><span data-stu-id="96215-448">A *variable_reference* is an *expression* that is classified as a variable.</span></span> <span data-ttu-id="96215-449">A *variable_reference* geçerli değeri getirilemedi ve yeni bir değeri depolamak için erişilebilir bir depolama konumu gösterir.</span><span class="sxs-lookup"><span data-stu-id="96215-449">A *variable_reference* denotes a storage location that can be accessed both to fetch the current value and to store a new value.</span></span>

```antlr
variable_reference
    : expression
    ;
```

<span data-ttu-id="96215-450">C ve C++, *variable_reference* olarak bilinen bir *lvalue*.</span><span class="sxs-lookup"><span data-stu-id="96215-450">In C and C++, a *variable_reference* is known as an *lvalue*.</span></span>

## <a name="atomicity-of-variable-references"></a><span data-ttu-id="96215-451">Kararlılık değişken başvuruları</span><span class="sxs-lookup"><span data-stu-id="96215-451">Atomicity of variable references</span></span>

<span data-ttu-id="96215-452">Okuma ve yazma işlemleri aşağıdaki veri türlerinden atomik: `bool`, `char`, `byte`, `sbyte`, `short`, `ushort`, `uint`, `int`, `float`ve başvuru türleri.</span><span class="sxs-lookup"><span data-stu-id="96215-452">Reads and writes of the following data types are atomic: `bool`, `char`, `byte`, `sbyte`, `short`, `ushort`, `uint`, `int`, `float`, and reference types.</span></span> <span data-ttu-id="96215-453">Ayrıca, okuma ve yazma işlemleri önceki listede bir temel türü ile numaralandırma türleri de atomiktir.</span><span class="sxs-lookup"><span data-stu-id="96215-453">In addition, reads and writes of enum types with an underlying type in the previous list are also atomic.</span></span> <span data-ttu-id="96215-454">Okuma ve yazma işlemleri dahil olmak üzere diğer türde `long`, `ulong`, `double`, ve `decimal`, kullanıcı tanımlı türler yanı sıra atomik olması garanti edilmez.</span><span class="sxs-lookup"><span data-stu-id="96215-454">Reads and writes of other types, including `long`, `ulong`, `double`, and `decimal`, as well as user-defined types, are not guaranteed to be atomic.</span></span> <span data-ttu-id="96215-455">Bu amaca göre tasarlanan kitaplığı işlevleri tarafından garantisi yoktur, atomik okuma-değiştirme-yazma gibi artırma veya azaltma söz konusu olduğunda.</span><span class="sxs-lookup"><span data-stu-id="96215-455">Aside from the library functions designed for that purpose, there is no guarantee of atomic read-modify-write, such as in the case of increment or decrement.</span></span>

