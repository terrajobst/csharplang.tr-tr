---
ms.openlocfilehash: 9ed1aa75d581cecbf754a84d1f523c6334b8c0ac
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79485261"
---
# <a name="async-streams"></a>Zaman uyumsuz akışlar

* [x] önerilir
* [x] prototipi
* [] Uygulama
* [] Belirtimi

## <a name="summary"></a>Özet
[summary]: #summary

C#Yineleyici yöntemlerine ve zaman uyumsuz yöntemlere yönelik desteğe sahiptir, ancak hem Yineleyici hem de async yöntemi olan bir yöntem için destek vermez.  `await`, yeni bir `IEnumerable<T>` `IEnumerator<T>`tüketilebilir bir `IAsyncEnumerable<T>` veya `await foreach`yerine bir `IAsyncEnumerable<T>` veya `IAsyncEnumerator<T>` döndüren yeni bir `async` Yineleyici biçiminde kullanılmasına izin vererek bunu düzeltmemiz gerekir.  Zaman uyumsuz temizlemeyi etkinleştirmek için bir `IAsyncDisposable` arabirimi de kullanılır.

## <a name="related-discussion"></a>İlgili tartışma
- https://github.com/dotnet/roslyn/issues/261
- https://github.com/dotnet/roslyn/issues/114

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

## <a name="interfaces"></a>Arabirimler

### <a name="iasyncdisposable"></a>IAsyncDisposable

`IAsyncDisposable` çok fazla tartışma vardır (örn. https://github.com/dotnet/roslyn/issues/114) ve bunun iyi bir fikir olup olmadığı.  Ancak, zaman uyumsuz yineleyiciler desteğini eklemek için gerekli bir kavramdır.  `finally` blokları `await`s içerdiğinden ve `finally` bloklarının yineleyiciler elden çıkarılma kapsamında çalıştırılması gerektiğinden, zaman uyumsuz olarak elden çıkarılmız gerekir.  Ayrıca, kaynakların temizlenmesi her zaman herhangi bir süre alabilir (örneğin, temizleme gerektiren), kaydını kaldırmak geri çağırmaları ve kayıt silme işleminin ne zaman tamamlandığını bilmeniz için bir yol sağlar.

Çekirdek .NET kitaplıklarına aşağıdaki arabirim eklenmiştir (örn. System. Private. CoreLib/System. Runtime):
```csharp
namespace System
{
    public interface IAsyncDisposable
    {
        ValueTask DisposeAsync();
    }
}
```
`Dispose`olduğu gibi, `DisposeAsync` birden çok kez çağırma kabul edilebilir ve ilki, zaman uyumlu olarak tamamlanan başarılı bir görev döndüren (`DisposeAsync` iş parçacığı açısından güvenli olmaması, ancak eşzamanlı çağrıyı desteklememesinin gerekli olması gerekir).  Ayrıca, türler hem `IDisposable` hem de `IAsyncDisposable`uygulayabilir ve bu durumda, `Dispose` çağırma ve `DisposeAsync` sonra da yalnızca ilki anlamlı olmalıdır ve sonraki etkinleştirmeleri bir NOP olmalıdır.  Bu nedenle, bir tür her ikisini de uygulıyorsa, tüketicilerin bir kez çağırmaları ve bağlamı temel alan daha ilgili yöntemi, zaman uyumlu bağlamlarda `Dispose` ve zaman uyumsuz olanlarda `DisposeAsync`.

(`IAsyncDisposable` `using` ile ayrı bir tartışmaya nasıl etkileşime giyorum.  Bu teklifin ilerleyen kısımlarında `foreach` ile nasıl etkileşime girdiğinin kapsamı.)

Dikkate alınan alternatifler:
- _`CancellationToken`kabul`DisposeAsync`_ : teorik olarak, zaman uyumsuz bir şekilde iptal edilebilir, elden çıkarma Temizleme, işlemleri kapatma, ücretsiz kaynakları, vb., genellikle iptal edilmesi gereken bir şey değildir. iptal edilen işler için Temizleme hala önemlidir.  Fiili çalışmanın iptal edilmesine neden olan `CancellationToken`, genellikle, işin iptal edilmesi `DisposeAsync` bir NOP olmasına neden olacağından, `DisposeAsync` bir `DisposeAsync`geçirilen belirteç olur.  Birisi, elden çıkarılmayı beklerken engellenmemek isterse, sonuçta elde edilen `ValueTask`beklemeyi veya yalnızca belirli bir süre boyunca beklemeyi önleyebilirsiniz.
- _`DisposeAsync` `Task`döndürme_ : artık genel olmayan bir `ValueTask` var ve bir `IValueTaskSource`oluşturulabilir. bundan sonra `ValueTask` döndürme, var olan bir nesnenin `DisposeAsync` son zaman uyumsuz tamamlamayı temsil eden Promise olarak yeniden kullanılmasına izin veriyor ve `DisposeAsync`zaman uyumsuz olarak tamamlandığına `Task` bir ayırmayı kaydetmektir.`DisposeAsync`
- _`DisposeAsync` `bool continueOnCapturedContext` (`ConfigureAwait`) Ile yapılandırma_: Bu tür bir kavramın `using`, `foreach`ve bunu kullanan diğer dil yapılarına, bir arabirim perspektifinden, hiçbir `await`değil ve yapılandırma için hiçbir şey yapma gibi sorunlar olabilir... `ValueTask` tüketicileri, ancak istedikleri gibi kullanabilir.
- _`IAsyncDisposable` devralan `IDisposable`_ : yalnızca bir veya diğeri kullanılması gerektiğinden, her ikisini de uygulamak için türleri zorlamak mantıklı değildir.
- _`IAsyncDisposable`yerine`IDisposableAsync`_ : "zaman uyumsuz bir şey" olan ve işlemler "tamamlandı" olarak, bu nedenle türlerin ön ek olarak "Async" olması ve yöntemlerin bir sonek olarak "zaman uyumsuz" olması gerekir.

### <a name="iasyncenumerable--iasyncenumerator"></a>Iasyncenumerable/ıasyncenumerator

Çekirdek .NET kitaplıklarına iki arabirim eklenir:

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken cancellationToken = default);
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> MoveNextAsync();
        T Current { get; }
    }
}
```

Tipik tüketim (ek dil özellikleri olmadan) şöyle görünür:

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
        Use(enumerator.Current);
    }
}
finally { await enumerator.DisposeAsync(); }
```

Değerlendirilen atılan seçenekler:
- _`Task<bool> MoveNextAsync(); T current { get; }`_ : `Task<bool>` kullanılması, zaman uyumlu, başarılı `MoveNextAsync` çağrılarını temsil etmek için önbelleğe alınmış bir görev nesnesinin kullanımını destekleyecektir, ancak zaman uyumsuz tamamlama için bir ayırma gerekli olmaya devam eder.  `ValueTask<bool>`döndüleyerek, Numaralandırıcı nesnesinin kendisi için `IValueTaskSource<bool>` uygulaması ve `MoveNextAsync`döndürülen `ValueTask<bool>` için yedekleme olarak kullanılması ve ayrıca önemli ölçüde azaltılan büyük kafa için izin verir.
- _`ValueTask<(bool, T)> MoveNextAsync();`_ : yalnızca tüketilmek zordur, ancak `T` artık değişkenle birlikte olmaması anlamına gelir.
- _`ValueTask<T?> TryMoveNextAsync();`_ : covaryant değil.
- _`Task<T?> TryMoveNextAsync();`_ : birlikte değişken değil, her çağrıda ayırmalar, vb.
- _`ITask<T?> TryMoveNextAsync();`_ : birlikte değişken değil, her çağrıda ayırmalar, vb.
- _`ITask<(bool,T)> TryMoveNextAsync();`_ : birlikte değişken değil, her çağrıda ayırmalar, vb.
- _`Task<bool> TryMoveNextAsync(out T result);`_ : `out` sonucunun, işlem zaman uyumlu olarak döndürüldüğünde, daha sonra görevi zaman uyumsuz olarak tamamladığı zaman, sonucu iletmenin bir yolu olmadığı anlamına gelir.
- _`IAsyncEnumerator<T>` `IAsyncDisposable`uygulamayız_ : bunları ayırmanızı seçebiliriz.  Ancak, daha sonra, kodun bir Numaralandırıcı sağlamaması olasılığını karşılayamaz ve bu sayede, model tabanlı yardımcılar yazmayı zorlaştırılması gerekir.  Ayrıca, numaralandırıcıların elden çıkarılma ihtiyacı olan (ör. bir finally bloğuna sahip tüm C# zaman uyumsuz Yineleyici, bir ağ bağlantısından verileri numaralandırma, vb.) ortak olacaktır ve bunlardan biri yoksa, yöntemi yalnızca en az ek yük `public ValueTask DisposeAsync() => default(ValueTask);` olarak uygulamak kolaydır.
- _ `IAsyncEnumerator<T> GetAsyncEnumerator()`: iptal belirteci parametresi yok.

#### <a name="viable-alternative"></a>Uygulanabilir alternatif:

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator();
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> WaitForNextAsync();
        T TryGetNext(out bool success);
    }
}
```

`TryGetNext`, zaman uyumlu olarak kullanılabilir oldukları sürece tek bir arabirim çağrısıyla öğeleri kullanmak için bir iç döngüde kullanılır.  Bir sonraki öğe zaman uyumlu olarak alınamadığından, false döndürür ve false döndürdüğünde bir çağıran, bir sonraki öğenin kullanılabilir olmasını beklemek ya da hiçbir zaman başka bir öğe olmadığını anlamak için `WaitForNextAsync` çağırmalıdır. Tipik tüketim (ek dil özellikleri olmadan) şöyle görünür:

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.WaitForNextAsync())
    {
        while (true)
        {
            int item = enumerator.TryGetNext(out bool success);
            if (!success) break;
            Use(item);
        }
    }
}
finally { await enumerator.DisposeAsync(); }
```

Bunun avantajı iki kat, bir küçük ve bir büyük:
- _İkincil: bir Numaralandırıcının birden çok tüketicileri desteklemesini sağlar_. Bir Numaralandırıcının birden çok eşzamanlı tüketicilerini desteklemesi için değerli senaryolar olabilir.  `MoveNextAsync` ve `Current` ayrı olduğunda bu, bir uygulamanın kullanım atomik hale getirme yapabilmeleri için elde edilebilir.  Buna karşılık bu yaklaşım, numaralandırıcı İleri doğru göndermeyi ve sonraki öğeyi almayı destekleyen `TryGetNext` tek bir yöntem sağlar. bu nedenle, Numaralandırıcı istenirse, Numaralandırıcının kararlılık 'i sağlayabilir.  Ancak, bu tür senaryolar, her tüketiciye paylaşılan bir numaralandırıcıdan kendi Numaralandırıcı vererek da etkinleştirilebilir.  Ayrıca, her Numaralandırıcı için çok sayıda büyük kafa gerektirmeyen büyük/küçük harfe basit olmayan büyük kafa ekler ve bu da bir arabirimin tüketicisinin genellikle bu herhangi bir şekilde dayanmadığı anlamına gelir.
- _Birincil: performans_. `MoveNextAsync`/`Current` yaklaşımı, işlem başına iki arabirim çağrısı gerektirir, ancak `WaitForNextAsync`/`TryGetNext` için en iyi durum, her bir işlem için yalnızca bir arabirim çağrısına sahip olduğumuz, `TryGetNext`ile sıkı bir iç döngüyü etkinleştirerek, en çok yineleme zaman uyumlu olarak tamamlanır.  Bu durum, arabirimin, hesaplamayı ayırt ettiği durumlarda ölçülebilir bir etkiye sahip olabilir.

Bununla birlikte, bu, el ile kullanılırken büyük ölçüde artan karmaşıklık ve bunları kullanırken hatalara yönelik daha fazla şans dahil olmak üzere, önemsiz olmayan küçük yanlar vardır.  Ayrıca, performans avantajları mikro kıyaslamalar halinde gösterilirken, gerçek kullanımın büyük çoğunluğunda ne kadar etkili olduğunu düşünmedik.  Bunlar ise, hafif bir şekilde ikinci bir arabirim kümesi tanıtıyoruz.

Değerlendirilen atılan seçenekler:
- `ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: `out` parametreler birlikte değişken olamaz.  Burada, bu durum büyük olasılıkla başvuru türü sonuçları için bir çalışma zamanı yazma engeli olan küçük bir etkisi (genel olarak TRY düzeniyle ilgili bir sorun) de vardır.

#### <a name="cancellation"></a>İptal

İptali desteklemeye yönelik birkaç olası yaklaşım vardır:
1. `IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` iptal-belirsiz: `CancellationToken` hiçbir yerde görünmez.  İptal işlemi, `CancellationToken` uygun şekilde sayılabilen ve/veya numaralandırıcıların mantıklı bir şekilde (örneğin, bir yineleyici çağrılırken), `CancellationToken` Yineleyici yöntemine bir bağımsız değişken olarak geçirerek ve yineleyicinin gövdesinde, diğer herhangi bir parametre ile yapıldığı gibi mantıksal olarak bir şekilde inceleyerek elde edilir.
2. `IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: bir `CancellationToken` `GetAsyncEnumerator`ve sonraki `MoveNextAsync` işlemleri buna göre daha da uygun hale gelir.
3. `IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: `CancellationToken` her bir `MoveNextAsync` çağrısına geçiş yapabilirsiniz.
4. 1 & & 2: `CancellationToken`her ikisi de numaralandırılabilir/numaralandırıcıuygulamanıza katıştırdığınızda ve `CancellationToken`s 'yi `GetAsyncEnumerator`olarak geçitirsiniz.
5. 1 & & 3: `CancellationToken`her ikisi de numaralandırılabilir/Numaralandırıcıya katıştırmanız ve `CancellationToken`s 'yi `MoveNextAsync`'e iletmeniz.

Tamamen teorik olarak bir perspektiften (5) en sağlam bir `CancellationToken` kabul `MoveNextAsync`, bu, iptal edilme ve (b) `CancellationToken`, yalnızca Yineleyicilerde bağımsız değişken olarak geçirilebilecek, rastgele türlere gömülü olan diğer herhangi bir tür olur.

Ancak, bu yaklaşımda birden fazla sorun vardır:
- `GetAsyncEnumerator` geçirilen `CancellationToken`, yineleyici gövdesinde nasıl yapılır?  `GetEnumerator`geçirilen `CancellationToken` erişim sağlamak için, nokta olarak kullanabileceğiniz yeni bir `iterator` anahtar sözcüğü kullanıma sunarız. ancak, çok fazla sayıda ek makine olan b) çok birinci sınıf vatandaşlık ve c), %99 servis talebinin bir yineleyici çağıran ve üzerinde `GetAsyncEnumerator` çağıran aynı kod gibi görünse de, bu durumda `CancellationToken` bir bağımsız değişken olarak yalnızca bir bağımsız değişken olarak geçirebileceği anlamına gelir.
- `MoveNextAsync` geçirilen bir `CancellationToken` yöntemin gövdesine nasıl alınır?  Bu, `iterator` yerel bir nesne tarafından sunulmamış gibi daha da kötüleşirse, bu değerin değeri await genelinde değişebilir, bu da belirteçle kaydedilen tüm kodların, bir süre sonra yeniden kaydolmadan önce kayıt yapması gerekir; Ayrıca, bir yineleyici içinde derleyici tarafından uygulanıp uygulanmadığı veya bir geliştirici tarafından el ile yapılan her `MoveNextAsync` çağrısında kayıt ve kayıt silme yapmak çok pahalı bir işlemdir.
- Geliştirici `foreach` döngüsünü nasıl iptal eder?  Bu işlem, numaralandırılabilir/Numaralandırıcı için bir `CancellationToken` verip, sonra da bir) Numaralandırıcılar üzerinde `foreach`' ı desteklememiz gerekir, bu, bunları ilk sınıf vatandaşları haline getiriyor ve artık numaralandırıcıların (örn. LINQ yöntemleri) veya b 'nin bulunduğu bir ekosistem hakkında düşünmeye başlamanız gerekir. `CancellationToken`, sunulan belirteci depolayan `IAsyncEnumerable<T>` `WithCancellation` uzantı yöntemine sahip olan ve daha sonra döndürülen yapı üzerindeki `GetAsyncEnumerator` olduğunda sarmalanmış numaralandırıcılara iletmeleri gerekir `GetAsyncEnumerator` çağrılır (Bu belirteç yok sayılıyor).  Ya da yalnızca foreach gövdesinde bulunan `CancellationToken` kullanabilirsiniz.
- Sorgu anlama destekleniyorsa, `GetEnumerator` veya `MoveNextAsync` `CancellationToken` sağlanan her bir yan tümcesine nasıl geçirilecek?  En kolay yol, yan tümce yalnızca bunu yakalamak için, `GetAsyncEnumerator`/`MoveNextAsync` yok sayılır.

Bu belgenin önceki bir sürümü önerilir (1), ancak karşılaştık (4).

(1) ile ilgili iki ana sorun:
- üreticileri of cancellenebilir numaralar, bazı ortak uygulama ve derleyicinin yalnızca zaman uyumsuz-yineleyiciler desteğini `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` bir yöntem uygulamak için kullanabilirler.
- birçok üreticileri, bunun yerine zaman uyumsuz olan imzaya bir `CancellationToken` parametresi eklemeyi tercih edilebilir, bu da tüketicilerin bir `IAsyncEnumerable` türü verildiğinde istedikleri iptal belirtecini geçmesini engeller.

İki ana tüketim senaryosu vardır:
1. tüketicinin zaman uyumsuz-yineleyici yöntemini çağırdığı `await foreach (var i in GetData(token)) ...`,
2. tüketicinin verilen bir `IAsyncEnumerable` örneğiyle ilgilenen `await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...`.

Her iki senaryoyu da hem üreticileri hem de Async-Streams kullanıcıları için uygun bir şekilde desteklemeye yönelik makul bir uzlaşma olduğunu fark ettik. Bu, zaman uyumsuz-yineleyici yönteminde özel olarak açıklamalı bir parametre kullanmaktır. `[EnumeratorCancellation]` özniteliği bu amaçla kullanılır. Bu özniteliği bir parametreye yerleştirmek derleyiciye bir belirteç `GetAsyncEnumerator` yöntemine geçirildiğinde, parametre için başlangıçta geçirilen değer yerine bu belirtecin kullanılması gerektiğini bildirir.

`IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`göz önünde bulundurun. Bu yöntemin uygulayıcısı Yöntem gövdesinde parametresini yalnızca kullanabilir. Tüketici, yukarıdaki tüketim desenlerini kullanabilir:
1. `GetData(token)`kullanırsanız, belirteç zaman uyumsuz-sıralanabilir olarak kaydedilir ve yineleme içinde kullanılacaktır,
2. `givenIAsyncEnumerable.WithCancellation(token)`kullanırsanız, `GetAsyncEnumerator` öğesine geçirilen belirteç, zaman uyumsuz-numaralandırılabilir ' a kaydedilen belirtecin yerini alır.

## <a name="foreach"></a>foreach

`foreach`, mevcut `IEnumerable<T>`desteğinin yanı sıra `IAsyncEnumerable<T>` desteklemek için de genişletilebilir.  Ayrıca, ilgili Üyeler genel kullanıma sunulduğunu bir düzen olarak `IAsyncEnumerable<T>` eşdeğerini destekleyecektir, ancak ayırmayı önleyememesi gereken yapı tabanlı uzantıları etkinleştirmek ve `MoveNextAsync` ve `DisposeAsync`dönüş türü olarak alternatif awaitables kullanmak için.

### <a name="syntax"></a>Sözdizimi

Sözdizimini kullanarak:

```csharp
foreach (var i in enumerable)
```

C#, zaman uyumsuz numaralar için ilgili API 'Leri kullanıma sunsa bile, zaman uyumlu olmayan bir şekilde işlemeye `enumerable` devam eder (model ortaya koyar veya arabirimini uygulayarak), yalnızca zaman uyumlu API 'Leri göz önünde bulunduracaktır.

`foreach` yerine yalnızca zaman uyumsuz API 'Leri göz önünde bulundurmanız için, `await` aşağıdaki gibi eklenir:

```csharp
await foreach (var i in enumerable)
```

Async veya Sync API 'Lerini kullanmayı destekleyecek bir sözdizimi sağlanmaz; geliştirici, kullanılan sözdizimine göre seçim yapmanız gerekir.

Değerlendirilen atılan seçenekler:
- _`foreach (var i in await enumerable)`_ : Bu, zaten geçerli bir söz dizimi ve anlamının değiştirilmesi bir son değişiklik olacaktır.  Bu, `enumerable``await`, zaman uyumlu bir şekilde tekrardan geri dönüp bundan sonra zaman uyumlu şekilde yineleyebileceği anlamına gelir.
- _`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)`, `foreach (await var i in enumerable)`_ : Bu tümü bir sonraki öğeyi bekletireceğiz, ancak foreach ' te yer alan başka bir bekler vardır, özellikle de numaralandırılabilir bir `IAsyncDisposable`ise, zaman uyumsuz elden çıkarma elden çıkarıl`await`acaktır.  Bu await, her bir öğe için değil, Foreach 'in kapsamı olarak, `await` anahtar sözcüğünün `foreach` düzeyinde olmasını sağlar.  Ayrıca, `foreach` ile ilişkili olması, farklı bir terime sahip `foreach` (örneğin, "await foreach") anlatmak için bir yol sunar.  Ancak daha da önemlisi, `using` söz dizimi ile aynı anda `foreach` söz konusu sözdizimini göz önünde bulundururken bir değer vardır ve `using (await ...)` zaten geçerli olan sözdizimidir.
- _`foreach await (var i in enumerable)`_

Yine de göz önünde bulundurun:
- Bugün `foreach`, bir Numaralandırıcı boyunca yineleme desteklemez.  `IAsyncEnumerator<T>`s 'nin etrafından daha yaygın olacağını umduk ve bu nedenle hem `IAsyncEnumerable<T>` hem de `IAsyncEnumerator<T>`ile `await foreach` desteklemek de mümkündür.  Ancak bu tür destek eklendikten sonra, `IAsyncEnumerator<T>` birinci sınıf bir vatandaşlık olup olmadığı ve numaralar 'e ek olarak Numaralandırıcılar üzerinde çalışan kombinatör 'nin aşırı yüklemelerinin olması gerekip gerekmediğini ortaya koyuyor musunuz?    Yöntemleri numaralar yerine Numaralandırıcılar döndürecek şekilde teşvik etmek istiyor musunuz? Bunu tartışmak için devam etmemiz gerekir.  Bunu desteklemek istemediğimiz için karar verdiğimiz bir genişletme yöntemi eklemek `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` bir Numaralandırıcının hala `foreach`gibi olmasına imkan tanıdık.  Bunu desteklemek istediğimiz karar verdiğimiz için, `await foreach` numaralandırıcı üzerinde `DisposeAsync` çağırmaktan sorumlu olup olmadığına ve yanıtın büyük olasılıkla "Hayır, elden çıkarılma sırasında denetim, `GetEnumerator`olarak adlandırılıyordu."

### <a name="pattern-based-compilation"></a>Model tabanlı derleme

Derleyici, varsa model tabanlı API 'lere bağlanır, bu da arabirimi kullanarak bunları tercih eder (model, örnek yöntemleriyle veya uzantı yöntemleriyle karşılanacaktır).  Düzenin gereksinimleri şunlardır:
- Numaralandırılabilir değer, bağımsız değişken olmadan çağrılabilecek ve ilgili düzene uyan bir Numaralandırıcı döndüren bir `GetAsyncEnumerator` yöntemi kullanıma sunmalıdır.
- Numaralandırıcı, bağımsız değişken olmadan çağrılabilecek bir `MoveNextAsync` yöntemi kullanıma sunmalı ve `await`gelmiş olabilecek ve `GetResult()` bir `bool`döndüren bir değer döndürüyor.
- Numaralandırıcı Ayrıca alıcı, numaralandırılan veri türünü temsil eden bir `T` döndüren `Current` özelliğini kullanıma sunmalıdır.
- Numaralandırıcı isteğe bağlı olarak, bağımsız değişken olmadan çağrılabilecek bir `DisposeAsync` yöntemi sunabilir ve `await`ve `GetResult()` `void`döndüren bir şey döndürüyor.

Bu kod:

```csharp
var enumerable = ...;
await foreach (T item in enumerable)
{
   ...
}
```

, öğesinin eşdeğerine çevrilir:

```csharp
var enumerable = ...;
var enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
       T item = enumerator.Current;
       ...
    }
}
finally
{
    await enumerator.DisposeAsync(); // omitted, along with the try/finally, if the enumerator doesn't expose DisposeAsync
}
```

Yinelenebilir tür doğru düzende kullanıma sunulmazsa, arabirimler kullanılacaktır.

### <a name="configureawait"></a>ConfigureAwait

Bu kalıp tabanlı derleme, bir `ConfigureAwait` genişletme yöntemi aracılığıyla tüm await üzerinde `ConfigureAwait` kullanılmasına izin verir:

```csharp
await foreach (T item in enumerable.ConfigureAwait(false))
{
   ...
}
```

Bu, büyük olasılıkla System. Threading. Tasks. Extensions. dll ' de .NET 'e ekleyebileceğiniz türlere göre yapılır:

```csharp
// Approximate implementation, omitting arg validation and the like
namespace System.Threading.Tasks
{
    public static class AsyncEnumerableExtensions
    {
        public static ConfiguredAsyncEnumerable<T> ConfigureAwait<T>(this IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext) =>
            new ConfiguredAsyncEnumerable<T>(enumerable, continueOnCapturedContext);

        public struct ConfiguredAsyncEnumerable<T>
        {
            private readonly IAsyncEnumerable<T> _enumerable;
            private readonly bool _continueOnCapturedContext;

            internal ConfiguredAsyncEnumerable(IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext)
            {
                _enumerable = enumerable;
                _continueOnCapturedContext = continueOnCapturedContext;
            }

            public ConfiguredAsyncEnumerator<T> GetAsyncEnumerator() =>
                new ConfiguredAsyncEnumerator<T>(_enumerable.GetAsyncEnumerator(), _continueOnCapturedContext);

            public struct Enumerator
            {
                private readonly IAsyncEnumerator<T> _enumerator;
                private readonly bool _continueOnCapturedContext;

                internal Enumerator(IAsyncEnumerator<T> enumerator, bool continueOnCapturedContext)
                {
                    _enumerator = enumerator;
                    _continueOnCapturedContext = continueOnCapturedContext;
                }

                public ConfiguredValueTaskAwaitable<bool> MoveNextAsync() =>
                    _enumerator.MoveNextAsync().ConfigureAwait(_continueOnCapturedContext);

                public T Current => _enumerator.Current;

                public ConfiguredValueTaskAwaitable DisposeAsync() =>
                    _enumerator.DisposeAsync().ConfigureAwait(_continueOnCapturedContext);
            }
        }
    }
}
```

Bu yaklaşımın, `ConfigureAwait` model tabanlı numaralar birlikte kullanılmasını etkinleştiremediğini unutmayın, ancak bu durumda `ConfigureAwait` yalnızca `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` bir uzantı olarak gösterildiğine ve rastgele bir zaman uyumlu şeylere uygulanamadığından emin olun. yalnızca görevlere uygulandığında anlamlı olduğundan (görevin devamlılık desteği ' nde uygulanan bir davranışı denetler) ve bu nedenle, awasever 'in görev olmadığı bir model kullanırken anlamlı değildir.  Daha fazla işlem döndüren herkes, bu Gelişmiş senaryolarda kendi özel davranışlarını sağlayabilir.

(Kapsam veya derleme düzeyi `ConfigureAwait` çözümünü desteklemeye yönelik bir şekilde karşılaştığımız için bu gerekli değildir.)

## <a name="async-iterators"></a>Zaman uyumsuz yineleyiciler

Dil/derleyici, `IAsyncEnumerable<T>`s ve `IAsyncEnumerator<T>`s oluşturmayı ve bunlara ek olarak desteklemeyi destekleyecektir. Günümüzde dil, şunun gibi bir yineleyici yazmayı destekler:

```csharp
static IEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            Thread.Sleep(1000);
            yield return i;
        }
    }
    finally
    {
        Thread.Sleep(200);
        Console.WriteLine("finally");
    }
}
```

Ancak `await` bu yineleyicilerin gövdesinde kullanılamaz.  Bu desteği ekleyeceğiz.

### <a name="syntax"></a>Sözdizimi

Yineleyiciler için mevcut dil desteği, bir `yield`s içerip içermediğini temel alarak yöntemin yineleyicisini belirtir.  Aynı zaman uyumsuz yineleyiciler için de doğru olacaktır.  Bu zaman uyumsuz yineleyiciler, imzaya `async` eklenerek zaman uyumlu yineleyiciler tarafından parçalanacaktır ve aynı zamanda `IAsyncEnumerable<T>` ya da dönüş türü olarak `IAsyncEnumerator<T>` olmalıdır.  Örneğin, yukarıdaki örnek aşağıdaki şekilde zaman uyumsuz bir yineleyici olarak yazılabilir:

```csharp
static async IAsyncEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            await Task.Delay(1000);
            yield return i;
        }
    }
    finally
    {
        await Task.Delay(200);
        Console.WriteLine("finally");
    }
}
```

Dikkate alınan alternatifler:
- _İmzada `async` kullanmamakta_: `async` kullanmak, bu bağlamda `await` geçerli olup olmadığını anlamak için kullandığı için teknik olarak derleyici tarafından gerektirilir.  Ancak gerekli olmasa bile, `await` yalnızca `async`olarak işaretlenen yöntemlerde kullanılabileceğini ve tutarlılığın tutulması önemli bir durum olduğunu belirledik.
- _`IAsyncEnumerable<T>`için özel oluşturucular etkinleştiriliyor_ : Bu, geleceğe bakabiliriz ancak makineler karmaşıktır ve zaman uyumlu ortaklarınıza yönelik olarak desteklenmez.
- _İmzada bir `iterator` anahtar sözcüğüne sahip olma_: zaman uyumsuz yineleyiciler İmzada `async iterator` kullanır ve `yield` yalnızca `iterator`dahil `async` metotlarda kullanılabilir. `iterator`, zaman uyumlu yineleyiciler üzerinde isteğe bağlı olarak yapılır.  Bakış açısına bağlı olarak, `yield` izin verilmediği ve yöntemin, kodun `yield` kullanıp kullanmadığına bağlı olarak derleyici üretimi yerine `IAsyncEnumerable<T>` türündeki örnekleri döndürmesinin yanı sıra, yöntemin imzasına göre çok açık hale getirme avantajına sahiptir.  Ancak zaman uyumlu yineleyiciler farklıdır, bu, bir tane gerektirmez ve bu yapılamaz.  Ayrıca, bazı geliştiriciler ek söz dizimini beğenmez.  Bunu sıfırdan tasarladığımızda, bu gerekli olacaktır, ancak bu noktada, zaman uyumsuz yineleyiciler eşitleme yineleyiciler için kapalı tutulması çok daha fazla değer vardır.

## <a name="linq"></a>LINQ

`System.Linq.Enumerable` sınıfında, hepsi `IEnumerable<T>`açısından tüm çalışan, bu yöntemlerin on ~ 200 aşırı yüklemesi vardır. Bu kabul `IEnumerable<T>`, bazıları `IEnumerable<T>`ve her ikisi de birçok do.  `IAsyncEnumerable<T>` için LINQ desteğinin eklenmesi, başka bir ~ 200 için bu aşırı yüklemelerin tümünü çoğaltarak sıraya alabilir.  `IAsyncEnumerator<T>`, zaman uyumsuz dünyada `IEnumerator<T>`, zaman uyumlu dünyada bulunan tek başına bir varlık olarak daha yaygın olduğundan, `IAsyncEnumerator<T>`ile çalışan başka bir ~ 200 aşırı yüklemesi de gerekebilir.  Ayrıca, çok sayıda aşırı yükleme, koşullar (örneğin, `Func<T, bool>`alan `Where`) ile ilgilidir ve hem zaman uyumlu hem de zaman uyumsuz koşullara (örn. `Func<T, bool>`ek olarak `Func<T, ValueTask<bool>>`) dayalı `IAsyncEnumerable<T>`tabanlı aşırı yüklemeler olması istenebilir.  Bu, şu an ~ 400 yeni aşırı yükleme için geçerli olmasa da, kabaca bir hesaplama, bu da bir ~ 200 aşırı yüklemesi, yani toplam ~ 600 yeni yöntem için geçerli olacaktır.

Bu, kademelendirme sayıda API 'dir ve etkileşimli uzantılar (x) gibi uzantı kitaplıkları kabul edildiğinde daha da mümkün olur.  Ancak x, bunlardan birçoğu için zaten bir uygulama içeriyor ve bu çalışmayı yinelemek için harika bir neden görünmüyor; Bunun yerine, topluluğun x 'i iyileştirmesine yardımcı olur ve geliştiriciler `IAsyncEnumerable<T>`LINQ kullanmak istediğinizde BT için önerilir.

Sorgu anlama söz dizimi sorunu da vardır.  Sorgu anlama 'nın model tabanlı doğası, bazı işleçlerle "yalnızca çalışma" yapmasına izin verir, örneğin, x aşağıdaki yöntemleri sağlar:

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> func);
public static IAsyncEnumerable<T> Where(this IAsyncEnumerable<T> source, Func<T, bool> func);
```

Bu C# kod, "yalnızca çalışır" olacaktır:

```csharp
IAsyncEnumerable<int> enumerable = ...;
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select item * 2;
```

Ancak, yan tümcelerinde `await` kullanımını destekleyen sorgu kavrama sözdizimi yoktur, yani x eklenirse, örneğin:

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, ValueTask<TResult>> func);
```

Bu durumda "yalnızca çalışma" olur:

```csharp
IAsyncEnumerable<string> result = from url in urls
                                  where item % 2 == 0
                                  select SomeAsyncMethod(item);

async ValueTask<int> SomeAsyncMethod(int item)
{
    await Task.Yield();
    return item * 2;
}
```

Ancak, `select` yan tümcesinde `await` satır içi olarak yazmak için bir yol yoktur.  Ayrı bir çaba olarak, dile `async { ... }` ifadeleri ekleme hakkında görünebiliriz. bu noktada, sorgu anlama 'da kullanılmasına izin verdiğimiz ve yukarıdaki şöyle yazılabilir:

```csharp
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select async
                               {
                                   await Task.Yield();
                                   return item * 2;
                               };
```

ya da `async from`destekleme gibi `await` doğrudan ifadelerde kullanılmasına olanak tanır.  Bununla birlikte, burada bir tasarımın bir yolu veya diğerini, bu özellik kümesinin geri kalanını etkiler ve bu, özellikle de bir çok yüksek değerli şey olmak üzere hemen yatırım yapacak bir şey değildir.

## <a name="integration-with-other-asynchronous-frameworks"></a>Diğer zaman uyumsuz çerçevelerle tümleştirme

`IObservable<T>` ve diğer zaman uyumsuz çerçeveler (örneğin, reaktif akışlar) ile tümleştirme, dil düzeyi yerine kitaplık düzeyinde yapılır.  Örneğin, bir `IAsyncEnumerator<T>` tüm veriler `IObserver<T>` bir `await foreach`' de Numaralandırıcının üzerinde yer alan ve verileri gözlemci 'e `OnNext`bir `AsObservable<T>` bir genişletme yöntemi olabilerek yayımlanabilir.  Bir `await foreach` `IObservable<T>` kullanmak için verilerin arabelleğe alınması gerekir (önceki öğe işlenmeye devam ederken başka bir öğenin itilmesi durumunda), ancak bir `IObservable<T>` `IAsyncEnumerator<T>`ile çekmesini sağlamak için bu tür bir çekme bağdaştırıcısı kolayca uygulanabilir.  Benzerlerini.  RX/x, bu tür uygulamaların prototiplerini zaten sağlıyor ve https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels gibi kitaplıklar çeşitli türlerde arabelleğe alma veri yapıları sunmaktadır.  Bu aşamada dilin dahil olmaması gerekir.
