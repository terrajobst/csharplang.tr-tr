---
ms.openlocfilehash: fa3326bf69c83b6042b1db7b5567fd5c28d6f81a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484526"
---
# <a name="callerargumentexpression"></a>CallerArgumentExpression

* [x] önerilir
* [] Prototipi: başlatılmadı
* [] Uygulama: başlatılmadı
* [] Belirtimi: başlatılmadı

## <a name="summary"></a>Özet
[summary]: #summary

Geliştiricilerin, tanılama/test API 'Lerinde daha iyi hata iletileri sağlamak ve tuş vuruşlarını azaltmak için bir yönteme geçirilen ifadeleri yakalamasına izin verin.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Bir onaylama veya bağımsız değişken doğrulaması başarısız olursa, geliştirici nerede ve neden başarısız olduğu konusunda olabildiğince fazla bilgi almak ister. Bununla birlikte, günümüzün tanılama API 'Leri bunu tam olarak kolaylaştırmaz. Aşağıdaki yöntemi göz önünde bulundurun:

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null);
    Debug.Assert(array.Length == 1);

    return array[0];
}
```

Onaylamalar başarısız olduğunda, yığın izlemesinde yalnızca dosya adı, satır numarası ve Yöntem adı sağlanacaktır. Geliştirici bu bilgilerden hangi onaylama işlemi başarısız olduğunu bildiremeyecektir. Bu, ne olduğunu görmek için dosyayı açmak ve girilen satır numarasına gitmeniz gerekecektir.

Bu ayrıca test çerçevelerinin çeşitli onay yöntemleri sağlamasına neden olur. XUnit ile `Assert.True` ve `Assert.False` sık kullanılmaz, çünkü başarısız olanlar hakkında yeterli bağlam sunmazlar.

Durum bağımsız değişken doğrulaması için biraz daha iyidir, çünkü geçersiz bağımsız değişkenlerin adları geliştiriciyle gösteriliyor, geliştirici bu adları özel durumlara el ile iletmelidir. Yukarıdaki örnek `Debug.Assert`yerine geleneksel bağımsız değişken doğrulamasını kullanmak için yeniden yazıldı, şöyle görünür

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

`nameof(array)`, bu bağımsız değişken geçersiz olan bağlamdan zaten açık olsa da, her bir özel duruma geçirilmesi gerektiğini unutmayın.

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

Yukarıdaki örneklerde, onay iletisindeki dize `"array != null"` veya `"array.Length == 1"` dahil olmak üzere geliştiricinin başarısız olan neleri belirlemesine yardımcı olur. `CallerArgumentExpression`girin: Framework 'ün belirli bir yöntem bağımsız değişkeniyle ilişkili dizeyi almak için kullanabileceği bir özniteliği. Bunu şöyle `Debug.Assert` ekleyeceğiz

```csharp
public static class Debug
{
    public static void Assert(bool condition, [CallerArgumentExpression("condition")] string message = null);
}
```

Yukarıdaki örnekteki kaynak kodu aynı kalır. Ancak, derleyicinin gerçekten gösterdiği kod şu şekilde gelir

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null, "array != null");
    Debug.Assert(array.Length == 1, "array.Length == 1");

    return array[0];
}
```

Derleyici, `Debug.Assert`özniteliği özel olarak tanır. Bu, çağrı sitesindeki öznitelik oluşturucusunda (Bu durumda, `condition`) başvurulan bağımsız değişkenle ilişkili dizeyi geçirir. Herhangi bir onaylama başarısız olduğunda, geliştirici false olan koşul olarak gösterilir ve hangisinin başarısız olduğunu bilecektir.

Bağımsız değişken doğrulaması için, öznitelik doğrudan kullanılamaz, ancak bir yardımcı sınıf aracılığıyla kullanımı yapılabilir:

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

Çerçeveye bu tür bir yardımcı sınıfı eklemek için bir teklif https://github.com/dotnet/corefx/issues/17068. Bu dil özelliği uygulanmışsa, teklif bu özellikten faydalanmak için güncelleştirilemeyebilir.

### <a name="extension-methods"></a>Uzantı yöntemleri

Bir genişletme yönteminde `this` parametreye `CallerArgumentExpression`tarafından başvurulabilir. Örneğin:

```csharp
public static void ShouldBe<T>(this T @this, T expected, [CallerArgumentExpression("this")] string thisExpression = null) {}

contestant.Points.ShouldBe(1337); // thisExpression: "contestant.Points"
```

`thisExpression`, noktadan önceki nesneye karşılık gelen ifadeyi alır. Statik yöntem sözdizimi ile çağrılırsa, örneğin `Ext.ShouldBe(contestant.Points, 1337)`, ilk parametre `this`işaretlenmediği gibi davranır.

Her zaman `this` parametresine karşılık gelen bir ifade olmalıdır. Bir sınıf örneği, bir koleksiyon türü içinden `this.Single()`, örneğin, bir koleksiyon türünün içinden bir genişletme yöntemi çağırırsa, bu `this` derleyici tarafından `"this"` geçirilir. Bu kural gelecekte değiştirilirse `null` veya boş dize geçirmeyi düşünebiliriz.

### <a name="extra-details"></a>Ek ayrıntılar

- `CallerMemberName`gibi diğer `Caller*` öznitelikleri gibi, bu öznitelik yalnızca varsayılan değerleri olan parametrelerde kullanılabilir.
- Yukarıda gösterildiği gibi `CallerArgumentExpression` ile işaretlenen birden çok parametreye izin verilir.
- Özniteliğin ad alanı `System.Runtime.CompilerServices`olacaktır.
- `null` veya parametre adı olmayan bir dize (örn. `"notAParameterName"`) sağlanmışsa, derleyici boş bir dizeye geçer.

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

- Dederleyicilerin nasıl kullanılacağını bilen kişiler, Bu öznitelikle işaretlenen yöntemler için çağrı sitelerindeki kaynak kodların bazılarını görebilir. Bu, kapalı kaynaklı yazılım için istenmeyen/beklenmeyen bir durum olabilir.

- Bu, özelliğin kendisindeki bir hata olmamasına karşın, bir sorun kaynağı bugün yalnızca bir `bool`alan bir API `Debug.Assert` var olabilir. Bir ileti almaya yönelik aşırı yükleme, ikinci parametresi Bu öznitelikle işaretlenmiş ve isteğe bağlı hale getirilse bile, derleyici aşırı yükleme çözünürlüğünde hiç ileti yok seçeneğini de seçer. Bu nedenle, bu özellikten yararlanmak için, hiçbir ileti aşırı yüklemesinin kaldırılması gerekir, bu da bir ikili (kaynak olmayan) bir değişiklik olabilir.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

- Bu özniteliği kullanan yöntemler için çağrı sitelerinde kaynak kodu görüyoruz bir sorun olması durumunda, özniteliğin etkilerini kabul edebilirsiniz. Geliştiriciler, `AssemblyInfo.cs`yerleştirdikleri derleme genelinde `[assembly: EnableCallerArgumentExpression]` özniteliği aracılığıyla etkinleştirir.
  - Özniteliğin etkileri etkin değilse, öznitelik ile işaretlenen çağırma yöntemleri bir hata olmaz, mevcut yöntemlerin özniteliği kullanmasına ve kaynak uyumluluğunu korumasına imkan tanır. Ancak, öznitelik yok sayılır ve yöntem varsayılan değer sağlandıysa çağırılır.

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

- `Debug.Assert`'e yeni arayan bilgileri eklemek istediğimiz her seferinde [ikili uyumluluk sorununun] [ drawbacks] oluşmasını önlemek için, bir alternatif çözüm, çağıran ile ilgili tüm gerekli bilgileri içeren çerçeveye bir `CallerInfo` struct eklemektir.

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

Bu, başlangıçta https://github.com/dotnet/csharplang/issues/87önerilir.

Bu yaklaşımın birkaç dezavantajları vardır:

- İhtiyacınız olan özellikleri belirtmenize izin vererek kolay bir şekilde ücret ödemenize rağmen, belirtme süresi geçtiğinde bile ifadeler/çağrı `MethodBase.GetCurrentMethod` için bir dizi ayırarak performans açısından de ciddi ölçüde zarar verebilir.

- Ayrıca, `CallerInfo` özniteliğine yeni bir bayrak geçirirken, bu yeni parametreyi metodun eski bir sürümüne göre derlenen çağrı sitelerinden almak için `Debug.Assert` garanti edilmez.

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

TBD

## <a name="design-meetings"></a>Tasarım toplantıları

YOK
