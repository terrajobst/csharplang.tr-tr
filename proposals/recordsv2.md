---
ms.openlocfilehash: 5636157ba54e93587847d6f2f7aac2dc675f3112
ms.sourcegitcommit: af27912886f22cda9b98b7769447d85cd9732736
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "79485002"
---

# <a name="records-v2"></a><span data-ttu-id="06cad-101">Kayıt v2</span><span class="sxs-lookup"><span data-stu-id="06cad-101">Records v2</span></span>

<span data-ttu-id="06cad-102">Geçmişte, verilerle çalışmayı sağlamak için kayıtları bir özellik olarak düşündük.</span><span class="sxs-lookup"><span data-stu-id="06cad-102">In the past we've thought about records as a feature to enable working with data.</span></span>

<span data-ttu-id="06cad-103">"Verilerle çalışma", çok sayıda model içeren büyük bir gruptur, bu nedenle her bir yalıtıma göre her birine bakmak ilginç olabilir.</span><span class="sxs-lookup"><span data-stu-id="06cad-103">"Working with data" is a big group with a number of facets, so it may be interesting to look at each in isolation.</span></span> <span data-ttu-id="06cad-104">Şimdi bir kayıt örneğine ve bazı dezavantajlarına bakarak başlayalım.</span><span class="sxs-lookup"><span data-stu-id="06cad-104">Let's start by looking at an example of records today and some of its drawbacks.</span></span>

<span data-ttu-id="06cad-105">Örneğin, basit bir kayıt bugün şu şekilde tanımlanır</span><span class="sxs-lookup"><span data-stu-id="06cad-105">For instance, a simple record would be defined today as follows</span></span>

```C#
public class UserInfo
{
    public string Username { get; set; }
    public string Email { get; set; }
    public bool IsAdmin  { get; set; } = false;
}
```

<span data-ttu-id="06cad-106">ve kullanımı okur</span><span class="sxs-lookup"><span data-stu-id="06cad-106">and the usage would read</span></span>

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

<span data-ttu-id="06cad-107">Bu kodda önemli avantajlar vardır:</span><span class="sxs-lookup"><span data-stu-id="06cad-107">There are significant advantages in this code:</span></span>

1. <span data-ttu-id="06cad-108">Tanım, sürüm dayanıklı, Özellikler kolayca eklenebilir veya taşınabilir</span><span class="sxs-lookup"><span data-stu-id="06cad-108">The definition is version resilient, properties can easily be added or moved</span></span>
2. <span data-ttu-id="06cad-109">Özellikler herhangi bir sırada ayarlanabilir ve başlatma içindeki adlar her zaman erişimcileri ile eşleşir</span><span class="sxs-lookup"><span data-stu-id="06cad-109">Properties can be set in any order, and the names in the initialization always match the accessors</span></span>
3. <span data-ttu-id="06cad-110">Varsayılan değerleri olan özellikler yalnızca atlanabilir</span><span class="sxs-lookup"><span data-stu-id="06cad-110">Properties with default values can simply be skipped</span></span>

<span data-ttu-id="06cad-111">İlk kusur, özelliklerin artık değişebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="06cad-111">The first flaw is that the properties must now be mutable.</span></span>

# <a name="mutability"></a><span data-ttu-id="06cad-112">Değişebilirliği</span><span class="sxs-lookup"><span data-stu-id="06cad-112">Mutability</span></span>

<span data-ttu-id="06cad-113">Beğendiğimiz özellikler, C# nesne başlatıcılarında `readonly` üye ayarlamak için bir yol sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="06cad-113">What we'd like is for C# to provide a way to set a `readonly` member in object initializers.</span></span>
<span data-ttu-id="06cad-114">Bazı türler bu başlatma için göz önünde bulundurularak tasarlanmamışsa, kabul etmek de istiyorum.</span><span class="sxs-lookup"><span data-stu-id="06cad-114">Since some types may not have been designed with this initialization in mind, we'd also like it to be opt-in.</span></span>

<span data-ttu-id="06cad-115">Önerilen çözüm, Özellikler ve alanlara uygulanabilen `initonly`yeni bir değiştiricisidir:</span><span class="sxs-lookup"><span data-stu-id="06cad-115">The proposed solution is a new modifier, `initonly`, that can be applied to properties and fields:</span></span>

```C#
public class UserInfo
{
    public initonly string Username { get; }
    public initonly string Email { get; }
    public initonly bool IsAdmin { get; } = false;
}
```

<span data-ttu-id="06cad-116">Bunun için CodeGen, normal olarak ileri sarma: salt okunur alanını ayarladık.</span><span class="sxs-lookup"><span data-stu-id="06cad-116">The codegen for this is surprisingly straight forward: we just set the readonly field.</span></span>
<span data-ttu-id="06cad-117">Özellikle de alçaltılan özellikler şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="06cad-117">Specifically, the lowered properties would look like:</span></span>

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

<span data-ttu-id="06cad-118">CLR salt okunur alanların doğrulanamayan şekilde ayarlanmasını kabul eder, ancak güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="06cad-118">The CLR considers setting readonly fields to be unverifiable, but not unsafe.</span></span> <span data-ttu-id="06cad-119">Daha gelişmiş bir doğrulayıcı desteklemek için aşağıdaki kural önerilir: salt okunur bir alan yalnızca `initonly` metotlarda veya CLR yığınında olan ve bir mağaza ya da yöntem çağrısıyla yayımlanmayan yeni bir nesne içinde değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="06cad-119">To support a more advanced verifier, the following rule is proposed: a readonly field can be modified only inside `initonly` methods, or on a new object that is on the CLR stack and has not been published via a store or method call.</span></span>

<span data-ttu-id="06cad-120">Bu, `UserInfo` örnekte karmaşık veya Brittle yayma stratejileri gerektirmeyen birçok sorunu çözmelidir.</span><span class="sxs-lookup"><span data-stu-id="06cad-120">This should solve many of the problems with mutability in the `UserInfo` example, while not requiring complicated or brittle emit strategies.</span></span> <span data-ttu-id="06cad-121">Ancak, dengeszlik kullanılabilirliği yeni bir sorun sunar: bir nesneyi değişikliklerle kolayca oluşturma.</span><span class="sxs-lookup"><span data-stu-id="06cad-121">However, immutability does present a new problem: easily constructing an object with changes.</span></span>

# <a name="with-ing"></a><span data-ttu-id="06cad-122">Birlikte</span><span class="sxs-lookup"><span data-stu-id="06cad-122">With-ing</span></span>

<span data-ttu-id="06cad-123">Değişiklik yapılbilme ile programlama yaparken, değişiklikleri doğrudan nesne üzerinde yapmak yerine değişiklikler içeren bir kopya oluşturarak bir nesnede değişiklik yapma işlemi yapılır.</span><span class="sxs-lookup"><span data-stu-id="06cad-123">When programming with immutability, making changes to an object is done by constructing a copy with changes instead of making the changes directly on the object.</span></span> <span data-ttu-id="06cad-124">Ne yazık ki, geçerli stil kayıtlarıyla bile bunu yapmak C#için kullanışlı bir yol yoktur.</span><span class="sxs-lookup"><span data-stu-id="06cad-124">Unfortunately, there's no convenient way to do this in C#, even with current-style records.</span></span> <span data-ttu-id="06cad-125">Daha önce bu işlevselliği uygulayan kayıtlar için bazı bir tür oluşturulmuş "with" yönteminin sağlanması önerilir.</span><span class="sxs-lookup"><span data-stu-id="06cad-125">It's been previously proposed that some kind of autogenerated "With" method be provided for records that implements that functionality.</span></span> <span data-ttu-id="06cad-126">Böyle bir mekanizmanız varsa `initonly` üyeleriyle çalışması önemlidir.</span><span class="sxs-lookup"><span data-stu-id="06cad-126">If we have such a mechanism, it's important that it work with `initonly` members.</span></span> <span data-ttu-id="06cad-127">Bunu başarmak için, bir nesne başlatıcısına benzer bir `with` ifadesi eklememiz önerilir.</span><span class="sxs-lookup"><span data-stu-id="06cad-127">To achieve this, it's proposed that we add a `with` expression, analogous to an object initializer.</span></span> <span data-ttu-id="06cad-128">Örnek kullanım aşağıdaki gibi olacaktır:</span><span class="sxs-lookup"><span data-stu-id="06cad-128">Sample usage would be as follows:</span></span>

```C#
var userInfo = new UserInfo() 
{
    Username = "andy",
    Email = "angocke@microsoft.com",
    IsAdmin = true
};
var newUserName = userInfo with { Username = "angocke" };
```

<span data-ttu-id="06cad-129">Elde edilen `newUserName` nesnesi, `Username` "angocke" olarak ayarlanan `userInfo`bir kopyasıdır.</span><span class="sxs-lookup"><span data-stu-id="06cad-129">The resulting `newUserName` object would be a copy of `userInfo`, with `Username` set to "angocke".</span></span>
<span data-ttu-id="06cad-130">`with` ifadesindeki CodeGen, nesne başlatıcısına de benzer: yeni bir nesne oluşturulur ve ardından Yöntem gövdesinde `initonly` `Username` ayarlayıcısı çağırılır.</span><span class="sxs-lookup"><span data-stu-id="06cad-130">The codegen on the `with` expression would also be similar to the object initializer: a new object is constructed, and then the `initonly` `Username` setter would be called in the method body.</span></span>

<span data-ttu-id="06cad-131">Tabii ki buradaki fark, oluşturulan yeni nesnenin basit bir yeni nesne oluşturma işlemi olmaması, özgün nesnenin yinelemesi olması olabilir.</span><span class="sxs-lookup"><span data-stu-id="06cad-131">Of course, the difference here is that the new object being constructed is not a simple new object creation, it is a duplicate of the original object.</span></span> <span data-ttu-id="06cad-132">Bu işlevi sağlamak için, nesnenin yinelenen bir nesne sağlayan bir "Oluşturucu Içeren" sağlaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="06cad-132">To provide this functionality, we require that the object provide a "With constructor" that provides a duplicate object.</span></span> <span data-ttu-id="06cad-133">Örnek bir `With` Oluşturucu şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="06cad-133">A sample `With` constructor would look like:</span></span>

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

<span data-ttu-id="06cad-134">Özellikle, `with` ifade, nesne Başlatıcısı gibi `initonly` üyeleri ayarlar, bu nedenle doğrulamayı desteklemek için nesnenin `initonly` Üyeler ayarlanmadan önce yayımlanmaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="06cad-134">Notably, the `with` expression will set `initonly` members, just like the object initializer, so to support verification we must ensure that the object cannot have been published before the `initonly` members are set.</span></span> <span data-ttu-id="06cad-135">Bunu zorlamak için, `WithConstructor` özniteliği (veya eşdeğer sözdizimi) yöntemi için yeni bir kural uygular: ALL Return deyimlerinin, muhtemelen bir nesne başlatıcısı ile bir nesne oluşturma ifadesini içermesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="06cad-135">To enforce this, the `WithConstructor` attribute (or equivalent syntax) will enforce a new rule for the method: all return statements must directly contain an object creation expression, possibly with an object initializer.</span></span>

<span data-ttu-id="06cad-136">`With` Oluşturucu doğrulama gerektiriyorsa, Kullanıcı bu doğrulamayı yapmak için bir Oluşturucu ortaya çıkarabilir, örn.</span><span class="sxs-lookup"><span data-stu-id="06cad-136">If the `With` constructor requires validation, the user may introduce a constructor to do that validation, e.g.</span></span>

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

<span data-ttu-id="06cad-137">`With` ilişkili en son karmaşıklık parçası devralıdır.</span><span class="sxs-lookup"><span data-stu-id="06cad-137">The last piece of complexity associated with `With` is inheritance.</span></span> <span data-ttu-id="06cad-138">Kaydınız Genişletilebilir ise, alt sınıf için yeni bir `With` sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="06cad-138">If your record is extensible, you will need to provide a new `With` for the subclass.</span></span> <span data-ttu-id="06cad-139">Bu, aşağıdaki gibi sağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="06cad-139">This can be achieved as follows:</span></span>

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

<span data-ttu-id="06cad-140">Buradaki ek bir karmaşıklık parçasını burada görebilirsiniz: `With` oluşturucusunu türetilmiş tür ile geçersiz kılmak için, dilin geçersiz Kılmalarda birlikte değişken dönüş türlerini desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="06cad-140">Note one additional piece of complexity here: in order to override the `With` constructor with the derived type the language will also need to support covariant return types in overrides.</span></span>
<span data-ttu-id="06cad-141">[Burada](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md)bu özellik için ayrı bir teklif zaten var.</span><span class="sxs-lookup"><span data-stu-id="06cad-141">There is already a separate proposal for this feature [here](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md).</span></span>

<span data-ttu-id="06cad-142">**Bulunmaktadır**</span><span class="sxs-lookup"><span data-stu-id="06cad-142">**Drawbacks**</span></span>

- <span data-ttu-id="06cad-143">`WithConstructor`s içindeki tüm dönüş deyimlerinin yeni nesne ifadeleri içermesi, kısıtlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="06cad-143">Making all return statements in `WithConstructor`s contain new object expressions is restrictive.</span></span>
  <span data-ttu-id="06cad-144">Bunun nedeni, yeni nesnenin yöntemi atmamasını sağlayan akış analizi tarafından azaltılabilir</span><span class="sxs-lookup"><span data-stu-id="06cad-144">This could be possibly be mitigated by flow analysis that ensures the new object doesn't escape the method</span></span>
- <span data-ttu-id="06cad-145">Derleyici püf noktaları aracılığıyla geçersiz kılmaların desteklenme sapması, devralma yöntemlerine ihtiyaç duyar ve bu, miras derinliğine göre dört büyüyerek artar.</span><span class="sxs-lookup"><span data-stu-id="06cad-145">Supporting variance in overrides through compiler tricks will require stub methods, which will grow quadratically with the inheritance depth.</span></span> <span data-ttu-id="06cad-146">Saplama yöntemi gereksinimi, imza eşleşmesini tam olarak geçersiz kılan bir çalışma zamanı gereksinimidir.</span><span class="sxs-lookup"><span data-stu-id="06cad-146">The need for a stub method is due to a runtime requirement that override signatures match exactly.</span></span> <span data-ttu-id="06cad-147">Çalışma zamanı gereksinimi gevşulmuş ise, saplama yöntemlerinin tümü gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="06cad-147">If the runtime requirement were loosened, the stub methods would not be required at all.</span></span>
- <span data-ttu-id="06cad-148">Formun zincirleme oluşturucularını kullanmak `Type(Type original)`, bu oluşturucuyu düzenin kullanımı için etkin bir şekilde ayırır.</span><span class="sxs-lookup"><span data-stu-id="06cad-148">Using chained constructors of the form `Type(Type original)` effectively reserves that constructor for the use of the pattern.</span></span> <span data-ttu-id="06cad-149">Oluşturucuların benzersiz anlamları olduğundan ve yeniden adlandırıldığından, bu sınırlama sınırlandırılamıyor.</span><span class="sxs-lookup"><span data-stu-id="06cad-149">Since constructors have unique semantics and cannot be re-named this could be limiting.</span></span>


## <a name="wrapping-it-all-up-records"></a><span data-ttu-id="06cad-150">Tümünü sarmalama: kayıtlar</span><span class="sxs-lookup"><span data-stu-id="06cad-150">Wrapping it all up: Records</span></span>

<span data-ttu-id="06cad-151">Yukarıdaki özellikler daha önce çok zor bir programlama stilini etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="06cad-151">The above features enable a style of programming that was very difficult before.</span></span> <span data-ttu-id="06cad-152">Bununla birlikte, yeni özellikler de oldukça ayrıntılıdır ve her şeyi kendinize ekleme konusunda bir hata olabilir.</span><span class="sxs-lookup"><span data-stu-id="06cad-152">But even with the new features it could be quite verbose and error prone to annotate everything yourself.</span></span> <span data-ttu-id="06cad-153">Ayrıca, eşittir ve GetHashCode gibi birkaç öğe de mevcuttur. Bu yalnızca bugün yazılabilir. yalnızca laborious.</span><span class="sxs-lookup"><span data-stu-id="06cad-153">There are also a few items, like Equals and GetHashCode, which can already be written today, it's just laborious.</span></span>
<span data-ttu-id="06cad-154">Üstelik, bu yeni temellerde eşitlik uygularken önemli bir hata, yapısal eşitlik, yeni veriler eklendikçe veri türü ile değiştirilmesi gereken bir şeydir, ancak bunu el ile gerçekleştirirken, büyük olasılıkla bu şeyler eşitlenmemiş olabilir.</span><span class="sxs-lookup"><span data-stu-id="06cad-154">Moreover, a significant flaw in implementing equality on top of these new primitives is that structural equality is something that should change with your data type as new data is added, but when handling it manually it is likely that these things can get out of sync.</span></span>

<span data-ttu-id="06cad-155">Bu nedenle, yeni özellikler sağlamak C# için değil, kayıtlar için yeni sözdizimini desteklemek, ancak varsayılanları ayarlamak ve kayıtlarda kullanılmak üzere tasarlanan kodu oluşturmak için önerilir.</span><span class="sxs-lookup"><span data-stu-id="06cad-155">Therefore, it is proposed that C# support new syntax for records, not for providing new features, but for setting defaults and generating code designed for use in records.</span></span> <span data-ttu-id="06cad-156">Örnek sözdizimi şöyle görünür</span><span class="sxs-lookup"><span data-stu-id="06cad-156">Example syntax would look like</span></span>

```C#
data class UserInfo
{
    public string Username { get; }
    public string Email { get; }
    public bool IsAdmin { get; } = false;
}
```

<span data-ttu-id="06cad-157">Bu sınıf için üretilen kod, tüm ortak alanları ve otomatik özellikleri kaydın yapısal üyeleri olarak bir arada olacaktır.</span><span class="sxs-lookup"><span data-stu-id="06cad-157">The generated code for this class would regard all public fields and auto-properties as structural members of the record.</span></span> <span data-ttu-id="06cad-158">Kayıt üyeleri, üyeleri dahil etmek veya hariç tutmak için kullanılabilecek yeni bir `RecordMember(bool)` özniteliği kullanılarak özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="06cad-158">Record members could be customized using a new `RecordMember(bool)` attribute that could be used to either include or exclude members.</span></span> <span data-ttu-id="06cad-159">Kayıt üyeleri varsayılan olarak `initonly` ve eşitlik, kayıt üyelerine bağlı olarak sınıf için otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="06cad-159">Record members would be `initonly` by default and equality would be autogenerated for the class based on the record members.</span></span> <span data-ttu-id="06cad-160">Herhangi bir noktada, bu üyelerin davranışı yalnızca kaynak olarak bildirerek özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="06cad-160">At any point the behavior of these members could be customized simply by declaring them in source.</span></span> <span data-ttu-id="06cad-161">Kullanıcı tarafından yazılmış uygulama, tüm model kullanımındaki varsayılan uygulamanın yerini alır.</span><span class="sxs-lookup"><span data-stu-id="06cad-161">The user-written implementation would replace the default implementation in all pattern usage.</span></span>

<span data-ttu-id="06cad-162">Devralım yüzündeki eşitlik karmaşıktır, ancak [diğer kayıtlar teklifinde](records.md)yeterince çözülebileceğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="06cad-162">Note that equality in the face of inheritance is complex, but seems to have been adequately solved in the [other records proposal](records.md).</span></span>

## <a name="primary-constructors"></a><span data-ttu-id="06cad-163">Birincil oluşturucular</span><span class="sxs-lookup"><span data-stu-id="06cad-163">Primary constructors</span></span>

<span data-ttu-id="06cad-164">Önceki kayıt teklifi, türün kendisindeki bir parametre listesi için yeni bir sözdizimi de içeriyordu. Örneğin,</span><span class="sxs-lookup"><span data-stu-id="06cad-164">Previous record proposal have also included a new syntax for a parameter list on the type itself, e.g.</span></span>

```C#
class Point(int X, int Y);
```

<span data-ttu-id="06cad-165">Yeni tasarımda parametre listesi, kayıtlar ile düzgün bir şekilde tümleştirilebilen, C# dikgen bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="06cad-165">In the new design, the parameter list would be an orthogonal C# feature, which could be cleanly integrated with records.</span></span> <span data-ttu-id="06cad-166">Birincil Oluşturucu bir kayda dahil ise, yalnızca ortak alanlar ve otomatik özellikler gibi yeni varsayılan değerlere sahip olur: birincil oluşturucudaki parametreler aynı ada sahip ortak kayıt-üye özellikleri oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="06cad-166">If a primary constructor is included in a record, it would have new defaults, just like public fields and auto-properties: the parameters in the primary constructor would be used to generate public record-member properties with the same name.</span></span> <span data-ttu-id="06cad-167">Ayrıca, birincil Oluşturucu artık bir Deconstructor otomatik olarak oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="06cad-167">In addition, the primary constructor could now be used to auto-generate a deconstructor.</span></span>

<span data-ttu-id="06cad-168">Örneğin, birincil Oluşturucusu olan aşağıdaki kayıt</span><span class="sxs-lookup"><span data-stu-id="06cad-168">For example, the following record with a primary constructor</span></span>

```C#
data class Point(int X, int Y);
```

<span data-ttu-id="06cad-169">eşdeğer olacaktır</span><span class="sxs-lookup"><span data-stu-id="06cad-169">would be equivalent to</span></span>

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

<span data-ttu-id="06cad-170">ve yukarıdaki son nesil</span><span class="sxs-lookup"><span data-stu-id="06cad-170">and the final generation of the above would be</span></span>

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

<span data-ttu-id="06cad-171">Birincil oluşturucuya sahip bir veri sınıfı için başka bir bilgi parçasını hesaba geçirdik. oluşturulan korumalı oluşturucunun içindeki birincil alanları ayarlamak yerine, birincil oluşturucuya temsilci seçiyoruz.</span><span class="sxs-lookup"><span data-stu-id="06cad-171">Note that we've taken one other piece of information into account for a data class with a primary constructor: instead of setting the primary fields inside the generated protected constructor, we delegate to the primary constructor.</span></span> <span data-ttu-id="06cad-172">Point sınıfının birincil olmayan başka bir kayıt üyesi varsa, ör.</span><span class="sxs-lookup"><span data-stu-id="06cad-172">If the Point class had another non-primary record member, e.g.</span></span>

```C#
data class Point(int X, int Y)
{
    public int Z { get; }
}
```

<span data-ttu-id="06cad-173">ardından, oluşturulan korumalı oluşturucuyu aşağıdaki gibi değiştirir:</span><span class="sxs-lookup"><span data-stu-id="06cad-173">then that would change the generated protected constructor as follows:</span></span>

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

<span data-ttu-id="06cad-174">Özellikle, bu, birincil oluşturucularla kayıtların devralınması hakkında ne yapılacağını yanıtlayamıyor.</span><span class="sxs-lookup"><span data-stu-id="06cad-174">Notably, this doesn't answer what to do about inheritance of records with primary constructors.</span></span> <span data-ttu-id="06cad-175">Örneğin,</span><span class="sxs-lookup"><span data-stu-id="06cad-175">For instance,</span></span>

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A;
```

<span data-ttu-id="06cad-176">Rastgele bir şekilde çözümlemek yerine, daha açık bir yaklaşım temel liste ile bir parametre listesinin sağlanmasını gerektirebilir, örn.</span><span class="sxs-lookup"><span data-stu-id="06cad-176">Rather than resolving in an arbitrary manner, a more explicit approach could require that a parameter list be provided with the base list, e.g.</span></span>

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A(X, Y);
```

<span data-ttu-id="06cad-177">Temel listedeki parametre listesi daha sonra oluşturulan birincil oluşturucuda bir `base` çağrısına uygulanır:</span><span class="sxs-lookup"><span data-stu-id="06cad-177">The parameter list in the base list would then be applied to a `base` call in the generated primary constructor:</span></span>

```C#
class B
{
    // ..
    public B(int x, int y, int z)
    : base(x, y)
    // ..
}
```

<span data-ttu-id="06cad-178">Birincil oluşturucunun bir kaydın dışından ne kadar önemli olduğu gibi, bu da daha fazla teklife açık olmaya devam eder.</span><span class="sxs-lookup"><span data-stu-id="06cad-178">As for what a primary constructor could mean outside of a record, that is still open to further proposal.</span></span>
