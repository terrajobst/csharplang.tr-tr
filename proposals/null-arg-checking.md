---
ms.openlocfilehash: 76065293f652979ab395e131d657e44899c5a65b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484554"
---
# <a name="simplified-null-argument-checking"></a>Basitleştirilmiş null bağımsız değişken denetimi

## <a name="summary"></a>Özet
Bu teklif, yöntem bağımsız değişkenlerinin doğrulanması için basitleştirilmiş bir sözdizimi sağlar `null` ve `ArgumentNullException` uygun şekilde oluşturmamalıdır.

## <a name="motivation"></a>Amacı
Null yapılabilir başvuru türleri tasarlamada yapılan iş, `null` bağımsız değişken doğrulaması için gereken kodu incelemektir. Bu, NRT 'nin kod yürütme geliştiricilerinin etkilemediği, tamamen `null` Temizleme olan projelerde bile `if (arg is null) throw` Boiler levha kodu eklemesi gerekir. Bu, dilde `null` doğrulaması için en az bir sözdizimi keşfetmeye yönelik bir değer vermiştir. 

Bu `null` parametre doğrulama sözdiziminin, NRT ile sıklıkla eşleştirilme beklenirken, teklif bundan tamamen bağımsızdır. Sözdizimi `#nullable` yönergelerinden bağımsız olarak kullanılabilir.

## <a name="detailed-design"></a>Ayrıntılı tasarım 

### <a name="null-validation-parameter-syntax"></a>Null doğrulama parametresi sözdizimi
Vurma işleci `!`, bir parametre listesindeki bir parametre adından sonra konumlandırılabilir ve bu parametre için C# derleyicinin standart `null` denetim kodunu yaymasına neden olur. Bu, `null` doğrulama parametresi sözdizimi olarak adlandırılır. Örneğin:

``` csharp
void M(string name!) {
    ...
}
```

Şu şekilde çevrilecek:

``` csharp
void M(string name) {
    if (name is null) {
        throw new ArgumentNullException(nameof(name));
    }
    ...
}
```

Oluşturulan `null` denetimi, herhangi bir geliştirici koddan önce oluşur. Birden çok parametre `!` işlecini içerdiğinde, parametreler bildirildiği sırada denetimler de gerçekleşir.

``` csharp
void M(string p1, string p2) {
    if (p1 is null) {
        throw new ArgumentNullException(nameof(p1));
    }
    if (p2 is null) {
        throw new ArgumentNullException(nameof(p2));
    }
    ...
}
```

Denetim, `null`için özel olarak başvuru eşitliği olacak, `==` veya Kullanıcı tanımlı işleçleri çağırmaz. Bu ayrıca `!` işlecinin yalnızca, `null`karşı eşitlik için test edilebilir parametrelere eklenebileceği anlamına gelir. Bu, türü değer türü olarak bilinen bir parametre üzerinde kullanılamayan anlamına gelir.

``` csharp
// Error: Cannot use ! on parameters who types derive from System.ValueType
void G<T>(T arg!) where T : struct {

}
```

Bir Oluşturucu söz konusu olduğunda `null` doğrulama, kurucudaki diğer koddan önce oluşur. Şunları içerir: 

- `this` veya `base` ile diğer oluşturuculara zincir oluşturma 
- Oluşturucuya örtük olarak oluşan alan başlatıcıları

Örneğin:

``` csharp
class C {
    string field = GetString();
    C(string name!): this(name) {
        ...
    }
}
```

, Kabaca şu şekilde çevrilecek:

``` csharp
class C {
    C(string name)
        if (name is null) {
            throw new ArgumentNullException(nameof(name));
        }
        field = GetString();
        :this(name);
        ...
}
```

Not: Bu yasal C# bir kod değildir, ancak yalnızca uygulamanın ne yaptığını bir yaklaşık olarak karşılarsınız. 

`null` doğrulama parametresi sözdizimi, lambda parametresi listelerinde de geçerli olacaktır. Bu, parleştirir olmayan tek parametre sözdiziminde bile geçerlidir.

``` csharp
void G() {
    // An identity lambda which throws on a null input
    Func<string, string> s = x! => x;
}
```

Sözdizimi Ayrıca Yineleyici yöntemlerine yönelik parametrelerde de geçerlidir. Yineleyicinizdeki diğer koddan farklı olarak, yineleyici yöntemi çağrıldığında, temeldeki Numaralandırıcı celenmiş olmadığında değil, `null` doğrulaması oluşur. Bu, geleneksel veya `async` yineleyiciler için geçerlidir.

``` csharp
class Iterators {
    IEnumerable<char> GetCharacters(string s!) {
        foreach (var c in s) {
            yield return c;
        }
    }

    void Use() {
        // The invocation of GetCharacters will throw
        IEnumerable<char> e = GetCharacters(null);
    }
}
```

`!` işleci yalnızca ilişkili yöntem gövdesine sahip olan parametre listeleri için kullanılabilir. Bu, bir `abstract` yöntemde, `interface`, `delegate` veya `partial` yöntem tanımında kullanılamayan anlamına gelir.

### <a name="extending-is-null"></a>Genişletme null
`is null` ifadesi geçerli olan türler, kısıtlanmış olmayan tür parametreleri içerecek şekilde genişletilir. Bu, bir `null` denetiminin geçerli olduğu tüm türlerde `null` denetleme amacını doldurmasına izin verir. Özellikle, kesin olarak değer türleri olmak üzere bilinen türlerdir. Örneğin, `struct` kısıtlı olan tür parametreleri bu söz dizimi ile kullanılamaz.

``` csharp
void NullCheck<T1, T2>(T1 p1, T2 p2) where T2 : struct {
    // Okay: T1 could be a class or struct here.
    if (p1 is null) {
        ...
    }

    // Error 
    if (p2 is null) { 
        ...
    }
}
```

Bir tür parametresindeki `is null` davranışı bugün `== null` aynı olacaktır. Tür parametresinin değer türü olarak örneklendiği durumlarda, kod `false`olarak değerlendirilir. Bir başvuru türü olduğu durumlarda kod, uygun bir `is null` denetimi olur.

### <a name="intersection-with-nullable-reference-types"></a>Null yapılabilir başvuru türleriyle kesişim
Bir `!` işlecinin adına uygulanmış olduğu herhangi bir parametre, null yapılabilir durum `null`olarak başlatılır. Bu, parametrenin kendisinin türü potansiyel olarak `null`olsa da geçerlidir. Bu, `string?`gibi açıkça null yapılabilir bir tür ile veya kısıtlanmış olmayan bir tür parametresiyle meydana gelebilir. 

Parametrelerde `!` söz dizimi parametre üzerinde açıkça null yapılabilir bir tür ile birleştirildiğinde, derleyici tarafından bir uyarı verilir:

``` csharp
void WarnCase<T>(
    string? name!, // Warning: combining explicit null checking with a nullable type
    T value1 // Okay
)
```

## <a name="open-issues"></a>Açık sorunlar
Yok

## <a name="considerations"></a>Dikkat edilmesi gerekenler

### <a name="constructors"></a>Oluşturucular
Oluşturucular için kod üretimi küçük bir, ancak bir standart `null` doğrulamadan ve `null` doğrulama parametresi söz dizimine (`!`) geçiş yaparken bir observable ve davranış değişikliği anlamına gelir. Standart doğrulama `null` denetimi hem alan başlatıcıları hem de `base` veya `this` çağrılarından sonra oluşur. Bu, bir geliştiricinin `null` doğrulamasının %100 ' i yeni sözdizimine geçiremeyeceği anlamına gelir. Oluşturucular en az bir inceleme gerektirir.

Tartışmadan sonra, önemli benimseme sorunlarına neden olması çok düşüktür. Kurucudaki herhangi bir mantığdan önce `null` denetiminin çalıştırılması daha mantıklı. Önemli uyumluluk sorunları tespit edildiğinde tekrar tekrar bulunabilir.

### <a name="warning-when-mixing--and-"></a>Karıştırdığınızda uyarı mı? '!
`!` sözdizimi, açıkça null olabilen bir tür olarak yazılmış bir parametreye uygulandığında, uyarı verilip verilmeyeceğini belirten bir sorun olup olmadığını belirten uzun bir tartışma vardı. Bu yüzey, geliştirici tarafından sensik olmayan bir bildirim gibi görünüyor, ancak tür hiyerarşisinin geliştiricilere bu gibi bir duruma zorlabildiği durumlar vardır. 

Aşağıdaki sınıf hiyerarşisini bir dizi derleme genelinde göz önünde bulundurun (tümünün `null` denetlemesi etkinleştirilmiş olarak derlendiğini varsayarak):

``` csharp
// Assembly1
abstract class C1 {
    protected abstract void M(object o); 
}

// Assembly2
abstract class C2 : C1 {

}

// Assembly3
abstract class C3 : C2 { 
    protected override void M(object o!) {
        ...
    }
}
```

Burada `C3` yazarı `o`parametreye `null` doğrulaması eklemeye karar verdi. Bu, özelliğin kullanım amacına göre tamamen bir satır içinde olur.

Şimdi, Assembly2 yazarı aşağıdaki geçersiz kılmayı eklemeye karar verdiğinde daha sonraki bir tarihte düşünün:

``` csharp
// Assembly2
abstract class C2 : C1 {
   protected override void M(object? o) { 
       ...
   }
}
```

Bu, sözleşmenin giriş konumlarına yönelik daha esnek hale getirilebileceği için, null yapılabilir başvuru türleri tarafından izin verilir. Genel olarak NRT özelliği, parametre veya dönüş null değer alabilme üzerinde makul ortak/değişken varyans sağlar. Ancak dil, özgün bildirime değil, en özel geçersiz kılma temelinde ortak/değişken varyans denetimi yapar. Bu, Assembly3 yazarı, eşleşmeyen `o` türü hakkında bir uyarı alacak ve bunu ortadan kaldırmak için imzayı aşağıdaki şekilde değiştirmesinin gerektiği anlamına gelir: 

``` csharp
// Assembly3
abstract class C3 : C2 { 
    protected override void M(object? o!) {
        ...
    }
}
```

Bu noktada, Assembly3 'in yazarı birkaç seçeneğe sahiptir:

- `object?` ve `object` uyumsuzluğu hakkında uyarı kabul edebilir/göstermez.
- `object?` ve `!` uyumsuzluğu hakkında uyarı kabul edebilir/göstermez.
- Yalnızca `null` doğrulama denetimini kaldırabilir (`!` silebilir ve açıkça denetim yapabilir)

Bu gerçek bir senaryodur ancak şimdilik, uyarı ile ileriye doğru ilerlemedir. Uyarı, daha sık öngörüldüğünde, daha sonra kaldırabiliriz (tersi doğru değildir).

### <a name="implicit-property-setter-arguments"></a>Örtük özellik ayarlayıcı bağımsız değişkenleri
Bir parametrenin `value` bağımsız değişkeni örtük ve herhangi bir parametre listesinde görünmez. Yani bu özelliğin hedefi olamaz. Özellik ayarlayıcı sözdizimi, `!` işlecinin uygulanmasına izin vermek için bir parametre listesi içerecek şekilde genişletilebilir. Ancak bu özellik, `null` doğrulamasını daha kolay hale getiren bu özelliğin fikrine karşı karşılaştırılır. Bu nedenle örtük `value` bağımsız değişkeni bu özellikle çalışmaz.

## <a name="future-considerations"></a>Gelecekteki konular
Yok
