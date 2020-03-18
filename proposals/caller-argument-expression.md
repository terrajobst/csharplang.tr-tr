---
ms.openlocfilehash: fa3326bf69c83b6042b1db7b5567fd5c28d6f81a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484526"
---
# <a name="callerargumentexpression"></a><span data-ttu-id="da767-101">CallerArgumentExpression</span><span class="sxs-lookup"><span data-stu-id="da767-101">CallerArgumentExpression</span></span>

* <span data-ttu-id="da767-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="da767-102">[x] Proposed</span></span>
* <span data-ttu-id="da767-103">[] Prototipi: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="da767-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="da767-104">[] Uygulama: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="da767-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="da767-105">[] Belirtimi: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="da767-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="da767-106">Özet</span><span class="sxs-lookup"><span data-stu-id="da767-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="da767-107">Geliştiricilerin, tanılama/test API 'Lerinde daha iyi hata iletileri sağlamak ve tuş vuruşlarını azaltmak için bir yönteme geçirilen ifadeleri yakalamasına izin verin.</span><span class="sxs-lookup"><span data-stu-id="da767-107">Allow developers to capture the expressions passed to a method, to enable better error messages in diagnostic/testing APIs and reduce keystrokes.</span></span>

## <a name="motivation"></a><span data-ttu-id="da767-108">Amacı</span><span class="sxs-lookup"><span data-stu-id="da767-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="da767-109">Bir onaylama veya bağımsız değişken doğrulaması başarısız olursa, geliştirici nerede ve neden başarısız olduğu konusunda olabildiğince fazla bilgi almak ister.</span><span class="sxs-lookup"><span data-stu-id="da767-109">When an assertion or argument validation fails, the developer wants to know as much as possible about where and why it failed.</span></span> <span data-ttu-id="da767-110">Bununla birlikte, günümüzün tanılama API 'Leri bunu tam olarak kolaylaştırmaz.</span><span class="sxs-lookup"><span data-stu-id="da767-110">However, today's diagnostic APIs do not fully facilitate this.</span></span> <span data-ttu-id="da767-111">Aşağıdaki yöntemi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="da767-111">Consider the following method:</span></span>

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null);
    Debug.Assert(array.Length == 1);

    return array[0];
}
```

<span data-ttu-id="da767-112">Onaylamalar başarısız olduğunda, yığın izlemesinde yalnızca dosya adı, satır numarası ve Yöntem adı sağlanacaktır.</span><span class="sxs-lookup"><span data-stu-id="da767-112">When one of the asserts fail, only the filename, line number, and method name will be provided in the stack trace.</span></span> <span data-ttu-id="da767-113">Geliştirici bu bilgilerden hangi onaylama işlemi başarısız olduğunu bildiremeyecektir. Bu, ne olduğunu görmek için dosyayı açmak ve girilen satır numarasına gitmeniz gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="da767-113">The developer will not be able to tell which assert failed from this information-- (s)he will have to open the file and navigate to the provided line number to see what went wrong.</span></span>

<span data-ttu-id="da767-114">Bu ayrıca test çerçevelerinin çeşitli onay yöntemleri sağlamasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="da767-114">This is also the reason testing frameworks have to provide a variety of assert methods.</span></span> <span data-ttu-id="da767-115">XUnit ile `Assert.True` ve `Assert.False` sık kullanılmaz, çünkü başarısız olanlar hakkında yeterli bağlam sunmazlar.</span><span class="sxs-lookup"><span data-stu-id="da767-115">With xUnit, `Assert.True` and `Assert.False` are not frequently used because they do not provide enough context about what failed.</span></span>

<span data-ttu-id="da767-116">Durum bağımsız değişken doğrulaması için biraz daha iyidir, çünkü geçersiz bağımsız değişkenlerin adları geliştiriciyle gösteriliyor, geliştirici bu adları özel durumlara el ile iletmelidir.</span><span class="sxs-lookup"><span data-stu-id="da767-116">While the situation is a bit better for argument validation because the names of invalid arguments are shown to the developer, the developer must pass these names to exceptions manually.</span></span> <span data-ttu-id="da767-117">Yukarıdaki örnek `Debug.Assert`yerine geleneksel bağımsız değişken doğrulamasını kullanmak için yeniden yazıldı, şöyle görünür</span><span class="sxs-lookup"><span data-stu-id="da767-117">If the above example were rewritten to use traditional argument validation instead of `Debug.Assert`, it would look like</span></span>

```csharp
T Single<T>(this T[] array)
{
    if (array == null)
    {
        throw new ArgumentNullException(nameof(array));
    }

    if (array.Length != 1)
    {
        throw new ArgumentException("Array must contain a single element.", nameof(array));
    }

    return array[0];
}
```

<span data-ttu-id="da767-118">`nameof(array)`, bu bağımsız değişken geçersiz olan bağlamdan zaten açık olsa da, her bir özel duruma geçirilmesi gerektiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="da767-118">Notice that `nameof(array)` must be passed to each exception, although it's already clear from context which argument is invalid.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="da767-119">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="da767-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="da767-120">Yukarıdaki örneklerde, onay iletisindeki dize `"array != null"` veya `"array.Length == 1"` dahil olmak üzere geliştiricinin başarısız olan neleri belirlemesine yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="da767-120">In the above examples, including the string `"array != null"` or `"array.Length == 1"` in the assert message would help the developer determine what failed.</span></span> <span data-ttu-id="da767-121">`CallerArgumentExpression`girin: Framework 'ün belirli bir yöntem bağımsız değişkeniyle ilişkili dizeyi almak için kullanabileceği bir özniteliği.</span><span class="sxs-lookup"><span data-stu-id="da767-121">Enter `CallerArgumentExpression`: it's an attribute the framework can use to obtain the string associated with a particular method argument.</span></span> <span data-ttu-id="da767-122">Bunu şöyle `Debug.Assert` ekleyeceğiz</span><span class="sxs-lookup"><span data-stu-id="da767-122">We would add it to `Debug.Assert` like so</span></span>

```csharp
public static class Debug
{
    public static void Assert(bool condition, [CallerArgumentExpression("condition")] string message = null);
}
```

<span data-ttu-id="da767-123">Yukarıdaki örnekteki kaynak kodu aynı kalır.</span><span class="sxs-lookup"><span data-stu-id="da767-123">The source code in the above example would stay the same.</span></span> <span data-ttu-id="da767-124">Ancak, derleyicinin gerçekten gösterdiği kod şu şekilde gelir</span><span class="sxs-lookup"><span data-stu-id="da767-124">However, the code the compiler actually emits would correspond to</span></span>

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null, "array != null");
    Debug.Assert(array.Length == 1, "array.Length == 1");

    return array[0];
}
```

<span data-ttu-id="da767-125">Derleyici, `Debug.Assert`özniteliği özel olarak tanır.</span><span class="sxs-lookup"><span data-stu-id="da767-125">The compiler specially recognizes the attribute on `Debug.Assert`.</span></span> <span data-ttu-id="da767-126">Bu, çağrı sitesindeki öznitelik oluşturucusunda (Bu durumda, `condition`) başvurulan bağımsız değişkenle ilişkili dizeyi geçirir.</span><span class="sxs-lookup"><span data-stu-id="da767-126">It passes the string associated with the argument referred to in the attribute's constructor (in this case, `condition`) at the call site.</span></span> <span data-ttu-id="da767-127">Herhangi bir onaylama başarısız olduğunda, geliştirici false olan koşul olarak gösterilir ve hangisinin başarısız olduğunu bilecektir.</span><span class="sxs-lookup"><span data-stu-id="da767-127">When either assert fails, the developer will be shown the condition that was false and will know which one failed.</span></span>

<span data-ttu-id="da767-128">Bağımsız değişken doğrulaması için, öznitelik doğrudan kullanılamaz, ancak bir yardımcı sınıf aracılığıyla kullanımı yapılabilir:</span><span class="sxs-lookup"><span data-stu-id="da767-128">For argument validation, the attribute cannot be used directly, but can be made use of through a helper class:</span></span>

```csharp
public static class Verify
{
    public static void Argument(bool condition, string message, [CallerArgumentExpression("condition")] string conditionExpression = null)
    {
        if (!condition) throw new ArgumentException(message: message, paramName: conditionExpression);
    }

    public static void InRange(int argument, int low, int high,
        [CallerArgumentExpression("argument")] string argumentExpression = null,
        [CallerArgumentExpression("low")] string lowExpression = null,
        [CallerArgumentExpression("high")] string highExpression = null)
    {
        if (argument < low)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be less than {lowExpression} ({low}).");
        }

        if (argument > high)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be greater than {highExpression} ({high}).");
        }
    }

    public static void NotNull<T>(T argument, [CallerArgumentExpression("argument")] string argumentExpression = null)
        where T : class
    {
        if (argument == null) throw new ArgumentNullException(paramName: argumentExpression);
    }
}

T Single<T>(this T[] array)
{
    Verify.NotNull(array); // paramName: "array"
    Verify.Argument(array.Length == 1, "Array must contain a single element."); // paramName: "array.Length == 1"

    return array[0];
}

T ElementAt(this T[] array, int index)
{
    Verify.NotNull(array); // paramName: "array"
    // paramName: "index"
    // message: "index (-1) cannot be less than 0 (0).", or
    //          "index (6) cannot be greater than array.Length - 1 (5)."
    Verify.InRange(index, 0, array.Length - 1);

    return array[index];
}
```

<span data-ttu-id="da767-129">Çerçeveye bu tür bir yardımcı sınıfı eklemek için bir teklif https://github.com/dotnet/corefx/issues/17068.</span><span class="sxs-lookup"><span data-stu-id="da767-129">A proposal to add such a helper class to the framework is underway at https://github.com/dotnet/corefx/issues/17068.</span></span> <span data-ttu-id="da767-130">Bu dil özelliği uygulanmışsa, teklif bu özellikten faydalanmak için güncelleştirilemeyebilir.</span><span class="sxs-lookup"><span data-stu-id="da767-130">If this language feature was implemented, the proposal could be updated to take advantage of this feature.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="da767-131">Uzantı yöntemleri</span><span class="sxs-lookup"><span data-stu-id="da767-131">Extension methods</span></span>

<span data-ttu-id="da767-132">Bir genişletme yönteminde `this` parametreye `CallerArgumentExpression`tarafından başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="da767-132">The `this` parameter in an extension method may be referenced by `CallerArgumentExpression`.</span></span> <span data-ttu-id="da767-133">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="da767-133">For example:</span></span>

```csharp
public static void ShouldBe<T>(this T @this, T expected, [CallerArgumentExpression("this")] string thisExpression = null) {}

contestant.Points.ShouldBe(1337); // thisExpression: "contestant.Points"
```

<span data-ttu-id="da767-134">`thisExpression`, noktadan önceki nesneye karşılık gelen ifadeyi alır.</span><span class="sxs-lookup"><span data-stu-id="da767-134">`thisExpression` will receive the expression corresponding to the object before the dot.</span></span> <span data-ttu-id="da767-135">Statik yöntem sözdizimi ile çağrılırsa, örneğin `Ext.ShouldBe(contestant.Points, 1337)`, ilk parametre `this`işaretlenmediği gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="da767-135">If it's called with static method syntax, e.g. `Ext.ShouldBe(contestant.Points, 1337)`, it will behave as if first parameter wasn't marked `this`.</span></span>

<span data-ttu-id="da767-136">Her zaman `this` parametresine karşılık gelen bir ifade olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="da767-136">There should always be an expression corresponding to the `this` parameter.</span></span> <span data-ttu-id="da767-137">Bir sınıf örneği, bir koleksiyon türü içinden `this.Single()`, örneğin, bir koleksiyon türünün içinden bir genişletme yöntemi çağırırsa, bu `this` derleyici tarafından `"this"` geçirilir.</span><span class="sxs-lookup"><span data-stu-id="da767-137">Even if an instance of a class calls an extension method on itself, e.g. `this.Single()` from inside a collection type, the `this` is mandated by the compiler so `"this"` will get passed.</span></span> <span data-ttu-id="da767-138">Bu kural gelecekte değiştirilirse `null` veya boş dize geçirmeyi düşünebiliriz.</span><span class="sxs-lookup"><span data-stu-id="da767-138">If this rule is changed in the future, we can consider passing `null` or the empty string.</span></span>

### <a name="extra-details"></a><span data-ttu-id="da767-139">Ek ayrıntılar</span><span class="sxs-lookup"><span data-stu-id="da767-139">Extra details</span></span>

- <span data-ttu-id="da767-140">`CallerMemberName`gibi diğer `Caller*` öznitelikleri gibi, bu öznitelik yalnızca varsayılan değerleri olan parametrelerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="da767-140">Like the other `Caller*` attributes, such as `CallerMemberName`, this attribute may only be used on parameters with default values.</span></span>
- <span data-ttu-id="da767-141">Yukarıda gösterildiği gibi `CallerArgumentExpression` ile işaretlenen birden çok parametreye izin verilir.</span><span class="sxs-lookup"><span data-stu-id="da767-141">Multiple parameters marked with `CallerArgumentExpression` are permitted, as shown above.</span></span>
- <span data-ttu-id="da767-142">Özniteliğin ad alanı `System.Runtime.CompilerServices`olacaktır.</span><span class="sxs-lookup"><span data-stu-id="da767-142">The attribute's namespace will be `System.Runtime.CompilerServices`.</span></span>
- <span data-ttu-id="da767-143">`null` veya parametre adı olmayan bir dize (örn. `"notAParameterName"`) sağlanmışsa, derleyici boş bir dizeye geçer.</span><span class="sxs-lookup"><span data-stu-id="da767-143">If `null` or a string that is not a parameter name (e.g. `"notAParameterName"`) is provided, the compiler will pass in an empty string.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="da767-144">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="da767-144">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="da767-145">Dederleyicilerin nasıl kullanılacağını bilen kişiler, Bu öznitelikle işaretlenen yöntemler için çağrı sitelerindeki kaynak kodların bazılarını görebilir.</span><span class="sxs-lookup"><span data-stu-id="da767-145">People who know how to use decompilers will be able to see some of the source code at call sites for methods marked with this attribute.</span></span> <span data-ttu-id="da767-146">Bu, kapalı kaynaklı yazılım için istenmeyen/beklenmeyen bir durum olabilir.</span><span class="sxs-lookup"><span data-stu-id="da767-146">This may be undesirable/unexpected for closed-source software.</span></span>

- <span data-ttu-id="da767-147">Bu, özelliğin kendisindeki bir hata olmamasına karşın, bir sorun kaynağı bugün yalnızca bir `bool`alan bir API `Debug.Assert` var olabilir.</span><span class="sxs-lookup"><span data-stu-id="da767-147">Although this is not a flaw in the feature itself, a source of concern may be that there exists a `Debug.Assert` API today that only takes a `bool`.</span></span> <span data-ttu-id="da767-148">Bir ileti almaya yönelik aşırı yükleme, ikinci parametresi Bu öznitelikle işaretlenmiş ve isteğe bağlı hale getirilse bile, derleyici aşırı yükleme çözünürlüğünde hiç ileti yok seçeneğini de seçer.</span><span class="sxs-lookup"><span data-stu-id="da767-148">Even if the overload taking a message had its second parameter marked with this attribute and made optional, the compiler would still pick the no-message one in overload resolution.</span></span> <span data-ttu-id="da767-149">Bu nedenle, bu özellikten yararlanmak için, hiçbir ileti aşırı yüklemesinin kaldırılması gerekir, bu da bir ikili (kaynak olmayan) bir değişiklik olabilir.</span><span class="sxs-lookup"><span data-stu-id="da767-149">Therefore, the no-message overload would have to be removed to take advantage of this feature, which would be a binary (although not source) breaking change.</span></span>

## <a name="alternatives"></a><span data-ttu-id="da767-150">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="da767-150">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="da767-151">Bu özniteliği kullanan yöntemler için çağrı sitelerinde kaynak kodu görüyoruz bir sorun olması durumunda, özniteliğin etkilerini kabul edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="da767-151">If being able to see source code at call sites for methods that use this attribute proves to be a problem, we can make the attribute's effects opt-in.</span></span> <span data-ttu-id="da767-152">Geliştiriciler, `AssemblyInfo.cs`yerleştirdikleri derleme genelinde `[assembly: EnableCallerArgumentExpression]` özniteliği aracılığıyla etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="da767-152">Developers will enable it through an assembly-wide `[assembly: EnableCallerArgumentExpression]` attribute they put in `AssemblyInfo.cs`.</span></span>
  - <span data-ttu-id="da767-153">Özniteliğin etkileri etkin değilse, öznitelik ile işaretlenen çağırma yöntemleri bir hata olmaz, mevcut yöntemlerin özniteliği kullanmasına ve kaynak uyumluluğunu korumasına imkan tanır.</span><span class="sxs-lookup"><span data-stu-id="da767-153">In the case the attribute's effects are not enabled, calling methods marked with the attribute would not be an error, to allow existing methods to use the attribute and maintain source compatibility.</span></span> <span data-ttu-id="da767-154">Ancak, öznitelik yok sayılır ve yöntem varsayılan değer sağlandıysa çağırılır.</span><span class="sxs-lookup"><span data-stu-id="da767-154">However, the attribute would be ignored and the method would be called with whatever default value was provided.</span></span>

```csharp
// Assembly1

void Foo(string bar); // V1
void Foo(string bar, string barExpression = "not provided"); // V2
void Foo(string bar, [CallerArgumentExpression("bar")] string barExpression = "not provided"); // V3

// Assembly2

Foo(a); // V1: Compiles to Foo(a), V2, V3: Compiles to Foo(a, "not provided")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")

// Assembly3

[assembly: EnableCallerArgumentExpression]

Foo(a); // V1: Compiles to Foo(a), V2: Compiles to Foo(a, "not provided"), V3: Compiles to Foo(a, "a")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")
```

- <span data-ttu-id="da767-155">`Debug.Assert`'e yeni arayan bilgileri eklemek istediğimiz her seferinde [ikili uyumluluk sorununun] [ drawbacks] oluşmasını önlemek için, bir alternatif çözüm, çağıran ile ilgili tüm gerekli bilgileri içeren çerçeveye bir `CallerInfo` struct eklemektir.</span><span class="sxs-lookup"><span data-stu-id="da767-155">To prevent the [binary compatibility problem][drawbacks] from occurring every time we want to add new caller info to `Debug.Assert`, an alternative solution would be to add a `CallerInfo` struct to the framework that contains all the necessary information about the caller.</span></span>

```csharp
struct CallerInfo
{
    public string MemberName { get; set; }
    public string TypeName { get; set; }
    public string Namespace { get; set; }
    public string FullTypeName { get; set; }
    public string FilePath { get; set; }
    public int LineNumber { get; set; }
    public int ColumnNumber { get; set; }
    public Type Type { get; set; }
    public MethodBase Method { get; set; }
    public string[] ArgumentExpressions { get; set; }
}

[Flags]
enum CallerInfoOptions
{
    MemberName = 1, TypeName = 2, ...
}

public static class Debug
{
    public static void Assert(bool condition,
        // If a flag is not set here, the corresponding CallerInfo member is not populated by the caller, so it's
        // pay-for-play friendly.
        [CallerInfo(CallerInfoOptions.FilePath | CallerInfoOptions.Method | CallerInfoOptions.ArgumentExpressions)] CallerInfo callerInfo = default(CallerInfo))
    {
        string filePath = callerInfo.FilePath;
        MethodBase method = callerInfo.Method;
        string conditionExpression = callerInfo.ArgumentExpressions[0];
        ...
    }
}

class Bar
{
    void Foo()
    {
        Debug.Assert(false);

        // Translates to:

        var callerInfo = new CallerInfo();
        callerInfo.FilePath = @"C:\Bar.cs";
        callerInfo.Method = MethodBase.GetCurrentMethod();
        callerInfo.ArgumentExpressions = new string[] { "false" };
        Debug.Assert(false, callerInfo);
    }
}
```

<span data-ttu-id="da767-156">Bu, başlangıçta https://github.com/dotnet/csharplang/issues/87önerilir.</span><span class="sxs-lookup"><span data-stu-id="da767-156">This was originally proposed at https://github.com/dotnet/csharplang/issues/87.</span></span>

<span data-ttu-id="da767-157">Bu yaklaşımın birkaç dezavantajları vardır:</span><span class="sxs-lookup"><span data-stu-id="da767-157">There are a few disadvantages of this approach:</span></span>

- <span data-ttu-id="da767-158">İhtiyacınız olan özellikleri belirtmenize izin vererek kolay bir şekilde ücret ödemenize rağmen, belirtme süresi geçtiğinde bile ifadeler/çağrı `MethodBase.GetCurrentMethod` için bir dizi ayırarak performans açısından de ciddi ölçüde zarar verebilir.</span><span class="sxs-lookup"><span data-stu-id="da767-158">Despite being pay-for-play friendly by allowing you to specify which properties you need, it could still hurt perf significantly by allocating an array for the expressions/calling `MethodBase.GetCurrentMethod` even when the assert passes.</span></span>

- <span data-ttu-id="da767-159">Ayrıca, `CallerInfo` özniteliğine yeni bir bayrak geçirirken, bu yeni parametreyi metodun eski bir sürümüne göre derlenen çağrı sitelerinden almak için `Debug.Assert` garanti edilmez.</span><span class="sxs-lookup"><span data-stu-id="da767-159">Additionally, while passing a new flag to the `CallerInfo` attribute won't be a breaking change, `Debug.Assert` won't be guaranteed to actually receive that new parameter from call sites that compiled against an old version of the method.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="da767-160">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="da767-160">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="da767-161">TBD</span><span class="sxs-lookup"><span data-stu-id="da767-161">TBD</span></span>

## <a name="design-meetings"></a><span data-ttu-id="da767-162">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="da767-162">Design meetings</span></span>

<span data-ttu-id="da767-163">YOK</span><span class="sxs-lookup"><span data-stu-id="da767-163">N/A</span></span>
