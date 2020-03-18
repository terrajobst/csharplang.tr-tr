---
ms.openlocfilehash: d9080202f9413f8beb80db222d47f5fc082ae641
ms.sourcegitcommit: f3170512e7a3193efbcea52ec330648375e36915
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79485506"
---
# <a name="function-pointers"></a>İşlev İşaretçileri

## <a name="summary"></a>Özet

Bu teklif, şu anda etkin şekilde erişilemeyen Il işlem kodları 'ları, bugün içinde C# `ldftn` ve `calli`. Bu Il Opcode 'ları yüksek performans kodunda önemli olabilir ve geliştiricilerin bunlara erişmek için etkili bir yol olması gerekir.

## <a name="motivation"></a>Amacı

Bu özelliğin ilgi çekici ve arka planı, aşağıdaki konuda açıklanmaktadır (özelliğin olası bir uygulamasıdır):

https://github.com/dotnet/csharplang/issues/191

Bu, [derleyici iç](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md) bilgileri için alternatif bir tasarım teklifinde bulunur

## <a name="detailed-design"></a>Ayrıntılı tasarım

### <a name="function-pointers"></a>İşlev işaretçileri

Dil, `delegate*` sözdizimini kullanarak işlev işaretçilerinin bildirimine izin verir. Tam sözdizimi, sonraki bölümde ayrıntılı olarak açıklanmıştır, ancak `Func` ve `Action` tür bildirimleri tarafından kullanılan sözdizimine benzer şekilde tasarlanmıştır.

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

Bu türler, ECMA-335 ' de özetlenen işlev işaretçisi türü kullanılarak temsil edilir. Bu, bir `delegate*` çağrısı, bir `delegate` çağırmanın `Invoke` yönteminde `callvirt` kullanacağı `calli` kullanacağı anlamına gelir.
Sözdizimi, her iki yapı için de özdeş.

Yöntem işaretçilerinin ECMA-335 tanımı, tür imzasının bir parçası olarak çağırma kuralını içerir (Bölüm 7,1).
Varsayılan çağırma kuralı `managed`olacaktır. `delegate*` sözdiziminden sonra uygun değiştirici eklenerek alternatif formlar belirtilebilir: `managed`, `cdecl`, `stdcall`, `thiscall`veya `unmanaged`. Örnek:

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

`delegate*` türler arasındaki dönüştürmeler, çağırma kuralı dahil olmak üzere imzalarına göre yapılır.

``` csharp
unsafe class Example {
    void Conversions() {
        delegate*<int, int, int> p1 = ...;
        delegate* managed<int, int, int> p2 = ...;
        delegate* cdecl<int, int, int> p3 = ...;

        p1 = p2; // okay p1 and p2 have compatible signatures
        Console.WriteLine(p2 == p1); // True
        p2 = p3; // error: calling conventions are incompatible
    }
}
```

`delegate*` türü, standart bir işaretçi türünün tüm özelliklerine ve kısıtlamalarına sahip olduğu anlamına gelen bir işaretçi türüdür:

- Yalnızca bir `unsafe` bağlamında geçerlidir.
- Bir `delegate*` parametresi veya dönüş türü içeren yöntemler yalnızca bir `unsafe` bağlamından çağrılabilir.
- `object`dönüştürülemiyor.
- Genel bağımsız değişken olarak kullanılamaz.
- `delegate*` `void*`örtülü olarak dönüştürebilir.
- `void*`, açıkça `delegate*`olarak dönüştürebilir.

Larından

- Özel öznitelikler bir `delegate*` veya öğelerinden hiçbirine uygulanamaz.
- Bir `delegate*` parametresi `params` olarak işaretlenemez
- Bir `delegate*` türü normal bir işaretçi türünün tüm kısıtlamalarına sahiptir.

### <a name="function-pointer-syntax"></a>İşlev işaretçisi sözdizimi

Tam işlev işaretçisi sözdizimi aşağıdaki dilbilgisinde temsil edilir:

```antlr
pointer_type
    : ...
    | funcptr_type
    ;

funcptr_type
    : 'delegate' '*' calling_convention? '<' (funcptr_parameter_modifier? type ',')* funcptr_return_modifier? return_type '>'
    ;

calling_convention
    : 'cdecl'
    | 'managed'
    | 'stdcall'
    | 'thiscall'
    | 'unmanaged'
    ;

funcptr_parameter_modifier
    : 'ref'
    | 'out'
    | 'in'
    ;

funcptr_return_modifier
    : 'ref'
    | 'ref readonly'
    ;
```

`unmanaged` çağırma kuralı, geçerli platformda yerel kod için varsayılan çağırma kuralını temsil eder ve WinAPI olarak kodlanır.
Tüm `calling_convention`tüm `delegate*`öncesinde bağlamsal anahtar kelimelerdir.

``` csharp
delegate int Func1(string s);
delegate Func1 Func2(Func1 f);

// Function pointer equivalent without calling convention
delegate*<string, int>;
delegate*<delegate*<string, int>, delegate*<string, int>>;

// Function pointer equivalent with calling convention
delegate* managed<string, int>;
delegate*<delegate* managed<string, int>, delegate*<string, int>>;
```

### <a name="function-pointer-conversions"></a>İşlev işaretçisi dönüştürmeleri

Güvenli olmayan bir bağlamda, kullanılabilir örtük dönüştürmeler (örtük dönüştürmeler) kümesi aşağıdaki örtük işaretçi dönüşümlerini içerecek şekilde genişletilir:
- [_Mevcut dönüşümler_](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- _Funcptr\_türü_ `F0` başka bir _funcptr\_türü_ `F1`, aşağıdakilerin tümü doğru olarak sağlandıysa:
    - `F0` ve `F1` aynı sayıda parametreye sahiptir ve `F0` `D0n` her bir parametre `ref`karşılık gelen parametre `out`aynı `in`, `D1n` veya `F1`değiştiricilerine sahiptir.
    - Her bir değer parametresi için (`ref`, `out`veya `in` değiştiricisi olmayan bir parametre), `F0` içindeki parametre türünden `F1`öğesine karşılık gelen parametre türüne bir kimlik dönüştürme, örtük başvuru dönüştürme veya örtük işaretçi dönüştürme bulunur.
    - Her `ref`, `out`veya `in` parametresi için `F0` parametre türü, `F1`karşılık gelen parametre türü ile aynıdır.
    - Dönüş türü değer (Hayır `ref` veya `ref readonly`) ise, `F1` dönüş türünden `F0`dönüş türüne bir kimlik, örtük başvuru veya örtük işaretçi dönüştürme bulunur.
    - Dönüş türü başvuruya (`ref` veya `ref readonly`), dönüş türü ve `F1` `ref` değiştiricileri, `ref` dönüş türü ve `F0`değiştiricilerine benzer.
    - `F0` çağırma kuralı, `F1`çağırma kuralıyla aynıdır.

### <a name="allow-address-of-to-target-methods"></a>Hedef yöntemlere yönelik adrese izin ver

Yöntem gruplarına artık bir adres ifadesi için bağımsız değişken olarak izin verilir. Böyle bir ifadenin türü, hedef yöntemin ve yönetilen çağırma kuralının denk imzasına sahip bir `delegate*` olacaktır:

``` csharp
unsafe class Util {
    public static void Log() { }

    void Use() {
        delegate*<void> ptr1 = &Util.Log;

        // Error: type "delegate*<void>" not compatible with "delegate*<int>";
        delegate*<int> ptr2 = &Util.Log;

        // Okay. Conversion to void* is always allowed.
        void* v = &Util.Log;
   }
}
```

Güvenli olmayan bir bağlamda, bir yöntem `M`, aşağıdakilerin tümü doğru ise bir işlev işaretçisi `F` türü ile uyumludur:
- `M` ve `F` aynı sayıda parametreye sahiptir ve `D` her bir parametre `out`karşılık gelen parametreyle aynı `ref`, `in` veya `F`değiştiricilerine sahiptir.
- Her bir değer parametresi için (`ref`, `out`veya `in` değiştiricisi olmayan bir parametre), `M` içindeki parametre türünden `F`öğesine karşılık gelen parametre türüne bir kimlik dönüştürme, örtük başvuru dönüştürme veya örtük işaretçi dönüştürme bulunur.
- Her `ref`, `out`veya `in` parametresi için `M` parametre türü, `F`karşılık gelen parametre türü ile aynıdır.
- Dönüş türü değer (Hayır `ref` veya `ref readonly`) ise, `F` dönüş türünden `M`dönüş türüne bir kimlik, örtük başvuru veya örtük işaretçi dönüştürme bulunur.
- Dönüş türü başvuruya (`ref` veya `ref readonly`), dönüş türü ve `F` `ref` değiştiricileri, `ref` dönüş türü ve `M`değiştiricilerine benzer.
- `M` çağırma kuralı, `F`çağırma kuralıyla aynıdır.
- `M` statik bir yöntemdir.

Güvenli olmayan bir bağlamda, hedefi bir yöntem grubu olan bir adres ifadeden (örneğin, hedefi bir yöntem grubu) `E` uyumlu bir işlev işaretçisi türünde bir örtük dönüştürme bulunur `F`, `E`, aşağıdaki konuda açıklandığı gibi, parametre türleri ve `F`değiştiriciler kullanılarak oluşturulan bir bağımsız değişken listesine sahiptir.
- Aşağıdaki değişikliklerle form `E(A)` bir yöntem çağrısına karşılık gelen tek bir yöntem `M` seçilir:
   - Bağımsız değişkenler listesi `A`, her biri bir değişken olarak sınıflandırılan ve karşılık gelen _biçimsel\_parametre\_listesinin_ türü ve değiştiricisi (`ref`, `out`veya `in`) olan ifadelerin bir listesidir.`D`
   - Aday yöntemler yalnızca kendi normal biçimlerinde geçerli olan ve genişletilmiş biçimlerinde geçerli olmayan yöntemler olan yöntemlerdir.
- Yöntem etkinleştirmeleri algoritması bir hata üretirse, derleme zamanı hatası oluşur. Aksi takdirde, algoritma `F` aynı sayıda parametreye sahip `M` tek bir en iyi yöntem üretir ve dönüştürme var olarak kabul edilir.
- Seçilen yöntem `M`, işlev işaretçisi türü `F`uyumlu olmalıdır (yukarıda tanımlandığı şekilde). Aksi takdirde, bir derleme zamanı hatası oluşur.
- Dönüştürmenin sonucu `F`türünde bir işlev işaretçisidir.

`E`içinde yalnızca bir statik yöntem `M` `void*` için, hedefi bir yöntem `E` grubu olan bir adres ifadesinde örtük bir dönüştürme bulunur.
Tek bir statik yöntem varsa, `E` en iyi yöntem `M`' dir.
Aksi takdirde, bir derleme zamanı hatası oluşur.

Bu, geliştiricilerin adres operatörü ile birlikte çalışmak için aşırı yükleme çözümleme kurallarına bağlı olabileceği anlamına gelir:

``` csharp
unsafe class Util {
    public static void Log() { }
    public static void Log(string p1) { }
    public static void Log(int i) { };

    void Use() {
        delegate*<void> a1 = &Log; // Log()
        delegate*<int, void> a2 = &Log; // Log(int i)

        // Error: ambiguous conversion from method group Log to "void*"
        void* v = &Log;
    }
```

Address-of işleci `ldftn` yönergesi kullanılarak uygulanır.

Bu özelliğin kısıtlamaları:

- Yalnızca `static`olarak işaretlenen yöntemler için geçerlidir.
- `static` olmayan yerel işlevler `&`kullanılamaz. Bu yöntemlerin uygulama ayrıntıları, dil tarafından kasıtlı olarak belirtilmez. Bu, statik ve örnek veya tam olarak hangi imzayı yayındıklarınızı içerir.

### <a name="better-function-member"></a>Daha iyi işlev üyesi

Daha iyi işlev üyesi belirtimi aşağıdaki satırı içerecek şekilde değiştirilecek:

> `delegate*`, `void*` daha belirgin

Diğer bir deyişle, `void*` ve bir `delegate*` aşırı yükleme yapılabilir ve yine de Address-of işleci kullanılır sensibly.

## <a name="open-issues"></a>Açık sorunlar

### <a name="nativecallableattribute"></a>NativeCallableAttribute

Bu, çağrılırken yerel olarak yönetilen bir prolog 'dan kaçınmak için CLR tarafından kullanılan bir özniteliktir. Bu öznitelik tarafından işaretlenen Yöntemler yönetilmeyen olarak yalnızca yerel koddan çağrılabilir (metotlar çağrılamaz, bir temsilci oluşturun vs...). Özniteliği mscorlib için özel değildir; çalışma zamanı, bu ada sahip herhangi bir özniteliği aynı semantiğe işleyecek.

Bu, çalışma zamanının ve dilin birlikte çalışarak bunu tam olarak desteklemesi mümkündür. Dil, `static` üyelerini belirtilen çağırma kuralına sahip `delegate*` olarak `NativeCallable` özniteliğiyle kabul etmek için seçim olabilir.

``` csharp
unsafe class NativeCallableExample {
    [NativeCallable(CallingConvention.CDecl)]
    static void CloseHandle(IntPtr p) => Marshal.FreeHGlobal(p);

    void Use() {
        delegate*<IntPtr, void> p1 = &CloseHandle; // Error: Invalid calling convention

        delegate* cdecl<IntPtr, void> p2 = &CloseHandle; // Okay
    }
}

```

Ayrıca, dilin de şunları yapmak isteyebilirsiniz:

- Bir hata olarak `NativeCallable` etiketli bir yönteme yapılan yönetilen çağrıları bayrakla işaretle. Verilen işlev yönetilen koddan çağrılamaz derleyicinin geliştiricilerin bu tür bir çağrı yapmasını engellemesini önlemelidir.
- Yöntem `NativeCallable`ile etiketlenmişse metot grubu dönüştürmelerini `delegate` engelleyin.

Bu da `NativeCallable` desteklemek için gerekli değildir. Derleyici, var olan söz dizimini kullanarak `NativeCallable` özniteliğini destekleyebilir. Programın doğru `delegate*` imzasına atama yapmadan önce `void*` 'e dönüştürülmesi gerekir. Bu, günümüzde destek 'ten daha kötü olmayacak.

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a>Genişletilebilir yönetilmeyen çağırma kuralları kümesi

Geçerli ECMA-335 kodlamaları tarafından desteklenen yönetilmeyen çağırma kuralları kümesi güncel değildir. Daha yönetilmeyen çağrı kuralları için destek ekleme isteklerini gördük, örneğin:

- [vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120
- Açık bu https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750 ile StdCall

Bu özelliğin tasarımı, daha sonra gerektiğinde, yönetilmeyen çağırma kuralları kümesinin genişlemesiyle izin verilmelidir. Sorunlar, kodlama kurallarını kodlamak için sınırlı alan içerir (`IMAGE_CEE_CS_CALLCONV_MASK`16 değerden 12 ' de alınır) ve yeni bir çağırma kuralı eklemek için dokunulmaları gereken yerlerin sayısı bulunur. Olası bir çözüm, [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) enum kullanarak çağırma kuralını temsil eden yeni bir kodlama tanıtmaktadır.

Başvuru için, https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h LLVM tarafından desteklenen çağırma kurallarının listesini içerir. .NET, tüm bunları desteklemeye gerek duymadan, çağrı kuralları alanının çok daha zengin olduğunu gösterir.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

### <a name="allow-instance-methods"></a>Örnek yöntemlerine izin ver

Teklif, `EXPLICITTHIS` CLı çağırma kuralı (kodda `instance` olarak C# adlandırılır) avantajlarından yararlanarak örnek yöntemlerini destekleyecek şekilde genişletilebilir. CLı işlev işaretçilerinden oluşan bu form `this` parametresini, işlev işaretçisi sözdiziminin açık bir ilk parametresi olarak koyar.

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

Bu ses, ancak teklife bazı karmaşıkma ekler. Özellikle, çağırma kuralına göre farklı olan işlev işaretçileri `instance` ve `managed` aynı C# imzaya sahip yönetilen yöntemleri çağırmak için her iki durumda da kullanılmasa bile uyumsuz olacak. Her durumda, bu durumun basit bir iş olması için değerli olduğu kabul edilir: `static` yerel bir işlev kullanın.

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a>Bildirimde güvenli olmayan iste

`delegate*`her kullanımda `unsafe` gerektirmek yerine, yalnızca bir yöntem grubunun `delegate*`dönüştürüldüğü noktada gereklidir. Bu, temel güvenlik sorunlarının oynatılmasına (değer etkin olduğu sürece kapsayan derlemenin kaldırılamayacağını bilmek) neden olur. Diğer konumlarda `unsafe` gerektirme, aşırı olarak görülebilir.

Tasarımın ilk amacı budur. Ancak elde edilen dil kuralları, çok daha fazla. Bu bir işaretçi değeri olduğu ve `unsafe` anahtar sözcüğü olmadan bile göz atma işlemini gizleyebilmek olanaksızdır. Örneğin `object` dönüştürmeye izin verilmiyorsa, bir `class`üyesi olamaz, vb... C# Tasarım, tüm işaretçinin kullanması için `unsafe` gerektirirken, bu tasarımın bu tasarımı takip eder.

Geliştiriciler, `delegate*` değerlerinin üzerine, bugün normal işaretçi türleri için yaptıkları gibi bir _güvenli_ sarmalayıcı sunma yeteneğine sahip olmaya devam edecektir. Aşağıdakileri dikkate alın:

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a>Temsilcileri kullanma

`delegate*`yeni bir sözdizimi öğesi kullanmak yerine, mevcut `delegate` türlerini, türü takip eden bir `*` ile kullanmanız yeterlidir:

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

Çağırma kuralını işleme, bir `CallingConvention` değeri belirten bir özniteliğe sahip `delegate` türlerine açıklama ekleyerek yapılabilir. Bir özniteliğin olmaması yönetilen çağırma kuralını işaret eder.

Il 'de bunu kodlamada sorun vardır. Temel alınan değerin bir işaretçi olarak temsil edilebilmesi için yine de şunları yapmanız gerekir:

1. Farklı işlev işaretçisi türleri olan aşırı yüklemelerin izin vermek için benzersiz bir türü vardır.
1. Derleme sınırları genelinde OHI amaçları için eşdeğer olmalıdır.

Son nokta özellikle sorunlu. Bu, `Func<int>*` bir derlemede tanımlansa bile, `Func<int>*` kullanan her derlemenin meta verilerde eşdeğer bir tür kodlanması gerektiği anlamına gelir.
Ayrıca, mscorlib olmayan bir derlemede `System.Func<T>` adı ile tanımlanan diğer herhangi bir tür, mscorlib içinde tanımlanan sürümden farklı olmalıdır.

Araştırılan bir seçenek, `mod_req(Func<int>) void*`gibi bir işaretçiyi yaymıştı. `mod_req` bir `TypeSpec` bağlanamaz ve bu nedenle genel örneklemeleri hedeflenemez.

### <a name="named-function-pointers"></a>Adlandırılmış işlev işaretçileri

İşlev işaretçisi sözdizimi, özellikle de iç içe geçmiş işlev işaretçileri gibi karmaşık durumlarda, kısabera olabilir. Geliştiricilerin, `delegate`işlev işaretçilerine yönelik adlandırılmış bildirimler için izin veren her seferinde imzayı yazmak yerine, ile yapılır.

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

Buradaki sorunun bir bölümü, temel alınan CLı ilkel adının adı yoktur, bu nedenle yalnızca bir C# hata oluşur ve etkinleştirilmesi için bir veri kümesi çalışması gerekir. Bu, iş açısından önemli olan, ancak önemli bir çalışmadır. Temelde bu adlar C# için def tablosuna yalnızca bir yardımcı olması gerekir.

Ayrıca, adlandırılmış işlev işaretçileri için bağımsız değişkenler incelendikleri zaman, diğer birçok senaryoya eşit bir şekilde uygulanamazlar. Örneğin, tüm durumlarda tam imzayı yazma ihtiyacını azaltmak için adlandırılmış tanımlama gruplarını bildirmek uygun olabilir.

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

Tartışmadan sonra `delegate*` türlerin adlandırılmış bildirimine izin verme kararı verdik. Bu, müşteri kullanımı geri bildirimlerine göre bu için önemli bir gereksinim olduğunu fark etmemiz halinde işlev işaretçileri, tanımlama grupları, genel türler, vb. için uygun bir adlandırma çözümü araştıracağız. Bu, formda tam `typedef` desteği gibi diğer önerilere benzer olması olasıdır.

## <a name="future-considerations"></a>Gelecekteki konular

### <a name="static-local-functions"></a>statik yerel işlevler

Bu, yerel işlevlerde `static` değiştiriciye izin veren [öneriyi](https://github.com/dotnet/csharplang/issues/1565) ifade eder. Bu tür bir işlevin, `static` olarak ve kaynak kodunda tam imzayla belirtilen şekilde yayınlanacağı garanti edilir. Bu tür bir işlev, yerel işlevlerin bugün bulunduğu sorunlardan hiçbirini içermediğinden `&` için geçerli bir bağımsız değişken olmalıdır

### <a name="static-delegates"></a>statik temsilciler

Bu, yalnızca `static` üyelere başvurabilen `delegate` türlerinin bildirimine izin veren [öneriyi](https://github.com/dotnet/csharplang/issues/302) ifade eder. Bu tür `delegate` örneklerin avantajı, performans duyarlı senaryolarda ücretsiz ve daha iyi bir şekilde ayrılabilmektedir.

İşlev işaretçisi özelliği uygulanmışsa, `static delegate` teklif büyük olasılıkla kapatılmış olur. Bu özelliğin önerilen avantajı, ayırma ücretsiz doğası olur. Ancak son araştırmalar, derleme kaldırma işlemi nedeniyle elde edilebilecek mümkün olmayan bir şekilde bulundu. Derlemenin içinden bellekten kaldırılmasını önlemek için `static delegate` için başvurduğu yönteme güçlü bir tanıtıcı olması gerekir.

Her `static delegate` örneğinin bakımını yapmak için, teklifin hedeflerine sayaç çalıştıran yeni bir tanıtıcı tahsis etmek gerekir. Ayırmanın çağrı-site başına tek bir ayırmaya salsatılabileceği, ancak bir bit karmaşıktı olduğu ve bu da ticareti kapatmasının gerektiği bazı tasarımlar vardı.

Diğer bir deyişle, geliştiriciler temelde aşağıdaki denge ile karar vermek zorunda değildir:

1. Derlemeyi kaldırma aşamasında güvenlik: Bu ayırmaları gerektirir ve bu nedenle `delegate` zaten yeterli bir seçenektir.
1. Derleme kaldırma aşamasında güvenlik yok: `delegate*`kullanın. Bu, kodun geri kalanında bir `unsafe` bağlamı dışında kullanılmasına izin vermek için bir `struct` kaydırılmış olabilir.
