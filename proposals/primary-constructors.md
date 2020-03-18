---
ms.openlocfilehash: 77c9ffda3a59cc5f3dcc3ec9848600c6c5e03eed
ms.sourcegitcommit: 27487fa0294f4cdb7e5f2478884149e05314fd8a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "79484974"
---
# <a name="primary-constructors"></a>Birincil oluşturucular

* [x] önerilir
* [] Prototipi: başlatılmadı
* [] Uygulama: başlatılmadı
* [] Belirtimi: başlatılmadı

## <a name="summary"></a>Özet
[summary]: #summary

Sınıfların bir parametre listesi olabilir ve bunlar ne zaman bir bağımsız değişken listesine sahip olabilir.
Birincil Oluşturucu parametreleri sınıf bildiriminin tamamında kapsamdadır ve bir işlev üyesi veya anonim işlev tarafından yakalanırsa, bunlar sınıfında özel alanlar olarak depolanır.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Program başlatma kodunda çok fazla ortak olması yaygındır. Genel durumda, belirli bir veri `x` parçası birçok kez bahsedilir:

- Özel bir alan `_x`
- Bir oluşturucuya `x` parametresi olarak
- Oluşturucunun parametresindeki alanın atama `_x = x;`
- Özellik olarak `X`
- Özellik ayarlayıcısı 'nda `x = value;`
- Özellik alıcısı `return x;`

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

Doğrulama veya hesaplama gerektirmeyen özellikler için, kaynaklanan çoğunu otomatik özellikler kullanılarak azaltılabilir, bu nedenle özellik için açık bir yedekleme alanı bildirme gereksinimini ortadan kaldırabilirsiniz. Ancak özellik, bir otomatik özelliğin sağladığı bir mantık sıralaması gerektiriyorsa, yukarıdaki en iyi yöntem sizin için en iyi seçenektir.

Bunun yerine birincil oluşturucular, Oluşturucu bağımsız değişkenlerini sınıfın tamamında doğrudan kapsama yerleştirerek yük yükünü azaltır, bir yedekleme alanını açıkça bildirme gereksinimini gereksinimini gidererek. Bu nedenle yukarıdaki örnek şöyle olacaktır:

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

Bu örnekte, birincil Oluşturucu `x` için adlandırılmış varlıkların sayısını üç ile ikiye azaltır, `_x` gereksinimini gidererek. Üç üye bildirimini kaldırır (yalnızca özellik bildiriminin kendisini tutarak) ve `x`/`_x`/`X` sekizlisi sayısını sekiz ile beş arasında azaltır.


## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

Sınıfların bir parametre listesi olabilir:

``` c#
public class C(int i, string s)
{
    ...
}
```

Parametre listesi, bir oluşturucunun sınıf için örtülü olarak bildirilmesine neden olur ve bu, sınıfın kendisiyle aynı erişilebilirliğe sahiptir.

``` c#
new C(5, "Hello");
```

Birincil Oluşturucu parametreleri sınıf gövdesinin tamamında kapsamdadır. Bunlar bir işlev üyesi veya anonim işlev tarafından yakalanırsa, sınıfında özel alanlar olarak depolanır. Yalnızca başlatma sırasında kullanılıyorsa, bu nesneler nesnede depolanmaz.

``` c#
public class C(int i, string s)
{
    int[] a = new int[i]; // i not captured
    public int S => s;    // s captured
}
```

Birincil oluşturucuya sahip bir sınıfın temel sınıf belirtimi varsa, bu bir bağımsız değişken listesine sahip olabilir. Bu, örtük olarak belirtilen oluşturucunun `base(...)` başlatıcısı için bağımsız değişken listesi görevi görür. Bağımsız değişken listesi sağlanmazsa boş bir değer kabul edilir.

``` c#
public class C(int i, string s) : B(s)
{
    ...
}
```
Sınıfı açıkça tanımlanmış oluşturucuları de içerebilir, ancak tümünün bir `this(...)` başlatıcısı kullanması gerekir. Bu, yeni bir örnek oluşturulduğunda birincil oluşturucunun her zaman çağrılabilir olmasını sağlar.

Sınıf gövdesindeki tüm başlatıcılar, oluşturulan oluşturucuda atamalar olur. Yani, diğer sınıfların aksine, başlatıcıların temel Oluşturucu *çağrıldıktan sonra* , önce değil, çalışır. Ayrıca, oluşturulan sınıf, Üyeler tarafından yakalanan birincil Oluşturucu parametrelerini depolamak için oluşturulan özel alanları başlatmak üzere Atamalar içerir. Bu Üyeler, lambda ifadelerinin kapanışlarını ifade eden bir şekilde parametresi yerine özel alanı kullanmak için yeniden yazılır. Oluşturulan birincil alanlar önce başlatılır, sonra başlatıcı tarafından oluşturulan atamalar, sınıfında görünüm sırasına göre yürütülür.

Yukarıdaki örnekte, sınıf bildiriminin etkisi şöyle olur:

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

Yakalama, yerel değişkenlerin lambda ifadelerine yakalamaya benzer kısıtlamalara sahiptir. Örneğin, `ref` ve `out` parametrelere birincil oluşturucularda izin verilir, ancak üye gövdelerim yakalanamaz.


## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Kabaca anlamlı bir sıra.

* Teklif, konumsal kayıtlar için de önerilen söz dizimini kullanır. Her iki özelliği de sağladığımızda, bazı konaklama gerekir. Örneğin kayıtlar üzerinde `data` değiştiricisi önerildi.
* Oluşturulan nesnelerin ayırma boyutu daha az açıktır, derleyici, sınıfın tam metnine göre birincil Oluşturucu parametresi için bir alan ayırmayı belirler. Bu risk, değişkenlerin lambda ifadelerine örtük olarak yakalamasına benzer.
* Ortak bir dürtüsüne (veya yanlışlıkla bir model), Oluşturucu zincirinin, temel sınıfta korunan bir alanı açıkça bir şekilde ayrılmasını yerine, yinelenen ayırmalara nesnelerdeki aynı veriler için. Bu, otomatik özelliklerle otomatik özellikleri geçersiz kılma riskine bugün benzer. 
* Yukarıda önerildiği gibi, genellikle Oluşturucu gövdelerinde belirtilebilen ek mantık için bir yer yoktur. Aşağıdaki "birincil Oluşturucu gövdeleri" uzantısı bu şekilde ele alınmaktadır.
* Önerildiği gibi, yürütme sırası semantiği sıradan oluşturucularla çok daha farklıdır. Bunun yapılması, bazı uzantı tekliflerinin (özellikle "birincil Oluşturucu gövdelerinin") maliyetinde gerçekleştirilebilir.
* Teklif yalnızca tek bir oluşturucunun birincil olarak belirlenmesi durumunda işe yarar.
* Sınıfının ve birincil oluşturucunun ayrı erişilebilirliğine sahip olmanın bir yolu yoktur. Örneğin, herkese açık bir özel "Build-It-All" oluşturucusuna temsilci seçmek için, bu durumda. Gerekirse, sözdizimi daha sonra önerilir.


## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

Tam konumsal konumsal kayıtlar alternatif olabilir veya özellikleri temel alınarak birincil oluşturucularla birlikte olabilir. Daha *az* sayıda senaryoda *daha fazla* kısaltmaya izin verir. Bu nedenle her ikisi de yararlı olur, ancak her ikisine de tek bir şekilde tümleştirilebilir.


## <a name="possible-extensions"></a>Olası uzantılar
[extensions]: #possible-extensions

Bunlar, temel teklife, onunla birlikte düşünüldüler veya eklemeler ya da kabul edilen yararlı olursa daha sonraki bir aşamada bulunabilir.

### <a name="primary-constructor-bodies"></a>Birincil Oluşturucu gövdeleri

Oluşturucular genellikle parametre doğrulama mantığını veya Başlatıcı olarak ifade edilebilen diğer önemsiz başlatma kodlarını içerir.

Birincil oluşturucular, ifade bloklarının doğrudan sınıf gövdesinde görünmesine izin vermek için genişletilebilir. Bu deyimler, başlatma atamaları arasında göründükleri noktada oluşturulan oluşturucuya eklenirler ve bu nedenle başlatıcılarla birlikte yürütülür. Örneğin:

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

### <a name="initializer-fields-and-initializer-functions"></a>Başlatıcı alanları ve Başlatıcı işlevleri

Birincil Oluşturucusu olan bir sınıfta, erişilebilirlik değiştiricileri olmadan alan ve Yöntem bildirimlerini, yerel değişkenler ve yerel işlevler gibi bir şekilde düşünebiliriz:

* Birincil Oluşturucu parametreleri gibi, "başlatıcı alanları" yalnızca işlev üyelerinde kullanılıyorsa gerçek bir özel alana yakalanırlar.
* "Başlatıcı işlevleri" yalnızca kendi diğer işlev üyelerinde kullanılsaydı birincil Oluşturucu parametrelerini ve Başlatıcı alanlarını yakalamak için değerlendirilir. Yakalanmadıysa, yerel işlevler gibi daha iyi bir şekilde oluşturulabilir.
* Yalnızca birincil Oluşturucu parametreleri gibi, yalnızca basit bir ad olarak üye erişimi aracılığıyla kullanılamaz.

Bu, yalnızca başlatma ile ilgili geçici değişkenler ve yardımcı işlevler için kullanılabilir:

``` c#
public class C(string s)
{
    int size = s.Length;             // not captured
    int[] Create() => new int[size]; // not captured
    int[] a = Create();
    ...
}
```

Bu, özellikle de erişilebilirlik değiştiricilerin yokluğu `private`anlamı olduğundan bu çok hafif olabilir. 

### <a name="initializer-statements"></a>Başlatıcı deyimleri

Yukarıdaki uzantıların kök birleşimi yalnızca sınıf gövdesinde doğrudan deyimlere izin vermek olacaktır. Bu tür deyimler, yukarıda önerilen ve `{ }`içine alınmaları dışında, yukarıda önerilen Oluşturucu gövdeleri olarak sunulur. Bunun yeterince yararlı olması için, "yerel" değişkenlerin ve yardımcı işlevlerin, yukarıdaki "başlatıcı alanları ve Başlatıcı işlevleri" uzantısında araştırılan sınıfının en üst düzeyinde ifade edilebilir olması gerekir.


### <a name="member-access"></a>Üye erişimi

Çekirdek teklif, birincil Oluşturucu parametrelerini yalnızca basit adlar olarak başvurulabilen parametreler olarak değerlendirir. Diğer bir seçenek de, bazen alan olarak *oluşturulmasa bile* , bunlara üye erişimle, yani özel alanlar gibi başvurulmasını sağlar. Bu, yerel değişkenlerle gölgelendirildikleri zaman `this.x` olarak başvurulmasını ve farklı bir örnekten `other.x`olarak erişilmesini sağlar.

"Başlatıcı alanları ve Başlatıcı işlevleri" uzantısı için uygulanmışsa, bu durum sıradan özel üyelerden farklı olan dereceyi de azaltır. Tek fark daha sonra, yalnızca başlatma sırasında kullanılıyorsa derleyicinin nesne içinden bir bütün olarak bu nesneleri sorunsuz bir şekilde boşaltmasını sağlar.

