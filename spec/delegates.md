---
ms.openlocfilehash: 08c14d9ef2afe30580f456995066c141653ede92
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229974"
---
# <a name="delegates"></a>Temsilciler

Temsilciler, diğer diller senaryoları etkinleştirmek — sahip işlev işaretçilerine, C++, Pascal ve Modula--gibi giderdik. C++ işlev işaretçilerinden farklı ancak temsilcileri nesne yönelimli tam olarak, ve hem nesne örneği hem de bir yöntem üye işlevlerinin C++ işaretçileri, temsilciler kapsüller.

Temsilci bildirimi sınıfından türetilen bir sınıf tanımlar `System.Delegate`. Bir temsilci örneği her biri için çağrılabilen bir varlık olarak adlandırılır yöntemleri, bir veya daha fazla listesi olan çağrı listesine kapsüller. Örneği için yöntemleri, örneği ve bir yöntem örneğine çağrılabilir varlık oluşur. Statik yöntemler için yalnızca bir yöntem çağrılabilir varlık oluşur. Uygun bir bağımsız değişkenler kümesiyle bir temsilci örneği çağırma her temsilcinin çağrılabilir varlıkların belirli bağımsız değişkenler kümesiyle çağrılmasına neden olur.

Bir ilgi çekici ve kullanışlı bir temsilci örneği bildirin veya desteklemez, kapsülleyen yöntemleri sınıfları hakkında önemli olduğunu özelliğidir; önemli olan bu yöntemleri uyumlu ([temsilci bildirimi](delegates.md#delegate-declarations)) temsilcinin türüne sahip. Bu temsilciler mükemmel "anonim" çağırma için uygun hale getirir.

## <a name="delegate-declarations"></a>Temsilci bildirimi

A *delegate_declaration* olduğu bir *type_declaration* ([tür bildirimleri](namespaces.md#type-declarations)), yeni bir temsilci türü bildirir.

```antlr
delegate_declaration
    : attributes? delegate_modifier* 'delegate' return_type
      identifier variant_type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;

delegate_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | delegate_modifier_unsafe
    ;
```

Aynı değiştiricisi bir temsilci bildiriminde birden çok kez görünmesi için bir derleme zamanı hatasıdır.

`new` Değiştiricisi temsilciler üzerinde yalnızca izin başka bir türde bildirilen, bu durumda, bu tür bir temsilci belirtir devralınmış bir üyeyi aynı adla açıklandığı gizler [yeni değiştiricisini](classes.md#the-new-modifier).

`public`, `protected`, `internal`, Ve `private` değiştiriciler temsilci türünün erişilebilirlik denetimi. Temsilci bildirimi oluştuğu bağlama bağlı olarak bu değiştiriciler bazıları değil izin verilir ([erişilebilirlik bildirilen](basic-concepts.md#declared-accessibility)).

Temsilcinin türü adı *tanımlayıcı*.

İsteğe bağlı *formal_parameter_list* temsilcinin parametre belirtir ve *Döndür_tür* temsilcinin dönüş türünü belirtir.

İsteğe bağlı *variant_type_parameter_list* ([değişken türü parametre listeleri](interfaces.md#variant-type-parameter-lists)) için temsilci tür parametreleri belirtir.

Dönüş türü bir temsilci türü olmalıdır `void`, veya çıkış için güvenli ([varyansı güvenliği](interfaces.md#variance-safety)).

Tüm biçimsel parametre türleri temsilci türü giriş uyumlu olması gerekir. Ayrıca, tüm `out` veya `ref` parametre türleri de çıkış uyumlu olmalıdır. Not, hatta `out` giriş güvenli bir temel yürütme platform sınırlaması nedeniyle olmasını gerekli parametreler.

C# ' temsilci türleri olan eşdeğer, yapısal olarak eşdeğer ad. Özellikle, aynı parametreye sahip iki farklı temsilci türleri listeler ve dönüş türü farklı bir temsilci türleri olarak kabul edilir. Ancak, iki farklı ancak yapısal olarak eşdeğer temsilci türleri örneklerini eşit olarak karşılaştırılır ([temsilci eşitlik işleçleri](expressions.md#delegate-equality-operators)).

Örneğin:

```csharp
delegate int D1(int i, double d);

class A
{
    public static int M1(int a, double b) {...}
}

class B
{
    delegate int D2(int c, double d);
    public static int M1(int f, double g) {...}
    public static void M2(int k, double l) {...}
    public static int M3(int g) {...}
    public static void M4(int g) {...}
}
```

Yöntemleri `A.M1` ve `B.M1 `her iki temsilci türleriyle uyumlu `D1` ve `D2` , sahip oldukları olduğundan aynı dönüş türü ve parametre listesi; ancak, bu temsilci türleri olmadıkları için iki farklı türleri olan değiştirilebilir. Yöntemleri `B.M2`, `B.M3`, ve `B.M4` temsilci türleriyle uyumsuz `D1` ve `D2`, parametre listeleri ya da farklı dönüş türlerine sahip olduğu.

Diğer genel tür bildirimleri gibi bir oluşturulmuş bir temsilci türü oluşturmak için tür bağımsız değişkenleri verilmelidir. Parametre türleri ve dönüş türü bir oluşturulmuş bir temsilci türü, her tür parametresi temsilci bildirimi için karşılık gelen tür bağımsız değişkeni oluşturulmuş bir temsilci türünün getirilmesiyle oluşturulur. Parametre türleri ve sonuçta elde edilen dönüş türünü belirlemede yöntemleri oluşturulmuş temsilci türüyle uyumlu kullanılır. Örneğin:

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

Yöntem `X.F` temsilci türüyle uyumlu `Predicate<int>` ve yöntem `X.G` temsilci türüyle uyumlu `Predicate<string>` .

Aracılığıyla bir temsilci türü bildirmek için tek yolu olan bir *delegate_declaration*. Bir temsilci türü türetilen bir sınıf türüdür `System.Delegate`. Örtük olarak temsilci türleri olan `sealed`, herhangi bir tür bir temsilci türünden türetilmesi döndürülmesine izin verilmez. Ayrıca bir temsilci olmayan sınıf türünden türetmek için izin verilen değil `System.Delegate`. Unutmayın `System.Delegate` kendisi bir temsilci türü; tüm temsilci türleri türetildiği bir sınıf türü bir değer.

C# özel sözdizimi için temsilci örnek oluşturma ve çağırma sağlar. Örnek oluşturma dışında bir sınıf veya sınıf örneği uygulanabilir herhangi bir işlem de bir temsilci sınıfı veya örneği, sırasıyla uygulanabilir. Özellikle, üyelere erişim mümkündür `System.Delegate` normal üye erişimi sözdizimini aracılığıyla türü.

Bir temsilci örneği tarafından kapsüllenmiş yöntem kümesi çağrı listesine çağrılır. Bir temsilci örneği oluşturulduğunda ([temsilci Uyumluluk](delegates.md#delegate-compatibility)) isteğe bağlı olarak tek bir yöntemi, bu yöntem kapsüller ve çağırma listesinde yalnızca bir giriş içeriyor. İki null olmayan temsilci örneklerini bir araya geldiğinde, ancak çağırma listelerini--iki veya daha fazla giriş içeren yeni bir çağırma listesi oluşturmak için işlenen sonra sağ işlenen--sol sırayla bitiştirilir.

Temsilciler, ikili kullanılarak birleştirilir `+` ([Toplama işleci](expressions.md#addition-operator)) ve `+=` işleçleri ([bileşik atama](expressions.md#compound-assignment)). Bir temsilci bir bileşimini kullanarak ikili temsilcilerin kaldırılabilir `-` ([çıkarma işleci](expressions.md#subtraction-operator)) ve `-=` işleçleri ([bileşik atama](expressions.md#compound-assignment)). Temsilciler, eşitlik için karşılaştırılabilir ([temsilci eşitlik işleçleri](expressions.md#delegate-equality-operators)).

Aşağıdaki örnek temsilcilerin sayısına örneğinin gösterir ve bunların karşılık gelen çağırma listeler:

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public static void M2(int i) {...}

}

class Test
{
    static void Main() {
        D cd1 = new D(C.M1);      // M1
        D cd2 = new D(C.M2);      // M2
        D cd3 = cd1 + cd2;        // M1 + M2
        D cd4 = cd3 + cd1;        // M1 + M2 + M1
        D cd5 = cd4 + cd3;        // M1 + M2 + M1 + M1 + M2
    }

}
```

Zaman `cd1` ve `cd2` olan örneği, her bir yöntem kapsüller. Zaman `cd3` olan örneği, iki yönteme bir çağrı listesine sahiptir `M1` ve `M2`bu sırası. `cd4`ın çağırma listesi içeren `M1`, `M2`, ve `M1`bu sırası. Son olarak, `cd5`'s çağırma listesi içeren `M1`, `M2`, `M1`, `M1`, ve `M2`bu sırası. Temsilciler (de olarak kaldırma) birleştirerek daha fazla örnek için bkz: [temsilci çağırma](delegates.md#delegate-invocation).

## <a name="delegate-compatibility"></a>Temsilci uyumluluğu

Bir yöntem veya temsilci `M` olduğu ***uyumlu*** bir temsilci türüyle `D` aşağıdakilerin tümü doğru olduğunda:

*  `D` ve `M` parametreleri ve her parametresinde aynı sayıda `D` aynı `ref` veya `out` karşılık gelen parametre olarak değiştiriciler `M`.
*  Her değer parametresi için (parametre olmadan `ref` veya `out` değiştiricisi), bir kimlik dönüştürme ([kimlik dönüştürme](conversions.md#identity-conversion)) ya da örtük bir başvuru dönüştürmesi ([örtükbirbaşvurudönüşümleri](conversions.md#implicit-reference-conversions)) parametre türünden var. `D` karşılık gelen parametre türüne `M`.
*  Her `ref` veya `out` içinde tür parametresi, parametre `D` parametre türü aynı `M`.
*  Dönüş türünü bir kimlik veya örtük bir başvuru dönüştürmesi var `M` dönüş türüne `D`.

## <a name="delegate-instantiation"></a>Temsilci örneği oluşturma

Bir temsilci örneği tarafından oluşturulan bir *delegate_creation_expression* ([temsilci oluşturma ifadeleri](expressions.md#delegate-creation-expressions)) veya bir temsilci türüne dönüştürme. Yeni oluşturulan temsilci örneği sonra ya da ifade eder:

*  Başvurulan statik yöntem *delegate_creation_expression*, veya
*  Hedef nesne (olamaz `null`) ve örnek yöntemi içinde başvurulan *delegate_creation_expression*, veya
*  Başka bir temsilci.

Örneğin:

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public void M2(int i) {...}
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);        // static method
        C t = new C();
        D cd2 = new D(t.M2);        // instance method
        D cd3 = new D(cd2);        // another delegate
    }
}
```

Oluşturulduktan sonra temsilci örneklerini her zaman aynı hedef nesne ve yöntem bakın. Unutmayın, ne zaman iki temsilci olduğunda birleştirilir veya bir başka bir yeni bir temsilci sonuçları kendi çağırma listesiyle kaldırılır; Birleştirilen ya da kaldırılmış temsilcileri çağırma listelerini değişmeden kalır.

## <a name="delegate-invocation"></a>Temsilci çağrısı

C# bir temsilci çağırma için özel bir sözdizimi sağlar. Bir null olmayan temsilci örneği olan çağrı listesine bir giriş içerir çağrıldığında verildi ve başvurulan aynı değeri döndürür aynı bağımsız değişkenlerle bir yöntem çağırdığı için yöntemi. (Bkz [temsilci çağrılarını](expressions.md#delegate-invocations) temsilci çağrısı hakkında ayrıntılı bilgi için.) Bu yöntem doğrudan adında gibi bir temsilcinin çağırma sırasında bir özel durum oluşur ve çağrıldı yöntemi içinde özel durum yakalandı, arama gibi özel durum yakalama yan tümcesi için temsilci çağıran yöntemi devam eder yöntemi, temsilci olarak adlandırılır.

Çağrı, çağırma listesinde birden fazla varlık içeriyor. bir temsilci örneği, çağırma listesinde, yöntemlerin her biri zaman uyumlu olarak, sırasıyla çağırarak devam eder. Yorumun her yöntem için temsilci örneği sağlanmadı olarak bağımsız değişkenler aynı dizi geçirilir. Bu tür bir temsilci çağrısı başvuru parametreleri içeriyorsa ([başvuru parametreleri](classes.md#reference-parameters)), her yöntem çağırma aynı değişken başvuru ile meydana gelir; bu değişkeni bir yöntem çağırma listesinde tarafından yapılan olacaktır çağırma listeyi daha ayrıntılı yöntemler için görünür. Temsilci çağrısı çıkış parametreleri veya dönüş değeri içeriyorsa, bunların son değerini listedeki son temsilcinin çağırma kaynağından gelir.

Bu tür bir temsilci çağrısı işlenirken bir özel durum oluşur ve çağrıldı yöntemi içinde özel durum yakalandı, temsilci çağrılan yöntemi bir özel durum yakalama yan tümcesi için arama işlemi devam eder ve herhangi bir yöntem daha da aşağı değil çağırma listesi çağrılır.

Türünde bir özel durum'içinde null sonuç değeri olan bir temsilci örneği çağırmaya çalışırken `System.NullReferenceException`.

Aşağıdaki örnek, örneği, birleştirmek, kaldırmak ve temsilci çağırma gösterilmektedir:

```csharp
using System;

delegate void D(int x);

class C
{
    public static void M1(int i) {
        Console.WriteLine("C.M1: " + i);
    }

    public static void M2(int i) {
        Console.WriteLine("C.M2: " + i);
    }

    public void M3(int i) {
        Console.WriteLine("C.M3: " + i);
    }
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);
        cd1(-1);                // call M1

        D cd2 = new D(C.M2);
        cd2(-2);                // call M2

        D cd3 = cd1 + cd2;
        cd3(10);                // call M1 then M2

        cd3 += cd1;
        cd3(20);                // call M1, M2, then M1

        C c = new C();
        D cd4 = new D(c.M3);
        cd3 += cd4;
        cd3(30);                // call M1, M2, M1, then M3

        cd3 -= cd1;             // remove last M1
        cd3(40);                // call M1, M2, then M3

        cd3 -= cd4;
        cd3(50);                // call M1 then M2

        cd3 -= cd2;
        cd3(60);                // call M1

        cd3 -= cd2;             // impossible removal is benign
        cd3(60);                // call M1

        cd3 -= cd1;             // invocation list is empty so cd3 is null

        cd3(70);                // System.NullReferenceException thrown

        cd3 -= cd1;             // impossible removal is benign
    }
}
```

İfadede gösterildiği `cd3 += cd1;`, bir temsilci birden çok kez bir çağırma listesinde var olabilir. Bu durumda, yalnızca bir kez örneği çağrılır. Bu temsilci kaldırıldığında, bunun gibi bir çağırma listesinde çağırma listedeki son oluşum gerçekten kaldırıldı bir bileşendir.

Son deyiminin yürütülmeden hemen önce `cd3 -= cd1;`, temsilci `cd3` bir boş çağırma listesine başvuruyor. Bir temsilci boş listeden kaldırmak için (veya var olmayan bir temsilci boş listeden kaldırmak için) çalışırken bir hata değildir.

Oluşturulan çıktı.

```
C.M1: -1
C.M2: -2
C.M1: 10
C.M2: 10
C.M1: 20
C.M2: 20
C.M1: 20
C.M1: 30
C.M2: 30
C.M1: 30
C.M3: 30
C.M1: 40
C.M2: 40
C.M3: 40
C.M1: 50
C.M2: 50
C.M1: 60
C.M1: 60
```
