---
ms.openlocfilehash: 77c9ffda3a59cc5f3dcc3ec9848600c6c5e03eed
ms.sourcegitcommit: 27487fa0294f4cdb7e5f2478884149e05314fd8a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "79484974"
---
# <a name="primary-constructors"></a><span data-ttu-id="a7133-101">Birincil oluşturucular</span><span class="sxs-lookup"><span data-stu-id="a7133-101">Primary constructors</span></span>

* <span data-ttu-id="a7133-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="a7133-102">[x] Proposed</span></span>
* <span data-ttu-id="a7133-103">[] Prototipi: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="a7133-103">[ ] Prototype: Not started</span></span>
* <span data-ttu-id="a7133-104">[] Uygulama: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="a7133-104">[ ] Implementation: Not started</span></span>
* <span data-ttu-id="a7133-105">[] Belirtimi: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="a7133-105">[ ] Specification: Not started</span></span>

## <a name="summary"></a><span data-ttu-id="a7133-106">Özet</span><span class="sxs-lookup"><span data-stu-id="a7133-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="a7133-107">Sınıfların bir parametre listesi olabilir ve bunlar ne zaman bir bağımsız değişken listesine sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="a7133-107">Classes can have a parameter list, and when they do, their base class specification can have an argument list.</span></span>
<span data-ttu-id="a7133-108">Birincil Oluşturucu parametreleri sınıf bildiriminin tamamında kapsamdadır ve bir işlev üyesi veya anonim işlev tarafından yakalanırsa, bunlar sınıfında özel alanlar olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="a7133-108">Primary constructor parameters are in scope throughout the class declaration, and if they are captured by a function member or anonymous function, they are stored as private fields in the class.</span></span>

## <a name="motivation"></a><span data-ttu-id="a7133-109">Amacı</span><span class="sxs-lookup"><span data-stu-id="a7133-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="a7133-110">Program başlatma kodunda çok fazla ortak olması yaygındır.</span><span class="sxs-lookup"><span data-stu-id="a7133-110">It is common to have a lot of boilerplate in program initialization code.</span></span> <span data-ttu-id="a7133-111">Genel durumda, belirli bir veri `x` parçası birçok kez bahsedilir:</span><span class="sxs-lookup"><span data-stu-id="a7133-111">In the general case, a given piece of data `x` is mentioned many times:</span></span>

- <span data-ttu-id="a7133-112">Özel bir alan `_x`</span><span class="sxs-lookup"><span data-stu-id="a7133-112">As a private field `_x`</span></span>
- <span data-ttu-id="a7133-113">Bir oluşturucuya `x` parametresi olarak</span><span class="sxs-lookup"><span data-stu-id="a7133-113">As a parameter `x` to a constructor</span></span>
- <span data-ttu-id="a7133-114">Oluşturucunun parametresindeki alanın atama `_x = x;`</span><span class="sxs-lookup"><span data-stu-id="a7133-114">In an assignment `_x = x;` of the field from the parameter in the constructor</span></span>
- <span data-ttu-id="a7133-115">Özellik olarak `X`</span><span class="sxs-lookup"><span data-stu-id="a7133-115">As a property `X`</span></span>
- <span data-ttu-id="a7133-116">Özellik ayarlayıcısı 'nda `x = value;`</span><span class="sxs-lookup"><span data-stu-id="a7133-116">In the property setter `x = value;`</span></span>
- <span data-ttu-id="a7133-117">Özellik alıcısı `return x;`</span><span class="sxs-lookup"><span data-stu-id="a7133-117">In the property getter `return x;`</span></span>

``` c#
class C
{
    private string _x;
    
    public C(string x)
    {
        _x = x;
    }
    public string X
    {
        get => _x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); _x = value; }
    }
}
```

<span data-ttu-id="a7133-118">Doğrulama veya hesaplama gerektirmeyen özellikler için, kaynaklanan çoğunu otomatik özellikler kullanılarak azaltılabilir, bu nedenle özellik için açık bir yedekleme alanı bildirme gereksinimini ortadan kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a7133-118">For properties that don't require validation or computation, the tedium can be reduced using auto-properties, thus cutting out the need to declare an explicit backing field for the property.</span></span> <span data-ttu-id="a7133-119">Ancak özellik, bir otomatik özelliğin sağladığı bir mantık sıralaması gerektiriyorsa, yukarıdaki en iyi yöntem sizin için en iyi seçenektir.</span><span class="sxs-lookup"><span data-stu-id="a7133-119">But if your property requires any sort of logic beyond what an auto-property provides, the above is the best you an do.</span></span>

<span data-ttu-id="a7133-120">Bunun yerine birincil oluşturucular, Oluşturucu bağımsız değişkenlerini sınıfın tamamında doğrudan kapsama yerleştirerek yük yükünü azaltır, bir yedekleme alanını açıkça bildirme gereksinimini gereksinimini gidererek.</span><span class="sxs-lookup"><span data-stu-id="a7133-120">Primary constructors instead reduce the overhead by putting constructor arguments directly in scope throughout the class, again obviating the need to explicitly declare a backing field.</span></span> <span data-ttu-id="a7133-121">Bu nedenle yukarıdaki örnek şöyle olacaktır:</span><span class="sxs-lookup"><span data-stu-id="a7133-121">Thus, the above example would become:</span></span>

``` c#
class C(string x)
{
    public string X
    {
        get => x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); x = value; }
    }
}
```

<span data-ttu-id="a7133-122">Bu örnekte, birincil Oluşturucu `x` için adlandırılmış varlıkların sayısını üç ile ikiye azaltır, `_x` gereksinimini gidererek.</span><span class="sxs-lookup"><span data-stu-id="a7133-122">In this example, the primary constructor reduces the number of named entities for `x` from three to two, obviating the `_x` backing field.</span></span> <span data-ttu-id="a7133-123">Üç üye bildirimini kaldırır (yalnızca özellik bildiriminin kendisini tutarak) ve `x`/`_x`/`X` sekizlisi sayısını sekiz ile beş arasında azaltır.</span><span class="sxs-lookup"><span data-stu-id="a7133-123">It removes two out of three member declarations (keeping only the property declaration itself), and reduces the total number of mentions of `x`/`_x`/`X` from eight to five.</span></span>


## <a name="detailed-design"></a><span data-ttu-id="a7133-124">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="a7133-124">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="a7133-125">Sınıfların bir parametre listesi olabilir:</span><span class="sxs-lookup"><span data-stu-id="a7133-125">Classes can have a parameter list:</span></span>

``` c#
public class C(int i, string s)
{
    ...
}
```

<span data-ttu-id="a7133-126">Parametre listesi, bir oluşturucunun sınıf için örtülü olarak bildirilmesine neden olur ve bu, sınıfın kendisiyle aynı erişilebilirliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a7133-126">The parameter list causes a constructor to be implicitly declared for the class, with the same accessibility as the class itself.</span></span>

``` c#
new C(5, "Hello");
```

<span data-ttu-id="a7133-127">Birincil Oluşturucu parametreleri sınıf gövdesinin tamamında kapsamdadır.</span><span class="sxs-lookup"><span data-stu-id="a7133-127">Primary constructor parameters are in scope throughout the class body.</span></span> <span data-ttu-id="a7133-128">Bunlar bir işlev üyesi veya anonim işlev tarafından yakalanırsa, sınıfında özel alanlar olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="a7133-128">If they are captured by a function member or anonymous function, they become stored as private fields in the class.</span></span> <span data-ttu-id="a7133-129">Yalnızca başlatma sırasında kullanılıyorsa, bu nesneler nesnede depolanmaz.</span><span class="sxs-lookup"><span data-stu-id="a7133-129">If they are only used during initialization they will not be stored in the object.</span></span>

``` c#
public class C(int i, string s)
{
    int[] a = new int[i]; // i not captured
    public int S => s;    // s captured
}
```

<span data-ttu-id="a7133-130">Birincil oluşturucuya sahip bir sınıfın temel sınıf belirtimi varsa, bu bir bağımsız değişken listesine sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="a7133-130">If a class with a primary constructor has a base class specification, that one can have an argument list.</span></span> <span data-ttu-id="a7133-131">Bu, örtük olarak belirtilen oluşturucunun `base(...)` başlatıcısı için bağımsız değişken listesi görevi görür.</span><span class="sxs-lookup"><span data-stu-id="a7133-131">This serves as the argument list to a `base(...)` initializer of the implicitly declared constructor.</span></span> <span data-ttu-id="a7133-132">Bağımsız değişken listesi sağlanmazsa boş bir değer kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="a7133-132">If no argument list is provided, an empty one is assumed.</span></span>

``` c#
public class C(int i, string s) : B(s)
{
    ...
}
```
<span data-ttu-id="a7133-133">Sınıfı açıkça tanımlanmış oluşturucuları de içerebilir, ancak tümünün bir `this(...)` başlatıcısı kullanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a7133-133">The class can have explicitly defined constructors as well, but those all have to use a `this(...)` initializer.</span></span> <span data-ttu-id="a7133-134">Bu, yeni bir örnek oluşturulduğunda birincil oluşturucunun her zaman çağrılabilir olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="a7133-134">This ensures that the primary constructor is always called when a new instance is constructed.</span></span>

<span data-ttu-id="a7133-135">Sınıf gövdesindeki tüm başlatıcılar, oluşturulan oluşturucuda atamalar olur.</span><span class="sxs-lookup"><span data-stu-id="a7133-135">All initializers in the class body will become assignments in the generated constructor.</span></span> <span data-ttu-id="a7133-136">Yani, diğer sınıfların aksine, başlatıcıların temel Oluşturucu *çağrıldıktan sonra* , önce değil, çalışır.</span><span class="sxs-lookup"><span data-stu-id="a7133-136">This means that, unlike other classes, initializers will run *after* the base constructor has been invoked, not before.</span></span> <span data-ttu-id="a7133-137">Ayrıca, oluşturulan sınıf, Üyeler tarafından yakalanan birincil Oluşturucu parametrelerini depolamak için oluşturulan özel alanları başlatmak üzere Atamalar içerir.</span><span class="sxs-lookup"><span data-stu-id="a7133-137">In addition, the generated class will contain assignments to initialize any private fields that were generated to store primary constructor parameters that were captured by members.</span></span> <span data-ttu-id="a7133-138">Bu Üyeler, lambda ifadelerinin kapanışlarını ifade eden bir şekilde parametresi yerine özel alanı kullanmak için yeniden yazılır.</span><span class="sxs-lookup"><span data-stu-id="a7133-138">Those members are rewritten to use the private field instead of the parameter in a manner similar to closures for lambda expressions.</span></span> <span data-ttu-id="a7133-139">Oluşturulan birincil alanlar önce başlatılır, sonra başlatıcı tarafından oluşturulan atamalar, sınıfında görünüm sırasına göre yürütülür.</span><span class="sxs-lookup"><span data-stu-id="a7133-139">The generated primary fields are initialized first, and then the initializer-generated assignments are executed in the order of appearance in the class.</span></span>

<span data-ttu-id="a7133-140">Yukarıdaki örnekte, sınıf bildiriminin etkisi şöyle olur:</span><span class="sxs-lookup"><span data-stu-id="a7133-140">For the above example, the effect of the class declaration is as if rewritten like this:</span></span>

``` c#
public class C : B
{
    public C(int i, string s) : base(s)
    {
        __s = s;        // store parameter s for captured use
        a = new int[i]; // initialize a
    }
    int __s; // generated field for capture of s
    
    int[] a;
    public int S => __s; // s replaced with captured __s
}
```

<span data-ttu-id="a7133-141">Yakalama, yerel değişkenlerin lambda ifadelerine yakalamaya benzer kısıtlamalara sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a7133-141">The capture has similar restrictions to the capture of local variables by lambda expressions.</span></span> <span data-ttu-id="a7133-142">Örneğin, `ref` ve `out` parametrelere birincil oluşturucularda izin verilir, ancak üye gövdelerim yakalanamaz.</span><span class="sxs-lookup"><span data-stu-id="a7133-142">For instance, `ref` and `out` parameters are allowed in primary constructors, but cannot be captured my member bodies.</span></span>


## <a name="drawbacks"></a><span data-ttu-id="a7133-143">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="a7133-143">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="a7133-144">Kabaca anlamlı bir sıra.</span><span class="sxs-lookup"><span data-stu-id="a7133-144">In rough order of significance.</span></span>

* <span data-ttu-id="a7133-145">Teklif, konumsal kayıtlar için de önerilen söz dizimini kullanır.</span><span class="sxs-lookup"><span data-stu-id="a7133-145">The proposal uses syntax that has also been proposed for positional records.</span></span> <span data-ttu-id="a7133-146">Her iki özelliği de sağladığımızda, bazı konaklama gerekir.</span><span class="sxs-lookup"><span data-stu-id="a7133-146">If we desire both features, some accommodation is required.</span></span> <span data-ttu-id="a7133-147">Örneğin</span><span class="sxs-lookup"><span data-stu-id="a7133-147">E.g.</span></span> <span data-ttu-id="a7133-148">kayıtlar üzerinde `data` değiştiricisi önerildi.</span><span class="sxs-lookup"><span data-stu-id="a7133-148">a `data` modifier on records has been proposed.</span></span>
* <span data-ttu-id="a7133-149">Oluşturulan nesnelerin ayırma boyutu daha az açıktır, derleyici, sınıfın tam metnine göre birincil Oluşturucu parametresi için bir alan ayırmayı belirler.</span><span class="sxs-lookup"><span data-stu-id="a7133-149">The allocation size of constructed objects is less obvious, as the compiler determines whether to allocate a field for a primary constructor parameter based on the full text of the class.</span></span> <span data-ttu-id="a7133-150">Bu risk, değişkenlerin lambda ifadelerine örtük olarak yakalamasına benzer.</span><span class="sxs-lookup"><span data-stu-id="a7133-150">This risk is similar to the implicit capture of variables by lambda expressions.</span></span>
* <span data-ttu-id="a7133-151">Ortak bir dürtüsüne (veya yanlışlıkla bir model), Oluşturucu zincirinin, temel sınıfta korunan bir alanı açıkça bir şekilde ayrılmasını yerine, yinelenen ayırmalara nesnelerdeki aynı veriler için.</span><span class="sxs-lookup"><span data-stu-id="a7133-151">A common temptation (or accidental pattern) might be to capture the "same" parameter at multiple levels of inheritance as it is passed up the constructor chain instead of explicitly allotting it a protected field at the base class, leading to duplicated allocations for the same data in objects.</span></span> <span data-ttu-id="a7133-152">Bu, otomatik özelliklerle otomatik özellikleri geçersiz kılma riskine bugün benzer.</span><span class="sxs-lookup"><span data-stu-id="a7133-152">This is very similar to today's risk of overriding auto-properties with auto-properties.</span></span> 
* <span data-ttu-id="a7133-153">Yukarıda önerildiği gibi, genellikle Oluşturucu gövdelerinde belirtilebilen ek mantık için bir yer yoktur.</span><span class="sxs-lookup"><span data-stu-id="a7133-153">As proposed above, there is no place for additional logic that might usually expressed in constructor bodies.</span></span> <span data-ttu-id="a7133-154">Aşağıdaki "birincil Oluşturucu gövdeleri" uzantısı bu şekilde ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a7133-154">The "Primary constructor bodies" extension below addresses that.</span></span>
* <span data-ttu-id="a7133-155">Önerildiği gibi, yürütme sırası semantiği sıradan oluşturucularla çok daha farklıdır.</span><span class="sxs-lookup"><span data-stu-id="a7133-155">As proposed, execution order semantics are subtly different than with ordinary constructors.</span></span> <span data-ttu-id="a7133-156">Bunun yapılması, bazı uzantı tekliflerinin (özellikle "birincil Oluşturucu gövdelerinin") maliyetinde gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="a7133-156">This could probably be remedied, but at the cost of some of the extension proposals (notably "Primary constructor bodies").</span></span>
* <span data-ttu-id="a7133-157">Teklif yalnızca tek bir oluşturucunun birincil olarak belirlenmesi durumunda işe yarar.</span><span class="sxs-lookup"><span data-stu-id="a7133-157">The proposal only works when a single constructor can be designated primary.</span></span>
* <span data-ttu-id="a7133-158">Sınıfının ve birincil oluşturucunun ayrı erişilebilirliğine sahip olmanın bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="a7133-158">There is no way to have separate accessibility of the class and the primary constructor.</span></span> <span data-ttu-id="a7133-159">Örneğin, herkese açık bir özel "Build-It-All" oluşturucusuna temsilci seçmek için, bu durumda.</span><span class="sxs-lookup"><span data-stu-id="a7133-159">For instance, in situations where public constructors all delegate to one private "build-it-all" constructor that would be needed.</span></span> <span data-ttu-id="a7133-160">Gerekirse, sözdizimi daha sonra önerilir.</span><span class="sxs-lookup"><span data-stu-id="a7133-160">If necessary, syntax could be proposed for that later.</span></span>


## <a name="alternatives"></a><span data-ttu-id="a7133-161">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="a7133-161">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="a7133-162">Tam konumsal konumsal kayıtlar alternatif olabilir veya özellikleri temel alınarak birincil oluşturucularla birlikte olabilir.</span><span class="sxs-lookup"><span data-stu-id="a7133-162">Full-on positional records may be an alternative, or may coexist with primary constructors, depending on the specifics.</span></span> <span data-ttu-id="a7133-163">Daha *az* sayıda senaryoda *daha fazla* kısaltmaya izin verir.</span><span class="sxs-lookup"><span data-stu-id="a7133-163">They would allow for *more* abbreviation in a *smaller* number of scenarios.</span></span> <span data-ttu-id="a7133-164">Bu nedenle her ikisi de yararlı olur, ancak her ikisine de tek bir şekilde tümleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="a7133-164">So both are potentially useful, but having both may be overkill, unless they can be somewhat neatly integrated with each other.</span></span>


## <a name="possible-extensions"></a><span data-ttu-id="a7133-165">Olası uzantılar</span><span class="sxs-lookup"><span data-stu-id="a7133-165">Possible extensions</span></span>
[extensions]: #possible-extensions

<span data-ttu-id="a7133-166">Bunlar, temel teklife, onunla birlikte düşünüldüler veya eklemeler ya da kabul edilen yararlı olursa daha sonraki bir aşamada bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="a7133-166">These are variations or additions to the core proposal that may be considered in conjunction with it, or at a later stage if deemed useful.</span></span>

### <a name="primary-constructor-bodies"></a><span data-ttu-id="a7133-167">Birincil Oluşturucu gövdeleri</span><span class="sxs-lookup"><span data-stu-id="a7133-167">Primary constructor bodies</span></span>

<span data-ttu-id="a7133-168">Oluşturucular genellikle parametre doğrulama mantığını veya Başlatıcı olarak ifade edilebilen diğer önemsiz başlatma kodlarını içerir.</span><span class="sxs-lookup"><span data-stu-id="a7133-168">Constructors themselves often contain parameter validation logic or other nontrivial initialization code that cannot be expressed as initializers.</span></span>

<span data-ttu-id="a7133-169">Birincil oluşturucular, ifade bloklarının doğrudan sınıf gövdesinde görünmesine izin vermek için genişletilebilir.</span><span class="sxs-lookup"><span data-stu-id="a7133-169">Primary constructors could be extended to allow statement blocks to appear directly in the class body.</span></span> <span data-ttu-id="a7133-170">Bu deyimler, başlatma atamaları arasında göründükleri noktada oluşturulan oluşturucuya eklenirler ve bu nedenle başlatıcılarla birlikte yürütülür.</span><span class="sxs-lookup"><span data-stu-id="a7133-170">Those statements would be inserted in the generated constructor at the point where they appear between initializing assignments, and would thus be executed interspersed with initializers.</span></span> <span data-ttu-id="a7133-171">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="a7133-171">For instance:</span></span>

``` c#
public class C(int i, string s) : B(s)
{
    {
        if (i < 0) throw new ArgumentOutOfRangeException(nameof(i));
    }
    int[] a = new int[i];
    public int S => s;
}
```

### <a name="initializer-fields-and-initializer-functions"></a><span data-ttu-id="a7133-172">Başlatıcı alanları ve Başlatıcı işlevleri</span><span class="sxs-lookup"><span data-stu-id="a7133-172">Initializer fields and initializer functions</span></span>

<span data-ttu-id="a7133-173">Birincil Oluşturucusu olan bir sınıfta, erişilebilirlik değiştiricileri olmadan alan ve Yöntem bildirimlerini, yerel değişkenler ve yerel işlevler gibi bir şekilde düşünebiliriz:</span><span class="sxs-lookup"><span data-stu-id="a7133-173">In a class with a primary constructor we could consider field and method declarations without accessibility modifiers to be more like local variables and local functions:</span></span>

* <span data-ttu-id="a7133-174">Birincil Oluşturucu parametreleri gibi, "başlatıcı alanları" yalnızca işlev üyelerinde kullanılıyorsa gerçek bir özel alana yakalanırlar.</span><span class="sxs-lookup"><span data-stu-id="a7133-174">Just like primary constructor parameters the "initializer fields" would only be captured into an actual private field if they were used in function members.</span></span>
* <span data-ttu-id="a7133-175">"Başlatıcı işlevleri" yalnızca kendi diğer işlev üyelerinde kullanılsaydı birincil Oluşturucu parametrelerini ve Başlatıcı alanlarını yakalamak için değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="a7133-175">The "initializer functions" would only be considered to capture primary constructor parameters and initializer fields if they were themselves used in other function members.</span></span> <span data-ttu-id="a7133-176">Yakalanmadıysa, yerel işlevler gibi daha iyi bir şekilde oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="a7133-176">If not captured, they could be generated in a more optimal fashion, like local functions.</span></span>
* <span data-ttu-id="a7133-177">Yalnızca birincil Oluşturucu parametreleri gibi, yalnızca basit bir ad olarak üye erişimi aracılığıyla kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="a7133-177">Just like primary constructor parameters they would not be available via member access, but only as a simple name.</span></span>

<span data-ttu-id="a7133-178">Bu, yalnızca başlatma ile ilgili geçici değişkenler ve yardımcı işlevler için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="a7133-178">This could be used for temporary variables and helper functions that are only relevant to initialization:</span></span>

``` c#
public class C(string s)
{
    int size = s.Length;             // not captured
    int[] Create() => new int[size]; // not captured
    int[] a = Create();
    ...
}
```

<span data-ttu-id="a7133-179">Bu, özellikle de erişilebilirlik değiştiricilerin yokluğu `private`anlamı olduğundan bu çok hafif olabilir.</span><span class="sxs-lookup"><span data-stu-id="a7133-179">This may be too subtle, especially since the absence of accessibility modifiers elsewhere simply means `private`.</span></span> 

### <a name="initializer-statements"></a><span data-ttu-id="a7133-180">Başlatıcı deyimleri</span><span class="sxs-lookup"><span data-stu-id="a7133-180">Initializer statements</span></span>

<span data-ttu-id="a7133-181">Yukarıdaki uzantıların kök birleşimi yalnızca sınıf gövdesinde doğrudan deyimlere izin vermek olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a7133-181">A radical combination of the above to extensions would be to simply allow statements directly in the class body.</span></span> <span data-ttu-id="a7133-182">Bu tür deyimler, yukarıda önerilen ve `{ }`içine alınmaları dışında, yukarıda önerilen Oluşturucu gövdeleri olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="a7133-182">Such statements are exactly as the interspersed constructor bodies proposed above, except they don't need to be enclosed in `{ }`.</span></span> <span data-ttu-id="a7133-183">Bunun yeterince yararlı olması için, "yerel" değişkenlerin ve yardımcı işlevlerin, yukarıdaki "başlatıcı alanları ve Başlatıcı işlevleri" uzantısında araştırılan sınıfının en üst düzeyinde ifade edilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a7133-183">For this to be sufficiently useful, "local" variables and helper functions would need to also be expressible at the top level of the class, in the manner explored in the extension "Initializer fields and initializer functions" above.</span></span>


### <a name="member-access"></a><span data-ttu-id="a7133-184">Üye erişimi</span><span class="sxs-lookup"><span data-stu-id="a7133-184">Member access</span></span>

<span data-ttu-id="a7133-185">Çekirdek teklif, birincil Oluşturucu parametrelerini yalnızca basit adlar olarak başvurulabilen parametreler olarak değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="a7133-185">The core proposal treats primary constructor parameters as parameters that can only be referred as simple names.</span></span> <span data-ttu-id="a7133-186">Diğer bir seçenek de, bazen alan olarak *oluşturulmasa bile* , bunlara üye erişimle, yani özel alanlar gibi başvurulmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="a7133-186">An alternative is to allow them to be referenced as if they were private fields, i.e. with a member access, *even* if they are sometimes not generated as fields.</span></span> <span data-ttu-id="a7133-187">Bu, yerel değişkenlerle gölgelendirildikleri zaman `this.x` olarak başvurulmasını ve farklı bir örnekten `other.x`olarak erişilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="a7133-187">This would allow them to be referenced as `this.x` when shadowed by local variables, and accessed from a different instance as `other.x`.</span></span>

<span data-ttu-id="a7133-188">"Başlatıcı alanları ve Başlatıcı işlevleri" uzantısı için uygulanmışsa, bu durum sıradan özel üyelerden farklı olan dereceyi de azaltır.</span><span class="sxs-lookup"><span data-stu-id="a7133-188">If applied to the "initializer fields and initializer functions" extension this would also reduce the degree to which those were different from ordinary private members.</span></span> <span data-ttu-id="a7133-189">Tek fark daha sonra, yalnızca başlatma sırasında kullanılıyorsa derleyicinin nesne içinden bir bütün olarak bu nesneleri sorunsuz bir şekilde boşaltmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="a7133-189">The only difference would then be that the compiler is free to elide them from the object if only used during initialization.</span></span>

