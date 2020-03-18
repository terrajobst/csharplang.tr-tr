---
ms.openlocfilehash: 5636157ba54e93587847d6f2f7aac2dc675f3112
ms.sourcegitcommit: af27912886f22cda9b98b7769447d85cd9732736
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "79485002"
---

# <a name="records-v2"></a>Kayıt v2

Geçmişte, verilerle çalışmayı sağlamak için kayıtları bir özellik olarak düşündük.

"Verilerle çalışma", çok sayıda model içeren büyük bir gruptur, bu nedenle her bir yalıtıma göre her birine bakmak ilginç olabilir. Şimdi bir kayıt örneğine ve bazı dezavantajlarına bakarak başlayalım.

Örneğin, basit bir kayıt bugün şu şekilde tanımlanır

```C#
public class UserInfo
{
    public string Username { get; set; }
    public string Email { get; set; }
    public bool IsAdmin  { get; set; } = false;
}
```

ve kullanımı okur

```C#
void M()
{
    var userInfo = new UserInfo() 
    {
        Username = "andy",
        Email = "angocke@microsoft.com",
        IsAdmin = true
    };
}
```

Bu kodda önemli avantajlar vardır:

1. Tanım, sürüm dayanıklı, Özellikler kolayca eklenebilir veya taşınabilir
2. Özellikler herhangi bir sırada ayarlanabilir ve başlatma içindeki adlar her zaman erişimcileri ile eşleşir
3. Varsayılan değerleri olan özellikler yalnızca atlanabilir

İlk kusur, özelliklerin artık değişebilir olması gerekir.

# <a name="mutability"></a>Değişebilirliği

Beğendiğimiz özellikler, C# nesne başlatıcılarında `readonly` üye ayarlamak için bir yol sağlamaktır.
Bazı türler bu başlatma için göz önünde bulundurularak tasarlanmamışsa, kabul etmek de istiyorum.

Önerilen çözüm, Özellikler ve alanlara uygulanabilen `initonly`yeni bir değiştiricisidir:

```C#
public class UserInfo
{
    public initonly string Username { get; }
    public initonly string Email { get; }
    public initonly bool IsAdmin { get; } = false;
}
```

Bunun için CodeGen, normal olarak ileri sarma: salt okunur alanını ayarladık.
Özellikle de alçaltılan özellikler şöyle görünür:

```C#
public class UserInfo
{
    private readonly string <Backing>_username;
    public string get_Username() => <Backing>_username;
    [return: modreq(initonly)]
    public void set_Username(string value) { <Backing>_username = value; }
    ...
}
```

CLR salt okunur alanların doğrulanamayan şekilde ayarlanmasını kabul eder, ancak güvenli değildir. Daha gelişmiş bir doğrulayıcı desteklemek için aşağıdaki kural önerilir: salt okunur bir alan yalnızca `initonly` metotlarda veya CLR yığınında olan ve bir mağaza ya da yöntem çağrısıyla yayımlanmayan yeni bir nesne içinde değiştirilebilir.

Bu, `UserInfo` örnekte karmaşık veya Brittle yayma stratejileri gerektirmeyen birçok sorunu çözmelidir. Ancak, dengeszlik kullanılabilirliği yeni bir sorun sunar: bir nesneyi değişikliklerle kolayca oluşturma.

# <a name="with-ing"></a>Birlikte

Değişiklik yapılbilme ile programlama yaparken, değişiklikleri doğrudan nesne üzerinde yapmak yerine değişiklikler içeren bir kopya oluşturarak bir nesnede değişiklik yapma işlemi yapılır. Ne yazık ki, geçerli stil kayıtlarıyla bile bunu yapmak C#için kullanışlı bir yol yoktur. Daha önce bu işlevselliği uygulayan kayıtlar için bazı bir tür oluşturulmuş "with" yönteminin sağlanması önerilir. Böyle bir mekanizmanız varsa `initonly` üyeleriyle çalışması önemlidir. Bunu başarmak için, bir nesne başlatıcısına benzer bir `with` ifadesi eklememiz önerilir. Örnek kullanım aşağıdaki gibi olacaktır:

```C#
var userInfo = new UserInfo() 
{
    Username = "andy",
    Email = "angocke@microsoft.com",
    IsAdmin = true
};
var newUserName = userInfo with { Username = "angocke" };
```

Elde edilen `newUserName` nesnesi, `Username` "angocke" olarak ayarlanan `userInfo`bir kopyasıdır.
`with` ifadesindeki CodeGen, nesne başlatıcısına de benzer: yeni bir nesne oluşturulur ve ardından Yöntem gövdesinde `initonly` `Username` ayarlayıcısı çağırılır.

Tabii ki buradaki fark, oluşturulan yeni nesnenin basit bir yeni nesne oluşturma işlemi olmaması, özgün nesnenin yinelemesi olması olabilir. Bu işlevi sağlamak için, nesnenin yinelenen bir nesne sağlayan bir "Oluşturucu Içeren" sağlaması gerekir. Örnek bir `With` Oluşturucu şöyle görünür:

```C#
class UserInfo
{
    ...
    [WithConstructor] // placeholder syntax, up for debate
    public UserInfo With()
    {
        return new UserInfo() { Username = this.Username, Email = this.Email, IsAdmin = this.IsAdmin };
    }
}
```

Özellikle, `with` ifade, nesne Başlatıcısı gibi `initonly` üyeleri ayarlar, bu nedenle doğrulamayı desteklemek için nesnenin `initonly` Üyeler ayarlanmadan önce yayımlanmaması gerekir. Bunu zorlamak için, `WithConstructor` özniteliği (veya eşdeğer sözdizimi) yöntemi için yeni bir kural uygular: ALL Return deyimlerinin, muhtemelen bir nesne başlatıcısı ile bir nesne oluşturma ifadesini içermesi gerekir.

`With` Oluşturucu doğrulama gerektiriyorsa, Kullanıcı bu doğrulamayı yapmak için bir Oluşturucu ortaya çıkarabilir, örn.

```C#
class UserInfo
{
    ...
    private UserInfo(UserInfo original)
    {
        // validation code
    }
    [WithConstructor]
    public UserInfo With() => new UserInfo(this);
}
```

`With` ilişkili en son karmaşıklık parçası devralıdır. Kaydınız Genişletilebilir ise, alt sınıf için yeni bir `With` sağlamanız gerekir. Bu, aşağıdaki gibi sağlanabilir:

```C#
class Base
{
    ...
    protected Base(Base original)
    {
        // validation
    }
    [WithConstructor]
    public virtual Base With() => new Base(this);
}
class Derived : Base
{
    ...
    protected Derived(Derived original)
    : base(original)
    {
        // validation
    }
    [WithConstructor]
    public override Derived With() => new Derived(this);
}
```

Buradaki ek bir karmaşıklık parçasını burada görebilirsiniz: `With` oluşturucusunu türetilmiş tür ile geçersiz kılmak için, dilin geçersiz Kılmalarda birlikte değişken dönüş türlerini desteklemesi gerekir.
[Burada](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md)bu özellik için ayrı bir teklif zaten var.

**Bulunmaktadır**

- `WithConstructor`s içindeki tüm dönüş deyimlerinin yeni nesne ifadeleri içermesi, kısıtlayıcıdır.
  Bunun nedeni, yeni nesnenin yöntemi atmamasını sağlayan akış analizi tarafından azaltılabilir
- Derleyici püf noktaları aracılığıyla geçersiz kılmaların desteklenme sapması, devralma yöntemlerine ihtiyaç duyar ve bu, miras derinliğine göre dört büyüyerek artar. Saplama yöntemi gereksinimi, imza eşleşmesini tam olarak geçersiz kılan bir çalışma zamanı gereksinimidir. Çalışma zamanı gereksinimi gevşulmuş ise, saplama yöntemlerinin tümü gerekli değildir.
- Formun zincirleme oluşturucularını kullanmak `Type(Type original)`, bu oluşturucuyu düzenin kullanımı için etkin bir şekilde ayırır. Oluşturucuların benzersiz anlamları olduğundan ve yeniden adlandırıldığından, bu sınırlama sınırlandırılamıyor.


## <a name="wrapping-it-all-up-records"></a>Tümünü sarmalama: kayıtlar

Yukarıdaki özellikler daha önce çok zor bir programlama stilini etkinleştirir. Bununla birlikte, yeni özellikler de oldukça ayrıntılıdır ve her şeyi kendinize ekleme konusunda bir hata olabilir. Ayrıca, eşittir ve GetHashCode gibi birkaç öğe de mevcuttur. Bu yalnızca bugün yazılabilir. yalnızca laborious.
Üstelik, bu yeni temellerde eşitlik uygularken önemli bir hata, yapısal eşitlik, yeni veriler eklendikçe veri türü ile değiştirilmesi gereken bir şeydir, ancak bunu el ile gerçekleştirirken, büyük olasılıkla bu şeyler eşitlenmemiş olabilir.

Bu nedenle, yeni özellikler sağlamak C# için değil, kayıtlar için yeni sözdizimini desteklemek, ancak varsayılanları ayarlamak ve kayıtlarda kullanılmak üzere tasarlanan kodu oluşturmak için önerilir. Örnek sözdizimi şöyle görünür

```C#
data class UserInfo
{
    public string Username { get; }
    public string Email { get; }
    public bool IsAdmin { get; } = false;
}
```

Bu sınıf için üretilen kod, tüm ortak alanları ve otomatik özellikleri kaydın yapısal üyeleri olarak bir arada olacaktır. Kayıt üyeleri, üyeleri dahil etmek veya hariç tutmak için kullanılabilecek yeni bir `RecordMember(bool)` özniteliği kullanılarak özelleştirilebilir. Kayıt üyeleri varsayılan olarak `initonly` ve eşitlik, kayıt üyelerine bağlı olarak sınıf için otomatik olarak oluşturulur. Herhangi bir noktada, bu üyelerin davranışı yalnızca kaynak olarak bildirerek özelleştirilebilir. Kullanıcı tarafından yazılmış uygulama, tüm model kullanımındaki varsayılan uygulamanın yerini alır.

Devralım yüzündeki eşitlik karmaşıktır, ancak [diğer kayıtlar teklifinde](records.md)yeterince çözülebileceğine unutmayın.

## <a name="primary-constructors"></a>Birincil oluşturucular

Önceki kayıt teklifi, türün kendisindeki bir parametre listesi için yeni bir sözdizimi de içeriyordu. Örneğin,

```C#
class Point(int X, int Y);
```

Yeni tasarımda parametre listesi, kayıtlar ile düzgün bir şekilde tümleştirilebilen, C# dikgen bir özelliktir. Birincil Oluşturucu bir kayda dahil ise, yalnızca ortak alanlar ve otomatik özellikler gibi yeni varsayılan değerlere sahip olur: birincil oluşturucudaki parametreler aynı ada sahip ortak kayıt-üye özellikleri oluşturmak için kullanılır. Ayrıca, birincil Oluşturucu artık bir Deconstructor otomatik olarak oluşturmak için kullanılabilir.

Örneğin, birincil Oluşturucusu olan aşağıdaki kayıt

```C#
data class Point(int X, int Y);
```

eşdeğer olacaktır

```C#
data class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }
}
```

ve yukarıdaki son nesil

```C#
class Point
{
    public initonly int X { get; }
    public initonly int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    protected Point(Point other)
    : this(other.X, other.Y)
    { }

    [WithConstructor]
    public virtual Point With() => new Point(this);

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }

    // Generated equality
}
```

Birincil oluşturucuya sahip bir veri sınıfı için başka bir bilgi parçasını hesaba geçirdik. oluşturulan korumalı oluşturucunun içindeki birincil alanları ayarlamak yerine, birincil oluşturucuya temsilci seçiyoruz. Point sınıfının birincil olmayan başka bir kayıt üyesi varsa, ör.

```C#
data class Point(int X, int Y)
{
    public int Z { get; }
}
```

ardından, oluşturulan korumalı oluşturucuyu aşağıdaki gibi değiştirir:

```C#
class Point
{
    // ...
    protected Point(Point other)
    : this(other.X, other.Y)
    {
        Z = other.Z;
    }
    // ...
}
```

Özellikle, bu, birincil oluşturucularla kayıtların devralınması hakkında ne yapılacağını yanıtlayamıyor. Örneğin,

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A;
```

Rastgele bir şekilde çözümlemek yerine, daha açık bir yaklaşım temel liste ile bir parametre listesinin sağlanmasını gerektirebilir, örn.

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A(X, Y);
```

Temel listedeki parametre listesi daha sonra oluşturulan birincil oluşturucuda bir `base` çağrısına uygulanır:

```C#
class B
{
    // ..
    public B(int x, int y, int z)
    : base(x, y)
    // ..
}
```

Birincil oluşturucunun bir kaydın dışından ne kadar önemli olduğu gibi, bu da daha fazla teklife açık olmaya devam eder.
