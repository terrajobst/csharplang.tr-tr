---
ms.openlocfilehash: 922353d043653ddb651534a01f3fb98f85cd756e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484484"
---
# <a name="readonly-locals-and-parameters"></a>salt okunur Yereller ve parametreler

* [x] önerilir
* [] Prototipi
* [] Uygulama
* [] Belirtimi

## <a name="summary"></a>Özet
[summary]: #summary

Bu Yereller ve parametrelerin yüzeysel bir şekilde bırakılmasını engellemek için Yereller ve parametrelerin ReadOnly olarak açıklanmasını sağlar.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Bugün, `readonly` anahtar sözcüğü alanlara uygulanabilir; Bu, bir alanın yalnızca oluşturma sırasında (statik bir alan olması durumunda statik oluşturma veya bir örnek alanı durumunda örnek oluşturma) yazılmasına olanak sağlayan, aksi durumda, yanlışlıkla değiştirilmemesi gereken durum üzerine yazarak, geliştiricilerin hata yaşamamasını sağlamaya yönelik etkiye sahiptir. Ancak alanlar, geliştiricilerin değer değiştirici olmamasını sağlamak istedikleri tek yer değildir. Özellikle, geçici durumu depolamak için yerel bir değişken oluşturmak yaygındır ve yanlışlıkla bu geçici durumu güncellemek, özellikle de bu tür "Yereller" lambda içinde yakalandıklarında, bu tür yükseltilmemiş alanlarını `readonly`olarak İşaretlemede bugün bir yol yoktur.

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

Yalnızca bildirim sırasında ayarlandıklarından (örneğin, bir ' foreach ' döngüsünde yineleme değişkeni veya ' Using ' bloğunda kullanılan değişken), ancak şu anda bir geliştiricinin C# diğer yerelleri `readonly`olarak işaretleyebilme özelliği olan ' de, yerel öğeler `readonly` olarak Annotatable olacaktır. Bu `readonly` Yereller bir başlatıcıya sahip olmalıdır:

```csharp
readonly long maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

`readonly var`için de toplu olarak, var olan bağlamsal anahtar sözcük `let` kullanılabilir, örn.

```csharp
let maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

Başlatıcıya ait olabilecek özel kısıtlamalar yoktur ve şu anda Yereller için bir başlatıcı olarak geçerli olan herhangi bir şey olabilir. Örneğin,

```csharp
readonly T data = arg1 ?? arg2;
```

Yereller üzerindeki `readonly`, Lambdalar ve kapanışlarla çalışırken özellikle değerlidir. Anonim bir yöntem veya lambda, kapsayan kapsamdan yerel duruma eriştiğinde, bu durum derleyici tarafından bir "görüntü sınıfı" ile temsil edilen bir kapatılmak üzere yakalanır.  Yakalanan her bir "yerel" Bu sınıftaki bir alandır, ancak derleyici sizin adınıza bu alanı oluşturduğundan, lambda 'nin "yerel" e (aslında yerel değil, sonuçta elde edilen MSIL 'de değil, gerçekten bir yerel olmadığı için) yanlışlıkla yazılmasını engellemek için `readonly` olarak not ekleme şansınız yoktur. `readonly` Yereller sayesinde, derleyici, lambda 'nin yerel konuma yazmasını önleyebilir, bu da özellikle de hatalı bir yazma tehlikeli, ancak nadir ve çok fazla eşzamanlılık hatasına neden olabilecek çoklu iş parçacığı oluşturma senaryolarıyla ilgili senaryolarda değerlidir.

```csharp
readonly long index = ...;
Parallel.ForEach(data, item => {
    T element = item[index];
    index = 0; // Error: can't assign to readonly locals outside of declaration
});
```

Özel bir yerel form olarak parametreler de `readonly`olarak Annotatable olacaktır. Bu, yöntemin çağıranın parametreye geçebileceği herhangi bir etkiye sahip değildir (yalnızca bir `readonly` alanında depolanabilecek değerler için hiçbir kısıtlama olmadığı gibi), derleyici, koddan sonra kodun `readonly` bir parametreye yazmasını yasakladığından, yöntemin gövdesi parametreye yazılmak anlamına gelir, bu da yöntemin gövdesinin parametreye yazılması yasaktır.

```csharp
public void Update(readonly int index = 0) // Default values are ok though not required
{
    ...
    index = 0; // Error: can't assign to readonly parameters
    ...
}
```

`readonly` parametreler, bu yöntem için derleyici tarafından yayılan imza/meta verileri etkilemez ve derleyicinin metodun gövdesinin derlemesini nasıl işleyeceğini etkiler. Bu nedenle, örneğin, bir taban sanal yönteminin bir `readonly` parametresi olabilir ve bu parametre bir geçersiz kılma içinde yazılabilir.

Alanlarda olduğu gibi, Yereller ve parametreler için `readonly` basit bir deyişle, depolama konumunu etkileyen, ancak nesne grafiğini etkilemeden geçişli bir değer kalmaz. Bununla birlikte, Ayrıca, bir `readonly` yerel/parametre yapısı üzerinde bir yöntemi çağırmak, aslında yapının bir kopyasını yapar ve `this`iç muatmasını önlemek için kopyada yöntemi çağırır.

`readonly` Yereller ve parametreler, `ref readonly` desteklenmediği sürece `ref` veya `out` bağımsız değişken olarak geçirilebilir.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

- `val` `let`için alternatif bir toplu değer olarak kullanılabilir.

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

- `readonly ref` / `ref readonly` / `readonly ref readonly`: Bu tekliften ayrı olarak `ref readonly` nasıl işleyeceğinizi gösteren sorudan ayrıldım.
- Bu teklif, salt okunur yapılar/Sabit türler oluşturmaz. Bu, ayrı bir teklif için bırakılır.

## <a name="design-meetings"></a>Tasarım toplantıları

- 21 Ocak 2015 ' de kısaca ele alınmaktadır (<https://github.com/dotnet/roslyn/issues/98>)
