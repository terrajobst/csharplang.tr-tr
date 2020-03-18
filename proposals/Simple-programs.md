---
ms.openlocfilehash: b9697fc1d772ba59ed3b1de339a5a3d4eb24b1bd
ms.sourcegitcommit: 36b028f4d6e88bd7d4a843c6d384d1b63cc73334
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "79485226"
---
# <a name="simple-programs"></a><span data-ttu-id="13381-101">Basit programlar</span><span class="sxs-lookup"><span data-stu-id="13381-101">Simple programs</span></span>

* <span data-ttu-id="13381-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="13381-102">[x] Proposed</span></span>
* <span data-ttu-id="13381-103">[x] prototipi: başlatıldı</span><span class="sxs-lookup"><span data-stu-id="13381-103">[x] Prototype: Started</span></span>
* <span data-ttu-id="13381-104">[] Uygulama: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="13381-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="13381-105">[] Belirtimi: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="13381-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="13381-106">Özet</span><span class="sxs-lookup"><span data-stu-id="13381-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="13381-107">Bir *compilation_unit* (örneğin, kaynak dosya) *namespace_member_declaration*bir dizi *deyimin* hemen öncesine oluşmasına izin verin.</span><span class="sxs-lookup"><span data-stu-id="13381-107">Allow a sequence of *statements* to occur right before the *namespace_member_declaration*s of a *compilation_unit* (i.e. source file).</span></span>

<span data-ttu-id="13381-108">Sözdizimi, böyle bir *deyim* sırası varsa, aşağıdaki tür bildiriminin, gerçek tür adı ve Yöntem adı bildiriminde yer aldığı şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="13381-108">The semantics are that if such a sequence of *statements* is present, the following type declaration, modulo the actual type name and the method name, would be emitted:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="13381-109">Ayrıca bkz. https://github.com/dotnet/csharplang/issues/3117.</span><span class="sxs-lookup"><span data-stu-id="13381-109">See also https://github.com/dotnet/csharplang/issues/3117.</span></span>

## <a name="motivation"></a><span data-ttu-id="13381-110">Amacı</span><span class="sxs-lookup"><span data-stu-id="13381-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="13381-111">Açık bir `Main` metoduna ihtiyaç duyduğundan, en basit programları bile çevreleyen belirli bir ortak miktar.</span><span class="sxs-lookup"><span data-stu-id="13381-111">There's a certain amount of boilerplate surrounding even the simplest of programs, because of the need for an explicit `Main` method.</span></span> <span data-ttu-id="13381-112">Bu, dil öğrenimi ve program açıklık gibi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="13381-112">This seems to get in the way of language learning and program clarity.</span></span> <span data-ttu-id="13381-113">Bu nedenle özelliğin birincil amacı, kullanıcıların, öğrenenler C# ve kodun netliğine yönelik gereksiz yere sahip olmayan programlara izin vermektir.</span><span class="sxs-lookup"><span data-stu-id="13381-113">The primary goal of the feature therefore is to allow C# programs without unnecessary boilerplate around them, for the sake of learners and the clarity of code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="13381-114">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="13381-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="syntax"></a><span data-ttu-id="13381-115">Sözdizimi</span><span class="sxs-lookup"><span data-stu-id="13381-115">Syntax</span></span>

<span data-ttu-id="13381-116">Yalnızca *namespace_member_declaration*s 'den önce, derleme biriminde bir dizi *deyimin*kullanılmasına izin veriliyor. ek sözdizimi yalnızca</span><span class="sxs-lookup"><span data-stu-id="13381-116">The only additional syntax is allowing a sequence of *statement*s in a compilation unit, just before the *namespace_member_declaration*s:</span></span>

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

<span data-ttu-id="13381-117">Her birinde, bir *compilation_unit* *deyimin*hepsi yerel işlev bildirimleri olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="13381-117">In all but one *compilation_unit* the *statement*s must all be local function declarations.</span></span> 

<span data-ttu-id="13381-118">Örnek:</span><span class="sxs-lookup"><span data-stu-id="13381-118">Example:</span></span>

``` c#
// File 1 - any statements
if (args.Length == 0
    || !int.TryParse(args[0], out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

// File 2 - only local functions
(int curr, int prev) Fib(int i)
{
    if (i == 0) return (1, 0);
    var (curr, prev) = Fib(i - 1);
    return (curr + prev, curr);
}
```

### <a name="semantics"></a><span data-ttu-id="13381-119">İçeriyor</span><span class="sxs-lookup"><span data-stu-id="13381-119">Semantics</span></span>

<span data-ttu-id="13381-120">Programın herhangi bir derleme biriminde herhangi bir en üst düzey deyim varsa, anlamı aşağıdaki gibi genel ad alanındaki bir `Program` sınıfının `Main` yönteminin blok gövdesinde birleştirilmiş gibi olur:</span><span class="sxs-lookup"><span data-stu-id="13381-120">If any top-level statements are present in any compilation unit of the program, the meaning is as if they were combined in the block body of a `Main` method of a `Program` class in the global namespace, as follows:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // File 1 statements
        // File 2 local functions
        // ...
    }
}
```

<span data-ttu-id="13381-121">"Program" ve "Main" adlarının yalnızca çizimler amacıyla kullanıldığını, derleyici tarafından kullanılan gerçek adların uygulamaya bağımlı olduğunu ve ne tür, ne de kaynak koddan ad tarafından başvurulduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="13381-121">Note that the names "Program" and "Main" are used only for illustrations purposes, actual names used by compiler are implementation dependent and neither the type, nor the method can be referenced by name from source code.</span></span>

<span data-ttu-id="13381-122">Yöntemi, programın giriş noktası olarak atanır.</span><span class="sxs-lookup"><span data-stu-id="13381-122">The method is designated as the entry point of the program.</span></span> <span data-ttu-id="13381-123">Kurala göre açıkça tanımlanmış yöntemler, giriş noktası adayları yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="13381-123">Explicitly declared methods that by convention could be considered as an entry point candidates are ignored.</span></span> <span data-ttu-id="13381-124">Meydana geldiğinde bir uyarı bildirilir.</span><span class="sxs-lookup"><span data-stu-id="13381-124">A warning is reported when that happens.</span></span> <span data-ttu-id="13381-125">`-main:<type>` derleyici anahtarı belirtmek hatadır.</span><span class="sxs-lookup"><span data-stu-id="13381-125">It is an error to specify `-main:<type>` compiler switch.</span></span>

<span data-ttu-id="13381-126">Herhangi bir derleme biriminde yerel işlev bildirimleri dışındaki deyimler varsa, bu derleme biriminden gelen deyimler önce oluşur.</span><span class="sxs-lookup"><span data-stu-id="13381-126">If any one compilation unit has statements other than local function declarations, statements from that compilation unit occur first.</span></span> <span data-ttu-id="13381-127">Bunun nedeni, bir dosyadaki yerel işlevlerin başka bir dosyada yerel değişkenlere başvurması için geçerli olmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="13381-127">This causes it to be legal for local functions in one file to reference local variables in another.</span></span> <span data-ttu-id="13381-128">Diğer derleme birimlerinden (tüm yerel işlevler olacak) ifade katkılarının sırası tanımsız.</span><span class="sxs-lookup"><span data-stu-id="13381-128">The order of statement contributions (which would all be local functions) from other compilation units is undefined.</span></span>

<span data-ttu-id="13381-129">Zaman uyumsuz işlemlere, en üst düzey deyimlerde, düzenli bir zaman uyumsuz giriş noktası yöntemi içindeki ifadelerde izin verilen dereceye kadar izin verilir.</span><span class="sxs-lookup"><span data-stu-id="13381-129">Async operations are allowed in top-level statements to the degree they are allowed in statements within a regular async entry point method.</span></span> <span data-ttu-id="13381-130">Ancak, bunlar gerekli değildir, `await` ifadeleri ve diğer zaman uyumsuz işlemler atlanırsa hiçbir uyarı üretilmez.</span><span class="sxs-lookup"><span data-stu-id="13381-130">However, they are not required, if `await` expressions and other async operations are omitted, no warning is produced.</span></span> <span data-ttu-id="13381-131">Bunun yerine, oluşturulan giriş noktası yönteminin imzası şu şekilde eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="13381-131">Instead the signature of the generated entry point method is equivalent to</span></span> 
``` c#
    static void Main()
```

<span data-ttu-id="13381-132">Yukarıdaki örnekte aşağıdaki `$Main` yöntemi bildirimi elde edersiniz:</span><span class="sxs-lookup"><span data-stu-id="13381-132">The example above would yield the following `$Main` method declaration:</span></span>

``` c#
static class $Program
{
    static void $Main()
    {
        // Statements from File 1
        if (args.Length == 0
            || !int.TryParse(args[0], out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
        // Local functions from File 2
        (int curr, int prev) Fib(int i)
        {
            if (i == 0) return (1, 0);
            var (curr, prev) = Fib(i - 1);
            return (curr + prev, curr);
        }
    }
}
```

<span data-ttu-id="13381-133">Aynı zamanda aşağıdakine benzer bir örnek:</span><span class="sxs-lookup"><span data-stu-id="13381-133">At the same time an example like this:</span></span>
``` c#
// File 1
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

<span data-ttu-id="13381-134">Şu şekilde olur:</span><span class="sxs-lookup"><span data-stu-id="13381-134">would  yield:</span></span>
``` c#
static class $Program
{
    static async Task $Main()
    {
        // Statements from File 1
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a><span data-ttu-id="13381-135">Üst düzey yerel değişkenlerin kapsamı ve yerel işlevler</span><span class="sxs-lookup"><span data-stu-id="13381-135">Scope of top-level local variables and local functions</span></span>

<span data-ttu-id="13381-136">En üst düzey yerel değişkenler ve işlevler, oluşturulan giriş noktası yöntemine "sarmalanmış" olsa da, bunlar programın tamamında hala kapsam içinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="13381-136">Even though top-level local variables and functions are "wrapped" into the generated entry point method, they should still be in scope throughout the program.</span></span>
<span data-ttu-id="13381-137">Basit ad değerlendirmesinin amacı için, genel ad alanına ulaşıldığında:</span><span class="sxs-lookup"><span data-stu-id="13381-137">For the purpose of simple-name evaluation, once the global namespace is reached:</span></span>
- <span data-ttu-id="13381-138">İlk olarak, oluşturulan giriş noktası yöntemi içinde adı değerlendirmek için bir girişimde bulunuldu ve bu deneme başarısız olursa</span><span class="sxs-lookup"><span data-stu-id="13381-138">First, an attempt is made to evaluate the name within the generated entry point method and only if this attempt fails</span></span> 
- <span data-ttu-id="13381-139">Genel ad alanı bildirimi içindeki "normal" değerlendirme gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="13381-139">The "regular" evaluation within the global namespace declaration is performed.</span></span> 

<span data-ttu-id="13381-140">Bu, genel ad alanı içinde ve içeri aktarılan adların gölgelendirilmesinden farklı ad alanları ve türlerin ad gölgelendirmesine yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="13381-140">This could lead to name shadowing of namespaces and types declared within the global namespace as well as to shadowing of imported names.</span></span>

<span data-ttu-id="13381-141">Basit ad değerlendirmesi en üst düzey deyimler dışında gerçekleşirse ve değerlendirme, bir hataya yol açacak bir en üst düzey yerel değişken veya işlev verir.</span><span class="sxs-lookup"><span data-stu-id="13381-141">If the simple name evaluation occurs outside of the top-level statements and the evaluation yields a top-level local variable or function, that should lead to an error.</span></span>

<span data-ttu-id="13381-142">Bu şekilde, gelecekte "en üst düzey işlevleri" (https://github.com/dotnet/csharplang/issues/3117)Senaryo 2) daha iyi ele alma imkanımızı koruyoruz ve bu kullanıcılara desteklerini yanlışlıkla düşünen kullanıcılara faydalı Tanılamalar verebiliyor.</span><span class="sxs-lookup"><span data-stu-id="13381-142">In this way we protect our future ability to better address "Top-level functions" (scenario 2 in https://github.com/dotnet/csharplang/issues/3117), and are able to give useful diagnostics to users who mistakenly believe them to be supported.</span></span>

