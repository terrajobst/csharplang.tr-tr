---
ms.openlocfilehash: d162d4b6a489032dcdfca9094a39d88fd03d4013
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704095"
---
# <a name="delegates"></a>Temsilciler

Temsilciler C++, diğer dillerin (örneğin, Pascal ve modüla) işlev işaretçileriyle ilgilenmesini sağlayan senaryolara olanak tanır. Ancak C++ , işlev işaretçilerinden farklı olarak, temsilciler tam nesne yönelimlidir ve C++ üye işlevlerine yönelik işaretçilerin aksine, temsilciler hem bir nesne örneğini hem de bir yöntemi kapsüller.

Temsilci bildirimi `System.Delegate` sınıfından türetilmiş bir sınıfı tanımlar. Bir temsilci örneği, her biri çağrılabilir bir varlık olarak adlandırılan bir veya daha fazla yöntemin listesi olan bir çağırma listesini kapsüller. Örnek yöntemleri için, çağrılabilir bir varlık bir örnekten ve bu örnekteki yöntemden oluşur. Statik yöntemler için, çağrılabilir bir varlık yalnızca bir yöntemden oluşur. Uygun bir bağımsız değişken kümesiyle bir temsilci örneği çağırmak, her bir temsilcinin çağrılabilir varlıkların belirtilen bağımsız değişken kümesiyle çağrılmasına neden olur.

Bir temsilci örneğinin ilginç ve yararlı bir özelliği, kapsülleyen yöntemlerin sınıflarını bilmiyor veya ilgilenmez; Bu yöntemlerin tümünün, temsilcinin türüyle uyumlu olması ([temsilci bildirimleri](delegates.md#delegate-declarations)). Bu, temsilcileri "anonim" çağrı için mükemmel bir hale getirir.

## <a name="delegate-declarations"></a>Temsilci bildirimleri

Bir *delegate_declaration* , yeni bir temsilci türü bildiren bir *type_declaration* ([tür bildirimleri](namespaces.md#type-declarations)).

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

Aynı değiştiricinin bir temsilci bildiriminde birden çok kez görünmesi için derleme zamanı hatası vardır.

@No__t-0 değiştiricisine yalnızca başka bir tür içinde belirtilen Temsilcilerde izin verilir. Bu durumda, bu tür bir temsilcinin [Yeni değiştiricide](classes.md#the-new-modifier)açıklandığı gibi aynı ada sahip bir devralınmış üyeyi gizlediğini belirtir.

@No__t-0, `protected`, `internal` ve `private` değiştiricileri temsilci türünün erişilebilirliğini denetler. Temsilci bildiriminin gerçekleştiği içeriğe bağlı olarak, bu değiştiricilerin bazılarına izin verilmeyebilir ([belirtilen erişilebilirlik](basic-concepts.md#declared-accessibility)).

Temsilcinin tür adı *tanımlayıcıdır*.

İsteğe bağlı *formal_parameter_list* , temsilcinin parametrelerini belirtir ve *Sbayrak* , temsilcinin dönüş türünü gösterir.

İsteğe bağlı *variant_type_parameter_list* ([değişken türü parametre listeleri](interfaces.md#variant-type-parameter-lists)), temsilcinin kendisi için tür parametrelerini belirtir.

Bir temsilci türünün dönüş türü `void` ya da çıkış açısından güvenli ([varyans güvenliği](interfaces.md#variance-safety)) olmalıdır.

Bir temsilci türünün tüm biçimsel parametre türleri giriş açısından güvenli olmalıdır. Ayrıca, `out` veya `ref` parametre türlerinin Ayrıca çıkış açısından güvenli olması gerekir. Temel yürütme platformunun sınırlamasından dolayı, `out` parametrelerinin de giriş açısından güvenli olması gerektiğini unutmayın.

İçindeki C# temsilci türleri, yapısal olarak eşdeğer değil, ad eşdeğerdedir. Özellikle, aynı parametre listelerine ve dönüş türüne sahip iki farklı temsilci türü farklı temsilci türleri olarak değerlendirilir. Ancak, iki ayrı ancak yapısal olarak eşdeğer temsilci türünün örnekleri eşit ([temsilci eşitlik işleçleri](expressions.md#delegate-equality-operators)) olarak karşılaştırılabilir.

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

@No__t-0 ve `B.M1` yöntemleri aynı dönüş türü ve parametre listesine sahip oldukları için hem `D1` hem de `D2` temsilci türleriyle uyumludur; Bununla birlikte, bu temsilci türleri iki farklı türlerdir, bu nedenle bunlar birbirinin yerine kullanılamaz. @No__t-0, `B.M3` ve `B.M4` yöntemleri farklı dönüş türlerine veya parametre listelerine sahip olduklarından, `D1` ve `D2` temsilci türleriyle uyumsuzdur.

Diğer genel tür bildirimleri gibi, oluşturulmuş bir temsilci türü oluşturmak için tür bağımsız değişkenleri verilmelidir. Oluşturulmuş bir temsilci türünün parametre türleri ve dönüş türü, temsilci bildirimindeki her bir tür parametresi için yerine, oluşturulan temsilci türünün karşılık gelen tür bağımsız değişkeni ile oluşturulur. Elde edilen dönüş türü ve parametre türleri, oluşturulan bir temsilci türüyle hangi yöntemlerin uyumlu olduğunu belirlemek için kullanılır. Örneğin:

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

@No__t-0 yöntemi, `Predicate<int>` temsilci türüyle uyumludur ve `X.G` yöntemi `Predicate<string>` temsilci türüyle uyumludur.

Bir temsilci türü bildirmenin tek yolu bir *delegate_declaration*aracılığıyla yapılır. Temsilci türü `System.Delegate` ' dan türetilen bir sınıf türüdür. Temsilci türleri örtük olarak `sealed` olduğundan, temsilci türünden herhangi bir tür türetmeye izin verilmez. Ayrıca, `System.Delegate` ' dan temsilci olmayan bir sınıf türü türetmeye izin verilmez. @No__t-0 ' ın kendisi bir temsilci türü değil; Bu, tüm temsilci türlerinin türetildiği bir sınıf türüdür.

C#temsilci örneklemesi ve çağrısı için özel sözdizimi sağlar. Örnek oluşturma haricinde, bir sınıfa veya sınıf örneğine uygulanabilen her türlü işlem sırasıyla temsilci sınıfına veya örneğe de uygulanabilir. Özellikle, `System.Delegate` türünde üyelere olağan üye erişim sözdizimi aracılığıyla erişmek mümkündür.

Bir temsilci örneğiyle kapsüllenmiş yöntemlerin kümesine çağrı listesi denir. Tek bir yöntemden bir temsilci örneği oluşturulduğunda ([temsilci uyumluluğu](delegates.md#delegate-compatibility)), bu yöntemi kapsüller ve çağırma listesi yalnızca bir giriş içerir. Bununla birlikte, iki null olmayan temsilci örneği birleştirildiğinde, iki veya daha fazla giriş içeren yeni bir çağırma listesi oluşturmak için, bu, iki veya daha fazla girişi içeren yeni bir çağırma listesi oluşturmak üzere, çağrı listeleri birleştirilir.

Temsilciler `+` ([toplama işleci](expressions.md#addition-operator)) ve `+=` Işleçleri ([bileşik atama](expressions.md#compound-assignment)) kullanılarak birleştirilir. Bir temsilci, `-` ([çıkarma işleci](expressions.md#subtraction-operator)) ve `-=` Işleçleri ([bileşik atama](expressions.md#compound-assignment)) kullanılarak bir temsilciler birleşiminden kaldırılabilir. Temsilciler eşitlik için karşılaştırılabilir ([temsilci eşitlik işleçleri](expressions.md#delegate-equality-operators)).

Aşağıdaki örnekte, bir dizi temsilciyi ve bunlara karşılık gelen çağrı listelerini gösteren örnek gösterilmektedir:

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

@No__t-0 ve `cd2` örneği oluşturulduğunda, her biri bir yöntemi kapsüller. @No__t-0 örneği oluşturulduğunda, iki yöntemin çağırma listesine sahiptir, bu sırayla `M1` ve `M2`. `cd4` ' ın çağrı listesi, bu sırayla `M1`, `M2` ve `M1` içerir. Son olarak, `cd5` ' ın çağrı listesi bu sırayla `M1`, `M2`, `M1`, `M1` ve `M2` ' i içerir. Temsilcileri birleştirme (ve kaldırmanın yanı sıra) hakkında daha fazla örnek için bkz. [temsilci çağrısı](delegates.md#delegate-invocation).

## <a name="delegate-compatibility"></a>Temsilci uyumluluğu

@No__t-0 bir yöntem veya temsilci, aşağıdakilerin tümü doğru ise, `D` bir temsilci türüyle ***uyumludur*** :

*  `D` ve `M` aynı sayıda parametreye sahiptir ve `D` ' deki her bir parametre `M` ' teki karşılık gelen parametreyle aynı `ref` veya `out` değiştiricilerine sahiptir.
*  Her değer parametresi için (`ref` veya `out` değiştiricisi olmayan bir parametre), `D` ' teki parametre türünden bir kimlik dönüştürme ([kimlik dönüştürme](conversions.md#identity-conversion)) veya örtük başvuru dönüştürmesi ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions)) var `M` ' teki karşılık gelen parametre türü.
*  Her `ref` veya `out` parametresi için, `D` ' deki parametre türü `M` ' teki parametre türü ile aynıdır.
*  @No__t-0 dönüş türünden `D` dönüş türüne bir kimlik veya örtük başvuru dönüştürmesi var.

## <a name="delegate-instantiation"></a>Temsilci oluşturma

Temsilcinin bir örneği, bir *delegate_creation_expression* ([temsilci oluşturma ifadeleri](expressions.md#delegate-creation-expressions)) veya bir temsilci türüne dönüştürme tarafından oluşturulur. Yeni oluşturulan temsilci örneği bundan sonra şu şekilde başvurur:

*  *Delegate_creation_expression*içinde başvurulan statik yöntem veya
*  Hedef nesne (`null` olamaz) ve *delegate_creation_expression*içinde başvurulan örnek yöntemi
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

Örneği oluşturulduktan sonra, temsilci örnekleri her zaman aynı hedef nesne ve yönteme başvurur. İki temsilci birleştirildiğinde ya da biri diğerinden kaldırıldığında, yeni bir temsilcinin kendi çağrı listesi ile sonuçlandığını unutmayın; Birleşik veya kaldırılan temsilcilerin çağırma listeleri değişmeden kalır.

## <a name="delegate-invocation"></a>Temsilci çağırma

C#bir temsilciyi çağırmak için özel bir sözdizimi sağlar. Çağırma listesi bir giriş içeren null olmayan bir temsilci örneği çağrıldığında, aynı bağımsız değişkenlerle bir yöntemi çağırır ve başvurulan yöntemiyle aynı değeri döndürür. (Temsilci çağırma hakkında ayrıntılı bilgi için bkz. [temsilci etkinleştirmeleri](expressions.md#delegate-invocations) .) Bu tür bir temsilcinin çağrılması sırasında bir özel durum oluşursa ve bu özel durum çağrılan yöntem içinde yakalanmadığında, özel durum catch yan tümcesi için arama, bu yöntemin doğrudan adı Bu temsilcinin başvurduğu yöntem.

Çağırma listesi birden çok giriş içeren bir temsilci örneğinin çağrılması, çağrı listesindeki yöntemlerin her birini zaman uyumlu olarak sırayla çağırarak ilerler. Çağrısı yapılan her yöntem, temsilci örneğine verilen olarak aynı bağımsız değişken kümesine geçirilir. Bu tür bir temsilci çağrısı başvuru parametreleri ([başvuru parametreleri](classes.md#reference-parameters)) içeriyorsa, her yöntem çağrısı aynı değişkene bir başvuru ile gerçekleşir; çağırma listesindeki bir yönteme göre söz konusu değişkende yapılan değişiklikler, çağrı listesinin daha aşağı bir yöntemi olarak görünür olacaktır. Temsilci çağrısı çıkış parametreleri veya dönüş değeri içeriyorsa, son değeri listedeki son temsilcinin çağrısından gelir.

Böyle bir temsilcinin çağrılması sırasında bir özel durum oluşursa ve bu özel durum çağrılan yöntem içinde yakalanmadığında, özel durum catch yan tümcesi için arama, temsilciyi çağıran yönteme ve diğer yöntemlere devam eder çağırma listesi çağrılmıyor.

Değeri null olan bir temsilci örneği çağrılmaya çalışılıyor `System.NullReferenceException` türünde bir özel durum oluşur.

Aşağıdaki örnek, temsilcileri oluşturma, birleştirme, kaldırma ve çağırma işlemlerinin nasıl yapılacağını göstermektedir:

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

@No__t-0 ifadesinde gösterildiği gibi, bir temsilci bir çağırma listesinde birden çok kez bulunabilir. Bu durumda, her oluşum için yalnızca bir kez çağrılır. Bunun gibi bir çağırma listesinde, bu temsilci kaldırıldığında, aslında kaldırılan çağrı listesindeki son oluşum, aslında kaldırılmış bir şeydir.

Son deyimin yürütmeden hemen önce, `cd3 -= cd1;`, `cd3` temsilcisi boş bir çağırma listesine başvurur. Bir temsilciyi boş bir listeden kaldırma girişimi (veya varolmayan bir temsilciyi boş olmayan bir listeden kaldırmak için) bir hata değildir.

Oluşturulan çıkış:

```console
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
