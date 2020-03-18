---
ms.openlocfilehash: 39fb0aab5e8bb0d422f25fd2e92ab3d8256d3f59
ms.sourcegitcommit: b8f1103eb686c5d82e294837c9386d9b667da292
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2019
ms.locfileid: "79485114"
---
# <a name="readonly-references"></a>Salt okunur başvurular

* [x] önerilir
* [x] prototipi
* [x] uygulama: başlatıldı
* [] Belirtimi: başlatılmadı

## <a name="summary"></a>Özet
[summary]: #summary

"Salt okunur başvurular" özelliği aslında değişkenleri başvuruya göre geçirme ve değişiklikler için verileri açığa çıkarmadan faydalanma verimliliğini kullanan bir özellik grubudur:  
- `in` parametreleri
- `ref readonly` döndürür
- `readonly` yapılar
- `ref`/`in` uzantısı yöntemleri
- `ref readonly` Yereller
- Koşullu ifadeleri `ref`

## <a name="passing-arguments-as-readonly-references"></a>Bağımsız değişkenleri ReadOnly başvuruları olarak geçirme.

Bu konu başlığı altında, çok sayıda ayrıntıya geçmeden salt okunur parametrelere özel bir durum olarak https://github.com/dotnet/roslyn/issues/115 dokunan mevcut bir teklif vardır.
Burada yalnızca kendi fikrinin çok yeni olmayacağını bildirmek istiyorum.

### <a name="motivation"></a>Amacı

Bu özellikten C# önce, hiçbir değişiklik yapılmadan, salt okunur amaçlar için yapı değişkenlerini metot çağrılarına geçirmek zorunda bir ifade etmenin etkili bir yolu yoktu. Normal değere sahip bağımsız değişken geçirme, gereksiz maliyetler ekleyen kopyalamayı gerektirir.  Bu, kullanıcıların-ref bağımsız değişkenini kullanmasını ve verilerin aranan tarafından atlanması gerektiğini belirtmek için açıklamaları/belgeleri geçen ve kullanıcılara yönlendiren bir belge kullanır. Birçok nedenden dolayı iyi bir çözüm değildir.  
Örnek olarak, performans konuları nedeniyle, [XNA](https://msdn.microsoft.com/library/bb194944.aspx) gibi grafik kitaplıklarında çok sayıda vektör/matris matematik işleçleri başvuru işlenenleri olarak bilinir. Roslyn derleyicisinde, ayırmaların önüne geçmek için yapılar kullanan ve sonra maliyetleri kopyalamayı önlemek için bunları başvuruya göre ileten kod vardır.

### <a name="solution-in-parameters"></a>Çözüm (`in` Parameters)

`out` parametrelerine benzer şekilde, `in` parametreler, çağrılan ek garantilere sahip yönetilen başvurular olarak geçirilir.  
Diğer herhangi bir kullanılmadan önce çağrılan tarafından atanması _gereken_ `out` parametrelerinden farklı olarak, `in` parametreleri, aranan tarafından atanamaz.

Sonuç olarak `in` parametreler, çağrılan dolaylı bağımsız değişken, çağıran tarafından barbeklere bağımsız değişkenler ortaya çıkarmadan geçen verimlilik için izin verir.

### <a name="declaring-in-parameters"></a>`in` parametrelerini bildirme

`in` parametreler, `in` anahtar sözcüğü parametre imzasında bir değiştirici olarak kullanılarak bildirilmiştir.

Tüm amaçlar için `in` parametresi bir `readonly` değişken olarak değerlendirilir. Yöntemi içinde `in` parametrelerinin kullanımı ile ilgili kısıtlamaların çoğu `readonly` alanlarla aynıdır.

> Gerçekten bir `in` parametresi bir `readonly` alanı temsil edebilir. Kısıtlamaların benzerliği bir rastlantı değildir.

Örneğin, bir yapı türüne sahip bir `in` parametresinin alanları, `readonly` değişkenleri olarak özyinelemeli olarak sınıflandırıldı.

```csharp
static Vector3 Add (in Vector3 v1, in Vector3 v2)
{
    // not OK!!
    v1 = default(Vector3);

    // not OK!!
    v1.X = 0;

    // not OK!!
    foo(ref v1.X);

    // OK
    return new Vector3(v1.X + v2.X, v1.Y + v2.Y, v1.Z + v2.Z);
}
```

- `in` parametrelere, normal ByVal parametrelerine izin verilen her yerde izin verilir. Bu, Dizin oluşturucular, işleçler (dönüşümler dahil), temsilciler, Lambdalar, yerel işlevler içerir.

> ```csharp
>  (in int x) => x                                                     // lambda expression  
>  TValue this[in TKey index];                                         // indexer
>  public static Vector3 operator +(in Vector3 x, in Vector3 y) => ... // operator
>  ```

- `in` `out` veya `out` birlikte birleştirmediğinden hiçbir şeyle birlikte kullanılamaz.

- `in` farklılıkları /`out``ref`/aşırı yüklenmesine izin verilmez.

- Normal ByVal üzerinde aşırı yükleme ve `in` farklılıklarına izin verilir.

- OHI (aşırı yükleme, gizleme, uygulama) amacı için `in` `out` parametreye benzer şekilde davranır.
Aynı kuralların hepsi geçerlidir.
Örneğin, geçersiz kılma yöntemi, kimlik dönüştürülebilir bir türün `in` parametreleriyle `in` parametrelerle eşleşmelidir.

- Temsilci/Lambda/Yöntem grubu dönüştürmelerinde `in`, bir `out` parametresine benzer şekilde davranır.
Lambdalar ve geçerli yöntem grubu dönüştürme adayları, bir kimlik dönüştürülebilir türün `in` parametreleriyle hedef temsilcinin `in` parametreleriyle eşleşmelidir.

- Genel varyans amacına yönelik `in` parametreler değişken değildir.

> NOTE: başvuru veya temel türler içeren `in` parametrelerinde uyarı yok.
Genel olarak daha az olabilir, ancak bazı durumlarda, kullanıcıların temelleri `in`olarak geçirmek gerekir. Örnekler-`T` `int`ya da `Volatile.Read(in int location)` gibi yöntemlere sahip olduğunda `Method(in T param)` gibi genel bir yöntemi geçersiz kılma.
>
> `in` parametrelerinin verimsiz kullanımı konusunda uyaran bir çözümleyici Conceivable, ancak bu tür analizler için kurallar bir dil belirtiminin parçası olmak için çok belirsiz olacaktır.

### <a name="use-of-in-at-call-sites-in-arguments"></a>Çağıran sitelerde `in` kullanımı. (`in` bağımsız değişkenler)

Bağımsız değişkenleri `in` parametrelere geçirmek için iki yol vardır.

#### <a name="in-arguments-can-match-in-parameters"></a>`in` bağımsız değişkenler `in` parametreleriyle eşleştirebilir:

Çağrı sitesinde `in` değiştiricisi olan bir bağımsız değişken `in` parametreleriyle eşleştirebilir.

```csharp
int x = 1;

void M1<T>(in T x)
{
  // . . .
}

var x = M1(in x);  // in argument to a method

class D
{
    public string this[in Guid index];
}

D dictionary = . . . ;
var y = dictionary[in Guid.Empty]; // in argument to an indexer
```

- `in` bağımsız değişkeni _okunabilir_ bir lvalue (*) olmalıdır.
Örnek: `M1(in 42)` geçersiz

> (*) [Lvalue/rvalue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) kavramı diller arasında farklılık gösterir.  
Burada, LValue tarafından doğrudan başvurulabilen bir konumu temsil eden bir ifade geliyor.
Ve RValue, kendi kendine kalıcı olmayan geçici bir sonuç veren bir ifade anlamına gelir.  

- Özellikle `readonly` alanları, `in` parametreleri veya diğer resmi `readonly` değişkenlerini `in` bağımsız değişken olarak geçirmek için geçerlidir.
Örnek: `dictionary[in Guid.Empty]` geçerlidir. `Guid.Empty` statik salt okunur bir alandır.

- `in` bağımsız değişkeni parametre türüne _dönüştürülebilir_ olmalıdır.
Örnek: `M1<object>(in Guid.Empty)` geçersiz. `Guid.Empty`, `object` için _kimlik dönüştürülebilir_ değil

Yukarıdaki kurallara ilişkin mosyon, bağımsız değişkenlerin bağımsız değişken değişkeninin _diğer_ adını garanti `in`. Aranan her zaman bağımsız değişkenle temsil edilen konuma bir doğrudan başvuru alır.

- `in` bağımsız değişkenlerin yığın olarak, aynı çağrının işlenenleri olarak kullanılan `await` ifadeler nedeniyle yığın olarak bir arada olması gerektiğinde, bu davranış `out` ve `ref` bağımsız değişkenlerle aynı olur. değişken, zaman uyumlu olarak saydam olarak şeffaf bir şekilde bir hata bildirilir.

Örnekler:
1. `M1(in staticField, await SomethingAsync())` geçerli.
`staticField`, observable yan etkileri olmadan birden çok kez erişilebilen statik bir alandır. Bu nedenle, yan etkileri ve diğer ad gereksinimlerinin sırası belirtilebilir.
2. `M1(in RefReturningMethod(), await SomethingAsync())` bir hata oluşturacak.
`RefReturningMethod()`, `ref` döndüren bir yöntemdir. Bir yöntem çağrısında observable yan etkileri olabilir, bu nedenle `SomethingAsync()` işleneni önce değerlendirilmelidir. Ancak, çağrının sonucu, doğrudan başvuru gereksinimini olanaksız hale getirecek `await` askıya alma noktası genelinde korunmayan bir başvurudur.

> Not: yığın atımı hatası, uygulamaya özgü sınırlamalar olarak kabul edilir. Bu nedenle, aşırı yükleme çözünürlüğü veya lambda çıkarımı üzerinde hiçbir etkisi yoktur.

#### <a name="ordinary-byval-arguments-can-match-in-parameters"></a>Sıradan ByVal bağımsız değişkenleri `in` parametreleriyle eşleştirebilir:

Değiştiriciler olmadan normal bağımsız değişkenler `in` parametreleriyle eşleşemez. Bu tür durumlarda bağımsız değişkenler, sıradan bir ByVal bağımsız değişkenleri ile aynı gevşek kısıtlamalara sahiptir.

Bu senaryo için mosyon, bağımsız değişkenler doğrudan başvuru-EX olarak geçirilmezse, API 'lerdeki `in` parametrelerinin Kullanıcı için nedeniyle ile sonuçlanmasına neden olabilir: değişmez değerler, hesaplanan veya `await`Ed sonuçlar veya daha belirli türlere sahip olan bağımsız değişkenler.  
Tüm bu durumlarda, bağımsız değişken değerini uygun bir yerel türde depolamanın ve bu yerel değeri bir `in` bağımsız değişkeni olarak geçirerek çok basit bir çözüm vardır.  
Bu tür bir ortak kod derleyicisi gereksinimini azaltmak için, arama sitesinde `in` değiştiricisi yoksa, gerekirse aynı dönüştürmeyi gerçekleştirebilir.  

Ayrıca, işleçler veya `in` genişletme yöntemlerinin çağrılması gibi bazı durumlarda `in` belirtmenin herhangi bir sözdizimi yoktur. Tek başına, `in` parametreleriyle eşleştiklerinde sıradan ByVal bağımsız değişkenlerinin davranışının belirtilmesini gerektirir.

Özellikle:

- RValues geçişi için geçerlidir.
Bu tür bir durumda geçici bir başvuru geçirilir.
Örnek:
```csharp
Print("hello");      // not an error.

void Print<T>(in T x)
{
  //. . .
}
```

- örtük Dönüştürmelere izin verilir.

> Bu aslında RValue geçirme özel bir durumdur  

Bu tür bir durumda, geçici olarak bir dönüştürülmüş değer tutan bir başvuru geçirilir.
Örnek:
```csharp
Print<int>(Short.MaxValue)     // not an error.
```

- `in` uzantısı yönteminin alıcısında (`ref` uzantısı yöntemlerinin aksine), RValues veya örtük _Bu bağımsız değişken dönüştürmelerine_ izin verilir.
Bu tür bir durumda, geçici olarak bir dönüştürülmüş değer tutan bir başvuru geçirilir.
Örnek:
```csharp
public static IEnumerable<T> Concat<T>(in this (IEnumerable<T>, IEnumerable<T>) arg)  => . . .;

("aa", "bb").Concat<char>()    // not an error.
```
`ref`/`in` uzantı yöntemleri hakkında daha fazla bilgi bu belgede daha fazla sunulmaktadır.

- bağımsız değişken, `await` işlenenleri nedeniyle, gerekirse "değere göre" taşarak taşma olabilir.
Bir bağımsız değişkene doğrudan başvuru sağlamak `await` için bağımsız değişken olarak sağlanan senaryolarda, bağımsız değişkenin değerinin bir kopyası bunun yerine taşılır.  
Örnek:
```csharp
M1(RefReturningMethod(), await SomethingAsync())   // not an error.
```
Yan etkili bir çağrının sonucu `await` askıya alma boyunca korunmayan bir başvuru olduğundan, gerçek değeri içeren geçici bir olay bunun yerine korunur (normal bir ByVal parametre durumunda olduğu gibi).

#### <a name="omitted-optional-arguments"></a>Atlanan isteğe bağlı bağımsız değişkenler

Bir `in` parametresi için varsayılan bir değer belirtmek için izin verilir. Bu, karşılık gelen bağımsız değişkeni isteğe bağlı hale getirir.

Çağrı sitesinde isteğe bağlı bağımsız değişkeni atlama, varsayılan değeri geçici olarak geçirme ile sonuçlanır.

```csharp
Print("hello");      // not an error, same as
Print("hello", c: Color.Black);

void Print(string s, in Color c = Color.Black)
{
    // . . .
}
```

### <a name="aliasing-behavior-in-general"></a>Genel olarak diğer ad davranışı

`ref` ve `out` değişkenleri gibi `in` değişkenler, var olan konumların başvuruları/diğer adları.

Çağrılan tarafından bunlara yazma izni verilmediğinden, bir `in` parametresi okumak diğer değerlendirmelere yan bir etkisi olarak farklı değerleri gözlemleyebilirsiniz.

Örnek:

```C#
static Vector3 v = Vector3.UnitY;

static void Main()
{
    Test(v);
}

static void Test(in Vector3 v1)
{
    Debug.Assert(v1 == Vector3.UnitY);
    // changes v1 deterministically (no races required)
    ChangeV();
    Debug.Assert(v1 == Vector3.UnitX);
}

static void ChangeV()
{
    v = Vector3.UnitX;
}
```

### <a name="in-parameters-and-capturing-of-local-variables"></a>parametreleri `in` ve yerel değişkenlerin yakalanması.  
Lambda/zaman uyumsuz yakalama `in` parametrelerinin amacı, `out` ve `ref` parametreleriyle aynı şekilde davranır.

- `in` parametreler bir kapanışda yakalanamaz
- Yineleyici metotlarda `in` parametrelere izin verilmiyor
- zaman uyumsuz yöntemlerde `in` parametrelere izin verilmez

### <a name="temporary-variables"></a>Geçici değişkenler.  
`in` parametresi geçirmenin bazı kullanımları, geçici bir yerel değişkenin dolaylı kullanımını gerektirebilir:  
- `in` bağımsız değişkenler her zaman, Call-site `in`kullandığında doğrudan diğer adlar olarak geçirilir. Geçici, böyle bir durumda hiçbir şekilde kullanılmaz.
- çağrı sitesi `in`kullanmıyorsa `in` bağımsız değişkenlerin doğrudan takma adlar olması gerekmez. Bağımsız değişken bir LValue olmadığında geçici bir şekilde kullanılabilir.
- `in` parametre varsayılan değere sahip olabilir. Çağrı sitesinde karşılık gelen bağımsız değişken atlandığında, varsayılan değer geçici olarak geçirilir.
- `in` bağımsız değişkenler, kimliği korumayan bulunanlar da dahil olmak üzere örtük Dönüştürmelere sahip olabilir. Geçici olarak bu durumlarda kullanılır.
- sıradan yapı çağrılarının alıcıları yazılabilir LValues (**mevcut durum!** ) olamaz. Geçici olarak bu durumlarda kullanılır.

Geçiciler bağımsız değişkeninin yaşam süresi, Call-site ' ın en yakın çevreleme kapsamıyla eşleşir.

Geçici değişkenlerin biçimsel yaşam süresi, başvuruya göre döndürülen değişkenlerin kaçış analizini içeren senaryolarda anlam açısından önemlidir.

### <a name="metadata-representation-of-in-parameters"></a>`in` parametrelerinin meta veri temsili.
`System.Runtime.CompilerServices.IsReadOnlyAttribute` bir ByRef parametresine uygulandığında, parametrenin bir `in` parametresi olduğu anlamına gelir.

Ayrıca, yöntem *soyut* veya *sanal*ise, bu parametre (ve yalnızca bu parametreler) imzasının `modreq[System.Runtime.InteropServices.InAttribute]`olması gerekir.

**Mosyon**: bu, `in` parametrelerini geçersiz kılan/uygulayan bir yöntem durumunda olduğundan emin olmak için yapılır.

Temsilcilerle `Invoke` yöntemler için de aynı gereksinimler geçerlidir.

**Mosyon**: Bu, mevcut derleyicilerin temsilcileri oluştururken veya atarken `readonly` yoksaymasını sağlamaktır.

## <a name="returning-by-readonly-reference"></a>ReadOnly başvurusuyla döndürülüyor.

### <a name="motivation"></a>Amacı
Bu alt özellik için mosyon, `in` parametrelerinin nedenleri için kabaca simetrik olarak yapılır, ancak döndürülen tarafta, kopyalamayı önler. Bu özellikten önce, bir yöntem veya dizin oluşturucunun iki seçeneği vardır: 1) başvuruya göre geri dönün ve olası mutasyonların veya 2), kopyalama ile sonuçlanan değere göre döndürülür.

### <a name="solution-ref-readonly-returns"></a>Çözüm (`ref readonly` döndürür)  
Özelliği, bir üyenin değişkenleri bir başvuruya göre geri almasına izin verir.

### <a name="declaring-ref-readonly-returning-members"></a>`ref readonly` döndüren Üyeler bildirme

Dönüş imzasında değiştiriciler `ref readonly` birleşimi, üyenin salt okunur bir başvuru döndürdüğünü göstermek için kullanılır.

Tüm amaçlar için bir `ref readonly` üye `readonly` alanlara ve `in` parametrelere benzer bir `readonly` değişken olarak değerlendirilir.

Örneğin, yapı türüne sahip `ref readonly` üyesinin alanları, `readonly` değişkenleri olarak özyinelemeli olarak sınıflandırıldı. -`ref` veya `out` bağımsız değişken olarak değil, bunları `in` bağımsız değişken olarak geçirmeye izin verilir.

```csharp
ref readonly Guid Method1()
{
}

Method2(in Method1()); // valid. Can pass as `in` argument.

Method3(ref Method1()); // not valid. Cannot pass as `ref` argument
```

- `ref readonly` dönüşler aynı yerlerde izin verilir `ref` dönüşlerine izin verilir.
Bu, Dizin oluşturucular, temsilciler, Lambdalar, yerel işlevler içerir.

- `ref`/`ref readonly`/farklılığı aşırı yüklenmesine izin verilmez.

- Normal ByVal üzerinde aşırı yükleme ve dönüş farklılıkları `ref readonly` izin verilir.

- OHI (aşırı yükleme, gizleme, uygulama) amacı için `ref readonly` benzer ancak `ref`farklıdır.
Örneğin, `ref readonly` geçersiz kılan bir yöntem, kendisinin `ref readonly` ve kimlik dönüştürülebilir türü olmalıdır.

- Temsilci/Lambda/Yöntem grubu dönüştürmelerinde, `ref readonly` benzer ancak `ref`farklıdır.
Lambdalar ve geçerli yöntem grubu dönüştürme adayları, kimlik dönüştürülebilir olan türün `ref readonly` dönüşü ile hedef temsilcinin `ref readonly` geri dönmesi ile eşleşmelidir.

- Genel varyans için `ref readonly` dönüşler, değişken olmayan bir değer değildir.

> NOTE: başvuru ya da ilkel türler içeren `ref readonly` dönüşde uyarı yok.
Genel olarak daha az olabilir, ancak bazı durumlarda, kullanıcıların temelleri `in`olarak geçirmek gerekir. Örnekler-`T` `int`yerine `ref readonly T Method()` gibi genel bir yöntemi geçersiz kılma.
>
>`ref readonly` dönüşün verimsiz kullanımı durumlarında uyaran bir çözümleyiciye sahip olmak, ancak bu tür analizler için kurallar bir dil belirtiminin parçası olmak için çok belirsiz olacaktır.

### <a name="returning-from-ref-readonly-members"></a>`ref readonly` üyelerinden döndürme
Yöntem gövdesinin içinde sözdizimi, normal ref döndürimiyle aynıdır. `readonly`, kapsayan yöntemden çıkarsanacaktır.

Mosyon, `return ref readonly <expression>` gereksiz bir süredir ve yalnızca `readonly` bölümünde hatalara neden olacak uyuşmazlıkları sağlar.
Ancak `ref`, bir şeyin kesin diğer ad ile ve değere göre geçirildiği diğer senaryolarla tutarlılık için gereklidir.

> `in` parametrelerinin aksine, `ref readonly` hiçbir süre bir yerel kopya aracılığıyla döndürmez. Kopyanın bu tür bir uygulama döndürüldüğünde hemen mevcut olmaya başlayacağından emin olmak, daha az ve tehlikeli olur. Bu nedenle `ref readonly` dönüşler her zaman doğrudan başvurulardır.

Örnek:

```csharp
struct ImmutableArray<T>
{
    private readonly T[] array;

    public ref readonly T ItemRef(int i)
    {
        // returning a readonly reference to an array element
        return ref this.array[i];
    }
}

```

- `return ref` bağımsız değişkeni bir LValue olmalıdır (**varolan kural**)
- `return ref` bağımsız değişkeninin "dönmesi için güvenli" olması gerekir (**var olan kural**)
- Bir `ref readonly` üyesinde `return ref` bağımsız değişkeninin _yazılabilir olması_ gerekmez.
Örneğin, bu tür bir üye, salt okunur bir alanı veya `in` parametrelerinden birini döndürür.

### <a name="safe-to-return-rules"></a>Kuralları döndürmek için güvenli.
Başvuru kuralları için normal güvenli, salt okunur başvurulara de uygulanır.

`ref readonly`, normal `ref` yerel/parametre/dönüşten elde edilebilir, ancak bunun etrafında değil. Aksi takdirde `ref readonly` dönüşün güvenliği, normal `ref` dönüşler ile aynı şekilde algılanır.

RValues 'un `in` parametresi olarak geçirilebileceğini ve `ref readonly` olarak döndürüldüğünden emin olmak için, bir daha fazla kural gerekir- **rvalues başvuruya göre güvenli-geri döndürülemez**.

> Bir RValue, bir `in` parametreye bir kopya aracılığıyla geçirildiğinde ve sonra bir `ref readonly`biçiminde geri döndürüldüğünde durumu göz önünde bulundurun. Çağıran bağlamında, bu tür çağrının sonucu, yerel verilere yönelik bir başvurudur ve bu nedenle bu, döndürülmek üzere güvenli değildir.
> RValues değeri dönmek için güvenli olmadıktan sonra var olan kural `#6` bu durumu zaten işler.

Örnek:
```csharp
ref readonly Vector3 Test1()
{
    // can pass an RValue as "in" (via a temp copy)
    // but the result is not safe to return
    // because the RValue argument was not safe to return by reference
    return ref Test2(default(Vector3));
}

ref readonly Vector3 Test2(in Vector3 r)
{
    // this is ok, r is returnable
    return ref r;
}
```

`safe to return` kuralları güncelleştirildi:

1.  **yığındaki değişkenlere başvuruların dönmesi için güvenlidir**
2.  **ref/in parametrelerinin geri dönmesi**
`in` parametreleri doğal olarak yalnızca ReadOnly olarak döndürülebilir.
3.  **Out parametrelerinin dönmesi güvenlidir** (ancak zaten bugün olduğu gibi, kesinlikle atanması gerekir)
4.  **alıcının dönmesi güvenli olduğu sürece örnek struct alanları geri dönmek için güvenlidir**
5.  **' this ', yapı üyelerinden dönmek için güvenli değildir**
6.  **başka bir yöntemden döndürülen ref, bu yönteme biçimsel parametreler olarak geçirilen tüm ReFS/ıse 'nin dönmesi güvenli olduğu durumlarda döndürülür.** alıcı, *alıcının bir struct, Class veya genel tür parametresi olarak yazılmış olmasına bakılmaksızın, bu, özellikle alıcının dönmesi güvenli hale getirdiğinden ilgisiz değildir.* 

7.  **Rvalues, başvuruya göre dönmek için güvenli değildir.** 
*özel olarak RValues, parametrelere göre doğrudan geçiş için güvenlidir.*

> NOTE: ref benzeri türler ve ref atamaları dahil edildiğinde yürütmeye gelen dönüşlerle ilgili ek kurallar vardır.
> Kurallar `ref` ve `ref readonly` üyelerine eşit olarak uygulanır ve bu nedenle burada bahsedilmez.

### <a name="aliasing-behavior"></a>Diğer ad davranışı.
`ref readonly` Üyeler sıradan `ref` üyeleriyle aynı diğer ad davranışını sağlar (salt okunur olması dışında).
Bu nedenle, Lambdalar, zaman uyumsuz, yineleyiciler, yığın sıçraıcı vb. için yakalama amacına yöneliktir. aynı kısıtlamalar geçerlidir. Yani. gerçek başvuruların yakalanmasının nedeni ve üye değerlendirmesinin yan yana etkili olması nedeniyle bu senaryolara izin verilmez.

> `ref readonly` return, normal bir yazılabilir başvuru olarak `this` alan bir düzenli yapı yöntemleri alıcısında olduğunda bir kopya oluşturmak için izin verilir ve gereklidir. Tarihsel olarak, bu tür bir çağırmaları ReadOnly değişkenine uygulandığı her durumda yerel bir kopya yapılır.

### <a name="metadata-representation"></a>Meta veri gösterimi.
ByRef döndüren metodun dönüşe `System.Runtime.CompilerServices.IsReadOnlyAttribute` uygulandığında, yöntemin salt okunur bir başvuru döndürdüğü anlamına gelir.

Ayrıca, bu yöntemlerin sonuç imzasında (ve yalnızca bu yöntemler) `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`olmalıdır.

**Mosyon**: Bu, mevcut derleyicilerin `ref readonly` dönüşlerle yöntemleri çağırırken `readonly` yoksaymasını sağlamaktır

## <a name="readonly-structs"></a>ReadOnly yapılar
Kısa bir özellik içinde, `in` oluşturucular hariç, bir yapının tüm örnek üyelerinin `this` parametresini oluşturan bir özelliktir.

### <a name="motivation"></a>Amacı
Derleyici, bir struct örneğindeki herhangi bir yöntem çağrısının örneği değiştireolabileceğini varsaymalıdır. Aslında yazılabilir bir başvuru yönteme `this` parametresi olarak geçirilir ve bu davranışı tamamen sağlar. `readonly` değişkenlerde bu tür çağırmaları sağlamak için, çağırmaları geçici kopyalara uygulanır. Bu, sezgisel hale gelebilir ve bazen kişilerin performans nedenleriyle `readonly` iptal etmeye zorlar.  
Örnek: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/

`in` parametreleri için destek eklendikten ve `ref readonly`, salt okunur değişkenler daha yaygın hale gelediğinden, savunmaya karşı kopyalama sorununu geri döndürmektedir.

### <a name="solution"></a>Çözüm
Yapı bildirimlerinde `readonly` değiştiriciye izin verin; bu, `this` oluşturucular hariç tüm struct örneği yöntemlerinde `in` parametre olarak değerlendirilmelidir.

```csharp
static void Test(in Vector3 v1)
{
    // no need to make a copy of v1 since Vector3 is a readonly struct
    System.Console.WriteLine(v1.ToString());
}

readonly struct Vector3
{
    . . .

    public override string ToString()
    {
        // not OK!!  `this` is an `in` parameter
        foo(ref this.X);

        // OK
        return $"X: {X}, Y: {Y}, Z: {Z}";
    }
}
```

### <a name="restrictions-on-members-of-readonly-struct"></a>ReadOnly yapısının üyeleri hakkında kısıtlamalar
- ReadOnly yapısının örnek alanları salt okunur olmalıdır.  
**Mosyon:** yalnızca harici olarak yazılabilir ancak üyelere eklenebilir.
- Salt okunur bir yapının örnek oto özellikleri salt al olmalıdır.  
**Mosyon:** örnek alanlarında kısıtlamanın sonucu.
- ReadOnly struct alan benzeri olayları bildiremeyebilir.  
**Mosyon:** örnek alanlarında kısıtlamanın sonucu.

### <a name="metadata-representation"></a>Meta veri gösterimi.
Değer türüne `System.Runtime.CompilerServices.IsReadOnlyAttribute` uygulandığında, türün bir `readonly struct`olduğu anlamına gelir.

Özellikle:
-  `IsReadOnlyAttribute` türünün kimliği önemli değildir. Aslında, gerekirse kapsayan derlemede derleyici tarafından gömülebilir.

## <a name="refin-extension-methods"></a>`ref`/`in` uzantısı yöntemleri
Aslında mevcut bir teklif (https://github.com/dotnet/roslyn/issues/165) ve karşılık gelen prototip PR (https://github.com/dotnet/roslyn/pull/15650)vardır.
Yalnızca bu fikrin tamamen yeni olduğunu bildirmek istiyorum. Bununla birlikte, bu tür yöntemler hakkında çok sayıda sorunu ortadan kaldırdığından ve RValue alıcılarından ne yapmanız gerektiğini `ref readonly`.

Genel fikir, tür bir yapı türü olarak bilindiğinde, uzantı yöntemlerinin `this` parametresini başvuruya göre geçirmesine olanak sağlar.

```csharp
public static void Extension(ref this Guid self)
{
    // do something
}
```

Bu tür uzantı yöntemlerini yazma nedenleri öncelikle şunlardır:  
1.  Alıcı büyük bir yapı olduğunda kopyalamayı önleyin
2.  Yapı birimlerinde uzantı yöntemlerinin değiştirilmesine izin ver

Sınıflarda buna izin vermek istemediğimiz nedenler  
1.  Çok sınırlı bir amaç olabilir.
2.  Yöntem çağrısının, çağrıdan sonra `null` hale gelmesi için`null` olmayan alıcıyı dönemeyeceği uzun bir bozar bozabilir.
> Aslında, `ref` veya `out`tarafından _açıkça_ atanmamışsa veya geçirilmemişse`null` olmayan bir değişken `null` olamaz.
> Okunabilirliği veya diğer "Bu tür", "Bu bir null" analizinden büyük ölçüde yardımcı olur.
3.  Null koşullu erişimlerin "bir kez değerlendir" semantiğinin uzlanması zor olabilir.
Örnek: `obj.stringField?.RefExtension(...)`-null denetimini anlamlı hale getirmek için bir `stringField` kopyasının yakalanması gerekir, ancak daha sonra RefExtension içindeki `this` atamaları alana geri yansıtılmaz.

Başvuruya göre ilk bağımsız değişkeni alan **yapılar** üzerinde uzantı yöntemleri bildirme yeteneği uzun süreli bir istek idi. Engellenmeden biri "alıcı LValue değilse ne olur?" idi.

- Herhangi bir uzantı yönteminin statik bir yöntem olarak da çağrılacağından, bazı durumlarda belirsizlik çözümlenmenin tek yolu vardır. RValue alıcılarının izin verilmemelidir.
- Öte yandan, yapı örneği yöntemleri dahil edildiğinde benzer durumlarda bir kopyaya çağrı yapma yöntemi vardır.

"Örtük kopyalama" olmasının nedeni, yapı yöntemlerinin büyük çoğunluğunun, bunu belirtemediği sürece yapıyı gerçekten değiştirmeleridir. Bu nedenle en pratik çözüm, yalnızca bir kopyaya çağrı yapmak için, ancak bu uygulama, çok fazla performans ve hatalara neden olduğu bilinmektedir.

Artık `in` parametrelerinin kullanılabilirliğiyle, bir uzantının amacı işaret etmek mümkündür. Bu nedenle, `in` uzantıları gerektiğinde örtük kopyalamaya izin verdiğinden `ref` uzantılarının yazılabilir alıcılarla çağrılması zorunlu kılarak Conundrum çözülebilir.

```csharp
// this can be called on either RValue or an LValue
public static void Reader(in this Guid self)
{
    // do something nonmutating.
    WriteLine(self == default(Guid));
}

// this can be called only on an LValue
public static void Mutator(ref this Guid self)
{
    // can mutate self
    self = new Guid();
}
```

### <a name="in-extensions-and-generics"></a>`in` uzantıları ve genel türler.
`ref` uzantısı yöntemlerinin amacı, alıcıyı doğrudan veya üye değiştirici çağırarak çağırmak için kullanılır. Bu nedenle, `T` bir struct olarak kısıtlandığı sürece `ref this T` uzantılarına izin verilir.

Diğer taraftan `in` genişletme yöntemleri özellikle örtülü kopyalamayı azaltmak için vardır. Ancak, bir `in T` parametresinin tüm kullanımı bir arabirim üyesi aracılığıyla yapılmalıdır. Tüm arabirim üyeleri değişikliğe karşı kabul edildiğinden, bu tür bir kullanım için bir kopya gerekir. -Kopyalamayı azaltmak yerine, efekt tersi olur. Bu nedenle, `T` bir genel tür parametresi kısıtlamalardan bağımsız olarak `in this T` izin verilmez.

### <a name="valid-kinds-of-extension-methods-recap"></a>Geçerli uzantı yöntemleri türleri (Recap):
Bir genişletme yönteminde aşağıdaki `this` bildirimine yönelik olarak şu formlara izin verilir:
1) `this T arg`-normal ByVal uzantısı. (**mevcut durum**)
- T, başvuru türleri veya tür parametreleri de dahil olmak üzere herhangi bir tür olabilir.
Örnek, çağrıdan sonra aynı değişken olacaktır.
_Bu bağımsız değişken dönüştürme_ türünün örtük dönüştürmelerine izin verir.
, RValues üzerinde çağrılabilir.

- `in this T self` - `in` uzantısı.
T gerçek bir yapı türü olmalıdır.
Örnek, çağrıdan sonra aynı değişken olacaktır.
_Bu bağımsız değişken dönüştürme_ türünün örtük dönüştürmelerine izin verir.
, RValues üzerinde çağrılabilir (gerekirse geçici bir durum üzerinde çağrılabilir).

- `ref this T self` - `ref` uzantısı.
T bir struct türü ya da bir struct olarak kısıtlanmış genel tür parametresi olmalıdır.
Örnek, çağırma tarafından yazılmış olabilir.
Yalnızca kimlik dönüştürmelerine izin verir.
Yazılabilir LValue üzerinde çağrılmalıdır. (bir geçici aracılığıyla hiçbir şekilde çağırılmaz).

## <a name="readonly-ref-locals"></a>Salt okunur başvuru yerelleri.

### <a name="motivation"></a>Amacı.
`ref readonly` Üyeler tanıtıldıktan sonra, uygun tür yerel ile eşleştirilmeleri gereken kullanımı ortadan kaldırıldı. Bir üyenin değerlendirilmesi yan etkileri oluşturabilir veya gözlemlenebilir, bu nedenle sonuç birden çok kez kullanılacaksa, depolanması gerekir. Sıradan `ref` Yereller, `readonly` bir başvuruya atanmadığından burada yardım etmez.   

### <a name="solution"></a>Çözümden.
`ref readonly` yerelleri bildirmek için izin verin. Bu, yazılabilir olmayan `ref` yeni yereltür. Sonuç olarak `ref readonly` Yereller, yazma işlemleri için bu değişkenleri kullanıma açmadan ReadOnly değişkenlerine başvuruları kabul edebilir.

### <a name="declaring-and-using-ref-readonly-locals"></a>`ref readonly` Yereller bildirme ve kullanma.

Bu tür Yereller söz dizimi, bildirim sitesinde (söz konusu sırada) `ref readonly` değiştiricileri kullanır. Sıradan `ref` Yerellerden benzer şekilde, `ref readonly` Yereller, bildirimde, ref tarafından başlatılmış olmalıdır. Normal `ref` Yerellerden farklı olarak, `ref readonly` Yereller `in` parametreler, `readonly` alanları `ref readonly` Yöntemler gibi `readonly` LValues 'a başvurabilir.

Tüm amaçlar için bir `ref readonly` yerel bir `readonly` değişken olarak kabul edilir. Kullanım üzerindeki kısıtlamaların çoğu `readonly` alanlarla veya `in` parametreleriyle aynıdır.

Örneğin, bir yapı türüne sahip bir `in` parametresinin alanları, `readonly` değişkenleri olarak özyinelemeli olarak sınıflandırıldı.   

```csharp
static readonly ref Vector3 M1() => . . .

static readonly ref Vector3 M1_Trace()
{
    // OK
    ref readonly var r1 = ref M1();

    // Not valid. Need an LValue
    ref readonly Vector3 r2 = ref default(Vector3);

    // Not valid. r1 is readonly.
    Mutate(ref r1);

    // OK.
    Print(in r1);

    // OK.
    return ref r1;
}
```

### <a name="restrictions-on-use-of-ref-readonly-locals"></a>`ref readonly` Yereller kullanımıyla ilgili kısıtlamalar
`readonly` doğası haricinde, `ref readonly` Yereller sıradan `ref` Yereller gibi davranır ve tamamen aynı kısıtlamalara tabidir.  
Örneğin, kapanışlarda yakalamaya yönelik kısıtlamalar, `async` yöntemleri veya `safe-to-return` analizini bildirmek `ref readonly` Yereller için geçerlidir.

## <a name="ternary-ref-expressions-aka-conditional-lvalues"></a>Üçlü `ref` ifadeleri. (diğer adıyla "şartlı LValues")

### <a name="motivation"></a>Amacı
`ref` kullanımı ve `ref readonly` Yereller, bir koşula bağlı olarak bir veya başka bir hedef değişkeni ile bu tür yerelleri bir veya daha fazla yerelden başlatmaya gerek

Tipik bir geçici çözüm, şunun gibi bir yöntemi tanıtmaktadır:

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

_Tüm_ bağımsız değişkenlerin, sezgisel olmayan davranış ve hatalara yönelik olarak önde gelen çağrı sitesinde değerlendirilmesi gerektiğinden, `Choice` üçlü bir değiştirme değildir.

Aşağıdakiler beklendiği gibi çalışmayacak:

```csharp
    // will crash with NRE because 'arr[0]' will be executed unconditionally
    ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

### <a name="solution"></a>Çözüm
Bir koşula bağlı olarak LValue bağımsız değişkeninden birine başvuru olarak değerlendirilen, özel tür koşullu ifadeye izin verin.

### <a name="using-ref-ternary-expression"></a>`ref` Üçlü ifade kullanma.

Koşullu bir ifadenin `ref` Flavor için sözdizimi `<condition> ? ref <consequence> : ref <alternative>;`

Yalnızca normal koşullu ifade gibi `<consequence>` veya `<alternative>`, Boolean koşul ifadesinin sonucuna bağlı olarak değerlendirilir.

Sıradan koşullu ifadenin aksine, koşullu ifade `ref`:
- `<consequence>` ve `<alternative>` LValues gerektirir.
- `ref` koşullu ifadenin kendisi bir LValue ve
- hem `<consequence>` hem de `<alternative>` yazılabilir LValues ise `ref` koşullu ifade yazılabilir

Örnekler:  
`ref` üçlü bir LValue ve başvuruya göre geçirilebilir/atanabilir/döndürülebilir.
```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

LValue olarak da atanabilir.
```csharp
     // assign to
     (arr != null ? ref arr[0]: ref otherArr[0]) = 1;

     // error. readOnlyField is readonly and thus conditional expression is readonly
     (arr != null ? ref arr[0]: ref obj.readOnlyField) = 1;
```

, Bir yöntem çağrısının alıcısı olarak kullanılabilir ve gerekirse kopyalamayı atlayabilirsiniz.
```csharp
     // no copies
     (arr != null ? ref arr[0]: ref otherArr[0]).StructMethod();

     // invoked on a copy.
     // The receiver is `readonly` because readOnlyField is readonly.
     (arr != null ? ref arr[0]: ref obj.readOnlyField).StructMethod();

     // no copies. `ReadonlyStructMethod` is a method on a `readonly` struct
     // and can be invoked directly on a readonly receiver
     (arr != null ? ref arr[0]: ref obj.readOnlyField).ReadonlyStructMethod();
```

`ref` Üçlü, normal (Ref değil) bağlamda de kullanılabilir.
```csharp
     // only an example
     // a regular ternary could work here just the same
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```

### <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Başvurular ve salt okunur başvurular için gelişmiş desteğe karşı iki önemli bağımsız değişken görebiliyorum:

1) Burada çözülen sorunlar çok eski. Özellikle de, var olan koda yardımcı olmadığından, neden daha önce bu dosyaları şimdi çözmektedir?

Yeni etki alanlarında C# bulduğumuz ve .NET tarafından kullanıldıkları gibi bazı sorunlar daha belirgin hale gelir.  
Hesaplama fazla kafaları hakkında ortalamaya göre daha kritik olan ortamlara örnek olarak, şunları Listelerim

* hesaplamanın faturalandırılması ve yanıt verebildiği bulut/veri merkezi senaryoları rekabetçi bir avantajdır.
* Gecikme sürelerinde geçici gerçek zamanlı gereksinimlere sahip Oyunlar/VR/AR     

Bu özellik, bazı yaygın senaryolarda daha fazla gözlerine izin verirken tür-güvenlik gibi mevcut güçlerin hiçbirini etkilemez.

2) `readonly` sözleşmelerde, çağrılan kuralların kuralları tarafından oynamasını makul bir şekilde garanti edebilir misiniz?

`out`kullanırken buna benzer bir güveniz var. Yanlış `out` uygulanması belirtilmeyen davranışa neden olabilir, ancak gerçekte nadiren meydana gelir.  

Resmi doğrulama kurallarının `ref readonly` tanıdık getirilmesi, güven sorununu daha da azaltır.

### <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

Asıl rekabet tasarımı gerçekten "hiçbir şey yapmaz".

### <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

### <a name="design-meetings"></a>Tasarım toplantıları

https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md
